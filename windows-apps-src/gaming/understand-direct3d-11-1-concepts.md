---
title: Modifications importantes de Direct3D 9 à Direct3D 11
description: Cette rubrique décrit les principales différences entre DirectX 9 et DirectX 11.
ms.assetid: 35a9e388-b25e-2aac-0534-577b15dae364
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jeux, directx,, direct3d 9, direct3d 11, changements
ms.localizationpriority: medium
ms.openlocfilehash: e3e3ecfaee8a99522623ee6b021d8e3a2d78ab85
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367542"
---
# <a name="important-changes-from-direct3d-9-to-direct3d-11"></a>Modifications importantes de Direct3D 9 à Direct3D 11



**Résumé**

-   [Planifier votre port DirectX](plan-your-directx-port.md)
-   Modifications importantes de Direct3D 9 à Direct3D 11
-   [Mappage de fonction](feature-mapping.md)


Cette rubrique décrit les principales différences entre DirectX 9 et DirectX 11.

Direct3D 11 correspond fondamentalement au même type d’API que Direct3D 9 : une interface virtualisée de bas niveau dans du matériel vidéo. Il vous permet encore d’effectuer des opérations de dessin graphique sur une variété d’implémentations matérielles. La disposition de l’API graphique a changé depuis Direct3D 9 ; le concept de contexte de périphérique a été étendu et une API a été ajoutée particulièrement pour l’infrastructure graphique. Les ressources stockées sur le périphérique Direct3D disposent d’un mécanisme original pour le polymorphisme des données appelé affichage des ressources.

## <a name="core-api-functions"></a>Principales fonctions d’API


Dans Direct3D 9, vous deviez créer une interface vers l’API Direct3D avant de pouvoir commencer à l’utiliser. Dans votre jeu de plateforme Windows universelle (UWP) Direct3D 11, vous appelez une fonction statique appelée [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) pour créer le périphérique et le contexte de périphérique.

## <a name="devices-and-device-context"></a>Périphériques et contexte de périphérique


Un périphérique Direct3D 11 représente une carte graphique virtualisée. Il sert à créer des ressources dans la mémoire vidéo, par exemple, à télécharger des textures vers le GPU, à créer des affichages sur les ressources de textures et les chaînes d’échange et à créer des échantillons de textures. Pour obtenir la liste complète des utilisations d’une interface de périphérique Direct3D 11, voir [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device) et [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1).

Un contexte de périphérique Direct3D 11 sert à définir l’état du pipeline et à générer des commandes de rendu. Par exemple, une chaîne de rendu Direct3D 11 utilise un contexte de périphérique pour configurer la chaîne de rendu et dessiner la scène (voir ci-dessous). Le contexte de périphérique est utilisé pour accéder (mapper) la mémoire vidéo utilisée par les ressources de périphérique Direct3D ; il sert également à mettre à jour les données de sous-ressources, par exemple les données de tampons constants. Pour obtenir la liste complète des utilisations du contexte de périphérique Direct3D 11, voir [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) et [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). Notez que la plupart de nos exemples utilisent un contexte immédiat pour générer directement le rendu sur le périphérique, mais que Direct3D 11 prend également en charge des contextes de périphérique différés, lesquels sont principalement utilisés pour le multithreading.

Dans Direct3D 11, le handle de périphérique et le handle de contexte de périphérique sont tous les deux obtenus en appelant la fonction [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). Cette méthode permet également de demander un ensemble spécifique de caractéristiques matérielles et de récupérer des informations sur les niveaux de fonctionnalité Direct3D pris en charge par la carte graphique. Voir [Présentation d’un périphérique dans Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-intro) pour plus d’informations sur les périphériques, les contextes de périphérique et les considérations liées au thread.

## <a name="device-infrastructure-frame-buffers-and-render-target-views"></a>Infrastructure de périphérique, tampons de trame et affichages de cibles de rendu


