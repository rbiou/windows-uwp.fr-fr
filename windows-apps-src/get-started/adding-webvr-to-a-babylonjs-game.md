---
title: Ajout de prise en charge WebVR à un jeu de Babylon.js 3D
description: Découvrez comment ajouter la prise en charge WebVR à un jeu Babylon.js 3D existant.
author: abbycar
ms.author: abigailc
ms.date: 11/29/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: webvr, edge, développement web, babylon, babylonjs, babylon.js, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 41665e8719493bb658f9926947061b1b5f81a139
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1018649"
---
# <a name="adding-webvr-support-to-a-3d-babylonjs-game"></a>Ajout de prise en charge WebVR à un jeu de Babylon.js 3D

Si vous avez créé un jeu en 3D avec Babylon.js et penser qu’il pourrait se présenter excellent dans virtuelle réalité, suivez les étapes simples dans ce didacticiel pour faire une réalité.

Nous allons ajouter la prise en charge WebVR au jeu indiqué ici. Continuez et branchez un contrôleur Xbox essayer!


<iframe height='300' scrolling='no' title='Jeu de dino Babylon.js à l’aide de Babylon.GUI' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/wrOvoj/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Voir le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/wrOvoj/'>Babylon.js dino jeu à l’aide de Babylon.GUI</a> par Microsoft Edge documents (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>

