---
Description: Décrit la configuration requise pour déclarer votre application Windows comme accessible dans le Microsoft Store.
ms.assetid: 59FA3B87-75A6-4B30-BA7C-A0E769D68050
title: Accessibilité dans le Windows Store
label: Accessibility in the Store
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad128aca1a633c5ce33830b5ee9231f7a794a60c
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82969674"
---
# <a name="accessibility-in-the-store"></a>Accessibilité dans le Windows Store  



Décrit la configuration requise pour déclarer votre application Windows comme accessible dans le Microsoft Store.

Lorsque vous soumettez votre application au Microsoft Store pour la certification, vous pouvez déclarer votre application comme accessible. La déclaration de votre application comme accessible permet aux utilisateurs qui sont intéressés par les applications accessibles, par exemple les utilisateurs malvoyants, de l’identifier plus facilement. Les utilisateurs découvrent les applications accessibles à l’aide du filtre **accessible** lors de la recherche du Microsoft Store. La déclaration de votre application comme accessible ajoute également la balise **Accessible** à la description de votre application.

En déclarant votre application comme accessible, vous stipulez qu’elle dispose des [informations d’accessibilité de base](basic-accessibility-information.md) dont les utilisateurs ont besoin pour des scénarios essentiels utilisant un ou plusieurs des éléments suivants :

* le clavier ;
* un thème à contraste élevé ;
* un paramètre de points par pouce (ppp) variable ;
* une technologie d’assistance commune telle que les fonctionnalités d’accessibilité Windows, notamment le Narrateur, la Loupe et le Clavier visuel.

Vous devez déclarer votre application comme accessible si vous l’avez créée et testée pour l’accessibilité. Cela signifie que vous avez effectué les opérations suivantes :

* défini toutes les informations d’accessibilité pertinentes pour les éléments d’interface utilisateur, notamment le nom, le rôle, la valeur, etc. ;
* implémenté l’accessibilité totale du clavier, ce qui permet à l’utilisateur :
    * d’accomplir les scénarios d’application principaux en utilisant uniquement le clavier ;
    * navigué à l’aide de la touche Tab entre les éléments de l’interface utilisateur dans un ordre logique ;
    * navigué entre les éléments de l’interface utilisateur dans un contrôle, à l’aide des touches de direction ;
    * d’utiliser des raccourcis clavier pour atteindre les fonctionnalités essentielles de l’application.
    * d’utiliser les mouvements tactiles du Narrateur comme équivalents de la touche Tab et des touches de direction pour les appareils sans clavier.
* vérifié que l’interface utilisateur de votre application est visuellement accessible, qu’elle a un coefficient de contraste de texte minimal de 4,5 pour 1, qu’elle n’utilise pas simplement la couleur pour transmettre des informations, etc. ;
* Utilisation d’outils de test d’accessibilité tels que [**inspecter**](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) et [**UIAVerify**](https://docs.microsoft.com/windows/desktop/WinAuto/ui-automation-verify) pour vérifier votre implémentation de l’accessibilité et résoudre toutes les erreurs de priorité 1 signalées par ces outils.
* vérifié les scénarios principaux de votre application de bout en bout à l’aide du Narrateur, de la Loupe, du Clavier visuel, d’un thème à contraste élevé et de paramètres haute résolution ajustés.

Pour obtenir un examen de cette procédure et des liens vers des ressources qui vous aideront à les appliquer, voir [Liste de vérification de l’accessibilité](accessibility-checklist.md).

<span id="related_topics"/>

## <a name="related-topics"></a>Rubriques connexes    
* [Accessibilité](accessibility.md) 
