# Lab 3 - Expose Open AI through APIM

In this lab, you'll be verifying and creating APIs in the deployed API Management service to updating the Docker image for the Recommendation service. The revision of the Recommendation service from the Container App encapsulates the meticulous approach to maintaining and optimizing containerized applications within the project's scope.

### Task 1: Verify the deployed API Management service and create an API

1. Navigate to Azure portal, open the Resource Group named **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>**  and select **miyagi-apim-<inject key="DeploymentID" enableCopy="false"/>** API Management service from the resources list.

   ![](./Media/lab3-t1-s1.png)

1. From the left-menu, click on **APIs** **(1)** and select **HTTP** **(2)** under Define a new API to create an HTTP API.

   ![](./Media/lab3-t1-s2.png)

1. Enter the following values in the Create an HTTP API pane:
   
   | **Parameter**        | **Values**           | 
   | -------------------- | -------------------- | 
   | API Type **(1)**     | **Basic**            | 
   | Display name **(2)** | **miyagi-api**       |
   | Name **(3)**         | **miyagi-api**       |
   | Web service URL **(4)** | Enter the Endpoint of Azure OpenAI Endpoint  **<inject key="OpenAIEndpoint" enableCopy="true"/>**  |
   | API URL suffix **(5)** | **openai** |
   | Click on  **(6)** | **Create** |

   ![](./Media/apim1.png)

1. Once API is created, click on **Overview** **(1)** from the left-menu copy the **Developer portal URL** and **Gateway URL** **(2)** of API Management service. Paste it into Notepad for later use.

   ![](./Media/api-key.png)

### Task 2: Create API Management Policy and Roles

1. Inside API Management select **APIs** **(1)**, click on the **three dots** **(2)** next to miyagi-api, select **Import** **(3)** and click on **OpenAPI** **(4)**.

   ![](./Media/apim2.png)

2. Once the popup shows paste the below link into the **textbox** **(1)**, select update, and then click **import** **(2)**. 

   ```
   https://raw.githubusercontent.com/Azure/azure-rest-api-specs/main/specification/cognitiveservices/data-plane/AzureOpenAI/inference/stable/2023-05-15/inference.json
   ```

   ![](./Media/apim3.png)

3. You should now see a series of APIs under the Azure OpenAI Service API.
   
    ![](./Media/apim4.png)

4. Navigate to the **settings** **(1)** tab and update the subscription key **Header Name** to **api-key** **(2)** and click on **Save** **(3)**.

   ![](./Media/apinew1.png)

5. In the **miyagi-apim-<inject key="DeploymentID" enableCopy="false"/>** API Management service, click on **Products** **(1)** under APIs from the left menu and click on **+ Add** **(2)**.

   ![](./Media/apim6.png)

6. In the **Add product** display name as **OpenAi** **(1)**, description as **OpenAI** **(2)**. Under the APIs menu click the **plus sign** add the **Azure OpenAI Service API** **(3)** hit Enter and click on **Create** **(4)**.

   ![](./Media/apim7.png)

7. In the **miyagi-apim-<inject key="DeploymentID" enableCopy="false"/>** API Management service, click on **subscriptions** **(1)** under APIs from the left menu and click on **+ Add subscription** **(2)**.

   ![](./Media/apim8.png)

8. In the **Add subscription**, enter the Name as **aoai-test** **(1)**, enter Display name as **AOAI Test** **(2)**, and click on **Create** **(3)**.

   ![](./Media/apim9.png)

8. Once the subscription is created click the **three dots** **(1)** next to the newly created key and then click **Show\hide keys** **(2)**. Copy the **primary subscription** **(3)** key and save it for later.

   ![](./Media/apim10.png)

9. Navigate to your **Azure OpenAI** in the Azure Portal, select the **OpenAIService-<inject key="DeploymentID" enableCopy="false"/>** Azure OpenAI resources.

10. In the **OpenAIService-<inject key="DeploymentID" enableCopy="false"/>**, select **Access control (IAM)** **(1)**, click on **+ Add** **(2)**, and select **Add role assignment** **(3)**.

    ![](./Media/apinew2.png)
   
