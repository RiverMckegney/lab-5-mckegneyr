# STAT210 Spring 2025: Lab 5 Student Evals
River Mckegney

In this lab, we will be using the `dplyr` package to explore student
evaluations of teaching data.

**You are expected to use functions from `dplyr` to do your data
manipulation!**

# Part 1: GitHub Workflow

Now that you have the Lab 5 repository cloned, you need to make sure you
can successfully push to GitHub. To do this you need to:

-   Open the `lab-5-student.qmd` file (in the lower right hand corner).
-   Change the `author` line at the top of the document (in the YAML) to
    your name.
-   Save your file either by clicking on the blue floppy disk or with a
    shortcut (command / control + s).
-   Click the “Git” tab in upper right pane
-   Check the “Staged” box for the `lab-5-student.qmd` file (the file
    you changed)
-   Click “Commit”
-   In the box that opens, type a message in “Commit message”, such as
    “Added my name”.
-   Click “Commit”.
-   Click the green “Push” button to send your local changes to GitHub.

RStudio will display something like:

    >>> /usr/bin/git push origin HEAD:refs/heads/main
    To https://github.com/atheobold/introduction-to-quarto-allison-theobold.git
       3a2171f..6d58539  HEAD -> main

Now you are ready to go! Remember, as you are going through the lab I
would strongly recommend rendering your HTML and committing your after
**every** question!

# Part 2: Some Words of Advice

Part of learning to program is learning from a variety of resources.
Thus, I expect you will use resources that you find on the internet.
There is, however, an important balance between copying someone else’s
code and *using their code to learn*.

Therefore, if you use external resources, I want to know about it.

-   If you used Google, you are expected to “inform” me of any resources
    you used by **pasting the link to the resource in a code comment
    next to where you used that resource**.

-   If you used ChatGPT, you are expected to “inform” me of the
    assistance you received by (1) indicating somewhere in the problem
    that you used ChatGPT (e.g., below the question prompt or as a code
    comment), and (2) downloading and including the `.txt` file
    containing your **entire** conversation with ChatGPT.

Additionally, you are permitted and encouraged to work with your peers
as you complete lab assignments, but **you are expected to do your own
work**. Copying from each other is cheating, and letting people copy
from you is also cheating. Please don’t do either of those things.

## Setting Up Your Code Chunks

-   The first chunk of this Quarto document should be used to *declare
    your libraries* (probably only `tidyverse` for now).
-   The second chunk of your Quarto document should be to *load in your
    data*.

## Save Regularly, Render Often

-   Be sure to **save** your work regularly.
-   Be sure to **render** your file every so often, to check for errors
    and make sure it looks nice.
    -   Make sure your Quarto document does not contain `View(dataset)`
        or `install.packages("package")`, both of these will prevent
        rendering.
    -   Check your Quarto document for occasions when you looked at the
        data by typing the name of the data frame. Leaving these in
        means the whole dataset will print out and this looks
        unprofessional. **Remove these!**
    -   If all else fails, you can set your execution options to
        `error: true`, which will allow the file to render even if
        errors are present.

# Part 3: Let’s Start Working with the Data!

## The Data

The `teacher_evals` dataset contains student evaluations of reaching
(SET) collected from students at a University in Poland. There are SET
surveys from students in all fields and all levels of study offered by
the university.

The SET questionnaire that every student at this university completes is
as follows:

> Evaluation survey of the teaching staff of University of Poland.
> Please complete the following evaluation form, which aims to assess
> the lecturer’s performance. Only one answer should be indicated for
> each question. The answers are coded in the following way: 5 - I
> strongly agree; 4 - I agree; 3 - Neutral; 2 - I don’t agree; 1 - I
> strongly don’t agree.
>
> Question 1: I learned a lot during the course.
>
> Question 2: I think that the knowledge acquired during the course is
> very useful.
>
> Question 3: The professor used activities to make the class more
> engaging.
>
> Question 4: If it was possible, I would enroll for a course conducted
> by this lecturer again.
>
> Question 5: The classes started on time.
>
> Question 6: The lecturer always used time efficiently.
>
> Question 7: The lecturer delivered the class content in an
> understandable and efficient way.
>
> Question 8: The lecturer was available when we had doubts.
>
> Question 9. The lecturer treated all students equally regardless of
> their race, background and ethnicity.

