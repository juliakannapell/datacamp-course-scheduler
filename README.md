# DataCamp Course Scheduler

# Author: Julia Kannapell

## Contents in the Repository:
1. Jupyter Notebook (includes User Interface)

## Background
DataCamp is an online learning platform, specializing in courses for Data Science, Data Engineering, and Data Analysis.  This platform hosts 582 courses encompassing Data Cleaning, Data Discovery, Data Modeling, AI tools, and much more.  DataCamp courses are often linked together through Prerequisite relationships, where certain courses must be completed before others can be started.

DataCamp offers track-based options for people wanting to develop the skills of a Data Scientist or Data Analyst, but few options for experienced Data Scientists wanting to upskill on very specific topics outside of searching the course catalog for the desired topic.  This is not a great solution, however, because that only points the Data Scientist toward a course that covers that topic while leaving the Data Scientist to identify the course prerequisites manually, each of which could have prerequisites of its own.

## Project Overview:
My project allows the Data Scientist to select common Data Science/Machine Learning topics from a drop-down menu to identify DataCamp courses that would assist in learning their desired topic.  Once the Data Scientist selects the courses from this topic that they would like to complete, my project generates a recommended course sequence incorporating all levels of prerequisites required for those courses as well as ensuring that each prerequisite is taken before the course in which it is required.

I accomplish this objective in four parts: 
- Web scraping of the DataCamp website to generate the course list and obtain attributes such as course description and prerequisites
- Custom Named Entity Recognition model to identify topics covered in the course description, such as Python packages, software, or general data science concepts.
- Implementation of Kahn's algorithm to order the courses in such a way that all prerequisites are fulfilled before a given course is scheduled.
- User interface in ipywidgets where the user can select topics of interest from a dropdown list, select the courses they would like to complete from the returned course list, and be provided with a recommended course schedule that allows the user to complete all courses and prerequisites required, ensuring that prerequisites are always completed before the courses that require them.

## Project Walkthrough:
DataCamp courses are connected to each other via prerequisite - course relationships.  Introductory level courses typically will not have prerequisites, however Intermediate and Advanced level courses almost always do.  In this example, you can see that the DataCamp course Introduction to Deep Learning with PyTorch has 3 direct prerequisites and a total of 8 courses that must be completed before Deep Learning with PyTorch can be started.
![alt text](https://github.com/juliakannapell/datacamp-course-scheduler/blob/main/DataCamp_Course_Flowchart.jpg?raw=true)

### Web Scraping
I accessed the DataCamp course data using Selenium and BeautifulSoup, as the available API did not include all of the fields I needed.  For each of the 582 available courses, I returned the course title, course description, and list of prerequisites (if any) for that course.

### Custom Named Entity Recognition Model
The course description includes detailed information about the topics that the course covers and is much more inclusive that the course title alone.  For example, the "Monte Carlo Simulations in Python" course covers NumPy, SciPy and Seaborn which are mentioned in the description but not the title.  For an advanced Data Scientist who is looking to learn specific Python packages, this information is very valuable.

![alt text](https://github.com/juliakannapell/datacamp-course-scheduler/blob/main/Monte_Carlo_Simulation_Description.jpg?raw=true)

To obtain the relevant topics from each course description, I created a custom Named Entity Recognition model using SpaCy to identify 4 types of entities: Python packages, software (i.e. Tableau, PowerBI), programming languages, and data concepts (i.e. regression, clustering). This model accurately identifies entities from these 4 categories with an average F1 score of 0.9465.

These captured named entities will be used to filter courses to those that cover a specific topic or tool.

#### Named Entity Recognition Model Metrics:
5 Fold Cross Validation
- Fold 1 F1: 0.9649
- Fold 2 F1: 0.9526
- Fold 3 F1: 0.9568
- Fold 4 F1: 0.9025
- Fold 5 F1: 0.9555

Cross Validation Results
- Average F1: 0.9465 (±0.0224)
- Average Precision: 0.9532 (±0.0123)
- Average Recall: 0.9544 (±0.0172)

### Kahn's Algorithm
The DataCamp web scraping provided a list of courses and their associated prerequisites.  Each course's prerequisites must be completed before the course can be started and multiple courses may share the same prerequisites.  I used Kahn's algorithm to find the optimal course order to ensure that levels of prerequisites were included for each course and that all prerequisites were completed before the course where they were required.

### User Interface
The user interface allows the user to select, from a dropdown, topics of interest that they would like to study.  A subset of courses is provided that covers that specific topic, and the user is able to select some or all of the provided courses to complete.  Once they click submit, all levels of prerequisites are gathered for each chosen course and, using Kahn's algorithm, are arranged in the optimal order to be completed.

Using the example of "Introduction to Deep Learning with PyTorch" mentioned earlier, you can see that the Optimal Course Order returned allows all prerequisites to be completed before the course in which they are required.

Course and Prerequisite Relationships
![alt text](https://github.com/juliakannapell/datacamp-course-scheduler/blob/main/DataCamp_Course_Flowchart.jpg?raw=true)

Optimal Course Order
![alt text](https://github.com/juliakannapell/datacamp-course-scheduler/blob/main/Optimal_Course_Order.jpg?raw=true)

## Constraints:
1. I considered using DataCamp's API to web scrape the course data, however their available API did not have the course prerequisite field that my output depended on.  As a result, I built my own web scraping process using Selenium and BeautifulSoup.
2. Because I created the dataset I used for my custom Named Entity Recognition model, I used a lower number of train and test rows as each one had to be tagged manually.  Despite this, I ended up with high accuracy metrics for the model.
