# The Data Engineering Lifecycle

1. We divide the data engineering lifecycle into five stages
   * Generation
   * Storage
   * Ingestion
   * Transformation
   * Serving data
2. Ensure a standard approach for implementing business logic across your transformations
3. **Reverse ETL** takes processed data from the output side of the data engineering lifecycle and feeds it back into source systems
4. The gist is that transformed data will need to be returned to source systems in some manner, ideally with the correct lineage and business process associated with the source system
5. Data engineering now encompasses far more than tools and technology. The field is now **moving up the value chain**, incorporating traditional enterprise practices such as data management and cost optimization and newer practices like DataOps
6. Data security is also about **timing**—providing data access to exactly the people and systems that need to access it and **only for the duration necessary** to perform their work.
7. First, data is increasingly stored in the cloud. This means we have pay-as-you-go storage costs instead of large up-front capital expenditures for an on-premises data lake
8. **Data products** differ from **software products** because of the way data is used. A software product provides specific functionality and technical features for end users. By contrast, a data product is built around sound business logic and metrics, whose users make decisions or build models that perform automated actions.
9. DataOps has three core technical elements: automation, monitoring and observability, and incident response
10. directionally correct -- “方向正确”，学习新词汇
11. Operational metadata describes the operational results of various systems and includes statistics about processes, job IDs, application runtime logs, data used in a process, and error logs. Orchestration systems can provide **a limited picture of operational metadata**, but the latter still tends to be scattered across many systems.
12. The process for converting data into a usable form is known as data modeling and design
13. **Data processing frameworks** such as Spark can ingest a whole spectrum of data, from flat structured relational records to raw unstructured text
14. It’s also imperative that a data engineer understand proper code-testing methodolo‐ gies, such as unit, regression, integration, end-to-end, and smoke.
15. (about open-source framework): This new generation of open source tools assists engineers in managing, enhancing, connecting, optimizing, and monitoring data
16. Airflow dominated the orchestration space from 2015 until the early 2020s. Now, a new batch of open source competitors (including Prefect, Dagster, and Metaflow) has sprung up to fix perceived limitations of Airflow, providing better metadata handling, portability, and dependency management
17. (About streaming processing): Engineers must also write code to apply a variety of windowing methods. Windowing allows real-time systems to calculate valuable metrics such as trailing statistics.
18. Pipelines as code: Data engineers use code (typically Python) to declare data tasks and dependencies among them. The orchestration engine interprets these instructions to run steps using available resources