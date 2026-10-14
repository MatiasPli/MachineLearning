# Lab 1: Regression Models: Predicting Flight Ticket Prices
**DA380A Machine Learning** · Dataset 5: AirTravel · Mateusz Plizga

This project predicts the **ticket price (INR)** of Indian domestic flights (about 300,000 flights, February-March 2022)
from airline, route, travel class, departure/arrival time, duration, number of stops and booking lead time.

## Results (test set, 20 % of the data)
| Model | MAE (INR) | RMSE (INR) | R² |
|---|---|---|---|
| Linear Regression (Ridge), tuned | 3,277 | 5,096 | 0.950 |
| **Random Forest, tuned (recommended)** | **1,117** | **2,741** | **0.985** |

## Contents
| File | Description |
|---|---|
| [`Lab1_AirTravel_Regression.ipynb`](Lab1_AirTravel_Regression.ipynb) | Full workflow: exploration, cleaning, models, evaluation, tuning, feature importance |
| [`Lab1_AirTravel_Report.pdf`](Lab1_AirTravel_Report.pdf) | Report with the discussion and conclusions |
| [`Project_Explained.md`](Project_Explained.md) | Plain-language summary of the project |
| `figures/` | Charts produced by the notebook |
| `results/metrics.json` | All final metrics |

## How to run
1. Install the packages: `pip install -r requirements.txt`
2. Place the course dataset files `business.csv` and `economy.csv` in `data/dataset5-AirTravel/`
   (the datasets are not included in this repository).
3. Open the notebook and run all cells (about 5 minutes).
