---
title: Impression 3D à partir de votre application
description: Découvrez comment ajouter des fonctionnalités d’impression 3D à votre application Windows universelle. Cette rubrique explique comment lancer la boîte de dialogue d’impression 3D après s’être assuré que votre modèle 3D est imprimable et au format qui convient.
ms.assetid: D78C4867-4B44-4B58-A82F-EDA59822119C
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 3dprinting, impression 3d
ms.localizationpriority: medium
ms.openlocfilehash: e2ed99720afdccef297d46853d4a2445b497195e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321697"
---
# <a name="3d-printing-from-your-app"></a>Impression 3D à partir de votre application

**API importantes**

-   [**Windows.Graphics.Printing3D**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D)

Découvrez comment ajouter des fonctionnalités d’impression 3D à votre application Windows universelle. Cette rubrique explique comment charger des données de géométrie 3D dans votre application et lancer la boîte de dialogue d’impression 3D après avoir vérifié que votre modèle 3D est imprimable et au format correct. Pour obtenir un exemple de mise en pratique de ces procédures, voir l’[Exemple d’impression 3D UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting).

> [!NOTE]
> Dans l’exemple de code de ce guide, le signalement et le traitement des erreurs sont grandement simplifiés.

## <a name="setup"></a>Installation


Dans la classe d’application devant recevoir les fonctionnalités d’impression 3D, ajoutez l’espace de noms [**Windows.Graphics.Printing3D**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D).

