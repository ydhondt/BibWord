## Frequently Asked Questions for BibWord Styles
This pages contains answers regarding questions about the styles published through this project. For questions regarding the citations and bibliography tool in general, check the [developer FAQ](FAQ).

* [Why is a new style not showing up in Word when I add it to the Style directory?](#Q1)
* [In Word 2008, new styles are only added for citations. How can I use the new styles for bibliographies?](#Q2)
* [Why does it take Word so long to show the dropdown list with style names the first time?](#Q3)
* [Why do I get 'BO' instead of a number when using certain styles?](#Q4)
* [Why do certain styles have a * at the end of their name?](#Q5)
* [Can I request to get a certain style?](#Q6)
* [Can I (not) link my in-text citations to their bibliography entries?](#Q7)
* [Can I change the surrounding brackets for in-text citations?](#Q8)
* [How do I get my in-text citations in superscript?](#Q9) 
* [How do I convert all my in-text citations to static text in one go?](#Q10)
* [Is there an easy way to get rid of sources which are not cited in the text?](#Q11)
* [How do I set the indentation of my bibliography?](#Q12)
* [Is it possible to group several citations? Currently I have something like (1)(2) and I want (1,2).](#Q13)
* [Only the name of the first author is displayed correctly, all other author names are abbreviated. Is this a bug?](#Q14)
* [When using a numbered style (e.g. IEEE), the number is wrapped over multiple lines. Is this a bug?](#Q15)
* [My in-text citations are displayed in bold. How do I change this?](#Q16)

<a name="Q1"></a>
_**Q:**_ Why is a new style not showing up in Word when I add it to the Style directory?

_**A:** The list of available reference styles gets loaded only once. So when you add a new style to the style directory, you need to restart Word._

<a name="Q2"></a>
_**Q:**_ In Word 2008, new styles are only added for citations. How can I use the new styles for bibliographies?

_**A:** Add the bibliography using one of the four predefined styles. Then go to the citation toolbox and select the style you want. This will update all the citations and bibliographies in your text to the new style._

<a name="Q3"></a>
_**Q:**_ Why does it take Word so long to show the dropdown list with style names the first time?

_**A:** Word has to retrieve the style names of every XSL in the style directory the first time. Hence, the more styles you put in the directory, the more time Word needs to fill the drop down list._

<a name="Q4"></a>
_**Q:**_ Why do I get 'BO' instead of a number when using certain styles?
_**A:** 'BO' is often printed when the BibOrder number is not available. Use the [BibWord Extender](BibWord-Extender) tool on the document to add the missing numbers._

<a name="Q5"></a>
_**Q:**_ Why do certain styles have a **{"**"}** at the end of their name?

_**A:** Although the usage of a **{"**"}** is not mandatory, it often indicates that part of the functionality of the style can only be used in combination with the [BibWord Extender](BibWord-Extender) tool._

<a name="Q6"></a>
_**Q:**_ Can I request to get a certain style?
_**A:** **No.** Using [BibWord](BibWord), you really should try to create the style yourself. Keep in mind that even if you find someone prepared to create the style for you that you will have to provide him/her with detailed information about the formatting guidelines for your style. Messages containing "I need style x." will most likely be ignored._

<a name="Q7"></a>
_**Q:**_ Can I (not) link my in-text citations to their bibliography entries?

_**A:** Yes. Set the value of **{"citation_as_link"}** to 'yes' if you want in-text citations to link to their specific bibliography entry, or to any other value if you do not._

<a name="Q8"></a>
_**Q:**_ Can I change the surrounding brackets for in-text citations?

_**A:** Yes. You can change the surrounding brackets by changing the values of **{"openbracket"}** and **{"closebracket"}**_
```xml
<openbracket>(</openbracket>
<closebracket>)</closebracket>
```

<a name="Q9"></a>
_**Q:**_ How do I get my in-text citations in superscript?

_**A:** In-text citations inherit the style of their surroundings. Only limited formatting (bold, underline, italic) can be applied to them through the reference style. For any further formatting, such as superscript, a character style has to be applied to all CITATION fields._

_The following macro creates a character style called **In-Text Citation** if it does not yet exist. When the style is newly created, it sets the font to superscript. Then the style is applied to all CITATION fields in the document. By changing/updating the style **In-Text Citation** you can then update the formatting of all citations_
 
```vbscript
Sub ApplyCitationStyle()
    Dim stylename As String
    Dim exists As Boolean
    Dim s As Style
    Dim fld As Field
                
    stylename = "In-Text Citation"
        
    ' Check if the style already exists.
    exists = False
        
    For Each s In ActiveDocument.Styles
        If s.NameLocal = stylename Then
           exists = True
           Exit For
        End If
    Next
    
    ' If the style did not exist yet, create it.
    If exists = False Then
        Set s = ActiveDocument.Styles.Add(stylename, wdStyleTypeCharacter)
        s.BaseStyle = ActiveDocument.Styles(wdStyleDefaultParagraphFont).BaseStyle
        s.Font.Superscript = True
    End If
    
    ' Now that the style really exists, select it.
    Set s = ActiveDocument.Styles(stylename)
     
    ' Apply the style to all in-text citations.
    For Each fld In ActiveDocument.Fields
        If fld.Type = wdFieldCitation Then
            fld.Select
            Selection.Style = s
        End If
    Next

End Sub
```

<a name="Q10"></a>
_**Q:**_ How do I convert all my in-text citations to static text in one go?

_**A:** You can use the following macro to convert all in-text citations:_

```vbscript
Sub CitationsToStaticText()
    Dim fld As Field
            
    ' Go over all stories, including main, footnotes, ...
    For Each sr In ActiveDocument.StoryRanges
        ' Find all citation fields and convert them to static text.
        For Each fld In sr.Fields
            If fld.Type = wdFieldCitation Then
                fld.Select
                WordBasic.BibliographyCitationToText
            End If
        Next
    Next

End Sub
```

<a name="Q11"></a>
_**Q:**_ Is there an easy way to get rid of sources which are not cited in the text?

_**A:** You can use the following macro to remove all uncited sources from a document:_

```vbscript
Sub RemoveUnusedCitations()
    ' Get the number of sources.
    idx = ActiveDocument.Bibliography.Sources.Count
    
    ' Remmove unused sources starting from the last one.
    Do While (idx > 0)
        If ActiveDocument.Bibliography.Sources(idx).Cited = False Then
            ActiveDocument.Bibliography.Sources(idx).Delete
        End If
        idx = idx - 1
    Loop
End Sub
```

<a name="Q12"></a>
_**Q:**_ How do I set the indentation of my bibliography?

_**A:** Add a bibliography to your document. Open the 'Styles' pane {"(CTRL+ALT+SHIFT+S)"} and look for a style called 'Bibliography' (or a localized translation of the word 'Bibliography'). Change the indentation settings there. That way, whenever your bibliography gets updated, the indentation will remain correct._

<a name="Q13"></a>
_**Q:**_ Is it possible to group several citations? Currently I have something like (1)(2) and I want (1,2).

_**A:** Yes. You can add a second source to a citation by using the '\m' switch and the tag of the source you want to add. In Word 2007, if you want to add a source with tag 'Bee99' to an existing citation, right click the citation and select 'Edit Field...'. It will show you something like 'CITATION Gup97 \l 2060'. To add the extra source, change it to 'CITATION Gup97 \l 2060 \m Bee99'. For more information, also see the Microsoft Office online help topic on the [CITATION](http://office.microsoft.com/en-us/word/HA102157071033.aspx) field code._

_Alternatively, you can put your cursor inside any in-text citation, then go to 'References' tab in the ribbon and click 'Insert Citation'._

_To change the separator between two grouped in-text citations, BibWord uses the **separator** element._

<a name="Q14"></a>
_**Q:**_ Only the name of the first author is displayed correctly, all other author names are abbreviated. Is this a bug?

_**A:** No. You probably made a mistake when entering the different author names. You should enter them one by one in the dialog that comes up when clicking the "Edit..." button next to the author field. That way you will not make a mistake._

_If you really want to enter them as a string, then be aware that the correct format is "Last1, First1 Middle1; Last2, First2 Middle2; ...". So the names are separated by a ";" while name parts are separated by a ","._

_Note that there is a bug in Word where sometimes the name conversion goes wrong. For more info, see [release:here](27834)._

<a name="Q15"></a>
_**Q:**_ When using a numbered style (e.g. IEEE), the number is wrapped over multiple lines. Is this a bug?

_**A:** No. Numbered styles are mostly represented using a 2 column table where the first column contains the number and the second column contains the text. The text wrap you see is caused by the first column not being wide enough. You can simple solve this by positioning your cursor on the the table border between the first and second column and drag it to the right._

_This can also be used to add extra white space after the number if you set the halign element to left of the first column._

<a name="Q16"></a>
_**Q:**_ My in-text citations are displayed in bold. How do I change this?

_**A:** If you link your in-text citation to your bibliography, Word formats the link using the 'Heading 2 Character style'. So if that style is configured to use bold, so will the in-text citation. Assuming you cannot or do not want to change that style, there are two possible solutions:_
* _Disable linking between in-text citations and bibliographies. This can be done easily be setting the value of **citation_as_link** to 'no' in the xsl file._
* _Format each in-text citation with another character style. This way you will be able to keep using the links between in-text citations and bibliographies. To ease this job, you could use the following macro which you can run every time you insert an in-text citation or once at the end._ 
```vbscript
Sub ApplyCitationStyle()
    Dim stylename As String
    Dim exists As Boolean
    Dim s As Style
    Dim fld As Field
                
    stylename = "In-Text Citation"
        
    ' Check if the style already exists.
    exists = False
        
    For Each s In ActiveDocument.Styles
        If s.NameLocal = stylename Then
           exists = True
           Exit For
        End If
    Next
    
    ' If the style did not exist yet, create it.
    If exists = False Then
        Set s = ActiveDocument.Styles.Add(stylename, wdStyleTypeCharacter)
        s.BaseStyle = ActiveDocument.Styles(wdStyleDefaultParagraphFont).BaseStyle
        s.Font.Bold = False
    End If
    
    ' Now that the style really exists, select it.
    Set s = ActiveDocument.Styles(stylename)
     
    ' Apply the style to all in-text citations.
    For Each fld In ActiveDocument.Fields
        If fld.Type = wdFieldCitation Then
            fld.Select
            Selection.Style = s
        End If
    Next

End Sub
```