These data are from the end of the winter semester of the 2020-2021
academic year. In the period of data collection, all university classes
were entirely online amid the COVID-19 pandemic. While expected learning
outcomes were not changed, the online mode of study could have affected
grading policies and could have implications for data.

**Average SET scores** were combined with many other variables,
including:

1.  **characteristics of the teacher** (degree, seniority, gender, SET
    scores in the past 6 semesters).
2.  **characteristics of the course** (time of day, day of the week,
    course type, course breadth, class duration, class size).
3.  **percentage of students providing SET feedback.**
4.  **course grades** (mean, standard deviation, percentage failed for
    the current course and previous 6 semesters).

This rich dataset allows us to **investigate many of the biases in
student evaluations of teaching** that have been reported in the
literature and to formulate new hypotheses.

Before tackling the problems below, study the description of each
variable included in the `teacher_evals_codebook.pdf`.

**1. Load the appropriate R packages for your analysis.**

`{r setup, include=FALSE} knitr::opts_chunk$set(echo = TRUE, include = TRUE, output = TRUE)`

``` {r}
#| label: Library setup
#| echo: TRUE
  
# loading packages
library(tidyverse)
```

**2. Load in the `teacher_evals` data.**

``` {r}
#| label: Import data
#| echo: TRUE

# Reading-in the dataset: The call...
instr_evals <- read.csv("data-raw/teacher_evals.csv")
```

### Data Inspection + Summary

**3. Provide a brief overview (~4 sentences) of the dataset.**

``` {r}
#| label: Data exploration ~ About this dataset

# Code used to check out the data
glimpse(instr_evals)
```

Question 3 answer/response: By River Mckegney

This dataset appears to consist of 22 columns and spans approximately
8,000 rows, comprising data types character, integer, and double. The
variables contain information about the instructor, the students, the
class itself, and the students evaluations. The dataset appears
relatively organized and tidy, as naming for columns and variable values
seem consistent throughout, and missing values identified as “NA”. Most
columns via name are somewhat self explanatory, however without explicit
metadata detailing the context of each variable (which I assume is
contained within the teacher_evals_codebook.pdf - a document I could not
figure out where to access) I may run into trouble figuring out this
dataset.

**4. What is the unit of observation (i.e. a single row in the dataset)
identified by?**

``` {r}
#| label: row-identification
#| echo: true
#| eval: true

# Code used to view rows of dataset
head(instr_evals)
```

Question 4 answer/response: By River Mckegney

Each row represents an individual observation or case of **a unique
question** (from the questionair) and its associated variables
collectively from the entire class (of students in a given class) about
the given instructor. Calculations of average SET scores are conducted
and included in the dataset pertaining to each row or question as an
index of the other column variables (ie. such as sample size or
“no_participants”, average value of student evaluation response scores
per question about an instructor, duration of class, student grades, and
other factors).

**5. Use *one*`dplyr` pipeline to clean the data by:**

-   **renaming the `gender` variable `sex`**
-   **removing all courses with fewer than 10 respondents**
-   **changing data types in whichever way you see fit (e.g., is the
    instructor ID really a numeric data type?)**
-   **only keeping the columns we will use – `course_id`, `teacher_id`,
    `question_no`, `no_participants`, `resp_share`, `SET_score_avg`,
    `percent_failed_cur`, `academic_degree`, `seniority`, and `sex`**

**Assign your cleaned data to a new variable named `teacher_evals_clean`
–- use these data going forward. Save the data as
`teacher_evals_clean.csv` in the `data-clean` folder.**

``` {r}
#| label: data-cleaning
#| echo: true
#| eval: true

# Data Filtering
teacher_evals_clean <- instr_evals %>%
  filter(!c(no_participants < 10)) %>%
  rename(sex = gender) %>% # new_name = old_name syntax
  mutate(teacher_id = factor(teacher_id),
         question_no = factor(question_no),
         seniority = factor(seniority)) %>%
  select(course_id, teacher_id, question_no, no_participants, resp_share, SET_score_avg, percent_failed_cur, academic_degree, seniority, sex)

# Check & make sure it works
glimpse(teacher_evals_clean)
head(min(teacher_evals_clean$no_participants))

# Save tidy data into csv, store in tidy-data folder
write.csv(teacher_evals_clean, 
          file = "data-tidy/teacher_evals_clean.csv")
```

Lines 9-11  
Link for method used to change data type of select columns:
<https://r4ds.hadley.nz/data-import>.

**6. How many unique instructors and unique courses are present in the
cleaned dataset?**

