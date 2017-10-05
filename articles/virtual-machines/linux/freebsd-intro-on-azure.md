---
title: Introduktion till FreeBSD i Azure | Microsoft Docs
description: "Lär dig mer om hur du använder FreeBSD-virtuella datorer på Azure"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 32b87a5f-d024-4da0-8bf0-77e233d1422b
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: kyliel
ms.openlocfilehash: 7ada9fddd7ffccc3dcbfe3eac05d99b710b67cbc
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="introduction-to-freebsd-on-azure"></a><span data-ttu-id="7fc79-103">Introduktion till FreeBSD på Azure</span><span class="sxs-lookup"><span data-stu-id="7fc79-103">Introduction to FreeBSD on Azure</span></span>
<span data-ttu-id="7fc79-104">Det här avsnittet innehåller en översikt över en FreeBSD virtuell dator som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="7fc79-104">This topic provides an overview of running a FreeBSD virtual machine in Azure.</span></span>

## <a name="overview"></a><span data-ttu-id="7fc79-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="7fc79-105">Overview</span></span>
<span data-ttu-id="7fc79-106">FreeBSD för Microsoft Azure är en avancerad operativsystem används för att power moderna servrar, stationära datorer, och inbäddade plattformar.</span><span class="sxs-lookup"><span data-stu-id="7fc79-106">FreeBSD for Microsoft Azure is an advanced computer operating system used to power modern servers, desktops, and embedded platforms.</span></span>

