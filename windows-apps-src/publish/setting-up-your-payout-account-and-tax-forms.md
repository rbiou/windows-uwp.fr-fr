---
Description: Pour recevoir de l’argent des ventes de l’application dans le Microsoft Store, vous devez configurer votre compte de paiement et remplir les formulaires fiscaux nécessaires.
title: Configurer votre compte de revenu et vos déclarations fiscales
ms.assetid: 690A2EBC-11B1-4547-B422-54F15A6C26A7
ms.date: 1/17/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ca046122a841c727b6f9ef46a868b2c9ba0be2e9
ms.sourcegitcommit: 74627903a18b14c1af68269b0a8c85840caa1898
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80759429"
---
# <a name="set-up-your-payout-account-and-tax-forms"></a>Configurer votre compte de revenu et vos déclarations fiscales

> [!NOTE]
> Si vous recherchez de l’aide sur les versements, notamment la configuration des comptes de paiement, les versements manquants, l’ajout de paiements en attente ou tout autre point, contactez le support [ici](https://partner.microsoft.com/support).

Pour recevoir de l’argent des ventes de l’application dans le Microsoft Store, vous devez configurer votre compte de paiement et remplir les formulaires fiscaux nécessaires dans l' [espace partenaires](https://partner.microsoft.com/dashboard).

Si vous envisagez de référencer uniquement des applications gratuites (et que vous ne voulez pas proposer d’achats in-app ou utiliser Microsoft Advertising), vous n’avez pas besoin de configurer de compte de revenu ni de remplir de déclaration fiscale. Si vous changez d’avis par la suite et que vous décidez de vendre des applications (ou des modules complémentaires), vous pouvez configurer votre compte de paiement et remplir les formulaires fiscaux à ce moment-là. Vous ne pourrez pas soumettre d’applications ou d’extensions payantes avant d’avoir créé votre compte de paiement et votre profil fiscal.

> [!NOTE]
> Dans [certains marchés](account-types-locations-and-fees.md#developer-account-and-app-submission-markets), les développeurs peuvent uniquement soumettre des applications gratuites. Si votre compte est enregistré sur l’un de ces marchés, vous n’aurez pas la possibilité de configurer un compte de paiement.

Une fois que vous avez [configuré votre compte de développeur](opening-a-developer-account.md), vous devez effectuer deux opérations pour pouvoir vendre des applications (ou des modules complémentaires) dans le Microsoft Store :

- [Remplissez vos formulaires fiscaux](#tax-forms)
- [Configurer votre compte de paiement](#payout-account)

> [!NOTE]
> Pour plus d’informations sur le mode et l’échéance des paiements de l’argent que vos applications vous ont rapporté, consultez l’article [Rémunération](getting-paid-apps.md).

## <a name="tax-forms"></a>Déclarations fiscales

### <a name="filling-out-your-tax-forms"></a>Remplissage de vos formulaires fiscaux

Tout d’abord, vous devez créer un profil fiscal et l’affecter aux programmes auxquels vous participez. Vous pouvez créer votre *Profil fiscal* pour le Microsoft Store en procédant comme suit :

- Spécifiez votre pays de résidence et votre nationalité.
- Remplissez les déclarations fiscales appropriées.

Vous pouvez compléter et soumettre vos formulaires fiscaux par voie électronique dans l’espace partenaires. dans la plupart des cas, vous n’avez pas besoin d’imprimer et de envoyer des formulaires.

> [!IMPORTANT]
> L’imposition varie selon les pays et les régions. Le montant exact des taxes dont vous devez vous affranchir varie selon les pays et les régions où vous vendez vos applications. Pour savoir dans quels pays Microsoft vous dispense des taxes d’utilisation et sur les ventes, voir le [Contrat du développeur de l’application](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) Dans d’autres pays, selon l’endroit où vous êtes inscrit, vous devrez peut-être verser directement les taxes d’utilisation et les taxes sur les ventes de vos applications à l’administration fiscale locale. Notez également que le produit des ventes d’applications que vous recevez peut être considéré comme un revenu imposable. Nous vous encourageons vivement à contacter l’autorité appropriée pour votre pays ou région qui peut vous aider à déterminer les bonnes informations fiscales pour vos activités de développeur Microsoft Store.

1. Dans l' [espace partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône **paramètres de compte** dans le coin supérieur droit, puis sélectionnez Paramètres du **développeur**.
2. Dans le menu de navigation de gauche, sélectionnez **décaissement et Tax**, puis sélectionnez le **paiement et les affectations de taxes**.

    ![Attribution du profil de paiement et de taxe](images/payout-tax-profile-assignment.png)

3. Sélectionnez la combinaison programme et nom du vendeur pour laquelle vous souhaitez configurer des informations fiscales.

    ![Paiement-sélectionner un ID de vendeur](images/payout-select-seller-id.png)

4. Si vous souhaitez utiliser un profil fiscal existant, sélectionnez-le dans la liste déroulante. Dans le cas contraire, sélectionnez **créer un nouveau profil** , puis appuyez sur **Envoyer**. Vous êtes dirigé vers la page profils fiscaux.
5. Cliquez sur le bouton **modifier** pour modifier vos informations fiscales.
6. Sélectionnez la case d’option appropriée, puis sélectionnez votre pays si vous y êtes invité. Cette étape détermine l’entité métier Microsoft qui sera utilisée pour effectuer des versements sur votre compte.

    ![Paiement-sélectionner le pays fiscal](images/payout-select-tax-country.png)

7. En fonction de vos sélections à l’étape 6, vous serez invité à fournir les informations fiscales requises pour votre pays.

> [!NOTE]
> Quel que soit votre pays de résidence ou de citoyenneté, vous devez remplir États-Unis formulaires fiscaux pour vendre des applications ou des modules complémentaires par le biais du Microsoft Store. Les développeurs répondant à certains critères de résidence aux États-Unis doivent remplir le formulaire W-9 du fisc américain (IRS). Les autres développeurs résidant en dehors des États-Unis doivent compléter le formulaire W-8 de l'IRS. Vous pouvez remplir ces formulaires en ligne lors de la création de votre profil fiscal.

### <a name="withholding-rates"></a>Taux de retenue

Les informations que vous indiquez dans vos déclarations fiscales déterminent le taux de retenue fiscale appropriée. Le taux de retenue s’applique uniquement aux ventes que vous réalisez aux États-Unis ; les ventes réalisées dans les autres pays ne sont pas assujetties à une retenue. Les taux de retenue peuvent varier, mais pour la plupart des développeurs s’inscrivant dans un autre pays que les États-Unis, le taux par défaut est de 30 %. Ce taux peut être revu à la baisse si votre pays a signé une convention fiscale avec les États-Unis.

### <a name="tax-treaty-benefits"></a>Avantages en vertu d’une convention fiscale

Si vous résidez en dehors des États-Unis, vous pouvez tirer parti d’avantages découlant d’une convention fiscale. Ces avantages varient d’un pays à l’autres, et peuvent vous permettre de réduire le montant des taxes que le Microsoft Store retenue. Pour revendiquer des avantages en vertu d’une convention fiscale, remplissez la deuxième partie du formulaire W-8BEN. Nous vous recommandons de contacter les autorités appropriées dans votre pays ou région pour déterminer si ces avantages vous concernent.

> [!NOTE]
> Il n'est pas nécessaire de fournir un numéro d'identification du contribuable ou ITIN (États-Unis) pour recevoir des paiements de Microsoft ou pour revendiquer des avantages en vertu d'une convention fiscale.

## <a name="payout-account"></a>Compte de revenu

Un compte de revenu est le compte bancaire vers lequel nous transférons l'argent de vos ventes. Vous pouvez afficher tous les comptes de paiement que vous avez entrés dans la page de profil.

> [!NOTE]
> Dans certains marchés, PayPal peut être utilisé pour votre compte de paiement. Consultez les [seuils de paiement, les méthodes et les délais](payment-thresholds-methods-and-timeframes.md) pour savoir si PayPal est pris en charge pour un marché spécifique et lisez les informations relatives à [PayPal](#paypal-info) ci-dessous pour plus de détails.

### <a name="create-a-payment-profile"></a>Créer un profil de paiement

1. Dans l' [espace partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône d’engrenage **paramètres** dans le coin supérieur droit, puis sélectionnez **paramètres du développeur**.
2. Sous l’en-tête *paiement et taxes* , sélectionnez **paiement et affectation du profil fiscal**.

    > [!NOTE]
    > Étant donné qu’il s’agit d’informations sensibles, vous serez peut-être invité à vous reconnecter.

3. Sélectionnez le mode de paiement que vous souhaitez configurer.

    ![Sélection du type de compte de paiement](images/payout-account-type-selection.png)

4. Sélectionnez un profil de paiement existant ou cliquez sur **créer un nouveau** profil de paiement pour créer un nouveau profil pour le mode de paiement choisi.

> [!NOTE]
> Si, pour une raison quelconque, votre compte n’est pas prêt à recevoir des fonds de Microsoft, vous pouvez activer la case à cocher **conserver mon paiement** . Vous continuerez à gagner vos ventes, mais les paiements ne seront pas distribués tant que vous n’aurez pas désactivé **conserver mon paiement.**

### <a name="create-a-bank-based-payment-profile"></a>Créer un profil de paiement basé sur la Banque

Si vous avez choisi d’utiliser un compte bancaire pour recevoir des paiements, vous devez effectuer la procédure suivante pour configurer votre compte bancaire.

1. Sur la page *Profil bancaire* , fournissez les informations requises sur votre banque.
2. Fournissez les détails de votre compte bancaire.

    > [!NOTE]
    > Les champs dans lesquels vous entrez des informations sur votre compte acceptent uniquement les caractères alphanumériques.

    ![Informations bancaires sur le paiement](images/payout-bank-info.png)

3. Fournissez les détails du bénéficiaire.
4. De retour sur la page *attribution de profil* , sélectionnez la devise que vous souhaitez que nous utilisons pour émettre vos paiements.

    > [!WARNING]
    > Assurez-vous que votre banque accepte la devise de paiement que vous sélectionnez.

5. Vous devrez sélectionner un profil de paiement pour chaque programme auquel vous participez, même si vous pouvez utiliser le même profil pour plusieurs programmes.

    ![Profil bancaire d’utilisation de paiement](images/payout-use-bank-profile.png)

6. Cliquez sur Envoyer pour enregistrer vos modifications.

> [!NOTE]
> Microsoft peut prendre jusqu’à 48 heures pour valider les informations de votre profil. Lorsque ce processus est terminé, l’état de la *vérification* s’affiche à l' **achèvement**

Pour garantir le succès du paiement, notez les points suivants :

- Le **nom du titulaire du compte** saisi pour votre compte de paiement dans l’espace partenaires doit correspondre exactement au nom associé à votre compte bancaire. Par exemple, si le nom de votre compte bancaire comporte un deuxième prénom, saisissez un deuxième prénom dans le champ **Nom du titulaire du compte**.
- Les paiements sont transférés directement de Microsoft vers votre compte bancaire en dollars USD.
- Les informations bancaires saisies dans l’espace partenaires, en caractères latins, sont traduites en caractères cyrilliques.

### <a name="editing-existing-payment-profiles"></a>Modification des profils de paiement existants

Vous pouvez modifier les profils de paiement existants si vous devez apporter des modifications ou corriger des informations incorrectes.

1. Dans l' [espace partenaires](https://partner.microsoft.com/dashboard), sélectionnez l’icône d’engrenage **paramètres** dans le coin supérieur droit, puis sélectionnez **paramètres du développeur**.
2. Sous l’en-tête *paiement et taxes* , sélectionnez **paiement et profils fiscaux**.
3. Vos profils de paiement sont listés avec leur état. Recherchez le profil que vous souhaitez modifier, puis cliquez sur **modifier** à l’extrême droite

> [!IMPORTANT]
> En cas de modification de votre compte de paiement, vos paiements peuvent être retardés d’un cycle de paiement au maximum. Ce retard s’explique par le fait que nous devons vérifier la modification apportée au compte, et ce, selon le même processus que celui utilisé lors de la configuration initiale du compte de revenu. Une fois votre compte vérifié, vous serez toujours rémunéré, mais nous ajouterons les paiements du cycle de paiement actuel au prochain cycle de paiement. Pour plus d'informations, voir l'article [Rémunération](getting-paid-apps.md).

### <a name="paypal-info"></a>Informations PayPal

Selon votre pays ou votre région, vous avez la possibilité de créer un compte de paiement en saisissant vos informations PayPal. Avant de choisir PayPal comme option de compte de paiement, vérifiez les points suivants :

- Vérifiez les [seuils de paiement, les méthodes et les délais](payment-thresholds-methods-and-timeframes.md) pour confirmer si PayPal est un mode de paiement pris en charge dans votre pays ou région.
- Consultez les FAQ ci-après. En fonction de votre situation, il se peut que PayPal ne soit pas l'option idéale pour votre compte de paiement et qu'un compte bancaire soit mieux adapté.

Questions courantes concernant l'utilisation de PayPal comme mode de paiement :

- **Quels sont les paramètres PayPal dont j’ai besoin pour recevoir des paiements ?** Vous devez vous assurer que votre compte PayPal ne bloque pas les paiements par eCheck. Ce paramètre peut être configuré sur la page des préférences de réception des paiements de votre compte PayPal. Pour plus d'informations, voir la [page de configuration du compte PayPal](https://developer.paypal.com/webapps/developer/docs/classic/admin/setup-account/).
- **Mon pays/région est-il pris en charge ?** Consultez [les seuils de paiement, les méthodes et les délais](payment-thresholds-methods-and-timeframes.md) pour savoir où Paypal est un mode de paiement pris en charge.
- **Mon compte PayPal doit-il être inscrit dans le même pays ou la même région que mon compte espace partenaires ?** Non. Lorsque vous créez un compte PayPal, vous pouvez accepter la configuration par défaut. Vous ne devriez rencontrer aucun problème d’incompatibilité entre les différents pays ou régions et entre les différentes devises, à moins que vous n’ayez bloqué le paiement dans certaines devises. Ce paramètre peut être configuré sur la page des préférences de réception des paiements de votre compte PayPal.
- **Dois-je accepter les paiements PayPal manuellement ?** Non. Les comptes PayPal demandent par défaut de valider manuellement chaque paiement, ce qui signifie que si vous n’acceptez pas le paiement dans un délai de 30 jours, celui-ci est rejeté. Vous pouvez modifier ce paramètre en désactivant l'option « Me demander » sur la page des paramètres supplémentaires de votre compte PayPal.
- **Quelles sont les devises prises en charge par PayPal ?** Veuillez consulter la [page de support de PayPal](https://developer.paypal.com/docs/classic/api/currency-codes/#paypal) pour obtenir la liste actuelle

### <a name="specific-requirements-for-certain-countriesregions"></a>Exigences spécifiques pour certains pays/régions

Certains pays et régions appliquent des exigences supplémentaires pour les comptes de revenu. Si vous résidez au Pakistan, en Russie ou en Ukraine, notez les obligations suivantes.

#### <a name="pakistan"></a>Pakistan

Le remplissage d’un formulaire R constitue une exigence réglementaire du secteur bancaire au Pakistan. Ce formulaire sert à indiquer la finalité et le motif d’un encaissement de fonds en provenance de l’étranger. Par conséquent, chaque fois que vous pouvez bénéficier d’un revenu mensuel de la part de Microsoft, vous devez soumettre un formulaire R à votre banque pour que ce paiement soit autorisé sur votre compte. Pour connaître la procédure d’obtention d’un formulaire R, contactez votre agence bancaire locale.

Vous devrez soumettre un formulaire R à votre banque pour chacun des mois où vous pouvez bénéficier d’un revenu. Par exemple, si vous prévoyez de recevoir un paiement tous les mois de l'année, vous devrez soumettre un formulaire R à 12 reprises (un par mois).

Une fois que le paiement a été soumis à votre banque, vous disposez de 30 jours pour envoyer un formulaire R. Passé ce délai, les fonds seront retournés à Microsoft.

#### <a name="russia"></a>Russie

Si vous êtes un développeur vivant en Russie, vous pouvez avoir besoin de fournir des documents à votre banque pour que celle-ci puisse déposer des fonds sur votre compte. Si vous pouvez bénéficier d’une rémunération, nous vous fournirons les documents suivants dans un e-mail :

1. Acceptance Certificate (AC) - contient le montant du paiement transféré sur votre compte.
2. App Developer Agreement (ADA) - copie signée de l’accord de développeur qui doit être contresignée.

Pour garantir le succès du paiement, notez les points suivants :

- Le **nom du titulaire du compte** saisi pour votre compte de paiement dans l’espace partenaires doit correspondre exactement au nom associé à votre compte bancaire. Par exemple, si le nom de votre compte bancaire comporte un deuxième prénom, saisissez un deuxième prénom dans le champ **Nom du titulaire du compte**.
- Les paiements sont transférés directement de Microsoft à votre compte bancaire en roubles (RUB).
- Les informations bancaires saisies dans l’espace partenaires, en caractères latins, sont traduites en caractères cyrilliques.
- Les paiements doivent être effectués sur un compte bancaire et non sur une carte bancaire.

#### <a name="ukraine"></a>Ukraine

Si vous êtes un développeur vivant en Ukraine, vous pouvez avoir besoin de fournir des documents à votre banque pour que celle-ci puisse déposer des fonds sur votre compte. Si vous pouvez bénéficier d’une rémunération, nous vous fournirons les documents suivants dans un e-mail :

1. Acceptance Certificate (AC) - contient le montant du paiement transféré sur votre compte.
2. App Developer Agreement (ADA) - copie signée de l’accord de développeur qui doit être contresignée.
3. Amendment Agreement (AA) – ce document est utilisable par votre banque pour faciliter l’identification de vos fonds de paiement.

Microsoft fournit ces trois documents lors de la première tentative de paiement. Pour les paiements ultérieurs, vous ne recevrez plus que le document AC. Conservez les documents ADA et AA au cas où vous en auriez besoin pour recevoir de futurs paiements de votre banque.

### <a name="create-a-paypal-payment-profile"></a>Créer un profil de paiement PayPal

Si vous avez choisi d’utiliser un compte bancaire pour recevoir des paiements, vous devez effectuer la procédure suivante pour configurer votre compte bancaire.

1. Sur la page *PayPal* , fournissez les informations requises sur votre compte PayPal.
2. Fournissez les détails de votre compte PayPal.

    > [!NOTE]
    > Les champs dans lesquels vous entrez des informations sur votre compte acceptent uniquement les caractères alphanumériques.

    ![Paiement des infos PayPal](images/payout-paypal-info.png)

3. Fournissez les détails du bénéficiaire.
4. De retour sur la page *attribution de profil* , sélectionnez la devise que vous souhaitez que nous utilisons pour émettre vos paiements.
5. Vous devrez sélectionner un profil de paiement pour chaque programme auquel vous participez, même si vous pouvez utiliser le même profil pour plusieurs programmes.
6. Cliquez sur Envoyer pour enregistrer vos modifications.
