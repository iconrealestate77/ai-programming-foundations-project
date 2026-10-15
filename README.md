# AI Programming Foundations Project

## Project Description
This project is a reproducible data workflow in Python. It loads the Titanic passenger dataset, cleans it with documented functions, explores survival patterns, and shows them in three labeled charts. The notebook ends with a written summary of the findings and their limits.

**Dataset:** Titanic passenger data (891 rows), the seaborn-data copy: https://github.com/mwaskom/seaborn-data/blob/master/titanic.csv (saved here as `titanic.csv`)

## How to Run the Project

1. Clone the repository and open the project folder:
   ```
   git clone https://github.com/iconrealestate77/ai-programming-foundations-project.git
   cd ai-programming-foundations-project
   ```
2. (Recommended) Create and activate a virtual environment:
   ```
   python3 -m venv venv
   source venv/bin/activate
   ```
   On Windows, activate it with `venv\Scripts\activate`.
3. Install the dependencies:
   ```
   pip install -r requirements.txt
   ```
4. Start Jupyter and open the notebook:
   ```
   jupyter notebook
   ```
   Open `data_workflow.ipynb`, then choose **Kernel → Restart & Run All**. `titanic.csv` must stay in the same folder as the notebook.

## Reflections

**Where poor data cleaning could introduce bias:** About 20% of passenger ages were missing, and they were not missing evenly: about 28% of third-class passengers had no age, compared with about 14% in first class and 6% in second class. If I had simply deleted those rows, third-class passengers would have been under-represented, and since third class had the lowest survival rate, the survival results would have been pushed upward. Filling missing ages with one overall average would also be a problem, because it would give very different groups the same age. I filled ages with the median for the same sex and class instead, but this still shrinks the natural spread of ages and creates an artificial peak, so age-based results should be read with care. I also dropped `deck` because about 77% of it was missing, which means I cannot say anything about where passengers were on the ship.

**How this workflow would change for machine learning:** I would split the data into training and test sets before any cleaning, and calculate the median ages from the training set only, so no information from the test set leaks into the model. I would also encode categories such as `sex` and `embarked` as numbers, scale numeric columns where the model needs it, and track which cleaning choices were used for each model run. The cleaning functions would be moved into a reusable pipeline.

**How this prepares me for neural networks:** Neural networks need clean, fully numeric, consistently scaled inputs and a clear target column, which here is `survived`. The cleaning functions already remove missing values and inconsistent labels, and the EDA shows which features (sex, class, fare) carry signal. A real project would also need a validation split and a way to avoid overfitting, since this dataset is small.

**How this could be automated with agentic AI:** An agent could run the same steps on a new dataset: load the file, report missing values, choose and apply cleaning functions, and produce the summary tables and charts. A human should still review the cleaning choices, because they affect bias, and the agent's decisions should be logged so the work stays reproducible. The functions in this notebook already have clear inputs, outputs and docstrings, which makes them usable as tools for an agent.
