---
title: Intégration de jeux Unity dans UWP sur Xbox
description: Développement UWP Unity sur Xbox
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: fca3267a-0c0f-4872-8017-90384fb34215
ms.localizationpriority: medium
ms.openlocfilehash: 64a686aea24d23b5e999780eaa0eda661af3637f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611684"
---
# <a name="bringing-unity-games-to-uwp-on-xbox"></a>Intégration de jeux Unity dans UWP sur Xbox


Dans ce didacticiel détaillé, nous supposons que vous disposez déjà d’un jeu dans Unity, prêt à être créé et déployé.

Voir aussi une [version vidéo de ce didacticiel](https://www.youtube.com/watch?v=f0Ptvw7k-CE).

Vous cherchez à créer une version de votre projet UWP Unity ? Voir [Unity : Gestion de version de votre projet UWP](development-lanes-unity-versioning.md).

## <a name="step-0-ensure-unity-is-installed-correctly"></a>Étape 0 : Vérifiez Qu'unity est installé correctement

Les composants suivants doivent être sélectionnés lors de l’installation d’Unity :

![Composants d’installation Unity](images/unity-install-components.png)

## <a name="step-1-building-the-uwp-solution"></a>Étape 1 : Génération de la solution UWP

Dans votre projet de jeu Unity, ouvrez la fenêtre **Paramètres de génération**, qui se trouve dans **Fichier -&gt; Paramètres de build**, puis accédez au menu Options du Microsoft Store.

![Fenêtre Paramètres de build](images/build-settings.png)

Assurez-vous que le paramètre **SDK** est défini sur **Universel 10**. Cliquez ensuite sur le bouton **Générer** pour ouvrir une fenêtre Explorateur de fichiers qui vous demande un dossier de destination. Créez un dossier nommé **UWP** dans le répertoire **Assets** de votre projet et choisissez-le comme dossier de destination de la build.

![Dossier de destination de la build](images/build-destination.png)

Unity a ainsi créé une solution Visual Studio que nous utiliserons pour déployer votre jeu UWP.

![Solution Visual Studio UWP](images/uwp-vs-solution.png)

## <a name="step-2-deploying-your-game"></a>Étape 2 : Déploiement de votre jeu

Ouvrez la solution qui vient d’être créée dans le dossier **UWP**, puis modifiez la plateforme cible en **x64**.

![Plateforme de génération x64](images/x64-build-platform.png)

À présent que vous disposez d’une solution Visual Studio UWP pour votre jeu, [suivez ces étapes](getting-started.md) pour déployer correctement votre jeu vers votre Xbox One achetée au détail !

## <a name="step-3-modify-and-rebuild"></a>Étape 3 : Modifier et régénérer

Si des modifications sont apportées à des éléments ne figurant pas dans le script, pour que ces modifications s’affichent dans la build UWP de votre jeu, le projet doit être régénéré à partir de l’éditeur (comme décrit à l’__Étape 1__).

## <a name="versioning-your-uwp-project"></a>Gestion de version de votre projet UWP

Il existe quelques situations courantes où l’ajout de parties de ce répertoire UWP nouvellement créé à la gestion de version devient nécessaire. Par exemple, si vous ajoutez une nouvelle dépendance au projet UWP (par exemple, le Kit SDK Xbox Live).  Cet exemple est étudié en détail dans [Unity : Gestion de version de votre projet UWP](development-lanes-unity-versioning.md).

## <a name="see-also"></a>Voir également
- [Intégration de jeux existants dans Xbox](development-lanes-landing.md)
- [UWP sur Xbox One](index.md)
