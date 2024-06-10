# Document converter


This microservice is intended to generate the final document after translation and digitization. This currently supports `pdf`, `txt`, `xlsx` document generation.

- **API Contract:** [here](#)
- **Code:** [here](#)

## Modules

### Converter Module

#### DocumentConverter

API to create digitized `txt` & `xlsx` files for Translation Flow. RBAC enabled.

**Mandatory parameters:** `record_id`, `user_id`, `file_type`

**Actions:**

- Validating input params as per the policies
- Page data is converted into dataframes
- Writing the data into file and storing them on Samba store

<details>
<summary>DocumentConverter CURL Request</summary>

```bash
curl --location --request POST 'http://localhost:5001//anuvaad-etl/document-converter/v0/document-converter' \
--header 'Content-Type: application/json' \
--data-raw '{ 
  "record_id":"A_OD10GV-IVRCU-1617009019569%7C0-16170090212740283.json", 
  "user_id":"d4e0b570-b72a-44e5-9110-5fdd54370a9d", 
  "file_type":"txt" 
}'
```
</details>

#### DocumentExporter

API to create digitized `txt` & `pdf` files on Document Digitization flow. RBAC enabled.

**Mandatory parameters:** `record_id`, `user_id`, `file_type`

**Actions:**

- Validating input params as per the policies
- Generating the docs using ReportLab
- Writing the data into file and storing them on Samba store

<details>
<summary>DocumentExporter CURL Request</summary>

```bash
curl --location --request POST 'http://localhost:5001//anuvaad-etl/document-converter/v0/document-exporter' \
--header 'Content-Type: application/json' \
--data-raw '{ 
  "record_id":"A_OD10GV-IVRCU-1617009019569%7C0-16170090212740283.json", 
  "user_id":"d4e0b570-b72a-44e5-9110-5fdd54370a9d", 
  "file_type":"txt" 
}'
```
</details>

