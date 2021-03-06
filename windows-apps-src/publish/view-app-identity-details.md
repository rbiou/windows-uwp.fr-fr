---
Description: Affichez les détails relatifs à l’identité unique assignée à votre application par le Microsoft Store et recevez un lien vers la liste des boutiques de votre application.
title: Affichage des détails d’identité de l’application
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 07c2d3308d204d37e246a9a56c0a7203a1340dc0
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210425"
---
# <a name="view-app-identity-details"></a>Affichage des détails d’identité de l’application


Vous pouvez afficher les détails relatifs à l’identité unique assignée à votre application par le Microsoft Store sur ses pages d' **identité d’application** . Vous pouvez également obtenir un lien vers la liste des boutiques de votre application sur cette page.

Pour consulter ces informations, accédez à l’une de vos applications, puis développez l’option **Gestion des applications** dans le menu de navigation gauche. Sélectionnez **Identité de l’application** pour afficher les détails correspondants.


## <a name="values-to-include-in-your-app-package-manifest"></a>Valeurs à inclure dans le manifeste du package de votre application

Les valeurs suivantes doivent être incluses dans le manifeste de votre package. Si vous [utilisez Microsoft Visual Studio pour générer vos packages](/windows/msix/package/packaging-uwp-apps), et que vous êtes connecté avec le même compte Microsoft que celui que vous avez associé à votre compte de développeur, ces informations sont incluses automatiquement. Si vous générez votre package manuellement, vous devrez y ajouter vous-même ces éléments :

-   **Package/identité/nom**
-   **Package/identité/serveur de publication**
-   **Package/propriétés/PublisherDisplayName**

Pour plus d'informations, voir [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) dans la [référence du schéma de manifeste de package](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Utilisés conjointement, ces éléments déclarent l'identité de votre application en établissant la « famille de packages » à laquelle appartiennent tous les packages de l'application. Les différents packages comporteront des détails supplémentaires, comme l'architecture et la version.


## <a name="additional-values-for-package-family"></a>Valeurs supplémentaires concernant la famille de packages

Les valeurs supplémentaires ci-après font référence à la famille de packages de votre application, mais ne figurent pas dans votre manifeste.

-   **Nom de la famille de packages (PFN)** : cette valeur est utilisée avec certaines API Windows.
-   **SID du package** : vous aurez besoin de cette valeur pour envoyer des notifications WNS à votre application. Pour plus d'informations, voir l'article [Vue d'ensemble des services de notifications Push Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).


## <a name="link-to-your-apps-listing"></a>Lien d’accès à la description de votre application

Vous pouvez partager le lien d’accès direct à la page de votre application afin d’aider vos clients à trouver cette dernière dans le Windows Store. Ce lien est au format **`https://www.microsoft.com/store/apps/<your app's Store ID>`** . Lorsqu’un client clique sur ce lien, la page de description web de votre application s’affiche. Sur les appareils Windows, l’application du Windows Store lancera et affichera également la description de votre application.

L’**ID Windows Store** de votre application figure également dans cette section. Cet ID Windows Store peut être utilisé pour [générer des badges Windows Store](https://developer.microsoft.com/store/badges) ou pour identifier votre application.

Le **lien du protocole Windows Store** vous permet de créer un lien d’accès direct à votre application dans le Windows Store sans ouvrir un navigateur, par exemple lorsque vous établissez ce lien à partir d’une application. Pour plus d’informations, consultez l’article [Créer un lien vers votre application](link-to-your-app.md).



 

 




