---
title: Boîtier de commande et vibration
description: Utilisez les API de boîtier de commande Windows.Gaming.Input pour détecter et lire les commandes de vibration et d’impulsion, et les envoyer aux boîtiers de commande.
ms.assetid: BB03BB8E-255F-4AE8-AC43-1E519CA860FE
ms.date: 09/06/2018
ms.topic: article
keywords: windows 10, uwp, jeux, boîtier de commande, vibration
ms.localizationpriority: medium
ms.openlocfilehash: e65b22039c381bd333516bd9f98c60bbddb9621c
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210595"
---
# <a name="gamepad-and-vibration"></a>Boîtier de commande et vibration

Cet article explique les notions de base de la programmation pour les boîtiers de commande Xbox One avec l’API [Windows.Gaming.Input.Gamepad][gamepad] et les API associées pour la plateforme Windows universelle (UWP).

Voici ce que vous allez apprendre à la lecture de cet article :

* Obtenir une liste des boîtiers de commande connectés et de leurs utilisateurs
* Détecter l’ajout ou la suppression d’un boîtier de commande
* Lire les entrées provenant d’un ou de plusieurs boîtiers de commande
* Envoyer des commandes de vibration et d’impulsion
* comportement des boîtiers en tant qu’appareils de navigation dans l’interface utilisateur

## <a name="gamepad-overview"></a>Vue d’ensemble des boîtiers de commande

Les boîtiers de commande tels que les manettes sans fil Xbox Wireless Controller et Xbox Wireless Controller S sont des périphériques d’entrée de jeu à usage général. Ils représentent le périphérique d’entrée standard sur Xbox One et ils sont couramment choisis par les joueurs Windows qui ne préfèrent pas utiliser un clavier et une souris. Les boîtiers de commande sont pris en charge dans les applications UWP Windows 10 et Xbox par l’espace de noms [Windows. Gaming. Input][].

Les boîtiers de manette Xbox One sont équipés d’un pavé directionnel (ou D-PAD). Les boutons **A**, **B**, **X**, **Y**, **vue**et **menu** ; Thumbsticks, champignons et déclencheurs de gauche et de droite ; et un total de quatre moteurs vibratoires. Les deux sticks analogiques fournissent des valeurs analogiques doubles sur les axes X et Y, et fonctionnent également comme un bouton quand l’utilisateur appuie dessus vers l’intérieur. Chaque déclencheur fournit une lecture analogique qui représente la distance à laquelle il est extrait.

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **Paddle** buttons on its underside. These can be used to provide redundant access to game commands that are difficult to use together (such as the right thumbstick together with any of the **A**, **B**, **X**, or **Y** buttons) or to provide dedicated access to additional commands. -->

> [!NOTE]
> `Windows.Gaming.Input.Gamepad` prend également en charge les boîtiers de commande Xbox 360, qui ont la même disposition de contrôle que les boîtiers de commande Xbox standard One.

### <a name="vibration-and-impulse-triggers"></a>Gâchettes de vibration et à impulsions

Les boîtiers de commande Xbox One ont deux moteurs indépendants qui déclenchent des vibrations fortes et légères, ainsi que deux moteurs dédiés qui produisent des vibrations rapides en série au niveau de chaque gâchette (d’où le nom de _gâchette à impulsions_, qui est une fonctionnalité spécifique au boîtier de commande Xbox One).

> [!NOTE]
> Les boîtiers de manette Xbox 360 ne sont pas équipés de _déclencheurs d’impulsions_.

