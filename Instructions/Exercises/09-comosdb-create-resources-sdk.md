---
lab:
  title: Creación de recursos de Azure Cosmos DB para NoSQL mediante .NET
  description: Aprende a crear recursos de contenedor y base de datos en Azure Cosmos DB con el SDK de Microsoft .NET v3.
---

# Creación de recursos de Azure Cosmos DB para NoSQL mediante .NET

En este ejercicio, crearás una aplicación de consola que crea un contenedor, una base de datos y un elemento en Azure Cosmos DB.

Tareas realizadas en este ejercicio:

* Creación de una cuenta de Azure Cosmos DB
* Crear la aplicación de consola
* Ejecución de la aplicación de consola y comprobación de los resultados

Este ejercicio debería tardar en completarse **30** minutos aproximadamente.

## Antes de comenzar

Antes de empezar asegúrese de que tiene los siguientes requisitos implementados:

* Suscripción a Azure. Si aún no tiene uno, puede [registrarse para obtenerlo](https://azure.microsoft.com/).

## Creación de una cuenta de Azure Cosmos DB

En esta sección de ejercicio, crearás un grupo de recursos y una cuenta de Azure Cosmos DB. También registrarás el punto de conexión y la clave de acceso de la cuenta.

1. En el explorador, ve a Azure Portal [https://portal.azure.com](https://portal.azure.com) e inicia sesión con tus credenciales de Azure, si se te solicita.

1. Usa el botón **[\>_]** situado a la derecha de la barra de búsqueda en la parte superior de la página para crear una nueva instancia de Cloud Shell en Azure Portal, para lo que deberás seleccionar un entorno de ***Bash***. Cloud Shell proporciona una interfaz de la línea de comandos en un panel situado en la parte inferior de Azure Portal.

    > **Nota**: si has creado anteriormente una instancia de Cloud Shell que usa un entorno de *PowerShell*, cámbiala a ***Bash***.

1. En la barra de herramientas de Cloud Shell, en el menú **Configuración**, selecciona **Ir a la versión clásica** (esto es necesario para usar el editor de código).

1. Cree un grupo de recursos para los recursos necesarios para este ejercicio. Sustituye **\<myResourceGroup>** por un nombre que quieras usar para el grupo de recursos. Puedes reemplazar **useast** por una región cercana si es necesario. 

    ```
    az group create --location useast --name <myResourceGroup>
    ```

1. Ejecuta el siguiente comando para crear la cuenta de Azure Cosmos DB. Reemplace **\<myCosmosDBacct>** por un nombre *único* para identificar su cuenta de Azure Cosmos DB. Reemplaza **\<myResourceGroup>** por el nombre que hayas elegido anteriormente.

    >**Nota:** el nombre de cuenta solo puede contener letras minúsculas, números y el carácter de guion (-). Debe tener una longitud de entre 3 y 31 caracteres. *Este comando tarda unos minutos en completarse.*

    ```
    az cosmosdb create -n <myCosmosDBacct> -g <myResourceGroup>
    ```

1.  Ejecuta el siguiente comando para recuperar el **documentEndpoint** para la cuenta de Azure Cosmos DB. Registra el punto de conexión a partir de los resultados del comando; es necesario más adelante en el ejercicio. Reemplaza **\<myCosmosDBacct>** y **\<myResourceGroup>** por el nombre que creaste.

    ```bash
    az cosmosdb show -n <myCosmosDBacct> -g <myResourceGroup> --query "documentEndpoint" --output tsv
    ```

1. Recupera la clave principal de la cuenta mediante el comando siguiente. Registra el valor **primaryMasterKey** de los resultados del comando para usarlo en el código. Reemplaza **\<myCosmosDBacct>** y **\<myResourceGroup>** por el nombre que creaste.

     ```
    # Retrieve the primary key
    az cosmosdb keys list -n <myCosmosDBacct> -g <myResourceGroup>
    ```

## Creación de la aplicación de consola

Ahora que los recursos necesarios están implementados en Azure, el siguiente paso consiste en configurar la aplicación de consola con la misma ventana de terminal de Visual Studio Code.

1. Cree una carpeta para el proyecto y vaya a la carpeta.

    ```bash
    mkdir cosmosdb
    cd cosmosdb
    ```

1. Cree la aplicación de consola de .NET.

    ```dotnetcli
    dotnet new console --framework net8.0
    ```

1. Ejecuta los siguientes comandos para agregar el paquete **Microsoft.Azure.Cosmos** al proyecto y también el paquete **Newtonsoft.Json** compatible.

    ```dotnetcli
    dotnet add package Microsoft.Azure.Cosmos --version 3.*
    dotnet add package Newtonsoft.Json --version 13.*
    ```

Ahora es el momento de reemplazar el código de plantilla en el archivo **Program.cs** mediante el editor de Cloud Shell.

### Adición del código inicial para el proyecto

1. Ejecuta el siguiente comando en Cloud Shell para empezar a editar la aplicación.

    ```bash
    code Program.cs
    ```

1. Reemplaza cualquier código existente por el siguiente fragmento de código después de las instrucciones **using**. Asegúrese de reemplazar los valores de marcadores de posición por **\<documentEndpoint>** y **\<primaryKey>** siguiendo las instrucciones en los comentarios del código.

    El código proporciona la estructura general de la aplicación y algunos elementos necesarios. Revisa los comentarios del código y agrega código en las áreas especificadas en las instrucciones. 

    ```csharp
    using Microsoft.Azure.Cosmos;
    
    namespace CosmosExercise
    {
        // This class represents a product in the Cosmos DB container
        public class Product
        {
            public string? id { get; set; }
            public string? name { get; set; }
            public string? description { get; set; }
        }
    
        public class Program
        {
            // Cosmos DB account URL - replace with your actual Cosmos DB account URL
            static string cosmosDbAccountUrl = "<documentEndpoint>";
    
            // Cosmos DB account key - replace with your actual Cosmos DB account key
            static string accountKey = "<primaryKey>";
    
            // Name of the database to create or use
            static string databaseName = "myDatabase";
    
            // Name of the container to create or use
            static string containerName = "myContainer";
    
            public static async Task Main(string[] args)
            {
                // Create the Cosmos DB client using the account URL and key
    
    
                try
                {
                    // Create a database if it doesn't already exist
    
    
                    // Create a container with a specified partition key
    
    
                    // Define a typed item (Product) to add to the container
    
    
                    // Add the item to the container
                    // The partition key ensures the item is stored in the correct partition
    
    
                }
                catch (CosmosException ex)
                {
                    // Handle Cosmos DB-specific exceptions
                    // Log the status code and error message for debugging
                    Console.WriteLine($"Cosmos DB Error: {ex.StatusCode} - {ex.Message}");
                }
                catch (Exception ex)
                {
                    // Handle general exceptions
                    // Log the error message for debugging
                    Console.WriteLine($"Error: {ex.Message}");
                }
            }
        }
    }
    ```

### Adición de código para crear el cliente y realizar operaciones 

En esta sección del ejercicio agregas código en áreas especificadas de los proyectos para crear: cliente, base de datos, contenedor y agregas un elemento de ejemplo al contenedor.

1. Agrega el código siguiente en el espacio después del comentario **// Create the Cosmos DB client using the account URL and key**. Este código define el cliente que se usa para conectarse a la cuenta de Azure Cosmos DB.

    ```csharp
    CosmosClient client = new(
        accountEndpoint: cosmosDbAccountUrl,
        authKeyOrResourceToken: accountKey
    );
    ```

    >Nota: el procedimiento recomendado es usar **DefaultAzureCredential** de la biblioteca de *identidad de Azure*. Esto puede implicar algunos requisitos de configuración adicional en Azure en función de cómo esté configurada la suscripción. 

1. Agrega el código siguiente en el espacio después del comentario **// Create a database if it doesn't already exist**. 

    ```csharp
    Microsoft.Azure.Cosmos.Database database = await client.CreateDatabaseIfNotExistsAsync(databaseName);
    Console.WriteLine($"Created or retrieved database: {database.Id}");
    ```

1. Agrega el código siguiente en el espacio después del comentario **// Create a container with a specified partition key**. 

    ```csharp
    Microsoft.Azure.Cosmos.Database database = await client.CreateDatabaseIfNotExistsAsync(databaseName);
    Console.WriteLine($"Created or retrieved database: {database.Id}");
    ```

1. Agrega el código siguiente en el espacio después del comentario **// Define a typed item (Product) to add to the container**. Esto define el elemento que se agrega al contenedor.

    ```csharp
    Product newItem = new Product
    {
        id = Guid.NewGuid().ToString(), // Generate a unique ID for the product
        name = "Sample Item",
        description = "This is a sample item in my Azure Cosmos DB exercise."
    };
    ```

1. Agrega el código siguiente en el espacio después del comentario **// Add the item to the container**. 

    ```csharp
    ItemResponse<Product> createResponse = await container.CreateItemAsync(
        item: newItem,
        partitionKey: new PartitionKey(newItem.id)
    );
    ```

1. Ahora que el código está completo, guarda el progreso, usa **ctrl + q** para guardar el archivo y **ctrl + q** para salir del editor.

1. Ejecuta el comando siguiente en Cloud Shell para probar si hay errores en el proyecto. Si ves errores, abre el archivo *Program.cs* en el editor y comprueba si falta código o errores de pegado.

    ```bash
    dotnet build
    ```

Ahora que el proyecto ha finalizado, es el momento de ejecutar la aplicación y comprobar los resultados en Azure Portal.

## Ejecución de la aplicación y comprobación de los resultados

1. Si estás en Cloud Shell, ejecuta el comando `dotnet run`. La salida debe reflejar algo parecido al siguiente ejemplo.

    ```
    Created or retrieved database: myDatabase
    Created or retrieved container: myContainer
    Created item: c549c3fa-054d-40db-a42b-c05deabbc4a6
    Request charge: 6.29 RUs
    ```

1. En Azure Portal, ve hasta el recurso de Azure Cosmos DB que has creado antes. En el panel de navegación izquierdo, selecciona **Explorador de datos**. En el **Explorador de datos**, selecciona **myDatabase** y expande **myContainer**. Para ver el elemento que creaste, selecciona **Elementos**.

    ![Captura de pantalla que muestra la ubicación de los elementos en el Explorador de datos.](./media/09/cosmos-data-explorer.png)

## Limpieza de recursos

Ahora que has terminado el ejercicio, debes eliminar los recursos en la nube que has creado para evitar el uso innecesario de recursos.

1. Ve al grupo de recursos que creaste y visualiza el contenido de los recursos usados en este ejercicio.
1. Selecciona **Eliminar grupo de recursos** en la barra de herramientas.
1. Escribe el nombre del grupo de recursos y confirma que deseas eliminarlo.
