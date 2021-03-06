---
title: Configurer un scanneur de code-barres
description: Découvrez comment configurer un scanneur de codes-barres pour l’application souhaitée.
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, point de vente, pdv
ms.localizationpriority: medium
ms.openlocfilehash: 8466c56ef73a1c38c67e28cf52de7f380e6c563a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321585"
---
# <a name="configure-a-barcode-scanner"></a>Configurer un scanneur de code-barres

Les scanneurs de codes-barres peuvent être configurés selon plusieurs modes différents.  Il est important que votre scanneur de codes-barres soit configuré correctement pour l’application prévue.

De nombreux scanneurs de codes-barres peuvent être configurés en mode **insertion clavier** qui fait paraître le scanneur de codes-barres comme un clavier à Windows.  Cela vous permet de scanner des codes-barres dans des applications qui ne sont pas spécifiquement dédiées à cette activité, comme le Bloc-notes.  Lorsque vous scannez un code-barres dans ce mode, les données décodées du scanneur de codes-barres sont insérées au point d’insertion comme si vous les tapiez au clavier.  Si vous souhaitez mieux contrôler votre scanneur de codes-barres à partir de votre application UWP, vous devez le configurer dans un mode autre que l'insertion clavier.

## <a name="usb-barcode-scanner"></a>Scanneur de codes-barres USB
Un scanneur de code-barres connectée par USB doit être configuré en mode **Scanneur de PDV HID** pour fonctionner avec le pilote de scanneur de code-barres qui est inclus dans Windows. Ce pilote est une implémentation de la **HID Point of Sale l’utilisation de Tables** spécification publié sur [-HID USB](https://www.usb.org/hid).  Reportez-vous à la documentation de votre scanneur de code-barres ou contactez son fabricant pour obtenir les instructions d'activation du mode **Scanneur de PDV HID**.  Une fois configuré comme un **Scanneur de PDV HID**, votre scanneur de codes-barres apparaît dans le Gestionnaire de périphériques sous le nœud **Scanneur de codes-barres de PDV** en tant que **Scanneur de codes-barres de PDV HID**.

Le fabricant de votre scanneur de code-barres peut également avoir un pilote spécifique au fournisseur qui prend en charge les API de scanneur de code-barres UWP dans mode autre que **Scanneur de PDV HID**.  Si vous avez déjà installé un pilote fourni par le fabricant compatible avec les API de scanneur de codes-barres UWP, vous pouvez voir un appareil spécifique au fournisseur répertorié sous **scanneur de codes-barres POS** dans le Gestionnaire de périphériques.

## <a name="bluetooth-barcode-scanner"></a>Scanneur de codes-barres Bluetooth
Un scanneur connecté par Bluetooth doit être configuré dans le mode **Protocole Port série - Interface série simple (SPP-SSI)** pour fonctionner avec les API de scanneur de code-barres UWP.  Reportez-vous à la documentation de votre scanneur de code-barres ou contactez son fabricant pour obtenir les instructions d'activation du **mode SPP-SSI**.

Avant de pouvoir utiliser votre scanneur de codes-barres Bluetooth vous devez l’associer à l’aide de **Paramètres > appareils > Bluetooth & autres appareils > ajouter le Bluetooth ou tout autre périphérique**.

Vous pouvez lancer et contrôler le processus d’appairage en utilisant le [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/windows.devices.enumeration) espace de noms.  Consultez [paire appareils](https://docs.microsoft.com/windows/uwp/devices-sensors/pair-devices) pour plus d’informations.

[!INCLUDE [feedback](./includes/pos-feedback.md)]