## **Challenges in a Normal Day-to-Day Data Platform**

> --- ***Too Many Tools and Integration Issues***

Organizations often use a multitude of separate tools for different tasks, such as data warehousing, ETL (Extract, Transform, Load), running Spark jobs, saving data to data lakes, orchestration, AI/ML solutions, and BI (Business Intelligence) reporting.

Each of these tools needs to integrate properly with one another to function effectively. For instance, if a BI dashboard doesn't integrate with a data warehousing solution, proper results cannot be obtained.

Furthermore, governance (handling security, lineage, and metadata) must work across all these tools; otherwise, data leaks and security issues can arise.
The complexity and challenges increase significantly when dealing with numerous individual tools.

---

> --- ***Proprietary Solutions or Vendor Lock-in***

Many data warehousing solutions are proprietary, meaning they require data to be stored in their specific, encoded format.
This creates a vendor lock-in, preventing direct communication with or extraction of data without using the vendor's data engine.
If the vendor's solution is not used, accessing and reading the data becomes impossible.

Databricks addresses this by providing open-source solutions. Data can reside in an organization's data cloud platform in open-source formats like Parquet, CSV, or Avro.

On top of this, Databricks uses an open-source data engine called Delta Lake, which communicates with the data. This allows users the freedom to switch vendors if desired, as their data remains accessible in an open format within their data lake, ensuring no vendor lock-in.

---

> --- ***Data Silos***

Traditional platforms often have separate data lakes (used for AI/ML solutions and ETL jobs) and data warehousing solutions (used by BI tools).
This leads to duplicate copies of data. Data is often moved from the data lake to populate a separate data warehouse, resulting in the same data existing in different places, possibly with different owners.

Maintaining multiple copies by different owners is a significant challenge. Databricks tackles this by merging the data lake and data warehouse into a Data Lakehouse

**------------------------------------------------------------------------------------------------------------**

## **The Data Lakehouse**

A datalakehouse is new, open data management architecture that combines the flexibity, cost efficiency and scale of datalakes with data management and ACID transactions of Data Warehouses, enabling business intelligence and machine learning on all data.

---

> --- ***Databricks Data Intelligence Platform***

The Databricks Data Intelligence Platform is defined as Data Lakehouse plus Generative AI.
Generative AI provides the platform with its power for natural language and allows enterprises to gain insights from their enterprise data.
Therefore, Databricks is called a data intelligence platform because it combines the benefits of a Data Lakehouse with Generative AI capabilities.

---

> --- ***Delta Lakehouse features***

1. Handles All Types of Data
2. Cheap Cloud Object Storage
3. Uses Open File Format
4. Support for All Workloads
5. Direct BI Integration
6. ACID Support & Version History
7. Improved Performance
8. Simple Architecture

---

> --- ***Disadvantages for datawarehouse***

1. Increased Data Volume & Variety
2. Longer Time to Ingest New Data
3. Proprietary Data Formats /Vendor Lock-in
4. Scalability Issues
5. High Storage Costs
6. Limited Support for Advanced Analytics

---

> --- ***Disadvantages for datalake***

1. No Support for ACID Transactions(Atomicity, Consistency, Isolation, and Durability)
2. Partial Data Loads
3. Inconsistent Reads
4. GDPR Challenges(general data protection regulation)
5. Complicated Data Corrections
6. No Rollback Capability
7. Poor BI Performance and Support
8. Complex to set-up
9. No Data Versioning
10. Streaming vs. Batch Processing

**------------------------------------------------------------------------------------------------------------**

## **Databricks Lakehouse Platform Architecture**

![Steps](databricksarc.svg)

The high-level architecture of Databricks consists of two main parts:

> --- ***Control Plane***

The control plane is managed by Databricks and resides within the Databricks cloud account. Its primary purpose is to manage Databricks' backend services. It also handles information like notebook configurations, cluster configurations, job information, and logs required to manage the data plane. The main purpose of the control plane is to orchestrate and provide configurations necessary to run jobs, clusters, and code.

> --- ***Data/Compute Plane(Serverless)***