11. In **Add role assignment** tab in the search bar search and select **Cognitive Services User** and click on **Next**.

    ![](./Media/apinew3.png)

12. In the **Members** tab, select **Managed identity** **(1)**, click on **+ Select Members** **(2)** in the select managed identity pop-up under Managed identity the drop-down select **API Management service** **(3)**, select the **miyagi-apim-<inject key="DeploymentID" enableCopy="false"/>** **(4)**, then click-on **Select** **(5)** and click on **Next** **(6)**.

    ![](./Media/apim-role.png)

13. In the **Review + assign** tab click on **Review + assign**.

    ![](./Media/apim-role1.png)

14. In the **API Management resource**, select **APIs** **(1)**, select the **Azure OpenAI Service API** **(2)** API created in the earlier step, select **All Operations** **(3)** and click on **Policy code editor** **(4)**.

    ![](./Media/api-api1.png)

15. In the code editor copy the below policy to overwrite the **inbound** **(1)** tags only, replace **<<API_MANAGMENT_URL>>** with **Developer portal URL** **(2)** of API manager which you copen in task 1 step 4 and click on **Save** **(3)**.

      ```
      <inbound>
         <base />
         <set-header name="api-key" exists-action="delete" />
         <authentication-managed-identity resource="https://cognitiveservices.azure.com" output-token-variable-name="msi-access-token" ignore-error="false" />
         <set-header name="Authorization" exists-action="override">
            <value>@("Bearer " + (string)context.Variables["msi-access-token"])</value>
         </set-header>
         <set-backend-service base-url="https://<<API_MANAGMENT_URL>>/openai" />
      </inbound>
      ```

    ![](./Media/api-inbound.png)

16. In API Management, click on **Test** **(1)**, select Creates a **completion for the chat message** **(2)**, enter **gpt-35-turbo** **(3)** in the deployment-id field, enter **2023-05-15** **(4)** in the API version field, and click **Send** **(5)**. 

     ![](./Media/apim-test.png)

17. Scroll down the response and you should see a 200 response and a message back from your OpenAI service.

    ![](./Media/openai-response.png)

### Task 3: Update the Docker Image for the Recommendation service

1. Navigate to Visual Studio Code, and open the `appsettings.json` file from the path `miyagi/services/recommendation-service/dotnet`.

   ![](./Media/lab3-t2-s1.png)

1. In the `appsettings.json` file, you have to replace the **endpoint** value from **OpenAI resource endpoint** with **API Gateway URL** which you have copied in Task-1 Step-4, **apiKey** value with the **subscription key** that was copied in Task-2 Step-9 and Save the file.

   ![](./Media/api-update.png)

1. From the Explorer, navigate to `Miyagi/services/recommendation-service/dotnet/` **(1)** path. Right-click on `dotnet` folder and select **Open in Integrated Terminal** **(2)** from the options tab to open the terminal with the required path.

   ![](./Media/lab3-t2-s3.png)

1. Now, you need to re-build the docker image for recommendation service by running the below docker command. Make to update the docker image name which was created earlier for recommendation service with the same name.

   ```
   docker build . -t miyagi-recommendation
   ```

   ![](./Media/lab3-t2-s4.png)

1. Run the following command to ACR login.

   > **Note**: Please replace **[ACRname]** with **<inject key="AcrLoginServer" enableCopy="true"/>**, **[uname]** with **<inject key="AcrUsername" enableCopy="true"/>**, and **[password]** with **<inject key="AcrPassword" enableCopy="true"/>**.

    ```
    docker login [ACRname] -u [uname] -p [password]
    ```

1. Once you are logged into ACR. Run the below command to push the updated docker image of the recommendation service to the container registry.

   **Note**: Make sure to replace **[ACRname]** with **<inject key="AcrLoginServer" enableCopy="true"/>**.

   ```
   docker push [ACRname]/miyagi-recommendation:latest
   ```

   ![](./Media/lab3-t2-s5.png)

### Task 4: Revision of Recommendation service from Container App

1. Navigate to Azure portal, open the Resource Group named **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>**  and select **ca-miyagi-rec-<inject key="DeploymentID" enableCopy="false"/>** Container App from the resources list.

   ![](./Media/continer-app-new1.png)

