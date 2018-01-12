# school_attendance
basic script that gives different attendance rates from a dimension level summary table

## a summary table with demographics for each student in the district: student_id | school_id | grade_level | date_of_birth | hometown
## Using this data, you could answer questions like the following:
## What was the overall attendance rate for the school district yesterday?
## Which grade level currently has the most students in this school district?
## Which school had the highest attendance rate? The lowest?

library(dplyr)
district <- rep("MENLO.PARK", 100)
student_id <- c(1:100)
school_id <- c(rep(1:5, 20))
grade <- c(rep(c("8th","9th", "10th", "11th", "12th"), 20))
hometown <- c(rep(c("MENLO", "PALO", "MV", "WOODSIDE"), 25))
attendance <- c(sample(c("T", "F"), 100, replace = TRUE))

df <- data.frame(district, student_id, school_id, grade, hometown, attendance)

# overall attendance
df %>% summarise(n.in = sum(attendance == "T"), n.out = sum(attendance == "F")) %>% mutate(percent.in = n.in/(n.in + n.out))

# most students in the school district
df %>% group_by(grade) %>% summarise(n.grade = n_distinct(student_id)) %>% arrange(desc(n.grade))

# which school has the highest attendance rate
df %>% group_by(school_id) %>% summarise(n.in = sum(attendance == "T"), n.out = sum(attendance == "F")) %>% 
  mutate(percent.in = n.in/(n.in + n.out)) %>% arrange(percent.in)
