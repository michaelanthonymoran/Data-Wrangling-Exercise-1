# Data-Wrangling-Exercise-1

#Step Zero: Load the Data into RStudio

library(dplyr)
library(readr)
library(tidyr)

refine_original <- read_csv("~/Foundations of Data Science/refine_original.csv")

tbl_df(refine_original)

Step One: Clean up the Brand Names

cleaned_company <- refine_original %>% mutate(company = tolower(company)) %>% mutate(company = case_when(.$company == c('phillips', 'phillps', 'phllps', 'fillips', 'phlips') ~ 'philips', .$company %in% c('akz0', 'ak zo') ~ 'akzo', .$company == c('unilver') ~ 'unilever', TRUE ~ .$company))

