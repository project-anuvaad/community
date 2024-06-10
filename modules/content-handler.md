# Content Handler


This microservice is served with multiple APIs to handle and retrieve the contents (final result) of files translated in the Anuvaad system.

## Modules

### Common Information

Some common info that is applicable to save, update operations on translations.

#### Workflow Code

- `WF_S_TR` and `WF_S_TKTR`: Changes the sentence structure, hence s0 pair needs to be updated.
- `DP_WFLOW_S_C`: Doesn't change the sentence structure, hence no need to update the s0 pair.

#### Sentence Keys

- `s0_src`: Source sentence extracted from the file.
- `s0_tgt`: Sentence translation from NMT.
- `tgt`: Translation updated by the user (User translation). (Source may vary if the user edits the input document, or else it keeps the same as `s0_src`).

### File Content Modules

#### SaveFileContent

API to save translated documents. The JSON request object is generated from block-merger and later updated by tokenizer and translator. This API is used internally.

**Mandatory parameters:** `userid`, `pages`, `record_id`, `src_lang`, `tgt_lang`

**Actions:**

- Validating input parameters as per the policies.
- The document to be saved is converted into blocks.
- Block can be of type images, lines, text_blocks, etc.
- Every block is created with UUID.
- Saving blocks in the database.

<details>
<summary>SaveFileContent CURL Request</summary>

```bash
curl --location --request POST 'http://gateway_anuvaad-content-handler:5001/anuvaad/content-handler/v0/save-content' \
--header 'userid: 06b5419ab0f14669b1dff654533416411608108799138' \
--header 'Content-Type: application/json' \
--data-raw '{
  "file_locale": "en",
  "record_id": "FC-BM-TOK-TRANS-1601531696387|0-16015317191287522.json",
  "src_lang": "en",
  "tgt_lang": "hi",
  "pages": [
    {
      "images": [],
      "lines": [],
      "page_height": 1188,
      "page_no": 1,
      "page_width": 918,
      "text_blocks": [
        {
          "attrib": null,
          "avg_line_height": 15,
          "block_id": "ae3165c2-03aa-11eb-a840-02420a00032e-0",
          "block_identifier": "24610b3f-c0fd-4cbf-9597-1c037e84fc70",
          "children": [
            {
              "attrib": "HEADER",
              "block_id": "ae3165c2-03aa-11eb-a840-02420a00032e-0-0",
              "children": null,
              "font_color": "#000000",
              "font_family": "ArialMT",
              "font_size": 13,
              "text": "Consulting Manager: Vijay Prasanth Sunkeswari",
              "text_height": 15,
              "text_left": 108,
              "text_top": 63,
              "text_width": 293
            }
          ],
          "data_type": "text_blocks",
          "file_locale": "68072f3c-c57a-4f62-a7fc-42ed6f776c1e",
          "font_color": "#000000",
          "font_family": "ArialMT",
          "font_size": 13,
          "job_id": "",
          "page_info": {
            "page_height": 1188,
            "page_no": 1,
            "page_width": 918
          },
          "record_id": "FC-BM-TOK-TRANS-1601531696387|0-16015317191287522.json",
          "text": " Consulting Manager: Vijay Prasanth Sunkeswari  Phone: +91-1234567898/+91-80 123456 Email:  ​ Vijay.Sunkeswari@tarento.com ",
          "text_height": 47,
          "text_left": 108,
          "text_top": 63,
          "text_width": 293,
          "tokenized_sentences": [
            {
              "input_subwords": "['▁Consult', 'ing', '▁Manager', '▁:']",
              "n_id": "FC-BM-TOK-TRANS-1601531696387|0-16015317191287522.json|1|ae3165c2-03aa-11eb-a840-02420a00032e-0",
              "output_subwords": "['▁परामर्श', '▁प्रबंधक', 'ः']",
              "pred_score": -0.8280696868896484,
              "s_id": "94695768-5976-4fdc-853d-9aa49630ce77",
              "src": "Consulting Manager:",
              "tagged_src": "Consulting Manager:",
              "tagged_tgt": "परामर्श प्रबंधकः",
              "tgt": "परामर्श प्रबंधकः"
            }
          ],
          "underline": 1
        }
      ]
    }
  ]
}'
```
</details>

#### GetFileContent

API to fetch back the documents. The response object would be an array of pages, with pagination enabled. RBAC enabled.

**Mandatory parameters:** `start_page`, `end_page`, `record_id`

**Actions:**

