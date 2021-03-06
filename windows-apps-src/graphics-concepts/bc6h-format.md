---
title: Format BC6H
description: Le format BC6H est un format de compression de texture conçu pour prendre en charge les espaces de couleur à plage dynamique élevée (HDR) dans les données source.
ms.assetid: 6781D967-9262-4EE7-B354-7A6D0EA0498E
keywords:
- Format BC6H
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 50c8fa623130412688f14307fa46540c81f38554
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370485"
---
# <a name="bc6h-format"></a>Format BC6H


Le format BC6H est un format de compression de texture conçu pour prendre en charge les espaces de couleur à plage dynamique élevée (HDR) dans les données source.

## <a name="span-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanspan-idabout-bc6h-dxgi-format-bc6hspanabout-bc6hdxgiformatbc6h"></a><span id="About-BC6H-DXGI-FORMAT-BC6H"></span><span id="about-bc6h-dxgi-format-bc6h"></span><span id="ABOUT-BC6H-DXGI-FORMAT-BC6H"></span>À propos de BC6H/DXGI\_FORMAT\_BC6H


Le format BC6H fournit une compression de haute qualité pour les images qui utilisent trois canaux de couleurs HDR, avec une valeur de 16 bits pour chaque canal de couleur de la valeur (16: 16:16). Le canaux alpha ne sont pas pris en charge.

BC6H est spécifié par le suivant DXGI\_valeurs d’énumération de FORMAT :

-   **DXGI\_FORMAT\_BC6H\_TYPELESS**.
-   **DXGI\_FORMAT\_BC6H\_UF16**. Ce format BC6H n’utilise pas de bit de signe dans les valeurs de canal de couleur à virgule flottante de 16 bits.
-   **DXGI\_FORMAT\_BC6H\_SF16**. Ce format BC6H utilise un bit de signe dans les valeurs de canal de couleur à virgule flottante de 16 bits.

**Remarque**    le bit 16 format floating point pour les canaux de couleur est souvent appelé format à virgule flottante « moitié ». Ce format présente la disposition de bits suivante :
|                       |                                                 |
|-----------------------|-------------------------------------------------|
| UF16 (flottant non signé) | 5 bits exposants + 11 bits mantisse              |
| SF16 (flottant signé)   | 1 bit de signe + 5 bits exposants + 10 bits mantisse |

 

 

Le format BC6H peut être utilisé pour les ressources de texture [Texture2D](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-reference-resource-structures) (y compris les tableaux), Texture3D ou TextureCube (y compris les tableaux). De même, ce format s’applique à toutes les surfaces de carte MIP associées à ces ressources.

BC6H utilise une taille de bloc fixe de 16 octets (128 bits) et une taille de vignette fixe de 4 x 4 texels. Comme pour les formats BC précédents, les images de texture d'une taille supérieure à la taille de vignette prise en charge (4 x 4) sont compressées à l’aide de plusieurs blocs. Cette identité d’adressage s’applique également aux images 3D, aux mipmaps, aux cartes de cube et aux tableaux de textures. Toutes les vignettes de l’image doivent être au même format.

Avertissements concernant le format BC6H :

-   Le format BC6H prend en charge la dénormalisation à virgule flottante, mais ni la représentation INF (infini) ni la représentation NaN (« Not a Number »). L’exception est le mode signé de BC6H (DXGI\_FORMAT\_BC6H\_SF16), qui prend en charge -INF (infini négatif). Cette prise en charge de -INF est simplement un artéfact du format lui-même et n’est pas spécifiquement prise en charge par les encodeurs pour ce format. En règle générale, quand un encodeur rencontre des données d'entrée INF (positif ou négatif) ou NaN, l’encodeur doit convertir ces données dans la valeur de représentation Non INF maximale autorisée et mapper NaN à la valeur 0 avant la compression.
-   BC6H ne prend pas en charge le canal alpha.
-   Le décodeur BC6H effectue la décompression avant d'appliquer le filtrage de textures.
-   La décompression BC6H doit être précise au bit près ; autrement dit, le matériel doit retourner des résultats identiques au décodeur décrit dans cette documentation.

## <a name="span-idbc6h-implementationspanspan-idbc6h-implementationspanspan-idbc6h-implementationspanbc6h-implementation"></a><span id="BC6H-implementation"></span><span id="bc6h-implementation"></span><span id="BC6H-IMPLEMENTATION"></span>Implémentation de BC6H