Dans Direct3D 11, la carte de périphérique et la configuration matérielle sont définies avec l’API DXGI (DirectX Graphics Infrastructure) à l’aide des interfaces COM [**IDXGIAdapter**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) et [**IDXGIDevice1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2). Les tampons et autres ressources de fenêtre (visibles ou hors écran) sont créés et configurés par des interfaces DXGI spécifiques ; l’implémentation du modèle de fabrique [**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) acquiert les ressources DXGI telles que le tampon de trame. Étant donné que DXGI possède la chaîne d’échange, une interface DXGI est utilisée pour présenter les trames à l’écran ; voir [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1).

Utilisez [**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) pour créer une chaîne d’échange compatible avec votre jeu. Vous devez créer une chaîne de permutation pour une fenêtre principale, ou pour une composition (technologie interop XAML), plutôt que de créer une chaîne de permutation pour un HWND.

## <a name="device-resources-and-resource-views"></a>Ressources de périphérique et affichages des ressources


Direct3D 11 prend en charge un niveau supplémentaire de polymorphisme sur les ressources de mémoire vidéo appelées vues. En gros, quand vous aviez un objet Direct3D 9 unique pour une texture, vous avez maintenant deux objets : la ressource de texture, qui contient les données et l’affichage des ressources, qui indique comment la vue est utilisée pour le rendu. Une vue basée sur une ressource permet à cette ressource d’être utilisée dans un but spécifique. Par exemple, une ressource de texture en 2D est créée en tant que [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d), puis un affichage des ressources de nuanceur ([**ID3D11ShaderResourceView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview)) est créé par-dessus pour être utilisable en tant que texture dans un nuanceur. Une vue de cible de rendu ([**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)) peut également être créée sur la même ressource de texture en 2D afin d’être utilisée en tant que surface de dessin. Dans un autre exemple, les mêmes données de pixel sont représentées dans 2 formats de pixel différents à l’aide de 2 affichages distincts sur une seule ressource de texture.

La ressource sous-jacente doit être créée avec des propriétés compatibles avec les types d’affichages qui seront créés à partir de cette ressource. Par exemple, si un [ **ID3D11RenderTargetView** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) est appliqué à une surface, surface est créée avec le [ **D3D11\_lier\_rendu\_ CIBLE** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ne-d3d11-d3d11_bind_flag) indicateur. La surface doit aussi avoir un format de surface d’exposition de DXGI compatible avec le rendu (consultez [ **DXGI\_FORMAT**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)).

La plupart des ressources que vous utilisez pour le rendu héritent de l’interface [**ID3D11Resource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11resource), laquelle hérite de [**ID3D11DeviceChild**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicechild). Les tampons vertex, les tampons d’index, les mémoires tampons constantes et les nuanceurs sont tous des ressources Direct3D 11. Les dispositions d’entrée et les états d’échantillonneur héritent directement de [**ID3D11DeviceChild**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicechild).

Vues de ressources utilisent une DXGI\_FORMAT valeur d’énumération indiquant le format de pixel. Pas chaque D3DFMT est pris en charge comme un DXGI\_FORMAT. Par exemple, il n’existe aucun format de RVB 24bpp dans DXGI équivaut à D3DFMT\_R8G8B8. Il ne sont pas équivalents BGR pour chaque format RVB (DXGI\_FORMAT\_R10G10B10A2\_UNORM est équivalente à D3DFMT\_A2B10G10R10, mais il n’existe aucun équivalent direct à D3DFMT\_A2R10G10B10). Tout contenu présentant l’un de ces formats hérités doit être converti dans un format pris en charge au moment de la création. Pour une liste complète des DXGI formats voir le [ **DXGI\_FORMAT** ](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) énumération.

Les ressources de périphérique Direct3D (et les affichages des ressources) sont créés avant la génération du rendu de la scène. Les contextes de périphérique sont utilisés pour configurer la chaîne de rendu, comme expliqué ci-dessous.

