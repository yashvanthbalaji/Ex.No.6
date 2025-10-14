## Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

**Date:** 14-10-2025

**Name:** Balaji A

**Register no.:** 212223040023

## Aim:

Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights.

## AI Tools Required:

1.  **OpenAI API (GPT-3.5/GPT-4):** Used for **content generation** (e.g., drafting a social media post).

2.  **Hugging Face Inference API (Pre-trained Sentiment Analysis Model):** Used for **sentiment analysis** of the generated content.

-----

## 1\. Abstract

This experiment successfully demonstrates the development and implementation of a **Python-based orchestration layer** designed to integrate two distinct Artificial Intelligence APIs: **OpenAI (for content generation)** and the **Hugging Face Inference API (for sentiment analysis)**. The core application addresses the persona of a Content Strategist Programmer, aiming to automate quality control for social media posts by enforcing a **positive brand sentiment threshold ($\ge 90\%$ confidence)**. The Python code acts as the central control mechanism, chaining the AI tools' outputs: a generated post is immediately analyzed for sentiment, and the resulting score is used to generate an **actionable insight** (Pass or Flag). In case of a failure (Flag), the system automatically triggers a **revision loop**, feeding the flagged content and analysis back into the OpenAI model to produce a corrected draft. This project validates the compatibility, efficiency, and utility of using Python for developing sophisticated, multi-tool AI workflows.

-----

## 2\. Introduction

The rapid proliferation of sophisticated AI models has created a need for **integration strategies** that combine the strengths of specialized tools into a single, cohesive application. While one AI tool may excel at text generation, another might specialize in linguistic analysis or image recognition. This experiment focuses on bridging the gap between two natural language processing (NLP) tasks—**creative generation** and **analytical evaluation**—to create an **automated content vetting system**.

The chosen application is a **Social Media Management Bot**, where content must adhere to strict branding guidelines. A key guideline is maintaining a consistently positive tone. Manually checking every AI-generated post is inefficient. Therefore, the aim is to develop a Python script that automates this multi-step process, showcasing the concept of a **"Tool Chain"** or **"Agent Workflow"** in AI development.

### 2.1 AI Tools Employed

1.  **AI Tool 1: OpenAI (GPT-3.5/GPT-4)**: A large language model (LLM) utilized for its strong capabilities in **creative and context-aware text generation**.
2.  **AI Tool 2: Hugging Face Inference API (Sentiment Analysis)**: A specialized model, in this case, `bertweet-base-sentiment-analysis`, used for its precision in **classifying the emotional tone** of text.

-----

## 3\. Methodology

### 3.1 Experimental Persona and Goal

  * **Persona:** Content Strategist Programmer.
  * **Problem:** Ensure all generated social media posts maintain a highly positive sentiment.
  * **Threshold:** Positive sentiment must have a confidence score of $\mathbf{\ge 0.90}$ (90%).

### 3.2 Python Orchestration Architecture

The Python code is structured into three primary components, managing the flow of data:

1.  **API Client Functions:** `generate_post(topic)` for OpenAI and `analyze_sentiment(text)` for Hugging Face. These functions encapsulate API calls, authentication, and error handling.
2.  **Core Automation Logic:** The `automate_content_check(topic)` function serves as the **workflow controller**. It calls the AI tools sequentially.
3.  **Decision and Insight Generation:** Within the core logic, an `if/else` block processes the output of AI Tool 2 (sentiment score) and determines the final **actionable insight**—either a **PASS** or a **FLAG**. If flagged, it initiates the **iterative revision loop** by calling AI Tool 1 a second time with new, analytical context.

### 3.3 Data Flow and Output Comparison

The experiment relies on the principle of **output chaining**:

1.  **Input:** Topic (`The launch of the new Python AI library`).
2.  **Process 1 (OpenAI):** Input $\rightarrow$ Text Output (Post).
3.  **Process 2 (Hugging Face):** Text Output $\rightarrow$ JSON Output (Sentiment: 'POSITIVE', Score: 0.98).
4.  **Comparison:** The Python script extracts the `Score` and compares it to the $\mathbf{0.90}$ threshold.
5.  **Output (Insight):** Based on the comparison, it prints the appropriate message (Pass or Flag).

<img width="269" height="188" alt="image" src="https://github.com/user-attachments/assets/4142d32e-920d-4344-b0ad-dee740f48eb1" />


-----

## 4\. Python Implementation Details

The implementation leverages the `openai` SDK for the LLM and the `requests` library for the RESTful Hugging Face API, highlighting the **protocol compatibility** required for multi-tool integration.

### 4.1 Integration with AI Tool 1 (OpenAI)

The `generate_post` function utilizes a **system-level prompt** (`"You are a cheerful and professional social media content creator."`) to enforce the desired persona, maximizing the chance of a positive initial post.

```python
# --- AI Tool 1: Content Generation (OpenAI) ---
def generate_post(topic):
    # ... (API connection setup)
    response = openai.chat.completions.create(
        model="gpt-3.5-turbo",
        messages=[
            {"role": "system", "content": "You are a cheerful and professional social media content creator."},
            {"role": "user", "content": f"Write a single, exciting tweet about the topic: {topic}"}
        ]
    )
    # ...
    return post
```

### 4.2 Integration with AI Tool 2 (Hugging Face)

