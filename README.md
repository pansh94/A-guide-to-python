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
```
class Robot:

    Three_Laws = (
"""A robot may not injure a human being or, through inaction, allow a human being to come to harm.""",
"""A robot must obey the orders given to it by human beings, except where such orders would conflict with the First Law.,""",
"""A robot must protect its own existence as long as such protection does not conflict with the First or Second Law."""
)

    def __init__(self, name, build_year):
        self.name = name
        self.build_year = build_year

    # other methods as usual
    
-------------------------------------------
from robot_asimov import Robot
for number, text in enumerate(Robot.Three_Laws):
    print(str(number+1) + ":\n" + text)
--------------------------------------------
output :
1:
A robot may not injure a human being or, through inaction, allow a human being to come to harm.
2:
A robot must obey the orders given to it by human beings, except where such orders would conflict with the First Law.,
3:
A robot must protect its own existence as long as such protection does not conflict with the First or Second Law.
```
We demonstrate in the following example, how you can count instance with class attributes. All we have to do is :
```
class C :
    counter = 0
    def __init__(self):
        type(self).counter += 1 
    def __del__(self):
        type(self).counter -= 1 
if __name == "__main__" :
    x = C()
    print("Number of instances: : " + str(C.counter))
    y = C()
    print("Number of instances: : " + str(C.counter))
    del x
    print("Number of instances: : " + str(C.counter))
    del y
    print("Number of instances: : " + str(C.counter))
------------------------------------------------------
output :
$ python3 counting_instances.py 
Number of instances: : 1
Number of instances: : 2
Number of instances: : 1
Number of instances: : 0
------------------------------------------------------
```

