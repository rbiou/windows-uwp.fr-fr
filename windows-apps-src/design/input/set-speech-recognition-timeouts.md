---
author: Karl-Bridge-Microsoft
Description: Set how long a speech recognizer ignores silence or unrecognizable sounds (babble) and continues listening for speech input.
title: Définir des délais d’expiration de reconnaissance vocale
ms.assetid: 58F446AC-4A56-454D-8125-62A2C4DBFCC8
label: Speech recognition timeouts
template: detail.hbs
keywords: voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: c82e0d77804e293176c53a9d8079e32054f9cca6
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1652748"
---
# <a name="set-speech-recognition-timeouts"></a>Définir des délais d’expiration de reconnaissance vocale


Définissez la durée pendant laquelle un moteur de reconnaissance vocale ignore les silences ou les sons incompréhensibles (brouhaha) et continue à écouter la saisie vocale.

> **API importantes**: [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253), [**SpeechRecognizerTimeouts**](https://msdn.microsoft.com/library/windows/apps/dn653230)

## <a name="set-a-timeout"></a>Définir un délai d’expiration


Ici, nous spécifions différentes valeurs [**Timeouts**](https://msdn.microsoft.com/library/windows/apps/dn653253):

-   InitialSilenceTimeout : durée pendant laquelle un SpeechRecognizer détecte du silence (avant que les résultats de la reconnaissance vocale aient été générés) avant de supposer qu’aucune saisie vocale ne va être effectuée.
-   BabbleTimeout : durée pendant laquelle un SpeechRecognizer continue à écouter les sons incompréhensibles (brouhaha) avant de supposer que la saisie vocale est terminée et de mettre fin à l’opération de reconnaissance vocale.
-   EndSilenceTimeout: durée pendant laquelle un SpeechRecognizer détecte du silence (après que les résultats de la reconnaissance vocale ont été générés) avant de supposer que la saisie vocale est terminée.

**Remarque** Les délais d’expiration sont définissables pour chaque moteur de reconnaissance vocale.

 

```CSharp
// Set timeout settings.
recognizer.Timeouts.InitialSilenceTimeout = TimeSpan.FromSeconds(6.0);
recognizer.Timeouts.BabbleTimeout = TimeSpan.FromSeconds(4.0);
recognizer.Timeouts.EndSilenceTimeout = TimeSpan.FromSeconds(1.2);
```

## <a name="related-articles"></a>Articles connexes


* [Interactions vocales](speech-interactions.md)
**Exemples**
* [Reconnaissance vocale et exemple de synthèse vocale](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 



