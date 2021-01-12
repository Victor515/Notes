Including the commands you already used, I will try my best to detail what can be done to do some forensic operations in an executable file.

The humble `strings` command can be useful to visualize text error messages which give hints of the binary functionalities. It also a simple way for [detecting packed binaries](https://unix.stackexchange.com/questions/419242/strange-linux-binary) as in the example (frequent with malware binaries):

```
$strings exe_file
UPX!
...
PROT_EXEC|PROT_WRITE failed.
$Info: This file is packed with the UPX executable packer http://upx.sf.net $
$Id: UPX 3.91 Copyright (C) 1996-2013 the UPX Team. All Rights Reserved. $
...
UPX!
```

> strings - print the strings of printable characters in files.
> For each file given, GNU strings prints the printable character sequences that are at least 4 characters long (or the number given with the options below) and are followed by an unprintable character.

`file` allows to see the executable properties, namely:

- the architecture it targets;
- the OS;
- if dynamically or statically linked;
- if compiled with debugging info or not.

In this example, "not stripped" denotes it was compiled with debugging info included.

```
$ file exe_file
exe_file: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 2.6.18, BuildID[sha1]=6f4c5f003e19c7a4bbacb30af3e84a41c88fc0d9, not stripped
```

> `file` tests each argument in an attempt to classify it. There are three sets of tests, performed in this order: filesystem tests, magic tests, and language tests. The first test that succeeds causes the file type to be printed.

`objdump` produces the disassembly listing of an executable:

```
$ objdump -d exe_file
ls:     file format Mach-O 64-bit x86-64

Disassembly of section __TEXT,__text:
__text:
100000f20:      55      pushq   %rbp
100000f21:      48 89 e5        movq    %rsp, %rbp
100000f24:      48 83 c7 68     addq    $104, %rdi
100000f28:      48 83 c6 68     addq    $104, %rsi
100000f2c:      5d      popq    %rbp
100000f2d:      e9 58 36 00 00  jmp     13912
100000f32:      55      pushq   %rbp
100000f33:      48 89 e5        movq    %rsp, %rbp
100000f36:      48 8d 46 68     leaq    104(%rsi), %rax
100000f3a:      48 8d 77 68     leaq    104(%rdi), %rsi
...............
```

`objdump` also allows to know the compiler used to compile the binary executable:

```
$ objdump -s --section .comment exe_file

exe_file:     file format elf64-x86-64

Contents of section .comment:
 0000 4743433a 2028474e 55292034 2e342e37  GCC: (GNU) 4.4.7
 0010 20323031 32303331 33202852 65642048   20120313 (Red H
 0020 61742034 2e342e37 2d313129 00        at 4.4.7-11).
```

`objdump` also lists external functions dynamic linked on run-time:

$ objdump -T exe_file

```
true:     file format elf64-x86-64

DYNAMIC SYMBOL TABLE:
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 __uflow
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 getenv
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 free
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 abort
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 __errno_location
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 strncmp
0000000000000000  w   D  *UND*  0000000000000000              _ITM_deregisterTMCloneTable
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 _exit
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 __fpending
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 textdomain
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 fclose
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 bindtextdomain
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 dcgettext
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 __ctype_get_mb_cur_max
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 strlen
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.4   __stack_chk_fail
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 mbrtowc
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 strrchr
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 lseek
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 memset
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 fscanf
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 close
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 __libc_start_main
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 memcmp
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 fputs_unlocked
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 calloc
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 strcmp
0000000000000000  w   D  *UND*  0000000000000000              __gmon_start__
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.14  memcpy
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 fileno
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 malloc
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 fflush
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 nl_langinfo
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 ungetc
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 __freading
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 realloc
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 fdopen
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 setlocale
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.3.4 __printf_chk
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 error
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 open
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 fseeko
0000000000000000  w   D  *UND*  0000000000000000              _Jv_RegisterClasses
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 __cxa_atexit
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 exit
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 fwrite
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.3.4 __fprintf_chk
0000000000000000  w   D  *UND*  0000000000000000              _ITM_registerTMCloneTable
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 mbsinit
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.2.5 iswprint
0000000000000000  w   DF *UND*  0000000000000000  GLIBC_2.2.5 __cxa_finalize
0000000000000000      DF *UND*  0000000000000000  GLIBC_2.3   __ctype_b_loc
0000000000207228 g    DO .bss   0000000000000008  GLIBC_2.2.5 stdout
0000000000207220 g    DO .bss   0000000000000008  GLIBC_2.2.5 __progname
0000000000207230  w   DO .bss   0000000000000008  GLIBC_2.2.5 program_invocation_name
0000000000207230 g    DO .bss   0000000000000008  GLIBC_2.2.5 __progname_full
0000000000207220  w   DO .bss   0000000000000008  GLIBC_2.2.5 program_invocation_short_name
0000000000207240 g    DO .bss   0000000000000008  GLIBC_2.2.5 stderr
```

> `objdump` displays information about one or more object files. The options control what particular information to display. This information is mostly useful to programmers who are working on the compilation tools, as opposed to programmers who just want their program to compile and work.

You may run the binary in a VM only created and then discarded just for the purpose of running the binary. Use `strace`, `ltrace`, `gdb` and `sysdig` to learn more about what the binary is doing at the system calls level at run time.

```
$strace exe_file
open("/opt/sms/AU/mo/tmp.RqBcjY", O_RDWR|O_CREAT|O_EXCL, 0600) = 3
open("/opt/sms/AU/mo/tmp.PhHkOr", O_RDWR|O_CREAT|O_EXCL, 0600) = 4
open("/opt/sms/AU/mo/tmp.q4MtjV", O_RDWR|O_CREAT|O_EXCL, 0600) = 5
```

> `strace` runs the specified command until it exits. It intercepts and records the system calls which are called by a process and the signals which are received by a process. The name of each system call, its arguments and its return value are printed on standard error or to the file specified with the -o option.

```
$ltrace exe_file
_libc_start_main(0x400624, 1, 0x7ffcb7b6d7c8, 0x400710 <unfinished ...>  
time(0)                                                                              = 1508018406  
srand(0x59e288e6, 0x7ffcb7b6d7c8, 0x7ffcb7b6d7d8, 0)                                 = 0  
sprintf("mkdir -p -- '/opt/sms/AU/mo'", "mkdir -p -- '%s'", "/opt/sms/AU/mo")        = 28  
system("mkdir -p -- '/opt/sms/AU/mo'" <no return ...>  
--- SIGCHLD (Child exited) ---  
<... system resumed> )                                                               = 0  
rand(2, 0x7ffcb7b6d480, 0, 0x7f9d6d4622b0)                                           = 0x2d8ddbe1  
sprintf("/opt/sms/AU/mo/tmp.XXXXXX", "%s/tmp.XXXXXX", "/opt/sms/AU/mo")      = 29  
mkstemp(0x7ffcb7b6d5c0, 0x40080b, 0x40081a, 0x7ffffff1)                              = 3  
sprintf("/opt/sms/AU/mo/tmp.XXXXXX", "%s/tmp.XXXXXX", "/opt/sms/AU/mo")      = 29  
mkstemp(0x7ffcb7b6d5c0, 0x40080b, 0x40081a, 0x7ffffff1)                              = 4  
+++ exited (status 0) +++  
```

> `ltrace` is a program that simply runs the specified command until it exits. It intercepts and records the dynamic library calls which are called by the executed process and the signals which are received by that process. It can also intercept and print the system calls executed by the program.

It can also be debugged step-by-step with `gdb`.

> The purpose of a debugger such as GDB is to allow you to see what is going on ''inside'' another program while it executes.

To follow/create dumps of much of its system activity running it, use sysdig as in:

```
#sudo sysdig proc.name=exe_file
……………….
11569 19:05:40.938743330 1 exe_file (35690) > getpid 
11570 19:05:40.938744605 1 exe_file (35690) < getpid 
11571 19:05:40.938749018 1 exe_file (35690) > open 
11572 19:05:40.938801508 1 exe_file (35690) < open fd=3(<f>/opt/sms/AU/mo/tmp.MhVlrl) name=/opt/sms/AU/mo/tmp.XXXXMhVlrl flags=39(O_EXCL|O_CREAT|O_RDWR) mode=0 
11573 19:05:40.938811276 1 exe_file (35690) > getpid 
11574 19:05:40.938812431 1 exe_file (35690) < getpid 
11575 19:05:40.938813171 1 exe_file (35690) > open 
11576 19:05:40.938826313 1 exe_file (35690) < open fd=4(<f>/opt/sms/AU/mo/tmp.5tlBSs) name=/opt/sms/AU/mo/tmp.5tlBSs flags=39(O_EXCL|O_CREAT|O_RDWR) mode=0 
11577 19:05:40.938848592 1 exe_file (35690) > getpid 
11578 19:05:40.938849139 1 exe_file (35690) < getpid 
11579 19:05:40.938849728 1 exe_file (35690) > open 
11580 19:05:40.938860629 1 exe_file (35690) < open fd=5(<f>/opt/sms/AU/mo/tmp.CJWQjA) name=/opt/sms/AU/mo/tmp.CJWQjA flags=39(O_EXCL|O_CREAT|O_RDWR) mode=0 
```

> `sysdig` is a tool for system troubleshooting, analysis and explo‐ ration. It can be used to capture, filter and decode system calls and other OS events. sysdig can be both used to inspect live systems, or to generate trace files that can be analyzed at a later stage.
>
> sysdig includes a powerful filtering language, has customizable output, and can be extended through Lua scripts, called chisels.

We will deal again with the static analisys of the binary file itself on the remainder of this answer.

`ldd exe_file` lists the libraries it uses;

```
$ ldd exe_file
    linux-vdso.so.1 (0x00007ffdf83bd000)  
    libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f14d9b32000)  
    /lib64/ld-linux-x86-64.so.2 (0x000055ededaea000)  
```

> `ldd` prints the shared objects (shared libraries) required by each program or shared object specified on the command line.

```
size -A exe_file
$ size -A exe_file  
exe_file  :  
section              size      addr  
.interp                28   4194816  
.note.ABI-tag          32   4194844  
.note.gnu.build-id     36   4194876  
.gnu.hash              28   4194912  
.dynsym               216   4194944  
.dynstr                90   4195160  
.gnu.version           18   4195250  
.gnu.version_r         32   4195272  
.rela.dyn              24   4195304  
.rela.plt             168   4195328  
.init                  24   4195496  
.plt                  128   4195520  
.text                 664   4195648  
.fini                  14   4196312  
.rodata                51   4196328  
.eh_frame_hdr          36   4196380  
.eh_frame             124   4196416  
.ctors                 16   6293696  
.dtors                 16   6293712  
.jcr                    8   6293728  
.dynamic              400   6293736  
.got                    8   6294136  
.got.plt               80   6294144  
.data                   4   6294224  
.bss                   16   6294232  
.comment               45         0  
Total                2306


$ size -d ls
   text    data     bss     dec     hex filename
 122678    4664    4552  131894   20336 ls
```

> The GNU `size` utility lists the section sizes---and the total size---for each of the object or archive files objfile in its argument list. By default, one line of output is generated for each object file or each module in an archive.

`readelf -x .rodata exe_file` lists static strings

```
$ readelf -x .rodata exe_file 

Hex dump of section '.rodata':
  0x004007e8 01000200 00000000 00000000 00000000 ................
  0x004007f8 6d6b6469 72202d70 202d2d20 27257327 mkdir -p -- '%s'
  0x00400808 0025732f 746d702e 58585858 58585858 .%s/tmp.XXXXXXXX
  0x00400818 585800                              XX.
```

`readelf -h exe_file` gets ELF header information

```
$ readelf -h exe_file   
ELF Header:  
  Magic:   7f 45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00   
  Class:                             ELF64  
  Data:                              2's complement, little endian  
  Version:                           1 (current)  
  OS/ABI:                            UNIX - System V  
  ABI Version:                       0  
  Type:                              EXEC (Executable file)  
  Machine:                           Advanced Micro Devices X86-64  
  Version:                           0x1  
  Entry point address:               0x400540  
  Start of program headers:          64 (bytes into file)  
  Start of section headers:          3072 (bytes into file)  
  Flags:                             0x0  
  Size of this header:               64 (bytes)  
  Size of program headers:           56 (bytes)  
  Number of program headers:         8  
  Size of section headers:           64 (bytes)  
  Number of section headers:         30  
  Section header string table index: 27  
```

`readelf -s exe_file` displays symbols

```
$ readelf -s exe_file 

Symbol table '.dynsym' contains 9 entries:  
   Num:    Value          Size Type    Bind   Vis      Ndx Name  
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND   
     1: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__  
     2: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND __libc_start_main@GLIBC_2.2.5 (2)  
     3: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND system@GLIBC_2.2.5 (2)  
     4: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND sprintf@GLIBC_2.2.5 (2)  
     5: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND mkstemp@GLIBC_2.2.5 (2)  
     6: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND srand@GLIBC_2.2.5 (2)  
     7: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND rand@GLIBC_2.2.5 (2)  
     8: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND time@GLIBC_2.2.5 (2)  

Symbol table '.symtab' contains 69 entries:  
   Num:    Value          Size Type    Bind   Vis      Ndx Name  
     0: 0000000000000000     0 NOTYPE  LOCAL  DEFAULT  UND   
     1: 0000000000400200     0 SECTION LOCAL  DEFAULT    1   
     2: 000000000040021c     0 SECTION LOCAL  DEFAULT    2   
     3: 000000000040023c     0 SECTION LOCAL  DEFAULT    3   
     4: 0000000000400260     0 SECTION LOCAL  DEFAULT    4   
     5: 0000000000400280     0 SECTION LOCAL  DEFAULT    5   
     6: 0000000000400358     0 SECTION LOCAL  DEFAULT    6   
     7: 00000000004003b2     0 SECTION LOCAL  DEFAULT    7   
     8: 00000000004003c8     0 SECTION LOCAL  DEFAULT    8   
     9: 00000000004003e8     0 SECTION LOCAL  DEFAULT    9   
    10: 0000000000400400     0 SECTION LOCAL  DEFAULT   10   
    11: 00000000004004a8     0 SECTION LOCAL  DEFAULT   11   
    12: 00000000004004c0     0 SECTION LOCAL  DEFAULT   12   
    13: 0000000000400540     0 SECTION LOCAL  DEFAULT   13   
    14: 00000000004007d8     0 SECTION LOCAL  DEFAULT   14   
    15: 00000000004007e8     0 SECTION LOCAL  DEFAULT   15   
    16: 000000000040081c     0 SECTION LOCAL  DEFAULT   16   
    17: 0000000000400840     0 SECTION LOCAL  DEFAULT   17   
    18: 00000000006008c0     0 SECTION LOCAL  DEFAULT   18   
    19: 00000000006008d0     0 SECTION LOCAL  DEFAULT   19   
    20: 00000000006008e0     0 SECTION LOCAL  DEFAULT   20   
    21: 00000000006008e8     0 SECTION LOCAL  DEFAULT   21   
    22: 0000000000600a78     0 SECTION LOCAL  DEFAULT   22   
    23: 0000000000600a80     0 SECTION LOCAL  DEFAULT   23   
    24: 0000000000600ad0     0 SECTION LOCAL  DEFAULT   24   
    25: 0000000000600ad8     0 SECTION LOCAL  DEFAULT   25   
    26: 0000000000000000     0 SECTION LOCAL  DEFAULT   26   
    27: 000000000040056c     0 FUNC    LOCAL  DEFAULT   13 call_gmon_start  
    28: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS crtstuff.c  
    29: 00000000006008c0     0 OBJECT  LOCAL  DEFAULT   18 __CTOR_LIST__  
    30: 00000000006008d0     0 OBJECT  LOCAL  DEFAULT   19 __DTOR_LIST__  
    31: 00000000006008e0     0 OBJECT  LOCAL  DEFAULT   20 __JCR_LIST__  
    32: 0000000000400590     0 FUNC    LOCAL  DEFAULT   13 __do_global_dtors_aux  
    33: 0000000000600ad8     1 OBJECT  LOCAL  DEFAULT   25 completed.6349  
    34: 0000000000600ae0     8 OBJECT  LOCAL  DEFAULT   25 dtor_idx.6351  
    35: 0000000000400600     0 FUNC    LOCAL  DEFAULT   13 frame_dummy  
    36: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS crtstuff.c  
    37: 00000000006008c8     0 OBJECT  LOCAL  DEFAULT   18 __CTOR_END__  
    38: 00000000004008b8     0 OBJECT  LOCAL  DEFAULT   17 __FRAME_END__  
    39: 00000000006008e0     0 OBJECT  LOCAL  DEFAULT   20 __JCR_END__  
    40: 00000000004007a0     0 FUNC    LOCAL  DEFAULT   13 __do_global_ctors_aux  
    41: 0000000000000000     0 FILE    LOCAL  DEFAULT  ABS    exe_file.c  
    42: 0000000000600a80     0 OBJECT  LOCAL  DEFAULT   23 _GLOBAL_OFFSET_TABLE_  
    43: 00000000006008bc     0 NOTYPE  LOCAL  DEFAULT   18 __init_array_end  
    44: 00000000006008bc     0 NOTYPE  LOCAL  DEFAULT   18 __init_array_start  
    45: 00000000006008e8     0 OBJECT  LOCAL  DEFAULT   21 _DYNAMIC  
    46: 0000000000600ad0     0 NOTYPE  WEAK   DEFAULT   24 data_start  
    47: 0000000000400700     2 FUNC    GLOBAL DEFAULT   13 __libc_csu_fini  
    48: 0000000000400540     0 FUNC    GLOBAL DEFAULT   13 _start  
    49: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__  
    50: 0000000000000000     0 NOTYPE  WEAK   DEFAULT  UND _Jv_RegisterClasses  
    51: 00000000004007d8     0 FUNC    GLOBAL DEFAULT   14 _fini  
    52: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND __libc_start_main@@GLIBC_  
    53: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND system@@GLIBC_2.2.5  
    54: 00000000004007e8     4 OBJECT  GLOBAL DEFAULT   15 _IO_stdin_used  
    55: 0000000000600ad0     0 NOTYPE  GLOBAL DEFAULT   24 __data_start  
    56: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND sprintf@@GLIBC_2.2.5  
    57: 00000000004007f0     0 OBJECT  GLOBAL HIDDEN    15 __dso_handle  
    58: 00000000006008d8     0 OBJECT  GLOBAL HIDDEN    19 __DTOR_END__  
    59: 0000000000400710   137 FUNC    GLOBAL DEFAULT   13 __libc_csu_init  
    60: 0000000000600ad4     0 NOTYPE  GLOBAL DEFAULT  ABS __bss_start  
    61: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND mkstemp@@GLIBC_2.2.5  
    62: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND srand@@GLIBC_2.2.5  
    63: 0000000000600ae8     0 NOTYPE  GLOBAL DEFAULT  ABS _end  
    64: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND rand@@GLIBC_2.2.5  
    65: 0000000000600ad4     0 NOTYPE  GLOBAL DEFAULT  ABS _edata  
    66: 0000000000000000     0 FUNC    GLOBAL DEFAULT  UND time@@GLIBC_2.2.5  
    67: 0000000000400624   207 FUNC    GLOBAL DEFAULT   13 main  
    68: 00000000004004a8     0 FUNC    GLOBAL DEFAULT   11 _init  
```

> `readelf` displays information about one or more ELF format object files. The options control what particular information to display.
>
> elffile... are the object files to be examined. 32-bit and 64-bit ELF files are supported, as are archives containing ELF files.

`nm exe_file` lists symbols from the object table:

```
$ nm exe_file   
0000000000600ad4 A __bss_start  
000000000040056c t call_gmon_start  
0000000000600ad8 b completed.6349  
00000000006008c8 d __CTOR_END__  
00000000006008c0 d __CTOR_LIST__  
0000000000600ad0 D __data_start  
0000000000600ad0 W data_start  
00000000004007a0 t __do_global_ctors_aux  
0000000000400590 t __do_global_dtors_aux  
00000000004007f0 R __dso_handle  
00000000006008d8 D __DTOR_END__  
0000000000600ae0 b dtor_idx.6351  
00000000006008d0 d __DTOR_LIST__  
00000000006008e8 d _DYNAMIC  
0000000000600ad4 A _edata  
0000000000600ae8 A _end  
00000000004007d8 T _fini  
0000000000400600 t frame_dummy  
00000000004008b8 r __FRAME_END__  
0000000000600a80 d _GLOBAL_OFFSET_TABLE_  
                 w __gmon_start__  
00000000004004a8 T _init  
00000000006008bc d __init_array_end  
00000000006008bc d __init_array_start  
00000000004007e8 R _IO_stdin_used  
00000000006008e0 d __JCR_END__  
00000000006008e0 d __JCR_LIST__  
                 w _Jv_RegisterClasses  
0000000000400700 T __libc_csu_fini  
0000000000400710 T __libc_csu_init  
                 U __libc_start_main@@GLIBC_2.2.5  
0000000000400624 T main  
                 U mkstemp@@GLIBC_2.2.5  
                 U rand@@GLIBC_2.2.5  
                 U sprintf@@GLIBC_2.2.5  
                 U srand@@GLIBC_2.2.5  
0000000000400540 T _start  
                 U system@@GLIBC_2.2.5  
                 U time@@GLIBC_2.2.5  
```

> `nm` lists the symbols from object files objfile.... If no object files are listed as arguments, nm assumes the file a.out.

Besides disassembling the binary with `objdump`, a decompiler can also be used.

For decompiling, recently I did a technical challenge where I needed to decompile two small 64-bit linux binaries.

I tried to use Boomerang and Snowman. The Boomerang project seems abandoned, and I was not impressed of the limitations of both of them. Several other alternatives, either open source/freeware/old including a recent one released by Avast, only decompiled 32 bit binaries.

I ended up trying the demo of [Hopper](https://www.hopperapp.com/) in MacOS (it also has a Linux version).

> Hopper Disassembler, the reverse engineering tool that lets you disassemble, decompile and debug your applications.

Hopper disassembles and decompiles either 32 or 64 bits binaries for OS/X, Linux and Windows. It is capable of tackling large binaries when licensed.

It also makes flow graphs of the functions of/program structure and variables.

It is also being actively maintained and updated. However it is commercial.

I enjoyed so much using it and the resulting output that bought a license. The license is far more affordable than hex-rays by a long shot.

In the comments of this answer, @d33tah and @Josh also mention as an open source alternatives [radare2](http://rada.re/r/) plus the corresponding graphical interface [Cutter](https://github.com/radareorg/cutter) being similar to Hopper in Linux, cannot vouch personally for it as I do not use them.

Also, as the target binary was compiled with debug info, you may get back the original name of functions and variables.

More notably, you won't ever get back the comments in the source code as they are not compiled in any way into binary executables.

Improving the quality of the output source, and the understanding of the binary will always imply some time and detective work. Decompilers only do so much of the work.

Example of Hopper output without debug info:

```
int EntryPoint(int arg0, int arg1, int arg2) {
    rdx = arg2;
    rbx = arg1;
    r12 = arg0;
    if (r12 <= 0x1) goto loc_100000bdf;

loc_10000093c:
    r15 = *(rbx + 0x8);
    if (strcmp(r15, "-l") == 0x0) goto loc_1000009c2;

loc_100000953:
    if (strcmp(r15, "-s") == 0x0) goto loc_100000a45;
```

The Hopper graphical interface is also very usable (several functionalities expanded at the same time on this picture):