The data plane resides within the customer's cloud account.
Client data always resides in the customer's cloud account within the data plane, never at the control plane.
Clusters created to process this data are also created and run within the customer's cloud account (data plane). These clusters are managed by configurations from the control plane.

Processed data is saved back to the client's cloud account only. There is no data movement to the control plane; configurations and access are managed by the control plane, but data remains in the data plane. If a cluster needs to connect to external data sources (e.g., MySQL, a different data lake), it will connect and process that data within the data plane.

**------------------------------------------------------------------------------------------------------------**

## **Databricks Workspace Components**

Databricks is composed of several main components that work together to provide a comprehensive data analytics and AI platform.

> --- ***Apache Spark***

At its core, Databricks is built on Apache Spark, a powerful open-source, distributed computing
system that provides fast data processing and analytics capabilities. Databricks enhances Spark with optimized
performance and additional features.

---

> --- ***Databricks Workspace***

This is a collaborative environment where data scientists, data engineers, and analysts
can work together. It includes interactive notebooks (supporting languages like Python, Scala, SQL, and R),
dashboards, and APIs for collaborative development and data exploration.

---

> --- ***Databricks Runtime***

A performance-optimized version of Apache Spark with enhancements for reliability and
performance, including optimizations for cloud environments and additional data sources.

---

> --- ***Delta Lake***

An open-source storage layer that brings reliability to Data Lakes. Delta Lake provides ACID
transactions, scalable metadata handling, and unifies streaming and batch data processing.

---

> --- ***Workflow***

Databricks Workflows simplify job orchestration, allowing you to create, schedule, and manage data
pipelines using a no-code or low-code interface. They support tasks like ETL, machine learning, and batch or
streaming workflows, ensuring seamless integration with Databricks Jobs and other tools.

---

> --- ***Databricks SQL***

A feature for running SQL queries on your data lakes. It provides a simple way for analysts and
data scientists to query big data using SQL, visualize results, and share insights.

---

> --- ***SQL Warehouses***

Databricks SQL Warehouse is a scalable, cloud-native data warehouse that supports
high-performance SQL queries on your data lake. It enables analytics and BI reporting with integrated tools like
Power BI and Tableau, ensuring fast, cost-efficient query execution with fully managed infrastructure.

---

> --- ***Catalog***

Databricks Catalog provides centralized governance, organizing your data and metadata.

---

> --- ***Data Integration Tools***

These allow for easy integration with various data sources, enabling users to import data
from different storage systems.

---

> --- ***Databricks compute configuration***

1. Severless

    reduce cluster start time, increase productivity ,expected lower cost due to reduced ideal time and autoscaling, maintainaing cluster for administration.

2. Classical compute (self managed)

	all purpose cluster -> created manually,persistent,suitable for interactive analytical workloads,shared among many users, expensive to run

	job cluster -> created by jobs, terminated at end of job, suitable for automated workloads, isolated just for the job, cheaper to run

Standard Access Mode clusters do not support Scala UDFs

> --- ***Databricks Cluster configuration***

1. Node type 

    Single node - not suitable for large scale workloads, incompatible with shared usage

    Multi node - shared compute is needed
2. Access mode  

    dedicated/single user - only single user access

    standard/shared - multiple user access (used for production)

    no isolation shared - multiple user access

3. Datbricks runtime  

    databricks runtime - photon(vectorized query enginer which accelerates apache spark workloads)

    databricks runtime ML

4. Auto termination

    Terminates the cluster after X minutes of inactivity

    Default value for Single Node and Standard clusters is 120 minutes

    Users can specify a value between 10 and 43200 mins as the duration

5. Autoscaling

    User specifies the min and max worker nodes

    Auto scales between min and max based on the workload

    Users can opt for spot instances(unused VM or spare capacity in the cloud) for the work

6. Cluster vm type/size

    Memory Optimized - suitable for memory intensive like ML

    Storage Optimized - high disk throuput IO

    Compute Optimized - ideal for structured streaming where peak time processing are criticial and distributed analytics

    General Purpose - enterprised grade application for analytical workloads in memory caching

    GPU Accelerated - deep learning models

