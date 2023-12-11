# Creating a Sensemaking Data Pipeline
### In this project, you will use the Python urllib library to pull data from MIT’s course catalog. The data being pulled from the web will be unstructured, so you will have to clean it first to extract the course names. Once the course names are extracted and the data is structured, you will perform some data analysis to learn how many times each word occurs throughout all of the course names. This analysis will be saved as a JSON file, which will then be referenced by your D3 web application. Lastly, the web application will generate a visual analysis of the JSON data that you just collected. To streamline the data analysis process, you will use software tools that you have learned about in previous modules, such as Docker and Airflow. You will be responsible for instantiating an Airflow web server within a Docker container to handle each Python task that you define. These tasks will divide the project into smaller, more manageable pieces. In the end, you will experiment with different D3 libraries to customize your project.

Part 1: Code Development
1.	Create a folder titled project-23. Place the code visualization folder within the project-23 folder, and create another empty folder titled airflow-docker. Then, create a new assignment.py file, which will contain your coding throughout the project. Provide a screenshot of the project-23 folder with the code visualization folder, airflow-docker folder, and assignment.py file within it.
 <img width="208" alt="image" src="https://github.com/sandip86/Creating-a-Sensemaking-Data-Pipeline/assets/153111110/1c581943-4eb4-4d92-b8b0-8f57256fb5e6">

2.	Open the assignment.py file using VS Code. First, you will need to import all of the libraries that you will be using. Import the following libraries:
i.	The DAG object (needed to instantiate a DAG)
ii.	From airflow, import DAG.
iii.	From datetime, import timedelta.
iv.	Operators (needed to operate)
v.	From airflow.operators.bash, import BashOperator.
vi.	From airflow.utils.dates, import days_ago.
vii.	From airflow.operators.python, import PythonOperator.
viii.	Task Functions (used to facilitate tasks)
ix.	Import urllib.request.
x.	Import time.
xi.	Import glob, os.
xii.	Import json.
xiii.	Provide a screenshot to show that you have imported the DAG object, the operators, and all of the necessary task functions into the assignment.py file.
 
3.	Generate your first task. This task will be a Python function named catalog()that accepts no arguments. Inside of the catalog()function, define two helper functions named pull(url)and store(data, file).
i.	The pull(url)function accepts, as an argument, a URL of type string. This function will execute the code provided in lines 6 and 7 in the code_starter/01_pull.py file and will return the data variable.
ii.	The store(data, file)function accepts two arguments: a filename of type string and the data that was returned by the pull()function. The store()method will execute the following:
b.	Create and open a file named after the URL where the data came from (example: m1a.html).
c.	Write the data to the file.
d.	Close the file.
i.	Below, you are provided with the pseudocode to define the catalog()function and the pull(url)and store(data, file)helper functions. Note: This is just pseudocode; you will need to complete it with the correct syntax:
ii.	# pull course catalog pages
def catalog():
iii.	#define pull(url) helper function
    def pull(url):
      
        return data
         
    #define store(data,file) helper function
    def store(data,file):
 
        print('wrote file: ' + file)
iv.	Provide a screenshot of the pull(url)and store(data, file)helper functions defined inside of the catalog()task.
 
4.	Once you have defined these two helper functions, you need to write some code within the catalog()method to utilize them. Create a list titled urls that contains the working URLs in the 00_urls.txt file as strings. Use a for loop to iterate through the urls list. First, the loop will call the pull(url)function to obtain the data. Then, the loop will call the store(data, file)function to store the data. Use the pseudocode provided below to define the for loop. Note: This is just pseudocode; you will need to complete it with the correct syntax:
i.	index = url.rfind('/') + 1
#call pull function
ii.	file = url[index:]
#call store function
iii.	print('pulled: ' + file)
print('--- waiting ---')
time.sleep(15)
iv.	Provide a screenshot of the entire catalog()function, including the urls list and the for loop that you just implemented.
 
5.	Continue coding within the assignment.py file. Create the second task in the Airflow pipeline called combine(). The combine()task will combine all of the unstructured data files into one large file. Use the pseudocode below to combine the files. Note: This is just pseudocode; you will need to complete it with the correct syntax:
i.	open('combo.txt') as outfile:
       for file in glob.glob("*.html"):
           open(file) as infile:
               outfile.write(infile.read())
ii.	Provide a screenshot of the combine()method with the correct code to combine the files.
 
6.	Continue coding within the assignment.py file. Create the third task method, called titles(). This function will utilize the BeautifulSoup4 Links to an external site.library, which enables you to scrape web pages.
i.	The titles()function will take no arguments. This function imports the BeautifulSoup4 library and contains one helper function, store_json(), to store the resulting JSON file. Copy the code below under the definition of the titles()function to import the BeautifulSoup4 library and define the store_json()function.
ii.	from bs4 import BeautifulSoup
def store_json(data,file):
       with open(file, 'w', encoding='utf-8') as f:
           json.dump(data, f, ensure_ascii=False, indent=4)
           print('wrote file: ' + file)
