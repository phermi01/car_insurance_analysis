# Overview

Welcome to  my analysis of car insurance dataset, which seeks to guide insurance companies in decision-making. This Project is one my portfolio projects for my course in Python for Data Analytics by Regonet Global. It explores the relationship between key car technical specifications and insurance ratings to help guide insurance companies.


The dataset consists detailed information about various cars, including their technical specifications, insurance risk ratings, and normalized loss values. Through a series of python scripts, I explored key relationships between car technical specifications and insurance risk ratings.


# The Questions

Below are the questions I want to answer in my project:
1.  What are the top car makes by average price?
2.  How do top car  cars makes  feared against normalized losses?
3.  What is the relationship between car price and fuel type?
4.  How do  car price affects normalized losses, and symboling?
5.  Are there relationship between normalized losses, car make and fuel type?
6.  What correlation exists between car features and insurance ratings?

# Tools I Used

For my deep dive into the car insurance analysis, I harnessed the power of several key tools:

Python: The backbone of my analysis, allowing me to analyze the data and find critical insights

I also used the following Python libraries:
1.  Pandas Library: This was used to analyze the data.
2.  Matplotlib Library: I used it to visualized the data.
3.  Seaborn Library: Helped me create more advanced visuals.
4.  Visual Studio Code: My go-to for executing my python scripts
5.  Jupyter Notebooks: The tools I used to run my Python scripts which let me easily include my noted and analysis.
6.  Git & GitHub: Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.


# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

Import and Cleanup Data: I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

``` python
# Importing Libraries

import pandas as pd
import matplotlib.pyplot as plt
from datasets import load_dataset
import seaborn as sns
from adjustText import adjust_text
import re
from matplotlib.ticker import FuncFormatter
import plotly.express as px

# Dataset Loading
car = pd.read_csv(r"C:\Users\DELL\Desktop\Regonet_project\automobile_dataset.csv")

car.sample(10, random_state=42)  # Displaying 10 random samples from the dataset

# make a copy of the dataset
car_1 = car.copy()

# Data Cleaning

# Removing extra space (including multiple internal spaces) from all string cells
for col in car_1.select_dtypes(include='object'):
    car_1[col] = car_1[col].map(lambda x: re.sub(r'\s+', ' ', x).strip() if isinstance(x,str) else x)

# Rename the columns to remove hyphens and replace them with underscores
car_1 = car_1.rename(columns={'highway-mpg': 'highway_mpg', 'city-mpg': 'city_mpg','peak-rpm': 'peak_rpm','compression-ratio': 'compression_ratio',
                      'fuel-system': 'fuel_system','engine-size': 'engine_size','num-of-cylinders': 'num_of_cylinders','engine-type': 'engine_type',
                      'curb-weight': 'curb_weight','wheel-base': 'wheel_base','engine-location': 'engine_location','drive-wheels': 'drive_wheels',
                      'body-style': 'body_style','num-of-doors': 'num_of_doors','fuel-type': 'fuel_type','normalized-losses': 'normalized_losses',})

# Convert numeric columns
numeric_cols = ['price', 'highway_mpg', 'city_mpg', 'peak_rpm', 'horsepower',
       'compression_ratio', 'stroke', 'bore', 'engine_size',
       'num_of_cylinders', 'curb_weight', 'height', 'width',
       'length', 'wheel_base', 'num-of-doors', 'normalized_losses',
       'symboling']

# dealing with missing values in the car dataset

car_1['price'] = car_1['price'].fillna(car_1['price'].median())                                              # Filling missing values in 'price' with the median value of the column
car_1['peak_rpm'] = car_1['peak_rpm'].fillna(car_1['peak_rpm'].median())                                     # Filling missing values in 'peak_rpm' with the median value of the column
car_1['horsepower'] = car_1['horsepower'].fillna(car_1['horsepower'].median())                               # Filling missing values in 'horsepower' with the median value of the column
car_1['stroke'] = car_1['stroke'].fillna(car_1['stroke'].median())                                           # Filling missing values in 'stroke' with the median value of the column
car_1['bore'] = car_1['bore'].fillna(car_1['bore'].median())                                                 # Filling missing values in 'bore' with the median value of the column
car_1['num_of_doors'] = car_1['num_of_doors'].fillna(car_1['num_of_doors'].mode())                           # Filling missing values in 'num_of_doors' with the mode value of the column
car_1['normalized_losses'] = car_1['normalized_losses'].fillna(car_1['normalized_losses'].median())          # Filling missing values in 'normalized_losses' with the median value of the column

```

