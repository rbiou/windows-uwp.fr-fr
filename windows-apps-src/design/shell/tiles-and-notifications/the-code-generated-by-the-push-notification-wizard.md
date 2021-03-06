---
Description: Grâce à un Assistant Visual Studio, vous pouvez générer des notifications Push à partir d’un service mobile créé par le biais de Microsoft Azure Mobile Services.
title: Code généré par l’Assistant Notification Push
ms.assetid: 340F55C1-0DDF-4233-A8E4-C15EF9030785
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6a3429607ecd9245dd10722fbb47d06d7aa6ef75
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234834"
---
# <a name="code-generated-by-the-push-notification-wizard"></a>Code généré par l’Assistant Notification Push
 

Grâce à un Assistant Visual Studio, vous pouvez générer des notifications Push à partir d’un service mobile créé par le biais de Microsoft Azure Mobile Services. L’Assistant Visual Studio génère du code qui devrait vous aider à démarrer. Cette rubrique explique comment l’Assistant modifie votre projet, ce que le code généré fait, comment utiliser ce code et ce que vous pouvez faire ensuite pour tirer le meilleur parti des notifications Push. Consultez [Vue d’ensemble des services de notifications Push Windows (WNS)](windows-push-notification-services--wns--overview.md).

## <a name="how-the-wizard-modifies-your-project"></a>Comment l’Assistant modifie votre projet


L’Assistant de notifications Push modifie votre projet comme suit :

-   Il ajoute une référence au client géré des services mobiles (MobileServicesManagedClient.dll). Ne s’applique pas aux projets JavaScript.
-   Il ajoute un fichier dans un sous-dossier sous services, puis nomme le fichier push.register.cs, push.register.vb, push.register.cpp ou push.register.js.
-   Il crée une table de canaux sur le serveur de base de données pour le service mobile. La table contient des informations nécessaires pour l’envoi de notifications Push aux instances de l’application.
-   Il crée les scripts des quatre fonctions suivantes : supprimer, insérer, lire et mettre à jour.
-   Il crée un script avec une API personnalisée, notifyallusers.js, qui envoie une notification Push à tous les clients.
-   Il ajoute une déclaration à votre fichier App.xaml.cs, App.xaml.vb ou App.xaml.cpp, ou à un nouveau fichier, service.js, pour les projets JavaScript. Cette déclaration déclare un objet MobileServiceClient, qui contient les informations nécessaires pour se connecter au service mobile. Vous pouvez accéder à cet objet MobileServiceClient, nommé *MyServiceName*Client, à partir de n’importe quelle page de votre application à l’aide du nom App.*MyServiceName*Client.

Le fichier services.js contient le code suivant :

```js
var <mobile-service-name>Client = new Microsoft.WindowsAzure.MobileServices.MobileServiceClient(
                "https://<mobile-service-name>.azure-mobile.net/",
                "<your client secret>");
```

## <a name="registration-for-push-notifications"></a>Inscription aux notifications Push


Dans push. Register. \* , la méthode UploadChannel inscrit l’appareil pour recevoir des notifications push. Le Windows Store effectue le suivi des instances installées de votre application et fournit le canal de notification Push. Consultez [**PushNotificationChannelManager**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager).

Le code client est semblable pour le système principal JavaScript et le système principal .NET. Par défaut, si vous ajoutez des notifications Push pour un service principal JavaScript, un exemple d’appel d’API personnalisée notifyAllUsers est inséré dans la méthode UploadChannel.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.MobileServices;
using Newtonsoft.Json.Linq;

namespace App2
{
    internal class mymobileservice1234Push
    {
        public async static void UploadChannel()
        {
            var channel = await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            try
            {
                await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri);
                await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers");
            }
            catch (Exception exception)
            {
                HandleRegisterException(exception);
            }
        }

        private static void HandleRegisterException(Exception exception)
        {
            
        }
    }
}
```

```vb
Imports Microsoft.WindowsAzure.MobileServices
Imports Newtonsoft.Json.Linq

Friend Class mymobileservice1234Push
    Public Shared Async Sub UploadChannel()
        Dim channel = Await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync()

        Try
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri)
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri, New String() {"tag1", "tag2"})
            Await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers")
        Catch exception As Exception
            HandleRegisterException(exception)
        End Try
    End Sub

    Private Shared Sub HandleRegisterException(exception As Exception)

    End Sub
End Class
```

```c++
#include "pch.h"
#include "services\mobile services\mymobileservice1234\mymobileservice1234Push.h"

using namespace AzureMobileHelper;

using namespace web;
using namespace concurrency;

using namespace Windows::Networking::PushNotifications;

