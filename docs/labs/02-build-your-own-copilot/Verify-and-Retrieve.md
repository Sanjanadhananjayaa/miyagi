# Laboratorio 1 - Verificar y Recuperar los valores de los Recursos de Azure (opcional)

En esta tarea, se verifican y recuperan valores específicos, incluidos el punto de conexión, la cadena de conexión y la clave para los recursos designados.

   - Azure OpenAI : **OpenAIService-<inject key="DeploymentID" enableCopy="false"/>** 
   - Cuenta de Azure Cosmos DB : **cosmos-<inject key="DeploymentID" enableCopy="false"/>**
   - Servicio Search : **acs-<inject key="DeploymentID" enableCopy="false"/>**

1. Para verificar los nombres de los modelos implementados para "**deploymentOrModelId**" y para "**embeddingDeploymentOrModelId**" siga los pasos a continuación:
   
      - En el Portal de Azure, haga clic en **Grupos de Recursos** en el panel Navegar.

      - Desde la página Grupos de Recursos, haga clic en **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>**.

         ![](./Media/image-rg-1.png)

      - En **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>**, en la pestaña Información general (1) seleccione **OpenAIService-<inject key="DeploymentID" enableCopy="false"/> (2)**.

        ![](./Media/select-openai.png)

      - En la página **Información General de OpenAI** **(1)**, haga clic derecho en el botón **Ir a Azure OpenAI Studio** (2) y haga clic en **Abrir enlace en una pestaña nueva**.

         ![](./Media/image-rg-03.png) 
   
      - En **Azure AI Studio**, seleccione **Implementaciones**, en la sección Administración.

      - En la hoja **Implementaciones** de Azure AI Studio, haga clic en el nombre de modelo **(1)** **gpt-35-turbo**  y verifique el **nombre de la implementación** del modelo **(2)** gpt-35-turbo.

          ![](./Media/image-rg-7.png)
        
          ![](./Media/image-rg-8.png)
      
      -  Vuelva a la página **Implementaciones**.

      - En la hoja Implementaciones de Azure AI Studio, haga clic en el **nombre del modelo (1) text-embedding-ada-002** y verifique el **nombre de la implementación** del **modelo (2) text-embedding-ada-002**.
        
         ![](./Media/image-rg-10.png)

         ![](./Media/image-rg-11.png)

1. Para verificar los valores para **endpoint** y **apiKey**, siga los pasos a continuación:

   -  Vuelva a la pestaña que muestra **Portal de Azure**. 

   -  En la hoja **OpenAIService-<inject key="DeploymentID" enableCopy="false"/>** en la sección **Administración de Recursos**, seleccione **Claves y punto de conexión**, verifique **Clave 1** y **Extremo**.
     
      ![](./Media/image-rg-3.png)

1. Para verificar los valores para  "azureCognitiveSearchEndpoint" y  "azureCognitiveSearchApiKey", siga los pasos a continuación:
   
   - Vuelva al grupo de recursos **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>**.

   - En la página **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>**, seleccione **acs-<inject key="DeploymentID" enableCopy="false"/>** de la lista de recursos.

      ![](./Media/select-acs.png)
 
   - En la hoja **acs-<inject key="DeploymentID" enableCopy="false"/>**, verifique la **URL**.
   
      ![](./Media/image-rg-13.png)

   - En la hoja **acs-<inject key="DeploymentID" enableCopy="false"/>**, en la sección **Configuración**, verifique el valor **Clave de administrador principal**.
   
      ![](./Media/image-rg-14.png)

1. Para verificar los valores de "**cosmosDbUri**" y "**cosmosDbName**," por favor siga los pasos a continuación:

   - Vuelva a la página del grupo de recursos **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>**, seleccione **cosmos-<inject key="DeploymentID" enableCopy="false"/>** de la lista de recursos.

     ![](./Media/select-cosmosdb.png)

   - En **cosmos-<inject key="DeploymentID" enableCopy="false"/>** verifique la **URL**.
     
     ![](./Media/image-rg-16.png)

   - En **cosmos-<inject key="DeploymentID" enableCopy="false"/>** en **Configuración**, seleccione **Claves** y verifique el valor de la **Cadena de Conexión Principal de Cosmos DB**.

     ![](./Media/cs.png)

1. Para obtener los valores de  "**blobServiceUri**", por favor siga los pasos a continuación:

   - Vuelva a la página del grupo de recursos **miyagi-rg-<inject key="DeploymentID" enableCopy="false"/>**, seleccione **miyagiblobstorge<inject key="DeploymentID" enableCopy="false"/>** de la lista de recursos.

     ![](./Media/select-storage.png)

   - En la cuenta de almacenamiento **miyagiblobstorge<inject key="DeploymentID" enableCopy="false"/>**, del menú de la izquierda seleccione **Puntos de conexión** **(1)** en Configuración y verifique **Blob service** **(1)** en Blob service.

     ![](./Media/blob-storage-endpoint.png)

1. Ahora, haga clic en **Siguiente** en la esquina inferior derecha para pasar a la página siguiente.
