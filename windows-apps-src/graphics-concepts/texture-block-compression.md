---
title: Compression de blocs de texture
description: "La prise en charge de la compression de blocs (BC) pour les textures a été étendue dans Direct3D 11 pour inclure les algorithmes BC6H et BC7."
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- Compression de blocs de texture
author: PeterTurcan
ms.author: pettur
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 3930d5d2350fcbb7878edec8a5a93dfd4c36caee
ms.lasthandoff: 02/07/2017

---

# <a name="texture-block-compression"></a>Compression de blocs de texture


La prise en charge de la compression de blocs (BC) pour les textures a été étendue dans Direct3D 11 pour inclure les algorithmes BC6H et BC7. L’algorithme BC6H prend en charge les données de source de couleur à plage dynamique élevée, tandis que l’algorithme BC7 offre une compression de meilleure qualité globale avec un nombre réduit d’artéfacts pour les données de source RVB standard.

Pour plus d’informations sur la prise en charge de l’algorithme de compression de blocs avant Direct3D 11 et la prise en charge des formats BC1 à BC5, voir [Compression de blocs (Direct3D 10)](https://msdn.microsoft.com/library/windows/desktop/bb694531).

**Remarque concernant les formats de fichiers : **les formats de compression de texture BC6H et BC7 utilisent le format de fichier DDS pour stocker les données de texture compressées. Pour plus d’informations, voir le [Guide de programmation pour DDS](https://msdn.microsoft.com/library/windows/desktop/bb943991).

## <a name="span-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanspan-idblockcompressionformatssupportedindirect3d11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>Formats de compression de blocs pris en charge dans Direct3D 11


| Données sources                                  | Résolution de la compression de données minimale requise                              | Format recommandé | Niveau de fonctionnalité minimal pris en charge |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| Trois canaux de couleurs avec canal alpha       | Trois canaux de couleurs (5 bits:6 bits:5 bits), avec 0 ou 1 bit d’alpha  | BC1                | Direct3D 9.1                    |
| Trois canaux de couleurs avec canal alpha       | Trois canaux de couleurs (5 bits:6 bits:5 bits), avec 4 bits d’alpha         | BC2                | Direct3D 9.1                    |
| Trois canaux de couleur avec canal alpha       | Trois canaux couleurs (5 bits:6 bits:5 bits) avec 8 bits d’alpha          | BC3                | Direct3D 9.1                    |
| Un canal de couleur                            | Un canal de couleur (8 bits)                                                | BC4                | Direct3D 10                     |
| Deux canaux de couleur                            | Deux canaux de couleur (8 bits:8 bits)                                        | BC5                | Direct3D 10                     |
| Trois canaux de couleur à plage dynamique élevée (HDR) | Trois canaux (16 bits:16 bits:16 bits) de couleur dans format à virgule flottante demi-précision\* | BC6H               | Direct3D 11                     |
| Trois canaux de couleur, canal alpha facultatif  | Trois canaux de couleur (4 à 7 bits par canal) avec 0 à 8 bits d’alpha  | BC7                | Direct3D 11                     |

 

\*La virgule flottante demi-précision est une valeur de 16 bits qui se compose d’un signe facultatif d’un bit, d’un exposant tronqué de 5 bits et d’une mantisse de 10 ou 11 bits.
## <a name="span-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanspan-idbc1bc2andb3formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>Formats BC1, BC2 et B3


Les formats BC1, BC2 et BC3 sont équivalents aux formats de compression de texture DXTn de Direct3D 9 et sont les mêmes que les formats BC1, BC2 et BC3 de Direct3D 10. La prise en charge de ces trois formats est requise par tous les niveaux de fonctionnalités (D3D\_FEATURE\_LEVEL\_9\_1, D3D\_FEATURE\_LEVEL\_9\_2, D3D\_FEATURE\_LEVEL\_9\_3, D3D\_FEATURE\_LEVEL\_10\_0, D3D\_FEATURE\_LEVEL\_10\_1 et D3D\_FEATURE\_LEVEL\_11\_0).

| Format de compression de blocs | Format DXGI                                                                           | Format Direct3D 9 équivalent                               | Octets par bloc de pixels 4 x 4 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI\_FORMAT\_BC1\_UNORM, DXGI\_FORMAT\_BC1\_UNORM\_SRGB, DXGI\_FORMAT\_BC1\_TYPELESS | D3DFMT\_DXT1, FourCC="DXT1"                                | 8                         |
| BC2                      | DXGI\_FORMAT\_BC2\_UNORM, DXGI\_FORMAT\_BC2\_UNORM\_SRGB, DXGI\_FORMAT\_BC2\_TYPELESS | D3DFMT\_DXT2\*, FourCC="DXT2", D3DFMT\_DXT3, FourCC="DXT3" | 16                        |
| BC3                      | DXGI\_FORMAT\_BC3\_UNORM, DXGI\_FORMAT\_BC3\_UNORM\_SRGB, DXGI\_FORMAT\_BC3\_TYPELESS | D3DFMT\_DXT4\*, FourCC="DXT4", D3DFMT\_DXT5, FourCC="DXT5" | 16                        |

 

\*Ces schémas de compression (DXT2 et DXT4) ne font aucune différence entre les formats alpha prémultipliés et les formats standard alpha de Direct3D 9. Cette distinction doit être traitée par les nuanceurs programmables au moment du rendu.

## <a name="span-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanspan-idbc4andbc5formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>Formats BC4 et BC5


| Format de compression de blocs | Format DXGI                                                                     | Format Direct3D 9 équivalent | Octets par bloc de pixels 4 x 4 |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC4                      | DXGI\_FORMAT\_BC4\_UNORM, DXGI\_FORMAT\_BC4\_SNORM, DXGI\_FORMAT\_BC4\_TYPELESS | FourCC="ATI1"                | 8                         |
| BC5                      | DXGI\_FORMAT\_BC5\_UNORM, DXGI\_FORMAT\_BC5\_SNORM, DXGI\_FORMAT\_BC5\_TYPELESS | FourCC="ATI2"                | 16                        |

 

## <a name="span-idbc6hformatspanspan-idbc6hformatspanspan-idbc6hformatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>Format BC6H


Pour plus d’informations sur ce format, voir la documentation sur le [Format BC6H](https://msdn.microsoft.com/library/windows/desktop/hh308952).

| Format de compression de blocs | Format DXGI                                                                      | Format Direct3D 9 équivalent | Octets par bloc de pixels 4 x 4 |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI\_FORMAT\_BC6H\_UF16, DXGI\_FORMAT\_BC6H\_SF16, DXGI\_FORMAT\_BC6H\_TYPELESS | Non applicable                          | 16                        |

 

Le format BC6H peut sélectionner différents modes de codage pour chaque bloc de pixels 4 x 4. Un total de 14 modes de codage différents sont disponibles, chacun présentant des compromis légèrement différents en termes de qualité visuelle rendue de la texture affichée. Le choix des modes permet un décodage rapide par le matériel avec le niveau de qualité sélectionné ou adapté au contenu source, mais il augmente également considérablement la complexité de l’espace de recherche.

## <a name="span-idbc7formatspanspan-idbc7formatspanspan-idbc7formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>Format BC7


Pour plus d’informations sur ce format, voir la documentation sur le [Format BC7](https://msdn.microsoft.com/library/windows/desktop/hh308953).

| Format de compression de blocs | Format DXGI                                                                           | Format Direct3D 9 équivalent | Octets par bloc de pixels 4 x 4 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI\_FORMAT\_BC7\_UNORM, DXGI\_FORMAT\_BC7\_UNORM\_SRGB, DXGI\_FORMAT\_BC7\_TYPELESS | Non applicable                          | 16                        |

 

Le format BC7 peut sélectionner différents modes de codage pour chaque bloc de pixels 4 x 4. Un total de 8 modes de codage différents sont disponibles, chacun présentant des compromis légèrement différents en termes de qualité visuelle rendue de la texture affichée. Le choix des modes permet un décodage rapide par le matériel avec le niveau de qualité sélectionné ou adapté au contenu source, mais il augmente également considérablement la complexité de l’espace de recherche.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques associées


[Annexes](appendix.md)

[Textures](https://msdn.microsoft.com/library/windows/desktop/ff476902)

 

 