void mymobileservice1234Push::UploadChannel()
{
    create_task(PushNotificationChannelManager::CreatePushNotificationChannelForApplicationAsync()).
    then([] (PushNotificationChannel^ newChannel) 
    {
        return mymobileservice1234MobileService::GetClient().get_push().register_native(newChannel->Uri->Data());
    }).then([]()
    {
        return mymobileservice1234MobileService::GetClient().invoke_api(L"notifyAllUsers");
    }).then([](task<json::value> result)
    {
        try
        {
            result.wait();
        }
        catch(...)
        {
            HandleExceptionsComingFromTheServer();
        }
    });
}

void mymobileservice1234Push::HandleExceptionsComingFromTheServer()
{
}
```

```js
(function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.addEventListener("activated", function (args) {
        if (args.detail.kind == activation.ActivationKind.launch) {
            Windows.Networking.PushNotifications.PushNotificationChannelManager.createPushNotificationChannelForApplicationAsync()
                .then(function (channel) {
                    mymobileserviceclient1234Client.push.registerNative(channel.Uri, new Array("tag1", "tag2"))
                    return mymobileservice1234Client.push.registerNative(channel.uri);
                })
                .done(function (registration) {
                    return mymobileservice1234Client.invokeApi("notifyAllUsers");
                }, function (error) {
                    // Error

                });
        }
    });
})();
```

Les balises de notification Push permettent de restreindre les notifications à un sous-ensemble de clients. Vous pouvez utiliser la méthode registerNative (ou la méthode RegisterNativeAsync) pour inscrire toutes les notifications Push sans spécifier de balises ou vous pouvez effectuer l’inscription avec des balises en fournissant le deuxième argument, un tableau de balises. Si vous effectuez l’inscription avec une ou plusieurs balises, vous ne recevrez que les notifications correspondant à ces balises.

## <a name="server-side-scripts-javascript-backend-only"></a>Scripts côté serveur (système principal JavaScript uniquement)


Pour les services mobiles qui utilisent le système principal JavaScript, les scripts côté serveur s’exécutent quand des opérations de suppression, insertion, lecture ou mise à jour se produisent. Les scripts n’implémentent pas ces opérations, mais ils s’exécutent quand un appel d’un client vers l’API REST de Windows Mobile déclenche ces événements. Les scripts passent ensuite le contrôle aux opérations elles-mêmes en appelant request.execute ou request.respond pour répondre au contexte d’appel. Voir [Informations de référence sur l’API REST d’Azure Mobile Services](https://msdn.microsoft.com/library/azure/jj710108.aspx).

De nombreuses fonctions sont disponibles dans le script côté serveur. Voir [Inscrire des opérations de table dans Azure Mobile Services](https://msdn.microsoft.com/library/azure/dn167708.aspx). Pour obtenir des informations de référence sur toutes les fonctions disponibles, consultez [Informations de référence sur les scripts serveur de Mobile Services](https://msdn.microsoft.com/library/windowsazure/jj554226).

Le code d’API personnalisé suivant est également créé dans Notifyallusers.js :

```js
exports.post = function(request, response) {
    response.send(statusCodes.OK,{ message : 'Hello World!' })
    
    // The following call is for illustration purpose only
    // The call and function body should be moved to a script in your app
    // where you want to send a notification
    sendNotifications(request);
};

