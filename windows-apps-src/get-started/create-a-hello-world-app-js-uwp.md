---
ms.assetid: 3a17e682-40be-41b4-8bd3-fbf0b15259d6
title: Créer une application « Hello World » (JS)
description: Ce didacticiel vous explique comment utiliser JavaScript et HTML pour créer une simple application &\#0034;Hello, world&\#0034; ciblant la plateforme Windows universelle (UWP) sur Windows 10.
ms.date: 09/12/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 88ae290bf88a21aa2846697833d099df663915f6
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492964"
---
# <a name="create-a-hello-world-app-js"></a>Créer une application « Hello, World! » (JS)

Ce didacticiel vous explique comment utiliser JavaScript et HTML pour créer une simple application « Hello World » ciblant la plateforme Windows universelle (UWP) sur Windows 10. Dans Microsoft Visual Studio, un seul projet vous permet de générer une application qui s’exécute sur n’importe quel appareil Windows 10.

> [!NOTE]
> Ce didacticiel utilise Visual Studio Community 2017. Si vous utilisez une autre version de Visual Studio, son aspect peut être légèrement différent.

> [!WARNING]
> Le développement d’applications UWP JavaScript n’est pas pris en charge dans Visual Studio 2019. Vous devez utiliser Visual Studio 2017 pour développer une application UWP JavaScript.

Dans cet article, vous allez apprendre à :

-   Créer un projet **Visual Studio 2017** qui cible **Windows 10** et **UWP**.
-   Ajouter du contenu HTML et JavaScript.
-   exécuter le projet sur l’ordinateur local dans Visual Studio.

## <a name="before-you-start"></a>Avant de commencer

-   [Qu’est-ce qu’une application UWP ?](universal-application-platform-guide.md)
-   Pour suivre ce didacticiel, vous avez besoin de Windows 10 et de Visual Studio. [Se préparer](get-set-up.md).
-   Nous partons également du principe que vous utilisez la disposition de fenêtre par défaut de Visual Studio. Si vous modifiez la disposition par défaut, vous pouvez la réinitialiser dans le menu **Fenêtre** en choisissant la commande **Rétablir la disposition de fenêtre**.

## <a name="step-1-create-a-new-project-in-visual-studio"></a>Étape 1 : Créer un projet dans Visual Studio.

1.  Lancez Visual Studio 2017.

2.  Dans le menu **Fichier**, sélectionnez **Nouveau > Projet** pour ouvrir la boîte de dialogue **Créer un projet**.

