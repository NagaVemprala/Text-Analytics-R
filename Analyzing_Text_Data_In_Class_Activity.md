Text Analysis - In_class_Activity
================
Student Name
2023-03-24

#### Analyzing text using stringr (stringi has similar functions that can perform wide range of functionalities while the stringr is much simpler to use)

#### Implement important functions from stringr packages.

-   str_c
-   str_sub  
-   str_extract  
-   str_detect  
-   str_remove

``` r
library(tidyverse) # Loads a suite of tidy packages 
```

    ## -- Attaching packages --------------------------------------- tidyverse 1.3.1 --

    ## v ggplot2 3.3.5     v purrr   0.3.4
    ## v tibble  3.1.6     v dplyr   1.0.8
    ## v tidyr   1.2.0     v stringr 1.4.0
    ## v readr   2.1.2     v forcats 0.5.1

    ## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(stringr) # redundant 
library(readr) # redundant 
```

``` r
# Q1. Using str_c and str_sub format the phone numbers. 
# 5039431234 should be formatted as (503) 943-1234
# 5039433241 should be formatted as (503) 943-3241

phone_numbers <- c("5039431234", "5039434321")
str_c(
  str_sub(phone_numbers, start = 1, end = 3), 
  " ",
  str_sub(phone_numbers, start = 4, end = 6),
  "-",
  str_sub(phone_numbers, start = 7, end = 10)
)
```

    ## [1] "503 943-1234" "503 943-4321"

``` r
# Q2. Combine two character vectors names and phone numbers into a third vector with a space in between the strings 
# 
names <- c("Adams", "Sarah")
phone_numbers <- c("5039431234", "5039434321")
# Example. Third vector should be:
# "Adams: 5039431234" "Sarah: 5039434321"

# Create the third vector as concatenated_name 
concatenated_names_phones <- str_c(names, ": ", phone_numbers)
concatenated_names_phones
```

    ## [1] "Adams: 5039431234" "Sarah: 5039434321"

``` r
# Q3. Combine two character columns of a dataframe (names and phone numbers) into a third columns or vector with a ": space" in between the strings 
# 
names <- c("Adams", "Sarah")
phone_numbers <- c("5039431234", "5039434321")
contacts <- data.frame(names, phone_numbers)
# Example. Third vector should be (part of dataframe):
# "Adams: 5039431234" "Sarah: 5039434321"

# Create the third vector as concatenated_name 
contacts$concatenated_names_phones <- str_c(contacts$names, 
                                            ": ", 
                                            contacts$phone_numbers)
```

``` r
# Q4. Create a vector of review sentences length. Reviews are in the text column 
reviews <- data.frame(text = c(" I do like writing reviews especially for used phones because it helps everyone.",
                                  "Any new phone from Apple costs a fortune",
                                  "Once you turn on your new/used iphone and enable bluetooth and set it next to your old iphone it pretty much transfers all your data"))
```

``` r
# Q5. Using str_extract extract the word iphone from each of these reviews
str_extract(reviews$text, "iphone")
```

    ## [1] NA       NA       "iphone"

``` r
# Q6. Using str_detect extract reviews that contain "new" in the text column 
# str_detect returns a logical vector 
str_detect(reviews$text, "new")
```

    ## [1] FALSE  TRUE  TRUE

#### Use the below matching patterns:

?: 0 or 1 +: 1 or more \*: 0 or more

``` r
# Q7. Extract only the salary portion from the text e.g, from "Pay: $49,359" extract $49,359 
# Use str_extract 

salaries_vector <- c("Pay: $49,359", "Estimated salary: 45,000", "This is a $109,201 paying job")

