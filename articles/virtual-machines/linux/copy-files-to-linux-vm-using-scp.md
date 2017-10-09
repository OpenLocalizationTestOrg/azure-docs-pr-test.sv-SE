---
title: "aaaMove filer tooand från virtuella Azure Linux-datorer med SCP | Microsoft Docs"
description: "Flytta filer tooand på ett säkert sätt från en Linux VM i Azure med hjälp av SCP och en SSH-nyckel."
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
ms.openlocfilehash: 9e4dce667aa581df74aec3d45a6ba5e52f777d1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-files-tooand-from-a-linux-vm-using-scp"></a><span data-ttu-id="60750-103">Flytta filer tooand från en Linux VM som använder SCP</span><span class="sxs-lookup"><span data-stu-id="60750-103">Move files tooand from a Linux VM using SCP</span></span>

<span data-ttu-id="60750-104">Den här artikeln visar hur toomove filer från din arbetsstation in tooan virtuella Azure Linux-datorn, eller från en Azure Linux VM ned tooyour arbetsstation, som använder säker kopia (SCP).</span><span class="sxs-lookup"><span data-stu-id="60750-104">This article shows how toomove files from your workstation up tooan Azure Linux VM, or from an Azure Linux VM down tooyour workstation, using Secure Copy (SCP).</span></span> <span data-ttu-id="60750-105">Flytta filer mellan din arbetsstation och en Linux VM, snabbt och säkert är viktigt för att hantera dina Azure-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="60750-105">Moving files between your workstation and a Linux VM, quickly and securely, is critical for managing your Azure infrastructure.</span></span> 

