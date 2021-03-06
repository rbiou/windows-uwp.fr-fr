---
Description: Vous pouvez créer des campagnes Active Directory dans l’espace partenaires pour améliorer la promotion de votre application et augmenter la base d’utilisateurs de votre application.
title: Création d’une campagne de publicité pour votre application
ms.assetid: 10D94929-92C4-4379-AA5F-6FEF879F2463
ms.date: 02/18/2020
ms.topic: article
keywords: Windows 10, uwp, Active Directory, de campagne, promouvoir
ms.localizationpriority: medium
ms.openlocfilehash: aa5c3c160d3bb69a2ba478606a3c3e04e935088d
ms.sourcegitcommit: 71f9013c41fc1038a9d6c770cea4c5e481c23fbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77507063"
---
# <a name="create-an-ad-campaign-for-your-app"></a>Création d’une campagne de publicité pour votre application

>[!WARNING]
> Depuis le 1er juin 2020, la plateforme de monétisation Microsoft AD pour les applications Windows UWP sera arrêtée. [En savoir plus](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

Vous pouvez créer des campagnes ad dans l' [espace partenaires](https://partner.microsoft.com/dashboard) pour améliorer la promotion de votre application et augmenter sa base d’utilisateurs. Par défaut, nous choisissons le public ciblé pour vos annonces en fonction des paramètres de votre application dans l’espace partenaires, mais vous pouvez éventuellement définir votre propre audience. Vous pouvez également utiliser un ensemble de modèles de publicité par défaut ou charger vers le serveur vos propres conceptions d’annonces. Pour plus d’informations sur les campagnes de publicité, voir [Questions courantes sur les campagnes de publicité](common-questions.md).

Vous ne pouvez créer des campagnes de publicité que pour les applications qui ont réussi la phase de publication finale du [processus de certification des applications](the-app-certification-process.md).

> [!NOTE]
> Cette section de la documentation explique comment créer une campagne publicitaire dans l’espace partenaires. D’autres options de campagne pour créer et gérer des campagnes de publicité par programmation incluent [Vungle](https://vungle.com/) et l' [API Microsoft Store promotions](../monetize/run-ad-campaigns-using-windows-store-services.md).

## <a name="instructions"></a>Instructions

Voici comment créer une campagne de publicité pour promouvoir une application :

1.  Dans le menu de navigation de gauche de l' [espace partenaires](https://partner.microsoft.com/dashboard), développez **attirer** , puis sélectionnez **campagnes Active Directory**.
2.  Sélectionnez **Créer une campagne** (ou si vous avez déjà créé des campagnes auparavant, sélectionnez **Nouvelle campagne**).
3.  Sur la page suivante, dans la section **Type d’objectif**, choisissez l’une des options suivantes :
    * **Augmenter les installations de votre application**. Sélectionnez cette option si votre campagne de publicité est conçue pour convaincre les clients d’installer votre application.
    * **Augmenter l’intérêt pour votre application**. Sélectionnez cette option si votre campagne de publicité vise à inciter les clients à utiliser votre application plus souvent. Lorsque vous sélectionnez cette option, vous pouvez cibler votre campagne de publicité sur les [segments de clientèle](create-customer-segments.md) spécifiques que vous définissez.

4.  Sélectionnez l’application que vous souhaitez promouvoir avec cette campagne. Notez que l’application doit être déjà disponible dans le Windows Store.
5.  Dans le champ **Nom de la campagne**, examinez le nom fourni pour votre campagne et modifiez-le si vous le souhaitez.
6.  Sous **Type de campagne**, choisissez l’une des options suivantes :
    * **Paid ad** : ces publicités s’exécutent dans n’importe quelle application correspondant à l’appareil et à la catégorie de votre application. Pour les nouvelles campagnes créées après le 9 janvier 2017, ces publicités apparaîtront également dans MSN.com, Outlook.com, Skype et les autres propriétés premium de Microsoft. Les campagnes de promotion d’applications qui ciblent les applications et les propriétés Premium de Microsoft sont appelés campagnes *universelles*.
    * **Community ad (free)**  : ces publicités s’exécutent dans les applications publiées par d’autres développeurs, qui créent également des campagnes de publicité de la communauté. Avant de sélectionner cette option, vous devez avoir accepté l’affichage des publicités de la communauté dans la page **Monétiser** -> **Publicités dans l’application**. Pour en savoir plus, voir [À propos des annonces de la communauté](about-community-ads.md).
    * **House ad (free)**  : ces publicités s’exécutent uniquement dans vos applications correspondant au type d’appareil de l’application publiée. Les publicités maison sont gratuites. Pour plus d’informations, consultez [À propos des publicités maison](about-house-ads.md).

7.  Pour les campagnes de publicité payées, vérifiez l’option sélectionnée dans **Durée de la campagne** (période pendant laquelle le budget de votre campagne sera dépensé). L’option par défaut est **Mensuelle**, ce qui signifie que votre budget de campagne sera utilisé tous les mois de manière récurrente jusqu’à l’arrêt de la campagne. Si vous disposez d’un compte Premium, vous pouvez éventuellement choisir l’option **Personnalisée** pour spécifier une date personnalisée et un intervalle de temps pendant lequel votre budget de campagne sera dépensé. Pour plus d’informations sur les comptes premium, consultez [Questions courantes sur les campagnes de publicité](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign).

8.  Vérifiez vos informations de budget et de paiement. (Si vous créez une campagne maison ou une campagne de publicité de la communauté, ces options n’apparaîtront pas, car ces campagnes sont gratuites.)
    * Sous **Budget**, utilisez le curseur pour définir la somme que vous souhaitez dépenser chaque mois pour exécuter la publicité (ou le budget total, si vous avez sélectionné une durée de campagne personnalisée).

        Le budget mensuel est calculé au prorata du mois au cours duquel vous avez créé la campagne de publicité. En d’autres termes, si vous créez une campagne au milieu du mois, vous êtes facturé la moitié du budget mensuel fixé pour le mois concerné.

    * Spécifiez un mode de paiement pour votre campagne de publicité en cliquant sur **Ajouter un mode de paiement**, puis renseignez les détails de votre compte. Si vous avez déjà fourni un instrument de paiement, vous pouvez sélectionner l’option **Choose a different payment method** si vous devez mettre à jour cette information. Le pays/la région de l’adresse de facturation de votre moyen de paiement doit correspondre au pays ou à la région associés à votre compte de développeur.

    * Si vous avez reçu un coupon d’un représentant Microsoft pour payer une campagne de publicité, cliquez sur **Utiliser un coupon**, entrez le code du coupon, puis cliquez sur **Appliquer** pour appliquer le coupon à la campagne.

    Lorsque vous avez terminé, cliquez sur **Enregistrer et Suivant** afin de poursuivre à l’étape **Audience**. Cette étape n’est pas disponible pour les campagnes ad maison, car elles s’exécutent uniquement dans vos propres applications.

9.  Dans la page **Audience**, nous affichons les paramètres d’audience que nous recommandons pour votre campagne. Si vous le souhaitez, vous pouvez ajuster ces informations :
    * **Pays/régions** : choisissez jusqu’à 5 pays ou régions dans lesquels vous souhaitez faire apparaître votre publicité. Pour obtenir la liste des régions ou pays pris en charge, consultez l’article [Questions courantes sur les campagnes de publicité](common-questions.md#where-will-my-ad-appear).

    * **Appareils** : choisissez les types d’appareils sur lesquels vous voulez que ces publicités apparaissent. Seuls les types d’appareils pris en charge par votre application sont affichés.

    * **Surface** : choisissez **Universelle** si vous voulez que votre publicité s’affiche dans les applications, ainsi que dans MSN.com, Outlook.com, Skype et les autres propriétés Premium de Microsoft. Si vous préférez que votre publicité apparaisse uniquement dans les applications, choisissez **Application**.

    * **Système d’exploitation** : choisissez le ou les systèmes d’exploitation sur lesquels votre publicité doit apparaître. Seuls les systèmes d’exploitation pris en charge par votre application sont affichés.

    * **Sexe** : déterminez si vous souhaitez restreindre l’audience de votre publicité à un sexe spécifique.

    * **Tranche d’âge** : sélectionnez la ou les tranches d’âge de l’audience ciblée.

    Cette section affiche également un graphique **Portée estimée**. Ce graphique présente l’audience que vous pouvez envisager d’atteindre avec les choix de ciblage en cours. Il s’agit d’un pourcentage de l’ensemble des utilisateurs d’applications Windows affichant des publicités sur les marchés sélectionnés.

10.  Si vous avez choisi **Augmenter l’intérêt pour votre application** comme objectif de campagne, vous pouvez sélectionner l’un des segments de clients à cibler. Les publicités créées à l’aide de cette campagne sont vues seulement par les clients qui sont inclus dans le segment. Vous ne pouvez sélectionner qu’un segment par campagne de publicité. Pour plus d’informations sur les segments, consultez [Créer des segments de clients](create-customer-segments.md). Lorsque vous avez terminé, cliquez sur **Enregistrer et Suivant** afin de poursuivre à l’étape **Conception d’annonces**. Cette étape n’est pas disponible pour les campagnes ad maison, car elles s’exécutent uniquement dans vos propres applications.

11.  Dans la page **Conception d’annonces**, choisissez l’une des options suivantes :
    * **Auto-généré**. Il s’agit de l’option par défaut, qui vous permet de créer une publicité à partir de nos modèles par défaut. Vous pouvez effectuer des sélections pour personnaliser le contenu de votre application, et nous afficherons alors un aperçu de l’aspect de votre publicité en fonction de vos choix (automatiquement mis à jour dès que vous effectuez des sélections).
        * Dans la liste déroulante **Langue**, sélectionnez la langue de votre publicité. Le texte du badge Microsoft Store s’affichera dans la langue que vous avez sélectionnée.
        * Pour ajouter une ligne de texte supplémentaire à votre publicité, entrez ce texte dans le champ **Slogan personnalisé**.
            > [!NOTE]
            > Le texte que vous entrez ici doit être localisé dans la langue sélectionnée. Le slogan personnalisé sera rejeté si le texte n’est pas conforme aux [politiques Bing Ads](https://advertise.bingads.microsoft.com/bing-ads-policies). Lisez cette page pour obtenir des conseils sur le style et les contenus non autorisés.
        * Pour personnaliser l’annonce davantage, développez **Personnaliser la conception d’annonce/Voir toutes les tailles d’annonces** et choisissez l’une des options suivantes :
            * **Couleur d’arrière-plan**. Choisissez parmi les options disponibles.
            * **Images**. Choisissez l’une des images disponibles (issues de la description de votre application dans le Windows Store).
            * **Afficher la notation de mon application**. Activez cette case à cocher si vous souhaitez afficher la notation de l’application.
            * **Indiquer que mon application est gratuite**. Si votre application est gratuite sur l’ensemble des marchés sélectionnés, vous avez la possibilité de cocher cette case.
            * **Appel à l’action**. Si vous avez choisi **Augmenter l’intérêt pour votre application** comme objectif de campagne, vous pouvez définir le bouton d’appel à l’action de votre annonce sur **Ouvrir**, **Lecture**, **Lire**, **Écouter** ou **Acheter**.  

    * **Personnalisé**. Choisissez cette option pour utiliser vos propres conceptions d’annonces. Notez que si vous avez précédemment sélectionné un segment de client, vous devez utiliser des éléments créatifs personnalisés. Vous pouvez charger différents fichiers pour chacune des tailles d’annonce disponibles. Les fichiers doivent répondre aux exigences et directives suivantes :
        * Chaque fichier doit être un .png ou .jpg inférieur ou égal à 40 Ko.
        * Vos conceptions d’annonces doivent respecter les critères spécifiés dans la [Microsoft Creative Acceptance Policy](https://about.ads.microsoft.com/solutions/ad-products/display-advertising/creative-acceptance-policies).
        * Le contenu de vos conceptions d’annonces doit être approprié à l’application dont vous faites la promotion. Les conceptions d’annonces qui ne sont pas liées à l’application ne seront pas distribuées aux publicités au sein des autres applications.
        * Tout le contenu de vos conceptions d’annonces doit être clairement lisible. Par exemple, le contenu ne doit pas être flou, pixelisé ou déformé.

12.  Si vous avez un [compte premium](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign), vous pouvez utiliser la zone **URL de destination** pour contrôler ce qui se passe quand un client clique sur votre annonce.
    * Si vous laissez la zone vide, quand un client clique sur votre annonce, la liste de vos applications dans le Windows Store s’affiche.
    * Si vous utilisez ajuster, Kochava, régler ou Vungle pour mesurer l’analyse d’installation pour votre application, entrez votre URL de suivi des installations. Lorsque vous enregistrez la campagne, l’URL de suivi est validée pour garantir sa résolution dans la page de description de votre application dans le Microsoft Store. Pour plus d’informations sur le suivi des installations avec ces services, consultez la documentation sur l' [ajustement](https://docs.adjust.com/en/), la [Kochava](https://support.kochava.com/), le [réglage](https://help.tune.com/hasoffers/)et la [Vungle](https://support.vungle.com/hc/en-us) .
    * Si vous avez choisi **Augmenter l’intérêt pour votre application** comme objectif de campagne, vous pouvez spécifier un [URI de lien ciblé](../launch-resume/handle-uri-activation.md) pour rediriger les clients du segment sélectionné vers une page spécifique de votre application.
    * Si vous spécifiez une destination qui n’est pas la page de description de votre application ni une page interne à votre application, votre campagne est automatiquement suspendue.

13.  Enfin, cliquez sur **Révision** pour vérifier les paramètres de votre campagne de publicité et pour vérifier son budget et les informations de paiement s’il s’agit d’une campagne de publicité payante. Cliquez sur **Confirmer**. Dans les heures qui suivent, vos annonces s’afficheront progressivement sur les appareils.

## <a name="review-ad-campaign-performance"></a>Vérifier les performances des campagnes de publicité

Pour vérifier l’efficacité de vos campagnes, revenez à la page **Campagnes de publicité**. Sélectionnez **Filtres de section** pour limiter les éléments inclus dans le rapport par **Date**, **Objectif de campagne**, **Nom d’application**, **Type de campagne** ou **État**. Outre l’accès à des informations sur les **impressions**, les **clics**, les **conversions** et les **dépenses** de votre campagne, vous pouvez utiliser le rapport pour **interrompre** ou **reprendre** une campagne. Pour plus d’informations, consultez l’article [Rapport de campagne de publicité](promote-your-app-report.md).

Pour modifier une campagne, sélectionnez son nom dans la liste.
