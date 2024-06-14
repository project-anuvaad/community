# Service Contracts

**In Anuvaad All the service are talked through Workflow-manager by respective workflow configs defined for each service and communicating between these service are done by means of Kafka. Each services has its own functionality and are not dependent to predecessor services outputs.** **Two Main Flows:**

**1. Translation - Single/Block Translation or Document (.pdf , .docx , .pptx) Translation.**

**2. Digitization - OCR on (.pdf or Images).**

### [WORKFlow-MANAGER](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-workflow-mgr)

[WFM-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-workflow-mgr/docs/etl-wf-manager-api-contract.yml)

[WFM-KAFKA-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-workflow-mgr/docs/etl-wf-manager-kafka-contract.yml)

[WFM-CONFIGS](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-workflow-mgr/config/etl-wf-manager-config-dev.yml)

WFM is the backbone service of the Anuvaad system, it is a centralized orchestrator which directs the user input through the dataflow pipeline to achieve the desired output. It maintains a record of all the jobs and all the tasks involved in each job. WFM is the SPOC for the clients to retrieve details, status, error reports etc about the jobs executed (sync/async) in the system. Using WFM, we’ve been able to use Anuvaad not just as a Translation platform but also as an OCR platform, Tokenization platform, Sentence Alignment platform for dataset curation. Every use-case in Anuvaad is defined as a ‘Workflow’ in the WFM, These workflow definitions are in the form of a YAML file, which is read by WFM as an external configuration file.

### [USER-MANAGEMENT](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-api/anuvaad-user-management/user-management)

[USER-MAGAGEMENT-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-api/anuvaad-user-management/user-management/docs/ums\_contract.yaml)

This microservice is served with multiple APIs to manage the User and Admin side functionalities in Anuvaad.

## Translation

if pdf document:

FILE-UPLOADER -> FILE-CONVERTER -> BLOCK-MERGER -> TOKENISER -> TRANSLATOR -> CONTENT-HANDLER

if docx and pptx:

FILE-UPLOADER -> FILE-TRANSLATOR -> TOKENISER -> TRANSLATOR -> CONTENT-HANDLER

## Digitization

V1.0

FILE-UPLOADER -> FILE-CONVERTER -> GOOGLE-VISION-OCR -> OCR-TOKENISER -> OCR-CONTENT-HANDLER

V1.5

FILE-UPLOADER -> FILE-CONVERTER -> WORD-DETECTOR -> LAYOUT-DETECTOR -> BLOCK-SEGMENTER -> GOOGLE-VISION-OCR -> OCR-TOKENISER -> OCR-CONTENT-HANDLER

V2.0

FILE-UPLOADER -> FILE-CONVERTER -> WORD-DETECTOR -> LAYOUT-DETECTOR -> BLOCK-SEGMENTER -> TESSERACT-OCR -> OCR-TOKENISER -> OCR-CONTENT-HANDLER

### [FILE-UPLOADER](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/file-uploader)

[FILE-UPLOADER-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/file-uploader/docs/file-handler-api-contract.yml)

The User Uploads the file and in return the file will be stored in the samba share for further api's to access them.

### [FILE-CONVERTER](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/file-converter)

[FILE-CONVERTER-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/file-converter/docs/pdf-converter-api-contract.yml)

This micro service, which in turn is a Kafka consumer service, consumes the input files and converts them into PDF. Best results are obtained only for the file formats supported by Libreoffice.

**If Document format is .pdf then Block-merger will be used for OCR on the document.**

### [BLOCK-MERGER](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/block-merger)

[BLOCK-MERGER-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/block-merger/docs/block-merger-api-contarct.yml)

It is used to extract text from a digital document in a structured format(paragraph,image,table) which is then used for translation purposes.

**If Document format is .docx or .pptx then File-Translator service will be used.**

### [FILE-TRANSLATOR](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/file\_translator)

This microservice is served with multiple APIs to transform the data in the file to form JSON file and download the translated files of type docx, pptx, and html.

### [TOKENISER](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/sentence)

[TOKENISER-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/sentence/docs/sentence-api-contarct.yml)

This service is used to tokenise the input paragraphs received into independently translatable sentences which can be consumed by downstream services to translate the entire input.Regular expressions and specific libraries such as Nltk are being used to build this tokeniser.

### [TRANSLATOR](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-translator)