[!code-cs[3DPrintNamespace](./code/3dprinthowto/cs/MainPage.xaml.cs#Snippet3DPrintNamespace)]

Les espaces de noms supplémentaires suivants sont utilisés dans ce guide.

[!code-cs[OtherNamespaces](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOtherNamespaces)]

Attribuez ensuite des champs de membre utiles à votre classe. Déclarez un objet [**Print3DTask**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTask) qui représente la tâche d’impression à transmettre au pilote d’impression. Déclarez un objet [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) pour contenir le fichier de données 3D d’origine à charger dans l’application. Déclarez un objet [**Printing3D3MFPackage**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Printing3D3MFPackage), qui représente un modèle 3D prêt à imprimer avec toutes les métadonnées nécessaires.

[!code-cs[DeclareVars](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeclareVars)]

## <a name="create-a-simple-ui"></a>Créer une interface utilisateur simple

Cet exemple utilise trois contrôles utilisateur : un bouton de chargement qui place un fichier dans la mémoire du programme, un bouton de correction qui modifie le fichier si nécessaire et un bouton d’impression qui lance une tâche d’impression. Le code suivant permet de créer ces boutons (avec leurs gestionnaires d’événements par clic) dans le fichier XAML de la classe .cs correspondante.

[!code-xml[Buttons](./code/3dprinthowto/cs/MainPage.xaml#SnippetButtons)]

Ajoutez un **TextBlock** pour le commentaire de l’interface utilisateur.

[!code-xml[OutputText](./code/3dprinthowto/cs/MainPage.xaml#SnippetOutputText)]



## <a name="get-the-3d-data"></a>Obtenir les données 3D


La méthode par laquelle votre application acquiert les données de géométrie 3D à imprimer varie. Votre application peut récupérer les données à partir d’une analyse 3D, télécharger les données du modèle à partir d’une ressource Web ou générer un maillage 3D par programmation à l’aide de formules mathématiques ou de l’entrée utilisateur. Par souci de simplicité, ce guide montre comment charger un fichier de données 3D (d’un type de fichier courant) dans la mémoire du programme à partir d’un stockage de fichiers. La [bibliothèque de modèles 3D Builder](https://developer.microsoft.com/windows/hardware/3d-print/windows-3d-printing) fournit plusieurs modèles que vous pouvez facilement télécharger sur votre appareil.

Dans votre méthode `OnLoadClick`, utilisez la classe [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) pour charger un seul fichier dans la mémoire de votre application.

[!code-cs[FileLoad](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileLoad)]

## <a name="use-3d-builder-to-convert-to-3d-manufacturing-format-3mf"></a>Utiliser 3D Builder pour une conversion au format 3D Manufacturing Format (.3mf)

À ce stade, vous êtes en mesure de charger un fichier de données 3D dans la mémoire de votre application. Il existe de nombreux formats de données de géométrie 3D, mais ils ne sont pas tous adaptés à l’impression 3D. Windows 10 utilise le type de fichier 3D Manufacturing Format (.3mf) pour toutes les tâches d’impression 3D.

> [!NOTE]  
> Le type de fichier .3mf offre d’autres fonctionnalités non abordées dans ce tutoriel. Pour en savoir plus sur le format 3MF et les fonctionnalités proposées aux fabricants et clients de produits 3D, reportez-vous à la [Spécification 3MF](https://3mf.io/what-is-3mf/3mf-specification/). Pour savoir comment utiliser ces fonctionnalités à l’aide des API Windows 10, consultez le didacticiel [Générer un package 3MF](https://docs.microsoft.com/windows/uwp/devices-sensors/generate-3mf).

L’application [3D Builder](https://www.microsoft.com/store/apps/3d-builder/9wzdncrfj3t6) peut ouvrir les formats de fichier 3D les plus courants et les enregistrer sous forme de fichiers .3mf. Dans cet exemple, où le type de fichier peut varier, une solution très simple consiste à ouvrir l’application 3D Builder et à inviter l’utilisateur à enregistrer les données importées dans un fichier .3mf, puis à le recharger.

> [!NOTE]  
> En plus de convertir les formats de fichier, 3D Builder offre des outils simples pour modifier vos modèles, ajouter des données de couleur et effectuer d’autres opérations spécifiques de l’impression. C’est pourquoi il est souvent recommandé de l’intégrer à une application qui prend en charge l’impression 3D.

[!code-cs[FileCheck](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetFileCheck)]

## <a name="repair-model-data-for-3d-printing"></a>Réparer des données de modèle pour l’impression 3D

Les données de modèle 3D ne sont pas toutes imprimables, même dans le type .3mf. Afin que l’imprimante puisse correctement déterminer les espaces à remplir et à laisser vides, le ou les modèles à imprimer doivent (chacun) constituer un ensemble unique et homogène, disposer de normales de surface orientées vers l’extérieur et d’une géométrie de collection. Les problèmes rencontrés dans ces zones peuvent survenir sous diverses formes et peuvent être difficiles à repérer dans des formes complexes. Toutefois, les solutions logicielles actuelles sont souvent adaptées à la conversion de la géométrie brute en formes 3D imprimables. C’est ce qu’on appelle la *réparation* du modèle, qui est effectuée dans la méthode `OnFixClick`.

Le fichier de données 3D doit être converti pour implémenter [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream), qui peut ensuite être utilisé pour générer un objet [**Printing3DModel**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Printing3DModel).

[!code-cs[RepairModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRepairModel)]

L’objet **Printing3DModel** est maintenant réparé et imprimable. Utilisez [**SaveModelToPackageAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.printing3d3mfpackage.savemodeltopackageasync) pour attribuer le modèle à l’objet **Printing3D3MFPackage** que vous avez déclaré pendant la création de la classe.

[!code-cs[SaveModel](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSaveModel)]

## <a name="execute-printing-task-create-a-taskrequested-handler"></a>Exécuter la tâche d’impression : créer un gestionnaire TaskRequested


Par la suite, lorsque la boîte de dialogue d’impression 3D s’affiche et que l’utilisateur choisit de commencer l’impression, votre application doit transmettre les paramètres souhaités au pipeline d’impression 3D. L’API d’impression 3D déclenche l’événement **[TaskRequested](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DManager.TaskRequested)** . Vous devez écrire une méthode pour gérer cet événement de manière appropriée. Comme toujours, la méthode de gestionnaire doit être du même type que son événement : Le **TaskRequested** événement a des paramètres [ **Print3DManager** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DManager) (il s’agit d’une référence à son objet expéditeur) et un [  **Print3DTaskRequestedEventArgs** ](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequestedEventArgs) objet qui conserve la plupart des informations pertinentes.

[!code-cs[MyTaskTitle](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetMyTaskTitle)]

L’objectif principal de cette méthode est d’utiliser le paramètre *args* pour envoyer un **Printing3D3MFPackage** dans le pipeline. Le **Print3DTaskRequestedEventArgs** type a une seule propriété : [**Demande**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtaskrequestedeventargs.request). Elle est de type [**Print3DTaskRequest**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTaskRequest) et représente une demande de travail d’impression. Sa méthode [**CreateTask**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtaskrequest.createtask) permet au programme d’envoyer les informations correctes relatives à votre travail d’impression, et renvoie une référence à l’objet **Print3DTask** qui a été envoyé dans le pipeline d’impression 3D.

**CreateTask** a les paramètres d’entrée suivants : une chaîne pour le nom du travail d’impression, une chaîne pour l’ID de l’imprimante à utiliser, ainsi qu’un délégué [**Print3DTaskSourceRequestedHandler**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedhandler). Le délégué est automatiquement appelé quand l’événement **3DTaskSourceRequested** est déclenché (cette opération est effectuée par l’API elle-même). Il est important de noter que ce délégué est appelé lorsqu’un travail d’impression est lancé et qu’il est responsable de la fourniture du package d’impression 3D approprié.

**Print3DTaskSourceRequestedHandler** prend un paramètre, un objet [**Print3DTaskSourceRequestedArgs**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Printing3D.Print3DTaskSourceRequestedArgs) qui fournit les données à envoyer. [  **SetSource**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dtasksourcerequestedargs.setsource), l’une des méthodes publiques de cette classe, accepte le package à imprimer. Implémentez un délégué **Print3DTaskSourceRequestedHandler** comme suit.

[!code-cs[SourceHandler](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetSourceHandler)]

Ensuite, appelez **CreateTask**, à l’aide du délégué `sourceHandler` que vous venez de définir.

[!code-cs[CreateTask](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetCreateTask)]

L’objet **Print3DTask** renvoyé est attribué à la variable de classe déclarée au début. Vous pouvez, si vous le souhaitez, utiliser cette référence pour gérer certains événements levés par la tâche.

[!code-cs[Optional](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetOptional)]

> [!NOTE]  
> Vous devez implémenter des méthodes `Task_Submitting` et `Task_Completed` si vous voulez les inscrire à ces événements.

## <a name="execute-printing-task-open-3d-print-dialog"></a>Exécuter la tâche d’impression : ouvrir la boîte de dialogue d’impression 3D


L’élément de code final nécessaire est celui qui lance la boîte de dialogue d’impression 3D. À l’instar d’une fenêtre de boîte de dialogue d’impression classique, la boîte de dialogue d’impression 3D fournit un certain nombre d’options d’impression de dernière minute et permet à l’utilisateur de choisir l’imprimante à utiliser (qu’elle soit connectée via USB ou le réseau).

Inscrivez votre méthode `MyTaskRequested` à l’événement **TaskRequested**.

[!code-cs[RegisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetRegisterMyTaskRequested)]

Après avoir inscrit votre gestionnaire d’événements **TaskRequested**, vous pouvez appeler la méthode [**ShowPrintUIAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.printing3d.print3dmanager.showprintuiasync), qui fait apparaître la boîte de dialogue d’impression 3D dans la fenêtre d’application active.

[!code-cs[ShowDialog](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetShowDialog)]

Enfin, il est conseillé de désinscrire vos gestionnaires d’événements une fois que votre application reprend le contrôle.  

[!code-cs[DeregisterMyTaskRequested](./code/3dprinthowto/cs/MainPage.xaml.cs#SnippetDeregisterMyTaskRequested)]

## <a name="related-topics"></a>Rubriques connexes

[Générer un package 3MF](https://docs.microsoft.com/windows/uwp/devices-sensors/generate-3mf)  
[Exemple d’UWP impression 3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 