3.  Choisissez **Application vide (Windows universel) JavaScript**, puis sélectionnez **Suivant**.

    (Si aucun modèle universel n’apparaît, c’est qu’il vous manque les composants permettant de créer des applications UWP. Vous pouvez répéter la procédure d’installation et ajouter la prise en charge UWP en sélectionnant **Ouvrir le programme d’installation de Visual Studio** dans la boîte de dialogue **Créer un projet**. Consultez [Préparation](get-set-up.md).

4.  Dans la boîte de dialogue **Configurer votre nouveau projet**, entrez **HelloWorld** comme **Nom de projet**, puis sélectionnez **Créer**.

> [!NOTE]
> Si vous utilisez Visual Studio pour la première fois, il est possible que la boîte de dialogue Paramètres s'affiche et vous demande d'activer le **Mode développeur**. Le mode développeur est un paramètre qui permet d’accéder à certaines fonctionnalités, telles que l’autorisation d’exécuter des applications directement plutôt qu’uniquement à partir du Store. Pour plus d’informations, consultez [Activer votre appareil pour le développement](enable-your-device-for-development.md). Pour continuer avec ce guide, sélectionnez le **Mode développeur**, sélectionnez **Oui**, puis fermez la boîte de dialogue.

 ![Boîte de dialogue d’activation du mode développeur](images/win10-cs-00.png)

5.  La boîte de dialogue Version cible/Version minimale s’affiche. Les paramètres par défaut étant appropriés pour ce didacticiel, sélectionnez **OK** pour créer le projet.

    ![Fenêtre Explorateur de solutions](images/win10-cs-02.png)

6.  Votre nouveau projet s’ouvre en affichant ses fichiers dans le volet **Explorateur de solutions**, à droite. Vous devrez peut-être choisir l’onglet **Explorateur de solutions** à la place de l’onglet **Propriétés** pour voir vos fichiers.

    ![Fenêtre Explorateur de solutions](images/win10-js-02.png)

Même si le modèle **Application vide (Windows universel)** est dépouillé, il contient cependant de nombreux fichiers. Ces fichiers sont indispensables pour toutes les applications UWP en JavaScript. Ils se trouvent dans tous les projets que vous créez dans Visual Studio.


### <a name="whats-in-the-files"></a>Que contiennent les fichiers ?

Pour afficher et modifier un fichier de votre projet, double-cliquez dessus dans l’**Explorateur de solutions**.

*default.css*

-  Feuille de style par défaut utilisée par l’application.

*main.js*

- Fichier JavaScript par défaut. Il est référencé dans le fichier index.html.

*index.html*

- Page de l’application web qui se charge et s’affiche lors du lancement de l’application.

*Ensemble d’images de logo*
-   Assets/Square150x150Logo.scale-200.png représente votre application dans le menu **Démarrer**.
-   Assets/StoreLogo.png représente votre application dans le Microsoft Store.
-   Assets/SplashScreen.scale-200.png est l’écran de démarrage qui s’affiche quand votre application démarre.

## <a name="step-2-adding-a-button"></a>Étape 2 : Ajouter un bouton

Sélectionnez **index.html** pour le sélectionner dans l’éditeur et remplacez le code HTML qu’il contient par ce qui suit.

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <title>Hello World</title>
    <script src="js/main.js"></script>
    <link href="css/default.css" rel="stylesheet" />
</head>

<body>
    <p>Click the button..</p>
    <button id="button">Hello world!</button>
</body>

</html>
```

Il doit se présenter ainsi.

 ![Code HTML du projet](images/win10-js-03.png)

Ce code HTML fait référence au fichier *main.js* qui contiendra notre code JavaScript. Il ajoute une seule ligne de texte et un bouton unique dans le corps de la page web. Un *ID* est attribué au bouton afin que le code JavaScript puisse le référencer.


## <a name="step-3-adding-some-javascript"></a>Étape 3 : Ajout de code JavaScript

Nous allons maintenant ajouter le code JavaScript. Sélectionnez **main.js** pour le sélectionner, puis ajoutez ce qui suit.

```javascript
// Your code here!

window.onload = function () {
    document.getElementById("button").onclick = function (evt) {
        sayHello()
    }
}


function sayHello() {
    var messageDialog = new Windows.UI.Popups.MessageDialog("Hello, world!", "Alert");
    messageDialog.showAsync();
}

```

Il doit se présenter ainsi.

 ![Code JavaScript du projet](images/win10-js-04.png)

Ce code JavaScript déclare deux fonctions. La fonction *window.onload* est appelée automatiquement lorsque le fichier *index.html* s’affiche. Il trouve le bouton (à l’aide de l’ID que nous avons déclaré) et ajoute un gestionnaire onclick : la méthode qui sera appelée lorsque l’utilisateur cliquera sur le bouton.

La deuxième fonction, *sayHello()* , crée et affiche une boîte de dialogue. Elle ressemble beaucoup à la fonction *Alert()* que vous connaissez peut-être d’un précédent développement JavaScript.


## <a name="step-4-run-the-app"></a>Étape 4 : Exécuter l’application.

Vous pouvez maintenant exécuter l’application en appuyant sur F5. L’application se charge, la page web s’affiche. Sélectionnez le bouton pour que la boîte de dialogue apparaisse.

 ![Exécution du projet](images/win10-js-05.png)



## <a name="summary"></a>Résumé


Félicitations ! Vous venez de créer une application JavaScript pour Windows 10 et UWP. Cet exemple est ridiculement simple, mais vous pouvez désormais commencer à ajouter vos bibliothèques et infrastructures JavaScript préférées pour créer votre propre application. Comme il s’agit d’une application UWP, vous pouvez la publier dans le Store. Pour obtenir un exemple d’ajout d’infrastructures tierces, consultez les projets suivants :

* [Jeu UWP 2D simple pour le Microsoft Store, écrit en JavaScript et CreateJS](get-started-tutorial-game-js2d.md)
* [Jeu UWP 3D pour le Microsoft Store, écrit en JavaScript et threeJS](get-started-tutorial-game-js3d.md)
