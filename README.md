# Personal Agentic AI Chatbot 🤖

A Personal Agentic AI Chatbot built using **LangGraph, LangChain, FastAPI, Streamlit, and Groq LLMs**.

This project demonstrates how to build an AI-powered conversational agent with custom system prompts, multiple LLM providers, optional web search, and conversation context management.

## 1. Project Overview

The main objective of this project is to develop a personal AI chatbot using Agentic AI concepts and integrate it with a frontend and backend architecture.

**Application Workflow:**

Streamlit Frontend → FastAPI Backend → LangGraph Agent → LLM / Tools → AI Response

## 2. Technologies Used

- **Programming Language:** Python
- **AI Agent Framework:** LangGraph
- **LLM Framework:** LangChain
- **LLM Providers:** Groq and OpenAI
- **Backend:** FastAPI
- **Frontend:** Streamlit
- **Web Search:** Tavily Search API
- **Data Validation:** Pydantic
- **Environment Management:** Python Virtual Environment
- **Environment Variables:** python-dotenv

## 3. Prerequisites

Before starting the project, make sure you have:

- Python installed on your system.
- Visual Studio Code installed.
- An internet connection.
- Your own API keys.

### Required API Keys

Create your own API keys from the respective platforms:

1. **Groq API Key:** For accessing Groq LLM models.
2. **Tavily API Key:** For enabling web search functionality.
3. **OpenAI API Key:** For accessing OpenAI models.

You can use the providers according to your requirements. Web search is optional.

**Important:** Always use your own API keys. Never share your API credentials or upload your actual `.env` file to GitHub.

## 4. Step-by-Step Project Setup

### Step 1: Create a Project Folder

- Create a new folder for your project.
- Open the folder in Visual Studio Code.
- Open the integrated terminal.

### Step 2: Create the Project Files

Inside your project folder, create the following three Python files:

```text
ai_agent.py
backend.py
frontend.py
```

Also create a `.env` file to store your API keys.

### Step 3: Create a Python Virtual Environment

Create a virtual environment:

```bash
python3 -m venv .venv
```

Activate the virtual environment on macOS/Linux:

```bash
source .venv/bin/activate
```

### Step 4: Install Required Dependencies

Install the required Python libraries:

```bash
pip install langgraph langchain langchain-core langchain-groq langchain-openai langchain-community
```

```bash
pip install fastapi uvicorn streamlit
```

```bash
pip install python-dotenv requests tavily-python
```

### Step 5: Configure Environment Variables

Open your `.env` file and add your own API keys:

```env
GROQ_API_KEY=your_groq_api_key
TAVILY_API_KEY=your_tavily_api_key
OPENAI_API_KEY=your_openai_api_key
```

The application uses `python-dotenv` to load these environment variables.

**Security:** Do not upload your real `.env` file to GitHub.

## 5. Step-by-Step Development Process

### Step 1: Building the AI Agent (`ai_agent.py`)

Developed the AI agent using LangGraph and LangChain.

Implemented:

1. Groq and OpenAI LLM integration.
2. Dynamic LLM provider and model selection.
3. Custom system prompt support.
4. Tavily Search tool integration.
5. ReAct agent using LangGraph's `create_react_agent`.
6. User and assistant message handling.
7. AI response generation through the agent.

### Step 2: Building the FastAPI Backend (`backend.py`)

Developed the backend using FastAPI.

Implemented:

1. Created the FastAPI application.
2. Defined Pydantic request models.
3. Created a `ChatMessage` model with `role` and `content` fields.
4. Created a `RequestState` model to validate incoming requests.
5. Implemented the `POST /chat` endpoint.
6. Added model-name validation.
7. Connected the API with the LangGraph agent.
8. Returned the generated AI response to the frontend.

**Backend URL:**

```text
http://127.0.0.1:9999
```

**API Documentation:**

```text
http://127.0.0.1:9999/docs
```

### Step 3: Building the Streamlit Frontend (`frontend.py`)

Developed an interactive chatbot interface using Streamlit.

Implemented:

1. Chatbot user interface.
2. Custom system prompt input.
3. LLM provider selection (Groq / OpenAI).
4. Model selection based on the selected provider.
5. Optional web search checkbox.
6. User query input area.
7. FastAPI integration using Python `requests`.
8. Chat-style message display.
9. Conversation history management using `st.session_state`.

### Step 4: Integrating Frontend and Backend

Connected Streamlit with FastAPI through HTTP POST requests.

The frontend sends:

- Model name
- LLM provider
- System prompt
- Conversation messages
- Web search permission

