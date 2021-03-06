---
Description: Les boîtes de dialogue et les menus volants affichent des éléments temporaires d’interface utilisateur quand l’utilisateur les sollicite ou quand un événement nécessite une notification ou une approbation.
title: Boîtes de dialogue et menus volants
template: detail.hbs
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c61d1478c38df315a3fe3c20151de8c2bfbca4e2
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81123562"
---
# <a name="dialogs-and-flyouts"></a>Boîtes de dialogue et menus volants

Les boîtes de dialogue et les menus volants sont des éléments temporaires d’interface utilisateur qui s’affichent quand un événement se produit qui nécessite une notification, une approbation ou d’autres informations de l’utilisateur.

> **API de plateforme :** [classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [classe Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout)

**Boîtes de dialogue**

![Exemple de boîte de dialogue](../images/dialogs/dialog_RS2_delete_file.png)

Les boîtes de dialogue sont des superpositions d’interface utilisateur modales qui fournissent des informations contextuelles sur l’application. Les boîtes de dialogue bloquent les interactions avec la fenêtre de l’application jusqu’à ce qu’elles soient masquées explicitement. Elles exigent souvent une forme d’action de la part de l’utilisateur.

**Menus volants**

![Exemple de menu volant](../images/flyout-example2.png)

Un menu volant est une fenêtre contextuelle légère qui affiche l’interface utilisateur liée aux opérations qu’effectue l’utilisateur. Il comprend une logique de placement et de dimensionnement, et peut être utilisé pour afficher un contrôle secondaire ou des détails supplémentaires sur un élément.

Contrairement à une boîte de dialogue, un menu volant peut être fermé rapidement en appuyant ou en cliquant en dehors du menu volant, en appuyant sur la touche ÉCHAP ou le bouton Précédent, en redimensionnant la fenêtre d’application ou en modifiant l’orientation de l’appareil.

## <a name="is-this-the-right-control"></a>Est-ce le contrôle approprié ?

Les boîtes de dialogue et les menus volants permettent aux utilisateurs de prendre connaissance d’informations importantes, mais elles perturbent également l’expérience utilisateur. Les boîtes de dialogue étant modales (bloquantes), elles interrompent les utilisateurs et les empêchent de faire autre chose tant qu’ils n’interagissent pas avec la boîte de dialogue. Les menus volants sont moins dérangeants, mais si vous en affichez trop, vous perturbez également l’utilisateur.

Une fois que vous avez déterminé que vous voulez utiliser une boîte de dialogue ou un menu volant, vous devez choisir lequel utiliser.

Étant donné que les boîtes de dialogue bloquent les interactions contrairement aux menus volants, elles doivent être réservées aux situations dans lesquelles vous voulez que l’utilisateur interrompe tout ce qu’il est en train de faire pour se concentrer sur une information particulière ou répondre à une question. Les menus volants, quant à eux, peuvent être utilisés quand vous voulez attirer l’attention de l’utilisateur sur quelque chose, mais qu’il a la possibilité de l’ignorer.

   <p><b>Utilisez une boîte de dialogue pour...</b> <br/>
<ul>
<li>Afficher des informations importantes que l’utilisateur <b>doit</b> lire et accepter avant de poursuivre. Exemples :
<ul>
  <li>Utilisez ce type de contrôle pour indiquer à l’utilisateur toute situation d’atteinte possible à la sécurité.</li>
  <li>Utilisez ce type de contrôle pour signaler à l’utilisateur qu’il s’apprête à modifier de manière irrémédiable un élément utile.</li>
  <li>Utilisez ce type de contrôle pour signaler à l’utilisateur qu’il s’apprête à supprimer un élément utile.</li>
  <li>Pour confirmer un achat dans l’application</li>
</ul>

</li>
<li>Les messages d’erreur qui s’appliquent au contexte global de l’application, liés par exemple à une erreur de connectivité.</li>
<li>Utilisez une boîte de dialogue à question pour indiquer que l’application doit poser à l’utilisateur une question bloquante, parce qu’elle ne peut pas choisir telle ou telle option à la place de l’utilisateur, par exemple. Une question bloquante ne peut pas être ignorée ni reportée, et doit offrir à l’utilisateur des options clairement définies.</li>
</ul>
</p>


   <p><b>Utilisez un menu volant pour...</b> <br/>
<ul>
<li>Collecter des informations supplémentaires nécessaires pour pouvoir effectuer une action.</li>
<li>Afficher des informations qui ne sont pas pertinentes le reste du temps. Par exemple, dans une application de galerie de photos, quand l’utilisateur clique sur une vignette d’image, vous pouvez utiliser un menu volant pour afficher une version agrandie de l’image.</li>
<li>Affichage d’informations supplémentaires, comme des détails ou des descriptions plus longues sur un élément de la page.</li>
</ul></p>

## <a name="ways-to-avoid-using-dialogs-and-flyouts"></a>Moyens d’éviter d’utiliser les boîtes de dialogue et menus volants

Mesurez l’importance des informations à partager : sont-elles suffisamment importantes pour interrompre l’utilisateur ? Évaluez également la fréquence à laquelle les informations doivent être affichées. Si vous affichez une boîte de dialogue ou une notification toutes les 5 minutes, vous pouvez peut-être plutôt leur allouer un emplacement dans l’interface utilisateur principale. Par exemple, dans un client de chat, au lieu d’afficher un menu volant chaque fois qu’un ami se connecte, vous pouvez afficher la liste des amis en ligne sur le moment et mettre en évidence les amis quand ils se connectent.

Les boîtes de dialogue sont fréquemment utilisées pour confirmer une action (par exemple, la suppression d’un fichier) avant de l’exécuter. Si vous voulez que l’utilisateur effectue souvent une action particulière, fournissez un moyen d’annuler l’action quand il fait une erreur plutôt que de forcer l’utilisateur à confirmer l’action chaque fois.

## <a name="how-to-create-a-dialog"></a>Procédure pour créer une boîte de dialogue

Consultez l’[article sur les boîtes de dialogue](dialogs.md). 

## <a name="how-to-create-a-flyout"></a>Procédure pour créer un menu volant

Consultez l’[article sur les menus volants](flyouts.md). 

## <a name="examples"></a>Exemples

<table>
<th align="left">Galerie de contrôles XAML<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Si vous disposez de l’application <strong style="font-weight: semi-bold">Galerie de contrôles XAML</strong>, cliquez ici pour ouvrir l’application et voir l’objet <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> ou <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> en action.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenir l’application Galerie de contrôles XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenir le code source (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

