---
title: Fonctionnalités d’échantillonnage de texture des ressources de diffusion en continu
description: Les fonctionnalités d'échantillonnage de texture des ressources de diffusion en continu incluent l'obtention des commentaires sur l'état du nuanceur concernant les zones mappées, la possibilité de vérifier que toutes les données auxquelles vous accédez ont été mappées dans la ressource, l'utilisation du déplacement de couleur pour aider les nuanceurs à éviter les zones mipmappées dans les ressources de diffusion en continu connues pour ne pas être mappées, et la détection de la LOD minimale mappée entièrement pour un encombrement de filtre de texture entier.
ms.assetid: C2B2DD69-8354-417A-894D-6235A8B48B53
keywords:
- Fonctionnalités d’échantillonnage de texture des ressources de diffusion en continu
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: eb0e870aa467641f82d24f03278a199ab56d0c8d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370974"
---
# <a name="streaming-resources-texture-sampling-features"></a>Fonctionnalités d’échantillonnage de texture des ressources de diffusion en continu


Les fonctionnalités d'échantillonnage de texture des ressources de diffusion en continu incluent l'obtention des commentaires sur l'état du nuanceur concernant les zones mappées, la possibilité de vérifier que toutes les données auxquelles vous accédez ont été mappées dans la ressource, l'utilisation du déplacement de couleur pour aider les nuanceurs à éviter les zones mipmappées dans les ressources de diffusion en continu connues pour ne pas être mappées, et la détection de la LOD minimale mappée entièrement pour un encombrement de filtre de texture entier.

## <a name="span-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanspan-idrequirementsofstreamingresourcestexturesamplingfeaturesspanrequirements-of-streaming-resources-texture-sampling-features"></a><span id="Requirements_of_streaming_resources_texture_sampling_features"></span><span id="requirements_of_streaming_resources_texture_sampling_features"></span><span id="REQUIREMENTS_OF_STREAMING_RESOURCES_TEXTURE_SAMPLING_FEATURES"></span>Les fonctionnalités d’échantillonnage de texture des exigences de diffusion en continu de ressources


Les fonctionnalités d’échantillonnage de texture décrites ici requièrent le [niveau 2](tier-2.md) de prise en charge des ressources de diffusion en continu.

## <a name="span-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanspan-idshaderstatusfeedbackaboutmappedareasspanshader-status-feedback-about-mapped-areas"></a><span id="Shader_status_feedback_about_mapped_areas"></span><span id="shader_status_feedback_about_mapped_areas"></span><span id="SHADER_STATUS_FEEDBACK_ABOUT_MAPPED_AREAS"></span>Commentaires d’état de nuanceur sur des domaines mappés


Les instructions de nuanceur qui agissent en lecture et/ou en écriture sur une ressource de diffusion en continu entraînent un enregistrement des informations d'état. Cet état est présenté en tant que valeur de retour supplémentaire facultative sur chaque instruction d'accès aux ressources située dans un registre temp 32 bits. Les contenus de la valeur de retour sont opaques. Autrement dit, la lecture directe par le programme de nuanceur n’est pas autorisée. Mais vous pouvez utiliser la fonction [**CheckAccessFullyMapped**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/checkaccessfullymapped) pour extraire les informations d’état.

## <a name="span-idfullymappedcheckspanspan-idfullymappedcheckspanspan-idfullymappedcheckspanfully-mapped-check"></a><span id="Fully_mapped_check"></span><span id="fully_mapped_check"></span><span id="FULLY_MAPPED_CHECK"></span>Vérification entièrement mappée


La fonction [**CheckAccessFullyMapped**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/checkaccessfullymapped) interprète l’état renvoyé à partir d’un accès à la mémoire et indique si toutes les données auxquelles vous accédez ont été mappées dans la ressource. **CheckAccessFullyMapped** renvoie la valeur true (0xFFFFFFFF) si les données a été mappées ou false (0x00000000) si les données n'ont pas été mappées.

Lors des opérations de filtrage, il arrive parfois que le poids d’un texel donné soit égal à 0.0. Un exemple est un exemple linéaire avec les coordonnées de texture qui se trouvent directement sur un centre de texel : 3 autres texels (ils sont celles qui peut varier par matériel) contribuent au filtre, mais avec un poids de 0. Ces texels d'un poids de 0 n'ont aucune influence sur le résultat du filtre, donc s'ils tombent sur les vignettes **NULL**, ils ne sont pas considérés comme un accès non mappé. Notez que la même garantie s’applique aux filtres de texture qui incluent plusieurs niveaux mip ; si les texels qui figurent sur l'un des mipmaps ne sont pas mappés, mais que le poids de ces texels est de 0, ces texels ne considérés comme un accès non mappés.

Lors de l’échantillonnage à partir d’un format qui a moins de 4 composants (tels que DXGI\_FORMAT\_R8\_UNORM), n’importe quel texels qui se situent **NULL** vignettes résultat dans l’un **NULL** mappé étant accès signalée, quel que soit les composants qui le nuanceur étudie dans le résultat. Par exemple, lecture à partir de R8\_UNORM et masquage de lire les résultats dans le nuanceur avec.gba/.yzw semble ne pas besoin de lire la texture du tout. Toutefois, si l’adresse du texel est une vignette mappée **NULL**, l’opération est toujours considérée comme un accès **NULL**.

