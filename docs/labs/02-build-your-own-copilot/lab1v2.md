# Laboratorio 2 - Ejecutar la Aplicación Miyagi Localmente

### Duración: 80 minutos

En esta práctica de laboratorio, la atención se centra en configurar la aplicación Miyagi para que esté lista para funcionar. Posteriormente, la atención se centra en comprender la implementación matizada del servicio de Recomendación. La fase práctica implica ejecutar el servicio de Recomendación e implementar el frontend Miyagi localmente para pruebas y desarrollo. Un paso crucial incluye optimizar la eficiencia de la recuperación de datos mediante la persistencia de los embeddings en Azure AI Search. El proyecto culmina con una exploración más amplia de la aplicación Miyagi y el servicio de Recomendación, enfatizando una experiencia de usuario personalizada. Este enfoque basado en tareas garantiza una progresión sistemática a través de las complejidades del proyecto, facilitando una comprensión integral y una implementación efectiva.

### Tarea 1: Configuración de la aplicación miyagi

1. Abra **Visual Studio Code** desde el escritorio de Lab VM haciendo doble clic en el acceso directo.

   ![](./Media/vs.png)

   >**Nota** Si aparece la ventana **¡Únase a nosotros para mejorar la extensión promt-flow!**, haga clic en **No,gracias**.

   ![](./Media/image-rg-01.png)
   
1. En **Visual Studio Code**, de la barra de menú seleccione **Archivo(1)>Abrir carpeta(2)**.

   ![](./Media/image-rg-02.png)

1. Dentro del **Explorador de Archivos**, navegue hasta **C:\LabFiles\miyagi** seleccione **miyagi**(1) y haga clic en **Seleccionar carpeta(2)**

   ![](./Media/image-rg(003).png)

1. En **Visual Studio Code**, haga clic en **Sí, confío en los autores** cuando se le solicite en la ventana **¿Confía en los autores de los archivos de esta carpeta?**.

   ![](./Media/image-rg-18.png) 
   
1. Expanda el directorio **miyagi>ui** y verifique que el archivo **.env** está presente. 

1. Expanda el directorio **miyagi/services/recommendation-service/dotnet** y verifique que el archivo **appsettings.json** está presente.

   ![](./Media/open-appsettings.png)
  
1. En el archivo **appsettings.json** reemplace los siguientes valores para las variables siguientes.

   | **Variables**                | **Valores**                                                    |
   | ---------------------------- |---------------------------------------------------------------|
   | deploymentOrModelId          | **<inject key="CompletionModel" enableCopy="true"/>**         |
   | embeddingDeploymentOrModelId | **<inject key="EmbeddingModel" enableCopy="true"/>**          |
   | endpoint                     | **<inject key="OpenAIEndpoint" enableCopy="true"/>**          |
   | apiKey                       | **<inject key="OpenAIKey" enableCopy="true"/>**               |
   | azureCognitiveSearchEndpoint | **<inject key="SearchServiceuri" enableCopy="true"/>**        |
   | azureCognitiveSearchApiKey   | **<inject key="SearchAPIkey" enableCopy="true"/>**            |
   | cosmosDbUri                  | **<inject key="CosmosDBuri" enableCopy="true"/>**             |
   | blobServiceUri               | **<inject key="StorageAccounturi" enableCopy="true"/>**       |
   | bingApiKey                   | **<inject key="Bing_API_KEY" enableCopy="true"/>**           |
   | cosmosDbConnectionString     | **<inject key="CosmosDBconnectinString" enableCopy="true"/>** |
   
   > **Nota**: Para su información, los valores/Claves/Puntos de conexión/Cadena de conexión de los Recursos de Azure están directamente inyectados en esta guía de laboratorio. Deje la configuración predeterminada para "cosmosDbContainerName": "recommendations" y "logLevel": "Trace".

      ![](./Media/appsetting-update.png)
   
1. Una vez que haya actualizado los valores, guarde el archivo presionando **CTRL + S**.

1. Navegue a **miyagi/sandbox/usecases/rag/dotnet** y verifique que el archivo **.env** está presente.
  
