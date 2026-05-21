# ✈️ TravelGPT

AI-Powered Smart Travel Planner using OpenAI, LangChain, and Streamlit

---

# 📌 Overview

TravelGPT is an AI-powered travel planning assistant that helps users generate personalized travel itineraries through natural conversation.

Users can:

- plan trips
- get itinerary suggestions
- estimate budgets
- receive hotel/activity recommendations
- generate travel plans instantly

The project uses:

- OpenAI GPT
- LangChain
- Streamlit
- Conversational Memory

---

# 🚀 Features

✅ Conversational AI Chatbot  
✅ Personalized Itinerary Generation  
✅ Budget Estimation  
✅ Travel Recommendations  
✅ Conversation Memory  
✅ Streamlit Chat UI  
✅ Prompt Engineering  

---

# 🏗️ Architecture

```text
User Input
    ↓
Streamlit Chat UI
    ↓
Conversation Memory
    ↓
Prompt Engineering
    ↓
OpenAI GPT Model
    ↓
Travel Itinerary Response
```

---

# 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| Python | Backend |
| Streamlit | Frontend |
| OpenAI GPT | LLM |
| LangChain | LLM orchestration |
| dotenv | Environment variables |

---

# 📂 Project Structure

```text
TravelGPT/
│
├── app.py
├── requirements.txt
├── .env
├── README.md
│
├── prompts/
│   └── travel_prompt.py
```

---

# ⚙️ Installation

## 1️⃣ Clone Repository

```bash
git clone <your_repo_url>
cd TravelGPT
```

---

## 2️⃣ Create Virtual Environment

### Windows

```bash
python -m venv venv
venv\Scripts\activate
```

### Mac/Linux

```bash
python3 -m venv venv
source venv/bin/activate
```

---

## 3️⃣ Install Dependencies

```bash
pip install -r requirements.txt
```

---

# 📦 requirements.txt

```txt
streamlit
openai
langchain
langchain-openai
python-dotenv
```

---

# 🔑 Environment Variables

Create a `.env` file in the root directory.

```env
OPENAI_API_KEY=your_openai_api_key
```

Get your API key from:

https://platform.openai.com/api-keys

---

# 🧠 Prompt Engineering

## `prompts/travel_prompt.py`

```python
SYSTEM_PROMPT = """
You are an AI Travel Planner.

Your responsibilities:
- Create personalized travel itineraries
- Suggest attractions
- Recommend activities
- Estimate budgets
- Recommend hotels
- Suggest local experiences

Rules:
1. Keep plans realistic
2. Respect the user's budget
3. Ask follow-up questions if needed
4. Generate day-wise plans
5. Include approximate costs
6. Keep responses well formatted
"""
```

---

# 💻 Main Application Code

## `app.py`

```python
import streamlit as st
from dotenv import load_dotenv
import os

from langchain_openai import ChatOpenAI

from langchain.schema import (
    SystemMessage,
    HumanMessage,
    AIMessage
)

from prompts.travel_prompt import SYSTEM_PROMPT

# -----------------------------------
# Load Environment Variables
# -----------------------------------
load_dotenv()

OPENAI_API_KEY = os.getenv("OPENAI_API_KEY")

# -----------------------------------
# Streamlit Configuration
# -----------------------------------
st.set_page_config(
    page_title="TravelGPT",
    page_icon="✈️"
)

st.title("✈️ TravelGPT")

st.write("AI-Powered Smart Travel Planner")

# -----------------------------------
# Initialize LLM
# -----------------------------------
llm = ChatOpenAI(
    model="gpt-3.5-turbo",
    temperature=0.7
)

# -----------------------------------
# Session State for Memory
# -----------------------------------
if "messages" not in st.session_state:

    st.session_state.messages = [
        SystemMessage(content=SYSTEM_PROMPT)
    ]

# -----------------------------------
# Display Chat History
# -----------------------------------
for message in st.session_state.messages:

    if isinstance(message, HumanMessage):

        with st.chat_message("user"):
            st.markdown(message.content)

    elif isinstance(message, AIMessage):

        with st.chat_message("assistant"):
            st.markdown(message.content)

# -----------------------------------
# User Input
# -----------------------------------
user_input = st.chat_input(
    "Where would you like to travel?"
)

if user_input:

    # Display User Message
    with st.chat_message("user"):
        st.markdown(user_input)

    # Save User Message
    st.session_state.messages.append(
        HumanMessage(content=user_input)
    )

    # Generate AI Response
    with st.spinner("Planning your trip..."):

        response = llm.invoke(
            st.session_state.messages
        )

    # Display AI Response
    with st.chat_message("assistant"):
        st.markdown(response.content)

    # Save AI Response
    st.session_state.messages.append(
        AIMessage(content=response.content)
    )
```

---

# ▶️ Run Application

```bash
streamlit run app.py
```

---

# 💬 Example Conversation

## User

```text
Plan a 5-day Goa trip under 30k
```

---

## AI

```text
Sure! Before I create your itinerary:

1. Solo or group trip?
2. Adventure or relaxation?
3. Preferred hotel type?
4. Any nightlife preferences?
```

---

## User

```text
Group trip, adventure, budget hotel
```

---

## AI Response

```text
Day 1:
Arrival + Baga Beach

Day 2:
Scuba Diving + Fort Aguada

Day 3:
Water Sports + Club Night

Day 4:
Dudhsagar Waterfalls

Day 5:
Shopping + Departure

Estimated Budget:
Hotel: 8k
Food: 5k
Transport: 7k
Activities: 8k
```

---

# 🌟 Future Enhancements

## ✅ Weather API Integration

Add:
- weather forecast
- best travel time
- clothing suggestions

---

## ✅ Google Maps Integration

Display:
- routes
- nearby attractions
- travel distance

---

## ✅ PDF Export

Generate downloadable travel itinerary PDF.

---

## ✅ Hotel Recommendation System

Suggest hotels based on:
- budget
- ratings
- location

---

## ✅ Voice Assistant

Speech-to-text travel planning assistant.

---

# 🎯 Hackathon Advantages

This project demonstrates:

- conversational AI
- prompt engineering
- memory handling
- AI workflow orchestration
- real-world use case
- clean Streamlit UI

---

# 📈 Resume Points

- Built AI-powered travel planning assistant using OpenAI and Streamlit.
- Implemented conversational memory and itinerary generation workflows.
- Designed dynamic budget-aware travel recommendation engine.
- Developed scalable LLM-based planning assistant with prompt orchestration.

---

# 👨‍💻 Author

Your Name

---