# Analytics

### Analytics

When Anuvaad is implemented at an organizational level, **analytics** is crucial for tracking usage and metrics. A dedicated module exists to serve this purpose.

**Code Repository:** [Anuvaad Metrics](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-api/anuvaad-metrics/anuvaad-org-judgement-count)

**API Contract:** [Metrics API Contract](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/project-anuvaad/anuvaad/master/anuvaad-api/anuvaad-metrics/anuvaad-org-judgement-count/docs/metrics\_contract.yaml)

For every X hours, a cron job creates a CSV file, and analytics are drawn based on it for time-consuming computations. For other metrics, data is fetched directly from the database. The following analytics are currently available, with room for more metrics to be visualized:

1.  **Total Documents Translated, Language-wise**\


    <figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Total Documents Translated, Language-wise in SUVAS</p></figcaption></figure>
2.  **Organization-wise Sentences Translated**\


    <figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Organization-wise Sentences Translated in SUVAS </p></figcaption></figure>
3.  **Organization-wise Dashboard**\


    <figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption><p>SUVAS Organization-wise Dashboard</p></figcaption></figure>
4.  **Reviewer Metrics**\


    <figure><img src="../.gitbook/assets/image (3).png" alt=""><figcaption><p>SUVAS Reviewer Metrics</p></figcaption></figure>
