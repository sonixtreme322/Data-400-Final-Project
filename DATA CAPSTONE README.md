# Starred and Questioned
### Are Michelin Stars Efficiently Allocated Across Global Markets in 2026?
**Data Science Capstone Project · David Whitmer & Benjamin Fox · 2026**

---

## Overview

This project investigates whether Michelin star allocation across 47 countries in 2026 can be explained by structural economic and market variables — or whether institutional history and geographic bias play a measurable role. We scrape the full 2026 Michelin Guide, compile external economic indicators, and fit a Negative Binomial regression to produce a country-level Star Allocation Residual: the gap between how many stars a country has and how many its economic profile predicts.

---

## Repository Structure

```
├── Every_michelin_restaurant_in_the_world.csv      # Raw scraped Michelin data
├── michelin_guide_2026 (1).csv                     # Cleaned restaurant-level data (18,837 rows)
├── michelin_star_restaurants_all_over_the_world_current.csv
├── external_variables.csv                          # Country-level external variables (47 rows)
├── michelin_country_model_dataset.csv              # Aggregated model-ready dataset (47 rows × 44 cols)
├── priceRanges FYI.csv                             # Michelin local-currency pricing tiers
├── Initial Data Collection.ipynb
├── US_Michelin_Stars_Current_v2 (1).ipynb
├── Michelin_NegativeBinomial_Regression.ipynb      # Negative binomial regression (David)
├── Michelin Classifications.ipynb                  # Classification models (Ben)
├── negbin_results copy.png                         # Regression scatter + residual bar chart
├── Michelin Restaurants Per Country.png
├── decisionTree1.png                               # Decision tree visualization
├── randomForestMatrix.png                          # Random forest confusion matrix
├── gradientBoostedMatrix.png                       # Gradient boosted confusion matrix
├── boostedFeatures.png                             # Boosted model feature importance
├── forestFeatures.png                              # Random forest feature importance
├── starred_and_questioned.Rmd                      # Xaringan presentation
├── michelin_man.png
└── Symposium Poster.pdf
```

---

## Data Sources

| Variable | Source |
|----------|--------|
| Michelin Guide 2026 | Algolia API — guide.michelin.com (scraped March 2026) |
| GDP per Capita | World Bank WDI `NY.GDP.PCAP.CD` · IMF WEO for Taiwan |
| Urbanization Rate | World Bank WDI `SP.URB.TOTL.IN.ZS` |
| Tourism Arrivals | UNWTO World Tourism Barometer + national authorities |
| Restaurant Counts | Eurostat NACE 56.1 · national equivalents for non-EU |

---

## Methodology Summary

The 2026 Michelin Guide was scraped via the Algolia frontend API, yielding 18,837 restaurants across 53 territories. After cleaning — consolidating absorbed territories (Andorra/Monaco → France, Liechtenstein → Switzerland, Hong Kong + Macao combined, Abu Dhabi + Dubai → UAE), fixing the China Mainland naming inconsistency, and normalizing Michelin's local-currency pricing tiers to USD — the data was collapsed to 47 country rows. A Negative Binomial regression (NB2) was fit with total starred restaurants as the dependent variable and log-transformed GDP per capita, restaurant count, and tourism arrivals as primary predictors. The Star Allocation Residual (actual minus predicted) identifies systematically over- and under-represented markets.

---

## David's Analysis — Negative Binomial Regression

GDP per capita, restaurant market size, and inbound tourism arrivals are each highly significant (p < 0.001) with comparable standardized coefficients. The dispersion parameter α = 0.52 indicates substantial unexplained variance after controlling for structural factors. The five most over-represented markets are France (+297), Italy (+206), Japan (+152), Germany (+147), and Spain (+141). The five most under-represented are the USA (−325), South Korea (−136), China Mainland (−108), Denmark (−87), and Türkiye (−46). The USA gap of −325 exceeds Italy's entire star count.

---

## Ben's Analysis - Random Forest and Gradient Boosted Classifiers

In determining which variables were most important to a given restaurant's Michelin Award, latitude, longitude, and pricing all played the largest roles. With the gradient boosted classifier giving the largest sway, 40% of award determination, onto whether or not one had luxury pricing. However, most highly significant variables hovered around the 10-20% feature importance range.

---

## Conclusion

The negative binomial regression confirms that Michelin star allocation is meaningfully but incompletely explained by structural economic and market variables. GDP per capita, restaurant market size, and inbound tourism arrivals each independently predict star counts with comparable magnitude — no single factor dominates. The dispersion parameter α = 0.52 indicates that substantial unexplained variance remains after controlling for these factors, and the residuals reveal that this variance is not random. Legacy European markets — France, Italy, Germany, Spain, and Belgium — are systematically over-represented, while large Asian and North American markets — the USA, South Korea, and China Mainland — are systematically under-represented. The USA gap of −325 stars, larger than Italy's entire star count, is the clearest quantitative expression of Michelin's geographic bias. The pattern is consistent with a guide whose allocation reflects over a century of institutional history in European fine dining as much as it reflects objective quality measurement across global markets.

Classification models further emphasize the economic component of Michelin Award allocation, but point to a large component of bias within their selection system. The feature importance of longitude and latitude, as well as initial decision tree models placing importance on Western Europe, signals that's where their bias lies. Which makes sense as Michelin Guide headquarters are in France. Still, this does not explain everything, especially with these models having a prediction accuracy of around 0.68. Likely only being that high due to most restaurants within the guide merely being selected. Thus, guessing that any given restaurant with a Michelin award was only selected is a fair one. All the same, Michelin's award principles seem to lie in pricing and geographic importance above all else.

---

## Requirements

**Python:** `pandas` `numpy` `scipy` `matplotlib`  
**R:** `xaringan`  
**Node.js:** `docx` (for methodology report generation)
