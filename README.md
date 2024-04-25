# YouTube Transcript Summarizer
This is a simple Streamlit application that summarizes the transcript of a YouTube video using Google's Gemini Pro API.

## Dependencies
Python 3.x
Streamlit
python-dotenv
google-generativeai
youtube_transcript_api
## You can install the dependencies using pip:

pip install streamlit python-dotenv google-generativeai youtube_transcript_api

## Usage
Make sure you have Python installed on your system.
Create a new directory for your project and navigate into it.
Create a new Python file (e.g., app.py) and paste the following code:
import streamlit as st
from dotenv import load_dotenv
load_dotenv()
import os
import google.generativeai as genai

genai.configure(api_key=os.getenv("GOOGLE_API_KEY"))

from youtube_transcript_api import YouTubeTranscriptApi

prompt = """You are the YouTube Video summarizer. You will be taking the transcript text and summarizing the entire video and provide the important summaryin point within 250 words. Please provide the summary of the text provided here: """

def extract_transcript_details(youtube_video_url):
    try:
        video_id=youtube_video_url.split("=")[1]
        transcript_text=YouTubeTranscriptApi.get_transcript(video_id)

        transcript=""
        for i in transcript_text:
            transcript+=" "+i["text"]

        return transcript    

    except Exception as e:
        raise e    
    
##getting the summary based on Prompt form Google Gemini pro
def generate_gemini_content(transcript_text):
    
    model=genai.GenerativeModel("gemini-pro")
    response=model.generate_content(prompt+transcript_text)
    return response.text

st.title("YouTube Transcript to Detailed Notes Converter")
youtube_link = st.text_input("Enter YouTube video Link:")
if youtube_link:
    video_id = youtube_link.split("=")[1]
    print(video_id)
    st.image(f"http://img.youtube.com/vi/{video_id}/0.jpg" , use_column_width=True)

if st.button("Get Detailed notes"):
    transcript_text=extract_transcript_details(youtube_link)

    if transcript_text:
        summary = generate_gemini_content(transcript_text)
        st.markdown("## Detailed notes:")
        st.write(summary)
Save the file and navigate to the directory containing app.py in your terminal or command prompt.
Set up your environment variables by creating a .env file in the same directory as your app.py file and add your Google API key:

GOOGLE_API_KEY=<your-api-key>
Run the Streamlit app:
streamlit run app.py

Access the Streamlit app in your web browser at http://localhost:8501.
Enter the YouTube video link in the text input field and click the "Get Detailed notes" button to generate the summary.
