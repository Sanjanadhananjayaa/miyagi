# Lab 2: Deploy Apps to Azure Container Apps

In this lab, you'll be building the docker images and publishing it to Azure Container Apps.

### Task 1: Build Docker Images for the Recommendation service

1. Open Docker Application from the Lab VM desktop by double-clicking on it.
   ![](./Media/docker1.png)
   
1. On **Docker Subscription Service Agreement** window, click **Accept**.
   ![](./Media/docker2.png)

1. On **Welcome to Docker Desktop** window, click on **sign up**.

   ![](./Media/docker3.png)

1. On **Create your account** window, specify the following details and click on sign up.

   | **Settings**         | **Values**           | 
   | -------------------- | -------------------- | 
   | Email                | <inject key="AzureAdUserEmail"></inject>  | 
   | Username             | Odluser<inject key="DeploymentID" enableCopy="false"/>   |
   | Password             | <inject key="AzureAdUserPassword"></inject>         |
   | Send me occasional product updates and announcements. | Select the checkbox |

   ![](./Media/docker5.png)

1. On **Sign in** window, enter your email <inject key="AzureAdUserEmail"></inject> and click on **Continue**.

   ![](./Media/docker4.png)

1. On **Enter your Password** window, provide <inject key="AzureAdUserPassword"></inject> and click on **Continue**

    ![](./Media/docker(6).png)

1. If **This site is trying to open docker desktop.** pop up's click on **Open**.

1. Return to **Visual studio code** window and navigate to **miyagi/services/recommendation-service/dotnet** right - click on dotnet in cascading menu, select **Open in intergate Terminal**

1. Run following command to build a **Docker image**

   ```
   docker build . -t miyagi-recommendation      
   ```

   ![](./Media/task2-1.png)

   **Note**: Please wait as this command may require some time to complete.

1. Run following command to get the newly created image.
   ```
   docker images
   ```
   ![](./Media/task2-2.png)

1. Run following command for running a Docker container.
   ```
   docker run -t miyagi-recommendation -p 80
   ```

1. Navigate back to **Docker desktop**, from the left pane select **Images**.
   ![](./Media/docker7.png)

1. On **Images** blade, notice **miyagi-recommendation(1)** image is created, select **run(2)** icon .
   ![](./Media/docker13.png)

1. On **Run a new containe** window select the dropdown arrow.

   ![](./Media/docker14-(1).png)

1. On **Run a new containe**, under **Ports** for **Host Port** enter **80** and click on **Run**.

    ![](./Media/docker14.png)

1. Click on **80:80** url link
   ![](./Media/docker15.png)
   
1. You should be able to see the application running locally
   
   ![](./Media/docker16.png)

### Task 2: Build Docker Images for the Miyagi-ui

1. Return to **Visual studio code** window and navigate miyagi/ui/ right - click on ui/typescript in cascading menu, select **Open in intergate Terminal**.

1. Run following command to build a Docker image

    ```
    docker build . -t miyagi-ui      
    ```
    ![](./Media/task2-3.png)

    **Note**: kindly wait this command may take some time to complete.

1. Run following command to get the newly created image.
   ```
   docker images
   ```
   ![](./Media/task2-4.png)

1. Run following command for running a Docker container.
   ```
   docker run -t miyagi-ui -p 3000
   ```

1. Navigate back to **Docker desktop**, from the left pane select **Images**.
   ![](./Media/docker7.png)

1. On **Images** blade, notice **miyagi-ui(1)** image is created, select **run(2)** icon .
   ![](./Media/docker8.png)

1. On **Run a new containe** window select the dropdown arrow.

   ![](./Media/docker9.png)

1. On **Run a new containe**, under **Ports** for **Host Port** enter **3000** and click on **Run**.

    ![](./Media/docker10.png)

1. Click on **3000:3000** url link
   ![](./Media/docker11.png)
   
1. You should be able to see the application running locally
   
   ![](./Media/docker12.png)

### Task 3: Push two Images to the Azure container registery(acr)

In this task, you'll Push miyagi-ui and miyagi-recommendation images to acr. 

1. Return to **Visual studio code** window and navigate to **miyagi/services/recommendation-service/dotnet** right - click on dotnet in cascading menu, select **Open in intergate Terminal**

1. Run following command to login.

   **Note**: Please replace **[ACRname>]** and **[uname]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>** for **[password]** navigate to resource group > and select **miyagiacr<inject key="DeploymentID" enableCopy="false"/>** from list of resource and under settings select **Access Keys**, select check box for **Admin user** and copy the password.

    ```
    docker login [ACRname].azurecr.io -u [uname] -p [password]
    ```
    ![](./Media/task2-5.png)

    
