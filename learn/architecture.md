---
description: Architecture of Anuvaad
---

# Architecture

The architecture is around 2 major blocks :&#x20;

* Document Digitization
* Document Translation

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt="" width="563"><figcaption><p>Document Digitization Flow</p></figcaption></figure>

<figure><img src="https://lh7-rt.googleusercontent.com/docsz/AD_4nXds1qaPJ4cBh-jW6zTz_XqXBXjQYL-dl_w9RgeONJv1fW37071qZYHvyx4KwhEN3IRojPbjkOAMsDq5DCzI-zYrjioqLOjK2BPJSQqLb-kBCh0CbbQ8yKdnqs81WQXcIANTkDzvcueprWuo_JKZnKsJpAs?key=JvwCIxJ4D3_YxZg0iSqTdA" alt=""><figcaption><p>Block Diagram</p></figcaption></figure>

#### Components

| Component                    | Details                                                                                                                                                                                                                                                                                     |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Workflow Manager(WM)         | Centralized Orchestrator based on user request.                                                                                                                                                                                                                                             |
| Auditor                      | Python package/library used for formatting , exception handling.                                                                                                                                                                                                                            |
| File Uploader                | Microservice to upload and maintain user documents.                                                                                                                                                                                                                                         |
| File Converter               | Microservice to convert files from one format to other. E.g: .doc to .pdf files.                                                                                                                                                                                                            |
| Aligner                      | Microservice accepts source and target sentances and align them to form parallel corpus.                                                                                                                                                                                                    |
| Tokenizer                    | Microservice tokenises pragraphs into independently translatable sentences.                                                                                                                                                                                                                 |
| Layout Detector              | Microservice interface for Layout detection model.                                                                                                                                                                                                                                          |
| Block Segmenter              | Handles layout detection miss-classifications , region unifying.                                                                                                                                                                                                                            |
| Word Detector                | Word detection.                                                                                                                                                                                                                                                                             |
| Block Merger                 | An OCR system that extracts texts, images, tables, blocks etc from the input file and makes it avaible in the format which can be utilised by downstream services to perform Translation. This can also be used as an independent product that can perform OCR on files, images, ppts, etc. |
| Translator                   | Translator pushes sentences to NMT module, which internally invokes IndicTrans model hosted in Dhruva to translate and push back sentences during the document translation flow.                                                                                                            |
| Content Handler              | Repository Microservice which maintains and manages all the translated documents                                                                                                                                                                                                            |
| Translation Memory X(TMX)    | System translation memory to facilitate overriding NMT translation with user preferred translation. TMX provides three levels of caching - Global , User , Organisation.                                                                                                                    |
| User Translation Memory(UTM) | System tracks and remembers individual user translations or corrected translations and applies automatically when same sentences are encountered again.                                                                                                                                     |