str_extract(salaries_vector, "\\$?[:digit:]*,[:digit:]+")
```

    ## [1] "$49,359"  "45,000"   "$109,201"

#### Use the number of matches to extract the required string for the next question:

{n}: exactly n {n,}: n or more {,m}: at most m {n,m}: between n and m

-   Extract Ip Addresses from this error log
-   <https://www.ibm.com/docs/en/zos/2.1.0?topic=problems-example-log-file>

sample_error_log \<- “03/22 08:51:06 INFO :…read_physical_netif: index
\#0, interface VLINK1 has address 129.1.1.1, ifidx 0 03/22 08:51:06 INFO
:…read_physical_netif: index \#1, interface TR1 has address 9.37.65.139,
ifidx 1 03/22 08:51:06 INFO :…read_physical_netif: index \#2, interface
LINK11 has address 9.67.100.1, ifidx 2 03/22 08:51:06 INFO
:…read_physical_netif: index \#3, interface LINK12 has address
9.67.101.1, ifidx 3 03/22 08:51:06 INFO :…read_physical_netif: index
\#4, interface CTCD0 has address 9.67.116.98, ifidx 4 03/22 08:51:06
INFO :…read_physical_netif: index \#5, interface CTCD2 has address
9.67.117.98, ifidx 5 03/22 08:51:06 INFO :…read_physical_netif: index
\#6, interface LOOPBACK has address 127.0.0.1, ifidx 0”

``` r
# Q8. Extract Ip Addresses from this error log
# 
sample_error_log <- "03//22 08:51:06 INFO   :...read_physical_netif: index #0, interface VLINK1 has address 129.1.1.1, ifidx 0
03/22 08:51:06 INFO   :...read_physical_netif: index #1, interface TR1 has address 9.37.65.139, ifidx 1
03/22 08:51:06 INFO   :...read_physical_netif: index #2, interface LINK11 has address 9.67.100.1, ifidx 2
03/22 08:51:06 INFO   :...read_physical_netif: index #3, interface LINK12 has address 9.67.101.1, ifidx 3
03/22 08:51:06 INFO   :...read_physical_netif: index #4, interface CTCD0 has address 9.67.116.98, ifidx 4
03/22 08:51:06 INFO   :...read_physical_netif: index #5, interface CTCD2 has address 9.67.117.98, ifidx 5
03/22 08:51:06 INFO   :...read_physical_netif: index #6, interface LOOPBACK has address 127.0.0.1, ifidx 0"

str_extract_all(sample_error_log, "[:digit:]{1,3}\\.[:digit:]*\\.[:digit:]*\\.[:digit:]*")
```

    ## [[1]]
    ## [1] "129.1.1.1"   "9.37.65.139" "9.67.100.1"  "9.67.101.1"  "9.67.116.98"
    ## [6] "9.67.117.98" "127.0.0.1"

#### Text pre-processing

``` r
# Q9. Replace new line and carriage return characters \n and \r with ""
job_description <- "We offer competitive salaries, benefits, equity, and unlimited PTO. \n
\r\r\r Conveyer is an Equal Opportunity Employer.\n\n \r\n"
str_replace_all(job_description, "\\n|\\r", "")
```

    ## [1] "We offer competitive salaries, benefits, equity, and unlimited PTO.  Conveyer is an Equal Opportunity Employer. "

``` r
# Q10. Use stringr::words for this question  
# Extract words that end with "ble" such as 
# The output should contain words: "able"        "available"   "double"      "possible"    "probable"    "responsible" "table" "terrible"    "trouble"
stringr::words[
  !is.na(
    str_extract(stringr::words, "[:alpha:]*ble$")
    )
]
```

    ## [1] "able"        "available"   "double"      "possible"    "probable"   
    ## [6] "responsible" "table"       "terrible"    "trouble"

``` r
# Q11. Use stringr::words for this question 
# Extract words that end with "ble" such as 
# The output should contain words: "able"        "available"   "double"      "possible"    "probable"    "responsible" "table" "terrible"    "trouble"
# Also, extract words that start with ex

# Final output should be: 
# [1] "able"        "available"   "double"      "exact"       "example"     "except"      "excuse"      "exercise"
# [9] "exist"       "expect"      "expense"     "experience"  "explain"     "express"     "extra"       "next"     
# [17] "possible"    "probable"    "responsible" "table"       "terrible"    "trouble" 

