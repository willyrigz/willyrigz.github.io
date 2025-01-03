# Focused Analysis of HbA1c Measures and Readmissions for Patients Discharged to Home

## Background and Overview

As a healthcare data analyst, the goal of this project is to explore the impact of HbA1c measurements on hospital readmission rates. This analysis is based on the dataset "Diabetes 130-US Hospitals for Years 1999-2008," provided by Clore et al. (2014), which aggregates clinical records from 130 hospitals across the United States. Patients discharged to home are the focus of this analysis, as they represent the largest discharge group in the dataset, accounting for 60,234 encounters (approximately 59.2% of all cases). This makes them a significant population to analyze for identifying patterns and predicting readmissions. The objective is to:

1. Identify patterns in readmission rates linked to HbA1c testing for patients discharged to home.
2. Highlight key demographic predictors of readmissions in this group.
3. Provide actionable insights for improving outcomes in this discharge category.

## Data Structure Overview

The dataset, "Diabetes 130-US Hospitals for Years 1999-2008," was obtained from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008). This dataset represents ten years of clinical care data from 130 US hospitals, featuring 101,766 patient records and 47 attributes. 

### Key Features
- **Focus of Analysis**: Attributes such as age, gender, race, HbA1c testing results, and readmission status.
- **Structure**: Primarily categorical and integer variables suitable for classification and clustering tasks.
- **Selection Criteria**: Includes inpatient diabetic encounters lasting 1-14 days, where laboratory tests and medications were administered.

This dataset provides a robust foundation for analyzing diabetes management and its impact on hospital readmissions.

## Executive Summary

Our analysis of 60,234 patient encounters discharged to home highlights critical insights into HbA1c testing and readmission trends:

- Only 17.82% of patients underwent HbA1c testing, leaving an 82.18% gap that represents a missed opportunity to identify and manage poorly controlled diabetes.
- Patients with normal HbA1c levels had the lowest readmission rates (7.89%), while those who were untested had significantly higher rates (9.56%).
- Demographic analysis identified high-risk groups, including elderly patients aged 90-100, African American and Hispanic populations, and males, who exhibited slightly higher readmission rates than females.

Addressing these gaps by expanding HbA1c testing, targeting interventions for high-risk groups, and enhancing post-discharge support will improve outcomes and reduce readmission rates.


## Insights Deep Dive

### Specific Analysis Queries

#### 1. Frequency of HbA1c Measurement for Home Discharges

**What This Means**: Understanding how often HbA1c tests are conducted for home-discharged patients can highlight gaps in current testing protocols.

```sql
SELECT 
    COUNT(*) AS total_encounters, 
    SUM(CASE WHEN A1Cresult != 'None' THEN 1 ELSE 0 END) AS measured_encounters, 
    ROUND((SUM(CASE WHEN A1Cresult != 'None' THEN 1 ELSE 0 END) * 100.0 / COUNT(*)), 2) AS percent_measured
FROM health
WHERE discharge_disposition_id = 1;
```

**Insight**: Among the 60,234 home-discharged encounters, only 10,733 (17.82%) included HbA1c testing, highlighting a significant gap in testing:

- **Missed Potential**: The remaining 82.18% of patients did not receive HbA1c testing, leaving their diabetes potentially unmanaged or undiagnosed.
- **Actionable Steps**: Prioritize systematic HbA1c testing during discharge planning to better identify high-risk patients and improve care outcomes.

This insight reinforces the need for testing protocols to close this gap and improve overall patient outcomes.

#### 2. Readmission Rates by HbA1c Measurement for Home Discharges

**What This Means**: Correlating HbA1c testing with readmission rates helps determine its effectiveness as a predictive tool for this discharge group.

```sql
SELECT 
    A1Cresult, 
    COUNT(*) AS total, 
    SUM(CASE WHEN readmitted = '<30' THEN 1 ELSE 0 END) AS readmitted_within_30_days, 
    ROUND((SUM(CASE WHEN readmitted = '<30' THEN 1 ELSE 0 END) * 100.0 / COUNT(*)), 2) AS readmission_rate
FROM health
WHERE discharge_disposition_id = 1
GROUP BY A1Cresult;
```

**Insight**: The analysis reveals distinct readmission patterns:

- **No HbA1c Testing ("None")**: This group comprises 49,501 encounters and has the highest readmission rate at **9.56%**.
- **Elevated HbA1c Levels (">8")**: Patients with elevated HbA1c results have a slightly lower readmission rate of **8.14%**, indicating some level of care intervention.
- **Normal HbA1c Levels ("Norm")**: Patients with normal HbA1c results exhibit the lowest readmission rate at **7.89%**, showcasing the benefits of effective diabetes management.
- **Moderately Elevated HbA1c Levels (">7")**: This group has a readmission rate of **8.36%**, slightly higher than those with very elevated levels.

