# XML
XML documents form a element tree structure that starts at "the root" and branches to "the leaves".
```
<?xml version="1.0" encoding="UTF-8"?>
<bookstore>
  <book category="cooking">
    <title lang="en">Everyday Italian</title>
    <author>Giada De Laurentiis</author>
    <year>2005</year>
    <price>30.00</price>
  </book>
  <book category="children">
    <title lang="en">Harry Potter</title>
    <author>J K. Rowling</author>
    <year>2005</year>
    <price>29.99</price>
  </book>
  <book category="web">
    <title lang="en">Learning XML</title>
    <author>Erik T. Ray</author>
    <year>2003</year>
    <price>39.95</price>
  </book>
</bookstore>
```
## Root Element
XML doc must have a root element that contains all the tag like (<bookstore>).

## The XML prolog
The XML prolog is optional. If it exists, it must come first in the document.
```
<?xml version="1.0" encoding="UTF-8"?>
```
## Rules
1. All element has closing tag.
2. Every tag should be closed properly.
3. Every tag must should be properly nested .
```
<b><i>This text is bold and italic</b></i> #this is wrong
```
4. All element tag are case sensitive.
```
<Letter> and <letter> are differnt in xml.
5. Attribute must always be quoted.
```
<book category="children"> # here children is attribute.
```
6. Use entity reference because some elemet has special meaning in xml like "<"(This means start of an element).
```
<message>salary < 1000</message> #error
<message>salary &lt; 1000</message> # use entity reference.
```
There are 5 pre-defined entity references in XML:
```
* &lt;   -> less than
* &gt;   -> greater than
* &amp;  -> ampersand(&)
* &apos; -> apostrophe(')
* &quot; -> quotation mark(")
```
7. Comments in xml
```
<!-- comments -->
```
8. White space is preserved in xml.

## XML Element
An XML element is everything from (including) the element's start tag to (including) the element's end tag. An elemnt can
contain text, attribute, other element or mix of these.
1. <title>, <author>, <year>, and <price> have text content because they contain text (like 29.99).
2. <bookstore> and <book> have element contents, because they contain elements.
3. <book> has an attribute (category="children").
### Empty xml element
Empty elemnt can have attribute.
use <element /> or <element></element>
### Naming rules
1. Element name are case sensitive.
2. Element names must start with a letter or underscore
3. Element names cannot start with the letters xml (or XML, or Xml, etc). You can use any other name.
4. Element names can contain letters, digits, hyphens, underscores, and periods.
5. Avoid "-". If you name something "first-name", some software may think you want to subtract "name" from "first".
6. Avoid ".". If you name something "first.name", some software may think that "name" is a property of the object "first".
7. Avoid ":". Colons are reserved for namespaces (more later).
8. Non-English letters like éòá are perfectly legal in XML, but watch out for problems if your software doesn't support them.

### Naming style
Follow one style through out.
1. lowercase
2. UPPERCASE
3. underscore (<first_name>)
4. Pascal Case (<FirstName>)
5. Camel Case (<firstName>)

## XML Attribute
Attributes are designed to contain data related to a specific element.
XML attribute must be quoted.
```
<person gender="female">
<person gender='female'>
<gangster name="George &quot;Shotgun&quot; Ziegler"> # if an attribute iteself contain quotes.
```
### Rules
1. attributes cannot contain multiple values (elements can)
2. attributes cannot contain tree structures (elements can)
3. attributes are not easily expandable (for future changes)
Don't do this
```
<note day="10" month="01" year="2008"
to="Tove" from="Jani" heading="Reminder"
body="Don't forget me this weekend!">
</note>
```
### Storing meta data
What I'm trying to say here is that metadata (data about data) should be stored as attributes, and
the data itself should be stored as elements. We use id for this.
```
<messages>
  <note id="501">
    <to>Tove</to>
    <from>Jani</from>
    <heading>Reminder</heading>
    <body>Don't forget me this weekend!</body>
  </note>
  <note id="502">
    <to>Jani</to>
    <from>Tove</from>
    <heading>Re: Reminder</heading>
    <body>I will not</body>
  </note>
</messages>
```


