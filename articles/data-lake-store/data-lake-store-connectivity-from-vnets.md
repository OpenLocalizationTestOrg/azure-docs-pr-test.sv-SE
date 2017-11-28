---
title: "aaaConnect tooAzure Data Lake Store från Vnet | Microsoft Docs"
description: "Ansluta tooAzure Data Lake Store från Azure Vnet"
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
ms.openlocfilehash: c695dcf49fe4e1a87a90729cf085a938f3b51fe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="access-azure-data-lake-store-from-vms-within-an-azure-vnet"></a><span data-ttu-id="1397b-103">Åtkomst till Azure Data Lake Store från virtuella datorer i ett Azure-VNET</span><span class="sxs-lookup"><span data-stu-id="1397b-103">Access Azure Data Lake Store from VMs within an Azure VNET</span></span>
<span data-ttu-id="1397b-104">Azure Data Lake Store är en PaaS-tjänst som körs på offentliga Internet IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="1397b-104">Azure Data Lake Store is a PaaS service that runs on public Internet IP addresses.</span></span> <span data-ttu-id="1397b-105">Alla servrar som kan ansluta toohello offentliga Internet kan vanligtvis ansluta tooAzure Data Lake Store-och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="1397b-105">Any server that can connect toohello public Internet can typically connect tooAzure Data Lake Store endpoints as well.</span></span> <span data-ttu-id="1397b-106">Alla virtuella datorer som finns i Azure Vnet kan komma åt hello Internet som standard och därför kan komma åt Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="1397b-106">By default, all VMs that are in Azure VNETs can access hello Internet and hence can access Azure Data Lake Store.</span></span> <span data-ttu-id="1397b-107">Det är dock möjligt tooconfigure virtuella datorer i ett VNET toonot har åtkomst toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="1397b-107">However, it is possible tooconfigure VMs in a VNET toonot have access toohello Internet.</span></span> <span data-ttu-id="1397b-108">För sådana virtuella datorer kan är åtkomst till tooAzure Data Lake Store begränsad även.</span><span class="sxs-lookup"><span data-stu-id="1397b-108">For such VMs, access tooAzure Data Lake Store is restricted as well.</span></span> <span data-ttu-id="1397b-109">Blockerar tillgång till Internet för virtuella datorer i Azure Vnet kan göras med hjälp av hello följande metod.</span><span class="sxs-lookup"><span data-stu-id="1397b-109">Blocking public Internet access for VMs in Azure VNETs can be done using any of hello following approach.</span></span>

* <span data-ttu-id="1397b-110">Genom att konfigurera Nätverkssäkerhetsgrupp grupper (NSG)</span><span class="sxs-lookup"><span data-stu-id="1397b-110">By configuring Network Security Groups (NSG)</span></span>
* <span data-ttu-id="1397b-111">Genom att konfigurera användare användardefinierade vägar (UDR)</span><span class="sxs-lookup"><span data-stu-id="1397b-111">By configuring User Defined Routes (UDR)</span></span>
* <span data-ttu-id="1397b-112">Genom att utbyta vägar via BGP (dynamisk routning standardprotokoll) när ExpressRoute används som blockerar åtkomst till toohello Internet</span><span class="sxs-lookup"><span data-stu-id="1397b-112">By exchanging routes via BGP (industry standard dynamic routing protocol) when ExpressRoute is used that block access toohello Internet</span></span>

<span data-ttu-id="1397b-113">I den här artikeln får du lära dig hur tooenable åt toohello Azure Data Lake Store från virtuella Azure-datorer som har varit begränsade tooaccess resurser med hjälp av någon av hello tre metoder som anges ovan.</span><span class="sxs-lookup"><span data-stu-id="1397b-113">In this article, you will learn how tooenable access toohello Azure Data Lake Store from Azure VMs which have been restricted tooaccess resources using one of hello three methods listed above.</span></span>

## <a name="enabling-connectivity-tooazure-data-lake-store-from-vms-with-restricted-connectivity"></a><span data-ttu-id="1397b-114">Aktivera anslutningen tooAzure Data Lake Store från virtuella datorer med begränsade anslutningen</span><span class="sxs-lookup"><span data-stu-id="1397b-114">Enabling connectivity tooAzure Data Lake Store from VMs with restricted connectivity</span></span>
<span data-ttu-id="1397b-115">tooaccess Azure Data Lake lagra från dessa virtuella datorer, måste du konfigurera dem tooaccess hello IP-adress där hello Azure Data Lake Store-konto är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="1397b-115">tooaccess Azure Data Lake Store from such VMs, you must configure them tooaccess hello IP address where hello Azure Data Lake Store account is available.</span></span> <span data-ttu-id="1397b-116">Du kan identifiera hello IP-adresser för dina Data Lake Store-konton genom hello DNS-namnmatchning konton (`<account>.azuredatalakestore.net`).</span><span class="sxs-lookup"><span data-stu-id="1397b-116">You can identify hello IP addresses for your Data Lake Store accounts by resolving hello DNS names of your accounts (`<account>.azuredatalakestore.net`).</span></span> <span data-ttu-id="1397b-117">För det här kan du använda verktyg som **nslookup**.</span><span class="sxs-lookup"><span data-stu-id="1397b-117">For this you can use tools such as **nslookup**.</span></span> <span data-ttu-id="1397b-118">Öppna en kommandotolk på datorn och kör följande kommando hello.</span><span class="sxs-lookup"><span data-stu-id="1397b-118">Open a command prompt on your computer and run hello following command.</span></span>

    nslookup mydatastore.azuredatalakestore.net