Un bloc BC6H se compose de bits de mode, de points de terminaison compressés, d'indices compressés et d'un index de partition facultatif. Ce format spécifie 14 modes différents.

Une couleur de point de terminaison est stockée sous la forme d'un triplet RVB. Le format BC6H définit une palette de couleurs sur une ligne approximative à travers un certain nombre de points de terminaison de couleur définis. En outre, selon le mode, une vignette peut être divisée en deux zones ou traitée comme une seule région, dans laquelle une vignette à deux zones comporte pour chaque zone un ensemble distinct de points de terminaison de couleur. BC6H stocke un index de palette par texel.

Dans le cas d'une vignette à deux zones, 32 partitions sont possibles.

## <a name="span-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspanspan-iddecoding-the-bc6h-formatspandecoding-the-bc6h-format"></a><span id="Decoding-the-BC6H-format"></span><span id="decoding-the-bc6h-format"></span><span id="DECODING-THE-BC6H-FORMAT"></span>Décodage du format BC6H


Le pseudo-code ci-dessous illustre les étapes permettant de décompresser le pixel (x, y) étant donné le bloc BC6H de 16 octets.

``` syntax
decompress_bc6h(x, y, block)
{
    mode = extract_mode(block);
    endpoints;
    index;
    
    if(mode.type == ONE)
    {
        endpoints = extract_compressed_endpoints(mode, block);
        index = extract_index_ONE(x, y, block);
    }
    else //mode.type == TWO
    {
        partition = extract_partition(block);
        region = get_region(partition, x, y);
        endpoints = extract_compressed_endpoints(mode, region, block);
        index = extract_index_TWO(x, y, partition, block);
    }
    
    unquantize(endpoints);
    color = interpolate(index, endpoints);
    finish_unquantize(color);
}
```

Le tableau suivant contient le nombre et les valeurs de bits pour chacun des 14 formats possibles pour les blocs BC6H.

| Mode | Index de partition | Partition | Points de terminaison de couleur                  | Bits de mode      |
|------|-------------------|-----------|----------------------------------|----------------|
| 1    | 46 bits           | 5 bits    | 75 bits (10.555, 10.555, 10.555) | 2 bits (00)    |
| 2    | 46 bits           | 5 bits    | 75 bits (7666, 7666, 7666)       | 2 bits (01)    |
| 3    | 46 bits           | 5 bits    | 72 bits (11.555, 11.444, 11.444) | 5 bits (00010) |
| 4    | 46 bits           | 5 bits    | 72 bits (11.444, 11.555, 11.444) | 5 bits (00110) |
| 5    | 46 bits           | 5 bits    | 72 bits (11.444, 11.444, 11.555) | 5 bits (01010) |
| 6    | 46 bits           | 5 bits    | 72 bits (9555, 9555, 9555)       | 5 bits (01110) |
| 7    | 46 bits           | 5 bits    | 72 bits (8666, 8555, 8555)       | 5 bits (10010) |
| 8    | 46 bits           | 5 bits    | 72 bits (8555, 8666, 8555)       | 5 bits (10110) |
| 9    | 46 bits           | 5 bits    | 72 bits (8555, 8555, 8666)       | 5 bits (11010) |
| 10   | 46 bits           | 5 bits    | 72 bits (6666, 6666, 6666)       | 5 bits (11110) |
| 11   | 63 bits           | 0 bits    | 60 bits (10.10, 10.10, 10.10)    | 5 bits (00011) |
| 12   | 63 bits           | 0 bits    | 60 bits (11.9, 11.9, 11.9)       | 5 bits (00111) |
| 13   | 63 bits           | 0 bits    | 60 bits (12.8, 12.8, 12.8)       | 5 bits (01011) |
| 14   | 63 bits           | 0 bits    | 60 bits (16.4, 16.4, 16.4)       | 5 bits (01111) |

 

Chaque format présenté dans ce tableau peut être identifié de manière unique par les bits de mode. Les dix premiers modes sont utilisés pour les vignettes à deux zones, et le champ de bits de mode peut contenir deux ou cinq bits. Ces blocs comportent également des champs pour les points de terminaison de couleur compressés (72 ou 75 bits), la partition (5 bits) et les index de partition (46 bits).

