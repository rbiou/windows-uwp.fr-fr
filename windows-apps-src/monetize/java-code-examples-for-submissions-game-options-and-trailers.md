---
author: mcleanbyron
description: "Servez-vous des exemples de code Java présentés dans cette section pour en savoir plus sur la soumission d'options de jeu et de bandes-annonces à l'aide de l’API de soumission au Windows Store."
title: "Exemple de code Java: soumission d’applications avec options de jeu et bandes-annonces"
ms.author: mcleans
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows10, uwp, API de soumission au Windows Store, exemples de code, options de jeu, bandes-annonces, descriptions avancées, java"
ms.openlocfilehash: 7112380e8bad9fc4e2255fd10d06fd86150e92c4
ms.sourcegitcommit: a7a1b41c7dce6d56250ce3113137391d65d9e401
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/11/2017
---
# <a name="java-sample-app-submission-with-game-options-and-trailers"></a>Exemple de code Java: soumission d’applications avec options de jeu et bandes-annonces

Cet article fournit des exemples de code Java qui décrivent comment utiliser l’[API de soumission au Windows Store](create-and-manage-submissions-using-windows-store-services.md) pour les tâches suivantes:

* Obtenir un jeton d’accès Azure AD à utiliser avec l’API de soumission au Windows Store.
* Créer une soumission d’applications
* Configurer des données de description dans le Store pour la soumission d’applications, y compris des options de description avancées de [jeux](manage-app-submissions.md#gaming-options-object) et de [bandes-annonces](manage-app-submissions.md#trailer-object).
* Télécharger le fichier ZIP contenant les packages, les images de description et les fichiers de bande-annonce pour la soumission d’applications.
* Valider la soumission d’applications.

<span id="create-app-submission" />
## <a name="create-an-app-submission"></a>Créer une soumission d’applications

La classe ```CreateAndSubmitSubmissionExample``` implémente un programme ```main``` qui appelle d'autres exemples de méthode pour utiliser l’API de soumission au Windows Store afin de créer et valider une soumission d’applications contenant des options de jeu et une bande-annonce. Adaptez ce code en fonction de vos besoins, comme suit:

* Affectez la variable ```tenantId``` à l’ID de locataire pour votre application et affectez les variables ```clientId``` et ```clientSecret``` à l'ID client et à la clé de votre application. Pour plus d’informations, consultez [Comment associer une application AzureAD à votre compte du Centre de développement Windows](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account)
* Affectez la variable ```applicationId``` à l'[ID Store](in-app-purchases-and-trials.md#store_ids) de l’application pour laquelle vous voulez créer une soumission.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/CreateAndSubmitSubmissionExample.java#L1-L313)]

<span id="token" />
## <a name="obtain-an-azure-ad-access-token"></a>Obtenir un jeton d’accès AzureAD

La classe ```DevCenterAccessTokenClient``` définit une méthode d’assistance qui utilise vos valeurs ```tenantId```, ```clientId``` et ```clientSecret``` pour créer un jeton d’accès AzureAD à utiliser avec l’API de soumission au Windows Store.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterAccessTokenClient.java#L1-L69)]

<span id="utilities" />
## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Méthodes d’assistance pour appeler l’API de soumission et charger les fichiers de soumission

La classe ```DevCenterClient``` définit des méthodes d’assistance qui appellent plusieurs méthodes dans l’API de soumission au Windows Store et chargent le fichier ZIP contenant les packages, les images de description et les fichiers de bande-annonce pour la soumission d’applications.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterClient.java#L1-L224)]

## <a name="related-topics"></a>Rubriques associées

* [Créer et gérer des soumissions à l’aide des services du Windows Store](create-and-manage-submissions-using-windows-store-services.md)