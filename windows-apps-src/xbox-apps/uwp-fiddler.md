---
title: Utilisation de Fiddler avec Xbox One lors du développement pour UWP
description: Décrit l’utilisation du logiciel gratuit Fiddler pour voir le trafic réseau sur un kit de développement Xbox One UWP.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 9c133c77-fe9d-4b81-b4b3-462936333aa3
ms.localizationpriority: medium
ms.openlocfilehash: 4b6bc87c4d1694b47823a92619e6ec9d1e91bb69
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319911"
---
# <a name="how-to-use-fiddler-with-xbox-one-when-developing-for-uwp"></a>Utilisation de Fiddler avec Xbox One lors du développement pour UWP

Fiddler est un proxy de débogage web qui consigne tout le trafic HTTP et HTTPS entre votre kit de développement Xbox One et Internet. Vous pouvez l’utiliser pour consigner et inspecter le trafic vers et depuis les services Xbox et les services web des parties de confiance, ainsi que pour comprendre et déboguer les appels de services web. 

En fonctionnement normal, une console qui communique via un proxy court le risque que ses communications soient modifiées par celui-ci, ce qui permet des triches éventuelles des joueurs. Par conséquent, les consoles sont conçues pour ne pas autoriser les communications via un proxy. SI vous utilisez Fiddler avec votre kit de développement Xbox One, vous devez effectuer une configuration spéciale sur le kit de développement pour lui permettre d’utiliser le proxy Fiddler. 

Fiddler est un logiciel gratuit, disponible au téléchargement sur le [site web de Fiddler](https://www.telerik.com/download/fiddler). 

Fiddler peut modifier l’état du réseau signalé par la console. Si une connexion en amont est désactivée à partir de l’ordinateur exécutant Fiddler, la console peut ne pas détecter cette déconnexion jusqu’à ce que l’authentification de la console expire. Si vous utilisez Fiddler, veillez à fermer la connexion entre la console et l’ordinateur exécutant Fiddler, plutôt que d’utiliser Fiddler pour simuler une déconnexion.

### <a name="to-install-and-enable-fiddler-on-your-development-pc"></a>Installer et activer Fiddler sur votre ordinateur de développement
Suivez la procédure ci-après pour installer et activer Fiddler pour la surveillance du trafic à partir de votre kit de développement :

1. Installez Fiddler sur votre PC de développement en suivant les instructions du [site web Fiddler](https://www.telerik.com/download/fiddler). 
2. Lancez Fiddler, puis sélectionnez **Fiddler Options** à partir du menu **Tools.** 
3. Sélectionnez l’onglet **Connections** et assurez-vous que l’option **Allow remote computers to connect** est sélectionnée. 
4. Cliquez sur **OK** pour accepter vos modifications des paramètres. Une boîte de dialogue s’affiche alors, indiquant que Fiddler doit être redémarré pour que les modifications prennent effet, et que vous devrez configurer votre pare-feu manuellement. Cliquez sur **OK** dans cette boîte de dialogue, mais *ne redémarrez pas immédiatement Fiddler*.
5. Configurez la règle de pare-feu requise pour permettre à des ordinateurs distants de se connecter. Démarrez l’applet de commande Panneau de configuration Pare-feu Windows. Cliquez sur **Paramètres avancés**, puis sur **Règle de trafic entrant**. Trouvez la règle nommée « FiddlerProxy » et faites défiler vers la droite, en vérifiant que chacun des paramètres dans le tableau suivant s’affiche pour cette règle.
  
  | Paramètre           | Valeur préférée                |
  | ----              | ----                           |
  | Nom              | FiddlerProxy                   |
  | Regrouper             | *Aucune valeur* |
  | Profil           | Toutes                            |
  | Enabled           | Oui                            |
  | Action            | Autoriser                          |
  | Remplacement          | Non                             |
  | Programme           | *chemin d’accès au fiddler.exe*          |
  | LocalAddress      | Indifférent                            |
  | RemoteAddress     | Indifférent                            |
  | Protocol          | TCP                            |
  | LocalPort         | Indifférent                            |
  | RemotePort        | Indifférent                            |
  | AllowedUsers      | Indifférent                            |
  | AllowedComputers  | Indifférent                            |


6. Configurez Fiddler pour capturer et déchiffrer le trafic HTTPS en procédant comme suit :
  1. Pour optimiser les performances, définissez Fiddler pour qu’il utilise le mode streaming en cliquant sur le bouton **Stream** dans la barre des boutons.
  2. Dans le menu **Tools**, sélectionnez **Fiddler Options**, puis cliquez sur **HTTPS**.
  3. Cochez la case **Decrypt HTTPS traffic**. Si une boîte de dialogue vous demande si vous souhaitez configurer Windows afin de faire confiance au certificat d’autorité de certification, cliquez sur **No**.
  4. Cliquez sur **Export Root Certificate to Desktop**.
7. Quittez et redémarrez Fiddler.

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>Configurer un kit de développement pour utiliser Fiddler comme proxy pour Internet

1. Naviguez vers l’outil **Réseau** dans l’interface utilisateur du portail des appareils Xbox.
2. Accédez au certificat racine Fiddler que vous avez exporté sur le Bureau. 
3. Tapez l’adresse IP ou le nom d’hôte du PC de développement exécutant Fiddler.
4. Entrez le numéro de port sur lequel Fiddler écoute (par défaut, Fiddler utilise le port 8888). 
5. Cliquez sur **Activer**. Cette opération va redémarrer votre kit de développement.

### <a name="to-stop-using-fiddler"></a>Arrêter l’utilisation de Fiddler
Pour arrêter d’utiliser Fiddler comme proxy pour Internet (et cesser le suivi de l’ensemble du trafic réseau du kit de développement par Fiddler), procédez comme suit :

1. Naviguez vers l’outil **Réseau** dans l’interface utilisateur du portail des appareils Xbox.
2. Cliquez sur **Désactiver**.

> [!NOTE]
> Chaque PC doté de Fiddler utilise un certificat racine Fiddler différent. Si vous avez plusieurs PC qui peuvent être utilisés pour fournir un proxy Fiddler pour votre kit de développement, vous devez sélectionner le nouveau certificat racine lorsque vous passez de l’un à l’autre. Si vous utilisez un seul PC, vous devez sélectionner le certificat racine seulement la première fois que vous activez Fiddler. Vous devez toujours spécifier l’adresse IP et le port.

## <a name="see-also"></a>Voir aussi
- [Référence des paramètres API de Fiddler](wdp-fiddler-api.md)
- [Forum Aux Questions](frequently-asked-questions.md)
- [UWP sur Xbox One](index.md)



