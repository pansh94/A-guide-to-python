# A-guide-to-python
This is what I have learned in python untill now.
## Introduction to Memory
Python allocates memory transparently, manages objects using a reference count system, and frees memory when an object’s reference count falls to zero. 
**(Names -> Reference -> Object)**
Names are just label, actual value stores in Object. Object maintain reference count to know how many names are link to it.
**Python Memory Manager** use private heap for storing objects and to satisfy all memory related needs.Memory reclamation is mostly handled by reference counting. That is, the Python VM keeps an internal journal of how many references refer to an object, and automatically garbage collects it when there are no more references referring to it. In addition, there is a mechanism to break circular references (which reference counting can't handle) by detecting unreachable "islands" of objects, somewhat in reverse of traditional GC algorithms that try to find all the reachable objects.
**Generation Garbage Collection** : It makes a generation list i.e (generation1,generation2,generation3). Newly creted object store in *gereration 0*. Only obj rc>0 is store in generation list. When no. of obj stored in generation is above a threshold then python run a garbage collection  algo on that generation and generation younger than that. It maintains a discard list, if an obj is not referenced from outside it added to discard list. After completion of algo it deletes the discard list. The obj who survives this are promoted to higher level.    


## Useful links :
1. [Jupyter Notebook Documentation](http://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/what_is_jupyter.html)
2. [SQLAlchemy basic tutorial](http://docs.sqlalchemy.org/en/latest/core/engines.html#database-urls) 
3. [Basic Python Tutorial](https://www.python-course.eu/python3_course.php)
4. [Fast Track Analytical Python Tutorial](https://www.analyticsvidhya.com/blog/2016/01/complete-tutorial-learn-data-science-python-scratch-2/)
5. [Think Stat Book for Data Scientist](http://greenteapress.com/thinkstats2/thinkstats2.pdf)
6. [Basic of memory management.](https://www.slideshare.net/nnja/memory-management-in-python-the-basics)
7. [Matplotlib Tutorial](https://matplotlib.org/gallery.html)
8. [Django with SQLAlchemy](https://www.compose.com/articles/using-postgresql-through-sqlalchemy/)