<span data-ttu-id="1397b-119">hello följande slag hello utdata.</span><span class="sxs-lookup"><span data-stu-id="1397b-119">hello output resembles hello following.</span></span> <span data-ttu-id="1397b-120">Hej värdet mot **adress** egenskapen är hello IP-adress som är kopplad till ditt Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="1397b-120">hello value against **Address** property is hello IP address associated with your Data Lake Store account.</span></span>

    Non-authoritative answer:
    Name:    1434ceb1-3a4b-4bc0-9c69-a0823fd69bba-mydatastore.projectcabostore.net
    Address:  104.44.88.112
    Aliases:  mydatastore.azuredatalakestore.net


### <a name="enabling-connectivity-from-vms-restricted-by-using-nsg"></a><span data-ttu-id="1397b-121">Aktivera anslutningen från virtuella datorer som har begränsats med hjälp av NSG</span><span class="sxs-lookup"><span data-stu-id="1397b-121">Enabling connectivity from VMs restricted by using NSG</span></span>
<span data-ttu-id="1397b-122">När en NSG-regel används åt tooblock toohello Internet, kan du skapa en annan NSG som tillåter åtkomst toohello Data Lake Store IP-adress.</span><span class="sxs-lookup"><span data-stu-id="1397b-122">When a NSG rule is used tooblock access toohello Internet, then you can create another NSG that allows access toohello Data Lake Store IP Address.</span></span> <span data-ttu-id="1397b-123">Mer information om NSG-regler finns på [vad är en Nätverkssäkerhetsgrupp?](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="1397b-123">More information on NSG rules is available at [What is a Network Security Group?](../virtual-network/virtual-networks-nsg.md).</span></span> <span data-ttu-id="1397b-124">Anvisningar för hur toocreate NSG: er Se [hur toomanage NSG: er med hjälp av hello Azure-portalen](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="1397b-124">For instructions on how toocreate NSGs see [How toomanage NSGs using hello Azure portal](../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span></span>

### <a name="enabling-connectivity-from-vms-restricted-by-using-udr-or-expressroute"></a><span data-ttu-id="1397b-125">Aktivera anslutningen från virtuella datorer som har begränsats med hjälp av UDR eller ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="1397b-125">Enabling connectivity from VMs restricted by using UDR or ExpressRoute</span></span>
<span data-ttu-id="1397b-126">När vägar udr: er eller utväxlats BGP-vägar används tooblock åtkomst toohello Internet, måste en särskild väg toobe konfigurerats så att virtuella datorer i dessa undernät har åtkomst till Data Lake Store-slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="1397b-126">When routes, either UDRs or BGP-exchanged routes, are used tooblock access toohello Internet, a special route needs toobe configured so that VMs in such subnets can access Data Lake Store endpoints.</span></span> <span data-ttu-id="1397b-127">Mer information finns i [vad är användardefinierade vägar?](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1397b-127">For more information, see [What are User Defined Routes?](../virtual-network/virtual-networks-udr-overview.md).</span></span> <span data-ttu-id="1397b-128">Instruktioner om hur du skapar udr: er finns i [skapa udr: er i Resource Manager](../virtual-network/virtual-network-create-udr-arm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="1397b-128">For instructions on creating UDRs, see [Create UDRs in Resource Manager](../virtual-network/virtual-network-create-udr-arm-ps.md).</span></span>

### <a name="enabling-connectivity-from-vms-restricted-by-using-expressroute"></a><span data-ttu-id="1397b-129">Aktivera anslutningen från virtuella datorer som har begränsats med hjälp av ExpressRoute</span><span class="sxs-lookup"><span data-stu-id="1397b-129">Enabling connectivity from VMs restricted by using ExpressRoute</span></span>
<span data-ttu-id="1397b-130">När en ExpressRoute-krets konfigureras hello lokala servrar kan komma åt Data Lake Store via offentlig peering.</span><span class="sxs-lookup"><span data-stu-id="1397b-130">When an ExpressRoute circuit is configured, hello on-premises servers can access Data Lake Store through public peering.</span></span> <span data-ttu-id="1397b-131">Mer information om hur du konfigurerar ExpressRoute för offentlig peering finns på [ExpressRoute vanliga frågor och svar](../expressroute/expressroute-faqs.md).</span><span class="sxs-lookup"><span data-stu-id="1397b-131">More details on configuring ExpressRoute for public peering is available at [ExpressRoute FAQs](../expressroute/expressroute-faqs.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="1397b-132">Se även</span><span class="sxs-lookup"><span data-stu-id="1397b-132">See also</span></span>
* [<span data-ttu-id="1397b-133">Översikt över Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="1397b-133">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
* [<span data-ttu-id="1397b-134">Att säkra data som lagras i Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="1397b-134">Securing data stored in Azure Data Lake Store</span></span>](data-lake-store-security-overview.md)

