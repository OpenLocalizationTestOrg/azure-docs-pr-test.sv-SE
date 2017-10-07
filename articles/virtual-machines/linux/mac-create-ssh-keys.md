---
title: "aaaCreate och använder en SSH-nyckelpar för Linux virtuella datorer i Azure | Microsoft Docs"
description: "Hur toocreate och använder ett SSH offentliga och privata nyckelpar för Linux virtuella datorer i Azure tooimprove hello säkerheten för hello autentiseringsprocessen."
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
ms.openlocfilehash: 7fb94841d34d5bc006f3134adf91102ddce5f174
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-use-an-ssh-public-and-private-key-pair-for-linux-vms-in-azure"></a><span data-ttu-id="28827-103">Hur toocreate och använder en SSH-offentliga och privata nyckel koppla för Linux virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="28827-103">How toocreate and use an SSH public and private key pair for Linux VMs in Azure</span></span>
<span data-ttu-id="28827-104">Du kan använda ett nyckelpar för secure shell (SSH), för att skapa virtuella datorer (VM) i Azure som använder SSH-nycklar för autentisering, vilket eliminerar hello behovet av att lösenord toolog i.</span><span class="sxs-lookup"><span data-stu-id="28827-104">With a secure shell (SSH) key pair, you can create virtual machines (VMs) in Azure that use SSH keys for authentication, eliminating hello need for passwords toolog in.</span></span> <span data-ttu-id="28827-105">Den här artikeln visar hur tooquickly generera och använda en SSH-protokollet version 2 RSA offentliga och privata nyckelfilen nyckelpar för Linux virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="28827-105">This article shows you how tooquickly generate and use an SSH protocol version 2 RSA public and private key file pair for Linux VMs.</span></span> <span data-ttu-id="28827-106">Mer detaljerad steg och ytterligare exempel finns [detaljerade steg toocreate SSH-nyckelpar och certifikat](create-ssh-keys-detailed.md).</span><span class="sxs-lookup"><span data-stu-id="28827-106">For more detailed steps and additional examples, see [detailed steps toocreate SSH key pairs and certificates](create-ssh-keys-detailed.md).</span></span>

## <a name="create-an-ssh-key-pair"></a><span data-ttu-id="28827-107">Skapa ett SSH-nyckelpar</span><span class="sxs-lookup"><span data-stu-id="28827-107">Create an SSH key pair</span></span>
<span data-ttu-id="28827-108">Använd hello `ssh-keygen` kommandot toocreate SSH offentliga och privata nyckelfiler som är som standard som skapats i hello `~/.ssh` directory, men du kan ange en annan plats och ytterligare lösenfras (en lösenord tooaccess hello fil för privat nyckel) när Du uppmanas till detta.</span><span class="sxs-lookup"><span data-stu-id="28827-108">Use hello `ssh-keygen` command toocreate SSH public and private key files that are by default created in hello `~/.ssh` directory, but you can specify a different location and additional passphrase (a password tooaccess hello private key file) when prompted.</span></span> <span data-ttu-id="28827-109">Kör följande kommando från ett Bash-gränssnitt hello besvara hello frågar med din egen information.</span><span class="sxs-lookup"><span data-stu-id="28827-109">Run hello following command from a Bash shell, answering hello prompts with your own information.</span></span>

```bash
ssh-keygen -t rsa -b 2048
```

