---
description: Frequently asked questions about Anuvaad
---

# FAQ

## FAQ

<details>

<summary>1. How to host Anuvaad locally?</summary>

Setting up the whole application locally is recommended as there are 10+ modules, some of which are resource-demanding. However, individual modules can be run locally for development purposes.

**General Guidelines:**

* Clone the repository and navigate to the module-specific directory.
* Run `pip3 install -r requirements.txt`.
* Make necessary changes to config files with respect to MongoDB and Kafka.
* Run `python3 src/app.py`.

**Alternatively,** modules can be run by building and running Docker images. Ensure configs and ports are configured as per your local setup:

* `docker build -t <service-name> .`
* `docker run -r <service-name>`

Apart from this, the Docker images running in the user's environment can be found here: [Anuvaad Docker Hub](https://hub.docker.com/u/anuvaadio).

</details>

<details>

<summary>2. How to make a contribution?</summary>

Fork the repo, make the necessary feature, and create a PR. We will review and merge it. Post queries in the discussions/issues section.

</details>

<details>

<summary>3. How to contact maintainers for credentials?</summary>

Check discussions or reach out to  nlp-nmt@tarento.com

</details>

<details>

<summary>4. Can I use individual modules of Anuvaad rather than the whole application?</summary>

Yes, refer to the documentation and KT of the specific module.

</details>

<details>

<summary>5. How to use Anuvaad features from my code?</summary>

Refer to the API specifications.

</details>

<details>

<summary>6. I need assistance in setting up Anuvaad in my organization's infrastructure. Who can help?</summary>

Reach out to nlp-nmt@tarento.com or feel free to raise a request in the discussions section.

</details>

<details>

<summary>7. Are there any videos of Anuvaad usage and codebase available?</summary>

Yes, they are available here: https://anuvaad.sunbird.org/engage/kt-videos

</details>
