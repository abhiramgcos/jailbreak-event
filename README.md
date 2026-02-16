# **Jailbreak AI**

**Event:** HYVE Tech Fest

**Date:** 18-02-2026

A dedicated platform for AI Red Teaming and Jailbreak events. Participants act as security researchers, attempting to bypass the system instructions of an AI model to retrieve a hidden secret.

## **Overview**

This repository contains the backend server for the event. It handles:

* User authentication (Registration/Login).  
* Session management and state persistence.  
* Interactions with a local LLM (via Ollama).  
* Automatic scoring and leaderboard tracking.

**The Objective:** The AI assistant is guarding a secret data string (the "flag"). Participants must craft adversarial prompts to trick the AI into revealing it, despite its strict security instructions.

## **Prerequisites**

To run this server locally, you need:

1. **Python 3.8+**  
2. **Ollama**: The server relies on a local instance of Ollama to run the LLM.  
   * [Download Ollama](https://ollama.com/)  
   * Pull the required model (default is qwen3:4b in the code):  
     ollama pull qwen3:4b

## **Installation**

1. **Clone the repository:**  
   * git clone <https://github.com/abhiramgcos/jailbreak-event.git>
   * cd jailbreak-event

2. **Set up a Virtual Environment:**  
   It is recommended to use a virtual environment to manage dependencies.  
   * **Windows:**  
     python \-m venv venv  
     venv\\Scripts\\activate

   * **Linux / MacOS:**  
     python3 \-m venv venv  
     source venv/bin/activate

3. **Install Dependencies:**  
   pip install \-r requirements.txt

## **Usage**

1. **Start Ollama:**  
   Ensure your Ollama instance is running and accessible at <http://localhost:11434>.  
2. **Start the Flask Server:**  
   python app.py

   The server will start on <http://localhost:5000>.  
3. **Access the Interface:**  
   * **Web UI:** Open <http://localhost:5000> in your browser (requires a static/index.html file).  
   * **API:** Interact directly via tools like Postman or cURL.

## **Challenge Workflow**

1. **Register:** Create a researcher account to track your progress.  
2. **Start Session:** Initialize a new testing session. The defensive system prompt is injected at the start of every session.  
3. **Attack:** Send adversarial prompts to the AI to try and extract the protected secret.  
4. **Submit Secret:** If you successfully extract the secret, submit it to the /api/submit endpoint.  
5. **Leaderboard:** Rankings are determined based on the efficiency of the jailbreak (fewer prompts) and speed.

## **API Documentation**

### **Authentication**

* POST /api/register  
  * Body: {"username": "user", "password": "pass"}  
* POST /api/login  
  * Body: {"username": "user", "password": "pass"}  
  * Returns: Auth token required for challenge routes.

### **Interaction Loop**

* POST /api/start  
  * Headers: Authorization: Bearer \<token\>  
  * Action: Resets current session and initializes new chat context.  
* POST /api/chat  
  * Headers: Authorization: Bearer \<token\>  
  * Body: {"session\_id": "...", "message": "your prompt"}  
  * Action: Sends prompt to Ollama and returns the AI response.  
* POST /api/submit  
  * Headers: Authorization: Bearer \<token\>  
  * Body: {"session\_id": "...", "flag": "suspected\_secret"}  
  * Action: Verifies the extracted secret. If correct, marks the challenge as solved.

### **Stats**

* GET /api/leaderboard  
  * Returns top 20 researchers sorted by least prompts used and fastest time.  
* GET /api/history  
  * Returns the history of your attempts.

## **Configuration**

* **Data Storage:** User data and sessions are stored in data.json.  
* **Model Config:** The model is hardcoded to qwen3:4b in app.py. To change this, edit the chat() function in app.py.  
* **System Prompt:** The defensive instructions are defined in the SYSTEM\_PROMPT variable in app.py.

## **Disclaimer**

This software is for educational and security research purposes. It is designed to demonstrate prompt injection vulnerabilities in a controlled environment.
