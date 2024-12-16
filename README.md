# Invoice Reader

A Python-based tool for reading and classifying invoice data using OCR (Tesseract), AI-powered text classification (Gemini API), and MongoDB for structured data storage.

## Features

- Extracts text from invoice images using Tesseract OCR.
- Classifies invoice data into **Total Bill**, **Taxes**, and **Addresses** using the Gemini AI model.
- Saves structured data into a MongoDB database.
- Provides regex-based validation and structured data extraction.

---

## Table of Contents

- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Output](#output)
- [Future Scope](#future-scope)
- [License](#license)

---

## Requirements

Ensure you have the following installed:

- **Python 3.8+**
- **Tesseract OCR** (set up with the correct executable path)
- **MongoDB** (local or remote instance)
- Libraries:
  - `pytesseract`
  - `opencv-python`
  - `google.generativeai`
  - `pymongo`
  - `re`

---

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/invoice-reader.git
   cd invoice-reader
Install dependencies:

bash
Copy code
pip install pytesseract opencv-python google-generativeai pymongo
Install Tesseract OCR:

Download and install Tesseract OCR from Tesseract's official site.
Set the pytesseract.pytesseract.tesseract_cmd variable in the script to the correct path (e.g., C:\Program Files\Tesseract-OCR\tesseract.exe on Windows).
Ensure MongoDB is running:

Download MongoDB from MongoDB's official site if you haven't installed it yet.
Start the MongoDB service on your system.
Set up Gemini AI:

Get your Gemini API key and replace "API key" with your actual key in the script.
Usage
Place the invoice image in a directory of your choice.
Update the image_path variable in the script with the path to the invoice image.
Run the script:
bash
Copy code
python invoice_reader.py
The extracted text will be displayed in the console, and structured invoice data will be saved in MongoDB.
Output
The program processes invoice images to produce:

Extracted Text: The raw text data extracted using Tesseract OCR.

Structured Data: The classified information in the following format:

json
Copy code
{
    "total_bill": 199.99,
    "taxes": 20.0,
    "address_from": "123 Your Company Address",
    "address_to": "456 Your Client Address"
}
The structured data is saved into MongoDB under the database invoice1_data and collection invoices.

Example
Input: An invoice image with the following fields:

Total: $199.99
Tax: $20.00
Address From: Your Company Address
Address To: Your Client Address
Output (console and MongoDB):

plaintext
Extracted text from the image:
Total: $199.99
Tax: $20.00
Address From: Your Company Address
Address To: Your Client Address

Structured data to save:
{
    "total_bill": 199.99,
    "taxes": 20.0,
    "address_from": "Your Company Address",
    "address_to": "Your Client Address"
}

Data saved to MongoDB successfully.
Future Scope
Improve text classification accuracy using more robust AI models.
Add support for multiple invoice formats and languages.
Enhance error handling and logging mechanisms.
Create a web-based interface for uploading images and viewing structured data.
License
This project is licensed under the MIT License. Feel free to modify and use it as needed.

csharp
Copy code

### Next Steps:
1. Save this content to a `README.md` file.
2. Commit the changes to your repository:
   ```bash
   git add README.md
   git commit -m "Added README for Invoice Reader project"
   git push


CODE:
import pytesseract
import cv2
import google.generativeai as genai
import re
import pymongo
from pymongo import MongoClient

# Path to Tesseract executable
pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"

# MongoDB setup (make sure MongoDB is running locally or use a remote DB URI)
client = MongoClient('mongodb://localhost:27017/')
db = client['invoice1_data']  # Database name
collection = db['invoices']  # Collection name

def imToString(image_path):
    try:
        # Load the image directly
        image = cv2.imread(image_path)
        
        # Check if the image was loaded correctly
        if image is None:
            print("Error: Could not load the image.")
            return None
        
        # Convert the image to grayscale for better OCR results
        img_gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
        
        # Use OCR to extract text from the image
        data = pytesseract.image_to_string(img_gray, lang='eng')
        
        # Print the extracted text in the console
        if data.strip():
            print(f"Extracted text from the image:")
            print(data)
            return data
        else:
            print("No text found in the image.")
            return None
    
    except Exception as e:
        print(f"Error processing the image: {e}")
        return None

def classify_invoice_data(extracted_text):
    genai.configure(api_key="API key")

    model = genai.GenerativeModel("gemini-1.5-flash")
    prompt = "Classify this invoice data into three classes: Total Bill, Taxes, and Address. Ignore irrelevant details like emails and bank account information. Here is the data: " + extracted_text

    response = model.generate_content(prompt)
    recipe_text = response.text

    print(f"Gemini Classification Response:\n{recipe_text}")

    # Extract specific fields using regex
    total_bill_match = re.search(r'Total\s*\$([\d,]+\.\d+)', recipe_text)
    taxes_match = re.search(r'Tax\s*\$([\d,]+\.\d+)', recipe_text)
    address_from_match = re.search(r'Your Company Address:\s*(.+?),', recipe_text)
    address_to_match = re.search(r'Your Client Address:\s*(.+?),', recipe_text)

    # Prepare structured data
    structured_data = {
        "total_bill": float(total_bill_match.group(1).replace(',', '')) if total_bill_match else 0.0,
        "taxes": float(taxes_match.group(1).replace(',', '')) if taxes_match else 0.0,
        "address_from": address_from_match.group(1).strip() if address_from_match else "N/A",
        "address_to": address_to_match.group(1).strip() if address_to_match else "N/A",
    }

    # Print structured data for debugging
    print("Structured data to save:", structured_data)

    # Insert into MongoDB
    try:
        collection.insert_one(structured_data)
        print("Data saved to MongoDB successfully.")
    except Exception as e:
        print(f"Error saving data to MongoDB: {e}")



# Image path you want to test
image_path = r"D:\609d5d3c4d120e370de52b70_invoice-lp-light-border.png"

# Get the extracted text from the image
extracted_text = imToString(image_path)

# Classify the extracted invoice data using the Gemini API
if extracted_text:
    classify_invoice_data(extracted_text)
