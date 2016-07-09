---
author: joannaleecy
title: Proposer des jeux accessibles
description: "Découvrez comment assurer l’accessibilité de vos jeux. Appliquez le principe de conception de jeux inclusifs pour garantir l’accessibilité des jeux."
ms.assetid: f5ba1e60-0d7c-11e6-91ec-0002a5d5c51b
ms.sourcegitcommit: 2492ff5c8b3ba0331e831234943a1db124f8fa4f
ms.openlocfilehash: 2b78d767a7ac75e27f759c0eb06953e6158fb88b

---
#  Proposer des jeux accessibles

L’accessibilité contribue à accroître les capacités de l’ensemble des individus et des organisations de la planète, et cela se vérifie également dans le domaine des jeux vidéo. Cet article s’adresse aux développeurs de jeux, et plus précisément aux concepteurs, aux producteurs et aux gestionnaires de jeux. Il offre une vue d’ensemble des recommandations en matière de conception de jeux accessibles émanant de diverses organisations (répertoriées dans la section de références à la fin de cet article) et présente le principe de conception de jeux inclusifs à appliquer pour améliorer l’accessibilité des jeux.

##  Pourquoi proposer des jeux accessibles?

### Élargissement de la communauté des joueurs

À la base, la justification économique de l’accessibilité repose sur une formule d’une grande simplicité:

Nombre d’utilisateurs pouvant jouer à votre jeu x niveau d’excellence du jeu = augmentation des ventes du jeu