Il s’agit d’un jeu en 3D qui fonctionne bien sur un écran plat, mais qu’à propos de VR?
Dans ce didacticiel, nous allons les étapes qu’il utilise pour obtenir et en cours d’exécution avec WebVR. Nous allons utiliser un casque [Réalité mixte Windows](https://developer.microsoft.com/en-us/windows/mixed-reality) qui permettre exploiter la prise en charge supplémentaire pour WebVR dans Microsoft Edge. Une fois que nous appliquer ces modifications à la partie, vous pouvez vous attendre à utiliser dans d’autres combinaisons de navigateur/casque qui prennent en charge WebVR.



## <a name="prerequisites"></a>Conditions préalables

- Un éditeur de texte (comme le [Code Visual Studio](https://code.visualstudio.com/download))
- Un contrôleur Xbox qui est connecté à votre ordinateur
- Windows10 Creators Update
- Un ordinateur avec les [spécifications requises minimales pour exécuter Windows mixte réalité](https://developer.microsoft.com/en-us/windows/mixed-reality/immersive_headset_setup)
- Un périphérique réalité mixte Windows (facultatif) 



## <a name="getting-started"></a>Prise en main

La plus simple pour commencer consiste à visiter [web Windows-didacticiels référentiels emprunteuses](https://github.com/Microsoft/Windows-tutorials-web), appuyez sur le vert **Clone ou télécharger** bouton, puis sélectionnez **Ouvrir dans Visual Studio**.

![Bouton Cloner ou télécharger](images/3dclone.png)

Si vous ne voulez pas cloner le projet, vous pouvez le télécharger sous forme de fichier zip.
Vous devez ensuite deux dossiers, [avant](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) et [après](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/after). Le dossier «avant» est notre jeu avant toutes les fonctionnalités VR sont ajoutées, et le dossier «après» est le jeu terminé avec prise en charge VR.

L’avant et après les dossiers contenant ces fichiers:
-   **textures /** - un dossier contenant des images utilisées dans le jeu.
-   **css /** - un dossier contenant le fichier CSS de la partie.
-   **js /** - un dossier contenant les fichiers JavaScript. Le fichier main.js est notre jeu et les autres fichiers sont les bibliothèques utilisées.
-   **modèles /** - un dossier contenant les modèles 3D. Pour ce jeu, nous n'avons qu’un seul modèle, pour les dinosaures.
-   **index.html** - la page Web qui héberge le convertisseur du jeu. Ouverture de cette page dans Microsoft Edge lance le jeu.

Vous pouvez tester les deux versions du jeu en ouvrant leurs fichiers respectifs index.html dans Microsoft Edge.



## <a name="the-mixed-reality-portal"></a>Le portail réalité mixte

Si vous n’êtes pas familiarisé avec la réalité mixte Windows et le 10: les créateurs de mise à jour Windows installés sur un ordinateur avec une carte graphique compatible, essayez d’ouvrir l’application **Mixte réalité portail** à partir du menu Démarrer de Windows 10.

![Recherche de portail réalité mixte](images/mixed-reality-portal.png)

Si vous remplissez toutes les conditions requises, vous pouvez activer les fonctionnalités de développement et simuler un casque réalité mixte Windows connecté à votre ordinateur. Si vous êtes chance un casque réel à proximité, branchez-le et exécuter le programme d’installation.

> [!IMPORTANT]
> Le portail de réalité mixte doit être ouvert à tout moment au cours de ce didacticiel.

Vous êtes maintenant prêt à rencontrer WebVR avec Microsoft Edge.

## <a name="2d-ui-in-a-virtual-world"></a>Interface utilisateur 2D dans un environnement virtuel

>[!NOTE]
> Récupérer le dossier [**avant**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) pour obtenir l’exemple de démarrage.

[Babylon.GUI](https://doc.babylonjs.com/how_to/gui) est une bibliothèque VR conviviale, ce qui vous pour créer de simples, d’interfaces utilisateur interactif qui fonctionnent correctement pour VR non-VR affiche.
Une extension de Babylon.js, le `GUI` bibliothèque est utilisée throuhout l’exemple pour créer des éléments 2D.


Un texte 2D `GUI` élément peut être créé avec quelques lignes en fonction du nombre d’attributs vous souhaitez modifier.
L’extrait de code suivant est déjà dans notre exemple [**avant**](https://github.com/Microsoft/Windows-tutorials-web/tree/master/BabylonJS-game-with-WebVR/before) , mais nous allons procédure pas à pas, ce qui se passe.
Nous avons d’abord effectuer une [`AdvancedDynamicTexture`](https://doc.babylonjs.com/how_to/gui#advanceddynamictexture) objet pour établir une relation de l’interface utilisateur graphique couvrira. L’exemple définit ce sur `CreateFullScreenUI()`, ce qui signifie que nos l’interface utilisateur va couvrir la totalité de l’écran. Avec `AdvancedDynamicTexture` créé, puis faire une zone de texte 2D qui s’affiche au démarrage du jeu à l’aide `GUI.Rectanlge()` et `GUI.TextBlock()`.


Ce code est ajouté au sein de [**main.js**](https://github.com/Microsoft/Windows-tutorials-web/blob/master/BabylonJS-game-with-WebVR/before/js/main.js#L157-L168).
```javascript
// GUI
var advancedTexture = BABYLON.GUI.AdvancedDynamicTexture.CreateFullscreenUI("UI");

// Start UI
startUI = new BABYLON.GUI.Rectangle("start");
startUI.background = "black"
startUI.alpha = .8;
startUI.thickness = 0;
startUI.height = "60px";
startUI.width = "400px";
advancedTexture.addControl(startUI); 
var tex2 = new BABYLON.GUI.TextBlock();
tex2.text = "Stay away from the dinosaur! \n Plug in an Xbox controller and press A to start";
tex2.color = "white";
startUI.addControl(tex2); 
```


Cette interface est visible une fois créée, mais peuvent être activées ou désactivées avec `isVisible` selon ce qui se passe dans le jeu.
```javascript
startUI.isVisible = false;
```



## <a name="detecting-headsets"></a>Détection de casques

Il est recommandé pour les applications VR avoir deux types de caméras afin que plusieurs scénarios peuvent être pris en charge. Pour ce jeu, nous allons prend en charge une caméra qui requiert un casque de travail doit être branché et un autre qui n’utilise aucun casque. Pour déterminer le jeu utiliserez, nous devons d’abord vérifier pour voir si un casque a été détecté. Pour ce faire, nous allons utiliser [`navigator.getVRDisplays()`](https://developer.mozilla.org/en-US/docs/Web/API/Navigator/getVRDisplays).


Ajoutez le code ci-dessus `window.addEventListener('DOMContentLoaded')` dans **main.js**.
```javascript
var headset;
// If a VR headset is connected, get its info
navigator.getVRDisplays().then(function (displays) {
    if (displays[0]) {
        headset = displays[0];
    }
});
```

Avec les informations stockées dans les `headset` variable, nous allons maintenant pouvoir choisir la caméra est un bon choix pour l’utilisateur.


## <a name="creating-and-selecting-the-initial-camera"></a>Création et en sélectionnant la caméra initiale

Babylon.js, WebVR permettre être ajouté rapidement à l’aide de le [`WebVRFreeCamera`](http://doc.babylonjs.com/classes/3.1/webvrfreecamera). Cette caméra peut prendre l’entrée au clavier et vous permet d’utiliser un casque VR pour contrôler la rotation de la «tête».


### <a name="step-1-checking-for-headsets"></a>Étape 1: Vérification des casques

Pour notre secours caméra, nous utilisons la [`UniversalCamera`](https://doc.babylonjs.com/classes/3.1/universalcamera) qui est actuellement utilisée dans le jeu d’origine.

Nous vérifions nos `headset` variable pour déterminer si nous pouvons utiliser la `WebVRFreeCamera` caméra.

Remplacez `camera = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);` par le code suivant.
```javascript
        if(headset){
            // Create a WebVR camera with the trackPosition property set to false so that we can control movement with the gamepad
            camera = new BABYLON.WebVRFreeCamera("vrcamera", new BABYLON.Vector3(0, 14, 0), scene, true, { trackPosition: false });
            camera.deviceScaleFactor = 1;
        } else {
            // No headset, use universal camera
            camera = new BABYLON.UniversalCamera("camera", new BABYLON.Vector3(0, 18, -45), scene);
        }
```


### <a name="step-2-activating-the-webvrfreecamera"></a>Étape 2: Activation de la WebVRFreeCamera
Pour activer cette caméra dans la plupart des navigateurs, l’utilisateur doit effectuer une intervention qui demande l’expérience virtuel.
Nous allons raccordées cette fonctionnalité jusqu'à un clic de souris.


Collez le code dans `createScene()` fonctionner après `camera.applyGravity = true;` .
```javascript
        scene.onPointerDown = function () {
            scene.onPointerDown = undefined
            camera.attachControl(canvas, true);
        }
```

Un clic sur le jeu maintenant crée une invite de commandes, comme ci-dessous, ou affiche le jeu dans le casque immédiatement si l’utilisateur a accepté l’invite avant.

![invite immersive](images/immersiveview.png)

Nous pouvons également ajouter un morceau de code qui permet d’afficher le le `UniversalCamera` afficher nous basculer de notre `WebVRFreeCamera`, permettant à l’utilisateur d’examiner le jeu au lieu d’une fenêtre bleue. 

Ajoutez le code suivant après `engine.runRenderLoop(function () {`.
```javascript
            if (headset) {
                if (!(headset.isPresenting)) {
                    var camera2 = new BABYLON.UniversalCamera("Camera", new BABYLON.Vector3(0, 18, -45), scene);
                    scene.activeCamera = camera2;
                } else {
                    scene.activeCamera = camera;
                }
            }
```

### <a name="step-3-adding-gamepad-support"></a>Étape 3: Ajout de prise en charge du boîtier de commande

Dans la mesure où le `WebVRFreeCamera` initialement ne gère pas les boîtiers de commande, nous allons mapper notre boutons boîtier sur les touches de direction du clavier. Nous effectuerons cela en examinant le `inputs` propriété de l’appareil photo. En ajoutant les codes correspondants pour module analogique gauche, haut, bas, gauche et droite pour faire coïncider avec les touches de direction, notre boîtier est à le œuvre.


Ajoutez le code ci-dessous les `scene.onPointerDown = function() {...}` appel.
``` javascript
    // Custom input, adding Xbox controller support for left analog stick to map to keyboard arrows
    camera.inputs.attached.keyboard.keysUp.push(211);    // Left analog up
    camera.inputs.attached.keyboard.keysDown.push(212);  // Left analog down
    camera.inputs.attached.keyboard.keysLeft.push(214);  // Left analog left
    camera.inputs.attached.keyboard.keysRight.push(213); // Left analog right
```


### <a name="step-4-give-it-a-try"></a>Étape 4: Faites un essai!

Si nous ouvrir **index.html** avec notre casque et contrôleur branché, un clic gauche de la fenêtre du jeu bleu passe notre jeu en mode VR! Continuez et placer votre casque pour extraire les résultats. 


<iframe height='300' scrolling='no' title='Jeu de dino Babylon.js à l’aide de Babylon.GUI - WebVR' src='//codepen.io/MicrosoftEdgeDocumentation/embed/preview/RjgpJd/?height=300&theme-id=23761&default-tab=result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Voir le stylet <a href='https://codepen.io/MicrosoftEdgeDocumentation/pen/RjgpJd/'>Babylon.js dino jeu à l’aide de Babylon.GUI - WebVR</a> par Microsoft Edge documents (<a href='https://codepen.io/MicrosoftEdgeDocumentation'>@MicrosoftEdgeDocumentation</a>) sur <a href='https://codepen.io'>CodePen</a>.
</iframe>


## <a name="conclusion"></a>Conclusion

Félicitations! Vous avez maintenant un jeu complet de Babylon.js avec prise en charge WebVR. À partir de là, vous pouvez effectuer ce que vous avez appris créer un jeu encore mieux, ou génération celle-ci.