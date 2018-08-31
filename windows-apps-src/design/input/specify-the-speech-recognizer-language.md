---
author: Karl-Bridge-Microsoft
Description: Learn how to select an installed language to use for speech recognition.
title: Spécifier la langue de reconnaissance vocale
ms.assetid: 4C463A1B-AF6A-46FD-A839-5D6724955B38
label: Specify the speech recognizer language
template: detail.hbs
keywords: voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 471f222bb22293ccb3b66f5387d43b74fc53b910
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653868"
---
# <a name="specify-the-speech-recognizer-language"></a>Spécifier la langue de reconnaissance vocale


Découvrez comment sélectionner une langue installée à utiliser pour la reconnaissance vocale.

> **API importantes**: [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251), [**SupportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250), [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804)


Nous avons répertorié les langues installées sur un système. Identifiez la langue par défaut et sélectionnez une autre langue pour la reconnaissance.

**Éléments requis:**

Cette rubrique s’appuie sur l’article [Reconnaissance vocale](speech-recognition.md).

Vous devez posséder des connaissances de base sur la reconnaissance vocale et ses contraintes.

Si vous débutez dans le développement d’applications pour plateforme Windows universelle (UWP), consultez les rubriques ci-dessous pour vous familiariser avec les technologies décrites ici.

-   [Créer votre première application](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Découvrir les événements avec [Vue d’ensemble des événements et des événements routés](https://msdn.microsoft.com/library/windows/apps/mt185584).

**Recommandations en matière d’expérience utilisateur:**

Pour obtenir de précieux conseils concernant la conception d’une application dotée de fonctions vocales à la fois utile et conviviale, voir [Recommandations en matière de conception](https://msdn.microsoft.com/library/windows/apps/dn596121).

## <a name="identify-the-default-language"></a>Identifiez la langue par défaut.


La reconnaissance vocale utilise la langue du système en tant que langue de reconnaissance par défaut. Cette langue est définie par l’utilisateur sur l’écran Paramètres &gt; Système &gt; Parole &gt; Langue vocale de l’appareil.

Nous identifions la langue par défaut en vérifiant la propriété statique [**SystemSpeechLanguage**](https://msdn.microsoft.com/library/windows/apps/dn653252).

```CSharp
var language = SpeechRecognizer.SystemSpeechLanguage; 
```

## <a name="confirm-an-installed-language"></a>Confirmer une langue installée


Les langues installées peuvent varier entre les appareils. Vérifiez l’existence d’une langue avant de vous en servir pour une contrainte particulière.

**Remarque** Un redémarrage est nécessaire lorsqu’un nouveau module linguistique est installé. Une exception avec le code d’erreur SPERR\_NOT\_FOUND (0x8004503a) est déclenchée si la langue spécifiée n’est pas prise en charge ou n’est pas encore installée.

 

Déterminez les langues prises en charge sur un appareil en vérifiant l’une des deux propriétés statiques de la classe [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226) :

-   [**SupportedTopicLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653251) : collection des objets [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) utilisés avec les grammaires de dictée et de recherche web prédéfinies.

-   [**supportedGrammarLanguages**](https://msdn.microsoft.com/library/windows/apps/dn653250): collection des objets [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) utilisés avec une contrainte de liste ou un fichier SRGS (Speech Recognition Grammar Specification).

## <a name="specify-a-language"></a>Spécifier une langue


Pour spécifier une langue, passez un objet [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) dans le constructeur [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226).

Ici, nous spécifions «en-US» comme langue de reconnaissance.


```CSharp
var language = new Windows.Globalization.Language(“en-US”); 
var recognizer = new SpeechRecognizer(language); 
```

## <a name="remarks"></a>Remarques


Une contrainte de rubrique peut être configurée en ajoutant une [**SpeechRecognitionTopicConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631446) à la collection [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241) de la [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226), puis en appelant [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240). Un [**speechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433) de **TopicLanguageNotSupported** est renvoyé si le module de reconnaissance n’est pas initialisé avec une langue de rubrique prise en charge.

Une contrainte de liste peut être configurée en ajoutant une [**SpeechRecognitionListConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631421) à la collection [**Constraints**](https://msdn.microsoft.com/library/windows/apps/dn653241) de la [**SpeechRecognizer**](https://msdn.microsoft.com/library/windows/apps/dn653226), puis en appelant [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240). Vous ne pouvez pas spécifier directement la langue d’une liste personnalisée. La liste est traitée à l’aide de la langue du module de reconnaissance.

Une grammaire SRGS est un format XML standard ouvert représenté par la classe [**SpeechRecognitionGrammarFileConstraint**](https://msdn.microsoft.com/library/windows/apps/dn631412). Contrairement aux listes personnalisées, vous pouvez spécifier la langue de la grammaire dans le balisage SRGS. [**CompileConstraintsAsync**](https://msdn.microsoft.com/library/windows/apps/dn653240) échoue avec un [**SpeechRecognitionResultStatus**](https://msdn.microsoft.com/library/windows/apps/dn631433) de **TopicLanguageNotSupported** si le module de reconnaissance n’est pas initialisé pour la même langue que le balisage SRGS.

## <a name="related-articles"></a>Articles connexes

**Développeurs**

* [Interactions vocales](speech-interactions.md)

**Concepteurs**

* [Recommandations en matière de conception de fonctions vocales](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Exemples**

* [Exemple de reconnaissance vocale et de synthèse vocale](http://go.microsoft.com/fwlink/p/?LinkID=619897)
 

 