<span data-ttu-id="60750-106">För den här artikeln behöver du Linux VM som distribuerats i Azure med hjälp av [SSH offentliga och privata nyckelfiler](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="60750-106">For this article, you need a Linux VM deployed in Azure using [SSH public and private key files](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="60750-107">Du behöver också en SCP-klient för den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="60750-107">You also need an SCP client for your local computer.</span></span> <span data-ttu-id="60750-108">Det är byggda på SSH och ingår i hello Bash standardgränssnittet för de flesta Linux och Mac-datorer och vissa Windows-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="60750-108">It is built on top of SSH and included in hello default Bash shell of most Linux and Mac computers and some Windows shells.</span></span>

## <a name="quick-commands"></a><span data-ttu-id="60750-109">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="60750-109">Quick commands</span></span>

<span data-ttu-id="60750-110">Kopiera en fil in toohello Linux VM</span><span class="sxs-lookup"><span data-stu-id="60750-110">Copy a file up toohello Linux VM</span></span>

```bash
scp file azureuser@azurehost:directory/targetfile
```

<span data-ttu-id="60750-111">Kopiera en fil från hello Linux VM</span><span class="sxs-lookup"><span data-stu-id="60750-111">Copy a file down from hello Linux VM</span></span>

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="60750-112">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="60750-112">Detailed walkthrough</span></span>

<span data-ttu-id="60750-113">Som exempel kan vi flytta en Azure-konfigurationsfil in tooa Linux VM och hämtar en loggfilskatalog både med SCP och SSH-nycklar.</span><span class="sxs-lookup"><span data-stu-id="60750-113">As examples, we move an Azure configuration file up tooa Linux VM and pull down a log file directory, both using SCP and SSH keys.</span></span>   

## <a name="ssh-key-pair-authentication"></a><span data-ttu-id="60750-114">SSH-nyckelpar autentisering</span><span class="sxs-lookup"><span data-stu-id="60750-114">SSH key pair authentication</span></span>

<span data-ttu-id="60750-115">SCP använder SSH för hello Transportskiktet.</span><span class="sxs-lookup"><span data-stu-id="60750-115">SCP uses SSH for hello transport layer.</span></span> <span data-ttu-id="60750-116">SSH handtag hello autentisering på målvärden för hello och flyttas hello-fil i en krypterad som standard med SSH-tunnel.</span><span class="sxs-lookup"><span data-stu-id="60750-116">SSH handles hello authentication on hello destination host, and it moves hello file in an encrypted tunnel provided by default with SSH.</span></span> <span data-ttu-id="60750-117">Användarnamn och lösenord kan användas för SSH-autentisering.</span><span class="sxs-lookup"><span data-stu-id="60750-117">For SSH authentication, usernames and passwords can be used.</span></span> <span data-ttu-id="60750-118">SSH offentliga och privata nycklar autentisering bör dock som en säkerhetsåtgärd.</span><span class="sxs-lookup"><span data-stu-id="60750-118">However, SSH public and private key authentication are recommended as a security best practice.</span></span> <span data-ttu-id="60750-119">När SSH har autentiserats hello anslutning börjar SCP sedan kopiera hello-filen.</span><span class="sxs-lookup"><span data-stu-id="60750-119">Once SSH has authenticated hello connection, SCP then begins copying hello file.</span></span> <span data-ttu-id="60750-120">Med en korrekt konfigurerad `~/.ssh/config` och SSH offentliga och privata nycklar, hello SCP-anslutning kan upprättas med bara ett servernamn (eller IP-adress).</span><span class="sxs-lookup"><span data-stu-id="60750-120">Using a properly configured `~/.ssh/config` and SSH public and private keys, hello SCP connection can be established by just using a server name (or IP address).</span></span> <span data-ttu-id="60750-121">Om du bara har en SSH-nyckeln SCP söker efter den i hello `~/.ssh/` directory, och använder som standard toolog i toohello VM.</span><span class="sxs-lookup"><span data-stu-id="60750-121">If you only have one SSH key, SCP looks for it in hello `~/.ssh/` directory, and uses it by default toolog in toohello VM.</span></span>

<span data-ttu-id="60750-122">Mer information om hur du konfigurerar din `~/.ssh/config` och offentliga och privata nycklar för SSH, se [skapa SSH-nycklar](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="60750-122">For more information on configuring your `~/.ssh/config` and SSH public and private keys, see [Create SSH keys](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="scp-a-file-tooa-linux-vm"></a><span data-ttu-id="60750-123">SCP fil-tooa Linux VM</span><span class="sxs-lookup"><span data-stu-id="60750-123">SCP a file tooa Linux VM</span></span>

<span data-ttu-id="60750-124">Exempelvis hello första kopiera vi en Azure-konfigurationsfil in tooa Linux VM som används toodeploy automation.</span><span class="sxs-lookup"><span data-stu-id="60750-124">For hello first example, we copy an Azure configuration file up tooa Linux VM that is used toodeploy automation.</span></span> <span data-ttu-id="60750-125">Säkerhet är viktigt eftersom den här filen innehåller autentiseringsuppgifter för Azure API, bland annat hemligheter.</span><span class="sxs-lookup"><span data-stu-id="60750-125">Because this file contains Azure API credentials, which include secrets, security is important.</span></span> <span data-ttu-id="60750-126">hello krypterade tunnel som tillhandahålls av SSH skyddar hello innehållet i hello-fil.</span><span class="sxs-lookup"><span data-stu-id="60750-126">hello encrypted tunnel provided by SSH protects hello contents of hello file.</span></span>

<span data-ttu-id="60750-127">Hej följande kommando kopior hello lokala *.azure/config* filen tooan virtuella Azure-datorn med FQDN *myserver.eastus.cloudapp.azure.com*. hello administratörsanvändarnamnet på hello Azure VM är *azureuser* .</span><span class="sxs-lookup"><span data-stu-id="60750-127">hello following command copies hello local *.azure/config* file tooan Azure VM with FQDN *myserver.eastus.cloudapp.azure.com*. hello admin user name on hello Azure VM is *azureuser*.</span></span> <span data-ttu-id="60750-128">hello-filen är riktade toohello */home/azureuser/* directory.</span><span class="sxs-lookup"><span data-stu-id="60750-128">hello file is targeted toohello */home/azureuser/* directory.</span></span> <span data-ttu-id="60750-129">Ersätt värdena i det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="60750-129">Substitute your own values in this command.</span></span>

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a><span data-ttu-id="60750-130">SCP en katalog Linux VM</span><span class="sxs-lookup"><span data-stu-id="60750-130">SCP a directory from a Linux VM</span></span>

<span data-ttu-id="60750-131">I det här exemplet kopierar vi en katalog med loggfiler från hello Linux VM ned tooyour arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="60750-131">For this example, we copy a directory of log files from hello Linux VM down tooyour workstation.</span></span> <span data-ttu-id="60750-132">En loggfil kan eller inte kan innehålla känsliga eller hemliga data.</span><span class="sxs-lookup"><span data-stu-id="60750-132">A log file may or may not contain sensitive or secret data.</span></span> <span data-ttu-id="60750-133">Med Tjänstanslutningspunkten innebär dock hello innehållet i loggfiler hello krypteras.</span><span class="sxs-lookup"><span data-stu-id="60750-133">However, using SCP ensures hello contents of hello log files are encrypted.</span></span> <span data-ttu-id="60750-134">Med hjälp av SCP tootransfer hello filer är hello enklaste sättet tooget hello log-katalogen och filerna ned tooyour arbetsstation samtidigt säker.</span><span class="sxs-lookup"><span data-stu-id="60750-134">Using SCP tootransfer hello files is hello easiest way tooget hello log directory and files down tooyour workstation while also being secure.</span></span>

<span data-ttu-id="60750-135">hello följande kommando kopierar filer i hello */home/azureuser/logs/* på hello Azure VM toohello lokala tmp:</span><span class="sxs-lookup"><span data-stu-id="60750-135">hello following command copies files in hello */home/azureuser/logs/* directory on hello Azure VM toohello local /tmp directory:</span></span>

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

<span data-ttu-id="60750-136">Hej `-r` cli flaggan instruerar SCP toorecursively kopiera hello filer och kataloger från hello punkt hello katalogen som anges i hello-kommandot.</span><span class="sxs-lookup"><span data-stu-id="60750-136">hello `-r` cli flag instructs SCP toorecursively copy hello files and directories from hello point of hello directory listed in hello command.</span></span>  <span data-ttu-id="60750-137">Observera att hello kommandoradssyntaxen är också liknande tooa `cp` kopiera kommandot.</span><span class="sxs-lookup"><span data-stu-id="60750-137">Also notice that hello command-line syntax is similar tooa `cp` copy command.</span></span>

## <a name="next-steps"></a><span data-ttu-id="60750-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="60750-138">Next steps</span></span>

* [<span data-ttu-id="60750-139">Hantera användare, SSH och kontrollera eller reparera diskar på virtuella Azure Linux-datorer med hjälp av hello VMAccess-tillägget</span><span class="sxs-lookup"><span data-stu-id="60750-139">Manage users, SSH, and check or repair disks on Azure Linux VMs using hello VMAccess Extension</span></span>](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
