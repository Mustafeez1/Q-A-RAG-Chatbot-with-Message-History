Conversational RAG Chatbot

A Conversational Retrieval-Augmented Generation (RAG) chatbot built
with LangChain, Groq, Ollama, and Chroma.

This project loads product information from a Flipkart product page,
splits the webpage content into smaller chunks, creates embeddings using
Ollama's nomic-embed-text model, stores them in Chroma, and retrieves
relevant information to answer user questions.

The project also demonstrates history-aware retrieval, allowing the
chatbot to use previous conversation context when answering follow-up
questions.

Features

Load webpage content using WebBaseLoader

Split documents using RecursiveCharacterTextSplitter

Generate local embeddings with Ollama nomic-embed-text

Store and retrieve document embeddings with Chroma

Use Groq's openai/gpt-oss-120b model for question answering

Build a basic RAG pipeline with LangChain

Build a history-aware retriever for conversational questions

Maintain chat history using LangChain message objects

Answer questions using retrieved webpage context

Project Workflow

Flipkart Product Webpage
        ↓
WebBaseLoader
        ↓
Document Loading
        ↓
RecursiveCharacterTextSplitter
        ↓
Document Chunks
        ↓
Ollama Embeddings
(nomic-embed-text)
        ↓
Chroma Vector Store
        ↓
Retriever
        ↓
History-Aware Retriever
        ↓
Question Answering Chain
        ↓
Groq LLM
(openai/gpt-oss-120b)
        ↓
Final Answer

Technologies Used

Python

LangChain

LangChain Classic

Groq

Ollama

Chroma

python-dotenv

Main Components

Component                           Purpose

WebBaseLoader                     Loads content from the product
webpage

RecursiveCharacterTextSplitter    Splits the document into chunks

OllamaEmbeddings                  Creates embeddings locally

Chroma                            Stores document embeddings

create_history_aware_retriever    Rewrites follow-up questions using
chat history

create_stuff_documents_chain      Combines retrieved documents with
the question-answer prompt

create_retrieval_chain            Connects retrieval and answer
generation

Installation

Create and activate a Python environment, then install the required
packages:

pip install langchain-groq langchain-core langchain-community langchain-text-splitters langchain-chroma langchain-ollama langchain-classic python-dotenv beautifulsoup4

Make sure Ollama is installed and running on your computer.

Pull the embedding model:

ollama pull nomic-embed-text

Environment Variables

Create a .env file in the project directory:

GROQ_API_KEY=your_groq_api_key

Do not commit your .env file or API keys to GitHub.

Add this to .gitignore:

.env
__pycache__/
.ipynb_checkpoints/

Running the Project

Open the notebook:

rag_chat.ipynb

Run the cells in order.

The notebook first creates a basic RAG pipeline:

question_answer_chain = create_stuff_documents_chain(llm, prompt)

rag_chain = create_retrieval_chain(
    retriever,
    question_answer_chain
)

A question can then be passed to the chain:

response = rag_chain.invoke({
    "input": "what is the price of iphone 17?"
})

print(response["answer"])

Conversational RAG

The second part of the notebook extends the basic RAG system with
conversation history.

A history-aware retriever is created using:

history_aware_retriever = create_history_aware_retriever(
    llm,
    retriever,
    context_prompt
)

The final conversational RAG chain is:

question_answer_chain = create_stuff_documents_chain(
    llm,
    qa_prompt
)

rag_chain = create_retrieval_chain(
    history_aware_retriever,
    question_answer_chain
)

Chat history is maintained using:

chat_history = []

and LangChain message objects:

chat_history.extend([
    HumanMessage(content=question),
    AIMessage(content=response["answer"])
])

A follow-up question can then be passed together with the history:

result = rag_chain.invoke({
    "input": question,
    "chat_history": chat_history
})

Example Questions

The notebook demonstrates questions such as:

What is the price of iPhone 17?

In what colours is it available?

and conversational questions such as:

What query I asked you before?

The history-aware section is designed to handle questions that refer to
previous conversation context.

Project Structure

BuildingChatbot(M31)/
│
├── rag_chat.ipynb
├── .env
└── README.md

Keep .env local and exclude it from GitHub using .gitignore.

Learning Outcomes

Through this project, I practiced:

Document loading

Text splitting

Vector embeddings

Vector databases

Semantic retrieval

Retrieval-Augmented Generation

Prompt templates

LangChain chains

Conversational memory using chat history

History-aware retrieval

Integrating a hosted LLM with a local embedding model

Future Improvements

Possible extensions include:

Add a user interface using Streamlit

Support multiple webpages or documents

Add persistent Chroma storage

Improve prompt instructions

Add source citations to responses

Store chat history by session

Add more robust error handling

Allow users to upload their own documents

Author

Khan

This project was created as part of my Generative AI and LangChain
learning journey.