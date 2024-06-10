# Block merger

This microservice is used to extract text from a digital document in a structured format (paragraph, image, table), which is then used for translation purposes.

## Architecture

- It takes an image or pdf as an input.
- If the input is a pdf, it converts the pdf into images.
- The `pdftohtml` tool is used to extract page-level information like text, word coordinates, page width, page height, tables, images, and others.
- If the document language is vernacular, the `pdftohtml` tool does not work well, so we use Tesseract (Google Vision) for OCR.
- Horizontal merging is used to get lines using word coordinates.
- Vertical merging is used to get blocks using line coordinates.
- The final JSON contains page-level information like page width, page height, paragraphs, lines, words, and layout class. 

- **API Contract:** [here](#)
- **Code location:** [here](#)


## Modules

### API Details

#### Local Testing

**URL:** [http://0.0.0.0:5001/anuvaad-etl/block-merger/v0/merge-blocks](http://0.0.0.0:5001/anuvaad-etl/block-merger/v0/merge-blocks)

**Input:**

```json
{
    "input": {
        "files": [
            {
                "locale": "en",
                "path": "1.pdf",
                "type": "pdf"
            }
        ]
    },
    "jobID": "BM-15913540488115873",
    "state": "INITIATED",
    "status": "STARTED",
    "stepOrder": 0,
    "workflowCode": "abc",
    "tool": "BM",
    "metadata": { 
        "module": "WORKFLOW-MANAGER",
        "receivedAt": 15993163946431696,
        "sessionID": "4M1qOZj53tIZsCoLNzP0oP",
        "userID": "d4e0b570-b72a-44e5-9110-5fdd54370a9d"
    }
}
```

Here it takes a PDF or image path as an input and the language of that document.

#### Workflow Initiate

**URL:** [https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate](https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate)

**Input:**

```json
{
    "workflowCode": "WF_A_BM",
    "files": [
        {
            "path": "763b0d80-4e82-423f-a432-23ddffe5ad92.pdf",
            "type": "pdf",
            "locale": "en"
        }
    ]
}
```

#### Steps:

1. **Upload a PDF or image file using the upload API:**

    **Upload URL:** [https://auth.anuvaad.org/anuvaad-api/file-uploader/v0/upload-file](https://auth.anuvaad.org/anuvaad-api/file-uploader/v0/upload-file)

2. **Get the upload ID and copy that to the path of wf-initiate input of the block merger.**

3. **Do bulk search using jobIDs to get JSON ID of the BM service response:**

    **Bulk search URL:** [https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/jobs/search/bulk](https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/jobs/search/bulk)

    **Bulk search input format:**

    ```json
    {
        "jobIDs": ["A_B-MtrjS-1640069221694"],
        "taskDetails": true
    }
    ```

4. **Download JSON using download API:**

    **Download URL:** [https://auth.anuvaad.org/download/0-1640069280533983.json](https://auth.anuvaad.org/download/0-1640069280533983.json)