7. Cluster policy

    restricted

    unrestricted

**------------------------------------------------------------------------------------------------------------**

## **Unity Catalog**

Unity Catalog is a unified governance solution for managing and securing your data assets in Databricks.

> --- ***Key features of Unity Catalog***

Databricks Catalog provides centralized governance, organizing your data and metadata.

Unity Catalog’s security model is based on standard ANSI SQL and allows administrators to grant permissions in their existing data lake using familiar syntax, at the level of catalogs, schemas (also called databases), tables, and views.

Unity Catalog automatically captures user-level audit logs that record access to your data. Unity Catalog also captures lineage data that tracks how data assets are created and used across all languages.

Unity Catalog lets you easily access and query your account’s operational data, including audit logs, billable usage, and lineage

> --- ***The Unity Catalog object model***

In Unity Catalog, all metadata is registered in a metastore. The hierarchy of database objects in any Unity Catalog metastore is divided into three levels, represented as a three-level namespace (catalog.schema.table-etc) when you reference tables, views, volumes, models, and functions.

Metastores: The metastore is the top-level container for metadata in Unity Catalog. It registers metadata about data
and AI assets and the permissions that govern access to them. For a workspace to use Unity Catalog, it must have a
Unity Catalog metastore attached.

DBRuntime requires  - 11.3 onwards to work with unity catalog

No isolation shared doesnt support unity catalog

Object hierarchy in the metastore: In a Unity Catalog metastore, the three-level database object hierarchy consists of
catalogs that contain schemas, which in turn contain data and AI objects, like tables and models.

1. Level one

    Catalogs are used to organize your data assets and are typically used as the top level in your data isolation scheme. Catalogs often mirror organizational units or software development lifecycle scopes.

    Non-data securable objects, such as storage credentials and external locations, are used to manage your data governance model in Unity Catalog. These also live directly under the metastore.

2. Level two

    Schemas (also known as databases) contain tables, views, volumes, AI models, and functions. Schemas organize data and AI assets into logical categories that are more granular than catalogs. Typically a schema represents a single use case, project, or team sandbox.

3. Level three

    Volumes is essentially a mapping between a cloud storage directory and Databricks’ Unity Catalog. It allows you to securely manage and access data stored in cloud object storage directly within Databricks. Tables are collections of data organized by rows and columns. Views are saved queries against one or more tables. Functions are units of saved logic that return a scalar value or set of rows. Models are AI models packaged with MLflow and registered in Unity Catalog as functions.

## **Auto Loader**

A new structured streaming source designed for large-scale, efficient data ingestion. Incrementally and efficiently processes new data files as they arrive in the cloud storage

Supports Azure Data Lake Storage, Amazon S3, Google Cloud Storage, Databricks File System. Supports JSON, CSV, XML, PARQUET, AVRO, ORC, TEXT, and BINARYFILE file formats

> --- ***Auto Loader features***

1. Efficient File Detection using Cloud Services
2. Scalability Improvements
3. Schema Evolution & Resiliency
4. Recommended in Lakeflow Declarative Pipelines

## **Databricks Asset Bundles**

Databricks Asset Bundles are a tool to facilitate the adoption of software engineering best practices, including source control, code review, testing, and continuous integration and delivery (CI/CD), for your data and AI projects.

Collection of:
    Code (notebooks, Python, SQL)
    Environment settings (dev, test, prod)
    Configurations for Databricks resources (jobs, pipelines, clusters, MLflow, etc.)

Represented in YAML

Deployed through CI/CD tools like GitHub Actions, Azure DevOps

Provides a standard, repeatable way to deliver Databricks projects

---

> --- ***Structure of bundle***

1. bundle → project name & metadata
2. resources → jobs, pipelines, clusters, MLflow, etc.
3. targets → environment specific settings (dev, test, prod)
4. variables → define reusable values (like parameters)
5. include → pull in configs from other files (for modular bundles)
6. run_as → specify the identity (user or service principal) to run jobs
7. artifacts → reference libraries, wheels, or other external assets
8. sync → control what gets synced between local and workspace

---

> --- ***Sample YAML***