## <a name="use-hello-ssh-key-pair"></a><span data-ttu-id="28827-110">Använd hello SSH-nyckel</span><span class="sxs-lookup"><span data-stu-id="28827-110">Use hello SSH key pair</span></span>
<span data-ttu-id="28827-111">hello offentliga nyckeln som placeras på din Linux VM i Azure är som standard i `~/.ssh/id_rsa.pub`, om du har ändrat hello plats när du har skapat.</span><span class="sxs-lookup"><span data-stu-id="28827-111">hello public key that you place on your Linux VM in Azure is by default stored in `~/.ssh/id_rsa.pub`, unless you changed hello location when you created them.</span></span> <span data-ttu-id="28827-112">Om du använder hello [Azure CLI 2.0](/cli/azure) toocreate din VM, ange hello platsen för den här offentliga nyckeln när du använder hello [az vm skapa](/cli/azure/vm#create) med hello `--ssh-key-path` alternativet.</span><span class="sxs-lookup"><span data-stu-id="28827-112">If you use hello [Azure CLI 2.0](/cli/azure) toocreate your VM, specify hello location of this public key when you use hello [az vm create](/cli/azure/vm#create) with hello `--ssh-key-path` option.</span></span> <span data-ttu-id="28827-113">Om du kopierar och klistrar in hello innehållet i hello offentlig nyckelfil toouse i hello Azure-portalen eller Resource Manager-mall, kontrollera att du inte kopiera några ytterligare blanksteg.</span><span class="sxs-lookup"><span data-stu-id="28827-113">If you copy and paste hello contents of hello public key file toouse in hello Azure portal or a Resource Manager template, make sure you don't copy any additional whitespace.</span></span> <span data-ttu-id="28827-114">Till exempel om du använder OS X, kan du skicka hello-fil för offentlig nyckel (som standard **~/.ssh/id_rsa.pub**) för**pbcopy** toocopy hello innehållet (det finns andra Linux-program som hello samma sak, till exempel `xclip`).</span><span class="sxs-lookup"><span data-stu-id="28827-114">For example, if you use OS X, you can pipe hello public key file (by default, **~/.ssh/id_rsa.pub**) too**pbcopy** toocopy hello contents (there are other Linux programs that do hello same thing, such as `xclip`).</span></span>

<span data-ttu-id="28827-115">Om du inte är bekant med offentliga nycklar för SSH kan du se din offentliga nyckel genom att köra `cat` enligt följande, och ersätta `~/.ssh/id_rsa.pub` med din egen plats för offentlig nyckelfil:</span><span class="sxs-lookup"><span data-stu-id="28827-115">If you're not familiar with SSH public keys, you can see your public key by running `cat` as follows, replacing `~/.ssh/id_rsa.pub` with your own public key file location:</span></span>

```bash
cat ~/.ssh/id_rsa.pub
```

<span data-ttu-id="28827-116">Med hello offentlig nyckel på Azure VM, SSH tooyour VM med hello IP-adress eller DNS-namnet på den virtuella datorn (Kom ihåg tooreplace `azureuser` och `myvm.westus.cloudapp.azure.com` nedan med hello administratörsanvändarnamnet och fullständigt kvalificerat domännamn för hello- eller IP-adress):</span><span class="sxs-lookup"><span data-stu-id="28827-116">With hello public key on your Azure VM, SSH tooyour VM using hello IP address or DNS name of your VM (remember tooreplace `azureuser` and `myvm.westus.cloudapp.azure.com` below with hello admin username and hello fully qualified domain name -- or IP address):</span></span>

```bash
ssh azureuser@myvm.westus.cloudapp.azure.com
```

<span data-ttu-id="28827-117">Om du angav en lösenfras när du skapade din nyckelpar, ange hello lösenfras när frågan under hello inloggningen.</span><span class="sxs-lookup"><span data-stu-id="28827-117">If you provided a passphrase when you created your key pair, enter hello passphrase when prompted during hello login process.</span></span> <span data-ttu-id="28827-118">(hello server läggs tooyour `~/.ssh/known_hosts` och du inte uppge tooconnect igen förrän hello offentliga nyckel på Virtuella Azure-ändringar eller hello servernamn tas bort från `~/.ssh/known_hosts`.)</span><span class="sxs-lookup"><span data-stu-id="28827-118">(hello server is added tooyour `~/.ssh/known_hosts` folder, and you won't be asked tooconnect again until hello public key on your Azure VM changes or hello server name is removed from `~/.ssh/known_hosts`.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="28827-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28827-119">Next steps</span></span>

<span data-ttu-id="28827-120">Virtuella datorer som skapats med hjälp av SSH-nycklar är som standard konfigurerade med lösenord inaktiveras, toomake nyckelsökningsattacker gissa försöker frågeprestanda avsevärt dyrare och därför svårt.</span><span class="sxs-lookup"><span data-stu-id="28827-120">VMs created using SSH keys are by default configured with passwords disabled, toomake brute-forced guessing attempts vastly more expensive and therefore difficult.</span></span> <span data-ttu-id="28827-121">I det här avsnittet beskrivs hur du skapar en enkel SSH-nyckel för snabb användning.</span><span class="sxs-lookup"><span data-stu-id="28827-121">This topic describes creating a simple SSH key pair for quick usage.</span></span> <span data-ttu-id="28827-122">Om du behöver mer hjälp med att skapa SSH-nyckel eller kräver ytterligare certifikat, se [detaljerade steg toocreate SSH-nyckelpar och certifikat](create-ssh-keys-detailed.md).</span><span class="sxs-lookup"><span data-stu-id="28827-122">If you need more assistance in creating your SSH key pair or require additional certificates, see [Detailed steps toocreate SSH key pairs and certificates](create-ssh-keys-detailed.md).</span></span>

<span data-ttu-id="28827-123">Du kan skapa virtuella datorer som använder en SSH-nyckel med hjälp av hello Azure-portalen, CLI och mallar:</span><span class="sxs-lookup"><span data-stu-id="28827-123">You can create VMs that use your SSH key pair using hello Azure portal, CLI, and templates:</span></span>

* [<span data-ttu-id="28827-124">Skapa en säker Linux VM som använder hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="28827-124">Create a secure Linux VM using hello Azure portal</span></span>](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="28827-125">Skapa en säker Linux VM som använder hello Azure CLI 2.0)</span><span class="sxs-lookup"><span data-stu-id="28827-125">Create a secure Linux VM using hello Azure CLI 2.0)</span></span>](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [<span data-ttu-id="28827-126">Skapa en säker virtuell Linux-dator med hjälp av en Azure-mall</span><span class="sxs-lookup"><span data-stu-id="28827-126">Create a secure Linux VM using an Azure template</span></span>](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
