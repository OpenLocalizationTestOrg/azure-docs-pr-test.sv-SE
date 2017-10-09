---
title: aaaAzure CLI prover Windows | Microsoft Docs
description: Windows Azure CLI-exempel
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 1eef61a24d14897dd0a88a3f467854cc21b1938d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a><span data-ttu-id="743af-103">Azure CLI-exempel för Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="743af-103">Azure CLI Samples for Windows virtual machines</span></span>

<span data-ttu-id="743af-104">hello innehåller följande tabell länkar toobash skript med hjälp av hello Azure CLI som distribuerar Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="743af-104">hello following table includes links toobash scripts built using hello Azure CLI that deploy Windows virtual machines.</span></span>

| | |
|---|---|
|<span data-ttu-id="743af-105">**Skapa virtuella datorer**</span><span class="sxs-lookup"><span data-stu-id="743af-105">**Create virtual machines**</span></span>||
| [<span data-ttu-id="743af-106">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="743af-106">Create a virtual machine</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="743af-107">Skapar en virtuell Windows-dator med minimal konfiguration.</span><span class="sxs-lookup"><span data-stu-id="743af-107">Creates a Windows virtual machine with minimal configuration.</span></span> |
| [<span data-ttu-id="743af-108">Skapa en helt konfigurerade virtuell dator</span><span class="sxs-lookup"><span data-stu-id="743af-108">Create a fully configured virtual machine</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="743af-109">Skapar en resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="743af-109">Creates a resource group, virtual machine, and all related resources.</span></span>|
| [<span data-ttu-id="743af-110">Skapa virtuella datorer med hög tillgänglighet</span><span class="sxs-lookup"><span data-stu-id="743af-110">Create highly available virtual machines</span></span>](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="743af-111">Skapar flera virtuella datorer i en hög tillgänglighet och konfiguration för Utjämning av nätverksbelastning.</span><span class="sxs-lookup"><span data-stu-id="743af-111">Creates several virtual machines in a highly available and load balanced configuration.</span></span> |
| [<span data-ttu-id="743af-112">Skapa en virtuell dator och kör konfigurationsskript</span><span class="sxs-lookup"><span data-stu-id="743af-112">Create a VM and run configuration script</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="743af-113">Skapar en virtuell dator och använder hello Azure anpassat skript tillägget tooinstall IIS.</span><span class="sxs-lookup"><span data-stu-id="743af-113">Creates a virtual machine and uses hello Azure Custom Script extension tooinstall IIS.</span></span> |
| [<span data-ttu-id="743af-114">Skapa en virtuell dator och kör DSC-konfiguration</span><span class="sxs-lookup"><span data-stu-id="743af-114">Create a VM and run DSC configuration</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="743af-115">Skapar en virtuell dator och använder hello Azure önskad tillstånd Configuration DSC ()-tillägget tooinstall IIS.</span><span class="sxs-lookup"><span data-stu-id="743af-115">Creates a virtual machine and uses hello Azure Desired State Configuration (DSC) extension tooinstall IIS.</span></span> |
|<span data-ttu-id="743af-116">**Virtuella datorer i nätverk**</span><span class="sxs-lookup"><span data-stu-id="743af-116">**Network virtual machines**</span></span>||
| [<span data-ttu-id="743af-117">Säkra nätverkstrafik mellan virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="743af-117">Secure network traffic between virtual machines</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="743af-118">Skapar två virtuella datorer, alla relaterade resurser och en interna och externa nätverkssäkerhetsgrupper (NSG).</span><span class="sxs-lookup"><span data-stu-id="743af-118">Creates two virtual machines, all related resources, and an internal and external network security groups (NSG).</span></span> |
|<span data-ttu-id="743af-119">**Skydda virtuella datorer**</span><span class="sxs-lookup"><span data-stu-id="743af-119">**Secure virtual machines**</span></span>||
| [<span data-ttu-id="743af-120">Kryptera en virtuell dator- och datadiskar</span><span class="sxs-lookup"><span data-stu-id="743af-120">Encrypt a VM and data disks</span></span>](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="743af-121">Skapar en Azure Key Vault, krypteringsnyckeln och tjänstens huvudnamn och krypterar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="743af-121">Creates an Azure Key Vault, encryption key, and service principal, then encrypts a VM.</span></span> |
|<span data-ttu-id="743af-122">**Övervaka virtuella datorer**</span><span class="sxs-lookup"><span data-stu-id="743af-122">**Monitor virtual machines**</span></span>||
| [<span data-ttu-id="743af-123">Övervaka en virtuell dator med Operations Management Suite</span><span class="sxs-lookup"><span data-stu-id="743af-123">Monitor a VM with Operations Management Suite</span></span>](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | <span data-ttu-id="743af-124">Skapar en virtuell dator, installerar hello Operations Management Suite-agenten och registrerar hello VM i en OMS-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="743af-124">Creates a virtual machine, installs hello Operations Management Suite agent, and enrolls hello VM in an OMS Workspace.</span></span>  |
| | |
