## Best Practice Guidelines for the EBM Datalab

### Preamble
What follows is detailed content, and a work in progress. Some of it is aspirant: we do most of it; but we are not perfect; and we do breach our own guidance. 
This is living document that will be updated with changes as we change. 

We are keen for all and any comments, please
add any comments by making a github issue on this repo. 

Please also see our 
Open Analytics Manifesto.

### Publish everything
- Use Jupyter Notebooks for your analysis and explanation. 

- All work should be on a public repository unless there is good reason not to do this. Default is to work in public. Some examples of good and bad reasons:
  * Good (or arguable!):
     * The associated paper is embargoed 
  * Bad:
     * You don’t want people to take your code
     * You are worried that the code is not perfect 

- There should be one repository at least for each paper published and this should be linked within the paper

- Pre-publication of any papers, follow the instructions [here](https://guides.github.com/activities/citable-code/) on how to cite code and place a doi citation using Zenodo within the readme (last section of the instructions). Unfortunately there is no way to get citation credit for code currently (in Web of Science). Once a paper has been published for your code, add a citation in the readme. This encourages others using your code to cite the paper associated and not just to associated  Zenodo doi. 

- Discussions about key decisions should be publicly available unless there is a good reason not to do this. The best place for this is within a GitHub issue. These can then be referenced within both a paper and the jupyter notebook. 

- Use a MIT licence as standard

### Make collaboration easy
**Readability of notebooks:**

- Notebooks are primarily to tell a story. They should be well documented with clear divisions between blocks of code. There should be clear accompanying text that tells you what the code is doing. 

- Consider keeping the code separate, imported by the notebook. At a minimum, a single file `custom_functions.py` which is imported once and contains all the necessary functions. ([#1](https://github.com/ebmdatalab/best_practice_guidance/issues/1))

- The author should not assume that the reader or user of the code will retain a large amount of prerequisite knowledge or facts in their head at once. Document any information that is relevant and necessary to understand the notebooks (Wilson 2014). Record all the processes relevant for processing the data (Wilson 2017). For example:

- Source of input files:
  - Relevant dates (i.e. when did it start/end, when was the cutoff for data to enter into the study)
  - What the processing has done
  - What the project is about
  - What problems there are with the data

- Think about notebook templates for analyses:
  - Re-use previous templates from the group
  - Think about whether what you’re doing could be a template for others.
  - Example templates that currently exist/in production:
    - Measures
    - STROBE extension for variation with “user need” pre-amble

- Standard environments: 
  - Use Docker to create containised code. A github template called [custom-docker](https://github.com/ebmdatalab/custom-docker) has been set up here. Follow the instructions in the readme to use this repo template to make your own project repo. 
  - A requirements.txt is required for all projects. Auto-write this using pip freeze > requirements.txt for accuracy rather than doing manually. 
  - Document your virtual environment in the readme as a bare minimum. Where appropriate use a script to launch docker (this is set up automatically when using the custom-docker template)

- Create each repo with a .gitignore
  - Use the .gitignore included with the custom docker repo 
  - Add additional exceptions depending on your IDE and OS. For example, if you are using a mac for example, please add .DS_Store to the git ignore

**Structure of repository:**

- Each repository should represent one project or one paper. The EBM DataLab github should be used rather than your own personal github. 

- Try to follow a standard structure to the repo if possible:

```bash
├── lib
│   └── custom_functions.py  
├── config                    
│   └── jupyter_notebook_config
├── data
├── notebooks
│   └── diffable_python
├── tests
├── .gitattributes
├── .gitignore
├── Dockerfile
├── README.md
├── requirements.in
├── requirements.txt
├── run.bat
├── run.sh
└── run_tests.sh
```
               
- Each repository should have a README that explains briefly what the project is, where the data comes from and what has been done with it. There should be some instructions on how to run the code and if using Docker, there should be a Binder button within the readme. A Zenodo DOI should be included when you have the final version of the code ready to publish. 

- Config folders contain specific Docker files to run the code

- Data outputs such as csv files should be saved with a clear name, ideally in a folder called “data”. It is often useful to save both raw and intermediate forms when working with large amounts of data that will be take a long time to re-run or require the repeated use of bigquery (and hence $) (Wilson 2017)

- Notebooks folder contains all jupyter notebooks

- If you are importing your functions from a pure python file, named this file custom_functions.py and store these files within the code folder. 
You will need a blank __init__.py within the code folder import properly into your notebook. 
Additionally you will likely have to use the os module to navigate using the appropriate path. 
[This](https://stackoverflow.com/questions/5137497/find-current-directory-and-files-directory) provides a good overview of how to do this using built in os functions. 

- Requirements should be in the main directory of the project 

**Best practice for writing code:**

- Make variable names consistent, distinctive, and meaningful (Wilson 2014)

- Code commenting should be consistent and useful. It should be aimed at a technical audience. 

  - Functions have docstrings and conform to PEP 257 (see appendix 1)
  
  - A module docstring (indicated by ``` this is the first sentence of code in a module ```) is used when creating a module. 
  
  - This is a useful document [here](https://blog.codinghorror.com/code-tells-you-how-comments-tell-you-why/https://blog.codinghorror.com/code-tells-you-how-comments-tell-you-why/) that discusses what makes useful commenting

**Data Management:** 

- Use the DataLab Python library for generating any SQL query to DataLab BigQuery. The datalab bigquery functions automatically save results as a CSV and use that as a cache as long as the SQL hasn't changed. 

- Save the raw data in a csv file 

- Record all steps for processing data 

- Save any final csv files to the data folder

- Do not assume that other users (ie. from outside our team) have access to the original database and hence we should build in sufficient intermediate data forms such as CSV to allow the notebook to still run if access to BigQuery is not possible

### Use coders’ best practice
**Best practice for using Github:**

- Work on git branches rather than a master branch. Git commit can be used several times before pushing to a branch. When you do push, push onto a branch (with a relevant name). If you want to push to master, then make a pull request

- Git commit messages should be consistent and useful to the reader. This link has useful guidance

- Git issues should be used extensively and individual people should be tagged in these if you want them to ask one specific question or address an issue. Git labels are good practice. 

**Code reviews:**

- Code reviews should be frequent, throughout the project (not just at the end) and well documented

- Nominate a reviewer by searching for their name. Briefly explain what you have done and if there are any areas of concern. This is an interesting guide on code reviews. Remember you can comment back and forth a few times on code reviews and it should be a two-way street. 

**Sharing technical knowledge within the team:**

- Use pair programming when bringing someone new up to speed and when tackling particularly tricky problems (Wilson 2014)

- The readme for a project should contain enough information that someone could use it to get your code running within a short time frame


### Automate tasks with code where possible
**Creating libraries:**

- Code that is going to be used time and time again should be refactored and implemented as a library. This should have appropriate documentation including a readme and docstrings.  

- Where possible, homemade libraries should be made publicly available. A good rule of thumb is that the first time you reuse a team members’ code, copy it. The second time, start a discussion about how to turn it into a package.  For example, you could think about updating the datalab-pandas package (this should be done with whole-team discussion). Multiple branches should be evaluated and merged or discarded. A

**Unit testing:**

- Where possible unit tests should be used to check code for errors, for example, each function can have their own unit test. See here for more information on how to write tests.

- Tests should be python files in the test folder in the main directory

- Tests should be built so that they can be run with pytest. This requires a certain naming convention:
  - Python files that contain tests should be named with test_* for example, test_outliers.py
  - Test functions should be named with `test_*name_of_function_tested()`. For example, `test_get_stats()` for testing `get_stats()` function. 

- Tests should use assert with a specific message if it fails the test. Do not use print statements. 

- At a bare minimum, each jupyter notebook should be checked that it can run from top to bottom without any errors. If you are using the custom 
docker template, this will come as standard. If this test failed, you will see a red cross next to your commit and you may receive an email (if notifications turned on)


### Appendix 1:
This is a full-on docstring and takes some time to construct. Pycharm and Spyder will autowrite this structure if you write the function within these IDEs. 

```python
def square_it(number):
    ```This function takes in a number and returns the square of that number
    
    Args:
        number (int): A whole number to be squared
        
    Returns:
        int: the squared whole number
        
    Raise:
        ValueError: If 'number' is not an integer
    
    ```
    
    if isinstance(number, int):
        result = number ** 2
        return result
    else:
        raise ValueError('{} is not an integer. Please supply an integer'.format(number))

```

As a first step, I think an explanation of what the function is doing is the minimum, and anything extra is good practice. For example:

```python
def square_it(number):
    ```This function takes in a number and returns the square of that number  
    ```
    
    if isinstance(number, int):
        result = number ** 2
        return result
    else:
        raise ValueError('{} is not an integer. Please supply an integer'.format(number))

```

### Useful References:
**Papers of interest:**
Both of these papers contain some useful points. A lot is not relevant for us and is not about jupyter notebooks but relates to using raw code files. I have picked out some boxes that are particularly useful. 

- Wilson G, Aruliah DA, Brown CT, Chue Hong NP, Davis M, Guy RT, et al. (2014) Best Practices for Scientific Computing. PLoS Biol 12(1): e1001745. https://doi.org/10.1371/journal.pbio.1001745  BOX 1 in particular
- Wilson G, Bryan J, Cranston K, Kitzes J, Nederbragt L, Teal TK (2017) Good enough practices in scientific computing. PLoS Comput Biol 13(6): e1005510. https://doi.org/10.1371/journal.pcbi.1005510 BOX 1 and the section immediately after it 



