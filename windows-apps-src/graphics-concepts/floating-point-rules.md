---
title: Règles en matière de virgule flottante
description: Direct3D prend en charge plusieurs représentations à virgule flottante. Tous les calculs à virgule flottante fonctionnent sous un sous-ensemble défini de règles de représentation des nombres à virgule flottante 32 bits simple précision conformément à l'IEEE 754.
ms.assetid: 3B0C95E2-1025-4F70-BF14-EBFF2BB53AFF
keywords:
- Règles en matière de virgule flottante
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a29fbe49e45b819ddf4ffc3172445996d3622360
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370627"
---
# <a name="span-iddirect3dconceptsfloating-pointrulesspanfloating-point-rules"></a><span id="direct3dconcepts.floating-point_rules"></span>Règles à virgule flottante


Direct3D prend en charge plusieurs représentations à virgule flottante. Tous les calculs à virgule flottante fonctionnent sous un sous-ensemble défini de règles de représentation des nombres à virgule flottante 32 bits simple précision conformément à l'IEEE 754.

## <a name="span-idalpha32bitspanspan-idalpha32bitspan32-bit-floating-point-rules"></a><span id="alpha_32_bit"></span><span id="ALPHA_32_BIT"></span>règles à virgule flottante 32 bits


Il existe deux ensembles de règles : les règles qui sont conformes à la norme IEEE-754, et celles qui s’en écartent.

### <a name="span-idalpha754rulesspanspan-idalpha754rulesspanspan-idalpha754rulesspanhonored-ieee-754-rules"></a><span id="alpha_754_Rules"></span><span id="alpha_754_rules"></span><span id="ALPHA_754_RULES"></span>Règles IEEE-754 honorés

Certaines de ces règles constituent une simple option dans laquelle IEEE-754 propose des choix.

-   La division par 0 produit +/- INF (infini), sauf 0/0 qui donne une valeur NaN (n’est pas un nombre).
-   Le logarithme de (+/-) 0 produit -INF.  

    Le logarithme d’une valeur négative (différente de -0) produit NaN.
-   La racine carrée réciproque (rsq) ou la racine carrée (sqrt) d’une valeur négative produit NaN.  

    La seule exception est -0 ; sqrt(-0) produit -0, et rsq(-0) donne -INF.
-   INF - INF = NaN
-   (+/-)INF / (+/-)INF = NaN
-   (+/-) INF \* 0 = NaN
-   NaN (toute opération) toute valeur = NaN
-   Lorsque l’une des opérandes ou les deux correspondent à NaN, les comparaisons EQ (égal à), GT (supérieur à), GE (supérieur ou égal à), LT (inférieur à) et LE (inférieur ou égal à) renvoient la valeur **FALSE**.
-   Les comparaisons ignorent le signe de la valeur 0 (donc, +0 est égal à -0).
-   Lorsque l’une des opérandes ou les deux correspondent à NaN, la comparaison NE (différent de) renvoie la valeur **TRUE**.
-   Les comparaisons de toute valeur différente de NaN avec +/- INF renvoient le résultat correct.

### <a name="span-idalpha754deviationsspanspan-idalpha754deviationsspanspan-idalpha754deviationsspandeviations-or-additional-requirements-from-ieee-754-rules"></a><span id="alpha_754_Deviations"></span><span id="alpha_754_deviations"></span><span id="ALPHA_754_DEVIATIONS"></span>Les écarts ou des exigences supplémentaires à partir de règles de la norme IEEE-754

-   IEEE-754 exige que les opérations en virgule flottante produisent un résultat qui constitue la valeur représentable la plus proche d’un résultat infiniment précis, connu sous le terme d’arrondi au chiffre pair le plus proche.

    Direct3D 11 et définir jusqu'à la même exigence en tant que norme IEEE-754 : les opérations à virgule flottante 32 bits produisent un résultat qui est au sein de 0,5 (ULP) unité-dernière place du résultat précis à l’infini. Cela signifie par exemple que le matériel est autorisé à tronquer les résultats à 32 bits au lieu d’effectuer un arrondi au chiffre pair le plus proche, car cette opération entraînerait une erreur de 0,5 ULP au plus. Cette règle s’applique uniquement aux additions, aux soustractions et aux multiplications.

    Les versions antérieures de Direct3D définissent un impératif plus souple que IEEE-754 : les opérations à virgule flottante 32 bits produisent un résultat qui se trouve dans une unité last-sur place (1 ULP) du résultat précis à l’infini. Cela signifie par exemple que le matériel est autorisé à tronquer les résultats à 32 bits au lieu d’effectuer un arrondi au chiffre pair le plus proche, car cette opération entraînerait une erreur de 1 ULP au plus.

