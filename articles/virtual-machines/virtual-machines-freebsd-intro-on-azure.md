---
title: aaaIntroduction tooFreeBSD i Azure | Microsoft Docs
description: "Lär dig mer om hur du använder FreeBSD-virtuella datorer på Azure"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: kyliel
ms.openlocfilehash: eab7aeda7f7ef893740b39c0250aacc29d6fd71b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toofreebsd-on-azure"></a><span data-ttu-id="2b07c-103">Introduktion tooFreeBSD på Azure</span><span class="sxs-lookup"><span data-stu-id="2b07c-103">Introduction tooFreeBSD on Azure</span></span>
<span data-ttu-id="2b07c-104">Det här avsnittet innehåller en översikt över en FreeBSD virtuell dator som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="2b07c-104">This topic provides an overview of running a FreeBSD virtual machine in Azure.</span></span>

## <a name="overview"></a><span data-ttu-id="2b07c-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="2b07c-105">Overview</span></span>
<span data-ttu-id="2b07c-106">FreeBSD för Microsoft Azure är en avancerad operativsystem används toopower moderna servrar, skrivbord och inbäddade plattformar.</span><span class="sxs-lookup"><span data-stu-id="2b07c-106">FreeBSD for Microsoft Azure is an advanced computer operating system used toopower modern servers, desktops, and embedded platforms.</span></span>

