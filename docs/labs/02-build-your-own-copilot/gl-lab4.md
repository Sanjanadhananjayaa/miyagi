## Exercise 1: Bringing your own data with Azure AI Search

### Task 1: Configure Azure Cognitive Search

1. Navigate back to the **Azure portal** tab, search and select **AI Search**.

    ![](./Media/ai-search1.png)    

1. In **Azure AI services | AI Search** tab, select **acs-<inject key="DeploymentID" enableCopy="false"/>**.

   > **Note**: Please click on the refresh button still you view the **Document Count**.

1. In the overiew tab of search service, click on the **Import data**.

    ![](./Media/import-data1.png)    
   
1. From the drop-down select **Data Source** as **Samples**, select the **CosmosDB hotels-sample**, and click on **Next : Add cognitive skills(optional)**.

   ![](./Media/import-data2.png)
   
1. In **Add cognitive skills** leave as default and click on **Skip to: Customize target index**.

1. In the **Customize target index**, Enter the index name as **realestate-us-sample-index** and click on **Next: Create an indexer**.

   ![](./Media/import-data3.png)

1. In the **Create an indexer**, change the indexer name as **realestate-us-sample-indexer** and click on **Submit**.

   ![](./Media/import-data4.png)

1. From left menu, select **Indexes** under **Search management**
    
1. Click on **realestate-us-sample-index** in the search bar enter **Seattle** and click on **Search**.

   ![](./Media/final-indexer.png)
