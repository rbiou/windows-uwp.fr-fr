---
Description: Gérez les options d’envoi, telles que les options de conservation de la publication, les notes pour la certification, et bien plus encore.
title: Gérer les options de soumission
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, mise en attente de publication, date de publication, envoi de la soumission à publier, approbation de fonctionnalité restreinte
ms.localizationpriority: medium
ms.openlocfilehash: c2548ddd35fc50f62727d986d2d934d2346eb7b5
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210325"
---
# <a name="manage-submission-options"></a>Gérer les options de soumission

La page **Options de soumission** du processus de soumission d’application vous permet de fournir plus d’informations pour nous aider à tester votre produit correctement. Cette étape est facultative, mais elle est recommandée pour de nombreuses soumissions. Vous pouvez également définir des options de mise en attente de publication si vous souhaitez différer le processus de publication.


## <a name="publishing-hold-options"></a>Options de mise en attente de publication

Par défaut, nous publierons votre soumission dès qu’elle aura obtenu la certification (ou selon les dates que vous avez spécifiées dans la section [Planification](configure-precise-release-scheduling.md) de la page **Tarification et disponibilité**). Si vous le souhaitez, vous pouvez choisir de mettre en attente la publication de votre soumission jusqu’à une date donnée, ou jusqu’à ce que vous indiquiez manuellement qu’elle doit être publiée. Les options disponibles dans cette section sont décrites ci-dessous. 


### <a name="publish-your-submission-as-soon-as-it-passes-certification-or-per-dates-you-specify"></a>Publier votre soumission dès qu’elle a obtenu la certification (ou selon les dates spécifiées)

La sélection par défaut est **Publish this submission as soon as it passes certification (or per dates you selected in the Schedule section)** . Cela signifie que le processus de publication de votre soumission commencera dès qu’elle aura obtenu la certification, sauf si vous avez configuré des dates dans la section [Planification](configure-precise-release-scheduling.md) de la page **Tarification et disponibilité**.   

Pour la plupart des soumissions, nous recommandons de laisser la section **Options de mise en attente de publication** définie sur cette option. Si vous souhaitez spécifier les dates auxquelles votre soumission doit être publiée, utilisez l’option **Publish this submission as soon as it passes certification (or per dates you selected in the Schedule section)** . Le fait de laisser cette section définie sur l’option par défaut n’aura pas pour conséquence la publication de la soumission avant les dates que vous avez définies dans la section **Planification**. Les dates que vous avez sélectionnées dans la section **planification** sont utilisées pour déterminer à quel moment votre produit devient disponible pour les clients du Store.


### <a name="publish-your-submission-manually"></a>Publier votre soumission manuellement

Si vous ne voulez pas encore définir de date de publication et que vous préférez que votre soumission reste non publiée jusqu’à ce que vous décidiez de déclencher manuellement le processus de publication, vous pouvez choisir **Ne publiez pas cette soumission tant que je n'ai pas sélectionné Publier maintenant**. La sélection de cette option signifie que votre soumission ne sera pas publiée tant vous n’aurez pas indiqué qu’elle doit l’être. Une fois que votre envoi est certifié, vous pouvez le publier en sélectionnant **publier maintenant** dans la page État de la certification, ou en sélectionnant une date spécifique de la même manière que celle décrite ci-dessous.


### <a name="start-publishing-your-submission-on-a-certain-date"></a>Démarrer la publication de votre soumission à une date donnée

Choisissez **Lancer la publication de cette soumission le** pour vous assurer que la soumission ne sera pas publiée avant une date donnée. Avec cette option, votre soumission sera publiée aussitôt que possible à la date spécifiée ou après. La date doit être postérieure de 24 heures au moins. En parallèle de la date, vous pouvez également définir l’heure à laquelle la publication de la soumission doit démarrer. 

Vous pouvez modifier cette date de publication après avoir envoyé votre produit, à condition qu’elle n’ait pas encore entré l’étape de publication. 
 
Comme indiqué précédemment, si vous souhaitez spécifier certaines dates de publication pour votre soumission, utilisez l’option **Publish this submission as soon as it passes certification (or per dates you selected in the Schedule section)** et laissez l’option **Options de mise en attente de publication** définie sur la sélection par défaut. L’utilisation de l’option **Lancer la publication de cette soumission le** signifie que le processus de publication de votre soumission ne démarrera qu'à cette date. Toutefois, des retards lors de la certification ou de la publication peuvent différer la publication réelle par rapport à la date demandée. 


## <a name="notes-for-certification"></a>Notes de certification

Lorsque vous soumettez votre application, vous avez la possibilité d’utiliser la section **Remarques pour la certification** pour fournir des informations supplémentaires aux testeurs de certification. Ces informations peuvent aider à garantir que votre application est testée correctement. 

Pour plus d’informations, voir [Remarques pour la certification](notes-for-certification.md).


## <a name="restricted-capabilities"></a>Fonctionnalités restreintes

Si nous détections que vos packages déclarent certaines [fonctionnalités restreintes](../packaging/app-capability-declarations.md#restricted-capabilities), vous devrez fournir des informations dans cette section afin de recevoir une approbation. Pour chaque fonctionnalité, faites-nous part de la raison pour laquelle votre application a besoin de déclarer la fonctionnalité et dites-nous comment elle sera utilisée. Veillez à fournir autant de détails que possible afin de nous aider à comprendre la raison pour laquelle votre produit nécessite de déclarer ces fonctionnalités. 

Lors du processus de certification, nos testeurs examinent les informations que vous avez fournies afin de déterminer si votre soumission est approuvée pour utiliser ces fonctionnalités. Notez que cette opération peut avoir pour effet de rallonger la durée nécessaire pour que votre soumission arrive à bout du processus de certification. Si nous approuvons votre utilisation des fonctionnalités, votre application continuera le reste du processus de certification. De manière générale, il vous est inutile de répéter le processus d'approbation des fonctionnalités lorsque vous appliquez des mises à jour à votre application (à moins que vous ne déclariez des fonctionnalités supplémentaires). 

Si nous n'approuvons pas votre utilisation des fonctionnalités, votre soumission n'obtiendra pas de certification et nous fournirons des commentaires dans le rapport de certification. Vous pouvez ensuite choisir de créer une nouvelle soumission et de télécharger les packages ne déclarant aucune fonctionnalité, ou, le cas échéant, de répondre aux problèmes liés à votre utilisation des fonctionnalités afin de demander l'approbation dans une nouvelle soumission.

Notez qu’il existe certaines fonctionnalités restreintes qui seront très rarement approuvées. Pour plus d'informations sur chacune des fonctionnalités restreintes, consultez [Déclarations des fonctionnalités d’application](../packaging/app-capability-declarations.md#restricted-capabilities).