// The following code should be moved to appropriate script in your app where notification is sent
function sendNotifications(request) {
    var payload = '<?xml version="1.0" encoding="utf-8"?><toast><visual><binding template="ToastText01">' +
        '<text id="1">Sample Toast</text></binding></visual></toast>';
    var push = request.service.push; 
    push.wns.send(null,
        payload,
        'wns/toast', {
            success: function (pushResponse) {
                console.log("Sent push:", pushResponse);
            }
        });
}
```

La fonction sendNotifications envoie une seule notification sous forme d’une notification toast. Vous pouvez également utiliser d’autres types de notifications Push.

**Conseil**    Pour plus d’informations sur la façon d’obtenir de l’aide lors de la modification de scripts, consultez [activation d’IntelliSense pour le JavaScript côté serveur](https://blogs.msdn.com/b/visualstudio/archive/2013/07/26/enabling-intellisense-for-mobile-services-javascript-in-visual-studio.aspx).

 

## <a name="push-notification-types"></a>Types de notifications Push


Windows prend en charge les notifications qui ne sont pas des notifications Push. Pour obtenir des informations générales sur les notifications, consultez [choix d’une méthode de remise des notifications](choosing-a-notification-delivery-method.md).

Les notifications toast sont faciles à utiliser. Vous pouvez retrouver un exemple dans le code du fichier Insert.js, dans la table du canal générée automatiquement. Si vous envisagez d’utiliser des notifications par vignette ou des notifications de badge, vous devez non seulement créer un modèle XML pour la vignette ou pour le badge, mais aussi spécifier l’encodage des informations empaquetées dans le modèle. Voir [Utilisation de vignettes, de badges et de notifications toast](https://docs.microsoft.com/previous-versions/windows/apps/hh868259(v=win.10)).

Dans la mesure où Windows répond aux notifications Push, il peut gérer la plupart de ces notifications quand l’application n’est pas en cours d’exécution. Par exemple, une notification Push peut avertir un utilisateur quand un nouveau message électronique est disponible, même si l’application de messagerie locale n’est pas en cours d’exécution. Windows gère les notifications toast en affichant un message, comme la première ligne d’un message texte. Windows gère les notifications par vignette ou les notifications de badge en mettant à jour la vignette dynamique d’une application de manière à refléter le nombre de nouveaux messages électroniques. De cette manière, vous pouvez inviter les utilisateurs de votre application à vérifier s’il existe de nouvelles informations. Votre application peut recevoir des notifications brutes quand elle est en cours d’exécution et vous pouvez les utiliser pour envoyer des données à votre application. Si votre application n’est pas en cours d’exécution, vous pouvez définir une tâche en arrière-plan pour surveiller les notifications Push.

Vous devez utiliser les notifications push conformément aux instructions pour les applications Windows, car ces notifications utilisent les ressources d’un utilisateur et peuvent être gênantes si elles sont utilisées. Voir [Recommandations et liste de vérification sur les notifications Push](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview).

Si vous mettez à jour des vignettes dynamiques avec des notifications Push, suivez également les recommandations énumérées dans [Recommandations et liste de vérification sur les vignettes et les badges](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles).

## <a name="next-steps"></a>Étapes suivantes


### <a name="using-the-windows-push-notification-services-wns"></a>Utilisation des Services de notifications Push Windows (WNS)

Vous pouvez appeler les Services de notifications Push Windows (WNS) directement si les services mobiles n’offrent pas suffisamment de flexibilité, si vous souhaitez écrire le code du serveur en C# ou Visual Basic, ou si vous disposez déjà d’un service cloud et souhaitez envoyer des notifications Push à partir de celui-ci. En appelant WNS directement, vous pouvez envoyer des notifications Push à partir de votre propre service cloud, par exemple un rôle de travail qui surveille des données à partir d’une base de données ou d’un autre service web. Votre service cloud doit s’authentifier auprès de WNS pour envoyer des notifications Push à vos applications. Voir [Comment s’authentifier auprès des services de notifications Push Windows (JavaScript)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10)) ou [(C#/C++/VB)](https://docs.microsoft.com/previous-versions/windows/apps/hh868206(v=win.10)).

Vous pouvez également envoyer des notifications Push en exécutant une tâche planifiée dans votre service mobile. Voir [Planifier des travaux périodiques dans Mobile Services](https://azure.microsoft.com/documentation/articles/mobile-services-schedule-recurring-tasks/).

**Avertissement**    Une fois que vous avez exécuté l’Assistant notifications push, n’exécutez pas l’Assistant une deuxième fois pour ajouter le code d’inscription d’un autre service mobile. Plusieurs exécutions de l’Assistant par projet génèrent du code qui engendre des chevauchements d’appels de la méthode [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync), ce qui entraîne une exception runtime. Si vous souhaitez inscrire des notifications Push pour plusieurs services mobiles, exécutez l’Assistant une fois, puis réécrivez le code d’inscription pour veiller à ce que les appels de **CreatePushNotificationChannelForApplicationAsync** ne s’exécutent pas en même temps. Par exemple, vous pouvez le faire en déplaçant le code généré par l’Assistant dans push. Register. \* (y compris l’appel à **CreatePushNotificationChannelForApplicationAsync**) en dehors de l’événement OnLaunched, mais les spécificités de cette opération dépendent de l’architecture de votre application.

 

## <a name="related-topics"></a>Rubriques connexes


* [Vue d’ensemble des services de notifications Push Windows (WNS)](windows-push-notification-services--wns--overview.md)
* [Vue d’ensemble des notifications brutes](raw-notification-overview.md)
* [Connexion aux services mobiles Microsoft Azure (JavaScript)](https://docs.microsoft.com/previous-versions/windows/apps/dn263160(v=win.10))
* [Connexion aux services mobiles Microsoft Azure (C#/C++/VB)](https://docs.microsoft.com/previous-versions/windows/apps/dn263175(v=win.10))
* [Démarrage rapide : ajout de notifications Push pour un service mobile (JavaScript)](https://docs.microsoft.com/previous-versions/windows/apps/dn263163(v=win.10))
 

 