Pour les points de terminaison de couleur compressés, les valeurs données dans le tableau précédent indiquent la précision des points de terminaison RVB stockés et le nombre de bits utilisés pour chaque valeur de couleur. Par exemple, le mode 3 spécifie un niveau de précision de point de terminaison de couleur de 11 et le nombre de bits utilisés pour stocker les valeurs de delta des points de terminaison transformés pour les couleurs rouge, bleu et vert (5, 4 et 4, respectivement). Le mode 10 n’utilise pas de compression delta et stocke les quatre points de terminaison de couleur de manière explicite.

Les quatre derniers modes de bloc sont utilisés pour les vignettes à une zone, où le champ de mode est de 5 bits. Ces blocs comportent des champs pour les points de terminaison (60 bits) et les index compressés (63 bits). Le mode 11 (comme le mode 10) n’utilise pas de compression delta et stocke deux points de terminaison de couleur de manière explicite.

Les modes 10011, 10111, 11011 et 11111 (non illustrés) sont réservés. Ne les utilisez pas dans votre encodeur. Si le matériel reçoit des blocs où l’un de ces modes est spécifié, le bloc décompressé obtenu doit contenir tous les zéros dans tous les canaux, à l’exception du canal alpha.

Pour le format BC6H, le canal alpha doit toujours retourner 1.0, quel que soit le mode.

### <a name="span-idbc6h-partition-setspanspan-idbc6h-partition-setspanspan-idbc6h-partition-setspanbc6h-partition-set"></a><span id="BC6H-partition-set"></span><span id="bc6h-partition-set"></span><span id="BC6H-PARTITION-SET"></span>Jeu de partitions BC6H

Une vignette à deux zones peut comporter 32 jeux de partitions possibles, que vous trouverez dans le tableau ci-dessous. Chaque bloc de 4 x 4 représente une forme unique.

![Tableau des jeux de partitions BC6H](images/bc6h-partition-sets.png)

Dans ce tableau de jeux de partitions, les entrées en gras et soulignées représentent l’emplacement de l’index de correction pour un sous-ensemble 1 (qui est spécifié avec un bit de moins). L’index de correction pour le sous-ensemble 0 est toujours l'index 0, puisque le partitionnement est toujours organisé de sorte que cet index 0 soit toujours dans le sous-ensemble 0. L'ordre des partitions se lit de haut à gauche vers le bas à droite, puis de gauche à droite et de haut en bas.

## <a name="span-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanspan-idbc6h-compressed-endpoint-formatspanbc6h-compressed-endpoint-format"></a><span id="BC6H-compressed-endpoint-format"></span><span id="bc6h-compressed-endpoint-format"></span><span id="BC6H-COMPRESSED-ENDPOINT-FORMAT"></span>Format de point de terminaison compressé BC6H


![Champs de bits pour les formats de point de terminaison BC6H compressés](images/bc6h-headers-med.png)

Ce tableau indique les champs de bits pour les points de terminaison compressés en fonction du format de point de terminaison, où chaque colonne spécifie un encodage et chaque ligne un champ de bits. Cette approche occupe 82 bits pour les vignettes à deux zones et 65 bits pour les vignettes à une zone. Par exemple, les 5 premiers bits pour la région d’un \[16 4\] encodage ci-dessus (plus précisément, la colonne la plus à droite) sont des bits m\[4:0\], les 10 bits suivants sont des bits rw\[9:0\], et ainsi de suite avec les derniers bits 6 contenant bw\[10:15\].

Les noms de champs dans le tableau ci-dessus sont définis comme suit :

| Champ | Variable          |
|-------|-------------------|
| m     | mode              |
| d     | index de forme       |
| rw    | endpt\[0\].A\[0\] |
| rx    | endpt\[0\].B\[0\] |
| ry    | endpt\[1\].A\[0\] |
| rz    | endpt\[1\].B\[0\] |
| gw    | endpt\[0\].A\[1\] |
| gx    | endpt\[0\].B\[1\] |
| gy    | endpt\[1\].A\[1\] |
| gz    | endpt\[1\].B\[1\] |
| bw    | endpt\[0\].A\[2\] |
| bx    | endpt\[0\].B\[2\] |
| par    | endpt\[1\].A\[2\] |
| bz    | endpt\[1\].B\[2\] |

 

Endpt\[je\], où i est 0 ou 1, fait référence à l’ensemble de points de terminaison 0 ou 1 st respectivement.
## <a name="span-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspanspan-idsign-extension-for-endpoint-valuesspansign-extension-for-endpoint-values"></a><span id="Sign-extension-for-endpoint-values"></span><span id="sign-extension-for-endpoint-values"></span><span id="SIGN-EXTENSION-FOR-ENDPOINT-VALUES"></span>Extension de signe pour les valeurs de point de terminaison


