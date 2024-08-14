# Api-task
# Text-to-speech

# Alternative Method Using pyttsx3

# Features
Convert text to speech using pyttsx3, which supports offline TTS.
Simple and user-friendly web interface.
Works offline without the need for internet access.
# Requirements
Python 3.x
Flask
pyttsx3

# Installation
Create and activate a virtual environment (optional but recommended):


python -m venv venv
For Windows:


venv\Scripts\activate
Install the required packages: Install Flask and pyttsx3 using pip:


pip install Flask pyttsx3
Create the static directory: Ensure a static folder exists to store generated audio files.

# Code
app.py
Imports:

Flask: Web framework for Python.
pyttsx3: Text-to-Speech library that works offline.
os: For file operations.
Flask App Setup:

Create a Flask app instance.
Define routes and handle HTTP methods.

from flask import Flask, render_template, request, send_file
import pyttsx3
import os

app = Flask(__name__)

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        text = request.form['text']
        language = request.form['language']

        # Initialize pyttsx3 engine
        engine = pyttsx3.init()

        if language == 'en':
            engine.setProperty('voice', 'english')
        elif language == 'ar':
            engine.setProperty('voice', 'arabic')

        # Save the speech to an mp3 file
        output_path = os.path.join('static', 'output.mp3')
        engine.save_to_file(text, output_path)
        engine.runAndWait()

        return render_template('index.html', audio_file='output.mp3')

    return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=True)
templates/index.html
HTML Form:

Textarea for user input.
Dropdown to select language (English or Arabic).
Submit button to trigger TTS conversion.
Audio Playback:

If audio is generated, display an HTML5 audio player to listen to the output.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Text to Speech</title>
</head>
<body>
    <h1>Text to Speech Converter</h1>
    <form action="/" method="POST">
        <textarea name="text" placeholder="Enter text here..."></textarea><br>
        <select name="language">
            <option value="en">English</option>
            <option value="ar">Arabic</option>
        </select><br>
        <button type="submit">Convert to Speech</button>
    </form>
    {% if audio_file %}
    <h2>Generated Speech:</h2>
    <audio controls>
        <source src="{{ url_for('static', filename=audio_file) }}" type="audio/mp3">
        Your browser does not support the audio element.
    </audio>
    {% endif %}
</body>
</html>

# Usage
Run the Flask app:
Start the server:


python app.py
This will host the app locally on http://127.0.0.1:5000.

Open your web browser and navigate to: http://127.0.0.1:5000.

Interact with the app:

Enter text in the provided text area.
Select a language (English or Arabic).
Click "Convert to Speech" to generate and play the audio.
This alternative method allows you to create a TTS application that works offline and provides a user-friendly interface.