## <a name="device-context-and-the-rendering-chain"></a>Contexte de périphérique et chaîne de rendu


Dans Direct3D 9 et Direct3D 10.x, il existait un seul objet de périphérique Direct3D qui gérait la création des ressources, l’état et le dessin. Dans Direct3D 11, l’interface de périphérique Direct3D gère encore la création des ressources, mais toutes les opérations d’état et de dessin sont gérées à l’aide d’un contexte de périphérique Direct3D. Voici un exemple d’utilisation du contexte de périphérique (interface [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) pour configurer la chaîne de rendu :

-   Définir et effacer les vues de cibles de rendu (et la vue de profondeur/gabarit)
-   Définir le tampon de vertex, le tampon d’index et le schéma d’entrée pour le stade d’assembleur d’entrée (stade IA)
-   Lier les nuanceurs de vertex et de pixels au pipeline
-   Lier les tampons constants aux nuanceurs
-   Lier les vues de textures et les échantillons au nuanceur de pixels
-   Dessiner la scène

Quand l’une des méthodes [**ID3D11DeviceContext::Draw**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) est appelée, la scène est dessinée sur l’affichage de la cible de rendu. Quand vous avez terminé tout le dessin, la carte DXGI est utilisée pour présenter la trame complète en appelant la méthode [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).

## <a name="state-management"></a>Gestion de l’état


Direct3D 9 gérait les paramètres d’état avec un large ensemble de bascules individuelles défini à l’aide des méthodes SetRenderState, SetSamplerState et SetTextureStageState. Étant donné que Direct3D 11 ne prend pas en charge le pipeline à fonction fixe hérité, la méthode SetTextureStageState est remplacée par l’écriture de nuanceurs de pixels. Il n’existe pas d’équivalent à un bloc d’état Direct3D 9. Direct3D 11 gère l’état par le biais de l’utilisation de 4 types d’objets d’état qui offrent un moyen plus rationalisé de regrouper l’état de rendu.

Par exemple, au lieu de l’utilisation de SetRenderState avec D3DRS\_ZENABLE, vous créez un objet DepthStencilState avec ce et autres paramètres d’état et l’utiliser pour modifier l’état lors du rendu.

Quand vous portez des applications Direct3D 9 vers des objets d’état, sachez que vos diverses combinaisons d’état sont représentées sous forme d’objets d’état inaltérables. Ils doivent être créés une seule fois et réutilisés tant qu’ils sont valides.

## <a name="direct3d-feature-levels"></a>Niveaux de fonctionnalités Direct3D


Direct3D possède un nouveau mécanisme pour déterminer la prise en charge matérielle, appelé niveaux de fonctionnalités. Les niveaux de fonctionnalités simplifient la tâche de détermination de ce que peut faire la carte graphique en vous permettant de demander un ensemble bien défini de fonctionnalités GPU. Par exemple, le 9\_implémente au niveau de 1 fonctionnalité les fonctionnalités fournies par les cartes de graphiques Direct3D 9, y compris le nuanceur de modèle 2.x. Depuis le 9\_1 est le plus bas niveau de fonctionnalité, vous pouvez vous attendre à tous les appareils pour prendre en charge d’un nuanceur de sommets et d’un nuanceur de pixels, qui ont été les mêmes étapes pris en charge par le modèle de nuanceur programmables Direct3D 9.

Votre jeu utilise la fonction [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) pour créer le périphérique Direct3D et le contexte de périphérique. Quand vous appelez cette fonction, vous fournissez la liste des niveaux de fonctionnalités que votre jeu peut prendre en charge. Elle renvoie le niveau de fonctionnalité pris en charge le plus élevé à partir de cette liste. Par exemple, si votre jeu peut utiliser des textures de textures BC4/BC5 (une fonctionnalité du matériel de DirectX 10), vous devez inclure au moins 9\_1 et 10\_0 dans la liste des niveaux de fonctionnalité pris en charge. Si le jeu est en cours d’exécution sur du matériel de DirectX 9 et les textures de textures BC4/BC5 ne peut pas être utilisés, puis [ **D3D11CreateDevice** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) retournera 9\_1. Votre jeu peut alors revenir à un format de texture différent (et à des textures plus petites).

Si vous décidez d’étendre votre jeu Direct3D 9 pour prendre en charge des niveaux de fonctionnalités Direct3D supérieurs, alors il est préférable de d’abord terminer le portage de votre code graphique Direct3D 9 existant. Une fois que votre jeu fonctionne dans Direct3D 11, il est plus facile d’ajouter des chemins de rendu supplémentaires avec des graphiques améliorés.

Pour obtenir une explication détaillée de la prise en charge des niveaux de fonctionnalités, voir [Niveaux de fonctionnalités Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro). Voir [Fonctionnalités de Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-features) et [Fonctionnalités de Direct3D 11.1](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-1-features) pour obtenir la liste complète des fonctionnalités de Direct3D 11.

## <a name="feature-levels-and-the-programmable-pipeline"></a>Niveaux de fonctionnalités et pipeline programmable


Le matériel a continué à évoluer depuis Direct3D 9 et plusieurs nouveaux stades facultatifs ont été ajoutés au pipeline graphique programmable. L’ensemble d’options dont vous disposez pour le pipeline graphique varie avec le niveau de fonctionnalité Direct3D. Le niveau de fonctionnalité 10.0 inclut le stade de nuanceur de géométrie avec flux facultatif pour le rendu multipasse sur le GPU. Fonctionnalité niveau 11\_0 incluent le nuanceur de coque et d’un nuanceur de domaine pour une utilisation avec le pavage de matériel. Fonctionnalité niveau 11\_0 inclut également une prise en charge complète pour les nuanceurs de DirectCompute, tandis que les niveaux de fonctionnalité 10.x incluent uniquement prise en charge pour une forme limitée de DirectCompute.

Tous les nuanceurs sont écrits en HLSL à l’aide d’un profil de nuanceur qui correspond à un niveau de fonctionnalité Direct3D. Profils de nuanceur compatibles vers le haut, donc un nuanceur HLSL qui compile à l’aide de Visual Studio\_4\_0\_niveau\_9\_1 ou ps\_4\_0\_niveau\_9\_1 fonctionnera sur tous les appareils. Profils de nuanceur ne sont pas compatible, de niveau inférieur afin d’un nuanceur compilé à l’aide de Visual Studio\_4\_1 ne fonctionneront que sur le niveau de fonctionnalité 10\_1, 11\_0 ou 11\_appareils 1.

Direct3D 9 gérait les constantes des nuanceurs en utilisant un tableau partagé avec SetVertexShaderConstant et SetPixelShaderConstant. Direct3D 11 utilise des tampons constants, lesquels sont des ressources comme un tampon de vertex ou un tampon d’index. Les tampons constants sont conçus pour être mis à jour de manière efficace. Au lieu d’organiser toutes les constantes de nuanceurs dans un seul tableau global, vous les organisez dans des groupes logiques et les gérez via un ou plusieurs tampons constants. Quand vous portez votre jeu Direct3D 9 vers Direct3D 11, envisagez d’organiser vos tampons constants afin de pouvoir les mettre à jour de manière appropriée. Par exemple, regroupez les constantes de nuanceurs dont toutes les trames ne sont pas mises à jour dans une mémoire tampon constante distincte, afin de ne pas avoir à charger constamment ces données sur la carte graphique avec vos constantes de nuanceurs plus dynamiques.

> **Remarque**    plus Direct3D 9 applications ai beaucoup utilisé nuanceurs, mais parfois mixte en cours d’utilisation de l’ancien comportement de fonction fixe. Notez que Direct3D 11 utilise uniquement un modèle de nuanceur programmable. Les fonctionnalités de fonction fixe héritées de Direct3D 9 sont obsolètes.

 

 

 




