# Lab 2.1: Containerizing Recommendation service to Azure Container Apps. [Read-Only]

In this lab, you'll be building the docker images and publishing them to Azure Container Apps.

### Task 1: Build Docker Images for the Recommendation service

1. Open the **Docker** Application from the Lab VM desktop by double-clicking on it.

   ![](./Media/docker1.png)
   
1. In the **Docker Subscription Service Agreement** window, click **Accept**.

   ![](./Media/docker2.png)

1. In the **Welcome to Docker Desktop** window, click on **Continue without signing in**.

   ![](./Media/without-signin.png)

1. In the **Tell us about the work you do** window, click on **Skip**.

1. Navigate back to **Visual studio code** window and navigate to **miyagi/services/recommendation-service/dotnet** right - click on dotnet in cascading menu, select **Open in integrate Terminal**.

1. Run the following command to build a **Docker image**.

   ```
   docker build --no-cache -t miyagi-recommendation .   
   ```

   ![](./Media/task2-1.png)

   > **Note**: Please wait as this command may require some time to complete.

1. Run the following command to get the newly created image.

   ```
   docker images
   ```
   
   ![](./Media/task2-2.png)

1. Navigate back to **Docker desktop**, from the left pane select **Images**.

   ![](./Media/docker7.png)

1. In the **Images** blade, notice **miyagi-recommendation(1)** image is created, select **run(2)** icon .

   ![](./Media/docker13.png)

1. In the **Run a new container** window select the dropdown arrow.

   ![](./Media/docker14-(1).png)

1. In the **Run a new containe**, under **Ports** for **Host Port** enter **5224** and click on **Run**.

    ![](./Media/recommendation-port.png)

1. Click on **5224:80** URL link

   ![](./Media/recommendation-port-open.png)
   
1. You should be able to see the application running locally
   
   ![](./Media/docker-recommend.png)

### Task 2: Push the Docker Image of the Recommendation service to the Container registry

In this task, you'll Push miyagi-recommendation images to acr. 

1. Navigate back to the **Visual studio code** window and navigate to **miyagi/services/recommendation-service/dotnet** right - click on dotnet in cascading menu, select **Open in integrate Terminal**

1. Run the following command to log in to the **Azure portal**.

    ```
    az login
    ```

1. This will redirect to **Microsoft login page**, select your Azure account **<inject key="AzureAdUserEmail"></inject>**, and navigate back to the **Visual studio code**.

   ![](./Media/azure-account-select.png)

1. Run the following command to log in to an **Azure Container Registry (ACR)** using the Azure CLI.

   > **Note**: Please replace **[ACRname]** and **[uname]** with **<inject key="AcrUsername" enableCopy="true"/>**, and **[password]** with **<inject key="AcrPassword" enableCopy="true"/>**.
   
   ```
   az acr login -n [ACRname] -u [uname] -p [password]
   ```

   ![](./Media/task2-5.png)
    
1. Run the following command to add the tag.

   > **Note**: Please replace **[ACRname]** with **<inject key="AcrLoginServer" enableCopy="true"/>**.

   ```
   docker tag miyagi-recommendation:latest [ACRname]/miyagi-recommendation:latest
   ```

1. Run the following command to push the image to the container registry.

   > **Note**: Please replace **[ACRname]** with **<inject key="AcrLoginServer" enableCopy="true"/>**.

   ```
   docker push [ACRname]/miyagi-recommendation:latest
   ```

   ![](./Media/task2-6.png)

### Task 3: Create a Container app for recommendation-service 

In this task, you'll will be creating a container app for the recommendation.

1. Run the following command to create **Container App environment**.

   > **Note**: Please replace **[DID]** with **<inject key="DeploymentID" enableCopy="true"/>** and **[Region]** with **<inject key="Region" enableCopy="true"/>**.

   ```
   az containerapp env create --name env-miyagi-[DID] --resource-group miyagi-rg-[DID] --location [Region]
   ```

