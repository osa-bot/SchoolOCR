# SchoolOCR

---

![License](https://img.shields.io/github/license/LISA-ITMO/SchoolOCR?style=flat&logo=opensourceinitiative&logoColor=white&color=blue)
[![OSA-improved](https://img.shields.io/badge/improved%20by-OSA-yellow)](https://github.com/aimclub/OSA)

---

## Overview

SchoolOCR aims to automate the grading of exams and assessments by intelligently extracting information from scanned documents. It tackles the challenge of accurately reading both printed and handwritten answers, especially within tables – a common difficulty for standard OCR software. The system works by first preparing images (even those originating as PDFs), then using a combination of technologies like optical character recognition and specialized machine learning models to identify key details such as student information, subject names, and answer choices. 

A core goal is reliable score calculation based on recognized digits within answer grids. It’s designed to handle the specific structure of test sheets, focusing on extracting data from headers and participant codes. The system isn't meant to be perfect; it flags potentially inaccurate readings for review, ensuring a balance between automation and accuracy. Ultimately, SchoolOCR seeks to significantly reduce manual grading effort while providing a robust solution for processing large volumes of assessments.

---

## Repository content

The SchoolOCR repository implements a service for automated recognition of data from school exam title sheets. It functions as an API endpoint that receives images (or PDFs) and returns structured information like subject, grade, variant, participant code, and task scores in JSON format.

The core components work together as follows:

1.  **API (app.py):** This is the entry point for external requests. It handles image decoding, API key validation, calls various processing functions, and returns results.
2.  **Image Processing Utilities:** Several utility modules (`utils/`) handle tasks like general preprocessing, code recognition, table recognition, and PDF conversion. These prepare images for analysis.
3.  **OCR Models:** The system utilizes multiple OCR models:
    *   **Tesseract OCR:** Used for recognizing printed text, particularly in the document's header.
    *   **MNIST Model (cnn_train/mnist_train.py & .keras/.h5 files):** A convolutional neural network trained to recognize handwritten digits, crucial for identifying scores on answer sheets.
    *   **YOLO Models (.pt files):** Used for detecting cells within tables, aiding in table structure recognition.
4.  **Configuration Files (config.json, api_keys.json):** These files store parameters like API keys, region coordinates for extracting specific information from the image (header, code area, table), and configurations for table recognition based on subject and grade.
5. **PDF Handling:** The system can directly process PDF documents by converting them into images using PyMuPDF (`fitz`).

The workflow involves receiving an image, preprocessing it, extracting relevant regions (header, code, table), applying appropriate OCR models to each region, parsing the results, and returning a structured JSON response.  The configuration files guide this process, defining where to look for information within the image and how to interpret it. The system also includes error handling and validation mechanisms to improve accuracy.

---

## Used algorithms

The SchoolOCR project utilizes a combination of algorithms for automated document analysis and grading. 

**Optical Character Recognition (OCR):** This is the foundational technology used to convert images of text into machine-readable text data. Specifically, Tesseract OCR is mentioned as being utilized for recognizing printed characters.

**Deep Learning Models (YOLO):** YOLO (You Only Look Once) is employed for object detection within the images, likely to identify specific regions like answer tables or key document elements such as subject names and variant numbers.

**Keras-based Digit Recognition:** Custom deep learning models built with Keras are used specifically to recognize handwritten digits. This is crucial for scoring answers in multiple-choice or numerical response sections of tests.

**Image Processing Techniques:** A variety of image processing methods are applied to enhance the quality of images before analysis. These techniques likely include noise reduction, contrast adjustment, and skew correction to improve OCR accuracy.

**Table Segmentation:** Algorithms dedicated to identifying and isolating table structures within the document images. This is essential for accurately extracting data from tabular answer keys or student responses.

**API Key Authentication:** A security algorithm that verifies the identity of users accessing the system through unique API keys, controlling access to the OCR service.

**Data Validation & Correction:** Mechanisms are in place to check the accuracy of recognized data and allow for corrections. This likely involves comparing extracted information against expected formats or using rule-based checks.

---
