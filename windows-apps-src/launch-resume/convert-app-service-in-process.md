---
title: Convertir un service d’application pour qu’il s’exécute dans le même processus que son application hôte
description: Convertissez un code de service d’application qui s’exécutait dans un processus distinct en arrière-plan en code qui s’exécute dans le même processus que votre fournisseur de service d’application.
ms.date: 11/03/2017
ms.topic: article
keywords: Windows 10, uwp, le service d’application
ms.assetid: 30aef94b-1b83-4897-a2f1-afbb4349696a
ms.localizationpriority: medium
ms.openlocfilehash: 2de79a5c5090f9dbe070f56ee6b2afd73d78f05f
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366347"
---
# <a name="convert-an-app-service-to-run-in-the-same-process-as-its-host-app"></a>Convertir un service d’application pour qu’il s’exécute dans le même processus que son application hôte

Une [AppServiceConnection](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection) permet à une autre application de réactiver votre application en arrière-plan et de démarrer une ligne directe de communication avec celle-ci.

Avec l’introduction des services d’application intégrés au processus, deux applications au premier plan en cours d’exécution peuvent disposer d’une ligne directe de communication via une connexion de service d’application. Les services d’application peuvent désormais s’exécuter dans le même processus que l’application au premier plan, ce qui simplifie grandement la communication entre des applications et évite d’avoir à séparer le code de service dans un projet distinct.

Transformer un service d’application de modèle en dehors du processus en modèle intégré au processus nécessite deux modifications. La première est une modification du manifeste.

> ```xml
> <Package
>    ...
>   <Applications>
>       <Application Id=...
>           ...
>           EntryPoint="...">
>           <Extensions>
>               <uap:Extension Category="windows.appService">
>                   <uap:AppService Name="InProcessAppService" />
>               </uap:Extension>
>           </Extensions>
>           ...
>       </Application>
>   </Applications>
> ```

Supprimer le `EntryPoint` attribut à partir de la `<Extension>` élément parce que maintenant [OnBackgroundActivated()](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.onbackgroundactivated) est le point d’entrée qui sera utilisé lorsque le service d’application est appelé.

La seconde modification consiste à déplacer la logique de service de son projet de tâche distinct en arrière-plan dans les méthodes qui peuvent être appelées à partir de **OnBackgroundActivated()** .

À présent, votre application peut directement exécuter votre service d’application. Par exemple, dans App.xaml.cs :

[!NOTE] Le code ci-dessous est différent de celui fourni par exemple 1 (out-of-process service). Le code ci-dessous est fourni à titre d’illustration uniquement et ne doit pas être utilisé dans le cadre de l’exemple 2 (dans le processus de service).  Pour continuer la transition de l’article à partir de l’exemple 1 (service out-of-process) dans l’exemple 2 (dans le processus de service) continuer à utiliser le code fourni par exemple 1 au lieu de l’illustration de code ci-dessous.

``` cs
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
...

sealed partial class App : Application
{
  private AppServiceConnection _appServiceConnection;
  private BackgroundTaskDeferral _appServiceDeferral;

  ...

  protected override void OnBackgroundActivated(BackgroundActivatedEventArgs args)
  {
      base.OnBackgroundActivated(args);
      IBackgroundTaskInstance taskInstance = args.TaskInstance;
      AppServiceTriggerDetails appService = taskInstance.TriggerDetails as AppServiceTriggerDetails;
      _appServiceDeferral = taskInstance.GetDeferral();
      taskInstance.Canceled += OnAppServicesCanceled;
      _appServiceConnection = appService.AppServiceConnection;
      _appServiceConnection.RequestReceived += OnAppServiceRequestReceived;
      _appServiceConnection.ServiceClosed += AppServiceConnection_ServiceClosed;
  }

  private async void OnAppServiceRequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
  {
      AppServiceDeferral messageDeferral = args.GetDeferral();
      ValueSet message = args.Request.Message;
      string text = message["Request"] as string;

      if ("Value" == text)
      {
          ValueSet returnMessage = new ValueSet();
          returnMessage.Add("Response", "True");
          await args.Request.SendResponseAsync(returnMessage);
      }
      messageDeferral.Complete();
  }

  private void OnAppServicesCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
  {
      _appServiceDeferral.Complete();
  }

  private void AppServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
  {
      _appServiceDeferral.Complete();
  }
}
```

Dans le code ci-dessus, la méthode `OnBackgroundActivated` gère l’activation du service d’application. Tous les événements nécessaires pour la communication via une [AppServiceConnection](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appservice.appserviceconnection) sont enregistrés, et l’objet de report de tâche est stocké de façon qu’il puisse être marqué comme terminé une fois la communication entre les applications effectuée.

Lorsque l’application reçoit une demande, elle lit l’élément [ValueSet](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset) fourni pour voir si les chaînes `Key` et `Value` sont présentes. Si elles sont bien présentes, le service d’application renvoie alors une paire de valeurs de chaîne `Response` et `True` à l’application sur l’autre côté de la **AppServiceConnection**.

Pour en savoir plus sur la connexion et la communication avec d’autres applications, consultez [Créer et utiliser un service d’application](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service?f=255&MSPPError=-2147217396).