stringr::words[
  !is.na(
    str_extract(stringr::words, "[:alpha:]*ble$|^ex[a-z]*")
    )
]
```

    ##  [1] "able"        "available"   "double"      "exact"       "example"    
    ##  [6] "except"      "excuse"      "exercise"    "exist"       "expect"     
    ## [11] "expense"     "experience"  "explain"     "express"     "extra"      
    ## [16] "possible"    "probable"    "responsible" "table"       "terrible"   
    ## [21] "trouble"

``` r
# Q12. Convert the tweets to lower case. Use str_to_lower()
tweets <- c("Analysts say generalized discord percolates in the U.S. to a greater degree than the powerful on Wall Street or in Washington might want to admit.", "The collapse of Silicon Valley Bank and Signature Bank is expected to cost the Federal Deposit Insurance Corp.’s fund roughly $22.5 billion, the agency estimates.

Will banks ask you to pay additional fees to help fill the hole? Experts weighed in.")
str_to_lower(tweets)
```

    ## [1] "analysts say generalized discord percolates in the u.s. to a greater degree than the powerful on wall street or in washington might want to admit."                                                                                                        
    ## [2] "the collapse of silicon valley bank and signature bank is expected to cost the federal deposit insurance corp.’s fund roughly $22.5 billion, the agency estimates.\n\nwill banks ask you to pay additional fees to help fill the hole? experts weighed in."

``` r
# Q13. Using str_remove remove "The/the" keyword from the stringr::sentences
# Convert the output to a tibble and print. Do not need to create a new variable out of it. 
str_replace_all("hello world", "world", "")
```

    ## [1] "hello "

``` r
str_remove_all("hello world", "world")
```

    ## [1] "hello "

``` r
head(str_remove_all(stringr::sentences, "the|The"))
```

    ## [1] " birch canoe slid on  smooth planks." 
    ## [2] "Glue  sheet to  dark blue background."
    ## [3] "It's easy to tell  depth of a well."  
    ## [4] "se days a chicken leg is a rare dish."
    ## [5] "Rice is often served in round bowls." 
    ## [6] " juice of lemons makes fine punch."

``` r
head(str_remove_all(stringr::sentences, "(t|T)he"))
```

    ## [1] " birch canoe slid on  smooth planks." 
    ## [2] "Glue  sheet to  dark blue background."
    ## [3] "It's easy to tell  depth of a well."  
    ## [4] "se days a chicken leg is a rare dish."
    ## [5] "Rice is often served in round bowls." 
    ## [6] " juice of lemons makes fine punch."

``` r
# Q14. Read the job descriptions column from the job details CSV file. Read the file into a tibble, "job_details". Extract the jobs that contain keywords "analysis" or "ability"
library(readxl)
jobs <- readxl::read_xlsx(str_c(getwd(), "/Input/indeed_job_details.xlsx"))
jobs_with_analysis_ability <- jobs[!is.na(str_extract(jobs$Description, "analysis|ability")), ]
```

``` r
# Q15. Use Job description for the following questions. 
#' Convert the job description to lower case 
#' Remove carriage return characters, new line characters. Remove the numeric information such as salary ($100,000.99), duration (6 months), etc. Other examples that we don’t want: $65,000.00 - $75,000.00
#' Remove special characters such as "@{}&!.[]". Replace the removed characters with a space "".
#' Check if the final job_details dataset is updated properly. 

jobs$Description <- str_to_lower(jobs$Description)
jobs$Description <- str_remove_all(jobs$Description, 
                                   "\\n|\\r")
jobs$Description <- str_remove_all(jobs$Description, 
                                   "\\$[:digit:]+,[:digit:]+")
jobs$Description <- str_remove_all(jobs$Description, 
                                   "[:digit:]")
jobs$Description <- str_remove_all(jobs$Description, 
                                   "@\\$^&\\{\\}#!\\-\\+")
jobs$Description <- str_remove_all(jobs$Description, 
                                   "\\[|\\]|\\(|\\)")
```

``` r
# Q16. Display the stop_words dataset from the tidytext package 
# they, than, the, an, a, 
library(tidytext)
tidytext::stop_words
```

    ## # A tibble: 1,149 x 2
    ##    word        lexicon
    ##    <chr>       <chr>  
    ##  1 a           SMART  
    ##  2 a's         SMART  
    ##  3 able        SMART  
    ##  4 about       SMART  
    ##  5 above       SMART  
    ##  6 according   SMART  
    ##  7 accordingly SMART  
    ##  8 across      SMART  
    ##  9 actually    SMART  
    ## 10 after       SMART  
    ## # ... with 1,139 more rows

``` r
# Q17. Using the unnest_tokens of the tidytext package, convert the sentences to keywords 
# Using the anti_join remove the stop_words from the output of unnest_tokens output 
# Count the words and then rank the word by the frequency using dense_rank function. 
# 
top_keywords <- jobs %>% 
  select(Description) %>% 
  unnest_tokens(word, Description) %>% 
  anti_join(stop_words) %>%
  count(word) %>% 
  # Arrange function of dplyr to sort the dataset by one or more columns 
  arrange(desc(n))
```

    ## Joining, by = "word"

``` r
top_keywords
```

    ## # A tibble: 11,025 x 2
    ##    word            n
    ##    <chr>       <int>
    ##  1 data         1573
    ##  2 experience    815
    ##  3 business      682
    ##  4 team          472
    ##  5 including     460
    ##  6 management    394
    ##  7 information   378
    ##  8 skills        378
    ##  9 analytics     376
    ## 10 support       350
    ## # ... with 11,015 more rows

``` r
# Q18. Plot the top 10 words and their frequencies on a bar plot 
# 
library(ggplot2)
top_10_keywords <- top_keywords %>% 
  filter(n >= 350)
ggplot(top_10_keywords) +
  geom_bar(aes(x = word, y = n), stat = "identity")
```

![](Analyzing_Text_Data_In_Class_Activity_files/figure-gfm/pre-processing-4-1.png)<!-- -->

``` r
# Q19. Install "wordcloud" package and plot the wordcloud using the wordcloud function. 

library(wordcloud)
```

    ## Loading required package: RColorBrewer

``` r
top_keywords <- top_keywords %>% 
  filter(n >= 10)
wordcloud(top_keywords$word, top_keywords$n)
```

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): effectively could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): background could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): requirements could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): companies could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): complete could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): matrixed could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): metrics could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): configuration could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): friendly could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): exchange could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): recognize could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): data could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): matching could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): email could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): proactively could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): age could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): michigan could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): developing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): platforms could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): industry could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): effective could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): physical could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): advertising could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): analysts could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): degree could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): develop could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): text could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): level could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): situations could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): collaborating could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): existing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): involving could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): operations could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): applying could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): verbally could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): ability could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): leader could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): environment could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): respond could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): patients could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): priorities could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): effectiveness could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): job could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): affirmative could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): combination could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): handling could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): coordinator could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): policies could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): comments could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): resource could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): consideration could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): cybersecurity could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): government could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): product could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): description could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): preparation could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): simple could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): enables could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): party could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): provided could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): stay could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): relevant could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): aspects could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): scientific could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): visualization could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): inform could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): expert could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): receive could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): mathematical could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): details could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): preferredexperience
    ## could not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): veteran could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): routine could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): principal could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): benefit could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): regulatory could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): correspondence could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): inclusive could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): primarily could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): operational could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): audience could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): player could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): seek could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): flexible could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): amazon could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): patterns could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): sources could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): channels could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): requests could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): applicants could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): alignment could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): individual could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): funding could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): adjust could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): applicable could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): results could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): quantitative could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): individuals could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): communicator could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): ensuring could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): powerpoint could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): teamwork could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): daily could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): active could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): compensation could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): quickly could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): enrollment could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): continuous could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): assess could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): writing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): education could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): sense could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): bonus could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): summary could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): matter could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): commute could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): coding could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): guiding could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): understanding could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): accredited could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): international could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): organize could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): zoom could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): significant could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): sites could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): behavior could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): professionals could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): caring could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): finance could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): engineering could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): prepare could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): online could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): managing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): settings could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): communities could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): financial could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): learn could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): multiple could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): access could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): advocates could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): leverage could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): edge could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): actions could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): possess could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): healthier could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): employment could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): advancing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): service could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): clients could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): proficiency could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): efficient could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): certification could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): shared could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): storytelling could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): utilize could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): engineers could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): cities could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): functionality could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): focused could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): levels could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): published could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): ready could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): continue could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): date could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): hypothesis could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): strong could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): annual could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): simmons could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): adapting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): updates could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): brand could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): intended could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): demonstrates could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): collaboration could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): supervision could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): excellent could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): channel could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): expectations could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): innovate could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): candidate could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): including could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): agency could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): selected could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): message could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): title could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): track could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): concepts could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): experience could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): school could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): defining could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): submit could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): prescriptive could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): quest could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): live could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): providers could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): senior could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): identifying could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): identify could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): history could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): feedback could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): experts could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): orientation could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): design could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): designs could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): guide could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): commitment could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): conduct could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): vendor could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): internet could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): support could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): follow could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): models could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): centered could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): inclusion could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): solution could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): starting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): amazing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): educational could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): actively could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): translating could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): letter could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): programming could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): student could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): outreach could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): guidelines could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): production could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): contact could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): editing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): scientists could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): behavioral could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): recovery could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): publications could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): objectives could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): progressive could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): joining could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): eligible could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): savvy could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): denver could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): network could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): recruitment could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): guidance could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): customer could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): organization could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): base could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): okta could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): newsletters could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): partner could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): automation could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): leadership could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): comply could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): carta could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): mathematics could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): establish could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): mining could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): basic could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): validating could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): facebook could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): suite could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): note could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): social could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): success could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): tables could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): accordance could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): targeted could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): skillsability could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): transforming could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): understand could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): protected could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): recognized could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): features could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): manager could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): sms could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): align could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): standards could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): faculty could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): reviewing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): investigation could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): outstanding could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): vaccine could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): coordinating could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): creates could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): credit could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): creed could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): grade could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): experience.three could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): produce could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): purpose could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): challenge could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): summarize could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): dependent could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): veterans could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): interactions could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): applied could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): helps could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): lines could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): interviews could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): connect could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): regulations could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): inferential could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): insights could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): supporting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): ownership could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): anticipating could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): national could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): team could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): director could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): private could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): accrediting could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): scope could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): kyndryl could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): modeling could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): querying could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): coverage could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): reference could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): unique could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): investment could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): indicators could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): application could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): represent could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): preparing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): plans could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): profit could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): typically could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): includes could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): constantly could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): potential could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): marketing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): integration could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): accuracy could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): opportunities could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): exchanges could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): employees could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): dealing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): interview could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): extraction could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): managers could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): designed could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): dixon could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): women could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): proper could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): volume could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): referral could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): slack could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): practices could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): term could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): satisfaction could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): advice could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): affiliate could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): remotely could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): core could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): departmental could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): changing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): workforce could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): strategic could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): appointment could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): engage could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): bring could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): posted could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): original could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): family could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): insight could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): actionable could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): requirement could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): discriminate could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): motivated could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): internal could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): solving could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): benefits could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): oversees could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): master's could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n):
    ## qualificationsbachelor's could not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): hiring could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): united could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): global could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): interpret could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): growth could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): experience.one could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): identifies could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): career could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): creation could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): mask could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): phone could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): retail could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): modern could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): progress could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): health could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): promoting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): advance could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): spreadsheets could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): linquest could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): visit could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): safety could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): passion could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): retirement could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): traffic could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): cloud could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): primary could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): line could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): algorithms could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): feature could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): qualitative could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): client could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): skilled could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): company's could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): oversight could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): move could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): final could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): dental could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): perspectives could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): quality could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): substituted could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): welfare could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): scientist could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): reporting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): meaningful could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): humana could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): display could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): verbal could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): perform could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): databases could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): law could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): accreditation could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): centric could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): build could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): solve could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): assigned could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): forward could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): relationships could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): sexual could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): insurancepaid could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): evaluated could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): cost could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): functions could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): unit could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): pregnancy could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): documents could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): engine could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): parking could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): apply could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): analyzing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): proof could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): maintains could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): continued could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): check could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): employer could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): abilities could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): account could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): collaborative could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): techniques could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): utilization could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): expertise could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): welcoming could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): machine could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): emerging could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): address could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): contribute could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): type could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): processing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): cross could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): recommend could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): commerce could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): crm could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): power could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): consumer could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): revenue could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): statistics could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): epic could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): charts could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): preferably could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): pride could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): write could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): website could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): approaches could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): budget could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): running could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): experienced could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): interpreting could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): traditional could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): variety could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): based could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): offerings could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): military could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): facing could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): http could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): assurance could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): contractors could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): master could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): peers could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): boston could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): compliance could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): terms could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): leads could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): serving could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): followers could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): measures could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): maps could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): collect could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): occasional could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): improve could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): common could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): continuing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): status could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): finding could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): impact could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): troubleshooting could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): retention could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): gender could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): surveys could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): multi could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): distribution could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): growing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): college could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): obtain could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): deep could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): expression could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): engaging could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): opportunity could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): origin could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): copy could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): color could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): behaviors could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): assisting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): center could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): concise could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): past could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): administration could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): discover could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): functionally could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): empower could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): visualizations could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): structures could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): economic could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): read could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): occasionally could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): campus could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): field could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): services could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): cultural could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): world's could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): owners could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): payment could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): interpretation could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): staffing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): focus could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): ongoing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): communicating could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): automates could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): performed could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): physician could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): messages could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): posting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): transparency could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): creative could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): spirit could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): organizational could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): fort could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): promotion could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): analyzes could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): tools could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): disabilities could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): technology could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): trusted could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): click could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): imagery could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): report could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): accommodation could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): specialist could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): enhance could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): america could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): business could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): setting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): accounts could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): root could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): stakeholder could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): nurture could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): meet could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): extract could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): english could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): moderate could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): technical could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): conditions could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): tuition could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): linkedin could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): recommending could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): exercise could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): formats could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): project could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): characteristic could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): custom could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): completing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): determining could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): efforts could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): assistance could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): written could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): allowing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): joint could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): skill could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): parameters could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): manage could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): top could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): love could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): religious could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): quarterly could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): extracting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): communicate could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): implements could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): story could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): organizations could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): warehouse could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): special could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): options could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): assessments could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): asset could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): division could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): nature could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): guides could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): evaluating could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): monitor could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): instructional could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): diversity could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): decisions could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): innovation could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): teams could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): audiences could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): context could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): robust could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): request could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): agencies could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): influence could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): interact could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): providing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): positive could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): supports could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): real could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): youtube could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): content could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): timepay could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): characteristics could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): prepares could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): generous could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): criteria could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): main could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): style could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): framework could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): stakeholders could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): outcomes could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): user could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): innovative could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): individual's could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): reviewed could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): transportation could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): regard could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): regularly could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): efficiency could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): dynamic could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): develops could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): roles could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): camera could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): certifications could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): identity could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): persons could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): nursing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): integrated could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): mobile could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): role could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): seeking could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): regression could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): transformation could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): consistent could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): priority could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): minimum could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): amount could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): external could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): targeting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): reviews could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): staff could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): frequently could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): deliver could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): assignments could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): qualified could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): teaching could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): assessment could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): limited could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): promote could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): execution could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): manages could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): collaborate could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): island could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): seamless could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): proud could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): ideas could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): free could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): junior could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): leading could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): pto could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): encourage could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): activities could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): photo could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): relational could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): information could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): ethnicity could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): religion could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): designers could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): presentation could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): times could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): series could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): mandatory could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): coworkers could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): set could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): query could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): products could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): child could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): accounting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): offered could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): billing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): citizenship could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): measurable could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): citizens could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): tasks could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): view could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): foundation could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): materials could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): forecasting could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): valuable could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): review could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): meets could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): schedules could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): associate could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): media could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): workday could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): define could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): recommendations could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): improvements could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): participate could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): vendors could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): provider could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): encourages could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): cover could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): platform could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): leaders could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): listening could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): organized could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): gilbert could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): optimize could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): globally could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): responding could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): preferred could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): covid could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): dashboards could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): action could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): strategy could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): engagement could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): personnel could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): governance could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): adoption could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): genetic could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): departments could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): facilitate could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): sound could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): fashion could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): careers could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): location could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): cycle could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): optimization could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): completion could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): secret could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): designing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): gather could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): collective could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): digital could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): communications could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): navigate could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): theories could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): students could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): determine could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): capabilities could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): research could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): graphic could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): process could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): planning could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): involves could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): care could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): grammar could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): culture could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): received could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): handle could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): policy could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): contracts could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): word could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): adobe could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): exceptional could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): interactive could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): jobs could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): serve could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): race could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): abroad could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): performance could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): search could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): editor could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): roadmap could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): workplace could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): duke could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): corporate could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): centers could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): resume could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): architecture could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): required could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): plan could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): respect could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): belonging could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): findings could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): talent could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): justice could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): enabling could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): depending could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): consultation could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): update could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): software could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): desired could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): completed could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): built could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): fields could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): consultant could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): company could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): thinking could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): include could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): employee could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): provide could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): goals could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): proposals could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): workflow could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): preference could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): enterprise could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): offices could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): demands could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): detail could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): issues could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): monthly could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): learning could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): defined could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): category could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): performing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): we’re could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): giving could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): patient could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): laws could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): participation could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): improving could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): demonstrate could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): university could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): study could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): bachelor's could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): synthesize could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): builds could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): structure could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): people could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): weekly could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): difference could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): answer could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): schools could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): deliverables could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): cx could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): hours could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): survey could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): ensure could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): familiarity could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): e.g could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): datasets could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): cleaning could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): hour could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): reliable could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): affect could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): lincoln could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): north could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): heart could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): mentor could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): select could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): producing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): that’s could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): home could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): event could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): fair could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): masters could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): videos could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): legally could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): mental could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): instagram could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): pipelines could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): skills could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): reimbursement could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): enhancements could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): body could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): prior could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): coordinate could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): utilizing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): immigration could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): energy could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): assists could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): campaigns could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): direct could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): intervention could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): office could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): parental could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): subject could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): educating could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): source could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): analyses could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): clarify could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): coaching could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): building could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): wide could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): scale could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): comfortable could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): entry could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): generate could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): reduce could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): questions could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): measuring could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): ancestry could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): ensures could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): day could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): graduate could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): country could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): hire could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): testing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): paced could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): test could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): recommendation could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): essential could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): medicine could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): monitors could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): extensive could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): eversana could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): francisco could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): initiative could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): sex could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): broad could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): conducted could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): geoint could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): salary could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): criminal could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): monitoring could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): conclusions could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): practical could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): documentation could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): committed could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): market could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): systems could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): timely could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): compelling could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): reasonable could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): class could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): accommodations could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): influencing could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): simulation could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): statistical could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): california could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): experience.two could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): leveraging could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): marijuana could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): space could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): sensitive could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): contractor could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): privacy could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): communication could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): specialized could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): additional could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): security could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): legal could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): disability could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): approach could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): kingland could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): providence could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): brings could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): environments could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): sales could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): documenting could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): listed could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): meeting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): consulting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): training could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): analyst could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): calls could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): instructions could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): risks could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): creatively could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): related could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): standardized could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): rapidly could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): systematic could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): implementation could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): associates could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): condition could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): landing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): exciting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): graphics could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): calendar could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): weaknesses could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): decision could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): remote could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): directly could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): contract could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): highly could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): unstructured could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): science could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): firm's could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): flexibility could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): specifications could
    ## not be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): locations could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): summarythe could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): independent could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): tracking could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): web could not be fit on
    ## page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): week could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): resulting could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): insurance could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): drug could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): strive could not be fit
    ## on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): organizing could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): knowledge could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): structured could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): implementing could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): political could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): program could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): increase could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): partnerships could not
    ## be fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): families could not be
    ## fit on page. It will not be plotted.

    ## Warning in wordcloud(top_keywords$word, top_keywords$n): protect could not be
    ## fit on page. It will not be plotted.

![](Analyzing_Text_Data_In_Class_Activity_files/figure-gfm/wordcloud-1.png)<!-- -->

``` r
# Q20. Install "wordcloud2" package and plot the wordcloud using the wordcloud2 function. 
library(wordcloud2)
#wordcloud2(top_keywords)
```
