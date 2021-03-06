---
Description: Vos vignettes et notifications toast peuvent charger des chaînes et des images adaptées à la langue d’affichage, au facteur d’échelle de l’affichage, au contraste élevé et à d’autres contextes d’exécution.
title: Prise en charge des vignettes et notifications toast pour la langue, l’échelle et le contraste élevé
template: detail.hbs
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp, ressources, image, MRT, qualificateur
ms.localizationpriority: medium
ms.openlocfilehash: aa6e93196d30c15374129eee7714604cfab7b82e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601474"
---
# <a name="tile-and-toast-notification-support-for-language-scale-and-high-contrast"></a>Prise en charge des vignettes et notifications toast pour la langue, l’échelle et le contraste élevé

Vos vignettes et toasts peuvent charger des chaînes et des images adaptées à la langue, au [facteur d’échelle de l’affichage](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md), au contraste élevé et à d’autres contextes d’exécution. Pour plus d’informations sur l’utilisation des qualificateurs dans les noms de vos fichiers de ressources, consultez [adapter vos ressources de langue, mise à l’échelle et autres qualificateurs](../../../app-resources/tailor-resources-lang-scale-contrast.md) et [icônes d’application et les logos](/windows/uwp/design/style/app-icons-and-logos).

Pour plus d’informations sur la proposition de valeur de la localisation de votre application, voir [Internationalisation et localisation](../../globalizing/globalizing-portal.md).

## <a name="refer-to-a-string-resource-from-a-template"></a>Faire référence à une ressource de chaîne à partir d’un modèle

Dans votre modèle de vignette ou de toast, vous pouvez faire référence à une ressource de chaîne à l’aide du schéma d’URI (Uniform Resource Identifier) `ms-resource`, suivi d’un identificateur de ressource de chaîne simple. Par exemple, si vous avez un fichier Resources.resx contenant une entrée de ressource dont le nom est « Farewell », vous disposez d’une ressource de chaîne avec l’identificateur « Farewell ». Pour plus d’informations sur les identificateurs de ressource de chaîne et les fichiers de ressources (.resw), voir [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../../app-resources/localize-strings-ui-manifest.md).

Voici comment se présente une référence à l’identificateur de ressource de chaîne « Farewell » dans le corps [text](/uwp/schemas/tiles/tilesschema/element-text?branch=live) de votre contenu de modèle, à l’aide de `ms-resource`.

```xml
<text id="1">ms-resource:Farewell</text>
```

Si vous omettez le schéma d’URI `ms-resource`, le corps « text » est juste une chaîne littérale et non *pas* une référence à un identificateur.

```xml
<text id="1">Farewell</text>
```

## <a name="refer-to-an-image-resource-from-a-template"></a>Faire référence à une ressource d’image à partir d’un modèle

Dans votre modèle de vignette ou de toast, vous pouvez faire référence à une ressource d’image à l’aide du schéma d’URI (Uniform Resource Identifier) `ms-appx`, suivi du nom de la ressource d’image. La procédure est la même que celle utilisée pour le référencement d’une ressource d’image dans le balisage XAML (pour plus d’informations, voir [Faire référence à une image ou une autre ressource à partir du code et du balisage XAML](../../../app-resources/images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)).

Par exemple, vos fichiers peuvent être nommés comme suit.

```
\Assets\Images\contrast-standard\welcome.png
\Assets\Images\contrast-high\welcome.png
```

Dans ce cas, vous avez une ressource d’image unique et son nom (en tant que chemin d’accès absolu) est `/Assets/Images/welcome.png`. Voici comment vous utilisez ce nom dans votre modèle.

```xml
<image id="1" src="ms-appx:///Assets/Images/welcome.png"/>
```

Notez comment dans cet exemple d’URI, le schéma (« `ms-appx` »") est suivi de « `://` », lui-même suivi d’un chemin d’accès absolu (un chemin d’accès absolu commence par « `/` »).

## <a name="hosting-and-loading-images-in-the-cloud"></a>Hébergement et chargement d’images dans le cloud

Les schémas d’URI `ms-resource` et `ms-appx` effectuent une mise en correspondance automatique des qualificateurs pour trouver la ressource la plus appropriée pour le contexte actuel. Les schémas d’URI web (par exemple, `http`, `https` et `ftp`) n’effectuent pas cette mise en correspondance automatique.

À la place, ajoutez à l’URI de votre image une chaîne de requête décrivant la ou les valeurs de qualificateur requises.

```xml
<image id="1" src="http://www.contoso.com/Assets/Images/welcome.png?ms-lang=en-US"/>
```

Ensuite, dans le service d’application qui fournit vos images, implémentez un gestionnaire HTTP qui inspecte et utilise la chaîne de requête pour déterminer l’image à renvoyer.

Vous devez également définir l’attribut [**addImageQuery**](/uwp/schemas/tiles/tilesschema/element-visual?branch=live) sur `true` dans la charge utile XML de notification [tile](/uwp/schemas/tiles/tilesschema/schema-root?branch=live) ou [toast](/uwp/schemas/tiles/toastschema/schema-root?branch=live). L’attribut **addImageQuery** s’affiche dans les éléments `visual`, `binding` et `image` des schémas de vignette et de toast. La définition explicite de **addImageQuery** sur un élément remplace toute valeur définie sur un ancêtre. Par exemple, si la valeur de **addImageQuery** est définie sur `true` dans un élément `image`, celle-ci remplace la valeur de **addImageQuery** définie sur `false` dans son élément `binding` parent.

Voici les chaînes de requête que vous pouvez utiliser.

| Qualificateur | Chaîne de requête | Exemple |
| --------- | ------------ | ------- |
| Scale | ms-scale | ?ms-scale=400 |
| Langue | ms-lang | ?ms-lang=en-US |
| Contraste : | ms-contrast | ?ms-contrast=high |

Pour consulter un tableau de référence de toutes les valeurs de qualificateur que vous pouvez utiliser dans vos chaînes de requête, voir [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

## <a name="important-apis"></a>API importantes

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)

## <a name="related-topics"></a>Rubriques connexes

* [Tailles d’écran et les points d’arrêt pour une conception réactive](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Adapter vos ressources de langue, mise à l’échelle et autres qualificateurs](../../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Recommandations en matière de ressources de vignette et d’icône](app-assets.md)
* [Globalisation et localisation](../../globalizing/globalizing-portal.md)
* [Localiser les chaînes dans l’interface utilisateur et le manifeste du package d’application](../../../app-resources/localize-strings-ui-manifest.md)
* [Référencer une image ou un autre actif à partir de code et le balisage XAML](../../../app-resources/images-tailored-for-scale-theme-contrast.md)
* [AddImageQuery](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)
* [Schéma de la vignette](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)
* [Schéma de toast](/uwp/schemas/tiles/toastschema/schema-root?branch=live)
