# LANGCHAIN
Doc info
Talk to your Data Base with GPT models using LangChain 🦜🔗! (CSV)
Rubentak
Rubentak
https://medium.com/@rubentak/talk-to-your-data-base-with-gpt-models-using-langchain-csv-19e2b32aa729  ORIG DOC
·
Follow

4 min read
·
May 24
5


1




In this short article, I will show you how you can use a Large Language Model (LLM) to ask questions about your personal CSV.


Linking OpenAI to your Database. Drawing: 
MalachkovaK
In my former article, I explain the basic principles of LangChain, how it can be used, and some common use cases. I also showed how to give the OpenAI GPT model Google search access. The article can be found here:

Give OpenAI models internet access using LangChain🦜🔗
Taking AI to the next level
medium.com

In this article, I will:

Show how to set an environment
Read in a CSV file
Create a Q&A chain that can answer questions about your file.
Use cases
There are several use cases where you would want to be able to talk to your database:

Data Analysis and Exploration: In the case of big files with thousands of rows, it could be nice to do an exploratory analysis of the data. This can be done using LLMs like open AI’s GPT4 if connected using LangChain. By asking questions about the database, you can create a general understanding of what data you are working with.
Natural Language Interface: By connecting the language model to your CSV files, you can create a natural language interface to interact with the data. This is particularly handy for people who do not know a lot about coding or data processing procedures. By simply asking the questions in English instead of the command line, one can receive relevant responses from the model.
Data Validation and Cleaning: The language model can assist in validating and cleaning your CSVs data. You can provide the model with specific rules or criteria to check against the data. For example, you could ask the model to identify and flag any inconsistent or incorrect entries in the CSV file, saving you time in the data cleaning process.
Personal Assistant: Connect the language model to your personal CSV files and create your own chatbot for your data.
Setting up LangChain
In this part, I will show you how you can easily set up a Python environment in which you can start talking to your database. The code can be found in my GitHub repository.

To start off with, you need to install the LangChain and OpenAI packages in your virtual environment.

!pip install -q langchain openai os
After this, we import the necessary libraries into our Notebook

from langchain.document_loaders import CSVLoader
from langchain.indexes import VectorstoreIndexCreator
from langchain.chains import RetrievalQA
from langchain.llms import OpenAI
import os
After importing the libraries, we need to create and set up a new OpenAI API key. If you do not have an OpenAI API key, you can create one here. Import this key in the following way:

os.environ["OPENAI_API_KEY"] = "sk-..."
Read in a CSV file
Now, we can load the ‘tracklist’ CSV file that you want to ask questions about. In this article, I will use the CSV file that I created in my article about preprocessing your Spotify data. If you are interested in getting the same data set, you can read more about it here:

Spotify in Python Part 1: Preprocessing
In this article, we will get started getting your Spotify data into Python.
medium.com

This CSV file contains data about songs in my saved songs playlists in Spotify.

# Load the documents
loader = CSVLoader(file_path='../data/tracklist.csv')
Now let's create indexes for the loaded CSV file. This will make it easier for the LLM to process the requests.

index_creator = VectorstoreIndexCreator()
docsearch = index_creator.from_loaders([loader])
Create a Q&A chain
Now, we can create a Q&A chain that will be able to answer questions about the CSV file. For the LLM I will use the OpenAI default model: 3.5-turbo. The chain type that we will use is the “stuff” chain. More about different chain types here.

chain = RetrievalQA.from_chain_type(llm=OpenAI(), chain_type="stuff", retriever=docsearch.vectorstore.as_retriever(), input_key="question")
After defining the Q&A chain, we can test if it works.

# Pass a query to the chain
query = "Do you have a column called tempo?"
response = chain({"question": query})
print(response['result'])
Output: Yes, there is a column called tempo.

This is great, it worked! This however is not tet the most beautiful way to use this tool in python. Let's wrap it into a function!

query = "What are all the columns in this file?"
def ask_question(query):
    response = chain({"question": query})
    return response['result']
print(ask_question(query))
Output:

added_at, id, name, popularity, uri, artist, album, release_date, duration_ms, length, danceability, acousticness, energy, instrumentalness, liveness, loudness, speechiness, tempo, time_signature, valence, mode, key, genres, and genre_group.

Continue the conversation yourself!

query = "...?"
def ask_question(query):
    response = chain({"question": query})
    return response['result']
ask_question(query)
End note
There is a lot more you can do with LangChain and I would recommend playing around with it.

I hope this article helps to create a general understanding of how can interact with different data sources in Python. Thank you for reading, if you liked the article please leave a clap 👏 and leave a command if you have any feedback or thoughts that you would like to share!


LangChain. Drawing: 
MalachkovaK
Illustration:

The wonderful illustration is made by Kateryna Malachkova. Be sure to check out her Medium and Instagram!

https://medium.com/@malachkovak
https://instagram.com/ph_malachkova?igshid=ZjE2NGZiNDQ=
Also, be sure to follow me on GitHub LinkedIn and if you like the work that I’m doing you could buy me a coffee:

https://github.com/rubentak
https://www.linkedin.com/in/ruben-tak-665b66194/
https://www.buymeacoffee.com/rubentak
