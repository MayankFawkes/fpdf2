# Unicode #

[TOC]

The FPDF class was modified adding UTF-8 support.
Moreover, it embeds only the necessary parts of the fonts that are used in the 
document, making the file size much smaller than if the whole fonts were 
embedded. These features were originally developed for the 
[mPDF](http://mpdf.bpm1.com/) project, and ported from 
[Ian Back](mailto:ian@bpm1.com?subject=sFPDF)'s
[sFPDF](http://www.fpdf.org/en/script/script91.php) LGPL PHP version.

Before you can use UTF-8, you have to install at least one Unicode font in the 
font directory (or system font folder). Some free font packages are available 
for download (extract them into the font folder):

  * [DejaVu](http://dejavu-fonts.org/) family: Sans, Sans Condensed, Serif,
Serif Condensed, Sans Mono (Supports more than 200 languages)

  * [GNU FreeFont](http://www.gnu.org/software/freefont/) family: FreeSans,
FreeSerif, FreeMono

  * [Indic](http://en.wikipedia.org/wiki/Help:Multilingual_support_(Indic))
(ttf-indic-fonts Debian and Ubuntu package) for Bengali, Devanagari, Gujarati,
Gurmukhi (including the variants for Punjabi), Kannada, Malayalam, Oriya,
Tamil, Telugu, Tibetan

  * [AR PL New Sung](http://www.study-area.org/apt/firefly-font/) (firefly):
The Open Source Chinese Font (also supports other east Asian languages)

  * [Alee](https://wiki.archlinux.org/index.php/Fonts) (ttf-alee Arch Linux
package): General purpose Hangul Truetype fonts that contain Korean syllable
and Latin9 (iso8859-15) characters.

  * [Fonts-TLWG](http://linux.thai.net/projects/fonts-tlwg/) (formerly
ThaiFonts-Scalable)

These fonts are included with this library's installers; see 
[Free Font Pack for FPDF](#free-font-pack-and-copyright-restrictions) below for
more information.

Then, to use a Unicode font in your script, pass `True` as the fourth parameter 
of [`add_font`](fpdf/fpdf.html#fpdf.fpdf.FPDF.add_font).

**Notes on non-latin languages**

Some users may encounter a problem where some characters displayed incorrectly. For example, using Thai language in the picture below

![thai-font-problem](https://raw.githubusercontent.com/PyFPDF/fpdf2/master/tutorial/thai-accent-error.png)

The solution is to find and use a font that covers the characters of your language.
From the error in the image above, Thai characters can be fixed using fonts from  [Fonts-TLWG](http://linux.thai.net/projects/fonts-tlwg/) which can be downloaded from
[this link](https://linux.thai.net/pub/thailinux/software/fonts-tlwg/fonts/). The example shown below.

![thai-font-working](https://raw.githubusercontent.com/PyFPDF/fpdf2/master/tutorial/thai-accent-working.png)

## Example ##

This example uses several free fonts to display some Unicode strings. Be sure to
install the fonts in the `font` directory first.

```python
#!/usr/bin/env python
# -*- coding: utf8 -*-

from fpdf import FPDF

pdf = FPDF()
pdf.add_page()

# Add a DejaVu Unicode font (uses UTF-8)
# Supports more than 200 languages. For a coverage status see:
# http://dejavu.svn.sourceforge.net/viewvc/dejavu/trunk/dejavu-fonts/langcover.txt
pdf.add_font('DejaVu', fname='DejaVuSansCondensed.ttf')
pdf.set_font('DejaVu', size=14)

text = u"""
English: Hello World
Greek: Γειά σου κόσμος
Polish: Witaj świecie
Portuguese: Olá mundo
Russian: Здравствуй, Мир
Vietnamese: Xin chào thế giới
Arabic: مرحبا العالم
Hebrew: שלום עולם
"""

for txt in text.split('\n'):
    pdf.write(8, txt)
    pdf.ln(8)

# Add a Indic Unicode font (uses UTF-8)
# Supports: Bengali, Devanagari, Gujarati, 
#           Gurmukhi (including the variants for Punjabi) 
#           Kannada, Malayalam, Oriya, Tamil, Telugu, Tibetan
pdf.add_font('gargi', fname='gargi.ttf')
pdf.set_font('gargi', size=14)
pdf.write(8, u'Hindi: नमस्ते दुनिया')
pdf.ln(20)

# Add a AR PL New Sung Unicode font (uses UTF-8)
# The Open Source Chinese Font (also supports other east Asian languages)
pdf.add_font('fireflysung', fname='fireflysung.ttf')
pdf.set_font('fireflysung', size=14)
pdf.write(8, u'Chinese: 你好世界\n')
pdf.write(8, u'Japanese: こんにちは世界\n')
pdf.ln(10)

# Add a Alee Unicode font (uses UTF-8)
# General purpose Hangul truetype fonts that contain Korean syllable 
# and Latin9 (iso8859-15) characters.
pdf.add_font('eunjin', fname='Eunjin.ttf')
pdf.set_font('eunjin', size=14)
pdf.write(8, u'Korean: 안녕하세요')
pdf.ln(20)

# Add a Fonts-TLWG (formerly ThaiFonts-Scalable) (uses UTF-8)
pdf.add_font('waree', fname='Waree.ttf')
pdf.set_font('waree', size=14)
pdf.write(8, u'Thai: สวัสดีชาวโลก')
pdf.ln(20)

# Select a standard font (uses windows-1252)
pdf.set_font('helvetica', size=14)
pdf.ln(10)
pdf.write(5, 'This is standard built-in font')

pdf.output("unicode.pdf")
```


View the result here: 
[unicode.pdf](https://github.com/PyFPDF/fpdf2/raw/master/tutorial/unicode.pdf)

## Free Font Pack and Copyright Restrictions ##

For your convenience, this library collected 96 TTF files in an optional 
["Free Unicode TrueType Font Pack for FPDF"](https://github.com/reingart/pyfpdf/releases/download/binary/fpdf_unicode_font_pack.zip),
with useful fonts commonly distributed with GNU/Linux operating systems (see 
above for a complete description). This pack is included in the Windows 
installers, or can be downloaded separately (for any operating system).

You could use any TTF font file as long embedding usage is allowed in the licence.
If not, a runtime exception will be raised saying: "ERROR - Font file 
filename.ttf cannot be embedded due to copyright restrictions."
