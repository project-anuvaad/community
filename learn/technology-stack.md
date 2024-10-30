---
description: Technology Stack
---

# Technology Stack



| Component                                 | Details                                                                                               |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| [Apache Kafka](https://kafka.apache.org/) | Internal modules are integrated through Kafka messaging.                                              |
| [MongoDB](https://www.mongodb.com/)       | Primary data storage.                                                                                 |
| [Redis](https://redis.io/)                | Secondary in memory storage.                                                                          |
| Cloud Storage                             | Samba storage is used to store user input files.                                                      |
| [NGINX](https://www.nginx.com/)           | Serve as a redirection server and also takes care of system level configs. Ngnix acts as the gateway. |
| [Zuul](https://github.com/Netflix/zuul)   | API Gateway to apply filters on client requests,authenticate,authorize,throttle client requests.      |

## AI ML Assets

| Component                                                       | Details                                                       |
| --------------------------------------------------------------- | ------------------------------------------------------------- |
| [PRIMA](https://github.com/Layout-Parser/layout-model-training) | Layout detection model.                                       |
| [CRAFT](https://github.com/clovaai/CRAFT-pytorch)               | Used for Line detection.                                      |
| [Tesseract](https://github.com/tesseract-ocr)                   | Custom trained Tesseract used for OCR.                        |
| [IndicTrans2](https://github.com/AI4Bharat/IndicTrans2)         | Custom trained model used for translation.                    |
| [Dhruva](https://github.com/AI4Bharat/Dhruva-Platform)          | open-source platform for serving language AI models at scale. |
