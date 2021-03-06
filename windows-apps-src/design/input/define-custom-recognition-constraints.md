---
Description: Découvrez comment définir et utiliser des contraintes personnalisées pour la reconnaissance vocale.
title: Définir des contraintes de reconnaissance vocale personnalisées
ms.assetid: 26289DE5-6AC9-42C3-A160-E522AE62D2FC
label: Define custom recognition constraints
template: detail.hbs
keywords: voix, vocal, reconnaissance vocale, langage naturel, dictée, saisie, interaction utilisateur
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5b94c946222f510c7f1b1f7619b67ee83e6c2256
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258003"
---
# <a name="define-custom-recognition-constraints"></a>Définir des contraintes de reconnaissance vocale personnalisées

Découvrez comment définir et utiliser des contraintes personnalisées pour la reconnaissance vocale.

> **API importantes** : [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint), [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint), [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint)

La reconnaissance vocale requiert au moins une contrainte pour définir un vocabulaire reconnaissable. Si aucune contrainte n’est spécifiée, la grammaire de dictée prédéfinie des applications Windows universelles est utilisée. Voir [Reconnaissance vocale](speech-recognition.md).

## <a name="add-constraints"></a>Ajouter des contraintes

Utilisez la propriété [**SpeechRecognizer.Constraints**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.constraints) pour ajouter des contraintes à un moteur de reconnaissance vocale.

