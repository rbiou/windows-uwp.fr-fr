---
title: Graphismes 2D pour jeux DirectX
description: Nous allons découvrir comment utiliser les graphismes et les effets 2D, puis comment vous en servir dans votre jeu.
ms.assetid: ad69e680-d709-83d7-4a4c-7bbfe0766bc7
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, jeux, directx, 2d, graphismes
ms.localizationpriority: medium
ms.openlocfilehash: 7e3a843c00d28d83157cf35a0bd9527be8c6b62f
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367354"
---
# <a name="2d-graphics-for-directx-games"></a>Graphismes 2D pour jeux DirectX



Nous allons découvrir comment utiliser les graphismes et les effets 2D, puis comment vous en servir dans votre jeu.

Les graphiques 2D sont un sous-ensemble des graphiques 3D qui gèrent des primitives ou des bitmaps 2D. Plus généralement, ils n’utilisent pas la coordonnée z comme le ferait un jeu en 3D, puisque l’action est limitée au plan x-y. Ils utilisent parfois des techniques graphiques 3D pour créer leurs composants visuels, et sont en général plus simples à développer. Si vous faites vos premiers pas dans le monde du jeu, un jeu en 2D constitue un excellent point de départ, et le développement de graphiques 2D peut vous permettre de bien vous familiariser avec DirectX.

