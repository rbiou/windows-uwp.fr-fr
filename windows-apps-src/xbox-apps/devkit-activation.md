---
title: Activation du mode Développeur Xbox One
description: Procédure d’activation du mode développeur de façon à pouvoir basculer du mode commercial vers le mode développeur et inversement.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: 95b65e63c081734a560a852a5d064ef76c423ef6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258724"
---
# <a name="xbox-one-developer-mode-activation"></a>Activation du mode Développeur Xbox One

## <a name="how-developer-mode-works"></a>Fonctionnement du Mode développeur
La console Xbox One possède deux modes : le mode *Commercial* (**1**) et le mode *Développeur* (**2**). En Mode commercial, la console permet à n’importe quel utilisateur de jouer et d’exécuter des applications en tant qu’utilisateur. Le Mode développeur, vous permet de développer des logiciels pour la console, mais pas de jouer à des jeux commerciaux ou d’exécuter des applications commerciales.

Ce mode peut être activé sur n’importe quelle console Xbox One commerciale. Une fois le Mode développeur activé, vous pouvez basculer entre le mode commercial (**2a**) et le mode développeur (**2b**).

![Modes Xbox One](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-one-console"></a>Activer le Mode développeur sur votre console Xbox One commerciale

1.  Démarrez votre console Xbox One.

2.  Recherchez l’application **Dev Mode Activation** dans le Marché Xbox One et installez-la.

    ![Installer l’application Dev Mode Activation](images/devkit-activation-1.png)

3.  Lancez l’application à partir de la page du Store.

    ![Application Dev Mode Activation](images/devkit-activation-2.png)

4.  Notez le code affiché dans l’application Dev Mode Activation.

    ![Activation - Étape 5](images/activation-step-5.png)  
    
5.  [Inscrire un compte de développeur d’applications dans l’espace partenaires](https://developer.microsoft.com/store/register).  C’est également la première étape de la publication de votre jeu.

6.  Connectez-vous à l' [espace partenaires](https://partner.microsoft.com/dashboard) à l’aide de votre compte de développeur d’applications de l’espace partenaires valide.  Si vous ne voyez pas plusieurs options dans le volet de navigation de gauche ou si vous ne voyez pas l’option **créer une nouvelle application** dans la section **vue d’ensemble** , les étapes et les liens d’activation suivants ne _fonctionnent pas_. Assurez-vous d’avoir inscrit entièrement votre compte de développeur d’applications à l’étape précédente.

7.  Accédez à [Partner.Microsoft.com/xboxconfig/Devices](https://partner.microsoft.com/xboxconfig/devices).

8.  Entrez le code d’activation affiché dans l’application Dev Mode Activation. Vous disposez d’un nombre limité d’activations associées à votre compte. Une fois le mode développeur activé, l’espace partenaires indique que vous avez utilisé l’une des activations associées à votre compte.

    ![Activation - Étape 8](images/activation-step-8-rs2.png)    
    
9.  Cliquez sur **Accepter et activer**. La page sera rechargée et votre appareil apparaîtra dans le tableau. Pour plus d’informations, voir [Programme d’activation du mode développeur Xbox One](https://docs.microsoft.com/legal/windows/agreements/xbox-one-developer-mode-activation).

10. Après avoir saisi votre code d’activation, votre console affiche un écran de progression pour le processus d’activation.  
    
11. Une fois l’activation est terminée, ouvrez l’application Dev Mode Activation, puis cliquez sur **Basculer et redémarrer** pour accéder au Mode développeur. Notez que cette opération peut être plus longue que d’habitude.

    ![Activation - Étape 12](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>Basculer entre le Mode commercial et développeur
Une fois le Mode développeur activé sur votre console, utilisez **Dev Home** pour basculer entre le Mode commercial et développeur. Pour en savoir plus sur le démarrage et l’utilisation de Dev Home, consultez [Présentation des outils Xbox One](introduction-to-xbox-tools.md).

* Pour basculer en Mode commercial, ouvrez **Accueil du développeur**. Sous **Actions rapides**, sélectionnez **Quitter le mode développeur**. Cette action a pour effet de redémarrer votre console en Mode commercial.    

  ![Activation - Étape 13](images/activation-step-13-rs4.png)  
  
* Pour basculer vers le Mode développeur, utilisez l’application Dev Mode Activation. Ouvrez l’application et sélectionnez **Basculer et redémarrer**. Cette action a pour effet de redémarrer votre console en Mode développeur.  

  ![Activation - Étape 14](images/activation-step-12.png)  

## <a name="see-also"></a>Articles associés
- [Désactivation du mode développeur Xbox One](devkit-deactivation.md)
- [UWP sur Xbox One](index.md)
