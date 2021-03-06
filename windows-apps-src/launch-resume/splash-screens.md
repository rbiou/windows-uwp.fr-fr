---
title: Écrans de démarrage
description: Cette section explique comment définir et configurer l’écran de démarrage de votre application.
ms.assetid: 6b954bb3-e5b0-46d1-8afc-fb805536cf6d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 09eadb8467725cbf40f3fb54d32741960fc89321
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66371863"
---
# <a name="splash-screens"></a>Écrans de démarrage

Toutes les applications UWP doivent disposer d’un écran de démarrage comprenant une image et une couleur d’arrière-plan, que vous pouvez personnaliser.

Votre écran de démarrage s’affiche dès que l’utilisateur lance votre application. Il fournit des informations pendant que les ressources d’applications sont initialisées. Dès que votre application autorise l’interaction, l’écran de démarrage disparaît.

Un écran de démarrage bien conçu peut contribuer à rendre votre application plus attrayante. Voici un écran de démarrage simple et discret :

![écran de démarrage redimensionné à 75 % de la taille de l’exemple.](images/regularsplashscreen.png)

Cet écran de démarrage est créé en combinant une couleur d’arrière-plan verte à une image PNG à arrière-plan transparent.

Une image simple avec une couleur d’arrière-plan s’affiche correctement, quel que soit l’appareil sur lequel votre application s’exécute. Seules les dimensions de l’arrière-plan sont modifiées pour prendre en charge différentes tailles d’écran. Votre image reste toujours intacte.

De plus, vous pouvez utiliser la classe [**SplashScreen**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Activation.SplashScreen) pour personnaliser l’expérience de démarrage de votre application. Vous pouvez placer un écran de démarrage étendu créé par vos propres soins, afin d’accorder à votre application davantage de temps pour effectuer des tâches supplémentaires telles que la préparation de l’interface utilisateur ou l’achèvement des opérations de mise en réseau. Vous pouvez également utiliser la classe **SplashScreen** pour vous avertir lorsque l’écran de démarrage est supprimé, ce qui vous permet de lancer les animations d’entrée.

| Rubrique | Description |
|-------|-------------|
| [Ajouter un écran de démarrage](add-a-splash-screen.md) | Définissez l’image et la couleur d’arrière-plan de l’écran de démarrage de votre application. |
| [Afficher un écran de démarrage plus longtemps](create-a-customized-splash-screen.md) | Affichez un écran de démarrage plus longtemps en créant et en affichant un écran de démarrage étendu pour votre application. Cet écran étendu imite l’écran de démarrage affiché quand votre application est lancée. Il peut être personnalisé. |