1. En el archivo **.env** reemplace los siguientes valores para las variables siguientes.

   | **Variables**                          | **Valores**                                            |
   | ---------------------------------------| ------------------------------------------------------|
   | AZURE_OPENAI_ENDPOINT                  | **<inject key="OpenAIEndpoint" enableCopy="true"/>**  |
   | AZURE_OPENAI_CHAT_MODEL                | **<inject key="CompletionModel" enableCopy="true"/>** |
   | AZURE_OPENAI_EMBEDDING_MODEL           | **<inject key="EmbeddingModel" enableCopy="true"/>**  |
   | AZURE_OPENAI_API_KEY                   | **<inject key="OpenAIKey" enableCopy="true"/>**       |
   | AZURE_COGNITIVE_SEARCH_ENDPOINT        | **<inject key="SearchServiceuri" enableCopy="true"/>**|
   |AZURE_COGNITIVE_SEARCH_API_KEY          | **<inject key="SearchAPIkey" enableCopy="true"/>**    |
   
   ![](./Media/env1new.png)

1. Una vez que haya actualizado los valores, guarde el archivo presionando **CTRL + S**.

  >**¡Felicitaciones** por completar la Tarea! Ahora es el momento de validarla. Aquí están los pasos:
> - Pulse el botón Validar para la tarea correspondiente. Si recibe un mensaje de éxito, ha validado correctamente el laboratorio.
> - De lo contrario, lea atentamente el mensaje de error y vuelva a intentar el paso, siguiendo las instrucciones de la guía de laboratorio. 
> - Si necesita ayuda, contáctenos en labs-support@spektrasystems.com.

   <validation step="d37dd2bb-631a-4ffe-a41e-fc3ef07aa2b5" />

### Tarea 2: Comprender la implementación del servicio de Recomendación

 El servicio de recomendación implementa el patrón RAG utilizando el SDK de Semantic Kernel. Los detalles de la implementación se capturan en el notebook (cuaderno) de Jupyter en la carpeta miyagi/sandbox/usecases/rag/dotnet. Puede abrir el cuaderno en VS Code y ejecutar las celdas para comprender los detalles paso a paso de cómo se implementa el servicio de Recomendación. Preste especial atención a cómo se implementa el patrón RAG utilizando Semantic Kernel. Seleccione el kernel como .NET Interactive en la esquina superior derecha del cuaderno.

1. En Visual Studio Code navegue hasta la carpeta **miyagi/sandbox/usecases/rag/dotnet** y seleccione **Getting-started.ipynb**

   ![](./Media/getting-start-01.png)

1. **Ejecute el cuaderno celda por celda** (ya sea usando Ctrl + Enter para permanecer en la misma celda o con Shift + Enter par avanzar a la siguiente celda) y observe los resultados de la ejecución de cada celda.
  
   > **Nota**: Asegúrese de que **.Net Interactive** se encuentre en el estado Listo. De lo contrario, espere de 15 a 20 segundos. Además, por favor no haga clic en la opción **Ejecutar Todo** para ejecutar todas las celdas a la vez, dado que puede provocar que se exceda el límite de tokens y se produzca el Error: 503 – Servicio inalcanzable. 

      ![](./Media/run.png)

   > **Nota**: En caso de que se produzca algún problema o error relacionado con exceder el límite de tasa de llamadas de su nivel de precios actual de OpenAI S0, espere de 15 a 20 segundos y vuelva a ejecutar la celda.

  >**¡Felicitaciones** por completar la Tarea! Ahora es el momento de validarla. Aquí están los pasos:
> - Pulse el botón Validar para la tarea correspondiente. Si recibe un mensaje de éxito, ha validado correctamente el laboratorio.
> - De lo contrario, lea atentamente el mensaje de error y vuelva a intentar el paso, siguiendo las instrucciones de la guía de laboratorio. 
> - Si necesita ayuda, contáctenos en labs-support@spektrasystems.com.

<validation step="f277b99e-c179-4bb8-b9c1-6479a526ee4b" />

### Tarea 3: Ejecutar el servicio de recomendaciones localmente

1. Abra una nueva terminal: navegando a **miyagi/services/recommendation-service/dotnet** y haciendo clic derecho, en el menú contextual seleccione **Abrir en Terminal Integrada**.

    ![](./Media/task4-1.png)

1. Ejecute el siguiente comando para ejecutar el servicio de recomendación localmente.
    
   ```
   dotnet build
   dotnet run
   ```

   **Nota**: Deje que se ejecute el comando, mientras tanto puede continuar con el siguiente paso.

1. Abra otra pestaña en Edge, en la ventana del navegador pegue el siguiente enlace

   ```
   http://localhost:5224/swagger/index.html 
   ```

   **Nota**: Actualice la página continuamente hasta que obtenga la página swagger para el servicio de recomendación como se muestra en la imagen a continuación.

   ![](./Media/miyagi2.png)


### Tarea 4: Ejecutar el frontend de Miyagi localmente

