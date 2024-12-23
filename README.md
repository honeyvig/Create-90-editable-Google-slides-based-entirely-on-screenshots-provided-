# Create-90-editable-Google-slides-based-entirely-on-screenshots-provided
I need 90 editable Google slides created (124 total but 34 are simply duplicated).

For each slide I will be giving you an example in form of PNG images for you to use as template.

There are images involved - in some cases I need you to use ideogram.ai to create a similar image - in other cases just use the current image as a placeholder which I can easily replace.

All text needs to be editable as I will be editing these slides.

I attach 1 example. To apply for the job please first confirm to me that you have an account at ideogram.ai and then create a Google Slide from the attached example based on the below instructions:

Amend text to:

We Will Take You By The Hand And Make Sure That YOU Succeed

We Can Guarantee You Success With This AI Software...

Use ideogram.ai to create a brand new image to replace current image - something like a robot celebrating with bundles of cash.
------------------
To automate the creation of Google Slides with editable text and images using Python, you can use the Google Slides API in conjunction with libraries like google-api-python-client and google-auth, as well as ideogram.ai for generating the required images.
Steps for the Python Code Implementation:

    Set Up Google Cloud and Install Required Libraries:

        To interact with the Google Slides API, you’ll need to create a project in Google Cloud, enable the Google Slides API, and get OAuth 2.0 credentials.

        Install required Python libraries:

        pip install google-api-python-client google-auth-httplib2 google-auth-oauthlib
        pip install requests  # For API requests to ideogram.ai

    Authenticate Google API: You'll need to authenticate your Google Slides API and set up your credentials using OAuth 2.0. You'll be redirected to the browser to log in with your Google account.

    Use Ideogram.ai API to Generate Images: To generate images using ideogram.ai, you would need to have an API key for ideogram.ai. In your case, we’ll simulate this by replacing the image via a placeholder URL (the generated image from Ideogram.ai API).

    Create Google Slides Programmatically: We'll then write Python code to create the Google Slides, replace the images and text, and make them editable.

Google Slides API Code Example:

Here's a code sample to create slides with editable text and replace images.

import os
import io
import json
import google.auth
from google.auth.transport.requests import Request
from google_auth_oauthlib.flow import InstalledAppFlow
from googleapiclient.discovery import build
from googleapiclient.http import MediaIoBaseDownload
import requests

# Step 1: Authenticate Google API
SCOPES = ['https://www.googleapis.com/auth/presentations', 'https://www.googleapis.com/auth/drive.file']
creds = None

# The file token.json stores the user's access and refresh tokens, and is
# created automatically when the authorization flow completes for the first time.
if os.path.exists('token.json'):
    creds = google.auth.load_credentials_from_file('token.json')[0]

# If there are no (valid) credentials available, let the user log in.
if not creds or not creds.valid:
    if creds and creds.expired and creds.refresh_token:
        creds.refresh(Request())
    else:
        flow = InstalledAppFlow.from_client_secrets_file(
            'credentials.json', SCOPES)
        creds = flow.run_local_server(port=0)
    # Save the credentials for the next run
    with open('token.json', 'w') as token:
        token.write(creds.to_json())

# Step 2: Build the API client for Google Slides
service = build('slides', 'v1', credentials=creds)

# Step 3: Replace Placeholder Text and Images
def create_slide_presentation(presentation_title, slide_data):
    # Create a new presentation
    presentation = service.presentations().create(body={
        'title': presentation_title
    }).execute()
    
    # Replace images and text for each slide
    slide_id = presentation['slides'][0]['objectId']
    requests = []
    
    # Replace text
    requests.append({
        'replaceAllText': {
            'containsText': {
                'text': '{{text_placeholder}}',
                'matchCase': True
            },
            'replaceText': 'We Will Take You By The Hand And Make Sure That YOU Succeed'
        }
    })
    
    # Replace second text
    requests.append({
        'replaceAllText': {
            'containsText': {
                'text': '{{text_placeholder2}}',
                'matchCase': True
            },
            'replaceText': 'We Can Guarantee You Success With This AI Software...'
        }
    })

    # Step 4: Replace Image Placeholder (using Ideogram AI image)
    ideogram_response = generate_ideogram_image()
    image_url = ideogram_response['image_url']

    # Replace image in the slide (assuming slide has image placeholder with specific id)
    requests.append({
        'replaceAllShapesWithImage': {
            'imageUrl': image_url,
            'imageReplaceMethod': 'IMAGE_REPLACE_METHOD_UNSPECIFIED',
            'containsText': {
                'text': '{{image_placeholder}}'
            }
        }
    })
    
    # Execute the batch update
    service.presentations().batchUpdate(
        presentationId=presentation['presentationId'], body={'requests': requests}
    ).execute()

    return presentation

def generate_ideogram_image():
    # Replace this with your ideogram API endpoint and API key
    api_url = "https://api.ideogram.ai/v1/generate"
    headers = {
        "Authorization": "Bearer YOUR_API_KEY",
        "Content-Type": "application/json"
    }
    data = {
        "description": "A robot celebrating with bundles of cash"
    }

    response = requests.post(api_url, headers=headers, json=data)
    return response.json()

# Example usage: Create a presentation
presentation = create_slide_presentation("New Year's Eve Party", slide_data={})
print(f"Presentation ID: {presentation['presentationId']}")

Step-by-Step Breakdown of the Code:

    Authenticate Google API: This part handles authentication to interact with Google Slides and allows the application to manage presentations in your Google Drive.

    Create Presentation: A new Google Slide presentation is created with a given title.

    Replace Text: The code finds specific placeholders (like {{text_placeholder}}) and replaces them with the required editable text.

    Generate Images with Ideogram AI: The generate_ideogram_image function interacts with the Ideogram API to generate an image of a robot celebrating with bundles of cash. You'll need to replace the placeholder API key and endpoint for your own setup.

    Replace Image: The script then replaces an image placeholder on the slide with the newly generated image from Ideogram AI.

Important Notes:

    Replace "YOUR_API_KEY" in the generate_ideogram_image() function with your actual API key from Ideogram AI.
    The placeholder text ({{text_placeholder}}) and images should be defined in the template slides you provide.
    Ensure you have created and shared the credentials file (credentials.json) for Google Slides API authentication.

Conclusion:

This Python code should help you create, customize, and generate images dynamically for a Google Slides presentation, along with editable text that you can later update manually. You just need to adjust the placeholder text and image logic to suit the exact requirements for the slides you're creating.
