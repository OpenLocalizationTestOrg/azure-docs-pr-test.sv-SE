---
title: "aaaHow tooreset nätverksgränssnittet för virtuell Azure Windows-dator | Microsoft Docs"
description: "Visar hur tooreset nätverksgränssnittet för virtuell Azure Windows-dator"
services: virtual-machines-windows, azure-resource-manager
documentationcenter: 
author: genlin
manager: willchen
editor: 
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: genli
ms.openlocfilehash: 1b653820927ef4c3bb8f384a7e752846a8be3da9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreset-network-interface-for-azure-windows-vm"></a><span data-ttu-id="05514-103">Hur tooreset nätverksgränssnittet för virtuell Azure Windows-dator</span><span class="sxs-lookup"><span data-stu-id="05514-103">How tooreset network interface for Azure Windows VM</span></span> 

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="05514-104">Du kan inte ansluta tooMicrosoft Azure Windows virtuell dator (VM) när du inaktiverar hello standard Network Interface (NIC) eller anger manuellt en statisk IP-adress för hello nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="05514-104">You cannot connect tooMicrosoft Azure Windows Virtual Machine (VM) after you disable hello default Network Interface (NIC) or manually sets a static IP for hello NIC.</span></span> <span data-ttu-id="05514-105">Den här artikeln visar hur tooreset hello nätverksgränssnittet för Azure Windows VM som löser hello anslutning till problemet.</span><span class="sxs-lookup"><span data-stu-id="05514-105">This article shows how tooreset hello network interface for Azure Windows VM, which will resolve hello remote connection issue.</span></span>

[!INCLUDE [support-disclaimer](../../../includes/support-disclaimer.md)]
## <a name="reset-network-interface"></a><span data-ttu-id="05514-106">Återställ nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="05514-106">Reset network interface</span></span>

### <a name="for-classic-vms"></a><span data-ttu-id="05514-107">För klassiska virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="05514-107">For Classic VMs</span></span>

<span data-ttu-id="05514-108">tooreset nätverk gränssnitt, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="05514-108">tooreset network interface, follow these steps:</span></span>