Translator receives input from the tokeniser module, the input is a JSON file that contains tokenised sentences. These tokenised sentences are extracted from the JSON file and then sent to NMT over kafka for translation. NMT expects a batch of ‘n’ sentences in one request, Translator created ‘m’ no of batches of ‘n’ sentences each and pushes to the NMT input topic. In parallel it also listens to the NMT’s output topic to recieve the translation of the batches sent. Once, all the ‘m’ no of batches are received back from the NMT, the translation of the document is marked complete. Next, Translator appends these translations back to the JSON file received from Tokeniser, The whole of this JSON which is now enriched with translations against every sentence is pushed to Content Handler via API, Content Handler then stores these translations.

### [CONTENT-HANDLER](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/content-handler)

[CONTENT-HANDLER](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/content-handler/docs/content-handler-api-contract.yml)

This microservice is served with multiple APIs to handle and retrieve back the contents (final result) of files translated in the Anuvaad system.

### [WORD-DETECTOR](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/document-processor/word-detector/craft)

[WORD-DETECTOR-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/document-processor/word-detector/craft/doc/word-detector-craft-api-contract.yml)

Input as pdf or image If input is pdf , then convert pdf into images Use custom prima line model to line detection in the image Return list of pages and each page includes a list of lines, it also includes page information(page path, page resolution).

### [LAYOUT-DETECTOR](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/document-processor/layout-detector/prima)

[LAYOUT-DETECTOR-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/document-processor/layout-detector/prima/doc/page-layout-prima-api-contract.yml)

Output of word detector as an input. Use a prima layout model for layout detection in the image. Layout classes: Paragraph, Image, Table, Footer, Header, Maths formula Return list of pages and each page includes a list of layouts and list of lines.

### [BLOCK-SEGMENTER](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/document-processor/block-segmenter)

[BLOCK-SEGMENTER-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/document-processor/block-segmenter/docs/block-sementer-api-contract.yml)

Output of layout detector as an input. Collation of line and word at layout level

### [GOOGLE-VISION-OCR](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/document-processor/gv-document-digitization)

[GOOGLE-VISION-OCR-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/document-processor/gv-document-digitization/doc/gv-document-digitize-api-contract.yaml)

Output of block segmenter as an input. Use google vision as OCR engine. Text collation at word,line and paragraph level.

### [TESSERACT-OCR](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/document-processor/ocr/tesseract)

[TESSERACT-OCR-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/document-processor/ocr/tesseract/doc/ocr-tesseract-api-contract.yml)

Output of block segmenter as an input. Use Anuvaad ocr model as OCR engine. Text collation at word,line and paragraph level.

### [OCR-TOKENISER](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/sentence\_ocr/sentence)

[OCR-TOKENISER-CONTRCAT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/sentence\_ocr/sentence/docs/sentence-api-contarct.yml)

This service is used to tokenise the input paragraphs received into independently translatable sentences which can be consumed by downstream services to translate the entire input.Regular expressions and specific libraries such as Nltk are being used to build this tokeniser.

### [OCR-CONTENT-HANDLER](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/ocr-content-handler)

[OCR-CONTENT-HANDLER-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/ocr-content-handler/docs/ocr-contenthandler-api-contract.yml)

This microservice is served with multiple APIs to handle and manipulate the digitized data from anuvaad-gv-document-digitize which is part of the Anuvaad system. This service is functionally similar to the Content Handler service but differs since the output document (digitized doc) structure varies.

### [ALIGNER](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/aligner)

[ALIGNER-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/aligner/docs/etl-aligner-api-contract.yml)

This Module is for “aligning” or simply, finding similar sentence pairs from two lists of sentences, preferably in different languages. The service is dependent on the file uploader and workflow manager (WFM) services. Aligner service is based on Google’s LaBse model and FB’s FAISS algorithm. It accepts two files as inputs, from which two lists of sentences are collected. LaBse Embeddings are calculated for each of the sentences in the list. Cosine similarity between embeddings is calculated to find meaningfully similar sentence pairs. Faiss algorithm is used to fasten up the whole process dramatically.

### [DOCUMENT-CONVERTER](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/document-converter)

[DOCUMENT-CONVERTER-CONTRACT](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/document-converter/docs/document-converter-api-contract.yml)

This microservice is intended to generate the final document after translation and digitization. This currently supports pdf, txt, xlsx document generation.
