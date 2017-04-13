---
author: laurenhughes
title: "Créer un certificat pour la signature de packages"
description: "Créer et exporter un certificat pour la signature de packages d&quot;applications à l&quot;aide d&quot;outils PowerShell."
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp"
ms.assetid: 7bc2006f-fc5a-4ff6-b573-60933882caf8
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 2332abe43732299dfb0f4bc265bf1b12877a17aa
ms.lasthandoff: 02/08/2017

---

# <a name="create-a-certificate-for-package-signing"></a>Créer un certificat pour la signature de packages

\[ Mise à jour pour les applications UWP sur Windows 10. Pour les articles sur Windows 8.x, voir l’[archive](http://go.microsoft.com/fwlink/p/?linkid=619132).\]

Cet article explique comment créer et exporter un certificat de signature de packages d'applications à l’aide des outils PowerShell. Il est recommandé d’utiliser Visual Studio pour les [packages d'applications UWP](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps), mais vous pouvez toujours empaqueter manuellement une application du Windows Store si vous n’utilisez pas Visual Studio pour développer votre application.

## <a name="prerequisites"></a>Prérequis

- **Une application empaquetée ou non**  
Une application contenant un fichier AppxManifest.xml. Vous devrez référencer le fichier manifeste pendant la création d’un certificat qui sera utilisé pour signer le package d’applications final. Pour plus d’informations sur l'empaquetage manuel d'une application, voir [Créer un package d’application avec l’outil MakeAppx.exe](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

- **Applets de commande d'infrastructure à clé publique (PKI)**  
Vous avez besoin d’applets de commande PKI pour créer et exporter votre certificat de signature. Pour plus d’informations, voir [Applets de commande d'infrastructure à clé publique](https://technet.microsoft.com/library/hh848636.aspx).

## <a name="create-a-self-signed-certificate"></a>Créer un certificat auto-signé

Un certificat auto-signé vous permet de tester votre application avant de la publier dans le Windows Store. Suivez les étapes décrites dans cette section pour créer un certificat auto-signé.

### <a name="determine-the-subject-of-your-packaged-app"></a>Déterminer l’objet de votre application empaquetée  

Pour utiliser un certificat de signature de votre package d’applications, « l'objet » contenu dans le certificat **doit** correspondre à la section « Publisher » du manifeste de votre application.

Par exemple, la section « Identity » dans le fichier AppxManifest.xml de votre application doit ressembler à ceci :
```
  <Identity Name="Contoso.AssetTracker" 
    Version="1.0.0.0" 
    Publisher="CN=Contoso Software, O=Contoso Corporation, C=US"/>
```

L'entité « Publisher », dans ce cas, est « CN = Contoso Software, O = Contoso Corporation, C = US » ; c'est cette entité que vous devez utiliser pour la création de votre certificat. 

### <a name="use-new-selfsignedcertificate-to-create-a-certificate"></a>Utiliser **New-SelfSignedCertificate** pour créer un certificat
Utilisez l'applet de commande PowerShell **New-SelfSignedCertificate** pour créer un certificat auto-signé. L'applet de commande **New-SelfSignedCertificate** comporte plusieurs paramètres de personnalisation, mais dans le cadre de cet article, nous allons créer un simple certificat qui fonctionne avec **SignTool**. Pour plus d’exemples et d’utilisations de cette applet de commande, voir [New-SelfSignedCertificate](https://technet.microsoft.com/library/hh848633.aspx).

À partir du fichier AppxManifest.xml de l’exemple précédent, vous devez utiliser la syntaxe suivante pour créer un certificat. Dans une invite PowerShell avec élévation de privilèges :
```
New-SelfSignedCertificate -Type Custom -Subject "CN=Contoso Software, O=Contoso Corporation, C=US" -KeyUsage DigitalSignature -FriendlyName <Your Friendly Name> -CertStoreLocation "Cert:\LocalMachine\My"
```

Après avoir exécuté cette commande, le certificat sera ajouté au magasin de certificats local, tel qu’indiqué dans le paramètre « -CertStoreLocation ». Le résultat de la commande génère également l’empreinte du certificat.  

**Remarque**  
Vous pouvez afficher votre certificat dans une fenêtre PowerShell en utilisant les commandes suivantes :
```
Set-Location Cert:\LocalMachine\My
Get-ChildItem | Format-Table Subject, FriendlyName, Thumbprint
```
Cela affichera tous les certificats contenus dans votre magasin local.

## <a name="export-a-certificate"></a>Exporter un certificat 

Pour exporter le certificat du magasin local dans un fichier Personal Information Exchange (PFX), utilisez l'applet de commande **Export-PfxCertificate**.

Lorsque vous utilisez **Export-PfxCertificate**, vous devez créer et utiliser un mot de passe ou utiliser le paramètre « -ProtectTo » pour spécifier les utilisateurs ou groupes qui peuvent accéder au fichier sans mot de passe. Notez qu’une erreur s’affiche si vous n’utilisez pas le paramètre « -Password » ou «-ProtectTo ».

- **Utilisation du paramètre « Password »**
```
$pwd = ConvertTo-SecureString -String <Your Password> -Force -AsPlainText 
Export-PfxCertificate -cert "Cert:\LocalMachine\My\<Certificate Thumbprint>" -FilePath <FilePath>.pfx -Password $pwd
```

- **Utilisation du paramètre « ProtectTo »**
```
Export-PfxCertificate -cert Cert:\LocalMachine\My\<Certificate Thumbprint> -FilePath <FilePath>.pfx -ProtectTo <Username or group name>
```

Après avoir créé et exporté votre certificat, vous êtes prêt à signer votre package d’applications avec **SignTool**. Pour l’étape suivante du processus d'empaquetage manuel, consultez [Signer un package d'application à l'aide de SignTool](https://msdn.microsoft.com/windows/uwp/packaging/sign-app-package-using-signtool).

## <a name="security-considerations"></a>Considérations de sécurité 
En ajoutant un certificat à des [magasins de certificats d'un ordinateur local](https://msdn.microsoft.com/windows/hardware/drivers/install/local-machine-and-current-user-certificate-stores), vous affectez la fiabilité du certificat pour tous les utilisateurs de l’ordinateur. Il est recommandé de supprimer ces certificats lorsque vous n'en avez plus besoin, ce afin d'éviter qu'ils ne soient utilisés pour compromettre la fiabilité du système.