# The Analysis

## 1.  Top 10 Car Makes by Average Price

#### Visualize Data

``` PYthon
sns.set_theme(style="ticks")

sns.barplot(data =top_10_cars_by_avg_price , x = 'avg_price', y= 'make',  hue='avg_price', palette ='dark:b_r')
plt.title('Top 10 Car Makes by Average Price')
plt.ylabel('')
plt.xlabel('')
ax = plt.gca()
ax.xaxis.set_major_formatter(FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

plt.legend().remove()

plt.tight_layout()
plt.show()
```

### Results
![Visualization of Top 10 Car Makes by Price](car_insurance\Images\1_top_10_car_makes_by_price.png)

### Insights
#### Luxury and Premium Vehicle Segment
-   Jaguar, Mercedes-Benz, Porsche, and BMW dominates the top four spots, with average prices ranging from $25K to over $35k. 
These are luxury brands that typically incur higher repair/replacement costs, may involve specialized parts and services, and are targets for theft or vandalism in some areas.

#### Upper-Mid Range Vehicles
-   Volvo, Audi, and Mercury fall in the $15K -$20K range. These are still relatively premium brands, often associated with safety features, moderate reapir cost, and a customer base with generally lower risk profiles.

#### Affordable Premium Tier
-   Alfa-Romeo, Peugeot, and Saab occupy the lower end of the top 10, averaging under $15K. Though ranked among the top 10 by price, they are affordable by luxury standards. Potentially older models or less market penetration may affect availability of parts and repiar costs, which can still drive claim sizes.
 
### Recommendations for Insurance Companies
-   Premiums for luxury and premium vehicle segment should reflect higher potential claim values, even if accident frequency is low.
-   Upper-mid range vehicles may merit adjusted premiums, balancing between moderate risk and repair costs.
-   Affordable premium tier could attract higher-than-expected losses relative to their price, particularly if parts availability is limited or niche.

# The Analysis

## 2.  Top 10 Car Makes by Normalized Losses

#### Visualize Data

```python
sns.set_theme(style="ticks")

sns.barplot(data =top_10_cars_by_avg_normalized_losses , x = 'avg_normalized_losses', y= 'make',  hue='avg_normalized_losses', palette ='dark:b_r')
plt.title('Top 10 Car Makes by Average Normalized Losses')
plt.ylabel('')
plt.xlabel('')

plt.legend().remove()

plt.tight_layout()
plt.show()
```

### Results

![Visualization of Top 10 Car Makes by Normalized Losses](car_insurance\Images\2_top_10_car_makes_by_normalized_losses.png)

### Insights
#### BMW Has the Highest Normalized Losses
BMW tops the list, signaling that on average, it incurs the highest adjusted insurance losses among all brands. This suggests either a high claim frequency, high repair costs, or both. BMWs are generally performance-oriented and expensive to repair, and they may be driven in ways that expose them to higher accident risk.

####    Luxury Brands Are Not Always Low-Risk
Other premium makes such as Audi, Porsche, and Jaguar also appear in the top 10. While luxury often implies better vehicle construction and safety features, these vehicles do not necessarily correlate with lower insurance losses.

####    Mainstream Brands Also Carry Significant Risk
Brands like Peugeot, Mitsubishi, Dodge, Nissan, and Plymouth being in the top 10 shows that normalized losses are not limited to high-end vehicles. These brands are generally more affordable, so their presence here suggests that they may experience higher claim frequencies or be more frequently involved in accidents.

####    Jaguar's Presence Is Noteworthy
Jaguar, while the most expensive brand based on average price (as seen in previous analysis), still appears in the top 10 for normalized losses. This indicates that despite a smaller user base and presumed owner caution, claim costs or frequencies remain high.

#### Repair Costs May Be a Major Driver
Normalized losses include both frequency and severity. Many of the makes listed (e.g., Porsche, BMW, Audi) are known for specialized components, imported parts, and complex engineering. Even with modest claim frequency, repair severity can drive up the total losses.

#### Need for Nuanced Premium Segmentation
This chart demonstrates that risk is not uniform across vehicle brands and cannot be predicted solely by price, type, or country of origin. Both economy and luxury brands are represented here, underscoring the need for granular segmentation strategies.

### Recommendations for Insurance Companies
-  BMW, Peugeot, and Audi pose the highest insurance risks and should be treated with stricter underwriting and higher base premiums.
-   Brands like Mitsubishi and Nissan signal the importance of examining driver behavior and demographics, not just vehicle type.
-   Luxury vehicle repair costs should be heavily factored into claims cost projections, even for vehicles that appear safer.


