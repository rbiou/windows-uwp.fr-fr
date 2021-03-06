---
Description: De nombreuses entreprises utilisent des pare-feu pour bloquer le trafic indésirable. Ce document décrit comment autoriser le trafic WNS à traverser des pare-feu.
title: Ajout du trafic WNS au pare-feu Allowlist
ms.assetid: 2125B09F-DB90-4515-9AA6-516C7E9ACCCD
template: detail.hbs
ms.date: 05/20/2019
ms.topic: article
keywords: Windows 10, UWP, WNS, service de notification Windows, notification, Windows, pare-feu, dépannage, IP, trafic, entreprise, réseau, IPv4, VIP, nom de domaine complet, adresse IP publique
ms.localizationpriority: medium
ms.openlocfilehash: 9594b90bd3bf9a4db082586a2823f4013340b85a
ms.sourcegitcommit: b5232934702b28102c4014a96b34f2e459c9660d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87928006"
---
# <a name="enterprise-firewall-and-proxy-configurations-to-support-wns-traffic"></a>Pare-feu d’entreprise et configurations de proxy pour prendre en charge le trafic WNS

## <a name="background"></a>Arrière-plan
De nombreuses entreprises utilisent des pare-feu pour bloquer le trafic réseau et les ports indésirables. Malheureusement, cela peut également bloquer des éléments importants tels que les communications du service de notification Windows. Cela signifie que toutes les notifications envoyées via WNS seront supprimées dans certaines configurations réseau. Pour éviter ce risque, les administrateurs réseau peuvent ajouter la liste des noms de domaine complets WNS approuvés ou des adresses IP virtuelles à leur liste d’exemptions pour permettre au trafic WNS de traverser le pare-feu. Vous trouverez ci-dessous des informations supplémentaires sur la façon dont et les éléments à ajouter, ainsi que la prise en charge de différents types de proxy.

## <a name="proxy-support"></a>Prise en charge du proxy

> [!Note]
> Les clients Windows **ne prennent pas** en charge tous les proxies, la connexion à WNS doit être une connexion directe.

**À venir!** Nous étudions activement différentes configurations réseau, proxies et pare-feu. Nous mettrons à jour cette page avec plus de détails sur les scénarios d’entreprise courants et la prise en charge de WNS rapidement.


## <a name="what-information-should-be-added-to-the-allowlist"></a>Les informations qui doivent être ajoutées à allowlist
Vous trouverez ci-dessous une liste contenant les noms de domaine complets, les adresses IP virtuelles et les plages d’adresses IP utilisées par le service de notification Windows. 

> [!IMPORTANT]
> Nous vous suggérons vivement d’autoriser la liste par nom de domaine complet (FQDN), car ceux-ci ne seront pas modifiés. Si vous autorisez la liste par nom de domaine complet, vous n’avez pas besoin d’autoriser également les plages d’adresses IP.

