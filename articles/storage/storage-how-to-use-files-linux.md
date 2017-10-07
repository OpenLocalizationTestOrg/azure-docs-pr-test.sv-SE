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
# <a name="use-azure-file-storage-with-linux"></a><span data-ttu-id="b9dcd-103">Använda Azure File storage med Linux</span><span class="sxs-lookup"><span data-stu-id="b9dcd-103">Use Azure File storage with Linux</span></span>
<span data-ttu-id="b9dcd-104">[Azure File storage](storage-dotnet-how-to-use-files.md) är Microsofts enkelt toouse moln-filsystem.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-104">[Azure File storage](storage-dotnet-how-to-use-files.md) is Microsoft's easy toouse cloud file system.</span></span> <span data-ttu-id="b9dcd-105">Azure-filresurser kan monteras i Linux-distributioner med hello [cifs-verktyg för webbplatsuppgradering paketet](https://wiki.samba.org/index.php/LinuxCIFS_utils) från hello [Samba projekt](https://www.samba.org/).</span><span class="sxs-lookup"><span data-stu-id="b9dcd-105">Azure File shares can be mounted in Linux distributions using hello [cifs-utils package](https://wiki.samba.org/index.php/LinuxCIFS_utils) from hello [Samba project](https://www.samba.org/).</span></span> <span data-ttu-id="b9dcd-106">Den här artikeln beskrivs två sätt toomount en Azure-filresurs: på begäran med hello `mount` kommandot och på Start genom att skapa en post i `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-106">This article shows two ways toomount an Azure File share: on-demand with hello `mount` command and on-boot by creating an entry in `/etc/fstab`.</span></span>

> [!NOTE]  
> <span data-ttu-id="b9dcd-107">I ordning toomount en Azure-filresurs utanför hello stöder Azure-region som den är värd för, till exempel lokalt eller i en annan Azure region hello OS hello kryptering funktionerna i SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-107">In order toomount an Azure File share outside of hello Azure region it is hosted in, such as on-premises or in a different Azure region, hello OS must support hello encryption functionality of SMB 3.0.</span></span> <span data-ttu-id="b9dcd-108">Krypteringsfunktionerna för SMB 3.0 för Linux introducerades i 4.11 kernel.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-108">Encryption feature for SMB 3.0 for Linux was introduced in 4.11 kernel.</span></span> <span data-ttu-id="b9dcd-109">Den här funktionen gör det möjligt för montering av Azure-filresursen från lokala eller en annan Azure-region.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-109">This feature enables mounting of Azure File share from on-premises or a different Azure region.</span></span> <span data-ttu-id="b9dcd-110">När hello publicering har den här funktionen anpassats tooUbuntu från 16.04 och senare.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-110">At hello time of publishing, this functionality has been backported tooUbuntu from 16.04 and above.</span></span>


## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-hello-cifs-utils-package"></a><span data-ttu-id="b9dcd-111">Prerequisities för att montera en filresurs på Azure med Linux- och hello cifs-verktyg för webbplatsuppgradering-paket</span><span class="sxs-lookup"><span data-stu-id="b9dcd-111">Prerequisities for mounting an Azure File share with Linux and hello cifs-utils package</span></span>
* <span data-ttu-id="b9dcd-112">**Välj en Linux-distributionsplats som kan ha hello cifs-verktyg för webbplatsuppgradering package installerad**: rekommenderar Microsoft hello följande Linux-distributioner i hello Azure-avbildning galleriet:</span><span class="sxs-lookup"><span data-stu-id="b9dcd-112">**Pick a Linux distribution that can have hello cifs-utils package installed**: Microsoft recommends hello following Linux distributions in hello Azure image gallery:</span></span>

    * <span data-ttu-id="b9dcd-113">Ubuntu Server 14.04 +</span><span class="sxs-lookup"><span data-stu-id="b9dcd-113">Ubuntu Server 14.04+</span></span>
    * <span data-ttu-id="b9dcd-114">RHEL 7 +</span><span class="sxs-lookup"><span data-stu-id="b9dcd-114">RHEL 7+</span></span>
    * <span data-ttu-id="b9dcd-115">CentOS 7 +</span><span class="sxs-lookup"><span data-stu-id="b9dcd-115">CentOS 7+</span></span>
    * <span data-ttu-id="b9dcd-116">Debian 8</span><span class="sxs-lookup"><span data-stu-id="b9dcd-116">Debian 8</span></span>
    * <span data-ttu-id="b9dcd-117">openSUSE 13.2 +</span><span class="sxs-lookup"><span data-stu-id="b9dcd-117">openSUSE 13.2+</span></span>
    * <span data-ttu-id="b9dcd-118">SUSE Linux Enterprise Server 12</span><span class="sxs-lookup"><span data-stu-id="b9dcd-118">SUSE Linux Enterprise Server 12</span></span>

