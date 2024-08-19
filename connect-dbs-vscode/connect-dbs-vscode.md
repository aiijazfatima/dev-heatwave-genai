# Create DB System

## Introduction

In this lab you will connect to the DB system you created in Lab 2 from VS Code.

_Estimated Time:_ 15 minutes

### Objectives

In this lab, you will be guided through the following tasks:

- Connect to the OCI tenancy.
- Connect to the DB System.

### Prerequisites

- You have completed Lab 2.


## Task 1:  Connect to the OCI tenancy

Before you can get started, you must set up a DB Connection to the HeatWave instance on the Oracle Cloud Infrastructure (OCI). This has to be done once since MySQL Shell for VS Code will store all registered DB connections.

1. Log into the [OCI Console](https://www.oracle.com/cloud/sign-in.html?redirect_uri=https%3A%2F%2Fcloud.oracle.com%2F).

2. Enter your **Cloud Account Name** and click **Next**. 
    
    ![Enter cloud account name](./images/1-cloud-account-name.png "Enter cloud account name")

3. Select **Default** as the identity domain.

    ![Default identity domain](./images/16-default-identity-domain.png "Default identity domain")

4. Enter your **Username** and **Password** and click **Sign In**.

    ![Enter username and password](./images/2-username.png "Enter username and password")

5. Click **Profile** and select **My Profile**.

    ![My profile](./images/3-profile.png "My profile")

6.	Under **Resources**, click **API keys**, and click **Add API key**.

7. Click **Download private key**.
    
    1. Store the API key in a .oci folder inside your home directory.

    2. Rename the API key to oci_api_key.pem.

    3. Click **Add**.
    
    ![Download private key](./images/4-add-api-key.png "Download private key")

8. Copy the configuration file text and switch to VS Code.

    ![Copy configuration file](./images/5-copy-config.png "Copy configuration file")

9. In VS Code, select the **MySQL Shell for VS Code** extension.

10. Click **Configure the OCI Profile List** in the **ORACLE CLOUD INFRASTRUCTURE** view, and paste the configuration file text into the config file.

    - Rename the top section from [DEFAULT] to the name of the tenancy, [TenancyName]
    
    - Update the path to the API Key you had stored in your home directory.

    ![Save configuration file](./images/6-save-config.png "Save configuration file")

11. Close the file and reload the **ORACLE CLOUD INFRASTRUCTURE** view. You can browse the resources of your OCI tenancy.

    ![Tenancy details](./images/7-tenancy-details.png "Tenancy details")


## Task 2: Connect to the DB System

1. Browse to the DB system, **heatwave-genai-dbs**, and right click and select **Create Connection with Bastion Service**.

    ![Create connection with Bastion service](./images/8-create-bastion.png "Create connection with Bastion service")

2. Click **Create New Bastion**.

    ![Create new connection with Bastion service](./images/9-create-new-bastion.png "Create new connection with Bastion service")

3. In the **Database Connection Configuration** dialog, enter the **User Name** and click **Store Password** to enter the password of your DB system.

    ![Database Connection Configuration](./images/10-database-connection.png "Database Connection Configuration")

4. Click **OK** to create the connection. 

5. Click **Open New Database Connection** icon to connect to the DB system. 

    ![Open New Database Connection](./images/11-open-database-connection.png "Open New Database Connection")

6. Check whether you are connected to the DB system by running the following command:

    ```bash
    show databases;
    ```

    ![New Database Connection](./images/12-show-databases.png "New Database Connection")

You may now **proceed to the next lab**.

## Learn More

- [HeatWave User Guide](https://dev.mysql.com/doc/heatwave/en/)

- [HeatWave on OCI User Guide](https://docs.oracle.com/en-us/iaas/mysql-database/index.html)

- [MySQL Documentation](https://dev.mysql.com/)

## Acknowledgements

- **Author** - Aijaz Fatima, Product Manager
- **Contributors** - Mandy Pang, Senior Principal Product Manager
- **Last Updated By/Date** - Aijaz Fatima, Product Manager, August 2024