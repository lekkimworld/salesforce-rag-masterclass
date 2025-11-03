# Salesforce RAG Masterclass
This is the metadata and data for the RAG Masterclass.

## Installation Steps
To prepare for running the exercise in a new org follow the below steps.

1. Add metadata and data (add any org into `TARGET_ORG` e.g. `TARGET_ORG="-o myorg"`)
```bash
TARGET_ORG=""
sf project deploy start $TARGET_ORG
sf org permset assign -n Insurance_Policy_Access $TARGET_ORG
sf data import bulk -s Insurance_Policy__c -f ./data/insurance_policies.csv  -w 10 --line-ending=LF $TARGET_ORG
```
2. Ensure Data 360 has access to the `Insurance Policies` custom object through the `Data Cloud Salesforce Connector` permission set
3. Ingest data using a Data Stream in Data 360 using `Other` as the type
4. Map the new DLO to a new custom DMO using standard mappings
5. Once data has been ingested (18 records) create a new Vector search index using the `Easy Setup` mode
6. Wait for the search index to be built and indexing to complete
7. Add the default retriever created for the search index into the `Insurance Coverage Verification` prompt template