# The Analysis

## 3.  What is the relationship between car price and fuel type?

#### Visualize Data

``` Python
plt.figure(figsize=(8, 6))
sns.boxplot(x='price', y='fuel_type', data=car_1, vert=False, order=fuel_type_order)
plt.title("Boxplot of Price by Fuel Type")
plt.show()
```
### Results

![Visualization of car price and fuel type](car_insurance\Images\3_car_price_by_fuel_type.png)

### Insights
#### Diesel Cars
-   Diesel cars show less price volatility. Price range is tighter, possibly indicating a more homogeneous market segment (e.g., SUVs, utility vehicles, etc.). These could be owned by commercial users or efficiency-conscious individuals, leading to different risk profiles (e.g., higher mileage per year, more wear and tear).

#### Gasoline Cars
-    The presence of many outliers for gasoline vehicles above $30,000 suggests that:
        -   Luxury cars (e.g., Jaguar, Porsche) predominantly use gas.
     -   Risk exposure could be inconsistent among gas cars- some are economy vehicles, others are high-value models

-    Lower medium price in gasoline vehicles suggests higher volume of budget vehicles which are more prone to:
        -   Higher claim frequencies (e.g., driven by younger or high-risk profiles)
     -   Higher utilization and lower safety features

### Recommendations for Insurance Companies
-   Insurers are advised to consider specialized underwriting policies or fleet-based for diesel owners.
-   Insurers are encourage to  use stratified pricing tiers with gas-powered vehicles, distinguishing economy from premium.


# The Analysis

## 4.  How does car price affects normalized losses and symboling?

#### Visualize Data

``` python
axes[1].set_title('Top 10 Car Makes by Avg Price')
axes[1].set_xlabel('Average Price')
axes[1].set_ylabel('')

# Add colorbar for symboling
norm = plt.Normalize(car_grouped['avg_symboling'].min(), car_grouped['avg_symboling'].max())
sm = plt.cm.ScalarMappable(cmap='viridis', norm=norm)
sm.set_array([])  # Empty array for the colorbar
cbar = fig.colorbar(sm, ax=axes, orientation='vertical', fraction=0.02, pad=0.04)
cbar.set_label('Average Symboling')

# Final layout tweaks
plt.show()
```
### Results
![Visualization of car price, noralized losses, and symboling](car_insurance\Images\4_normalized_losses_price_and_symboling_of_top_and_bottom_10_cars.png)


### Insights
#### Price Is Not a Direct Proxy for Risk
A crucial insight is that higher average price does not automatically lead to higher normalized insurance losses:
-   Volvo, although a high-priced brand, shows the lowest average losses among all makes in both panels. This implies high safety standards, cautious driving behaviour, and favourable claim profiles.
-    In contrast, BMW, another high-end brand, shows the highest losses among all brands, indicating a poor-to-price ratio.

#### Volvo Sets the Gold Standard for Safe Luxury
Volvo is an outlier in the top 10 panel, showing:
-   Lowest average normalized losses (~90).
-   Average price around $18,000 - $20,000, mid-range in the high-end panel
-   Average symboling = -1, indicating manufacturer-recognized low risk.
This alignment between low loss, moderate price, and low symboling underscores Volvo's reputation for safety and reliability.

#### BMW and Peugeot are High-Risk Luxury Makes
Both BMW and Peugeot are concerning:
-   BMW shows the highest normalized losses (~150+) despite being a high-cost brand (~$25,000 - $30,000).
-   Peugeot exhibits very high losses (~140+) while being less expensive (~$15,000 - $18,000).
-   BMW's symboling is low (around 0), which does not reflect its actual insurance risk, suggesting a misalignment between manufacturer expectations and real-world loss experience.

#### Mixed Symboling Accuracy: Manufacturer Estimates Are Inconsistent
Symboling, meant to indicate manufacturer-assessed risk, is not always predictive of actual loss behaviour:
-   Mitsubishi (in bottom 10) shows a very high insurance losses (~140) and a symboling of 2, somewhat accurately flagging risk.
-   BMW, on the other hand, has a symboling of 0 but is the riskiest overall.
 -   Volvo has a negative symboling(-1) and the lowest losses, confirming good alignment.

