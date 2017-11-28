---
title: "aaaEnable eller inaktivera Azure VM-övervakning"
description: "Beskriver hur tooEnable eller inaktivera Azure VM-övervakning"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 6ce366d2-bd4c-4fef-a8f5-a3ae2374abcc
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/08/2016
ms.author: kmouss
ms.openlocfilehash: 39c2211e4e5f3ad364513b52c5acc70c00b432fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-or-disable-azure-vm-monitoring"></a><span data-ttu-id="c6b52-103">Aktivera eller inaktivera övervakning på Azure VM</span><span class="sxs-lookup"><span data-stu-id="c6b52-103">Enable or Disable Azure VM Monitoring</span></span>

<span data-ttu-id="c6b52-104">Det här avsnittet beskrivs hur tooenable eller inaktivera övervakning på virtuella datorer som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="c6b52-104">This section describes how tooenable or disable monitoring on Virtual machines running on Azure.</span></span> <span data-ttu-id="c6b52-105">Du kan aktivera eller inaktivera övervakning med hjälp av hello-portalen eller Azure-kommandoradsgränssnittet för Mac, Linux och Windows (hello Azure CLI).</span><span class="sxs-lookup"><span data-stu-id="c6b52-105">You can enable or disable monitoring using hello portal or Azure Command-line Interface for Mac, Linux, and Windows (hello Azure CLI).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c6b52-106">Det här dokumentet beskriver version 2.3 av hello diagnostiska Linux-tillägg, som har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="c6b52-106">This document describes version 2.3 of hello Linux Diagnostic Extension, which has been deprecated.</span></span> <span data-ttu-id="c6b52-107">Version 2.3 kommer att stödjas tills 30 juni 2018.</span><span class="sxs-lookup"><span data-stu-id="c6b52-107">Version 2.3 will be supported until June 30, 2018.</span></span>
>
> <span data-ttu-id="c6b52-108">Version 3.0 av hello Linux diagnostiska tillägget kan aktiveras i stället.</span><span class="sxs-lookup"><span data-stu-id="c6b52-108">Version 3.0 of hello Linux Diagnostic Extension can be enabled instead.</span></span> <span data-ttu-id="c6b52-109">Mer information finns i [hello dokumentationen](./diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="c6b52-109">For more information, see [hello documentation](./diagnostic-extension.md).</span></span>

## <a name="enable--disable-monitoring-through-hello-azure-portal"></a><span data-ttu-id="c6b52-110">Aktivera / inaktivera övervakning via hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c6b52-110">Enable / Disable Monitoring through hello Azure portal</span></span>

<span data-ttu-id="c6b52-111">Du kan aktivera övervakning av din Azure VM, som ger data om din instans under perioder på 1 minut.</span><span class="sxs-lookup"><span data-stu-id="c6b52-111">You can enable  monitoring of your Azure VM, which provides data about your instance in 1-minute periods.</span></span> <span data-ttu-id="c6b52-112">(storage ändringarna).</span><span class="sxs-lookup"><span data-stu-id="c6b52-112">(storage changes apply).</span></span> <span data-ttu-id="c6b52-113">Detaljerad diagnostikdata är sedan tillgängliga för hello VM i hello portal diagram eller via hello API.</span><span class="sxs-lookup"><span data-stu-id="c6b52-113">Detailed diagnostics data is then available for hello VM in hello portal graphs or through hello API.</span></span> <span data-ttu-id="c6b52-114">Som standard aktiverar Azure-portalen värdbaserad övervakning av en begränsad uppsättning mått.</span><span class="sxs-lookup"><span data-stu-id="c6b52-114">By default, Azure portal enables host-based monitoring of a limited set of metrics.</span></span> <span data-ttu-id="c6b52-115">Du kan aktivera övervakning av mått på en virtuell dator när hello virtuella datorn körs eller i ett stoppat tillstånd.</span><span class="sxs-lookup"><span data-stu-id="c6b52-115">You can enable monitoring of metrics from within a VM while hello VM is running or in stopped state.</span></span>

* <span data-ttu-id="c6b52-116">Öppna hello Azure portal på [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c6b52-116">Open hello Azure portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
* <span data-ttu-id="c6b52-117">I vänster navigeringsfält hello, klickar du på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c6b52-117">In hello left navigation, click Virtual machines.</span></span>
* <span data-ttu-id="c6b52-118">Välj en igång eller stoppad instans i hello lista över virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c6b52-118">In hello list Virtual machines, select a running or stopped instance.</span></span> <span data-ttu-id="c6b52-119">Hej ”virtuell dator” blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="c6b52-119">hello "Virtual machine" blade opens.</span></span>
* <span data-ttu-id="c6b52-120">Klicka på alla inställningar.</span><span class="sxs-lookup"><span data-stu-id="c6b52-120">Click All settings.</span></span>
* <span data-ttu-id="c6b52-121">Klicka på diagnostik.</span><span class="sxs-lookup"><span data-stu-id="c6b52-121">Click Diagnostics.</span></span>
* <span data-ttu-id="c6b52-122">Ändra status tooOn eller Off.</span><span class="sxs-lookup"><span data-stu-id="c6b52-122">Change status tooOn or Off.</span></span> <span data-ttu-id="c6b52-123">Du kan också hämta i det här bladet hello nivån för övervakning information som du vill använda tooenable för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="c6b52-123">You can also pick in this blade hello level of monitoring details you would like tooenable for your virtual machine.</span></span>

![Aktivera eller inaktivera övervakning via hello Azure-portalen.][1]

## <a name="enable--disable-monitoring-with-azure-cli"></a><span data-ttu-id="c6b52-125">Aktivera / inaktivera övervakning med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="c6b52-125">Enable / Disable Monitoring with Azure CLI</span></span>

<span data-ttu-id="c6b52-126">tooenable övervakning för en Azure VM.</span><span class="sxs-lookup"><span data-stu-id="c6b52-126">tooenable monitoring for an Azure VM.</span></span>

* <span data-ttu-id="c6b52-127">Skapa en fil (med namnet, till exempel PrivateConfig.json):</span><span class="sxs-lookup"><span data-stu-id="c6b52-127">Create a file (named such as PrivateConfig.json):</span></span>

```json
{
        "storageAccountName":"hello storage account tooreceive data",
        "storageAccountKey":"hello key of hello account"
}
```

* <span data-ttu-id="c6b52-128">Aktivera hello tillägget via Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="c6b52-128">Enable hello extension via Azure CLI.</span></span>

```azurecli
azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.3 --private-config-path PrivateConfig.json
```

<span data-ttu-id="c6b52-129">Mer information finns i [med Linux diagnostiska tillägget tooMonitor Linux VM prestanda och diagnostikdata](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c6b52-129">For more information, see [Using Linux Diagnostic Extension tooMonitor Linux VM’s performance and diagnostic data](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

<!--Image references-->
[1]: ./media/vm-monitoring/portal-enable-disable.png
