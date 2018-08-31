---
author: normesta
Description: Distribute a packaged desktop app (Desktop Bridge)
Search.Product: eADQiWindows 10XVcnh
title: Publiez votre application de bureau mise en package dans un Store ou chargez-la de manière indépendante sur un ou plusieurs périphériques.
ms.author: normesta
ms.date: 05/18/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.localizationpriority: medium
ms.openlocfilehash: fe36fec72645558c539dd8270fd15d35d92b66b5
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/31/2018
ms.locfileid: "3235320"
---
# <a name="distribute-a-packaged-desktop-app-desktop-bridge"></a>Distribuer une application de bureau empaquetée (Pont du bureau)

Publiez votre application de bureau empaquetée dans un Store ou chargez-la de manière indépendante sur un ou plusieurs périphériques.  

> [!NOTE]
> Avez-vous un plan pour la méthode de transition des utilisateurs vers votre application empaquetée? Avant de distribuer votre application, consultez la section [Migration des utilisateurs vers votre application empaquetée](#transition-users) de ce guide où vous trouverez quelques idées.

## <a name="distribute-your-app-by-publishing-it-to-the-microsoft-store"></a>Distribuer votre application en la publiant sur le MicrosoftStore

Le [MicrosoftStore](https://www.microsoft.com/store/apps) est une méthode pratique pour rendre votre application accessible aux clients.

Publiez votre application dans ce Store pour atteindre un public plus large. En outre, les clients professionnels peuvent acquérir votre application pour la distribuer en interne au sein de leur organisation par le biais du [MicrosoftStore pour Entreprises](https://www.microsoft.com/business-store).

Si vous envisagez de publier dans le MicrosoftStore, vous êtes invité à répondre à quelques questions supplémentaires dans le cadre du processus de soumission. Ce, parce que votre manifeste du package déclare une fonctionnalité restreinte nommée **runFullTrust** et que nous avons besoin d'approuver l’utilisation de cette fonctionnalité par votre application. Vous trouverez davantage de détails sur ces exigences ici: [Fonctionnalités restreintes](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities).

Vous n’êtes pas obligé de signer votre application avant de la soumettre au Store.

>[!IMPORTANT]
> Si vous prévoyez de publier votre application dans le Microsoft Store, assurez-vous que votre application fonctionne correctement sur les appareils qui exécutent Windows10 S. Il s'agit d'une condition requise par le Store. Consultez [Tester votre application pour Windows10 S](desktop-to-uwp-test-windows-s.md).

<a id="side-load" />

## <a name="distribute-your-app-without-placing-it-onto-the-microsoft-store"></a>Distribuer votre application sans la mettre sur le MicrosoftStore

Si vous préférez distribuer votre application sans passer par leStore, sachez qu’il est possible de distribuer manuellement des applications pour un ou plusieurs périphériques.

Ceci peut être utile si vous souhaitez contrôler davantage l’expérience de distribution ou si vous ne voulez pas vous impliquer dans le processus de certification du MicrosoftStore.

Pour distribuer votre application à d’autres appareils sans passer par le Store, vous devez obtenir un certificat, signer votre application à l’aide de ce certificat, puis charger de manière indépendante votre application sur ces appareils.

Vous pouvez [créer un certificat](../packaging/create-certificate-package-signing.md) ou en obtenir un auprès d’un fournisseur populaire, tel que [Verisign](https://www.verisign.com/).

Si vous envisagez de distribuer votre application sur des périphériques exécutant Windows10 S, votre application doit être signée par le MicrosoftStore afin de passer par le processus de soumission de ce dernier, préalable à la distribution de votre application sur ces appareils.

Si vous créez un certificat, vous devez l’installer dans le magasin de certificats **Racine approuvée** ou **Personnes autorisées** de chaque appareil exécutant votre application. Si vous obtenez un certificat auprès d’un fournisseur populaire, vous n’aurez rien à installer sur les autres systèmes, hormis votre application.  

> [!IMPORTANT]
> Assurez-vous que le nom de l’éditeur mentionné sur votre certificat correspond à celui de l’éditeur de votre application.

Pour signer votre application à l’aide d’un certificat, voir [Signer un package d’application à l’aide de SignTool](../packaging/sign-app-package-using-signtool.md).

Pour charger votre application sur d’autres périphériques, consultez [Chargement indépendant d’applications métier dans Windows10](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10).

**Vidéos**

|Publier votre application dans le MicrosoftStore |Distribuer une application d’entreprise  |
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Windows-Store-Publication-3cWyG5WhD_5506218965"      width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Distribution-for-Enterprise-Apps-XJ5Hd5WhD_1106218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>Migration des utilisateurs vers votre application empaquetée

Avant de distribuer votre application, envisagez d’ajouter quelques extensions à votre manifeste du package pour aider les utilisateurs à prendre l’habitude d’utiliser votre application empaquetée. Voici certaines choses que vous pouvez faire.

* Pointez les vignettes existantes de l’écran de démarrage et les boutons de barre des tâches vers votre application empaquetée.
* Associez votre application empaquetée à un ensemble de types de fichiers.
* Autorisez votre application empaquetée à ouvrir certains types de fichiers par défaut.

Pour obtenir la liste complète des extensions et des conseils pour leur utilisation, voir [Migration des utilisateurs vers votre application](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Pensez également à ajouter du code à votre application empaquetée chargée d’accomplir ces tâches:

* migrer les données utilisateur associées à votre application de bureau dans les dossiers appropriés de votre application empaquetée;
* permettre aux utilisateurs de désinstaller la version de bureau de votre application.

Examinons chacune de ces tâches. Nous allons commencer par la migration des données utilisateur.

### <a name="migrate-user-data"></a>Migrer les données utilisateur

Si vous allez ajouter du code qui effectuera la migration des données utilisateur. Il est préférable de n’exécuter ce code que lors du démarrage initial de l’application. Avant de migrer les données des utilisateurs, affichez une boîte de dialogue expliquant à l’utilisateur ce qui est en train de se produire, les raisons pour lesquelles cette action est recommandée, et ce qui va advenir des données existantes.

Voici un exemple montrant comment vous pourriez procéder dans une application empaquetée .NET.

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        DialogResult migrateResult = MessageBox.Show
            ("Would you like to migrate your data from the previous version of this app?",
             "Data Migration", MessageBoxButtons.YesNo);

        if (migrateResult.Equals(DialogResult.Yes))
        {
            String destinationDir =
                Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

            Process process = new Process();
            process.StartInfo.FileName = "robocopy.exe";
            process.StartInfo.Arguments = "%LOCALAPPDATA%\\AppName " + destinationDir + " /move";
            process.StartInfo.CreateNoWindow = true;
            process.Start();
            process.WaitForExit();

            if (process.ExitCode > 1)
            {
                //Migration was unsuccessful -- you can choose to block/retry/other action
            }
        }
    }
}
```

### <a name="uninstall-the-desktop-version-of-your-app"></a>Désinstaller la version bureau de votre application

Il est préférable de ne pas désinstaller l’application de bureau des utilisateurs sans leur permission. Affichez une boîte de dialogue demandant l’autorisation de l’utilisateur. Les utilisateurs peuvent décider de conserver la version bureau de votre application. Dans ce cas de figure, vous devrez décider si vous souhaitez bloquer l’utilisation de l’application de bureau ou prendre en charge la cohabitation des deux applications.

Voici un exemple montrant comment vous pourriez procéder dans une application empaquetée .NET.

Pour voir le contexte complet de cet extrait de code, consultez le fichier **MainWindow.cs** de cet exemple [Visionneuse d’images WPF avec transition/migration/désinstallation](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition).

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop App is installed.
    if (uninstallString != null)
    {
        DialogResult uninstallResult = MessageBox.Show
            ("To have the best experience, consider uninstalling the "
              + " previous version of this app. Would you like to do that now?",
              "Uninstall the previous version", MessageBoxButtons.YesNo);

        if (uninstallResult.Equals(DialogResult.Yes))
        {
                    string[] uninstallArgs = uninstallString.Split(' ');

            Process process = new Process();
            process.StartInfo.FileName = uninstallArgs[0];
            process.StartInfo.Arguments = uninstallArgs[1];
            process.StartInfo.CreateNoWindow = true;

            process.Start();
            process.WaitForExit();

            if (process.ExitCode != 0)
            {
                //Uninstallation was unsuccessful - You can choose to block the app here.
            }
        }
    }

}
```

### <a name="video"></a>Vidéo

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Transition-Taskbar-Pins-Start-Tiles-File-Type-Associations-and-Protocol-Handlers-MD5mv5WhD_2406218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="next-steps"></a>Étapes suivantes

**Trouvez des réponses à vos questions**

Des questions? Contactez-nous sur Stack Overflow. Notre équipe contrôle ces [balises](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Vous pouvez également nous poser vos questions [ici](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Si vous rencontrez des problèmes lors de la publication de votre application sur le Store, ce [billet de blog](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/) offre certaines astuces utiles.

**Transmettre des commentaires ou suggérer des fonctionnalités**

Consultez [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).