# Working with Zeek Data Using Azure Synapse Analytics

## Create Azure Resources

The deployment script creates the following resources:

- An Azure Synapse Workspace
- An Apache Spark pool in the Azure Synapse Workspace
- An Azure Data Lake Storage Account

The script also uploads several CSV files to the Azure Data Lake.

1. Open a Powershell session and go to the repository root.
2. Login to Azure by running `az login`.
3. Get your subscription id by running `az account show --query id`.
4. Run the deployment script specifying your subscription id.

```powershell
.\deploy\deploy.ps1 -location westus -subscriptionId [your subscription id]
```

> NOTE: Use the **westus** region as this seems to work best with this example.

## Open the Azure Synapse Workspace

When the deployment script successfuly completes, your Azure Synapse Workspace URL and file system endpoint is shown in the output:

```bash
Your Azure Synapse workspace url:
https://web.azuresynapse.net?workspace=%2fsubscriptions%2f1e32e17d-db2c-4254-ac01-5010575e89dd%2fresourceGroups%2fsynapsepoc-atjrra-rg%2fproviders%2fMicrosoft.Synapse%2fworkspaces%2fatjrrasynws

Your file system endpoint:
atjrrasynfs@atjrrasyndl.dfs.core.windows.net
```

> **NOTE**: you will need the file system endpoint value when running the notebooks.

Browse to the workspace URL.

# Run the "RunML" Notebook

Run a notebook that predicts tactics using the Naive Bayes algorithm.

1. In the navigation menu on the left, click **Develop**.
2. Expand **Notebooks** and select **RunML**.
3. In the **third code cell**, set the value of the `csv_file_name` and `filesystem_endpoint` variables to the name of the CSV file to use and your file system endpoint, respectively. The file system endpoint can be found in the deployment script output.
4. In the **Attach to** drop-down in the toolbar, select **sparkPool01**.
5. Click **Run all**.
6. The Spark cluster starts, and the code in the notebook is executed.
7. Close the notebook. In the **Keep current session?** dialog, click **Stop session**. Then click **Close + discard changes**.

> **NOTE**:
>
> - It can take several minutes to start the Spark cluster. The cluster is configured to shut down after 30 minutes of inactivity.
> - The notebooks take around 20-30 minutes to run to completion.

# Run the "RunML" Notebook using a different algorithm

1. On the **third code cell**, set the value of the `algorithm` variable to the algorithm of your choice. See the `MLAlgorithm` class defined in the **Common** notebook for a list of algorithms.
2. Click **Run all**.
3. The code in the notebook is executed.

# View the results

1. In the navigation menu on the left, click **Data**.
2. Click the **Linked** tab.
3. In the **Azure Data Lake Storage Gen2** tree, expand the storage account, then click on the container.
4. Click into the **data\zeke** folder.
5. You should see two new folders containing the name of the algorithm. These contain CSV files with the algorithms' results.
6. Click into a folder to see the CSV file.
7. If you would like to view the file, select it, then click **Download.**

# Cleanup

To delete the Azure Synapse workspace, run the following:

```powershell
az group delete [resource group name] --no-wait --yes
```

> **NOTE**: This command will delete all data and notebooks. If you need to save them, download them locally prior to deleting the resource group.
