# Python Review



## Regular Expression

1. Python basic functions like index() , find() , split() , count() , replace() do not handle well with some complicated searching and replacing cases.

2. **Verbose** regular expression is a good way to write maintainable regular expressions:

   ```python
   pattern = '''
   ^ # beginning of string
   M{0,3} # thousands - 0 to 3 Ms
   (CM|CD|D?C{0,3}) # hundreds - 900 (CM), 400 (CD), 0-300 (0 to 3 Cs),
   #
   (XC|XL|L?X{0,3})
   # tens - 90 (XC), 40 (XL), 0-30 (0 to 3 Xs),
   #
   (IX|IV|V?I{0,3})
   or 50-80 (L, followed by 0 to 3 Xs)
   # ones - 9 (IX), 4 (IV), 0-3 (0 to 3 Is),
   #
   $
   or 500-800 (D, followed by 0 to 3 Cs)
   or 5-8 (V, followed by 0 to 3 Is)
   # end of string
   '''
   
   re.search(pattern, 'M', re.VERBOSE)
   ```

   Both whitespace and comments are ignored.

3. Basic usage summary:

   ◦ ^ matches the beginning of a string.
   ◦ $ matches the end of a string.
   ◦ \b matches a word boundary.
   ◦ \d matches any numeric digit.
   ◦ \D matches any non-numeric character.
   ◦ x? matches an optional x character (in other words, it matches an x zero or one times).
   ◦ x* matches x zero or more times.
   ◦ x+ matches x one or more times.
   ◦ x{n,m} matches an x character at least n times, but not more than m times.
   ◦ (a|b|c) matches either a or b or c .
   ◦ (x) in general is a remembered group. You can get the value of what matched by using the groups() method of the object returned by re.search



## Closures and Generators

1. "This technique of using the values of **outside parameters** within a dynamic function is called
   closures." This parameters will stay if after returning from that function.

2. Generator is a special kind of function that generates values one at a time.

   ```python
   # This is python interative shell
   >>> def make_counter(x):
   ... print('entering make_counter')
   ... while True:
   ... yield x
   ... print('incrementing x')
   ... x = x + 1
   1
   ...
   >>> counter = make_counter(2)
   >>> counter 3
   <generator object at 0x001C9C10>
   >>> next(counter)
   4
   entering make_counter
   2
   >>> next(counter)
   5
   incrementing x
   3
   >>> next(counter)
   6
   incrementing x
   4
   ```

3. "yield" pauses a generator, and "next" resumes where it is left off.



## Classes & Iterators

1. In python, `__init__` method is called **after** the instance is created. So it is not a constructor.

2. All meta data about the class of an instance can be obtained from its attributes.

3. An iterator is just a class that defines an `__iter__()` method. The `__iter__()` method can return any object that implements a `__next__()` method.

   ```python
   # Define a fibonacci iterator
   class Fib:
       def __init__(self, max):
           self.max = max
           
       def __iter__(self):
           self.a = 0
           self.b = 1
           return self
   
       def __next__(self):
           fib = self.a
           if fib > self.max:
               raise StopIteration
           self.a, self.b = self.b, self.a + self.b
           return fib
   ```


4. A *class variable* is shared among all instances of a class (just like static field in Java).  You can modify it in the attributes of `__class__`.



## Advanced Iterators

1. Using **generator expression** instead of list comprehension saves both CPU and RAM.

   ```python
   unique_characters = {'E', 'D', 'M', 'O', 'N', 'S', 'R', 'Y'}
   gen = (ord(c) for c in unique_characters)
   
   # gen is a generator. It does not evaluate next value until it is used
   
   list_comp = [ord(c) for c in unique_characters]
   
   ## list_comp is a list. All its values have been evaluated during list comprehension
   ```

2. check out `itertools` modules: https://docs.python.org/3/library/itertools.html

   ```python
   itertools.permutations([1,2,3],2)
   itertools.combinations([1,2,3],2)
   itertools.product([1,2,3], ['A', 'B', 'C'])
   ```

3. `zip()` is an useful function to create pairs of values, which can be applied to construct a dict.

   ```python
   characters = ('S', 'M', 'E', 'D', 'O', 'N', 'R', 'Y')
   guess = ('1', '2', '0', '3', '4', '5', '6', '7')
   tuple(zip(characters, guess))
   '''
   (('S', '1'), ('M', '2'), ('E', '0'), ('D', '3'),
   ('O', '4'), ('N', '5'), ('R', '6'), ('Y', '7'))
   '''
   dict(zip(characters, guess))
   '''
   {'E': '0', 'D': '3', 'M': '2', 'O': '4',
   'N': '5', 'S': '1', 'R': '6', 'Y': '7'}
   '''
   ```



## Unit Testing

1. There is a limited range of numbers that can be expressed as Roman numerals, specifically 1 through 3999. There is no way to represent 0 in Roman numerals.

2. A test case answers a single question about the code it is testing (**Every test is an island**).

3. Test-driven development: Write a test that fails, then code until it passes.

4. Using Python's reserved word `pass` is good way when you want to stub a method that you are going to write later.

5. When a test case doesn't pass, `unittest` distinguishes between **failures and errors**.

6. It is not enough to test with good input, you should also test bad input.

7. Define exceptions:

   ```python
   # These two exceptions extend ValueError exception
   class OutOfRangeError(ValueError): pass
   class NotIntegerError(ValueError): pass
   ```