<span data-ttu-id="7fc79-107">Microsoft Corporation gör avbildningar av FreeBSD tillgängligt på Azure med den [Azure VM-Gästagent](https://github.com/Azure/WALinuxAgent/) förkonfigurerade.</span><span class="sxs-lookup"><span data-stu-id="7fc79-107">Microsoft Corporation is making images of FreeBSD available on Azure with the [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) pre-configured.</span></span> <span data-ttu-id="7fc79-108">För närvarande erbjuds följande FreeBSD-versioner som avbildningar av Microsoft:</span><span class="sxs-lookup"><span data-stu-id="7fc79-108">Currently, the following FreeBSD versions are offered as images by Microsoft:</span></span>

- <span data-ttu-id="7fc79-109">FreeBSD 10.3-versionen</span><span class="sxs-lookup"><span data-stu-id="7fc79-109">FreeBSD 10.3-RELEASE</span></span>
- <span data-ttu-id="7fc79-110">FreeBSD 11.0-versionen</span><span class="sxs-lookup"><span data-stu-id="7fc79-110">FreeBSD 11.0-RELEASE</span></span>

<span data-ttu-id="7fc79-111">Agenten är ansvarig för kommunikation mellan FreeBSD VM och Azure-strukturen för till exempel etablering av den virtuella datorn vid första användning (användarnamn, lösenord eller SSH-nyckel, värdnamn, etc.) och aktiverar funktioner för selektiv VM-tillägg.</span><span class="sxs-lookup"><span data-stu-id="7fc79-111">The agent is responsible for communication between the FreeBSD VM and the Azure fabric for operations such as provisioning the VM on first use (user name, password or SSH key, host name, etc.) and enabling functionality for selective VM extensions.</span></span>

<span data-ttu-id="7fc79-112">För framtida versioner av FreeBSD är strategin att hålla dig informerad om de senaste versionerna tillgängliga strax efter att de har publicerats av FreeBSD versionen teknikteamet.</span><span class="sxs-lookup"><span data-stu-id="7fc79-112">As for future versions of FreeBSD, the strategy is to stay current and make the latest releases available shortly after they are published by the FreeBSD release engineering team.</span></span>

## <a name="deploying-a-freebsd-virtual-machine"></a><span data-ttu-id="7fc79-113">Distribuera en virtuell dator FreeBSD</span><span class="sxs-lookup"><span data-stu-id="7fc79-113">Deploying a FreeBSD virtual machine</span></span>
<span data-ttu-id="7fc79-114">Distribuera en virtuell dator FreeBSD är enkelt att använda en avbildning från Azure Marketplace från Azure portal:</span><span class="sxs-lookup"><span data-stu-id="7fc79-114">Deploying a FreeBSD virtual machine is a straightforward process using an image from the Azure Marketplace from the Azure portal:</span></span>

- [<span data-ttu-id="7fc79-115">FreeBSD 10.3 på Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="7fc79-115">FreeBSD 10.3 on the Azure Marketplace</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
- [<span data-ttu-id="7fc79-116">FreeBSD 11.0 på Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="7fc79-116">FreeBSD 11.0 on the Azure Marketplace</span></span>](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/)

### <a name="create-a-freebsd-vm-through-azure-cli-20-on-freebsd"></a><span data-ttu-id="7fc79-117">Skapa en FreeBSD-VM via Azure CLI 2.0 på FreeBSD</span><span class="sxs-lookup"><span data-stu-id="7fc79-117">Create a FreeBSD VM through Azure CLI 2.0 on FreeBSD</span></span>
<span data-ttu-id="7fc79-118">Du måste först installera [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) trots att följande kommando på en FreeBSD-dator.</span><span class="sxs-lookup"><span data-stu-id="7fc79-118">First you need to install [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) though following command on a FreeBSD machine.</span></span>

```bash 
curl -L https://aka.ms/InstallAzureCli | bash
```

<span data-ttu-id="7fc79-119">Om bash inte är installerat på datorn FreeBSD, kör du följande kommando innan installationen.</span><span class="sxs-lookup"><span data-stu-id="7fc79-119">If bash is not installed on your FreeBSD machine, run following command before the installation.</span></span> 

```bash
sudo pkg install bash
```

<span data-ttu-id="7fc79-120">Om python inte är installerat på datorn FreeBSD, kör du följande kommandon innan installationen.</span><span class="sxs-lookup"><span data-stu-id="7fc79-120">If python is not installed on your FreeBSD machine, run following commands before the installation.</span></span> 

```bash
sudo pkg install python35
cd /usr/local/bin 
sudo rm /usr/local/bin/python 
sudo ln -s /usr/local/bin/python3.5 /usr/local/bin/python
```

<span data-ttu-id="7fc79-121">Under installationen uppmanas du `Modify profile to update your $PATH and enable shell/tab completion now? (Y/n)`.</span><span class="sxs-lookup"><span data-stu-id="7fc79-121">During the installation, you are asked `Modify profile to update your $PATH and enable shell/tab completion now? (Y/n)`.</span></span> <span data-ttu-id="7fc79-122">Om du svarar `y` och ange `/etc/rc.conf` som `a path to an rc file to update`, du kan uppfylla problemet `ERROR: [Errno 13] Permission denied`.</span><span class="sxs-lookup"><span data-stu-id="7fc79-122">If you answer `y` and enter `/etc/rc.conf` as `a path to an rc file to update`, you may meet the problem `ERROR: [Errno 13] Permission denied`.</span></span> <span data-ttu-id="7fc79-123">Lös problemet, bör du bevilja skrivningen till aktuell användare mot filen `etc/rc.conf`.</span><span class="sxs-lookup"><span data-stu-id="7fc79-123">To resolve this problem, you should grant the write right to current user against the file `etc/rc.conf`.</span></span>

<span data-ttu-id="7fc79-124">Du kan nu logga in i Azure och skapa din FreeBSD VM.</span><span class="sxs-lookup"><span data-stu-id="7fc79-124">Now you can log in Azure and create your FreeBSD VM.</span></span> <span data-ttu-id="7fc79-125">Nedan visas ett exempel för att skapa en virtuell dator 11.0 FreeBSD.</span><span class="sxs-lookup"><span data-stu-id="7fc79-125">Below is an example to create a FreeBSD 11.0 VM.</span></span> <span data-ttu-id="7fc79-126">Du kan också lägga till parametern `--public-ip-address-dns-name` med ett globalt unikt DNS-namn för en nyligen skapad offentliga IP-adress.</span><span class="sxs-lookup"><span data-stu-id="7fc79-126">You can also add the parameter `--public-ip-address-dns-name` with a globally unique DNS name for a newly created Public IP.</span></span> 

```azurecli
az login 
az group create --name myResourceGroup --location eastus
az vm create --name myFreeBSD11 \
    --resource-group myResourceGroup \
    --image MicrosoftOSTC:FreeBSD:11.0:latest \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="7fc79-127">Sedan kan du logga in till din FreeBSD-VM via ip-adressen som skrivs ut i ovanstående distribution utdata.</span><span class="sxs-lookup"><span data-stu-id="7fc79-127">Then you can log in to your FreeBSD VM through the ip address that printed in the output of above deployment.</span></span> 

```bash
ssh azureuser@xx.xx.xx.xx -i /etc/ssh/ssh_host_rsa_key
```   

## <a name="vm-extensions-for-freebsd"></a><span data-ttu-id="7fc79-128">VM-tillägg för FreeBSD</span><span class="sxs-lookup"><span data-stu-id="7fc79-128">VM extensions for FreeBSD</span></span>
<span data-ttu-id="7fc79-129">Följande är VM-tillägg som stöds i FreeBSD.</span><span class="sxs-lookup"><span data-stu-id="7fc79-129">Following are supported VM extensions in FreeBSD.</span></span>

### <a name="vmaccess"></a><span data-ttu-id="7fc79-130">VMAccess</span><span class="sxs-lookup"><span data-stu-id="7fc79-130">VMAccess</span></span>
<span data-ttu-id="7fc79-131">Den [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) tillägg kan:</span><span class="sxs-lookup"><span data-stu-id="7fc79-131">The [VMAccess](https://github.com/Azure/azure-linux-extensions/tree/master/VMAccess) extension can:</span></span>

* <span data-ttu-id="7fc79-132">Återställa lösenordet för den ursprungliga sudo-användaren.</span><span class="sxs-lookup"><span data-stu-id="7fc79-132">Reset the password of the original sudo user.</span></span>
* <span data-ttu-id="7fc79-133">Skapa en ny sudo-användare med det angivna lösenordet.</span><span class="sxs-lookup"><span data-stu-id="7fc79-133">Create a new sudo user with the password specified.</span></span>
* <span data-ttu-id="7fc79-134">Ange den offentliga värdnyckeln med nyckel anges.</span><span class="sxs-lookup"><span data-stu-id="7fc79-134">Set the public host key with the key given.</span></span>
* <span data-ttu-id="7fc79-135">Återställa nyckeln offentliga värden som angavs under VM etablering om värdnyckeln inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="7fc79-135">Reset the public host key provided during VM provisioning if the host key is not provided.</span></span>
* <span data-ttu-id="7fc79-136">Öppna SSH-port (22) och återställa sshd_config om reset_ssh har angetts till true.</span><span class="sxs-lookup"><span data-stu-id="7fc79-136">Open the SSH port (22) and restore the sshd_config if reset_ssh is set to true.</span></span>
* <span data-ttu-id="7fc79-137">Ta bort befintliga användare.</span><span class="sxs-lookup"><span data-stu-id="7fc79-137">Remove the existing user.</span></span>
* <span data-ttu-id="7fc79-138">Kontrollera diskar.</span><span class="sxs-lookup"><span data-stu-id="7fc79-138">Check disks.</span></span>
* <span data-ttu-id="7fc79-139">Reparera en lagts till disk.</span><span class="sxs-lookup"><span data-stu-id="7fc79-139">Repair an added disk.</span></span>

### <a name="customscript"></a><span data-ttu-id="7fc79-140">CustomScript</span><span class="sxs-lookup"><span data-stu-id="7fc79-140">CustomScript</span></span>
<span data-ttu-id="7fc79-141">Den [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) tillägg kan:</span><span class="sxs-lookup"><span data-stu-id="7fc79-141">The [CustomScript](https://github.com/Azure/azure-linux-extensions/tree/master/CustomScript) extension can:</span></span>

* <span data-ttu-id="7fc79-142">Om du hämta anpassade skript från Azure Storage eller externa offentlig lagring (till exempel GitHub).</span><span class="sxs-lookup"><span data-stu-id="7fc79-142">If provided, download the customized scripts from Azure Storage or external public storage (for example, GitHub).</span></span>
* <span data-ttu-id="7fc79-143">Kör skriptet post punkt.</span><span class="sxs-lookup"><span data-stu-id="7fc79-143">Run the entry point script.</span></span>
* <span data-ttu-id="7fc79-144">Stöd för infogade kommandon.</span><span class="sxs-lookup"><span data-stu-id="7fc79-144">Support inline commands.</span></span>
* <span data-ttu-id="7fc79-145">Konvertera automatiskt Windows-typ ny rad i gränssnittet och Python-skript.</span><span class="sxs-lookup"><span data-stu-id="7fc79-145">Convert Windows-style newline in shell and Python scripts automatically.</span></span>
* <span data-ttu-id="7fc79-146">Ta bort BOM i gränssnittet och Python skript automatiskt.</span><span class="sxs-lookup"><span data-stu-id="7fc79-146">Remove BOM in shell and Python scripts automatically.</span></span>
* <span data-ttu-id="7fc79-147">Skydda känsliga data i CommandToExecute.</span><span class="sxs-lookup"><span data-stu-id="7fc79-147">Protect sensitive data in CommandToExecute.</span></span>

> [!NOTE]
> <span data-ttu-id="7fc79-148">FreeBSD VM stöder endast CustomScript version 1.x nu.</span><span class="sxs-lookup"><span data-stu-id="7fc79-148">FreeBSD VM only supports CustomScript version 1.x by now.</span></span>  

## <a name="authentication-user-names-passwords-and-ssh-keys"></a><span data-ttu-id="7fc79-149">Autentisering: användarnamn, lösenord och SSH-nycklar</span><span class="sxs-lookup"><span data-stu-id="7fc79-149">Authentication: user names, passwords, and SSH keys</span></span>
<span data-ttu-id="7fc79-150">När du skapar en FreeBSD virtuell dator med hjälp av Azure portal, måste du ange ett användarnamn, lösenord eller offentlig SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="7fc79-150">When you're creating a FreeBSD virtual machine by using the Azure portal, you must provide a user name, password, or SSH public key.</span></span>
<span data-ttu-id="7fc79-151">Användarnamn för att distribuera en FreeBSD virtuell dator på Azure måste inte matcha namnen på Systemkonton (UID < 100) finns redan i den virtuella datorn (”rot”, till exempel).</span><span class="sxs-lookup"><span data-stu-id="7fc79-151">User names for deploying a FreeBSD virtual machine on Azure must not match names of system accounts (UID <100) already present in the virtual machine ("root", for example).</span></span>
<span data-ttu-id="7fc79-152">För närvarande stöds endast den RSA SSH-nyckeln.</span><span class="sxs-lookup"><span data-stu-id="7fc79-152">Currently, only the RSA SSH key is supported.</span></span> <span data-ttu-id="7fc79-153">Flerradig SSH-nyckeln måste börja med `---- BEGIN SSH2 PUBLIC KEY ----` och sluta med `---- END SSH2 PUBLIC KEY ----`.</span><span class="sxs-lookup"><span data-stu-id="7fc79-153">A multiline SSH key must begin with `---- BEGIN SSH2 PUBLIC KEY ----` and end with `---- END SSH2 PUBLIC KEY ----`.</span></span>

## <a name="obtaining-superuser-privileges"></a><span data-ttu-id="7fc79-154">Hämta superanvändare</span><span class="sxs-lookup"><span data-stu-id="7fc79-154">Obtaining superuser privileges</span></span>
<span data-ttu-id="7fc79-155">Det användarkonto som anges under distributionen av virtuella datorer instans i Azure är ett konto med privilegier.</span><span class="sxs-lookup"><span data-stu-id="7fc79-155">The user account that is specified during virtual machine instance deployment on Azure is a privileged account.</span></span> <span data-ttu-id="7fc79-156">Paket med sudo har installerats i publicerade FreeBSD-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="7fc79-156">The package of sudo was installed in the published FreeBSD image.</span></span>
<span data-ttu-id="7fc79-157">När du är inloggad med det här användarkontot, kan du köra kommandon som rot med hjälp av kommandosyntax.</span><span class="sxs-lookup"><span data-stu-id="7fc79-157">After you're logged in through this user account, you can run commands as root by using the command syntax.</span></span>

```
$ sudo <COMMAND>
```

<span data-ttu-id="7fc79-158">Du kan du hämta ett rot-gränssnitt med hjälp av `sudo -s`.</span><span class="sxs-lookup"><span data-stu-id="7fc79-158">You can optionally obtain a root shell by using `sudo -s`.</span></span>

## <a name="known-issues"></a><span data-ttu-id="7fc79-159">Kända problem</span><span class="sxs-lookup"><span data-stu-id="7fc79-159">Known issues</span></span>
<span data-ttu-id="7fc79-160">Den [Azure VM-Gästagent](https://github.com/Azure/WALinuxAgent/) version 2.2.2 har [känt problem] (https://github.com/Azure/WALinuxAgent/pull/517) som orsakar felet etablera för FreeBSD VM på Azure.</span><span class="sxs-lookup"><span data-stu-id="7fc79-160">The [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.2 has a [known issue] (https://github.com/Azure/WALinuxAgent/pull/517) that causes the provision failure for FreeBSD VM on Azure.</span></span> <span data-ttu-id="7fc79-161">Korrigering inhämtats via [Azure VM-Gästagent](https://github.com/Azure/WALinuxAgent/) version 2.2.3 och senare versioner.</span><span class="sxs-lookup"><span data-stu-id="7fc79-161">The fix was captured by [Azure VM Guest Agent](https://github.com/Azure/WALinuxAgent/) version 2.2.3 and later releases.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="7fc79-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7fc79-162">Next steps</span></span>
* <span data-ttu-id="7fc79-163">Gå till [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) att skapa en FreeBSD VM.</span><span class="sxs-lookup"><span data-stu-id="7fc79-163">Go to [Azure Marketplace](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd110/) to create a FreeBSD VM.</span></span>
* <span data-ttu-id="7fc79-164">Om du vill ta med din egen FreeBSD till Azure, se [skapa och ladda upp en FreeBSD VHD till Azure](classic/freebsd-create-upload-vhd.md).</span><span class="sxs-lookup"><span data-stu-id="7fc79-164">If you want to bring your own FreeBSD to Azure, refer to [Create and upload a FreeBSD VHD to Azure](classic/freebsd-create-upload-vhd.md).</span></span>