Pour les vignettes à deux zones, quatre valeurs de point de terminaison peuvent avoir une extension de signe. Endpt\[0\]. Est signé uniquement si le format est un format signé ; les autres points de terminaison sont signés uniquement si le point de terminaison a été transformé, ou si le format est un format signé. Le code ci-dessous illustre l’algorithme permettant d’étendre le signe des valeurs de point de terminaison à deux zones.

``` syntax
static void sign_extend_two_region(Pattern &p, IntEndpts endpts[NREGIONS_TWO])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
      if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
      if (p.transformed || BC6H::FORMAT == SIGNED_F16)
      {
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
        endpts[1].A[i] = SIGN_EXTEND(endpts[1].A[i], p.chan[i].delta[1]);
        endpts[1].B[i] = SIGN_EXTEND(endpts[1].B[i], p.chan[i].delta[2]);
      }
    }
}
```

Pour les vignettes de celui de la région, le comportement est le même qu’avec endpt\[1\] supprimé.

``` syntax
static void sign_extend_one_region(Pattern &p, IntEndpts endpts[NREGIONS_ONE])
{
    for (int i=0; i<NCHANNELS; ++i)
    {
    if (BC6H::FORMAT == SIGNED_F16)
        endpts[0].A[i] = SIGN_EXTEND(endpts[0].A[i], p.chan[i].prec);
    if (p.transformed || BC6H::FORMAT == SIGNED_F16) 
        endpts[0].B[i] = SIGN_EXTEND(endpts[0].B[i], p.chan[i].delta[0]);
    }
}
```

## <a name="span-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspanspan-idtransform-inversion-for-endpoint-valuesspantransform-inversion-for-endpoint-values"></a><span id="Transform-inversion-for-endpoint-values"></span><span id="transform-inversion-for-endpoint-values"></span><span id="TRANSFORM-INVERSION-FOR-ENDPOINT-VALUES"></span>Transformer l’inversion des valeurs de point de terminaison


Pour les vignettes de deux régions, la transformation s’applique à l’inverse de la différence d’encodage, l’ajout de la valeur de base au endpt\[0\]. Un pour les trois autres entrées pour un total de 9 Ajout d’opérations. Dans l’image ci-dessous, la valeur de base est représentée par « A0 » et est associée à la plus haute précision de virgule flottante. « A1 », « B0 » et « B1 » sont tous des deltas calculés à partir de la valeur d’ancrage, et ces valeurs delta sont représentées avec une précision plus faible. (A0 correspond à endpt\[0\]. A, B0 correspond à endpt\[0\]. B, A1 correspond à endpt\[1\]. A, B1 et correspond à endpt\[1\]. B.)

