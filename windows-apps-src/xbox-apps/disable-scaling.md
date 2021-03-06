---
title: Comment désactiver la mise à l’échelle
description: Instructions pour la désactivation du facteur d’échelle par défaut.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6e68c1fc-a407-4c0b-b0f4-e445ccb72ff3
ms.localizationpriority: medium
ms.openlocfilehash: 44688ff40792ba2ee72cbd1d96bae1ac59834efa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604744"
---
# <a name="how-to-turn-off-scaling"></a>Comment désactiver la mise à l’échelle   
Par défaut, les applications sont mises à une échelle de 200 % pour XAML et de 150 % pour les applications HTML. Il est possible de désactiver le facteur d’échelle par défaut. Votre application utilisera ainsi les dimensions en pixels réelles de l’appareil (1910 x 1080 pixels).   
   
## <a name="html"></a>HTML   
Vous pouvez choisir d’annuler le facteur d’échelle à l’aide de l’extrait de code suivant : 
   
```
var result = Windows.UI.ViewManagement.ApplicationViewScaling.trySetDisableLayoutScaling(true);
```

Ou, vous pouvez utiliser une méthode web conviviale :   

```   
@media (max-height: 1080px) {   
    @-ms-viewport {   
        height: 1080px;   
    }   
}   
```

## <a name="xaml"></a>XAML
Vous pouvez choisir d’annuler le facteur d’échelle à l’aide de l’extrait de code suivant :   
   
```
bool result = Windows.UI.ViewManagement.ApplicationViewScaling.TrySetDisableLayoutScaling(true);
```
   
## <a name="directxc"></a>DirectX/C++   
Les applications DirectX/C++ ne sont pas mises à l’échelle. La mise à l’échelle automatique s’applique uniquement aux applications HTML et XAML.  

## <a name="see-also"></a>Voir également
- [Meilleures pratiques pour Xbox](tailoring-for-xbox.md)
- [UWP sur Xbox One](index.md)
