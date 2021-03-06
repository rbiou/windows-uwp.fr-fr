---
description: En plus de créer une campagne de publicité pour votre application qui s’exécute dans des applications Windows, vous pouvez promouvoir votre application à l’aide d’autres canaux.
title: Créer une campagne personnalisée de promotion d’applications
ms.assetid: 7C9BF73E-B811-4FC7-B1DD-4A0C2E17E95D
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, personnalisé, application, promotion, campagne
ms.localizationpriority: medium
ms.openlocfilehash: 407a34294155e688e672db392c262e1607c01a39
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57653734"
---
# <a name="create-a-custom-app-promotion-campaign"></a>Créer une campagne personnalisée de promotion d’applications

En plus de créer une [campagne de publicité pour votre application](create-an-ad-campaign-for-your-app.md) qui s'exécute dans des applications Windows, vous pouvez également promouvoir votre application à l'aide d'autres canaux. Par exemple, vous pouvez promouvoir votre application en faisant appel à un fournisseur de commercialisation d’applications tiers, ou en publiant des liens vers votre application sur des médias sociaux. Ces activités sont appelées *campagnes personnalisées*.

Si vous lancez des campagnes personnalisées pour votre application, vous pouvez suivre les performances de chacune d’elles en créant pour chaque campagne personnalisée une URL distincte contenant un *ID de campagne* différent. Lorsqu’un client exécutant Windows 10 clique sur une URL qui contient un ID de campagne, Microsoft associe la, cliquez sur la campagne correspondante personnalisée et met ces données à votre disposition dans [partenaires](https://partner.microsoft.com/dashboard).

> [!IMPORTANT]
> Ces données sont suivies uniquement pour les clients sur Windows 10. Les clients utilisant d’autres systèmes d’exploitation peuvent suivre le lien vers la description de votre application, mais les données sur les activités de ces clients ne seront pas incluses.

Il existe deux principaux types de données associés à des campagnes personnalisées : les *vues de page* de la description de votre application dans le Store et les *conversions*. Une conversion est une acquisition d’application qui résulte de la consultation par un client de la page de description de votre application dans le Windows Store à partir d’une URL incluant un ID de campagne personnalisée. Pour plus d’informations sur les conversions, voir la section [Comprendre comment les acquisitions d’application sont éligibles en tant que conversions](#understanding-how-acquisitions-qualify-as-conversions) dans cette rubrique.

Pour récupérer les données de performances de campagne personnalisée de votre application, procédez comme suit :

* Vous pouvez afficher des données sur les vues de page et des conversions pour votre application ou d’un module complémentaire à partir de la **affichages de pages d’application et les conversions par ID de campagne** et **nombre Total de conversions de campagne** graphiques dans le [Acquisitions rapport](acquisitions-report.md).
* Si votre application est une application de plateforme Windows universelle (UWP), vous pouvez utiliser les API du SDK Windows pour récupérer par programme l’ID de campagne personnalisée qui a abouti à une conversion.

## <a name="example-custom-campaign-scenario"></a>Exemple de scénario de campagne personnalisée

Imaginez un développeur de jeu venant d’achever la création d’un jeu et désireux de promouvoir celui-ci auprès des joueurs de ses jeux existants. Il publie une annonce relative au lancement du nouveau de jeu sur sa page Facebook, incluant un lien vers la description du jeu dans le Store. Bon nombre de ses joueurs le suivant sur Twitter, il publie également un tweet d’annonce contenant le lien vers la description du jeu dans le Store.

Pour suivre le succès de chacun de ces canaux de promotion, le développeur crée deux variantes de l’URL vers la description du jeu dans le Store :

* L’URL qu’elle va publier sa page Facebook inclut l’ID de campagne personnalisée `my-facebook-campaign`

* L’URL qu’elle publie sur Twitter inclut l’ID de campagne personnalisée `my-twitter-campaign`

Lorsque les abonnés du développeur sur Facebook et Twitter cliquent sur l’URL, Microsoft suit chaque clic et l’associe à la campagne personnalisée correspondante. Les acquisitions éligibles du jeu et achats de modules complémentaires sont associés à la campagne personnalisée et déclarés comme autant de conversions.

<span id="conversions" />

## <a name="understanding-how-acquisitions-qualify-as-conversions"></a>Comprendre comment les acquisitions sont éligibles en tant que conversions

Une *conversion* de campagne personnalisée est une acquisition qui résulte d’un clic par un client sur une URL promue par le biais d’une campagne personnalisée. Il existe différents scénarios de qualifier une conversion pour les **affichages de pages d’application et les conversions par ID de campagne** et **nombre Total de conversions de campagne** graphiques dans le [rapport d’Acquisitions ](acquisitions-report.md) et permet de qualifier une conversion pour [extraction par programmation de l’ID de campagne](#programmatically).

### <a name="qualifying-conversions-in-the-acquisitions-report"></a>Qualification des conversions dans le rapport d’Acquisitions

Les scénarios ci-après permettent de faire figurer une conversion dans les graphiques **Conversions et accès aux pages de l’application par ID de campagne** et **Conversions totales de campagnes** du [rapport Acquisitions](acquisitions-report.md) :

* Un client *disposant ou non d’un compte Microsoft reconnu* clique sur une URL d’application contenant un ID de campagne personnalisée, puis est redirigé vers la description de l’application dans le Windows Store. Ensuite, ce même client acquiert l’application dans les 24 heures après avoir cliqué pour la première fois sur l’URL du Microsoft Store avec l’ID de campagne personnalisée.

* Si le client acquiert l’application sur un appareil autre que celui qu’il a utilisé pour cliquer sur l’URL avec l’ID de campagne personnalisée, la conversion sera uniquement prise en compte si le client s’est connecté avec le même compte Microsoft que celui avec lequel il a cliqué sur l’URL.

> [!NOTE]
> Pour les acquisitions d’application comptant comme conversions dans le cadre d’une campagne personnalisée, les achats d’extension dans cette application sont également comptabilisés comme conversions pour la même campagne personnalisée.

### <a name="qualifying-conversions-when-programmatically-retrieving-the-campaign-id"></a>Conversions éligibles lors de la récupération par programme de l’ID de campagne

Pour être éligible en tant que conversion lors de la récupération par programme de l’ID de campagne associé à l’application, les conditions suivantes doivent être réunies :

* Sur un appareil exécutant **Windows 10, version 1607 ou ultérieure**: Un client (qu’il soit connecté à un compte Microsoft reconnu ou non) clique sur une URL qui contient un ID de campagne personnalisé et est redirigée vers la page de liste Store de l’application. Le client acquiert l’application lorsqu’il en visualise la description dans le Windows Store après avoir cliqué sur l’URL.

* Sur un appareil exécutant **Windows 10, version 1511, ou une version antérieure**: Un client (qui doit être connecté avec un compte Microsoft reconnu) clique sur une URL qui contient un ID de campagne personnalisé et est redirigée vers la page de liste Store de l’application. Le client acquiert l’application lorsqu’il en visualise la description dans le Windows Store après avoir cliqué sur l’URL. Sur ces versions de Windows 10, l’utilisateur doit s’être connecté avec un compte Microsoft reconnu pour que l’acquisition soit éligible en tant que conversion lors de la récupération par programme de l’ID de campagne.

> [!NOTE]
> Si le client quitte la page de description dans le Windows Store, mais réaccède à cette page dans les 24 heures (soit sur le même appareil, soit sur un autre appareil tout en étant connecté avec le même compte Microsoft) et acquiert l’application, cette acquisition **sera** éligible en tant que conversion dans les graphiques **Conversions et accès aux pages de l’application par ID de campagne** et **Conversions totales de campagnes** du [rapport Acquisitions](acquisitions-report.md). Toutefois, elle ne **sera pas** éligible en tant que conversion si vous récupérez par programme l’ID de campagne.

## <a name="embed-a-custom-campaign-id-to-your-apps-microsoft-store-page-url"></a>Incorporer un ID de campagne personnalisée dans l’URL de la page de votre application dans le Microsoft Store

Pour créer une URL pour la page de votre application dans le Microsoft Store avec un ID de campagne personnalisée :

1.  Créez une chaîne d’ID pour la campagne personnalisée. Cette chaîne peut contenir jusqu’à 100 caractères, même s’il est recommandé de définir un ID de campagne court facilement identifiable.

   > [!NOTE]
   > La chaîne d’ID de campagne peut être visible par d’autres développeurs lorsque ces derniers consultent le [rapport Acquisitions](acquisitions-report.md) concernant leurs applications. Cela peut se produire lorsqu’un client clique sur votre ID de campagne personnalisée pour entrer dans le Windows Store, puis achète l’application d’un autre développeur au cours de la même session, attribuant ainsi cette conversion à votre ID de campagne. Ce développeur verra le nombre de conversions obtenues pour sa propre application après un clic initial sur votre ID de campagne, y compris le nom de la campagne, mais il ne verra aucune information concernant le nombre d’utilisateurs ayant acheté vos propres applications (ou les applications d’autres développeurs) après avoir cliqué sur votre ID de campagne.

2.  Obtenez le lien de la description de votre application dans le Store au format HTML ou de protocole.

    * Utilisez l’URL HTML si vous souhaitez que les clients accèdent à la description de votre application dans le Windows Store sur le web par le biais d’un navigateur sur n’importe quel système d’exploitation. Sur les appareils Windows, l’application du Windows Store lancera et affichera également la description de votre application. Cette URL est au format **`https://www.microsoft.com/store/apps/*your app ID*`**. Par exemple, l’URL HTML de Skype est `https://www.microsoft.com/store/apps/9wzdncrfj364`. Vous pouvez trouver cette URL sur votre page [Identité des applications](view-app-identity-details.md#link-to-your-apps-listing).

    * Utilisez le format de protocole si vous promouvez votre application à partir d’autres applications Windows s’exécutant sur un appareil ou un ordinateur sur lequel l’application UWP est installée, ou lorsque vous savez que vos clients utilisent un appareil prenant en charge l’application du Microsoft Store. Ce lien permettra d’accéder directement à la description de votre application dans le Windows Store sans ouvrir de navigateur. Cette URL est au format **`ms-windows-store://pdp/?PRODUCTID=*your app id*`**. Par exemple, l’URL de protocole pour Skype est `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364`.

3.  Ajouter la chaîne suivante à la fin de l’URL de votre application :

    * Pour une URL au format HTML, ajoutez **`?cid=*my custom campaign ID*`**. Par exemple, si Skype introduit un ID de campagne avec la valeur **personnalisé\_campagne**, la nouvelle URL, y compris la campagne ID serait : `https://www.microsoft.com/store/apps/skype/9wzdncrfj364?cid=custom\_campaign`.

    * Pour une URL au format de protocole, ajoutez **`&cid=*my custom campaign ID*`**. Par exemple, si Skype introduit un ID de campagne avec la valeur **personnalisé\_campagne**, la nouvelle URL de protocole, y compris la campagne ID serait : `ms-windows-store://pdp/?PRODUCTID=9wzdncrfj364&cid=custom\_campaign`.

<span id="programmatically" />

## <a name="programmatically-retrieve-the-custom-campaign-id-for-an-app"></a>Récupérer par programme l’ID de campagne personnalisée pour une application

Si votre application est une application UWP, vous pouvez récupérer par programme l’ID de campagne personnalisée associé à une acquisition de l’application en utilisant les API du SDK Windows. Ces API rendent possibles de nombreux scénarios d’analyse et de monétisation. Par exemple, vous pouvez déterminer si l’utilisateur actuel a acquis votre application après l’avoir découverte via votre campagne Facebook, puis personnaliser l’expérience de l’application en conséquence. En guise d’alternative, si vous utilisez un fournisseur de commercialisation d’applications tiers, vous pouvez lui renvoyer des données.

Ces API ne renvoient une chaîne d’ID de campagne que si le client a cliqué sur votre URL incorporant l’ID de campagne, qu’il a visualisé la page de votre application dans le Microsoft Store, puis qu’il a acquis votre application sans quitter la page de description dans le Store. Si l’utilisateur quitte la page, puis y revient pour acquérir l’application, cela n’est pas [éligible en tant que conversion](#conversions) lors de l’utilisation de ces API.

Vous pouvez utiliser différentes API en fonction de la version de Windows 10 ciblée par votre application :

* Windows 10, version 1607 ou ultérieure : Utilisez le [ **StoreContext** ](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) classe dans le **Windows.Services.Store** espace de noms. Lorsque vous utilisez cette API, vous pouvez récupérer les ID de campagne personnalisée pour les [acquisitions éligibles](#conversions), que l’utilisateur se soit ou non connecté avec un compte Microsoft reconnu.

* Windows 10 version 1511, ou une version antérieure : Utilisez le [ **CurrentApp** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) classe dans le **Windows.ApplicationModel.Store** espace de noms. Lorsque vous utilisez cette API, vous ne pouvez récupérer les ID de campagne personnalisée pour les [acquisitions éligibles](#conversions) que si l’utilisateur s’est connecté avec un compte Microsoft reconnu.

> [!NOTE]
> Même si l’espace de noms **Windows.ApplicationModel.Store** est disponible dans toutes les versions de Windows 10, nous vous recommandons d’utiliser les API de l’espace de noms **Windows.Services.Store** si votre application cible Windows 10, version 1607 ou ultérieure. Pour plus d’informations sur les différences entre ces espaces de noms, voir [Achats dans l’application et versions d’évaluation](../monetize/in-app-purchases-and-trials.md#choose-namespace). L’exemple de code suivant montre comment structurer votre code pour utiliser les deux API dans le même projet.

### <a name="code-example"></a>Exemple de code

L’exemple de code suivant montre comment récupérer l’ID de campagne personnalisée. Cet exemple utilise les deux ensembles d’API dans les espaces de noms **Windows.Services.Store** et **Windows.ApplicationModel.Store** à l’aide du [code adaptatif de version](../debug-test-perf/version-adaptive-code.md). En suivant ce processus, votre code peut s’exécuter sur toute version de Windows 10. L’utilisation de ce code nécessite que la version du système d’exploitation cible de votre projet soit **Édition anniversaire Windows 10 (version 10.0, build 14394)** ou une version ultérieure, bien que la version minimale du système d’exploitation puisse correspondre à une version antérieure.

``` csharp
// This example assumes the code file has using statements for
// System.Linq, System.Threading.Tasks, Windows.Data.Json,
// and Windows.Services.Store.
public async Task<string> GetCampaignId()
{
    // Use APIs in the Windows.Services.Store namespace if they are available
    // (the app is running on a device with Windows 10, version 1607, or later).
    if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent(
         "Windows.Services.Store.StoreContext"))
    {
        StoreContext context = StoreContext.GetDefault();

        // Try to get the campaign ID for users with a recognized Microsoft account.
        StoreProductResult result = await context.GetStoreProductForCurrentAppAsync();
        if (result.Product != null)
        {
            StoreSku sku = result.Product.Skus.FirstOrDefault(s => s.IsInUserCollection);

            if (sku != null)
            {
                return sku.CollectionData.CampaignId;
            }
        }

        // Try to get the campaign ID from the license data for users without a
        // recognized Microsoft account.
        StoreAppLicense license = await context.GetAppLicenseAsync();
        JsonObject json = JsonObject.Parse(license.ExtendedJsonData);
        if (json.ContainsKey("customPolicyField1"))
        {
            return json["customPolicyField1"].GetString();
        }

        // No campaign ID was found.
        return String.Empty;
    }
    // Fall back to using APIs in the Windows.ApplicationModel.Store namespace instead
    // (the app is running on a device with Windows 10, version 1577, or earlier).
    else
    {
#if DEBUG
        return await Windows.ApplicationModel.Store.CurrentAppSimulator.GetAppPurchaseCampaignIdAsync();
#else
        return await Windows.ApplicationModel.Store.CurrentApp.GetAppPurchaseCampaignIdAsync() ;
#endif
    }
}
```

Ce code procède aux opérations suivantes :

1. Tout d’abord, il vérifie si la classe [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) de l’espace de noms **Windows.Services.Store** est disponible sur l’appareil actuel (cela signifie que l’appareil exécute Windows 10, versions 1607 ou ultérieures). Si tel est le cas, le code utilise cette classe.

2. Il tente ensuite d’obtenir l’ID de campagne personnalisée pour le cas où l’utilisateur actuel dispose d’un compte Microsoft reconnu. Pour ce faire, le code obtient un objet [**StoreSku**](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreSku) qui représente la référence de l’application actuelle, puis il accède à la propriété [**CampaignId**](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.CampaignId) afin de récupérer l’ID de campagne, le cas échéant.
3. Le code tente ensuite de récupérer l’ID de campagne pour le cas où l’utilisateur actuel ne dispose pas d’un compte Microsoft reconnu. Dans ce cas, l’ID de campagne est incorporé à la licence de l’application. Le code récupère la licence à l’aide de la méthode [**GetAppLicenseAsync**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.GetAppLicenseAsync), puis il analyse le contenu JSON de la licence et recherche la valeur d’une clé nommée *customPolicyField1*. Cette valeur contient l’ID de campagne.

4. Si la classe [**StoreContext**](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext) de l’espace de noms **Windows.Services.Store** n’est pas disponible, le code a recours à la méthode [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp#Windows_ApplicationModel_Store_CurrentApp_GetAppPurchaseCampaignIdAsync) dans l’espace de noms **Windows.ApplicationModel.Store** pour récupérer l’ID de campagne personnalisée (cet espace de noms est disponible dans toutes les versions de Windows 10, y compris les versions 1511 et antérieures). Lorsque vous utilisez cette méthode, notez que vous ne pouvez récupérer les ID de campagne personnalisée pour les [acquisitions éligibles](#conversions) que si l’utilisateur dispose d’un compte Microsoft reconnu.

### <a name="specify-the-campaign-id-in-the-proxy-file-for-the-windowsapplicationmodelstore-namespace"></a>Spécifiez l’ID de campagne dans le fichier proxy pour l’espace de noms Windows.ApplicationModel.Store

L’espace de noms **Windows.ApplicationModel.Store** inclut [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator), une classe spéciale qui simule les opérations du Windows Store pour tester votre code avant que vous ne soumettiez votre application au Windows Store. Cette classe récupère les données [d’un fichier local nommé Windows.StoreProxy.xml](../monetize/in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#using-the-windowsstoreproxyxml-file-with-currentappsimulator). L’exemple de code précédent montre comment inclure les classes **CurrentApp** et **CurrentAppSimulator** dans le code de débogage et de non-débogage de votre projet. Pour tester ce code dans un environnement de débogage, ajoutez un élément **AppPurchaseCampaignId** dans le fichier WindowsStoreProxy.xml sur votre ordinateur de développement, comme illustré dans l’exemple suivant. Lorsque vous exécutez l’application, la méthode [**GetAppPurchaseCampaignIdAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentappsimulator.GetAppPurchaseCampaignIdAsync) retourne toujours cette valeur.

``` xml
<CurrentApp>
    ...
    <AppPurchaseCampaignId>your custom campaign ID</AppPurchaseCampaignId>
</CurrentApp>
```

L’espace de noms **Windows.Services.Store** ne fournit pas une classe que vous pouvez utiliser pour simuler des informations de licence lors d’un test. En revanche, vous devez publier une application dans le Windows Store et télécharger cette application sur votre appareil de développement pour utiliser sa licence à des fins de test. Pour plus d’informations, consultez [Versions d’évaluation et achats in-app](../monetize/in-app-purchases-and-trials.md#testing).

## <a name="test-your-custom-campaign"></a>Tester votre campagne personnalisée

Avant de promouvoir une URL de campagne personnalisée, nous vous recommandons de tester celle-ci en procédant comme suit :

1.  Connectez-vous à un compte Microsoft sur l’appareil que vous utilisez pour le test.

2.  Cliquez sur l’URL de votre campagne personnalisée. Assurez-vous que vous accédez à la page de votre application, puis fermez l’application UWP ou la page du navigateur.

3.  Cliquez sur l’URL plusieurs fois pour fermer l’application UWP ou la page du navigateur après chaque visite de la page de votre application. Lors de **l’une** de ces visites de la page de votre application, procédez à l’acquisition de votre application pour générer une conversion. Comptez le nombre total de fois où vous avez cliqué sur l’URL.

4. Vérifiez si les vues de page attendu et les conversions s’affichent dans le **affichages de pages d’application et les conversions par ID de campagne** et **nombre Total de conversions de campagne** graphiques dans le [rapport d’Acquisitions ](acquisitions-report.md)et tester le code de votre application pour vérifier si elle peut récupérer avec succès de l’ID de campagne à l’aide de l’API décrites ci-dessus.
