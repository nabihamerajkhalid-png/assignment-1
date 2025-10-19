# assignment-1
living room decor
# This project must run in Google's Colab
from google import genai
from google.colab import userdata
from google.genai import types
import requests
from IPython.display import display, Image 

# Image of a living space with decor 
image_path = "https://cdn.apartmenttherapy.info/image/upload/f_auto,q_auto:eco,c_fit,w_730,h_521/k%2FPhoto%2FSeries%2F2019-10--power-hour-instant-pot%2FPower-Hour-Instant-Pot_001-rotated"

image = requests.get(image_path)

# Initialize Gemini client with your API key
client = genai.Client(api_key=userdata.get('AIzaSyAJlRnhUrUDZ-Iaz9jCupT7zQBD6ouAwHc'))

# Ask Gemini to list home decor elements in the image
response = client.models.generate_content(
    model="gemini-2.0-flash-exp",
    contents=[
        "List the visible home decor elements in the image in bullet points.",
        types.Part.from_bytes(data=image.content, mime_type="image/jpeg")
    ]
)

res = response.text

# Extract only lines that start with '*', then clean them
decor_items = [line.lstrip('*').strip() for line in res.splitlines() if line.strip().startswith('*')]

print(response.text)
print(decor_items)
display(Image(url=image_path))
