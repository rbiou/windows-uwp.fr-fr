---
Description: Lorsque vous créez un module complémentaire dans l’espace partenaires, vous devez spécifier un type de produit et lui affecter un ID de produit.
title: Définir le type et l’ID de produit d’un module complémentaire
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, extensions, iap, durable, consommable, abonnement, type de produit, id produit, achat in-app, produit in-app
ms.localizationpriority: medium
ms.openlocfilehash: a6ef1ca71ffcd7b2d445292bfb38a6a8d29e7a74
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210505"
---
# <a name="set-your-add-on-product-type-and-product-id"></a>Définir le type et l’ID de produit d’un module complémentaire

Un module complémentaire doit être associé à une application que vous avez créée dans l' [espace partenaires](https://partner.microsoft.com/dashboard) (même si vous ne l’avez pas encore envoyée). Recherchez le bouton **Créer un nouveau module complémentaire** sur la page **Vue d’ensemble** ou **Modules complémentaires** de votre application.

Après avoir sélectionné **Créer un nouveau module complémentaire**, vous serez invité à spécifier un type de produit et à attribuer un ID de produit pour votre extension.

## <a name="product-type"></a>Type de produit

Vous devez commencer par indiquer le type de module complémentaire que vous proposez. Cette sélection détermine la façon dont le client pourra utiliser votre module complémentaire.

> [!NOTE]
> Vous ne pourrez pas modifier le type de produit après avoir enregistré cette page pour créer l’extension. Si vous choisissez un type de produit incorrect, vous pouvez toujours supprimer votre soumission d’extension en cours et recommencer en créant une autre extension.

<span id="durable" />

### <a name="durable"></a>Durable

Sélectionnez **Durable** comme type de produit si votre extension n’est généralement achetée qu’une seule fois. Ces extensions servent généralement à déverrouiller des fonctionnalités supplémentaires d’une application.

Par défaut, le champ **Durée de vie du produit** d’un module complémentaire durable affiche **Toujours**, ce qui signifie que ce module n’expire jamais. Vous avez la possibilité de définir le champ **Durée de vie du produit** sur une autre durée à l’étape [Propriétés](enter-add-on-properties.md) du processus de soumission de l’extension. Si vous procédez ainsi, l’extension arrivera à expiration au terme de la durée que vous spécifiez (comprise entre 1 et 365 jours), auquel cas un client pourra la racheter après son expiration.

### <a name="consumable"></a>Consommable

Si l’extension peut être achetée, utilisée (consommée), puis rachetée, vous devez sélectionner l’un des types de produits **consommables**. Les modules complémentaires consommables sont souvent utilisés pour la monnaie d’un jeu par exemple (or, pièces, etc.), qui peuvent être achetés en montants prédéfinis puis dépensés par le client. Pour plus d’informations, voir [Activer les achats d’extensions consommables](../monetize/enable-consumable-add-on-purchases.md).

Il existe deux types d’extensions consommables :
- **Consommable géré par le développeur** : le solde et l’acquisition doivent être gérés dans votre application. Pris en charge dans toutes les versions de système d’exploitation.
- **Consommable géré par le Windows store :** Microsoft assure le suivi du solde sur tous les appareils du client fonctionnant sous Windows 10, version 1607 ou ultérieure ; non pris en charge sur les versions antérieures du système d’exploitation. Pour utiliser cette option, le produit parent doit être compilé à l’aide du Kit de développement logiciel Windows 10 version 14393 ou ultérieure. Notez également que vous ne pouvez pas envoyer un module complémentaire de consommation géré par le magasin au Store tant que le produit parent n’a pas été publié (vous pouvez créer l’envoi dans l’espace partenaires et commencer à l’utiliser à tout moment). Vous devez renseigner la quantité concernant votre extension consommable gérée par le Windows Store à l’étape **Propriétés** de votre soumission.

### <a name="subscription"></a>Abonnement

Si vous souhaitez facturer les clients de manière récurrente pour votre extension, choisissez **Abonnement**.

Une fois qu’un client a acquis une extension d’abonnement, il est facturé à intervalles réguliers afin de continuer à utiliser l’extension. Le client peut annuler l’abonnement à tout moment pour éviter d’autres frais. Vous devez spécifier la période d’abonnement et choisir de proposer ou non un essai gratuit à l’étape **Propriétés** de votre soumission.

Les extensions d’abonnement sont uniquement prises en charge pour les clients qui exécutent Windows 10, version 1607 ou ultérieure. L’application parente doit être compilée à l’aide du SDK Windows 10, version 14393 ou ultérieure, et elle doit utiliser l’API d’achat in-app de l’espace de noms **Windows.Services.Store** en lieu et place de l’espace de noms **Windows.ApplicationModel.Store**. Pour plus d’informations, consultez l’article [Activer les extensions d’abonnement de votre application](../monetize/enable-subscription-add-ons-for-your-app.md).

Vous devez soumettre le produit parent avant de pouvoir publier des modules complémentaires d’abonnement dans le magasin (vous pouvez néanmoins créer l’envoi dans l’espace partenaires et commencer à l’utiliser à tout moment).

## <a name="product-id"></a>Product ID

Quel que soit le type de produit que vous choisissez, vous devez entrer un ID produit unique pour votre extension. Ce nom sera utilisé pour identifier votre module complémentaire dans l’espace partenaires. vous pouvez utiliser cet identificateur pour [faire référence au module complémentaire de votre code](../monetize/in-app-purchases-and-trials.md#how-to-use-product-ids-for-add-ons-in-your-code).

Voici quelques éléments à prendre en considération lors du choix d’un ID produit :

-   Un ID de produit doit être unique dans le produit parent.
-   Vous ne pouvez plus modifier ni supprimer l’ID produit d’un module complémentaire après la publication de ce dernier.
-   Un ID produit ne peut pas comporter plus de 100 caractères.
-   Un ID de produit ne peut pas inclure les caractères suivants : **&lt; &gt; \*% &: \\ ? +,**
-   Les clients ne verront pas l’ID du produit. (Par la suite, vous pourrez entrer un [titre et une description](create-add-on-descriptions.md) qui seront visibles par les clients.)
-   Si votre application précédemment publiée prend en charge Windows Phone 8,1 ou une version antérieure, vous devez utiliser uniquement des caractères alphanumériques, des points et/ou des traits de soulignement dans votre ID de produit. Si vous utilisez d’autres types de caractère, le module complémentaire ne sera pas disponible à l’achat pour les clients utilisant Windows Phone 8.1 ou une version antérieure.

 
