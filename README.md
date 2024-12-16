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

Get your Gemini API key and replace "AIzaSyDr_qZH6tVihVRBWIeEVvUlGlwjHCRLZQ8" with your actual key in the script.
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
Copy code
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
