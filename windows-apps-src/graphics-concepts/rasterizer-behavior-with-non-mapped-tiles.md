---
title: Comportement du rastériseur avec les vignettes non mappées
description: Cette section décrit le comportement du rastériseur avec les vignettes non mappées.
ms.assetid: AC7B818D-E52B-4727-AEA2-39CFDC279CE7
keywords:
- Comportement du rastériseur avec les vignettes non mappées
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e3089444820f990644526eaafb7f2ef9007fa70a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57631884"
---
# <a name="span-iddirect3dconceptsrasterizerbehaviorwithnon-mappedtilesspanrasterizer-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.rasterizer_behavior_with_non-mapped_tiles"></span>Comportement du rastériseur avec des vignettes non mappés


Cette section décrit le comportement du rastériseur avec les vignettes non mappées.

## <a name="span-iddepthstencilviewspanspan-iddepthstencilviewspanspan-iddepthstencilviewspandepthstencilview"></a><span id="DepthStencilView"></span><span id="depthstencilview"></span><span id="DEPTHSTENCILVIEW"></span>DepthStencilView


Le comportement des lectures et écritures de la vue de profondeur/gabarit varie en fonction du niveau de prise en charge matérielle. Pour une analyse des conditions requises, reportez-vous au comportement global des lectures et écritures pour [Niveaux de fonctionnalité des ressources de diffusion en continu](streaming-resources-features-tiers.md).

Le comportement idéal est le suivant :

Si une vignette n’est pas mappée dans DepthStencilView, la valeur renvoyée par la lecture de la profondeur est 0. Cette valeur est ensuite utilisée dans toutes les opérations configurées pour la valeur de lecture de profondeur. Les écritures vers la vignette de profondeur manquante sont ignorées. Cette définition idéale pour le traitement de l'écriture n’est pas requise par le [Niveau 2](tier-2.md) ; les écritures vers les vignettes non mappées peuvent se retrouver dans un cache qui peut être récupéré par les lectures suivantes.

## <a name="span-idrendertargetviewspanspan-idrendertargetviewspanspan-idrendertargetviewspanrendertargetview"></a><span id="RenderTargetView"></span><span id="rendertargetview"></span><span id="RENDERTARGETVIEW"></span>RenderTargetView


Le comportement des lectures et écritures de la vue de cible de rendu varie selon le niveau de prise en charge matérielle. Pour une analyse des conditions requises, reportez-vous au comportement global des lectures et écritures pour [Niveaux de fonctionnalité des ressources de diffusion en continu](streaming-resources-features-tiers.md).

Dans toutes les implémentations, les différentes vues de cible de rendu (et vues de profondeur/gabarit) liées simultanément peuvent être dotées de différentes zones mappées ou non mappées et peuvent avoir des formats de surface de différentes tailles (ce qui implique des vignettes de différentes formes).

Le comportement idéal est le suivant :

Les lectures des vues de cible de rendu renvoient 0 dans les vignettes manquantes et les écritures sont ignorées. Cette définition idéale pour le traitement de l'écriture n’est pas requise par le [Niveau 2](tier-2.md) ; les écritures vers les vignettes non mappées peuvent se retrouver dans un cache qui peut être récupéré par les lectures suivantes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Accès de pipeline pour la diffusion en continu de ressources](pipeline-access-to-streaming-resources.md)

 

 




