---
title: "Ansluta till Azure Data Lake Store från Vnet | Microsoft Docs"
description: "Ansluta till Azure Data Lake Store från Azure Vnet"
services: data-lake-store,data-catalog
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 683fcfdc-cf93-46c3-b2d2-5cb79f5e9ea5
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ff7d28d7b53e872b804788647b1e672fafcf6995
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="access-azure-data-lake-store-from-vms-within-an-azure-vnet"></a><span data-ttu-id="021af-103">Åtkomst till Azure Data Lake Store från virtuella datorer i ett Azure-VNET</span><span class="sxs-lookup"><span data-stu-id="021af-103">Access Azure Data Lake Store from VMs within an Azure VNET</span></span>
<span data-ttu-id="021af-104">Azure Data Lake Store är en PaaS-tjänst som körs på offentliga Internet IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="021af-104">Azure Data Lake Store is a PaaS service that runs on public Internet IP addresses.</span></span> <span data-ttu-id="021af-105">Alla servrar som kan ansluta till Internet kan vanligtvis ansluta till Azure Data Lake Store-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="021af-105">Any server that can connect to the public Internet can typically connect to Azure Data Lake Store endpoints as well.</span></span> <span data-ttu-id="021af-106">Som standard alla virtuella datorer som finns i virtuella Azure-nätverk kan ansluta till Internet och därför kan komma åt Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="021af-106">By default, all VMs that are in Azure VNETs can access the Internet and hence can access Azure Data Lake Store.</span></span> <span data-ttu-id="021af-107">Det är dock möjligt att konfigurera virtuella datorer i ett VNET till inte har åtkomst till Internet.</span><span class="sxs-lookup"><span data-stu-id="021af-107">However, it is possible to configure VMs in a VNET to not have access to the Internet.</span></span> <span data-ttu-id="021af-108">För sådana virtuella datorer kan är åtkomst till Azure Data Lake Store begränsad även.</span><span class="sxs-lookup"><span data-stu-id="021af-108">For such VMs, access to Azure Data Lake Store is restricted as well.</span></span> <span data-ttu-id="021af-109">Blockerar tillgång till Internet för virtuella datorer i Azure Vnet kan göras med hjälp av följande metod.</span><span class="sxs-lookup"><span data-stu-id="021af-109">Blocking public Internet access for VMs in Azure VNETs can be done using any of the following approach.</span></span>

* <span data-ttu-id="021af-110">Genom att konfigurera Nätverkssäkerhetsgrupp grupper (NSG)</span><span class="sxs-lookup"><span data-stu-id="021af-110">By configuring Network Security Groups (NSG)</span></span>
* <span data-ttu-id="021af-111">Genom att konfigurera användare användardefinierade vägar (UDR)</span><span class="sxs-lookup"><span data-stu-id="021af-111">By configuring User Defined Routes (UDR)</span></span>
* <span data-ttu-id="021af-112">Genom att utbyta vägar via BGP (dynamisk routning standardprotokoll) när ExpressRoute används som blockerar åtkomst till Internet</span><span class="sxs-lookup"><span data-stu-id="021af-112">By exchanging routes via BGP (industry standard dynamic routing protocol) when ExpressRoute is used that block access to the Internet</span></span>

<span data-ttu-id="021af-113">I den här artikeln får du lära dig hur du aktiverar åtkomst till Azure Data Lake Store från Azure virtuella datorer som har begränsad åtkomst till resurser med hjälp av en av de tre metoderna ovan.</span><span class="sxs-lookup"><span data-stu-id="021af-113">In this article, you will learn how to enable access to the Azure Data Lake Store from Azure VMs which have been restricted to access resources using one of the three methods listed above.</span></span>

## <a name="enabling-connectivity-to-azure-data-lake-store-from-vms-with-restricted-connectivity"></a><span data-ttu-id="021af-114">Aktivera anslutningen till Azure Data Lake Store från virtuella datorer med begränsade anslutningen</span><span class="sxs-lookup"><span data-stu-id="021af-114">Enabling connectivity to Azure Data Lake Store from VMs with restricted connectivity</span></span>
<span data-ttu-id="021af-115">Om du vill komma åt Azure Data Lake Store från dessa virtuella datorer, måste du konfigurera dem för att få åtkomst till IP-adressen där Azure Data Lake Store-konto är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="021af-115">To access Azure Data Lake Store from such VMs, you must configure them to access the IP address where the Azure Data Lake Store account is available.</span></span> <span data-ttu-id="021af-116">Du kan identifiera IP-adresserna för dina Data Lake Store-konton genom DNS-namnmatchning konton (`<account>.azuredatalakestore.net`).</span><span class="sxs-lookup"><span data-stu-id="021af-116">You can identify the IP addresses for your Data Lake Store accounts by resolving the DNS names of your accounts (`<account>.azuredatalakestore.net`).</span></span> <span data-ttu-id="021af-117">För det här kan du använda verktyg som **nslookup**.</span><span class="sxs-lookup"><span data-stu-id="021af-117">For this you can use tools such as **nslookup**.</span></span> <span data-ttu-id="021af-118">Öppna en kommandotolk på datorn och kör följande kommando.</span><span class="sxs-lookup"><span data-stu-id="021af-118">Open a command prompt on your computer and run the following command.</span></span>

    nslookup mydatastore.azuredatalakestore.net

