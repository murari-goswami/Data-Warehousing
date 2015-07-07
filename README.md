## Applicata – Case Study Solution (Detail Level Design)

Get Summarized and aggregated Visits Information in Table – across four major dimension

#### Problem Statement: - Raw data are available for visitors Click stream Data in the Fact table named VISITS.

CONNECT_AWS_STORAGE schema. A master COUNTRY table in COMMON schema holds the look up information on county details. The requirement is to populate the appropriate non duplicate data in the dimension tables viz. – DOMAIN table in BASE schema, DEVICE_TYPE and OPERATING_SYSTEM table in COMMON schema and at the end create a summarized tables from VISITS across 4 dimesnions.

#### Technical Solution:

A stored procedure is written in the CONNECT_AWS_STORAGE schema named -

CONNECT_AWS_STORAGE.TRANSFORM with two input parameters as “from date” and “until date”. The procedure loops through the selection criteria and in for loop calls three modular separate functions. These are stored as separate encapsulation utility.

1. The first procedure function is CONNECT_AWS_STORAGE.DIM_DOMAIN_INS insert the domain details which is stripped out from the FACT table column – LANDING_URL and then validated through the COUNTRY table details from the COMMON schema. If the domain details are not present in the DOMAIN table in BASE schema, the function will insert a row in the BASE table. This will return the ID which will be updated to the DOMAIN_ID column in the FACT table VISITS. Hence complete a cycle of updating the dimension table and confirming back to the FACT table. This loop will continue for all DISTINCT domain id stripped out from the Fact table.

The use OF SQL FACTORING WITH clause is used to fine the tune the insertion and update process.

2. The second procedure function is CONNECT_AWS_STORAGE.DIM_COMMON_DEVICE_TYPE_UPD. This procedure checks for all DISTINCT device from the FACT table and looks in the DEVICE_TYPE table in COMMON schema if the device detail is already present in this dimension. In case there are no match i.e. the device detail is not present, then the function will add a record in the DEVICE_TYPE table. This will return the ID which will be updated in the FACT as per the previous routine. The loop will continue for all the DISTICT device from the Fact table.

The use OF SQL FACTORING WITH clause is used to fine the tune the insertion and update process.

3. The third procedure function is CONNECT_AWS_STORAGE.DIM_COMMON_OPERATING_SYSTEM_UPD.

This procedure function checks for DISTINCT OPERATING_SYSTEM from the Fact table and looks in the OPERATING_SYSTEM table in common schema if the operating system detail is present in this dimension. In case there are no match i.e. the operating system is not present, then the function will add a record in the OPERATING_SYSTE< table. This will return ID which will be updated in the FACT as per the previous routine. The loop will continue for all the DICTINCT operating system from the FACT table.

The use OF SQL FACTORING WITH clause is used to fine the tune the insertion and update process.

Once all the update done, a summary table will be created named as CONNECT_AWS_STORAGE.VISITS_CLEANED.

However before the extraction begins, GIN B-tree index is created on DOMAIN_ID, DEVICE_TYPE_ID, OPERATING_SYSTEM_ID to get a fast extraction from the VISITS table.

Select the updated columns from the FACT table grouping across DOMAIN_ID, DEVICE_TYPE_ID, OPERATING_SYSTEM_ID, and DATE to reflect the count of records for these verticals.
