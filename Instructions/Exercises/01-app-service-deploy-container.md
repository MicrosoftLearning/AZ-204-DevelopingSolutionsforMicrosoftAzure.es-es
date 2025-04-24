---
lab:
  title: "Implementación de una aplicación contenedorizada en Azure\_App Service"
  description: Aprende a implementar una aplicación contenedorizada en Azure App Service.
---

# Implementación de una aplicación contenedorizada en Azure App Service

En este ejercicio, implementarás una aplicación contenedorizada sencilla en Azure App Service. 

Tareas realizadas en este ejercicio:

* Creación de un recurso de Azure App Service para una aplicación contenedorizada
* Visualización de los resultados
* Limpieza de recursos

Este ejercicio debería tardar aproximadamente **15** minutos en completarse.

## Antes de comenzar

Para completar el ejercicio, necesitarás lo siguiente:

* Suscripción a Azure. Si aún no tiene uno, puede [registrarse para obtenerlo](https://azure.microsoft.com/).

## Crea un recurso de la aplicación web.

1. En el explorador, ve a Azure Portal [https://portal.azure.com](https://portal.azure.com) e inicia sesión con tus credenciales de Azure, si se te solicita.
1. Selecciona **+ Crear un recurso** ubicado en el encabezado **Servicios de Azure** situado cerca de la parte superior de la página principal. 
1. En la barra de búsqueda **Buscar en Marketplace**, escribe *aplicación web* y presiona **Entrar** para iniciar la búsqueda.
1. En el icono Aplicación web, selecciona la lista desplegable **Crear** y, a continuación, selecciona **Aplicación web**.

    ![Captura de pantalla del icono de la aplicación web.](./media/01/create-web-app-tile.png)

Al seleccionar **Crear**, se abrirá una plantilla con algunas pestañas para rellenar la información sobre la implementación. Los pasos siguientes te guiarán por los cambios que se deben realizar en las pestañas pertinentes.

1. Completa la pestaña **Datos básicos** con la información de la tabla siguiente:

    | Configuración | Acción |
    |--|--|
    | **Suscripción** | Conserve los valores predeterminados. |
    | **Grupo de recursos** | Selecciona Crear nuevo, escribe `rg-WebApp` y, a continuación, selecciona Aceptar. |
    | **Nombre** | Especifica un nombre único, por ejemplo, `<your-initials>-containerwebapp`. Reemplaza *\<your-initials>* por tus iniciales o por algún otro valor. El nombre debe ser único, por lo que puede requerir algunos cambios. |
    | Control deslizante en la configuración de **Nombre** | Selecciona el control deslizante para desactivarlo. |
    | **Publicar** | Selecciona la opción **Contenedor**. |
    | **Sistema operativo** | Asegúrate de que **Linux** está seleccionado. |
    | **Región** | Conserva la selección predeterminada o elige una región cercana. |
    | **Plan de Linux** | Conserva la selección predeterminada. |
    | **Plan de precios** | Selecciona la lista desplegable y elige el plan **F1 gratis**. |

1. Selecciona o ve a la pestaña **Contenedor** y escribe la información en la tabla siguiente:

    | Configuración | Acción |
    |--|--|
    | **Compatibilidad con Sidecar** | Conserva la posición desactivada predeterminada. |
    | **Origen de la imagen** | Selecciona **Otros registros de contenedor**. |
    | **Tipo de acceso** | Conserva la selección **Pública** predeterminada. |
    | **Dirección URL del servidor de registro** | Escriba `mcr.microsoft.com/k8se`. |
    | **Imagen y etiqueta** | Escriba `quickstart:latest`. |
    | **Comando de inicio** | Deje  en blanco. |

1. Seleccione la pestaña **Revisar y crear**.
1. Revisa las selecciones realizadas y, a continuación, selecciona el botón **Crear**.

La implementación puede tardar unos minutos en completarse. Cuando hayas finalizado, selecciona el botón **Ir al recurso**.

Ahora que la implementación ha finalizado, es el momento de ver la aplicación web. Selecciona el vínculo a la aplicación web que se encuentra junto al campo **Dominio predeterminado** de la sección **Essentials**. El vínculo abrirá el sitio en una nueva pestaña.

>**Nota:** la aplicación de contenedor implementada puede tardar unos minutos en ejecutarse y mostrarse en la nueva pestaña.

## Limpieza de recursos

Ahora que has terminado el ejercicio, debes eliminar los recursos en la nube que has creado para evitar el uso innecesario de recursos.

1. Ve al grupo de recursos que creaste y visualiza el contenido de los recursos usados en este ejercicio.
1. Selecciona **Eliminar grupo de recursos** en la barra de herramientas.
1. Escribe el nombre del grupo de recursos y confirma que deseas eliminarlo.
