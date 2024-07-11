# Lab 3.1: Containerizing Miyagi UI and Recommendation service to Azure Kubernetes Service(AKS)

In this lab, you'll be building the docker images and publishing them to Azure Kubernetes Service(AKS).

### Task 1: Deploy AKS Services

In this task, You will deploy the Miyagi recommendation and UI services on an Azure Kubernetes Service (AKS) cluster. This involves logging into the Azure portal, applying Kubernetes configurations, and updating configuration files with the services' external IP addresses.

1. Navigate back to the Visual Studio code window and navigate to **miyagi/deploy/infrastructure/kubernetes/manifests/50-miyagi**, right-click on **50-miyagi** in cascading menu, and select **Open in integrate Terminal**.

    ![](./Media/aks-01.png)

2. Run the following command to log in to the Azure portal.

   > **Note**: replace [ClusterName] with **<inject key="aksname" enableCopy="true"/>** and [ResourceGroupName] with **<inject key="rgname" enableCopy="true"/>**

   ```
   az aks get-credentials -n [ClusterName] -g [ResourceGroupName]
   ```

   >**Important** : The command az aks get-credentials -n [ClusterName] -g [ResourceGroupName] is used in Azure's command-line interface (CLI) to retrieve and merge the Kubernetes configuration files for a specified Azure Kubernetes Service (AKS) cluster into the local kubeconfig file.
   
3. Once the command finishes you should now have access to the cluster and can run the following commands to deploy the application services.

   ```
   kubectl apply -f ./miyagi-recommendation-service.yaml
   ```
   ```
   kubectl apply -f ./miyagi-ui-service.yaml
   ```


   >**Important**: Upon successful execution of above commands The Kubernetes will read the YAML file and apply its configurations to the cluster.
It will create miyagi-recommendation-service and miyagi-ui

   > Upon successful execution of the command kubectl apply -f ./miyagi-recommendation-service.yaml, Kubernetes will read the YAML file (miyagi-recommendation-service.yaml) and apply its configurations to the cluster. This will result in the creation of the miyagi-recommendation-service along with any other Kubernetes resources defined in the YAML file, such as deployments, services, config maps, etc. Additionally, if you have another YAML file for miyagi-ui and apply it similarly (kubectl apply -f ./miyagi-ui.yaml), it will create or update the miyagi-ui service and related resources in the cluster as specified in that file.

   
4. Once the services have been deployed run the below command and keep track of the service's **external ip's**. It could take a few minutes for the **external ip's** to appear so wait a few minutes before running the command.

   ```
   kubectl get svc
   ```

   ![](./Media/external-ip.png)

5. Next navigate to **miyagi/services/recommendation-service/dotnet** and open app settings.

   ![](./Media/aks-02.png)
   
6. Copy the **miyagi-ui** External IP address from the console and paste it in the **CorsAllowedOrigins** section formatted as an **http** endpoint  and save the file by **Ctrl + S**.  

   ![](./Media/ui-cors.png)

7. Next navigate to **miyagi/ui/typescript** and open the **.env** file. 

   ![](./Media/aks-03.png)

8. Copy the **miyagi-recommendation-service** External IP address from the console and paste it in the **NEXT_PUBLIC_RECCOMMENDATION_SERVICE_URL** value and save the file by **Ctrl + S**.

   ![](./Media/miyagi-ui-env.png)

### Task 2: Build a Docker Image for the Miyagi UI
In this task, you will build and run the Miyagi UI Docker container locally. Begin by opening Docker Desktop and completing the initial setup. Next, use Visual Studio Code to build the Docker image for the Miyagi UI. Once the image is created, verify it and run the image in Docker. Configure the host port and access the application locally via the provided URL.

1. Open the **Docker** Application from the Lab VM desktop by double-clicking on it.

   ![](./Media/docker1.png)
   
2. In the **Docker Subscription Service Agreement** window, click **Accept**.

   ![](./Media/docker2.png)

3. In the **Welcome to Docker Desktop** window, click on **Continue without signing in**.

   ![](./Media/without-signin.png)

4. In the **Tell us about the work you do** window, click on **Skip**.
   
5. Navigate back to **Visual studio code** window and navigate to **miyagi/ui/typescript** right - click in cascading menu, select **Open in integrate Terminal**.

   ```
   docker build . -t miyagi-ui      
   ```

   > **Note**: Please wait as this command may require some time to complete.

   > **Note**: this command reads instructions from the Dockerfile, processes them to create a Docker image based on those instructions, and then tags the resulting image with the name miyagi-ui.
   
6. Run the following command to get the newly created image.

   ```
   docker images
   ```
7. Navigate back to **Docker desktop**, from the left pane select **Images**.

   ![](./Media/docker7.png)

8. In the **Images** blade, notice **miyagi-ui(1)** image is created, select **run(2)** icon .

   ![](./Media/docker-miyagi-ui.png)

9. In the **Run a new container** window select the dropdown arrow.

   ![](./Media/docker14-(1).png)

10. In the **Run a new containe**, under **Ports** for **Host Port** enter **3000** and click on **Run**.

    ![](./Media/ui-port.png)

11. Click on **3000:3000** URL link

    ![](./Media/ui-port-open.png)
   
