# Lab 2: Create Docker image for Recommendation Service and deploy to Azure Container Apps

In this lab, you'll be building the docker images and publishing them to Azure Container Apps.

### Task 1: Verify the deployed API Management service and create an API

1. Navigate to Azure portal, open the Resource Group named **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>**  and select **miyagi-apim-<inject key="DeploymentID" enableCopy="false"/>** API Management service from the resources list.

   ![](./Media/lab3-t1-s1.png)

1. From the left menu, click on **APIs** **(1)** and select **HTTP** **(2)** under Define a new API to create an HTTP API.

   ![](./Media/lab3-t1-s2.png)

1. Enter the following values in the Create an HTTP API pane:
   
   | **Parameter**        | **Values**           | 
   | -------------------- | -------------------- | 
   | API Type **(1)**     | **Basic**            | 
   | Display name **(2)** | **miyagi-api**       |
   | Name **(3)**         | **miyagi-api**       |
   | Web service URL **(4)** | Enter the Endpoint of OpenAI resource named **OpenAIService-<inject key="DeploymentID" enableCopy="false"/>**  |
   | API URL suffix **(5)** | **miyagi** |
   | Click on  **(6)** | **Create** |

   ![](./Media/lab3-t1-s3.png)

1. Once API is created, click on **Overview** **(1)** from the left-menu and copy the **Gateway URL** **(2)** of API Management service. Paste it into Notepad for later use.

   ![](./Media/lab3-t1-s4.png)

### Task 2: Update the Docker Image for the Recommendation service

In this task, you will be updating the API Management Gateway URL as an endpoint in appsettings.json file of Recommendation Service.

1. Navigate to Visual Studio Code, open the `appsettings.json` file from the path `C:\LabFiles\miyagi\services\recommendation-service\dotnet\appsettings.json`.

   ![](./Media/lab3-t2-s1.png)

1. In the `appsettings.json` file, you have to replace the **endpoint** value from **OpenAI resource endpoint** with **API Gateway URL** which you have copied in Task-1 Step-4.

   ![](./Media/lab3-t2-s2.png)

1. From the Explorer, navigate to `Miyagi/services/recommendation-service/dotnet/` **(1)** path. Right-click on the `dotnet` folder and select **Open in Integrated Terminal** **(2)** from the options tab to open the terminal with the required path.

   ![](./Media/lab3-t2-s3.png)

1. Now, you need to rebuild the docker image for the recommendation service by running the below docker command. Make to update the docker image name which was created earlier for recommendation service with the same name.

   ```
   docker build . -t miyagi-recommendation
   ```

   ![](./Media/lab3-t2-s4.png)

1. Run the following command to ACR login.

   **Note**: Please replace **<ACR_Name>** and **[Uname]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>**. For [password], navigate to resource group > and select **miyagiacr<inject key="DeploymentID" enableCopy="false"/>** from the resources list. Under Settings, select **Access Keys** and select the check box for **Admin user** and then copy the password.

    ```
    docker login miyagiacr<inject key="DeploymentID" enableCopy="false"/>.azurecr.io -u miyagiacr<inject key="DeploymentID" enableCopy="false"/> -p [password]
    ```

1. Once you are logged into ACR. Run the below command to push the update docker image of the recommendation service to the container registry.

   **Note**: Make sure to replace **miyagiacr[DID]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>**.

   ```
   docker push miyagiacr<inject key="DeploymentID" enableCopy="false"/>.azurecr.io/miyagi-recommendation:latest
   ```

   ![](./Media/lab3-t2-s5.png)

### Task 3: Build Docker Images for the Recommendation service

1. Open the **Docker** Application from the Lab VM desktop by double-clicking on it.

   ![](./Media/docker1.png)
   
1. In the **Docker Subscription Service Agreement** window, click **Accept**.

   ![](./Media/docker2.png)

1. In the **Welcome to Docker Desktop** window, click on **Continue without signing in**.

1. Navigate back to **Visual studio code** window and navigate to **miyagi/services/recommendation-service/dotnet** right - click on dotnet in cascading menu, select **Open in integrate Terminal**

1. Run the following command to build a **Docker image**

   ```
   docker build . -t miyagi-recommendation      
   ```

   ![](./Media/task2-1.png)

   **Note**: Please wait as this command may require some time to complete.

1. Run the following command to get the newly created image.

   ```
   docker images
   ```
   
   ![](./Media/task2-2.png)

1. Run the following command for running a Docker container.

   ```
   docker run -t miyagi-recommendation -p 80
   ```

1. Navigate back to **Docker desktop**, from the left pane select **Images**.

   ![](./Media/docker7.png)

1. In the **Images** blade, notice **miyagi-recommendation(1)** image is created, select **run(2)** icon .

   ![](./Media/docker13.png)

1. In the **Run a new container** window select the dropdown arrow.

   ![](./Media/docker14-(1).png)

1. In the **Run a new containe**, under **Ports** for **Host Port** enter **80** and click on **Run**.

    ![](./Media/docker14.png)

1. Click on **80:80** URL link

   ![](./Media/docker15.png)
   
1. You should be able to see the application running locally
   
   ![](./Media/docker16.png)


### Task 4: Push the Docker Image of Recommendation service to ACR

