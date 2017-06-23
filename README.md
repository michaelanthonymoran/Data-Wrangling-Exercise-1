# Data-Wrangling-Exercise-1

#Step Zero: Load the Data into RStudio

library(dplyr)
library(readr)
library(tidyr)

refine_original <- read_csv("~/Foundations of Data Science/refine_original.csv")

tbl_df(refine_original)