8. Write docstrings is also a valid way to stub a function:

   ```python
   # roman5.py
   def from_roman(s):
   '''convert Roman numeral to integer'''
   ```



## Refactoring

1. Today’s unit test is tomorrow’s regression test.

2. Be aware to differentiate between failures and errors.

3. The best thing about unit testing is that it gives you the freedom to refactor mercilessly.

4. Lesson learned about unit testing:

   • Designing test cases that are specific, automated, and independent

   • Writing test cases before the code they are testing

   • Writing tests that test good input and check for proper results

   • Writing tests that test bad input and check for proper failure responses

   • Writing and updating test cases to reflect new requirements

   • Refactoring mercilessly to improve performance, scalability, readability, maintainability, or whatever other -ility you’re lacking



## File

1. Bytes are bytes; characters are an abstraction.

2. The default encoding is platform-dependent (On Windows, it is CP-1252). Always specify the encoding when you open a file!

3. Use `with` statement when opening a file:

4. ```python
   # with will automatically close the file when the block ends
   with open('examples/chinese.txt', encoding='utf-8') as a_file:
      	a_file.seek(17)
      	a_character = a_file.read(1)
      	print(a_character)
   
   # Read a whole text file
   with open('examples/chinese.txt', encoding='utf-8') as a_file:
       text = a_file.read()
       print(text)
   
   # Read a text file line by line
   line_number = 0
   with open('examples/favorite-people.txt', encoding='utf-8') as a_file:
       for a_line in a_file:
           line_number += 1
           print('{:>4} {}'.format(line_number, a_line.rstrip()))
   
   # Write/Append to a text file
   >>> with open('test.log', mode='w', encoding='utf-8') as a_file:
   ...    a_file.write('test succeeded')
   >>> with open('test.log', encoding='utf-8') as a_file:
   ...    print(a_file.read())
   test succeeded
   >>> with open('test.log', mode='a', encoding='utf-8') as a_file:
   ...    a_file.write('and again')
   >>> with open('test.log', encoding='utf-8') as a_file:
   ...    print(a_file.read())
   test succeededand again
   
   # Read a binary file
   with open('examples/beauregard.jpg', mode='rb') as an_image:
       data = an_image.read(3) # read 3 bytes
       print(data)
   
   ```

    File object does not always need files as source:

   ```python
   # We can treat strings as file objects
   >>> a_string = 'PapayaWhip is the new black.'
   >>> import io 1
   >>> a_file = io.StringIO(a_string)
   >>> a_file.read()
   'PapayaWhip is the new black.'
   
   # This is how to treat compressed files
   >>> import gzip
   >>> with gzip.open('out.log.gz', mode='wb') as z_file:
   ...     z_file.write('A nine mile walk is no joke, especially in the rain.'.encode('utf-8'))
   ...
   >>> exit()
   ```

5. standard input, output and error:

   ```python
   import sys
   sys.stdin
   sys.stdout
   sys.stderr
   
   ## Below is how to define a custom context manager to redirect stdout
   import sys
   class RedirectStdoutTo:
       def __init__(self, out_new):
           self.out_new = out_new
       def __enter__(self):
           self.out_old = sys.stdout
           sys.stdout = self.out_new
       def __exit__(self, *args):
           sys.stdout = self.out_old
   
   print('A')
   with open('out.log', mode='w', encoding='utf-8') as a_file,RedirectStdoutTo(a_file):
       print('B')
   print('C')
   
   # Only 'A', 'C' will print on screen
   # 'B' will be printed to file out.log
   ```



## Serializing Python Objects

1. For Python-specific serialization, we can use pickle:

   ```python
   import pickle
   # Dump to a disk file
   with open('entry.pickle', 'wb') as f:
       pickle.dump(entry, f)
   # Load from a disk file
   with open('entry.pickle', 'rb') as f:
       entry = pickle.load(f)
   
   # Pickle without a file
   b = pickle.dumps(entry) # dump to a byte object
   type(b) #<class 'bytes'>
   entry3 = pickle.loads(b)
   entry3 == entry #True
   
   ```

2. For cross-language compatibility, we can use JSON (JavaScript Object Notation).

3. **There ain’t no such thing as “plain text”.** Encoding always play an important role.

4. JSON must be stored in Unicode Encoding.

5. bytes, tuples and custom class instances are not serializable with json modules. We need to define custom `to_json` and `from_json` converter and supply then to `json.dump` and `json.load` accordingly. 

   ```python
   # in customserializer.py
   import time
   def to_json(python_object):
       if isinstance(python_object, time.struct_time):
           return {'__class__': 'time.asctime','__value__':time.asctime(python_object)}
       if isinstance(python_object, bytes):
           return {'__class__': 'bytes','__value__': list(python_object)}
       raise TypeError(repr(python_object) + ' is not JSON serializable')
   
   def from_json(json_object):
   	if '__class__' in json_object:
   		if json_object['__class__'] == 'time.asctime':
   			return time.strptime(json_object['__value__'])
   		if json_object['__class__'] == 'bytes':
   			return bytes(json_object['__value__'])
   	return json_object
   
   
   import customserializer
   with open('entry.json', 'w', encoding='utf-8') as f:
       json.dump(entry, f, default=customserializer.to_json)
   
   with open('entry.json', 'r', encoding='utf-8') as f:
       entry = json.load(f, object_hook=customserializer.from_json)
   ```

6. json module does not distinguish between lists and tuples.



