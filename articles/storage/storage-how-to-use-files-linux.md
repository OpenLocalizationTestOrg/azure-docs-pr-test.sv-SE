---
title: "Använda Azure File storage med Linux | Microsoft Docs"
description: "Lär dig mer om att montera en filresurs på Azure över SMB på Linux."
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
ms.openlocfilehash: 27b393a899c60a3a0393619f338a396dff659498
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-azure-file-storage-with-linux"></a><span data-ttu-id="0a00b-103">Använda Azure File storage med Linux</span><span class="sxs-lookup"><span data-stu-id="0a00b-103">Use Azure File storage with Linux</span></span>
<span data-ttu-id="0a00b-104">[Azure File Storage](storage-dotnet-how-to-use-files.md) är Microsofts lättanvända filsystem i molnet.</span><span class="sxs-lookup"><span data-stu-id="0a00b-104">[Azure File storage](storage-dotnet-how-to-use-files.md) is Microsoft's easy to use cloud file system.</span></span> <span data-ttu-id="0a00b-105">Azure-filresurser kan monteras i Linux-distributioner som använder den [cifs-verktyg för webbplatsuppgradering paketet](https://wiki.samba.org/index.php/LinuxCIFS_utils) från den [Samba projekt](https://www.samba.org/).</span><span class="sxs-lookup"><span data-stu-id="0a00b-105">Azure File shares can be mounted in Linux distributions using the [cifs-utils package](https://wiki.samba.org/index.php/LinuxCIFS_utils) from the [Samba project](https://www.samba.org/).</span></span> <span data-ttu-id="0a00b-106">Den här artikeln beskrivs två sätt att montera en filresurs i Azure: på begäran med den `mount` kommandot och på Start genom att skapa en post i `/etc/fstab`.</span><span class="sxs-lookup"><span data-stu-id="0a00b-106">This article shows two ways to mount an Azure File share: on-demand with the `mount` command and on-boot by creating an entry in `/etc/fstab`.</span></span>

> [!NOTE]  
> <span data-ttu-id="0a00b-107">För att montera en filresurs på Azure utanför Azure stöder region som den är värd för, till exempel lokalt eller i en annan Azure region Operativsystemet kryptering funktionerna i SMB 3.0.</span><span class="sxs-lookup"><span data-stu-id="0a00b-107">In order to mount an Azure File share outside of the Azure region it is hosted in, such as on-premises or in a different Azure region, the OS must support the encryption functionality of SMB 3.0.</span></span> <span data-ttu-id="0a00b-108">Krypteringsfunktionerna för SMB 3.0 för Linux introducerades i 4.11 kernel.</span><span class="sxs-lookup"><span data-stu-id="0a00b-108">Encryption feature for SMB 3.0 for Linux was introduced in 4.11 kernel.</span></span> <span data-ttu-id="0a00b-109">Den här funktionen gör det möjligt för montering av Azure-filresursen från lokala eller en annan Azure-region.</span><span class="sxs-lookup"><span data-stu-id="0a00b-109">This feature enables mounting of Azure File share from on-premises or a different Azure region.</span></span> <span data-ttu-id="0a00b-110">Vid tidpunkten för publicering har den här funktionen anpassats till Ubuntu från 16.04 och senare.</span><span class="sxs-lookup"><span data-stu-id="0a00b-110">At the time of publishing, this functionality has been backported to Ubuntu from 16.04 and above.</span></span>


