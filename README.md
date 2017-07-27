# BibWord
Easy bibliography styles for Microsoft Word

## Introduction
The main goal of this project is to collect and maintain a number of bibliography styles which can be used by Microsoft Word 2007 and later. Most styles are either created using [BibWord](BibWord) or derived from the styles which come with Microsoft Word. 

Everybody who wants to submit a style can do so by contacting one of the project coordinators. The only requirement for putting a style on the project page, is that you release your style under the MIT license. That basically allows people to do whatever they want with your style as long as they give you credit for it.

## Installation on Windows (Word 2007/2010/2013)
To use the bibliography styles, they have to be copied into the Microsoft Word bibliography style directory. This directory can vary depending on where Word is installed: 
### Word 2007
```
    <winword.exe directory>\Bibliography\Style
```
On most _32-bits_ machines with Microsoft Word 2007 this will be:

```
    %programfiles%\Microsoft Office\Office12\Bibliography\Style
```
Once the styles are copied to the directory, they will show up every time Microsoft Word is opened.

### Word 2010
```
    <winword.exe directory>\Bibliography\Style
```
On most _32-bits_ machines with Microsoft Word 2010 this will be:

```
    %programfiles%\Microsoft Office\Office14\Bibliography\Style
```
Once the styles are copied to the directory, they will show up every time Microsoft Word is opened.

### Word 2013
```
    <user directory>\AppData\Roaming\Microsoft\Bibliography\Style
```
On most machines with Micrososft Word 2013 this will be:

```
    %userprofile%\AppData\Roaming\Microsoft\Bibliography\Style
```
Once the styles are copied to the directory, they will show up every time Microsoft Word is opened.

### Word 365
```
    %AppData%\Microsoft\Templates\LiveContent\15\Managed\Word Document Bibliography Styles
```
Once the styles are copied to the directory, they will show up every time Microsoft Word is opened.

**Remark:** the types.xml included with the stylesheets should be used in combination with BibType to create some extra fields for the different types.
## Installation on Mac (Word 2008/2011/2016)
### Word 2008
To use the bibliography styles, right-click on Microsoft Word 2008 and select show package contents. Put the files in:
```
    Contents/Resources/Style/
```
On most Macs with Microsoft Word 2008 this will be:

```
    /Applications/Microsoft Office 2008/Microsoft Word.app/Contents/Resources/Style/
```
### Word 2011
To use the bibliography styles, right-click on Microsoft Word 2008 and select show package contents. Put the files in:
```
    Contents/Resources/Style/
```
On most Macs with Microsoft Word 2011 this will be:

```
    /Applications/Microsoft Office 2011/Microsoft Word.app/Contents/Resources/Style/
```
### Word 2016 (version 15.17.0 and up)
To use the bibliography styles, place them in the following folder
```
    /Library/AppSupport/Microsoft/Office365/Citations/
```