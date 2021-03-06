---
Description: Le rapport d’analytique de Xbox dans Partner Center vous indique les statistiques sur la façon dont vos clients sont impliqués grâce aux fonctionnalités Xbox dans votre produit.
title: Rapport d’analyse Xbox
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp, analyse xbox, analyse xbox live, statistiques xbox
ms.localizationpriority: medium
ms.openlocfilehash: a6dc1650e1220cd808422a2aa705d4cbab9f59fb
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63788173"
---
# <a name="xbox-analytics-report"></a>Rapport d’analyse Xbox

Le **Xbox analytique** signaler dans [partenaires](https://partner.microsoft.com/dashboard) vous affiche les statistiques sur la façon dont vos clients sont attrayantes avec les fonctionnalités de Xbox dans votre jeu. Il fournit également des informations sur l’intégrité du service pour vous aider à corriger les erreurs de client.

> [!IMPORTANT]
> Vous ne verrez ce rapport que si vous publiez un jeu pour Xbox ou un jeu qui utilise les services Xbox Live. Pour ce faire, vous devez passer par le [processus d’approbation de concept](../gaming/concept-approval.md), qui inclut les jeux publiés par [partenaires Microsoft](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview#microsoft-partners) et des jeux soumises via le [ ID@Xbox programme](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview#id). Jeux publiés via le [programme Xbox Live Creators](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) ne sont pas actuellement visible dans ce rapport.

Vous pouvez afficher le **Xbox analytique** rapport dans le menu de navigation de gauche pour votre jeu en développant **analyser** et en sélectionnant **Xbox analytique**.  Vous pouvez afficher ces données dans le centre de partenaires, ou [télécharger le rapport](download-analytic-reports.md) à afficher en mode hors connexion.


## <a name="overview-tab"></a>Onglet Vue d'ensemble

Les sections sur l'onglet **Vue d’ensemble** présentent des informations sur vos lecteurs et comment ils interagissent avec les fonctionnalités Xbox Live.

Pour la plupart de ces statistiques, nous affichons également la **moyenne Xbox** afin que vous puissiez facilement voir comment vos clients interagissent avec la Xbox comparés au client Xbox moyen.

> [!NOTE]
> Ces statistiques sont générées à partir des clients qui sont connectés à Xbox Live, et non pas à partir de tous les clients Xbox.


### <a name="concurrent-usage"></a>Concurrent usage

Cette section montre les données d’utilisation quasiment en temps réel (avec une latence de 5 à 15 minutes) sur le nombre moyen de clients qui jouent à votre jeu chaque minute ou chaque heure. Vous pouvez choisir l’intervalle de temps (à partir de la **dernière heure** jusqu'aux **7 derniers jours**) en sélectionnant l’icône de filtre dans le coin supérieur droit de cette section.


### <a name="gamerscore-distribution"></a>Gamerscore distribution

Cette section présente des informations sur le score de joueur de vos clients. Vous pouvez sélectionner **All games** pour afficher la répartition du score de joueur total pour tous vos clients ou sélectionner **This game** pour afficher la répartition du score de joueur obtenu par le biais de votre jeu uniquement.


### <a name="achievement-unlocks"></a>Achievement unlocks

Cette section présente le nombre total de clients qui ont déverrouillé chaque succès dans l’intervalle de temps spécifié. Vous pouvez choisir l’intervalle de temps (**Dernière heure**, **30 derniers jours** ou **Durée de vie**) en sélectionnant l’icône de filtre dans le coin supérieur droit de cette section.


### <a name="game-statistics"></a>Game statistics

Cette section inclut des onglets que vous pouvez sélectionner pour afficher différentes données pour les clients de votre jeu. Notez que les statistiques de cette section font référence à l’utilisation des fonctionnalités en général et non dans votre produit spécifique.

- L’onglet **Social usage** affiche les données relatives à la façon dont vos clients interagissent socialement.
   - **Game invites** affiche le pourcentage de vos clients qui ont envoyé des invitations (pour n’importe quel jeu).
   - **Chat du groupe** affiche le pourcentage de vos clients qui utilisent la conversation instantanée (pour n’importe quel jeu).
   - **Messages texte** affiche le pourcentage de vos clients qui envoient des messages par le biais de l’interpréteur de commandes Xbox (pour n’importe quel jeu).
- L’onglet **Streaming usage** affiche les pourcentages des clients de votre jeu qui regardent ou diffusent en continu un jeu (pour n’importe quel jeu) sur Twitch et YouTube.
- L’onglet **Game DVR usage** affiche des données relatives à la façon dont vos clients enregistrent et affichent le jeu. Vous pouvez voir les pourcentages des clients qui ont affiché et téléchargé des extraits de jeu et des captures d’écran de jeu (pour n’importe quel jeu).


### <a name="friends-and-followers"></a>Friends and followers

Cette section indique le **nombre moyen d’amis** et le **nombre moyen d’abonnés** pour les clients qui jouent à votre jeu.


### <a name="accessory-usage"></a>Accessory usage

Ce graphique présente les pourcentages des clients de votre jeu qui utilisent des disques durs externes et des manettes sans fil Xbox Elite (sur Xbox).

Ces données ne signifient pas que ces clients ont installé votre produit sur des disques durs externes ou ont utilisé une manette sans fil Xbox Elite en y jouant. Il désigne le nombre de clients de votre produit qui utilisent ces fonctionnalités en général.


### <a name="connection-type"></a>Type de connexion

Ce graphique indique les pourcentages des clients de votre produit qui utilisent des connexions internet **câblées** et **sans fil** (sur Xbox).


## <a name="xbox-live-service-health-tab"></a>Onglet Xbox Live service health

Les sections de l’**onglet Xbox Live service health** vous aident à comprendre l’impact des erreurs de client Xbox Live, y compris la limitation du débit. Vous pouvez également effectuer une exploration par point de terminaison et code d’état afin d’accéder à des informations qui vous aident à résoudre ces problèmes, et rester informé sur la disponibilité du service Xbox Live spécifique aux appels de votre produit.

> [!NOTE]
> Lors de l’examen de ces informations et de la résolution des problèmes, nous vous recommandons de classer par ordre de priorité les limitations du débit, du fait que ces erreurs ont généralement le plus grand impact sur les clients.


### <a name="apply-filters"></a>Appliquer les filtres

Dans la zone supérieure de l’onglet, vous pouvez sélectionner la période sur laquelle portent les données qui vous intéressent. La valeur par défaut est **30D** (30 jours), mais vous pouvez choisir d’afficher les données portant sur une période de **7D** (7 jours) ou une plage de dates personnalisée que vous spécifiez (de moins de 30 jours). Pour une plage de dates personnalisée, notez que tous les graphiques réduiront la plage du graphique au premier et au dernier jour de données fournies dans la plage de dates spécifiée.

Vous pouvez également développer l’option **Filtres** pour filtrer toutes les données de cette page par version de package, type d'appareil et/ou bac à sable.
- **Version du package**: Le filtre par défaut est **toutes les versions**, mais vous pouvez limiter les données de contrôle d’intégrité de service vers une version de package spécifique.
- **Type d’appareil**: Le paramètre par défaut est **tous les appareils**, mais vous pouvez limiter les données d’intégrité du service à un type de périphérique spécifique.
- **Bac à sable**: Le paramètre par défaut est **RETAIL**, mais vous pouvez limiter les données de contrôle d’intégrité de service à un bac à sable spécifique.

Les informations figurant dans tous les graphiques répertoriés ci-après correspondent à la plage de dates et à tous les filtres que vous avez sélectionnés. Certaines sections vous permettent également d’appliquer des filtres supplémentaires.


### <a name="client-errors-by-service"></a>Client errors by service

Le graphique **Client errors by service** indique le nombre d’erreurs de client (4xx) quotidiennes sur chaque service Xbox Live, sur la période sélectionnée.

Vous pouvez également afficher uniquement les erreurs liées à la limitation du débit en sélectionnant **Rate limiting**. Cela affiche le nombre d’erreurs quotidiennes liées à la limitation du débit (429) et à l’exemption de limitation du débit (429E) sur chaque service Xbox Live, sur la période sélectionnée.

> [!NOTE]
> Un code d’état 429E a été en fait retourné avec succès comme un code d'état 200, mais le débit aurait été limité si le service avait été confronté à un volume élevé à ce moment-là, c’est pourquoi nous vous recommandons de le traiter exactement comme s’il était appliqué (429).

Par défaut, ce graphique affiche les six principaux services par nombre d’erreurs. Vous pouvez sélectionner l’icône de filtre dans le coin supérieur droit de cette section pour choisir d’autres services. Vous pouvez afficher les erreurs pour un maximum de six services à la fois.

> [!NOTE]
> La légende affiche uniquement le préfixe distinctif pour chaque service (par exemple, **presence** au lieu de **presence.xboxlive.com**). Vous pouvez trouver l’adresse complète du service dans le tableau **Client errors by endpoint** situé plus bas dans l'onglet **Xbox Live service health**.


### <a name="service-availability"></a>Service availability

Le graphique **Service availability** indique la disponibilité quotidienne sur chaque service Xbox Live, sur la période sélectionnée. Cette valeur est calculée comme suit : *1-(erreurs serveur totales (5xx) / réponses totales)* et est spécifique à votre produit et non pas à Xbox Live dans l’ensemble.

Par défaut, ce graphique affiche les six services qui ont enregistré la disponibilité la plus basse. Vous pouvez sélectionner l’icône de filtre dans le coin supérieur droit de cette section pour choisir d’autres services. Vous pouvez afficher la disponibilité pour un maximum de six services à la fois.

> [!NOTE]
> La légende affiche uniquement le préfixe distinctif pour chaque service (par exemple, **presence** au lieu de **presence.xboxlive.com**). Vous pouvez trouver l’adresse complète du service dans le tableau **Client errors by endpoint** situé plus bas dans l'onglet **Xbox Live service health**.


### <a name="client-errors-by-endpoint"></a>Client errors by endpoint

Le tableau **Client errors by endpoint** indique le nombre d’erreurs de client (4xx) quotidiennes pour chaque service Xbox Live, point de terminaison et code d’état sur la période sélectionnée. Par défaut, le tableau est trié en fonction du nombre total de réponses du service, par ordre décroissant, mais vous pouvez modifier l’ordre de tri en cliquant sur l’un des en-têtes de colonne.

Vous pouvez également afficher uniquement les erreurs liées à la limitation du débit en sélectionnant **Rate limiting**. Cela affiche le nombre d’erreurs quotidiennes liées à la limitation du débit (429) et à l’exemption de limitation du débit (429E) sur chaque service Xbox Live, point de terminaison et code d’état, sur la période sélectionnée.

> [!NOTE]
> Un code d’état 429E a été en fait retourné avec succès comme un code d'état 200, mais le débit aurait été limité si le service avait été confronté à un volume élevé à ce moment-là, c’est pourquoi nous vous recommandons de le traiter exactement comme s’il était appliqué (429).










 

 
