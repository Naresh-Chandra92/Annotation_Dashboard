# Data Preparation & Cleaning Steps

## Overview
This document outlines the data quality checks and transformation steps performed on the Project Himalaya dataset before analysis.

---

## 1. Data Import & Type Validation

### Power Query Transformations

**Annotator_Master.csv**
- Verified all 40 Annotator IDs are unique (no duplicates)
- Set Annotator_ID as Text type
- No null values present

**Attendance_Records.csv**
- Changed Date column from Text to Date type
- Verified Attendance_Status contains only "Present" or "Leave" values
- Confirmed all Annotator_IDs exist in master table (referential integrity)
- Total records: 10,440 (40 annotators × 261 working days)

**Annotation_Records.csv**
- Changed Date from Text to Date type
- Changed Time_Completed from Text to DateTime type
- Set Annotation_ID as Text (unique identifier)
- Set Annotator_ID as Text
- Confirmed Status contains only "Correct" or "Wrong"
- Validated Error_Category is NULL when Status = "Correct"
- Total records: 4,438,740

---

## 2. Data Quality Checks

### Check 1: No Orphan Records
```
Verified that every Annotator_ID in fact tables 
exists in the Annotator_Master dimension table.
Result: ✓ All IDs valid
```

### Check 2: Date Range Consistency
```
All three tables cover January 1 - December 31, 2025
Only weekdays (Monday-Friday) included in working days
Result: ✓ Consistent date range
```

### Check 3: Business Rule Validation
```
Rule: Annotations should only exist on days when annotator was Present
Validation Query:
  - Joined Annotation_Records with Attendance_Records
  - Filtered for Leave days
  - Result: 0 records (no annotations on leave days)
Result: ✓ Business rule satisfied
```

### Check 4: Error Category Logic
```
Rule: Error_Category should be NULL when Status = "Correct"
Validation:
  - Filtered Correct annotations
  - Checked Error_Category column
  - Result: All NULL as expected
Result: ✓ Logic correct
```

### Check 5: Time Boundaries
```
Rule: All timestamps should be within working hours (9 AM - 5 PM)
and exclude lunch break (1 PM - 2 PM)
Validation:
  - Extracted hour from Time_Completed
  - Checked range
  - Result: All within 9-17 range, no 13:00-14:00 entries
Result: ✓ Timestamps valid
```

---

## 3. Handling Nulls

### Analysis Results:

| Table | Column | Null Count | Action Taken |
|-------|--------|------------|--------------|
| Annotation_Records | Error_Category | ~3.9M | Expected (NULL when Correct) |
| All tables | All other columns | 0 | No action needed |

**Decision:** Error_Category nulls are intentional and represent correct annotations. No imputation required.

---

## 4. Data Model Setup

### Relationships Created:
1. Annotator_Master[Annotator_ID] → Annotation_Records[Annotator_ID] (One-to-Many)
2. Annotator_Master[Annotator_ID] → Attendance_Records[Annotator_ID] (One-to-Many)
3. Date[Date] → Annotation_Records[Date] (One-to-Many)
4. Date[Date] → Attendance_Records[Date] (One-to-Many)

### Cardinality: All relationships set to Single direction filtering
### Cross-filter direction: Single (from dimension to fact)

---

## 5. Performance Optimization

### Steps Taken:
1. **Removed unnecessary columns** - Kept only required fields for analysis
2. **Set proper data types** - Reduced memory footprint
3. **Disabled auto date/time** - Created custom Date dimension instead
4. **Used DAX variables** - Improved measure calculation speed
5. **Avoided calculated columns** - Used measures instead for better performance

### Result:
- Dashboard refresh time: ~8 seconds for 4.4M rows
- Average query response: <2 seconds
- File size: ~45 MB compressed

---

## 6. Data Validation Summary

✅ **No missing critical data**  
✅ **No duplicate records**  
✅ **Referential integrity maintained**  
✅ **Business rules validated**  
✅ **Data types correct**  
✅ **Date ranges consistent**  
✅ **Nulls are intentional and documented**

---

## 7. Known Data Characteristics

### Expected Patterns:
- Overall team accuracy slightly below 90% target (88.75%)
- Attendance rate below 90% target (82.43%)
- Error distribution roughly balanced across 4 categories
- Quarter-end months show slight accuracy dips
- Individual performance varies (85-96% accuracy range)

These patterns reflect realistic business scenarios and operational challenges.

---

*All data quality checks passed. Dataset ready for analysis.*
