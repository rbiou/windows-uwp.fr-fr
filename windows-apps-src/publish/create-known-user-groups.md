---
Description: Découvrez comment créer des groupes d’utilisateurs connus à utiliser pour la distribution de version d’évaluation de package et bien davantage.
title: Créer des groupes d’utilisateurs connus
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, groupe ciblé, clients, groupe de versions d’évaluation, groupes d’utilisateurs connus, utilisateurs connus
ms.localizationpriority: medium
ms.openlocfilehash: 1fcb111121511553bba22cef55f94125d47e9f21
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605284"
---
# <a name="create-known-user-groups"></a>Créer des groupes d’utilisateurs connus

Les groupes d’utilisateurs connus vous permettent d’ajouter des personnes spécifiques à un groupe à l’aide de l’adresse e-mail associée à leur compte Microsoft. Ces groupes d’utilisateurs connus sont le plus souvent utilisés pour distribuer des packages spécifiques à un groupe de personnes sélectionné avec les [versions d’évaluation de package](package-flights.md) ou pour la distribution d’une soumission à un [public privé](choose-visibility-options.md#audience). Ils peuvent également être utilisés pour les campagnes visant à susciter l’intérêt, comme l’envoi de [notifications ciblées](send-push-notifications-to-your-apps-customers.md) ou d’[offres ciblées](use-targeted-offers-to-maximize-engagement-and-conversions.md) à un groupe de clients spécifiques.

Pour être comptabilisée en tant que membre du groupe, chaque personne doit être authentifiée auprès du Windows Store à l’aide du compte Microsoft associé à l’adresse e-mail que vous fournissez. Pour télécharger l’application avec la distribution de version d’évaluation de package, les membres du groupe doivent utiliser une version de Windows 10 qui prend en charge les versions d’évaluation de package (Windows.Desktop build 10586 ou ultérieure, Windows.Mobile build 10586.63 ou ultérieure, ou Xbox One). Avec les soumissions pour un public privé, les membres du groupe doivent utiliser Windows 10, version 1607 ou ultérieure (y compris Xbox One).

## <a name="to-create-a-known-user-group"></a>Pour créer un groupe d’utilisateurs connus

1. Dans [partenaires](https://partner.microsoft.com/dashboard), développez **engager** dans le menu de navigation gauche, puis sélectionnez **les groupes de clients**. 
2. Dans la section **Mes groupes de clients**, sélectionnez **Créer un groupe**.
3. Sur la page suivante, entre un nom pour votre groupe dans la zone **Nom du groupe**.
4. Assurez-vous que la case d’option **Groupe d'utilisateurs connus** est sélectionnée.
5. Entrez les adresses e-mail des personnes que vous souhaitez ajouter au groupe. Vous devez inclure au moins une adresse e-mail, le nombre maximal d’adresses étant de 10 000. Vous pouvez saisir les adresses e-mail directement dans le champ (séparées par des espaces, des virgules, des points-virgules ou des sauts de ligne) ou cliquer sur le lien **Importer au format CSV** afin de créer le groupe de versions d’évaluation à partir d’une liste d’adresses e-mail d’un fichier .csv.
6. Sélectionnez **Enregistrer**.

Vous pouvez alors utiliser le groupe à votre convenance.

Vous pouvez également créer un groupe d’utilisateurs connus en sélectionnant **Create a flight group** sur la page de création de [version d’évaluation de package](package-flights.md). Notez que si vous effectuez cette opération, vous devrez saisir de nouveau toutes les informations que vous avez déjà fournies sur la page de création de versions d’évaluation de package.

> [!IMPORTANT]
> Lorsque vous utilisez des groupes d’utilisateurs connus avec la distribution de version d’évaluation de package, veillez à obtenir le consentement des utilisateurs ajoutés à votre groupe, et vérifiez qu’ils comprennent que les packages qui leur seront proposés seront différents de ceux de votre soumission sans version d’évaluation. 

## <a name="to-edit-a-known-user-group"></a>Pour modifier un groupe d’utilisateurs connus

Vous ne pouvez pas supprimer un groupe d’utilisateurs connus de partenaires (ou modifiez son nom) après sa création, mais vous pouvez modifier son appartenance à tout moment.

Pour examiner et modifier vos groupes d’utilisateurs connus, développez le menu **Engager** dans le menu de navigation de gauche, puis sélectionnez **Groupes de clients**. Sous **Mes groupes de clients**, sélectionnez le nom du groupe que vous souhaitez modifier. Vous pouvez également modifier un groupe d’utilisateurs connus de la page de création de version d’évaluation de package en sélectionnant **Afficher et gérer les groupes existants** lorsque vous créez une version d’évaluation, ou en sélectionnant le nom du groupe sur la page de présentation d’une version d’évaluation de package. 

Une fois que vous avez sélectionné le groupe à modifier, vous pouvez ajouter ou supprimer des adresses e-mail directement dans le champ.

Si vous souhaitez apporter des modifications plus importantes, sélectionnez **Exporter au format CSV** pour enregistrer vos informations d’appartenance au groupe dans un fichier .csv. Effectuez vos modifications dans ce fichier, puis cliquez sur **Importer au format CSV** pour utiliser la nouvelle version afin de mettre à jour l’appartenance au groupe.

Notez que l’implémentation des modifications d’appartenance au groupe peut prendre jusqu’à 30 minutes. Vous n’avez pas besoin de publier une nouvelle soumission pour que les nouveaux membres du groupe puissent accéder à votre soumission par le biais de versions d’évaluation de package ou du public privé ; ils y auront accès dès que les modifications seront appliquées. 






