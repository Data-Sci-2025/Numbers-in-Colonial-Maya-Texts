# Project Plan

## Working Title
Numbers in Colonial-era Maya Texts

## Project summary  
I want to look at the occurrence of numbers in colonial-era Maya texts to see how often Spanish versus Maya numbers are used, whether either or both are used with numeral classifiers, and what forms those classifiers take

## Data
I have access to a huge (approx 2500 pages), publicly-available PDF containing a corpus (approx 1500 pages) of data which takes the form of running chunks of 5 lines of text: a transcription of the original colonial text (in original orthography), a transcription of the Maya with modern orthography, a morphological breakdown, a morphological gloss, and a Spanish translation. The data will require significant cleaning to be usable, including reading in the PDF, breaking it down by line, removing page numbers, numbering associated with the text, and chunks of Spanish text interspersed throughout. To work with all of the data would exceed the scope of this project, so my current plan is to work with a 14-page excerpt that begins and ends with complete five-line chunks. I hope to find identifiers to allow me to pick out these five-line chunks, work to find a way to correlate the lines word-for-word into tables (5 rows, one column per word), and then concatenate all those tables together, so I have a super wide table in which the first row is all the original text, the next line is all the modern text, etc. That seems like a form I could work with.  
  
There will be parts of this process that are difficult. It's not possible, for example, to correlate the text to the Spanish translation because morphology and word order are different. But, for what I want to do, I really only need the line two through four of each chunk. I am looking at packages meant to work with morphological data because they may be able to help me parse the breakdown and gloss. If they are not helpful, I think I can at least correlate the breakdown and gloss to the transcript using the presence of hyphens; they primarily occur within words in the morphological breakdown and glossing, so I could write a script to attach any text to the text immediately before or after it, if it ends (before) or starts (after) with a hyphen. I will likely then have to go through and either translate the data to Spanish/English, manually correlate it to the included Spanish translation, or at least implement some kind of tagging system to highlight numbers and indicate what linguistic form they take (Maya, Spanish, or numeral); this tagging may take the form of some kind of meta-data, I will need to see what kinds of packages I can find for corpus analysis.

**Data sources:**

* [Here](https://ru.dgb.unam.mx/items/62ecc542-2ca8-46cc-84a3-b055179249b4) is a page with a link to download the PDF (on the left)
* [Here](https://tesiunamdocumentos.dgb.unam.mx/ptd2025/abr_jun/0869324/Index.html) is a digital version of the PDF

## Analysis
My end goal is to see how numeral classifiers are used in colonial-era texts. Numeral classifiers are often described as obligatory in modern Maya, but in practice they are not always used with Spanish numerals, which are used for almost all numbers over three/five (regional), and when they are used in this context, they take on different forms. I'm interested to see what this looked like in early Maya-Spanish contact.