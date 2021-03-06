---
description: Décrit le schéma de données JSON étendu des produits du Windows Store dans l’espace de noms Windows.Services.Store.
title: Schémas de données des produits du Windows Store
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp, ExtendedJsonData, produits du Microsoft Store, schéma
ms.localizationpriority: medium
ms.openlocfilehash: 77f63ce409a576b3c873d95df0d2e8d0f0933808
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372534"
---
# <a name="data-schemas-for-store-products"></a>Schémas de données des produits du Windows Store

Lorsque vous soumettez un produit (par exemple, une application ou une extension) dans le Windows Store, celui-ci gère un ensemble complet de données pour le produit et ses licences. Dans le code de votre application, vous pouvez accéder par programme à certaines de ces données à l’aide des propriétés de l'espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). Par exemple, vous pouvez récupérer la description et le prix de l’application actuelle ou de son extension à l’aide des propriétés [StoreProduct.Description](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Description) et [StoreProduct.Price](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.Price).

Toutefois, la plupart des données des produits du Windows Store ne sont pas exposées par des propriétés prédéfinies dans l'espace de noms [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). Pour accéder à toutes les données d’un produit dans votre code, vous pouvez utiliser à la place les propriétés générales suivantes :

* [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData)
* [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData)
* [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData)
*   [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData)
*   [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData)
* [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData)
*   [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData)

Ces propriétés renvoient les données complètes de l’objet correspondant sous forme de chaîne au format JSON. Cet article fournit le schéma complet des données renvoyées par ces propriétés.

> [!NOTE]
> Les produits du Windows Store sont organisés selon une hiérarchie d'objets [produit](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct), [référence (SKU)](https://docs.microsoft.com/uwp/api/windows.services.store.storesku) et [disponibilité](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability). Pour plus d’informations, voir [Produits, références et disponibilités](in-app-purchases-and-trials.md#products-skus).

## <a name="schema-for-storeproduct-storesku-storeavailability-and-storecollectiondata"></a>Schéma pour StoreProduct, StoreSku, StoreAvailability et StoreCollectionData

Le schéma suivant décrit la chaîne au format JSON renvoyée par [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData). Les propriétés [StoreSku.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.ExtendedJsonData), [StoreAvailability.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.ExtendedJsonData) et [StoreCollectionData.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storecollectiondata.ExtendedJsonData) renvoient uniquement les parties du schéma qui sont définies sous les chemins d’accès `DisplaySkuAvailabilities\Sku`, `DisplaySkuAvailabilities\Availabilities` et `DisplaySkuAvailabilities\Sku\CollectionData`, respectivement.

Pour obtenir un exemple de chaîne au format JSON renvoyée par [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData), voir [cette section](#product-example).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonData.json#L1-L729)]

<span id="product-example" />

### <a name="example"></a>Exemple

L’exemple suivant montre une chaîne au format JSON renvoyée par la propriété [StoreProduct.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.ExtendedJsonData) d'application.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreProduct.ExtendedJsonDataExample.json#L1-L268)]

## <a name="schema-for-storeapplicense-and-storelicense"></a>Schéma pour StoreAppLicense et StoreLicense

Le schéma suivant décrit la chaîne au format JSON renvoyée par [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData). La propriété [StoreLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.ExtendedJsonData) renvoie uniquement les parties du schéma qui sont définies sous le chemin d’accès `productAddOns`.

Pour obtenir un exemple de chaîne au format JSON renvoyée par [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData), voir [cette section](#license-example).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonData.json#L1-L80)]

<span id="license-example" />

### <a name="example"></a>Exemple

L’exemple suivant montre une chaîne au format JSON renvoyée par la propriété [StoreAppLicense.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storeapplicense.ExtendedJsonData) d'application.

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StoreAppLicense.ExtendedJsonDataExample.json#L1-L28)]

## <a name="schema-for-storepurchaseproperties"></a>Schéma pour StorePurchaseProperties

Le schéma suivant décrit la chaîne au format JSON renvoyée par [StorePurchaseProperties.ExtendedJsonData](https://docs.microsoft.com/uwp/api/windows.services.store.storepurchaseproperties.ExtendedJsonData).

[!code-json[ExtendedJsonDataSchema](./code/InAppPurchasesAndLicenses_RS1/json/StorePurchaseProperties.ExtendedJsonData.json#L1-L12)]

## <a name="related-topics"></a>Rubriques connexes

* [Achats dans l’application et essais](in-app-purchases-and-trials.md)
* [Obtenir des informations sur les produits pour les applications et modules complémentaires](get-product-info-for-apps-and-add-ons.md)
* [Obtenir les informations de licence pour les applications et modules complémentaires](get-license-info-for-apps-and-add-ons.md)
* [Activer les achats dans l’application des applications et des modules complémentaires](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Activer les achats du module complémentaire consommables](enable-consumable-add-on-purchases.md)
* [Implémenter une version d’évaluation de votre application](implement-a-trial-version-of-your-app.md)
