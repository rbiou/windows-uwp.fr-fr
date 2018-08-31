---
author: mcleanbyron
description: Servez-vous des exemples de code C# présentés dans cette section pour en savoir plus sur la soumission d'options de jeu et de bandes-annonces à l'aide de l’API de soumission au MicrosoftStore.
title: 'Exemple de code C#: soumission d’applications avec options de jeu et bandes-annonces'
ms.author: mcleans
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de soumission au MicrosoftStore, exemples de code, options de jeu, bandes-annonces, listes avancées, C#
ms.localizationpriority: medium
ms.openlocfilehash: 3c13b8f0e65d7ca6715acc5ebcee5ca70605a668
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653128"
---
# <a name="c-sample-app-submission-with-game-options-and-trailers"></a>Exemple de code C\#: soumission d’applications avec options de jeu et bandes-annonces

Cet article fournit des exemples de code C# expliquant l’utilisation de l’[API de soumission au MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes:

* Obtenir un jeton d’accès Azure AD pour l'utiliser avec l'API de soumission au MicrosoftStore.
* Créer une soumission d’applications
* Configurer des données de description dans le Store pour la soumission d’applications, y compris des options de description avancées de [jeux](manage-app-submissions.md#gaming-options-object) et de [bandes-annonces](manage-app-submissions.md#trailer-object).
* Télécharger le fichier ZIP contenant les packages, les images de description et les fichiers de bande-annonce pour la soumission d’applications.
* Valider la soumission d’applications.

Vous pouvez passer en revue chaque exemple pour en savoir plus sur la tâche qu’elle illustre, ou vous pouvez générer tous les exemples de code de cet article dans une application console. Pour générer les exemples, créez une application console C# nommée **DevCenterApiSample** dans Visual Studio, copiez chaque exemple dans un fichier de code distinct dans le projet et générez le projet.

## <a name="prerequisites"></a>Éléments prérequis

La configuration requise pour ces exemples est la suivante:

* Ajoutez une référence à l'assembly System.Web de votre projet.
* Installez le package NuGet [Newtonsoft.Json](http://www.newtonsoft.com/json) de Newtonsoft dans votre projet.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Créer une soumission d’applications

La catégorie ```CreateAndSubmitSubmissionExample``` définit une méthode ```Execute``` publique qui appelle d'autres méthodes d'exemple pour utiliser l'API de soumission au MicrosoftStore afin de créer et de valider une soumission d'applications contenant des options de jeu et une bande-annonce. Adaptez ce code en fonction de vos besoins, comme suit:

* Affectez la variable ```tenantId``` à l’ID de locataire pour votre application et affectez les variables ```clientId``` et ```clientSecret``` à l'ID client et à la clé de votre application. Pour plus d’informations, consultez [Comment associer une application AzureAD à votre compte du Centre de développement Windows](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account)
* Affectez la variable ```applicationId``` à l'[ID Store](in-app-purchases-and-trials.md#store-ids) de l’application pour laquelle vous voulez créer une soumission.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/CreateAndSubmitSubmissionExample.cs#CreateAndSubmitSubmissionExample)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtenir un jeton d’accès AzureAD

La classe ```DevCenterAccessTokenClient``` définit une méthode d'assistance qui utilise vos valeurs ```tenantId```, ```clientId``` et ```clientSecret``` pour créer un jeton d'accès à Azure AD à utiliser avec l'API de soumission au MicrosoftStore.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterAccessTokenClient.cs#DevCenterAccessTokenClient)]

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Méthodes d’assistance pour appeler l'API de soumission et charger les fichiers de soumission

La catégorie ```DevCenterClient``` définit les méthodes d'assistance qui invoquent une variété de méthodes dans l'API de soumission au MicrosoftStore et chargent le fichier .zip contenant les packages, les images de listing et les fichiers de bande-annonce pour la soumission d'applications.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterClient.cs#DevCenterClient)]

## <a name="related-topics"></a>Rubriques associées

* [Créer et gérer des soumissions à l’aide des services du MicrosoftStore](create-and-manage-submissions-using-windows-store-services.md)