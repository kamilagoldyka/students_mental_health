## students_mentalhealth
# 🌍 Mental Health & Studying Abroad: An Analysis  

Does studying in a different country impact mental health? A 2018 survey conducted by a Japanese international university examined this question, with findings published in 2019 after approval from multiple ethical and regulatory boards.  

The study revealed that international students face a higher risk of mental health challenges compared to the general population. It also identified **social connectedness** (sense of belonging) and **acculturative stress** (challenges of adapting to a new culture) as key predictors of depression.  

This project analyzes student data using **PostgreSQL** to assess whether these findings hold for this sample and to explore whether the **length of stay** influences mental health outcomes.  

Data source: Nguyen, M.-H., Ho, M.-T., Nguyen, Q.-Y. T., & Vuong, Q.-H. (2019). A Dataset of Students’ Mental Health and Help-Seeking Behaviors in a Multicultural Environment. Data, 4(3), 124. [https://doi.org/10.3390/data4030124](https://doi.org/10.3390/data4030124)

## Description of the columns

| **Field Name**    | **Description**                                         |
|-------------------|---------------------------------------------------------|
| `inter_dom`       | Types of students (international or domestic)           |
| `japanese_cate`   | Japanese language proficiency                           |
| `english_cate`    | English language proficiency                            |
| `academic`        | Current academic level (undergraduate or graduate)      |
| `age`             | Current age of student                                  |
| `stay`            | Current length of stay in years                         |
| `todep`           | Total score of depression (PHQ-9 test)                  |
| `tosc`            | Total score of social connectedness (SCS test)          |
| `toas`            | Total score of acculturative stress (ASISS test)        |


## Analysis
```sql
-- First query to get overall statistics
SELECT 
    COUNT(*) AS total_count,
    ROUND(MIN(LEAST(todep, tosc, toas)), 2) AS min_phq, 
    ROUND(MAX(GREATEST(todep, tosc, toas)), 2) AS max_phq, 
    ROUND(AVG((todep + tosc + toas) / 3.0), 2) AS avg_phq
FROM 
    students;

-- Second query to find the number of international students and their average scores by length of stay, in descending order of length of stay
SELECT 
    stay, 
    COUNT(*) AS count_int,
    ROUND(AVG(todep), 2) AS average_phq, 
    ROUND(AVG(tosc), 2) AS average_scs, 
    ROUND(AVG(toas), 2) AS average_as
FROM 
    students
WHERE 
    inter_dom = 'Inter'
GROUP BY 
    stay
ORDER BY 
    stay DESC;
```

## Table with findings

| index | stay | count_int | average_phq | average_scs | average_as |
|-------|------|-----------|-------------|-------------|------------|
| 0     | 10   | 1         | 13          | 32          | 50         |
| 1     | 8    | 1         | 10          | 44          | 65         |
| 2     | 7    | 1         | 4           | 48          | 45         |
| 3     | 6    | 3         | 6           | 38          | 58.67      |
| 4     | 5    | 1         | 0           | 34          | 91         |
| 5     | 4    | 14        | 8.57        | 33.93       | 87.71      |
| 6     | 3    | 46        | 9.09        | 37.13       | 78         |
| 7     | 2    | 39        | 8.28        | 37.08       | 77.67      |
| 8     | 1    | 95        | 7.48        | 38.11       | 72.8       |

## Conclusions

### Trend for students staying 1-4 years
Average AS (Academic Success) Score: There's a noticeable increase in the average AS score as the length of stay increases for students who have been in the program for 1-4 years. This suggests that students may be improving in their academic success over time.

### Average SCS (Social and Cultural Success) Score
Interestingly, the average SCS score slightly decreases as the length of stay increases. This could suggest that, while students may become more academically successful over time, they might experience a slight decline in social or cultural adaptation.

### Caution for students staying 5+ years
For students who have stayed for 5 years or more, the sample sizes are very small (1-3 students), so it's difficult to draw reliable conclusions from this data.

### Overall Observation:
Overall, the data suggests that for students who stay between 1 and 4 years, academic success (AS score) seems to improve, while social and cultural success (SCS score) experiences a small decline. However, further analysis with a larger sample or over a longer period might provide more concrete insights into these trends.


