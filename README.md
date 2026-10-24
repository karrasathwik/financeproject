# financeproject
Finance Data Lakehouse on Azure Databricks
An Azure data engineering project for bringing financial data from different sources into one lakehouse, checking its quality, and preparing it for reporting and analysis.

The design uses SQL Server, PostgreSQL and Azure Blob Storage as sources. Data is processed in Azure Databricks with Python, PySpark and Delta Lake, following Bronze, Silver and Gold layers.
Architecture

Download the architecture image generated for this project, name it finance-lakehouse-architecture.png, and add it to the repository's images folder so GitHub displays it here.

Project objective
Financial data often arrives from multiple systems and file extracts. This project brings those sources into a consistent structure, with an auditable path from raw data to reporting-ready outputs.

Example finance datasets include transactions, invoices, payments, accounts and customer records. The exact data model and metrics depend on the source datasets available.
Technology stack
Area	Technology	Purpose
Sources	SQL Server, PostgreSQL, Azure Blob Storage	Relational data and incoming files
Ingestion	JDBC, Databricks Auto Loader	Reading databases and processing new files
Transformation	Azure Databricks, Python, PySpark	Data cleaning and transformation
Storage	Delta Lake	Bronze, Silver and Gold datasets
Orchestration	Databricks Jobs and Workflows	Scheduling and managing pipeline tasks
Metadata and auditing	Azure SQL Database	Configuration and execution records
Governance	Unity Catalog	Access management
Monitoring	Data quality checks, email alerts	Detecting problems and notifying the team
Reporting	Databricks Dashboards and Genie	Exploring curated finance data


Data flow
1. Ingestion
- SQL Server and PostgreSQL: Ingest structured data through JDBC.
- Azure Blob Storage: Use Databricks Auto Loader for new files.
- Load patterns: Support initial full loads and incremental processing where the source provides reliable change tracking.
Source settings and run details can be recorded in Azure SQL Database for easier configuration and auditing.
2. Bronze — raw data
Keep ingested records in Delta format with minimal modification. This makes it easier to trace information back to its source and reprocess data when rules change.
3. Silver — validated data
Use PySpark to clean and standardise records before reporting. Typical checks include:
- Required fields, null values and duplicate business keys
- Data types, dates, amount precision and currency formats
- Source-to-target record count checks
- Finance-specific rules for payment and invoice statuses
Rejected or unexpected records should be logged for investigation rather than silently removed.
4. Gold — reporting data
Create curated datasets for downstream analysis. Depending on the available finance data, these can support transaction trends, invoice ageing, payment status, account activity and monthly reporting.

Databricks Dashboards and Genie can use these outputs for visualisation and data exploration.
Orchestration and monitoring
Databricks Jobs and Workflows coordinate ingestion and transformation tasks. Azure SQL Database holds configuration and audit information. Data quality checks and email notifications help identify failed runs and problematic records.

A useful audit record includes the source, run ID, start and end times, load type, status, record count and error details.
Governance and security
Unity Catalog helps organise data assets and manage access. Credentials should be stored securely and never committed to GitHub. Use synthetic or anonymised records when demonstrating financial or customer data in a public repository.
How to run
The exact steps depend on the notebooks and Azure resources in your implementation:
1. Configure connections to SQL Server, PostgreSQL and Azure Blob Storage.
2. Set up the necessary Unity Catalog permissions and storage locations.
3. Configure source metadata and full or incremental load options.
4. Deploy the Databricks notebooks and Jobs or Workflows.
5. Run ingestion, followed by the Bronze, Silver and Gold transformations.
6. Review pipeline output, audit logs and data quality checks before reporting.
Testing
- Run a full load followed by an incremental load.
- Rerun processing and verify it does not create unintended duplicates.
- Test missing and invalid values against validation rules.
- Trigger a controlled failure and check the audit entry and email notification.
- Check access permissions for different roles.
- Reconcile record counts and relevant financial totals between stages.
Possible improvements
- Expand automated tests and recovery procedures.
- Add more detailed performance and cost monitoring.
- Build additional Gold-layer finance measures.
- Document source-specific incremental load handling.
The project is intended to demonstrate the full data engineering lifecycle: ingestion, transformation, orchestration, data quality, governance and reporting.