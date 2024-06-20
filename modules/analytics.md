# Analytics

### Analytics

When Anuvaad is implemented at an organizational level, **analytics** is crucial for tracking usage and metrics. A dedicated module exists to serve this purpose.

**Code Repository:** [Anuvaad Metrics](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-api/anuvaad-metrics/anuvaad-org-judgement-count)

**API Contract:** [Metrics API Contract](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/project-anuvaad/anuvaad/master/anuvaad-api/anuvaad-metrics/anuvaad-org-judgement-count/docs/metrics\_contract.yaml)

For every X hours, a cron job creates a CSV file, and analytics are drawn based on it for time-consuming computations. For other metrics, data is fetched directly from the database. The following analytics are currently available, with room for more metrics to be visualized:

1. **Total Documents Translated, Language-wise**
2. **Organization-wise Sentences Translated**
3. **Organization-wise Dashboard**
4. **Reviewer Metrics**
