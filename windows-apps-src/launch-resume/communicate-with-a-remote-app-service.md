---
title: Communiquer avec un service d’application distant
description: Échanger des messages avec un service d'application exécuté sur un appareil distant utilisant le projet Rome.
ms.assetid: a0261e7a-5706-4f9a-b79c-46a3c81b136f
ms.date: 02/08/2017
ms.topic: article
keywords: appareils Windows 10, uwp, connectés, systèmes distants, rome, project rome, tâche en arrière-plan, service d’application
ms.localizationpriority: medium
ms.openlocfilehash: 067b465feccda424dd6a8e3f44e784166afe6d48
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366418"
---
# <a name="communicate-with-a-remote-app-service"></a>Communiquer avec un service d’application distant

En plus de lancer une application sur un appareil distant avec un URI, vous pouvez exécuter des *services d’application* et communiquer avec eux sur des appareils distants. Un appareil Windows peut servir d’appareil client ou d’appareil hôte. Ce qui vous donne un nombre quasiment illimité de modes d’interaction avec les appareils connectés, sans avoir besoin d’amener une application au premier plan.

## <a name="set-up-the-app-service-on-the-host-device"></a>Configurer le service d’application sur l’appareil hôte
Pour exécuter un service d’application sur un appareil distant, un fournisseur de ce service d’application doit être installé sur cet appareil. Ce guide utilise la version CSharp de l'[exemple du service d’application Générateur de nombres aléatoires](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices), qui est disponible dans le [référentiel d’exemples d’application Windows universelle](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices). Pour obtenir des instructions sur la rédaction du code de votre service d’application, consultez [Créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md).

Que vous utilisiez un service d’application prêt à l’emploi ou créé par vos soins, vous devez lui apporter quelques modifications pour le rendre compatible avec les systèmes distants. Dans Visual Studio, accédez au projet du fournisseur du service d’application (appelé « AppServicesProvider ») et sélectionnez son fichier _Package.appxmanifest_. Cliquez sur le bouton droit et sélectionnez **Afficher le code** pour afficher le contenu du fichier. Créer un **Extensions** élément à l’intérieur de la main **Application** élément (ou trouver si elle existe déjà). Créez ensuite un **Extension** pour définir le projet comme un service d’application et de faire référence à son projet parent.

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap3:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

À côté du **AppService** élément, ajouter le **SupportsRemoteSystems** attribut :

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

Pour pouvoir utiliser des éléments dans ce **uap3** espace de noms, vous devez ajouter la définition de l’espace de noms en haut du fichier manifeste s’il n’est déjà fait.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3">
  ...
</Package>
```

Puis générez votre projet de fournisseur de service application et déployez-la sur l’appareil (s) hôte.

## <a name="target-the-app-service-from-the-client-device"></a>Cibler le service d’application à partir de l’appareil client
L’appareil à partir duquel le service d’application distant doit être appelé a besoin de l’application avec la fonctionnalité Systèmes distants. Vous pouvez l’ajouter dans l’application qui fournit le service d’application sur l’appareil hôte (auquel cas la même application doit être installée sur les deux appareils) ou dans une autre application.

Les instructions **using** suivantes sont nécessaires pour que le code de cette section s’exécute tel quel :

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetUsings)]


Vous devez d’abord instancier un objet [**AppServiceConnection**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection), comme si vous vouliez appeler un service d’application localement. Ce processus est décrit plus en détail dans [Créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md). Dans cet exemple, le service d’application à cibler est le service Générateur de nombres aléatoires.

> [!NOTE]
> On suppose qu’un objet [RemoteSystem](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystem) a déjà été acquis par un moyen quelconque dans le code qui appelle la méthode suivante. Pour obtenir des instructions sur ce type de configuration, consultez [Lancer une application sur un appareil distant](launch-a-remote-app.md).

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetAppService)]

Ensuite, un objet [**RemoteSystemConnectionRequest**](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems.RemoteSystemConnectionRequest) est créé pour l’appareil distant concerné. Il est utilisé pour ouvrir la connexion **AppServiceConnection** à cet appareil. Notez que dans l’exemple ci-dessous, le traitement et le signalement des erreurs sont grandement simplifiés par souci de concision.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetRemoteConnection)]

À ce stade, vous devez avoir une connexion ouverte à un service d’application sur un ordinateur distant.

## <a name="exchange-service-specific-messages-over-the-remote-connection"></a>Échanger des messages avec le service sur la connexion à distance

Maintenant, vous pouvez échanger des messages avec le service sous la forme d’objets [**ValueSet**](https://docs.microsoft.com/uwp/api/windows.foundation.collections.valueset). Pour plus d’informations, consultez [Créer et utiliser un service d’application](how-to-create-and-consume-an-app-service.md). Le service Générateur de nombres aléatoires prend deux entiers avec les clés `"minvalue"` et `"maxvalue"` comme entrées, sélectionne de manière aléatoire un entier entre ces deux valeurs et le renvoie au processus appelant avec la clé `"Result"`.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetSendMessage)]

Maintenant que vous êtes connecté à un service d’application sur un appareil hôte ciblé, exécutez une opération sur cet appareil et recevez les données sur votre appareil client.

## <a name="related-topics"></a>Rubriques connexes

[Vue d’ensemble des (Project Rome) applications et des appareils connecté](connected-apps-and-devices.md)  
[Lancer une application à distance](launch-a-remote-app.md)  
[Créer et consommer un service d’application](how-to-create-and-consume-an-app-service.md)  
[Référence de l’API de systèmes à distance](https://docs.microsoft.com/uwp/api/Windows.System.RemoteSystems)  
[Exemple de systèmes distants](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
