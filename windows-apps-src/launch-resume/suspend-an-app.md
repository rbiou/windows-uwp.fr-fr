---
title: Gérer la suspension d’une application
description: Découvrez comment enregistrer d’importantes données d’application lorsque le système suspend l’exécution de votre application.
ms.assetid: F84F1512-24B9-45EC-BF23-A09E0AC985B0
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: f912e6212346a4019d8421c542a81eb2318dc5d9
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260408"
---
# <a name="handle-app-suspend"></a>Gérer la suspension d’une application

**API importantes**

- [**Suspension**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.suspending)

Découvrez comment enregistrer d’importantes données d’application lorsque le système suspend l’exécution de votre application. L’exemple inscrit un gestionnaire pour l’événement [**Suspending**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.suspending) et enregistre une chaîne dans un fichier.

## <a name="register-the-suspending-event-handler"></a>Enregistrer le gestionnaire d’événements de suspension

Enregistrez-vous pour gérer l’événement [**Suspending**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.suspending), qui indique que votre application doit enregistrer ses données d’application avant que le système la suspende.

```csharp
using System;
using Windows.ApplicationModel;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

partial class MainPage
{
   public MainPage()
   {
      InitializeComponent();
      Application.Current.Suspending += new SuspendingEventHandler(App_Suspending);
   }
}
```

```vb
Public NotInheritable Class MainPage

   Public Sub New()
      InitializeComponent()
      AddHandler Application.Current.Suspending, AddressOf App_Suspending
   End Sub
   
End Class
```

```cppwinrt
MainPage::MainPage()
{
    InitializeComponent();
    Windows::UI::Xaml::Application::Current().Suspending({ this, &MainPage::App_Suspending });
}
```

```cpp
using namespace Windows::ApplicationModel;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;

MainPage::MainPage()
{
   InitializeComponent();
   Application::Current->Suspending +=
       ref new SuspendingEventHandler(this, &MainPage::App_Suspending);
}
```

## <a name="save-application-data-before-suspension"></a>Enregistrer des données d’application avant sa suspension

Lorsque votre application traite l’événement [**Suspending**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.suspending), elle a la possibilité d’enregistrer ses données d’application importantes dans la fonction du gestionnaire. L’application doit utiliser l’API de stockage [**LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) pour enregistrer les données d’application simples de manière synchrone.

```csharp
partial class MainPage
{
    async void App_Suspending(
        Object sender,
        Windows.ApplicationModel.SuspendingEventArgs e)
    {
        // TODO: This is the time to save app data in case the process is terminated.
    }
}
```

```vb
Public NonInheritable Class MainPage

    Private Sub App_Suspending(
        sender As Object,
        e As Windows.ApplicationModel.SuspendingEventArgs) Handles OnSuspendEvent.Suspending

        ' TODO: This is the time to save app data in case the process is terminated.
    End Sub

End Class
```

```cppwinrt
void MainPage::App_Suspending(
    Windows::Foundation::IInspectable const& /* sender */,
    Windows::ApplicationModel::SuspendingEventArgs const& /* e */)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

```cpp
void MainPage::App_Suspending(Object^ sender, SuspendingEventArgs^ e)
{
    // TODO: This is the time to save app data in case the process is terminated.
}
```

## <a name="release-resources"></a>Libérer des ressources

Vous devez également libérer les ressources exclusives et les descripteurs de fichiers pour permettre aux autres applications d’y accéder lorsque votre application est suspendue. Appareils photo, périphériques d’E/S, appareils externes et ressources réseau sont autant d’exemples de ressources exclusives. En libérant explicitement les ressources exclusives et les descripteurs de fichiers, vous permettez aux autres applications d’y accéder lorsque votre application est suspendue. Lorsqu’elle est réactivée, l’application doit se réapproprier ses ressources exclusives et descripteurs de fichiers.

## <a name="remarks"></a>Notes

Le système suspend votre application chaque fois que l’utilisateur passe à une autre application, au Bureau ou à l’écran d’accueil. Le système en reprend l’exécution lorsque l’utilisateur revient à votre application. Dès lors, le contenu de vos variables et structures de données restent identiques à ce qu’elles étaient avant que le système ne suspende l’application. Le système rétablit l’application exactement dans l’état où il l’a laissée, de sorte qu’elle semble s’être exécutée en arrière-plan.

Le système tente de conserver votre application et ses données en mémoire pendant sa suspension. Toutefois, si le système ne dispose pas des ressources pour conserver votre application en mémoire, il met fin à votre application. Lorsque l’utilisateur rebascule vers une application suspendue qui a été arrêtée, le système envoie un événement [**Activated**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) et doit restaurer ses données d’application dans sa méthode [**OnLaunched**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onlaunched).

Le système ne vous notifie pas de l’arrêt d’une application. Celle-ci doit donc enregistrer ses données d’application et libérer les ressources exclusives et descripteurs de fichiers au moment où elle est mise en suspens pour ensuite les restaurer lorsque l’application est activée après avoir été arrêtée.

Si vous effectuez un appel asynchrone dans votre gestionnaire, le contrôle revient immédiatement de cet appel. Cela signifie que l’exécution peut ensuite revenir de votre gestionnaire d’événements et votre application prend l’état suivant, même si l’appel asynchrone n’est pas encore terminé. Utilisez la méthode [**GetDeferral**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel?redirectedfrom=MSDN) sur l’objet [**EnteredBackgroundEventArgs**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel?redirectedfrom=MSDN) qui est transmis à votre gestionnaire d’événements pour retarder la suspension jusqu'à ce que vous appeliez la méthode [**Complete**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.complete) sur l’objet [**Windows.Foundation.Deferral**](https://docs.microsoft.com/uwp/api/windows.foundation.deferral) renvoyé.

Un report n’augmente pas le temps d’exécution nécessaire de votre code avant l’arrêt de votre application. Cela ne retarde l’arrêt que jusqu’à ce que la méthode *Complete* soit appelée ou que la date d’échéance ne soit passée, *la première de ces deux éventualités prévalant*. Pour étendre la durée de l’état Interruption en cours, utilisez [**ExtendedExecutionSession**](run-minimized-with-extended-execution.md).

> [!NOTE]
> Pour améliorer la réactivité du système dans Windows 8.1, les applications reçoivent un accès de faible priorité aux ressources une fois qu’elles ont été interrompues. Pour prendre en charge cette nouvelle priorité, le délai de l’opération de suspension est prolongé afin que l’application dispose d’un délai de 5 secondes en priorité normale sur Windows ou de 1 à 10 secondes sur Windows Phone. Vous ne pouvez pas étendre ni modifier ce délai.

**Remarque concernant le débogage à l’aide de Visual Studio :** Visual Studio empêche Windows de suspendre une application qui est jointe au débogueur. afin que l’utilisateur puisse voir l’interface de débogage de Visual Studio pendant l’exécution de l’application. Lorsque vous déboguez une application, vous pouvez lui envoyer un événement de suspension à l’aide de Visual Studio. Vérifiez que la barre d’outils **Emplacement de débogage** est visible et cliquez sur l’icône **Suspendre**.

## <a name="related-topics"></a>Rubriques connexes

* [Cycle de vie de l’application](app-lifecycle.md)
* [Gérer l’activation d’une application](activate-an-app.md)
* [Gérer la reprise d’une application](resume-an-app.md)
* [Instructions d’expérience utilisateur pour le lancement, l’interruption et la reprise](https://docs.microsoft.com/windows/uwp/launch-resume/index)
* [Exécution étendue](run-minimized-with-extended-execution.md)

 

 
