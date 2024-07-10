PDF Data Extraction and Processing
This project aims to extract structured data from PDF files and store it in an Excel file. The PDF files are obtained from Google Drive using the PyDrive library. The PyMuPDF library is used for extracting text content from the PDF, and pandas is used for data manipulation and exporting the results to an Excel file.

Prerequisites
Before running the code, make sure you have the following installed:

Python (3.6+)
pip (Python package installer)
Install the required libraries:
bash
Copy code
pip install PyDrive pymupdf pandas
Set up Google Drive API
Go to the Google Drive API Quickstart page and follow the instructions to enable the Google Drive API.
Download the credentials.json file and place it in your working directory.
Project Structure
graphql
Copy code
.
├── credentials.json  # Google Drive API credentials
├── extract_pdf.py    # Main Python script for extracting data from PDF
└── README.md         # This README file
Usage
Download the PDF file from Google Drive and extract data:
python
Copy code
from pydrive.auth import GoogleAuth
from pydrive.drive import GoogleDrive
import fitz  # PyMuPDF
import pandas as pd
import re

# Authenticate and create the PyDrive client
gauth = GoogleAuth()
gauth.LocalWebserverAuth()  # Creates local webserver and auto handles authentication.
drive = GoogleDrive(gauth)

# Replace with your actual Google Drive file ID
file_id = 'YOUR_GOOGLE_DRIVE_FILE_ID'

# Download the PDF file from Google Drive
downloaded = drive.CreateFile({'id': file_id})
pdf_path = '/content/downloaded_pdf.pdf'
downloaded.GetContentFile(pdf_path)

# Open the downloaded PDF file
pdf_document = fitz.open(pdf_path)

# Initialize variables to store data
data = {
    'Column 1': [],
    'Column 2': [],
    'Date': [],
    'Text': [],
    'Transaction ID': [],
    'Date 2': []
}

# Regex patterns
date_pattern = r'\d{2}/\d{2}/\d{2}'
text_pattern = r'[A-Z]+-\d+-[A-Z\s\d]+'
transaction_id_pattern = r'\d{12}'

# Iterate through all the pages and extract text
for page_num in range(pdf_document.page_count):
    page = pdf_document.load_page(page_num)
    text = page.get_text()

    lines = text.split('\n')
    for line in lines:
        if re.search(date_pattern, line):
            # Split by spaces and handle each part
            parts = line.split()
            # Column 1 and Column 2
            column_1 = parts[0]
            column_2 = parts[1]

            # Date, Text, Transaction ID
            date_match = re.search(date_pattern, line)
            text_match = re.search(text_pattern, line)
            transaction_id_match = re.search(transaction_id_pattern, line)
            
            date = date_match.group() if date_match else ''
            text = text_match.group() if text_match else ''
            transaction_id = transaction_id_match.group() if transaction_id_match else ''

            # Date 2
            date2 = parts[-2] if len(parts) > 3 else ''

            # Append to data dictionary
            data['Column 1'].append(column_1)
            data['Column 2'].append(column_2)
            data['Date'].append(date)
            data['Text'].append(text)
            data['Transaction ID'].append(transaction_id)
            data['Date 2'].append(date2)

# Close the PDF document
pdf_document.close()

# Create a DataFrame
df = pd.DataFrame(data)

# Save to Excel
excel_output_path = '/content/drive/MyDrive/HACKATHON/test5_extracted_data.xlsx'
df.to_excel(excel_output_path, index=False)

print(f"Extracted data saved to '{excel_output_path}'")
Explanation of the Script
Authentication and Google Drive Setup:

The script uses the PyDrive library to authenticate and create a Google Drive client.
The credentials.json file is required for authentication.
The script downloads the specified PDF file from Google Drive.
PDF Text Extraction:

The PyMuPDF library is used to read the text content from the PDF file.
The text is split into lines for easier processing.
Data Extraction:

Regular expressions are used to identify and extract specific patterns from the text, such as dates, transaction IDs, and amounts.
The extracted data is stored in a dictionary, which is then converted to a pandas DataFrame.
Saving to Excel:

The pandas DataFrame is saved to an Excel file using the to_excel method.
Contributing
If you would like to contribute to this project, please follow these steps:

Fork the repository.
Create a new branch (git checkout -b feature-branch).
Make your changes and commit them (git commit -am 'Add new feature').
Push to the branch (git push origin feature-branch).
Create a new Pull Request.
License
This project is licensed under the MIT License. See the LICENSE file for details.
