# Perform a Vector Search

## Introduction

Using the in-built vector store and retrieval-augmented generation (RAG), you can load and query unstructured documents stored in Object Storage using natural language within the HeatWave ecosystem.

_Estimated Time:_ 10 minutes

### Objectives

In this lab, you will be guided through the following task:

- Create a bucket and a folder.
- Upload files to the bucket folder.
- Copy the OCID of your compartment.
- Create a dynamic group.
- Write policies for the dynamic group.
- Set up a vector store.
- Perform a vector search.

### Prerequisites

- Must complete Lab 4.

## Task 1: Create a bucket and a folder

The Object Storage service provides reliable, secure, and scalable object storage. Object Storage uses buckets to organize your files. 

1. Open the **Navigation menu** and click **Storage**. Under **Object Storage & Archive Storage**, click **Buckets**.

    ![Click bucket](./images/1-click-bucket.png "Click bucket")

2. In the **heatwave-genai** compartment, click **Create Bucket**. 

    ![Create bucket](./images/2-create-bucket.png "Create bucket")

3. Enter the **Bucket Name**, and accept the defaults for the rest of the fields.

    ```bash
    <copy>bucket-vector-search</copy>
    ```

4. Click **Create**.

    ![Enter bucket details](./images/3-enter-bucket-details.png "Enter bucket details")

5. Once the bucket is created, click the name of the bucket to open the **Bucket Details** page. 

    ![Created bucket](./images/4-created-bucket.png "Created bucket")

6. Copy the bucket name and **Namespace**, and paste the it somewhere for future reference.

    ![Bucket namespace](./images/35-bucket-namespace.png "Bucket namespace")

7. Under **Objects**, click **More Action**, and then click **Create New Folder**.

    ![Create new folder](./images/31-create-new-folder.png "Create new folder")

8. In the **Create New Folder** page, enter a **Name** of the folder, and note it for future reference.

    ```bash
    <copy>bucket-folder-heatwave</copy>
    ```

    ![Enter new folder details](./images/32-enter-folder-details.png "Enter new folder details")

## Task 2: Upload files to the bucket folder

1.  In the **Object Storage & Archive Storage** page, click the bucket name.

    ![View bucket details](./images/5-view-bucket-details.png "View bucket details")

2.  Under **Objects**, click the bucket folder name.

    ![Click bucket folder](./images/33-click-bucket-folder.png "Click bucket folder")

2.  Click **Upload**.

    ![Click upload](./images/34-click-upload.png "Click upload")

3. Click **select files** to display a file selection dialog box.

4. Select the files you want to perform a vector search on, and click **Upload**.

    ![Upload files](./images/6-upload-files.png "Upload files")

5. When the file status shows **Finished**, click **Close** to return to the bucket.

    ![Upload files finished](./images/7-upload-files-finished.png "Upload files finished")

## Task 3: Copy the OCID of your compartment

1. Open the **Navigation menu**, click **Identity & Security**, and under **Identity**, click **Compartments**.

    ![Click Domain](./images/23-click-compartment.png "Click Domain")

2. In the **Compartments** page, hover over the OCID of your compartment, click **Copy**, and paste the OCID somewhere for future reference.

    ![Copy compartment OCID](./images/24-copy-comparment-ocid.png "Copy compartment OCID")

## Task 4: Create a dynamic group

Dynamic groups allow you to group Oracle Cloud Infrastructure resources as principal. You can then create policies to permit the dynamic groups to access Oracle Cloud Infrastructure services.  

1. Open the **Navigation menu**, click **Identity & Security**, and under **Identity**, click **Domains**.

    ![Click Domain](./images/19-click-domain.png "Click Domain")

2. If you have logged in using an identity domain (Lab 3, Task 1), click the name of the identity domain. 

    ![Current Domain](./images/20-current-domain.png "Click Domain")

3. If you have logged in using the Default identity domain (Lab 3, Task 1), click **Create domain**.

    1. Enter a name for the domain in **Display name**.

        ```bash
        <copy>heatwave-genai-domain</copy>
        ```

    2. Enter a description:

        ```bash
        <copy>Domain for HeatWave GenAI</copy>
        ```

    3. Select **Free** in **Domain Type**.

    4. Deselect **Create an administrator user for this domain**.

    5. Click **Create domain**.

        ![Create domain](./images/36-create-domain.png "Create domain")

    6. Once the domain is created, click the name of the domain, **heatwave-genai-domain**.

4. Under **Identity domain**, click **Dynamic groups**.

    ![Click dynamic group](./images/21-click-dynamic-group.png "Click group")

5. Click **Create dynamic group**.

    ![Create dynamic group](./images/22-create-dynamic-group.png "Create dynamic group")

