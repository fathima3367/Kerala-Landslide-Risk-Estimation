# Kerala Landslide Risk Estimation

This project analyzes how rainfall conditions, terrain susceptibility, and land-use characteristics relate to different landslide types observed during Kerala's 2018 monsoon. The work is based on the project presentation in [04_PRESENTATION/Kerala_Landslide_Risk_Estimation.pptx](./04_PRESENTATION/Kerala_Landslide_Risk_Estimation.pptx) and a set of geospatial and tabular datasets prepared for machine learning and GIS-based exploration.

## Project overview

Kerala experienced a major monsoon landslide crisis in 2018, with 4,728 landslides mapped across the state. The project addresses a practical research question: can rainfall and land-use information add predictive insight beyond existing geological susceptibility maps when classifying the landslide mechanism?

The presentation frames the problem as follows:

- 4,728 landslides were mapped during the 2018 monsoon.
- Three landslide types were recorded: Debris Flow, Shallow/Surficial Slide, and Rockfall.
- Existing GSI susceptibility maps are important but static and expert-derived; they do not directly model dynamic rainfall exposure or land-use conditions.
- More than 65% of landslides occurred in areas classified as only Moderate or Low susceptibility by GSI, suggesting that rainfall and contextual land-use variables can add meaningful information.

## Research goal

The project does not attempt to predict whether a landslide will happen at a given location without negative samples. Instead, it addresses a valid supervised classification problem: given observed landslide locations, can rainfall and land-use variables help distinguish the type of landslide mechanism observed?

The target variable is:

- Type_of_sl
  - Debris Flow
  - Shallow/Surficial Slide
  - Rockfall

## Data sources

### 1. Landslide inventory
- The analysis uses the 2018 Kerala landslide inventory and its event-type attributes.
- The landslide dataset is in UTM zone 43N (EPSG:32643), matching the geospatial processing used in the workflow.

### 2. Rainfall data
- Manual daily rainfall observations from Kerala stations for 1991-2020.
- Used to derive site-level rainfall exposure metrics for each landslide event.

### 3. Geological susceptibility layers
- District-wise GSI susceptibility datasets for Kerala.
- These layers provide terrain susceptibility labels such as High, Moderate, and Low.

### 4. Processed geospatial dataset
- The repository includes the final ML dataset ([Kerala_Landslide_ML_Dataset.csv](./02_PROCESSED_DATA/Kerala_Landslide_ML_Dataset.csv)) and rainfall station features ([Rainfall_Station_Features_2018.csv](./02_PROCESSED_DATA/Rainfall_Station_Features_2018.csv)).
- The processed table includes approximately 4,606 records and multiple variables including:
  - District
  - GSI susceptibility
  - Land-use class (LU_2010)
  - Slope / elevation-derived terrain measure (RASTERVALU)
  - Total rainfall
  - Mean daily rainfall
  - Max 1-day rainfall
  - Max 3-day rainfall
  - Max 7-day rainfall
  - Coverage ratio (rainfall station coverage / data reliability proxy)

The raw inventory, rainfall observations, and district susceptibility source layers are not included in this repository file set.

## Methodology

The project follows an integrated GIS + machine-learning workflow:

1. Use the 2018 landslide inventory as the event database.
2. Combine landslide locations with rainfall station observations.
3. Attach geological susceptibility information from GSI layers.
4. Incorporate land-use context and terrain-related attributes.
5. Construct a supervised multi-class prediction problem for landslide type.
6. Evaluate competing models using 5-fold cross-validation.

The presentation indicates that the predictive workflow used Altair AI Studio and the target/predictor structure was defined as:

- Target: Type_of_sl
- Predictors:
  - GSI_Susceptibi
  - LU_2010
  - RASTERVALU (slope)
  - Total_Rainfall
  - Mean_Daily_Rainfall
  - Max_1Day_Rainfall
  - Max_3Day_Rainfall
  - Max_7Day_Rainfall
  - Coverage_Ratio

