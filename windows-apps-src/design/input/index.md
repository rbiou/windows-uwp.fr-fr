---
description: Optimisez votre application pour un stylet, Surface Dial et d’autres types d’entrées.
title: Entrée et interactions
keywords: entrées d’application, personnaliser une application UWP
label: Input and interactions
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
ms.assetid: b771d452-c3ac-4d97-8482-eaf81bf34306
ms.localizationpriority: medium
ms.openlocfilehash: 4c54101306d2382a4e0301e89410d58abfb87743
ms.sourcegitcommit: dccba7256765116f06cf96143eb3cbaa12d7fe0b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87523535"
---
# <a name="input-and-interactions"></a>Entrée et interactions

![Icône d’entrées](../images/inputs-2x.png)

<!-- <div>
  <img src="images/keyboard/keyboard-hero.jpg" alt="" />
  <img src="images/input-interactions/icons-inputdevices03.png" />
</div> -->

Les applications UWP gèrent automatiquement un grand nombre d’entrées et fonctionnent sur différents appareils. Aucune action supplémentaire de votre part n’est nécessaire pour activer l’entrée tactile, par exemple. Mais il peut arriver que vous souhaitiez optimiser votre application pour certains types de saisies ou d’appareils. Par exemple, si vous créez une application de peinture, vous voudrez peut-être personnaliser la gestion des saisies au stylet.

Avec les instructions de conception et de codage fournies dans cette section, vous pourrez personnaliser votre application UWP pour certains types de saisies.

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="input-primer.md">Notions fondamentales sur la saisie</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Familiarisez-vous avec chaque type de périphérique d’entrée, ses comportements, ses fonctionnalités et ses limites, selon son association à certains facteurs de forme.</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="gaze-interactions.md">Entrée avec le pointage du regard</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Effectuez le suivi du regard de l’utilisateur en fonction de l’emplacement et du mouvement de ses yeux et de sa tête.</p>
    :::column-end:::
:::row-end:::

<!-- 
## Input primer

See our <b>[Input primer](index.md)</b> to familiarize yourself with each input device type and its behaviors, capabilities, and limitations when paired with certain form factors. -->

:::row:::
    :::column:::
        <h2 style="margin-top: 10px; margin-bottom: 0px">Entrée</h2>
        <a href="/windows/uwp/design/input/identify-input-devices">Identification des périphériques d’entrée</a><br/>
        <a href="/windows/uwp/design/input/handle-pointer-input">Pointeur</a><br/>
        <a href="/windows/uwp/design/input/pen-and-stylus-interactions">Stylet et Windows Ink</a><br/>
        <a href="/windows/uwp/design/input/touch-interactions">Interface tactile</a><br/>
        <a href="/windows/uwp/design/input/mouse-interactions">Souris</a><br/>
        <a href="/windows/uwp/design/input/keyboard-interactions">Clavier</a><br/>
        <a href="/windows/uwp/design/input/gamepad-and-remote-interactions">Gamepad et commande à distance</a><br/>
        <a href="/windows/uwp/design/input/touchpad-interactions">Pavé tactile</a><br/>
        <a href="/windows/uwp/design/input/windows-wheel-interactions">Surface Dial</a><br/>
        <a href="/windows/uwp/design/input/multiple-input-design-guidelines">Entrées multiples</a><br/>
        <a href="/windows/uwp/design/input/input-injection">Injection d’entrée</a><br/>
        <a href="/windows/uwp/design/input/custom-text-input">Saisie de texte personnalisé</a><br/>
    :::column-end:::
    :::column:::
        <h2 style="margin-top: 10px; margin-bottom: 0px">Interactions</h2>
        <a href="/windows/uwp/design/input/drag-and-drop">Glisser-déplacer</a><br/>
        <a href="/windows/uwp/design/input/guidelines-for-panning">Panoramique</a><br/>
        <a href="/windows/uwp/design/input/guidelines-for-rotation">Rotation</a><br/>
        <a href="/windows/uwp/design/input/guidelines-for-textselection">Sélection de texte et d’images</a><br/>
        <a href="/windows/uwp/design/input/guidelines-for-targeting">Ciblage</a><br/>
        <a href="/windows/uwp/design/input/guidelines-for-visualfeedback">Retour visuel</a><br/>
    :::column-end:::
    :::column:::
        <h2 style="margin-top: 10px; margin-bottom: 0px">Voix et IA</h2>
        <a href="/windows/uwp/design/input/speech-interactions">Fonctions vocales</a><br/>
        <a href="/windows/uwp/design/input/cortana-interactions">Cortana</a><br/>
    :::column-end:::
