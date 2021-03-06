---
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: Servez-vous des exemples de code C# présentés dans cette section pour en savoir plus sur l’utilisation de l’API de soumission au Microsoft Store.
title: "Exemple de code C# : soumissions d'applications, d'extensions et de versions d’évaluation"
ms.date: 08/03/2017
ms.topic: article
keywords: windows 10, uwp, API de soumission au Microsoft Store, exemples de code, C#
ms.localizationpriority: medium
ms.openlocfilehash: b3073e2a5ffa445a39bdf6d54dd288be97c88207
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334967"
---
# <a name="c-sample-submissions-for-apps-add-ons-and-flights"></a>C\# exemple : envois pour les applications, les modules complémentaires et les vols

Cet article fournit des exemples de code C# expliquant l’utilisation de l’[API de soumission au Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes :

* [Créer une soumission de l’application](#create-app-submission)
* [Créer une soumission de module complémentaire](#create-add-on-submission)
* [Mettre à jour une module complémentaire soumission](#update-add-on-submission)
* [Créer une soumission de vol de package](#create-flight-submission)

Vous pouvez passer en revue chaque exemple pour en savoir plus sur la tâche qu’elle illustre, ou vous pouvez générer tous les exemples de code de cet article dans une application console. Pour générer les exemples, créez une application console C# nommée **DeveloperApiCSharpSample** dans Visual Studio, copiez chaque exemple dans un fichier de code distinct dans le projet et générez le projet.

## <a name="prerequisites"></a>Prérequis

Ces exemples utilisent les bibliothèques suivantes :

* Microsoft.WindowsAzure.Storage.dll. Cette bibliothèque est disponible dans le [kit de développement logiciel Microsoft Azure SDK pour .NET](https://azure.microsoft.com/downloads/), ou vous pouvez l’obtenir en installant le [package NuGet WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage).
* Package NuGet [Newtonsoft.Json](https://www.newtonsoft.com/json) de Newtonsoft.

## <a name="main-program"></a>Programme principal

L’exemple suivant implémente un programme de ligne de commande qui appelle les autres exemples de méthode indiqués dans cet article pour illustrer les différentes façons d’utiliser l’API de soumission au Microsoft Store. Adaptez ce programme en fonction de vos besoins, comme suit :

* Affectez les propriétés ```ApplicationId```, ```InAppProductId``` et ```FlightId``` à l’ID de l’application, de l’extension et de la version d’évaluation du package que vous souhaitez gérer.
* Affectez les propriétés ```ClientId``` et ```ClientSecret``` à l’ID client et à la clé de votre application, et remplacez la chaîne *tenantid* dans l’URL ```TokenEndpoint``` par l’ID de locataire pour votre application. Pour plus d’informations, consultez [comment associer une application Azure AD à votre compte espace partenaires](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/Program.cs#Main)]

<span id="clientconfiguration" />

## <a name="clientconfiguration-helper-class"></a>Classe d’assistance ClientConfiguration

L’exemple d’app utilise la classe d’assistance ```ClientConfiguration``` pour passer des données Azure Active Directory et des données d’app à chaque autre exemple de méthode qui utilise l’API de soumission au Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/ClientConfiguration.cs#ClientConfiguration)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

L’exemple suivant implémente une classe qui utilise plusieurs méthodes dans l’API de soumission au Microsoft Store pour mettre à jour une soumission d’app. Le ```RunAppSubmissionUpdateSample``` méthode dans la classe crée une nouvelle soumission comme un clone de la dernière soumission publiée, puis il met à jour et la présentation clonée à Partner Center est validée. Plus précisément, la méthode ```RunAppSubmissionUpdateSample``` effectue les tâches suivantes :

1. Pour commencer, la méthode [récupère les données de l’application indiquée](get-an-app.md).
2. Ensuite, elle [supprime la soumission en attente de l’application](delete-an-app-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour l’application](create-an-app-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Il modifie certains détails de cette soumission, puis charge un nouveau package associé à cette dernière dans le stockage Blob Azure.
5. Ensuite, il [mises à jour](update-an-app-submission.md) , puis [valide](commit-an-app-submission.md) la nouvelle soumission de partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-app-submission.md) jusqu’à ce que celle-ci soit validée.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs#AppSubmissionUpdateSample)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Créer une soumission d’extension

L’exemple suivant implémente une classe qui utilise plusieurs méthodes dans l’API de soumission au Microsoft Store pour créer une soumission d’extension. Plus précisément, la méthode ```RunInAppProductSubmissionCreateSample``` fournie dans cette classe effectue les tâches suivantes :

1. Pour commencer, la méthode [crée une extension](create-an-add-on.md).
2. Ensuite, elle [crée une soumission pour la nouvelle extension](create-an-add-on-submission.md).
3. Elle charge une archive ZIP contenant des icônes associées à la soumission dans le stockage d’objets blob Azure.
4. Ensuite, il [valide la nouvelle soumission pour partenaires](commit-an-add-on-submission.md).
5. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-add-on-submission.md) jusqu’à ce que celle-ci soit validée.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs#InAppProductSubmissionCreateSample)]

<span id="update-add-on-submission" />

## <a name="update-an-add-on-submission"></a>Mettre à jour une soumission d’extension

L’exemple suivant implémente une classe qui utilise plusieurs méthodes dans l’API de soumission au Microsoft Store pour mettre à jour une soumission d’extension existante. Le ```RunInAppProductSubmissionUpdateSample``` méthode dans la classe crée une nouvelle soumission comme un clone de la dernière soumission publiée, puis il met à jour et la présentation clonée à Partner Center est validée. Plus précisément, la méthode ```RunInAppProductSubmissionUpdateSample``` effectue les tâches suivantes :

1. Pour commencer, la méthode [récupère les données de l’extension indiquée](get-an-add-on.md).
2. Ensuite, il [supprime la soumission en attente de l’extension](delete-an-add-on-submission.md), s’il en existe une.
3. Après cela, elle crée [une soumission pour l’extension](create-an-add-on-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
5. Ensuite, il [mises à jour](update-an-add-on-submission.md) , puis [valide](commit-an-add-on-submission.md) la nouvelle soumission de partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-an-add-on-submission.md) jusqu’à ce que celle-ci soit validée.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs#InAppProductSubmissionUpdateSample)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Créer une soumission de version d’évaluation du package

L’exemple suivant implémente une classe qui utilise plusieurs méthodes dans l’API de soumission au Microsoft Store pour mettre à jour une soumission de version d’évaluation de package. Le ```RunFlightSubmissionUpdateSample``` méthode dans la classe crée une nouvelle soumission comme un clone de la dernière soumission publiée, puis il met à jour et la présentation clonée à Partner Center est validée. Plus précisément, la méthode ```RunFlightSubmissionUpdateSample``` effectue les tâches suivantes :

1. Pour commencer, la méthode [récupère les données de la version d’évaluation du package indiquée](get-a-flight.md).
2. Ensuite, il [supprime la soumission en attente de la version d’évaluation du package](delete-a-flight-submission.md), s’il en existe une.
3. Cela fait, il [crée une soumission pour la version d’évaluation du package](create-a-flight-submission.md) (la nouvelle soumission est une copie de la dernière soumission publiée).
4. Elle charge un nouveau package associé à la soumission dans le stockage d’objets blob Azure.
5. Ensuite, il [mises à jour](update-a-flight-submission.md) , puis [valide](commit-a-flight-submission.md) la nouvelle soumission de partenaires.
6. Pour finir, elle [vérifie régulièrement l’état de la nouvelle soumission](get-status-for-a-flight-submission.md) jusqu’à ce que celle-ci soit validée.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs#FlightSubmissionUpdateSample)]

<span id="ingestionclient" />

## <a name="ingestionclient-helper-class"></a>Classe d’assistance IngestionClient

La classe ```IngestionClient``` fournit des méthodes d’assistance qui sont utilisées par d’autres méthodes dans l’exemple d’application pour effectuer les tâches suivantes :

* [Obtenir un jeton d’accès Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que vous pouvez utiliser pour appeler des méthodes dans l’API de soumission au Microsoft Store. Une fois le jeton obtenu, vous avez 60 minutes pour l’utiliser dans les appels à l’API de soumission au Microsoft Store avant expiration. Une fois le jeton arrivé à expiration, vous pouvez en générer un nouveau.
* Charger une archive ZIP contenant de nouvelles ressources pour une soumission d’application ou d’extension dans le stockage Blob Azure. Pour plus d’informations sur le chargement d’une archive ZIP dans le stockage Blob Azure pour les soumissions d’application et d’extension, consultez les instructions correspondantes dans [Créer une soumission d’application](manage-app-submissions.md#create-an-app-submission) et [Créer une soumission d’extension](manage-add-on-submissions.md#create-an-add-on-submission).
* Traiter les demandes HTTP pour l’API de soumission au Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/IngestionClient.cs#IngestionClient)]

## <a name="related-topics"></a>Rubriques connexes

* [Créer et gérer des envois à l’aide des services de Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
