---
title: "Skapa och använd ett SSH-nyckelpar för virtuella Linux-datorer i Azure | Microsoft Docs"
description: "Så här skapar och använder du ett offentligt och privat SSH-nyckelpar för virtuella Linux-datorer i Azure för att förbättra säkerheten för autentiseringsprocessen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 34ae9482-da3e-4b2d-9d0d-9d672aa42498
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2017
ms.author: iainfou
ms.openlocfilehash: 0fb71d2ffe533afba6e1e527b727a7b085e7da14
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-create-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a><span data-ttu-id="cfdee-103">Så här skapar du säkert ett offentligt och ett privat SSH-nyckelpar för virtuella Linux-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="cfdee-103">How to create and use an SSH public and private key pair for Linux VMs in Azure</span></span>
<span data-ttu-id="cfdee-104">Med ett SSH-nyckelpar kan du skapa virtuella datorer i Azure som använder SSH-nycklar för autentisering, vilket gör att inga lösenord krävs för att logga in.</span><span class="sxs-lookup"><span data-stu-id="cfdee-104">With a secure shell (SSH) key pair, you can create virtual machines (VMs) in Azure that use SSH keys for authentication, eliminating the need for passwords to log in.</span></span> <span data-ttu-id="cfdee-105">I den här artikeln visas hur du kan skapa ett RSA-nyckelfilpar med SSH-protokollversion 2 med en offentlig och en privat nyckel för virtuella Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="cfdee-105">This article shows you how to quickly generate and use an SSH protocol version 2 RSA public and private key file pair for Linux VMs.</span></span> <span data-ttu-id="cfdee-106">Mer detaljerade steg och ytterligare exempel finns i [detaljerade steg för att skapa SSH-nyckelpar och certifikat](create-ssh-keys-detailed.md).</span><span class="sxs-lookup"><span data-stu-id="cfdee-106">For more detailed steps and additional examples, see [detailed steps to create SSH key pairs and certificates](create-ssh-keys-detailed.md).</span></span>

