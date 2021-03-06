---
Description: Dans cette procédure pas à pas, vous allez créer, exécuter et gérer votre première expérience avec des tests A/B.
title: Créer et exécuter votre première expérience
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, tests A/B, expériences
ms.localizationpriority: medium
ms.openlocfilehash: 463eb17d341ccad494058861b2e6d1cfd276005e
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334677"
---
# <a name="create-and-run-your-first-experiment"></a>Créer et exécuter votre première expérience

Dans cette procédure pas à pas, vous allez :
* Créer une expérimentation [projet](run-app-experiments-with-a-b-testing.md#terms) dans partenaires qui définit plusieurs variables à distance qui représentent le texte et la couleur d’un bouton de l’application.
* Créer une application avec du code qui Récupère les valeurs des variables à distance, utilise ces données pour modifier la couleur d’arrière-plan d’un bouton et afficher les journaux et données d’événement de conversion vers les partenaires.
* créer une expérience dans le projet pour tester si la modification de la couleur d’arrière-plan du bouton d’application augmente effectivement le nombre de clics de bouton ;
* exécuter l’application pour collecter des données d’expérience ;
* Passez en revue les résultats de l’expérience dans l’espace partenaires, choisissez une variante à activer pour tous les utilisateurs de l’application et terminer l’expérience.

Pour une vue d’ensemble de A / B test avec des partenaires, consultez [exécuter des expériences d’application avec un test a / B](run-app-experiments-with-a-b-testing.md).

## <a name="prerequisites"></a>Prérequis

Pour suivre cette procédure pas à pas, vous devez disposer d’un compte espace partenaires et vous devez configurer votre ordinateur de développement comme décrit dans [exécuter des expériences d’application avec un test a / B](run-app-experiments-with-a-b-testing.md).

## <a name="create-a-project-with-remote-variables-in-partner-center"></a>Créer un projet avec des variables à distance dans le centre de partenaires

1. Connectez-vous à l'[Espace partenaires](https://partner.microsoft.com/dashboard).
2. Si vous disposez déjà d’une application dans le centre de partenaires que vous souhaitez utiliser pour créer une expérience, sélectionnez cette application dans le centre de partenaires. Si vous n’avez pas encore d’une application dans le centre de partenaires, [créer une application en réservant un nom](../publish/create-your-app-by-reserving-a-name.md) , puis sélectionnez cette application dans le centre de partenaires.
3. Dans le volet de navigation, cliquez sur **Services**, puis sur **Expérimentation**.
4. Dans la section **Projets** de la page suivante, cliquez sur le bouton **Nouveau projet**.
5. Dans la page **Nouveau projet**, entrez le nom **Expériences sur les clics de bouton** pour votre nouveau projet.
6. Développez la section **Variables distantes**, puis cliquez sur **Ajouter une variable** à quatre reprises. Vous devez maintenant avoir quatre lignes de variable vides.
  * Dans la première ligne, tapez **buttonText** pour le nom de la variable et **Bouton gris** dans la colonne **Valeur par défaut**.
  * Dans la deuxième ligne, tapez **r** pour le nom de la variable et **128** dans la colonne **Valeur par défaut**.
  * Dans la troisième ligne, tapez **g** pour le nom de la variable et **128** dans la colonne **Valeur par défaut**.
  * Dans la quatrième ligne, tapez **b** pour le nom de la variable et **128** dans la colonne **Valeur par défaut**.
7. Cliquez sur **Enregistrer** et notez la valeur [ID de projet](run-app-experiments-with-a-b-testing.md#terms) qui apparaît dans la section **Intégration au SDK**. Dans la section suivante, vous allez mettre à jour votre code d’application et faire référence à cette valeur dans votre code.

## <a name="code-the-experiment-in-your-app"></a>Coder l’expérience dans votre application

1. Dans Visual Studio, créez un projet de plateforme Windows universelle à l’aide de Visual C#. Nommez le projet **SampleExperiment**.
2. Dans l’Explorateur de solutions, développez votre nœud de projet, cliquez avec le bouton droit sur **Références**, puis sélectionnez **Ajouter une référence**.
3. Dans le **Gestionnaire de références**, développez **Windows universel**, puis cliquez sur **Extensions**.
4. Dans la liste des kits de développement logiciel (SDK), cochez la case en regard de **Microsoft Engagement Framework** et cliquez sur **OK**.
5. Dans l’**Explorateur de solutions**, double-cliquez sur MainPage.xaml pour ouvrir le concepteur pour la page principale de l’application.
6. Faites glisser un **Bouton** de la **Boîte à outils** vers la page.
7. Double-cliquez sur le bouton dans le concepteur pour ouvrir le fichier de code et ajoutez un gestionnaire d’événements pour l’événement **Click**.  
8. Remplacez l’ensemble du contenu du fichier de code par le code ci-après. Affecter le ```projectId``` à la variable le [ID de projet](run-app-experiments-with-a-b-testing.md#terms) valeur que vous avez obtenue à partir du centre de partenaires dans la section précédente.
    [!code-csharp[SampleExperiment](./code/StoreSDKSamples/cs/ExperimentPage.xaml.cs#SampleExperiment)]

9. Enregistrez le fichier de code et créez le projet.

## <a name="create-the-experiment-in-partner-center"></a>Création de l’expérience dans l’espace partenaires

1. Retour à la **expériences de cliquez sur bouton** page de projet dans l’espace partenaires.
2. Dans la section **Expériences**, cliquez sur le bouton **Nouvelle expérience**.
3. Dans la section **Experiment details (Détails de l’expérience)**, tapez le nom **Optimiser les clics de bouton** dans le champ **Experiment name (Nom de l’expérience)**.
4. Dans la section **View event (Événement d’affichage)**, tapez **userViewedButton** dans le champ **View event name (Nom de l’événement d’affichage)**. Notez que ce nom correspond à la chaîne de l’événement d’affichage enregistré dans le code que vous avez ajoutée dans la section précédente.
5. Dans la section **Objectifs et événements de conversion**, entrez les valeurs suivantes :
  * Dans le champ **Nom d’objectif**, tapez **Increase Button Clicks**.
  * Dans le champ **Nom de l’événement de conversion**, tapez le nom **userClickedButton**. Notez que ce nom correspond à la chaîne de l’événement de conversion enregistré dans le code que vous avez ajoutée dans la section précédente.
  * Dans le champ **Objectif**, choisissez **Agrandir**.
6. Dans la section **Variables distantes et variantes**, vérifiez que la case **Répartir en valeurs égales** est cochée pour que les variantes soient distribuées équitablement à votre application.
7. Ajoutez des variables à votre expérience :
    1. Cliquez sur le contrôle de liste déroulante, choisissez **buttonText**, puis cliquez sur **Ajouter une variable**. La chaîne **Bouton gris** apparaît automatiquement dans la colonne **Variante A** (cette valeur est dérivée des paramètres du projet). Dans la colonne **Variante B**, tapez **Bouton bleu**.
    2. Cliquez une nouvelle fois sur le contrôle de liste déroulante, choisissez **r**, puis cliquez sur **Ajouter une variable**. La chaîne **128** doit apparaître automatiquement dans la colonne **Variante A**. Dans la colonne **Variante B**, tapez **1**.
    3. Cliquez une nouvelle fois sur le contrôle de liste déroulante, choisissez **g**, puis cliquez sur **Ajouter une variable**. La chaîne **128** doit apparaître automatiquement dans la colonne **Variante A**. Dans la colonne **Variante B**, tapez **1**.  
    4. Cliquez une nouvelle fois sur le contrôle de liste déroulante, choisissez **b**, puis cliquez sur **Ajouter une variable**. La chaîne **128** doit apparaître automatiquement dans la colonne **Variante A**. Dans la colonne **Variante B**, tapez **255**.  
8. Cliquez sur **Enregistrer**, puis sur **Activer**.

> [!IMPORTANT]
> Une fois que vous avez activé une expérience, vous ne pouvez plus en modifier les paramètres, sauf si vous avez cliqué sur la case **Editable experiment (Expérience modifiable)** quand vous avez créé l’expérience. D’habitude, nous vous recommandons de coder l’expérience dans votre application avant de l’activer.

## <a name="run-the-app-to-gather-experiment-data"></a>Exécuter l’application pour collecter des données d’expérience

1. Exécutez l’application **SampleExperiment** que vous avez créée précédemment.
2. Vérifiez que vous voyez soit un bouton bleu, soit un bouton gris. Cliquez sur le bouton, puis fermez l’application.
3. Répétez les étapes ci-dessus plusieurs fois sur le même ordinateur pour confirmer que votre application affiche la même couleur de bouton.

## <a name="review-the-results-and-complete-the-experiment"></a>Passer en revue les résultats et terminer l’expérience

Laissez s’écouler plusieurs heures après avoir terminé la section précédente, puis suivez ces étapes pour passer en revue les résultats de votre expérience et terminer l’expérience.

> [!NOTE]
> Dès que vous activez une expérience, partenaires commence immédiatement la collecte de données à partir de toutes les applications instrumentés pour consigner les données de votre expérience. Toutefois, il peut prendre plusieurs heures pour données expérience s’affichent dans l’espace partenaires.

1. De partenaires, revenez à la **expérimentation** page de votre application.
2. Dans la section **Active experiments** (Expériences actives), cliquez sur **Optimize Button Clicks** (Optimiser les clics de bouton) pour accéder à la page de cette expérience.
3. Vérifiez que les résultats affichés dans les sections **Résumé des résultats** et **Détails des résultats** correspondent à ce que vous attendez. Pour plus d’informations sur ces sections, consultez [gérer votre expérience dans partenaires](manage-your-experiment.md#review-the-results-of-your-experiment).
    > [!NOTE]
    > Partenaires signale uniquement le premier événement de conversion pour chaque utilisateur sur une période de 24 heures. Si un utilisateur déclenche plusieurs événements de conversion dans votre application au cours d’une période de 24 heures, seul le premier événement de conversion est signalé. Cette approche est destinée à éviter qu’un utilisateur unique avec de nombreux événements de conversion ne fausse les résultats de l’expérience pour un groupe représentatif d’utilisateurs.

4. Vous êtes désormais prêt à terminer l’expérience. Dans la section **Résumé des résultats**, cliquez sur **Basculer** dans la colonne **Variante B**. Cela permet de basculer tous les utilisateurs de votre application sur le bouton bleu.
5. Cliquez sur **OK** pour confirmer que vous souhaitez mettre fin à l’expérience.
6. Exécutez l’application **SampleExperiment** que vous avez créée dans la section précédente.
7. Vérifiez que vous voyez un bouton bleu. Notez que jusqu’à 2 minutes peuvent être nécessaires pour que votre application reçoive une affectation de variante mise à jour.

## <a name="related-topics"></a>Rubriques connexes

* [Créer un projet et définir des variables à distance dans Partner Center](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [Coder votre application pour l’expérimentation](code-your-experiment-in-your-app.md)
* [Définir votre expérience dans l’espace partenaires](define-your-experiment-in-the-dev-center-dashboard.md)
* [Gérer votre expérience dans l’espace partenaires](manage-your-experiment.md)
* [Exécuter des expériences d’application avec un test a / B](run-app-experiments-with-a-b-testing.md)
