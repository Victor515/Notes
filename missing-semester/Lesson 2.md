# Lesson 2 Shell Tools and Scripting

 

[TOC]

## Shell Scripting

1. focus on **bash scripting**

2. variable assignment pro tip: no space between variables and values. In shell scripting, space will perform argument splitting

3. how to access a variable: `$foo`

4. `""` will expand dollar signs while `''` (literal string) won't

5. special signs in command line

   $0

   $1-9

   $@ -- all the arguments

   $# -- number of arguments

   $$ -- PID for the current script

   $_ -- last argument from the last command

   !! -- entire last command, including arguments

6. $? -- Return code of the previous command

   error code of a command: 0 usually means everything went okay, other return code means an error occurred.

   || and &&:  [short-circuiting](https://en.wikipedia.org/wiki/Short-circuit_evaluation) operators, can be used to conditionally execute commands based on exit codes (**common pattern 1**)

7. Commands can also be separated within the same line using a semicolon `;`

8. command substitution (common pattern 2): get the output of a command as a variable:

   ```bash
   for file in $(ls)
   ```

   less know pattern: process substitution, `<( CMD )` will execute `CMD` and place the output in a temporary file and substitute the `<()` with that file’s name. Example:

   ```bash
   diff <(ls foo) <(ls bar)
   ```

9. a simple program to showcase some features

   ```bash
   #!/bin/bash
   
   echo "Starting program at $(date)" # Date will be substituted
   
   echo "Running program $0 with $# arguments with pid $$"
   
   for file in "$@"; do
       grep foobar "$file" > /dev/null 2> /dev/null
       # When pattern is not found, grep has exit status 1
       # We redirect STDOUT and STDERR to a null register since we do not care about them
       if [[ $? -ne 0 ]]; then
           echo "File $file does not have any foobar, adding one"
           echo "# foobar" >> "$file"
       fi
   done
   ```

   $#:  number of arguments

   $$: process id

   $@: expand to all arguments

   `> /dev/null 2> /dev/null`: discard standard output and standard error

   When performing comparisons in bash, **try to use double brackets `[[ ]]`** in favor of simple brackets `[ ]`

10. shell *globbing*:
   * Wildcards: you can use `?` and `*` to match one or any amount of characters respectively
   * Curly braces `{}` - Whenever you have a common substring in a series of commands, you can use curly braces for bash to expand this automatically

11. Useful static analysis tool: https://www.shellcheck.net/
12. shebang line: `#!/bin/bash`, `#!/usr/local/bin/python`



## Shell Tools

### Finding how to use commands

a simplified  version of `man`: `tldr`

### Finding files

1. `find` : a powerful tool to find stuff, but also do stuff

```bash
# Find all directories named src
find . -name src -type d
# Find all python files that have a folder named test in their path
find . -path '*/test/*.py' -type f
# Find all files modified in the last day
find . -mtime -1
# Find all zip files with size in range 500k to 10M
find . -size +500k -size -10M -name '*.tar.gz'

# Delete all files with .tmp extension
find . -name '*.tmp' -exec rm {} \;
# Find all PNG files and convert them to JPG
find . -name '*.png' -exec convert {} {}.jpg \;
```

2. an alternative to find: https://github.com/sharkdp/fd

3. for speed: `locate`, comparison: https://unix.stackexchange.com/questions/60205/locate-vs-find-usage-pros-and-cons-of-each-other

### Finding Code

1. `grep`: finding code:

   - `-C` for getting **C**ontext around the matching line

   - `-v` for in**v**erting the match, i.e. print all lines that do **not** match the pattern

   - you want to use `-R` since it will **R**ecursively go into directories and look for files for the matching string



2. `rg`: ripgrep, a better and efficent version of `grep`, fast and intuitive

```bash
# Find all python files where I used the requests library
rg -t py 'import requests'
# Find all files (including hidden files) without a shebang line
rg -u --files-without-match "^#!"
# Find all matches of foo and print the following 5 lines
rg foo -A 5
# Print statistics of matches (# of matched lines and files )
rg --stats PATTERN
```



### Find shell commands

1. `history`:  search your own history of commands, can be combined with grep:

`history | grep find`

run the command from history `![number in history]`

2. `fzf`: fuzzy, interactive search in command line



### Directory Navigation

1. tools to quickly do directory listing and navigation:`ls -R`, `tree`, `broot`
2.  [`fasd`](https://github.com/clvv/fasd) and [`autojump`](https://github.com/wting/autojump):  Finding frequent and/or recent files and directories





## Exercises

1. write an `ls` command that lists files in the following manner

   - Includes all files, including hidden files (-a)
   - Sizes are listed in human readable format (e.g. 454M instead of 454279954) (-lh)
   - Files are ordered by recency (-t)
   - Output is colorized

   answer: `ls -alht .`  

2. Write bash functions `marco` and `polo` that do the following. Whenever you execute `marco` the current working directory should be saved in some manner, then when you execute `polo`, no matter what directory you are in, `polo` should `cd` you back to the directory where you executed `marco`. For ease of debugging you can write the code in a file `marco.sh` and (re)load the definitions to your shell by executing `source marco.sh`.

```bash
#!/bin/bash

macro() {
        saved_dir=$(pwd)
}

polo() {
        cd $saved_dir
}

# notice the different usage of $ in two functions. in macro, $ is for command substitution, in polo, $ is for variable access
```

3. Say you have a command that fails rarely. In order to debug it you need to capture its output but it can be time consuming to get a failure run. Write a bash script that runs the **following script** until it fails and captures its standard output and error streams to files and prints everything at the end. Bonus points if you can also report how many runs it took for the script to fail.

```bash
 #!/usr/bin/env bash

 n=$(( RANDOM % 100 ))

 if [[ n -eq 42 ]]; then
    echo "Something went wrong"
    >&2 echo "The error was using magic numbers"
    exit 1
 fi

 echo "Everything went according to plan"
```

My answer:

```bash
#!/usr/bin/env bash

count=0
echo "Debug start..."

while [[ $? -eq 0 ]];
do
    count=$((count+1))
    ./random.sh >> stdout.capture 2>> stderr.capture
done

echo "Script ran for $count times"
```

4. As we covered in the lecture `find`’s `-exec` can be very powerful for performing operations over the files we are searching for. However, what if we want to do something with **all** the files, like creating a zip file? As you have seen so far commands will take input from both arguments and STDIN. **When piping commands, we are connecting STDOUT to STDIN, but some commands like `tar` take inputs from arguments**. To bridge this disconnect there’s the [`xargs`](https://www.man7.org/linux/man-pages/man1/xargs.1.html) command which will execute a command using STDIN as arguments. For example `ls | xargs rm` will delete the files in the current directory.

   Your task is to write a command that recursively finds all HTML files in the folder and makes a zip with them. Note that your command should work even if the files have spaces (hint: check `-d` flag for `xargs`).

   If you’re on macOS, note that the default BSD `find` is different from the one included in [GNU coreutils](https://en.wikipedia.org/wiki/List_of_GNU_Core_Utilities_commands). You can use `-print0` on `find` and the `-0` flag on `xargs`. As a macOS user, you should be aware that command-line utilities shipped with macOS may differ from the GNU counterparts; you can install the GNU versions if you like by [using brew](https://formulae.brew.sh/formula/coreutils).

   Answer: 

   ```bash
   find . -name '*.html' -print0 | xargs -0 tar czf html_bundle.tar.gz
   ```

5. (Advanced) Write a command or script to recursively find the most recently modified file in a directory. More generally, can you list all files by recency?

```bash
find . -type f -printf '%T@ %t%p\n' | sort -rn | | cut -f2- -d" " | head -1
```

```bash
find . -type f -printf '%T@ %t%p\n' | sort -rn | cut -f2- -d" "
```

reference:

- https://stackoverflow.com/questions/4561895/how-to-recursively-find-the-latest-modified-file-in-a-directory
- https://askubuntu.com/questions/684220/how-to-list-the-last-modified-files-in-a-specific-directory-recursively
- https://www.codedodle.com/find-printf.html#time-directives
- sort -n:  --numeric-sort          compare according to string numerical value