Pour plus d’informations, consultez [Vue d’ensemble des gâchettes de vibration et à impulsions](#vibration-and-impulse-triggers-overview).

### <a name="thumbstick-deadzones"></a>Zones mortes (ou « dead zones ») des sticks analogiques

Quand un stick analogique se trouve à la position centrale, il devrait idéalement toujours fournir la même valeur neutre sur les axes X et Y. Or, en raison de certaines forces mécaniques et de la sensibilité du stick analogique, les valeurs réelles fournies à la position centrale ne correspondent pas exactement à la valeur neutre idéale et peuvent varier d’une lecture de valeur à une autre. Pour cette raison, vous devez toujours utiliser un petit _deadzone_&mdash;une plage de valeurs près de la position centrale idéale ignorées&mdash;pour compenser les différences de fabrication, l’usure mécanique ou d’autres problèmes de boîtier de manette.

L’utilisation de zones mortes plus larges est une solution simple pour faire la distinction entre les entrées intentionnelles et les entrées accidentelles.

Pour plus d’informations, consultez [Lecture des entrées des sticks analogiques](#reading-the-thumbsticks).

### <a name="ui-navigation"></a>Navigation dans l'interface utilisateur

Pour réduire la difficulté associée à la prise en charge de plusieurs périphériques d’entrée différents lors de la navigation dans l’interface utilisateur, et pour favoriser la cohérence entre les jeux et les périphériques, la plupart des périphériques d’entrée _physiques_ agissent en tant que périphérique d’entrée _logique_ distinct, appelé [contrôleur de navigation d’interface utilisateur](ui-navigation-controller.md). Le contrôleur de navigation d’interface utilisateur fournit un vocabulaire commun pour les commandes de navigation dans l’interface utilisateur, sur plusieurs périphériques d’entrée.

En tant que contrôleur de navigation de l’interface utilisateur, les boîtiers de commande mappent le [jeu](ui-navigation-controller.md#required-set) de commandes de navigation requis sur les boutons du joystick gauche, du pavé D-Pad, **de**l' **affichage**, du **menu**, du et du **B** .

| Commande de navigation | Entrée boîtier de commande                       |
| ------------------:| ----------------------------------- |
|                 Monter | Stick analogique gauche vers le haut / bouton multidirectionnel vers le haut       |
|               Bas | Stick analogique gauche vers le bas / bouton multidirectionnel vers le bas   |
|               Left | Stick analogique gauche vers la gauche / bouton multidirectionnel vers la gauche   |
|              Droit | Stick analogique gauche vers la droite / bouton multidirectionnel vers la droite |
|               Affichage | Bouton Afficher                         |
|               Menu | Bouton Menu                         |
|             Accepter | Bouton A                            |
|             Annuler | Bouton B                            |

De plus, les boîtiers de commande mappent toutes les commandes de navigation de l’[ensemble facultatif](ui-navigation-controller.md#optional-set) aux entrées restantes.

| Commande de navigation | Entrée boîtier de commande          |
| ------------------:| ---------------------- |
|            Page précédente | Gâchette gauche           |
|          Page suivante | Gâchette droite          |
|          Page vers la gauche | Gâchette haute gauche            |
|         Page vers la droite | Gâchette haute droite           |
|          Faire défiler vers le haut | Stick analogique droit vers le haut    |
|        Faire défiler vers le bas | Stick analogique droit vers le bas  |
|        Faire défiler vers la gauche | Stick analogique droit vers la gauche  |
|       Faire défiler vers la droite | Stick analogique droit vers la droite |
|          Contexte 1 | Bouton X               |
|          Contexte 2 | Bouton Y               |
|          Contexte 3 | Appui sur stick analogique gauche  |
|          Contexte 4 | Appui sur stick analogique droit |

## <a name="detect-and-track-gamepads"></a>Détecter et suivre des boîtiers de commande

Comme les boîtiers de commande sont gérés par le système, vous n’avez pas à les créer ni à les initialiser. Le système fournit une liste des boîtiers de commande connectés et des événements de notification d’ajout ou de suppression d’un boîtier de commande.

### <a name="the-gamepads-list"></a>Liste des boîtiers de commande

La classe [Gamepad][] fournit la propriété statique [Gamepads][], qui correspond à une liste en lecture seule de tous les boîtiers de commande qui sont actuellement connectés. Étant donné que vous pouvez être intéressé uniquement par certains des boîtiers connectés, il est recommandé de conserver votre propre collection au lieu d’y accéder via la propriété `Gamepads`.

L’exemple de code suivant copie tous les boîtiers de commande connectés dans une nouvelle collection. Notez que, étant donné que les autres threads en arrière-plan accèdent à cette collection (dans les événements [GamepadAdded][] et [GamepadRemoved][] ), vous devez placer un verrou autour du code qui lit ou met à jour la collection.

```cpp
auto myGamepads = ref new Vector<Gamepad^>();
critical_section myLock{};

for (auto gamepad : Gamepad::Gamepads)
{
    // Check if the gamepad is already in myGamepads; if it isn't, add it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), gamepad);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all gamepads.
        myGamepads->Append(gamepad);
    }
}
```

```cs
private readonly object myLock = new object();
private List<Gamepad> myGamepads = new List<Gamepad>();
private Gamepad mainGamepad;

private void GetGamepads()
{
    lock (myLock)
    {
        foreach (var gamepad in Gamepad.Gamepads)
        {
            // Check if the gamepad is already in myGamepads; if it isn't, add it.
            bool gamepadInList = myGamepads.Contains(gamepad);

            if (!gamepadInList)
            {
                // This code assumes that you're interested in all gamepads.
                myGamepads.Add(gamepad);
            }
        }
    }   
}
```

### <a name="adding-and-removing-gamepads"></a>Ajout et suppression de boîtiers de commande

Lors de l’ajout ou de la suppression d’un boîtier, les événements [GamepadAdded][] et [GamepadRemoved][] sont déclenchés. Vous pouvez inscrire des gestionnaires pour ces deux événements afin d’effectuer le suivi des boîtiers de commande actuellement connectés.

L’exemple de code suivant démarre le suivi d’un boîtier de commande qui a été ajouté.

```cpp
Gamepad::GamepadAdded += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    critical_section::scoped_lock lock{ myLock };
    auto it = std::find(begin(myGamepads), end(myGamepads), args);

    if (it == end(myGamepads))
    {
        // This code assumes that you're interested in all new gamepads.
        myGamepads->Append(args);
    }
}
```

```cs
Gamepad.GamepadAdded += (object sender, Gamepad e) =>
{
    // Check if the just-added gamepad is already in myGamepads; if it isn't, add
    // it.
    lock (myLock)
    {
        bool gamepadInList = myGamepads.Contains(e);

        if (!gamepadInList)
        {
            myGamepads.Add(e);
        }
    }
};
```

L’exemple suivant arrête le suivi d’un boîtier de commande qui a été supprimé. Vous devez également gérer ce qui se passe aux boîtiers de souscription que vous suivez lorsqu’ils sont supprimés. par exemple, ce code effectue uniquement le suivi de l’entrée d’un boîtier de sélection et le définit simplement sur `nullptr` lorsqu’il est supprimé. Vous devez vérifier chaque frame si votre boîtier de connexion est actif, puis mettre à jour le boîtier d’enregistrement à partir duquel vous collectez des données lorsque les contrôleurs sont connectés et déconnectés.

```cpp
Gamepad::GamepadRemoved += ref new EventHandler<Gamepad^>(Platform::Object^, Gamepad^ args)
{
    unsigned int indexRemoved;
    critical_section::scoped_lock lock{ myLock };

    if(myGamepads->IndexOf(args, &indexRemoved))
    {
        if (m_gamepad == myGamepads->GetAt(indexRemoved))
        {
            m_gamepad = nullptr;
        }

        myGamepads->RemoveAt(indexRemoved);
    }
}
```

```cs
Gamepad.GamepadRemoved += (object sender, Gamepad e) =>
{
    lock (myLock)
    {
        int indexRemoved = myGamepads.IndexOf(e);

        if (indexRemoved > -1)
        {
            if (mainGamepad == myGamepads[indexRemoved])
            {
                mainGamepad = null;
            }

            myGamepads.RemoveAt(indexRemoved);
        }
    }
};
```

Pour plus d’informations, consultez [pratiques d’entrée pour les jeux](input-practices-for-games.md) .

### <a name="users-and-headsets"></a>Utilisateurs et casques

Chaque boîtier de commande peut être associé à un compte d’utilisateur pour lier l’identité de l’utilisateur à son jeu, et peut être branché à un casque permettant d’utiliser plus facilement les fonctionnalités de chat vocal et dans le jeu. Pour en savoir plus sur le suivi des utilisateurs et l’utilisation de casques, voir [Suivi des utilisateurs et de leurs périphériques](input-practices-for-games.md#tracking-users-and-their-devices) et [Casques](headset.md).

## <a name="reading-the-gamepad"></a>Lecture des entrées du boîtier de commande

Une fois que vous avez identifié le boîtier de commande qui vous intéresse, vous pouvez commencer à collecter les entrées de ce boîtier. Toutefois, contrairement à d’autres sortes d’entrées que vous connaissez peut-être, les boîtiers de commande ne communiquent pas les changements d’état en déclenchant des événements. À la place, vous devez _interroger_ régulièrement ces boîtiers de commande pour connaître leur état actuel.

### <a name="polling-the-gamepad"></a>Interrogation du boîtier de commande

Le processus d’interrogation capture un instantané du périphérique de navigation à un moment précis. Cette approche de collecte des entrées convient à la plupart des jeux, dont la logique s’exécute généralement selon une boucle déterministe plutôt que sur la base des événements. Par ailleurs, elle facilite l’interprétation des commandes de jeu, car toutes les entrées sont collectées en même temps, et non pas les unes après les autres.

Vous interrogez un boîtier de commande en appelant la fonction [GetCurrentReading][]. Cette fonction renvoie un élément [GamepadReading][] qui contient l’état du boîtier de commande.

L’exemple de code suivant interroge un boîtier de commande pour obtenir son état actuel.

```cpp
auto gamepad = myGamepads[0];

GamepadReading reading = gamepad->GetCurrentReading();
```

```cs
Gamepad gamepad = myGamepads[0];

GamepadReading reading = gamepad.GetCurrentReading();
```

En plus de l’état du boîtier de commande, chaque valeur comprend un horodatage qui indique précisément le moment d’extraction de cet état. Cet horodatage est utile pour faire le lien avec le minutage des valeurs précédentes ou de la simulation de jeu.

### <a name="reading-the-thumbsticks"></a>Lecture des entrées des sticks analogiques

Chaque stick analogique fournit une valeur analogique située entre -1.0 et +1.0 sur les axes X et Y. Sur l’axe X, une valeur de -1.0 correspond à la position de stick analogique la plus à gauche ; une valeur de +1.0 indique la position la plus à droite. Sur l’axe Y, une valeur de -1.0 correspond à la position de stick analogique la plus en bas ; une valeur de +1.0 indique la position la plus en haut. Dans les deux axes, la valeur est approximativement de 0,0 lorsque le Stick est dans la position centrale, mais il est normal que la valeur précise varie, même entre les lectures suivantes. les stratégies d’atténuation de cette variation sont décrites plus loin dans cette section.

La valeur de l’axe X du stick analogique gauche est lue à partir de la propriété `LeftThumbstickX` de la structure [GamepadReading][] ; la valeur de l’axe Y est lue à partir de la propriété `LeftThumbstickY`. La valeur de l’axe X du stick analogique droit est lue à partir de la propriété `RightThumbstickX` ; la valeur de l’axe Y est lue à partir de la propriété `RightThumbstickY`.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
float rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
float rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0
double rightStickX = reading.RightThumbstickX; // returns a value between -1.0 and +1.0
double rightStickY = reading.RightThumbstickY; // returns a value between -1.0 and +1.0
```

Si vous examinez les valeurs que les sticks analogiques fournissent, vous remarquerez qu’ils ne fournissent pas toujours la valeur neutre 0.0 quand ils se trouvent à la position centrale. Les sticks analogiques fournissent différentes valeurs proches de 0.0 chaque fois qu’ils sont déplacés, puis remis à la position centrale. Pour compenser ces variations, vous pouvez implémenter une petite _zone morte_, qui correspond à une plage de valeurs proches de la position centrale idéale à ignorer. Pour implémenter une zone morte, vous pouvez déterminer de quelle distance le stick analogique a été déplacé à partir de sa position centrale, en ignorant les valeurs qui sont plus proches que la distance de référence spécifiée. Vous pouvez calculer la distance à peu près&mdash;ce n’est pas exact, car les valeurs de Stick analogiques sont essentiellement des valeurs polaires, non planaires,&mdash;simplement à l’aide du Pythagorean. Vous obtenez ainsi une zone morte radiale.

L’exemple suivant illustre une zone morte radiale simple calculée à l’aide du théorème de Pythagore.

```cpp
float leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
float leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const float deadzoneRadius = 0.1;
const float deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
auto oppositeSquared = leftStickY * leftStickY;
auto adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

```cs
double leftStickX = reading.LeftThumbstickX;   // returns a value between -1.0 and +1.0
double leftStickY = reading.LeftThumbstickY;   // returns a value between -1.0 and +1.0

// choose a deadzone -- readings inside this radius are ignored.
const double deadzoneRadius = 0.1;
const double deadzoneSquared = deadzoneRadius * deadzoneRadius;

// Pythagorean theorem -- for a right triangle, hypotenuse^2 = (opposite side)^2 + (adjacent side)^2
double oppositeSquared = leftStickY * leftStickY;
double adjacentSquared = leftStickX * leftStickX;

// accept and process input if true; otherwise, reject and ignore it.
if ((oppositeSquared + adjacentSquared) > deadzoneSquared)
{
    // input accepted, process it
}
```

Chaque stick analogique fonctionne également comme un bouton quand l’utilisateur appuie dessus vers l’intérieur. Pour plus d’informations sur la lecture de cette entrée, consultez [Lecture des entrées des boutons](#reading-the-buttons).

### <a name="reading-the-triggers"></a>Lecture des entrées des gâchettes

Les gâchettes sont représentées par des valeurs à virgule flottante comprises entre 0.0 (gâchette entièrement relâchée) et 1.0 (gâchette entièrement enclenchée). La valeur de la gâchette gauche est lue à partir de la propriété `LeftTrigger` de la structure [GamepadReading][] ; la valeur de la gâchette droite est lue à partir de la propriété `RightTrigger`.

```cpp
float leftTrigger  = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
float rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

```cs
double leftTrigger = reading.LeftTrigger;  // returns a value between 0.0 and 1.0
double rightTrigger = reading.RightTrigger; // returns a value between 0.0 and 1.0
```

### <a name="reading-the-buttons"></a>Lecture des boutons

Chacun des boutons de la manette&mdash;les quatre directions des champignons D-Pad, de gauche et de droite, de gauche et de droite Press, **A**, **B**, **X**, **Y**, **View**et **menu**&mdash;fournit une lecture numérique qui indique s’il est appuyé (enfoncé) ou relâché (haut). Pour plus d’efficacité, les lectures de bouton ne sont pas représentées sous forme de valeurs booléennes individuelles. au lieu de cela, ils sont tous regroupés dans un seul champ de champ qui est représenté par l’énumération [GamepadButtons][] .

<!-- > [!NOTE]
> The Xbox Elite Wireless Controller is equipped with four additional **paddle** buttons on its underside. These buttons are also represented in the `GamepadButtons` enumeration and their values are read in the same way as the standard gamepad buttons. -->

Les valeurs des boutons sont lues à partir de la propriété `Buttons` de la structure [GamepadReading][]. Comme cette propriété est un champ de bits, un masquage au niveau du bit est effectué pour isoler la valeur du bouton qui vous intéresse. Le bouton est à l’état enfoncé (position basse) lorsque le bit correspondant est défini ; dans le cas contraire, il se trouve à l’état relâché (position haute).

L’exemple suivant détermine si le bouton A est à l’état appuyé.

```cpp
if (GamepadButtons::A == (reading.Buttons & GamepadButtons::A))
{
    // button A is pressed
}
```

```cs
if (GamepadButtons.A == (reading.Buttons & GamepadButtons.A))
{
    // button A is pressed
}
```

L’exemple suivant détermine si le bouton A est à l’état relâché.

```cpp
if (GamepadButtons::None == (reading.Buttons & GamepadButtons::A))
{
    // button A is released
}
```

```cs
if (GamepadButtons.None == (reading.Buttons & GamepadButtons.A))
{
    // button A is released
}
```

Parfois, vous souhaiterez peut-être déterminer à quel moment un bouton passe de enfoncé à relâché ou relâché, si plusieurs boutons sont enfoncés ou relâché, ou si un ensemble de boutons est organisé d’une façon particulière&mdash;certains enfoncés, d’autres non. Pour plus d’informations sur la détection de chacun de ces états, consultez [Détecter les changements d’état des boutons](input-practices-for-games.md#detecting-button-transitions) et [Détecter les dispositions de boutons complexes](input-practices-for-games.md#detecting-complex-button-arrangements).

## <a name="run-the-gamepad-input-sample"></a>Exécuter l’exemple d’entrée de boîtier de commande

L’[exemple GamepadUWP _(github)_ ](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadUWP) montre comment connecter un boîtier de commande et lire son état.

## <a name="vibration-and-impulse-triggers-overview"></a>Vue d’ensemble des gâchettes de vibration et à impulsions

Les moteurs de vibration intégrés au boîtier de commande sont conçus pour offrir une expérience tactile au joueur. Les jeux utilisent cette fonctionnalité pour renforcer la sensation d’immersion, mieux communiquer les informations d’état (par exemple, les dommages subis), signaler la proximité d’objets importants, ou pour d’autres usages créatifs.

Les boîtiers de commande Xbox One comportent au total quatre moteurs de vibration indépendants. Deux sont des moteurs volumineux situés dans le corps du boîtier de soucaissement ; le moteur de gauche fournit des vibrations rugueuses à grande amplitude, tandis que le moteur de droite fournit une vibration plus légère et plus subtile. Les deux autres moteurs sont des petits moteurs intégrés à chaque gâchette. Ils déclenchent des vibrations rapides en série qui sont directement ressenties par le joueur qui manipulent les gâchettes (d’où le nom de _gâchette à impulsions_, qui est une fonctionnalité spécifique au boîtier de commande Xbox One). L’utilisation combinée de ces quatre moteurs permet de recréer une large palette de sensations tactiles.

## <a name="using-vibration-and-impulse"></a>Utilisation des vibrations et des impulsions

Les vibrations du boîtier de commande sont contrôlées par la propriété [Vibration][] de la classe [Gamepad][]. `Vibration` est une instance de la structure [GamepadVibration][] qui est constituée de quatre valeurs à virgule flottante ; chaque valeur représente l’intensité de l’un des moteurs.

Bien que les membres de la propriété `Gamepad.Vibration` puissent être modifiés directement, il est recommandé d’initialiser une instance `GamepadVibration` distincte aux valeurs souhaitées, puis de la copier dans la propriété `Gamepad.Vibration` pour modifier simultanément les véritables inintensités de moteur.

L’exemple suivant montre comment modifier simultanément toutes les intensités de moteur.

```cpp
// get the first gamepad
Gamepad^ gamepad = Gamepad::Gamepads->GetAt(0);

// create an instance of GamepadVibration
GamepadVibration vibration;

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

```cs
// get the first gamepad
Gamepad gamepad = Gamepad.Gamepads[0];

// create an instance of GamepadVibration
GamepadVibration vibration = new GamepadVibration();

// ... set vibration levels on vibration struct here

// copy the GamepadVibration struct to the gamepad
gamepad.Vibration = vibration;
```

### <a name="using-the-vibration-motors"></a>Utilisation des moteurs de vibration

Les moteurs de vibration gauche et droite acceptent des valeurs à virgule flottante comprises entre 0.0 (aucune vibration) et 1.0 (intensité de vibration la plus élevée). L’intensité du moteur gauche est définie par la propriété `LeftMotor` de la structure [GamepadVibration][] ; celle du moteur droit est définie par la propriété `RightMotor`.

L’exemple suivant définit l’intensité des deux moteurs de vibration et active la vibration du boîtier de commande.

```cpp
GamepadVibration vibration;
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftMotor = 0.80;  // sets the intensity of the left motor to 80%
vibration.RightMotor = 0.25; // sets the intensity of the right motor to 25%
mainGamepad.Vibration = vibration;
```

N’oubliez pas que ces deux moteurs ne sont pas identiques. La définition de leurs propriétés à la même valeur ne produit donc pas la même vibration dans ces moteurs. Pour toute valeur, le moteur de gauche produit une vibration plus élevée à une fréquence inférieure à celle du moteur de droite qui&mdash;pour la même valeur&mdash;produit une vibration plus élevée à une fréquence plus élevée. Même avec une valeur maximale, le moteur gauche ne peut pas produire une vibration à des fréquences aussi élevées que le moteur droit, et le moteur droit ne peut pas produire une vibration aussi forte que le moteur gauche. Toutefois, du fait que les deux moteurs sont connectés physiquement par le même boîtier de commande, les joueurs ne ressentent pas les vibrations de chaque moteur de manière totalement indépendante, et ce même si les moteurs présentent des caractéristiques distinctes et des intensités de vibration différentes. Cette disposition permet d’offrir aux joueurs une plus grande palette de sensations tactiles qu’avec deux moteurs identiques.

### <a name="using-the-impulse-triggers"></a>Utilisation des gâchettes à impulsions

Chaque moteur de gâchette à impulsions accepte une valeur à virgule flottante comprise entre 0.0 (aucune vibration) et 1.0 (intensité de vibration la plus élevée). L’intensité du moteur de la gâchette gauche est définie par la propriété `LeftTrigger` de la structure [GamepadVibration][] ; celle du moteur de la gâchette droite est définie par la propriété `RightTrigger`.

L’exemple suivant définit l’intensité des deux moteurs des gâchettes à impulsions, et les active.

```cpp
GamepadVibration vibration;
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
gamepad.Vibration = vibration;
```

```cs
GamepadVibration vibration = new GamepadVibration();
vibration.LeftTrigger = 0.75;  // sets the intensity of the left trigger to 75%
vibration.RightTrigger = 0.50; // sets the intensity of the right trigger to 50%
mainGamepad.Vibration = vibration;
```

Contrairement aux autres moteurs, les deux moteurs de vibration intégrés aux gâchettes sont identiques et fournissent chacun la même vibration pour une valeur identique. Toutefois, du fait que ces moteurs ne sont pas connectés physiquement, les joueurs ressentent les vibrations de chaque moteur de manière indépendante. Cette disposition permet au joueur de ressentir simultanément des sensations distinctes au niveau des deux gâchettes, tout en facilitant la communication d’informations plus précises que les moteurs installés physiquement dans le boîtier de commande.

## <a name="run-the-gamepad-vibration-sample"></a>Exécuter l’exemple de vibration du boîtier de commande

L’[exemple GamepadVibrationUWP _(github)_ ](https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/System/GamepadVibrationUWP) montre comment utiliser les moteurs de vibration et les gâchettes à impulsions du boîtier de commande pour produire des effets variés.

## <a name="see-also"></a>Voir aussi

* [Windows.Gaming.Input.UINavigationController][]
* [Windows. Gaming. Input. IGameController][]
* [Pratiques d’entrée pour les jeux](input-practices-for-games.md)

[Windows. Gaming. Input]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.aspx
[Windows.Gaming.Input.UINavigationController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.uinavigationcontroller.aspx
[Windows. Gaming. Input. IGameController]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.igamecontroller.aspx
[Gamepad]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.aspx
[Gamepads]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepads.aspx
[gamepadadded]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadadded.aspx
[gamepadremoved]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.gamepadremoved.aspx
[getcurrentreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.getcurrentreading.aspx
[Vibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepad.vibration.aspx
[gamepadreading]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadreading.aspx
[gamepadbuttons]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadbuttons.aspx
[gamepadvibration]: https://msdn.microsoft.com/library/windows/apps/windows.gaming.input.gamepadvibration.aspx
