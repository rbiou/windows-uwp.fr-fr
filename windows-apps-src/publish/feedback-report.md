---
Description: Le rapport de commentaires de l’espace partenaires vous permet de voir les problèmes, les suggestions et les votes que vos clients Windows 10 ont soumis via le hub de commentaires.
title: Rapport sur les commentaires
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50ffec5beafd88ddc852e4d9b2fb995fb12bd939
ms.sourcegitcommit: 56d777134bc85f049e281e34660de612ac938a01
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80417435"
---
# <a name="feedback-report"></a>Rapport sur les commentaires

> [!WARNING]
> Désapprobation du rapport de commentaires le 15 avril 2020 ce rapport ne sera plus pris en charge après le 15 avril 2020. Les données de ce rapport ne seront pas actualisées après cette date et le rapport sera supprimé ultérieurement sans préavis. Vous pouvez continuer à afficher les commentaires reçus de vos clients directement dans le hub de commentaires.

Le **rapport de commentaires** de l’espace partenaires vous permet de voir les problèmes, les suggestions et les votes que vos clients Windows 10 ont soumis via le hub de commentaires. Vous pouvez afficher ces données dans l’espace partenaires ou exporter les données pour les afficher hors connexion.

> [!NOTE]
> Vous pouvez également [répondre aux commentaires](respond-to-customer-feedback.md) directement à partir de ce rapport pour faire savoir aux clients que vous êtes à leur écoute.

Inciter vos clients à faire des commentaires sur votre application est un excellent moyen d’en savoir plus sur les problèmes et les fonctionnalités qui sont plus importantes pour eux. Lorsque vos clients savent qu’ils peuvent vous envoyer directement leurs commentaires, ils sont moins susceptibles de le faire par le biais d’un avis négatif dans le Windows Store.

Vous pouvez utiliser l’API de commentaires dans le [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK) pour permettre aux clients de [lancer directement le Hub de commentaires à partir de votre application](../monetize/launch-feedback-hub-from-your-app.md). N’oubliez pas que tout client ayant téléchargé votre application sur un appareil Windows 10 prenant en charge le Hub de commentaires a la possibilité de laisser des commentaires à son sujet à l’aide de cette application. Pour cette raison, vous pouvez voir les commentaires des clients dans ce rapport, même si vous n’avez pas demandé spécifiquement de commentaires à partir de votre application.

Les commentaires peuvent également être utiles lors de l’utilisation de la fonction de [vol de packages](package-flights.md), puisque le rapport de **Commentaires** vous indique le package spécifique que chaque client avait installé sur son appareil lorsqu’il a laissé les commentaires.

> [!TIP]
> Pour consulter rapidement les avis, les évaluations et les commentaires des utilisateurs au cours des 30 derniers jours, développez **engagement** dans le menu de navigation de gauche, puis sélectionnez **critiques et commentaires.** 


## <a name="apply-filters"></a>Appliquer les filtres

Dans la zone supérieure de la page, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La sélection par défaut est **Durée de vie**, mais vous pouvez choisir d’afficher les données portant sur 30 jours ou sur 3, 6 ou 12 mois.

Vous pouvez également développer **Filtres** pour filtrer toutes les données de cette page en fonction des options suivantes.

- **Type d’appareil** : le paramètre par défaut est **Tous**. Vous pouvez sélectionner **Problème** ou **Suggestions** pour n’afficher que ce type de commentaire.
- **Type d'appareil** : le paramètre par défaut est **Tous les appareils**. Vous pouvez choisir un type d’appareil spécifique si vous souhaitez que cette page affiche uniquement les commentaires laissés par les clients qui utilisent ce type d’appareil.
- **Version du package** : le paramètre par défaut est **Tous les packages**. Vous pouvez sélectionner l’un de vos packages pour afficher uniquement les commentaires laissés par les clients ayant utilisé ce package spécifique lorsqu’ils ont laissé leur commentaire.
- **Marché** : la valeur par défaut de ce filtre est **Tous les marchés**. Vous pouvez choisir un marché spécifique pour n’afficher que les commentaires des clients de ce marché.
- **Groupe** : le paramètre par défaut est **Tous**. Vous pouvez choisir d’afficher uniquement les commentaires soumis par les [Windows Insiders](https://insider.windows.com).

> [!TIP]
> Si cette page ne contient aucun commentaire, assurez-vous que vos filtres n’ont pas exclu la totalité des commentaires concernant votre application. Par exemple, si vous filtrez les commentaires en fonction d’un **type d’appareil** non pris en charge par votre application, aucun commentaire n’apparaîtra sur cette page.


## <a name="viewing-feedback-details"></a>Affichage des détails de vos commentaires

Ce rapport vous présente les commentaires individuels laissés par vos clients. À gauche du texte du commentaire de chaque élément s’affiche le nombre de fois où d’autres clients ont voté pour ce commentaire dans l’application Hub de commentaires. Vous pouvez trier le commentaire de trois façons :

- **Votes pour** (par défaut) : affiche les commentaires pour lesquels les autres clients ont voté, en commençant par le commentaire ayant reçu le plus de votes.
- **Fréquents** : affiche les commentaires pour lesquels les autres clients ont voté au cours des sept derniers jours en commençant par le commentaire ayant fait l’objet de l’activité la plus récente.
- **Les plus récents** : montre tous les commentaires en commençant par le commentaire laissé le plus récemment.

La date à laquelle le commentaire a été laissé et le type de commentaire s’affiche en regard de chaque commentaire. Vous verrez également le marché du client, le package spécifique qui a été installé sur l’appareil qu’il utilisait lorsqu’il a quitté les commentaires, le type de cet appareil et **Windows Insider** si le client qui envoie les commentaires est membre du programme Windows Insider.

En outre, une option vous permet de [répondre aux commentaires](respond-to-customer-feedback.md).


## <a name="translating-feedback"></a>Traduction des commentaires

Par défaut, les commentaires qui n’ont pas été écrits dans votre langue par défaut sont traduits pour vous. Si vous le souhaitez, vous pouvez désactiver la traduction des commentaires en décochant la case **Traduire les commentaires** située dans la zone supérieure droite près des filtres de page.

Notez que les commentaires sont traduits par un système de traduction automatique et que le résultat de la traduction n’est pas toujours précis. Le texte d’origine est fourni si vous souhaitez comparer la traduction ou utiliser un autre moyen de traduction.


## <a name="launching-feedback-hub-directly-from-your-app"></a>Lancement du Hub de commentaires directement depuis votre application

Comme indiqué plus haut, nous vous recommandons d’intégrer un lien direct vers le Hub de commentaires directement dans votre application afin d’inciter les clients à envoyer leurs commentaires. Pour plus d’informations, voir [Lancer le Hub de commentaires à partir de votre application](../monetize/launch-feedback-hub-from-your-app.md)