Le nuanceur peut vérifier l’état et poursuivre toutes les mesures à suivre en cas d’échec. Par exemple, une mesure à suivre peut prendre la forme d'une journalisation des échecs (par exemple, via l'écriture d'un accès sans ordre) et/ou de l'émission d'une autre lecture limitée à un niveau de détail plus grand connu pour être mappé. Une application peut également souhaiter enregistrer les accès réussis afin d’avoir une idée de la partie de l’ensemble de vignettes mappé à laquelle vous avez accédé.

La journalisation peut s'avérer compliquée car aucun mécanisme ne permet de consigner le nombre exact de vignettes auxquelles vous avez accédé. L’application peut émettre des hypothèses conservatrices basées sur la connaissance des coordonnées utilisées pour l'accès, ainsi que l'utilisation des instructions de niveau de détail ; par exemple, [**tex2Dlod**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod)) renvoie le calcul du niveau de détail du matériel.

En outre, de nombreux accès concernent les mêmes vignettes, c'est pourquoi une journalisation redondante peut survenir et possiblement entraîner la saturation de la mémoire. Il peut être pratique si le matériel peut être accordé ne pas besoin de rapport vignette accès si elles ont été signalés à un autre emplacement avant la possibilité. L’état d'un suivi de ce type peut être réinitialisé à partir de l’API (probablement aux limites de l'image).

## <a name="span-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanspan-idper-sampleminlodclampspanper-sample-minlod-clamp"></a><span id="Per-sample_MinLOD_clamp"></span><span id="per-sample_minlod_clamp"></span><span id="PER-SAMPLE_MINLOD_CLAMP"></span>Clamp de MinLOD par exemple


Pour aider les nuanceurs à éviter les ressources de diffusion en continu mipmappées qui sont reconnues comme non mappées, la plupart des instructions de nuanceur qui impliquent l’utilisation d’un échantillon (filtrage) ont un mode qui permet au nuanceur de transmettre un paramètre de clamp MinLOD float32 supplémentaire à l’échantillon de texture. Cette valeur figure dans l'espace du numéro de mipmap, par opposition à la ressource sous-jacente.

Le matériel exécute` max(fShaderMinLODClamp,fComputedLOD) `au même emplacement dans le calcul du niveau de détail où survient le clamp MinLOD, qui est également un [**max**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-max)().

Si le résultat de l’application de la bride LOD par exemple et n’importe quel autres pinces LOD définis dans l’échantillonneur est un ensemble vide, le résultat est le même accès aux limites du résultat en tant que crochet minLOD par ressource : 0 pour les composants dans le format de surface et les valeurs par défaut pour les composants manquants.

L’instruction LOD (par exemple, [**tex2Dlod**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-tex2dlod)), qui a permis de restaurer le clamp minLOD par échantillon décrit ici, renvoie à la fois un niveau de détail limité et non limité. Le niveau de détail limité renvoyé par cette instruction LOD reflète l'ensemble des limitations, compris la limitation par ressource, mais pas la limitation par échantillon. La limitation par échantillon est contrôlée et connue par le nuanceur, de manière à ce que si nécessaire, l'auteur du nuanceur puisse appliquer manuellement cette limitation à la valeur de retour des instructions au niveau du détail.

## <a name="span-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanspan-idminmaxreductionfilteringspanminmax-reduction-filtering"></a><span id="Min_Max_reduction_filtering"></span><span id="min_max_reduction_filtering"></span><span id="MIN_MAX_REDUCTION_FILTERING"></span>Filtrage de réduction Min/Max


Les applications peuvent choisir de gérer leurs propres structures de données qui les informent de ce à quoi ressemblent les mappages pour une ressource de diffusion en continu. Par exemple, dans le cas d'une surface qui contient un texel pour intégrer les informations pour chaque vignette dans une ressource de diffusion en continu. Il est possible de stocker le premier niveau de détail mappé sur un emplacement de vignette donné. En échantillonnant avec précaution cette structure de données de la même manière que pour une ressource de diffusion en continu, il est possible de déterminer quel sera le niveau de détail minimal pour un encombrement de filtre de texture entière. Pour vous aider à faciliter le processus, Direct3D 11.2 introduit un nouveau mode d’échantillonneur à usage général, le filtrage min/max.

L’utilitaire de filtrage min/max pour le suivi du niveau de détail peut s'avérer utile dans d'autres contextes, comme par exemple, le filtrage des surfaces de profondeur.

Le filtrage de réduction min/max est un mode sur les échantillonneurs qui récupère le même jeu de texels qu'un filtre de texte normal. Mais plutôt que de fusionner les valeurs afin de générer une réponse, il renvoie le min() ou max() des texels récupérés, composant par composant (par exemple, le minimum de toutes les valeurs R, séparément du minimum de toutes les valeurs G, etc).

Les opérations min/max respectent les règles de précision arithmétiques Direct3D. L’ordre des comparaisons n’a pas d’importance.

Lors des opérations de filtrage qui ne concernent pas les min/max, il arrive parfois que le poids d’un texel donné soit égal à 0.0. Par exemple, dans le cas d'un échantillon linéaire avec des coordonnées de texture qui tombent directement au centre d'un texel, trois autres texels (ils sont susceptibles de varier en fonction du matériel) contribuent au filtre mais avec un poids égal à 0. Si l'un de ces texels possède un poids égal à 0 sur un filtre non-min/max, si le filtre est min/max, ces texels n'ont aucune influence sur le résultat (et les poids n'affectent d'aucune manière que ce soit les opérations de filtrage min/max).

La prise en charge de cette fonctionnalité dépend de la prise en charge de [niveau 2](tier-2.md) pour les ressources de diffusion en continu.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Accès de pipeline pour la diffusion en continu de ressources](pipeline-access-to-streaming-resources.md)

 

 