## <a name="prerequisities-for-mounting-an-azure-file-share-with-linux-and-the-cifs-utils-package"></a><span data-ttu-id="0a00b-111">Prerequisities för att montera en Azure-fil dela med Linux- och cifs-verktyg för webbplatsuppgradering-paket</span><span class="sxs-lookup"><span data-stu-id="0a00b-111">Prerequisities for mounting an Azure File share with Linux and the cifs-utils package</span></span>
* <span data-ttu-id="0a00b-112">**Välj en Linux-distributionsplats som kan ha cifs-verktyg för webbplatsuppgradering paketet installeras**: Microsoft rekommenderar följande Linux-distributioner i Azure-avbildning galleriet:</span><span class="sxs-lookup"><span data-stu-id="0a00b-112">**Pick a Linux distribution that can have the cifs-utils package installed**: Microsoft recommends the following Linux distributions in the Azure image gallery:</span></span>

    * <span data-ttu-id="0a00b-113">Ubuntu Server 14.04 +</span><span class="sxs-lookup"><span data-stu-id="0a00b-113">Ubuntu Server 14.04+</span></span>
    * <span data-ttu-id="0a00b-114">RHEL 7 +</span><span class="sxs-lookup"><span data-stu-id="0a00b-114">RHEL 7+</span></span>
    * <span data-ttu-id="0a00b-115">CentOS 7 +</span><span class="sxs-lookup"><span data-stu-id="0a00b-115">CentOS 7+</span></span>
    * <span data-ttu-id="0a00b-116">Debian 8</span><span class="sxs-lookup"><span data-stu-id="0a00b-116">Debian 8</span></span>
    * <span data-ttu-id="0a00b-117">openSUSE 13.2 +</span><span class="sxs-lookup"><span data-stu-id="0a00b-117">openSUSE 13.2+</span></span>
    * <span data-ttu-id="0a00b-118">SUSE Linux Enterprise Server 12</span><span class="sxs-lookup"><span data-stu-id="0a00b-118">SUSE Linux Enterprise Server 12</span></span>

