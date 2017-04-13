---
title: Affichages
description: "Le terme \\ 0034;affichage \\ 0034; est utilisé pour désigner des \\ 0034;données au format requis \\ 0034;. Par exemple, un affichage des mémoires tampons de constantes (CBV) correspond à des données correctement formatées de mémoire tampon de constante. Cette section décrit les affichages les plus courants et les plus utiles."
ms.assetid: 0C7FB99F-7391-472F-BA53-576888DFC171
keywords:
- Affichages
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: beaed3bbaf6cab56c826788d0361b1200c73a003
ms.lasthandoff: 02/07/2017

---

# <a name="views"></a>Affichages


Le terme « affichage » est utilisé pour désigner des « données au format requis ». Par exemple, un affichage des mémoires tampons de constantes (CBV) correspond à des données correctement formatées de mémoire tampon de constante. Cette section décrit les affichages les plus courants et les plus utiles.

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>Dans cette section


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Article</th>
<th align="left">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Affichage des mémoires tampons de constantes (CBV)](constant-buffer-view--cbv-.md)</p></td>
<td align="left"><p>Les mémoires tampons contiennent des données constantes de nuanceur. Leur intérêt consiste à rendre les données persistantes et accessibles depuis n'importe quel nuanceur GPU jusqu'à ce qu’il soit nécessaire de les modifier.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Affichage d'une mémoire tampon de vertex (VBV) et d’index (IBV)](vertex-buffer-view--vbv-.md)</p></td>
<td align="left"><p>La mémoire tampon de vertex conserve les données d'une liste de sommets. Les données de chaque vertex sont notamment la position, la couleur, le vecteur normal, les coordonnées de texture, etc. La mémoire tampon d’index conserve des index entiers (décalages) dans une mémoire tampon de vertex et permet de définir et rendre un objet constitué d’un sous-ensemble de la liste complète des sommets.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Affichage des ressources de nuanceur (SRV) et des accès sans ordre (UAV)](shader-resource-view--srv-.md)</p></td>
<td align="left"><p>Les vues de ressource de nuanceur habillent en général des textures sous un format accessible aux nuanceurs. Un affichage des accès sans ordre offre une fonctionnalité similaire, mais permet la lecture et l'écriture sur la texture (ou sur d’autres ressources) dans n’importe quel ordre.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Échantillonneur](sampler.md)</p></td>
<td align="left"><p>L’échantillonnage est le processus consistant à lire des valeurs d’entrée à partir d’une texture ou d'une autre ressource. Un &quot;échantillonneur&quot; est un objet capable de lire depuis des ressources.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Affichage de la cible du rendu (RTV)](render-target-view--rtv-.md)</p></td>
<td align="left"><p>Les cibles de rendu permettent d'afficher une scène dans une mémoire tampon intermédiaire temporaire, au lieu d'afficher la mémoire tampon d'arrière-plan à l'écran. Cette fonctionnalité permet d’utiliser des scènes complexes qui peuvent être affichées, en tant que texture de réflexion ou à d’autres fins dans le pipeline graphique ou, par exemple, pour ajouter des effets de nuanceur de pixels supplémentaires à la scène avant le rendu.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Affichage du gabarit de profondeur (DSV)](depth-stencil-view--dsv-.md)</p></td>
<td align="left"><p>Un affichage de gabarit de profondeur fournit le format et la mémoire tampon permettant de contenir les informations de profondeur et de gabarit. Le tampon de profondeur sert à supprimer le dessin de pixels qui seraient invisibles pour l’observateur puisqu'ils sont masqués par un objet plus proche. Le tampon de gabarit peut servir à éliminer tout le dessin en dehors d’une forme définie.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Affichage de la sortie du flux (SOV)](stream-output-view--sov-.md)</p></td>
<td align="left"><p>Les affichages de sortie de flux activent les informations de vertex fournies par les nuanceurs de vertex, de pavage et de géométrie pour les rediffuser vers l’application pour utilisation ultérieure. Par exemple, un objet qui a été déformé par ces nuanceurs pourra être réécrit sur l’application pour offrir une saisie plus précise dans un moteur physique ou autre. Dans la pratique, toutefois, les affichages de sortie de flux sont une fonctionnalité du pipeline graphique rarement utilisée.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Affichage ordonné du rastériseur (ROV)](rasterizer-ordered-view--rov-.md)</p></td>
<td align="left"><p>Les affichages ordonnés du rastériseur permettent de traiter certaines limitations associées aux tampons de profondeur, en particulier le fait d'avoir plusieurs textures contenant de la transparence, toutes s'appliquant aux mêmes pixels.</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Guide d’apprentissage des graphiques Direct3D](index.md)

 

 




