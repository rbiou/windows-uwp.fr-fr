---
Description: Ce guide permet de rendre votre application compatible pour traiter les données d’entreprise gérées par la stratégie de Protection des informations Windows, ainsi que les données personnelles.
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Guide du développeur sur la Protection des informations Windows
ms.date: 06/21/2017
ms.topic: article
keywords: Windows 10, uwp, wip, Protection des informations Windows, données d’entreprise, protection des données d’entreprise, PDE, applications compatibles
ms.assetid: 913ac957-ea49-43b0-91b3-e0f6ca01ef2c
ms.localizationpriority: medium
ms.openlocfilehash: 6d026c00b1aec4fd8e80b10c0b86c8bd8145f925
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258601"
---
# <a name="windows-information-protection-wip-developer-guide"></a>Guide du développeur sur la Protection des informations Windows

Une application *compatible* fait la distinction entre les données personnelles et d’entreprise et sait lesquelles protéger en fonction des stratégies de Protection des informations Windows définies par l’administrateur.

Dans ce guide, nous allons vous montrer comment créer une stratégie de ce type. Une fois cette stratégie créée, les administrateurs de stratégie pourront faire confiance à votre application pour l’utilisation des données de leur organisation. De plus, les employés apprécieront que vous ayez conservé leurs données personnelles intactes sur leur appareil même s’ils se sont désinscrits de la gestion des périphériques mobiles (GPM) de leur organisation ou s’ils ont quitté totalement l’organisation.

