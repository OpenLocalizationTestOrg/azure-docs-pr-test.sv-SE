---
title: "aaaCannot ta bort ett virtuellt nätverk i Azure | Microsoft Docs"
description: "Lär dig hur tootroubleshoot hello problem som du inte kan ta bort ett virtuellt nätverk i Azure."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2017
ms.author: genli
ms.openlocfilehash: a9050ab238ccb0380fd46130430222efb8f42388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-failed-toodelete-a-virtual-network-in-azure"></a><span data-ttu-id="835dc-103">Felsöka: Det gick inte toodelete ett virtuellt nätverk i Azure</span><span class="sxs-lookup"><span data-stu-id="835dc-103">Troubleshooting: Failed toodelete a virtual network in Azure</span></span>

<span data-ttu-id="835dc-104">Du kan råka ut för fel vid försök toodelete ett virtuellt nätverk i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="835dc-104">You might receive errors when you try toodelete a virtual network in Microsoft Azure.</span></span> <span data-ttu-id="835dc-105">Den här artikeln innehåller felsökning steg toohelp du lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="835dc-105">This article provides troubleshooting steps toohelp you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="troubleshooting-guidance"></a><span data-ttu-id="835dc-106">Felsökningsanvisningar</span><span class="sxs-lookup"><span data-stu-id="835dc-106">Troubleshooting guidance</span></span> 