* <span data-ttu-id="0a00b-119"><a id="install-cifs-utils"></a>**Cifs-verktyg för webbplatsuppgradering paketet har installerats**: den cifs-verktyg för webbplatsuppgradering kan installeras med hjälp av package manager på Linux-distribution av ditt val.</span><span class="sxs-lookup"><span data-stu-id="0a00b-119"><a id="install-cifs-utils"></a>**The cifs-utils package is installed**: The cifs-utils can be installed using the package manager on the Linux distribution of your choice.</span></span> 

    <span data-ttu-id="0a00b-120">På **Ubuntu** och **Debian-baserade** distributioner, använda den `apt-get` Pakethanteraren:</span><span class="sxs-lookup"><span data-stu-id="0a00b-120">On **Ubuntu** and **Debian-based** distributions, use the `apt-get` package manager:</span></span>

    ```
    sudo apt-get update
    sudo apt-get install cifs-utils
    ```

    <span data-ttu-id="0a00b-121">På **RHEL** och **CentOS**, använda den `yum` Pakethanteraren:</span><span class="sxs-lookup"><span data-stu-id="0a00b-121">On **RHEL** and **CentOS**, use the `yum` package manager:</span></span>

    ```
    sudo yum install samba-client samba-common cifs-utils
    ```

    <span data-ttu-id="0a00b-122">På **openSUSE**, använda den `zypper` Pakethanteraren:</span><span class="sxs-lookup"><span data-stu-id="0a00b-122">On **openSUSE**, use the `zypper` package manager:</span></span>

    ```
    sudo zypper install samba*
    ```

    <span data-ttu-id="0a00b-123">Använd lämplig package manager på andra distributioner eller [kompilera från källan](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).</span><span class="sxs-lookup"><span data-stu-id="0a00b-123">On other distributions, use the appropriate package manager or [compile from source](https://wiki.samba.org/index.php/LinuxCIFS_utils#Download).</span></span>

* <span data-ttu-id="0a00b-124">**Besluta om filen/katalogen behörigheterna för den monterade resursen**: vi att använda 0777 i exemplen nedan, för att ge Läs-, Skriv- och körbehörighet till alla användare.</span><span class="sxs-lookup"><span data-stu-id="0a00b-124">**Decide on the directory/file permissions of the mounted share**: In the examples below, we use 0777, to give read, write, and execute permissions to all users.</span></span> <span data-ttu-id="0a00b-125">Du kan ersätta den med andra [chmod-behörigheter](https://en.wikipedia.org/wiki/Chmod) enligt önskemål.</span><span class="sxs-lookup"><span data-stu-id="0a00b-125">You can replace it with other [chmod permissions](https://en.wikipedia.org/wiki/Chmod) as desired.</span></span> 

* <span data-ttu-id="0a00b-126">**Lagringskontonamn**: Om du vill montera en Azure-filresurs behöver du namnet på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="0a00b-126">**Storage Account Name**: To mount an Azure File share, you will need the name of the storage account.</span></span>

* <span data-ttu-id="0a00b-127">**Lagringskontonyckel**: Om du vill montera en Azure-filresurs behöver du den primära (eller sekundära) lagringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="0a00b-127">**Storage Account Key**: To mount an Azure File share, you will need the primary (or secondary) storage key.</span></span> <span data-ttu-id="0a00b-128">SAS-nycklar stöds inte för montering.</span><span class="sxs-lookup"><span data-stu-id="0a00b-128">SAS keys are not currently supported for mounting.</span></span>

* <span data-ttu-id="0a00b-129">**Kontrollera att port 445 är öppen**: SMB kommunicerar via TCP-port 445 - Kontrollera att se om brandväggen inte blockerar TCP-portarna 445 från klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="0a00b-129">**Ensure port 445 is open**: SMB communicates over TCP port 445 - check to see if your firewall is not blocking TCP ports 445 from client machine.</span></span>

## <a name="mount-the-azure-file-share-on-demand-with-mount"></a><span data-ttu-id="0a00b-130">Montera filen Azure dela på begäran med`mount`</span><span class="sxs-lookup"><span data-stu-id="0a00b-130">Mount the Azure File share on-demand with `mount`</span></span>
1. <span data-ttu-id="0a00b-131">**[Installera paketet cifs-verktyg för webbplatsuppgradering för Linux-distribution](#install-cifs-utils)**.</span><span class="sxs-lookup"><span data-stu-id="0a00b-131">**[Install the cifs-utils package for your Linux distribution](#install-cifs-utils)**.</span></span>

2. <span data-ttu-id="0a00b-132">**Skapa en mapp för monteringspunkten**: Detta kan göras var som helst i filsystemet.</span><span class="sxs-lookup"><span data-stu-id="0a00b-132">**Create a folder for the mount point**: This can be done anywhere on the file system.</span></span>

    ```
    mkdir mymountpoint
    ```

3. <span data-ttu-id="0a00b-133">**Använd mount-kommando för att montera filresursen Azure**: Kom ihåg att ersätta `<storage-account-name>`, `<share-name>`, och `<storage-account-key>` med rätt information.</span><span class="sxs-lookup"><span data-stu-id="0a00b-133">**Use the mount command to mount the Azure File share**: Remember to replace `<storage-account-name>`, `<share-name>`, and `<storage-account-key>` with the proper information.</span></span>

    ```
    sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> ./mymountpoint -o vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino
    ```

> [!Note]  
> <span data-ttu-id="0a00b-134">När du är klar använder Azure-filresurs, kan du använda `sudo umount ./mymountpoint` demontera resursen.</span><span class="sxs-lookup"><span data-stu-id="0a00b-134">When you are done using the Azure File share, you may use `sudo umount ./mymountpoint` to unmount the share.</span></span>

## <a name="create-a-persistent-mount-point-for-the-azure-file-share-with-etcfstab"></a><span data-ttu-id="0a00b-135">Skapa en beständig monteringspunkt för Azure-filresursen med`/etc/fstab`</span><span class="sxs-lookup"><span data-stu-id="0a00b-135">Create a persistent mount point for the Azure File share with `/etc/fstab`</span></span>
1. <span data-ttu-id="0a00b-136">**[Installera paketet cifs-verktyg för webbplatsuppgradering för Linux-distribution](#install-cifs-utils)**.</span><span class="sxs-lookup"><span data-stu-id="0a00b-136">**[Install the cifs-utils package for your Linux distribution](#install-cifs-utils)**.</span></span>

2. <span data-ttu-id="0a00b-137">**Skapa en mapp för monteringspunkten**: Detta kan göras var som helst i filsystemet, men du behöver Observera den absoluta sökvägen till mappen.</span><span class="sxs-lookup"><span data-stu-id="0a00b-137">**Create a folder for the mount point**: This can be done anywhere on the file system, but you need to note the absolute path of the folder.</span></span> <span data-ttu-id="0a00b-138">I följande exempel skapas en mapp under roten.</span><span class="sxs-lookup"><span data-stu-id="0a00b-138">The following example creates a folder under root.</span></span>

    ```
    sudo mkdir /mymountpoint
    ```

3. <span data-ttu-id="0a00b-139">**Använd följande kommando för att lägga till följande rad `/etc/fstab`** : Kom ihåg att ersätta `<storage-account-name>`, `<share-name>`, och `<storage-account-key>` med rätt information.</span><span class="sxs-lookup"><span data-stu-id="0a00b-139">**Use the following command to append the following line to `/etc/fstab`**: Remember to replace `<storage-account-name>`, `<share-name>`, and `<storage-account-key>` with the proper information.</span></span>

    ```
    sudo bash -c 'echo "//<storage-account-name>.file.core.windows.net/<share-name> /mymountpoint cifs vers=3.0,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino" >> /etc/fstab'
    ```

> [!Note]  
> <span data-ttu-id="0a00b-140">Du kan använda `sudo mount -a` att montera filresursen Azure när du har redigerat `/etc/fstab` i stället för att startas om.</span><span class="sxs-lookup"><span data-stu-id="0a00b-140">You can use `sudo mount -a` to mount the Azure File share after editing `/etc/fstab` instead of rebooting.</span></span>

## <a name="feedback"></a><span data-ttu-id="0a00b-141">Feedback</span><span class="sxs-lookup"><span data-stu-id="0a00b-141">Feedback</span></span>
<span data-ttu-id="0a00b-142">Linux-användare som vi vill gärna höra av dig!</span><span class="sxs-lookup"><span data-stu-id="0a00b-142">Linux users, we want to hear from you!</span></span>

<span data-ttu-id="0a00b-143">Azure File storage för Linux användargrupp innehåller ett forum för att dela feedback när du utvärdera och anta File storage i Linux.</span><span class="sxs-lookup"><span data-stu-id="0a00b-143">The Azure File storage for Linux users' group provides a forum for you to share feedback as you evaluate and adopt File storage on Linux.</span></span> <span data-ttu-id="0a00b-144">E-post [Azure File storage Linux-användare](mailto:azurefileslinuxusers@microsoft.com) att ansluta till den användargruppen.</span><span class="sxs-lookup"><span data-stu-id="0a00b-144">Email [Azure File storage Linux Users](mailto:azurefileslinuxusers@microsoft.com) to join the users' group.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a00b-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0a00b-145">Next steps</span></span>
<span data-ttu-id="0a00b-146">Mer information om Azure File Storage finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="0a00b-146">See these links for more information about Azure File storage.</span></span>
* [<span data-ttu-id="0a00b-147">File Service REST API referens</span><span class="sxs-lookup"><span data-stu-id="0a00b-147">File Service REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dn167006.aspx)
* [<span data-ttu-id="0a00b-148">Använda Azure PowerShell med Azure storage</span><span class="sxs-lookup"><span data-stu-id="0a00b-148">Using Azure PowerShell with Azure storage</span></span>](storage-powershell-guide-full.md)
* [<span data-ttu-id="0a00b-149">Använda AzCopy med Microsoft Azure storage</span><span class="sxs-lookup"><span data-stu-id="0a00b-149">How to use AzCopy with Microsoft Azure storage</span></span>](storage-use-azcopy.md)
* [<span data-ttu-id="0a00b-150">Använda Azure CLI med Azure storage</span><span class="sxs-lookup"><span data-stu-id="0a00b-150">Using the Azure CLI with Azure storage</span></span>](storage-azure-cli.md#create-and-manage-file-shares)
* [<span data-ttu-id="0a00b-151">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="0a00b-151">FAQ</span></span>](storage-files-faq.md)
* [<span data-ttu-id="0a00b-152">Felsökning</span><span class="sxs-lookup"><span data-stu-id="0a00b-152">Troubleshooting</span></span>](storage-troubleshoot-file-connection-problems.md)
