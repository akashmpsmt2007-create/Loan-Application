# Loan-Application
Improving Loan Application Process
import gradio as gr
import google.generativeai as genai

# Configure Gemini API Key
genai.configure(api_key="YOUR_GEMINI_API_KEY")

model = genai.GenerativeModel("gemini-1.5-flash")

def loan_agent(name, age, income, loan_amount, query):
    
    eligibility = "Eligible" if income >= 25000 and age >= 21 else "Not Eligible"

    prompt = f"""
    You are an AI Loan Assistant.

    Applicant Details:
    Name: {name}
    Age: {age}
    Monthly Income: {income}
    Requested Loan Amount: {loan_amount}

    Eligibility Status: {eligibility}

    User Query:
    {query}

    Provide guidance about the loan application process.
    """

    response = model.generate_content(prompt)

    return f"Eligibility: {eligibility}\n\n{response.text}"

interface = gr.Interface(
    fn=loan_agent,
    inputs=[
        gr.Textbox(label="Applicant Name"),
        gr.Number(label="Age"),
        gr.Number(label="Monthly Income"),
        gr.Number(label="Loan Amount"),
        gr.Textbox(label="Ask a Question")
    ],
    outputs="text",
    title="AI Loan Application Assistant",
    description="AI Agent for Improving the Loan Application Process"
)

interface.launch()
