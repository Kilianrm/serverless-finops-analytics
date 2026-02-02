# Serverless FinOps Analytics Pipeline

## OVERVIEW

This project demonstrates how to build a serverless, event-driven data pipeline on AWS to analyze cloud costs using custom FinOps business rules.

Instead of relying solely on native AWS cost dashboards, the pipeline treats cloud cost data as events, enabling custom classification, enrichment, and analytics tailored to a specific workload or organization.

The final output is an analytics-ready dataset stored in Amazon S3 and queried using Amazon Athena.


## ARCHITECTURE

The pipeline follows an event-driven, serverless architecture:

EventBridge (scheduled)
  -> Lambda: Cost Ingest
      -> S3 (raw cost events)
          -> Lambda: Cost Processor
              -> S3 (processed cost events)
                  -> Athena

Key characteristics:
- Fully serverless
- Loosely coupled components
- Clear separation between raw and processed data
- Designed with cost-awareness in mind


DATA FLOW

1. Ingestion
- A scheduled EventBridge rule triggers the ingestion Lambda.
- Cost events are generated (simulated) using a realistic cloud cost schema.
- Events are stored unmodified in the raw S3 layer.

2. Processing
- A second Lambda processes raw events.
- FinOps-specific business rules are applied.
- Enriched and classified events are written to the processed S3 layer.

3. Analytics
- Processed data can be queried using Athena.
- Enables cost trend analysis, anomaly detection, and optimization insights.


## FINOPS BUSINESS RULES (v1)

The initial version of the pipeline applies a small set of realistic FinOps rules:

- Cost classification
  Normal vs spike based on historical baseline thresholds

- Tagging validation
  Detection of missing or incomplete resource tags

- Low-value cost detection
  Identification of small but recurring costs that may indicate waste

Each processed event includes a rules_version field to allow future evolution of business logic without breaking historical data.

## PROJECT STRUCTURE

This repository contains the initial structure for a serverless data pipeline
built on AWS. 

- infra/: Infrastructure as Code definitions
- app/: AWS Lambda functions
- docs/: Architecture and design documentation
- sample-data/: Example input and output data

The project is intentionally minimal at this stage and will evolve iteratively.


## COST CONSIDERATIONS

This project is designed to remain within the AWS Free Tier where possible.

- AWS Lambda with short execution times
- Amazon S3 for low-cost storage
- Amazon Athena used only for ad-hoc queries

Cost optimization is considered both as a project constraint and as part of the business logic itself.


## SCOPE AND LIMITATIONS

- This project does not aim to replace AWS Cost Explorer or native AWS billing tools.
- Cost data is simulated, but follows realistic formats inspired by AWS Cost and Usage Reports.
- The focus is on architecture, data processing, and FinOps reasoning, not on dashboards or UI.


## GOALS

- Demonstrate event-driven, serverless data processing on AWS
- Apply domain-specific FinOps business rules to cloud cost data
- Produce analytics-ready datasets for cost analysis and optimization
- Showcase clean architecture, cost awareness, and cloud best practices


## FUTURE IMPROVEMENTS

- Additional FinOps rules and aggregations
- More advanced anomaly detection
- Daily and monthly cost summaries
- Optional visualization layer


## DISCLAIMER

This is a personal portfolio project created for learning and demonstration purposes.