1. <span data-ttu-id="835dc-107">[Kontrollera om en virtuell nätverksgateway körs i virtuella nätverk för hello](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="835dc-107">[Check whether a virtual network gateway is running in hello virtual network](#check-whether-a-virtual-network-gateway-is-running-in-the-virtual-network).</span></span>
2. <span data-ttu-id="835dc-108">[Kontrollera om en Programgateway körs i virtuella nätverk för hello](#check-whether-an-application-gateway-is-running-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="835dc-108">[Check whether an application gateway is running in hello virtual network](#check-whether-an-application-gateway-is-running-in-the-virtual-network).</span></span>
3. <span data-ttu-id="835dc-109">[Kontrollera om Azure Active Directory Domain Service har aktiverats i det virtuella nätverket för hello](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="835dc-109">[Check whether Azure Active Directory Domain Service is enabled in hello virtual network](#check-whether-azure-active-directory-domain-service-is-enabled-in-the-virtual-network).</span></span>
4. <span data-ttu-id="835dc-110">[Kontrollera om hello virtuellt nätverk är anslutna tooother resurs](#check-whether-the-virtual-network-is-connected-to-other-resource).</span><span class="sxs-lookup"><span data-stu-id="835dc-110">[Check whether hello virtual network is connected tooother resource](#check-whether-the-virtual-network-is-connected-to-other-resource).</span></span>
5. <span data-ttu-id="835dc-111">[Kontrollera om en virtuell dator körs fortfarande i hello virtuellt nätverk](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).</span><span class="sxs-lookup"><span data-stu-id="835dc-111">[Check whether a virtual machine is still running in hello virtual network](#check-whether-a-virtual-machine-is-still-running-in-the-virtual-network).</span></span>
6. <span data-ttu-id="835dc-112">[Kontrollera om hello virtuellt nätverk har fastnat i migrering](#check-whether-the-virtual-network-is-stuck-in-migration).</span><span class="sxs-lookup"><span data-stu-id="835dc-112">[Check whether hello virtual network is stuck in migration](#check-whether-the-virtual-network-is-stuck-in-migration).</span></span>

## <a name="troubleshooting-steps"></a><span data-ttu-id="835dc-113">Felsökningssteg</span><span class="sxs-lookup"><span data-stu-id="835dc-113">Troubleshooting steps</span></span>

### <a name="check-whether-a-virtual-network-gateway-is-running-in-hello-virtual-network"></a><span data-ttu-id="835dc-114">Kontrollera om en virtuell nätverksgateway körs i hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="835dc-114">Check whether a virtual network gateway is running in hello virtual network</span></span>

<span data-ttu-id="835dc-115">tooremove hello virtuellt nätverk, måste du först ta bort hello virtuell nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="835dc-115">tooremove hello virtual network, you must first remove hello virtual network gateway.</span></span>

<span data-ttu-id="835dc-116">Klassiska virtuella nätverk finns i toohello **översikt** sidan av hello klassiska virtuella nätverk i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="835dc-116">For classic virtual networks, go toohello **Overview** page of hello classic virtual network in hello Azure portal.</span></span> <span data-ttu-id="835dc-117">I hello **VPN-anslutningar** avsnittet, om hello gateway körs i hello virtuellt nätverk ser du hello IP-adressen för hello gateway.</span><span class="sxs-lookup"><span data-stu-id="835dc-117">In hello **VPN connections** section, if hello gateway is running in hello virtual network, you will see hello IP address of hello gateway.</span></span> 

![Kontrollera om gatewayen körs](media/virtual-network-troubleshoot-cannot-delete-vnet/classic-gateway.png)

<span data-ttu-id="835dc-119">Virtuella nätverk finns i toohello **översikt** sidan av hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="835dc-119">For virtual networks, go toohello **Overview** page of hello virtual network.</span></span> <span data-ttu-id="835dc-120">Kontrollera **anslutna enheter** för hello virtuell nätverksgateway.</span><span class="sxs-lookup"><span data-stu-id="835dc-120">Check **Connected devices** for hello virtual network gateway.</span></span>

![Kontrollera hello ansluten enhet](media/virtual-network-troubleshoot-cannot-delete-vnet/vnet-gateway.png)

<span data-ttu-id="835dc-122">Innan du tar bort hello gateway först ta bort alla **anslutning** objekt i hello gateway.</span><span class="sxs-lookup"><span data-stu-id="835dc-122">Before you can remove hello gateway, first remove any **Connection** objects in hello gateway.</span></span> 

### <a name="check-whether-an-application-gateway-is-running-in-hello-virtual-network"></a><span data-ttu-id="835dc-123">Kontrollera om en Programgateway körs i hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="835dc-123">Check whether an application gateway is running in hello virtual network</span></span>

<span data-ttu-id="835dc-124">Gå toohello **översikt** sidan av hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="835dc-124">Go toohello **Overview** page of hello virtual network.</span></span> <span data-ttu-id="835dc-125">Kontrollera hello **anslutna enheter** för hello Programgateway.</span><span class="sxs-lookup"><span data-stu-id="835dc-125">Check hello **Connected devices** for hello application gateway.</span></span>

![Kontrollera hello ansluten enhet](media/virtual-network-troubleshoot-cannot-delete-vnet/app-gateway.png)

<span data-ttu-id="835dc-127">Om det finns en Programgateway måste du ta bort det innan du kan ta bort hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="835dc-127">If there is an application gateway, you must remove it before you can delete hello virtual network.</span></span>

### <a name="check-whether-azure-active-directory-domain-service-is-enabled-in-hello-virtual-network"></a><span data-ttu-id="835dc-128">Kontrollera om Azure Active Directory Domain Service har aktiverats i hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="835dc-128">Check whether Azure Active Directory Domain Service is enabled in hello virtual network</span></span>

<span data-ttu-id="835dc-129">Om hello Active Directory Domain Service är aktiverade och anslutna toohello virtuella nätverk, kan du ta bort det här virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="835dc-129">If hello Active Directory Domain Service is enabled and connected toohello virtual network, you cannot delete this virtual network.</span></span> 

![Kontrollera hello ansluten enhet](media/virtual-network-troubleshoot-cannot-delete-vnet/enable-domain-services.png)

<span data-ttu-id="835dc-131">toodisable Hej tjänsten, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="835dc-131">toodisable hello service, follow these steps:</span></span>

1. <span data-ttu-id="835dc-132">Gå toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="835dc-132">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="835dc-133">I hello vänster och välj **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="835dc-133">In hello left pane, select  **Active Directory**.</span></span>
3. <span data-ttu-id="835dc-134">Välj hello Azure Active Directory (AD Azure) katalog med Active Directory Domain Service aktiverat.</span><span class="sxs-lookup"><span data-stu-id="835dc-134">Select hello Azure Active Directory (Azure AD) directory that has Active Directory Domain Service enabled.</span></span>
4. <span data-ttu-id="835dc-135">Välj hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="835dc-135">Select hello **Configure** tab.</span></span>
5. <span data-ttu-id="835dc-136">Under **domäntjänster**, ändra hello **aktivera domain services för den här katalogen** alternativet för**nr**.</span><span class="sxs-lookup"><span data-stu-id="835dc-136">Under **domain services**, change hello **Enable domain services for this directory** option too**No**.</span></span>  

### <a name="check-whether-hello-virtual-network-is-connected-tooother-resource"></a><span data-ttu-id="835dc-137">Kontrollera om hello virtuellt nätverk är anslutna tooother resurs</span><span class="sxs-lookup"><span data-stu-id="835dc-137">Check whether hello virtual network is connected tooother resource</span></span>

<span data-ttu-id="835dc-138">Sök efter krets länkar, anslutningar och peerkopplingar mellan virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="835dc-138">Check for Circuit Links, connections, and virtual network peerings.</span></span> <span data-ttu-id="835dc-139">Dessa kan orsaka en toofail för borttagning av virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="835dc-139">Any of these can cause a virtual network deletion toofail.</span></span> 

<span data-ttu-id="835dc-140">hello är rekommenderade borttagning ordning följande:</span><span class="sxs-lookup"><span data-stu-id="835dc-140">hello recommended deletion order is as follows:</span></span>

1. <span data-ttu-id="835dc-141">Gateway-anslutningar</span><span class="sxs-lookup"><span data-stu-id="835dc-141">Gateway connections</span></span>
2. <span data-ttu-id="835dc-142">Gateways</span><span class="sxs-lookup"><span data-stu-id="835dc-142">Gateways</span></span>
3. <span data-ttu-id="835dc-143">IP-adresser</span><span class="sxs-lookup"><span data-stu-id="835dc-143">IPs</span></span>
4. <span data-ttu-id="835dc-144">Peerkopplingar mellan virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="835dc-144">Virtual network peerings</span></span>
5. <span data-ttu-id="835dc-145">Apptjänst-miljö (ASE)</span><span class="sxs-lookup"><span data-stu-id="835dc-145">App Service Environment (ASE)</span></span>

### <a name="check-whether-a-virtual-machine-is-still-running-in-hello-virtual-network"></a><span data-ttu-id="835dc-146">Kontrollera om en virtuell dator körs fortfarande i hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="835dc-146">Check whether a virtual machine is still running in hello virtual network</span></span>

<span data-ttu-id="835dc-147">Se till att ingen virtuell dator är i hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="835dc-147">Make sure that no virtual machine is in hello virtual network.</span></span>

### <a name="check-whether-hello-virtual-network-is-stuck-in-migration"></a><span data-ttu-id="835dc-148">Kontrollera om hello virtuellt nätverk har fastnat i migrering</span><span class="sxs-lookup"><span data-stu-id="835dc-148">Check whether hello virtual network is stuck in migration</span></span>

<span data-ttu-id="835dc-149">Om hello virtuellt nätverk har fastnat i migreringstillståndet, kan inte tas bort.</span><span class="sxs-lookup"><span data-stu-id="835dc-149">If hello virtual network is stuck in a migration state, it cannot be deleted.</span></span> <span data-ttu-id="835dc-150">Kör hello efter kommandot tooabort hello migreringen och tar bort hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="835dc-150">Run hello following command tooabort hello migration, and then delete hello virtual network.</span></span>

    Move-AzureVirtualNetwork -VirtualNetworkName "Name" -Abort

## <a name="next-steps"></a><span data-ttu-id="835dc-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="835dc-151">Next steps</span></span>

- [<span data-ttu-id="835dc-152">Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="835dc-152">Azure Virtual Network</span></span>](virtual-networks-overview.md)
- [<span data-ttu-id="835dc-153">Vanliga frågor och svar (FAQ) om Azure Virtual Network</span><span class="sxs-lookup"><span data-stu-id="835dc-153">Azure Virtual Network frequently asked questions (FAQ)</span></span>](virtual-networks-faq.md)