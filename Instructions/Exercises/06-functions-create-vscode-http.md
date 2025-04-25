---
lab:
  title: Creación de una función de Azure en Visual Studio Code
  description: 'Aprende a crear una función de Azure con un desencadenador HTTP. Después de crear y probar el código localmente en Visual Studio Code, implementarás la función en Azure.'
---

# Creación de una función de Azure en Visual Studio Code

En este ejercicio, aprenderás a crear una función de C\# que responda a solicitudes HTTP. Después de crear y probar el código localmente en Visual Studio Code, implementarás y probarás la función en Azure.

Tareas realizadas en este ejercicio:

* Creación de un recurso de Azure App Service para una aplicación contenedorizada
* Visualización de los resultados
* Limpieza de recursos

Este ejercicio debería tardar en completarse **30** minutos aproximadamente.

## Antes de comenzar

Antes de empezar asegúrese de que tiene los siguientes requisitos implementados:

* Suscripción a Azure. Si aún no tiene uno, puede [registrarse para obtenerlo](https://azure.microsoft.com/).

* [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools#installing), versión 4.x.

* [Visual Studio Code](https://code.visualstudio.com/) en una de las [plataformas admitidas](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

* [.NET 8](https://dotnet.microsoft.com/en-us/download/dotnet/8.0) es la plataforma de destino.

* [Kit de desarrollo de C#](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csdevkit) para Visual Studio Code.

* [Extensión de Azure Functions](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) para Visual Studio Code.

## Creación del proyecto local

En esta sección se usa Visual Studio Code para crear un proyecto local de Azure Functions en C#. Más adelante en este ejercicio, publicará el código de la función en Azure.

1. En Visual Studio Code, presione F1 para abrir la paleta de comandos y busque y ejecute el comando `Azure Functions: Create New Project...`.

1. Seleccione una ubicación de directorio para el área de trabajo del proyecto y, después, seleccione el botón **Seleccionar**. Debe crear una nueva carpeta o elegir una carpeta vacía en la que ubicar el área de trabajo del proyecto. No elija una carpeta de proyecto que ya forme parte de un área de trabajo.

1. Escriba la siguiente información cuando se le indique:

    | Prompt | Action |
    |--|--|
    | Seleccionar la carpeta que contendrá los proyectos de función | Selecciona **Examinar...** para seleccionar una carpeta para tu aplicación.
    | Selección de un idioma | seleccione **C#**. |
    | Seleccione un entorno de ejecución .NET. | Selecciona **.NET 8.0. aislado**. |
    | Seleccionar una plantilla para la primera función de su proyecto | Selecciona **Desencadenador HTTP**.<sup>1</sup> |
    | Proporcionar un nombre de función | Escriba `HttpExample`. |
    | Proporcionar un espacio de nombres | Escriba `My.Function`. |
    | Nivel de autorización | Selecciona **Anónimo**, que permite que cualquiera llame al punto de conexión de la función. |

    <sup>1</sup> En función de la configuración de VS Code, es posible que tengas que usar la opción**Cambiar filtro de plantilla** para ver la lista completa de plantillas.

1. Visual Studio Code generará un proyecto de Azure Functions con un desencadenador HTTP a partir de la información que se proporciona. Los archivos del proyecto locales se pueden ver en Explorer.

### Ejecución local de la función

Visual Studio Code se integra con Azure Functions Core Tools para que pueda ejecutar este proyecto en un equipo de desarrollo local antes de publicarlo en Azure.

1. Asegúrese de que el terminal esté abierto en Visual Studio Code. Para abrir el terminal, seleccione **Terminal** y, a continuación, **Nuevo terminal** en la barra de menús. 

1. Presione **F5** para iniciar el proyecto de aplicación de funciones en el depurador. La salida de Core Tools aparece en el panel **Terminal**. La aplicación se inicia en el panel **Terminal**. Puede ver el punto de conexión de la dirección URL de la función desencadenada por HTTP que se ejecuta localmente.

    ![La captura de pantalla del punto de conexión de la función desencadenada por HTTP se muestra en el panel Terminal.](./media/06/run-function-local.png)

1. Mientras se ejecuta Core Tools, vaya al área de **Azure: Área Functions**. En **Functions**, expanda **Proyecto local** > **Functions**. Haz clic con el botón derecho en la función **HttpExample** y selecciona **Ejecutar función ahora...**.

    ![Captura de pantalla que muestra la ubicación del paso Ejecutar función ahora...](./media/06/execute-function-local.png)

1. En **Especificar cuerpo de la solicitud**, escribe el valor de `{ "name": "Azure" }` del cuerpo del mensaje de la solicitud. Presione **Entrar** para enviar este mensaje de solicitud a la función. Cuando la función se ejecuta localmente y devuelve una respuesta, se genera una notificación en Visual Studio Code.

    selecciona el icono de campana de notificación para ver la notificación. La información sobre la ejecución de la función se muestra en el panel **Terminal**.

1. Presione **Mayús + F5** para detener Core Tools y desconectar el depurador.

Después de comprobar que la función se ejecuta correctamente en el equipo local, es el momento de usar Visual Studio Code para publicar el proyecto directamente en Azure.

## Implementación y ejecución de la función en Azure

En esta sección, crearás un recurso de Aplicación de funciones de Azure e implementarás la función en el recurso.

### Inicio de sesión en Azure

Para poder publicar la aplicación, debe iniciar sesión en Azure. Si ya ha iniciado sesión, vaya a la sección siguiente.

1. Si todavía no ha iniciado sesión, seleccione el icono de Azure en la barra de actividades y después, en el área **Azure: Functions**, elija **Iniciar sesión en Azure...**.

    ![Captura de pantalla del botón Iniciar sesión de Azure.](./media/06/functions-sign-into-azure.png)

1. Cuando se le solicite en el explorador, elija su cuenta de Azure e inicie sesión con las credenciales de la misma.

1. Después de iniciar sesión correctamente, puede cerrar la nueva ventana del explorador. Las suscripciones que pertenecen a su cuenta de Azure se muestran a la barra lateral.

### Creación de recursos en Azure

En esta sección, creará los recursos de Azure que necesita para implementar la aplicación de funciones local.

1. Elija el icono de Azure en la barra de actividades y, después, en el área **Recursos**, seleccione el botón **Crear recurso…**

    ![Captura de pantalla del botón Crear recursos.](./media/06/create-resource.png)    

1. Escriba la siguiente información cuando se le indique:

    | Prompt | Action |
    |--|--|
    | Seleccione un recurso para crear | Seleccione **Crear aplicación de funciones en Azure…** |
    | Seleccionar suscripción | Seleccione la subscripción que desee utilizar. *No se mostrará esta opción si solo tiene una suscripción.* |
    | Escribir un nombre único global para la aplicación de funciones | Escribe un nombre que sea válido en una ruta de acceso de dirección URL, por ejemplo `myfunctionapp`. El nombre que escribas se valida para asegurarse de que es único. |
    | Seleccionar una ubicación para los nuevos recursos | Para mejorar el rendimiento, seleccione una región cerca de usted. |
    | Seleccione una pila en tiempo de ejecución | Selecciona **.NET 8.0. aislado**. |
    | Seleccionar un tamaño de memoria de instancia | Seleccionar el **valor predeterminado 2048** |
    | Modificar el recuento de instancias máximo | Acepta el valor predeterminado **100**. |

    La extensión muestra el estado de los recursos individuales a medida que se crean en el área **AZURE** de la ventana del terminal.
    
1. Cuando se complete, se crearán los siguientes recursos de Azure en la suscripción con nombres que se basan en el nombre de la aplicación de funciones:

    * Un grupo de recursos, que es un contenedor lógico de recursos relacionados.
    * Una cuenta de Azure Storage estándar, que mantiene el estado e información adicional sobre los proyectos.
    * Un plan de consumo flexible, que define el host subyacente para tu aplicación de funciones sin servidor.
    * Una aplicación de funciones, que proporciona el entorno para ejecutar el código de función. Una aplicación de funciones permite agrupar funciones como una unidad lógica para facilitar la administración, la implementación y el uso compartido de recursos en el mismo plan de hospedaje.
    * Una instancia de Application Insights conectada a la aplicación de funciones, que realiza un seguimiento del uso de la función sin servidor.

### Implementar el proyecto en Azure

> **! Importante:** la publicación en una función existente sobrescribe las implementaciones anteriores.

1. En la paleta de comandos, busca y ejecuta el comando **Azure Functions: Deploy to Function App...**.

1. Selecciona la suscripción que usaste al crear los recursos.

1. Seleccione la aplicación de funciones que ha creado anteriormente. Cuando se le solicite sobrescribir las implementaciones anteriores, seleccione **Implementar** para implementar el código de función en el nuevo recurso de aplicación de funciones.

1. Una vez completada la implementación, selecciona **Ver salida** para ver los detalles de los resultados de la implementación. Si se pierde la notificación, selecciona el icono de campana de notificación en la esquina inferior derecha para verla de nuevo.

    ![Captura de pantalla de la ventana Ver salida.](./media/06/function-view-output.png)

### Ejecución de la función en Azure

1. Vuelva al área **Recursos** en la barra lateral y expanda su suscripción, la nueva aplicación de funciones y **Funciones**. **Haz clic con el botón derecho** en la función **HttpExample** y selecciona **Ejecutar función ahora...**.

    ![Captura de pantalla de la opción Ejecutar función ahora.](./media/06/execute-function-remote.png)

1. En **Enter request body** (Especificar el cuerpo de la solicitud) verá el valor del cuerpo del mensaje de solicitud de `{ "name": "Azure" }`. Presione Entrar para enviar este mensaje de solicitud a la función.

1. Cuando la función se ejecuta en Azure y devuelve una respuesta, se genera una notificación en Visual Studio Code. selecciona el icono de campana de notificación para ver la notificación.

## Limpieza de recursos

Ahora que has terminado el ejercicio, debes eliminar los recursos en la nube que has creado para evitar el uso innecesario de recursos.

1. En el explorador, ve a Azure Portal [https://portal.azure.com](https://portal.azure.com) e inicia sesión con tus credenciales de Azure, si se te solicita.
1. Ve al grupo de recursos que creaste y visualiza el contenido de los recursos usados en este ejercicio.
1. Selecciona **Eliminar grupo de recursos** en la barra de herramientas.
1. Escribe el nombre del grupo de recursos y confirma que deseas eliminarlo.
