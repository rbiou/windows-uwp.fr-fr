---
ms.assetid: ECE848C2-33DE-46B0-BAE7-647DB62779BB
title: Étalonner les capteurs
description: Dans un appareil basé sur le magnétomètre (la boussole, l’inclinomètre et le capteur d’orientation), il peut s’avérer nécessaire d’étalonner les capteurs en raison de facteurs environnementaux.
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 21e902daac01d8ed2645625320dec27bf7805fba
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66370312"
---
# <a name="calibrate-sensors"></a>Étalonner les capteurs


**API importantes**

-   [**Windows.Devices.Sensors**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors)
-   [**Windows.Devices.Sensors.Custom**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.Custom)

Dans un appareil basé sur le magnétomètre (la boussole, l’inclinomètre et le capteur d’orientation), il peut s’avérer nécessaire d’étalonner les capteurs en raison de facteurs environnementaux. L’énumération [**MagnetometerAccuracy**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) peut aider à déterminer la marche à suivre quand votre appareil doit être étalonné.

## <a name="when-to-calibrate-the-magnetometer"></a>Quand étalonner le magnétomètre

L’énumération [**MagnetometerAccuracy**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.MagnetometerAccuracy) comporte quatre valeurs qui vous permettent de déterminer si l’appareil sur lequel votre application s’exécute doit être étalonné. Si un appareil doit être étalonné, vous devez faire savoir à l’utilisateur que cette opération est nécessaire. Toutefois, vous ne devez pas demander trop souvent à l’utilisateur d’effectuer un étalonnage. Nous recommandons une fréquence maximale d’une fois toutes les 10 minutes.

| Value           | Description    |
| ----------------- | ------------------- |
| **Inconnu**     | Le pilote du capteur n’a pas pu indiquer la précision actuelle. Cela ne signifie pas nécessairement que l’appareil n’est pas étalonné. Votre application doit décider de la meilleure marche à suivre si la valeur **Unknown** est retournée. Si votre application dépend d’une lecture précise des capteurs, vous pouvez demander à l’utilisateur d’étalonner l’appareil. |
| **Non fiable**  | Il y a actuellement un niveau élevé d’imprécision dans le magnétomètre. Les applications doivent toujours demander un étalonnage par l’utilisateur quand cette valeur est retournée pour la première fois. |
| **Approximatif** | Les données sont suffisamment précises pour certaines applications. Une application de réalité virtuelle, qui doit uniquement savoir si l’utilisateur a déplacé l’appareil vers le haut/bas ou vers la gauche/droite, peut continuer sans étalonnage. Les applications qui ont besoin d’un en-tête absolu, comme une application de navigation qui doit connaître la direction dans laquelle vous conduisez pour vous indiquer des directions, doivent demander un étalonnage. |
| **Haute**        | Les données sont précises. Aucun étalonnage n’est nécessaire, même pour les applications qui doivent connaître un en-tête absolu, telles que la réalité augmentée ou les applications de navigation. |