```
# Databricks Asset Bundle for dab_demo_project
bundle:
  name: dab_demo_project

resources:
  jobs:
    dab_demo_project_job:
      name: dab_demo_project_job
      tasks:
        - task_key: dab_demo_notebook
          notebook_task:
            notebook_path: src/dab_demo_notebook.ipynb
          job_cluster_key: job_cluster

      job_clusters:
        - job_cluster_key: job_cluster
          new_cluster:
            spark_version: 15.4.x-scala2.12
            node_type_id: Standard_D3_v2
            data_security_mode: SINGLE_USER
            num_workers: 1

targets:
  dev:
    default: true
    mode: development
    workspace:
      host: https://adb-12345.14.azuredatabricks.net

  prod:
    workspace:
      host: https://adb-67890.14.azuredatabricks.net

```

---

> --- ***Databricks CLI commands***

1. databricks bundle init
Initializes a new Databricks bundle project in your working directory.

databricks bundle init
Prompts you for a template (Python, SQL, MLflow, etc.).

Creates a starter databricks.yml file and project folder structure.

2. databricks bundle validate
Validates the configuration of your bundle before deployment.

databricks bundle validate
Ensures your databricks.yml file is correctly structured.

Catches missing or invalid fields early.

Always a good practice to run before deploying.

3. databricks bundle deploy
Deploys your bundle to the Databricks workspace.

databricks bundle deploy -t dev
-t specifies the target environment (e.g. dev, test, prod).

Uploads source code and provisions jobs, clusters, pipelines, etc.

Creates a hidden .bundle folder in the workspace with your files.

4. databricks bundle run
Runs a defined workflow (job) from your bundle.

databricks bundle run my_job
Executes a specific job defined in the bundle.

Useful for testing deployments.

5. databricks bundle destroy
Removes all deployed resources from the specified target environment.

databricks bundle destroy -t dev
Cleans up jobs, clusters, and other resources created by the bundle.

Helpful when resetting environments or avoiding conflicts in shared workspaces.


## **Delta Lake**

Delta Lake is the optimized storage layer that provides the foundation for tables in a Lakehouse on Databricks. Delta Lake is open source software that extends Parquet data files with a file-based transaction log for ACID transactions and scalable metadata handling.

It allows you to use a single copy of data for both batch and streaming operations and providing incremental processing at scale.

> --- ***Delta Lake features***

1. ACID Transactions
2. Scalable Metadata
3. Time Travel
4. Simple Solution Architecture
5. Support for DML Operations
6. Better Performance
7. Open Source

---

> --- ***ACID transaction***

1. Transaction Logs are written at the end of the transaction
2. Readers will always read the transaction logs first to identify the list of data files to read

---

> --- ***Delta Lake compaction***

Process of combining many smaller files into few larger files to improve performance and

1. optimize storage.
2. Faster Reads
3. Efficient Storage

Compaction types:-
    File Compaction (OPTIMIZE Command)
    Z-Order Compaction (ZORDER BY Clause)
    Liquid Clustering (Preview Feature)
---

> --- ***Liquid Clustering***

Liquid Clustering is an innovative data layout technique in Delta Lake that automatically manages how data is organized to improve performance and cost efficiency.

Motivation for LC:-
    Hard to design right partitioning & z-ordering strategy
    Leads to data skew and concurrency conflicts
    Performance drops as data grows and workloads change
    Re-partitioning required rewriting the entire data

Data is divided into clusters based on the clustering keys. As new data arrives, databricks monitors and rebalances these clusters automatically.

No manual re-organization or z-ordering required Changing the Clustering columns doesn’t require re-writing  entire data

Benefits:-
    Faster Queries
    Automatic Optimization
    Adaptive Data Layout
    Lower maintenance cost
    Simpler Operations

## **Delta Live Tables**

Delta Live Tables is a declarative ETL framework for building reliable, maintainable, and testable data processing pipelines.

You define the transformations to perform on your data and Delta Live. Tables manages task orchestration, cluster management, monitoring,data quality, and error handling.

It handles both streaming and batch workloads with minimal manual intervention.

