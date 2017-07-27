# Non BibWord specific frequently asked questions

* [Is it possible to group several citations?](#Q1)
* [Is it possible to order the different sources in an in-text citation by year, author, ...?](#Q2)
* [Is it possible to add grouping logic to citations?](#Q3)
* [Why is Word 2007 not displaying the HTML output of the stylesheet correctly?](#Q4)
* [How can I set the position of a source in my bibliography as a value for my in-text citation?](#Q5)
* [How do I change the brackets around an in-text citation?](#Q6)
* [How do I change the separator between multiple grouped citations?](#Q7)
* [How do I change the name of one of the pre-defined Word Bibliography styles?](#Q8)
* [How can I manually update a citation field?](#Q9) 

{anchor:Q1}
**Q:** Is it possible to group several citations? Currently I have something like {"[1](1)[2](2)"} and I want {"[1,2](1,2)"}.

_**A:** Yes. You can add a second source to a citation by using the '\m' switch and the tag of the source you want to add. In Word 2007, if you want to add a source with tag 'Bee99' to an existing citation, right click the citation and select 'Edit Field...'. It will show you something like 'CITATION Gup97 \l 2060'. To add the extra source, change it to 'CITATION Gup97 \l 2060 \m Bee99'. For more information, also see the Microsoft Office online help topic on the [CITATION](http://office.microsoft.com/en-us/word/HA102157071033.aspx) field code._

_Alternatively, you can put your cursor inside any in-text citation, then go to 'References' tab in the ribbon and click 'Insert Citation'._


{anchor:Q2}
**Q:** Is it possible to order the different sources in an in-text citation by year, author, ...?

_**A:** No. Word 2007 passes the source of each citation in a group of in-text citations separately to the stylesheet. Hence it is not possible to put any ordering logic into the formatting process. However, you can set the order yourself in your citation field. For example 'CITATION \l 2060 Gup97 \m Bee99' while show the citation with tag 'Gup97' before the citation with tag 'Bee99' while 'CITATION \l 2060 Bee99 \m Gup97' will do the opposite._


{anchor:Q3}
**Q:** Is it possible to add grouping logic to citations? Currently I have something like {"[1,2,3,4](1,2,3,4)"} and I want something like {"[1-4](1-4)"}.

_**A:** No. This is not possible for two reasons. Firstly, if you try to display no information about a certain source in a group, an '**Invalid source specified.**' warning will be displayed in your text. Hence, it is not possible to skip sources elements. Secondly, Word 2007 passes the source of each citation in a group of in-text citations separately to the stylesheet. Hence, it is not possible to add any inter-source logic to the formatting process._

_Of course it is possible to convert a citation to static text (click the dropdown arrow next to the citation and select 'Convert citation to static text') and then edit the result to however you want the in-text citation to look._


{anchor:Q4}
**Q:** I created a stylesheet which outputs valid HTML. Why is Word 2007 not displaying it correctly?

_**A:** There are several possible answers here. Firstly, Word 2007 only supports a limited subset of the HTML specification. For an overview of what is supported and what is not, you should read the [Word 2007 HTML and CSS Rendering Capabilities](http://msdn.microsoft.com/en-us/library/aa338200.aspx) article on the MSDN website. Secondly, in case of in-text citations, the citation inherits the paragraph style of the paragraph it belongs to. So for example, it is not possible to display the in-text citation as superscript through the stylesheet unless the entire paragraph is set to superscript. However, you can create a 'Character' style in Word 2007 and apply that to a citation field. It will remain applied, even if you reformat all citation and bibliography fields by selecting the reference style from the reference tab._ 


{anchor:Q5}
**Q:** My stylesheet orders the entries in a bibliography in a specific order. How can I set the position of a source in my bibliography as a value for my in-text citation?

_**A:** You can not. In-text citations, even those part of a group, are passed one at a time to the stylesheet. The part of the stylesheet, formatting the in-text citation, is not able to either retrieve the order of the source in the bibliography from somewhere or calculate it somehow. However, stylesheets created with the [BibWord](BibWord) template can make use of the [BibWord Extender](BibWord-Extender) tool to get around this issue._



{anchor:Q6}
**Q:** How do I change the brackets around an in-text citation?

_**A:** When a b:Citation element is send to the stylesheet, it contains a b:FirstAuthor element if this is the first citation in the group and a b:LastAuthor element if it is the last citation in the group. The availability of these elements is used to decide if a bracket should be displayed or not. Hence, if you want to change the bracket in the stylesheet, you might want to look for a piece of code which looks similar to this:_

```xml
  <xsl:if test="/b:Citation/b:FirstAuthor">
    <!-- Comment: place your open bracket here. -->
    <xsl:text>[</xsl:text>
  </xsl:if>

  ...
  
  <xsl:if test="/b:Citation/b:LastAuthor">
    <!-- Comment: place your close bracket here. -->
    <xsl:text>]</xsl:text>
  </xsl:if>
```
{anchor:Q7}
**Q:** How do I change the separator between multiple grouped citations?

_**A:** When a b:Citation element is send to the stylesheet, it contains a b:LastAuthor element if this is the last citation in the group. So the separator between grouped citations is displayed when this element is not available. So you might want to look for a piece of code similar to the following:_
```xml
  <xsl:if test="not(/b:Citation/b:LastAuthor)">
    <!-- Comment: place your citation separator here. -->
    <xsl:text>;</xsl:text>
  </xsl:if>
```
{anchor:Q8}
**Q:** How do I change the name of one of the pre-defined Word Bibliography styles?

_**A:** The pre-defined styles use the b:OfficeStyleKey element to retrieve the name of the style. Those are mapped to hard-coded style names and can not be used to create your own style. To give a stylesheet a different name, one has to use the b:StyleName element instead. In the code, it comes down to changing:_
```xml
  <xsl:when test="b:OfficeStyleKey">
    <xsl:text>Some key (APA, ISO690NR, ...)</xsl:text>
  </xsl:when>
```
_into_
```xml
  <xsl:when test="b:StyleName">
    <xsl:text>The name of my style</xsl:text>
  </xsl:when>
```
{anchor:Q9}
**Q:** How can I manually update a citation field? 

_**A:** Citation fields added by means of the 'Insert citation' button on the ribbon are locked by default. [Jennifer Michelstein](http://blogs.msdn.com/joe_friend/archive/2006/07/13/664960.aspx) gives the following reason:_

```
Citations inserted from the UI are locked from manual edits; therefore you must insert the citation from scratch with CTRL+F9. Why? This was a design decision, since all fields in word lose manual edits when the field updates. Since bibliography fields automatically update when bibliography actions are taken, our early users were frequently losing their manual edits without understanding why. Now, most edits can be handled through the Edit Citation UI; “advanced” edits can be handled through field switches.
```

_However, citations are added by means of **Structured Data Tag** (SDT) elements. By selecting **Design Mode** from the **Developer** tab, the SDT elements and their result can be edited._