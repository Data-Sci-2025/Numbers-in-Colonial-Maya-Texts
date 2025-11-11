# Progress Report

* 10/07/2025: Created GitHub repository and cloned it to my computer. Made project_plan and progress_report documents. Refined my project plan to include more information about my data, how I plan to break it down, and what kinds of packages I anticipate using.

## Progress report 1, 11/08/2025

* I created a quarto document to hold my code for this project and rendered in into a [GitHub flavored markdown file](Numbers-in-Colonial-Maya-Texts.md).
* I [read in the data](Numbers-in-Colonial-Maya-Texts.qmd#reading-in-the-data) I am currently working with, which is a 14-page portion of the source PDF. I selected this portion because it begins and ends with complete 5-line chunks of text, but I am interested in trying to expand to a larger sample once the pipeline is effective; this may require me to manually eliminate additional sections of text, like footnotes. By default, `pdf_text()` reads each page as a string, so I broke each line into a row using "\n" (new line character) as a delimiter, then combined all the pages into one vector and converted that to a dataframe.
* I then worked on [cleaning up the rows](Numbers-in-Colonial-Maya-Texts.qmd#cleaning-up-the-rows), creating a subset that didn't include rows that were only spaces and numbers. I also manually eliminated four rows that were a footnote and not part of the data. I reset the row numbers (so they would not include the rows I eliminated from the subset) and converted the data back to a dataframe.
* I [broke the data out](Numbers-in-Colonial-Maya-Texts.qmd#breaking-the-data-into-chunks) into five-line chunks to correspond with the way it was formatted in the original PDF and made each group into its own dataframe.
* **Next steps:**
  * I plan to eliminate the first and fifth lines from each chunk, as explained in the qmd.
  * I plan to break each row down by word, with each word as a column. I anticipate that the presence of NAs will void a whole chunk, as that means the transcript and morphology were not done in a way that was 1:1, but I will obviously review the data to see if any NAs are due to errors in pdftools parsing or my processing.
  * I plan to concatenate all of the chunks, so I have a single dataframe of three long rows, with one row corresponding to each line of the text.
  * I plan to transform the rows to columns, so I will end up with three long columns:
    * transcription of the text with modern orthography
    * morphological breakdown
    * morphological gloss
  * At this point, I anticipate more data cleaning, making sure no unexpected spaces or weird characters remain and the data is tidy and ready to work with.

## Progress report 2, 11/11/2025

* I chose to build onto my existing quarto document.
* I [added some code](Numbers-in-Colonial-Maya-Texts.md#combining-morphological-breakdowns-into-single-units) early-on (before the data were in a dataframe) that got rid of all spaces next to hyphens, so words broken down by morpheme and the corresponding morphological glosses would stay together when the data was broken down by word.
* I [removed rows](Numbers-in-Colonial-Maya-Texts.md#removing-rows-not-used-in-analysis) 1, and 5 from each 5-line chunk, leaving only the data I needed for my analysis.
* I [broke each chunk down](Numbers-in-Colonial-Maya-Texts.md#split-chunks-by-word) by word, so each column contained one word (row 1), its morphological breakdown (row 2), and its morphological gloss (row 3).
* I did some work to [find and eliminate NAs](Numbers-in-Colonial-Maya-Texts.md#na-wrangling) from the data. First I found out which chunks contained NAs, then I looked at them to see where they were coming from. Some were due to encoding errors I could not handle on a macro-level but I noticed some were caused by issues in which the second and third lines weren't lining up. I had previously been using three lines of the original data (2:4) But I realized 2 and 3 were the same data formatted differently, so I went back up and reduced my working data down to (3:4) and that eliminated a few NAs, one of which unfortunately contained number data. I then removed all chunks with remaining NAs because it indicated that the morphological gloss and morphological breakdown didn't correspond properly one-to-one, and ran a check to make sure the number of chunks decreased;it did, from 68 to 63, which lined up with the five NAs I was still observing.
* I then [concatenated the chunks](Numbers-in-Colonial-Maya-Texts.md#chunk-concatenation) end-to-end, so I would have two long rows: each word broken down by morpheme and morphological glosses for those breakdowns.
* I [transposed the data](Numbers-in-Colonial-Maya-Texts.md#data-transposition), turning the rows into columns, to approximate the tidy-data text format.
* I did a final [clean of the data](Numbers-in-Colonial-Maya-Texts.md#final-data-cleaning), since it was finally in the format I plan to use it in. I trimmed any remaining white space (there shouldn't have been any, I was just trying to be thorough), removed any observations that were just ellipses (either the ellipses character or three consecutive periods), named my columns, and ran `str()` to get information about the format, shape, and size of my working data: 323 observations of 2 variables, breakdown and gloss, both a character data type.
* I began my [analysis portion](Numbers-in-Colonial-Maya-Texts.md#analysis). I read in lists of numbers in Spanish and Maya and fixed the encoding so it would properly display characters with accents. From there, I mutated three new columns, all TRUE/FALSE, showing: whether the "breakdown" observation in a given row is numerical (arabic numerals), whether it contains a Spanish number, and whether it contains a Maya number.
* **Next steps:**
  * I need to find a way to handle false positives so I can find actual numbers within the data (e.g. "Kochola" gets flagged as containing a Spanish number because the string "ocho" occurs within the word).
  * I should also make sure I am string matching with the numbers in a way that is not case sensitive, or otherwise convert all of my data to lower-case.
  * I need to look within the morphological breakdowns to see if any morphemes are appended the numbers I find, and if so, which ones.
  * I need to look at the 2-3 observations following numbers to see if those contain numeral classifiers.

### Sharing scheme:

* I received permission from the author of the paper from which I extracted my data to include an excerpt in my repository, so I added the [PDF](data/ebtun_sample.pdf) to the "data" folder. I also added a [PERMISSION](PERMISSION.md) file, documenting permission to include this data, and a "data source" section in my [README](README.md). I am working with a 14-page excerpt from this PDF (pages 58-71 of the original document), selected because it begins and ends with complete 5-line chunks of text, but I am interested in trying to expand to a larger sample in the future

### License:

* I opted to use the "GNU GPLv3" license for this project, because it is important to me that anyone who builds on the work in this project does so in a way that is accessible to as many people as possible with minimal barriers. I believe any work done with Maya data should be as accessible as possible to members of the Maya community.
