# Numbers in Colonial Maya Texts
Alex LaPrevotte
2025-12-12

My goal for processing these data is to get the text into a format I can
work with. The data, available
[here](https://tesiunamdocumentos.dgb.unam.mx/ptd2025/abr_jun/0869324/Index.html),
are currently in a pdf, broken into chunks of five lines:

- a transcription of the original colonial Maya text (in original
  orthography)
- a transcription of the text with modern orthography
- a morphological breakdown
- a morphological gloss
- a Spanish translation.

I am currently working with a 14-page excerpt from this PDF, selected
because it begins and ends with complete 5-line chunks of text, but I am
interested in trying to expand to a larger sample later. I anticipate my
final working data will look like a data frame with three columns (text
with modern orthography, morphological breakdown, morphological gloss)
and many rows, one per word. I plan to eliminate the original
orthography and Spanish translation because they contain special
characters, do not correspond to the other lines in number of words, and
the only way to correlate the Spanish translations with the Maya would
be manually.

## Reading in the data

``` r
# read in the text with each page as a row
ebtun_text <-pdf_text("private/Machault_sample.pdf")

# lines to rows
ebtun_text <- strsplit(ebtun_text, "\n")

# combine all pages into one vector
ebtun_text <- unlist(ebtun_text)

# convert to dataframe
ebtun_data <- data.frame(line = ebtun_text)

# currently 1 column, 543 rows
```

## Cleaning up the rows

``` r
# eliminate rows that only include numbers or spaces
ebtun_data <- subset(ebtun_data, !grepl("^\\s*$", line) & !grepl("^\\s*\\d+\\s*$", line))

# eliminate a 4-row footnote
ebtun_data <- ebtun_data[-c(192:195), ]

# reset row numbers
rownames(ebtun_data) <- NULL

# make it a dataframe again
ebtun_data <- data.frame(line = ebtun_data, stringsAsFactors = FALSE)

# currently 1 column, 340 rows
```

## Breaking the data into chunks

``` r
# my goal here is to break the data back down into the 5-line chunks that were the basis of the PDF formatting

# every 5 lines to a group
ebtun_grouped <- ebtun_data %>%
  mutate(group = ceiling(row_number() / 5))

# each group to a dataframe
ebtun_chunks <- split(ebtun_grouped, ebtun_grouped$group)

# currently 68 data frames, each two columns (lines of text and group number) and five rows
```

``` r
sessionInfo()
```

    R version 4.3.0 (2023-04-21 ucrt)
    Platform: x86_64-w64-mingw32/x64 (64-bit)
    Running under: Windows 11 x64 (build 26100)

    Matrix products: default


    locale:
    [1] LC_COLLATE=English_United States.utf8 
    [2] LC_CTYPE=English_United States.utf8   
    [3] LC_MONETARY=English_United States.utf8
    [4] LC_NUMERIC=C                          
    [5] LC_TIME=English_United States.utf8    

    time zone: America/New_York
    tzcode source: internal

    attached base packages:
    [1] stats     graphics  grDevices utils     datasets  methods   base     

    other attached packages:
     [1] pdftools_3.5.0  lubridate_1.9.2 forcats_1.0.0   stringr_1.5.0  
     [5] dplyr_1.1.2     purrr_1.0.1     readr_2.1.4     tidyr_1.3.0    
     [9] tibble_3.2.1    ggplot2_3.4.2   tidyverse_2.0.0

    loaded via a namespace (and not attached):
     [1] gtable_0.3.3     jsonlite_1.8.4   qpdf_1.3.5       compiler_4.3.0  
     [5] Rcpp_1.0.14      tidyselect_1.2.0 scales_1.2.1     yaml_2.3.7      
     [9] fastmap_1.1.1    R6_2.5.1         generics_0.1.3   knitr_1.42      
    [13] munsell_0.5.0    pillar_1.9.0     tzdb_0.4.0       rlang_1.1.1     
    [17] utf8_1.2.3       stringi_1.7.12   xfun_0.39        timechange_0.2.0
    [21] cli_3.6.1        withr_2.5.0      magrittr_2.0.3   digest_0.6.31   
    [25] grid_4.3.0       rstudioapi_0.14  askpass_1.1      hms_1.1.3       
    [29] lifecycle_1.0.3  vctrs_0.6.2      evaluate_0.21    glue_1.6.2      
    [33] fansi_1.0.4      colorspace_2.1-0 rmarkdown_2.21   tools_4.3.0     
    [37] pkgconfig_2.0.3  htmltools_0.5.5 
