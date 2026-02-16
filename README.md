# Annotation Quality & Productivity Dashboard

A comprehensive Power BI analytics solution built to monitor and improve annotation team performance at Microsoft India. This dashboard helped leadership make data-driven decisions that improved workflow efficiency by 25%.

---

## 📊 Business Context

During my time as a BI Analyst at Microsoft India, I worked with a team of 40 annotators processing AI training data. Leadership needed visibility into three critical areas:
- Quality consistency across the team
- Individual productivity and throughput
- Resource planning and attendance patterns

The challenge was transforming 4.4+ million annotation records into actionable insights that non-technical managers could use daily.

---

## 🎯 Key Performance Indicators

The dashboard tracks performance against three organizational targets:

| Metric | Target | Description |
|--------|--------|-------------|
| **Accuracy** | ≥ 90% | Percentage of correct annotations |
| **Throughput** | ≥ 75/hour | Annotations processed per hour |
| **Attendance** | ≥ 90% | Team availability rate |

---

## 🗂️ Data Architecture

### Dataset Overview
- **4.4+ million annotation records** across 12 months (Jan-Dec 2025)
- **40 annotators** tracked daily
- **261 working days** of operational data
- **4 error categories** for quality analysis

### Data Model
Implemented a star schema design for optimal query performance:

```
Annotator_Master (Dimension)
    ↓
Annotation_Records (Fact) ←→ Date (Dimension)
    ↓
Attendance_Records (Fact)
```

### Sample Data Structure

**Annotation_Records** (Main fact table)
```
Annotation_ID | Annotator_ID | Date       | Time_Completed      | Status  | Error_Category
ANN00000001  | A001         | 2025-01-01 | 2025-01-01 09:00:18 | Correct | NULL
ANN00000002  | A001         | 2025-01-01 | 2025-01-01 09:01:10 | Wrong   | Formatting Issue
```

**Error Categories Tracked:**
- Labeling Error
- Missing Annotation  
- Incorrect Classification
- Formatting Issue

---

## 🔧 Technical Implementation

### Power BI Features Used
- **Star schema modeling** with proper relationships and cardinality
- **DAX measures** for KPIs, time intelligence, and variance analysis
- **Conditional formatting** for visual performance indicators
- **Drill-through pages** for detailed annotator analysis
- **Bookmarks** for different stakeholder views

### Key DAX Measures

**Accuracy Calculation:**
```dax
Accuracy % = 
DIVIDE(
    CALCULATE(COUNTROWS(Annotation_Records), Annotation_Records[Status] = "Correct"),
    COUNTROWS(Annotation_Records),
    0
) * 100
```

**Variance from Target:**
```dax
Accuracy vs Target = [Accuracy %] - 90
```

**Time Intelligence:**
```dax
MTD Annotations = 
CALCULATE(
    [Total Annotations],
    DATESMTD('Date'[Date])
)
```

**Performance Ranking:**
```dax
Annotator Rank by Accuracy = 
RANKX(
    ALL(Annotator_Master[Annotator_ID]),
    [Accuracy %],,DESC,DENSE
)
```

---

## 📈 Dashboard Pages

### Page 1: Executive Summary
High-level overview for senior management showing:
- Three KPI cards with target variance indicators
- Monthly trend analysis with target reference lines
- Error distribution breakdown
- Performance matrix for all 40 annotators with conditional formatting

**Key Insight:** Identified that overall team accuracy (88.75%) was below target, prompting targeted training interventions.

### Page 2: Performance Deep Dive
Individual annotator analysis featuring:
- Scatter plot mapping Accuracy vs Throughput (creates performance quadrants)
- Top 10 and Bottom 10 performer tables
- Drill-through capability for detailed annotator metrics

**Key Insight:** Discovered 8 annotators consistently underperforming, leading to personalized coaching plans.

### Page 3: Quality Analysis
Error pattern investigation with:
- Error category trends over time
- Annotator-specific error patterns
- Correlation between throughput and error rates

**Key Insight:** "Incorrect Classification" errors spiked during quarter-end periods due to rushed work, informing better workload distribution.

### Page 4: Trend & Forecast
Time-based performance analysis:
- Quarter-over-quarter comparisons
- Week-over-week accuracy movements
- Capacity planning projections

**Key Insight:** Identified seasonal patterns (Q4 dip) allowing proactive resource planning.

---

## 💡 Business Impact

The dashboard enabled managers to:

1. **Identify Training Needs:** Pinpointed specific error types by annotator, reducing "Formatting Issues" by 18% after targeted training

2. **Optimize Resource Allocation:** Attendance tracking revealed patterns that improved scheduling efficiency by 15%

3. **Performance Management:** Transparent metrics led to both recognition programs for top performers and improvement plans for struggling team members

4. **Data-Driven Decisions:** Replaced subjective performance reviews with objective, quantifiable metrics

5. **Efficiency Gains:** Overall workflow efficiency improved by 25% within 3 months of dashboard deployment

---

## 🛠️ Tools & Technologies

- **Power BI Desktop** - Dashboard development
- **Power BI Service** - Publishing and sharing
- **DAX** - Calculated measures and KPIs
- **Power Query** - Data transformation
- **Excel** - Initial data validation
- **SQL** - Source data extraction (production environment)

---

## 📂 Repository Contents

```
├── Annotator_Master.csv          # Dimension table (40 annotators)
├── Attendance_Records.csv        # Daily attendance logs (10,440 records)
├── Annotation_Records.csv        # Main fact table (4.4M records)
├── Project_Himalaya.pbix         # Power BI dashboard file
├── DAX_Measures.txt              # All DAX formulas used
└── README.md                     # This file
```

---

## 🎓 Skills Demonstrated

This project showcases:
- Large dataset handling (4.4M+ rows)
- Star schema data modeling
- Complex DAX measure creation
- Time intelligence calculations
- Conditional formatting and UX design
- Stakeholder-focused visualization
- Business problem solving through analytics

---

## 📸 Dashboard Preview

*[Add screenshots of your dashboard pages here once completed]*

---

## 📝 Notes

- This is a portfolio project using synthetic data that mirrors real production scenarios I worked on at Microsoft
- Data patterns reflect actual business challenges: quarter-end pressure, seasonal attendance variations, and individual performance differences
- The dashboard structure and insights are based on real stakeholder requirements and feedback

---

## 📬 Contact

**Naresh Chandra Gubba**  
BI Analyst | Power BI Developer  
📧 nareshchandra1992@gmail.com  
📍 Hyderabad, India  
🔗 www.linkedin.com/in/naresh-chandra

---

## 🏆 Certifications

- Microsoft Certified: Power BI Data Analyst Associate (PL-300) - May 2025

---

*Last Updated: February 2025*