:::row-end:::


<!-- <div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[Surface Dial](windows-wheel-interactions.md)</b><br/>
Learn how to integrate this brand new category of input device into your Windows apps.</br>
This device is intended as a secondary, multi-modal input device that complements or modifies input from a primary device.
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Cortana](cortana-interactions.md)</b><br/>
Extend the basic functionality of Cortana with voice commands that launch and execute a single action in an external application.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Speech](speech-interactions.md)</b><br/>
Integrate speech recognition and text-to-speech (also known as TTS, or speech synthesis) directly into the user experience of your app.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Pen](pen-and-stylus-interactions.md)</b><br/>
Optimize your UWP app for pen input to provide both standard pointer device functionality and the best Windows Ink experience for your users.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Keyboard](keyboard-interactions.md)</b><br/>
Keyboard input is an important part of the overall user interaction experience for apps. The keyboard is indispensable to people with certain disabilities or users who just consider it a more efficient way to interact with an app.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Touch](touch-interactions.md)</b><br/>
UWP includes a number of different mechanisms for handling touch input, all of which enable you to create an immersive experience that your users can explore with confidence.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Touchpad](touchpad-interactions.md)</b><br/>
A touchpad combines both indirect multi-touch input with the precision input of a pointing device, such as a mouse. This combination makes the touchpad suited to both a touch-optimized UI and the smaller targets of productivity apps.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Mouse](mouse-interactions.md)</b><br/>
Mouse input is best suited for user interactions that require precision when pointing and clicking. This inherent precision is naturally supported by the UI of Windows, which is optimized for the imprecise nature of touch.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Gamepad and remote control](gamepad-and-remote-interactions.md)</b><br/>
UWP apps now support gamepad and remote control input. Gamepads and remote controls are the primary input devices for Xbox and TV experiences.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[Multiple inputs](multiple-input-design-guidelines.md)</b><br/>
To accommodate as many users and devices as possible, we recommend that you design your apps to work with as many input types as possible (gesture, speech, touch, touchpad, mouse, and keyboard). Doing so will maximize flexibility, usability, and accessibility.
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Identify input devices](identify-input-devices.md)</b><br/>
Identify the input devices connected to a Windows app device and identify their capabilities and attributes.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Handle pointer input](handle-pointer-input.md)</b><br/>
Receive, process, and manage input data from pointing devices, such as touch, mouse, pen/stylus, and touchpad, in Windows apps.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p><b>[Custom text input](custom-text-input.md)</b><br/>
The core text APIs in the Windows.UI.Text.Core namespace enable a UWP app to receive text input from any text service supported on Windows devices. This enables the app to receive text in any language and from any input type, like keyboard, speech, or pen.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Selecting text and images](guidelines-for-textselection.md)</b><br/>
This article describes selecting and manipulating text, images, and controls and provides user experience guidelines that should be considered when using these mechanisms in your apps.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<p>
<b>[Panning](guidelines-for-panning.md)</b><br/>
Panning or scrolling lets users navigate within a single view, to display the content of the view that does not fit within the viewport.
</p>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p>
<b>[Optical zoom and resizing](guidelines-for-optical-zoom.md)</b><br/>
This article describes Windows zooming and resizing elements and provides user experience guidelines for using these interaction mechanisms in your apps.
</p>
</div>
<div class="side-by-side-content-right">
<p>
<b>[Rotation](guidelines-for-rotation.md)</b><br/>
This article describes the new Windows UI for rotation and provides user experience guidelines that should be considered when using this new interaction mechanism in your UWP app.
</p>
</div>
</div>
</div>

<div class="side-by-side">
<div class="side-by-side-content">
<div class="side-by-side-content-left">
<p><b>[Targeting](guidelines-for-targeting.md)</b><br/>
Touch targeting in Windows uses the full contact area of each finger that is detected by a touch digitizer. The larger, more complex set of input data reported by the digitizer is used to increase precision when determining the user's intended (or most likely) target.
</p>
</div>
<div class="side-by-side-content-right">
<p><b>[Visual feedback](guidelines-for-visualfeedback.md)</b><br/>
Use visual feedback to show users when their interactions are detected, interpreted, and handled. Visual feedback can help users by encouraging interaction. It indicates the success of an interaction, which improves the user's sense of control. It also relays system status and reduces errors.
</p>
</div>
</div>
</div> -->


