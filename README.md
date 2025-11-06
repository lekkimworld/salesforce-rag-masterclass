# Salesforce RAG Masterclass
This is the metadata and data for the RAG Masterclass.

## Installation Steps
To prepare for running the exercise in a new org follow the below steps. I've recorded a Youtube video on how to do the setup.

[![Watch the video on how to setup your org](https://raw.githubusercontent.com/lekkimworld/salesforce-rag-masterclass/main/assets/video/thumbnail.png)](https://youtu.be/8I2-nongG5g)

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
7. Salesforce no longer (as of November 2025) creates a default Retriever. To create one - a REALLY BAD one to form the base of the exercise - one go to Einstein Studio and create a new Individual retriever as follows:
    1. Select "Data Cloud" for the type,  `default` as the data space, `Insurance_Policy__c_c_Home` for the data model object and `Insurance_Policy__c_c_Home` for the DMO search index. Click Next.
    2. Select "All Documents" and click Next.
    3. On the "Configure Retriever Results" page add the following fields (Direct attributes of `Insurance_Policy__c_c_Home`) and then click Next:
        * `Record ID`
        * `Data Source`
        * `Data Source Object`
    4. Click Save
8. Activate the retriever you just created
9. Add the retriever you created for the search index into the `Insurance Coverage Verification` prompt template where it says `RETRIEVER`. Add the Free Text as the retriever input and return all 3 fields from the retriever.