``` {r}
#| label: unique-courses
#| echo: true
#| eval: true

# Investigate unique values for course_id and teacher_id
length(unique(teacher_evals_clean$course_id))
length(unique(teacher_evals_clean$teacher_id))

# Check to make sure: View A tibble details
teacher_evals_clean %>% 
  group_by(course_id) %>%
  summarise(Total_classes = length(n()))

teacher_evals_clean %>% 
  group_by(teacher_id) %>%
  summarise(Total_Instructors = length(n()))
```

Lines 12,16  
Link that helped me figure out how to count unique classes and teachers:
<https://stackoverflow.com/questions/38382990/in-r-how-do-i-count-unique-values-of-a-factor-based-on-another-factor>.

Question 6 answer/response: By River Mckegney

This dataset appears to contain a total of 939 unique classes/courses
and 297 instructors.

**7. One teacher-course combination has some missing values, coded as
`NA`. Which instructor has these missing values? Which course? What
variable are the missing values in?**

``` {r}
#| label: uncovering-missing-values
#| echo: true
#| eval: true

# Quest. 7. Finding NAs 

teacher_evals_clean %>%
  group_by(teacher_id, course_id) %>%
  filter(if_any(everything(), is.na))
```

Line 9  
How to filter for NA values:
<https://stackoverflow.com/questions/74525000/how-to-filter-for-rows-containing-na>.

Question 7 answer/response: By River Mckegney

The class that contains NAs is course ID PAB3SE004PA, with instructor ID
56347. The missing values are located within the column
“percent_failed_cur”.

**8. What are the demographics of the instructors in this study?
Investigate the variables `academic_degree`, `seniority`, and `sex` and
summarize your findings in ~3 complete sentences.**

``` {r}
#| label: exploring-demographics-of-instructors
#| echo: true
#| eval: true

# Quest 8. Summary stats about instructors

# Count total unique/distinct male instructors
teacher_evals_clean %>%
  filter(sex == "male") %>%
  distinct(teacher_id) %>%
  count()

# Count total unique/distinct female instructors
teacher_evals_clean %>%
  filter(sex == "female") %>%
  distinct(teacher_id) %>%
  count() 

# Calculate proportion of gender
teacher_evals_clean %>%
  summarize(prop_male = 161/297,
            prop_female = 136/297)

# Generate table of total degree types from all individual instructors
teacher_evals_clean %>%
  group_by(academic_degree) %>%
  distinct(teacher_id) %>%
  summarize(n_degree_type = n())

# Calculate degree field proportions
teacher_evals_clean %>%
  group_by(academic_degree) %>%
  distinct(teacher_id) %>%
  summarize(
    prop_no_deg = sum(academic_degree == "no_dgr")/297,
    prop_ma = sum(academic_degree == "ma")/297,
    prop_dr = sum(academic_degree == "dr")/297,
    prop_prof = sum(academic_degree == "prof")/297)
# New: Its possible to use a filter within summarize as well as during calculations

# And/or calculate percent degree types: neat table
teacher_evals_clean %>%
  group_by(academic_degree) %>%
  distinct(teacher_id) %>%
  summarize(percent_deg_type = sum(n()/297*100))


# Table of seniority ranking proportions
teacher_evals_clean %>%
  group_by(seniority) %>%
  distinct(teacher_id) %>%
  summarize(n_seniority = n())

# Calculate proportion of seniority
teacher_evals_clean %>%
  group_by(seniority) %>%
  distinct(teacher_id) %>%
  summarize(Percent_rank = sum(n()/297*100))
```

Question 8 answer/response: By River Mckegney

This dataset comprises approximately 54% male and 46% female
instructors. More than half (about 57%) of the teachers have doctorate
degrees, while a quarter (roughly 26%) have master degrees, 14% have no
degree, and 3% have professional degrees. The majority of instructors
(about 30%) have seniority rank of 2, followed by 6 (approx. 12%), 1
(approx. 11%), 8 (approx. 10%), 4 (approx. 8%), followed by 3, 5, 7 (all
equivalent and about 5.4% each), 10 (approx. 5%), and 9 (approx. 3%)
respectively.

**9. Each course seems to have used a different subset of the nine
evaluation questions. How many teacher-course combinations asked all
nine questions?**

