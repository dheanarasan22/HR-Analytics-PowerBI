# HR Analytics Dashboard (Power BI)

An interactive Power BI dashboard that analyses workforce size, attrition, pay fairness, hiring effectiveness and employee engagement for a 311-employee company.

## Business questions answered

| Page | Question |
|---|---|
| 1. Overview | How big is the workforce and how is it changing? |
| 2. Attrition | Why and where are people leaving? |
| 3. Pay and Performance | Is pay fair, and does it follow performance? |
| 4. Hiring and Engagement | Which hiring sources work, and how engaged are people? |
| 5. Workforce Profile | Who works here? |

Every page shares the same slicer panel (Department, Year, Employment Status), colour theme and navigation buttons, and ends with a dynamic one-line insight written in DAX.

## Dashboard preview

### 1. Overview
([01_Overview.png](https://github.com/dheanarasan22/HR-Analytics-PowerBI/blob/main/01_Overview.png])

### 2. Attrition
([02_Attrition.png](https://github.com/dheanarasan22/HR-Analytics-PowerBI/blob/main/02_Attrition.png))

### 3. Pay and Performance
([03_Pay_Performance.png](https://github.com/dheanarasan22/HR-Analytics-PowerBI/blob/main/03_Pay_Performance.png))

### 4. Hiring and Engagement
([04_Hiring_Engagement.png](https://github.com/dheanarasan22/HR-Analytics-PowerBI/blob/main/04_Hiring_Engagement.png))

### 5. Workforce Profile
([05_Workforce_Profile.png](https://github.com/dheanarasan22/HR-Analytics-PowerBI/blob/main/05_Workforce_Profile.png))

## Key findings

- 311 employees in total: 207 active and 104 terminated, an overall attrition rate of **33.4%**.
- **85%** of exits were voluntary; the most common single reason was "Another position".
- **Production** has the highest attrition at about 40%.
- Male employees earn about **4.0%** more on average than female employees.
- **Indeed** is the largest hiring source (87 hires); **Website** hires have the best retention (92%).
- Average salary is about $69,021 and average engagement score is 4.11.

## Dataset

`Dataset/HR_Analytics_Dataset.csv` is the HR Dataset v14 (Human Resources Data Set, Dr. Rich Huebner and Dr. Carla Patalano): 311 employee records with 36 columns covering demographics, department, position, salary, hire and termination dates, termination reason, recruitment source, performance score, engagement and satisfaction.

## Data model

- Fact table: `HRDataset_v14`
- Date table: `DimDate`, related to `HireDate` (active) and `TermDate` (inactive, used with `USERELATIONSHIP` for terminations)
- Helper columns created in DAX because several fields were imported as text: `HireDate`, `TermDate`, `Salary_Num`, `Engagement_Num`, `Satisfaction_Num`, `Term_Flag`, `TenureYears`, `TenureBand`, `Age`, `AgeGroup`, `HireYear`, `TermYear`
- All measures live in a dedicated `measure` table, grouped into display folders by report page

## Key DAX measures

```DAX
Terminated Count =
CALCULATE ( COUNTROWS ( HRDataset_v14 ), HRDataset_v14[Term_Flag] = 1 )

Attrition % =
DIVIDE ( [Terminated Count], [total_employee] )

Terminations =
CALCULATE (
    COUNTROWS ( HRDataset_v14 ),
    HRDataset_v14[Term_Flag] = 1,
    USERELATIONSHIP ( HRDataset_v14[TermDate], DimDate[Date] )
)

Retention Rate % =
DIVIDE ( [total_employee] - [Terminated Count], [total_employee] )

Gender Pay Gap % =
DIVIDE ( [Avg Salary Male] - [Avg Salary Female], [Avg Salary Male] )

Avg Engagement - Left =
CALCULATE ( [Avg Engagement], HRDataset_v14[Term_Flag] = 1 )
```

Other measures: `Hires`, `Net Headcount Change`, `Voluntary Exits`, `For-Cause Exits`, `Voluntary Share of Exits %`, `Avg Salary`, `Median Salary`, `Avg Tenure (Years)`, `Avg Engagement`, `Avg Satisfaction`, `Female %`, `Non-White %`, `Avg Age`, plus five `Insight - ...` text measures.

## How to use

1. Download or clone this repository.
2. Open `HR_Analytics_Dashboard.pbix` in Power BI Desktop (free).
3. If Power BI shows a data source error, go to **Transform data > Data source settings > Change Source** and point it at `Dataset/HR_Analytics_Dataset.csv`.

## Repository structure

```
HR-Analytics-PowerBI/
├── HR_Analytics_Dashboard.pbix
├── README.md
├── Screenshots/
│   ├── 01_Overview.png
│   ├── 02_Attrition.png
│   ├── 03_Pay_Performance.png
│   ├── 04_Hiring_Engagement.png
│   └── 05_Workforce_Profile.png
└── Dataset/
    └── HR_Analytics_Dataset.csv
```

## Tools

Power BI Desktop, DAX, Power Query

## Author

Your Name | [LinkedIn](https://www.linkedin.com/in/your-profile) | [GitHub](https://github.com/your-username)
