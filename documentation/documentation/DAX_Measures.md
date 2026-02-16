# DAX Measures - Annotation Project

This file contains all DAX measures used in the Project Himalaya dashboard.

---

## BASE MEASURES

```dax
Total Annotations = COUNTROWS(Annotation_Records)
```

```dax
Correct Annotations = 
CALCULATE(
    COUNTROWS(Annotation_Records),
    Annotation_Records[Status] = "Correct"
)
```

```dax
Wrong Annotations = 
CALCULATE(
    COUNTROWS(Annotation_Records),
    Annotation_Records[Status] = "Wrong"
)
```

```dax
Total Working Days = COUNTROWS(Attendance_Records)
```

```dax
Present Days = 
CALCULATE(
    COUNTROWS(Attendance_Records),
    Attendance_Records[Attendance_Status] = "Present"
)
```

```dax
Total Annotators = DISTINCTCOUNT(Annotator_Master[Annotator_ID])
```

---

## KPI MEASURES

```dax
Accuracy % = 
DIVIDE(
    [Correct Annotations],
    [Total Annotations],
    0
) * 100
```

```dax
Error Rate % = 
DIVIDE(
    [Wrong Annotations],
    [Total Annotations],
    0
) * 100
```

```dax
Attendance % = 
DIVIDE(
    [Present Days],
    [Total Working Days],
    0
) * 100
```

```dax
Daily Throughput = 
DIVIDE(
    [Total Annotations],
    [Present Days],
    0
)
```

```dax
Hourly Throughput = 
DIVIDE(
    [Daily Throughput],
    7,
    0
)
```

```dax
Weekly Throughput = [Daily Throughput] * 5
```

---

## TARGET & VARIANCE MEASURES

```dax
Target Accuracy = 90
```

```dax
Target Throughput (Hourly) = 75
```

```dax
Target Attendance = 90
```

```dax
Accuracy vs Target = [Accuracy %] - [Target Accuracy]
```

```dax
Throughput vs Target = [Hourly Throughput] - [Target Throughput (Hourly)]
```

```dax
Attendance vs Target = [Attendance %] - [Target Attendance]
```

---

## TIME INTELLIGENCE MEASURES

```dax
MTD Annotations = 
CALCULATE(
    [Total Annotations],
    DATESMTD('Date'[Date])
)
```

```dax
QTD Annotations = 
CALCULATE(
    [Total Annotations],
    DATESQTD('Date'[Date])
)
```

```dax
YTD Annotations = 
CALCULATE(
    [Total Annotations],
    DATESYTD('Date'[Date])
)
```

```dax
Previous Month Accuracy = 
CALCULATE(
    [Accuracy %],
    DATEADD('Date'[Date], -1, MONTH)
)
```

```dax
Accuracy MoM Change = [Accuracy %] - [Previous Month Accuracy]
```

---

## RANKING MEASURES

```dax
Annotator Rank by Accuracy = 
RANKX(
    ALL(Annotator_Master[Annotator_ID]),
    [Accuracy %],
    ,
    DESC,
    DENSE
)
```

```dax
Annotator Rank by Throughput = 
RANKX(
    ALL(Annotator_Master[Annotator_ID]),
    [Hourly Throughput],
    ,
    DESC,
    DENSE
)
```

```dax
Average Team Accuracy = 
AVERAGEX(
    VALUES(Annotator_Master[Annotator_ID]),
    [Accuracy %]
)
```

```dax
Average Team Throughput = 
AVERAGEX(
    VALUES(Annotator_Master[Annotator_ID]),
    [Hourly Throughput]
)
```

---

## ERROR ANALYSIS MEASURES

```dax
Labeling Errors = 
CALCULATE(
    COUNTROWS(Annotation_Records),
    Annotation_Records[Error_Category] = "Labeling Error"
)
```

```dax
Missing Annotations = 
CALCULATE(
    COUNTROWS(Annotation_Records),
    Annotation_Records[Error_Category] = "Missing Annotation"
)
```

```dax
Incorrect Classifications = 
CALCULATE(
    COUNTROWS(Annotation_Records),
    Annotation_Records[Error_Category] = "Incorrect Classification"
)
```

```dax
Formatting Issues = 
CALCULATE(
    COUNTROWS(Annotation_Records),
    Annotation_Records[Error_Category] = "Formatting Issue"
)
```

---

## DATE DIMENSION TABLE

```dax
Date = 
ADDCOLUMNS(
    CALENDAR(DATE(2025,1,1), DATE(2025,12,31)),
    "Year", YEAR([Date]),
    "Month", FORMAT([Date], "MMM"),
    "Month Number", MONTH([Date]),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Week Number", WEEKNUM([Date]),
    "Day Name", FORMAT([Date], "DDD"),
    "Day Number", DAY([Date]),
    "Is Weekend", IF(WEEKDAY([Date]) IN {1,7}, "Weekend", "Weekday")
)
```

---

## CONDITIONAL FORMATTING MEASURES

```dax
Accuracy Status = 
IF(
    [Accuracy %] >= [Target Accuracy],
    "✅ Met Target",
    "⚠️ Below Target"
)
```

```dax
Throughput Status = 
IF(
    [Hourly Throughput] >= [Target Throughput (Hourly)],
    "✅ Met Target",
    "⚠️ Below Target"
)
```

```dax
Attendance Status = 
IF(
    [Attendance %] >= [Target Attendance],
    "✅ Met Target",
    "⚠️ Below Target"
)
```

---

*These measures are optimized for performance with large datasets (4.4M+ rows)*
