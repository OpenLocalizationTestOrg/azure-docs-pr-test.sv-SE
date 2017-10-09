---
title: aaaGet ett Azure Linux VM-ID | Microsoft Docs
description: "Beskriver hur tooget och använda en Azure Linux VM unika ID: t."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 136c5d28-ff6b-4466-b27f-7a29785b5d27
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/23/2017
ms.author: kmouss
ms.openlocfilehash: 4c8ddfc2e892824581e77649285ee8adbccd5def
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-and-using-azure-vm-unique-id"></a><span data-ttu-id="b31df-103">Visa och använda virtuella Azure-datorns unika ID: T</span><span class="sxs-lookup"><span data-stu-id="b31df-103">Accessing and Using Azure VM Unique ID</span></span>
<span data-ttu-id="b31df-104">Azure VM unika ID: T är en 128-identifierare kodade och lagras i alla Azure IaaS Virtuella datorns SMBIOS och för närvarande kan läsas med hjälp av plattform BIOS-kommandon.</span><span class="sxs-lookup"><span data-stu-id="b31df-104">Azure VM unique ID is a 128bits identifier encoded and stored in all Azure IaaS VM’s SMBIOS and can currently be read using platform BIOS commands.</span></span>

<span data-ttu-id="b31df-105">Azure VM unika ID: T är en skrivskyddad egenskap.</span><span class="sxs-lookup"><span data-stu-id="b31df-105">Azure VM unique ID is a Read-only property.</span></span> <span data-ttu-id="b31df-106">Azure unikt ID för virtuell dator ändras inte vid omstart avstängning (antingen den planerade för oplanerad), starta eller stoppa Frigör, tjänsten återställning eller Återställ på plats.</span><span class="sxs-lookup"><span data-stu-id="b31df-106">Azure Unique VM ID won’t change upon reboot shutdown (either planned for unplanned), Start/Stop de-allocate, service healing or restore in place.</span></span> <span data-ttu-id="b31df-107">Om hello VM är en ögonblicksbild och kopierade toocreate en ny instans, konfigureras nya Azure VM-ID.</span><span class="sxs-lookup"><span data-stu-id="b31df-107">However, if hello VM is a snapshot and copied toocreate a new instance, new Azure VM ID is configured.</span></span>

> [!NOTE]
> <span data-ttu-id="b31df-108">Om du har äldre virtuella datorer skapas och kör sedan denna nya funktion har distribuerat (18 September 2014), starta om VM-tooautomatically hämta ett Azure unikt ID.</span><span class="sxs-lookup"><span data-stu-id="b31df-108">If you have older VMs created and running since this new feature got rolled out (September 18, 2014), please restart your VM tooautomatically get an Azure unique ID.</span></span>
> 
> 

<span data-ttu-id="b31df-109">tooaccess Azure unika VM-ID från inom hello VM:</span><span class="sxs-lookup"><span data-stu-id="b31df-109">tooaccess Azure Unique VM ID from within hello VM:</span></span>

## <a name="create-a-vm"></a><span data-ttu-id="b31df-110">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b31df-110">Create a VM</span></span>
<span data-ttu-id="b31df-111">Mer information finns i [skapa en virtuell dator](../windows/creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="b31df-111">For more information, see [Create a Virtual Machine](../windows/creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="connect-toohello-vm"></a><span data-ttu-id="b31df-112">Ansluta toohello VM</span><span class="sxs-lookup"><span data-stu-id="b31df-112">Connect toohello VM</span></span>
<span data-ttu-id="b31df-113">Mer information finns i [SSH från Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="b31df-113">For more information, see [SSH from Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)</span></span>

## <a name="query-vm-unique-id"></a><span data-ttu-id="b31df-114">Unikt ID för frågan VM</span><span class="sxs-lookup"><span data-stu-id="b31df-114">Query VM Unique ID</span></span>
<span data-ttu-id="b31df-115">Kommandot (exempel används **Ubuntu**):</span><span class="sxs-lookup"><span data-stu-id="b31df-115">Command (example uses **Ubuntu**):</span></span>

```bash
sudo dmidecode | grep UUID
```

<span data-ttu-id="b31df-116">Exempel förväntat resultat:</span><span class="sxs-lookup"><span data-stu-id="b31df-116">Example Expected Results:</span></span>

```bash
UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
```

<span data-ttu-id="b31df-117">På grund av tooBig Endian bitar ordning, kommer hello faktiska unikt ID för virtuell dator i det här fallet att:</span><span class="sxs-lookup"><span data-stu-id="b31df-117">Due tooBig Endian bit ordering, hello actual Unique VM ID in this case will be:</span></span>

```bash
DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
```

<span data-ttu-id="b31df-118">Azure VM unika ID: T kan användas i olika scenarier om hello VM körs på Azure eller lokalt och kan hjälpa dina licenser, reporting eller allmänna spårning krav i Azure IaaS-distributioner kan vara.</span><span class="sxs-lookup"><span data-stu-id="b31df-118">Azure VM unique ID can be used in different scenarios whether hello VM is running on Azure or on-premises and can help your licensing, reporting or general tracking requirements you may have on your Azure IaaS deployments.</span></span> <span data-ttu-id="b31df-119">Många oberoende programvaruleverantörer bygga program och certifierar dem på Azure kan kräva tooidentify en Azure VM under hela dess livscykel och tootell om hello VM körs på Azure, lokala eller andra molntjänstleverantörer av.</span><span class="sxs-lookup"><span data-stu-id="b31df-119">Many independent software vendors building applications and certifying them on Azure may require tooidentify an Azure VM throughout its lifecycle and tootell if hello VM is running on Azure, on-Premises or on other cloud providers.</span></span> <span data-ttu-id="b31df-120">Den här plattformen identifieraren kan till exempel att identifiera om hello programvaran licensieras korrekt eller att toocorrelate alla VM-tooits datakällor, till exempel tooassist på inställningen hello rätt mått för hello rätt plattform och tootrack och korrelera de här måtten bland andra använder.</span><span class="sxs-lookup"><span data-stu-id="b31df-120">This platform identifier can for example help detect if hello software is properly licensed or help toocorrelate any VM data tooits source such as tooassist on setting hello right metrics for hello right platform and tootrack and correlate these metrics amongst other uses.</span></span>

