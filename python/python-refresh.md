Nitty-gritty of python and algorithms from this book: https://donsheehy.github.io/datastructures/fullbook.html

1. slicing a sequence creates a new object. That means a big slice will do a lot of copying. It's really easy to write inefficient code this way.
2. A single .py file is called a module. You can import one module into another using the import keyword. 
3. When we import a module, the code in that module is executed. 
4. The module also has a namespace in which these functions and classes are defined.
5. Python’s polymorphism is based on the idea of duck typing. The name comes from an old expression that if something walks like a duck and quacks like a duck, then it is a duck. 
6. we use tests to determine two things:
Does it work? That is, does the code do what it’s supposed to do?
Does it still work? Can you be confident that the changes you made haven’t caused other part of the code to break?
7. Concatenation and slicing both produce a new collection and thus the running times are proportional to the length of the newly created collection.
