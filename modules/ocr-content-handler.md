# OCR Content handler

This microservice is served with multiple APIs to handle and manipulate the digitized data from `anuvaad-gv-document-digitize`, which is part of the Anuvaad system. This service is functionally similar to the Content Handler service but differs since the output document (digitized doc) structure varies.

## Modules

### OCR Document Modules

#### DigitalDocumentSave

API to save translated documents. The JSON request object is generated from `anuvaad-gv-document-digitizer` and later updated by tokenizer. This API is being used internally.

**Mandatory parameters:** `files`, `record_id`

**Actions:**

- Validating input params as per the policies
- The document to be saved is converted into blocks of pages
- Each block contains regions such as line, word, table, etc.
- Every block is created with UUID
- Saving blocks in the database

<details>
<summary>DigitalDocumentSave CURL Request</summary>

```bash
curl --location --request POST 'http://localhost:5001//anuvaad/ocr-content-handler/v0/ocr/save-document' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "jobID": "BM-15913540488115873", 
    "state": "INITIATED", 
    "status": "STARTED", 
    "stepOrder": 0, 
    "workflowCode": "abc", 
    "taskID": "vision_ocr1615969391110792", 
    "tool": "GVOCR", 
    "message": "OCR", 
    "metadata": { 
        "module": "WORKFLOW-MANAGER", 
        "receivedAt": 15993163946431696, 
        "sessionID": "4M1qOZj53tIZsCoLNzP0oP", 
        "userID": "d4e0b570-b72a-44e5-9110-5fdd54370a9d" 
    }, 
    "files": [{ 
        "file": { 
            "identifier": "string", 
            "name": "20695.pdf", 
            "type": "json" 
        }, 
        "config": { 
            "language": "en" 
        }, 
        "pages": [ 
            { 
                "identifier": "958b00e5-7864-4a73-a3ed-7640b1c3c1cf", 
                "resolution": 300, 
                "path": "/home/naresh/anuvaad/anuvaad-etl/anuvaad-extractor/document-processor/gv-document-digitization/upload/20695_41c92afd-53fd-4446-aaee-bedd194c59cf/images/206950001-1.jpg", 
                "boundingBox": { 
                    "vertices": [{ 
                        "x": 0, 
                        "y": 0 
                    }, { 
                        "x": 2481, 
                        "y": 0 
                    }, { 
                        "x": 2481, 
                        "y": 3508 
                    }, { 
                        "x": 0, 
                        "y": 3508 
                    }] 
                }, 
                "page_no": 0, 
                "regions": [ 
                    { 
                        "identifier": "1e0f1313-4c2f-47f3-9971-a797452439f8", 
                        "boundingBox": { 
                            "vertices": [{ 
                                "x": 0, 
                                "y": 0 
                            }, { 
                                "x": 2481, 
                                "y": 0 
                            }, { 
                                "x": 2481, 
                                "y": 3508 
                            }, { 
                                "x": 0, 
                                "y": 3508 
                            }] 
                        }, 
                        "class": "BGIMAGE", 
                        "data": "/home/naresh/anuvaad/anuvaad-etl/anuvaad-extractor/document-processor/gv-document-digitization/upload/20695_41c92afd-53fd-4446-aaee-bedd194c59cf/images/206950001-1_bgimages_.jpg" 
                    }
                ]
            }
        ]
    }]
}'
```
</details>

#### DigitalDocumentUpdateWord

API to update the text in the digitized doc. RBAC enabled.

**Mandatory parameters:** `words`, `record_id`, `region_id`, `word_id`, `updated_word`

**Actions:**

- Validating input params as per the policies
- Looping over the regions to locate the word to be updated
- Updating the word and setting a flag `save=True`

<details>
<summary>DigitalDocumentUpdateWord CURL Request</summary>

```bash
curl --location --request POST 'https://auth.anuvaad.org/anuvaad/ocr-content-handler/v0/ocr/update-word' \
--header 'userID: d4e0b570-b72a-44e5-9110-5fdd54370a9d' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6ImphaW55LmpveUB0YXJlbnRvLmNvbSIsInBhc3N3b3JkIjoiYickMmIkMTIkcXFjYUM2WW5yU2RFM2hDT2h4aXpnT0ZILjBxeFR4UWJBTHloZDFjTjBFOWluSnRqaTguOWknIiwiZXhwIjoxNjE2NTcxMjM3fQ.vCOncRM7BNK0qsv0OWnioIDfy-lOusTcMERsusm_ics' \
--header 'Content-Type: application/json' \
--data-raw '{ 
    "words":[ 
        { 
            "record_id":"A_OD10GV-msJYb-1616508492867|0-1616508495552232.json", 
            "region_id":"7df5afdc-6aac-498d-af2c-e73cdf438b90", 
            "word_id":"78f682ba-5571-4099-9055-f51d4d82368a", 
            "updated_word":"Constituency" 
        }
    ] 
}'
```
</details>

#### DigitalDocumentGet

API to fetch back the document. RBAC enabled.

**Mandatory parameters:** `record_id`, `start_page`, `end_page`

**Actions:**

- Validating input params as per the policies
- Returning back the document as an array of pages

<details>
<summary>DigitalDocumentGet CURL Request</summary>

```bash
curl --location --request GET 'https://auth.anuvaad.org/anuvaad/ocr-content-handler/v0/ocr/fetch-document?recordID=A_FWLBOD15GOT-eAIRP-1632812802745%7C0-16328129323475454.json&start_page=0&end_page=0' \
--header 'auth-token: eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VyTmFtZSI6ImphaW55LmpveUB0YXJlbnRvLmNvbSIsIkphaW55QDEyMyI6ImInJDJiJDEyJFh4VU9ZbVBGZ1NyMkhuclFZNTVqR2U3a3VmUmRoakxmTTdjU2NLSkxHZVNTZkxBQmJ4UGlPJyIsImV4cCI6MTYzMzA4Mzk2OX0.-hWfzbCR7ErGjK8B8PjnkpvtVBm1Rpavmjast0E4P4I'
```
</details>
