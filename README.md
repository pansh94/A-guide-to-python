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

## Useful links :
1. [Jupyter Notebook Documentation](http://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/what_is_jupyter.html)
2. [SQLAlchemy basic tutorial](http://docs.sqlalchemy.org/en/latest/core/engines.html#database-urls) 
3. [Basic Python Tutorial](https://www.python-course.eu/python3_course.php)
4. [Fast Track Analytical Python Tutorial](https://www.analyticsvidhya.com/blog/2016/01/complete-tutorial-learn-data-science-python-scratch-2/)
5. [Think Stat Book for Data Scientist](http://greenteapress.com/thinkstats2/thinkstats2.pdf)
6. [Basic of memory management.](https://www.slideshare.net/nnja/memory-management-in-python-the-basics)
7. [Matplotlib Tutorial](https://matplotlib.org/gallery.html)
8. [Django with SQLAlchemy](https://www.compose.com/articles/using-postgresql-through-sqlalchemy/)