### Static method
Making class attribute as private, so now we will need a method to access that.
```
class Robot:
    __counter = 0
    
    def __init__(self):
        type(self).__counter += 1
        
    def RobotInstances(self):
        return Robot.__counter
        

if __name__ == "__main__":
    x = Robot()
    print(x.RobotInstances())
    y = Robot()
    print(x.RobotInstances())
--------------------------------------------
```
o/p : This will create error because If we try to invoke the method with the class name Robot.RobotInstances(), we get an error message, because it needs an instance as an argument.
The solution consists in static methods, which don't need a reference to an instance. 
It's easy to turn a method into a static method. All we have to do is to add a line with "@staticmethod" directly in front of the method header. It's the decorator syntax. 
Note :
1. Static methods are bound to a class not a object.
2. Static function use as a Utility functins like perform very common task like read/write in file, open/close db.
```
class Robot:
    __counter = 0
    
    def __init__(self):
        type(self).__counter += 1
        
    @staticmethod
    def RobotInstances():
        return Robot.__counter
        

if __name__ == "__main__":
    print(Robot.RobotInstances())
    x = Robot()
    print(x.RobotInstances())
    y = Robot()
    print(x.RobotInstances())
    print(Robot.RobotInstances())
------------------------------------------------------
o/p : 
0
1
2
2
```
### Class method
Static methods shouldn't be confused with class methods. Like static methods class methods are not bound to instances, but unlike static methods class methods are bound to a class. The first parameter of a class method is a reference to a class, i.e. a class object. They can be called via an instance or the class name. 
```
class Robot:
    __counter = 0
    
    def __init__(self):
        type(self).__counter += 1
        
    @classmethod
    def RobotInstances(cls):
        return cls, Robot.__counter
        

if __name__ == "__main__":
    print(Robot.RobotInstances())
    x = Robot()
    print(x.RobotInstances())
    y = Robot()
    print(x.RobotInstances())
    print(Robot.RobotInstances())
---------------------------------------
o/p :
$ python3 static_methods4.py 
<class '__main__.Robot'>, 0)
<class '__main__.Robot'>, 1)
<class '__main__.Robot'>, 2)
<class '__main__.Robot'>, 2)
```
We have defined a static gcd function to calculate the greatest common divisor of two numbers. the greatest common divisor (gcd) of two or more integers (at least one of which is not zero), is the largest positive integer that divides the numbers without a remainder. The static method "gcd" is called by our class method "reduce" with "cls.gcd(n1, n2)". "CLS" is a reference to "fraction".
```
class fraction(object):

    def __init__(self, n, d):
        self.numerator, self.denominator = fraction.reduce(n, d)
        

    @staticmethod
    def gcd(a,b):
        while b != 0:
            a, b = b, a%b
        return a

    @classmethod
    def reduce(cls, n1, n2):
        g = cls.gcd(n1, n2)
        return (n1 // g, n2 // g)

    def __str__(self):
        return str(self.numerator)+'/'+str(self.denominator)
------------------------------------------------------------------
>>> from fraction1 import fraction
>>> x = fraction(8,24)
>>> print(x)
1/3
```
We define a class "Pets" with a method "about". This class will be inherited in a subclass "Dogs" and "Cats". The method "about" will be inherited as well. We will define the method "about" as a "staticmethod" in our first implementation to show the disadvantage of this approach:
```
class Pets:
    name = "pet animals"

    @staticmethod
    def about():
        print("This class is about {}!".format(Pets.name))   
    

class Dogs(Pets):
    name = "'man's best friends' (Frederick II)"

class Cats(Pets):
    name = "cats"

p = Pets()
p.about()
d = Dogs()
d.about()
c = Cats()
c.about()
------------------------------------------------------------
o/p:
This class is about pet animals!
This class is about pet animals!
This class is about pet animals!
```
The "problem" is that the method "about" doesn't know that it has been called by an instance of the Dogs or Cats class. 
```
class Pets:
    name = "pet animals"

    @classmethod
    def about(cls):
        print("This class is about {}!".format(cls.name))
    
class Dogs(Pets):
    name = "'man's best friends' (Frederick II)"

class Cats(Pets):
    name = "cats"

p = Pets()
p.about()

d = Dogs()
d.about()

c = Cats()
c.about()
---------------------------------------------------------------
o/p:
This class is about pet animals!
This class is about 'man's best friends' (Frederick II)!
This class is about cats!
```
### Properties
The class with a property looks like this:
```
class P:

    def __init__(self,x):
        self.__x = x

    @property
    def x(self):
        return self.__x

    @x.setter
    def x(self, x):
        if x < 0:
            self.__x = 0
        elif x > 1000:
            self.__x = 1000
        else:
            self.__x = x
---------------------------------------
>>> from p import P
>>> p1 = P(1001)
>>> p1.x
1000
>>> p1.x = -12
>>> p1.x
0
>>> 
++++++++++++++++++++++++++++++++++++++++++++++++
class C:
    def __init__(self):
        self._x = None

    def getx(self):
        return self._x

    def setx(self, value):
        self._x = value

    def delx(self):
        del self._x

    x = property(getx, setx, delx, "I'm the 'x' property.")
#If c is an instance of C, c.x will invoke the getter, c.x = value will invoke the setter and del c.x the deleter.
```
A method which is used for getting a value is decorated with "@property", i.e. we put this line directly in front of the header. The method which has to function as the setter is decorated with "@x.setter". If the function had been called "f", we would have to decorate it with "@f.setter". 
Two things are noteworthy: We just put the code line "self.x = x" in the __init__ method and the property method x is used to check the limits of the values. The second interesting thing is that we wrote "two" methods with the same name and a different number of parameters "def x(self)" and "def x(self,x)". We have learned in a previous chapter of our course that this is not possible. It works here due to the decorating.

