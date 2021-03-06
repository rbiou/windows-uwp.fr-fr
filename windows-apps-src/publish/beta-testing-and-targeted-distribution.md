---
Description: Partenaires vous propose plusieurs options pour permettre aux testeurs de tester votre application avant de vous proposer l’offre au public.
title: Tests bêta et distribution ciblée
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, test bêta, distribution limitée, bêta, test, testeurs
ms.localizationpriority: medium
ms.openlocfilehash: 6cf7fb20129c0b616fcdb537ff8e612aec9b94a4
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468915"
---
# <a name="beta-testing-and-targeted-distribution"></a>Tests bêta et distribution ciblée

Quelle que soit le soin avec lequel vous testez votre application, rien de vaut un test dans des conditions réelles, avec d’autres utilisateurs. Les testeurs peuvent épingler des problèmes qui vous auraient échappés, comme des fautes d’orthographe, un flux d’application peu clair ou des erreurs pouvant entraîner un blocage de l’application. Vous pouvez ensuite résoudre ces problèmes avant de mettre à la disposition du public un produit finalement irréprochable. 

Partenaires vous propose plusieurs options pour permettre aux testeurs de tester votre application avant de vous proposer l’offre au public.

Quelle que soit la méthode choisie, voici quelques éléments à garder à l’esprit au moment de réaliser des tests bêta sur votre application.

