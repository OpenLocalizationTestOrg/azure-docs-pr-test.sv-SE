---
title: "Flytta filer till och från virtuella Azure Linux-datorer med SCP | Microsoft Docs"
description: "Flytta filer på ett säkert sätt till och från en Linux VM i Azure med hjälp av SCP och en SSH-nyckel."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 736f7c11ec3de04f1ad52ee29d0a4c952c9b0545
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="move-files-to-and-from-a-linux-vm-using-scp"></a><span data-ttu-id="ecfa3-103">Flytta filer till och från en Linux VM som använder SCP</span><span class="sxs-lookup"><span data-stu-id="ecfa3-103">Move files to and from a Linux VM using SCP</span></span>

<span data-ttu-id="ecfa3-104">Den här artikeln visar hur du flyttar filerna från din arbetsstation upp till en Azure Linux-dator eller från en Azure virtuell Linux-dator till din arbetsstation med hjälp av Secure kopia (SCP).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-104">This article shows how to move files from your workstation up to an Azure Linux VM, or from an Azure Linux VM down to your workstation, using Secure Copy (SCP).</span></span> <span data-ttu-id="ecfa3-105">Flytta filer mellan din arbetsstation och en Linux VM, snabbt och säkert är viktigt för att hantera dina Azure-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-105">Moving files between your workstation and a Linux VM, quickly and securely, is critical for managing your Azure infrastructure.</span></span> 