## Inheritance
A class can inherit attributes and behaviour methods from another class, called the superclass. Python supports multiple inheritance too. The name BaseClassName must be defined in a scope containing the derived class definition. 
```
Syntax:
class DerivedClassName(BaseClassName):
    pass
--------------------------------------
class Person:

    def __init__(self, first, last):
        self.firstname = first
        self.lastname = last

    def Name(self):
        return self.firstname + " " + self.lastname

class Employee(Person):

    def __init__(self, first, last, staffnum):
        Person.__init__(self,first, last)
        self.staffnumber = staffnum

    def GetEmployee(self):
        return self.Name() + ", " +  self.staffnumber

x = Person("Marge", "Simpson")
y = Employee("Homer", "Simpson", "1007")

print(x.Name())
print(y.GetEmployee())
---------------------------------------
$ python3 person.py 
Marge Simpson
Homer Simpson, 1007
```
The __init__ method of our Employee class explicitly invokes the __init__method of the Person class. We could have used super instead. super().__init__(first, last) is automatically replaced by a call to the superclasses method, in this case __init__:
```
    def __init__(self, first, last, staffnum):
        super().__init__(first, last)
        self.staffnumber = staffnum
```
### Overloading and Overriding
#### Overriding
```
class Person:

    def __init__(self, first, last):
        self.firstname = first
        self.lastname = last

    def __str__(self):
        return self.firstname + " " + self.lastname

class Employee(Person):

    def __init__(self, first, last, staffnum):
        super().__init__(first, last)
        self.staffnumber = staffnum


x = Person("Marge", "Simpson")
y = Employee("Homer", "Simpson", "1007")

print(x)
print(y)
--------------------------------------------------
o/p : python3 person2.py 
Marge Simpson
Homer Simpson
```
First of all, we can see that if we print an instance of the Employee class, the __str__ method of Person is used. This is due to inheritance. The only problem we have now is the fact that the output of "print(y)" is not the same as the "print(y.GetEmployee())". This means that our Employee class needs its own __str__ method. We could write it like this:
```
def __str__(self):
        return super().__str__() + ", " +  self.staffnumber
```
We have overridden the method __str__ from Person in Employee. By the way, we have overridden __init__ also. Method overriding is an object-oriented programming feature that allows a subclass to provide a different implementation of a method that is already defined by its superclass or by one of its superclasses. The implementation in the subclass overrides the implementation of the superclass by providing a method with the same name, same parameters or signature, and same return type as the method of the parent class. 

#### Overloading
The second definition of f with two parameters redefines or overrides the first definition with one argument. Overriding means that the first definition is not available anymore. This explains the error message: 
```
>>> def f(n):
...     return n + 42
... 
>>> def f(n,m):
...     return n + m + 42
... 
>>> f(3,4)
49
>>> f(3)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: f() takes exactly 2 arguments (1 given)
>>> 
```
If we need such a behaviour, we can simulate it with default parameters: 
```
def f(n, m=None):
    if m:
        return n + m +42
    else:
        return n + 42
        
def f(*x):
    if len(x) == 1:
        return x[0] + 42
    else: 
        return x[0] + x[1] + 42
```
### Public instead of Private Attributes
1. Will the value of "OurAtt" be needed by the possible users of our class?
   If not, we can or should make it a private attribute.
2. If it has to be accessed, we make it accessible as a public attribute
3. We will define it as a private attribute with the corresponding property, if and only if we have to do some checks or 
   transformation of the data. (As an example, you can have a look again at our class P, where the attribute has to be in the    interval between 0 and 1000, which is ensured by the property "x")
4. Alternatively, you could use a getter and a setter, but using a property is the Pythonic way to deal with it!