#### Bottom-Tier Cars Also Exhibit High Losses
Not all affordable cars are safe bets:
-   Mitsubishi, Dodge, and Plymouth all show losses well above 120, with average prices below $10,000.
-   These brands likely attracts riskier drivers (e.g., younger demographics, budget-constrained individuals), leading to frequent and/or severe claims.

#### Chevrolet and Subaru Show Favourable Loss Profiles
In the bottom-tier panel:
-   Chevrolet and Subaru both demonstrate low normalized losses (~100 or below) despite low prices.
-   Subaru has a negative symboling (-1), accurately flagging it as a low-risk make.

#### Middle-Risk Brands Cluster Around Moderate Symboling
In the top panel:
    -   Audi, Alfa-Romeo, Mercury, and Saab cluster around moderate losses (~115-130) and have symboling values around 1 and 2.
    -   Their insurance risk in neither too high nor particularly favourable.

#### Jaguar and Porsche: Premium Brands with High Variability
- jaguar has high prices (~$34,000) and elevated losses (~135). its symboling is 0, again underestimating its actual risk.
 -   Porsche shows slightly lower losses (~125 -130), but its symboling is 2, better indicating risk.

#### Volkswagen and Toyota: Balanced in Low-Tier Segment
-   Both makes are in the bottom-10 panel with moderate prices(~$9,000-$10,000) and losses around 110-120.
-   Their symboling is high (2), which somewhat overstate their risk.

#### Recommendations for Insurance Companies.
-   Incorporate Actual Loss Data Over Symboling Alone: While syboling offers a manufacturer perspective, real-world loss experience varies significantly. Use it as a supporting feature, not a primary driver.
-   Reward efficient risk makes like Volvo, Subaru, and Chevrolet with premium incentives or and inclusion in preferred policy groups.
-   High-risk brands like BMW, Peugeot, Mitsubishi, and Dodge should be flagged for actuarial review because they demonstrate poor risk profiles and require targeted pricing reviews and possibly stricter underwriting criteria.
-   Avoid relying solely on Manufacturer's Suggested Retail Price (MSRP) or luxury status. Risk-adjusted profitability is more important than vehicle segment alone.
-   Segment premiums by risk tiers, not just price by blending loss trends, driver profiles, and make/model data to create precise premium bands.
-   Telematics usage-based pricing, especially for brands attracting higher-risk drivers as dynamic pricing based on bahaviour could help mitigate exposure.

# The Analysis

## 5.  What correlation  exists between normalized losses, car make and fuel type?

#### Visualize Data
```python
plt.figure(figsize=(10,8))
sns.heatmap(car_1_pivot, annot=True, cmap='YlOrRd')
plt.title('Average Normalized Losses by Make and Fuel Type')
plt.show()
``` 
### Results
![Visualization of  car makes, noralized losses, and fuel type](car_insurance\Images\5_heatmap_of_car_makes_normalized_losses_and_fuel_type.png)

### Insights
#### High-Risk Manufacturers
Some car brands consistently show higher normalized losses, indicating greater insurance risk regardless of fuel type. These include:
-   BMW and Peugeot exhibit among the highest average losses (around 150), suggesting frequent or costly claims. These brands likely attract more aggressive driving behaviours or carry higher repair costs.
-   Audi, Nissan, and Mitsubishi also rank high, with losses in the 140s. These may be seen as performance or mid-range sporty vehicles, again implying greater exposure.
For underwriting purposes, vehicles from these manufacturers should be flagged for higher premium loading or stricter policy terms.

####    Low-Risk Manufacturers
On the opposite end, some brands consistently exhibit lower normalized losses, suggesting lower claims frequency or severity:
-   Toyota and Mercedes-Benz have diesel variants with significantly lower losses (82 and 93 respectively). These could be considered safer bets for insurers and may qualify for discounts or standard risk categories.
-   Volvo (diesel: 95) also performs well, aligning with its longstanding reputation for safety.
These brands should be considered for favorable pricing models or policy incentives.

####    Impact of Fuel Type on Risk
Fuel type plays a subtle but noticeable role in insurance risk:
-   In several brands (e.g., Peugeot, Nissan, Volkswagen), gas variants exhibit higher normalized losses than diesel counterparts. This could be attributed to:
    -   Gas-powered cars being more performance-oriented and used more aggressively.
    -   Diesel cars often being more economical and used for longer commutes, encouraging safer driving.
This pattern suggests that fuel type can be incorporated into risk rating models. Diesel vehicles might receive a slight reduction in base premiums, especially when tied to historically safer brands.