In this task, you'll Push miyagi-recommendation images to acr. 

1. Return to **Visual studio code** window and navigate to **miyagi/services/recommendation-service/dotnet** right - click on dotnet in cascading menu, select **Open in intergate Terminal**

1. Run the following command to login.

   **Note**: Please replace **[ACRname>]** and **[uname]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>** for **[password]** navigate to resource group > and select **miyagiacr<inject key="DeploymentID" enableCopy="false"/>** from list of resource and under settings select **Access Keys**, select check box for **Admin user** and copy the password.

    ```
    az login
    az acr login -n miyagiacr<inject key="DeploymentID" enableCopy="false"/>.azurecr.io -u miyagiacr<inject key="DeploymentID" enableCopy="false"/> -p [password]
    ```

   ![](./Media/task2-5.png)
    
1. Run the following command to add the tag.

   **Note**: Please replace **miyagiacr[DID]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>**

   ```
   docker tag miyagi-recommendation:latest miyagiacr<inject key="DeploymentID" enableCopy="false"/>.azurecr.io/miyagi-recommendation:latest
   ```

1. Run the following command to push the image to the container registry.

   **Note**: Please replace **miyagiacr[DID]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>**

   ```
   docker push miyagiacr<inject key="DeploymentID" enableCopy="false"/>.azurecr.io/miyagi-recommendation:latest
   ```

   ![](./Media/task2-6.png)

### Task 5: Create a container app for miyagi-recommendation.

In this task, you'll will be creating a container app for the recommendation.

1. Run the following command to **Container App environment**.

   ```
   az containerapp env create --name env-miyagi --resource-group miyagi-rg-<inject key="DeploymentID" enableCopy="false"/> --location <inject key="Region" enableCopy="false"/>
   ```

1. Run the following command to **Container App**.

   ```
   az containerapp create --name ca-miyagi-rec --resource-group miyagi-rg-<inject key="DeploymentID" enableCopy="false"/> --image $imageName --environment $envName --registry-server miyagiacr<inject key="DeploymentID" enableCopy="false"/>.azurecr.io --registry-username miyagiacr<inject key="DeploymentID" enableCopy="false"/> --registry-password [password].
   ```

1. Run the following command to enable **Container App ingress**
   
   ```
   az containerapp ingress enable -n ca-miyagi-rec -g $rgName --type external --allow-insecure --target-port 80
   ```
 
### Task 6: Update Container App Recommendation service URL for miyagi-ui 

1. In the Azure Portal page, in the Search resources, services, and docs (G+/) box at the top of the portal, enter **Container Apps (1)**, and then select **Container Apps (2)** under services.

   ![](./Media/cntr1.png)

1. In the **Container Apps** blade, select **miyagi-rec-ca-<inject key="DeploymentID" enableCopy="false"/>**.

   ![](./Media/cntr2.png)

1. In the **miyagi-rec-ca-<inject key="DeploymentID" enableCopy="false"/>** page, from left navigation pane select **Ingress** and copy **Endpoints** URL link.

   ![](./Media/cntr3.png)

1. Navigate back to **Visual Studio Code**, navigate to **miyagi>ui>.env.** and replace existing code for **RECCOMMENDATION_SERVICE_URL** with copied for **Endpoints** and save the file 

   ![](./Media/cntr4.png)

1. Right-click on **ui/typescript** in cascading menu, select **Open in intergate Terminal**.

1. Run the following command to log in.

    > **Note**: Please replace **[ACRname>]** and **[uname]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>** for **[password]** navigate to resource group > and select **miyagiacr<inject key="DeploymentID" enableCopy="false"/>** from list of recources and under settings select **Access Keys**, select check box for **Admin user** and copy the password.

    ```
    docker login miyagiacr<inject key="DeploymentID" enableCopy="false"/>.azurecr.io -u miyagiacr<inject key="DeploymentID" enableCopy="false"/> -p [password]
    ```
   
1. Run the following command to push the image to the container registry

   > **Note**: Please replace **miyagiacr[DID]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>**

   ```
   docker push miyagiacr<inject key="DeploymentID" enableCopy="false"/>.azurecr.io/miyagi-recommendation:latest
   ```

1. Reture to **Azure Portal** in Search resources, services and docs (G+/) box at the top of the portal, enter **Container Apps**, and then select **Container Apps** under services.

1. In the **Container Apps** blade, select **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>**.

   ![](./Media/cntr5.png)

1. In the **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>**, from left navigation pane select **Ingress** and click on **Endpoints** URL link.

   ![](./Media/cntr6.png)

1. You should get miyagi app running locally as depicted in the image below.

   ![](./Media/cntr7.png)

1. Return to **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>** page, under **Application** select **Revisions** and click on **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>**, on **Revision details** window, select **Refresh**.

   ![](./Media/cntr8.png)

1. When **Are you sure you want to restart the revision?** prompt on **Restart revision** window clcik on **Continue**.

   ![](./Media/cntr9.png)

1. Once restarting is done for **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>** from left pane select **Ingress** and click on **Endpoints**.

   ![](./Media/cntr6.png)

1. You should get miyagi app running locally as depicted in the image below.

   ![](./Media/cntr10.png)
