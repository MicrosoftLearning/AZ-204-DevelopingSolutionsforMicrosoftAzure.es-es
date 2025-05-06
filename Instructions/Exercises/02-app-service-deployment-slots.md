---
lab:
  title: Cambio de ranuras de implementación en Azure App Service
  description: 'Aprende a cambiar ranuras de implementación en Azure App Service. En este ejercicio, implementarás una aplicación sencilla en App Service; realizarás un pequeño cambio en la aplicación y lo implementarás en un espacio de ensayo; y, por último, cambiarás los espacios para que la aplicación actualizada esté en producción.'
---

# Cambio de ranuras de implementación en Azure App Service

En este ejercicio, implementará un sitio de HTML y CSS básico en Azure App Service mediante el comando **az webapp up** de la CLI de Azure. A continuación, actualiza el código e implementa el cambio en un espacio de ensayo. Por último, cambia las ranuras.

Tareas realizadas en este ejercicio:

* Descarga e implementación de la aplicación de ejemplo en Azure App Service.
* Creación de un espacio de implementación de ensayo.
* Realización de un cambio en la aplicación de ejemplo e implementación en el espacio de ensayo.
* Cambio de las ranuras de producción predeterminadas y de ensayo para mover los cambios al espacio de producción.

Este ejercicio debería tardar aproximadamente **20** minutos en completarse.

## Antes de comenzar

Para completar el ejercicio, necesitarás lo siguiente:

