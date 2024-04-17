## Overview
The project aims to use daily milking data and alflex data to predict mastitis in cows. We proposed two different models to achieve this goal: 
* Binary Randon Forest Classification
* Somatic Cell Count (SCC) Randon Forest Regression

For each model, we also have two different sets of data to train the model one of them only including milking data the other including milking and alflex data. The purposet of this is trying to see how Alflex data impact the performance of the model.

![](/Result/overview.png)

## Dataset
The dataset is from [Agriculture Victoria Research](https://agriculture.vic.gov.au), [DairyFeedbase](https://dairyfeedbase.com.au)

## Files
* **preprocess_binary.ipynb**: 
Conduct pre-processing and generate the following files:
Sampling/binary/am.csv: Contain milking data only to train binary model
Sampling/binary/pm.csv: Contain milking data only to train binary model (We haven't use this dataset to train the model)
Sampling/binary_alflex/am.csv: Contain milking+alflex data to train binary model

* **preprocess_scc.ipynb**: 
Conduct pre-processing and generate the following files:
Sampling/binary/am.csv: Contain milking data only to train binary model
Sampling/binary/pm.csv: Contain milking data only to train binary model (We haven't use this dataset to train the model)
Sampling/binary_alflex/am.csv: Contain milking+alflex data to train scc regression model

* **model_binary.ipynb**: 
The file that used to train and test binary model

* **model_scc.ipynb**: 
The file that used to train and test SCC regression model 


## Pre-processing
The following explain the steps of pre-processing:
1. Filter to limit the date of data from 2022/01 to 2023/12
2. Preserve CowID only from Elinbank, get the CowID list from "Stocklist" dataset
3. Seperate AM and PM data from a single data row
4. Get mastitis label from "Ailment" dataset and SCC from herd test
5. Get DIM from "Lactation" dataset, and remove CowID that has any missing DIM or has any DIM larger than 365
6. Handling missing data
    1. Fill in Duration if Volumn and Flow are not missing
    2. Fill in Flow if Volumn and Duration are not missing
    3. If Volumn missing, fill in with mean value
    (Note: This should have an issue, should fill in with mean value of that specific cow)
    4. If both Duration and Flow missing, fill in Duration with mean value and fill in Flow with given Duration value
7. Ensure "MilkDate" is continuous in dataset, fill in rows with missing date
8. Delete the 14 days of data after mastitis occur [2]
9. For each rows, compute mean and std of Milking Volumn, Folw, Duration by previous five days [1]


## Result
#### Binary Model
* Milking Only

|  | Recall | Precision |
|----------|----------|----------|
| 0 | 1.00 | 0.77 |
| 1 | 0.01 | 0.61 |

![](/Result/binary_milkonly_feature.png)

* Alflex + Milking

|  | Recall | Precision |
|----------|----------|----------|
| 0 | 1.00 | 0.88 |
| 1 | 0.02 | 0.50 |

![](/Result/binary_milkalflex_feature.png)

#### SCC Regression Model
* Milking Only

| R square | LCCC |
|----------|----------|
| 0.48 | 0.23 |

![](/Result/scc_milkonly.png)
![](/Result/scc_milkonly_feature.png)

* Alflex + Milking

| R square | LCCC |
|----------|----------|
| 0.50 | 0.25 |

![](/Result/scc_milkalflex.png)
![](/Result/scc_milkalflex_feature.png)


## Reference
 [1]	W. Luo, Q. Dong, and Y. Feng, ‘Risk prediction model of clinical mastitis in lactating dairy cows based on machine learning algorithms’, Preventive Veterinary Medicine, vol. 221, p. 106059, Dec. 2023, doi: 10.1016/j.prevetmed.2023.106059.

 [2]	X. Fan, R. D. Watters, D. V. Nydam, P. D. Virkler, M. Wieland, and K. F. Reed, ‘Multivariable time series classification for clinical mastitis detection and prediction in automated milking systems’, Journal of Dairy Science, vol. 106, no. 5, pp. 3448–3464, May 2023, doi: 10.3168/jds.2022-22355.