``` {r}
#| label: teacher-course-asked-every-question
#| echo: true
#| eval: true

# Classes that conducted entire questionair

  teacher_evals_clean %>%
    group_by(course_id, teacher_id) %>%
    summarize(question_count = n_distinct(question_no)) %>%
    ungroup() %>%
    filter(question_count == 9)
  
# Sample Test: confirm code above works
  teacher_evals_clean %>%
    filter(course_id == "0000-SEM-SP",
           teacher_id == "38335")
```

Line 9  
Source used to figure out how to count questions for each teacher-course
combo:
<https://stackoverflow.com/questions/34637206/dplyr-n-distinct-with-condition>

Question 9 answer/response: By River Mckegney

It appears 49 teacher/course combos (aka specific class sections)
contained all 9 questions of the questionair.

## Rate my Professor

**10. Which instructors had the highest and lowest average rating for
Question 1 (I learnt a lot during the course.) across all their
courses?**

``` {r}
#| label: quest 10 high & low instructor avg SET score 
#| echo: true

# Quest 10

# Why max and min do not work here???
# teacher_evals_clean %>%
#   filter(question_no == "901") %>%
#   group_by(teacher_id) %>%
#   summarize(high_score = max(SET_score_avg),
#             low_score = min(SET_score_avg))

         
# How to make this method work???
# teacher_evals_clean %>%
#   filter(question_no == "901") %>%
#   mutate(max_score = filter(SET_score_avg == 5),
#          min_score = filter(SET_score_avg == 1))
#          %>%
#   group_by(teacher_id) 


# Alternative method(s)
teacher_evals_clean %>%
  filter(question_no == "901",
         SET_score_avg == 5) %>%
  distinct(course_id, teacher_id, .keep_all = TRUE) %>%
  select(course_id, teacher_id, SET_score_avg)

teacher_evals_clean %>%
  filter(question_no == "901",
         SET_score_avg == 1) %>%
  group_by(course_id, teacher_id) %>%
  select(course_id, teacher_id, SET_score_avg)
  
```

Question 10 answer/response: By River Mckegney

It appears 38 instructor course combinations scored the highest rank,
while 8 instructor course combinations scored the lowest.

**11. Which instructors with one year of experience had the highest and
lowest average percentage of students failing in the current semester
across all their courses?**

``` {r}
#| label: one-year-experience-failing-students

# Quest 11
teacher_evals_clean %>%
  filter(seniority == "1") %>%
  group_by(teacher_id) %>%
  summarize(highs = max(percent_failed_cur),
            lows = min(percent_failed_cur)) %>%
  ungroup() %>%
  mutate(difference = (highs - lows)) %>%
  arrange(desc(difference))
```

Question 11 answer/response: By River Mckegney

It appears that instructors 104362 (about 70%) followed by 84688 (about
50%), and 101703 (35%) show the greatest range between max and min
values for percent of students that failed the class.

**12. Which female instructors with either a doctorate or professional
degree had the highest and lowest average percent of students responding
to the evaluation across all their courses?**

``` {r}
#| label: female-instructor-student-response
# code chunk for Q11

teacher_evals_clean %>%
  filter(sex == "female", academic_degree == "dr" | academic_degree == "prof") %>% group_by(teacher_id) %>%
  summarize(prop_max = max(resp_share), prop_min = min(resp_share)) %>%
  mutate(prop_range = prop_max - prop_min) %>%
  arrange(desc(prop_range))
```

Question 12 answer/response: By River Mckegney

It appears that instructor 76394 (47% difference in percentage of
respondents) and 38347 (about 35% difference in percentage of
respondents) had the greatest difference or range in respondents for the
evaluation questionair across all their classes.

## References:

1.  R for Data Science, 2nd edition. Hadley Wickham, Mine
    Çetinkaya-Rundel, and Garrett Grolemund. “7 Data import”. Website
    designed using Quarto. <https://r4ds.hadley.nz/data-import>.

2.  Stack overflow. “In R, how do I count unique values of a factor
    based on another factor?”. Last modified 2016-07-14.
    <https://stackoverflow.com/questions/38382990/in-r-how-do-i-count-unique-values-of-a-factor-based-on-another-factor>.

3.  Stack overflow. “How to filter for rows containing NA?”. Last
    modified 2022-11-21.
    <https://stackoverflow.com/questions/74525000/how-to-filter-for-rows-containing-na>.

4.  Stack overflow. “dplyr n_distinct with condition”. Last modified
    2022-11-02.
    <https://stackoverflow.com/questions/34637206/dplyr-n-distinct-with-condition>.