1. Run following command to add tag.

   **Note**: Please replace **miyagiacr[DID]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>**

   ```
    docker tag miyagi-recommendation:latest miyagiacr[DID].azurecr.io/miyagi-recommendation:latest
   ```
1. Run the following command to push image to container registery

   **Note**: Please replace **miyagiacr[DID]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>**

   ```
    docker push miyagiacr[DID].azurecr.io/miyagi-recommendation:latest

   ```
   ![](./Media/task2-6.png)

1. Return to **Visual studio code** window and navigate to **miyagi/ui**, right - click on ui/typescript in cascading menu, select **Open in intergate Terminal**.

1. Run following command to login.

   **Note**: Please replace **[ACR_Name]** and **[Uname]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>**. For **[password]**, navigate to resource group > and select **miyagiacr<inject key="DeploymentID" enableCopy="false"/>** from the resource list. Under Settings, select **Access Keys** and select the check box for **Admin user** and then copy the password.

   ```
    docker login [ACRname].azurecr.io -u [uname] -p [password]
   ```

   ![](./Media/task2-7.png)
   
1. Run following command to add tag

   **Note**: Please replace **miyagiacr[DID]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>**

   ```
    docker tag miyagi-ui:latest miyagiacr[DID].azurecr.io/miyagi-ui:latest
   ```
1. Run the following command to push image to container registery

   **Note**: Please replace **miyagiacr[DID]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>**

   ```
    docker push miyagiacr[DID].azurecr.io/miyagi-ui:latest

   ```
   ![](./Media/task2-8.png)


### Task 4: Configure the Container App for miyagi-ui

1. Return to **Azure Portal** window and navigate to **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>** resource group.

1. On **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>** blade, from the resources list select **Container Apps Environment**.

    ![](./Media/acr1.png)

1. on **Container Apps Environment**. blade from left pane under **Apps** section select **Apps** and click on **+ Create**.
    ![](./Media/acr2.png)

1. On the basics tab of **Create Container App** blade specify the following details and select **Next:container>**.

   | **Settings**         | **Values**           | 
   | -------------------- | -------------------- | 
   | Subscription         | Leave default | 
   | Resource group       | **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>** resource group. |
   | Container app name	  | Enter **miyagi-rec-ca-<inject key="DeploymentID" enableCopy="false"/>**           |
   |||

   ![](./Media/acr-ui-2.png)

1. On the **Container** tab of **Create Container App** blade specify the following details and select **Next:Ingress>**.

   | **Settings**         | **Values**           | 
   | -------------------- | -------------------- | 
   | Use quickstart image | Unselect the checkbox | 
   | Name                 | **miyagi-rec-ca-<inject key="DeploymentID" enableCopy="false"/>** |
   | Image source         | Select Azure Container Registry  |
   | Registry             | Select miyagiacr<inject key="DeploymentID" enableCopy="false"/>.azurecr.io |
   | Image                | Select miyagi-recommendation              |
   | Image tag     	     | Latest         | 
   | CPU and Memory       | 1 CPU cores, 2Gi memory        | 
   |||

    ![](./Media/acr-ui-3.png)

1. On the **Ingress** tab of **Create Container App** blade specify the following details, select **Review + Create** and **Create**.

   | **Settings**         | **Values**           | 
   | -------------------- | -------------------- | 
   | Ingress              | select the checkbox | 
   | Ingress traffic      | Select **Accepting traffic from anywhere**             |
   | Ingress type  	     | Select **HTTP**            |
   | Client certificate mode | Select **Ingore** |
   | Insecure connections | Select checkbox **Allowed**        |
   | Target port   	     | **80**         |
   |||

   ![](./Media/acr-ui-4.png)
   
1. Once deployment gets success click on **Go to resource**
   ![](./Media/acr-ui-5.png)
1. On **miyagi-rec-ca-<inject key="DeploymentID" enableCopy="false"/>** page, from left navigation pane select **Ingress** and click on **Endpoints** URL link.
   ![](./Media/acr-ui-6.png)

1. You should get swagger page for the recommendation service as depicted in the image below.
   ![](./Media/acr-ui-6-1.png)

1. Reture back to **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>** blade, from the resources list select **Container Apps Environment**.

    ![](./Media/acr1.png)

1. on **Container Apps Environment**. blade from left pane under **Apps** section select **Apps** and click on **+ Create**.
    ![](./Media/acr-ui-7.png)

