# Predicting Monthly Residential Electricity Consumption 

Using the United States Energy Information Administration's 2015 Residential Energy Consumption Survey (RECS) dataset, CRISP-DM data mining methodologies are used to better understand the housing features, characteristics, socio-economics, socio-demographics, appliance presence, age and usage frequencies that contribute to an American household's monthly KWh consumption. 

# Overview 

The dataset can be found here: https://www.eia.gov/consumption/residential/data/2015/ in a *.csv file along with the necessary codebook to decipher and better understand the context of all the input variables and imputed values. 

# Data Processing Notes 

The data processing and data wrangling stage required a careful and thorough analysis of the dataset’s 700+ features and 5500+ observations. Its many features included data and information related to a single household’s physical building characteristics, appliance type, age and use frequency in addition to a variety of socio-economic and socio-demographics data about the occupants in each household. Due to the nature of the dataset, which is essentially based on subjective, user-inputs from a questionnaire, many features in the dataset are binned categories or factor variables with relatively few integers, numerics or ordinals (age, annual KwH, annual dollar energy costs, etc.). As such, the dataset contained a significant amount of collinearity among certain features in addition to a high degree of NaN or NA values, and thus required a highly iterative process of data wrangling, processing and cleaning.

-	Many features related to household consumption and appliance use of British Thermal Units or BTUs was immediately culled.  
-	In addition, many detailed and specific cost amounts (in US dollars) was also culled because of its redundancy, as it would likely contribute to multicollinearity, such as annual costs related to specific home appliances. 
-	Not Applicable (NA) or NaN (don’t know, refused to answer) values, coded as “-9” or “-8” were converted to “0”. 
-	Features with extremely high percentage of NAs or NaNs were dropped, but after careful examination, features with high instances of NAs or many NaNs were kept for a modified, inclusionary one-hot encoding effort explained in greater detail in the next section. 

# Feature Encoding & Feature Engineering

Feature encoding and feature engineering is an important aspect of data mining, especially when working with a dataset that has extremely high dimensionality. Prior to culling its extraneous, redundant features, the 2015 RECS dataset contained more than 700 features, many of which are factor variables that include categoricals, ordinals and binary classes. But after manually culling the extraneous features and other independent features that had no ability to affect the dependent outcome variable, specific encoding techniques were considered to retain the integrity and depth of the data’s categorical variables, though it contains only about 400+ features. Converting factors to integers (usually binary) was a commonly used factor encoding technique to help reduce dimensionality and interpretability of the variables’ coefficients. 

-	For example, age-based ordinal factors (age of appliances, etc.) were converted to integers using the mean for each binned category (e.g. ‘15 years old to 19 years old’ was converted just to ‘17’) so as to make them distinct from other binned categoricals that had no obvious order quality (e.g. housing type, etc.). 
-	Certain categorical variables (has pool, has heated pool, uses electricity to heat pool) were consolidated and coded as binary for easier interpretation and simplicity, e.g. (has electric heated pool, yes/no or 0/1). 
-	In addition, one-hot encoding was modified slightly and used to isolate electricity-based information from certain binned categorical variables of fuel use for appliances (central heating, central air conditioning, ovens, stoves/ranges, etc). This task not only reduce dimensionality by removing redundant variables (has a stove, has an oven, has central heating, has central air condition, etc), but also simplified the variable to make it binary (has central heating powered by electricity, yes/no, 0/1). 
-	Separate designations for race in different categorical factors were combined (Hispanic, yes/no with white, Asian, African-American, Native American, etc.) for dimensionality reduction and simplicity.
-	Since appliance use frequency data was captured on a monthly basis, the dataset’s output variable (KwH) was converted from an annual integer total to a monthly integer by dividing by 12. In addition, the annual electricity cost ordinal feature was converted to a monthly integer as well. 
-	In addition, use frequency for various appliances (stoves, ovens, microwaves, etc.) had to be multiplied by four since the value requested was weekly. 
-	Other features engineered included incorporating the average price (in U.S. Dollars) per kwH (in 2015 dollars) based on each household’s particular census region. 
-	The mean annual and monthly income for each household was determined based on the mean range for each binned category of household income, e.g. $20,000-$39,999 was binned as an ordinal number of 3, and was converted to $30,000 and then divided by 12 for a monthly value of 2500. 
-	The monthly income values were divided by monthly electricity costs for a percent known as the energy affordability threshold (~6-7% of monthly take home pay). The threshold was then dummy coded as either ‘0’ or ‘1’, depending on whether the household was above or below the energy affordability threshold. 
-	Also, KWh and dollar amounts were rounded to the nearest ten, e.g. (487 kwH/month becomes 490, etc.) as a way to convert the floats to integers for more accurate interpretability and predictive accuracy. 

