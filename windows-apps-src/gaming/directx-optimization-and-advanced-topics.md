---
title: Optimisation et rubriques avancées pour les jeux DirectX
description: Optimisation et rubriques avancées pour la programmation de jeux DirectX.
ms.assetid: b5f29fb2-3bcf-43b2-9a68-f8819473bf62
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeu, directx, optimiser, échantillonnage multiple, chaînes d’échange
ms.localizationpriority: medium
ms.openlocfilehash: e9618a35ecd8f9d1a37b627494c0f00a5ed84806
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595014"
---
# <a name="optimization-and-advanced-topics-for-directx-games"></a>Optimisation et rubriques avancées pour les jeux DirectX

Cette section fournit des informations sur l’optimisation des performances de votre jeu DirectX et sur d’autres rubriques avancées.

La rubrique Programmation asynchrone pour les jeux traite des divers points à prendre en compte lorsque vous souhaitez utiliser la programmation asynchrone pour paralléliser certains composants et utiliser le multithreading pour optimiser l’utilisation d’un GPU puissant.

La rubrique Gérer des scénarios de suppression de périphériques dans Direct3D 11 utilise une procédure pas à pas pour expliquer comment les jeux développés à l’aide de Direct3D 11 sont capables de détecter et de répondre à des situations où la carte graphique est réinitialisée, supprimée ou modifiée.

La rubrique Échantillonnage multiple dans les applications UWP fournit une vue d’ensemble sur l’utilisation de l’anticrénelage multi-échantillon, une technique graphique permettant de réduire l’apparence des bords crénelés dans les jeux UWP créés avec Direct3D.

La rubrique Optimiser la boucle d’entrée et de rendu fournit des conseils sur la manière de choisir l’option de traitement d'événement d’entrée appropriée pour gérer la latence d'entrée et optimiser la boucle de rendu.

La rubrique Réduire la latence avec des chaînes d’échange DXGI 1.3 explique comment réduire la latence d’image effective en attendant que la chaîne de permutation signale le moment approprié pour débuter le rendu d’une nouvelle trame.

La rubrique Mise à l’échelle et superpositions de chaînes d’échange explique comment améliorer les temps de rendu à l’aide de chaînes d’échange mises à l’échelle pour un rendu du contenu de jeu en temps réel d'une résolution inférieure aux performances natives de l'écran. Cette rubrique explique également comment créer des superpositions de chaînes d'échange pour les appareils dotés de capacités de surcouche vidéo matérielle ; cette technique peut permettre de résoudre un problème de réduction de l'interface utilisateur à une échelle inférieure suite à une mise à l'échelle des chaînes d'échange.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Rubrique</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="asynchronous-programming-directx-and-cpp.md">Programmation asynchrone pour les jeux</a></p></td>
<td align="left"><p>Comprendre la programmation asynchrone et le threading avec DirectX.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="handling-device-lost-scenarios.md">Gérer les scénarios d’appareil supprimé dans Direct3D 11</a></p></td>
<td align="left"><p>Recréer la chaîne d’interface de périphériques Direct3D et DXGI quand la carte graphique est supprimée ou réinitialisée.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="multisampling--multi-sample-anti-aliasing--in-windows-store-apps.md">L’échantillonnage multiple dans les applications UWP</a></p></td>
<td align="left"><p>Utiliser l’échantillonnage multiple dans les jeux UWP générés à l’aide de Direct3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="optimize-performance-for-windows-store-direct3d-11-apps-with-coredispatcher.md">Optimiser la boucle d’entrée et de rendu</a></p></td>
<td align="left"><p>Réduire la latence d’entrée et optimiser la boucle de rendu.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="reduce-latency-with-dxgi-1-3-swap-chains.md">Réduisez la latence avec des chaînes de permutation DXGI 1.3</a></p></td>
<td align="left"><p>Utiliser DXGI 1.3 pour réduire la latence d’image effective.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="multisampling--scaling--and-overlay-swap-chains.md">Mise à l’échelle de chaîne de permutation et les superpositions</a></p></td>
<td align="left"><p>Créer des chaînes d’échange mises à l’échelle pour accélérer le rendu sur les appareils mobiles, et utilisez la superposition des chaînes d’échange pour améliorer la qualité visuelle.</p></td>
</tr>
</tbody>
</table>