####    Consistency Across Fuel Types
Some brands maintain consistent risk profiles regardless of fuel type, such as:
-   Mazda, Jaguar, and Renault show little variance between fuel types, with average losses around 120.
This indicates that for these makes, fuel type may be a non-significant factor in determining risk, and pricing should focus more on other vehicle or driver-level factors.

### Recommendations for Insurance Companies.
-   Group makes into risk tiers based on their average normalized losses. For instance:
    -   High-risk tier: BMW, Peugeot, Audi.
    -   Mid-risk tier: Mazda, Mitsubishi, Jaguar.
    -   Low-risk tier: Toyota, Mercedes-Benz, Volvo.
-   Apply modest premium reductions for diesel variants of known low-risk makes. Penalize gas variants when linked with historically high-loss brands.
-   Combine make-fuel risk insights with other vehicle attributes (engine size, weight, horsepower) and customer behavior to refine underwriting standards.
-   Consider offering telematics programs or defensive driving discounts for drivers of high-loss vehicles. This could help reduce exposure while keeping high-risk customers engaged.
-   Position insurance products with value-based incentives toward drivers of lower-risk makes, especially diesel Toyota and Mercedes-Benz owners. These are cost-efficient segments with lower expected losses.


# The Analysis
## 6.  What correlation exists between car features and insurance ratings?

#### Visualize Data
```python
corr_matrix = car_1[numeric_cols].corr()

plt.figure(figsize=(12,10))
sns.heatmap(corr_matrix, annot=True, cmap='coolwarm', center=0)
plt.title('Correlation Heatmap of Car Features')
plt.show()
```
### Results
![Correlation heatmap of car fearures](car_insurance\Images\6_correlation_heatmap_of_car_features.png)

### Insights
#### Factors Positively Correlated with Normalized Losses
Features with a moderate positive correlation to normalized losses are potentially indicative of higher claim risk:
-   Symboling (r = 0.46): This is the most strongly correlated feature with normalized losses. “Symboling” is a manufacturer’s assigned risk rating for the vehicle; higher values likely denote sportier or riskier cars. Insurers should give significant weight to this when assessing base risk.

-   Engine Size (r = 0.073) and Horsepower (r = 0.071) show weak positive correlations, suggesting that more powerful cars may slightly increase insurance risk. However, the correlation is not strong enough to be used independently—these should be factored as modifiers rather than primary risk drivers.

-   Curb Weight, Width, Length, and Bore have negligible to weak correlations with normalized losses. These structural features may affect repair costs rather than claim frequency.

####    Factors Negatively Correlated with Normalized Losses
Certain features are negatively associated with normalized losses, though most have weak or very weak correlations:
 -   Height (r = -0.37): Taller vehicles may be associated with lower risk, possibly because they tend to be larger, safer, or used more conservatively (e.g., SUVs or family cars). This implies a potential for marginal premium discounts for vehicles with greater height.
-   Wheel Base (r = -0.36) and Length (r = -0.23) also show some weak negative correlation, aligning with the idea that larger vehicles may be involved in fewer or less severe incidents.

####    Price vs. Risk
Interestingly, price and normalized losses are nearly uncorrelated (r = -0.095). This counters a common assumption that more expensive cars automatically carry higher insurance risk.
High-price vehicles are often better built and driven more cautiously, but they also come with expensive repair costs. The near-zero correlation suggests that price alone is a poor predictor of insurance loss, and relying on it in isolation could misprice risk.

####    Strong Correlations Among Vehicle Features
Several internal vehicle features are highly correlated with each other:
-   Engine Size and Horsepower (r = 0.81)
-   Engine Size and Price (r = 0.86)
 -   Curb Weight and Engine Size (r = 0.85)
-   Length, Width, and Wheel Base all show strong   positive inter-correlations
These high correlations suggest that many of these physical and performance attributes tend to scale together. For modeling purposes, multicollinearity must be managed, as these features are not independent.

#### Fuel Efficiency (MPG) vs. Risk
Highway MPG (r = -0.15) and City MPG (r = -0.19) both show weak negative correlations with normalized losses. This suggests that more fuel-efficient cars may tend to be safer, potentially due to their lower performance profiles or more conservative usage.

### Recommendations for Insurance Companies.
-   Use symboling as a core risk feature because its strong correlation with normalized losses makes it highly predictive of claim behavior. Base premiums can be scaled accordingly.
-   Design risk tiers based on physical characteristics by grouping vehicles into broad risk categories using height, wheel base, and engine size. Larger and taller vehicles generally pose lower risk.
-   Avoid overreliance on price because despite intuitive appeal, price does not predict loss frequency or severity. Focus on mechanical and performance traits instead.
-   Bundle correlated features intelligently  by developing composite or principal component scores to avoid redundancy, instead of including all related features (like engine size, horsepower, and curb weight).
-   Promote low-risk, high-efficiency vehicles by
considering offering discounts for cars with high MPG and modest performance specs, especially when combined with favorable brand histories.


