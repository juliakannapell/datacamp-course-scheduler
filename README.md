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

## Step By Step Overview:

### Web Scraping
Packages used: Selenium, BeautifulSoup, Webdriver Manager, Pandas
![alt text](https://github.com/juliakannapell/datacamp-course-scheduler/DataCamp Course Flowchart.jpg?raw=true)


### Custom Named Entity Recognition Model
Packages used: SpaCy

### Kahn's Algorithm
Packages used: networkx

### User Interface
Packages used: ipywidgets

## Named Entity Recognition Model Metrics:
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

## Constraints:
1. I considered using DataCamp's API to web scrape the course data, however their available API did not have the course prerequisite field that my output depended on.  As a result, I built my own web scraping process using Selenium and BeautifulSoup.
2. Because I created the dataset I used for my custom Named Entity Recognition model, I used a lower number of train and test rows as each one had to be tagged manually.  Despite this, I ended up with high accuracy metrics for the model.