<span data-ttu-id="2b07c-107">Microsoft Corporation gör avbildningar av FreeBSD tillgängligt på Azure med hello [Azure VM-Gästagent](https://github.com/Azure/WALinuxAgent/) förkonfigurerade.</span><span class="sxs-lookup"><span data-stu-id="2b07c-107">Microsoft Corporation is making images of FreeBSD available on Azure with hello [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) pre-configured.</span></span> <span data-ttu-id="2b07c-108">För närvarande erbjuds hello följande FreeBSD versioner som avbildningar av Microsoft:</span><span class="sxs-lookup"><span data-stu-id="2b07c-108">Currently, hello following FreeBSD versions are offered as images by Microsoft:</span></span>

- <span data-ttu-id="2b07c-109">FreeBSD 10.3-versionen</span><span class="sxs-lookup"><span data-stu-id="2b07c-109">FreeBSD 10.3-RELEASE</span></span>
- <span data-ttu-id="2b07c-110">FreeBSD 11.0-versionen</span><span class="sxs-lookup"><span data-stu-id="2b07c-110">FreeBSD 11.0-RELEASE</span></span>

<span data-ttu-id="2b07c-111">hello-agenten är ansvarig för kommunikation mellan hello FreeBSD VM och hello Azure-strukturen för till exempel etablering hello VM på första användning (användarnamn, lösenord eller SSH-nyckel, värdnamn, etc.) och aktiverar funktioner för selektiv VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="2b07c-111">hello agent is responsible for communication between hello FreeBSD VM and hello Azure fabric for operations such as provisioning hello VM on first use (user name, password or SSH key, host name, etc.) and enabling functionality for selective VM extensions.</span></span>

<span data-ttu-id="2b07c-112">För framtida versioner av FreeBSD hello-strategi är toostay aktuella och tillgängliggöra hello senaste versioner strax efter att de har publicerats av hello FreeBSD versionen Utvecklingsteamet.</span><span class="sxs-lookup"><span data-stu-id="2b07c-112">As for future versions of FreeBSD, hello strategy is toostay current and make hello latest releases available shortly after they are published by hello FreeBSD release engineering team.</span></span>

## <a name="deploying-a-freebsd-virtual-machine"></a><span data-ttu-id="2b07c-113">Distribuera en virtuell dator FreeBSD</span><span class="sxs-lookup"><span data-stu-id="2b07c-113">Deploying a FreeBSD virtual machine</span></span>
<span data-ttu-id="2b07c-114">Distribuera en virtuell dator FreeBSD är enkelt att använda en bild från hello Azure Marketplace från hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="2b07c-114">Deploying a FreeBSD virtual machine is a straightforward process using an image from hello Azure Marketplace from hello Azure portal:</span></span>

- [<span data-ttu-id="2b07c-115">FreeBSD 10.3 på hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="2b07c-115">FreeBSD 10.3 on hello Azure Marketplace</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [<span data-ttu-id="2b07c-116">FreeBSD 11.0 på hello Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="2b07c-116">FreeBSD 11.0 on hello Azure Marketplace</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)

### <a name="create-a-freebsd-vm-through-azure-cli-20-on-freebsd"></a><span data-ttu-id="2b07c-117">Skapa en FreeBSD-VM via Azure CLI 2.0 på FreeBSD</span><span class="sxs-lookup"><span data-stu-id="2b07c-117">Create a FreeBSD VM through Azure CLI 2.0 on FreeBSD</span></span>
<span data-ttu-id="2b07c-118">Du måste först tooinstall [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) trots att följande kommando på en FreeBSD-dator.</span><span class="sxs-lookup"><span data-stu-id="2b07c-118">First you need tooinstall [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) though following command on a FreeBSD machine.</span></span>

```bash 
    curl -L https://aka.ms/InstallAzureCli | bash
```

<span data-ttu-id="2b07c-119">Om bash inte är installerat på datorn FreeBSD, kör du följande kommando innan hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="2b07c-119">If bash is not installed on your FreeBSD machine, run following command before hello installation.</span></span> 

```
    sudo pkg install bash
```

<span data-ttu-id="2b07c-120">Om python inte är installerat på datorn FreeBSD, kör du följande kommandon innan hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="2b07c-120">If python is not installed on your FreeBSD machine, run following commands before hello installation.</span></span> 

```
    sudo pkg install python35
    cd /usr/local/bin 
    sudo rm /usr/local/bin/python 
    sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

<span data-ttu-id="2b07c-121">Under installationen av hello uppmanas du `Modify profile tooupdate your $PATH and enable shell/tab completion now? (Y/n)`.</span><span class="sxs-lookup"><span data-stu-id="2b07c-121">During hello installation, you are asked `Modify profile tooupdate your $PATH and enable shell/tab completion now? (Y/n)`.</span></span> <span data-ttu-id="2b07c-122">Om du svarar `y` och ange `/etc/rc.conf` som `a path tooan rc file tooupdate`, du kan uppfylla hello problemet `ERROR: [Errno 13] Permission denied`.</span><span class="sxs-lookup"><span data-stu-id="2b07c-122">If you answer `y` and enter `/etc/rc.conf` as `a path tooan rc file tooupdate`, you may meet hello problem `ERROR: [Errno 13] Permission denied`.</span></span> <span data-ttu-id="2b07c-123">tooresolve det här problemet bör du bevilja hello skriva rätt toocurrent användaren mot hello filen `etc/rc.conf`.</span><span class="sxs-lookup"><span data-stu-id="2b07c-123">tooresolve this problem, you should grant hello write right toocurrent user against hello file `etc/rc.conf`.</span></span>

<span data-ttu-id="2b07c-124">Du kan nu logga in i Azure och skapa din FreeBSD VM.</span><span class="sxs-lookup"><span data-stu-id="2b07c-124">Now you can log in Azure and create your FreeBSD VM.</span></span> <span data-ttu-id="2b07c-125">Nedan visas ett exempel toocreate en virtuell dator 11.0 FreeBSD.</span><span class="sxs-lookup"><span data-stu-id="2b07c-125">Below is an example toocreate a FreeBSD 11.0 VM.</span></span> <span data-ttu-id="2b07c-126">Du kan också lägga till parametern hello `--public-ip-address-dns-name` med ett globalt unikt DNS-namn för en nyligen skapad offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="2b07c-126">You can also add hello parameter `--public-ip-address-dns-name` with a globally unique DNS name for a newly created Public IP.</span></span> 

```azurecli
    az login 
    az group create -n myResourceGroup -l westus az vm create -n myFreeBSD11 -g myResourceGroup --image MicrosoftOSTC:FreeBSD:11.0:latest --admin-username azureuser --ssh-key-value /etc/ssh/ssh_host_rsa_key.pub 
```

<span data-ttu-id="2b07c-127">Logga sedan in tooyour FreeBSD VM via hello ip-adress som skrivs ut i hello utdata från ovan distribution.</span><span class="sxs-lookup"><span data-stu-id="2b07c-127">Then you can log in tooyour FreeBSD VM through hello ip address that printed in hello output of above deployment.</span></span> 

```bash
    ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a><span data-ttu-id="2b07c-128">VM-tillägg för FreeBSD</span><span class="sxs-lookup"><span data-stu-id="2b07c-128">VM extensions for FreeBSD</span></span>
<span data-ttu-id="2b07c-129">Följande är VM-tillägg som stöds i FreeBSD.</span><span class="sxs-lookup"><span data-stu-id="2b07c-129">Following are supported VM extensions in FreeBSD.</span></span>

### <a name="vmaccess"></a><span data-ttu-id="2b07c-130">VMAccess</span><span class="sxs-lookup"><span data-stu-id="2b07c-130">VMAccess</span></span>
<span data-ttu-id="2b07c-131">Hej [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) tillägg kan:</span><span class="sxs-lookup"><span data-stu-id="2b07c-131">hello [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) extension can:</span></span>

* <span data-ttu-id="2b07c-132">Återställa hello lösenord på hello ursprungliga sudo användaren.</span><span class="sxs-lookup"><span data-stu-id="2b07c-132">Reset hello password of hello original sudo user.</span></span>
* <span data-ttu-id="2b07c-133">Skapa en ny sudo-användare med hello lösenord har angetts.</span><span class="sxs-lookup"><span data-stu-id="2b07c-133">Create a new sudo user with hello password specified.</span></span>
* <span data-ttu-id="2b07c-134">Ange hello offentliga värdnyckeln med hello nyckel anges.</span><span class="sxs-lookup"><span data-stu-id="2b07c-134">Set hello public host key with hello key given.</span></span>
* <span data-ttu-id="2b07c-135">Återställa hello offentliga värdnyckel som tillhandahålls under etablering om hello värdnyckel inte tillhandahålls av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="2b07c-135">Reset hello public host key provided during VM provisioning if hello host key is not provided.</span></span>
* <span data-ttu-id="2b07c-136">Öppna hello SSH-port (22) och återställa hello sshd_config om reset_ssh anges tootrue.</span><span class="sxs-lookup"><span data-stu-id="2b07c-136">Open hello SSH port (22) and restore hello sshd_config if reset_ssh is set tootrue.</span></span>
* <span data-ttu-id="2b07c-137">Ta bort hello befintliga användare.</span><span class="sxs-lookup"><span data-stu-id="2b07c-137">Remove hello existing user.</span></span>
* <span data-ttu-id="2b07c-138">Kontrollera diskar.</span><span class="sxs-lookup"><span data-stu-id="2b07c-138">Check disks.</span></span>
* <span data-ttu-id="2b07c-139">Reparera en lagts till disk.</span><span class="sxs-lookup"><span data-stu-id="2b07c-139">Repair an added disk.</span></span>

### <a name="customscript"></a><span data-ttu-id="2b07c-140">CustomScript</span><span class="sxs-lookup"><span data-stu-id="2b07c-140">CustomScript</span></span>
<span data-ttu-id="2b07c-141">Hej [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) tillägg kan:</span><span class="sxs-lookup"><span data-stu-id="2b07c-141">hello [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) extension can:</span></span>

* <span data-ttu-id="2b07c-142">Om hämta hello anpassade skript från Azure Storage eller externa offentlig lagring (till exempel GitHub).</span><span class="sxs-lookup"><span data-stu-id="2b07c-142">If provided, download hello customized scripts from Azure Storage or external public storage (for example, GitHub).</span></span>
* <span data-ttu-id="2b07c-143">Kör skriptet för hello post punkt.</span><span class="sxs-lookup"><span data-stu-id="2b07c-143">Run hello entry point script.</span></span>
* <span data-ttu-id="2b07c-144">Stöd för infogade kommandon.</span><span class="sxs-lookup"><span data-stu-id="2b07c-144">Support inline commands.</span></span>
* <span data-ttu-id="2b07c-145">Konvertera automatiskt Windows-typ ny rad i gränssnittet och Python-skript.</span><span class="sxs-lookup"><span data-stu-id="2b07c-145">Convert Windows-style newline in shell and Python scripts automatically.</span></span>
* <span data-ttu-id="2b07c-146">Ta bort BOM i gränssnittet och Python skript automatiskt.</span><span class="sxs-lookup"><span data-stu-id="2b07c-146">Remove BOM in shell and Python scripts automatically.</span></span>
* <span data-ttu-id="2b07c-147">Skydda känsliga data i CommandToExecute.</span><span class="sxs-lookup"><span data-stu-id="2b07c-147">Protect sensitive data in CommandToExecute.</span></span>

> [!NOTE]
> <span data-ttu-id="2b07c-148">FreeBSD VM stöder endast CustomScript version 1.x nu.</span><span class="sxs-lookup"><span data-stu-id="2b07c-148">FreeBSD VM only supports CustomScript version 1.x by now.</span></span>  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a><span data-ttu-id="2b07c-149">Autentisering: användarnamn, lösenord och SSH-nycklar</span><span class="sxs-lookup"><span data-stu-id="2b07c-149">Authentication: user names, passwords, and SSH keys</span></span>
<span data-ttu-id="2b07c-150">När du skapar en FreeBSD virtuell dator med hjälp av hello Azure-portalen, måste du ange ett användarnamn, lösenord eller offentlig SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="2b07c-150">When you're creating a FreeBSD virtual machine by using hello Azure portal, you must provide a user name, password, or SSH public key.</span></span>
<span data-ttu-id="2b07c-151">Användarnamn för att distribuera en FreeBSD virtuell dator på Azure måste inte matcha namnen på Systemkonton (UID < 100) finns redan i hello virtuell dator (”rot”, till exempel).</span><span class="sxs-lookup"><span data-stu-id="2b07c-151">User names for deploying a FreeBSD virtual machine on Azure must not match names of system accounts (UID <100) already present in hello virtual machine ("root", for example).</span></span>
<span data-ttu-id="2b07c-152">För närvarande stöds endast hello RSA SSH-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="2b07c-152">Currently, only hello RSA SSH key is supported.</span></span> <span data-ttu-id="2b07c-153">Flerradig SSH-nyckeln måste börja med `---- BEGIN SSH2 PUBLIC KEY ----` och sluta med `---- END SSH2 PUBLIC KEY ----`.</span><span class="sxs-lookup"><span data-stu-id="2b07c-153">A multiline SSH key must begin with `---- BEGIN SSH2 PUBLIC KEY ----` and end with `---- END SSH2 PUBLIC KEY ----`.</span></span>

## <a name="obtaining-superuser-privileges"></a><span data-ttu-id="2b07c-154">Hämta superanvändare</span><span class="sxs-lookup"><span data-stu-id="2b07c-154">Obtaining superuser privileges</span></span>
<span data-ttu-id="2b07c-155">hello-användarkonto som anges under distributionen av virtuella datorer instans i Azure är ett konto med privilegier.</span><span class="sxs-lookup"><span data-stu-id="2b07c-155">hello user account that is specified during virtual machine instance deployment on Azure is a privileged account.</span></span> <span data-ttu-id="2b07c-156">hello paket med sudo har installerats i hello publicerade FreeBSD-bild.</span><span class="sxs-lookup"><span data-stu-id="2b07c-156">hello package of sudo was installed in hello published FreeBSD image.</span></span>
<span data-ttu-id="2b07c-157">När du är inloggad med det här användarkontot, kan du köra kommandon som rot med hjälp av hello kommandosyntax.</span><span class="sxs-lookup"><span data-stu-id="2b07c-157">After you're logged in through this user account, you can run commands as root by using hello command syntax.</span></span>

```
    $ sudo <COMMAND>
```

<span data-ttu-id="2b07c-158">Du kan du hämta ett rot-gränssnitt med hjälp av `sudo -s`.</span><span class="sxs-lookup"><span data-stu-id="2b07c-158">You can optionally obtain a root shell by using `sudo -s`.</span></span>

## <a name="known-issues"></a><span data-ttu-id="2b07c-159">Kända problem</span><span class="sxs-lookup"><span data-stu-id="2b07c-159">Known issues</span></span>
<span data-ttu-id="2b07c-160">Hej [Azure VM-Gästagent](https://github.com/Azure/WALinuxAgent/) version 2.2.2 har [känt problem] (https://github.com/Azure/WALinuxAgent/pull/517) som orsakar hello etablera fel för FreeBSD VM på Azure.</span><span class="sxs-lookup"><span data-stu-id="2b07c-160">hello [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.2 has a [known issue] (https://github.com/Azure/WALinuxAgent/pull/517) that causes hello provision failure for FreeBSD VM on Azure.</span></span> <span data-ttu-id="2b07c-161">hello korrigering inhämtats via [Azure VM-Gästagent](https://github.com/Azure/WALinuxAgent/) version 2.2.3 och senare versioner.</span><span class="sxs-lookup"><span data-stu-id="2b07c-161">hello fix was captured by [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.3 and later releases.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="2b07c-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b07c-162">Next steps</span></span>
* <span data-ttu-id="2b07c-163">Gå för[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) toocreate en FreeBSD VM.</span><span class="sxs-lookup"><span data-stu-id="2b07c-163">Go too[Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) toocreate a FreeBSD VM.</span></span>
* <span data-ttu-id="2b07c-164">Om du vill toobring egna FreeBSD tooAzure finns för[skapa och ladda upp en FreeBSD VHD-tooAzure](linux/classic/freebsd-create-upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="2b07c-164">If you want toobring your own FreeBSD tooAzure, refer too[Create and upload a FreeBSD VHD tooAzure](linux/classic/freebsd-create-upload-vhd.md).</span></span>
