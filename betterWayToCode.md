## 1. Don't use multiple or statement
This is more user friendly, readable and changeble.
```
if(i==20 or i==2 or i==3):

instead do
if_choices = [20,2,3] #choose meaningfull name here
if i in if_choices:
```

## 2. Multiple value
When you want to give several variables the same values, use the following method :
```
not this
a = var1
b = var1
c = var1
but this
a=b=c=var1
```
## 3. Variable swapping 
use
```
a,b = b,a
```

## 4. Exended slicing
Negative means start from the last and 1 means go one element at a time.
```
our_list = [1, 2, 3]
reversed_list = our_list[::-1]
# reversed_list == [3, 2, 1]
```

## 5. All and any
allow us to check if all elements in an iterable are True, and if any element is True.
```
all_true = [True, True, 1, 'hello']
any_true = [0, False, True, '', []]
# all(all_true) == True
# any(all_true) == True
# all(any_true) == False
# any(any_true) == True
```