iii.	Next, you will use the BeautifulSoup4 library to inspect the HTML page and gather the information within the h3 element of the page where the course titles are found. You will then place the result in a JSON file. Complete the pseudocode for the titles()function below to open and read the HTML file generated by the combine()function.
iv.	def titles():
      from bs4 import BeautifulSoup
      def store_json(data,file):
           with open(file, 'w', encoding='utf-8') as f:
               json.dump(data, f, ensure_ascii=False, indent=4)
               print('wrote file: ' + file)
v.	#Open and read the large html file generated by combine()
vi.	
   #the following replaces new line and carriage return char
   html = html.replace('\n', ' ').replace('\r', '')
   #the following create an html parser
   soup = BeautifulSoup(html, "html.parser")
   results = soup.find_all('h3')
   titles = []
vii.	# tag inner text
   for item in results:
       titles.append(item.text)
   store_json(titles, 'titles.json')
viii.	Provide a screenshot of the completed titles()method with the correct code to open and read the HTML file generated by the combine()function.
 
7.	Continue coding within the assignment.py file. The next task method will remove all punctuation, numbers, and one-character words from the titles.json file. This task method, clean(), takes no arguments and uses the same helper function, store_json(data,file), that you used in the title()function defined in the previous step. Below is the pseudocode for the clean()method. Implement any needed commands to complete the pseudocode:
i.	def clean():
   #complete helper function definition below
   def store_json(data,file):
      .. 
   with open(titles.json) as file:
       titles = json.load(file)
       # remove punctuation/numbers
       for index, title in enumerate(titles):
           punctuation= '''!()-[]{};:'"\,<>./?@#$%^&*_~1234567890'''
           translationTable= str.maketrans("","",punctuation)
           clean = title.translate(translationTable)
           titles[index] = clean
1.	# remove one character words
       for index, title in enumerate(titles):
           clean = ' '.join( [word for word in title.split() if len(word)>1] )
           titles[index] = clean
2.	store_json(titles, 'titles_clean.json')
ii.	Provide a screenshot of the fully implemented clean()method with the correct code to remove all punctuation, numbers, and one-character words from the titles.json file.
 

8.	Continue coding within the assignment.py file. The final task method you will define is count_words(). The count_words()method accepts no arguments and uses the helper function store_json(data,file)that was used in the previous two tasks to save the resulting JSON file. The pseudocode for the count_words()method is below. Implement any needed commands to complete the pseudocode:
i.	def count_words():
     from collections import Counter
     def store_json(data,file):
           ..
     with open(titles_clean.json) as file:
            titles = json.load(file)
            words = []
a.	# extract words and flatten
            for title in titles:
                words.extend(title.split())
b.	# count word frequency
            counts = Counter(words)
            store_json(counts, 'words.json')
ii.	Provide a screenshot of the completed count_words()method with the correct code to call the store_json(data,file)helper function.
 


9.	Continue coding within the assignment.py file. Because this Python file will be run using Airflow, you need to design an Airflow pipeline as the final part to this file. The first step is to define the DAG. Then, define each task, from t0 to t5, for a total of six tasks. The first task, t0, will be a bash command to install the BeautifulSoup4 library within the Docker environment. The next five tasks will each call one Python function that you have defined in the previous steps.
i.	Below, you are provided with some code to get started. The t0 task is already defined for you. The t1 task is also already defined for you, and it executes the catalog()function in your DAG. Complete the code below to define the remaining four tasks:
ii.	with DAG(
   "assignment",
   start_date=days_ago(1),
   schedule_interval="@daily",catchup=False,
) as dag:
iii.	# INSTALL BS4 BY HAND THEN CALL FUNCTION
iv.	# ts are tasks
   t0 = BashOperator(
       task_id='task_zero',
       bash_command='pip install beautifulsoup4',
       retries=2
   )
   t1 = PythonOperator(
       task_id='task_one',
       depends_on_past=False,
       python_callable=catalog
   )
   #define tasks from t2 to t5 below
v.	
   t0>>t1>>t2>>t3>>t4>>t5
vi.	Provide a screenshot of the DAG declaration with all six tasks, from t0 to t5, correctly defined.
![image](https://github.com/sandip86/Creating-a-Sensemaking-Data-Pipeline/assets/153111110/913070f5-506e-4a65-a02c-32bb52b6d3e5)

<img width="382" alt="image" src="https://github.com/sandip86/Creating-a-Sensemaking-Data-Pipeline/assets/153111110/7fce929d-16ba-4e95-9b0e-11a394cc0895">