* <span data-ttu-id="b9dcd-119"><a id="install-cifs-utils"></a>**hello cifs-verktyg för webbplatsuppgradering paket installeras**: hello cifs-verktyg för webbplatsuppgradering kan installeras med hello Pakethanteraren på hello Linux distribution av ditt val.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-119"><a id="install-cifs-utils"></a>**hello cifs-utils package is installed**: hello cifs-utils can be installed using hello package manager on hello Linux distribution of your choice.</span></span> 

    <span data-ttu-id="b9dcd-120">På **Ubuntu** och **Debian-baserade** distributioner, använda hello `apt-get` Pakethanteraren:</span><span class="sxs-lookup"><span data-stu-id="b9dcd-120">On **Ubuntu** and **Debian-based** distributions, use hello `apt-get` package manager:</span></span>

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    <span data-ttu-id="b9dcd-121">På **RHEL** och **CentOS**, använda hello `yum` Pakethanteraren:</span><span class="sxs-lookup"><span data-stu-id="b9dcd-121">On **RHEL** and **CentOS**, use hello `yum` package manager:</span></span>

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    <span data-ttu-id="b9dcd-122">På **openSUSE**, använda hello `zypper` Pakethanteraren:</span><span class="sxs-lookup"><span data-stu-id="b9dcd-122">On **openSUSE**, use hello `zypper` package manager:</span></span>

    ```
    sudo zypper install samba*
    ```

    <span data-ttu-id="b9dcd-123">Använd hello lämpliga Pakethanteraren på andra distributioner eller [kompilera från källan](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).</span><span class="sxs-lookup"><span data-stu-id="b9dcd-123">On other distributions, use hello appropriate package manager or [compile from source](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).</span></span>

