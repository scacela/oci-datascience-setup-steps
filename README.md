# oci-datascience-setup-steps
Configure your Oracle Cloud Infrastructure (OCI) Tenancy for Data Science Platform usage.
## Sections
- [Identity and Access Management Setup](#identity-and-access-management)
- [Data Science Notebook Session Setup](#data-science-notebook-session-setup)
- [Verify Network Connectivity to Object Storage](#verify-network-connectivity-to-object-storage)
- [Verify Network Connectivity to Autonomous Data Warehouse (ADW)](#verify-network-connectivity-to-autonomous-data-warehouse-adw)

### Identity and Access Management Setup
1. Create a Compartment to isolate Data-Science-related resources. A compartment is a logical container to which a tenancy administrator can grant Groups and Oracle resources access. Copy the Compartment OCID and paste it onto a text editor where it can be easily accessed in a later step.

2. Create a Group intended for those who should be able to access resources within the Compartment you created in `Step 1.`, and add users to the Group. Copy the Group Name and paste it onto a text editor where it can be easily accessed in a later step.

3. Create a Policy object within the parent Compartment of the Compartment you created in `Step 1.`. Replace all instances of `COMPARTMENT_ID` and `GROUP_NAME` with the Compartment OCID and Group Name you pasted into your text editor, respectively.

```
Allow any-user to manage data-science-family in compartment id COMPARTMENT_ID where all{any{request.principal.type='datasciencenotebooksession', request.principal.type='datasciencejobrun'}, request.principal.compartment.id='COMPARTMENT_ID'}

Allow service datascience to use virtual-network-family in compartment id COMPARTMENT_ID

Allow service FaaS to use virtual-network-family in compartment id COMPARTMENT_ID

Allow any-user to use object-family in compartment id COMPARTMENT_ID where all{any{request.principal.type='datasciencenotebooksession', request.principal.type='datasciencejobrun'}, request.principal.compartment.id='COMPARTMENT_ID'}

Allow any-user to use autonomous-database-family in compartment id COMPARTMENT_ID where all{any{request.principal.type='datasciencenotebooksession', request.principal.type='datasciencejobrun'}, request.principal.compartment.id='COMPARTMENT_ID'}

Allow group GROUP_NAME to manage all-resources in compartment id COMPARTMENT_ID
```

### Data Science Notebook Session Setup

Create a Data Science Project and Notebook Session within the project. Select `Default Networking` when designing the Notebook Session instance.

### Verify Network Connectivity to Object Storage
We will verify network connectivity to Object Storage by importing a dataset from and exporting a dataset to an existing Object Storage bucket deployment, in that order.
- If you do not have an Object Storage bucket, and wish to test this connectivity, please create an Object Storage bucket within the compartment you created [earlier](#identity-and-access-management-setup).
- If you do not have a dataset in your Object Storage bucket, and wish to test this connectivity, feel free to use this [Iris](https://objectstorage.us-ashburn-1.oraclecloud.com/p/PjnbwhhdixsEAnKuWJJ5a5YdcUgXBK0X8wvkbRJOQAstqJD9Ov1yUDEYfSv8-H7Z/n/orasenatdpltintegration03/b/samcac/o/Iris.csv) dataset.

1. Open your Notebook Session
2. Open the Launcher with `File > New Launcher`, and select `Environment Explorer`. Locate the Conda Environment with the slug name `dataexpl_p3_cpu_v2`, and copy the installation command includd in the tile for this Conda Environment.
3. Open Terminal with `File > New Terminal`, paste the installation command into the command prompt in Terminal, and press Enter to install the Conda Environment, following the prompts with default responses.
4. Once the Conda Environment installation has completed, run the following command to install a notebook template, which we will use to connect to Object Storage:
```
wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/FP6mD8uri0D-IQNcjNTK5MK-aGELa4YpZoZ-3Kgj65E2nJLnL0UPPwgiyqgIqFzV/n/orasenatdpltintegration03/b/samcac/o/ImportExport_ObjS.ipynb
```
5. Open the notebook named `ImportExport_ObjS.ipynb`, and select the Conda Environment you previously installed. Note that it may take a few minutes for the Conda Environment to appear as an option.
6. Replace the indicated placeholder values in the `ImportExport_ObjS.ipynb` notebook with values corresponding to your Object Storage bucket deloyment.

### Verify Network Connectivity to Autonomous Data Warehouse (ADW)
We will verify network connectivity to ADW by importing a dataset from and exporting a dataset to the ADMIN schema in an existing ADW deployment, in that order.
- If you do not have an ADW, and wish to test this connectivity, please create an ADW within the compartment you created [earlier](#identity-and-access-management-setup).
- If you do not have a dataset in your ADW ADMIN schema, and wish to test this connectivity, feel free to use this [Iris](https://objectstorage.us-ashburn-1.oraclecloud.com/p/PjnbwhhdixsEAnKuWJJ5a5YdcUgXBK0X8wvkbRJOQAstqJD9Ov1yUDEYfSv8-H7Z/n/orasenatdpltintegration03/b/samcac/o/Iris.csv) dataset.

1. Open your Notebook Session
2. Open the Launcher with `File > New Launcher`, and select `Environment Explorer`. Locate the Conda Environment with the slug name `dataexpl_p3_cpu_v2`, and copy the installation command includd in the tile for this Conda Environment.
3. Open Terminal with `File > New Terminal`, paste the installation command into the command prompt in Terminal, and press Enter to install the Conda Environment, following the prompts with default responses.
4. Once the Conda Environment installation has completed, run the following command to install a notebook template, which we will use to connect to ADW:
```
wget https://objectstorage.us-ashburn-1.oraclecloud.com/p/VwIIUvgakVY3mmn-0Et-LHFp5R2L6961-CJKuuwT2G6rbrtgt5GD5djeWzkoW36R/n/orasenatdpltintegration03/b/samcac/o/ImportExport_ADW.ipynb
```
5. Open the notebook named `ImportExport_ADW.ipynb`, and select the Conda Environment you previously installed. Note that it may take a few minutes for the Conda Environment to appear as an option.
6. Replace the indicated placeholder values in the `ImportExport_ADW.ipynb` notebook with values corresponding to your ADW deloyment.
