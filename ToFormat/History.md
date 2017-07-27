## Project History
{anchor:BibWord}
### BibWord
**BibWord v.2.9** (01/04/2013)
* added support for Word 2013 
**BibWord v.2.8** (28/11/2009)
* added the 's' option to split URLs over multiple lines (splitting is done after a '/') 
* added more separators for page ranges (dash, en dash, colon)
**BibWord v.2.7** (25/10/2009)
* added the 'n' option to always display Month and MonthAccessed with 2 digits 
* added the 'i' option to invert Month and MonthAccessed (13 - Month) (useful for sorting purposes) 
* added the 'i' option to invert Day and DayAccessed (32 - Month) (useful for sorting purposes) 
* added the 's' option to SourceType
* fixed a bug in the options of Year and YearAccessed formatting where the options were ignored in combination with the 'r' flag
**BibWord v.2.6** (27/09/2009)
* added the 'i' option to invert Year and YearAccessed (9999 - Year) (useful for sorting purposes) 
* added the 'a' option to abbreviate Corporate contributors
* grouped and cleaned up all sorting functionality
**BibWord v.2.5** (10/05/2009)
* added the 'm' option to title formatting to allow for the selection of the master title
* added the 's' option to title formatting to allow for the selection of the sub title
* added the 'f' option to title formatting to capitalize the first word
* extended the 'a' option so now other articles then 'a', 'an', and 'the' can be moved to the end (i18n)
* fixed bug in name abbreviation where abbreviated parts without spaces did get further abbreviated (A.B. became A.)
* fixed a display bug where an URL between < and > was not shown
**BibWord v.2.4** (26/04/2009)
* added default 'MsoBibliography' style to footnote citations (Word 2008 only)
* added a 'c' option to contributors so they can be counted (e.g. %Author:c%)
* fixed a year suffix issue where sources where not sorted but still required year suffices
**BibWord v.2.3** (26/02/2009)
* added functionality to make in-text citations links which point to their accompanying bibliography entry through the citationaslink element in the general section 
* added a version variable at the end of the stylesheet (for easier BibWord updates)
* removed an obsolete debugging routine 
**BibWord v.2.2** (17/02/2009) 
* added support for b:FootnoteCitation (Word 2008)
* added support for b:Description (Word 2008)
* added support for b:updateURL (Word 2008)
**BibWord v.2.1** (11/02/2009) 
* changed both the file encoding and output encoding to utf-8 (Word 2008 support)
* changed html output encoding to utf-8 (Word 2008 support)
**BibWord v.2.0** (04/02/2009) 
* renamed all key variables to skey to prevent double definition of key when the stylesheet is used in combination with Internet Explorer
* fixed a recursion bug in the get-person-parameter routine
**BibWord v.1.9** (08/12/2008) 
* added the possibility to format placeholders using the 'Placeholder' type for sources
* fixed two bugs in the abbreviate-name routine (reported by ilbajec)
**BibWord v.1.8** (29/11/2008) 
* simplified ordinal formatting 
* introduced roman numerals for CitationVolume, Edition, Month, MonthAccessed, and Volume 
* added the possibility to specify a minimum length for abbreviated pages (e.g. 'a2': 135-137 becomes 135-37 rather than 135-7)
* fixed a bug in the recursion where sometimes a field was marked as used while it was not
**BibWord v.1.7** (23/11/2008) 
* fixed a bug influencing all BibWord extensions
**BibWord v.1.6** (22/11/2008)
* added the option to display only the first page of a range of pages 
* added the option to display the ordinal suffix in superscript
* fixed a bug in the formatting of ordinals where sometimes the wrong suffix was shown 
* fixed a bug in the abbreviation of pages where sometimes nothing was shown
**BibWord v.1.5** (11/11/2008)
* fixed a namelist formatting bug where the wrong separator was displayed before the last contributor and the overflow 
* fixed a bug in the formatting of ordinals 
* added 4 new variables for in-text citations: CitationPages, CitationPrefix, CitationSuffix, CitationVolume 
* added support for abbreviating and extending page ranges (e.g. 137-139 => 137-9) 
* added support for year suffices (requires the BibWord Extender tool)
**BibWord v.1.4**
* fixed a recursion error where variables got marked as being used while they were not 
* added 'r' parameter to all variables allowing variables being used multiple times within the same format 
* added day formatting options 
* added the possibility to use string laterals instead of variables (e.g.: %Author:2|"Anonymous"%) 
* changed name abbreviation to support the display and/or removal of dashes and spaces between name parts 
* added a possibility to display an error message when there is no formatting information for a source string
**BibWord v.1.3**
* fixed the way the 'x' at the start and the end of a bibliography are hidden in case tables are used
**BibWord v.1.2**
* added month formatting
**BibWord v.1.1**
* added the BibOrder extension
**BibWord v.1.0**
* initial release

{anchor:Extender}
### BibWord Extender
**BibWord Extender v.2.0** (16/03/2009) 
* runs on Mono
* allows for selection of a different style directory than the default one 
* allows for creating backups of your file (just so you can always be on the safe side) 
* handles non-BibWord styles better
**BibWord Extender v.1.2** (11/02/2009) 
* fixed encoding issue (this should hopefully also fix the \u2013 issues some people reported)
**BibWord Extender v.1.1** (06/12/2008) 
* made stylesheet encoding independent
* compiled targetting x86 so registry access on a 64 bit machine does not fail 
**BibWord Extender v.1.0** (16/11/2008)
* initial release

{anchor:BibOrder}
### BibOrder _(deprecated)_
**BibOrder v.1.0** (5/9/2008)
* initial release

{anchor:BibType}
### BibType
**BibType v.1.0** (4/8/2008)
* initial release