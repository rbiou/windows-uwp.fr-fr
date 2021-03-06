---
title: Référence API d’entrée à distance de Device Portal
description: Découvrez comment envoyer à distance l'entrée du contrôleur, du clavier et de la souris sur une Xbox.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 882e84c5126e4f67e246dd479008133c979c06b1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595954"
---
# <a name="remote-input-api-reference"></a>Référence API de l'Entrée distante   
Vous pouvez envoyer l’entrée du contrôleur, du clavier et de la souris en temps réel et à distance par le biais de cette API.

**Demande**

Méthode      | URI de requête
:------     | :-----
Websocket | /ext/remoteinput
<br />
**Paramètres d’URI**

- Aucune

**En-têtes de demande**

- Aucune

**Demande**

L’objet websocket une série de messages de tableau d’octets. Pour chaque message le format est le suivant.

Le premier octet indique le type d’entrée. Les type d’entrée pris en charge sont les suivants :

| Type d'entrée        | Valeur d’octet |
|------------|-------------|
KeyCodes clavier | 0x01
ScanCodes clavier | 0x02
Souris | 0x03
Effacer tout | 0x04

Pour les KeyboardKeyCodes et KeyboardScanCodes, le deuxième octet est la valeur du code de touche ou de scan et le troisième octet est 0x01 pour une touche enfoncée et 0x00 pour une touche relâchée.

Pour un message de souris, la valeur suivante est un UINT16 dans l’ordre du réseau (2 octets) qui indique le type d’événement de souris :

| Type d'action        | Valeur UINT16 |
|------------|-------------|
Move | 0x0001
LeftDown | 0x0002
LeftUp | 0x0004
RightDown | 0x0008
RightUp | 0x0010
MiddleDown | 0x0020
MiddleUp | 0x0040
X1Down | 0x0080
X1Up | 0x0100
X2Down | 0x0200
X2Up | 0x0400
VerticalWheelMoved | 0x0800
HorizontalWheelMoved | 0x1000

Cet octet est suivi de deux valeurs UINT32 dans l’ordre du réseau et d'un troisième UINT32 facultatif pour les actions de la molette. Les deux premières valeurs sont les coordonnées X et Y et le troisième est le delta de la roulette. Les coordonnées X et Y sont censées être une valeur comprise entre 0 et 65 535 indiquant la position relative de la souris dans le plan horizontal et vertical.

ClearAll indique que les touches actuellement enfoncées doivent être relâchées. Aucun autre octet n'est attendu.

Pour envoyer des entrées de boîtier de commande, les valeurs de code de touche qui représentent les pressions sur les boutons du boîtier peuvent être utilisées avec KeyboardKeyCodes. Ces valeurs sont les suivantes :

| Type de boîtier de commande        | Valeur d’octet |
|------------|-------------|
VK_GAMEPAD_A                       |  0xC3
VK_GAMEPAD_B                       |  0xC4
VK_GAMEPAD_X                       |  0xC5
VK_GAMEPAD_Y                       |  0xC6
VK_GAMEPAD_RIGHT_SHOULDER          |  0xC7
VK_GAMEPAD_LEFT_SHOULDER           |  0xC8
VK_GAMEPAD_LEFT_TRIGGER            |  0xC9
VK_GAMEPAD_RIGHT_TRIGGER           |  0xCA
VK_GAMEPAD_DPAD_UP                 |  0xCB
VK_GAMEPAD_DPAD_DOWN               |  0xCC
VK_GAMEPAD_DPAD_LEFT               |  0xCD
VK_GAMEPAD_DPAD_RIGHT              |  0xCE
VK_GAMEPAD_MENU                    |  0xCF
VK_GAMEPAD_VIEW                    |  0xD0
VK_GAMEPAD_LEFT_THUMBSTICK_BUTTON  |  0xD1
VK_GAMEPAD_RIGHT_THUMBSTICK_BUTTON |  0xD2
VK_GAMEPAD_LEFT_THUMBSTICK_UP      |  0xD3
VK_GAMEPAD_LEFT_THUMBSTICK_DOWN    |  0xD4
VK_GAMEPAD_LEFT_THUMBSTICK_RIGHT   |  0xD5
VK_GAMEPAD_LEFT_THUMBSTICK_LEFT    |  0xD6
VK_GAMEPAD_RIGHT_THUMBSTICK_UP     |  0xD7
VK_GAMEPAD_RIGHT_THUMBSTICK_DOWN   |  0xD8
VK_GAMEPAD_RIGHT_THUMBSTICK_RIGHT  |  0xD9
VK_GAMEPAD_RIGHT_THUMBSTICK_LEFT   |  0xDA


**Réponse**   

- Aucune

**Code d’état**

Cette API comporte les codes d’état attendus suivants.

Code d’état HTTP      | Description
:------     | :-----
200 | Demande réussie
4XX | Codes d’erreur
5XX | Codes d’erreur

<br />
**Familles de périphériques disponibles**

* Windows Xbox