Important note from the presentation: several raw variables were excluded to avoid leakage, including area, length, width, damage, remarks, coordinates, and other post-event or source-confirmation fields.

## Model results summary

The presentation compares three classifiers:

- Decision Tree
- Random Forest
- Gradient Boosted Trees

Result highlights from the presentation:

- Overall accuracy:
  - Decision Tree: 72.12%
  - Random Forest: 73.88%
  - Gradient Boosted Trees: 73.93%
- Best performing model: Gradient Boosted Trees
- The model achieved the best recall and precision for the minority Rockfall class.
- The project notes this as a meaningful result rather than a mere "highest accuracy" outcome.

## Feature importance and interpretation

The presentation ranks the major drivers as follows:

1. Slope (RASTERVALU) — dominant predictor
2. Coverage_Ratio — rainfall-data reliability proxy
3. Total_Rainfall
4. Mean_Daily_Rainfall
5. Max_1Day_Rainfall
6. Max_3Day_Rainfall
7. GSI_Susceptibi (High)

Key interpreted findings:

- Slope is the strongest predictor of landslide type by a wide margin.
- Rainfall intensity measures form a strong second tier of influence.
- GSI susceptibility still carries useful signal.
- Individual land-use categories were comparatively weaker as predictors in this setup.

Important caveat: Coverage_Ratio is not treated as a direct physical driver; it also reflects data quality and station coverage.

## Honest limitations

The presentation explicitly highlights the project’s data and methodological limits:

- This is not a landslide occurrence model; it is a landslide-type classification model.
- It does not provide real-time early warning because event-level rainfall timing is not available per landslide.
- It does not establish direct causation between land use and landslides.
- Rockfall is an underrepresented minority class.
- Only around 60 rainfall stations exist statewide, so some landslides may be matched to stations several kilometers away.
- 122 landslides (2.58%) lacked GSI data and were excluded from the modeling step.

## Future scope

The presentation identifies several next steps:

- Address class imbalance more directly.
- Build a validated spatially-aware negative/background dataset to support true landslide-occurrence modeling.
- Include land-use change between 2010 and 2018 as a predictor.
- Expand or improve rainfall monitoring coverage and interpolation.
- Map model outputs back into a GIS platform for spatial risk characterization and disaster-preparedness planning.

## Repository structure

```text
.
├── README.md
├── 02_PROCESSED_DATA/
│   ├── Kerala_Landslide_ML_Dataset.csv
│   └── Rainfall_Station_Features_2018.csv
├── 03_RAPIDMINER/
│   └── Kerala_Landslide_Risk_Model.rmp
├── 04_PRESENTATION/
│   └── Kerala_Landslide_Risk_Estimation.pptx
└── screenshots/
    ├── qgis_map.png
    ├── model_workflow.png
    └── feature_importance.png
```

The screenshots directory contains project visuals:

![Kerala landslide map](./screenshots/qgis_map.png)

![Machine-learning workflow](./screenshots/model_workflow.png)

![Feature importance](./screenshots/feature_importance.png)

## Tools and environment used

Based on the project files and presentation, the project uses:

- QGIS / geospatial processing for spatial data integration
- RapidMiner for the ML process model
- Altair AI Studio for the machine learning workflow in the presentation
- CSV, shapefile, and GeoPackage datasets for the analytical pipeline

## Practical notes

This repository is best viewed as a GIS and predictive analytics prototype for Kerala landslide characterization rather than a full production risk model. The present implementation is focused on understanding how observed rainfall and susceptibility signals are associated with different landslide mechanisms in a historical inventory.

## Citation / project identity

The presentation identifies the project as:

- Kerala Landslide Risk Estimation Using Rainfall, Terrain Susceptibility and Land-Use Factors
- Author: Fathima M J
- Academic affiliation shown in the deck: B.Tech Artificial Intelligence & Data Science, Ilahia College of Engineering and Technology
- Achievement: Consolation Prize, IEDC Summit Data Science Contest

## License

No explicit project license file is included in this repository snapshot. Please treat the content as research/project material unless an additional license is added later.
