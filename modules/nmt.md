# NMT

The **NMT** module is responsible for the translation of sentences. It can be invoked directly or via the Workflow Manager. The NMT module works in correlation with the ETL Translator to enhance translation efficiency based on previous translations or pre-provided glossary and TMX support (refer to other sections). The module supports batch inferencing and provides APIs that return model details for language and other dropdown menus.

### Initial Version

In the early days of Anuvaad, **OpenNMT-py** based models trained on Anuvaad's proprietary data were used. These models were primarily focused on judicial content. The inference code for this initial version is available here: [OpenNMT-py Inference Code](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-nmt-inference).

### Intermediary Version

With the collaboration between Anuvaad and [**Ai4Bharat**](https://ai4bharat.iitm.ac.in/) , data from Anuvaad and other sources were used to publish the **Samanantar** paper (https://arxiv.org/abs/2104.05596). Using the Samanantar dataset, **IndicTrans**, a more general domain model, was trained. This model performed well for legal use cases, leading to the replacement of OpenNMT with [IndicTrans](https://github.com/AI4Bharat/IndicTrans2) . The IndicTrans-based inferencing code is available here: [IndicTrans Inference Code](https://github.com/project-anuvaad/aaib4-inference/tree/main).

### Current Version

As the **Sunbird ecosystem** developed, the need for hosting multiple ML models independently became resource-intensive. This led to the development of [**Dhruva**](https://github.com/AI4Bharat/Dhruva-Platform), a centralized platform for hosting models. Applications can now utilize models from Dhruva using APIs. In Dhruva, models are wrapped with **NVIDIA Triton**, facilitating a scalable architecture. The IndicTrans model was moved to Dhruva, and currently, models are invoked from Dhruva via wrapper APIs from the NMT module rather than using dedicated inference. The Dhruva-ported code is available here: [Dhruva Ported Code](https://github.com/project-anuvaad/aaib4-inference/tree/main).

***