__Remarque__ Ce guide vous aide à rendre compatible une application UWP. Si vous souhaitez rendre compatible une application de bureau Windows C++, consultez le [Guide du développeur sur la Protection des informations Windows (C++)](https://docs.microsoft.com/previous-versions/windows/desktop/EDP/wip-developer-guide?redirectedfrom=MSDN).

Pour en savoir plus sur la stratégie de Protection des informations Windows et les applications compatibles, reportez-vous ici : [Protection des informations Windows](wip-hub.md).

Vous trouverez [ici](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/EnterpriseDataProtection) un exemple complet.

Si vous êtes prêt à réaliser toutes les tâches, commençons.

## <a name="first-gather-what-you-need"></a>Tout d’abord, rassemblez ce dont vous avez besoin.

Éléments requis :

* Une machine virtuelle (VM) de test exécutée sous Windows 10, version 1607 ou ultérieure. Vous allez déboguer votre application par rapport à cette machine virtuelle de test.

* Un ordinateur de développement qui exécute Windows 10, version 1607 ou ultérieure. Il peut s’agir de votre machine virtuelle si vous y avez installé Visual Studio.

## <a name="setup-your-development-environment"></a>Configurer votre environnement de développement

Vous allez effectuer les opérations suivantes :

* [Installer l’Assistant de développement de l’installation de WIP sur votre machine virtuelle de test](#install-assistant)

* [Créer une stratégie de protection à l’aide de l’Assistant Configuration des travaux en cours](#create-protection-policy)

* [Configurer un projet Visual Studio](#setup-vs-project)

* [Configurer le débogage distant](#setup-remote-debugging)

* [Ajouter des espaces de noms à vos fichiers de code](#add-namespaces)

<a id="install-assistant" />

### <a name="install-the-wip-setup-developer-assistant-onto-your-test-vm"></a>Installer l’outil WIP Setup Developer Assistant sur votre machine virtuelle de test

 Utilisez cet outil pour configurer une stratégie de protection des informations Windows (WIP) sur votre machine virtuelle de test.

 Téléchargez l’outil ici : [WIP Setup Developer Assistant](https://www.microsoft.com/store/p/wip-setup-developer-assistant/9nblggh526jf).

<a id="create-protection-policy" />

### <a name="create-a-protection-policy"></a>Créer une stratégie de protection

Définissez votre stratégie en ajoutant des informations dans chaque section de l’outil WIP Setup Developer Assistant. Cliquez sur l’icône d’aide située en regard d’un paramètre pour en savoir plus sur la manière de l’utiliser.

Pour obtenir des recommandations plus générales sur la façon d’utiliser cet outil, voir la section relative aux notes de version sur la page de téléchargement de l’application.

<a id="setup-vs-project" />

### <a name="setup-a-visual-studio-project"></a>Configurer un projet Visual Studio

1. Sur votre ordinateur de développement, ouvrez votre projet.

2. Ajoutez une référence aux extensions mobiles et de bureau pour la plateforme Windows universelle (UWP).

    ![Ajouter des extensions UWP](images/extensions.png)

3. Ajoutez cette fonctionnalité à votre fichier manifeste de package :

    ```xml
       <rescap:Capability Name="enterpriseDataPolicy"/>
    ```
   >*Lecture facultative* : le préfixe « rescap » signifie *fonctionnalité restreinte*. Voir [Fonctionnalités spéciales et restreintes](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations).

4. Ajoutez cet espace de noms à votre fichier manifeste de package :

    ```xml
      xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    ```
5. Ajoutez le préfixe d’espace de noms à l’élément ``<ignorableNamespaces>`` de votre fichier manifeste de package.

    ```xml
        <IgnorableNamespaces="uap mp rescap">
    ```

    Ainsi, si votre application s’exécute sur une version du système d’exploitation Windows qui ne prend pas en charge les fonctionnalités restreintes, Windows ignore la fonctionnalité ``enterpriseDataPolicy``.

<a id="setup-remote-debugging" />

### <a name="setup-remote-debugging"></a>Configurer le débogage à distance

Installez Visual Studio Remote Tools sur votre machine virtuelle de test uniquement si vous développez votre application sur un ordinateur autre que votre machine virtuelle. Ensuite, lancez le débogueur à distance sur votre ordinateur de développement et voyez si votre application s’exécute sur la machine virtuelle de test.

Voir [Instructions pour un PC distant](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

<a id="add-namespaces" />

### <a name="add-these-namespaces-to-your-code-files"></a>Ajouter ces espaces de noms aux fichiers de code

Ajoutez-les à l’aide des instructions dans la partie supérieure de vos fichiers de code (les extraits de code de ce guide les utilisent) :

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Windows.Security.EnterpriseData;
using Windows.Web.Http;
using Windows.Storage.Streams;
using Windows.ApplicationModel.DataTransfer;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml;
using Windows.ApplicationModel.Activation;
using Windows.Web.Http.Filters;
using Windows.Storage;
using Windows.Data.Xml.Dom;
using Windows.Foundation.Metadata;
using Windows.Web.Http.Headers;
```

## <a name="determine-whether-to-use-wip-apis-in-your-app"></a>Déterminer si vous devez utiliser des API WIP dans votre application

Vérifiez que le système d’exploitation qui exécute votre application prend en charge la fonctionnalité WIP, et que cette dernière est activée sur l’appareil.

```csharp
bool use_WIP_APIs = false;

if ((ApiInformation.IsApiContractPresent
    ("Windows.Security.EnterpriseData.EnterpriseDataContract", 3)
    && ProtectionPolicyManager.IsProtectionEnabled))
{
    use_WIP_APIs = true;
}
else
{
    use_WIP_APIs = false;
}
```
N’appelez pas les API WIP si le système d’exploitation ne prend pas en charge la fonctionnalité WIP, ou si cette dernière n’est pas activée sur l’appareil.

## <a name="read-enterprise-data"></a>Lire des données d’entreprise

Pour lire les fichiers protégés, les points de terminaisons réseau, les données du Presse-papiers et les données acceptées à partir d’un contrat de partage, votre application devra solliciter l’accès.

La fonctionnalité de Protection des informations Windows octroie l’autorisation à votre application si cette dernière se trouve sur la liste autorisée de la stratégie de protection.

**Dans cette section :**

* [Lire des données à partir d’un fichier](#read-file)
* [Lire des données à partir d’un point de terminaison réseau](#read-network)
* [Lire les données à partir du presse-papiers](#read-clipboard)
* [Lire les données d’un contrat de partage](#read-share)

<a id="read-file" />

### <a name="read-data-from-a-file"></a>Lire les données d’un fichier

**Étape 1 : obtenir le descripteur de fichier**

```csharp
    Windows.Storage.StorageFolder storageFolder =
        Windows.Storage.ApplicationData.Current.LocalFolder;

    Windows.Storage.StorageFile file =
        await storageFolder.GetFileAsync(fileName);
```

**Étape 2 : déterminer si votre application peut ouvrir le fichier**

Appelez [FileProtectionManager.GetProtectionInfoAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync) afin de déterminer si votre application peut ouvrir le fichier.

```csharp
FileProtectionInfo protectionInfo = await FileProtectionManager.GetProtectionInfoAsync(file);

if ((protectionInfo.Status != FileProtectionStatus.Protected &&
    protectionInfo.Status != FileProtectionStatus.Unprotected))
{
    return false;
}
else if (protectionInfo.Status == FileProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}
```

Si l’élément [FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus) présente la valeur **Protected**, cela signifie que le fichier est protégé et que votre application peut l’ouvrir, car elle se trouve sur la liste autorisée de la stratégie.

Si l’élément [FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus) présente la valeur **UnProtected**, cela signifie que le fichier n’est pas protégé et que votre application peut ouvrir le fichier, quand bien même elle ne se trouverait pas sur la liste autorisée de la stratégie.

> **Interfaces** <br>
[FileProtectionManager.GetProtectionInfoAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync)<br>
[FileProtectionInfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo)<br>
[FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>
[ProtectionPolicyManager.IsIdentityManaged](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)

**Étape 3 : lire le fichier dans un flux ou une mémoire tampon**

*Lire le fichier dans un flux*

```csharp
var stream = await file.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```

*Lire le fichier dans une mémoire tampon*

```csharp
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(file);
```
<a id="read-network" />

### <a name="read-data-from-a-network-endpoint"></a>Lire les données à partir d’un point de terminaison réseau

Créez un contexte de thread protégé pour une lecture à partir d’un point de terminaison d’entreprise.

**Étape 1 : obtenir l’identité du point de terminaison réseau**

```csharp
Uri resourceURI = new Uri("http://contoso.com/stockData.xml");

Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

Si le point de terminaison n’est pas géré par la stratégie, vous obtenez une chaîne vide.

> **Interfaces** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync)


**Étape 2 : créer un contexte de thread protégé**

Si le point de terminaison est géré par la stratégie, créez un contexte de thread protégé. Cela associe les connexions réseau que vous effectuez sur le même thread à l’identité.

Cela vous donne également accès aux ressources réseau d’entreprise qui sont gérées par cette stratégie.

```csharp
if (!string.IsNullOrEmpty(identity))
{
    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        return await GetDataFromNetworkRedirectHelperMethod(resourceURI);
    }
}
else
{
    return await GetDataFromNetworkRedirectHelperMethod(resourceURI);
}
```
Cet exemple joint les appels de socket dans un bloc ``using``. Si vous ne procédez pas ainsi, veillez à fermer le contexte de thread une fois votre ressource récupérée. Voir [ThreadNetworkContext.Close](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.threadnetworkcontext.close).

Ne créez pas de fichiers personnels sur ce thread protégé dans la mesure où ces fichiers sont automatiquement chiffrés.

La méthode [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext) renvoie un objet [**ThreadNetworkContext**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.threadnetworkcontext) que le point de terminaison soit géré ou non par la stratégie. Si votre application gère à la fois des ressources personnelles et des ressources d’entreprise, appelez [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext) pour toutes les identités.  Après avoir récupéré la ressource, supprimez ThreadNetworkContext afin d’effacer tout marquage d’identité sur le thread actuel.

> **Interfaces** <br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)

**Étape 3 : lire la ressource dans une mémoire tampon**

```csharp
private static async Task<IBuffer> GetDataFromNetworkHelperMethod(Uri resourceURI)
{
    HttpClient client;

    client = new HttpClient();

    try { return await client.GetBufferAsync(resourceURI); }

    catch (Exception) { return null; }
}
```

**Facultatif Utiliser un jeton d’en-tête au lieu de créer un contexte de thread protégé**

```csharp
public static async Task<IBuffer> GetDataFromNetworkbyUsingHeader(Uri resourceURI)
{
    HttpClient client;

    Windows.Networking.HostName hostName =
        new Windows.Networking.HostName(resourceURI.Host);

    string identity = await ProtectionPolicyManager.
        GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);

    if (!string.IsNullOrEmpty(identity))
    {
        client = new HttpClient();

        HttpRequestHeaderCollection headerCollection = client.DefaultRequestHeaders;

        headerCollection.Add("X-MS-Windows-HttpClient-EnterpriseId", identity);

        return await GetDataFromNetworkbyUsingHeaderHelperMethod(client, resourceURI);
    }
    else
    {
        client = new HttpClient();
        return await GetDataFromNetworkbyUsingHeaderHelperMethod(client, resourceURI);
    }

}

private static async Task<IBuffer> GetDataFromNetworkbyUsingHeaderHelperMethod(HttpClient client, Uri resourceURI)
{

    try { return await client.GetBufferAsync(resourceURI); }

    catch (Exception) { return null; }
}
```

**Gérer les redirections de page**

Parfois, un serveur web redirige le trafic vers une version plus récente d’une ressource.

Pour gérer cette situation, effectuez des requêtes jusqu’à ce que l’état de la réponse à votre requête ait la valeur **OK**.

Utilisez ensuite l’URI de cette réponse pour obtenir l’identité du point de terminaison. Voici une façon d’effectuer cette opération :

```csharp
private static async Task<IBuffer> GetDataFromNetworkRedirectHelperMethod(Uri resourceURI)
{
    HttpClient client = null;

    HttpBaseProtocolFilter filter = new HttpBaseProtocolFilter();
    filter.AllowAutoRedirect = false;

    client = new HttpClient(filter);

    HttpResponseMessage response = null;

        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Get, resourceURI);
        response = await client.SendRequestAsync(message);

    if (response.StatusCode == HttpStatusCode.MultipleChoices ||
        response.StatusCode == HttpStatusCode.MovedPermanently ||
        response.StatusCode == HttpStatusCode.Found ||
        response.StatusCode == HttpStatusCode.SeeOther ||
        response.StatusCode == HttpStatusCode.NotModified ||
        response.StatusCode == HttpStatusCode.UseProxy ||
        response.StatusCode == HttpStatusCode.TemporaryRedirect ||
        response.StatusCode == HttpStatusCode.PermanentRedirect)
    {
        message = new HttpRequestMessage(HttpMethod.Get, message.RequestUri);
        response = await client.SendRequestAsync(message);

        try { return await response.Content.ReadAsBufferAsync(); }

        catch (Exception) { return null; }
    }
    else
    {
        try { return await response.Content.ReadAsBufferAsync(); }

        catch (Exception) { return null; }
    }
}

```

> **Interfaces** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)<br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

<a id="read-clipboard" />

### <a name="read-data-from-the-clipboard"></a>Lire des données à partir du Presse-papiers

**Obtient l’autorisation d’utiliser les données du presse-papiers**

Pour obtenir des données à partir du Presse-papiers, demandez l’autorisation à Windows. Utilisez [**DataPackageView.RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.requestaccessasync) pour effectuer cette opération.

```csharp
public static async Task PasteText(TextBox textBox)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

        if (result == ProtectionPolicyEvaluationResult..Allowed)
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            textBox.Text = contentsOfClipboard;
        }
    }
}
```

> **Interfaces** <br>
[DataPackageView.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.requestaccessasync)

**Masquer ou désactiver les fonctionnalités qui utilisent les données du presse-papiers**

Déterminez si l’affichage en cours a l’autorisation d’obtenir des données du Presse-papiers.

Si tel n’est pas le cas, vous pouvez désactiver ou masquer les contrôles qui permettent aux utilisateurs de coller des informations à partir du Presse-papiers ou d’afficher un aperçu de son contenu.

```csharp
private bool IsClipboardAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;

    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))

        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed |
        protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.ConsentRequired);
}
```

> **Interfaces** <br>
[ProtectionPolicyEvaluationResult](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicyevaluationresult)<br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

**Empêcher les utilisateurs d’être invités par une boîte de dialogue de consentement**

Un nouveau document n’est pas *personnel* ou d’*entreprise*. Il est tout simplement nouveau. Si un utilisateur colle des données d’entreprise dans celui-ci, Windows applique la stratégie et l’utilisateur est invité via une boîte de dialogue de consentement. Ce code évite que cela ne se produise. Cette opération ne vise pas à protéger les données. Mais davantage à éviter aux utilisateurs de recevoir des boîtes de dialogue de consentement lorsque l’application crée un tout nouvel élément.

```csharp
private async void PasteText(bool isNewEmptyDocument)
{
    DataPackageView dataPackageView = Clipboard.GetContent();

    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
            {
                ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                // add this string to the new item or document here.          

            }
            else
            {
                ProtectionPolicyEvaluationResult result = await dataPackageView.RequestAccessAsync();

                if (result == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string contentsOfClipboard = contentsOfClipboard = await dataPackageView.GetTextAsync();
                    // add this string to the new item or document here.
                }
            }
        }
    }
}
```

> **Interfaces** <br>
[DataPackageView.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datapackageview.requestaccessasync)<br>
[ProtectionPolicyEvaluationResult](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicyevaluationresult)<br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy)

<a id="read-share" />

### <a name="read-data-from-a-share-contract"></a>Lire des données à partir d’un contrat de partage

Lorsque les employés choisissent votre application pour partager leurs informations, votre application ouvre un nouvel élément qui contient ce contenu.

Comme nous l’avons mentionné précédemment, un nouvel élément n’est pas *personnel* ou d’*entreprise*. Il est tout simplement nouveau. Si votre code ajoute un contenu d’entreprise à l’élément, Windows applique la stratégie et l’utilisateur est invité via une boîte de dialogue de consentement. Ce code évite que cela ne se produise.

```csharp
protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isNewEmptyDocument = true;
    string identity = "corp.microsoft.com";

    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            if (isNewEmptyDocument)
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog
                ProtectionPolicyManager.TryApplyProcessUIPolicy(shareOperation.Data.Properties.EnterpriseId);
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.

                ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();

                if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
                {
                    string text = await shareOperation.Data.GetTextAsync();

                    // Do something with that text.
                }
            }
        }
        else
        {
            // If the data has no enterprise identity, then we already have access.
            string text = await shareOperation.Data.GetTextAsync();

            // Do something with that text.
        }

    }

}
```

> **Interfaces** <br>
[ProtectionPolicyManager.RequestAccessAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.requestaccessasync)<br>
[ProtectionPolicyEvaluationResult](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicyevaluationresult)<br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy)

## <a name="protect-enterprise-data"></a>Protéger les données d’entreprise

Protégez les données d’entreprise qui quittent votre application. Les données quittent votre application quand vous les affichez dans une page, les enregistrez dans un fichier ou un point de terminaison réseau ou via un contrat de partage.

**Dans cette section :**

* [Protéger les données qui apparaissent dans les pages](#protect-pages)
* [Protéger des données dans un fichier en tant que processus en arrière-plan](#protect-background)
* [Protéger une partie d’un fichier](#protect-part-file)
* [Lire la partie protégée d’un fichier](#read-protected)
* [Protéger des données dans un dossier](#protect-folder)
* [Protéger les données à un point de terminaison réseau](#protect-network)
* [Protéger les données que votre application partage via un contrat de partage](#protect-share)
* [Protéger les fichiers que vous copiez vers un autre emplacement](#protect-other-location)
* [Protéger les données d’entreprise lorsque l’écran de l’appareil est verrouillé](#protect-locked)

<a id="protect-pages" />

### <a name="protect-data-that-appears-in-pages"></a>Protéger les données qui s’affichent dans les pages

Lorsque vous affichez des données dans une page, signalez à Windows de quel type de données il s’agit (personnel ou entreprise). Pour ce faire, *marquez* la vue actuelle de l’application ou le processus d’application tout entier.

Lorsque vous marquez la vue ou le processus, Windows applique la stratégie sur celui ou celle-ci. Cela permet d’empêcher toute fuite de données résultant d’actions que votre application ne contrôle pas. Par exemple, sur un ordinateur, un utilisateur pourrait utiliser CTRL + V pour copier les informations d’entreprise à partir d’une vue, puis les coller dans une autre application. Windows offre une protection contre ce type d’action. Windows vous permet également d’appliquer des contrats de partage.

**Baliser l’affichage actuel de l’application**

Procédez ainsi si votre application comporte plusieurs vues utilisant pour certaines des données d’entreprise et pour d’autres des données personnelles.

```csharp

// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
ProtectionPolicyManager.GetForCurrentView().Identity = identity;

// tag as personal data.
ProtectionPolicyManager.GetForCurrentView().Identity = String.Empty;
```

> **Interfaces** <br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

**Baliser le processus**

Procédez ainsi si toutes les vues de votre application fonctionnent avec un seul type de données (personnelles ou d’entreprise).

Cela vous empêche d’avoir à gérer des vues marquées de manière indépendante.

```csharp


// tag as enterprise data. "identity" the string that contains the enterprise ID.
// You'd get that from a file, network endpoint, or clipboard data package.
bool result =
            ProtectionPolicyManager.TryApplyProcessUIPolicy(identity);

// tag as personal data.
ProtectionPolicyManager.ClearProcessUIPolicy();
```

> **Interfaces** <br>
[ProtectionPolicyManager.TryApplyProcessUIPolicy](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.tryapplyprocessuipolicy)

<a id="protect-file" />

### <a name="protect-data-to-a-file"></a>Protéger les données dans un fichier

Créez un fichier protégé, puis écrivez dans celui-ci.

**Étape 1 : déterminer si votre application peut créer un fichier d’entreprise**

Votre application peut créer un fichier d’entreprise si la chaîne d’identité est gérée par la stratégie et si votre application se trouve sur la liste des éléments autorisés de cette stratégie.

```csharp
  if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **Interfaces** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)


**Étape 2 : créer le fichier et le protéger à l’identité**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
StorageFile storageFile = await storageFolder.CreateFileAsync("sample.txt",
    CreationCollisionOption.ReplaceExisting);

FileProtectionInfo fileProtectionInfo =
    await FileProtectionManager.ProtectAsync(storageFile, identity);
```

> **Interfaces** <br>
[FileProtectionManager.ProtectAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.protectasync)

**Étape 3 : écrire le flux ou la mémoire tampon dans le fichier**

*Écrire un flux*

```csharp
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

        using (var outputStream = stream.GetOutputStreamAt(0))
        {
            using (var dataWriter = new DataWriter(outputStream))
            {
                dataWriter.WriteString(enterpriseData);
            }
        }

    }
```

*Écrire un tampon*

```csharp
     if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
     {
         var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
             enterpriseData, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

         await FileIO.WriteBufferAsync(storageFile, buffer);

      }
```

> **Interfaces** <br>
[FileProtectionInfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo)<br>
[FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>

<a id="protect-background" />

### <a name="protect-data-to-a-file-as-a-background-process"></a>Protéger les données dans un fichier sous forme de processus d’arrière-plan

Ce code peut s’exécuter lorsque l’écran de l’appareil est verrouillé. Si l’administrateur a configuré une stratégie sécurisée de protection des données d’entreprise verrouillées, Windows supprime les clés de chiffrement nécessaires pour accéder aux ressources protégées à partir de la mémoire de l’appareil. Cela empêche toute fuite de données si l’appareil est perdu. Cette même fonctionnalité supprime également les clés associées aux fichiers protégés lorsque leurs descripteurs sont fermés.

Vous devez utiliser une approche qui maintient le descripteur de fichier ouvert lorsque vous créez un fichier.  

**Étape 1 : déterminer si vous pouvez créer un fichier d’entreprise**

Vous pouvez créer un fichier d’entreprise si l’identité utilisée est gérée par la stratégie et si votre application se trouve sur la liste des éléments autorisés de cette stratégie.

```csharp
if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;
```

> **Interfaces** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)

**Étape 2 : créer un fichier et le protéger à l’identité**

[  **FileProtectionManager.CreateProtectedAndOpenAsync**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync) crée un fichier protégé et garde le descripteur de fichier ouvert lorsque vous écrivez dans celui-ci.

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

ProtectedFileCreateResult protectedFileCreateResult =
    await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
        "sample.txt", identity, CreationCollisionOption.ReplaceExisting);
```

> **Interfaces** <br>
[FileProtectionManager.CreateProtectedAndOpenAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.createprotectedandopenasync)

**Étape 3 : écrire un flux ou une mémoire tampon dans le fichier**

Cet exemple écrit un flux de données dans un fichier.

```csharp
if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
{
    IOutputStream outputStream =
        protectedFileCreateResult.Stream.GetOutputStreamAt(0);

    using (DataWriter writer = new DataWriter(outputStream))
    {
        writer.WriteString(enterpriseData);
        await writer.StoreAsync();
        await writer.FlushAsync();
    }

    outputStream.Dispose();
}
else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
{
    // Perform any special processing for the access suspended case.
}

```

> **Interfaces** <br>
[ProtectedFileCreateResult.ProtectionInfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectedfilecreateresult.protectioninfo)<br>
[FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>
[ProtectedFileCreateResult. Stream](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectedfilecreateresult.stream)<br>

<a id="protect-part-file" />

### <a name="protect-part-of-a-file"></a>Protéger une partie de fichier

Dans la plupart des cas, il est préférable de stocker séparément vos données personnelles et d’entreprise. Toutefois, vous pouvez les stocker dans le même fichier si vous le souhaitez. Par exemple, Microsoft Outlook peut stocker des messages d’entreprise avec des messages personnels dans un fichier d’archive unique.

Chiffrez les données d’entreprise, mais pas la totalité du fichier. De cette façon, les utilisateurs peuvent continuer d’utiliser ce fichier, même s’ils se désinscrivent de la GPM ou si leurs droits d’accès aux données de l’entreprise sont révoqués. En outre, votre application doit suivre les données qu’elle chiffre afin de savoir quelles données protéger lorsqu’elle relit de nouveau le fichier en mémoire.

**Étape 1 : ajouter des données d’entreprise à un flux ou à une mémoire tampon chiffré**

```csharp
string enterpriseDataString = "<employees><employee><name>Bill</name><social>xxx-xxx-xxxx</social></employee></employees>";

var enterpriseData= Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        enterpriseDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);

BufferProtectUnprotectResult result =
   await DataProtectionManager.ProtectAsync(enterpriseData, identity);

enterpriseData= result.Buffer;
```

> **Interfaces** <br>
[DataProtectionManager. ProtectAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectionmanager.protectasync)<br>
[BufferProtectUnprotectResult. buffer](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.bufferprotectunprotectresult.buffer)


**Étape 2 : ajouter des données personnelles à un flux ou à une mémoire tampon non chiffré**

```csharp
string personalDataString = "<recipies><recipe><name>BillsCupCakes</name><cooktime>30</cooktime></recipe></recipies>";

var personalData = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
    personalDataString, Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
```

**Étape 3 : écrire des flux ou des mémoires tampons dans un fichier**

```csharp
StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

StorageFile storageFile = await storageFolder.CreateFileAsync("data.xml",
    CreationCollisionOption.ReplaceExisting);

 // Write both buffers to the file and save the file.

var stream = await storageFile.OpenAsync(FileAccessMode.ReadWrite);

using (var outputStream = stream.GetOutputStreamAt(0))
{
    using (var dataWriter = new DataWriter(outputStream))
    {
        dataWriter.WriteBuffer(enterpriseData);
        dataWriter.WriteBuffer(personalData);

        await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
    }
}
```

**Étape 4 : effectuer le suivi de l’emplacement des données de votre entreprise dans le fichier**

Il incombe à votre application de suivre les données de ce fichier dont l’entreprise est propriétaire.

Vous pouvez stocker ces informations dans une propriété associée au fichier, dans une base de données ou dans le texte d’en-tête du fichier.

Cet exemple enregistre ces informations dans un fichier XML distinct.

```csharp
StorageFile metaDataFile = await storageFolder.CreateFileAsync("metadata.xml",
   CreationCollisionOption.ReplaceExisting);

await Windows.Storage.FileIO.WriteTextAsync
    (metaDataFile, "<EnterpriseDataMarker start='0' end='" + enterpriseData.Length.ToString() +
    "'></EnterpriseDataMarker>");
```
<a id="read-protected" />

### <a name="read-the-protected-part-of-a-file"></a>Lire la partie protégée d’un fichier

Voici comment vous liriez les données d’entreprise issues de ce fichier.

**Étape 1 : obtenir la position des données de votre entreprise dans le fichier**

```csharp
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;

 Windows.Storage.StorageFile metaDataFile =
   await storageFolder.GetFileAsync("metadata.xml");

string metaData = await Windows.Storage.FileIO.ReadTextAsync(metaDataFile);

XmlDocument doc = new XmlDocument();

doc.LoadXml(metaData);

uint startPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("start")).InnerText);

uint endPosition =
    Convert.ToUInt16((doc.FirstChild.Attributes.GetNamedItem("end")).InnerText);
```

**Étape 2 : Ouvrez le fichier de données et assurez-vous qu’il n’est pas protégé**

```csharp
Windows.Storage.StorageFile dataFile =
    await storageFolder.GetFileAsync("data.xml");

FileProtectionInfo protectionInfo =
    await FileProtectionManager.GetProtectionInfoAsync(dataFile);

if (protectionInfo.Status == FileProtectionStatus.Protected)
    return false;
```

> **Interfaces** <br>
[FileProtectionManager.GetProtectionInfoAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.getprotectioninfoasync)<br>
[FileProtectionInfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo)<br>
[FileProtectionStatus](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionstatus)<br>

**Étape 3 : lire les données d’entreprise à partir du fichier**

```csharp
var stream = await dataFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);

stream.Seek(startPosition);

Windows.Storage.Streams.Buffer tempBuffer = new Windows.Storage.Streams.Buffer(50000);

IBuffer enterpriseData = await stream.ReadAsync(tempBuffer, endPosition, InputStreamOptions.None);
```

**Étape 4 : déchiffrer la mémoire tampon qui contient des données d’entreprise**

```csharp
DataProtectionInfo dataProtectionInfo =
   await DataProtectionManager.GetProtectionInfoAsync(enterpriseData);

if (dataProtectionInfo.Status == DataProtectionStatus.Protected)
{
    BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync(enterpriseData);
    enterpriseData = result.Buffer;
}
else if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
{
    // Code goes here to handle this situation. Perhaps, show UI
    // saying that the user's data has been revoked.
}

```

> **Interfaces** <br>
[DataProtectionInfo](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectioninfo)<br>
[DataProtectionManager. GetProtectionInfoAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectionmanager.getstreamprotectioninfoasync)<br>

<a id="protect-folder" />

### <a name="protect-data-to-a-folder"></a>Protéger les données dans un fichier

Vous pouvez créer un dossier et le protéger. De cette façon tous les éléments que vous ajoutez à ce dossier sont automatiquement protégés.

```csharp
private async Task<bool> CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
        return false;
    }
    return true;
}
```

Assurez-vous que le dossier est vide avant de le protéger. Vous ne pouvez pas protéger un dossier contenant déjà des éléments.

> **Interfaces** <br>
[ProtectionPolicyManager.IsIdentityManaged](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.isidentitymanaged)<br>
[FileProtectionManager.ProtectAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.protectasync)<br>
[FileProtectionInfo. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo.identity)<br>
[FileProtectionInfo. Status](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectioninfo.status)

<a id="protect-network" />

### <a name="protect-data-to-a-network-end-point"></a>Protéger les données sur un point de terminaison réseau

Créez un contexte de thread protégé pour envoyer ces données vers un point de terminaison d’entreprise.  

**Étape 1 : obtenir l’identité du point de terminaison réseau**

```csharp
Windows.Networking.HostName hostName =
    new Windows.Networking.HostName(resourceURI.Host);

string identity = await ProtectionPolicyManager.
    GetPrimaryManagedIdentityForNetworkEndpointAsync(hostName);
```

> **Interfaces** <br>
[ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getprimarymanagedidentityfornetworkendpointasync)

**Étape 2 : créer un contexte de thread protégé et envoyer des données au point de terminaison réseau**

```csharp
HttpClient client = null;

if (!string.IsNullOrEmpty(m_EnterpriseId))
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;

    using (ThreadNetworkContext threadNetworkContext =
            ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        client = new HttpClient();
        HttpRequestMessage message = new HttpRequestMessage(HttpMethod.Put, resourceURI);
        message.Content = new HttpStreamContent(dataToWrite);

        HttpResponseMessage response = await client.SendRequestAsync(message);

        if (response.StatusCode == HttpStatusCode.Ok)
            return true;
        else
            return false;
    }
}
else
{
    return false;
}
```

> **Interfaces** <br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)<br>
[ProtectionPolicyManager.CreateCurrentThreadNetworkContext](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.createcurrentthreadnetworkcontext)

<a id="protect-share" />

### <a name="protect-data-that-your-app-shares-through-a-share-contract"></a>Protéger les données que votre application partage via un contrat de partage

Si vous voulez que les utilisateurs partagent le contenu à partir de votre application, vous devez implémenter un contrat de partage et gérer l’événement [**DataTransferManager.DataRequested**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested).

Dans votre gestionnaire d’événements, définissez le contexte de l’identité d’entreprise dans le package de données.

```csharp
private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

> **Interfaces** <br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)

<a id="protect-other-location" />

### <a name="protect-files-that-you-copy-to-another-location"></a>Protéger les fichiers que vous copiez vers un autre emplacement

```csharp
private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

> **Interfaces** <br>
[FileProtectionManager.CopyProtectionAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.fileprotectionmanager.copyprotectionasync)<br>

<a id="protect-locked" />

### <a name="protect-enterprise-data-when-the-screen-of-the-device-is-locked"></a>Protéger les données d’entreprise lorsque l’écran de l’appareil est verrouillé

Supprimez toutes les données sensibles de la mémoire quand l’appareil est verrouillé. Lorsque l’utilisateur déverrouille l’appareil, votre application peut rajouter en toute sécurité ces données.

Gérez l’événement [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending) pour que votre application sache que l’écran est verrouillé. Cet événement est déclenché uniquement si l’administrateur configure une stratégie sécurisée de protection des données d’entreprise verrouillées. Windows supprime temporairement les clés de protection des données approvisionnées sur l’appareil. Windows supprime ces clés pour s’assurer qu’il n’y a aucun accès non autorisé à des données chiffrées, tandis que l’appareil est verrouillé et éventuellement pas en possession de son propriétaire.  

Gérez l’événement [**ProtectionPolicyManager.ProtectedAccessResumed**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed) de sorte que votre application sache que l’écran est déverrouillé. Cet événement est déclenché que l’administrateur ait configuré ou non une stratégie sécurisée de protection des données d’entreprise verrouillées.

#### <a name="remove-sensitive-data-in-memory-when-the-screen-is-locked"></a>Supprimer les données sensibles dans la mémoire quand l’écran est verrouillé

Protégez les données sensibles et fermez les flux de fichiers ouverts par votre application sur les fichiers protégés pour s’assurer que le système ne met pas en cache de données sensibles.

Cet exemple enregistre le contenu d’un élément textblock dans une mémoire tampon chiffrée et supprime le contenu de cet élément.

```csharp
private async void ProtectionPolicyManager_ProtectedAccessSuspending(object sender, ProtectedAccessSuspendingEventArgs e)
{
    Deferral deferral = e.GetDeferral();

    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        IBuffer documentBodyBuffer = CryptographicBuffer.ConvertStringToBinary
           (documentTextBlock.Text, BinaryStringEncoding.Utf8);

        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (documentBodyBuffer, ProtectionPolicyManager.GetForCurrentView().Identity);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            this.protectedDocumentBuffer = result.Buffer;
            documentTextBlock.Text = null;
        }
    }

    // Close any open streams that you are actively working with
    // to make sure that we have no unprotected content in memory.

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    deferral.Complete();
}
```

> **Interfaces** <br>
[ProtectionPolicyManager.ProtectedAccessSuspending](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccesssuspending)<br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)</br>
[DataProtectionManager. ProtectAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectionmanager.protectasync)<br>
[BufferProtectUnprotectResult. buffer](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.bufferprotectunprotectresult.buffer)<br>
[ProtectedAccessSuspendingEventArgs. GetDeferral](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectedaccesssuspendingeventargs.getdeferral)<br>
[Report. complet](https://docs.microsoft.com/uwp/api/windows.foundation.deferral.complete)<br>

#### <a name="add-back-sensitive-data-when-the-device-is-unlocked"></a>Ajouter des données sensibles revenir lorsque l’appareil est déverrouillé

[**ProtectionPolicyManager. ProtectedAccessResumed**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed) est déclenché lorsque l’appareil est déverrouillé et que les clés sont à nouveau disponibles sur l’appareil.

[**ProtectedAccessResumedEventArgs. Identities**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectedaccessresumedeventargs.identities) est un regroupement vide si l’administrateur n’a pas configuré une protection sécurisée des données sous la stratégie de verrouillage.

Cet exemple effectue l’opération inverse de l’exemple précédent. Il déchiffre la mémoire tampon, rajoute des informations à partir de celle-ci dans l’élément textbox, puis supprime la mémoire tampon.

```csharp
private async void ProtectionPolicyManager_ProtectedAccessResumed(object sender, ProtectedAccessResumedEventArgs e)
{
    if (ProtectionPolicyManager.GetForCurrentView().Identity != String.Empty)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.protectedDocumentBuffer);

        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            documentTextBlock.Text = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.protectedDocumentBuffer = null;
        }
    }

}
```

> **Interfaces** <br>
[ProtectionPolicyManager.ProtectedAccessResumed](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedaccessresumed)<br>
[ProtectionPolicyManager.GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.getforcurrentview)<br>
[ProtectionPolicyManager. Identity](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager)</br>
[DataProtectionManager. UnprotectAsync](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.dataprotectionmanager.unprotectasync)<br>
[BufferProtectUnprotectResult. Status](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.bufferprotectunprotectresult)<br>

## <a name="handle-enterprise-data-when-protected-content-is-revoked"></a>Gérer les données d’entreprise lorsque du contenu protégé est révoqué

Si vous voulez que votre application soit avertie lorsque l’appareil est désinscrit de la GPM ou lorsque l’administrateur de la stratégie révoque explicitement l’accès aux données d’entreprise, gérez l’événement [**ProtectionPolicyManager_ProtectedContentRevoked**](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked).

Cet exemple détermine si les données d’une boîte aux lettres d’entreprise pour une application de messagerie ont été révoquées.

```csharp
private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for 'mailIdentity'.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with 'mailIdentity'.
}
```

> **Interfaces** <br>
[ProtectionPolicyManager_ProtectedContentRevoked](https://docs.microsoft.com/uwp/api/windows.security.enterprisedata.protectionpolicymanager.protectedcontentrevoked)<br>

## <a name="related-topics"></a>Rubriques connexes

[Exemple Windows Information Protection (WIP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/EnterpriseDataProtection)