The backend validates the request and passes it to the LangGraph agent.

The generated response is returned to the frontend and displayed in the chat interface.

### Step 5: Implementing Conversation History and Context Memory

This was an important improvement to make the chatbot capable of handling contextual conversations.

**A. Conversation History Using Streamlit Session State**

Implemented `st.session_state.chat_history` to maintain conversation history during the Streamlit session.

Key functionality:

- Stores user messages.
- Stores assistant responses.
- Displays previous messages in the chat interface.
- Maintains conversation history while the user continues interacting in the same session.

**B. Pydantic Message Schema in FastAPI**

Updated the backend request structure using a `ChatMessage` model:

- `role`: Identifies whether the message is from the user or assistant.
- `content`: Stores the actual message text.

Used `List[ChatMessage]` in the request model to receive multiple conversation messages.

**C. LangChain Message Conversion**

Converted incoming conversation messages into LangChain message objects:

- User messages → `HumanMessage`
- Assistant messages → `AIMessage`

These messages are passed to the LangGraph agent as conversation state.

**D. Context-Aware Responses**

The agent receives previous conversation messages along with the latest query.

For example:

**User:** My name is Rahul and I am learning Machine Learning.

**User:** Can you suggest what I should learn next?

The chatbot can use the earlier conversation as context while generating its response.

**Result:** Successfully implemented conversation history and contextual message handling across the Streamlit, FastAPI, and LangGraph components.

**Note:** The current conversation history is maintained using Streamlit session state and passed to the agent. Persistent SQLite Checkpointer-based memory has not been implemented yet.

## 6. How to Run the Application

After completing the setup and development process, follow these steps to run the chatbot.

You need to open **two separate terminals in Visual Studio Code**.

### Terminal 1 – Run the FastAPI Backend

1. Open the first terminal in VS Code.
2. Activate the virtual environment.
3. Run the backend file.

```bash
source .venv/bin/activate
python backend.py
```

The FastAPI backend will start at:

```text
http://127.0.0.1:9999
```

### Terminal 2 – Run the Streamlit Frontend

1. Open a second terminal in VS Code by clicking the `+` icon in the terminal panel.
2. Activate the virtual environment in the second terminal as well.
3. Run the frontend file.

```bash
source .venv/bin/activate
streamlit run frontend.py
```

The Streamlit application will start at:

```text
http://localhost:8501
```

Open this URL in your browser to interact with your AI chatbot.

**Important:**

- Keep Terminal 1 running because it manages the FastAPI backend.
- Keep Terminal 2 running because it manages the Streamlit frontend.
- Both terminals must remain active while using the application.
- Press `Ctrl + C` in each terminal when you want to stop the application.

## 7. Key Features

- Interactive AI chatbot interface.
- Multiple LLM provider support.
- Groq and OpenAI integration.
- Custom system prompt configuration.
- LangGraph ReAct agent architecture.
- Optional web search using Tavily.
- Conversation history management.
- Context-aware follow-up conversations.
- FastAPI-based backend.
- Streamlit-based frontend.
- Modular application architecture.

## 8. Application Architecture

```text
              USER
                |
                v
       STREAMLIT FRONTEND
          frontend.py
                |
                v
       CONVERSATION HISTORY
          st.session_state
                |
                v
          FASTAPI API
          backend.py
                |
                v
        LANGGRAPH AGENT
          ai_agent.py
                |
          +-----+-----+
          |           |
          v           v
       GROQ /       TAVILY
       OPENAI       SEARCH
          |           |
          +-----+-----+
                |
                v
         AI GENERATED RESPONSE
                |
                v
        STREAMLIT CHAT UI
```

## 9. Future Improvements

- SQLite Checkpointer for persistent conversation state.
- Thread ID-based conversation management.
- Multiple independent chat sessions.
- Long-term memory integration.
- Advanced tool-calling capabilities.
- Multi-agent architecture using LangGraph.
- Improved user interface.
- Deployment with a publicly accessible URL.

## 10. Learning Outcomes

Through this project, I gained practical experience in:

- Building AI agents using LangGraph.
- Understanding the ReAct agent architecture.
- Integrating LLMs using LangChain.
- Implementing tool calling and web search.
- Developing REST APIs using FastAPI.
- Building interactive applications using Streamlit.
- Managing conversation history using session state.
- Passing contextual messages to LangGraph agents.
- Connecting frontend, backend, and AI models.

## 11. Author

Developed as a hands-on project to explore and implement **Agentic AI, LangGraph, LangChain, and LLM-powered applications**.

⭐ If you find this project useful, feel free to star the repository!
