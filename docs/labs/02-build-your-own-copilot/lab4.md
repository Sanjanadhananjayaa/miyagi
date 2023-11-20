# Getting Started with Your Own Copilot

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

    ![](./Media/location.png)

 1. Click on **Yes,I trust authors**

    ![](./Media/trustauthor.png)

 1. Expand **Config**, rename **appsettings.json.azure-openai-example** to **appsettings.json** replace and save the file

   | **Variables**                | **Values**                                                    |
   | ---------------------------- |---------------------------------------------------------------|
   | serviceId                    |  **gpt-3.5-turbo **                                           |
   | deploymentOrModelId          | **<inject key="CompletionModel" enableCopy="true"/>**         |
   | endpoint                     | **<inject key="OpenAIEndpoint" enableCopy="true"/>**          |
   | apiKey                       | **<inject key="OpenAIKey" enableCopy="true"/>**               |
    
 1. Configure an Azure OpenAI endpoint by Opening a **Terminal**, Replace the value and run it
  
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

1. Configure the **Semantic Kernel logging level**

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