-   Il n’existe aucune prise en charge pour les exceptions de virgule flottante, les bits d’état ou les interruptions.
-   Les nombres dénormalisés sont remplacés par une valeur zéro avec conservation du signe sur l’entrée et la sortie de toute opération mathématique en virgule flottante. Il existe des exceptions pour des opérations d’E/S ou de déplacement de données qui ne manipulent pas les données.
-   Les états qui contiennent des valeurs en virgule flottante, telles que des valeurs Viewport MinDepth/MaxDepth ou BorderColor, peuvent être fournis sous forme de valeurs dénormalisées et être ou non vidés avant que le matériel ne les utilise.
-   Les opérations min ou max vident les nombres dénormalisés pour la comparaison, mais le résultat peut ou non faire l’objet d’un vidage des nombres dénormalisés.
-   L’entrée d’une valeur NaN dans une opération produit toujours une valeur NaN en sortie. Toutefois, il n’est pas nécessaire que la configuration binaire exacte de la valeur NaN reste identique (sauf si l’opération est une instruction de déplacement brute, ce qui ne modifie pas les données).
-   Les opérations min ou max dans lesquelles une seule opérande est une valeur NaN renvoient l’autre opérande comme résultat (contrairement aux règles de comparaison que nous avons examinées plus haut). Ceci est une règle énoncée par la norme IEEE 754R.

    La spécification IEEE-754R relative aux opérations min et max en virgule flottante stipule que si l’une des entrées de l’opération min ou max est une valeur NaN silencieuse (QNaN), le résultat de l’opération correspond à l’autre paramètre. Exemple :

    ```ManagedCPlusPlus
    min(x,QNaN) == min(QNaN,x) == x (same for max)
    ```

    Une révision apportée à la spécification IEEE-754R stipule désormais un autre comportement pour les opérations min et max lorsque l’une des entrées est une valeur NaN « signalée » (SNaN) opposée à une valeur QNaN :

    ```ManagedCPlusPlus
    min(x,SNaN) == min(SNaN,x) == QNaN (same for max)
     
    ```

    En règle générale, Direct3D suit les normes pour les opérations arithmétiques : IEEE-754 et IEEE-754R. Toutefois, dans ce cas précis, il existe une différence.

    Les règles arithmétiques dans Direct3D 10 et les versions ultérieures ne font pas de distinction entre les valeurs NaN silencieuses et signalées (QNaN/SNaN). Toutes les valeurs NaN sont gérées de la même façon. Dans le cas des opérations min et max, le comportement Direct3D vis-à-vis de toute valeur NaN est identique au traitement des valeurs QNaN défini dans la spécification IEEE-754R. (À titre de complément d’information, si les deux entrées correspondent à une valeur NaN, n’importe quelle valeur NaN est renvoyée.)