## Multiple Inheritance
The critics point out that multiple inheritance comes along with a high level of complexity and ambiguity in situations such as the diamond problem.
A class definition, where a child class SubClassName inherits from the parent classes BaseClass1, BaseClass2, BaseClass3, and so on, looks like this: 
```
class SubclassName(BaseClass1, BaseClass2, BaseClass3, ...):
    pass
```
We are implementing a clock and a calander class and will inherit those in third class.
```
class Clock(object):

    def __init__(self, hours, minutes, seconds):
        """
        The paramaters hours, minutes and seconds have to be 
        integers and must satisfy the following equations:
        0 <= h < 24
        0 <= m < 60
        0 <= s < 60
        """

        self.set_Clock(hours, minutes, seconds)

    def set_Clock(self, hours, minutes, seconds):
        """
        The parameters hours, minutes and seconds have to be 
        integers and must satisfy the following equations:
        0 <= h < 24
        0 <= m < 60
        0 <= s < 60
        """

        if type(hours) == int and 0 <= hours and hours < 24:
            self._hours = hours
        else:
            raise TypeError("Hours have to be integers between 0 and 23!")
        if type(minutes) == int and 0 <= minutes and minutes < 60:
            self.__minutes = minutes 
        else:
            raise TypeError("Minutes have to be integers between 0 and 59!")
        if type(seconds) == int and 0 <= seconds and seconds < 60:
            self.__seconds = seconds
        else:
            raise TypeError("Seconds have to be integers between 0 and 59!")

    def __str__(self):
        return "{0:02d}:{1:02d}:{2:02d}".format(self._hours,
                                                self.__minutes,
                                                self.__seconds)

    def tick(self):
        """
        This method lets the clock "tick", this means that the 
        internal time will be advanced by one second.

        Examples:
        >>> x = Clock(12,59,59)
        >>> print(x)
        12:59:59
        >>> x.tick()
        >>> print(x)
        13:00:00
        >>> x.tick()
        >>> print(x)
        13:00:01
        """

        if self.__seconds == 59:
            self.__seconds = 0
            if self.__minutes == 59:
                self.__minutes = 0
                if self._hours == 23:
                    self._hours = 0
                else:
                    self._hours += 1
            else:
                self.__minutes += 1
        else:
            self.__seconds += 1


if __name__ == "__main__":
    x = Clock(23,59,59)
    print(x)
    x.tick()
    print(x)
    y = str(x)
    print(type(y))
----------------------------------------------
o/p:
$ python3 clock.py 
23:59:59
00:00:00
<class 'str'>
```
Calander class :
Check for leap yr :
1. If a year is divisible by 400, it is a leap year.
2. If a year is not divisible by 400 but by 100, it is not a leap year.
3. A year number which is divisible by 4 but not by 100, it is a leap year.
4. All other year numbers are common years, i.e. no leap years.
```
class Calendar(object):

    months = (31,28,31,30,31,30,31,31,30,31,30,31)
    date_style = "British"

    @staticmethod
    def leapyear(year):
        """ 
        The method leapyear returns True if the parameter year
        is a leap year, False otherwise
        """
        if not year % 4 == 0:
            return False
        elif not year % 100 == 0:
            return True
        elif not year % 400 == 0:
            return False
        else:
            return True


    def __init__(self, d, m, y):
        """
        d, m, y have to be integer values and year has to be 
        a four digit year number
        """

        self.set_Calendar(d,m,y)


    def set_Calendar(self, d, m, y):
        """
        d, m, y have to be integer values and year has to be 
        a four digit year number
        """

        if type(d) == int and type(m) == int and type(y) == int:
            self.__days = d
            self.__months = m
            self.__years = y
        else:
            raise TypeError("d, m, y have to be integers!")


    def __str__(self):
        if Calendar.date_style == "British":
            return "{0:02d}/{1:02d}/{2:4d}".format(self.__days,
                                                   self.__months,
                                                   self.__years)
        else: 
            # assuming American style
            return "{0:02d}/{1:02d}/{2:4d}".format(self.__months,
                                                   self.__days,
                                                   self.__years)



    def advance(self):
        """
        This method advances to the next date.
        """

        max_days = Calendar.months[self.__months-1]
        if self.__months == 2 and Calendar.leapyear(self.__years):
            max_days += 1
        if self.__days == max_days:
            self.__days= 1
            if self.__months == 12:
                self.__months = 1
                self.__years += 1
            else:
                self.__months += 1
        else:
            self.__days += 1


if __name__ == "__main__":
    x = Calendar(31,12,2012)
    print(x, end=" ")
    x.advance()
    print("after applying advance: ", x)
    print("2012 was a leapyear:")
    x = Calendar(28,2,2012)
    print(x, end=" ")
    x.advance()
    print("after applying advance: ", x)
    x = Calendar(28,2,2013)
    print(x, end=" ")
    x.advance()
    print("after applying advance: ", x)
    print("1900 no leapyear: number divisible by 100 but not by 400: ")
    x = Calendar(28,2,1900)
    print(x, end=" ")
    x.advance()
    print("after applying advance: ", x)
    print("2000 was a leapyear, because number divisibe by 400: ")
    x = Calendar(28,2,2000)
    print(x, end=" ")
    x.advance()
    print("after applying advance: ", x)
    print("Switching to American date style: ")
    Calendar.date_style = "American"
    print("after applying advance: ", x)  
-----------------------------------------------------------------
o/p:
$ python3 calendar.py
31.12.2012 after applying advance:  01.01.2013
2012 was a leapyear:
28.02.2012 after applying advance:  29.02.2012
28.02.2013 after applying advance:  01.03.2013
1900 no leapyear: number divisible by 100 but not by 400: 
28.02.1900 after applying advance:  01.03.1900
2000 was a leapyear, because number divisibe by 400: 
28.02.2000 after applying advance:  29.02.2000
Switching to American date style: 
after applying advance:  02/29/2000
```
CalendarClock, which will inherit from both Clock and Calendar. The method "tick" of Clock will have to be overridden. However, the new tick method of CalendarClock has to call the tick method of Clock: Clock.tick(self) 
```
from clock import Clock
from calendar import Calendar


class CalendarClock(Clock, Calendar):
    """ 
        The class CalendarClock implements a clock with integrated 
        calendar. It's a case of multiple inheritance, as it inherits 
        both from Clock and Calendar      
    """

    def __init__(self,day, month, year, hour, minute, second):
        Clock.__init__(self,hour, minute, second)
        Calendar.__init__(self,day, month, year)


    def tick(self):
        """
        advance the clock by one second
        """
        previous_hour = self._hours
        Clock.tick(self)
        if (self._hours < previous_hour): 
            self.advance()

    def __str__(self):
        return Calendar.__str__(self) + ", " + Clock.__str__(self)


if __name__ == "__main__":
    x = CalendarClock(31,12,2013,23,59,59)
    print("One tick from ",x, end=" ")
    x.tick()
    print("to ", x)

    x = CalendarClock(28,2,1900,23,59,59)
    print("One tick from ",x, end=" ")
    x.tick()
    print("to ", x)

    x = CalendarClock(28,2,2000,23,59,59)
    print("One tick from ",x, end=" ")
    x.tick()
    print("to ", x)

    x = CalendarClock(7,2,2013,13,55,40)
    print("One tick from ",x, end=" ")
    x.tick()
    print("to ", x)
-----------------------------------------------------------------------
o/p:
$ python3 calendar_clock.py 
One tick from  31/12/2013, 23:59:59 to  01/01/2014, 00:00:00
One tick from  28/02/1900, 23:59:59 to  01/03/1900, 00:00:00
One tick from  28/02/2000, 23:59:59 to  29/02/2000, 00:00:00
One tick from  07/02/2013, 13:55:40 to  07/02/2013, 13:55:41
```
### The Diamond Problem or the ,,deadly diamond of death''
Is the generally used term for an ambiguity that arises when two classes B and C inherit from a superclass A, and another class D inherits from both B and C. If there is a method "m" in A that B or C (or even both of them) )has overridden, and furthermore, if does not override this method, then the question is which version of the method does D inherit? It could be the one from A, B or C.
```
class A:
    def m(self):
        print("m of A called")

class B(A):
    def m(self):
        print("m of B called")
    
class C(A):
    def m(self):
        print("m of C called")

class D(B,C):
    pass
```
If you call the method m on an instance x of D, i.e. x.m(), we will get the output "m of B called". And if you change the order B,C into C,B then o/p will be "m of C called". 

