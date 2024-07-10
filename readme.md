##Purpose:
This Python script extracts structured data from a PDF file and saves it into an Excel spreadsheet. It utilizes PyMuPDF for PDF handling and pandas for data manipulation.

##Features:
PDF Parsing: The script reads a specified PDF file (test3.pdf in this case) using PyMuPDF, extracting text from each page.

##Data Extraction:

**Field and Value Extraction:** It identifies fields and their corresponding values by splitting lines based on colon (:) where possible.

**Amount Extraction:** It extracts amounts with decimals and commas from lines using regular expressions.

**Amounts with "Dr":** It captures amounts followed by "Dr" for financial transactions.

**Transaction Type:** It identifies transaction types ('C' for Credit, 'T' for Debit) and extracts text following them.

**Important Dates:** It captures dates in the format DD-MMM-YYYY from the text.

**Transaction IDs:** It identifies transaction IDs, typically numeric sequences of 10 digits or more.

**Data Organization:** Extracted data is stored in a pandas DataFrame, which allows for easy manipulation and analysis.

**Output:** The processed data is saved into an Excel file (test3_extracted_text_final_v7.xlsx), preserving the structure and details extracted from the PDF.

##Usage:
Ensure Python environment has necessary libraries installed: fitz, pandas, and re.

Replace pdf_path variable with the path to your PDF file.

Run the script to extract data.

Check the generated Excel file for extracted and organized data (test3_extracted_text_final_v7.xlsx).

##Notes:
Ensure the PDF file is accessible and correctly specified in pdf_path.

The script uses regular expressions to handle varying formats of data entries (amounts, dates, transaction IDs, etc.).

Adjustments may be needed based on specific PDF formats and data extraction requirements.
