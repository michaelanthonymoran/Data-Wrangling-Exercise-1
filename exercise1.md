# Data-Wrangling-Exercise-1

#Step Zero: Load the data into RStudio

library(dplyr)
library(readr)
library(tidyr)

refine_original <- read_csv("~/Foundations of Data Science/refine_original.csv")

tbl_df(refine_original)

#Step One: Clean up the brand ames

cleaned_company <- refine_original %>% mutate(company = tolower(company)) %>% mutate(company = case_when(.$company == c('phillips', 'phillps', 'phllps', 'fillips', 'phlips') ~ 'philips', .$company %in% c('akz0', 'ak zo') ~ 'akzo', .$company == c('unilver') ~ 'unilever', TRUE ~ .$company))

#Step Two: Separate product code and number

separated_product_info <- cleaned_company %>% separate('Product code / number', c("product_code", "product_number"), sep = '-', remove = TRUE)

#Step Three: Add product categories

product_categories <- separated_product_info %>% mutate(category = case_when(.$product_code == c('p') ~ 'Smartphone', .$product_code == c('q') ~ 'Tablet', .$product_code == c('v') ~ 'TV', .$product_code == c('x') ~ 'Laptop', TRUE ~ .$product_code))

#Step Four: Add full address for geocoding

complete_address <- product_categories %>% unite(full_address, address:country, sep = ", ")

#Step Five: Create dummy variables for company and product category

dummy_var <- complete_address %>% mutate(company = paste0("company_", company), category = paste0("product_", category)) %>% mutate(company_binary = 1, product_binary = 1) %>% spread(company, company_binary, fill = 0) %>% spread(category, product_binary, fill = 0)

#Step Six: Publish code

write.csv(dummy_var, "refine_clean.csv")
