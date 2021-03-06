---
title: Présentation des applications multi-utilisateurs
description: Découvrez une présentation détaillée du modèle multi-utilisateur Xbox One qui prend en charge plusieurs utilisateurs qui jouent des jeux de manière coopérative sur un seul appareil.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 2dde6ed3-7f53-48a6-aebe-2605230decb8
ms.localizationpriority: medium
ms.openlocfilehash: 0364964f069537ee934771184eb9c6ffda6f346f
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970287"
---
# <a name="introduction-to-multi-user-applications"></a>Présentation des applications multi-utilisateurs

Cette rubrique est une simple présentation générale du modèle multi-utilisateur Xbox.

Le modèle utilisateur Xbox One répond aux exigences d’une console de jeu qui prend en charge plusieurs utilisateurs jouant ensemble sur un seul et même appareil. Ce modèle permet à plusieurs utilisateurs, disposant chacun de leur propre manette, de se connecter et d’accéder à la console simultanément dans le cadre d’une même session interactive. Cette technique diffère de celle mise en œuvre sur d’autres appareils Windows. Par exemple :
* Les **PC de bureau Windows** permettent à plusieurs utilisateurs d’utiliser le même appareil, mais chaque utilisateur dispose de sa propre session interactive, et chacune des sessions est totalement indépendante des autres sessions sur l’appareil.
* Les **téléphones Windows** ne sont utilisables que par une seule personne. Cet utilisateur unique est déterminé lors de la phase OOBE (out-of-box experience) et ne peut plus se déconnecter une fois connecté. En effet, si une autre personne veut utiliser l’appareil, ce dernier doit être réinitialisé. 
* Les **consoles Xbox One** permettent à plusieurs utilisateurs de se connecter et d’utiliser l’appareil en même temps au cours d’une seule et même session interactive.

Chaque utilisateur du modèle utilisateur Xbox One dispose d’un compte d’utilisateur local. Ce compte d’utilisateur local est associé à un compte Xbox Live (et donc à un compte Microsoft). Cela signifie qu’il existe un mappage strict un-à-un entre un compte d’utilisateur Xbox et un compte Xbox Live ou un compte Microsoft.

## <a name="single-user-applications"></a>Applications mono-utilisateur
Par défaut, les applications de plateforme Windows universelle (UWP) s’exécutent dans le contexte de l’utilisateur ayant lancé l’application. Ces *applications mono-utilisateur* ne reconnaissent que cet utilisateur unique et s’exécutent dans un mode compatible avec le modèle utilisateur sur d’autres appareils Windows. Le modèle utilisateur Xbox gère l’utilisateur associé à l’application et garantit qu’un utilisateur est connecté au moment où l’application est lancée. Dans ce modèle, les auteurs d’applications et de jeux UWP n’ont aucune opération spéciale à effectuer pour exécuter ces programmes sur Xbox. 

## <a name="multi-user-applications"></a>Applications multi-utilisateur
Les jeux UWP peuvent choisir de souscrire au modèle multi-utilisateur Xbox One. Ces *applications multi-utilisateur* s’exécutent dans le contexte d’un compte système (appelé compte par défaut) et peuvent tirer pleinement parti de la flexibilité et de la puissance du modèle utilisateur Xbox One. Dans le cas de ces jeux, le modèle utilisateur Xbox ne gère pas l’utilisateur associé au jeu et n’exige même pas qu’un utilisateur soit connecté pour que le jeu s’exécute. Cela implique que ces jeux soient écrits pour reconnaître explicitement et gérer leurs exigences vis-à-vis des utilisateurs, notamment la nécessité ou non qu’un utilisateur soit connecté, l’éventuelle implémentation du concept d’utilisateur actuel, l’autorisation ou l’interdiction des entrées simultanées par plusieurs utilisateurs, etc.
   
Pour choisir le modèle multi-utilisateur, procédez comme suit :   
1. Ouvrez votre projet dans Visual Studio.   
2. Sélectionnez le fichier package.appxmanifest.xml.   
3. Cliquez avec le bouton droit sur ce dernier et sélectionnez **Afficher le code**.   
4. Ajoutez la ligne suivante dans la section `<Properties></Properties>` :

```
<uap:SupportedUsers>multiple</uap:SupportedUsers>
```

### <a name="identifying-users-and-inputs"></a>Identification des utilisateurs et des entrées
Les développeurs peuvent utiliser KeyRoutedEventArgs.DeviceId, utilisé par les événements routés KeyUp et KeyDown, pour différencier les événements générés à partir d’entrées différentes.
La méthode Windows.System.UserDeviceAssociation.FindUserFromDeviceId permet d’identifier l’utilisateur associé à une entrée spécifique.

Pour plus d’informations, voir [KeyRoutedEventArgs.DeviceId](https://docs.microsoft.com/uwp/api/windows.ui.xaml.input.keyroutedeventargs.deviceid).


## <a name="guidance-on-which-model-to-choose"></a>Aide concernant le modèle à utiliser
Toutes les applications UWP et la majorité des jeux pour joueur unique peuvent être écrits sous la forme d’applications mono-utilisateur. Le choix du modèle multi-utilisateur Xbox One n’est recommandé que pour les jeux multijoueurs coopératifs.

## <a name="see-also"></a>Voir aussi
- [UWP sur Xbox One](index.md)