Nous abordons ici les trois types de contraintes de reconnaissance vocale utilisés à partir d’une application. (Pour les contraintes de commande vocale Cortana, consultez [lancer une application de premier plan avec des commandes vocales dans Cortana](https://docs.microsoft.com/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana).)

- [**SpeechRecognitionTopicConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint): contrainte basée sur une grammaire prédéfinie (dictée ou recherche Web).
- [**SpeechRecognitionListConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint): contrainte basée sur une liste de mots ou d’expressions.
- [**SpeechRecognitionGrammarFileConstraint**](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint): contrainte définie dans un fichier de spécification de la grammaire de la reconnaissance vocale (SRGS).

Chaque moteur de reconnaissance vocale possède sa propre collection de contraintes. Seules les combinaisons de contraintes suivantes sont valides :

- Une contrainte de sujet unique (dictée ou recherche web)
- Pour Windows 10 Fall Creators Update (10.0.16299.15) et versions ultérieures, une contrainte de sujet unique peut être combinée avec une contrainte de liste
- Une combinaison de contraintes de liste et/ou de contraintes de fichier de grammaire.

> [!Important]
> Appelez la méthode **[SpeechRecognizer.CompileConstraintsAsync](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.compileconstraintsasync)** pour compiler les contraintes avant de lancer le processus de reconnaissance.

## <a name="specify-a-web-search-grammar-speechrecognitiontopicconstraint"></a>Spécifier une grammaire de recherche web (SpeechRecognitionTopicConstraint)

Les contraintes de sujet (dictée ou grammaire de recherche web) doivent être ajoutées à la collection de contraintes d’un moteur de reconnaissance vocale.

> [!NOTE]
> Vous pouvez utiliser un objet [SpeechRecognitionListConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint) conjointement avec un [SpeechRecognitionTopicConstraint](https://docs.microsoft.com/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint) pour accroître la précision de la dictée grâce à un jeu de mots clés spécifiques à un domaine que vous pensez susceptibles d’être utilisés lors de la dictée.

Ici, nous ajoutons une grammaire de recherche web à la collection de contraintes.

```CSharp
private async void WeatherSearch_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Listen for audio input issues.
    speechRecognizer.RecognitionQualityDegrading += speechRecognizer_RecognitionQualityDegrading;

    // Add a web search grammar to the recognizer.
    var webSearchGrammar = new Windows.Media.SpeechRecognition.SpeechRecognitionTopicConstraint(Windows.Media.SpeechRecognition.SpeechRecognitionScenario.WebSearch, "webSearch");


    speechRecognizer.UIOptions.AudiblePrompt = "Say what you want to search for...";
    speechRecognizer.UIOptions.ExampleText = @"Ex. 'weather for London'";
    speechRecognizer.Constraints.Add(webSearchGrammar);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();
    //await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="specify-a-programmatic-list-constraint-speechrecognitionlistconstraint"></a>Spécifier une contrainte de liste de programmation (SpeechRecognitionListConstraint)

Les contraintes de liste doivent être ajoutées à la collection de contraintes d’un moteur de reconnaissance vocale.

Gardez à l’esprit les points suivants :

- Vous pouvez ajouter plusieurs contraintes de liste à une collection de contraintes.
- Vous pouvez utiliser toute collection implémentant **IIterable&lt;String&gt;** pour les valeurs de chaîne.

Ici, nous spécifions par programmation un tableau de mots sous la forme d’une contrainte de liste et nous l’ajoutons à la collection de contraintes d’un moteur de reconnaissance vocale.

```CSharp
private async void YesOrNo_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // You could create this array dynamically.
    string[] responses = { "Yes", "No" };


    // Add a list constraint to the recognizer.
    var listConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionListConstraint(responses, "yesOrNo");

    speechRecognizer.UIOptions.ExampleText = @"Ex. 'yes', 'no'";
    speechRecognizer.Constraints.Add(listConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

## <a name="specify-an-srgs-grammar-constraint-speechrecognitiongrammarfileconstraint"></a>Spécifier une contrainte de grammaire SRGS (SpeechRecognitionGrammarFileConstraint)

Les fichiers de grammaire SRGS doivent être ajoutés à la collection de contraintes d’un moteur de reconnaissance vocale.

La norme SRGS version 1.0 définit le langage de balisage standard à l’aide duquel vous pouvez créer des grammaires au format XML pour la reconnaissance vocale. Les applications Windows universelles proposent plusieurs méthodes pour créer des grammaires de reconnaissance vocale. Toutefois, comme vous le constaterez peut-être, la norme SRGS permet de créer des grammaires qui donnent les meilleurs résultats, en particulier dans les scénarios de reconnaissance vocale plus élaborés.

Les grammaires SRGS offrent un ensemble complet de fonctionnalités pour vous aider à établir une interaction vocale complexe pour vos applications. Les grammaires SRGS vous permettent notamment d’effectuer les tâches suivantes :

- spécifier l’ordre dans lequel les mots et les expressions doivent être prononcés pour qu’ils soient reconnus ;
- combiner des mots issus de plusieurs listes et expressions ;
- créer des liens vers d’autres grammaires ;
- affecter une valeur à un mot ou à une expression de remplacement pour augmenter ou diminuer ses chances d’être associé à une saisie vocale ;
- inclure des mots ou des expressions facultatifs ;
- utiliser des règles spéciales permettant de filtrer des saisies vocales non spécifiées ou imprévues, comme des paroles aléatoires qui ne correspondent pas à la grammaire, ou le bruit de fond ;
- utiliser la sémantique pour définir ce que la reconnaissance vocale signifie pour votre application ;
- spécifier des prononciations (soit intégrées à une grammaire, soit via un lien vers un lexique).

Pour plus d’informations sur les éléments et les attributs SRGS, voir [Informations de référence XML sur la grammaire SRGS](https://msdn.microsoft.com/library/hh361653). Pour commencer à créer une grammaire SRGS, voir [Comment créer une grammaire XML de base](https://msdn.microsoft.com/library/hh361658).

Gardez à l’esprit les points suivants :

- Vous pouvez ajouter plusieurs contraintes de fichier de grammaire à une collection de contraintes.
- Utilisez l’extension de fichier .grxml pour les documents de grammaire XML qui sont conformes aux règles SRGS.

L’exemple suivant utilise une grammaire SRGS définie dans un fichier nommé srgs.grxml (décrit ultérieurement). Dans les propriétés du fichier, l’option **Action de package** a la valeur **Contenu**, avec l’option **Copier dans le répertoire de sortie** définie sur **Toujours copier** :

```CSharp
private async void Colors_Click(object sender, RoutedEventArgs e)
{
    // Create an instance of SpeechRecognizer.
    var speechRecognizer = new Windows.Media.SpeechRecognition.SpeechRecognizer();

    // Add a grammar file constraint to the recognizer.
    var storageFile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///Colors.grxml"));
    var grammarFileConstraint = new Windows.Media.SpeechRecognition.SpeechRecognitionGrammarFileConstraint(storageFile, "colors");

    speechRecognizer.UIOptions.ExampleText = @"Ex. 'blue background', 'green text'";
    speechRecognizer.Constraints.Add(grammarFileConstraint);

    // Compile the constraint.
    await speechRecognizer.CompileConstraintsAsync();

    // Start recognition.
    Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = await speechRecognizer.RecognizeWithUIAsync();

    // Do something with the recognition result.
    var messageDialog = new Windows.UI.Popups.MessageDialog(speechRecognitionResult.Text, "Text spoken");
    await messageDialog.ShowAsync();
}
```

Ce fichier SRGS (srgs.grxml) inclut des balises d’interprétation sémantique. Ces balises fournissent un mécanisme permettant de retourner à votre application les données correspondant à la grammaire. World Wide Web Consortium les grammaires doivent se conformer à la spécification SISR (W3C) [interprétation sémantique pour la reconnaissance vocale () 1,0](https://www.w3.org/TR/semantic-interpretation/) .

Ici, nous écoutons des variantes de « yes » et « no ».

```CSharp
<grammar xml:lang="en-US" 
         root="yesOrNo"
         version="1.0" 
         tag-format="semantics/1.0"
         xmlns="http://www.w3.org/2001/06/grammar">

    <!-- The following rules recognize variants of yes and no. -->
      <rule id="yesOrNo">
         <one-of>
            <item>
              <one-of>
                 <item>yes</item>
                 <item>yeah</item>
                 <item>yep</item>
                 <item>yup</item>
                 <item>un huh</item>
                 <item>yay yus</item>
              </one-of>
              <tag>out="yes";</tag>
            </item>
            <item>
              <one-of>
                 <item>no</item>
                 <item>nope</item>
                 <item>nah</item>
                 <item>uh uh</item>
               </one-of>
               <tag>out="no";</tag>
            </item>
         </one-of>
      </rule>
</grammar>
```

## <a name="manage-constraints"></a>Gérer les contraintes

Quand une collection de contraintes est chargée pour la reconnaissance vocale, votre application peut gérer les contraintes qui sont activées pour les opérations de reconnaissance vocale en affectant à la propriété [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.ispeechrecognitionconstraint.isenabled) d’une contrainte la valeur **true** ou **false**. Le paramètre par défaut est **true**.

Plutôt que de charger, décharger et compiler des contraintes pour chaque opération de reconnaissance vocale, il est généralement plus efficace de charger les contraintes une fois, et de les activer ou désactiver en fonction des besoins. Utilisez la propriété [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.ispeechrecognitionconstraint.isenabled), si nécessaire.

En limitant le nombre de contraintes, vous limitez la quantité de données que le moteur de reconnaissance vocale doit parcourir pour trouver une correspondance à la saisie vocale de l’utilisateur. Cela permet d’améliorer aussi bien les performances que la précision de la reconnaissance vocale.

Déterminez les contraintes à activer en fonction des expressions susceptibles d’être communiquées à votre application dans le cadre de la reconnaissance vocale. Par exemple, si votre application a pour contexte l’affichage d’une couleur, il est inutile d’activer une contrainte qui reconnaît les noms d’animaux.

Pour informer l’utilisateur des expressions qu’il peut énoncer, utilisez les propriétés [**SpeechRecognizerUIOptions.AudiblePrompt**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizeruioptions.audibleprompt) et [**SpeechRecognizerUIOptions.ExampleText**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizeruioptions.exampletext), que vous définissez à l’aide de la propriété [**SpeechRecognizer.UIOptions**](https://docs.microsoft.com/uwp/api/windows.media.speechrecognition.speechrecognizer.uioptions). En informant les utilisateurs de ce qu’ils peuvent dire pendant l’opération de reconnaissance vocale, ils seront plus à même de prononcer une expression qui pourra être associée à une contrainte active.

## <a name="related-articles"></a>Articles associés

- [Interactions vocales](speech-interactions.md)

### <a name="samples"></a>Exemples

- [Exemple de reconnaissance vocale et de synthèse vocale](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SpeechRecognitionAndSynthesis)