### super and MRO
The order is defined by the so-called "Method Resolution Order" or in short MRO. To solve Diamond problem. We will extend our previous example, so that every class defines its own method m: 
```
class A:
    def m(self):
        print("m of A called")

class B(A):
    def m(self):
        print("m of B called")
    
class C(A):
    def m(self):
        print("m of C called")

class D(B,C):
    def m(self):
        print("m of D called")
```
We can see that only the code of the method m of D will be executed. We can also explicitly call the methods m of the other classes via the class name, as we demonstrate in the following interactive Python session: 
```
from super1 import A,B,C,D
>>> x = D()
>>> B.m(x)
m of B called
>>> C.m(x)
m of C called
>>> A.m(x)
m of A called
----------------------------------
class D(B,C):
    def m(self):
        print("m of D called")
        B.m(self)
        C.m(self)
        A.m(self)
```
The optimal way to solve the problem, which is the "super" pythonic way, consists in calling the super function:
```
class A:
    def m(self):
        print("m of A called")

class B(A):
    def m(self):
        print("m of B called")
        super().m()
    
class C(A):
    def m(self):
        print("m of C called")
        super().m()

class D(B,C):
    def m(self):
        print("m of D called")
        super().m()
---------------------------------------
>>> from super5 import D
>>> x = D()
>>> x.m()
m of D called
m of B called
m of C called
m of A called
```

