---
description: Technology Stack
---

# Technology Stack



| Component                                 | Details                                                                                               |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| [Apache Kafka](https://kafka.apache.org/) | Translator and [OpenNMT](https://opennmt.net/) are integrated through Kafka messaging.                |
| [MongoDB](https://www.mongodb.com/)       | Primary data storage.                                                                                 |
| [Redis](https://redis.io/)                | Secondary in memory storage.                                                                          |
| Cloud Storage                             | Samba storage is used to store user input files.                                                      |
| [NGINX](https://www.nginx.com/)           | Serve as a redirection server and also takes care of system level configs. Ngnix acts as the gateway. |
| [Zuul](https://github.com/Netflix/zuul)   | API Gateway to apply filters on client requests,authenticate,authorize,throttle client requests.      |