**What This Means for Stakeholders**:

- The data emphasizes the importance of **systematic HbA1c testing**. Patients who undergo testing and have normal or elevated results show better managed outcomes compared to those who were not tested.

- **Actionable Steps**: Focus on increasing HbA1c testing for patients discharged to home and providing targeted interventions for those with elevated levels to further reduce readmissions. This underscores the importance of systematic HbA1c monitoring to manage diabetes effectively and reduce hospital returns.


#### 3. Demographic Predictors of Readmissions for Home Discharges

**What This Means**: This query examines demographic factors like gender, race, and age for patients discharged to home to identify high-risk subgroups.

```sql
SELECT 
    gender, 
    race, 
    age, 
    COUNT(*) AS total_patients,
    SUM(CASE WHEN readmitted = '<30' THEN 1 ELSE 0 END) AS total_readmitted,
    ROUND(SUM(CASE WHEN readmitted = '<30' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS readmission_rate
from health
JOIN demographics ON health.patient_nbr = demographics.patient_nbr
WHERE discharge_disposition_id = 1
GROUP BY gender, race, age
ORDER BY readmission_rate DESC;
```

**Insight**: Key findings include:

- **Elderly Patients**: Patients aged 90-100 show the highest readmission rates, particularly males.
- **Racial Trends**: African American and Hispanic patients demonstrate elevated readmission rates across multiple age groups. For example, African American males aged 30-40 and 80-90 exhibit consistently higher rates.
- **Age as a Factor**: Younger patients, particularly those aged 10-20, show lower readmission rates, except for certain racial subgroups.

These results suggest a need for tailored interventions focusing on elderly and racially diverse populations, particularly African Americans and Hispanics, to reduce readmission risks. These trends suggest a need for targeted interventions, including tailored follow-up care and support.


#### 4. Gender-Based Readmission Analysis

**What This Means**: This analysis examines differences in readmission rates between genders for patients discharged to home. The goal is to identify patterns that can guide targeted interventions.

```sql
WITH home_discharged AS (
    -- Step 1: Filter for patients discharged to home
    SELECT 
        gender, 
        readmitted
    FROM health
    JOIN demographics ON health.patient_nbr = demographics.patient_nbr
    WHERE discharge_disposition_id = 1
),
readmission_summary AS (
    -- Step 2: Summarize readmission rates by gender
    SELECT 
        gender, 
        COUNT(*) AS total_patients,
        SUM(CASE WHEN readmitted = '<30' THEN 1 ELSE 0 END) AS total_readmissions,
        ROUND(SUM(CASE WHEN readmitted = '<30' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS readmission_rate
    FROM home_discharged
    GROUP BY gender
)
-- Final Query: Retrieve and order results
SELECT *
FROM readmission_summary
ORDER BY readmission_rate DESC;
```

**Insight**: The analysis revealed the following patterns:

- **Male Patients**: Representing 29,396 encounters, male patients had a readmission rate of 9.51%, which is slightly higher compared to females.
- **Female Patients**: Representing 30,836 encounters, female patients had a readmission rate of 9.10%.

These findings suggest that while the difference in readmission rates between genders is marginal, male patients may benefit from additional post-discharge support to address slightly elevated risks.

#### 5. Visual Representation of Key Insights

![image](/diabetesresearch/images/HbA1cViz.png)

View the interactive Tableau dashboard here:

[**Visualizing HbA1c Testing and Readmissions**](https://public.tableau.com/views/HealthCareDiabetes_17359138048600/VisualizingDiabetesManagementandReadmissions?:language=en-GB&publish=yes&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link)  

This dashboard highlights key insights, including testing gaps, demographic risks, and the relationship between HbA1c testing and readmission rates.

## Recommendation

Based on the findings:

1. **Increase HbA1c Testing**: Standardize HbA1c testing for patients discharged to home to better predict and prevent readmissions.
2. **Target High-Risk Subgroups**: Focus on elderly patients and certain racial groups with higher readmission rates to improve post-discharge outcomes.
3. **Follow-Up Interventions**: Develop follow-up programs tailored to home-discharged patients with a focus on medication management and outpatient care.

## Summary

This analysis emphasizes the importance of HbA1c testing for patients discharged to home, showcasing its potential to reduce readmissions and improve patient outcomes. By focusing on high-risk subgroups, healthcare institutions can enhance care quality and operational efficiency.

## References

Clore, John, et al. "Diabetes 130-US Hospitals for Years 1999-2008." *UCI Machine Learning Repository*, 2014, https://doi.org/10.24432/C5230J.