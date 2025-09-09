import os
import requests
from urllib.parse import urlparse
import uuid

def download_image():
    # Prompt user for image URL
    url = input("https://unsplash.com/photos/a-person-swimming-in-the-ocean-with-a-camera-NhWxAIs61MM").strip()
    
    # Directory to save images
    save_dir = "Fetched_Images"
    os.makedirs(save_dir, exist_ok=True)
    
    try:
        # Fetch image
        response = requests.get(url, timeout=10)
        response.raise_for_status()  # Raise error for bad responses
        
        # Extract filename from URL
        parsed_url = urlparse(url)
        filename = os.path.basename(parsed_url.path)
        
        # If no filename, generate one
        if not filename or '.' not in filename:
            filename = f"image_{uuid.uuid4().hex}.jpg"
        
        # Full path to save
        save_path = os.path.join(save_dir, filename)
        
        # Save file in binary mode
        with open(save_path, "wb") as f:
            f.write(response.content)
        
        print(f"Image saved successfully at: {save_path}")
    
    except requests.exceptions.RequestException as e:
        print(f"Error fetching image: {e}")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")

if __name__ == "__main__":
    download_image()