- Validating input parameters as per the policies.
- Fetching back the blocks as per the page number requested.

<details>
<summary>GetFileContent CURL Request</summary>

```bash
curl --location --request GET 'https://auth.anuvaad.org/anuvaad/content-handler/v0/fetch-content?record_id=A_FTTTR-GBWSA-1623682123483%7CDOCX-c7759250-6952-4575-9514-66a1383caabb.json&start_page=0&end_page=0' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6ImphaW55LmpveUB0YXJlbnRvLmNvbSIsInBhc3N3b3JkIjoiYickMmIkMTIkNzJjY1ZFRmNIcC9qSkg5dzBGMXFTdU5ZQlNXQThSMzdRak1zdm8wN01rMnNYeVI2N24xRlcnIiwiZXhwIjoxNjIzNzY5Njg0fQ.a6gaxGvG-yCLrE6qeTshf2V8j_S44-U6obgWyyHZRK8'
```
</details>

#### UpdateFileContent

API to update the block content; triggered on split, merge, re-translate operations. Used internally.

**Mandatory parameters:** `record_id`, `user_id`, `blocks`, `workflowCode`

**Actions:**

- Validating input parameters as per the policies.
- Updating the list of blocks.

<details>
<summary>UpdateFileContent CURL Request</summary>

```bash
curl --location --request POST 'http://gateway_anuvaad-content-handler:5001//anuvaad/content-handler/v0/update-content' \
--header 'userid: kd' \
--header 'Content-Type: application/json' \
--data-raw '{
  "record_id": "FC-BM-TOK-TRANS-1601531696387|0-16015317191287522.json",
  "blocks": [
    {
      "attrib": null,
      "avg_line_height": 15,
      "block_id": "ae3165c2-03aa-11eb-a840-02420a00032e-0",
      "block_identifier": "24610b3f-c0fd-4cbf-9597-1c037e84fc70",
      "children": [
        {
          "attrib": "HEADER",
          "block_id": "ae3165c2-03aa-11eb-a840-02420a00032e-0-0",
          "children": null,
          "font_color": "#000000",
          "font_family": "ArialMT",
          "font_size": 13,
          "text": "Consulting Manager: Vijay Prasanth Sunkeswari",
          "text_height": 15,
          "text_left": 108,
          "text_top": 63,
          "text_width": 293
        }
      ],
      "data_type": "text_blocks",
      "file_locale": "68072f3c-c57a-4f62-a7fc-42ed6f776c1e",
      "font_color": "#000000",
      "font_family": "ArialMT",
      "font_size": 13,
      "job_id": "",
      "page_info": {
        "page_height": 1188,
        "page_no": 1,
        "page_width": 918
      },
      "record_id": "FC-BM-TOK-TRANS-1601531696387|0-16015317191287522.json",
      "text": " Consulting Manager: Vijay Prasanth Sunkeswari  Phone: +91-1234567898/+91-80 123456 Email:  ​ Vijay.Sunkeswari@tarento.com ",
      "text_height": 47,
      "text_left": 108,
      "text_top": 63,
      "text_width": 293,
      "tokenized_sentences": [
        {
          "input_subwords": "['▁Consult', 'ing', '▁Manager', '▁:']",
          "n_id": "FC-BM-TOK-TRANS-1601531696387|0-16015317191287522.json|1|ae3165c2-03aa-11eb-a840-02420a00032e-0",
          "output_subwords": "['▁परामर्श', '▁प्रबंधक', 'ः']",
          "pred_score": -0.8280696868896484,
          "s_id": "94695768-5976-4fdc-853d-9aa49630ce77",
          "src": "Consulting Manager:",
          "tagged_src": "Consulting Manager:",
          "tagged_tgt": "परामर्श प्रबंधकः",
          "tgt": "परामर्श प्रबंधकः"
        }
      ],
      "underline": 1
    }
  ]
}'
```
</details>

#### SaveFileContentReferences

Internal API to store S3 link references to translated documents (on docx flow).

**Mandatory parameters:** `job_id`, `file_link`

**Actions:**

- Validating input parameters as per the policies.
- Storing records in the database.

<details>
<summary>SaveFileContentReferences CURL Request</summary>

