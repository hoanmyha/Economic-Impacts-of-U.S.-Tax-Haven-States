# Economic-Impacts-of-U.S.-Tax-Haven-States 🏦

## By My Ha

## Introduction
This project examines whether U.S. tax-haven states—defined as states with little to no personal income tax—demonstrate different economic patterns compared to non-haven states. Using 20 years of publicly available state tax and economic data, the analysis focuses on GDP per capita, labor force growth, and tax revenue composition.

## Data Dictionary 📖
Our dataset includes the following columns:

- **State: U.S. state name

- **Year: Calendar year of the observation

- **Quarter: Calendar quarter of the observation (1–4)

- **Population: Total population of the state

- **Unemployment_Rate: State unemployment rate (%)

- **Civilian noninstitutional population: Population aged 16+ not living in institutions

Total Civilian Labor Force: Total employed and unemployed individuals in the labor force

Total Employed Civilian Labor Force: Number of employed individuals

Total Unemployed Civilian Labor Force: Number of unemployed individuals

% Civilian Labor Force Unemployed: Percentage of the labor force that is unemployed

Personal_Income: Total personal income earned in the state (USD)

Applications: Number of new business applications

Formations: Number of business formations

Births: Number of new employer business births

GDP_Total: Total gross domestic product (USD)

GDP_Accom_Food: GDP from accommodation and food services

GDP_Admin_Waste: GDP from administrative and waste services

GDP_Ag_Fish_Hunt: GDP from agriculture, fishing, and hunting

GDP_Arts_Recreation: GDP from arts, entertainment, and recreation

GDP_Construction: GDP from the construction sector

GDP_Durable_Goods: GDP from durable goods manufacturing

GDP_Education: GDP from educational services

GDP_Finance_Insurance: GDP from finance and insurance

GDP_Gov_Federal: GDP from the federal government

GDP_Gov_Military: GDP from military-related activities

GDP_Gov_State_Local: GDP from state and local government

GDP_Gov_Total: Total government GDP

GDP_Healthcare_Social: GDP from healthcare and social assistance

GDP_Information: GDP from the information sector

GDP_Management: GDP from management of companies

GDP_Manufacturing: Total manufacturing GDP

GDP_Mining_Oil_Gas: GDP from mining, oil, and gas extraction

GDP_Nondurable_Goods: GDP from nondurable goods manufacturing

GDP_Other_Services: GDP from other services

GDP_Private_Total: Total private-sector GDP

GDP_Prof_Tech_Services: GDP from professional and technical services

GDP_Real_Estate: GDP from real estate and rental services

GDP_Retail: GDP from retail trade

GDP_Transport_Warehousing: GDP from transportation and warehousing

GDP_Utilities: GDP from utilities

GDP_Wholesale: GDP from wholesale trade

Housing_Price_Index: State-level housing price index

## Data Cleaning Methodology To Ensure Tidy Data 🧹

### Tag column include multiple variables
- Split tag columns into seperate rows if there are more than 2 tags

```r
complaints_tibble <- complaints_tibble %>%
  separate_rows(Tags, sep = ", ")
```

### Standardize empty cells to na format
- Unified country name formats
- Standardized variations of USA/U.S.A to "United States"
- Determined missing countries from locality information

```r
# convert all empty cells into N/A instead of ""
col_w_empty_cells <- names(complaints_tibble[colSums(complaints_tibble == "", na.rm = TRUE) > 0])

complaints_tibble <- complaints_tibble %>%
  mutate(across(col_w_empty_cells, ~na_if(., "")))

# convert "N/A" to na
na_cols_logical <- sapply(complaints_tibble, function(x) {
  if(is.character(x)) {
    return(any(x == "N/A", na.rm = TRUE))
  } else {
    return(FALSE)
  }
})

cols_with_NA_text <- names(na_cols_logical)[na_cols_logical]

complaints_tibble <- complaints_tibble %>%
  mutate(across(cols_with_NA_text, ~na_if(., "N/A")))

```

### Date Formatting 📆
- Converted all date column from varchar to date format

```r
complaints_tibble$Date.received <- as.Date(complaints_tibble$Date.receive, format = '%m/%d/%Y')
complaints_tibble$Date.sent.to.company <- as.Date(complaints_tibble$Date.sent.to.company, format = '%m/%d/%Y')
```

## Key Findings

### High-level view of the customer complaint
![Word Cloud](https://github.com/minhnbnguyen/BMO/blob/main/pics/wordcloud.png)
- Among 6,000 complaints, the most common problem are likely related to fraudulent, frozen, and denied issue/problem

### Year-Over-Year trend
![Line Graph](https://github.com/minhnbnguyen/BMO/blob/main/pics/Rplot.png)
- Average complaints per quarter: 3,027
- Peak quarter: 2023 Q4 with 11,929 complaints

### Comparative Analysis using nrc sentiment
![Emotional content](https://github.com/minhnbnguyen/BMO/blob/main/pics/Rplot1.png)
- Method: Compare the emotional content of disputed vs. non-disputed complaints
- Goal: Identify emotional patterns that might predict complaint resolution difficulty
- Result: Largest dispute ratio falls within positive and trust emotions
