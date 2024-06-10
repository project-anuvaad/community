# Aligner


The **Aligner** module is designed for “aligning” or finding similar sentence pairs from two lists of sentences, preferably in different languages. The Aligner is a standalone service that cannot be accessed from the UI as of now. The service is dependent on the **file uploader** and **workflow manager (WFM)** services.

The Aligner service is based on **Google’s LaBSE model** and **FB’s FAISS algorithm**. It accepts two files as inputs, from which two lists of sentences are collected. LaBSE Embeddings are calculated for each of the sentences in the list. Cosine similarity between embeddings is calculated to find meaningfully similar sentence pairs. The FAISS algorithm is used to dramatically speed up the whole process.

### Simplified implementation: [Here](#)

The service accepts two text files, and the aligner module can ideally be invoked using WFM. It is time-consuming and hence an async service. Once the run is fully done, a WFM-based search can be conducted using the job ID to obtain the result.

The response is typically a **JSON file path**, which can be downloaded using the download API. The JSON file is self-explanatory and it contains `source_text`, `target_text`, and the corresponding cosine similarity between them.

## Local Setup (Without WFM & Uploader)

1. Clone the Repo
2. Install dependencies
    ```bash
    pip install -r requirements.txt
    ```
3. Run the application
    ```bash
    python app.py
    ```
4. Access from local:

<details>
<summary>Aligner CURL Request</summary>

```bash
curl --location --request POST 'http://127.0.0.1:5001/anuvaad-etl/extractor/aligner/v1/sentences/align' \
--header 'Content-Type: application/json' \
--data-raw '{
    "source": {
        "filepath": "/home/test.en",
        "locale": "en",
        "type": "json"
    },
    "target": {
        "filepath": "/home/test.ml",
        "locale": "ml",
        "type": "json"
    }
}'

</details>

<details>
<summary>Search Jobs in Local</summary>

```bash
curl --location --request GET 'http://127.0.0.1:5001/anuvaad-etl/extractor/aligner/v1/alignment/jobs/get/ALIGN-1614743930159'
```
</details>

## Remote (Invoked via WFM)

<details>
<summary>Initiate Workflow</summary>

```bash
curl --location --request POST 'https://stage-auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/async/initiate' \
--header 'Content-Type: application/json' \
--header 'auth-token: {{auth-token}}' \
--header 'context:' \
--data-raw '{
    "workflowCode": "WF_A_JAL",
    "files": [
        {
            "locale": "ml",
            "path": "983da7e1-7cde-4091-8db4-cf845b5ea3c3.txt",
            "type": "txt"
        },
        {
            "locale": "en",
            "path": "aab70b95-ec0d-4c1c-9bfe-0c4864aecda0.txt",
            "type": "txt"
        }
    ]
}'
```
</details>

It returns a JOB ID, which can be searched using the WFM Bulk search API to see job progress and pull out results once done.

<details>
<summary>Search Bulk Workflow Jobs</summary>

```bash
curl --location --request POST 'https://stage-auth.anuvaad.org/anuvaad-etl/wf-manager/v1/workflow/jobs/search/bulk' \
--header 'auth-token: {{auth-token}}' \
--header 'Content-Type: application/json' \
--data-raw '{
    "jobIDs": [
        "{{jobIDs}}"
    ],
    "taskDetails": false
}'
```
</details>

- `WF_A_JAL` is the Workflow code for JSON-based aligner, which returns the filepath of a JSON file that could be downloaded using the download API.
- `WF_A_AL` is the old workflow code, that returns multiple txt files.

## Testing

1. Upload two files.
2. Call API endpoint with file paths as parameters.
3. Verify if sentences are matching properly in the JSON.

## Notes

- Can be used as an independent service by deploying file-uploader and aligner modules alone on a server, preferably GPU-based (tested working well on `g4dn2xlarge`).
- Simplified implementations of the aligner could be found [here](#).
- An explanatory article could be found [here](#) and [here](#).