12. You should be able to see the application running locally
   
     ![](./Media/docker-ui.png)

### Task 3: Build Docker Images for the Recommendation service

1. Navigate back to **Visual studio code** window and navigate to **miyagi/services/recommendation-service/dotnet** right - click on dotnet in cascading menu, select **Open in integrate Terminal**.

   ![](./Media/aks-04.png)

1. Run the following command to build a **Docker image**

   ```
   docker build . -t miyagi-recommendation      
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

    ![](./Media/recommendation-port-new.png)

1. Click on **5224:80** URL link

   ![](./Media/recommendation-port-open.png)
   
1. You should be able to see the application running locally
   
   ![](./Media/docker-recommend.png)

### Task 4: Push the Docker Image of Recommendation service to the Container registry

In this task, you'll Push miyagi-recommendation images to acr. 

1. Navigate back to the **Visual studio code** window and navigate to **miyagi/services/recommendation-service/dotnet** right - click on dotnet in cascading menu, select **Open in integrate Terminal**

1. Run the following command to log in to the **Azure portal**.

    ```
    az login
    ```

1. This will redirect to **Microsoft login page**, select your Azure account **<inject key="AzureAdUserEmail"></inject>**, and navigate back to the **Visual studio code**.

   ![](./Media/azure-account-select.png)

1. Run the following command to log in to an **Azure Container Registry (ACR)** using the Azure CLI.

   > **Note**: Please replace **[ACRname]** **<inject key="AcrUsername" enableCopy="true"/>**.
   
   ```
   az acr login -n [ACRname] 
   ```
    
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

1. Navigate back to **Visual studio code** window and navigate to **miyagi/ui/typescript** right - click in cascading menu, select **Open in integrate Terminal**.

1. Run the following command to add the tag.

   > **Note**: Please replace **[ACRname]** with **<inject key="AcrLoginServer" enableCopy="true"/>**.

   ```
   docker tag miyagi-ui:latest [ACRname]/miyagi-ui:latest
   ```

1. Run the following command to push the image to the container registry.

   > **Note**: Please replace **[ACRname]** with **<inject key="AcrLoginServer" enableCopy="true"/>**.

   ```
   docker push [ACRname]/miyagi-ui:latest
   ```

### Task 5: Deploy AKS Pods

1. Navigate back to the Visual Studio code window and navigate to **miyagi/deploy/infrastructure/kubernetes/manifests/50-miyagi** click on **50-miyagi** in the cascading menu, and select **Open in integrate Terminal**.

    ![](./Media/aks-01.png)

2. Open the **miyagi-recommendation.yaml** file and replace the &lt;ACR-NAME&gt; with **<inject key="acrUsername" enableCopy="true"/>** Azure container registry name created earlier and save the file by **Ctrl + S**.

   ![](./Media/service-acr.png)

4. Open the **miyagi-ui.yaml** file and replace the &lt;ACR-NAME&gt; with **<inject key="acrUsername" enableCopy="true"/>** Azure container registry name created earlier and save the file by **Ctrl + S**.

   ![](./Media/ui-acr.png)

5. Run the following commands to deploy the application pods.

   ```
    kubectl apply -f ./miyagi-recommendation.yaml
   ```
   ```
    kubectl apply -f ./miyagi-ui.yaml
   ```

6. The applications should now be deployed. To verify run the below command and you should see both pods in a running state.

   >**Note** : It could take a few minutes for the output to appear so wait a few minutes before running the command.
   
   ```
    kubectl get pods
   ```
   
   ![](./Media/AKS-running.png)

 
   >**Congratulations** on completing the Task! Now, it's time to validate it. Here are the steps:
> - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
> - If you need any assistance, please contact us at labs-support@spektrasystems.com.

    <validation step="f50c7e4e-0b5a-4ae2-bd9e-ff29a023f1d2" />

# Lab 3.2: Explore and Verify the Containerized Miyagi UI and Recommendation service in AKS

### Task 1: Explore Recommendation service in AKS using Ingress Endpoint

1. To test the API run the below command to get the service IP addresses

   >**Note** : It could take a few minutes for the output to appear so wait a few minutes before running the command.

   ```
   kubectl get svc
   ```
   
   ![](./Media/aks-endpoint.png)


2. Copy the External IP address of the **miyagi-recommendation-service** and enter it into the browser. You should now see the swagger endpoint.
   
   ![](./Media/service-swagger.png)
   
### Task 2: Explore Miyagi App in AKS using Ingress Endpoint

1. To test the UI run the below command to get the service IP addresses
   ```
   kubectl get svc
   ```
   
   ![](./Media/aks-endpoint.png)

2. Copy the External IP address of the **miyagi-ui** and enter it into the browser. You should now see the Miyagi frontend.

   ![](./Media/miyagi-ui.png)

### Summary

In this Lab, you deployed Azure Kubernetes Service (AKS) for both the Miyagi UI and Miyagi Recommendation service. It began with constructing Docker images for these services, containing all necessary components like code and configuration files. After image creation, the next step involved pushing the Recommendation service's Docker image to a Container registry, a storage and deployment platform for Kubernetes clusters. Finally, AKS pods were deployed, representing running containers within the Kubernetes cluster, thereby making the Miyagi UI and Recommendation service operational