- Dès qu’un testeur a téléchargé l’application, vous ne pouvez plus révoquer l’accès à cette dernière. Après avoir téléchargé l’application, les testeurs peuvent l’utiliser et recevoir les mises à jour éventuelles.
- Vous devez déterminer le mode de collecte de leurs commentaires. Envisagez de fournir un lien qui permet à vos testeurs de communiquer facilement leurs commentaires par e-mail (ou par le biais de l’application [Hub de commentaires](../monetize/launch-feedback-hub-from-your-app.md), si la confidentialité n’est pas une préoccupation. 
- Vous pouvez consulter des [rapports d’analyse](analytics.md) concernant votre application, notamment des rapports d’utilisation et d’intégrité, ainsi que des évaluations ou avis laissés par vos testeurs.
- Lorsque vous distribuez l’application aux testeurs, vous pouvez y inclure des modules complémentaires. Dans la mesure où vous ne voulez probablement pas les facturer pour une extension, vous pouvez [générer des codes promotionnels](generate-promotional-codes.md) et les distribuer à vos testeurs pour leur permettre d’obtenir l’extension gratuitement, ou vous pouvez définir le prix de l’extension sur **Gratuit** pendant le test (ensuite, avant de rendre l’application disponible pour les autres clients, créez une nouvelle soumission pour l’extension et modifiez son prix). Notez que chaque extension peut uniquement être achetée une fois par compte Microsoft. De ce fait, le même testeur ne pourra tester le processus d’acquisition d'extension qu’une seule fois. 
- Vous pouvez donner à vos testeurs une version mise à jour de votre application à tout moment, en créant une nouvelle soumission avec de nouveaux packages. Vos testeurs obtiendront la mise à jour une fois cette dernière traitée par le processus de certification, de la même façon qu’ils ont reçu le package d’origine. En revanche, personne d’autre ne pourra l’obtenir (sauf si vous apportez des modifications supplémentaires, comme permuter une application de l’option **Public privé** vers **Public non privé**, ou changer l’appartenance aux groupes pouvant l’obtenir).

## <a name="private-audience"></a>Public privé

Si vous souhaitez permettre aux testeurs d’utiliser votre application avant qu’elle ne soit disponible pour les autres utilisateurs, tout en vous assurant que personne d’autre ne peut voir sa description, utilisez l’option **Public privé** sous [Visibilité](choose-visibility-options.md) (sur la page **Tarification et disponibilité** de votre soumission). Il s’agit de la seule méthode vous permettant de distribuer votre application aux testeurs tout en empêchant complètement toute autre utilisateur de voir la description du Store pour l’application, même s’il a été en mesure d’entrer son lien direct. 

Le **audience privé** option peut uniquement être utilisée lorsque vous n’avez pas déjà publié votre application en public. Vous pouvez utiliser cette option avec les applications ciblant n’importe quelle version du système d’exploitation, mais les testeurs doivent exécuter Windows 10, version 1607 ou version ultérieure (y compris Xbox One) et doivent être signés avec le compte Microsoft associé à l’adresse de messagerie que vous fournissez.

Pour plus d’informations, voir [Public privé](choose-visibility-options.md#audience).


## <a name="package-flights"></a>Versions d’évaluation de package

Si vous avez déjà publié votre application, vous pouvez créer des versions d’évaluation de package pour distribuer un ensemble différent de packages aux personnes que vous spécifiez. Vous pouvez même créer plusieurs versions d’évaluation de package pour une même application et utiliser ces versions avec différents groupes d’utilisateurs. Cela constitue un excellent moyen d’expérimenter différents packages simultanément, et vous pouvez retirer des packages d’une version d’évaluation pour les intégrer à votre soumission sans version d’évaluation si vous décidez qu’ils peuvent être rendus publics.

Les versions d’évaluation des packages peuvent être utilisées avec des applications destinées à n’importe quelle version du système d’exploitation. Toutefois, vos testeurs peuvent uniquement obtenir l’application s’ils exécutent Windows.Desktop build 10586 ou ultérieure, Windows.Mobile build 10586.63 ou ultérieure, ou Xbox One.

Pour en savoir plus, consultez [Versions d’évaluation de package](package-flights.md).


<span id="hide" />

## <a name="hiding-the-app-in-the-store-and-using-promotional-codes"></a>Masquage de l’application dans Windows Store et utilisation des codes promotionnels

Cette option offre une autre façon de limiter la distribution d’une application à uniquement un certain groupe de testeurs, tout en empêchant quiconque de découvrir votre application dans le Store (ou l’acquisition sans un code promotionnel). Toutefois, contrairement à l’option Public privé, n’importe qui peut voir la description de votre application s’il a le lien direct. Si la confidentialité est essentielle pour votre soumission, nous vous recommandons de choisir plutôt une publication pour un public privé.

Le masquage de l’application et l’utilisation de codes promotionnels peuvent être utilisés avec des applications destinées à n’importe quelle version du système d’exploitation, mais vos testeurs ne pourront obtenir l’application que s’ils exécutent Windows 10.

Pour utiliser cette option :

- Dans la section **Visibilité** de la page **Tarification et disponibilité**, sous [Détectabilité](choose-visibility-options.md#discoverability), sélectionnez **Rendre ce produit disponible mais non détectable dans le Store**. Choisissez l’option pour **arrêter acquisition : Tous les clients avec un lien direct peuvent voir la liste Store du produit, mais ils peuvent uniquement télécharger si le propriétaire du produit avant, ou un code promotionnel et utilisez un appareil Windows 10**. 
- Une fois l’application certifiée, vous devez [générer des codes promotionnels](generate-promotional-codes.md) pour celle-ci et les distribuer à vos testeurs. Vous pouvez générer des codes pour permettre jusqu’à 1600 échanges par application sur une période de six mois. Ces codes fournissent aux testeurs un lien direct vers la description de l’application et leur permettent de télécharger celle-ci gratuitement, même si vous avez défini un prix lors de la création de votre soumission.
- Lorsque vous êtes prêt à rendre l’application accessible au public, créez une soumission et redéfinissez l’option **Visibilité** sur **Rendre ce produit disponible et détectable dans le Store** (en plus d’autres modifications que vous souhaitez apporter).


## <a name="targeted-distribution-with-a-link-to-your-apps-listing"></a>Distribution ciblée via un lien d’accès à la description de votre application

Contrairement aux options décrites ci-dessus, cette option fonctionne pour les clients sur Windows Phone 8.1, ainsi que Windows 10 (mais non sur Windows 8.x). Nul ne peut trouver l’application en recherchant ou en naviguant dans le Store, mais quiconque disposant du lien direct vers sa description du Store peut la télécharger sur un appareil exécutant Windows Phone 8.1 ou version antérieure, ou sur Windows 10. Gardez à l’esprit que, pour que vos testeurs puissent télécharger l’application librement, vous devez définir son prix sur **Gratuit**.

Pour utiliser cette option :
- Dans la section **Visibilité** de la page **Tarification et disponibilité**, sous [Détectabilité](choose-visibility-options.md#discoverability), sélectionnez **Rendre ce produit disponible mais non détectable dans le Store**. Choisissez l’option pour **lien Direct uniquement : Tous les clients avec un lien direct vers la liste du produit peuvent télécharger, sauf sur Windows 8.x.** .
- Après la publication de votre produit, distribuez le lien (l’**URL** sur la [page Identité des applications](view-app-identity-details.md)) à vos testeurs afin qu’ils puissent l’essayer.
- Lorsque vous êtes prêt à rendre l’application accessible au public, créez une soumission et redéfinissez l’option **Visibilité** sur **Rendre ce produit disponible et détectable dans le Store** (en plus d’autres modifications que vous souhaitez apporter).

> [!IMPORTANT]
> À compter du 31 octobre 2018, produits nouvellement créé ne peut pas inclure les packages ciblant Windows Phone 8.x ou une version antérieure. Pour plus d’informations, consultez ce [billet de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="targeted-distribution-to-windows-phone-customers-with-specified-email-addresses"></a>Distribution ciblée à des clients Windows Phone disposant d’adresses de messagerie spécifiées

> [!IMPORTANT]
> Cette option n’est pas disponible pour les nouvelles soumissions. Si vous aviez précédemment sélectionné cette option pour une application ciblant Windows Phone 8.1 ou version antérieure, vous pourrez continuer à l’utiliser pour cette application. Vous pouvez apporter des modifications à la liste de testeurs (jusqu'à 10 000) en créant une nouvelle soumission. 

Avec cette option, les personnes avec les adresses de messagerie que vous avez spécifiées peuvent télécharger l’application (sur un appareil exécutant Windows Phone 8.1 ou version antérieure uniquement) en utilisant le lien direct vers sa description. Aucun autre client ne sera en mesure de télécharger l’application, même s’il a le lien, et personne d’autre ne pourra trouver l’application dans le Store par recherche ou par navigation. Afin que les testeurs puissent télécharger l’application, vous devez leur donner son lien (l’**URL** sur la [page Identité des applications](view-app-identity-details.md)), et il doivent être connectés avec un compte Microsoft associé à une adresse de messagerie que vous avez fournie. Vous pouvez également rendre l’application disponible pour les testeurs sur les appareils Windows 10 par [générer des codes promotionnels](generate-promotional-codes.md); toute personne disposant d’un des codes de promotion de votre application permettre le télécharger sur un appareil Windows 10, même si vous n’avez pas entré leur courrier électronique ici.
