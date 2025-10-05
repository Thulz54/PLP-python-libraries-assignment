import os
import requests
from urllib.parse import urlparse

def fetch_image():
    print("Welcome to the Ubuntu Image Fetcher")
    print("A tool for mindfully collecting images from the web\n")
    
    # Prompt the user for an image URL
    url = input("Please enter the image URL: ").strip()
    save_dir = "Fetched_Images"
    os.makedirs(save_dir, exist_ok=True)
    
    # Extract filename from URL or generate one
    parsed_url = urlparse(url)
    filename = os.path.basename(parsed_url.path)
    if not filename:
        filename = "downloaded_image.jpg"
    
    file_path = os.path.join(save_dir, filename)
    
    try:
        response = requests.get(url, timeout=10)
        response.raise_for_status() # Raise HTTPError for bad responses
        
        # Save the image in binary mode
        with open(file_path, "wb") as f:
            f.write(response.content)
        
        print(f"Successfully fetched: {filename}")
        print(f"Image saved to {file_path}")
        print("Connection strengthened. Community enriched.")
    
    except requests.exceptions.HTTPError as http_err:
        print(f"HTTP error occurred: {http_err}")
    except requests.exceptions.ConnectionError:
        print("Connection error: Unable to reach the server.")
    except requests.exceptions.Timeout:
        print("Timeout error: The server took too long to respond.")
    except Exception as e:
        print(f"An unexpected error occurred: {e}")

if __name__ == "__main__":
    fetch_image()
