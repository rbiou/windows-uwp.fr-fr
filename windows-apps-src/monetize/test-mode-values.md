---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: Utilisez les valeurs de test d’ID d’application et d’ID d’unité publicitaire pour voir comment votre application restitue les publicités au cours du test.
title: Valeurs du mode test
---

# Valeurs du mode test


\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132). \]

Lorsque vous utilisez une classe [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) pour afficher des publicités dans votre application, vous devez spécifier un ID d’application et un ID d’unité publicitaire. Lors du développement de votre application, utilisez les valeurs de test d’ID d’application et d’ID d’unité publicitaire de cet article pour voir comment votre application restitue les publicités au cours du test.

> **Important** Si votre application utilise la médiation publicitaire (elle utilise un objet **AdMediatorControl**), il n’est pas nécessaire de demander des unités publicitaires. Dans ce scénario, les unités publicitaires sont automatiquement générées pour vous. Pour plus d’informations, voir [Quelle est la différence entre AdMediatorControl et AdControl ?](what-is-the-difference-admediatorcontrol-or-adcontrol.md).

Si vous essayez d’utiliser des valeurs de test dans votre application après l’avoir publiée, votre application dynamique ne recevra pas de publicités. Pour recevoir des publicités dans votre application publiée, vous devez mettre à jour votre code pour utiliser un ID de l’application et un ID d’unité publicitaire fournis par le tableau de bord du Centre de développement Windows. Pour plus d’informations, voir [Configurer des unités publicitaires dans votre application](set-up-ad-units-in-your-app.md).
 

Voici les valeurs de test à utiliser pour les bannières et les spots vidéo publicitaires.

* Spots vidéo publicitaires :

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>11389925</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    </tbody>
    </table>

     
* Bannières publicitaires :

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>10865270</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    </tbody>
    </table>


> **Important** La taille d’une publicité dynamique est définie par les propriétés **Width** et **Height** du **AdControl**. Pour obtenir de meilleurs résultats, vérifiez que les propriétés **Width** et **Height** de votre code font partie des [tailles de bannières publicitaires prises en charge](supported-ad-sizes-for-banner-ads.md). Les propriétés **Width** et **Height** ne changent pas en fonction de la taille d’une publicité dynamique.



 

 


<!--HONumber=May16_HO2-->

