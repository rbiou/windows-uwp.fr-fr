---
title: Redimensionnement d’un pool de vignettes
description: Redimensionnez un pool de vignettes pour l’augmenter si l’application nécessite plus de plages de travail pour les ressources de diffusion en continu mappées ou le réduire si moins d’espace est nécessaire.
ms.assetid: A54A06DC-BDDB-42DC-85E8-C64241100ED5
keywords:
- Redimensionnement d’un pool de vignettes
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7e08447c575e99178e503e99eb651cd5e225a898
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607774"
---
# <a name="tile-pool-resizing"></a>Redimensionnement d’un pool de vignettes


Redimensionnez un pool de vignettes pour l’augmenter si l’application nécessite plus de plages de travail pour les ressources de diffusion en continu mappées ou le réduire si moins d’espace est nécessaire. Les applications peuvent également allouer des pools de tuiles supplémentaires aux nouvelles ressources de diffusion en continu. Cependant, si une simple ressource de diffusion en continu nécessite plus d’espace disponible qu’auparavant dans son pool de tuiles, l’augmentation du pool de tuiles est une bonne option. Une ressource de diffusion en continu ne peut pas avoir de mappages dans plusieurs pools de tuiles en même temps.

Lorsqu’un pool de tuiles est agrandi, des tuiles supplémentaires sont ajoutées à la fin par le pilote d’affichage via une ou plusieurs nouvelles allocations. Cette répartition en allocations n’est pas visible pour l’application. La mémoire existante dans ce pool de tuiles est conservée et les mappages de ressources de diffusion en continu existants dans la mémoire restent intactes.

Lorsqu’un pool de tuiles est réduit, les tuiles sont supprimées de la fin du pool. Les tuiles sont supprimées même si leur taille est inférieure à la taille de l’allocation initiale, jusqu’à 0, ce qui signifie que les nouveaux mappages ne peuvent pas être définis au-delà de cette nouvelle taille. Toutefois, les mappages existants après la fin du redimensionnement restent intacts et utilisables. Le pilote d’affichage conservera la mémoire tant que les mappages de certaines allocations utilisées par le pilote pour le pool de tuiles sont conservés. Si après une réduction, la mémoire a été conservée car des mappages de tuiles y sont liés, et que le pool de tuiles est de nouveau agrandi (quel que soit le volume), la mémoire existante est d’abord réutilisée avant d’ajouter des allocations supplémentaires permettant l’opération d’agrandissement.

Pour économiser de la mémoire, une application doit non seulement réduire un pool de tuiles, mais également supprimer/remapper les mappages existants à la fin de la réduction du pool de tuiles.

L’opération de réduction (et la suppression des mappages) ne produit pas nécessairement des économies de mémoire immédiates. La libération d’espace dans la mémoire dépend du niveau de granularité des allocations sous-jacentes du pilote d’affichage pour le pool de tuiles. Lorsqu’une réduction est suffisante pour que des allocations de pilote d’affichage se retrouvent inutilisées, le pilote d’affichage peut libérer de l’espace. Si un pool de tuiles s’est agrandi, la réduction à la taille précédente (et la suppression/le remappage des mappages de tuiles associés) est conseillée pour engendrer des économies de mémoire, même si celles-ci ne sont pas garanties dans le cas où les tailles ne s’alignent pas exactement avec la taille des allocations sous-jacentes choisies par le pilote d’affichage.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Les mappages sont dans un pool de vignette](mappings-are-into-a-tile-pool.md)

 

 