-   Une autre règle IEEE 754R stipule que min(-0,+0) == min(+0,-0) == -0, et que max(-0,+0) == max(+0,-0) == +0, ce qui conserve le signe, par contraste avec les règles de comparaison relatives au zéro signé (comme nous l’avons vu précédemment). Direct3D recommande le comportement IEEE 754R dans ce cas précis, mais ne l’applique pas ; le résultat de la comparaison de valeurs 0 est autorisé à dépendre de l’ordre des paramètres, à l’aide d’une comparaison qui ignore les signes.
-   x\*1.0f entraîne toujours x (sauf denorm vidées).
-   x/1.0f produit toujours x (à l’exception du vidage des nombres dénormalisés).
-   x +/- 0.0f produit toujours x (à l’exception du vidage des nombres dénormalisés). Mais -0 + 0 = +0.
-   Les opérations fusionnées (telles que mad, dp3) produisent des résultats aussi précis que le pire ordre séquentiel d’évaluation possible du développement non fusionné de l’opération. La définition du pire ordre possible, pour les besoins de tolérance, n’est pas une définition fixe pour une opération fusionne donnée ; elle dépend des valeurs spécifiques des entrées. Une tolérance de 1 ULP est autorisée pour chacune des étapes du développement non fusionné (dans le cas des instructions appelées par Direct3D avec une tolérance plus souple que 1 ULP, cette tolérance plus souple est autorisée).
-   Les opérations fusionnées respectent les mêmes règles en matière de valeurs NaN que les opérations non fusionnées.
-   Les opérations sqrt et rcp présentent une tolérance de 1 ULP. Les instructions de réciproque de nuanceur et de racine carrée réciproque, [**rcp**](https://docs.microsoft.com/previous-versions/windows/desktop/legacy/hh447205(v=vs.85)) et [**rsq**](https://docs.microsoft.com/windows/desktop/direct3dhlsl/rsq--sm4---asm-), présentent leur propre exigence distincte assouplie en matière de précision.
-   Les multiplications et les divisions opèrent chacune au niveau de précision des nombres en virgule flottante 32 bits (niveau de précision de 0,5 ULP pour les multiplication, et de 1,0 ULP pour la réciproque). Si x/y est implémenté directement, les résultats doivent présenter une précision supérieure ou égale à celle d’une méthode à deux étapes.

## <a name="span-iddoubleprec64bitspanspan-iddoubleprec64bitspan64-bit-double-precision-floating-point-rules"></a><span id="double_prec_64_bit"></span><span id="DOUBLE_PREC_64_BIT"></span>64 bits (double précision) flottante des règles de point


Les pilotes matériels et d’affichage prennent en charge en option les nombres en virgule flottante double précision. Pour indiquer la prise en charge, lorsque vous appelez [ **ID3D11Device::CheckFeatureSupport** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-checkfeaturesupport) avec [ **D3D11\_fonctionnalité\_DOUBLES** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ne-d3d11-d3d11_feature), les jeux de pilote **DoublePrecisionFloatShaderOps** de [ **D3D11\_fonctionnalité\_données\_DOUBLES** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ns-d3d11-d3d11_feature_data_doubles) sur TRUE. Le pilote et le matériel doivent alors prendre en charge toutes les instructions en virgule flottante double précision.

Les instructions double précision respectent les exigences de comportement stipulées par la norme IEEE 754R.

Les données double précision requièrent la prise en charge de la génération de valeurs dénormalisées (aucun comportement de remplacement de ces valeurs par zéro). De même, les instructions ne lisent pas les données dénormalisées sous la forme d’un zéro signé, mais respectent la valeur dénormalisée.

## <a name="span-idalpha16bitspanspan-idalpha16bitspan16-bit-floating-point-rules"></a><span id="alpha_16_bit"></span><span id="ALPHA_16_BIT"></span>règles à virgule flottante 16 bits


Direct3D prend également en charge les représentations de nombres en virgule flottante 16 bits.

Format :

-   1 bit de signe (s) à la position binaire du bit de poids fort (MSB)
-   5 bits d’exposant biaisé (e)
-   10 bits de fraction (f), avec un bit masqué supplémentaire

Une valeur float16 (v) respecte les règles suivantes :

-   Si e == 31 et f != 0, alors v est égal à NaN, quel que soit s.
-   if e == 31 and f == 0, then v = (-1)s\*infinity (signed infinity)
-   Si e est entre 0 et 31, v = (-1) s\*2(e-15)\*(1.f ne le)
-   Si e == 0 et f ! = 0, v = (-1) s\*2(e-14)\*(0.f) (nombres dénormalisés)
-   if e == 0 and f == 0, then v = (-1)s\*0 (signed zero)

Les règles en matière de virgule flottante 32 bits s’appliquent également aux nombres en virgule flottante 16 bits, et sont ajustées pour la disposition binaire précédemment décrite. Les exceptions à cette règle sont les suivantes :

-   Précision : Structure d’opérations sur les nombres à virgule flottante 16 bits produire un résultat qui est le plus proche de la valeur représentable en un résultat précis à l’infini (arrondi à la plus proche, par la norme IEEE-754, appliquée aux valeurs de 16 bits). Les règles en matière de virgule flottante 32 bits respectent une tolérance de 1 ULP, et celles en matière de virgule flottante 16 bits appliquent une tolérance de 0,5 ULP pour les opérations non fusionnées et de 0,6 ULP pour les opérations fusionnées.
-   Les nombres en virgule flottante 16 bits conservent les valeurs dénormalisées.

## <a name="span-idalpha11bitspanspan-idalpha11bitspan11-bit-and-10-bit-floating-point-rules"></a><span id="alpha_11_bit"></span><span id="ALPHA_11_BIT"></span>règles à virgule flottante 11 et 10 bits


Direct3D prend également en charge les formats de virgule flottante 11 et 10 bits.

Format :

-   Aucun bit de signe
-   5 bits d’exposant biaisé (e)
-   6 bits de fraction (f) pour un format 11 bits, 5 bits de fraction (f) pour un format 10 bits, avec un bit masqué supplémentaire dans les deux cas

Une valeur float11/float10 (v) respecte les règles suivantes :

-   Si e == 31 et f != 0, alors v est égal à NaN.
-   Si e == 31 et f == 0, alors v = +infini.
-   Si e est entre 0 et 31, v puis = 2(e-15)\*(1.f ne le)
-   Si e == 0 et f ! = 0, v = \*2(e-14)\*(0.f) (nombres dénormalisés)
-   Si e == 0 et f == 0, alors v = 0 (zéro).

Les règles en matière de virgule flottante 32 bits s’appliquent également aux nombres en virgule flottante 11 et 10 bits, et sont ajustées pour la disposition binaire précédemment décrite. Les exceptions sont les suivantes :

-   Précision : 0,5 ULP respectent les règles à virgule flottante 32 bits.
-   Les nombres en virgule flottante 10/11 bits conservent les valeurs dénormalisées.
-   Toute opération qui produirait un résultat inférieur à zéro est limitée à zéro.

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>Rubriques connexes


[Annexes](appendix.md)

[Ressources](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources)

[Textures](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 