Si vous créez un jeu spectaculaire, mais si complexe ou alambiqué que seule une poignée de joueurs peuvent en profiter, vous limiterez votre nombre de ventes. De même, si vous concevez un jeu inutilisable par des personnes présentant des troubles physiques, sensoriels ou cognitifs, vous raterez des opportunités de ventes. Si l’on considère que [19% de la population souffre d’une forme quelconque de handicap ou d’invalidité rien qu’aux États-Unis](http://www.census.gov/newsroom/releases/archives/miscellaneous/cb12-134.html), cet aspect peut avoir une incidence considérable sur les revenus rapportés par votre jeu. 

Pour découvrir d’autres justifications commerciales, voir [Proposer des jeux vidéo accessibles](https://msdn.microsoft.com/library/windows/desktop/ee415219).

### Amélioration des jeux

La création d’un jeu plus accessible permet d’offrir un produit plus abouti au final. 

Les jeux qui proposent des sous-titres en constituent un bon exemple. Par le passé, les jeux prenaient rarement en charge le sous-titrage des dialogues. Aujourd’hui, il est d’usage de proposer des jeux incluant des sous-titres et des légendes. Cette évolution n’a pas été inspirée par des joueurs souffrant de handicaps ou d’invalidités. Elle s’est effectuée sous l’impulsion d’utilisateurs ayant tout simplement estimé que les jeux avec sous-titres leur offraient une meilleure expérience de jeu. Les joueurs activent les sous-titres et les légendes lorsqu’ils jouent dans un environnement présentant un bruit de fond important, lorsqu’ils éprouvent des difficultés d’audition des voix accompagnées de divers effets sonores ou bruits ambiants, ou simplement lorsqu’ils doivent maintenir le volume à bas niveau pour ne pas déranger leur entourage. L’affichage de sous-titres et de légendes a non seulement contribué à améliorer l’expérience de jeu des utilisateurs, mais a également étendu cette expérience aux personnes souffrant de handicaps ou d’invalidités.

Pour les mêmes raisons, la reconfiguration des manettes est une autre fonctionnalité progressivement adoptée en standard par l’industrie des jeux électroniques. Les joueurs apprécient de pouvoir personnaliser leur expérience de jeu. La plupart des gens ignorent que la possibilité de reconfigurer les boutons d’un périphérique d’entrée constitue en réalité une fonctionnalité d’accessibilité initialement conçue pour permettre aux utilisateurs présentant différents handicaps psychomoteurs de manipuler des jeux.

Au final, la réflexion que vous aurez menée pour améliorer l’accessibilité de votre jeu se traduira généralement par l’obtention d’un jeu plus abouti, car vous offrirez à vos utilisateurs une expérience plus conviviale et personnalisable.

### Création d’un espace social

Les jeux sont une forme de divertissement pouvant se révéler une source de joie des heures durant. Toutefois, pour certaines personnes, le jeu permet non seulement de se divertir, mais également d’échapper à un lit d’hôpital, à une douleur chronique ou à une anxiété sociale invalidante. Les joueurs sont transportés au sein d’un univers qui leur offre la possibilité de devenir les personnages principaux du jeu vidéo. Grâce aux jeux, ils peuvent donner vie et prendre part à un espace social qui leur est destiné et qui leur fait oublier agréablement leur combat quotidien contre le handicap dont ils souffrent tout en leur offrant l’opportunité de communiquer avec des personnes avec lesquelles ils n’auraient peut-être pas la possibilité d’échanger dans d’autres circonstances.

##  Le jeu que vous proposez aujourd’hui est-il accessible?

Si vous envisagez pour la première fois de rendre votre jeu accessible, voici quelques questions que vous devez vous poser:

* Pouvez-vous exécuter le jeu avec une seule main? 
* La prise en main de votre jeu peut-elle être effectuée par une personne lambda?
* Pouvez-vous jouer facilement au jeu sur un moniteur ou un téléviseur de petite taille à une certaine distance?
* Prenez-vous en charge plusieurs types de périphériques d’entrée utilisables tout au long du jeu?
* Pouvez-vous jouer au jeu avec le son désactivé?
* Pouvez-vous jouer au jeu avec votre moniteur configuré en noir et blanc?

Si vous répondez non à la plupart de ces questions ou que vous n’en connaissez pas les réponses, il est temps de passer à la vitesse supérieure et de garantir l’accessibilité de votre jeu.

## Définition de la notion de handicap/invalidité

Le terme «handicap/invalidité» désigne une incompatibilité entre les besoins de l’individu et le service, le produit ou l’environnement proposés. ([Vidéo Inclusive](https://www.microsoft.com/design/inclusive), Microsoft.com.) Cela signifie que tout le monde peut souffrir d’un handicap ou d’une invalidité et qu’il peut s’agir d’une situation à court terme ou circonstancielle. Envisagez toutes les difficultés que les joueurs atteints de tels handicaps risquent de rencontrer en utilisant votre jeu, et réfléchissez à la façon dont vous pouvez améliorer la conception de votre jeu à leur intention. Voici quelques-uns des handicaps ou invalidités que vous devez prendre en compte:

### Vision

*   Affections médicales à long terme telles que le glaucome, la cataracte, le daltonisme, la myopie et la rétinopathie diabétique
*   Situations circonstancielles à court terme comme un moniteur ou un écran de petite taille, un écran basse résolution ou la présence de reflets sur l’écran en raison de l’exposition du moniteur à des sources de lumière vive
        
### Audition

* Affections médicales à long terme telles qu’une surdité totale ou une perte d’audition partielle découlant de maladies ou de facteurs génétiques
* Situations circonstancielles à court terme comme un bruit de fond excessif ou une baisse du volume pour ne pas gêner l’entourage
        
### Motricité

* Affections médicales à long terme telles que la maladie de Parkinson, la sclérose latérale amyotrophique (SLA), l’arthrite et la dystrophie musculaire
* Situations circonstancielles à court terme comme une blessure à la main ou le fait de tenir une boisson ou de porter un enfant dans ses bras
  
### Capacités cognitives

* Affections médicales à long terme telles que la dyslexie, l’épilepsie, le trouble déficitaire de l’attention avec hyperactivité (TDAH), la démence et l’amnésie
* Situations circonstancielles à court terme comme le manque de sommeil ou les distractions temporaires telles que la sirène d’une ambulance passant à proximité

### Parole

* Affections médicales à long terme telles qu’une lésion des cordes vocales, une dysarthrie ou une apraxie
* Situations circonstancielles à court terme comme des soins dentaires ou le fait de manger et de boire


## Comment proposer des jeux plus accessibles?

### Évolution des pratiques de conception: adoptez une approche de conception de jeux inclusive

La conception inclusive est axée sur la création de produits et de services plus accessibles à un plus vaste éventail de clients, y compris aux personnes souffrant de handicaps ou d’invalidités.

Pour atteindre cet objectif, les concepteurs de jeux doivent désormais viser davantage que la simple création de jeux distrayants destinés à un petit groupe d’utilisateurs. Il leur faut tenir compte de l’impact de leur décisions de conception sur l’accessibilité globale du jeu, autrement dit sur la facilité de manipulation du jeu par l’ensemble des utilisateurs auxquels ils s’adressent, y compris ceux atteints de handicaps ou d’invalidités.

Les paradigmes de conception de jeux traditionnels doivent donc évoluer afin de prendre en compte le concept de conception de jeux inclusive. La conception de jeux inclusive implique de dépasser le processus élémentaire de développement de jeux uniquement axé sur le divertissement du public cible en créant des personnages supplémentaires ou modifiés afin d’inclure un plus large éventail de personnes. 

Cette étape supplémentaire permet d’identifier les lacunes dans la conception d’origine. En identifiant ces lacunes, vous pouvez itérer sur le concept de conception d’origine et améliorer ce dernier. Lorsque vous prenez le temps de mettre en place un processus de conception de jeux plus inclusif, vous améliorez l’accessibilité de votre jeu au final.

### Accroissement des capacités des joueurs: élargissez les options des utilisateurs

L’accessibilité consiste essentiellement à offrir davantage de possibilités. Offrez à vos joueurs une multitude d’options pour personnaliser leur expérience de jeu. Si vos jeux connaissent déjà une immense popularité, il est possible qu’une part non négligeable de vos fans refuse toute modification de l’expérience de jeu. Cela ne pose aucun problème. Offrez à vos joueurs la possibilité d’activer ou de désactiver les fonctionnalités d’accessibilité et de configurer ces dernières individuellement.

### Innovation: faites preuve de créativité

Vous disposez de nombreux moyens pour améliorer l’accessibilité de votre jeu. Soyez inventif et inspirez-vous des autres jeux accessibles existants. Si vous avez déjà créé un jeu, efforcez-vous d’en identifier les fonctionnalités que vous pourriez améliorer tout en conservant les mécanismes clés du jeu et l’expérience de jeu initiale. Comme indiqué précédemment, l’accessibilité en matière de jeux repose entièrement sur l’offre de possibilités de personnalisation de l’expérience de jeu.

### Sensibilisation: mettez l’accent sur l’accessibilité dans votre studio de jeu

Le développement de jeux étant fréquemment soumis à des délais serrés, votre capacité à hisser l’accessibilité au rang de vos priorités contribuera à simplifier ce processus. Un bon moyen de procéder consiste à concevoir vos jeux à partir de zéro en gardant l’accessibilité à l’esprit. Partagez vos connaissances sur l’accessibilité avec les membres de votre équipe et présentez-leur les justifications commerciales de cette approche.

### Vérification: évaluez continuellement votre jeu

Au cours du développement, vous pouvez introduire un processus de vérification destiné à vous assurer que chaque étape de la conception est axée sur l’accessibilité. Établissez une liste de contrôle semblable à celle ci-dessous pour aider votre équipe à déterminer continuellement si le jeu que vous créez est ou non accessible.

| Liste de contrôle                                         | Fonctionnalités d’accessibilité                                                                                                         |
|---------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|
| Séquences animées au sein du jeu                                | Présentation de sous-titres et de légendes, test de photosensibilité des animations                                                                           |
| Conception graphique globale (graphismes 2D et 3D)              | Couleurs et options compatibles avec le daltonisme, possibilités d’identification non entièrement basées sur les couleurs et tirant également parti des formes et des motifs|
| Écran d’accueil, menu de paramètres et autres menus       | Possibilité de lecture des options à voix haute, possibilité de mémorisation des paramètres, méthode d’entrée de commande alternative, possibilité de réglage de la taille des polices de l’interface utilisateur  |
| Expérience de jeu                                          | Vaste choix de niveaux de difficulté, affichage de sous-titres et de légendes, retour visuel et audio pertinent pour le joueur                           |
| Affichage tête haute                                       | Position d’écran réglable, taille de police ajustable, option destinée aux personnes daltoniennes                                                  |        
| Entrée de contrôle                                     | Possibilité de reconfigurer des contrôles sur le périphérique d’entrée, prise en charge de manettes personnalisées, autorisation d’entrées simplifiées pour le jeu                               |        

### Test du jeu et itération: recueillez les commentaires des joueurs

Lorsque vous organisez des sessions de test du jeu, invitez des personnes souffrant de handicaps ou d’invalidités à y participer afin de vérifier l’accessibilité de votre jeu. Observez la façon dont ces personnes utilisent votre jeu et recueillez leurs commentaires. Déterminez les changements à apporter afin d’améliorer votre jeu.

### Promotion: signalez l’accessibilité de votre jeu au monde entier

Les utilisateurs ont besoin de savoir si votre jeu est manipulable par des personnes souffrant de handicaps ou d’invalidités. Signalez lisiblement l’accessibilité de votre jeu sur le site web et sur l’emballage du jeu pour vous assurer que les clients comprennent exactement les possibilités que leur offrira votre jeu. Pensez également à assurer l’accessibilité de votre site web et de tous les circuits de vente de votre jeu. Plus important encore, faites la promotion de votre jeu auprès de la communauté de joueurs concernés par l’accessibilité.

## Fonctionnalités d’accessibilité des jeux

Cette section décrit certaines des fonctionnalités vous permettant d’améliorer l’accessibilité de votre jeu. Ces fonctionnalités émanent des [recommandations en matière de conception de jeux accessibles](http://gameaccessibilityguidelines.com/) (en anglais), qui présentent les conclusions d’un groupe de concertation réunissant différents studios, spécialistes et universitaires. Pour plus d’informations, voir l’article [Game accessibility guidelines](http://gameaccessibilityguidelines.com/) (en anglais). 

### Graphismes et interface utilisateur compatibles avec le daltonisme

La rétine comporte deux types de cellules photoréceptrices: les cônes qui permettent de percevoir la lumière, et les bâtonnets qui facilitent la vision dans des conditions de faible luminosité. Il existe trois types de cônes (rouges, verts et bleus) qui nous aident à visualiser correctement les couleurs. Le daltonisme survient lorsque l’un ou plusieurs de ces trois types de cônes ne fonctionnent pas comme prévu. Il existe différents degrés de daltonisme, depuis une perception des couleurs quasi-normale avec réduction de la sensibilité à la lumière rouge, verte ou bleue jusqu’à une totale incapacité à percevoir les couleurs rouge, verte ou bleue. Du fait de la rareté des cas de sensibilité réduite à la lumière bleue, lorsque vous concevez un jeu pour des personnes daltoniennes, choisissez les couleurs en fonction des individus incapables de percevoir la couleur rouge ou verte:
 
  + Utilisez des combinaisons de couleurs pouvant être distinguées par les personnes daltoniennes incapables de percevoir le rouge et le vert:
  
    * couleurs apparaissant comme similaires: toutes les nuances de rouge et de vert, y compris le marron et l’orange;
    * couleurs qui ressortent: le bleu et le jaune.
    
  + L’identification des éléments de votre jeu ne doit pas reposer entièrement sur la couleur; elle doit également être rendue possible par les formes et les motifs.
  
### Sous-titres et légendes

Lorsque vous concevez les sous-titres et les légendes de votre jeu, vous devez offrir la possibilité d’activer des sous-titres lisibles pour permettre aux utilisateurs de jouer à votre jeu sans le son. Vous devez faire en sorte que certains composants du jeu, tels que les dialogues et les animations et effets sonores, soient affichables sous forme de texte à l’écran.

Voici quelques recommandations de base à prendre en compte lors de la conception des sous-titres et des légendes:

*   Sélectionnez une police lisible simple.
*   Choisissez une police suffisamment grande ou envisagez de proposer une option de taille de police ajustable afin d’améliorer la flexibilité. (La taille de police idéale dépend de la taille de l’écran, de l’éloignement de l’écran, etc.)
*   Créez un contraste élevé entre la couleur d’arrière-plan et la couleur de police. (Pour plus d’informations, voir [Coefficients de contraste](https://msdn.microsoft.com/windows/uwp/accessibility/accessible-text-requirements).)
* Affichez des phrases courtes à l’écran. (Veillez à ne pas gâcher le suspense de votre jeu en affichant le texte avant que l’événement correspondant ne se produise.)
*   Indiquez clairement la provenance de l’effet sonore ou le nom du personnage en train de parler. (Par exemple: «Daniel: Bonjour!»)
*   Offrez la possibilité d’activer et de désactiver les sous-titres et les légendes. (Fonctionnalité supplémentaire: possibilité de sélection de la quantité d’informations sonores affichées en fonction de leur importance.)

### Retour audio

Le son fournit un retour au joueur en complément du retour visuel. La conception d’une structure audio efficace peut améliorer l’accessibilité de votre jeu pour les joueurs souffrant d’une déficience visuelle. Voici quelques recommandations à prendre en compte:

*   Utilisez des signaux audio 3D pour fournir des informations spatiales complémentaires.
* Séparez les contrôles de volume de la musique, de la voix et des effets sonores.
*   Choisissez des formulations qui fournissent des informations pertinentes pour les joueurs. (Par exemple: «Les ennemis sont en train d’entrer par la porte de derrière» plutôt que «Les ennemis sont en approche».)
*   Assurez-vous que la vitesse d’élocution est correcte et proposez un contrôle permettant d’ajuster cette vitesse pour améliorer l’accessibilité.

### Contrôles entièrement configurables

Certaines entreprises et organisations, telles que [Special Effect](http://www.specialeffect.org.uk/), conçoivent des manettes de jeu personnalisées qui sont utilisables avec différents systèmes de jeu, comme Windows et XboxOne. Cette personnalisation permet à des personnes atteintes de différentes formes de handicap ou d’invalidité de jouer à des jeux qu’ils n’auraient pas pu expérimenter sans cela. Pour découvrir des exemples d’utilisateurs qui sont désormais en mesure de jouer à des jeux de façon autonome grâce aux manettes personnalisées, consultez la page présentant [les personnes ayant bénéficié de l’aide de SpecialEffect](http://www.specialeffect.org.uk/who-we-helped).

En tant que développeur de jeux, vous pouvez améliorer l’accessibilité de votre jeu en autorisant les contrôles entièrement configurables afin d’offrir aux joueurs la possibilité de brancher leurs manettes personnalisées et de reconfigurer les touches selon leurs besoins.

Les manettes XboxOne standard et XboxElite sont personnalisables pour les jeux de précision. Pour plus d’informations, voir [XboxOne](http://support.xbox.com/xbox-one/accessories/customize-standard-controller-with-accessories-app) et [XboxElite](http://support.xbox.com/xbox-one/accessories/use-accessories-app-configure-elite-controller).

### Large éventail de niveaux de difficulté

Le rôle des jeux vidéo est de divertir les utilisateurs. La difficulté pour les développeurs consiste à ajuster le niveau de difficulté afin de proposer au joueur le défi le mieux adapté. Pour commencer, l’habilité et les capacités des joueurs diffèrent selon les individus. L’élargissement de l’éventail de niveaux de difficulté est donc plus susceptible de satisfaire aux attentes précises des joueurs en matière de défi à relever. Parallèlement, le fait d’offrir un choix de niveaux de difficulté plus étendu contribue également à optimiser l’accessibilité de votre jeu en permettant à un plus grand nombre de personnes handicapées d’utiliser votre jeu. N’oubliez pas que les joueurs sont motivés par la perspective de relever les différents défis d’un jeu et d’en être récompensés. Ils ne veulent pas d’un jeu impossible à gagner.

La mise au point du niveau de difficulté de votre jeu constitue un processus délicat. Si votre jeu est trop simple, les joueurs risquent de s’en lasser. S’il est trop difficile, il est possible que les utilisateurs finissent par baisser les bras et par abandonner votre jeu. Trouver le juste équilibre entre les deux relève à la fois de l’art et de la science. Il existe de nombreuses façons de concevoir un jeu offrant le niveau de difficulté approprié. Certains jeux proposent des entrées simplifiées, comme la possibilité de jouer par une simple pression sur un bouton, une option de rembobinage et de recommencement de la partie pour obtenir une nouvelle chance de gagner, ou une possibilité de diminuer le nombre ou la force des adversaires afin de faciliter la progression dans le jeu après plusieurs tentatives.

### Test contre les risques d’épilepsie photosensible

L’épilepsie photosensible désigne le déclenchement de crises d’épilepsie par des stimuli visuels tels que l’exposition à des lumières clignotantes ou à certains types de formes et de motifs visuels en mouvement. Ce type de trouble touche près de trois pour cent de la population et survient plus fréquemment chez les enfants et les adolescents.

De nombreux facteurs peuvent entraîner une réaction photosensible lors de l’utilisation d’un jeu vidéo, comme la durée de la partie, la fréquence des clignotements, l’intensité lumineuse, le contraste de l’arrière-plan et des motifs lumineux, la distance entre l’écran et le joueur, ainsi que la longueur d’onde de la lumière.

Si vous développez des jeux, voici quelques conseils à suivre pour concevoir des jeux utilisables par les personnes souffrant d’épilepsie photosensible:

*   Évitez d’introduire des lumières clignotantes dont la fréquence est comprise entre 5 et 30clignotements par seconde (Hertz), car cette fréquence de clignotements est la plus susceptible de déclencher des crises.
*   Utilisez un système automatisé pour rechercher dans votre jeu la présence éventuelle de stimuli risquant de provoquer une épilepsie photosensible. (Par exemple, utilisez [l’outil Harding Flash and Pattern Analyzer (FPA) G2](http://www.hardingfpa.com/harding-fpa-for-games/) conçu par Cambridge Research System Ltd et le professeur GrahamHarding.) 
*   Introduisez des pauses entre les niveaux de jeu afin d’inciter les joueurs à s’arrêter de temps en temps au lieu de jouer pendant plusieurs heures d’affilée.

## Autres ressources en matière d’accessibilité

Vous trouverez ci-après quelques sites externes fournissant des informations supplémentaires sur l’accessibilité des jeux.

### Recommandations en matière de conception de jeux accessibles
* [Game accessibility guidelines (en anglais)](http://gameaccessibilityguidelines.com/)
* [Recommandations d’AbleGamers Foundation (en anglais)](http://www.includification.com/)
* [Concevoir des jeux universellement accessibles (en anglais)](http://www.ics.forth.gr/hci/ua-games/index_main.php?l=e&c=555)

### Manettes de jeu personnalisées
* [Special Effect (en anglais)](http://www.specialeffect.org.uk/)
* [Warfighter Engaged (en anglais)](http://www.warfighterengaged.org/)

## Références utilisées
* [Game accessibility guidelines (en anglais)](http://gameaccessibilityguidelines.com/)
* [Recommandations d’AbleGamers Foundation (en anglais)](http://www.includification.com/)
* [Color Blind Awareness, entreprise d’intérêt communautaire (en anglais)](http://www.colourblindawareness.org/colour-blindness/types-of-colour-blindness/)
* [How to do subtitles well (en anglais) - Article de blog sur Gamasutra par IanHamilton](http://www.gamasutra.com/blogs/IanHamilton/20150715/248571/How_to_do_subtitles_well__basics_and_good_practices.php)
* [Innovation for All Programme (en anglais)](http://www.inclusivedesign.no/practical-tools/definitions-article56-127.html)

## Liens connexes
* [Conception inclusive](https://www.microsoft.com/design/inclusive)
* [Hub de développeurs axés sur l’accessibilité Microsoft](https://developer.microsoft.com/windows/accessible-apps)
* [Développement d’applications UWP accessibles](https://msdn.microsoft.com/windows/uwp/accessibility/accessibility)
* [Livre électronique sur la conception de logiciels accessibles](https://www.microsoft.com/download/details.aspx?id=19262)



<!--HONumber=Jun16_HO4-->

