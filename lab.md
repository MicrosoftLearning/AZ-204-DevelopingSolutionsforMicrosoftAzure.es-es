# <a name="lab-virtual-machine-setup"></a>Configuración de la máquina virtual de laboratorio

## <a name="installed-software"></a>Software instalado

| Software | Vínculo |
| --- | --- |
| Windows 10 (compilación 2004) | <https://www.microsoft.com/software-download/windows10> |
| Visual Studio Code | <https://code.visualstudio.com> |
| Extensión de la cuenta de Azure de Visual Studio Code | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account> |
| Extensión de la instancia de Azure Functions de Visual Studio Code | <https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions> |
| Extensión de las herramientas de Azure Resource Manager de Visual Studio Code | <https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools> |
| Extensión de las herramientas de la CLI de Azure de Visual Studio Code | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli> |
| Extensión de PowerShell de Visual Studio Code | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell> |
| Extensión de C# de Visual Studio Code | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp> |
| PowerShell 7 | <https://github.com/PowerShell/PowerShell/releases/tag/v7.0.3> |
| .NET 6 SDK | <https://dotnet.microsoft.com/download/dotnet/6.0> |
| Azure PowerShell | <https://docs.microsoft.com/powershell/azure/install-az-ps> |
| Azure CLI | <https://docs.microsoft.com/cli/azure/install-azure-cli> |
| Explorador de Azure Storage | <https://azure.microsoft.com/features/storage-explorer> |
| .NET Tool - HttpRepl | <https://github.com/dotnet/HttpRepl> |
| Azure Functions Core Tools | <https://docs.microsoft.com/azure/azure-functions/functions-run-local#v3> |
| Terminal Windows | <https://aka.ms/terminal> |
| Edge (Chromium) | <https://www.microsoft.com/edge> |

## <a name="additional-configuration"></a>Configuración adicional

- Habilitar ClearType
  
- Configuración de Microsoft Edge como explorador predeterminado

- Actualización de la configuración de VSCode

  ```json
  {
    "editor.fontFamily": "'Cascadia Code', Consolas, 'Courier New', monospace",
    "update.enableWindowsBackgroundUpdates": false,
    "update.mode": "manual",
    "terminal.integrated.shell.windows": "C:\\Program Files\\PowerShell\\7\\pwsh.exe",
    "workbench.startupEditor": "none",
    "terminal.integrated.rendererType": "dom",
    "csharp.suppressDotnetInstallWarning": true,
    "csharp.suppressDotnetRestoreNotification": true,
    "csharp.supressBuildAssetsNotification": true,
    "azureFunctions.showProjectWarning": false
  }
  ```

- Actualización de la configuración de Terminal Windows

  ```json
  {
    "$schema": "https://aka.ms/terminal-profiles-schema",
    "defaultProfile": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
    "profiles": [
      {
        "guid": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
        "useAcrylic": true,
        "acrylicOpacity": 0.85,
        "colorScheme": "Campbell",
        "fontFace": "Cascadia Code",
        "hidden": false,
        "name": "PowerShell",
        "source": "Windows.Terminal.PowershellCore"
      },
      {
        "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
        "hidden": false,
        "name": "Azure Cloud Shell",
        "source": "Windows.Terminal.Azure"
      }
    ],
    "schemes": [],
    "keybindings": []
  }
  ```

- Configure el menú Inicio y la barra de tareas para incluir solo los iconos siguientes:
  - Explorador de archivos
  - Edge
  - Terminal Windows
  - Visual Studio Code
  - Explorador de Azure Storage

- Deshabilitación de las notificaciones de actualización de PowerShell 7

  1. [Cree una variable de entorno](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_update_notifications?view=powershell-7) denominada ``POWERSHELL_UPDATECHECK``.
  
  1. Establezca el valor de la variable de entorno en ``Off`` (distingue mayúsculas de minúsculas).

- Ejecute Azure Functions Core Tools al menos una vez para configurar el firewall dew Windows.

  ```bash
  func init test --worker-runtime dotnet
  cd test
  func new --template 'HTTP trigger' --name web
  func start --build
  ```