6. In the **Create dynamic group** page, enter a **Name** and a **Description** for the dynamic group. *Note* the name of the dynamic group.

    **Name**:
    
    ```bash
    <copy>heatwave-genai-dynamic-group</copy>
    ```
    **Description**:

    ```bash
    <copy>Dynamic group for HeatWave GenAI</copy>
    ```

    ![Create dynamic group](./images/38-create-dynamic-group.png "Create dynamic group")

7. Enter the followng rule:

    ```bash
    <copy>ALL{resource.type='mysqldbsystem', resource.compartment.id = '<OCIDComparment>'}</copy>
    ```
    For example:

    ```bash
    <copy>ALL{resource.type='mysqldbsystem', resource.compartment.id = 'ocid1.compartment.oc1..aaaaaaaad2mplktxh7unkxodwcqpvtj32ierpvvixvu7qedzsfonq'}</copy>
    ```

    ![Enter rule](./images/37-dynamic-group-rule.png "Enter rule")

8. Click **Create**.


## Task 5: Write policies for the dynamic group

To access Object Storage from HeatWave, you must define a policy that enables the dynamic group to access to buckets and its folders.

 1. Open the **Navigation menu**, click **Identity & Security**, and under **Identity**, click **Policies**.

    ![Click policy](./images/28-click-policies.png "Click policy")

 2. Click **Create Policy**.

   ![Create dynamic group](./images/29-create-policies.png "Create dynamic group")

3. In **Create policy** page, enter the **Name**, and **Description** of the policy.

    **Name**:

    ```bash
    <copy>heatwave-genai-policy</copy>
    ```
    **Description**:
    ```bash
    <copy>Policy for HeatWave GenAI</copy>
    ```

4. Ensure that the Compartment is **heatwave-genai**.

5. Toggle the **Show manual editor** button, and paste the following policy.

    - If you have logged in using an identity domain:

        ```bash
        <copy>Allow dynamic-group <IdentityDomain>/<DynamicGroupName> to read buckets in compartment <CompartmentName></copy>
        <copy>Allow dynamic-group <IdentityDomain>/<DynamicGroupName> to read objects in compartment <CompartmentName></copy>
        ```

        Replace IdentityDomain with the identity domain, DynamicGroupName with the name of the dynamic group, and CompartmentName with the name of the compartment.

        For example:
        
        ```bash
        <copy>Allow dynamic-group OracleCloudIdentityDomain/heatwave-genai-dynamic-group to read buckets in compartment heatwave-genai</copy>
        <copy>Allow dynamic-group OracleCloudIdentityDomain/heatwave-genai-dynamic-group to read objects in compartment heatwave-genai</copy>
        ```

    - If you have logged in using the default identity domain:

        ```bash
        <copy>Allow dynamic-group <DynamicGroupName> to read buckets in compartment <CompartmentName></copy>
        <copy>Allow dynamic-group <DynamicGroupName> to read objects in compartment <CompartmentName></copy>
        ```

        Replace DynamicGroupName with the name of the dynamic group, and CompartmentName with the name of the compartment.

        For example:
        
        ```bash
        <copy>Allow dynamic-group heatwave-genai-dynamic-group to read buckets in compartment heatwave-genai</copy>
        <copy>Allow dynamic-group heatwave-genai-dynamic-group to read objects in compartment heatwave-genai</copy>
        ```

  6. Click **Create**.

    ![Enter policy details](./images/30-enter-policy-details.png "Enter policy details")


<!-- /*## Task 3: Create a Pre-Authenticated Request

Pre-authenticated requests provide a way to let you access a bucket or an object without having your own credentials.

1. In the **Bucket Details** page, under **Resources**, click **Pre-Authenticated Request**, and then click **Create Pre-Authenticated Request**.

    ![Create Pre-Authenticated Request](./images/8-create-par.png "Create Pre-Authenticated Request")

2. In the **Create Pre-Authenticated Request** dialog box, do the following:
    - Enter the **Name**.
    - Select **Bucket** in **Pre-Authenticated Request Target**.
    - Select **Permits object reads** in **Access Type**.
    - Select **Enable Object Listing**.
    - You can choose to increase the **Expiration** date of the Pre-Authenticated Request.
    - Click **Create Pre-Authenticated Request**. 

    ![Enter Pre-Authenticated Request details](./images/9-par-details.png "Enter Pre-Authenticated Request details")

3. Copy the URL that appears in the **Pre-Authenticated Request Details** dialog box, paste the URL somewhere for future reference, and click **Close**.

    ![Copy Pre-Authenticated Request details](./images/10-copy-par.png "Copy Pre-Authenticated Request details")*/ -->


## Task 6: Set up a vector store

1. Create a new schema and select it. You can also select any existing schema.

    ```bash
    <copy>create database genai_db;</copy>
	<copy>use genai_db;</copy>
    ```

    ![Create database](./images/11-create-database.png "Create database")