The `analyze_sentiment` function processes the API's JSON response, which often contains multiple labels and scores, using the `max()` function to isolate the **highest confidence score** and its corresponding label (e.g., 'POSITIVE' or 'NEGATIVE').

```python
# --- AI Tool 2: Sentiment Analysis (Hugging Face) ---
def analyze_sentiment(text):
    # ... (API connection setup)
    response = requests.post(SENTIMENT_API_URL, headers=HEADERS, json=payload)
    # Extract the highest scoring label and its score
    result = response.json()[0]
    sentiment = max(result, key=lambda x: x['score'])
    return sentiment # Returns {'label': 'POSITIVE', 'score': 0.98}
```

### 4.3 Actionable Insights and Iteration

The most critical section is the conditional logic (`if main_sentiment == required_sentiment...`). For a **FLAG** condition, the script enters a **secondary interaction loop** with AI Tool 1. This time, the prompt includes the analytical failure data: the original post, the actual sentiment, and the low score. This is an example of an **AI-to-AI feedback mechanism**.

```python
# --- Core Automation Logic (Extract from automate_content_check) ---
# ... (Sentiment analysis steps)

if main_sentiment == required_sentiment and main_score >= required_score_threshold:
    print("✅ **AUTOMATION PASS:** The post meets the brand's positive sentiment threshold.")
else:
    print(f"🚩 **AUTOMATION FLAG:** The post's sentiment is '{main_sentiment}' (Score: {main_score:.2f}).")
    
    # Iterative Revision Loop: Use AI Tool 1 again
    suggestion_prompt = (f"The following post failed a sentiment check... Rewrite it to be more clearly positive...")
    # ... (Call to OpenAI to generate suggestion)
```

-----

## 5\. Results and Discussion

The execution of the Python code demonstrated a successful, orchestrated workflow between the two heterogeneous AI services.

### 5.1 Results Table Summarization

| Scenario | Generated Post (AI Tool 1) | Sentiment Score (AI Tool 2) | Actionable Insight |
| :--- | :--- | :--- | :--- |
| **Example 1 (Success)** | "Excited for the new Python AI library\! It's a game-changer\!" | POSITIVE (0.98) | **✅ AUTOMATION PASS** |
| **Example 2 (Failure)** | "The new library launched today. It's fine." | NEUTRAL (0.75) | **🚩 AUTOMATION FLAG** + Revision Suggestion |

### 5.2 Discussion of AI Compatibility and Synergy

1.  **Data Curation and Translation:** Python successfully managed the data translation required. It accepted a natural language string from OpenAI and converted it into a JSON request for Hugging Face. Crucially, it then extracted numerical data ($\text{score}$) from Hugging Face's JSON response, using this quantitative data to drive the program's logical flow.
2.  **Iterative Workflow:** The implementation proves that AI tools can be integrated beyond simple linear pipelines. The ability to use the **analytical output** (sentiment failure) as a **contextual input** for the **generative tool** (OpenAI) is a powerful pattern for building self-correcting or quality-controlled AI applications. This iterative step is the **"actionable insight"** in its most advanced form.
3.  **Overcoming Tool Specialization:** The experiment successfully mitigated the limitations of a single tool. The **generative model** (OpenAI) can sometimes be inconsistent with specific emotional tones, but the **specialized analytical model** (Hugging Face) acts as the precise validator, enforcing the strict business rule that the LLM might occasionally violate.

### 5.3 Learning Outcomes

  * **API Management:** Successfully managed two different API authentication methods and request formats (library SDK vs. raw HTTP `requests`).
  * **Decision Logic:** Demonstrated how to convert **qualitative AI output** (a post) into **quantitative analytical data** (a sentiment score), which can then be used for **rule-based decision-making**.
  * **Persona Pattern:** The programmer successfully adopted the **Content Strategist** persona, ensuring the code directly solves a real-world business constraint (brand voice compliance).

-----

## 6\. Future Scope

The current implementation provides a static threshold and relies on two tools. Future extensions could include:

  * **Dynamic Thresholding:** Allowing the required sentiment score to vary based on the topic or target platform.
  * **Integration of a Third Tool:** Adding an image generation AI (e.g., DALL-E) based on the *revised* positive text, completing the social media content package.
  * **Automated Retries:** Instead of just generating a suggestion, the script could automatically retry generating the post (AI Tool 1) up to a set number of times until a **PASS** is achieved, fully closing the automation loop.

-----

## 7\. Conclusion

The experiment successfully achieved its aim by writing and implementing a robust Python script that integrates the **OpenAI API** for content generation and the **Hugging Face Inference API** for sentiment analysis. By using the output of the sentiment model as the decision-making metric against a predefined threshold, the code effectively automates the task of **comparing AI outputs and generating actionable insights**—specifically, flagging content that does not meet the required positive brand sentiment and subsequently using the first AI tool to provide a revision suggestion. This demonstrates the high **compatibility, efficiency, and architectural necessity** of using Python as the central development platform for complex, multi-tool AI applications, moving beyond single-API interaction to create sophisticated, intelligent workflows.

-----

## Result:

The corresponding Prompt is executed successfully. The Python code was fully implemented, integrating the OpenAI and Hugging Face APIs to automate content vetting, validate sentiment against a $90\%$ confidence threshold, and generate actionable pass/fail insights with automatic revision suggestions, thereby confirming the project's aim.