> [!IMPORTANT]
> Les plages d’adresses IP seront modifiées périodiquement. pour cette raison, elles ne sont pas incluses dans cette page. Si vous souhaitez afficher la liste des plages d’adresses IP, vous pouvez télécharger le fichier à partir du centre de téléchargement : [VIP et plages d’adresses IP du service de notification Windows (WNS)](https://www.microsoft.com/download/details.aspx?id=44238). Vérifiez régulièrement pour vous assurer que vous disposez des informations les plus récentes. 


### <a name="fqdns-vips-ips-and-ports"></a>FQDN, VIP, adresses IP et ports
Quelle que soit la méthode choisie ci-dessous, vous devez autoriser le trafic réseau vers les destinations listées via le **port 443**. Chacun des éléments du document XML suivant est expliqué dans le tableau qui le suit (en [termes et notations](#terms-and-notations)). Les plages d’adresses IP ont été intentionnellement omises dans ce document pour vous inciter à utiliser uniquement les noms de domaine complets, car les noms de domaine complets restent constants. Toutefois, vous pouvez télécharger le fichier XML contenant la liste complète à partir du centre de téléchargement : [VIP et plages d’adresses IP du service de notification Windows (WNS)](https://www.microsoft.com/download/details.aspx?id=44238). Les nouvelles adresses IP virtuelles ou les plages d’adresses IP seront **effectives une semaine après leur téléchargement**.

```XML
<?xml version="1.0" encoding="UTF-8"?>
<WNSPublicIpAddresses xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <!-- This file contains the FQDNs, VIPs, and IP address ranges used by the Windows Notification Service. A new text file will be uploaded every time a new VIP or IP range is released in production.  Please copy the below information and perform the necessary changes on your site. Endpoints in CloudService nodes are used for cloud services to send notifications to WNS. Endpoints in Client nodes are used by devices to receive notifications from WNS. --> 
    <CloudServiceDNS>
    <DNS FQDN="*.notify.windows.com"/>
    </CloudServiceDNS>
    <ClientDNS>
        <DNS FQDN="*.wns.windows.com"/>
        <DNS FQDN="*.notify.live.net"/>
    </ClientDNS>
    <CloudServiceIPs>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </CloudServiceIPs>
    <ClientIPsIPv4>
        <IpRange Subnet=""/>
        <!-- See the file in Download Center for the complete list of IP ranges -->
    </ClientIPsIPv4>
    <IdentityServiceDNS>
        <DNS FQDN="login.microsoftonline.com"/>
        <DNS FQDN="login.live.com"/>
    </IdentityServiceDNS>
</WNSPublicIpAddresses>

```

### <a name="terms-and-notations"></a>Termes et notations
Vous trouverez ci-dessous des explications sur les notations et les éléments utilisés dans l’extrait de code XML ci-dessus.

| Terme | Explication |
|---|---|
| **Point-notation décimale (par exemple, 64.4.28.0/26)** | La notation à points décimaux est un moyen de décrire la plage d’adresses IP. Par exemple, 64.4.28.0/26 signifie que les 26 premiers bits de 64.4.28.0 sont constants, tandis que les 6 derniers bits sont variables.  Dans ce cas, la plage IPv4 est 64.4.28.0-64.4.28.63. |
| **ClientDNS** | Il s’agit des filtres de nom de domaine complet (FQDN) pour les périphériques clients (PC Windows, ordinateurs de bureau) recevant des notifications de WNS. Elles doivent être autorisées par le biais du pare-feu pour permettre aux clients WNS d’utiliser la fonctionnalité WNS.  Il est recommandé d’autoriser la liste des noms de domaine complets au lieu des plages IP/VIP, puisque celles-ci ne seront jamais modifiées. |
| **ClientIPsIPv4** | Il s’agit des adresses IPv4 des serveurs auxquels les périphériques clients accèdent (ordinateurs Windows, ordinateurs de bureau) recevant des notifications de WNS. |
| **CloudServiceDNS** | Il s’agit des filtres de nom de domaine complet (FQDN) pour les serveurs WNS auxquels votre service Cloud doit communiquer pour envoyer des notificatios à WNS. Celles-ci doivent être autorisées par le biais du pare-feu pour permettre aux services d’envoyer des notifications WNS.  Il est recommandé d’autoriser la liste des noms de domaine complets au lieu des plages IP/VIP, puisque celles-ci ne seront jamais modifiées.|
| **CloudServiceIPs** | CloudServiceIPs sont les adresses IPv4 des serveurs utilisés pour les services Cloud pour envoyer des notifications à WNS  |


## <a name="microsoft-push-notifications-service-mpns-public-ip-ranges"></a>Plages d’adresses IP publiques du service de notifications push Microsoft (MPNS)
Si vous utilisez le service de notification hérité, MPNS, les plages d’adresses IP que vous devrez ajouter à la liste verte sont disponibles dans le centre de téléchargement : plages d’adresses [IP publiques du service de notifications push Microsoft (MPNS)](https://www.microsoft.com/download/details.aspx?id=44535).


## <a name="related-topics"></a>Rubriques connexes

* [Démarrage rapide : envoi d’une notification Push](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10))
* [Comment demander, créer et enregistrer un canal de notification](https://docs.microsoft.com/previous-versions/windows/apps/hh465412(v=win.10))
* [Comment intercepter les notifications pour les applications en cours d’exécution](https://docs.microsoft.com/previous-versions/windows/apps/jj709907(v=win.10))
* [Comment s’authentifier auprès des services de notifications Push Windows (WNS)](https://docs.microsoft.com/previous-versions/windows/apps/hh465407(v=win.10))
* [En-têtes des demandes et des réponses du service de notifications Push](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10))
* [Recommandations et liste de vérification sur les notifications Push](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview)
 