1. On the basics tab of **Create Container App** blade specify the following details and select **Next:container>**.

   | **Settings**         | **Values**           | 
   | -------------------- | -------------------- | 
   | Subscription         | Leave default | 
   | Resource group       | **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>** resource group. |
   | Container app name	  | Enter **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>**           |
   |||

   ![](./Media/acr-ui-8.png)

1. On the **Container** tab of **Create Container App** blade specify the following details and select **Next:Ingress>**.

   | **Settings**         | **Values**           | 
   | -------------------- | -------------------- | 
   | Use quickstart image | Unselect the checkbox | 
   | Name                 | **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>** |
   | Image source         | Select Azure Container Registry  |
   | Registry             | Select miyagiacr<inject key="DeploymentID" enableCopy="false"/>.azurecr.io |
   | Image                | Select miyagi-ui            |
   | Image tag     	     | Latest         | 
   | CPU and Memory       | 1 CPU cores, 2Gi memory        | 
   |||

   ![](./Media/acr-ui-9.png)

1. On the **Ingress** tab of **Create Container App** blade specify the following details, select **Review + Create** and **Create**.

   | **Settings**         | **Values**           | 
   | -------------------- | -------------------- | 
   | Ingress              | select the checkbox | 
   | Ingress traffic      | Select **Accepting traffic from anywhere**             |
   | Ingress type  	     | Select **HTTP**            |
   | Client certificate mode | Select **Ingore** |
   | Insecure connections | Select checkbox **Allowed**        |
   | Target port   	     | **3000**         |
   |||

   ![](./Media/acr-ui-010.png)
   
   
1. Once deployment gets success click on **Go to resource**
   ![](./Media/acr-ui-11.png)
1. On **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>** page, from left navigation pane select **Ingress** and click on **Endpoints** URL link.
   ![](./Media/acr-ui-12.png)

1. You should get swagger page for the recommendation service as depicted in the image below.
   ![](./Media/acr-ui-13.png)


### Task 6: Update Container App Recommendation service URL for miyagi-ui 

1. On Azure Portal page, in Search resources, services and docs (G+/) box at the top of the portal, enter **Container Apps (1)**, and then select **Container Apps (2)** under services.
   ![](./Media/cntr1.png)
1. On **Container Apps** blade, select **miyagi-rec-ca-<inject key="DeploymentID" enableCopy="false"/>**.
   ![](./Media/cntr2.png)

1. On **miyagi-rec-ca-<inject key="DeploymentID" enableCopy="false"/>** page, from left navigation pane select **Ingress** and copy **Endpoints** URL link.
   ![](./Media/cntr3.png)

1. Reture to **Visual Studio Code**, navigate to **miyagi>ui>.env.** and replace existing code for **RECCOMMENDATION_SERVICE_URL** with copied for **Endpoints** and save the file 
   ![](./Media/cntr4.png)

1. Right - click on **ui/typescript** in cascading menu, select **Open in intergate Terminal**.
1. Run following command to login.

   **Note**: Please replace **[ACRname>]** and **[uname]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>** for **[password]** navigate to resource group > and select **miyagiacr<inject key="DeploymentID" enableCopy="false"/>** from list of recources and under settings select **Access Keys**, select check box for **Admin user** and copy the password.

    ```
    docker login [ACRname].azurecr.io -u [uname] -p [password]
    ```
   
1. Run the following command to push image to container registery

   **Note**: Please replace **miyagiacr[DID]** with **miyagiacr<inject key="DeploymentID" enableCopy="false"/>**

   ```
    docker push miyagiacr[DID].azurecr.io/miyagi-recommendation:latest

   ```

1. Reture to **Azure Portal** in Search resources, services and docs (G+/) box at the top of the portal, enter **Container Apps**, and then select **Container Apps** under services.

1. On **Container Apps** blade, select **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>**.
    ![](./Media/cntr5.png)

1. On **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>**,from left navigation pane select **Ingress** and click on **Endpoints** URL link.
   ![](./Media/cntr6.png)

1. you should get miyagi app running locally as depicted in the image below.
   ![](./Media/cntr7.png)

1. Return to **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>** page, under **Application** select **Revisions** and click on **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>**, on **Revision details** window, select **Refresh**.

   ![](./Media/cntr8.png)

1. when **Are you sure you want to restart the revision?** prompt on **Restart revision** window clcik on **Continue**.
   ![](./Media/cntr9.png)

1. Once restarting is done for **miyagi-ui-ca-<inject key="DeploymentID" enableCopy="false"/>** from left pane select **Ingress** and click on **Endpoints**.
    ![](./Media/cntr6.png)

1. you should get miyagi app running locally as depicted in the image below.
   ![](./Media/cntr10.png)

