---
title: Rectangles
description: Dans la programmation Direct3D et Windows, les objets à l'écran sont appelés « rectangles englobants ».
ms.assetid: 3B78AE66-2C1A-4191-BDCA-D737E33460BA
keywords:
- Rectangles
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 22aa6da9a26e3bd50fc5ff4fe4272f6da91cdd08
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320994"
---
# <a name="rectangles"></a>Rectangles

Dans la programmation Direct3D et Windows, les objets à l'écran sont appelés « rectangles englobants ». Les côtés d'un rectangle englobant sont toujours parallèles aux côtés de l'écran, ce qui signifie que le rectangle peut être décrit par deux points : l'angle supérieur gauche et l'angle inférieur droit.

## <a name="span-idboundingrectanglesspanspan-idboundingrectanglesspanspan-idboundingrectanglesspanbounding-rectangles"></a><span id="Bounding_rectangles"></span><span id="bounding_rectangles"></span><span id="BOUNDING_RECTANGLES"></span>Rectangles englobants


La plupart des applications utilisent la structure [**RECT**](https://docs.microsoft.com/previous-versions/dd162897(v=vs.85)) (ou un alias typedef’d) pour acheminer les informations relatives à un rectangle englobant à utiliser lors du rendu à l’écran par blitting ou lors de la détection des correspondances. En langage C++, la structure **RECT** a la définition suivante.

```cpp
typedef struct tagRECT { 
    LONG    left;    // This is the upper-left corner x-coordinate.
    LONG    top;     // The upper-left corner y-coordinate.
    LONG    right;   // The lower-right corner x-coordinate.
    LONG    bottom;  // The lower-right corner y-coordinate.
} RECT, *PRECT, NEAR *NPRECT, FAR *LPRECT; 
```

Dans l’exemple précédent, les membres left et top sont les coordonnées x et y de l’angle supérieur gauche d’un rectangle englobant. De la même façon, les membres right et bottom forment les coordonnées de l’angle inférieur droit. L’illustration suivante montre comment vous pouvez visualiser ces valeurs.

![illustration du rectangle délimité par les valeurs left, top, right et bottom](images/rect.png)

La colonne de pixels du bord droit et la ligne de pixels du bord inférieur ne sont pas incluses dans la structure RECT. Par exemple, le résultat du verrouillage d’une structure RECT avec des membres {10, 10, 138, 138} est un objet de 128 pixels de largeur et hauteur.

Dans un souci d’efficacité, de cohérence et de simplicité d’utilisation, toutes les fonctions de présentation Direct3D fonctionnent avec les rectangles.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Systèmes de coordonnées et géométrie](coordinate-systems-and-geometry.md)

 

 




