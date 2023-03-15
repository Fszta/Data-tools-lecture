# Introduction
## Some general reading ...

What is data science?
Data science is an interdisciplinary field that involves the extraction of insights from data using statistical, computational, and machine learning techniques. Data science involves the entire process of collecting, processing, analyzing, and interpreting data to solve complex problems and make data-driven decisions.

Why is data science important?
Data science is important because it allows us to make sense of vast amounts of data that are generated every day. By extracting insights from data, we can identify patterns, trends, and relationships that can inform business decisions, scientific research, and public policy. Data science also plays a critical role in developing and improving machine learning algorithms that power many modern technologies.

What are some common applications of data science?
Data science is used in a wide range of industries and fields, including business, healthcare, finance, marketing, and scientific research. Some common applications of data science include fraud detection, customer segmentation, predicting disease outbreaks, personalized recommendations, and image and speech recognition.

What are data science tools?
Data science tools are software applications, libraries, and frameworks that are used to facilitate the various tasks involved in data science, such as data manipulation, data analysis, machine learning, and data visualization. Some common data science tools include Python, R, Jupyter Notebook, pandas, NumPy, Matplotlib, scikit-learn, and TensorFlow.





## Session 1
``` mermaid
    %%{init: { 'logLevel': 'debug', 'theme': 'base', 'gitGraph': {'showBranches': true, 'showCommitLabel':true,'mainBranchName': 'session1'}} }%%

    gitGraph
       checkout session1
       commit id: "Git & Version control"
       commit id: "Python package management"
       branch foo
       checkout foo
       commit id: "Anaconda + Conda"
       commit id: "pip"
       commit id: "poetry"
       checkout session1
       merge foo
       commit id: "Notebooks"
       
```


## Session 2
This session will be oriented around basic data visualization libraries & framework.

``` mermaid
    %%{init: { 'logLevel': 'debug', 'theme': 'base', 'gitGraph': {'showBranches': true, 'showCommitLabel':true,'mainBranchName': 'session2'}} }%%

    gitGraph
       commit id: "Data viz libraries"
       branch libraries
       checkout libraries
       commit id: "matplotlib"
       commit id: "seaborn"
       checkout session2
       merge libraries
       commit id: "Data viz framework"
       branch framework
       checkout framework
       commit id: "plotly dash"
       checkout session2
       merge framework
       
 
       
```


## Session 3 
* Docker
  * Setup a BI Platform with postgres & metabase based on docker
* Advance technical documentation

## Session 4
* Practice
