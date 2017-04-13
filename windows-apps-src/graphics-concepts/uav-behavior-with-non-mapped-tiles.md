---
title: "Comportement de l’UAV avec des vignettes non mappées"
description: "Le comportement des lectures et écritures d&quot;un accès sans ordre (UAV) varie selon le niveau de prise en charge du matériel."
ms.assetid: CDB224E2-CC07-4568-9AAC-C8DC74536561
keywords:
- "Comportement de l’UAV avec des vignettes non mappées"
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ecaeca63e6d3d9d9a7e40282b9c4c16b5d24a179
ms.lasthandoff: 02/07/2017

---

# <a name="span-iddirect3dconceptsuavbehaviorwithnon-mappedtilesspanuav-behavior-with-non-mapped-tiles"></a><span id="direct3dconcepts.uav_behavior_with_non-mapped_tiles"></span>Comportement de l’UAV avec des vignettes non mappées


Le comportement des lectures et écritures d'un accès sans ordre (UAV) varie selon le niveau de prise en charge du matériel. Pour une analyse des conditions requises, reportez-vous au comportement global des lectures et écritures pour [Niveaux de fonctionnalité des ressources de diffusion en continu](streaming-resources-features-tiers.md). Cette section résume le comportement idéal.

Les opérations de nuanceur qui lisent à partir d’une vignette non mappée dans un UAV renvoient 0 dans tous les composants non manquants du format, ainsi que la valeur par défaut pour les composants manquants.

Lorsque des opérations de nuanceur tentent d'écrire sur une vignette non mappée, rien ne s'écrit dans la zone non mappée (alors que les écritures dans une zone mappée ont lieu). Cette définition idéale pour le traitement de l'écriture n’est pas requise par le [Niveau 2](tier-2.md) ; les écritures vers les vignettes non mappées peuvent se retrouver dans un cache qui peut être récupéré par les lectures suivantes.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Accès du pipeline aux ressources de diffusion en continu](pipeline-access-to-streaming-resources.md)

 

 




