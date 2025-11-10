# Progress Report

* 10/07/2025: Created GitHub repository and cloned it to my computer. Made project_plan and progress_report documents. Refined my project plan to include more information about my data, how I plan to break it down, and what kinds of packages I anticipate using.

## Progress report 1, 11/08/2025

* I am not providing the data in the public repo yet because I am not sure about the licensing, please reach out to me for a copy.
* I created a quarto document to hold my code for this project and rendered in into a [GitHub flavored markdown file](Numbers-in-Colinial-Maya-Texts.md).
* I [read in the data](Numbers-in-Colonial-Maya-Texts.qmd#reading-in-the-data) I am currently working with, which is a 14-page portion of the source PDF. I selected this portion because it begins and ends with complete 5-line chunks of text, but I am interested in trying to expand to a larger sample once the pipeline is effective; this may require me to manually eliminate additional sections of text, like footnotes. By default, `pdf_text()` reads each page as a string, so I broke each line into a row using "\n" (new line character) as a delimiter, then combined all the pages into one vector and converted that to a dataframe.
* I then worked on [cleaning up the rows](Numbers-in-Colonial-Maya-Texts.qmd#cleaning-up-the-rows), creating a subset that didn't include rows that were only spaces and numbers. I also manually eliminated four rows that were a footnote and not part of the data. I reset the row numbers (so they would not include the rows I eliminated from the subset) and converted the data back to a dataframe.
* I [broke the data out](Numbers-in-Colonial-Maya-Texts.qmd#breaking-the-data-into-chunks) into five-line chunks to correspond with the way it was formatted in the original PDF and made each group into its own dataframe.
* **Next steps:**
  * I plan to eliminate the first and fifth lines from each chunk, as explained in the qmd.
  * I plan to break each row down by word, with each word as a column. I anticipate that the presence of NAs will void a whole chunk, as that means the transcript and morphology were not done in a way that was 1:1, but I will obviously review the data to see if any NAs are due to errors in pdftools parsing or my processing.
  * I plan to concatenate all of the chunks, so I have a single dataframe of three long rows, with one row corresponding to each line of the text.
  * I plan to transform the rows to columns, so I Will end up with three long columns:
    * transcription of the text with modern orthography
    * morphological breakdown
    * morphological gloss
  * At this point, I anticipate more data cleaning, making sure no unexpected spaces or weird characters remain and the data is tidy and ready to work with.
