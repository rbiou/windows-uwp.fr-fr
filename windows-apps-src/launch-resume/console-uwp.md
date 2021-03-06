---
title: Créer une application de console de plateforme Windows universelle
description: Cette rubrique décrit comment écrire une application UWP qui s’exécute dans une fenêtre de console.
keywords: console uwp
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c2dba15d78301c84f4064bcd6548d44e3c17beb2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366354"
---
# <a name="create-a-universal-windows-platform-console-app"></a>Créer une application de console de plateforme Windows universelle

Cette rubrique décrit comment créer un [C++ / c++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) ou C / c++ / CX Universal Windows Platform (UWP) console application.

À partir de Windows 10, version 1803, vous pouvez écrire C + c++ / WinRT ou C++ / c++ / CX UWP les applications de console qui s’exécutent dans une fenêtre de console, telle qu’une fenêtre de console DOS ou PowerShell. Applications de console utiliser la fenêtre de console pour l’entrée et sortie et pouvez utiliser [Runtime C universel](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference) fonctions telles que **printf** et **getchar**. Les applications de console UWP peuvent être publiées dans le Microsoft Store. Elles ont une entrée dans la liste des applications et une vignette principale qui peut être épinglée au menu Démarrer. Les applications de console UWP peuvent être lancées depuis le menu Démarrer, bien que vous allez généralement les lancer à partir de la ligne de commande.

Pour visualiser un en action, voici une vidéo sur la création d’une application de Console UWP.

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>Utiliser un modèle d’application de console UWP 

Pour créer une application de console UWP, commencez par installer les **modèles de projets (universels) d’application de console**, disponibles dans le [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal). Les modèles installés présents sont alors disponibles sous **nouveau projet** > **installé** > **autres langages**  >  **Visual C++**  > **Windows universel** comme **Console application C++ / c++ / WinRT (Windows universel)** et **Console application C++ / c++ / CX (Universal Windows )** .

## <a name="add-your-code-to-main"></a>Ajouter votre code à main()

Les modèles ajoutent **Program.cpp**, qui contient la fonction `main()`. Voilà où commence l’exécution dans une application de console UWP. Accédez aux arguments de ligne de commande avec les paramètres `__argc` et `__argv`. L’application de console UWP se ferme lorsque le contrôle revient de `main()`.

L’exemple suivant de **Program.cpp** est ajouté par le **c++ d’application Console c++ / WinRT** modèle :

```cppwinrt
#include "pch.h"

using namespace winrt;

// This example code shows how you could implement the required main function
// for a Console UWP Application. You can replace all the code inside main
// with your own custom code.

int __cdecl main()
{
    // You can get parsed command-line arguments from the CRT globals.
    wprintf(L"Parsed command-line arguments:\n");
    for (int i = 0; i < __argc; i++)
    {
        wprintf(L"__argv[%d] = %S\n", i, __argv[i]);
    }

    // Keep the console window alive in case you want to see console output when running from within Visual Studio
      wprintf(L"Press 'Enter' to continue: ");
    getchar();
}
```

## <a name="uwp-console-app-behavior"></a>Comportement d'une application de console UWP

Une application de console UWP peut accéder au système de fichiers à partir du répertoire depuis lequel elle est exécutée, et à un niveau inférieur. Cela est possible, car le modèle ajoute l'extension [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) au fichier Package.appxmanifest de votre application. Cette extension permet également à l’utilisateur de saisir l’alias à partir d’une fenêtre de console pour lancer l’application. L’application n’a pas besoin de se trouver dans le chemin d’accès système pour être lancée.

En outre, vous pouvez accorder un accès étendu au système de fichiers à votre application de console UWP en ajoutant la fonctionnalité restreinte `broadFileSystemAccess` comme décrit dans [Autorisations d’accès aux fichiers](https://docs.microsoft.com/windows/uwp/files/file-access-permissions). Cette fonctionnalité fonctionne avec les API se trouvant dans l'espace de noms [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage).

Plusieurs instances d’une application de console UWP peuvent s’exécuter simultanément, car le modèle ajoute la fonctionnalité [SupportsMultipleInstances](multi-instance-uwp.md) au fichier Package.appxmanifest de votre application.

Le modèle ajoute également la fonctionnalité `Subsystem="console"` au fichier Package.appxmanifest, ce qui indique que cette application UWP est une application de console. Notez les préfixes `desktop4` et iot2 `namespace`. Les applications de console UWP sont uniquement prises en charge sur le bureau et les projets IoT.

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4" 
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2" 
  IgnorableNamespaces="uap mp uap5 desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:Subsystem="console" 
      desktop4:SupportsMultipleInstances="true" 
      iot2:Subsystem="console" 
      iot2:SupportsMultipleInstances="true"  >
      ...
      <Extensions>
          <uap5:Extension 
            Category="windows.appExecutionAlias" 
            Executable="YourApp.exe" 
            EntryPoint="YourApp.App">
            <uap5:AppExecutionAlias desktop4:Subsystem="console">
              <uap5:ExecutionAlias Alias="YourApp.exe" />
            </uap5:AppExecutionAlias>
          </uap5:Extension>
      </Extensions>
    </Application>
  </Applications>
    ...
</Package>
```

## <a name="additional-considerations-for-uwp-console-apps"></a>Autres considérations sur les applications de console UWP

- Uniquement C + c++ / WinRT et C / c++ / CX UWP apps peuvent être des applications console.
- Les applications de console UWP doivent cibler le bureau ou un type de projet IoT.
- Les applications de console UWP ne peuvent pas créer une fenêtre. Ils ne peuvent pas utiliser MessageBox(), ou Location() ou toute autre API qui peut créer une fenêtre pour une raison quelconque, telles que les invites de consentement de l’utilisateur.
- Les applications de console UWP ne peuvent ni utiliser des tâches en arrière‑plan, ni servir de tâche en arrière‑plan.
- À l’exception de l'[activation par ligne de commande](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97), les applications de console UWP ne prennent pas en charge les contrats d’activation, notamment l’association de fichiers, l'association de protocoles, etc.
- Même si les applications de console UWP prennent en charge les instances multiples, elles ne prennent pas en charge la [redirection des instances multiples](multi-instance-uwp.md)
- Pour obtenir la liste des API Win32 à la disposition des applications UWP, voir [API Win32 et COM pour les applications UWP](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps)