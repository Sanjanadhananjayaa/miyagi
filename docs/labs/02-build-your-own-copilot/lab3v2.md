# Lab 3 - Explore and Verify the Containerized Miyagi UI and Recommendation service in Azure Container Apps

In this lab, you'll be verifying and creating APIs in the deployed API Management service to updating the Docker image for the Recommendation service. The revision of the Recommendation service from the Container App encapsulates the meticulous approach to maintaining and optimizing containerized applications within the project's scope.

### Task 1: Explore Recommendation service in Azure Container Apps using Ingress Endpoint

1. In the Azure Portal page, in the Search resources, services, and docs (G+/) box at the top of the portal, enter **Container Apps (1)**, and then select **Container Apps (2)** under services.

   ![](./Media/container-app-select.png)

1. In the **Container Apps** blade, select **ca-miyagi-rec-<inject key="DeploymentID" enableCopy="false"/>**.

   ![](./Media/container-ca-miyagi.png)

1. In the **ca-miyagi-rec-<inject key="DeploymentID" enableCopy="false"/>** page, from left navigation pane select **Ingress** **(1)** under setting session and click on **Endpoints** **(2)** URL link.

   ![](./Media/container-ca-ingress.png)

1. You can view the **miyagi Recommendation service** website running through the Container Apps.

   ![](./Media/online-output-recommendation.png)    

### Task 2: Explore Miyagi App in Azure Container Apps using Ingress Endpoint

1. Return to **Azure Portal** in Search resources, services box at the top of the portal, enter **Container Apps**, and then select **Container Apps** under services.

1. In the **Container Apps** blade, select **ca-miyagi-ui-<inject key="DeploymentID" enableCopy="false"/>**.

   ![](./Media/container-select-ui.png)

1. In the **ca-miyagi-ui-<inject key="DeploymentID" enableCopy="false"/>**, from left navigation pane select **Ingress** under setting and click on **Endpoints** URL link.

   ![](./Media/container-ui-ingress.png)

1. You can view the **miyagi app** running through the Container Apps.

   ![](./Media/online-output-u.png)   