<span data-ttu-id="ecfa3-106">För den här artikeln behöver du Linux VM som distribuerats i Azure med hjälp av [SSH offentliga och privata nyckelfiler](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-106">For this article, you need a Linux VM deployed in Azure using [SSH public and private key files](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="ecfa3-107">Du behöver också en SCP-klient för den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-107">You also need an SCP client for your local computer.</span></span> <span data-ttu-id="ecfa3-108">Det är byggda på SSH och ingår i Bash standardgränssnittet för de flesta Linux och Mac-datorer och vissa Windows-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-108">It is built on top of SSH and included in the default Bash shell of most Linux and Mac computers and some Windows shells.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="ecfa3-109">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="ecfa3-109">Quick commands</span></span>

<span data-ttu-id="ecfa3-110">Kopiera en fil till Linux VM</span><span class="sxs-lookup"><span data-stu-id="ecfa3-110">Copy a file up to the Linux VM</span></span>

```bash
scp file azureuser@azurehost:directory/targetfile
```

<span data-ttu-id="ecfa3-111">Kopiera en fil från Linux VM</span><span class="sxs-lookup"><span data-stu-id="ecfa3-111">Copy a file down from the Linux VM</span></span>

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="ecfa3-112">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="ecfa3-112">Detailed walkthrough</span></span>

<span data-ttu-id="ecfa3-113">Som exempel kan vi flytta en konfigurationsfil för Azure upp till en Linux VM och en loggfilskatalog nedrullningsbara både med SCP och SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-113">As examples, we move an Azure configuration file up to a Linux VM and pull down a log file directory, both using SCP and SSH keys.</span></span>   

## <a name="ssh-key-pair-authentication"></a><span data-ttu-id="ecfa3-114">SSH-nyckelpar autentisering</span><span class="sxs-lookup"><span data-stu-id="ecfa3-114">SSH key pair authentication</span></span>

<span data-ttu-id="ecfa3-115">SCP använder SSH för transportskiktet.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-115">SCP uses SSH for the transport layer.</span></span> <span data-ttu-id="ecfa3-116">SSH hanterar autentisering på målvärden och flyttas filen i en krypterad som standard med SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-116">SSH handles the authentication on the destination host, and it moves the file in an encrypted tunnel provided by default with SSH.</span></span> <span data-ttu-id="ecfa3-117">Användarnamn och lösenord kan användas för SSH-autentisering.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-117">For SSH authentication, usernames and passwords can be used.</span></span> <span data-ttu-id="ecfa3-118">SSH offentliga och privata nycklar autentisering bör dock som en säkerhetsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-118">However, SSH public and private key authentication are recommended as a security best practice.</span></span> <span data-ttu-id="ecfa3-119">När anslutningen har autentiserats SSH börjar SCP sedan filen.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-119">Once SSH has authenticated the connection, SCP then begins copying the file.</span></span> <span data-ttu-id="ecfa3-120">Med en korrekt konfigurerad `~/.ssh/config` och SSH offentliga och privata nycklar, SCP-anslutning kan upprättas med bara ett servernamn (eller IP-adress).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-120">Using a properly configured `~/.ssh/config` and SSH public and private keys, the SCP connection can be established by just using a server name (or IP address).</span></span> <span data-ttu-id="ecfa3-121">Om du bara har en SSH-nyckeln SCP söker efter den i den `~/.ssh/` directory, och används som standard för att logga in på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-121">If you only have one SSH key, SCP looks for it in the `~/.ssh/` directory, and uses it by default to log in to the VM.</span></span>

<span data-ttu-id="ecfa3-122">Mer information om hur du konfigurerar din `~/.ssh/config` och offentliga och privata nycklar för SSH, se [skapa SSH-nycklar](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ecfa3-122">For more information on configuring your `~/.ssh/config` and SSH public and private keys, see [Create SSH keys](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="scp-a-file-to-a-linux-vm"></a><span data-ttu-id="ecfa3-123">SCP en fil till en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="ecfa3-123">SCP a file to a Linux VM</span></span>

<span data-ttu-id="ecfa3-124">För det första exemplet kopierar vi en Azure-konfigurationsfil upp till en Linux VM som används för att distribuera automation.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-124">For the first example, we copy an Azure configuration file up to a Linux VM that is used to deploy automation.</span></span> <span data-ttu-id="ecfa3-125">Säkerhet är viktigt eftersom den här filen innehåller autentiseringsuppgifter för Azure API, bland annat hemligheter.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-125">Because this file contains Azure API credentials, which include secrets, security is important.</span></span> <span data-ttu-id="ecfa3-126">Krypterade tunnel som tillhandahålls av SSH skyddar innehållet i filen.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-126">The encrypted tunnel provided by SSH protects the contents of the file.</span></span>

<span data-ttu-id="ecfa3-127">Följande kommando kopierar du lokalt *.azure/config* filen till en virtuell Azure-dator med FQDN *myserver.eastus.cloudapp.azure.com*. Administratörsanvändarnamnet på Azure VM är *azureuser*.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-127">The following command copies the local *.azure/config* file to an Azure VM with FQDN *myserver.eastus.cloudapp.azure.com*. The admin user name on the Azure VM is *azureuser*.</span></span> <span data-ttu-id="ecfa3-128">Filen är riktad mot den */home/azureuser/* directory.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-128">The file is targeted to the */home/azureuser/* directory.</span></span> <span data-ttu-id="ecfa3-129">Ersätt värdena i det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-129">Substitute your own values in this command.</span></span>

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a><span data-ttu-id="ecfa3-130">SCP en katalog Linux VM</span><span class="sxs-lookup"><span data-stu-id="ecfa3-130">SCP a directory from a Linux VM</span></span>

<span data-ttu-id="ecfa3-131">I det här exemplet kopierar vi en katalog med loggfiler från Linux VM till din arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-131">For this example, we copy a directory of log files from the Linux VM down to your workstation.</span></span> <span data-ttu-id="ecfa3-132">En loggfil kan eller inte kan innehålla känsliga eller hemliga data.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-132">A log file may or may not contain sensitive or secret data.</span></span> <span data-ttu-id="ecfa3-133">Med Tjänstanslutningspunkten innebär dock innehållet i filerna krypteras.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-133">However, using SCP ensures the contents of the log files are encrypted.</span></span> <span data-ttu-id="ecfa3-134">Med hjälp av SCP för att överföra filer är det enklaste sättet att hämta loggkatalogen och filerna till din arbetsstation samtidigt säker.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-134">Using SCP to transfer the files is the easiest way to get the log directory and files down to your workstation while also being secure.</span></span>

<span data-ttu-id="ecfa3-135">Följande kommando kopierar filer i den */home/azureuser/logs/* på Virtuella Azure till lokala tmp:</span><span class="sxs-lookup"><span data-stu-id="ecfa3-135">The following command copies files in the */home/azureuser/logs/* directory on the Azure VM to the local /tmp directory:</span></span>

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

<span data-ttu-id="ecfa3-136">Den `-r` cli flaggan instruerar SCP till rekursivt kopiera filer och kataloger från punkten på katalogen som anges i kommandot.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-136">The `-r` cli flag instructs SCP to recursively copy the files and directories from the point of the directory listed in the command.</span></span>  <span data-ttu-id="ecfa3-137">Observera också att kommandoradssyntaxen liknar en `cp` kopiera kommandot.</span><span class="sxs-lookup"><span data-stu-id="ecfa3-137">Also notice that the command-line syntax is similar to a `cp` copy command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ecfa3-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ecfa3-138">Next steps</span></span>

* [<span data-ttu-id="ecfa3-139">Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Azure Linux-datorer med hjälp av VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="ecfa3-139">Manage users, SSH, and check or repair disks on Azure Linux VMs using the VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)