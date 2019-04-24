# A-guide-to-python
This is what I have learned in python untill now.
## Introduction to Memory
Python allocates memory transparently, manages objects using a reference count system, and frees memory when an object’s reference count falls to zero.  
**(Names -> Reference -> Object)**  
Names are just label, actual value stores in Object. Object maintain reference count to know how many names are link to it.  
**Python Memory Manager** use private heap for storing objects and to satisfy all memory related needs.Memory reclamation is mostly handled by reference counting. That is, the Python VM keeps an internal journal of how many references refer to an object, and automatically garbage collects it when there are no more references referring to it. In addition, there is a mechanism to break circular references (which reference counting can't handle) by detecting unreachable "islands" of objects, somewhat in reverse of traditional GC algorithms that try to find all the reachable objects.  
**Generation Garbage Collection** : It makes a generation list i.e (generation1,generation2,generation3). Newly creted object store in *gereration 0*. Only obj rc>0 is store in generation list. When no. of obj stored in generation is above a threshold then python run a garbage collection  algo on that generation and generation younger than that. It maintains a discard list, if an obj is not referenced from outside it added to discard list. After completion of algo it deletes the discard list. The obj who survives this are promoted to higher level.    

## Oops in Python 

### 4 oops concept :
1. Inheritance.
2. Encapsulation.
3. Abstraction
4. Polymorphism.

"Everything" is treated the same way, everything is a class: functions and methods are values just like lists, integers or floats. Each of these are instances of their corresponding classes.
```
class Robot:
    pass

if __name__ == "__main__":
    x = Robot()
    y = Robot()
    y2 = y
    print(y == y2) #True
    print(y == x)  #False
```
### Attribute
Properties and attributes are essentially different things in Python. Attribute can be created inside of a class definition or  dynamically create arbitrary new attributes for existing instances of a class.  
```
>>> class Robot:
...     pass
... 
>>> x = Robot()
>>> y = Robot()
>>> 
>>> x.name = "Marvin" #Out of the class attribute.
>>> x.build_year = "1979"
>>> 
>>> y.name = "Caliban"
>>> y.build_year = "1993"
>>> 
>>> print(x.name)
Marvin
>>> print(y.build_year)
1993
>>> 
```
The instances possess dictionaries __dict__, which they use to store their attributes and their corresponding values:  
```
>>> x.__dict__
{'name': 'Marvin', 'build_year': '1979'}
>>> y.__dict__
{'name': 'Caliban', 'build_year': '1993'}
```
Attributes can be bound to class names as well. In this case, each instance will possess this name as well.  
```
>>> class Robot(object):
...     pass 
>>> x = Robot()
>>> Robot.brand = "Kuka"
>>> x.brand
'Kuka'
>>> x.brand = "Thales"
>>> Robot.brand
'Kuka'
>>> y = Robot()
>>> y.brand
'Kuka'
>>> Robot.brand = "Thales"
>>> y.brand
'Thales'
>>> x.brand
'Thales'
#this is what happening in __dict__ :
>>> x.__dict__
{'brand': 'Thales'}
>>> y.__dict__
{}
>>>
>>> Robot.__dict__
mappingproxy({'__module__': '__main__', '__weakref__': , '__doc__': None, '__dict__': , 'brand': 'Thales'})
```
If you try to access y.brand, Python checks first, if "brand" is a key of the y.__dict__ dictionary. If it is not, Python checks, if "brand" is a key of the Robot.__dict__. If so, the value can be retrieved. If an attribute name is not in included in either of the dictionary, the attribute name is not defined. If you try to access a non-existing attribute, you will raise an **AttributeError**.  
Even function names can be attributed.  
```
>>> def f(x):
...     return 42 
>>> f.x = 42
>>> print(f.x)
42
```
This can be used as a replacement for the static function variables of C and C++, which are not possible in Python. We use a counter attribute in the following example:  
```
def f(x):
    f.counter = getattr(f, "counter", 0) + 1 
    return "Monty Python"
        

for i in range(10):
    f(i)
print(f.counter)
#o/p : 10

```

### Method
1. Instead of defining a function outside of a class definition and binding it to a class attribute, we define a method directly inside (indented) of a class definition.  
2. A method is "just" a function which is defined inside of a class.
3. The first parameter is used a reference to the calling instance. 
4. This parameter is usually called self.

We have seen that a method differs from a function only in two aspects:  
1. It belongs to a class, and it is defined within a class.
2. The first parameter in the definition of a method has to be a reference to the instance, which called the method. This parameter is usually called "self"(self is not a keyword in python).
All the three are same :
```
type(x).m(x, ...)
C.m(x, ...)
x.m(...)
#hi is function which except an obj.
def hi(obj):
    print("Hi, I am " + obj.name + "!")

class Robot:
    pass
    

x = Robot()
x.name = "Marvin"
hi(x)
```
### The __init__ method :
 __init__ is a method which is immediately and automatically called after an instance has been created.  
 ```
 class Robot:
 
    def __init__(self, name=None):
        self.name = name   
        
    def say_hi(self):
        if self.name:
            print("Hi, I am " + self.name)
        else:
            print("Hi, I am a robot without a name")
    

x = Robot()
x.say_hi() #Hi, I am a robot without a name
y = Robot("Marvin")
y.say_hi() #Hi, I am Marvin 

```

### Data Abstraction, Data Encapsulation, and Information Hiding :
**Encapsulation** is seen as the bundling of data with the methods that operate on that data. **Information hiding** on the other hand is the principle that some internal information or data is "hidden", so that it can't be accidentally changed. **Data encapsulation** via methods doesn't necessarily mean that the data is hidden.  Finally, **data abstraction** is present, if both data hiding and data encapsulation is used. This means data abstraction is the broader term.  
**Encapsulation** generally achieve through getter setter method.
```
class Robot:
 
    def __init__(self, name=None):
        self.name = name   
        
    def say_hi(self):
        if self.name:
            print("Hi, I am " + self.name)
        else:
            print("Hi, I am a robot without a name")
            
    def set_name(self, name):
        self.name = name
        
    def get_name(self):
        return self.name
    

x = Robot()
x.set_name("Henry")
x.say_hi()
y = Robot()
y.set_name(x.get_name())
print(y.get_name())
```
### __str__- and __repr__-Methods
If you apply str or repr to an object, Python is looking for a corresponding method __str__ or __repr__ in the class definition of the object. If the method does exist, it will be called. Otherwise they will display the string of normal obj.
```
>>> l = ["Python", "Java", "C++", "Perl"]
>>> print(l)
['Python', 'Java', 'C++', 'Perl']
>>> str(l)
"['Python', 'Java', 'C++', 'Perl']"
>>> repr(l)
"['Python', 'Java', 'C++', 'Perl']"
>>> d = {"a":3497, "b":8011, "c":8300}
>>> print(d)
{'a': 3497, 'c': 8300, 'b': 8011}
>>> str(d)
"{'a': 3497, 'c': 8300, 'b': 8011}"
>>> repr(d)
"{'a': 3497, 'c': 8300, 'b': 8011}"
>>> x = 587.78
>>> str(x)
'587.78'
>>> repr(x)
'587.78'
#########################################################################################################
# when __str__ & __repr__ function not available in class then python refer to default obj.
>>> class A:
...     pass
>>> a = A()
>>> print(a)
<__main__.A object at 0xb720a64c>
>>> print(repr(a))
<__main__.A object at 0xb720a64c>
>>> print(str(a))
<__main__.A object at 0xb720a64c>
>>> a
<__main__.A object at 0xb720a64c>
##########################################################################################################
# when __str__ function is available the python always call the defined function.
>>> class A:
...     def __str__(self):
...         return "42"
>>> a = A()
>>> print(repr(a))
<__main__.A object at 0xb720a4cc>
>>> print(str(a))
42
>>> a
<__main__.A object at 0xb720a4cc>
############################################################################################################
# when __repr__ function is available in class and no __str__ function is available then in both case __repr__ will be applied.
>>> class A:
...     def __repr__(self):
...         return "42" 
>>> a = A()
>>> print(repr(a))
42
>>> print(str(a))
42
>>> a
42
```
A frequently asked question is when to use __repr__ annd when __str__. __str__ is always the right choice, if the output should be for the end user or in other words, if it should be nicely printed. __repr__ on the other hand is used for the internal representation of an object. The output of __repr__ should be - if feasible - a string which can be parsed by the python interpreter. The result of this parsing is in an equal object.
This means that the following should be true for an object "o":
**o == eval(repr(o))**
```
class Robot:

    def __init__(self, name, build_year):
        self.name = name
        self.build_year = build_year

    def __repr__(self):
        return "Robot('" + self.name + "', " +  str(self.build_year) +  ")"

    def __str__(self):
        return "Name: " + self.name + ", Build Year: " +  str(self.build_year)
     
if __name__ == "__main__":
    x = Robot("Marvin", 1979)
        
    x_str = str(x)
    print(x_str)
    print("Type of x_str: ", type(x_str))
    new = eval(x_str) # This will show error becoz eval can only convert repr value into an obj not str's.
    print(new)
    print("Type of new:", type(new))    
```

### Public- Protected- and Private Attributes
1.  name : Public :: These attributes can be freely used inside or outside of a class definition.
2. _name : Protected :: Protected attributes should not be used outside of the class definition, unless inside of a subclass definition.
3. __name : 	Private :: This kind of attribute is inaccessible and invisible. It's neither possible to read nor write to those attributes, except inside of the class definition itself.

```
class Robot:
 
    def __init__(self, name=None, build_year=2000):
        self.__name = name
        self.__build_year = build_year
        
    def say_hi(self):
        if self.__name:
            print("Hi, I am " + self.__name)
        else:
            print("Hi, I am a robot without a name")
            
    def set_name(self, name):
        self.__name = name
        
    def get_name(self):
        return self.__name    

    def set_build_year(self, by):
        self.__build_year = by
        
    def get_build_year(self):
        return self.__build_year    
    
    def __repr__(self):
        return "Robot('" + self.__name + "', " +  str(self.__build_year) +  ")"

    def __str__(self):
        return "Name: " + self.__name + ", Build Year: " +  str(self.__build_year)

     
if __name__ == "__main__":
    x = Robot("Marvin", 1979)
    y = Robot("Caliban", 1943)
    for rob in [x, y]:
        rob.say_hi()
        if rob.get_name() == "Caliban":
            rob.set_build_year(1993)
        print("I was built in the year " + str(rob.get_build_year()) + "!")
```
if you try to access the private variable directly it will blow on your damn face and "Attribute not exit" error will apear.  

### Destructor
There is no "real" destructor, but something similar, i.e. the method **__del__**. It is called when the instance is about to be destroyed and if there is no other reference to this instance. If base class has destructor then derived class constructor should be called explicitly. Otherwise there would not be any proper deletion of the base class part of the instance.  
```
class Robot():
    
    def __init__(self, name):
        print(name + " has been created!")
        
    def __del__(self):
        print ("Robot has been destroyed")
        
        
if __name__ == "__main__":
    x = Robot("Tik-Tok")
    y = Robot("Jenkins")
    z = x
    print("Deleting x")
    del x
    print("Deleting z")
    del z
    del y
o/p :::::::::::::::
Tik-Tok has been created!
Jenkins has been created!
Deleting x
Deleting z
Robot has been destroyed
Robot has been destroyed
```
### Class Attribute
Class attributes are attributes which are owned by the class itself. They will be shared by all the instances of the class. Therefore they have the same value for every instance. We define class attributes outside of all the methods, usually they are placed at the top, right below the class header. But be careful, if you want to change a class attribute, you have to do it with the notation **ClassName.AttributeName**. Otherwise, you will create a new instance variable.
```
>>> class A:
...     a = "I am a class attribute!"
... 
>>> x = A()
>>> y = A()
>>> x.a = "This creates a new instance attribute for x!"
>>> y.a
'I am a class attribute!'
>>> A.a
'I am a class attribute!'
>>> A.a = "This is changing the class attribute 'a'!"
>>> A.a
"This is changing the class attribute 'a'!"
>>> y.a
"This is changing the class attribute 'a'!"
>>> # but x.a is still the previously created instance variable:
... 
>>> x.a
'This creates a new instance attribute for x!'
>>> 
```
Python's class attributes and object attributes are stored in separate dictionaries, as we can see here: 
```
>>> x.__dict__
{'a': 'This creates a new instance attribute for x!'}
>>> y.__dict__
{}
>>> A.__dict__
dict_proxy({'a': "This is changing the class attribute 'a'!", '__dict__': <attribute '__dict__' of 'A' objects>, '__module__': '__main__', '__weakref__': <attribute '__weakref__' of 'A' objects>, '__doc__': None})
>>> x.__class__.__dict__
dict_proxy({'a': "This is changing the class attribute 'a'!", '__dict__': <attribute '__dict__' of 'A' objects>, '__module__': '__main__', '__weakref__': <attribute '__weakref__' of 'A' objects>, '__doc__': None})
>>> 
```

### Example with Class Attributes

## Useful links :
1. [Jupyter Notebook Documentation](http://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/what_is_jupyter.html)
2. [SQLAlchemy basic tutorial](http://docs.sqlalchemy.org/en/latest/core/engines.html#database-urls) 
3. [Basic Python Tutorial](https://www.python-course.eu/python3_course.php)
4. [Fast Track Analytical Python Tutorial](https://www.analyticsvidhya.com/blog/2016/01/complete-tutorial-learn-data-science-python-scratch-2/)
5. [Think Stat Book for Data Scientist](http://greenteapress.com/thinkstats2/thinkstats2.pdf)
6. [Basic of memory management.](https://www.slideshare.net/nnja/memory-management-in-python-the-basics)
7. [Matplotlib Tutorial](https://matplotlib.org/gallery.html)
8. [Django with SQLAlchemy](https://www.compose.com/articles/using-postgresql-through-sqlalchemy/)
9. [Docker Docs](https://docs.docker.com/engine/docker-overview/)
10. [Rules how to write clear python code](https://www.python.org/dev/peps/pep-0008/)
11. [Read this book before starting python](https://effectivepython.com/)
