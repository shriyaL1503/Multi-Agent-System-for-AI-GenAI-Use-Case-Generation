Requirements
Make sure you have the necessary libraries installed. You can install them using:
pip install streamlit openai langchain

Streamlit Code
Below is the complete Streamlit app code for the multi-agent system. It uses interactive widgets to take user input for the industry and company name, runs the multi-agent workflow, and displays the generated report.
# Import necessary libraries
import streamlit as st
from langchain.llms import OpenAI

# Initialize OpenAI LLM
llm = OpenAI(model="gpt-4", api_key="YOUR_API_KEY")

# Define agent functions
def industry_company_research(industry, company):
    research_prompt = f"Research the {industry} industry and the company {company}. \
    Identify key trends, company focus areas, and potential areas for AI integration."
    response = llm(research_prompt)
    return response

def use_case_generation(research_summary):
    use_case_prompt = f"Based on the following industry and company research summary: {research_summary}, \
    propose relevant AI and GenAI use cases that would benefit the company's operations and customer experience."
    response = llm(use_case_prompt)
    return response

def resource_asset_collection(use_cases):
    dataset_prompt = f"Find relevant datasets for these use cases on Kaggle, HuggingFace, or GitHub: {use_cases}. \
    List the URLs to relevant datasets."
    response = llm(dataset_prompt)
    return response

def compile_final_proposal(research_summary, use_cases, datasets):
    proposal_prompt = f"Create a final proposal based on the following information. \
    \nIndustry and Company Research: {research_summary} \
    \nProposed Use Cases: {use_cases} \
    \nDatasets and Resources: {datasets}. \
    Format the report clearly with actionable insights and link to resources."
    response = llm(proposal_prompt)
    return response

def main_workflow(industry, company):
    research_summary = industry_company_research(industry, company)
    use_cases = use_case_generation(research_summary)
    datasets = resource_asset_collection(use_cases)
    final_proposal = compile_final_proposal(research_summary, use_cases, datasets)
    return research_summary, use_cases, datasets, final_proposal

# Streamlit App Interface
st.title("AI & GenAI Use Case Generation")

st.header("Enter Company and Industry Information")
industry = st.text_input("Industry", "Retail")
company = st.text_input("Company Name", "ExampleCorp")

if st.button("Generate Use Cases"):
    with st.spinner("Generating use cases and resources..."):
        research_summary, use_cases, datasets, final_proposal = main_workflow(industry, company)

        st.subheader("1. Industry & Company Research Summary")
        st.write(research_summary)

        st.subheader("2. Generated Use Cases")
        st.write(use_cases)

        st.subheader("3. Resource Assets")
        st.write(datasets)

        st.subheader("4. Final Proposal")
        st.write(final_proposal)

        st.success("Use case generation completed!")

How to Run the Streamlit App
Save the code in a Python file, e.g., multi_agent_app.py.
Run the Streamlit app with:
streamlit run multi_agent_app.py

Explanation of the Streamlit Components
User Input: st.text_input fields allow users to enter the industry and company name.
Button: The "Generate Use Cases" button triggers the main_workflow function to execute the multi-agent pipeline.
Results Display: Each agent’s output is displayed in a separate section (st.subheader and st.write) so users can view the research summary, generated use cases, datasets, and the final proposal.
Deploying on Streamlit Cloud
Once the app is working locally, you can deploy it to Streamlit Cloud by following these steps:

Push your code to a GitHub repository.
Go to Streamlit Cloud.
Link your GitHub repository and select your app file (multi_agent_app.py).
Deploy the app, and you’ll get a shareable link!
