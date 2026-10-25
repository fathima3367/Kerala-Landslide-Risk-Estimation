# Kerala Landslide Risk Estimation

I developed this project to study how rainfall, terrain susceptibility, and land-use information relate to the types of landslides observed during Kerala's 2018 monsoon. I combined geospatial data in QGIS and used machine-learning models to classify recorded landslides as debris flows, shallow slides, or rockfalls.

I received the **Consolation Prize at the IEDC Summit Data Science Contest** for this project.

> This is a historical landslide-type classification project. It does not predict whether or when a landslide will occur.

## What I set out to investigate

Kerala's 2018 monsoon landslide inventory contains 4,728 mapped events and three recorded landslide types. I wanted to investigate whether rainfall and land-use data could add useful information to existing GSI susceptibility data when distinguishing between these types.

The inventory contains confirmed landslide locations, but does not provide a reliable set of locations where landslides did not occur. For that reason, I focused on predicting landslide **type**, rather than building a landslide-versus-no-landslide occurrence model.

## My workflow

1. I used the 2018 Kerala landslide inventory and its event-type labels.
2. I integrated landslide locations with rainfall station data in QGIS, matching each event with the nearest station.
3. I joined GSI susceptibility, land-use, and terrain information to the landslide records.
4. I prepared an analysis dataset of 4,606 events with valid GSI data.
5. I compared Decision Tree, Random Forest, and Gradient Boosted Trees models using 5-fold cross-validation in Altair AI Studio / RapidMiner.
6. I reviewed class-level performance and feature importance, in addition to overall accuracy.

### Prediction target and features

My target field is `Type_of_sl`, with three classes:

- Debris Flow
- Shallow/Surficial Slide
- Rockfall

The nine predictor fields used in the workflow are:

- `GSI_Susceptibi` — GSI susceptibility category
- `LU_2010` — land-use category
- `RASTERVALU` — slope measure
- `Total_Rainfall`
- `Mean_Daily_Rainfall`
- `Max_1Day_Rainfall`
- `Max_3Day_Rainfall`
- `Max_7Day_Rainfall`
- `Coverage_Ratio` — rainfall data coverage measure

I excluded fields such as landslide dimensions, damage, remarks, coordinates, source-confirmation flags, and distance to station from the predictors to reduce the risk of information leakage.

## Results

In my 5-fold cross-validation comparison, Gradient Boosted Trees achieved the highest overall accuracy and the strongest reported performance for the minority Rockfall class.

| Model | Overall accuracy | Rockfall recall | Rockfall precision |
|---|---:|---:|---:|
| Decision Tree | 72.12% | 20.39% | 50.82% |
| Random Forest | 73.88% | 11.84% | 60.00% |
| Gradient Boosted Trees | 73.93% | 22.37% | 60.71% |

The feature-importance results placed slope (`RASTERVALU`) first, followed by `Coverage_Ratio` and rainfall measures. I interpret the coverage ratio carefully: it can reflect the completeness and reliability of rainfall observations, so it should not be treated as a direct physical cause of landslides. Individual land-use categories contributed less than slope and rainfall in this model.

## Limitations

I consider these results exploratory and limited by the available data:

- My model classifies landslide type at recorded events; it does not estimate landslide occurrence probability or provide real-time warnings.
- The inventory does not include reliable negative/background samples for occurrence modeling.
- The analysis shows predictive associations, not proof that land use or another feature causes a landslide.
- Rockfall is a small class (152 records, about 3%), and recall remains low across the models.
- The rainfall network has about 60 stations statewide. Some events are matched to a station up to approximately 78 km away.
- I excluded 122 events (2.58%) without valid GSI data from the modeling dataset.

## Future work

If I continue this project, I would like to:

- Improve Rockfall classification with additional data or class-imbalance methods.
- Develop and validate spatially appropriate background samples for occurrence modeling.
- Examine changes in land use between 2010 and 2018.
- Improve rainfall coverage or evaluate spatial interpolation methods.
- Map model results and feature importance in QGIS for spatial exploration.

## Repository contents

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

### Project visuals

**QGIS map**

![Kerala landslide map](./screenshots/qgis_map.png)

**Machine-learning workflow**

![Machine-learning workflow](./screenshots/model_workflow.png)

**Feature importance**

![Feature importance](./screenshots/feature_importance.png)

## Tools and data

I used QGIS for spatial data integration and Altair AI Studio / RapidMiner for the machine-learning workflow. The repository includes the processed CSV files, RapidMiner process, presentation, and screenshots. The original landslide inventory, rainfall observations, and district susceptibility source layers are not included in this repository.

The project presentation is available at [04_PRESENTATION/Kerala_Landslide_Risk_Estimation.pptx](./04_PRESENTATION/Kerala_Landslide_Risk_Estimation.pptx).

## About me

- **Author:** Fathima M J
- **Program:** B.Tech Artificial Intelligence & Data Science
- **Institution:** Ilahia College of Engineering and Technology
- **Achievement:** Consolation Prize, IEDC Summit Data Science Contest

## License

No license is currently included in this repository.