1. Abra una nueva terminal: navegando a **miyagi/ui** y haciendo clic derecho en **ui/typescript**, en el menú contextual seleccione **Abrir en Terminal Integrada**.

   ![](./Media/image-rg-25.png)

1. Ejecute el siguiente comando para instalar las dependencias
   
    ```
    npm install --global yarn
    yarn install
    yarn dev
    ```

   **Nota**: Por favor espere hasta que el comando se ejecute correctamente. Tardará hasta 5 minutos.

1. Abra una nueva pestaña en Edge, navegue a la siguiente dirección

   ```
   http://localhost:4001
   ```

### Tarea 5: Persistiendo embeddings en Azure AI Search

1. Vuelva a la página **swagger UI**, desplácese hasta la sección **Memory**, haga clic en **POST /dataset** para ampliar y haga clic en **Try it out**.

   ![](./Media/swaggerUI-memory.png)

1. Reemplace el código con el siguiente código y haga clic en **Execute**.

     ```
     {
        "metadata": {
              "userId": "50",
              "riskLevel": "aggressive",
              "favoriteSubReddit": "finance",
              "favoriteAdvisor": "Jim Cramer"
            },
          "dataSetName": "intelligent-investor"
      }
      ```

      ![](./Media/swaggerUI-Execution.png)
      
1. En la página **swagger UI**, desplácese hasta la sección **Responses** revise que se ha ejecutado exitosamente verificando que el código de estado es **200**.

    ![](./Media/swaggerUI-Responses.png)

1. Vuelva a la pestaña **Portal de Azure**, busque y seleccione **AI Search**.

    ![](./Media/ai-search1.png)    

1. En la pestaña **Azure AI services | AI Search**, seleccione **acs-<inject key="DeploymentID" enableCopy="false"/>**.

1. En la pestaña **acs-<inject key="DeploymentID" enableCopy="false"/>** del servicio Search, haga clic en **Índices** **(1)** en Administración de búsquedas y revise que **miyagi-embeddings** **(2)** ha sido creado.   

   ![](./Media/ai-search-update.png)

   > **Nota**: Por favor haga clic en el botón Actualizar mientras ve el **Recuento de Documentos**.

>**¡Felicitaciones** por completar la Tarea! Ahora es el momento de validarla. Aquí están los pasos:
> - Pulse el botón Validar para la tarea correspondiente. Si recibe un mensaje de éxito, ha validado correctamente el laboratorio.
> - De lo contrario, lea atentamente el mensaje de error y vuelva a intentar el paso, siguiendo las instrucciones de la guía de laboratorio. 
> - Si necesita ayuda, contáctenos en labs-support@spektrasystems.com.

   <validation step="940ebf1a-9add-4bf0-a7fd-c6d929961497" />

### Tarea 6: Explorar la Aplicación Miyagi y el servicio de Recomendación mediante Personalización

1. Vuelva a la página de la interfaz de usuario del **servicio de recomendación** y haga clic en el botón **personalize**.

    ![](./Media/service-personalize.png)

1. En la página **Personalize**, seleccione su **asesor financiero** en el menú desplegable y haga clic en **Personalize**.

   ![](./Media/financial-advisor.png)  

1. Debería ver las recomendaciones del servicio de recomendación en el widget Top Stocks.

   ![](./Media/financial-advisor-output.png) 

1. Navegue a **Visual Studio Code** y haga clic en **dotnet** desde la terminal, puede revisar los registros.

   ![](./Media/terminal-output.png)    

1. Una vez que revise los registros, presione **Ctrl + C** para detener la página **swagger UI**.

1. Desde la **Terminal** seleccione la terminal **Node**, presione **Ctrl + C** para detener la página de la interfaz de usuario del **servicio de recomendación**.

1. Ahora, haga clic en **Siguiente** en la esquina inferior derecha para pasar a la página siguiente.

## Resumen

En esta práctica de laboratorio, comenzó con la configuración de la Aplicación Miyagi para su preparación operativa, seguido de una exploración detallada de la implementación del servicio de Recomendación. La ejecución práctica implica ejecutar el servicio de Recomendación e implementar el frontend Miyagi localmente para realizar pruebas. Mejorar la eficiencia de la recuperación de datos es un paso fundamental que se logra mediante la persistencia de embeddings en Azure AI Search. El proyecto concluye con una amplia exploración de la Aplicación Miyagi y el servicio de Recomendación, priorizando una experiencia de usuario personalizada. Este enfoque sistemático garantiza una comprensión profunda y una implementación efectiva durante todo el proyecto.
