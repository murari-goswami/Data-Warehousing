Applicata – Case Study Solution (Detail Level Design)

Get Summarized and aggregated Visits Information in Table – across four major dimension

Problem Statement: - Raw data are available for visitors Click stream Data in the Fact table named VISITS in the

CONNECT_AWS_STORAGE schema. A master COUNTRY table in COMMON schema holds the look up information on county details. The requirement is to populate the appropriate non duplicate data in the dimension tables viz. – DOMAIN table in BASE schema, DEVICE_TYPE and OPERATING_SYSTEM table in COMMON schema and at the end create a summarized tables from VISITS across 4 dimesnions.

Technical Solution:

A stored procedure is written in the CONNECT_AWS_STORAGE schema named -

CONNECT_AWS_STORAGE.TRANSFORM with two input parameters as “from date” and “until date”. The procedure loops through the selection criteria and in for loop calls three modular separate functions. These are stored as separate encapsulation utility.

1. The first procedure function is CONNECT_AWS_STORAGE.DIM_DOMAIN_INS insert the domain details which is stripped out from the FACT table column – LANDING_URL and then validated through the COUNTRY table details from the COMMON schema. If the domain details are not present in the DOMAIN table in BASE schema, the function will insert a row in the BASE table. This will return the ID which will be updated to the DOMAIN_ID column in the FACT table VISITS. Hence complete a cycle of updating the dimension table and confirming back to the FACT table. This loop will continue for all DISTINCT domain id stripped out from the Fact table.

The use OF SQL FACTORING WITH clause is used to fine the tune the insertion and update process.
