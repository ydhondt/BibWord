## Format Strings
### Table of Contents
* [Introduction](#Intro)
* [Variables/Parameters](#Variables)
* [Conditional Brackets](#Conditions)
* [Examples](#Examples)
	* [Example bibliography format string](#ex1)
	* [Example contributor format strings](#ex2)
	* [Example sorting format strings](#ex3)
{anchor:Introduction}
### Introduction
Format strings are the core of BibWord. They are responsible for the way
* a (footnote)citation looks
* a bibliography entry looks
* a bibliography is sorted
* decisions are made for year suffices
* the name of a single contributor looks
Hence, understanding how format strings work, is understanding 90% of BibWord.
{anchor:Variables}
### Variables/Parameters
Variables or parameters are defined in between _**percentage**_ (_**%**_) marks. A basic example of a variable is

{{
     %First%
}}which represents the first name of a person. For a full list of all possible parameters, please check the guide coming with BibWord.

Extra options can be assigned to a parameter by adding a _**colon**_ (_**:**_) after the parameter's name followed by one or more symbols. An example is:

{{
     %First:u%
}}which indicates that the first name should be displayed in upper case characters. For a full list of all options of each parameter, please check the guide coming with BibWord.

Parameters can also be combined so that if the first part has no value, the value of a second or latter part can be displayed instead. This type of parameter or'ing is done using the _**vertical bar**_ (_**|**_) symbol. An example is:

{{
     %First:l|Last:u%
}}which indicates that if the first name is available, it should be displayed in lower case characters. If it is not available, then the last name of a person should be displayed in upper case characters if available.

Instead of parameters, literal strings can also be used. This is done by placing the string between double quotes. An example is:

{{
     %Last:u|"Anon"%
}}which displays the last name of a person in upper case characters if available. If the last name is not available, "Anon", without the quotes, will be displayed. Using literal strings it is possible to guarantee that always something will be shown.
##### Pages, a special parameter
The _Pages_ parameter does not follow the regular syntax. Instead, it consists of 3 sets of options separated by a _**colon**_ (_**:**_) after the parameter's name. The first set indicates the prefix to show if _Pages_ contains a single page number. The second indicates the prefix to show if _Pages_ contains a range of pages. The third one contains the remaining options. An example is:

{{
     %Pages:p. :pp. :a2%
}}
{anchor:Conditions}
### Conditional Brackets
_**Braces**_ (_**{"{"}**_ and _**{"}"}**_) are used by BibWord as conditional brackets. Conditional brackets ensure that data only gets displayed if each parameter within the conditional brackets has a value. That way, leading or trailing literal strings will not be displayed if not required. A basic example is:

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

_**Warning:** Conditional brackets should always be used in a balanced way. When your style does not output anything, you should verify if your brackets are balanced._
{anchor:Examples}
### Examples
{anchor:ex1}
#### Example bibliography format string
Consider the following journal articles:
* Doe, John. 2009. Creating bibliography styles using BibWord. _Modern Bibliography Tools_. **25**(3):513-22.
* Bibliographies throughout the ages. 2007. _Modern Bibliography Tools_. 3:15-18.
* Doe, Jane. 2005. Bibliographies for dummies. _Modern Bibliography Tools_. 51-57.
Now we are going to create a format string step by step.

The first element in the format string is the author followed by a period:

{{
  {%Author:1%.}
}}In case there is no author, like in the second example, the title is used instead:

{{
  {%Author:1|Title%.}
}}Next the year is displayed:

{{
  {%Author:1|Title%.}{ %Year%.}
}}After the year, the title is displayed:

{{
  {%Author:1|Title%.}{ %Year%.}{ %Title%.}
}}But what if there is no author? Then the title is already displayed in front of the year. This is not an issue for BibWord as by default, different variables can only be displayed once. So if the title is already displayed at the beginning, it won't be displayed anymore. 

Next we add the name of the journal:

{{
  {%Author:1|Title%.}{ %Year%.}{ %Title%.}{ %JournalName%.}
}}As you can see from the examples, the name of the journal is displayed in italics. The result of the formatting is actually html code, so to obtain italics, the html italics element <i></i> can be used:

{{
  {%Author:1|Title%.}{ %Year%.}{ %Title%.}{ &lt;i&gt;%JournalName%&lt;/i&gt;.}
}}Note that you should use &lt; for < and &gt; for >.

So far, the format string was pretty straightforward. Now comes a slightly more difficult part. The difficulty lies in the fact that the formatting changes depending on the elements that are available. The easiest solution is to consider each of the possible cases and generate a format string for them. So here we have:

{{
  { &lt;b&gt;%Volume%&lt;/b&gt;(%Issue%):%Pages:::a2%}
  { &lt;b&gt;%Volume%&lt;/b&gt;(%Issue%)}
  { &lt;b&gt;%Volume%&lt;/b&gt;:%Pages:::a2%}
  { %Issue%:%Pages:::a2%}
  { &lt;b&gt;%Volume%&lt;/b&gt;}
  { %Issue%}
  { %Pages:::a2%}
}}Two points to explain here. The first is the volume being displayed in bold. Just like the <i></i> element can be used for italics, the <b></b> element can be used for bold.

The 'a2' as options for the Pages parameter indicates that page ranges should be _a_bbreviated and that the second part of the range should have a length of at least 2.

As BibWord does not allow the reuse of the same element by default, you can put the different format strings after each other starting with those with the most parameters to cover all the cases:

{{
  { &lt;b&gt;%Volume%&lt;/b&gt;(%Issue%):%Pages:::a2%}{ &lt;b&gt;%Volume%&lt;/b&gt;(%Issue%)}{ &lt;b&gt;%Volume%&lt;/b&gt;:%Pages:::a2%}{ %Issue%:%Pages:::a2%}
  { &lt;b&gt;%Volume%&lt;/b&gt;}{ %Issue%}{ %Pages:::a2%}
}} So now that everything is covered, both parts can be put after each other, and a final period can be added:

{{
  {%Author:1|Title%.}{ %Year%.}{ %Title%.}{ &lt;i&gt;%JournalName%&lt;/i&gt;.}{ &lt;b&gt;%Volume%&lt;/b&gt;(%Issue%):%Pages:::a2%}
  { &lt;b&gt;%Volume%&lt;/b&gt;(%Issue%)}{ &lt;b&gt;%Volume%&lt;/b&gt;:%Pages:::a2%}{ %Issue%:%Pages:::a2%}
  { &lt;b&gt;%Volume%&lt;/b&gt;}{ %Issue%}{ %Pages:::a2%}.
}}
{anchor:ex2}
#### Example contributor format strings
{{
First : Henry
Middle: Alfred
Last  : Kissinger
}}
{{
KISSINGER               : {%Last:u%}
Kissinger               : {%Last%}
Kissinger, Henry        : {%Last%}{, %First%}
Kissinger, Henry Alfred : {%Last%}{, %First|Middle%}{ %Middle%}
Kissinger, H. A.        : {%Last%}{, %First:adsp|Middle:adsp%}{ %Middle:adsp%}
Kissinger, H.A.         : {%Last%}{, %First:adp|Middle:adp%}{%Middle:adp%}
Kissinger, H.           : {%Last%}{, %First:adp%}
Kissinger, H A          : {%Last%}{, %First:ads|Middle:ads%}{ %Middle:ads%}
Kissinger, HA           : {%Last%}{, %First:a|Middle:a%}{%Middle:a%}
Kissinger, H            : {%Last%}{, %First:as%}
Kissinger Henry         : {%Last%}{ %First%}
Kissinger Henry Alfred  : {%Last%}{ %First%}{ %Middle%}
Kissinger H. A.         : {%Last%}{ %First:adsp%}{ %Middle:adsp%}
Kissinger H.A.          : {%Last%}{ %First:adp|Middle:adp%}{%Middle:adp%}
Kissinger H.            : {%Last%}{ %First:adp%}
Kissinger H A           : {%Last%}{ %First:ads%}{ %Middle:ads%}
Kissinger HA            : {%Last%}{ %First:a|Middle:a%}{%Middle:a%}
Kissinger H             : {%Last%}{ %First:as%}
Henry Kissinger         : {%First% }{%Last%}
Henry Alfred Kissinger  : {%First% }{%Middle% }{%Last%}
H. A. Kissinger         : {%First:adsp% }{%Middle:adsp% }{%Last%}
H.A. Kissinger          : {%First:adp%}{%Middle:adp%}{ %Last%}
H. Kissinger            : {%First:adp%}{ %Last%}
H A Kissinger           : {%First:ads% }{%Middle:ads% }{%Last%}
HA Kissinger            : {%First:a%}{%Middle:a%}{ %Last%}
H Kissinger             : {%First:as%}{ %Last%}
}}
{anchor:ex3}
#### Example sorting format strings
{{
Order of appearance                   : nothing, this is the default setting for BibWord
Author + Title                        : {%Author:10%} {%Title:a%}
Author + Year + Title                 : {%Author:10|Title:a%} {%Year%} {%Title:a%}
First Author + Year + Other Authors   : {%Author:11r%} {%Year%} {%Author:12%}
First Author + # of Authors + Year    : {%Author:11r%} {%Author:c%} {%Year%}
Date (ascending - oldest first)       : {%Year|"0000"%}{%Month:n|"01"%}{%Day:n|"01"%}
Date (descending - most recent first) : {%Year:i|"9999"%}{%Month:in|"12"%}{%Day:in|"31"%}
}} with the following options for formatting the author:

{code:xml}
<list id="10" name="all_contributors">
  <single_prefix></single_prefix>
  <multi_prefix></multi_prefix>
  <corporate>{%Corporate%}</corporate>
  <first_person>{%Last|First|Middle%}{ %First%}{ %Middle%}</first_person>
  <other_persons>{%Last|First|Middle%}{ %First%}{ %Middle%}</other_persons>
  <separator_between_if_two> </separator_between_if_two>
  <separator_between_if_more_than_two> </separator_between_if_more_than_two>
  <separator_before_last></separator_before_last>
  <max_number_of_persons_to_display>500</max_number_of_persons_to_display>
  <number_of_persons_to_display_if_more_than_max>500</number_of_persons_to_display_if_more_than_max>
  <overflow></overflow>
  <single_suffix></single_suffix>
  <multi_suffix></multi_suffix>
</list>

<list id="11" name="first_contributor">
  <single_prefix></single_prefix>
  <multi_prefix></multi_prefix>
  <corporate>{%Corporate%}</corporate>
  <first_person>{%Last|First|Middle%}{ %First%}{ %Middle%}</first_person>
  <other_persons></other_persons>
  <separator_between_if_two> </separator_between_if_two>
  <separator_between_if_more_than_two> </separator_between_if_more_than_two>
  <separator_before_last></separator_before_last>
  <max_number_of_persons_to_display>500</max_number_of_persons_to_display>
  <number_of_persons_to_display_if_more_than_max>500</number_of_persons_to_display_if_more_than_max>
  <overflow></overflow>
  <single_suffix></single_suffix>
  <multi_suffix></multi_suffix>
</list>

<list id="12" name="second_and_later_contributor">
  <single_prefix></single_prefix>
  <multi_prefix></multi_prefix>
  <corporate>{%Corporate%}</corporate>
  <first_person></first_person>
  <other_persons>{%Last|First|Middle%}{ %First%}{ %Middle%}</other_persons>
  <separator_between_if_two> </separator_between_if_two>
  <separator_between_if_more_than_two> </separator_between_if_more_than_two>
  <separator_before_last></separator_before_last>
  <max_number_of_persons_to_display>500</max_number_of_persons_to_display>
  <number_of_persons_to_display_if_more_than_max>500</number_of_persons_to_display_if_more_than_max>
  <overflow></overflow>
  <single_suffix></single_suffix>
  <multi_suffix></multi_suffix>
</list>
{code:xml}
