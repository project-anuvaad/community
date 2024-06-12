# Document Digitization

<figure><img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXdLr95e_bBkqYMvWo8RyV2_NMgNTCwC17gLh4a_MPNCyEsF7xQ0fKVDvhqsCS6ZIVc6Q08iY6fC-nD2YHTR8DARlAQzBNJrey4MtdWnAj8IE96ahZKSeARgg8ZtglkRiHfmlOEIqpkUIibBNdzxwyaOR-so?key=JvwCIxJ4D3_YxZg0iSqTdA" alt=""><figcaption><p>Documet Digitization</p></figcaption></figure>

This pipeline is used to extract text from a digital/scanned document. Lines and layouts (header, footer, paragraph, table, cell, image) are detected by a custom-trained Prima layout model and OCR is done using the Anuvaad OCR model.

**Github repo:** [Anuvaad Document Processor](https://github.com/project-anuvaad/anuvaad/tree/develop/anuvaad-etl/anuvaad-extractor/document-processor)

**API contract:** [API Contract](https://github.com/project-anuvaad/anuvaad/blob/develop/anuvaad-etl/anuvaad-extractor/document-processor/ocr/ocr-tesseract-server/doc/google-vision-api-contract.yml)

## How to Use

1.  **Upload a PDF or image file using the upload API:**

    **Upload URL:** [https://auth.anuvaad.org/anuvaad-api/file-uploader/v0/upload-file](https://auth.anuvaad.org/anuvaad-api/file-uploader/v0/upload-file)

    Get the upload ID and copy it to the DD2.0 input path.
2.  **Initiate the Workflow:**

    **WF URL:** [https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate](https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate)

    **DD2.0 Input:**

    ```json
    {
        "files": [
            {
                "locale": "language",
                "path": "file_name",
                "type": "file_format",
                "config": {
                    "OCR": {
                        "option": "HIGH_ACCURACY",
                        "language": "language"
                    }
                }
            }
        ],
        "workflowCode": "WF_A_FCWDLDBSOD20TESOTK"
    }
    ```

## Microservices

### Word Detector

* **Input:** PDF or image
* **Output:** List of pages with detected lines and page information.

<figure><img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXekE4Zz0G__TeyG_JG67w98bDuNVsIcHDaVCTKD3jgTE-E7BiKg5gMUWaad5kfk8H7yGkqZGoeKQPa7smrDIQ-MnhB1Mnb8A4WAWAK3VmO6zhvXpzZspRqwLSdHZNHXF4r3cfaT9jcuoNGUV7zrtC5-hoaj?key=JvwCIxJ4D3_YxZg0iSqTdA" alt=""><figcaption><p>sample</p></figcaption></figure>

**Github repo:** [Word Detector Craft](https://github.com/project-anuvaad/anuvaad/tree/develop/anuvaad-etl/anuvaad-extractor/document-processor/word-detector/craft)

**API contract:** [Word Detector API Contract](https://github.com/project-anuvaad/anuvaad/blob/develop/anuvaad-etl/anuvaad-extractor/document-processor/word-detector/craft/doc/word-detector-craft-api-contract.yml)

<details>

<summary>How to use: Word Detector</summary>

1.  **Upload a PDF or image file using the upload API:**

    **Upload URL:** [https://auth.anuvaad.org/anuvaad-api/file-uploader/v0/upload-file](https://auth.anuvaad.org/anuvaad-api/file-uploader/v0/upload-file)
2.  **Initiate the Word Detector Workflow:**

    **WF URL:** [https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate](https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate)

    **Word Detector Input:**

    ```json
    {
        "files": [
            {
                "locale": "language",
                "path": "file_name",
                "type": "file_format",
                "config": {
                    "OCR": {
                        "option": "HIGH_ACCURACY",
                        "language": "language"
                    }
                }
            }
        ],
        "workflowCode": "WF_A_WD"
    }
    ```

</details>

### Layout Detector

* **Input:** Output of word detector
* **Output:** List of pages with detected layouts and lines.

<figure><img src="https://lh7-us.googleusercontent.com/docsz/AD_4nXcpfqBrfX8OiFT1mghhzudtUBBADa-kEgPrnQeXIMmKskaurmUZq6MNW_Sw-srnupPjHC1ss22dP9hllflBneXcFp_NE5raHaGbpjWAMBG3MPfwNg_jFVhx7lo8xeY8v507Mh26RqnbAGUWZrTR4WWRKnHx?key=JvwCIxJ4D3_YxZg0iSqTdA" alt=""><figcaption><p>sample</p></figcaption></figure>

**Github repo:** [Layout Detector Prima](https://github.com/project-anuvaad/anuvaad/tree/develop/anuvaad-etl/anuvaad-extractor/document-processor/layout-detector/prima)

**API contract:** [Layout Detector API Contract](https://github.com/project-anuvaad/anuvaad/blob/develop/anuvaad-etl/anuvaad-extractor/document-processor/layout-detector/prima/doc/page-layout-prima-api-contract.yml)

<details>

<summary>How to use: Layout Detector</summary>

1. **Input JSON file of the word detector as an input path.**
2.  **Initiate the Layout Detector Workflow:**

    **WF URL:** [https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate](https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate)

    **Layout Detector Input:**

    ```json
    {
        "files": [
            {
                "locale": "language",
                "path": "word_detector_output",
                "type": "json",
                "config": {
                    "OCR": {
                        "option": "HIGH_ACCURACY",
                        "language": "language"
                    }
                }
            }
        ],
        "workflowCode": "WF_A_LD"
    }
    ```

</details>

### Block Segmenter

* **Input:** Output of layout detector
* **Output:** Collation of line and word at layout level.

**Github repo:** [Block Segmenter](https://github.com/project-anuvaad/anuvaad/tree/develop/anuvaad-etl/anuvaad-extractor/document-processor/block-segmenter)

**API contract:** [Block Segmenter API Contract](https://github.com/project-anuvaad/anuvaad/blob/develop/anuvaad-etl/anuvaad-extractor/document-processor/block-segmenter/docs/block-sementer-api-contract.yml)

<details>

<summary>How to use: Block Segmenter</summary>

1. **Input JSON file of the layout detector as an input path.**
2.  **Initiate the Block Segmenter Workflow:**

    **WF URL:** [https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate](https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate)

    **Block Segmenter Input:**

    ```json
    {
        "files": [
            {
                "locale": "language",
                "path": "layout_detector_output",
                "type": "json",
                "config": {
                    "OCR": {
                        "option": "HIGH_ACCURACY",
                        "language": "language"
                    }
                }
            }
        ],
        "workflowCode": "WF_A_BS"
    }
    ```

</details>

### Google OCR

* **Input:** Output of block segmenter
* **Output:** Text collation at word, line, and paragraph level using Google Vision as the OCR engine.

**Github repo:** [OCR Google Vision Server](https://github.com/project-anuvaad/anuvaad/tree/develop/anuvaad-etl/anuvaad-extractor/document-processor/ocr/ocr-gv-server)

**API contract:** [Google Vision API Contract](https://github.com/project-anuvaad/anuvaad/blob/develop/anuvaad-etl/anuvaad-extractor/document-processor/ocr/ocr-gv-server/doc/google-vision-api-contract.yml)

<details>

<summary>How to use: Google OCR</summary>

1. **Input JSON file of the block segmenter as an input path.**
2.  **Initiate the Google OCR Workflow:**

    **WF URL:** [https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate](https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate)

    **Google OCR Input:**

    ```json
    {
        "files": [
            {
                "locale": "language",
                "path": "block_segmenter_output",
                "type": "json",
                "config": {
                    "OCR": {
                        "option": "HIGH_ACCURACY",
                        "language": "language"
                    }
                }
            }
        ],
        "workflowCode": "WF_A_OTES"
    }
    ```

</details>

### Tesseract OCR

* **Input:** Output of block segmenter
* **Output:** Text collation at word, line, and paragraph level using Anuvaad OCR model.

**Github repo:** [OCR Tesseract Server](https://github.com/project-anuvaad/anuvaad/tree/develop/anuvaad-etl/anuvaad-extractor/document-processor/ocr/ocr-tesseract-server)

**API contract:** [Google Vision API Contract](https://github.com/project-anuvaad/anuvaad/blob/develop/anuvaad-etl/anuvaad-extractor/document-processor/ocr/ocr-tesseract-server/doc/google-vision-api-contract.yml)

<details>

<summary>How to use: Tesseract OCR</summary>

1. **Input JSON file of the block segmenter as an input path.**
2.  **Initiate the Tesseract OCR Workflow:**

    **WF URL:** [https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate](https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate)

    **Tesseract OCR Input:**

    ```json
    {
        "files": [
            {
                "locale": "language",
                "path": "block_segmenter_output",
                "type": "json",
                "config": {
                    "OCR": {
                        "option": "HIGH_ACCURACY",
                        "language": "language"
                    }
                }
            }
        ],
        "workflowCode": "WF_A_OD20TES"
    }
    ```

</details>
