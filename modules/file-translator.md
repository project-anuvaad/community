# File translator

This microservice is served with multiple APIs to transform the data in the file to form JSON file and download the translated files of type DOCX, PPTX, and HTML.

## Modules

### Workflow Code

#### WF_A_FTTKTR

**Steps:**

1. **Transformation Flow**  
   - Use the data in DOCX, PPTX, or HTML file to create a JSON file.
2. **Tokenizer Flow**  
   - Read the JSON file created in the Transformation Flow and tokenize each paragraph.
   - Tokenization is a process where we extract all the sentences in a paragraph.
3. **Translation Flow**  
   - Translate each sentence.

#### WF_S_FT

**Steps:**

1. **WF_A_FTTKTR Flow**  
   - This flow must be completed before calling the download flow.
2. **Download Flow**  
   - Fetch content for the file, replace original sentences with translated ones, and download the file.

### Through WF Manager

#### Transform Flow

**Mandatory Params for File Translator:**

- `Path`
- `Type`
- `Locale`

**Actions:**

1. Validate input parameters.
2. Generate a JSON file from the data of the given file.
3. Convert the given file to HTML, PDF, and push it to S3 (For showing it on UI).
4. Get the S3 link of the converted file and call content handler API to store the link.

<details>
<summary>Transform Flow CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate' \
--header 'auth-token: AUTHTOKEN' \
--header 'content-type: application/json' \
--data-raw '{
   "workflowCode": "WF_A_FTTKTR",
   "jobName": "HTML FILE.html",
   "jobDescription": "",
   "files": [
       {
           "path": "f3cf11bd-c6b8-4ea2-9bd2-9828b9847c8a.html",
           "type": "html",
           "locale": "en",
           "model": {...},
           "context": "JUDICIARY",
           "modifiedSentences": "A"
       }
   ]
}'
```
</details>

#### Download Flow

**Mandatory Params for File Translator:**

- `Path`
- `Type`
- `Locale`

**Actions:**

1. Validate input parameters.
2. Call fetch-content to get the translation of the file passed in the param.
3. Replace the original text in the file with the translated text.
4. Return the path of the translated file.

<details>
<summary>Download Flow CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/sync/initiate' \
--header 'auth-token: AUTHTOKEN' \
--header 'Content-Type: application/json' \
--data-raw '{
   "workflowCode": "WF_S_FT",
   "jobName": "ch 2 communication skills.docx",
   "jobDescription": "",
   "files": [
       {
           "path": "A_FTTTR-RJjbi-1623847596274|DOCX-8f8c43a9-ac35-407f-874e-51d91be7f433.json",
           "type": "json",
           "locale": "en",
           "model": {...},
           "context": "JUDICIARY",
           "modifiedSentences": "A"
       }
   ]
}'
```
</details>