* Suscripción a Azure. Si aún no la tienes, puedes registrarte para obtener una [https://azure.microsoft.com/](https://azure.microsoft.com/).

## Descarga e implementación de la aplicación de ejemplo

En esta sección, descargarás la aplicación de ejemplo y establecerás variables para facilitar la entrada de los comandos y, a continuación, crearás un recurso de Azure App Service e implementarás un sitio HTML estático mediante comandos de la CLI de Azure.

1. En el explorador, ve a Azure Portal [https://portal.azure.com](https://portal.azure.com) e inicia sesión con tus credenciales de Azure, si se te solicita.

1. Usa el botón **[\>_]** situado a la derecha de la barra de búsqueda en la parte superior de la página para crear una nueva instancia de Cloud Shell en Azure Portal, para lo que deberás seleccionar un entorno de ***Bash***. Cloud Shell proporciona una interfaz de la línea de comandos en un panel situado en la parte inferior de Azure Portal.

    > **Nota**: si has creado anteriormente una instancia de Cloud Shell que usa un entorno de *PowerShell*, cámbiala a ***Bash***.

1. En la barra de herramientas de Cloud Shell, en el menú **Configuración**, selecciona **Ir a la versión clásica** (esto es necesario para usar el editor de código).

1. Ejecuta el comando **git** siguiente para clonar el repositorio de la aplicación de ejemplo.

    ```bash
    git clone https://github.com/Azure-Samples/html-docs-hello-world.git
    ```

1. Establezca variables para contener el grupo de recursos y los nombres de la aplicación mediante la ejecución de los comandos siguientes. Anota el valor de **appName** que se muestra después de ejecutar los comandos; lo necesitarás más adelante en este ejercicio.

    ```bash
    resourceGroup=rg-mywebapp

    appName=mywebapp$RANDOM
    echo $appName
    ```

1. Ve al directorio que contiene el código de ejemplo y ejecuta el comando **az webapp up**. **Nota:** este comando puede tardar unos minutos en ejecutarse.

    ```bash
    cd html-docs-hello-world

    az webapp up -g $resourceGroup -n $appName --sku P0V3 --html
    ```

    Ahora que la implementación ha finalizado, es el momento de ver la aplicación web.

1. En Azure Portal, ve a la aplicación web que implementaste. Puedes escribir el nombre que anotaste anteriormente en la barra de búsqueda **Buscar recursos, servicios y documentos (G + /)** y seleccionar el recurso de la lista.

1. Selecciona el vínculo a la aplicación web que se encuentra en el campo **Dominio predeterminado** de la sección **Essentials**. El vínculo abrirá el sitio en una nueva pestaña.

## Implementación de código actualizado en una ranura de implementación

En esta sección, crearás una ranura de implementación, modificarás el código HTML de la aplicación e implementarás el código actualizado en la nueva ranura de implementación.

### Creación de una ranura de implementación 

1. Vuelve a la pestaña con Azure Portal y Cloud Shell.

1. Escribe el comando siguiente en Cloud Shell para crear una ranura de implementación denominada *staging*.

    ```bash
    az webapp deployment slot create -n $appName -g $resourceGroup --slot staging
    ```

1. Espera a que finalice el comando y selecciona **Implementación > Ranuras de implementación** en el menú de la izquierda para ver las ranuras de implementación de la aplicación web. Ten en cuenta que el nombre de la nueva ranura contiene *-staging* anexado al nombre de la aplicación web.

### Actualización de código e implementación en el espacio de ensayo

1. En Cloud Shell, escribe `code index.html` para abrir el editor. En la etiqueta de encabezado `<h1>`, cambia *Azure App Service - Sample Static HTML Site* por *Azure App Service Staging Slot* o por cualquier otra cosa que quieras.

1. Usa los comandos **ctrl-s** para guardar y **ctrl-q** para salir.

1. En Cloud Shell, ejecuta el siguiente comando para crear un archivo ZIP del proyecto actualizado. Se necesita un archivo ZIP o un recurso de aplicación web (WAR) para el paso siguiente.

    ```bash
    zip -r stagingcode.zip .
    ```

1. Ejecuta el comando siguiente en Cloud Shell para implementar las actualizaciones en el espacio de ensayo.

    ```bash
    az webapp deploy -g $resourceGroup -n $appName --src-path ./stagingcode.zip --slot staging
    ```

1. Selecciona **Implementación > Ranuras de implementación** en el menú izquierdo de la aplicación web y, a continuación, seleccionael espacio de ensayo que creaste anteriormente.

1. Selecciona el vínculo en el campo **Dominio predeterminado** de la sección **Essentials**. El vínculo abrirá el sitio web para el espacio de ensayo en una nueva pestaña.

## Cambio de los espacios de ensayo y producción

Puedes realizar un cambio en Azure Portal con la opción **Cambiar** de la barra de herramientas. La opción **Cambiar** aparecerá en la barra de herramientas si seleccionas **Información general** o **Implementación > Ranuras de implementació** en el menú de la izquierda.

1. En Azure Portal, selecciona **Cambiar** en la barra de herramientas para abrir el panel **Cambiar**.

1. Revisa la configuración en el panel de cambio. El **Origen** debe mostrar la ranura **-staging** y el **Destino** debe mostrar el espacio de producción predeterminado.

    ![Captura de pantalla del panel de cambio.](./media/02/app-service-swap-panel.png)

1. Selecciona **Iniciar cambio** y espera a que finalice la operación. Puedes realizar un seguimiento de la finalización en el panel **Notificaciones** que puedes abrir con la selección del icono de campana en la parte superior del portal.

1. Para comprobar el cambio, ve a la aplicación web que implementaste. Escribe el nombre de la aplicación web que creaste anteriormente (por ejemplo, *mywebapp12360*) en la barra de búsqueda **Buscar recursos, servicios y documentos (G + /)** y, a continuación, selecciona el recurso de la lista.

1. Selecciona el vínculo a la aplicación web que se encuentra en el campo **Dominio predeterminado** de la sección **Essentials**. El vínculo abrirá el sitio (espacio de producción) en una nueva pestaña.

1. Comprueba los cambios, es posible que tengas que actualizar la página para que aparezcan.

## Limpieza de recursos

Ahora que has terminado el ejercicio, debes eliminar los recursos en la nube que has creado para evitar el uso innecesario de recursos.

1. Ve al grupo de recursos que creaste y visualiza el contenido de los recursos usados en este ejercicio.
1. Selecciona **Eliminar grupo de recursos** en la barra de herramientas.
1. Escribe el nombre del grupo de recursos y confirma que deseas eliminarlo.
