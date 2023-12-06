# Lab 1 - Getting Started with Your Own Copilot

Semantic Kernel is an SDK that integrates Large Language Models (LLMs) like OpenAI, Azure OpenAI, and Hugging Face with conventional programming languages like C#, Python, and Java. Semantic Kernel achieves this by allowing you to define plugins that can be chained together in just a few lines of code.

What makes Semantic Kernel _special_, however, is its ability to _automatically_ orchestrate plugins with AI. With Semantic Kernel
[planners](https://learn.microsoft.com/en-us/semantic-kernel/ai-orchestration/planner), you can ask an LLM to generate a plan that achieves a user's unique goal. Afterwards, Semantic Kernel will execute the plan for the user.

### Task 1: Configure and Run the Semantic Kernel Sample

1. Open **Visual Studio Code** from the Lab VM desktop by double-clicking on it.

   ![](./Media/vs.png)

1. In the **Visual Studio Code** from the left panel select **Semantic Kernel** **(1)** plugin, expand  by click on **AIENDPONTS(OPEN AI)** **(2)**, click on **Switch EndPoint Provider** **(3)**, and select **AzureOpenAI** **(4)**.

   ![](./Media/Semantic-select.png)

1. Under **AIENDPONTS(AZURE OPENAI)**, click on **sign in to Azure** **(1)**, in the pop-up  **The extension 'Semantic Kernel Tools' wants to sign in using Microsoft** click on **Allow** **(2)**.

   ![](./Media/Semantic-sign-in.png)

1. This will redirect to **Microsoft login page**, select your Azure account **<inject key="AzureAdUserEmail"></inject>**, and navigate back to the **Visual studio code**.

   ![](./Media/azure-account-select.png)

1. Navigate back to the **Visual Studio Code** From the **Functions panel**, click on the **Get started icon** and follow the wizard to **create your app** with the semantic function and save it

    ![](./Media/sskernal.png)

1. Choose **C# Hello World**

    ![](./Media/kkernal.png)

1. Browse the location **C:\LabFiles** and **select location for new app**

    ![](./Media/image1-lab(4).png)

1. Click on **Yes, I trust authors**

    ![](./Media/trustauthor.png)

1. Expand **Config**, rename **appsettings.json.azure-example** to **appsettings.json** replace the values .

   | **Variables**                | **Values**                                                    |
   | ---------------------------- |---------------------------------------------------------------|
   | serviceId                    |  **gpt-35-turbo**                                           |
   | deploymentOrModelId          | **<inject key="CompletionModel" enableCopy="true"/>**         |
   | endpoint                     | **<inject key="OpenAIEndpoint" enableCopy="true"/>**          |
   | apiKey                       | **<inject key="OpenAIKey" enableCopy="true"/>**               |

1. Comment the line 2 by adding **//** and save the file .

    ![](./Media/image3-lab4.png)

1. Configure an Azure OpenAI endpoint by Opening a New **Terminal** click on **(...) (1)** next to **View** menu and select **Terminal(2)** > **New Terminal(3)**

    ![](./Media/image2-lab4.png)

1. Replace the value and run it
  
   | **Variables**                | **Values**                                                    |
   | ---------------------------- |---------------------------------------------------------------|
   | deploymentOrModelId          | **<inject key="CompletionModel" enableCopy="true"/>**         |
   | endpoint                     | **<inject key="OpenAIEndpoint" enableCopy="true"/>**          |
   | apiKey                       | **<inject key="OpenAIKey" enableCopy="true"/>**               |
    

   ```powershell
   dotnet user-secrets set "serviceType" "AzureOpenAI"
   dotnet user-secrets set "serviceId" "gpt-35-turbo"
   dotnet user-secrets set "deploymentOrModelId" "gpt-35-turbo"
   dotnet user-secrets set "endpoint" "https:// ... your endpoint ... .openai.azure.com/"
   dotnet user-secrets set "apiKey" "... your Azure OpenAI key ..."
   ```
1. Run the following command to Configure the **Semantic Kernel logging level**

   ```powershell
   dotnet user-secrets set "LogLevel" 0
   ```

   Log levels:

   - 0 = Trace
   - 1 = Debug
   - 2 = Information
   - 3 = Warning
   - 4 = Error
   - 5 = Critical
   - 6 = None
     
1. To build and run the console application from the terminal use the following commands:

   ```powershell
   dotnet build
   dotnet run
   ```
   
   >**Note**: Getting a 400 (BadRequest) and error "Azure.RequestFailedException: logprobs, best_of and echo parameters are not available on gpt-35-turbo model. Please remove the parameter and try again."

   > A chat completion model (gpt-35-turbo) was set in serviceId/deploymentOrModelId while the kernel was configured to use a text completion model. The type of model used by the kernel can be configured with the endpointType secret. To fix, you can either:

   > change endpointType to chat-completion and Re-run step 13 

   ```powershell
   dotnet user-secrets set "endpointType" "chat-completion"
   ```

### Task 2: Configure Azure AI Search

1. Navigate back to the **Azure portal** tab, search and select **AI Search**.

    ![](./Media/ai-search1.png)    

1. In **Azure AI services | AI Search** tab, select **acs-<inject key="DeploymentID" enableCopy="false"/>**.

   >**Note**: Please click on the refresh button still you view the **Document Count**.

1. In the overiew tab of search service, click on the **Import data**.

    ![](./Media/import-data1.png)    
   
1. From the drop-down select **Data Source** as **Sample**, select the **CosmosDB hotels-sample**, and click on **Next: Add cognitive skills(optional)**.

   ![](./Media/import-data2.png)
   
1. In **cognitive skills** leave as default and click on **Customize target index**.

1. In the **Customize target index**, Enter the index name as **realestate-us-sample-index** and click on **Next: Create an indexer**.

   ![](./Media/import-data3.png)

1. In the **create an indexer**, change the indexer name as **realestate-us-sample-indexer** and click on **submit**.

   ![](./Media/import-data4.png)
    
1. Click on **realestate-us-sample-index** in the search bar enter **Seattle** and click on **Search**.

   ![](./Media/final-indexer.png)