1. In the **ca-miyagi-rec-<inject key="DeploymentID" enableCopy="false"/>** Container App pane, select **Revisions and replicas** **(1)** under Applications from left-menu and then open the **Active Revision** named **ca-miyagi-rec-<inject key="DeploymentID" enableCopy="false"/>** **(2)**.

   ![](./Media/continer-app-new2.png)

1. You will see the **Revision details** pop-up in the right-side, click on **Restart**. You will see a pop-up to restart the revision, click on **Continue** to confirm.

   ![](./Media/continer-app-new3.png)

   ![](./Media/lab3-t3-s3.1.png)

1. You will see the notification once the Revision is restarted successfully.

   ![](./Media/lab3-t3-s4.1.png)

1. Select **Ingress** **(1)** under Settings from the left menu and then scroll down to Endpoints of Container App i.e, **ca-miyagi-rec-<inject key="DeploymentID" enableCopy="false"/>-SUFFIX** **(2)**. Click on the secured link to open it.

   ![](./Media/continer-app-new5.png)

1. You can see the swagger page for the recommendation service as shown in the below image:

   ![](./Media/lab3-t3-s5.png)

### Task 5: Setup Event Hub Logging and Validate Input

1. Navigate to your event hub in the Azure Portal and select the Identity and Access Management tab. Select Add and role-assignment and at the next screen select Azure Event Hubs Data Sender, click next, then the managed identity radio button, and select memebers. In the managed identity drop down you should see your API Management, select the manage identity and click select. Once finished select Review and Assign and save the role assignment.
    ![](./Media/apim-role.png)

2. Navigate to your event hub in the Azure Portal and select event hubs, then select your event hub name. In the left menu select share access policies and create a new policy that can send data.
    ![](./Media/eh-access-policy.png)

3. Next navigate to the shared access policy and copy the Connection string–primary key to your clipboard.

4. Navigate to the `miyagi` root folder in your file explorer and create a new file called aoai-logger.bicep. Paste the content into that file and update the <<API_MANAGEMENT_NAME>> name and the <<EVENT_HUB_CONNECTION_STRING>> copied from the step above.

   ```
   resource existingApiManagement 'Microsoft.ApiManagement/service@2023-03-01-preview' existing = {
      name: '<<API_MANAGEMENT_NAME>>'
    }
    
    resource ehLoggerWithConnectionString 'Microsoft.ApiManagement/service/loggers@2023-05-01-preview' = {
      name: 'AOAILogger'
      parent: existingApiManagement
      properties: {
        loggerType: 'azureEventHub'
        description: 'Event hub logger with connection string'
        credentials: {
          connectionString: '<<EVENT_HUB_CONNECTION_STRING>>'
          name: 'ApimEventHub'
        }
      }
    }
   ```

6.  Navigate to the `miyagi` root folder in your terminal and execute the below command to run the bicep file.
   ```
    az deployment group create --resource-group <<RESOURCE_GROUP_NAME>> --template-file .\aoai-logger.bicep
   ```
    
6. In the Azure Portal navigate back to the API Management resource and select APIs. Select the Azure OpenAI Service API create in the earlier step and select All Operations. Copy the below policy to overwrite the **outbound** tags only.
   ```
   <outbound>
      <base />
      <choose>
         <when condition="@(!context.Variables.GetValueOrDefault<bool>("isStream"))">
               <log-to-eventhub logger-id="AOAILogger" partition-id="0">@{
               var responseBody = context.Response.Body?.As<JObject>(true);
               return new JObject(
                  new JProperty("prompt_tokens", responseBody["usage"]["prompt_tokens"].ToString()),
                  new JProperty("total_tokens", responseBody["usage"]["total_tokens"].ToString())
               ).ToString();
         }</log-to-eventhub>
         </when>
      </choose>
   </outbound>
   ```

7. Inside the Azure portal navigate to your Event Hub, select Event Hubs, and click on your event hub name. Next click process data and find the Process your Event Hub data using Stream Analytics Query Language and click start
    ![](./Media/eventhub-processdata.png)

8. Next open the Miyagi UI in a separate browser tab and change your stock preferences. In the Event Hub query you should see log information for the tokens used.
    ![](./Media/event-hub-data.png)
