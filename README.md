# Kerala Landslide Risk Estimation

I built this project to explore how rainfall, terrain susceptibility, and land use relate to landslide types recorded during Kerala's 2018 monsoon. I used QGIS to combine the spatial data and compared machine-learning models to classify recorded events as debris flows, shallow slides, or rockfalls.

**Achievement:** Consolation Prize, IEDC Summit Data Science Contest

> This project classifies landslide types; it does not predict whether or when a landslide will occur.

## Workflow and results

I integrated the landslide inventory with rainfall station, GSI susceptibility, land-use, and terrain data. The final dataset contains 4,606 events. I compared Decision Tree, Random Forest, and Gradient Boosted Trees using 5-fold cross-validation.

| Model | Accuracy |
|---|---:|
| Decision Tree | 72.12% |
| Random Forest | 73.88% |
| Gradient Boosted Trees | 73.93% |

Gradient Boosted Trees had the highest overall accuracy. Slope was the most important feature. Rockfall was underrepresented, so its classification remained challenging.

## Limitations

- The inventory has no reliable non-landslide samples, so I classify landslide type rather than occurrence risk.
- The results show associations, not causation, and are not a real-time warning system.
- Rainfall estimates may be uncertain because some events are far from the nearest station.

## Repository contents

```text
.
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

## Screenshots

![QGIS landslide map](./screenshots/qgis_map.png)

![Machine-learning workflow](./screenshots/model_workflow.png)

![Feature importance](./screenshots/feature_importance.png)

## About me

**Fathima M J** · B.Tech Artificial Intelligence & Data Science · Ilahia College of Engineering and Technology

I used QGIS and Altair AI Studio / RapidMiner. The repository contains processed datasets and project materials; original source datasets are not included.