### Polymorphism
Polymorphic functions or methods can be applied to arguments of different types, and they can behave differently depending on the type of the arguments to which they are applied. We can also define the same function name with a varying number of parameter.
Python is implicitly polymorphic. We can apply our previously defined function f even to lists, strings or other types, which can be printed: 
```
def f(x, y):
    print("values: ", x, y)

f(42, 43)
f(42, 43.7) 
f(42.3, 43)
f(42.0, 43.9)
>>> f([3,5,6],(3,5))
values:  [3, 5, 6] (3, 5)
>>> f("A String", ("A tuple", "with Strings"))
values:  A String ('A tuple', 'with Strings')
>>> f({2,3,9}, {"a":3.4,"b":7.8, "c":9.04})
values:  {9, 2, 3} {'a': 3.4, 'c': 9.04, 'b': 7.8}
```

## Magic Methods and Operator Overloading
They are the methods with this clumsy syntax, i.e. the double underscores at the beginning and the end. "Double underscore init double underscore" is a lot better, but the ideal way is "dunder init dunder"1 That's why magic methods methods are sometimes called dunder methods! 
The mechanism works like this: If we have an expression "x + y" and x is an instance of class K, then Python will check the class definition of K. If K has a method __add__ it will be called with x.__add__(y), otherwise we will get an error message.

## Slots
##### Avoiding Dynamically Created Attributes
The attributes of objects are stored in a dictionary "__dict__". In other words, you can add elements to dictionaries after they have been defined, as we have seen in our chapter on dictionaries. This is the reason, why you can dynamically add attributes to objects of classes that we have created so far: 
```
>>> class A(object):
...     pass
... 
>>> a = A()
>>> a.x = 66
>>> a.y = "dynamically created attribute"
------------------------------------------
>>> a.__dict__
{'y': 'dynamically created attribute', 'x': 66}
```
You might have wondered that you can dynamically add attributes to the classes, we have defined so far, but that you can't do this with built-in classes like 'int', or 'list':
```
>>> x = 42
>>> x.a = "not possible to do it"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'int' object has no attribute 'a'
>>> 
>>> lst = [34, 999, 1001]
>>> lst.a = "forget it"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'list' object has no attribute 'a'
```
Using a dictionary for attribute storage is very convenient, but it can mean a waste of space for objects, which have only a small amount of instance variables. The space consumption can become critical when creating large numbers of instances. Slots are a nice way to work around this space consumption problem. Instead of having a dynamic dict that allows adding attributes to objects dynamically, slots provide a static structure which prohibits additions after the creation of an instance. 
When we design a class, we can use slots to prevent the dynamic creation of attributes. To define slots, you have to define a list with the name __slots__. The list has to contain all the attributes, you want to use. We demonstrate this in the following class, in which the slots list contains only the name for an attribute "val". 
```
class S(object):

    __slots__ = ['val']

    def __init__(self, v):
        self.val = v


x = S(42)
print(x.val)

x.new = "not possible"
```
## Difference b/w class method and static method.
[Difference b/w class and static method with an example](https://www.geeksforgeeks.org/class-method-vs-static-method-python/)

## Metaclasses
A metaclass is a class whose instances are classes. Metaclasses are defined like any other Python class, but they are classes that inherit from "type". If no "metaclass" keyword is passed after the base classes (there may be no base classes either) of the class header, type() (i.e. __call__ of type) will be called. If a metaclass keyword is used on the other hand, the class assigned to it will be called instead of type.
We have learned in our chapter "Type and Class Relationship" that after the class definition has been processed, Python calls
```
type(classname, superclasses, attributes_dict)
```
#### Creating singleton using metaclasses
The singleton pattern is a design pattern that restricts the instantiation of a class to one object.
```
class Singleton(type):
    _instances = {}
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instances[cls]
    
    
class SingletonClass(metaclass=Singleton):
    pass
class RegularClass():
    pass
x = SingletonClass()
y = SingletonClass()
print(x == y)
x = RegularClass()
y = RegularClass()
print(x == y)
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
9. [Docker Docs](https://docs.docker.com/engine/docker-overview/)
10. [Rules how to write clear python code](https://www.python.org/dev/peps/pep-0008/)
11. [Read this book before starting python](https://effectivepython.com/)
12. [Databse Schema creation](https://beginnersbook.com/2015/05/normalization-in-dbms/)