* <span data-ttu-id="b9dcd-124">**Besluta om hello directory/filbehörigheter hello monterade resursen**: I hello exemplen nedan, vi använda 0777, toogive läsa, skriva och köra behörigheter tooall användare.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-124">**Decide on hello directory/file permissions of hello mounted share**: In hello examples below, we use 0777, toogive read, write, and execute permissions tooall users.</span></span> <span data-ttu-id="b9dcd-125">Du kan ersätta den med andra [chmod-behörigheter](https://en.wikipedia.org/wiki/Chmod) enligt önskemål.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-125">You can replace it with other [chmod permissions](https://en.wikipedia.org/wiki/Chmod) as desired.</span></span> 

* <span data-ttu-id="b9dcd-126">**Lagringskontonamnet**: toomount ett Azure-filresurs, du behöver hello namnet på hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-126">**Storage Account Name**: toomount an Azure File share, you will need hello name of hello storage account.</span></span>

* <span data-ttu-id="b9dcd-127">**Lagringskontonyckel**: toomount ett Azure-filresurs, du behöver hello primära (eller sekundära) lagringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-127">**Storage Account Key**: toomount an Azure File share, you will need hello primary (or secondary) storage key.</span></span> <span data-ttu-id="b9dcd-128">SAS-nycklar stöds inte för montering.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-128">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="b9dcd-129">**Kontrollera att port 445 är öppen**: SMB kommunicerar via TCP-port 445 - Kontrollera toosee om brandväggen inte blockerar TCP-port 445 från klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-129">**Ensure port 445 is open**: SMB communicates over TCP port 445 - check toosee if your firewall is not blocking TCP ports 445 from client machine.</span></span>

## <a name="mount-hello-azure-file-share-on-demand-with-mount"></a><span data-ttu-id="b9dcd-130">Montera hello Azure-filresurs på begäran med`mount`</span><span class="sxs-lookup"><span data-stu-id="b9dcd-130">Mount hello Azure File share on-demand with `mount`</span></span>
1. <span data-ttu-id="b9dcd-131">**[Installera hello cifs-verktyg för webbplatsuppgradering paketet för Linux-distribution](#install-cifs-utils)**.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-131">**[Install hello cifs-utils package for your Linux distribution](#install-cifs-utils)**.</span></span>

2. <span data-ttu-id="b9dcd-132">**Skapa en mapp för hello monteringspunkt**: Detta kan göras var som helst i hello-filsystemet.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-132">**Create a folder for hello mount point**: This can be done anywhere on hello file system.</span></span>

    ```
    mkdir mymountpoint
    ```

3. <span data-ttu-id="b9dcd-133">**Använd hello montera kommandot toomount hello Azure-filresurs**: spara tooreplace `<storage-account-name>`, `<share-name>`, och `<storage-account-key>` med hello rätt information.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-133">**Use hello mount command toomount hello Azure File share**: Remember tooreplace `<storage-account-name>`, `<share-name>`, and `<storage-account-key>` with hello proper information.</span></span>

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> <span data-ttu-id="b9dcd-134">När du är klar med hello Azure-filresursen, men du kan använda `sudo umount ./mymountpoint` toounmount hello resursen.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-134">When you are done using hello Azure File share, you may use `sudo umount ./mymountpoint` toounmount hello share.</span></span>

## <a name="create-a-persistent-mount-point-for-hello-azure-file-share-with-etcfstab"></a><span data-ttu-id="b9dcd-135">Skapa en beständig monteringspunkt för hello Azure-filresurs med`/etc/fstab`</span><span class="sxs-lookup"><span data-stu-id="b9dcd-135">Create a persistent mount point for hello Azure File share with `/etc/fstab`</span></span>
1. <span data-ttu-id="b9dcd-136">**[Installera hello cifs-verktyg för webbplatsuppgradering paketet för Linux-distribution](#install-cifs-utils)**.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-136">**[Install hello cifs-utils package for your Linux distribution](#install-cifs-utils)**.</span></span>

2. <span data-ttu-id="b9dcd-137">**Skapa en mapp för hello monteringspunkt**: Detta kan göras var som helst i hello filsystemet, men du måste toonote hello absolut sökväg hello-mappen.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-137">**Create a folder for hello mount point**: This can be done anywhere on hello file system, but you need toonote hello absolute path of hello folder.</span></span> <span data-ttu-id="b9dcd-138">hello följande exempel skapas en mapp under roten.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-138">hello following example creates a folder under root.</span></span>

    ```
    sudo mkdir /mymountpoint
    ```

3. <span data-ttu-id="b9dcd-139">**Använd hello följande kommando tooappend hello följer raden för`/etc/fstab`**: spara tooreplace `<storage-account-name>`, `<share-name>`, och `<storage-account-key>` med hello rätt information.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-139">**Use hello following command tooappend hello following line too`/etc/fstab`**: Remember tooreplace `<storage-account-name>`, `<share-name>`, and `<storage-account-key>` with hello proper information.</span></span>

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> <span data-ttu-id="b9dcd-140">Du kan använda `sudo mount -a` toomount hello Azure-filresurs när du har redigerat `/etc/fstab` i stället för att startas om.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-140">You can use `sudo mount -a` toomount hello Azure File share after editing `/etc/fstab` instead of rebooting.</span></span>

## <a name="feedback"></a><span data-ttu-id="b9dcd-141">Feedback</span><span class="sxs-lookup"><span data-stu-id="b9dcd-141">Feedback</span></span>
<span data-ttu-id="b9dcd-142">Linux-användare, och vi vill toohear från dig!</span><span class="sxs-lookup"><span data-stu-id="b9dcd-142">Linux users, we want toohear from you!</span></span>

<span data-ttu-id="b9dcd-143">hello Azure File storage för Linux användargrupp ger ett forum du tooshare feedback när du utvärdera och anta File storage i Linux.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-143">hello Azure File storage for Linux users' group provides a forum for you tooshare feedback as you evaluate and adopt File storage on Linux.</span></span> <span data-ttu-id="b9dcd-144">E-post [Azure File storage Linux-användare](mailto:azurefileslinuxusers@microsoft.com) toojoin hello användargrupp.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-144">Email [Azure File storage Linux Users](mailto:azurefileslinuxusers@microsoft.com) toojoin hello users' group.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b9dcd-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9dcd-145">Next steps</span></span>
<span data-ttu-id="b9dcd-146">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="b9dcd-146">See these links for more information about Azure File storage.</span></span>
* [<span data-ttu-id="b9dcd-147">File Service REST API referens</span><span class="sxs-lookup"><span data-stu-id="b9dcd-147">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [<span data-ttu-id="b9dcd-148">Använda Azure PowerShell med Azure storage</span><span class="sxs-lookup"><span data-stu-id="b9dcd-148">Using Azure PowerShell with Azure storage</span></span>](storage-powershell-guide-full.md)
* [<span data-ttu-id="b9dcd-149">Hur toouse AzCopy med Microsoft Azure storage</span><span class="sxs-lookup"><span data-stu-id="b9dcd-149">How toouse AzCopy with Microsoft Azure storage</span></span>](storage-use-azcopy.md)
* [<span data-ttu-id="b9dcd-150">Använda hello Azure CLI med Azure storage</span><span class="sxs-lookup"><span data-stu-id="b9dcd-150">Using hello Azure CLI with Azure storage</span></span>](storage-azure-cli.md#create-and-manage-file-shares)
* [<span data-ttu-id="b9dcd-151">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="b9dcd-151">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="b9dcd-152">Felsökning</span><span class="sxs-lookup"><span data-stu-id="b9dcd-152">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)
