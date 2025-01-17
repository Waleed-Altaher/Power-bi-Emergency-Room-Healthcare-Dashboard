<div align="center">

# Emergency-Room-Healthcare-Dashboard


![Logo](https://github.com/Waleed-Altaher/Power-bi-Emergency-Room-Healthcare-Dashboard/blob/main/healthcare-analytics.jpg)
</div>

## Table of Contents
- [Introduction](#introduction)
- [Data Source](#data-source)
- [Dashboard](#dashboard)
- [Modeling](#modeling)
- [DAX Measures](#dax-measures)

## Introduction 
Emergency Room Dashboard provides a comprehensive overview of key performance indicators (KPIs) and patient flow within the emergency department. This interactive tool is designed to help healthcare professionals monitor and improve the efficiency and effectiveness of emergency care services.

Key features of this dashboard include:

Patient Visits , Average Wait Time,  Average Satisfaction, Gender Breakdown, Patient Flow per Month, Patients by Department Referral and
Day and Time Analysis and others 
By leveraging this dashboard, healthcare organizations can gain valuable insights into emergency department operations, identify areas for improvement, and make data-driven decisions to enhance patient care.

## Dashboard
![dashboard](https://github.com/Waleed-Altaher/Power-bi-Emergency-Room-Healthcare-Dashboard/blob/main/Dashboard.gif) 
--- 
## Data Source 
The dataset is from and hospital emergency room ,, check it out [here](https://github.com/Waleed-Altaher/Power-bi-Emergency-Room-Healthcare-Dashboard/blob/main/Hospital%20ER.csv)

## Modeling 
simple modeling for simple dataset 
![modelin](https://github.com/Waleed-Altaher/Power-bi-Emergency-Room-Healthcare-Dashboard/blob/main/modeling.png)
## DAX Measures 

### 1. **Average SAT Score**  
**Expression**:  
```DAX
AVERAGE('Hospital ER'[SAT Score])
```

---

### 2. **Average Wait Time**  
**Expression**:  
```DAX
AVERAGE('Hospital ER'[Patient Waittime])
```

---

### 3. **Avg Patient Age**  
**Expression**:  
```DAX
ROUNDDOWN(
    CALCULATE(
        AVERAGE('Hospital ER'[Age]),
        ALL('Hospital ER'[Patient ID])
    ), 0
)
```

---

### 4. **Female Patient**  
**Expression**:  
```DAX
CALCULATE(
    DISTINCTCOUNT('Hospital ER'[Patient ID]),
    'Hospital ER'[Gender] = "F"
)
```

---

### 5. **Male Patient**  
**Expression**:  
```DAX
CALCULATE(
    DISTINCTCOUNT('Hospital ER'[Patient ID]),
    'Hospital ER'[Gender] = "M"
)
```

---

### 6. **Wait Time Color**  
**Expression**:  
```DAX
VAR _maxPatient = MAXX(
    ALL('Calendar'[Month], 'Calendar'[Month Num], 'Calendar'[Month-Year], 'Calendar'[MonthYearNum]),
    [Average Wait Time]
)
VAR _minPatient = MINX(
    ALL('Calendar'[Month], 'Calendar'[Month Num], 'Calendar'[Month-Year], 'Calendar'[MonthYearNum]),
    [Average Wait Time]
)
RETURN SWITCH(
    TRUE(),
    [Average Wait Time] = _maxPatient, "#1B539A",
    [Average Wait Time] = _minPatient, "#7698C2",
    "#E7EBEF"
)
```

---

### 7. **Patient Color (Monthly)**  
**Expression**:  
```DAX
VAR _maxPatient = MAXX(
    ALL('Calendar'[Month], 'Calendar'[Month Num], 'Calendar'[Month-Year], 'Calendar'[MonthYearNum]),
    [Total Patients]
)
VAR _minPatient = MINX(
    ALL('Calendar'[Month], 'Calendar'[Month Num], 'Calendar'[Month-Year], 'Calendar'[MonthYearNum]),
    [Total Patients]
)
RETURN SWITCH(
    TRUE(),
    [Total Patients] = _maxPatient, "#264D26",
    [Total Patients] = _minPatient, "#5B855C",
    "#84A385"
)
```

---

### 8. **Total Patients**  
**Expression**:  
```DAX
DISTINCTCOUNT('Hospital ER'[Patient ID])
```

---

### 9. **Patient YTD**  
**Expression**:  
```DAX
CALCULATE(
    [Total Patients],
    DATESYTD('Calendar'[Date])
)
```

---

### 10. **Patient PYTD**  
**Expression**:  
```DAX
CALCULATE(
    [Patient YTD],
    DATEADD('Calendar'[Date], -1, YEAR)
)
```

---

### 11. **Patient Growth %**  
**Expression**:  
```DAX
DIVIDE(([Patient YTD] - [Patient PYTD]), [Patient PYTD], 0)
```

---

### 12. **AWT YTD**  
**Expression**:  
```DAX
CALCULATE(
    [Average Wait Time],
    DATESYTD('Calendar'[Date])
)
```

---

### 13. **AWT PYTD**  
**Expression**:  
```DAX
CALCULATE(
    [AWT YTD],
    DATEADD('Calendar'[Date], -1, YEAR)
)
```

---

### 14. **AWT Growth %**  
**Expression**:  
```DAX
DIVIDE(([AWT YTD] - [AWT PYTD]), [AWT PYTD], 0)
```

---

### 15. **Avg Age YTD**  
**Expression**:  
```DAX
CALCULATE(
    [Avg Patient Age],
    DATESYTD('Calendar'[Date])
)
```

---

### 16. **Avg Age PYTD**  
**Expression**:  
```DAX
CALCULATE(
    [Avg Age YTD],
    DATEADD('Calendar'[Date], -1, YEAR)
)
```

---

### 17. **Age Growth %**  
**Expression**:  
```DAX
DIVIDE(([Avg Age YTD] - [Avg Age PYTD]), [Avg Age PYTD], 0)
```

---

### 18. **SAT YTD**  
**Expression**:  
```DAX
CALCULATE(
    [Average SAT Score],
    DATESYTD('Calendar'[Date])
)
```

---

### 19. **SAT PYTD**  
**Expression**:  
```DAX
CALCULATE(
    [SAT YTD],
    DATEADD('Calendar'[Date], -1, YEAR)
)
```

---

### 20. **SAT Growth %**  
**Expression**:  
```DAX
DIVIDE(([SAT YTD] - [SAT PYTD]), [SAT PYTD], 0)
```

---

### 21. **Total Patients (Bar)**  
**Expression**:  
```DAX
[Total Patients]
```

---

### 22. **Title 1**  
**Expression**:  
```DAX
IF(
    MAX('Race-Department'[Race-Department]) = "Dept", 
    "Patients by Department Referral", 
    "Patients by Race"
)
```

---

### 23. **Patient Font Color (Monthly)**  
**Expression**:  
```DAX
VAR _maxPatient = MAXX(
    ALL('Calendar'[Month], 'Calendar'[Month Num], 'Calendar'[Month-Year], 'Calendar'[MonthYearNum]),
    [Total Patients]
)
VAR _minPatient = MINX(
    ALL('Calendar'[Month], 'Calendar'[Month Num], 'Calendar'[Month-Year], 'Calendar'[MonthYearNum]),
    [Total Patients]
)
RETURN SWITCH(
    TRUE(),
    [Total Patients] = _maxPatient, "White",
    [Total Patients] = _minPatient, "White",
    "#5B855C"
)
```

---

### 24. **SAT Score Color**  
**Expression**:  
```DAX
VAR _maxPatient = MAXX(
    ALL('Calendar'[Month], 'Calendar'[Month Num], 'Calendar'[Month-Year], 'Calendar'[MonthYearNum]),
    [Average SAT Score]
)
VAR _minPatient = MINX(
    ALL('Calendar'[Month], 'Calendar'[Month Num], 'Calendar'[Month-Year], 'Calendar'[MonthYearNum]),
    [Average SAT Score]
)
RETURN SWITCH(
    TRUE(),
    [Average SAT Score] = _maxPatient, "#264D26",
    [Average SAT Score] = _minPatient, "#5B855C",
    "#84A385"
)
```

---

### 25. **Age Bucket Color**  
**Expression**:  
```DAX
VAR _maxPatient = MAXX(
    ALL('Age Bucket'),
    [Total Patients]
)
VAR _minPatient = MINX(
    ALL('Age Bucket'),
    [Total Patients]
)
RETURN SWITCH(
    TRUE(),
    [Total Patients] = _maxPatient, "#264D26",
    [Total Patients] = _minPatient, "#5B855C",
    "#84A385"
)
```

---

This cleaned list provides a structured view of the names and their associated DAX expressions. Let me know if further formatting is needed!