<span data-ttu-id="021af-119">Utdata liknar följande.</span><span class="sxs-lookup"><span data-stu-id="021af-119">The output resembles the following.</span></span> <span data-ttu-id="021af-120">Värdet mot **adress** egenskapen är IP-adressen som är kopplad till ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="021af-120">The value against **Address** property is the IP address associated with your Data Lake Store account.</span></span>

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a><span data-ttu-id="021af-121">Aktivera anslutningen från virtuella datorer som har begränsats med hjälp av NSG</span><span class="sxs-lookup"><span data-stu-id="021af-121">Enabling connectivity from VMs restricted by using NSG</span></span>
<span data-ttu-id="021af-122">När en NSG-regel för att blockera åtkomst till Internet, kan du skapa en annan NSG som ger åtkomst till Data Lake Store IP-adress.</span><span class="sxs-lookup"><span data-stu-id="021af-122">When a NSG rule is used to block access to the Internet, then you can create another NSG that allows access to the Data Lake Store IP Address.</span></span> <span data-ttu-id="021af-123">Mer information om NSG-regler finns på [vad är en Nätverkssäkerhetsgrupp?](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="021af-123">More information on NSG rules is available at [What is a Network Security Group?](../virtual-network/virtual-networks-nsg.md).</span></span> <span data-ttu-id="021af-124">Anvisningar om hur du skapar NSG: er finns [hantera NSG: er med hjälp av Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="021af-124">For instructions on how to create NSGs see [How to manage NSGs using the Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span></span>

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a><span data-ttu-id="021af-125">Aktivera anslutningen från virtuella datorer som har begränsats med hjälp av UDR eller ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="021af-125">Enabling connectivity from VMs restricted by using UDR or ExpressRoute</span></span>
<span data-ttu-id="021af-126">När vägar udr: er eller utväxlats BGP-vägar används för att blockera åtkomst till Internet, måste en särskild väg konfigureras så att virtuella datorer i dessa undernät har åtkomst till Data Lake Store-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="021af-126">When routes, either UDRs or BGP-exchanged routes, are used to block access to the Internet, a special route needs to be configured so that VMs in such subnets can access Data Lake Store endpoints.</span></span> <span data-ttu-id="021af-127">Mer information finns i [vad är användardefinierade vägar?](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="021af-127">For more information, see [What are User Defined Routes?](../virtual-network/virtual-networks-udr-overview.md).</span></span> <span data-ttu-id="021af-128">Instruktioner om hur du skapar udr: er finns i [skapa udr: er i Resource Manager](../virtual-network/virtual-network-create-udr-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="021af-128">For instructions on creating UDRs, see [Create UDRs in Resource Manager](../virtual-network/virtual-network-create-udr-arm-ps.md).</span></span>

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a><span data-ttu-id="021af-129">Aktivera anslutningen från virtuella datorer som har begränsats med hjälp av ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="021af-129">Enabling connectivity from VMs restricted by using ExpressRoute</span></span>
<span data-ttu-id="021af-130">När en ExpressRoute-krets konfigureras lokala servrar kan komma åt Data Lake Store via offentlig peering.</span><span class="sxs-lookup"><span data-stu-id="021af-130">When an ExpressRoute circuit is configured, the on-premises servers can access Data Lake Store through public peering.</span></span> <span data-ttu-id="021af-131">Mer information om hur du konfigurerar ExpressRoute för offentlig peering finns på [ExpressRoute vanliga frågor och svar](../expressroute/expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="021af-131">More details on configuring ExpressRoute for public peering is available at [ExpressRoute FAQs](../expressroute/expressroute-faqs.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="021af-132">Se även</span><span class="sxs-lookup"><span data-stu-id="021af-132">See also</span></span>
* [<span data-ttu-id="021af-133">Översikt över Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="021af-133">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
* [<span data-ttu-id="021af-134">Att säkra data som lagras i Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="021af-134">Securing data stored in Azure Data Lake Store</span></span>](data-lake-store-security-overview.md)

