## Introduction

In today's fast-paced and competitive educational environment, understanding the factors that influence student success is more important than ever. Just like the transport system in a bustling city like London must adapt to serve its residents, schools and educators must adapt to meet the needs of students. In this project, we will take a deep dive into a dataset containing rich details about various aspects of student life, such as hours studied, sleep patterns, attendance, and more, to uncover what truly impacts exam performance.

The dataset we'll be working with includes a wide range of factors influencing student performance. By analyzing this data, we'll be able to identify key drivers of success and provide insights that could help students, teachers, and policymakers make informed decisions. The table we'll use for this project is called `student_performance` and includes the following data:


| Column                   | Definition                                                      | Data type             |
|--------------------------|-----------------------------------------------------------------|-----------------------|
| `attendance`              | Percentage of classes attended                                  |     `float`               |
| `extracurricular_activities` | Participation in extracurricular activities                   |     `varchar` (Yes, No)    |
| `sleep_hours`             | Average number of hours of sleep per night                      |     `float`               |
| `tutoring_sessions`       | Number of tutoring sessions attended per month                  |     `integer`             |
| `teacher_quality`         | Quality of the teachers                                         |     `varchar` (Low, Medium, High) |
| `exam_score`              | Final exam score                                                |     `float`               |

You will execute SQL queries to answer three questions, as listed in the instructions.

1. Do more study hours and extracurricular activities lead to better scores? Analyze how studying more than 10 hours per week, while also participating in extracurricular activities, impacts exam performance. The output should include two columns: 1) hours_studied and 2) avg_exam_score. Group and sort the results by hours_studied in descending order. Save the query as avg_exam_score_by_study_and_extracurricular.
-- avg_exam_score_by_study_and_extracurricular

SELECT hours_studied, AVG(exam_score) AS avg_exam_score
FROM student_performance
WHERE  hours_studied > 10 
	AND extracurricular_activities = 'Yes'
GROUP BY hours_studied
ORDER BY hours_studied DESC;

### Output (first 3 rows):

index | hours_studied | avg_exam_score |
------|---------------|----------------|
0     | 43            | 78             |
1     | 39            | 75             |
2     | 38            | 73.5           |

2. Is there a sweet spot for study hours? Explore how different ranges of study hours impact exam performance by calculating the average exam score for each study range. Categorize students into four groups based on hours studied per week: 1-5 hours, 6-10 hours, 11-15 hours, and 16+ hours. The output should contain two columns: 1) hours_studied_range and 2) avg_exam_score. Group the results by hours_studied_range and sort them by avg_exam_score in descending order. Save the query as avg_exam_score_by_hours_studied_range.
-- avg_exam_score_by_hours_studied_range
 
SELECT 
	CASE 
		WHEN hours_studied BETWEEN 1 AND 5 THEN '1-5 hours'
	 	WHEN hours_studied BETWEEN 6 AND 10 THEN '6-10 hours'
	 	WHEN hours_studied BETWEEN 11 AND 15 THEN '11-15 hours'
		ELSE '16+ hours'
	END AS hours_studied_range,
	AVG(exam_score) AS avg_exam_score
FROM student_performance
GROUP BY hours_studied_range
ORDER BY avg_exam_score DESC;

### Output (first 3 rows):

index | hours_studied_range | avg_exam_score |
------|---------------------|----------------|
0     | 16+ hours           | 67.9233633869  |
1     | 11-15 hours         | 65.2043859649  |
2     | 6-10 hours          | 64.2254901961  |

3. A teacher wants to show their students their relative rank in the class, without revealing their exam scores to each other. Use a window function to assign ranks based on exam_score, ensuring that students with the same exam score share the same rank and no ranks are skipped. Return the columns attendance, hours_studied, sleep_hours, tutoring_sessions, and exam_rank. The students with the highest exam score should be at the top of the results, so order your query by exam_rank in ascending order. Limit your query to 30 students.

-- student_exam_ranking
-- Add solution code below

SELECT attendance, hours_studied, sleep_hours, tutoring_sessions,
	DENSE_RANK() OVER (ORDER BY exam_score DESC) AS exam_rank
FROM student_performance
ORDER BY exam_rank ASC
LIMIT 30;

### Output (first 3 rows):

index |attendance | hours_studied | sleep_hours | tutoring_sessions | exam_rank |
------|-----------|---------------|-------------|-------------------|-----------|
0     |    98     |      27       |      6      |         5         |     1     |
1     |    89     |      18       |      4      |         3         |     2     |
2     |    90     |      14       |      8      |         4         |     3     |

