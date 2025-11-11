# Numbers in Colonial Maya Texts
Alex LaPrevotte
2025-12-12

**I have opted to add my new code, as I go, to my existing quarto
document.**

My goal for processing these data is to get the text into a format I can
work with. The data, available as part of “Los títulos de ebtún :
transcripción, traducción y análisis histórico,” the PhD dissertation of
Dr. Julien Machault, which can be found
[here](https://tesiunamdocumentos.dgb.unam.mx/ptd2025/abr_jun/0869324/Index.html),
are currently in a PDF, broken into chunks of five lines:

- a transcription of the original colonial Maya text (in original
  orthography)
- a transcription of the text with modern orthography
- a morphological breakdown
- a morphological gloss
- a Spanish translation.

I am working with a 14-page excerpt from this PDF (pages 58-71),
selected because it begins and ends with complete 5-line chunks of text,
but I am interested in trying to expand to a larger sample in the future
and I am attempting to account for situations that do not arise in the
current data within the code, so it could be applied to a larger data
set. I anticipate my working data, pre-analysis, will take the form of a
data frame with two columns (morphological breakdown, morphological
gloss) and many rows, one per word. I plan to eliminate the original
orthography, modern orthography, and Spanish translation because they
contain special characters, do not correspond well with the other lines
in number of words, and the only way to correlate the Spanish
translations with the Maya would be manually, due to differences in word
order and agglutination.

## Reading in the data

``` r
# read in the text with each page as a row
ebtun_text <-pdf_text("data/ebtun_sample.pdf")

# lines to rows
ebtun_text <- strsplit(ebtun_text, "\n")

# combine all pages into one vector
ebtun_text <- unlist(ebtun_text)

# currently 1 column, 543 rows
```

## Combining morphological breakdowns into single units

``` r
# eliminating multiple spaces and spaces before and after hyphens
# this should keep morphological breakdowns and glosses together
ebtun_text <- str_squish(gsub("-[ /t]+","-",gsub("[ /t]+-","-",ebtun_text)))

# eliminating null characters
ebtun_text <- str_replace_all(ebtun_text, "\\bø\\b", "")

# convert to dataframe
ebtun_data <- data.frame(line = ebtun_text)
```

## Cleaning up the rows

``` r
# eliminate rows that only include numbers or spaces
ebtun_data <- subset(ebtun_data, !grepl("^\\s*$", line) & !grepl("^\\s*\\d+\\s*$", line))

# reset row numbers
rownames(ebtun_data) <- NULL

# eliminate a 4-row footnote
ebtun_data <- ebtun_data[-c(195:198), ]

# make it a dataframe again
ebtun_data <- data.frame(line = ebtun_data, stringsAsFactors = FALSE)

# currently 1 column, 340 rows
```

## Breaking the data into chunks

``` r
# my goal here is to break the data back down into the 5-line chunks that were the basis of the PDF formatting

# every 5 lines to a group
ebtun_grouped <- ebtun_data |>
  mutate(group = ceiling(row_number() / 5))

# each group to a dataframe
ebtun_chunks <- split(ebtun_grouped, ebtun_grouped$group)

# currently 68 data frames, each two columns (text and group number) and five rows
```

## Removing rows not used in analysis

``` r
# eliminating rows 1, 2, and 5 from each chunk
ebtun_chunks <- lapply(ebtun_chunks, function(df) {
  df[3:4, , drop = FALSE]
})
```

## Split chunks by word

``` r
# function: split chunk into words
  # split each line by spaces
  # find max words in the chunk and add NAs to standardize columns
  # combine into data frame
split_chunk <- function(df_chunk) {
  split_words <- str_split(df_chunk$line, "\\s+", simplify = FALSE)
  max_words <- max(lengths(split_words))
  split_words_padded <- lapply(split_words, function(x) {
    length(x) <- max_words
    x
  })
  df_words <- as.data.frame(do.call(rbind, split_words_padded), stringsAsFactors = FALSE)
  return(df_words)
}

# use function on chunks
ebtun_chunks <- lapply(ebtun_chunks, split_chunk)
```

## NA wrangling

``` r
# which chunks have NAs?
na_counts <- sapply(ebtun_chunks, function(df) sum(is.na(df)))
na_counts
```

     1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 
     0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  1 
    27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 
     0  0  0  0  0  0  0  0  1  0  0  0  0  0  0  0  0  1  0  0  0  0  0  0  1  0 
    53 54 55 56 57 58 59 60 61 62 63 64 65 66 67 68 
     0  0  0  0  0  0  1  0  0  0  0  0  0  0  0  0 

``` r
# eliminate chunks with NAs
# noting that eliminating chunk 51 does get rid of one of my number data points; óox means three
ebtun_chunks_nona <- ebtun_chunks[!sapply(ebtun_chunks, function(df) any(is.na(df)))]

# check to make sure number of chunks has decreased
length(ebtun_chunks_nona)
```

    [1] 63

## Chunk concatenation

``` r
# number of rows
num_rows <- nrow(ebtun_chunks_nona[[1]])

# concatenate rows by chunk
ebtun_combined <- map_dfr(1:num_rows, function(i) {
  rows <- lapply(ebtun_chunks_nona, function(df) df[i, , drop = FALSE])
  suppressMessages(
    bind_cols(rows, .name_repair = "minimal")
  )
})

#drop the column names
colnames(ebtun_combined) <- NULL
```

## Data transposition

``` r
# turn those rows into columns
ebtun_long <- t(ebtun_combined)

# convert back to data frame
ebtun <- as.data.frame(ebtun_long)

# currently 2 columns (morphological breakdown and morphological gloss), 349 rows
```

## Final data cleaning

``` r
# removing leading and trailing spaces
ebtun$V1 <- str_trim(ebtun$V1)
ebtun$V2 <- str_trim(ebtun$V2)

# remove rows containing cells that are just ellipses (either the ellipses character or three consecutive periods)
ebtun <- ebtun |>
  filter(if_all(everything(), ~ !str_trim(.) %in% c("...", "…")))

#naming the columns because I'm about to add more and it seems responsible
ebtun <- ebtun |>
  rename(
    breakdown = V1,
    gloss = V2
  )

# final form of "found" data
str(ebtun)
```

    'data.frame':   323 obs. of  2 variables:
     $ breakdown: chr  "ti'" "almejen" "la'-e'" "je'ix" ...
     $ gloss    : chr  "PREP" "almejen" "PROX-TOP" "así_como" ...

## Analysis

``` r
# creating column: is this a numeral?
ebtun <- ebtun |>
  mutate(
    numeral = str_detect(str_trim(breakdown), "^[0-9]+$")
  )

# reading in lists of Spanish and Maya numbers (textual)
esp_numbers <- read_csv("data/esp_numbers.csv", col_names = FALSE) |> pull(X1)
maya_numbers <- read_csv("data/maya_numbers.csv", col_names = FALSE) |> pull(X1)

# fix encoding for accents
esp_numbers <- iconv(esp_numbers, from = "latin1", to = "UTF-8")
maya_numbers <- iconv(maya_numbers, from = "latin1", to = "UTF-8")

# does this cell contain a spanish or maya number?
ebtun <- ebtun |>
  mutate(
    spanish_number = str_detect(breakdown, paste(esp_numbers, collapse = "|")),
    maya_number = str_detect(breakdown, paste(maya_numbers, collapse = "|"))
  )
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
     [1] bit_4.0.5        gtable_0.3.3     jsonlite_1.8.4   crayon_1.5.2    
     [5] qpdf_1.3.5       compiler_4.3.0   Rcpp_1.0.14      tidyselect_1.2.0
     [9] parallel_4.3.0   scales_1.2.1     yaml_2.3.7       fastmap_1.1.1   
    [13] R6_2.5.1         generics_0.1.3   knitr_1.42       munsell_0.5.0   
    [17] pillar_1.9.0     tzdb_0.4.0       rlang_1.1.1      utf8_1.2.3      
    [21] stringi_1.7.12   xfun_0.39        bit64_4.0.5      timechange_0.2.0
    [25] cli_3.6.1        withr_2.5.0      magrittr_2.0.3   digest_0.6.31   
    [29] grid_4.3.0       vroom_1.6.3      rstudioapi_0.14  askpass_1.1     
    [33] hms_1.1.3        lifecycle_1.0.3  vctrs_0.6.2      evaluate_0.21   
    [37] glue_1.6.2       fansi_1.0.4      colorspace_2.1-0 rmarkdown_2.21  
    [41] tools_4.3.0      pkgconfig_2.0.3  htmltools_0.5.5 