## <a name="create-an-ssh-key-pair"></a><span data-ttu-id="cfdee-107">Skapa ett SSH-nyckelpar</span><span class="sxs-lookup"><span data-stu-id="cfdee-107">Create an SSH key pair</span></span>
<span data-ttu-id="cfdee-108">Använd kommandot `ssh-keygen` för att skapa offentliga och privata SSH-nyckelfiler som skapas i katalogen `~/.ssh` som standard. Du kan också ange en annan plats och ytterligare lösenfras (ett lösenord för att få åtkomst till den privata nyckelfilen) när du blir ombedd att göra det.</span><span class="sxs-lookup"><span data-stu-id="cfdee-108">Use the `ssh-keygen` command to create SSH public and private key files that are by default created in the `~/.ssh` directory, but you can specify a different location and additional passphrase (a password to access the private key file) when prompted.</span></span> <span data-ttu-id="cfdee-109">Kör följande kommando från ett Bash-kommando och svara på frågorna med din egen information.</span><span class="sxs-lookup"><span data-stu-id="cfdee-109">Run the following command from a Bash shell, answering the prompts with your own information.</span></span>

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-the-ssh-key-pair"></a><span data-ttu-id="cfdee-110">Använda SSH-nyckelpar</span><span class="sxs-lookup"><span data-stu-id="cfdee-110">Use the SSH key pair</span></span>
<span data-ttu-id="cfdee-111">Den offentliga nyckel du placerar i din virtuella Linux-dator i Azure lagras som standard i `~/.ssh/id_rsa.pub`, om du inte har ändrat plats när du skapade den.</span><span class="sxs-lookup"><span data-stu-id="cfdee-111">The public key that you place on your Linux VM in Azure is by default stored in `~/.ssh/id_rsa.pub`, unless you changed the location when you created them.</span></span> <span data-ttu-id="cfdee-112">Om du använder [Azure CLI 2.0](/cli/azure) när du skapar din virtuella dator anger du plats för den offentliga nyckeln när du använder [az vm create](/cli/azure/vm#create) med alternativet `--ssh-key-path`.</span><span class="sxs-lookup"><span data-stu-id="cfdee-112">If you use the [Azure CLI 2.0](/cli/azure) to create your VM, specify the location of this public key when you use the [az vm create](/cli/azure/vm#create) with the `--ssh-key-path` option.</span></span> <span data-ttu-id="cfdee-113">Om du kopierar och klistrar in innehållet i den offentliga nyckelfilen för att använda det i Azure Portal eller i en Resource Manager-mall ska du se till att inte kopiera några extra blanksteg.</span><span class="sxs-lookup"><span data-stu-id="cfdee-113">If you copy and paste the contents of the public key file to use in the Azure portal or a Resource Manager template, make sure you don't copy any additional whitespace.</span></span> <span data-ttu-id="cfdee-114">Om du exempelvis använder OS X kan du skicka den offentliga nyckelfilen (som standard **~/.ssh/id_rsa.pub**) till **pbcopy** för att kopiera innehållet (det finns andra Linux-program som gör samma sak, som `xclip`).</span><span class="sxs-lookup"><span data-stu-id="cfdee-114">For example, if you use OS X, you can pipe the public key file (by default, **~/.ssh/id_rsa.pub**) to **pbcopy** to copy the contents (there are other Linux programs that do the same thing, such as `xclip`).</span></span>

<span data-ttu-id="cfdee-115">Om du inte är bekant med offentliga nycklar för SSH kan du se din offentliga nyckel genom att köra `cat` enligt följande, och ersätta `~/.ssh/id_rsa.pub` med din egen plats för offentlig nyckelfil:</span><span class="sxs-lookup"><span data-stu-id="cfdee-115">If you're not familiar with SSH public keys, you can see your public key by running `cat` as follows, replacing `~/.ssh/id_rsa.pub` with your own public key file location:</span></span>

```bash
cat ~/.ssh/id_rsa.pub
```

<span data-ttu-id="cfdee-116">Med den offentliga nyckeln på din virtuella Azure-dator, SSH till din virtuella dator med datorns IP-adress eller DNS-namn (kom ihåg att ersätta `azureuser` och `myvm.westus.cloudapp.azure.com` nedan med adminanvändarnamnet och det fullständigt kvalificerade domännamnet – eller IP-adress):</span><span class="sxs-lookup"><span data-stu-id="cfdee-116">With the public key on your Azure VM, SSH to your VM using the IP address or DNS name of your VM (remember to replace `azureuser` and `myvm.westus.cloudapp.azure.com` below with the admin username and the fully qualified domain name -- or IP address):</span></span>

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

<span data-ttu-id="cfdee-117">Om du angav en lösenfras när du skapade ditt nyckelpar ska du ange lösenfrasen när frågan kommer under inloggningen.</span><span class="sxs-lookup"><span data-stu-id="cfdee-117">If you provided a passphrase when you created your key pair, enter the passphrase when prompted during the login process.</span></span> <span data-ttu-id="cfdee-118">(Servern läggs till i din `~/.ssh/known_hosts`-mapp, och du blir inte ombedd att ansluta igen förrän den offentliga nyckeln på din virtuella Azure-dator ändras eller när servernamnet tas bort från `~/.ssh/known_hosts`.)</span><span class="sxs-lookup"><span data-stu-id="cfdee-118">(The server is added to your `~/.ssh/known_hosts` folder, and you won't be asked to connect again until the public key on your Azure VM changes or the server name is removed from `~/.ssh/known_hosts`.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="cfdee-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cfdee-119">Next steps</span></span>

<span data-ttu-id="cfdee-120">Lösenord är som standard inaktiverade för virtuella datorer som skapas med SSH-nycklar för att göra råstyrkeattacker något dyrare och därför svåra.</span><span class="sxs-lookup"><span data-stu-id="cfdee-120">VMs created using SSH keys are by default configured with passwords disabled, to make brute-forced guessing attempts vastly more expensive and therefore difficult.</span></span> <span data-ttu-id="cfdee-121">I det här avsnittet beskrivs hur du skapar en enkel SSH-nyckel för snabb användning.</span><span class="sxs-lookup"><span data-stu-id="cfdee-121">This topic describes creating a simple SSH key pair for quick usage.</span></span> <span data-ttu-id="cfdee-122">Om du behöver mer hjälp för att skapa SSH-nyckelpar eller om du behöver fler certifikat finns [detaljerade steg för att skapa SSH-nyckelpar och certifikat](create-ssh-keys-detailed.md).</span><span class="sxs-lookup"><span data-stu-id="cfdee-122">If you need more assistance in creating your SSH key pair or require additional certificates, see [Detailed steps to create SSH key pairs and certificates](create-ssh-keys-detailed.md).</span></span>

<span data-ttu-id="cfdee-123">Du kan skapa virtuella datorer som använder ditt SSH-nyckelpar med Azure Portal, CLI och mallar:</span><span class="sxs-lookup"><span data-stu-id="cfdee-123">You can create VMs that use your SSH key pair using the Azure portal, CLI, and templates:</span></span>

* [<span data-ttu-id="cfdee-124">Skapa en säker virtuell Linux-dator med hjälp av Azure Portal</span><span class="sxs-lookup"><span data-stu-id="cfdee-124">Create a secure Linux VM using the Azure portal</span></span>](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="cfdee-125">Skapa en säker virtuell Linux-dator med hjälp av Azure CLI 2.0)</span><span class="sxs-lookup"><span data-stu-id="cfdee-125">Create a secure Linux VM using the Azure CLI 2.0)</span></span>](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="cfdee-126">Skapa en säker virtuell Linux-dator med hjälp av en Azure-mall</span><span class="sxs-lookup"><span data-stu-id="cfdee-126">Create a secure Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
