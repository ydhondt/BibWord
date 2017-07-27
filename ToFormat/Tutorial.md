## BibWord Tutorial
#### Introduction
BibWord is created with the idea that creating a new bibliography style for Word should be pretty straightforward. On this page, a step by step tutorial will be given on how to create a new style from scratch. Currently the page is only partially finished so if you happen to pass by, please check back in a couple of days while I keep adding more information and examples to it.
#### Prerequisites
Word uses an xml based toolchain to generate bibliographies. The different sources used in a bibliography are stored in an xml file while the style definition is stored in an xslt file. Both those files are send to a transformation engine which transforms the xml file into an html file (i.e. a normal web page) containing your formatted bibliography. This html file is then inserted into your Word document.

Using BibWord does not require any knowledge of xslt. Having some very basic knowledge of xml (what are tags, what is an xml tree, what is an xml comment) is advised though. If you really have no idea what xml is, you might want to check out the [w3schools.com](http://www.w3schools.com/xml/default.asp) tutorial on xml. If you get past the first 6 to 7 pages, you will have more than enough knowledge to be able to work with BibWord.

As you are editing a xml file while creating a style, having an xml editor will simplify your work. However, you can stick to notepad if you wish.
#### Conditional format strings
BibWord provides the functionality to format output strings based on the availability of certain elements in a source. Its formatting scheme is very easy but allows for highly complicated formatting. This scheme forms the core of BibWord. It consists out of two parts: parameters and conditional brackets. If you understand how to use those, creating a style comes down to inserting a few xml tags which such conditional format strings.

##### Parameters
Parameters are defined in between _**%**_ marks. A basic example of a parameter is

{{
  %First%
}}which represents the first name of a person. For a full list of all parameter, please check the guide coming with BibWord.

Extra options can be assigned to a parameter by adding a _**:**_ after the parameter's name followed by one or more symbols. An example is:

{{
  %First:u%
}}which indicates that the first name should be displayed in upper case characters. For a full list of all options of each parameter, please check the guide coming with BibWord.

Parameters can also be combined so that if the first part has no value, the value of a second or latter part can be displayed instead. This type of parameter or'ing is done using the _**|**_ symbol. An example is:

{{
  %First:l|Last:u%
}}which indicates that if the first name is available, it should be displayed in lower case characters. If it is not available, then the last name of a person should be displayed in upper case characters if available.

Instead of parameters, literal strings can also be used. This is done by placing the string between quotes. An example is:

{{
  %Last:u|"Anon"%
}}which displays the last name of a person in upper case characters if available. If the last name is not available, "Anon", without the quotes, will be displayed. Using literal strings it is possible to guarantee that always something will be shown.

##### Conditional brackets
_**{**_ and _**}**_ are used by BibWord as conditional brackets. Conditional brackets which ensure that data only gets displayed if each parameter within the conditional brackets has a value. That way leading or trailing literal strings will not be displayed if not required. A basic example is:

{{
  {Vol. %Volume%}
}}which displays "Vol. x" if the variable Volume has an actual value, or displays nothing otherwise. Note that if you do an or'ing of a parameter with a literal string, there will always be output shown. For example:

{{
  {%Edition|"1st"% edition}
}}will display the content of the Edition parameter if available followed by " edition" or, if the parameter is not available, just "1st edition". So in this example, the conditional brackets are not required.

Conditional brackets can also be nested. This can be interesting if the way you wish to display a parameter depends on the availability of other parameters. Take the following example:

{{
  { %Volume%{(%Issue%)} }{ no. %Issue% }
}}Depending on the availability of the two parameters Volume and Issue there are 4 different outcomes:
* Volume is available, Issue is available: Volume(Issue) 
* Volume is available, Issue is _not_ available: Volume 
* Volume is _not_ available, Issue is available: no. Issue 
* Volume is _not_ available, Issue is _not_ available: -
Note that unless the _**r**_ option is added to a parameter, a parameter can only be used once. That explains why you do not get "Volume(Issue) no. Issue" for the first outcome.

_**Warning:** Conditional brackets should always be used in a balanced way. When your style does not output anything, you should verify if your brackets are really balanced._
#### Getting started
Now that you know how to format output strings, you are ready to create your own style. Start by getting the latest version of [BibWord](BibWord). You can find a download link at the top of the [BibWord page](BibWord). Open the file in your favourite editor. Everything defining your style is held in one element:

{code:xml}
<xsl:variable name="data">
  <!-- A bunch of data. -->
</xsl:variable>
{code:xml}
Everything outside this element, which is over 2500 lines of code, can be ignored. By default, the template comes with some predefined examples but for the sake of this tutorial, I suggest you start by replacing the entire xsl variable by the following code:

{code:xml}
<xsl:variable name="data">
  <general></general>
  <importantfields></importantfields>
  <citation></citation>
  <bibliography></bibliography>
  <namelists></namelists>
  <strings></strings>
  <extensions></extensions>
</xsl:variable>
{code:xml}
In the remainder of this tutorial, each of these seven elements will be filled with data. Note that this tutorial only covers the basics of BibWord, for an overview of all possibilities, you will have to check the guide.
#### Making your style show up in Word
The first thing you want to do when creating a new style, is making it show up in the style dropdown menu in Word. To do so, you need to add two elements to the _general_ element:
* **_stylename_**: the name by which the style is identified in the dropdown style menu in Word; _**(required)**_
* **_version_**: identifies the version of the style in case there are multiple styles with the same name. _version_ is a date in the _yyyy.mm.dd_ format; _**(required)**_
So if we wanted to create a style named "Harvard - Exeter*" on "February 8, 2009", the _general_ element would look like this:

{code:xml}
    <general>
      <stylename>Harvard - Exeter*</stylename>
      <version>2009.02.08</version>
    </general>
{code:xml}
Now if you copy the style into the Word style directory, and (re)start Word, your style will be in the dropdown list. You could add more child elements to the _general_ element to provide extra information, but this is actually all the basics you need. 

_**Note**: style names are loaded only once by Word. So if you change the name of a style, you will have to restart Word to see the new name show up in Word. All other parts of a style can be edited while Word is open as Word reloads the stylesheet whenever it needs it. This allows for easy testing of your style during development._