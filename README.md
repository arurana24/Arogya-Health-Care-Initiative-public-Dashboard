# Ārogya: Public Health Outreach Analytics & Strategic Resource Dashboard

## 📌 Project Overview
**Ārogya ("Swasth Tan, Sukhi Jeevan")** is an executive-level healthcare intelligence platform that transforms raw clinical data collected from community health camps into actionable operational insights. Processing **1,280+ clinical records**, the platform utilizes multi-dimensional demographic modeling, advanced cohort analysis, and spatial mapping to help non-governmental organization (NGO) leadership identify high-risk populations, address public health disparities, and allocate mobile medical assets with maximum efficiency.

The dashboard's design philosophy is heavily inspired by premier healthcare and management consulting reporting standards (McKinsey, Deloitte, and the World Health Organization), prioritizing clean typography, strict grid alignment, an intuitive left-to-right story flow, and a low data-to-ink ratio.

---

## 📊 Core Analytical Features & Dashboard Architecture

The intelligence platform is architected around critical clinical pillars, laid out across an executive tracking grid:

### 1. Unified Operational Scale (Left Sidebar)
*   Provides continuous visibility into programmatic scale using highly scannable, non-truncated metric cards tracking **Total Beneficiaries (1.3K)**, **High-Risk Caseload (447)**, **Hypertension Prevalence (29%)**, and **System-Wide Resting Heart Rate Averages**.
*   Houses an integrated, synced global navigation panel allowing stakeholders to instantaneously slice the entire environment by localized camp sectors.

### 2. Clinical Risk Profile Distribution (Top Grid)
*   **Overall Beneficiary Risk Distribution:** A normalized population donut chart revealing that **34.92%** of the screened population falls under an acute **"At Risk"** status.
*   **Hidden Risk (Patients with Normal BMI):** An isolated diagnostic card capturing a critical public health blind spot—uncovering that **12.04%** of individuals maintaining a completely healthy BMI ($\leq 25$) are already exhibiting high-risk clinical phenotypes.

### 3. Spatial & Geospatial Intelligence (Center-Left)
*   Integrates an interactive dark-canvas map plotting community camp distributions across regional administrative boundaries (Chandigarh, Panchkula, Sahibzada Ajit Singh Nagar). 
*   Bridges geographical coordinates directly with a localized tracking metric to pinpoint exact resource deployment hotspots.

### 4. Dynamic Gender Disparity Engine (Center Canvas Feature)
*   **"The Matriarch’s Burden" KPI:** Captures a severe, non-obvious gender risk gap. Within highly stressed and adult working cohorts, the severe metabolic and cardiovascular risk footprint among women is heavily elevated compared to corresponding male control baselines.

### 5. Chronic & Behavioral Strain Mapping (Right Column)
*   **The Burnout Tax:** A chronological column chart tracking the steady, upward trajectory of **Average Systolic Blood Pressure** as the workforce ages past 30.
*   **The Metabolic Cliff:** A continuous line visual capturing a volatile, sharp escalation in **Average Blood Sugar Levels** triggering immediately post-youth brackets.
*   **Generational Shift:** A 100% stacked bar chart visually plotting the contraction of "Healthy" baselines and the progressive stacking of "At Risk" states across aging cohorts.

---

## 🔍 Key Data Insights & Strategic Recommendations

*   **Targeted Women’s Wellness Programs:** To mitigate *The Matriarch’s Burden*, the data indicates a critical need to establish specialized **"Women's Wellness Sundays"** within high-density zones like Bapudham and Dhanas. This operational adjustment bypasses socio-cultural accessibility friction and optimizes preventive health investments.
*   **Expanding Screening Criteria:** Because **12.04% of normal-weight patients** sit in the severe risk tier due to stress-induced hypertension, the NGO must expand its screening metrics beyond basic BMI tracking. Field teams should deploy specialized mobile diagnostic checkups directly to high-strain employment zones like construction sites.

---

## 🛠️ Data Architecture & Custom DAX Formulations

To bypass commercial calculation limits and compute complex cohort deviations across rows, the reporting model utilizes specialized DAX measures:

### 1. High-Risk Multi-Condition Demarcation
```dax
High_Risk_Caseload = 
CALCULATE(
    COUNTROWS('HealthRecords'),
    FILTER(
        'HealthRecords',
        'HealthRecords'[Health Risk Status] = "At Risk" || 
        'HealthRecords'[Systolic BP] >= 140 || 
        'HealthRecords'[Diastolic BP] >= 90
    )
)


Hypertension_Prevalence_Rate = 
DIVIDE(
    CALCULATE(
        COUNTROWS('HealthRecords'),
        'HealthRecords'[Systolic BP] >= 140 || 'HealthRecords'[Diastolic BP] >= 90
    ),
    COUNTROWS('HealthRecords'),
    0
)


Disparity_Percentage_Flag = 
VAR FemaleHighRisk = CALCULATE(COUNTROWS('HealthRecords'), 'HealthRecords'[Gender] = "Female", 'HealthRecords'[Health Risk Status] = "At Risk", 'HealthRecords'[Stress Level] = "High")
VAR MaleHighRisk = CALCULATE(COUNTROWS('HealthRecords'), 'HealthRecords'[Gender] = "Male", 'HealthRecords'[Health Risk Status] = "At Risk", 'HealthRecords'[Stress Level] = "High")
RETURN
DIVIDE(FemaleHighRisk - MaleHighRisk, MaleHighRisk, 0)