1. Run the following command to create **Container App**.

   > **Note**: Please replace **[DID]** with **<inject key="DeploymentID" enableCopy="true"/>**, **[ACRname]** with **<inject key="AcrLoginServer" enableCopy="true"/>**, **[uname]** with **<inject key="AcrUsername" enableCopy="true"/>**, and **[password]** with **<inject key="AcrPassword" enableCopy="true"/>**.

   ```
   az containerapp create --name ca-miyagi-rec-[DID] --resource-group miyagi-rg-[DID] --image [ACRname]/miyagi-recommendation:latest --environment env-miyagi-[DID] --registry-server [ACRname] --registry-username [uname] --registry-password [password]
   ```

1. Run the following command to enable **Container App ingress**.

   > **Note**: Please replace **[DID]** with **<inject key="DeploymentID" enableCopy="true"/>**
   
   ```
   az containerapp ingress enable -n ca-miyagi-rec-[DID] -g miyagi-rg-[DID] --type external --allow-insecure --target-port 80
   ```

### Task 4: Verify Recommendation Service using Swagger


1. In the Azure Portal page, in the Search resources, services, and docs (G+/) box at the top of the portal, enter **Container Apps (1)**, and then select **Container Apps (2)** under services.

   ![](./Media/container-app-select.png)

1. In the **Container Apps** blade, select **ca-miyagi-rec-<inject key="DeploymentID" enableCopy="false"/>**.

   ![](./Media/container-ca-miyagi.png)

1. In the **ca-miyagi-rec-<inject key="DeploymentID" enableCopy="false"/>** page, from left navigation pane select **Ingress** **(1)** under setting session and click on **Endpoints** **(2)** URL link.

   ![](./Media/container-ca-ingress.png)

1. You can view the **miyagi Recommendation service** website running through the Container Apps.

   ![](./Media/online-output-recommendation.png)    

## Lab 2.2: Explore and Verify the Containerized Recommendation service in Azure Container Apps using Local Miyagi UI.
 
### Task 1: Update Container App Recommendation service URL for Miyagi UI

1. In the Azure Portal page, in the Search resources, services, and docs (G+/) box at the top of the portal, enter **Container Apps (1)**, and then select **Container Apps (2)** under services.

   ![](./Media/container-app-select.png)

1. In the **Container Apps** blade, select **ca-miyagi-rec-<inject key="DeploymentID" enableCopy="false"/>**.

   ![](./Media/container-ca-miyagi.png)

1. In the **ca-miyagi-rec-<inject key="DeploymentID" enableCopy="false"/>** page, from left navigation pane select **Ingress** **(1)** under setting session and copy **Endpoints** **(2)** URL link.

   ![](./Media/container-ca-ingress.png)

1. Navigate back to **Visual Studio Code**, navigate to **miyagi>ui>typescript>.env.** and replace existing code for **NEXT_PUBLIC_RECCOMMENDATION_SERVICE_URL** with copied for **Endpoints** and save the file 

   ![](./Media/cntr4.png)

### Task 2: Access Recommendation Service running on Azure Container Apps from Local Miyagi-UI 

1. Open a new terminal: by navigating  **miyagi/ui** and right-click on **ui/typescript** , in cascading menu select **Open in intergate Terminal**.

   ![](./Media/image-rg-25.png)

1. Run the following command to install the dependencies
   
    ```
    yarn dev
    ```

   **Note**: Let the command run, meanwhile you can proceed with the next step.

1. Open another tab in Edge, and  browse the following

   ```
   http://localhost:4001
   ```

   **Note**: Refresh the page continuously until you get miyagi app running locally as depicted in the image below.
                       
   ![](./Media/miyagi1.png)

1. In the to the **recommendation service** ui page, and click on **personalize** button.

    ![](./Media/service-personalize.png)

1. In the **personalize** page, select your **financial advisor** from the drop-down, and click on **Personalize**.

   ![](./Media/financial-advisor.png)  

1. You should see the recommendations from the recommendation service in the Top Stocks widget.

   ![](./Media/financial-advisor-output.png)    