2. Call the following procedure to create a schema that is used for task management.

    ```bash
    <copy>select mysql_task_management_ensure_schema();</copy>
    ```

    ![Call procedure](./images/12-call-procedure.png "Call procedure")

3. Click **Reload Database Information** to view the mysql\_task\_management schema.

    ![Reload Database Information](./images/13-reload-database-information.png "Reload Database Information")

4. Ingest the file from the Object Storage, create vector embeddings, and load the vector embeddings into HeatWave:

    ```bash
    <copy>call sys.VECTOR_STORE_LOAD('<PAR>', '{"table_name": "<EmbeddingsTableName>"}');</copy>
    ```
    Replace the following:

    - PAR: Pre-Authenticated Request of the bucket, which you had copied in Task 3.
    - EmbeddingsTableName: The name you want for the vector embeddings table.

    For example:

    ```bash
    <copy>call sys.VECTOR_STORE_LOAD('https://axoumfbmk7ld.objectstorage.uk-cardiff-1.oci.customer-oci.com/p/XKR6gUuquT8bSL-ftu89F58FTblCr8QcoN5JcuUzpVE8FqQZ5kgEvr1WR-a5lIYv/n/axoumfbmk7ld/b/bucket-vector-search/o/', '{"table_name": "livelab_embedding"}');</copy>
    ```

    ![Ingest files from Object Storage](./images/14-ingest-files.png "Ingest files from Object Storage")

5. Wait for a few minutes, and verify that embeddings are loaded in the vector embeddings table.

    ```bash
    <copy>select count(*) from <EmbeddingsTableName>;</copy>
    ```
    For example:
    ```bash
    <copy>select count(*) from livelab_embedding; </copy>
    ```
    You should see a numerical value in the output, which means your embeddings are successfully loaded in the table. If you get an error, wait for a few minutes and try again. It takes a couple of minutes to create vector embeddings.

    ![Vector embeddings](./images/15-check-count.png "Vector embeddings")

## Task 7: Perform a vector search

HeatWave retrieves content from the vector store and provide that as context to the LLM. This process is called as retrieval-augmented generation or RAG. This helps the LLM to produce more relevant and accurate results for your queries.

1. Load the LLM in HeatWave by entering the following command, and clicking **Execute the selection or full block on HeatWave and create a new block**.

    ```bash
    <copy>call sys.ML_MODEL_LOAD('LLMModel', NULL);</copy>
    ```

    Replace LLMModel with the name of the LLM model that you want to use. The available models are: mistral-7b-instruct-v1 and llama2-7b-v1.

    For example:

    ```bash
    <copy>call sys.ML_MODEL_LOAD('mistral-7b-instruct-v1', NULL);</copy>
    ```

    ![Load LLMs](./images/16-load-llm.png "Load LLMs")

2. Set the @options session variable to specify the table for retrieving the vector embeddings.

    ```bash
    <copy>set @options = JSON_OBJECT("vector_store", JSON_ARRAY("<DBName>.<EmbeddingsTableName>"));</copy>
    ```

    For example:

    ```bash
    <copy>set @options = JSON_OBJECT("vector_store", JSON_ARRAY("genai_db.livelab_embedding_pdf"));</copy>
    ```

3. Set the session @query variable to define your natural language query.

    ```bash
    <copy>set @query="<AddYourQuery>";</copy>
    ```
    
    Replace AddYourQuery with your natural language query.

    For example:

    ```bash
    <copy>set @query="What is HeatWave AutoML?";</copy>
    ```

4. Retrieve the augmented prompt, using the ML_RAG routine, and click **Execute the selection or full block on HeatWave and create a new block**.

    ```bash
    <copy>call sys.ML_RAG(@query,@output,@options);</copy>
    ```

    ![Set options](./images/17-set-options.png "Set options")

5. Print the output:

    ```bash
    <copy>select JSON_PRETTY(@output);</copy>
    ```
    
    Text-based content that is generated by the LLM in response to your query is printed as output. The output generated by RAG is comprised of two parts:

    - The text section contains the text-based content generated by the LLM as a response for your query.

    - The citations section shows the segments and documents it referred to as context.

    ![Vector search results](./images/18-vector-search-output.png "Vector search results")
## Learn More

- [HeatWave User Guide](https://dev.mysql.com/doc/heatwave/en/)

- [HeatWave on OCI User Guide](https://docs.oracle.com/en-us/iaas/mysql-database/index.html)

- [MySQL Documentation](https://dev.mysql.com/)


## Acknowledgements

- **Author** - Aijaz Fatima, Product Manager
- **Contributors** - Mandy Pang, Senior Principal Product Manager, Aijaz Fatima, Product Manager
- **Last Updated By/Date** - Aijaz Fatima, Product Manager, August 2024