Vous pouvez développer des graphiques de jeu 2D dans DirectX avec Direct2D ou Direct3D, ou en combinaison. Nombre des classes les plus utiles pour le développement de jeux en 2D se trouvent dans Direct3D, telles que la classe [**Sprite**](https://docs.microsoft.com/windows/desktop/direct3d10/id3dx10sprite). Direct2D est un ensemble d’API qui vise principalement les interfaces utilisateur et les applications nécessitant une prise en charge pour dessiner les primitives (telles que les cercles, les lignes et les formes polygonales plates). Il n’en fournit pas moins un jeu puissant et performant de classes et de méthodes pour créer des graphiques de jeu, notamment les superpositions, les interfaces et les affichages tête haute (HUD) -- ou pour créer une variété de jeux 2D, simples ou raisonnablement détaillés. Néanmoins, l’approche la plus efficace en matière de création de jeux 2D consiste à utiliser des éléments des deux bibliothèques, et c’est donc celle que nous allons utiliser pour le développement de graphiques 2D dans cette rubrique.

## <a name="concepts-at-a-glance"></a>Aperçu rapide des concepts


Avant l’avènement du graphisme 3D moderne et du matériel capable de prendre en charge cette technologie, les jeux étaient principalement en 2D, et nombre de leurs techniques graphiques impliquaient de déplacer des blocs de mémoire (souvent des tableaux de données de couleur à traduire ou transformer en pixels à l’écran dans un format 1:1).

Dans DirectX, les graphiques 2D font partie du pipeline 3D. La variété de résolutions d’écran et de matériels graphiques est aujourd’hui beaucoup plus grande, et votre logiciel graphique 2D doit être capable de les prendre en charge sans changement notable de fidélité.

Voici quelques-uns des concepts de base avec lesquels vous devez vous familiariser lorsque vous commencez à développer des graphiques 2D.

-   Pixels et coordonnées de raster. Un pixel est un point unique sur un écran à balayage, qui possède sa propre paire de coordonnées (x, y) pour indiquer sa position sur l’écran. (Le terme « pixel » est souvent utilisé indifféremment entre les pixels physiques qui composent l’affichage et les éléments de la mémoire adressable utilisés pour contenir les valeurs de couleur et alpha des pixels avant leur envoi à l’affichage.) La trame est traitée par les API comme une grille rectangulaire des éléments de pixel, ce qui a souvent une correspondance 1:1 avec la grille de pixels physiques d’un affichage. Les systèmes de coordonnées de raster commencent dans le coin supérieur gauche de la grille, au pixel de coordonnées (0, 0).
-   Les graphiques bitmaps (parfois appelés graphiques raster) sont des éléments graphiques représentés sous la forme d’une grille rectangulaire de valeurs de pixels. Les sprites, tableaux de pixels calculés gérés indépendamment du raster, sont un type de graphique bitmap, souvent utilisés pour les personnages ou les objets animés et indépendants de l’arrière-plan dans un jeu. Les différentes trames d’animation d’un sprite sont représentées sous forme de collections de bitmaps appelés « feuilles » ou « lots ». Les arrière-plans sont des objets bitmap plus grands de résolution identique ou supérieure à celle du raster d’écran, et servent souvent de toile(s) de fond pour l’action du jeu.
-   Les graphiques vectoriels sont des graphiques qui utilisent des primitives géométriques, comme des points, des lignes, des cercles des polygones pour définir les objets 2D. Ils sont représentés non sous forme de tableaux de pixels, mais comme les équations mathématiques qui les définissent dans un espace 2D. Ils n’ont pas nécessairement de correspondance 1:1 avec la grille de pixels de l’écran, et doivent être convertis du système de coordonnées dans lequel vous les avez rendus, dans le système de coordonnées raster de l’écran.
-   La traduction est la procédure de calcul du nouvel emplacement d’un point ou d’un vertex dans le même système de coordonnées.
-   La mise à l’échelle est la procédure d’élargissement ou de réduction d’un objet en fonction d’un facteur d’échelle spécifié. Avec une image vectorielle, vous réduisez et agrandissez les vertex de ses composants ; avec une bitmap, vous réduisez ou agrandissez les éléments de pixel. Avec des images bitmap, vous perdez des données de pixel lorsque l’image se réduit, et vous agrandissez chaque pixel lorsque l’image est mise à une échelle plus proche. Dans ce dernier cas, vous utilisez des opérations d’interpolation de couleur de pixel, comme le filtrage bilinéaire, pour lisser les bordures de couleur abruptes entre les pixels agrandis.
-   La rotation est la procédure consistant à faire pivoter un objet autour d’un ou plusieurs axes spécifiés. Avec une image vectorielle, les vertex de la géométrie sont multipliés par rapport à une matrice de rotation afin d’obtenir un vertex pivoté ; avec une image bitmap, différents algorithmes peuvent être employés, chacun avec un degré plus ou moins grand de fidélité dans les résultats. Dans la mise à l’échelle comme dans la traduction, il existe des API spécifiquement conçues pour les opérations de rotation.
-   La transformation est la procédure consistant à prendre un point ou un vertex dans un système de coordonnées et à calculer son point ou vertex correspondant dans un autre système de coordonnées. Cela inclut la traduction, la mise à l’échelle et la rotation, ainsi que d’autres opérations de calcul de coordonnées.
-   Le découpage consiste à retirer des parties de bitmaps ou de géométries qui ne se trouvent pas dans la partie visible de l’écran, ou qui sont masquées par des objets de plus grande priorité visuelle.
-   Le tampon de trame est une zone dans la mémoire, souvent dans la mémoire du matériel graphique lui-même, qui contient la carte de raster finale que vous dessinerez à l’écran. La chaîne d’échange est une collection de tampons, où vous dessinez dans une mémoire tampon d’arrière-plan que vous faites passer à l’avant pour l’afficher quand l’image est prête.

## <a name="design-considerations"></a>Considérations relatives à la conception


Le développement de graphiques 2D est un excellent moyen de vous familiariser avec Direct3D. Il vous permet de passer plus de temps sur d’autres aspects très importants du développement de jeu : le son, les commandes et les mécanismes du jeu.

Dessinez toujours dans une mémoire tampon d’arrière-plan. Dessiner directement dans votre tampon de trame signifie que votre image s’affiche à la réception du signal d’affichage (généralement tous les 1/60e de seconde), même si votre opération de dessin n’est pas terminée !

Configurez votre logiciel graphique de sorte qu’il prenne en charge une bonne sélection de résolutions, de 1024x600 à 1920x1080 (ou au-delà). Les utilisateurs de votre jeu vous remercieront si vous prenez en charge la résolution native de leur écran LCD, particulièrement avec les graphiques 2D.

En ce qui concerne les visuels, un esthétisme soigné sera votre meilleur atout. Si vos graphiques bitmap n’ont peut-être pas le punch des visuels photoréalistes 3D qui exploitent les fonctionnalités des modèles de nuanceurs les plus récents, un travail graphique en haute résolution de qualité véhicule souvent plus de style et de personnalité -- en sollicitant beaucoup moins les ressources.

## <a name="reference"></a>Référence


-   [Vue d’ensemble de Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-overview)
-   [Démarrage rapide de Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/getting-started-with-direct2d)
-   [Vue d’ensemble de l’interopérabilité Direct2D et Direct3D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-and-direct3d-interoperation-overview)
