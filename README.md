# Economic-Impacts-of-U.S.-Tax-Haven-States 🏦

## By My Ha

## Introduction
![Tax Paradise Banner](https://github.com/minhnbnguyen/Datathon/blob/main/UMich_2026/viz/Screen%20Shot%202026-02-15%20at%2016.59.54.png)

## Introduction
This project analyzes the economics impact on tax haven states and whether a state should become tax haven or not.

Data is collected to support the 2026 University of Michigan Datathon.

## Data Dictionary 📖
First Data set

- **Date.received**: Date that the complaint was received
- **Year**: The calendar year in which the tax data was recorded.
- **Quarter**: The fiscal quarter of the year (1–4) corresponding to when the tax amount was reported.
- **State**: U.S. state associated with the tax record.
- **Tax_Category**: High-level classification of the tax type (e.g., sales tax, income tax, corporate tax).
- **Tax_Code**: Specific code used to identify the detailed tax type within each tax category.
- **Amount**: Monetary value of the tax collected or reported for the given year, quarter, state, and tax code (in USD).

Second Dataset
- State: U.S. state associated with the economic and demographic data.
- Year: The calendar year in which the data was recorded.
- Quarter: The fiscal quarter of the year (1–4) corresponding to when the data was reported.
- Population: Total population of the state for the given year and quarter.
- Unemployment_Rate: Percentage of the labor force that is unemployed during the specified period.
- Civilian noninstitutional population: Number of civilians aged 16 and older who are not institutionalized (e.g., not in prisons or nursing homes).
- Total Civilian Labor Force: Total number of individuals in the labor force, including both employed and unemployed persons actively seeking work.
- Total Employed Civilian Labor Force: Number of individuals currently employed within the civilian labor force.
- Total Unemployed Civilian Labor Force: Number of individuals in the labor force who are unemployed but actively seeking employment.
- % Civilian Labor Force Unemployed: Percentage of the civilian labor force that is unemployed.
- Personal_Income: Aggregate personal income of residents within the state, typically measured in USD.
- Applications: Number of business or economic-related applications submitted (e.g., business formation applications) during the specified period.
- Formations: Number of new businesses officially formed within the given time frame.
- Births: Number of new business establishments or entities created during the period.
- GDP_Total: Total gross domestic product of the state across all industries.
- GDP_Accom_Food: GDP contribution from accommodation and food services industries.
- GDP_Admin_Waste: GDP contribution from administrative support and waste management services.
- GDP_Ag_Fish_Hunt: GDP contribution from agriculture, fishing, and hunting industries.
- GDP_Arts_Recreation: GDP contribution from arts, entertainment, and recreation sectors.
- GDP_Construction: GDP contribution from the construction industry.
- GDP_Durable_Goods: GDP contribution from durable goods manufacturing industries.
- GDP_Education: GDP contribution from educational services.
- GDP_Finance_Insurance: GDP contribution from finance and insurance industries.
- GDP_Gov_Federal: GDP contribution from federal government activities.
- GDP_Gov_Military: GDP contribution from military-related government activities.
- GDP_Gov_State_Local: GDP contribution from state and local government activities.
- GDP_Gov_Total: Total GDP contribution from all government sectors combined.
- GDP_Healthcare_Social: GDP contribution from healthcare and social assistance industries.
- GDP_Information: GDP contribution from information and data-related industries.
- GDP_Management: GDP contribution from management of companies and enterprises.
- GDP_Manufacturing: GDP contribution from all manufacturing sectors combined.
- GDP_Mining_Oil_Gas: GDP contribution from mining, oil, and gas extraction industries.
- GDP_Nondurable_Goods: GDP contribution from nondurable goods manufacturing industries.
- GDP_Other_Services: GDP contribution from other miscellaneous service industries.
- GDP_Private_Total: Total GDP contribution from all private sector industries.
- GDP_Prof_Tech_Services: GDP contribution from professional and technical services.
- GDP_Real_Estate: GDP contribution from real estate, rental, and leasing industries.
- GDP_Retail: GDP contribution from retail trade industries.
- GDP_Transport_Warehousing: GDP contribution from transportation and warehousing sectors.
- GDP_Utilities: GDP contribution from utilities such as electricity, water, and gas.
- GDP_Wholesale: GDP contribution from wholesale trade industries.
- Housing_Price_Index: Index measuring changes in residential housing prices over time for the state.

Third Dataset
- State: U.S. state associated with the demographic and immigration data.
- Year: The calendar year in which the data was recorded.
- Total Population: Total number of residents in the state for the specified year.
- White Alone: Number of individuals identifying as White alone (not in combination with other races).
- Black Alone: Number of individuals identifying as Black or African American alone.
- American Indian or Alaskan Native: Number of individuals identifying as American Indian or Alaska Native alone.
- Asian Alone: Number of individuals identifying as Asian alone.
- Hawaiian or Pacific Islander Alone: Number of individuals identifying as Native Hawaiian or other Pacific Islander alone.
- Two or More Races: Number of individuals identifying with two or more racial categories.
- Not Hispanic: Number of individuals identifying as non-Hispanic.
- Hispanic: Number of individuals identifying as Hispanic or Latino.
- Pop_Youth: Number of residents classified as youth (typically under age 18).
- Pop_Working: Number of residents within working-age population (typically ages 18–64).
- Pop_Senior: Number of residents classified as seniors (typically age 65 and above).
- Age_Median: Median age of the state’s population.
- Population: Total population used as a reference for immigration-related metrics.
- Lawful_Permanent_Residents: Number of individuals granted lawful permanent resident status (green card holders).
- Nonimmigrants: Number of individuals admitted temporarily for specific purposes (e.g., work, study, tourism).
- Naturalizations: Number of individuals granted U.S. citizenship through naturalization.
- Refugees: Number of individuals admitted as refugees.
- Asylees: Number of individuals granted asylum.
- Lawful_Permanent_Residents_Per_Million: Number of lawful permanent residents per one million residents.
- Nonimmigrants_Per_Million: Number of nonimmigrant admissions per one million residents.
- Naturalizations_Per_Million: Number of naturalizations per one million residents.
- Refugees_Per_Million: Number of refugees admitted per one million residents.
- Asylees_Per_Million: Number of asylees granted per one million residents.

For this project, we only use the first and second dataset.

## Data Processing 🧹

- This data is already precleand by the organizer and I also re-check.

### Merge dataset 1 and dataset 2

```py
state_merge = df1.merge(df2,
                                          left_on=['State', 'Year','Quarter'],
                                          right_on=['State', 'Year','Quarter'],
                                          how='left')
```

### Metrics configuration

```py
# Define what metrics you want to analyze and how to display them
METRICS = {
    'Total_Tax_Revenue': {
        'column': 'Amount',
        'aggregation': 'sum',
        'title': 'Total Tax Revenue',
        'ylabel': 'Tax Revenue ($)',
        'format': lambda x, p: f'${x/1e9:.1f}B' if x >= 1e9 else f'${x/1e6:.0f}M'
    },
    'GDP_Total': {
        'column': 'GDP_Total',
        'aggregation': 'sum',
        'title': 'GDP Total',
        'ylabel': 'GDP ($)',
        'format': lambda x, p: f'${x/1e9:.1f}B' if x >= 1e9 else f'${x/1e6:.0f}M'
    },
    'Total Civilian Labor Force': {
        'column': 'Total Civilian Labor Force',
        'aggregation': 'sum',
        'title': 'Total Civilian Labor Force',
        'ylabel': 'Total Civilian Labor Force',
        'format': lambda x, p: f'{x:,.0f}',  # Format with commas, no decimals
    }
}
```

### Calculate metrics by year 📆

```py
def calculate_metric_by_year(df, states, metric_name, year_range=(2014, 2024)):
    """
    Calculate any metric by state and year

    Parameters:
    -----------
    df : DataFrame
        The main dataframe
    states : list
        List of states to include
    metric_name : str
        Name of the metric from METRICS dictionary
    year_range : tuple
        (start_year, end_year)

    Returns:
    --------
    DataFrame with columns: State, Year, {metric_name}
    """
    if metric_name not in METRICS:
        raise ValueError(f"Metric '{metric_name}' not found. Available: {list(METRICS.keys())}")

    metric_config = METRICS[metric_name]
    column = metric_config['column']
    aggregation = metric_config['aggregation']

    # Filter for year range
    df_filtered = df[(df['Year'] >= year_range[0]) & (df['Year'] <= year_range[1])].copy()

    # Special handling for GDP Per Capita (calculated metric)
    if metric_name == 'GDP_Per_Capita':
        # Calculate GDP per capita = GDP_Total / Population
        # First get averages of GDP and Population by State and Year
        gdp_avg = df_filtered.groupby(['State', 'Year'])['GDP_Total'].mean().reset_index()
        pop_avg = df_filtered.groupby(['State', 'Year'])['Population'].mean().reset_index()

        # Merge and calculate per capita
        result = gdp_avg.merge(pop_avg, on=['State', 'Year'])
        result['GDP_Per_Capita'] = (result['GDP_Total'] * 1_000_000) / result['Population']  # GDP is in millions, so multiply by 1M
        result = result[['State', 'Year', 'GDP_Per_Capita']]
        result.columns = ['State', 'Year', metric_name]
    else:
        # Group by State and Year
        if aggregation == 'sum':
            result = df_filtered.groupby(['State', 'Year'])[column].sum().reset_index()
        elif aggregation == 'mean':
            result = df_filtered.groupby(['State', 'Year'])[column].mean().reset_index()
        elif aggregation == 'median':
            result = df_filtered.groupby(['State', 'Year'])[column].median().reset_index()
        else:
            raise ValueError(f"Unknown aggregation: {aggregation}")

        result.columns = ['State', 'Year', metric_name]

    # Filter for our states of interest
    result = result[result['State'].isin(states)]

    return result
```
## Background information
- Tax haven states are states with personal income tax smaller than 1%. (According to the Reuven S. Avi-Yonah, the Professor of Law from University of Michigan)
- Over the past decade, these states have attracted attention for their relatively strong economic performance, including higher GDP per capita and growing labor forces.
- In January 2025, New Hampshire eliminated its personal income tax. (According to NH Business Review)

This makes it unclear whether the economic benefits come from having low taxes, or from special conditions unique to that state. Because of that, it’s uncertain whether other states would see the same growth if they eliminated their income tax. (According to Urban Institute)

## Key Findings

### High Severance Tax Revenue
![Word Cloud](https://github.com/minhnbnguyen/Datathon/blob/main/UMich_2026/viz/Screen%20Shot%202026-02-15%20at%2017.00.16.png)
- Tax haven states earn higher severance tax (*) revenue than their normal peers, more than $120k/year on average.
(*) Severance Taxes. Taxes on the "severing" of natural resources (oil, gas, timber).


### GDP Per Capita
![Line Graph](https://github.com/minhnbnguyen/Datathon/blob/main/UMich_2026/viz/Screen%20Shot%202026-02-15%20at%2017.00.29.png)
- Based on the economic dataset provided, accounting for all states classified as either tax haven* or non-haven, the tax haven states performed better per capita in mean GDP.
- Tax haven and non-haven states evened out during Covid (2020) but the data shows that tax haven states convincingly outpaced other states and are projected to continue to do better based on a linear regression forecast.


### Impact on Labor Force
![Emotional content](https://github.com/minhnbnguyen/Datathon/blob/main/UMich_2026/viz/Screen%20Shot%202026-02-15%20at%2017.00.36.png)
- The labor force grows much stronger in tax haven states than non-haven states. Based  on the dataset, in the 20 years recorded, tax haven states increased by 1 million while non-haven states increased by around 250k.
- Being a tax haven creates a need for specialized sectors which show more resilience in hiring for specialized labor. It attracts the working population, growing the workforce and increasing the need for essential services.

### Limitations: Tax-haven states can make low income families pay higher overall tax rates
![Emotional content](https://github.com/minhnbnguyen/Datathon/blob/main/UMich_2026/viz/Screen%20Shot%202026-02-15%20at%2017.00.49.png)
![Emotional content](https://github.com/minhnbnguyen/Datathon/blob/main/UMich_2026/viz/Screen%20Shot%202026-02-15%20at%2017.00.55.png)
- Tax-haven states are likely to have higher sales & excise taxes.
- Highest sales & excise taxes states can cause low income families pay higher overall tax rates.
- In the chart above, 6 of the 10 states that depend most on sales and excise taxes are tax havens.

## Recommendations
Stakeholders: State Governments and Policy Makers
→ Based on the historical data, tax havens tend to have:
Increase GDP per Capita
Increasing labor force
Increase severance tax revenue
→ The goal of tax havens
Offset reductions in income tax revenue by increasing alternative sources, such as:
+ Sales tax (e.g., Arizona)
+ Natural resource taxes (e.g., North Dakota)

The cost-of-living is not available in our dataset, so we recommend further research on this problem.
+ Sales tax (e.g., Arizona)
+ Natural resource taxes (e.g., North Dakota)

The cost-of-living is not available in our dataset, so we recommend further research on this problem.

ions