```bash
curl --location --request POST 'http://gateway_anuvaad-content-handler:5001//anuvaad/content-handler/v0/ref-link/store' \
--header 'ad-userid: kd' \
--header 'userid: kd' \
--header 'Content-Type: application/json' \
--data-raw '{
  "records": [
    {
      "job_id": "abc1",
      "file_link": {
        "HTML": {
          "LIBRE": "https://anuvaad1.s3.amazonaws.com/upload/sample3tableshredacrossPages/LIBRE/sample3tableshredacrossPages.html",
          "PDFTOHTML": "https://anuvaad1.s3.amazonaws.com/upload/sample3tableshredacrossPages/PDFTOHTML/sample3tableshredacrossPages-html.html"
        },
        "PDF": {
          "LIBRE": "https://anuvaad1.s3.amazonaws.com/upload/sample3tableshredacrossPages/PDFTOHTML/sample3tableshredacrossPages.pdf"
        }
      }
    }
  ]
}'
```
</details>

#### GetFileContentReferences

API to fetch back the S3 link for docx files. RBAC enabled.

**Mandatory parameters:** `job_ids`

**Actions:**

- Validating input parameters as per the policies.
- Fetching back the data from the database.

<details>
<summary>GetFileContentReferences CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/content-handler/v0/ref-link/fetch' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6ImphaW55LmpveUB0YXJlbnRvLmNvbSIsIkphaW55QDEyMyI6ImInJDJiJDEyJDk2YzRMb0ZCTG05ZU1XVlJXNVFzTE9ydTlLZVc1emJnVnBhaFouclBuYnFReU96YUNDMFVpJyIsImV4cCI6MTY0MDY3MDg4N30.R0zEJyEeXhOZ41TnsPTD0rFov3kPmUVfL_DdOxKU0QI' \
--header 'Content-Type: application/json' \
--data-raw '{"job_ids":["A_FTTTR-cSCim-1632805831132"]}'
```
</details>

### Sentence Modules

#### SaveSentence

API to store user translations. RBAC enabled.

**Mandatory parameters:** `sentences`, `workflowCode`, `user_id`

**Actions:**

- Validating input parameters as per the policies.
- Updating the sentence blocks.
- Saved sentences are always updated with `"save": true` flag.
- Saved sentences are also saved in the Redis store for Sentence Memory.

<details>
<summary>SaveSentence CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/content-handler/v0/save-content-sentence' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6ImphaW55LmpveUB0YXJlbnRvLmNvbSIsInBhc3N3b3JkIjoiYickMmIkMTIkaXJXU2xrdjFDSWUzNzJZMzZiWlhFdTdKSDQ0QlViR2d2QlVSMW5OMXJxNEEuMWpuQ0JsTi4nIiwiZXhwIjoxNjEzNzM5NTI5fQ.g-JLNqFen-ol3y40OAFA82q1pi-b3BDSGtoWi-OyjhA' \
--header 'Content-Type: application/json' \
--data-raw '{"workflowCode":"DP_WFLOW_S_C",
"sentences": [
  {
    "bleu_score": 1,
    "n_id": "",
    "s0_src": "He was released on bail on the 1st of December. We used to go there to bail out the old man.",
    "s0_tgt": "उन्हें 1 दिसंबर को जमानत पर रिहा कर दिया गया था। हम वहां पुराने आदमी को जमानत देने जाते थे।",
    "s_id": "4e412457-e357-419b-b477-1676b314afd5",
    "save": true,
    "src": "He was released on bail on the 1st of December. We used to go there to bail out the old man.",
    "src_lang": "en",
    "tagged_src": "He was released on bail on the NnUuMm०st of December. We used to go there to bail out the old man.",
    "tagged_tgt": "उन्हें NnUuMm० दिसंबर को जमानत पर रिहा कर दिया गया था। हम वहां पुराने आदमी को जमानत देने जाते थे।",
    "tgt": "उन्हें 1 दि��ंबर को जमानत पर रिहा कर दिया गया था। हम वहां पुराने आदमी को जमानत देने जाते थे।",
    "tgt_lang": "hi",
    "time_spent_ms": 6797,
    "tmx_phrases": []
  }
]}'
```
</details>

#### FetchSentence

Bulk API to fetch back sentences. RBAC enabled.

**Mandatory parameters:** `sentences`, `record_id`, `block_identifier`, `s_id`

**Actions:**

- Validating input parameters as per the policies.
- Returning back an array of sentences searched for.

<details>
<summary>FetchSentence CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/content-handler/v0/fetch-content-sentence' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6Imt1bWFyLmRlZXBha0B0YXJlbnRvLmNvbSIsInBhc3N3b3JkIjoiYickMmIkMTIkTWVEZzhpUGY3dWJFR21jbDRaNUE3dUo0bEk4VEdJcVpzL3R4ckJZOF