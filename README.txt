====== xml2json.xslt ======

version 0.3, Copyright (c) 2006, Doeke Zanstra, see COPYRIGHT.txt

xml2json.xslt is a XSLT 1.0 stylesheet to transform arbitrary XML to JSON. There is also a version for javascript. The workings are demonstrated with the accompanied xml files. The target of this library is to create javascript-like JSON, not XML-like JSON, like the BadgerFish convention (see Resources). 

The downside is it won't translate all markup, and there's a chance the JSON is invalid. However, if you control the XML, you will enjoy the merits of javascript. Enjoy ;-)

I will call this the Parker convention, after the comic Parker & Badger by Cuadrado. I can really recommend the comic, also in relation to this work and the BadgerFish convention.

====== Using ====== 

You can load the xml files in your browser (Firefox/Camino work, Safari doesn't, Internet Explorer 6 works, except test-js.xml). On windows, you can also try the commandline Msxsl utility (see resources). And of course you can fire up your favorite transformation engine.

====== Translation JSON ====== 

1. The root element will be absorbed, for there is only one
<root>test</root>
   becomes
"test"

2. Element names become object properties:
<root><name>Xml</name><encoding>ASCII</encoding></root>
   becomes
{"name":"Xml","encoding":"ASCII"}

3. Numbers are recognized (integers and decimals):
<root><age>12</age><height>1.73</height></root>
  becomes
{"age":12,"height":1.73}

4. Booleans are recognized case insensitive:
<root><checked>True</checked><answer>FALSE</answer></root>
  becomes
{"checked":true,"answer":false}

5. Strings are escaped:
<root>Quote: &quot; New-line:
</root>
  becomes
"Quote: \" New-line:\n"

6. Empty elements will become null:
<root><nil/><empty></empty></root>
  becomes
{"nil":null,"empty":null}

7. If all sibling elements have the same name, they become an array
<root><item>1</item><item>2</item><item>three</item></root>
   becomes
[1,2,"three"]

8. Mixed mode text-nodes, comments and attributes get absorbed:
<root version="1.0">testing<!--comment--><element test="true">1</element></root>
  becomes
{element:true}

9. Namespaces get absorbed, and prefixes will just be part of the property name:
<root xmlns:ding="http://zanstra.com/ding"><ding:dong>binnen</ding:dong></root>
   becomes
{"ding:dong":"binnen"}

====== Translation JS extra ====== 

All the same as the JSON translation, but with these extra's:

1. Property names are only escaped when necessary
<root><while>true</while><wend>false</wend><only-if/></root>
   becomes
{"while":true,wend:false,"only-if":null}

2. Within a string, closing elements "</" are escaped as "<\/"
<root><![CDATA[<script>alert("YES");</script>]]></root>
   becomes
{script:"<script>alert(\"YES\")<\/script>"}

3. Dates are created as new Date() objects
<root>2006-12-25</root>
   becomes
new Date(2006,12-1,25)

4. Attributes and comments are shown as comments (for testing-purposes):
<!--testing--><root><test version="1.0">123</test></root>
   becomes
/*testing*/{test/*@version="1.0"*/:123}

5. A bit of indentation is done, to keep things ledgible

====== Plans ====== 

I don't really have much plans with this library. It was mere a result of learning XSLT and XPath better. However, I will perform bug-fixes. Issues I might address, if I can find a satisfactory solution:

- convert attributes to properties in some way

- handle siblings with duplicate names, when there are also other names:
<root><item>1</item><item>2</item><name>test</name></root>
  becomes (faulty JSON)
{"item":1,"item":2,"name":"test"}

====== Resources ====== 

  * Xml: http://www.w3.org/TR/xml/
  * Xslt: http://www.w3.org/TR/xslt
  * XPath: http://www.w3.org/TR/xpath
  * Json: http://www.json.org
  * BadgerFish: http://badgerfish.ning.com/
  * Msxsl: http://www.microsoft.com/downloads/details.aspx?FamilyId=2FB55371-C94E-4373-B0E9-DB4816552E41&displaylang=en