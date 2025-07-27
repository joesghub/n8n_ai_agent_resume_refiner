# Resume Refiner AI Agent in n8n

## Overview

## AI Agent Flow diagram
![N8N Resume Refine AI Agent Systems Diagram](https://github.com/joesghub/n8n_ai_agent_resume_refiner/blob/main/img/Resume_refiner_n8n.png?raw=true)

## Building the Agent
- What approach did you take to design your agent?
![Resume Refiner Workflow](https://github.com/joesghub/n8n_ai_agent_resume_refiner/blob/main/img/resume%20refiner%20workflow.png?raw=true)
    - I thought about the components the AI Agent would need to deliver a useful output. The simplest pieces are the applicant name and email address, those are string type. The resume and job link both need further extraction before the AI agent would be able to use the associated data. In choosing tools I tried to use less to limit the amount of data transformations. 

- What challenges did you face in parsing, formatting, or integrating?
![Parameter Token Length](https://github.com/joesghub/n8n_ai_agent_resume_refiner/blob/main/img/parameter%20token%20length.png?raw=true)
  - Error: Bad request - please check your parameters
    
    This model's maximum context length is 16385 tokens. However, your messages resulted in 101142 tokens (101105 in the messages, 37 in the functions). Please reduce the length of the messages or functions.

I reduced my prompt size by changing the references to my resume and job link. First I was sending in the full webpage and resume text into the prompt model. To reduce the size I added an HTTP Request tool to the model so the lookup would be run after the prompt. Instead of linking the full resume text, I referenced the file from the Collection Form. This allowed the prompt to be significantly shorter and enable the agent to deliver a response in a more effecient manner. 

- How did you ensure that the AI returned JSON reliably?
![Chat Gpt Json Schema](https://github.com/joesghub/n8n_ai_agent_resume_refiner/blob/main/img/chat%20gpt%20json%20schema.png?raw=true)
![Json Schema](https://github.com/joesghub/n8n_ai_agent_resume_refiner/blob/main/img/json%20schema.png?raw=true)

I asked ChatGPT: 
- "I'm creating a structured output parser tool in a n8n flow. How would I best create a json format for this output response so it can be put into emails to read easily? Can you create a json schema? based on the instructions provided here: https://json-schema.org/learn/miscellaneous-examples"

Since I was able to get the AI agent to read the inputs and generate 

## Results
**My Final Prompt**
![Final Prompt](https://github.com/joesghub/n8n_ai_agent_resume_refiner/blob/main/img/final%20prompt.png?raw=true)

**My Resume Refinement Email**
![Output Email](https://github.com/joesghub/n8n_ai_agent_resume_refiner/blob/main/img/refiner%20output%20email.png?raw=true)


## Optimization
1. Summarize Job Links Before Prompting
🔍 Use a Readability.js, cheerio, or OpenAI call to extract just the relevant job post info (title, responsibilities, required skills) before feeding it into the main prompt.

✅ Why it matters: Reduces token count drastically and improves prompt relevance.

2. Modularize with Sub-Workflows
🧩 Break the workflow into reusable sub-workflows:

Resume Preprocessing

Job Post Summarizer

AI Prompt Builder

Email Formatter

✅ Why it matters: Easier debugging, reuse for other use cases (cover letters, skills match), and better long-term scalability.

3. Validate and Retry AI JSON Output
🧠 Add a Function node or IF node to validate that the response is parsable JSON.

If invalid, auto-retry with a modified prompt:
“Please return valid JSON. Do not include commentary or markdown formatting.”

✅ Why it matters: Prevents broken automation when AI returns incorrect formats.

4. Use a Resume Parser API or Script
📄 Instead of sending full resume text, extract structured fields (skills, experience, job titles) with a parser like resumey.pro, Affinda, or a custom Node.js script.

✅ Why it matters: Greatly reduces token usage and makes prompts cleaner and more focused.

5. Templated HTML Email Output
💌 Use Handlebars or Mustache in the Email node to insert structured AI output (from JSON) into a clean, branded email template.

✅ Why it matters: Improves readability and professionalism of the final result.


