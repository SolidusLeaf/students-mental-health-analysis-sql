# Students' Mental Health: SQL Analysis

A PostgreSQL analysis of how length of stay relates to mental health scores among international university students, completed as a guided practice project on DataCamp's DataLab while working through their Data Engineer career track.

## Overview

A Japanese international university surveyed its students in 2018 and published a study the following year. It found that international students have a higher risk of mental health difficulties than the general population, and that social connectedness and acculturative stress (stress from joining a new culture) are predictive of depression.

This project explores the survey data with SQL to see whether the average scores change with how long international students have been in the country.

## Dataset

A single table, `students`, with 286 rows (one per surveyed student), provided as the CSV file `students.csv` in this repository:

| Column | Description |
|---|---|
| `inter_dom` | International or domestic student |
| `japanese_cate` | Japanese language proficiency |
| `english_cate` | English language proficiency |
| `academic` | Academic level (undergraduate or graduate) |
| `age` | Age of student |
| `stay` | Current length of stay in years |
| `todep` | Total depression score (PHQ-9 test) |
| `tosc` | Total social connectedness score (SCS test) |
| `toas` | Total acculturative stress score (ASISS test) |

The data and task framing come from DataCamp's guided project.

## Task

Return one row per length of stay (nine rows) for international students only, with the number of students and the average of each of the three scores, rounded to two decimals, sorted by length of stay in descending order.

## Query

The query is written for PostgreSQL and lives in the Jupyter notebook (`.ipynb`) as an SQL cell that runs against the `students` table.

```sql
SELECT
    stay,
    COUNT(*)              AS count_int,
    ROUND(AVG(todep), 2)  AS average_phq,
    ROUND(AVG(tosc), 2)   AS average_scs,
    ROUND(AVG(toas), 2)   AS average_as
FROM students
WHERE inter_dom = 'Inter'
GROUP BY stay
ORDER BY stay DESC;
```

SQL used: `SELECT`, `WHERE`, `GROUP BY`, aggregate functions (`COUNT`, `AVG`), `ROUND`, `ORDER BY`, column aliases.

## Results

| Stay (years) | Students | Avg depression (PHQ) | Avg social connectedness (SCS) | Avg acculturative stress (ASISS) |
|---|---|---|---|---|
| 10 | 1 | 13 | 32 | 50 |
| 8 | 1 | 10 | 44 | 65 |
| 7 | 1 | 4 | 48 | 45 |
| 6 | 3 | 6 | 38 | 58.67 |
| 5 | 1 | 0 | 34 | 91 |
| 4 | 14 | 8.57 | 33.93 | 87.71 |
| 3 | 46 | 9.09 | 37.13 | 78 |
| 2 | 39 | 8.28 | 37.08 | 77.67 |
| 1 | 95 | 7.48 | 38.11 | 72.8 |

## Findings

- **Only stays of 1 to 4 years have meaningful sample sizes.** Those four groups hold 194 of the 201 international students. Each group of 5 or more years has 1 to 3 students, so averages like the 0 depression score at 5 years describe one person, not a trend.
- **Acculturative stress rises with length of stay.** Across the 1 to 4 year groups it climbs from 72.8 to 87.71, with an increase at every step.
- **Social connectedness drifts down.** It falls from 38.11 at one year to 33.93 at four years, though the 2 and 3 year groups are almost flat.
- **Depression rises, then dips.** Average PHQ scores go from 7.48 to 8.28 to 9.09 over the first three years, then fall to 8.57 at four years, where the group is only 14 students. There is no clean trend.
- **The direction is consistent with the original study**, which linked higher acculturative stress and lower social connectedness to depression.

## Limitations

- These are group averages, so they show association, not cause. The query cannot show that stress or low connectedness drives depression.
- The survey is a single snapshot. The 4-year group is not the 1-year group followed over time, so differences could partly reflect who stays longer.
- The query covers international students only and does not compare them to domestic students. It also does not test whether the differences are statistically significant.

## Tools

- **PostgreSQL**: the SQL dialect used for the query
- **Jupyter notebook (`.ipynb`)**: exported from DataCamp's DataLab, where SQL cells run directly against the `students` table

To reproduce the results outside DataLab, import the data from `students.csv` into a PostgreSQL table named `students` and run the query from the notebook.

## Repository contents

```
students-mental-health-sql/
├── README.md
├── students_mental_health_sql.ipynb
└── students.csv
```

- `students_mental_health_sql.ipynb` contains the PostgreSQL query and its output. GitHub displays `.ipynb` files directly in the browser, so no setup is needed to read it.
- `students.csv` is the `students` table (CSV format) that the query runs against.
