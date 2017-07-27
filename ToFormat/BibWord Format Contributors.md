## Formatting a set of contributors
### Table of Contents
* [Introduction](#intro)
* [Variables](#vars)
	* [Formatting a single name](#names)
	* [Combining names](#combination)
	* [Limiting the number of authors shown](#limitation)
	* [Prefix and suffix](#presuf)
* [Examples](#examples)
{anchor:intro}
### Introduction
Cited works often have multiple authors, editors, and other types of contributors. To format a set of contributors, BibWord use 13 variables. All the variables are grouped inside a single _list_ element which in turn belongs to the _namelists_ section of the BibWord _data_ element. To make this a bit more clear, consider the following definition of a list:

{code:xml}
  <xsl:variable name="data">
    <namelists>
      <list name="citation" id="1">
        <!-- The 13 variables influencing the look of a list of contributors. -->
      </list>
    </namelists>
  </xsl:variable>
{code:xml}
As you can see in the example, the _list_ element contains two attributes: _name_ and _id_.

The _id_ attribute is a mandatory numerical value and is used by [format strings](BibWord-Format-Strings) to know which _list_ to use when formatting a set of contributors. For example, a format string looking like {%Author:5%} indicates that the authors should be formatted according to the rules provided in the _list_ with an id equal to 5.

The _name_ attribute is optional and purely intended for developers. It can help them identify what the _list_ is used for. In the above example, it indicates that this list is probably used to format contributors in a citation.
{anchor:vars}
### Variables
{anchor:names}
#### Formatting a single name
BibWord provides three variables/elements to format names in a set of contributors:
* _{"first_person"}_: format information for the name of the first contributor in the set
* _{"other_persons"}_: format information for all contributors in the set except the first one
* _{"corporate"}_: format information in case the contributor is a corporation
Each of these three elements take a [format strings](BibWord-Format-Strings) as value. For example:

{code:xml}
  <first_person>{%Last:u%}{, %First%}</first_person>
  <other_persons>{%First% }{%Last%}</other_persons>
  <corporate>{%Corporate:u%}</corporate>
{code:xml}
{anchor:combination}
#### Combining names
When there is more than one contributor in a set, the contributors have to be grouped in some way. BibWord provides three variables to do this grouping:
* _{"separator_between_if_two"}_: the separator to display if there are only two contributors
* _{"separator_between_if_more_than_two"}_: the separator to display if there are more than two contributors
* _{"separator_before_last"}_: the separator to display before the last contributor if there are more than two contributors
For each of these elements, any possible string value can be used. For example:

{code:xml}
  <separator_between_if_two> and </separator_between_if_two>
  <separator_between_if_more_than_two>; </separator_between_if_more_than_two>
  <separator_before_last>; and </separator_before_last>
{code:xml}
{anchor:limitation}
#### Limiting the number of authors shown
Sometimes a set of contributors consists of more persons than the style allows to display. In such case, only a limited number of contributors is shown followed by a string like 'et al.' to indicate that there are more contributors. BibWord can do this by means of 3 elements:
* _{"max_number_of_persons_to_display"}_: indicates the maximum number of contributors that can be displayed
* _{"number_of_persons_to_display_if_more_than_max"}_: indicates the number of contributors to display if the set of contributors contains more contributors than the maximum number that can be displayed
* _{"overflow"}_: contains the string to display after the last contributor if there are too many contributers
The first two elements require a numerical value while for the last one any string value can be used. For example:

{code:xml}
  <max_number_of_persons_to_display>5</max_number_of_persons_to_display>
  <number_of_persons_to_display_if_more_than_max>1</number_of_persons_to_display_if_more_than_max>
  <overflow>, et al.</overflow>
{code:xml}
{anchor:presuf}
#### Prefix and suffix
Sometimes a set of contributors are no ordinary authors, they can be editors, translators, ... in which case you might want to add a prefix or suffix string like 'ed.' or 'eds.'.
* _{"single_prefix"}_: the text to display in front of the contributors if there is only one
* _{"single_suffix"}_: the text to display behind the contributors if there is only one
* _{"multi_prefix"}_: the text to display in front of the contributors if there is more than one 
* _{"multi_suffix"}_: the text to display behind the contributors if there is more than one

{code:xml}
  <single_prefix>Editor: </single_prefix>
  <single_suffix> (ed.)</single_suffix>
  <multi_prefix>Editors: </multi_prefix>
  <multi_suffix> (eds.)</multi_suffix>
{code:xml}
{anchor:examples}
### Examples
Consider the following contributor set:
* Mick Jagger
* Brian Jones
* Keith Richards
* Bill Wyman
* Charlie Watts
With the following output:
* JAGGER M. and B. Jones
* JAGGER M., B. Jones, K. Richards, and B. Wyman
* JAGGER M., _et al._
Ït is clear that the first contributor should be displayed in capitals with his last name first and his first name abbreviated:

{code:xml}
  <first_person>{%Last:u%}{ %First:asdp%}</first_person>
{code:xml}
The other contributors are formatted with an abbreviated first name followed by their last name:

{code:xml}
  <other_persons>{%First:asdp% }{%Last%}</other_persons>
{code:xml}
There is no information on how to format corporate contributors, but it seems logical that they follow the same rule as for the first contributor:

{code:xml}
  <corporate>{%Corporate:u%}</corporate>
{code:xml}
Now that the different names in the contributor set are formatted correctly, it is time to combine them. From the first example, one can see that if only two contributors are available, " and " is used to combine them. If there are more than two contributors, as in the second example, a comma is used for concatenating. Also, from the second example, one can see that ", and " is used to combine the last contributor with the others. So we get:

{code:xml}
  <separator_between_if_two> and </separator_between_if_two>
  <separator_between_if_more_than_two>, </separator_between_if_more_than_two>
  <separator_before_last>, and </separator_before_last>
{code:xml}
The last example shows what happens in case there are too many contributors to display them all. Only the first is displayed, followed by 'et al.' in italics. In combination with the second example, we see that 4 contributors can be displayed at most at one time. Hence, we get:

{code:xml}
  <max_number_of_persons_to_display>4</max_number_of_persons_to_display>
  <number_of_persons_to_display_if_more_than_max>1</number_of_persons_to_display_if_more_than_max>
  <overflow>, &lt;i&gt;et al.&lt;/i&gt;</overflow>
{code:xml}
Note that the overflow contains "&lt;i&gt;" which translates to the html tag "<i>" which is used to indicate that "et al." should be displayed in italics.

Combining all these items results in:

{code:xml}
  <xsl:variable name="data">
    <namelists>
      <list name="authors" id="2">
        <first_person>{%Last:u%}{ %First:asdp%}</first_person>
        <other_persons>{%First:asdp% }{%Last%}</other_persons>
        <corporate>{%Corporate:u%}</corporate>
        <separator_between_if_two> and </separator_between_if_two>
        <separator_between_if_more_than_two>, </separator_between_if_more_than_two>
        <separator_before_last>, and </separator_before_last>
        <max_number_of_persons_to_display>4</max_number_of_persons_to_display>
        <number_of_persons_to_display_if_more_than_max>1</number_of_persons_to_display_if_more_than_max>
        <overflow>, &lt;i&gt;et al.&lt;/i&gt;</overflow>
      </list>
    </namelists>
  </xsl:variable>
{code:xml}