# What I Learned
Throughout this project, I deepened my understanding of the car insurance underwriting and risk analysis and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:
-   Advanced Python Usage: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
-   Data Cleaning importance: I learned that thorough data cleaning and preparation are crucial before any meaningful analysis can be conducted, ensuring the accuracy of insights derived from the data.
-   

# Insights
This project provided several general insights for insurance companies for assessing and determining underwriting for cars:

-   Luxury Doesn’t Always Mean Low Risk
Luxury brands like BMW, Jaguar, and Porsche show high normalized insurance losses, driven by expensive repairs, performance-driven use, and theft risk. Meanwhile, Volvo stands out with low losses despite being a high-priced brand—thanks to its safety reputation and cautious drivers. This proves that vehicle price isn't a reliable predictor of risk, as price and loss correlation is nearly zero (r = -0.095).
-   Some Affordable Brands Carry High Risk
Lower-cost makes such as Peugeot, Mitsubishi, Dodge, and Plymouth show surprisingly high loss rates, indicating that budget vehicles can attract higher-risk drivers, like younger or economically constrained individuals. Chevrolet and Subaru, however, break this trend with low losses, showing that affordability doesn't always equal risk.
-   Symboling Scores Are Inconsistent Predictors
Manufacturer-assigned symboling values are meant to reflect risk, but their accuracy varies. For example, BMW (symboling 0) has the highest losses, while Volvo (symboling -1) accurately reflects its low-risk profile. This inconsistency suggests symboling should be used cautiously in risk assessment.
-   Fuel Type Affects Risk Profiles
Diesel vehicles tend to show lower and more consistent losses, possibly due to more conservative use (e.g., long-distance or commercial driving). In contrast, gasoline cars show higher variability and higher loss averages, especially for brands like Peugeot and Nissan, hinting at performance-driven usage patterns.
-   Risk is Multifactorial, Not Price-Driven
Physical and performance attributes (engine size, horsepower, curb weight, etc.) have weak to moderate correlation with losses. Symboling has the strongest link (r = 0.46), while others like MPG, height, and wheelbase show weak negative correlations. These factors must be considered together, especially given high inter-feature correlations, to avoid multicollinearity in predictive models.

# General Recommendations
Below are my recommendation for insurance companies based on my indept analysis of the car insurance dataset:

-   Adopt tiered premium strategies based on risk, not price by grouping vehicles into high-, mid-, and low-risk tiers using normalized loss data, rather than MSRP, and align premiums accordingly—rewarding low-risk brands like Volvo and penalizing high-risk ones like BMW and Peugeot.

-   Incorporate fuel type and driving behavior into pricing models by applying modifiers for fuel type (e.g., discounts for low-risk diesel variants), and implement telematics or usage-based pricing to better assess real-world driver risk—especially for gas-powered or high-loss vehicles.

-   Refine underwriting using multivariate risk factors by enhancing underwriting criteria by blending vehicle attributes (e.g., engine size, height, horsepower) with driver demographics and brand-specific loss trends to build a more accurate risk profile.

-   Use symboling and loss data together for pricing accuracy by leveraging symboling as a secondary indicator of manufacturer-intended risk, but prioritize actual historical loss performance for setting premiums and assessing exposure.

-   Promote and incentivize low-risk, high-efficiency vehicles by encouraging adoption of vehicles with high MPG, conservative specs, and strong brand safety histories (e.g., Toyota, Chevrolet, Subaru) through premium discounts and value-based marketing.

# challenges I Faced
This project presents certain challenges which I turned into good learning opportunities:

-   Data Inconsistencies: Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
-   Complex Data Visualization: Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
-   Balancing Breadth and Depth: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details

# Conclusion
This exploration into car insurance analysis has being highly  informative, emphasizing how effective car risk assessment must integrate multiple vehicle and driver factors rather than rely on simple proxies like price or band status. The insights I got enhanced my understanding and provide actionable guidance for insurance companies. As car makes and complexities continue to evolve, continuous analysis will be critical to stay ahead in vehicle risk assessments. This project is a good foundation for future exploration and underscores the importance of continuous learning and adaptation in insurance field.