![Calcul des valeurs de point de terminaison pour l'inversion de transformation](images/bc6h-transform-inverse.png)

Pour les vignettes à une zone, il n'y a qu'un seul décalage delta, et par conséquent seulement 3 opérations d'ajout.

Décompresseur devez vous assurer que le que les résultats de l’inverse transformer ne sera pas déborder la précision d’endpt\[0\]. un. Dans le cas contraire, les valeurs résultant de la transformation inverse doivent être encapsulées dans le même nombre de bits. Si la précision de A0 est « p bits », l’algorithme de transformation est le suivant :

`B0 = (B0 + A0) & ((1 << p) - 1)`

Pour les formats signés, une extension du signe doit également être appliquée aux résultats du calcul du delta. Si l’opération d’extension du signe considère une extension des deux signes, où 0 est positif et 1 est négatif, l’extension de signe de 0 s’occupe du clamp ci-dessus. De la même manière, après le clamp ci-dessus, seule une valeur de 1 (négative) doit avoir une extension de signe.

## <a name="span-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanspan-idunquantization-of-color-endpointsspanunquantization-of-color-endpoints"></a><span id="Unquantization-of-color-endpoints"></span><span id="unquantization-of-color-endpoints"></span><span id="UNQUANTIZATION-OF-COLOR-ENDPOINTS"></span>Unquantization des points de terminaison de couleur


Étant donnés les points de terminaison non compressés, l’étape suivante consiste à effectuer une déquantification initiale des points de terminaison de couleur. Cette opération comprend trois étapes :

-   Déquantification des palettes de couleurs
-   Interpolation des palettes
-   Finalisation de la déquantification

La séparation du processus de déquantification en deux parties (déquantification des palettes de couleurs avant interpolation et déquantification finale après interpolation) permet de réduire le nombre d’opérations de multiplication nécessaires par rapport à un processus de déquantification complet avant l’interpolation des palettes.

Le code ci-dessous illustre le processus de récupération des estimations des valeurs de couleur 16 bits d’origine, qui utilise ensuite les valeurs de poids spécifiées pour ajouter 6 valeurs de couleurs supplémentaires à la palette. La même opération est effectuée sur chaque canal.

``` syntax
int aWeight3[] = {0, 9, 18, 27, 37, 46, 55, 64};
int aWeight4[] = {0, 4, 9, 13, 17, 21, 26, 30, 34, 38, 43, 47, 51, 55, 60, 64};

// c1, c2: endpoints of a component
void generate_palette_unquantized(UINT8 uNumIndices, int c1, int c2, int prec, UINT16 palette[NINDICES])
{
    int* aWeights;
    if(uNumIndices == 8)
        aWeights = aWeight3;
    else  // uNumIndices == 16
        aWeights = aWeight4;

    int a = unquantize(c1, prec); 
    int b = unquantize(c2, prec);

    // interpolate
    for(int i = 0; i < uNumIndices; ++i)
        palette[i] = finish_unquantize((a * (64 - aWeights[i]) + b * aWeights[i] + 32) >> 6);
}
```

L’exemple de code suivant illustre le processus d’interpolation, avec les observations suivantes :

-   Puisque la plage complète des valeurs de couleur pour la fonction **unquantize** (ci-dessous) est comprise entre-32 768 et 65 535, l’interpolateur est implémenté à l’aide d'une arithmétique signée de 17 bits.
-   Après l’interpolation, les valeurs sont passées à la **Terminer\_unquantize** (décrite dans le troisième exemple dans cette section), de la fonction qui s’applique la mise à l’échelle finale.
-   Tous les décompresseurs matériels sont nécessaires pour renvoyer des résultats d'une précision au bit près avec ces fonctions.

``` syntax
int unquantize(int comp, int uBitsPerComp)
{
    int unq, s = 0;
    switch(BC6H::FORMAT)
    {
    case UNSIGNED_F16:
        if(uBitsPerComp >= 15)
            unq = comp;
        else if(comp == 0)
            unq = 0;
        else if(comp == ((1 << uBitsPerComp) - 1))
            unq = 0xFFFF;
        else
            unq = ((comp << 16) + 0x8000) >> uBitsPerComp;
        break;

    case SIGNED_F16:
        if(uBitsPerComp >= 16)
            unq = comp;
        else
        {
            if(comp < 0)
            {
                s = 1;
                comp = -comp;
            }

            if(comp == 0)
                unq = 0;
            else if(comp >= ((1 << (uBitsPerComp - 1)) - 1))
                unq = 0x7FFF;
            else
                unq = ((comp << 15) + 0x4000) >> (uBitsPerComp-1);

            if(s)
                unq = -unq;
        }
        break;
    }
    return unq;
}
```

**Terminer\_unquantize** est appelée après l’interpolation de palette. La fonction **unquantize** diffère la mise à l’échelle de 31/32 pour les bits signés et de 31/64 pour les bits non signés. Ce comportement est nécessaire pour obtenir la valeur finale dans la demi-plage valide (-0x7BFF ~ 0x7BFF) après l'interpolation des palettes afin de réduire le nombre de multiplications nécessaires. **Terminer\_unquantize** applique la mise à l’échelle finale et retourne un **unsigned short** valeur obtient réinterprétée dans **moitié**.

``` syntax
unsigned short finish_unquantize(int comp)
{
    if(BC6H::FORMAT == UNSIGNED_F16)
    {
        comp = (comp * 31) >> 6;                                         // scale the magnitude by 31/64
        return (unsigned short) comp;
    }
    else // (BC6H::FORMAT == SIGNED_F16)
    {
        comp = (comp < 0) ? -(((-comp) * 31) >> 5) : (comp * 31) >> 5;   // scale the magnitude by 31/32
        int s = 0;
        if(comp < 0)
        {
            s = 0x8000;
            comp = -comp;
        }
        return (unsigned short) (s | comp);
    }
}
```

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Compression de bloc de texture](texture-block-compression.md)

 

 




