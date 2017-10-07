---
title: aaaUse Azure File storage med Linux | Microsoft Docs
description: "Lär dig hur toomount en Azure-fil för att dela över SMB i Linux."
services: storage
documentationcenter: na
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 6edc37ce-698f-4d50-8fc1-591ad456175d
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/8/2017
ms.author: renash
ms.openlocfilehash: ed6ba9b98f121d6629d858320ca3760384303ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-file-storage-with-linux"></a>Använda Azure File storage med Linux
[Azure File storage](storage-dotnet-how-to-use-files.md) är Microsofts enkelt toouse moln-filsystem. Azure-filresurser kan monteras i Linux-distributioner med hello [cifs-verktyg för webbplatsuppgradering paketet](https://wiki.samba.org/index.php/LinuxCIFS_utils) från hello [Samba projekt](https://www.samba.org/). Den här artikeln beskrivs två sätt toomount en Azure-filresurs: på begäran med hello `mount` kommandot och på Start genom att skapa en post i `/etc/fstab`.

> [!NOTE]  
> I ordning toomount en Azure-filresurs utanför hello stöder Azure-region som den är värd för, till exempel lokalt eller i en annan Azure region hello OS hello kryptering funktionerna i SMB 3.0. Krypteringsfunktionerna för SMB 3.0 för Linux introducerades i 4.11 kernel. Den här funktionen gör det möjligt för montering av Azure-filresursen från lokala eller en annan Azure-region. När hello publicering har den här funktionen anpassats tooUbuntu från 16.04 och senare.


## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-hello-cifs-utils-package"></a>Prerequisities för att montera en filresurs på Azure med Linux- och hello cifs-verktyg för webbplatsuppgradering-paket
* **Välj en Linux-distributionsplats som kan ha hello cifs-verktyg för webbplatsuppgradering package installerad**: rekommenderar Microsoft hello följande Linux-distributioner i hello Azure-avbildning galleriet:

    * Ubuntu Server 14.04 +
    * RHEL 7 +
    * CentOS 7 +
    * Debian 8
    * openSUSE 13.2 +
    * SUSE Linux Enterprise Server 12

* <a id="install-cifs-utils"></a>**hello cifs-verktyg för webbplatsuppgradering paket installeras**: hello cifs-verktyg för webbplatsuppgradering kan installeras med hello Pakethanteraren på hello Linux distribution av ditt val. 

    På **Ubuntu** och **Debian-baserade** distributioner, använda hello `apt-get` Pakethanteraren:

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    På **RHEL** och **CentOS**, använda hello `yum` Pakethanteraren:

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    På **openSUSE**, använda hello `zypper` Pakethanteraren:

    ```
    sudo zypper install samba*
    ```

    Använd hello lämpliga Pakethanteraren på andra distributioner eller [kompilera från källan](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).

* **Besluta om hello directory/filbehörigheter hello monterade resursen**: I hello exemplen nedan, vi använda 0777, toogive läsa, skriva och köra behörigheter tooall användare. Du kan ersätta den med andra [chmod-behörigheter](https://en.wikipedia.org/wiki/Chmod) enligt önskemål. 

* **Lagringskontonamnet**: toomount ett Azure-filresurs, du behöver hello namnet på hello storage-konto.

* **Lagringskontonyckel**: toomount ett Azure-filresurs, du behöver hello primära (eller sekundära) lagringsnyckel. SAS-nycklar stöds inte för montering.

* **Kontrollera att port 445 är öppen**: SMB kommunicerar via TCP-port 445 - Kontrollera toosee om brandväggen inte blockerar TCP-port 445 från klientdatorn.

## <a name="mount-hello-azure-file-share-on-demand-with-mount"></a>Montera hello Azure-filresurs på begäran med`mount`
1. **[Installera hello cifs-verktyg för webbplatsuppgradering paketet för Linux-distribution](#install-cifs-utils)**.

2. **Skapa en mapp för hello monteringspunkt**: Detta kan göras var som helst i hello-filsystemet.

    ```
    mkdir mymountpoint
    ```

3. **Använd hello montera kommandot toomount hello Azure-filresurs**: spara tooreplace `<storage-account-name>`, `<share-name>`, och `<storage-account-key>` med hello rätt information.

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> När du är klar med hello Azure-filresursen, men du kan använda `sudo umount ./mymountpoint` toounmount hello resursen.

## <a name="create-a-persistent-mount-point-for-hello-azure-file-share-with-etcfstab"></a>Skapa en beständig monteringspunkt för hello Azure-filresurs med`/etc/fstab`
1. **[Installera hello cifs-verktyg för webbplatsuppgradering paketet för Linux-distribution](#install-cifs-utils)**.

2. **Skapa en mapp för hello monteringspunkt**: Detta kan göras var som helst i hello filsystemet, men du måste toonote hello absolut sökväg hello-mappen. hello följande exempel skapas en mapp under roten.

    ```
    sudo mkdir /mymountpoint
    ```

3. **Använd hello följande kommando tooappend hello följer raden för`/etc/fstab`**: spara tooreplace `<storage-account-name>`, `<share-name>`, och `<storage-account-key>` med hello rätt information.

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> Du kan använda `sudo mount -a` toomount hello Azure-filresurs när du har redigerat `/etc/fstab` i stället för att startas om.

## <a name="feedback"></a>Feedback
Linux-användare, och vi vill toohear från dig!

hello Azure File storage för Linux användargrupp ger ett forum du tooshare feedback när du utvärdera och anta File storage i Linux. E-post [Azure File storage Linux-användare](mailto:azurefileslinuxusers@microsoft.com) toojoin hello användargrupp.

## <a name="next-steps"></a>Nästa steg
Mer information om Azure File Storage finns på följande länkar.
* [File Service REST API referens](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [Använda Azure PowerShell med Azure storage](storage-powershell-guide-full.md)
* [Hur toouse AzCopy med Microsoft Azure storage](storage-use-azcopy.md)
* [Använda hello Azure CLI med Azure storage](storage-azure-cli.md#create-and-manage-file-shares)
* [Vanliga frågor och svar](storage-files-faq.md)
* [Felsökning](storage-troubleshoot-file-connection-problems.md)