1.  <span data-ttu-id="05514-109">Gå toohello [Azure-portalen]( https://ms.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="05514-109">Go toohello [Azure portal]( https://ms.portal.azure.com).</span></span>
2.  <span data-ttu-id="05514-110">Välj **virtuella datorer (klassisk)**.</span><span class="sxs-lookup"><span data-stu-id="05514-110">Select **Virtual Machines (Classic)**.</span></span>
3.  <span data-ttu-id="05514-111">Välj hello påverkas virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="05514-111">Select hello affected Virtual Machine.</span></span>
4.  <span data-ttu-id="05514-112">Välj **IP-adresser**.</span><span class="sxs-lookup"><span data-stu-id="05514-112">Select **IP addresses**.</span></span>
5.  <span data-ttu-id="05514-113">Om hello **privata IP-tilldelning** är inte **statiska**, ändra också**statiska**.</span><span class="sxs-lookup"><span data-stu-id="05514-113">If hello **Private IP assignment**  is not  **Static**, change it too**Static**.</span></span>
6.  <span data-ttu-id="05514-114">Ändra hello **IP-adress** tooanother IP-adress som är tillgängliga i hello undernät.</span><span class="sxs-lookup"><span data-stu-id="05514-114">Change hello **IP address** tooanother IP address that is available in hello Subnet.</span></span>
7.  <span data-ttu-id="05514-115">Klicka på Spara.</span><span class="sxs-lookup"><span data-stu-id="05514-115">Select Save.</span></span>
8.  <span data-ttu-id="05514-116">hello virtuella datorn startar om tooinitialize hello nya NIC toohello system.</span><span class="sxs-lookup"><span data-stu-id="05514-116">hello virtual machine will restart tooinitialize hello new NIC toohello system.</span></span>
9.  <span data-ttu-id="05514-117">Försök tooRDP tooyour datorn.</span><span class="sxs-lookup"><span data-stu-id="05514-117">Try tooRDP tooyour machine.</span></span> <span data-ttu-id="05514-118">Om det lyckas, kan du ändra hello privat IP-adress tillbaka toohello ursprungliga om du vill ha.</span><span class="sxs-lookup"><span data-stu-id="05514-118">If successful, you can change hello Private IP address back toohello original if you would like.</span></span> <span data-ttu-id="05514-119">Annars kan du behålla den.</span><span class="sxs-lookup"><span data-stu-id="05514-119">Otherwise, you can keep it.</span></span> 

### <a name="for-vms-deployed-in-resource-group-model"></a><span data-ttu-id="05514-120">För virtuella datorer som distribueras i gruppen modell</span><span class="sxs-lookup"><span data-stu-id="05514-120">For VMs deployed in Resource group model</span></span>

1.  <span data-ttu-id="05514-121">Gå toohello [Azure-portalen]( https://ms.portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="05514-121">Go toohello [Azure portal]( https://ms.portal.azure.com).</span></span>
2.  <span data-ttu-id="05514-122">Välj hello påverkas virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="05514-122">Select hello affected Virtual Machine.</span></span>
3.  <span data-ttu-id="05514-123">Välj **nätverksgränssnitt**.</span><span class="sxs-lookup"><span data-stu-id="05514-123">Select **Network Interfaces**.</span></span>
4.  <span data-ttu-id="05514-124">Välj hello nätverksgränssnitt som är kopplade till datorn</span><span class="sxs-lookup"><span data-stu-id="05514-124">Select hello Network Interface associated with your machine</span></span>
5.  <span data-ttu-id="05514-125">Välj **IP-konfigurationer**.</span><span class="sxs-lookup"><span data-stu-id="05514-125">Select **IP configurations**.</span></span>
6.  <span data-ttu-id="05514-126">Välj hello IP.</span><span class="sxs-lookup"><span data-stu-id="05514-126">Select hello IP.</span></span> 
7.  <span data-ttu-id="05514-127">Om hello **privata IP-tilldelning** är inte **statiska**, ändra också**statiska**.</span><span class="sxs-lookup"><span data-stu-id="05514-127">If hello **Private IP assignment**  is not  **Static**, change it too**Static**.</span></span>
8.  <span data-ttu-id="05514-128">Ändra hello **IP-adress** tooanother IP-adress som är tillgängliga i hello undernät.</span><span class="sxs-lookup"><span data-stu-id="05514-128">Change hello **IP address** tooanother IP address that is available in hello Subnet.</span></span>
9. <span data-ttu-id="05514-129">hello virtuella datorn startar om tooinitialize hello nya NIC toohello system.</span><span class="sxs-lookup"><span data-stu-id="05514-129">hello virtual machine will restart tooinitialize hello new NIC toohello system.</span></span>
10. <span data-ttu-id="05514-130">Försök tooRDP tooyour datorn.</span><span class="sxs-lookup"><span data-stu-id="05514-130">Try tooRDP tooyour machine.</span></span> <span data-ttu-id="05514-131">Om det lyckas, kan du ändra hello privat IP-adress tillbaka toohello ursprungliga om du vill ha.</span><span class="sxs-lookup"><span data-stu-id="05514-131">If successful, you can change hello Private IP address back toohello original if you would like.</span></span> <span data-ttu-id="05514-132">Annars kan du behålla den.</span><span class="sxs-lookup"><span data-stu-id="05514-132">Otherwise, you can keep it.</span></span> 

## <a name="delete-hello-unavailable-nics"></a><span data-ttu-id="05514-133">Ta bort hello tillgänglig nätverkskort</span><span class="sxs-lookup"><span data-stu-id="05514-133">Delete hello unavailable NICs</span></span>
<span data-ttu-id="05514-134">När du kan remote desktop toohello datorn måste du ta bort hello gamla nätverkskort tooavoid hello potentiella problem:</span><span class="sxs-lookup"><span data-stu-id="05514-134">After you can remote desktop toohello machine, you must delete hello old NICs tooavoid hello potential problem:</span></span>

1.  <span data-ttu-id="05514-135">Öppna Enhetshanteraren.</span><span class="sxs-lookup"><span data-stu-id="05514-135">Open Device Manager.</span></span>
2.  <span data-ttu-id="05514-136">Välj **visa** > **Visa dolda enheter**.</span><span class="sxs-lookup"><span data-stu-id="05514-136">Select **View** > **Show hidden devices**.</span></span>
3.  <span data-ttu-id="05514-137">Välj **nätverkskort**.</span><span class="sxs-lookup"><span data-stu-id="05514-137">Select **Network Adapters**.</span></span> 
4.  <span data-ttu-id="05514-138">Sök efter hello nätverkskort med samma namn som ”Microsoft Hyper-V-nätverkskort”.</span><span class="sxs-lookup"><span data-stu-id="05514-138">Check for hello adapters named as "Microsoft Hyper-V Network Adapter".</span></span>
5.  <span data-ttu-id="05514-139">Du kan se ett tillgängligt nätverkskort som är nedtonad. Högerklicka på hello-kort och välj sedan avinstallera.</span><span class="sxs-lookup"><span data-stu-id="05514-139">You might see an unavailable adapter that is grayed out. Right-click hello adapter and then select Uninstall.</span></span>

    ![hello bild av hello NIC](media/reset-network-interface/nicpage.png)

    > [!NOTE]
    > <span data-ttu-id="05514-141">Avinstallera endast hello tillgänglig nätverkskort som har hello namn ”Microsoft Hyper-V-nätverkskort”.</span><span class="sxs-lookup"><span data-stu-id="05514-141">Only uninstall hello unavailable adapters that have hello name "Microsoft Hyper-V Network Adapter".</span></span> <span data-ttu-id="05514-142">Om du avinstallerar någon hello andra dolda kort kan orsaka det ytterligare problem.</span><span class="sxs-lookup"><span data-stu-id="05514-142">If you uninstall any of hello other hidden adapters, it could cause additional issues.</span></span>
    >
    >

6.  <span data-ttu-id="05514-143">Nu ska alla tillgängliga nätverkskort vara avmarkerade från datorn.</span><span class="sxs-lookup"><span data-stu-id="05514-143">Now all unavailable adapter should be cleared out from your system.</span></span>