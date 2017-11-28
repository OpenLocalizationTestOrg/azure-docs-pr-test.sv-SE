---
title: "aaaExtend HDInsight med ett virtuellt nätverk - Azure | Microsoft Docs"
description: "Lär dig hur toouse Azure Virtual Network tooconnect HDInsight tooother cloud resurser eller resurser i ditt datacenter"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 37b9b600-d7f8-4cb1-a04a-0b3a827c6dcc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/23/2017
ms.author: larryfr
ms.openlocfilehash: ba80be4d9f280c6c62fa8acc996ef5f921acdbbd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="extend-azure-hdinsight-using-an-azure-virtual-network"></a><span data-ttu-id="1558d-103">Utöka Azure HDInsight med hjälp av ett virtuellt Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="1558d-103">Extend Azure HDInsight using an Azure Virtual Network</span></span>

<span data-ttu-id="1558d-104">Lär dig hur toouse HDInsight med en [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1558d-104">Learn how toouse HDInsight with an [Azure Virtual Network](../virtual-network/virtual-networks-overview.md).</span></span> <span data-ttu-id="1558d-105">Med ett Azure Virtual Network kan hello följande scenarier:</span><span class="sxs-lookup"><span data-stu-id="1558d-105">Using an Azure Virtual Network enables hello following scenarios:</span></span>

* <span data-ttu-id="1558d-106">Ansluter tooHDInsight direkt från ett lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-106">Connecting tooHDInsight directly from an on-premises network.</span></span>

* <span data-ttu-id="1558d-107">Ansluta HDInsight toodata lagras i ett virtuellt Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-107">Connecting HDInsight toodata stores in an Azure Virtual network.</span></span>

* <span data-ttu-id="1558d-108">Direkt åtkomst till Hadoop-tjänster som inte är tillgängliga offentligt över hello internet.</span><span class="sxs-lookup"><span data-stu-id="1558d-108">Directly accessing Hadoop services that are not available publicly over hello internet.</span></span> <span data-ttu-id="1558d-109">Till exempel Kafka API: er eller hello HBase Java API.</span><span class="sxs-lookup"><span data-stu-id="1558d-109">For example, Kafka APIs or hello HBase Java API.</span></span>

> [!WARNING]
> <span data-ttu-id="1558d-110">hello informationen i det här dokumentet kräver en förståelse av TCP/IP-nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-110">hello information in this document requires an understanding of TCP/IP networking.</span></span> <span data-ttu-id="1558d-111">Om du inte är bekant med TCP/IP-nätverk bör du samarbeta med någon som innan du gör ändringar tooproduction nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-111">If you are not familiar with TCP/IP networking, you should partner with someone who is before making modifications tooproduction networks.</span></span>

## <a name="planning"></a><span data-ttu-id="1558d-112">Planering</span><span class="sxs-lookup"><span data-stu-id="1558d-112">Planning</span></span>

<span data-ttu-id="1558d-113">hello följande är hello frågor som du måste svara på när du planerar tooinstall HDInsight i ett virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="1558d-113">hello following are hello questions that you must answer when planning tooinstall HDInsight in a virtual network:</span></span>

* <span data-ttu-id="1558d-114">Behöver du tooinstall HDInsight till ett befintligt virtuellt nätverk?</span><span class="sxs-lookup"><span data-stu-id="1558d-114">Do you need tooinstall HDInsight into an existing virtual network?</span></span> <span data-ttu-id="1558d-115">Eller skapar du ett nytt nätverk?</span><span class="sxs-lookup"><span data-stu-id="1558d-115">Or are you creating a new network?</span></span>

    <span data-ttu-id="1558d-116">Om du använder ett befintligt virtuellt nätverk kan behöva du toomodify hello nätverkskonfigurationen innan du kan installera HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1558d-116">If you are using an existing virtual network, you may need toomodify hello network configuration before you can install HDInsight.</span></span> <span data-ttu-id="1558d-117">Mer information finns i hello [lägga till HDInsight tooan befintligt virtuellt nätverk](#existingvnet) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-117">For more information, see hello [add HDInsight tooan existing virtual network](#existingvnet) section.</span></span>

* <span data-ttu-id="1558d-118">Vill du tooconnect hello virtuellt nätverk som innehåller HDInsight tooanother virtuellt nätverk eller lokala nätverk?</span><span class="sxs-lookup"><span data-stu-id="1558d-118">Do you want tooconnect hello virtual network containing HDInsight tooanother virtual network or your on-premises network?</span></span>

    <span data-ttu-id="1558d-119">tooeasily arbete med resurser över nätverk, kan du behöver toocreate en anpassad DNS och konfigurera vidarebefordran av DNS.</span><span class="sxs-lookup"><span data-stu-id="1558d-119">tooeasily work with resources across networks, you may need toocreate a custom DNS and configure DNS forwarding.</span></span> <span data-ttu-id="1558d-120">Mer information finns i hello [ansluta flera nätverk](#multinet) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-120">For more information, see hello [connecting multiple networks](#multinet) section.</span></span>

* <span data-ttu-id="1558d-121">Vill du toorestrict/omdirigering tooHDInsight för inkommande eller utgående trafik?</span><span class="sxs-lookup"><span data-stu-id="1558d-121">Do you want toorestrict/redirect inbound or outbound traffic tooHDInsight?</span></span>

    <span data-ttu-id="1558d-122">HDInsight har obegränsad kommunikation med specifika IP-adresser i hello Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="1558d-122">HDInsight must have unrestricted communication with specific IP addresses in hello Azure data center.</span></span> <span data-ttu-id="1558d-123">Det finns också flera portar som får genom brandväggar för klientkommunikation.</span><span class="sxs-lookup"><span data-stu-id="1558d-123">There are also several ports that must be allowed through firewalls for client communication.</span></span> <span data-ttu-id="1558d-124">Mer information finns i hello [styra nätverkstrafiken](#networktraffic) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-124">For more information, see hello [controlling network traffic](#networktraffic) section.</span></span>

## <span data-ttu-id="1558d-125"><a id="existingvnet"></a>Lägg till HDInsight tooan befintligt virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="1558d-125"><a id="existingvnet"></a>Add HDInsight tooan existing virtual network</span></span>

<span data-ttu-id="1558d-126">Använd hello steg i det här avsnittet toodiscover hur tooadd en ny HDInsight tooan befintliga Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="1558d-126">Use hello steps in this section toodiscover how tooadd a new HDInsight tooan existing Azure Virtual Network.</span></span>

> [!NOTE]
> <span data-ttu-id="1558d-127">Du kan inte lägga till ett befintligt HDInsight-kluster till ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-127">You cannot add an existing HDInsight cluster into a virtual network.</span></span>

1. <span data-ttu-id="1558d-128">Du använder ett klassiskt eller Resource Manager-modellen för hello virtuella nätverket?</span><span class="sxs-lookup"><span data-stu-id="1558d-128">Are you using a classic or Resource Manager deployment model for hello virtual network?</span></span>

    <span data-ttu-id="1558d-129">HDInsight 3,4 och större kräver ett virtuellt nätverk för hanteraren för filserverresurser.</span><span class="sxs-lookup"><span data-stu-id="1558d-129">HDInsight 3.4 and greater requires a Resource Manager virtual network.</span></span> <span data-ttu-id="1558d-130">Tidigare versioner av HDInsight krävs ett klassiskt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-130">Earlier versions of HDInsight required a classic virtual network.</span></span>

    <span data-ttu-id="1558d-131">Om det befintliga nätverket är ett klassiskt virtuellt nätverk, måste du skapa ett virtuellt nätverk för Resource Manager och anslut sedan hello två.</span><span class="sxs-lookup"><span data-stu-id="1558d-131">If your existing network is a classic virtual network, then you must create a Resource Manager virtual network and then connect hello two.</span></span> <span data-ttu-id="1558d-132">[Ansluta klassiska Vnet toonew Vnet](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1558d-132">[Connecting classic VNets toonew VNets](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md).</span></span>

    <span data-ttu-id="1558d-133">När ansluten, kan HDInsight installeras i nätverk med hello Resource Manager interagera med resurser i hello klassiska nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-133">Once joined, HDInsight installed in hello Resource Manager network can interact with resources in hello classic network.</span></span>

2. <span data-ttu-id="1558d-134">Använder du Tvingad tunneling?</span><span class="sxs-lookup"><span data-stu-id="1558d-134">Do you use forced tunneling?</span></span> <span data-ttu-id="1558d-135">Tvingad tunneling är undernätsinställning som tvingar utgående Internet-trafik tooa enheten för granskning och loggning.</span><span class="sxs-lookup"><span data-stu-id="1558d-135">Forced tunneling is a subnet setting that forces outbound Internet traffic tooa device for inspection and logging.</span></span> <span data-ttu-id="1558d-136">HDInsight stöder inte Tvingad tunneling.</span><span class="sxs-lookup"><span data-stu-id="1558d-136">HDInsight does not support forced tunneling.</span></span> <span data-ttu-id="1558d-137">Ta bort Tvingad tunneling innan du installerar HDInsight i ett undernät eller skapa ett nytt undernät för HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1558d-137">Either remove forced tunneling before installing HDInsight into a subnet, or create a new subnet for HDInsight.</span></span>

3. <span data-ttu-id="1558d-138">Du använder nätverkssäkerhetsgrupper, användardefinierade vägar och virtuella nätverksenheter toorestrict trafik till eller från hello virtuella nätverket?</span><span class="sxs-lookup"><span data-stu-id="1558d-138">Do you use network security groups, user-defined routes, or Virtual Network Appliances toorestrict traffic into or out of hello virtual network?</span></span>

    <span data-ttu-id="1558d-139">Som en hanterad tjänst kräver HDInsight obegränsad åtkomst tooseveral IP-adresser i hello Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="1558d-139">As a managed service, HDInsight requires unrestricted access tooseveral IP addresses in hello Azure data center.</span></span> <span data-ttu-id="1558d-140">tooallow kommunikation med IP-adresserna, uppdatera alla befintliga nätverkssäkerhetsgrupper eller användardefinierade vägar.</span><span class="sxs-lookup"><span data-stu-id="1558d-140">tooallow communication with these IP addresses, update any existing network security groups or user-defined routes.</span></span>

    <span data-ttu-id="1558d-141">HDInsight är värd för flera tjänster som använder olika portar.</span><span class="sxs-lookup"><span data-stu-id="1558d-141">HDInsight  hosts multiple services, which use a variety of ports.</span></span> <span data-ttu-id="1558d-142">Inte blockera trafik toothese portar.</span><span class="sxs-lookup"><span data-stu-id="1558d-142">Do not block traffic toothese ports.</span></span> <span data-ttu-id="1558d-143">En lista över portar tooallow genom virtuell installation brandväggar, finns hello [säkerhet](#security) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-143">For a list of ports tooallow through virtual appliance firewalls, see hello [Security](#security) section.</span></span>

    <span data-ttu-id="1558d-144">toofind befintliga säkerhetskonfiguration Använd hello följande Azure PowerShell eller Azure CLI-kommandon:</span><span class="sxs-lookup"><span data-stu-id="1558d-144">toofind your existing security configuration, use hello following Azure PowerShell or Azure CLI commands:</span></span>

    * <span data-ttu-id="1558d-145">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="1558d-145">Network security groups</span></span>

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
        get-azurermnetworksecuritygroup -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
        az network nsg list --resource-group $RESOURCEGROUP
        ```

        <span data-ttu-id="1558d-146">Mer information finns i hello [felsöka nätverkssäkerhetsgrupper](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1558d-146">For more information, see hello [Troubleshoot network security groups](../virtual-network/virtual-network-nsg-troubleshoot-portal.md) document.</span></span>

        > [!IMPORTANT]
        > <span data-ttu-id="1558d-147">Regler för nätverkssäkerhetsgrupper tillämpas i ordning baserat på prioritet för regeln.</span><span class="sxs-lookup"><span data-stu-id="1558d-147">Network security group rules are applied in order based on rule priority.</span></span> <span data-ttu-id="1558d-148">Inga andra tillämpas efter den trafiken hello första regeln som matchar hello trafik mönster tillämpas.</span><span class="sxs-lookup"><span data-stu-id="1558d-148">hello first rule that matches hello traffic pattern is applied, and no others are applied for that traffic.</span></span> <span data-ttu-id="1558d-149">Regler från mest Tillåtande tooleast Tillåtande.</span><span class="sxs-lookup"><span data-stu-id="1558d-149">Order rules from most permissive tooleast permissive.</span></span> <span data-ttu-id="1558d-150">Mer information finns i hello [filtrera nätverkstrafik med nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1558d-150">For more information, see hello [Filter network traffic with network security groups](../virtual-network/virtual-networks-nsg.md) document.</span></span>

    * <span data-ttu-id="1558d-151">Användardefinierade vägar</span><span class="sxs-lookup"><span data-stu-id="1558d-151">User-defined routes</span></span>

        ```powershell
        $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
        get-azurermroutetable -resourcegroupname $resourceGroupName
        ```

        ```azurecli-interactive
        read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
        az network route-table list --resource-group $RESOURCEGROUP
        ```

        <span data-ttu-id="1558d-152">Mer information finns i hello [felsöka vägar](../virtual-network/virtual-network-routes-troubleshoot-portal.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1558d-152">For more information, see hello [Troubleshoot routes](../virtual-network/virtual-network-routes-troubleshoot-portal.md) document.</span></span>

4. <span data-ttu-id="1558d-153">Skapa ett HDInsight-kluster och välj hello Azure Virtual Network under konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="1558d-153">Create an HDInsight cluster and select hello Azure Virtual Network during configuration.</span></span> <span data-ttu-id="1558d-154">Använd hello steg i följande dokument toounderstand hello klusterskapandeprocessen hello:</span><span class="sxs-lookup"><span data-stu-id="1558d-154">Use hello steps in hello following documents toounderstand hello cluster creation process:</span></span>

    * [<span data-ttu-id="1558d-155">Skapa HDInsight med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="1558d-155">Create HDInsight using hello Azure portal</span></span>](hdinsight-hadoop-create-linux-clusters-portal.md)
    * <span data-ttu-id="1558d-156">[Create HDInsight using Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md) (Skapa HDInsight med hjälp av Azure PowerShell)</span><span class="sxs-lookup"><span data-stu-id="1558d-156">[Create HDInsight using Azure PowerShell](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)</span></span>
    * [<span data-ttu-id="1558d-157">Skapa HDInsight med hjälp av Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="1558d-157">Create HDInsight using Azure CLI 1.0</span></span>](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [<span data-ttu-id="1558d-158">Skapa HDInsight med hjälp av en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="1558d-158">Create HDInsight using an Azure Resource Manager template</span></span>](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

  > [!IMPORTANT]
  > <span data-ttu-id="1558d-159">Lägga till HDInsight är tooa virtuellt nätverk en valfri konfigurationssteg.</span><span class="sxs-lookup"><span data-stu-id="1558d-159">Adding HDInsight tooa virtual network is an optional configuration step.</span></span> <span data-ttu-id="1558d-160">Vara säker på att tooselect hello virtuellt nätverk när du konfigurerar hello klustret.</span><span class="sxs-lookup"><span data-stu-id="1558d-160">Be sure tooselect hello virtual network when configuring hello cluster.</span></span>

## <span data-ttu-id="1558d-161"><a id="multinet"></a>Ansluta flera nätverk</span><span class="sxs-lookup"><span data-stu-id="1558d-161"><a id="multinet"></a>Connecting multiple networks</span></span>

<span data-ttu-id="1558d-162">hello största utmaningen med en konfiguration med flera är namnmatchning mellan hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-162">hello biggest challenge with a multi-network configuration is name resolution between hello networks.</span></span>

<span data-ttu-id="1558d-163">Azure tillhandahåller namnmatchning för Azure-tjänster som är installerade i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-163">Azure provides name resolution for Azure services that are installed in a virtual network.</span></span> <span data-ttu-id="1558d-164">Den här inbyggda namnmatchning kan HDInsight tooconnect toohello följande resurser genom att använda ett fullständigt kvalificerat domännamn (FQDN):</span><span class="sxs-lookup"><span data-stu-id="1558d-164">This built-in name resolution allows HDInsight tooconnect toohello following resources by using a fully qualified domain name (FQDN):</span></span>

* <span data-ttu-id="1558d-165">Alla resurser som är tillgängligt på hello internet.</span><span class="sxs-lookup"><span data-stu-id="1558d-165">Any resource that is available on hello internet.</span></span> <span data-ttu-id="1558d-166">Till exempel microsoft.com, google.com.</span><span class="sxs-lookup"><span data-stu-id="1558d-166">For example, microsoft.com, google.com.</span></span>

* <span data-ttu-id="1558d-167">Alla resurser som är i hello samma Azure-nätverk med hjälp av hello __interna DNS-namnet__ hello resurs.</span><span class="sxs-lookup"><span data-stu-id="1558d-167">Any resource that is in hello same Azure Virtual Network, by using hello __internal DNS name__ of hello resource.</span></span> <span data-ttu-id="1558d-168">När du använder hello standard namnmatchning är hello följande exempel interna DNS-namn tilldelade tooHDInsight arbetsnoderna:</span><span class="sxs-lookup"><span data-stu-id="1558d-168">For example, when using hello default name resolution, hello following are example internal DNS names assigned tooHDInsight worker nodes:</span></span>

    * <span data-ttu-id="1558d-169">wn0 hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="1558d-169">wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span></span>
    * <span data-ttu-id="1558d-170">wn2 hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="1558d-170">wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net</span></span>

    <span data-ttu-id="1558d-171">Båda dessa noder kan kommunicera direkt med varandra och andra noder i HDInsight med hjälp av interna DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="1558d-171">Both these nodes can communicate directly with each other, and other nodes in HDInsight, by using internal DNS names.</span></span>

<span data-ttu-id="1558d-172">hello standard namnmatchning har __inte__ Tillåt HDInsight tooresolve hello namnen på de resurser i nätverk som är kopplade toohello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-172">hello default name resolution does __not__ allow HDInsight tooresolve hello names of resources in networks that are joined toohello virtual network.</span></span> <span data-ttu-id="1558d-173">Till exempel är det vanliga toojoin ditt lokala nätverk toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-173">For example, it is common toojoin your on-premises network toohello virtual network.</span></span> <span data-ttu-id="1558d-174">HDInsight kan inte med endast hello standard namnmatchning kan komma åt resurser i hello lokalt nätverk efter namn.</span><span class="sxs-lookup"><span data-stu-id="1558d-174">With only hello default name resolution, HDInsight cannot access resources in hello on-premises network by name.</span></span> <span data-ttu-id="1558d-175">hello motsatt gäller även, resurser i det lokala nätverket inte kan komma åt resurser i hello virtuella nätverket efter namn.</span><span class="sxs-lookup"><span data-stu-id="1558d-175">hello opposite is also true, resources in your on-premises network cannot access resources in hello virtual network by name.</span></span>

> [!WARNING]
> <span data-ttu-id="1558d-176">Du måste skapa hello anpassad DNS-server och konfigurera hello virtuellt nätverk toouse den innan du skapar hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="1558d-176">You must create hello custom DNS server and configure hello virtual network toouse it before creating hello HDInsight cluster.</span></span>

<span data-ttu-id="1558d-177">tooenable namnmatchning mellan hello virtuella nätverk och nätverksresurser på anslutna nätverk, måste du utföra följande åtgärder hello:</span><span class="sxs-lookup"><span data-stu-id="1558d-177">tooenable name resolution between hello virtual network and resources in joined networks, you must perform hello following actions:</span></span>

1. <span data-ttu-id="1558d-178">Skapa en anpassad DNS-server i hello Azure Virtual Network där du planerar att tooinstall HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1558d-178">Create a custom DNS server in hello Azure Virtual Network where you plan tooinstall HDInsight.</span></span>

2. <span data-ttu-id="1558d-179">Konfigurera hello virtuellt nätverk toouse hello anpassad DNS-server.</span><span class="sxs-lookup"><span data-stu-id="1558d-179">Configure hello virtual network toouse hello custom DNS server.</span></span>

3. <span data-ttu-id="1558d-180">Hitta hello Azure tilldelade DNS-suffixet för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="1558d-180">Find hello Azure assigned DNS suffix for your virtual network.</span></span> <span data-ttu-id="1558d-181">Det här värdet är liknande för`0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`.</span><span class="sxs-lookup"><span data-stu-id="1558d-181">This value is similar too`0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net`.</span></span> <span data-ttu-id="1558d-182">Information om hur du hittar hello DNS-suffix finns hello [exempel: anpassad DNS](#example-dns) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-182">For information on finding hello DNS suffix, see hello [Example: Custom DNS](#example-dns) section.</span></span>

4. <span data-ttu-id="1558d-183">Konfigurera vidarebefordran mellan hello DNS-servrar.</span><span class="sxs-lookup"><span data-stu-id="1558d-183">Configure forwarding between hello DNS servers.</span></span> <span data-ttu-id="1558d-184">hello konfigurationen beror på hello typ av nätverk med fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="1558d-184">hello configuration depends on hello type of remote network.</span></span>

    * <span data-ttu-id="1558d-185">Om hello fjärrnätverket är ett lokalt nätverk, konfigurera DNS på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1558d-185">If hello remote network is an on-premises network, configure DNS as follows:</span></span>
        
        * <span data-ttu-id="1558d-186">__Anpassad DNS__ (i hello virtuellt nätverk):</span><span class="sxs-lookup"><span data-stu-id="1558d-186">__Custom DNS__ (in hello virtual network):</span></span>

            * <span data-ttu-id="1558d-187">Vidarebefordra begäranden för hello DNS-suffix hello virtuellt nätverk toohello Azure rekursiv matcharen (168.63.129.16).</span><span class="sxs-lookup"><span data-stu-id="1558d-187">Forward requests for hello DNS suffix of hello virtual network toohello Azure recursive resolver (168.63.129.16).</span></span> <span data-ttu-id="1558d-188">Azure hanterar begäranden för resurser i hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="1558d-188">Azure handles requests for resources in hello virtual network</span></span>

            * <span data-ttu-id="1558d-189">Vidarebefordra alla andra begäranden toohello lokala DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="1558d-189">Forward all other requests toohello on-premises DNS server.</span></span> <span data-ttu-id="1558d-190">hello lokalt DNS hanterar alla andra namnmatchning, även begäranden om Internetresurser, till exempel Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="1558d-190">hello on-premises DNS handles all other name resolution requests, even requests for internet resources such as Microsoft.com.</span></span>

        * <span data-ttu-id="1558d-191">__Lokal DNS__: vidarebefordrar begäranden för hello virtuellt nätverk DNS-suffix toohello anpassade DNS-server.</span><span class="sxs-lookup"><span data-stu-id="1558d-191">__On-premises DNS__: Forward requests for hello virtual network DNS suffix toohello custom DNS server.</span></span> <span data-ttu-id="1558d-192">hello anpassade DNS-servern vidarebefordrar sedan toohello Azure rekursiv matchning.</span><span class="sxs-lookup"><span data-stu-id="1558d-192">hello custom DNS server then forwards toohello Azure recursive resolver.</span></span>

        <span data-ttu-id="1558d-193">Den här konfigurationen vägar begäranden om fullständiga domännamn som innehåller hello DNS-suffix hello-virtuellt nätverk toohello anpassad DNS-server.</span><span class="sxs-lookup"><span data-stu-id="1558d-193">This configuration routes requests for fully qualified domain names that contain hello DNS suffix of hello virtual network toohello custom DNS server.</span></span> <span data-ttu-id="1558d-194">Alla andra begäranden (även för offentliga internet-adresser) hanteras av hello lokala DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="1558d-194">All other requests (even for public internet addresses) are handled by hello on-premises DNS server.</span></span>

    * <span data-ttu-id="1558d-195">Om hello fjärrnätverket är en annan Azure-nätverk kan du konfigurera DNS enligt följande:</span><span class="sxs-lookup"><span data-stu-id="1558d-195">If hello remote network is another Azure Virtual Network, configure DNS as follows:</span></span>

        * <span data-ttu-id="1558d-196">__Anpassad DNS__ (i varje virtuellt nätverk):</span><span class="sxs-lookup"><span data-stu-id="1558d-196">__Custom DNS__ (in each virtual network):</span></span>

            * <span data-ttu-id="1558d-197">Begäranden för hello DNS-suffix hello virtuella nätverk vidarebefordras toohello anpassade DNS-servrar.</span><span class="sxs-lookup"><span data-stu-id="1558d-197">Requests for hello DNS suffix of hello virtual networks are forwarded toohello custom DNS servers.</span></span> <span data-ttu-id="1558d-198">hello DNS i varje virtuellt nätverk är ansvarig för att lösa resurser inom sitt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-198">hello DNS in each virtual network is responsible for resolving resources within its network.</span></span>

            * <span data-ttu-id="1558d-199">Vidarebefordra alla andra begäranden toohello Azure rekursiv matchning.</span><span class="sxs-lookup"><span data-stu-id="1558d-199">Forward all other requests toohello Azure recursive resolver.</span></span> <span data-ttu-id="1558d-200">hello rekursiv lösare är ansvarig för lösning av lokala och internet-resurser.</span><span class="sxs-lookup"><span data-stu-id="1558d-200">hello recursive resolver is responsible for resolving local and internet resources.</span></span>

        <span data-ttu-id="1558d-201">hello DNS-server för varje nätverk som vidarebefordrar begäranden toohello andra, baserat på DNS-suffix.</span><span class="sxs-lookup"><span data-stu-id="1558d-201">hello DNS server for each network forwards requests toohello other, based on DNS suffix.</span></span> <span data-ttu-id="1558d-202">Andra begäranden matchas hello Azure rekursiv resolvern.</span><span class="sxs-lookup"><span data-stu-id="1558d-202">Other requests are resolved using hello Azure recursive resolver.</span></span>

    <span data-ttu-id="1558d-203">Ett exempel på varje konfiguration finns hello [exempel: anpassad DNS](#example-dns) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-203">For an example of each configuration, see hello [Example: Custom DNS](#example-dns) section.</span></span>

<span data-ttu-id="1558d-204">Mer information finns i hello [namnmatchning för virtuella datorer och Rollinstanser](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1558d-204">For more information, see hello [Name Resolution for VMs and Role Instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) document.</span></span>

## <a name="directly-connect-toohadoop-services"></a><span data-ttu-id="1558d-205">Ansluta direkt tooHadoop tjänster</span><span class="sxs-lookup"><span data-stu-id="1558d-205">Directly connect tooHadoop services</span></span>

<span data-ttu-id="1558d-206">De flesta dokumentation på HDInsight förutsätter att du har åtkomst toohello klustret via hello internet.</span><span class="sxs-lookup"><span data-stu-id="1558d-206">Most documentation on HDInsight assumes that you have access toohello cluster over hello internet.</span></span> <span data-ttu-id="1558d-207">Till exempel att du kan ansluta toohello klustret på https://CLUSTERNAME.azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="1558d-207">For example, that you can connect toohello cluster at https://CLUSTERNAME.azurehdinsight.net.</span></span> <span data-ttu-id="1558d-208">Den här adressen använder offentliga hello-gatewayen, som inte är tillgängligt om du har använt NSG: er eller udr: er toorestrict åtkomst från hello internet.</span><span class="sxs-lookup"><span data-stu-id="1558d-208">This address uses hello public gateway, which is not available if you have used NSGs or UDRs toorestrict access from hello internet.</span></span>

<span data-ttu-id="1558d-209">tooconnect tooAmbari och andra webbsidor via hello virtuella nätverk, använder du hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="1558d-209">tooconnect tooAmbari and other web pages through hello virtual network, use hello following steps:</span></span>

1. <span data-ttu-id="1558d-210">toodiscover hello interna fullständigt kvalificerade domännamn (FQDN) för hello HDInsight-klusternoder med någon av följande metoder hello:</span><span class="sxs-lookup"><span data-stu-id="1558d-210">toodiscover hello internal fully qualified domain names (FQDN) of hello HDInsight cluster nodes, use one of hello following methods:</span></span>

    ```powershell
    $resourceGroupName = "hello resource group that contains hello virtual network used with HDInsight"

    $clusterNICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group <resourcegroupname> --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    <span data-ttu-id="1558d-211">Hitta hello FQDN för hello huvudnoderna i hello listan över noder returneras, och använder hello FQDN tooconnect tooAmbari och andra webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="1558d-211">In hello list of nodes returned, find hello FQDN for hello head nodes and use hello FQDNs tooconnect tooAmbari and other web services.</span></span> <span data-ttu-id="1558d-212">Till exempel använda `http://<headnode-fqdn>:8080` tooaccess Ambari.</span><span class="sxs-lookup"><span data-stu-id="1558d-212">For example, use `http://<headnode-fqdn>:8080` tooaccess Ambari.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="1558d-213">Vissa tjänster som finns på hello huvudnoderna är bara aktiva på en nod i taget.</span><span class="sxs-lookup"><span data-stu-id="1558d-213">Some services hosted on hello head nodes are only active on one node at a time.</span></span> <span data-ttu-id="1558d-214">Om du försöker komma åt en tjänst på en huvudnod och returnerar ett 404-fel, växla toohello andra huvudnod.</span><span class="sxs-lookup"><span data-stu-id="1558d-214">If you try accessing a service on one head node and it returns a 404 error, switch toohello other head node.</span></span>

2. <span data-ttu-id="1558d-215">toodetermine hello nod och port som en tjänst är tillgänglig, finns hello [portar som används av Hadoop-tjänster på HDInsight](./hdinsight-hadoop-port-settings-for-services.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1558d-215">toodetermine hello node and port that a service is available on, see hello [Ports used by Hadoop services on HDInsight](./hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

## <span data-ttu-id="1558d-216"><a id="networktraffic"></a>Kontrollera nätverkstrafik</span><span class="sxs-lookup"><span data-stu-id="1558d-216"><a id="networktraffic"></a> Controlling network traffic</span></span>

<span data-ttu-id="1558d-217">Nätverkstrafik i en virtuell Azure-nätverk kan kontrolleras med hello följande metoder:</span><span class="sxs-lookup"><span data-stu-id="1558d-217">Network traffic in an Azure Virtual Networks can be controlled using hello following methods:</span></span>

* <span data-ttu-id="1558d-218">**Nätverkssäkerhetsgrupper** (NSG) kan du toofilter inkommande och utgående trafik toohello nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-218">**Network security groups** (NSG) allow you toofilter inbound and outbound traffic toohello network.</span></span> <span data-ttu-id="1558d-219">Mer information finns i hello [filtrera nätverkstrafik med nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1558d-219">For more information, see hello [Filter network traffic with network security groups](../virtual-network/virtual-networks-nsg.md) document.</span></span>

    > [!WARNING]
    > <span data-ttu-id="1558d-220">HDInsight stöder inte begränsa utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="1558d-220">HDInsight does not support restricting outbound traffic.</span></span>

* <span data-ttu-id="1558d-221">**Användardefinierade vägar** (UDR) definierar hur trafiken flödar mellan resurser i hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-221">**User-defined routes** (UDR) define how traffic flows between resources in hello network.</span></span> <span data-ttu-id="1558d-222">Mer information finns i hello [användardefinierade vägar och IP-vidarebefordring](../virtual-network/virtual-networks-udr-overview.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1558d-222">For more information, see hello [User-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md) document.</span></span>

* <span data-ttu-id="1558d-223">**Nätverks-virtuella installationer** replikera hello funktionerna på enheter, till exempel brandväggar och routrar.</span><span class="sxs-lookup"><span data-stu-id="1558d-223">**Network virtual appliances** replicate hello functionality of devices such as firewalls and routers.</span></span> <span data-ttu-id="1558d-224">Mer information finns i hello [nätverksinstallationer](https://azure.microsoft.com/solutions/network-appliances) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1558d-224">For more information, see hello [Network Appliances](https://azure.microsoft.com/solutions/network-appliances) document.</span></span>

<span data-ttu-id="1558d-225">Som en hanterad tjänst kräver HDInsight obegränsad åtkomst tooAzure hälso- och tjänster i hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="1558d-225">As a managed service, HDInsight requires unrestricted access tooAzure health and management services in hello Azure cloud.</span></span> <span data-ttu-id="1558d-226">När du använder NSG: er och udr: er, måste du se till att HDInsight dessa tjänster kan fortfarande kommunicera med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1558d-226">When using NSGs and UDRs, you must ensure that HDInsight these services can still communicate with HDInsight.</span></span>

<span data-ttu-id="1558d-227">HDInsight visar tjänster på flera portar.</span><span class="sxs-lookup"><span data-stu-id="1558d-227">HDInsight exposes services on several ports.</span></span> <span data-ttu-id="1558d-228">När du använder en virtuell installation brandvägg måste du tillåta trafik på hello portar som används för dessa tjänster.</span><span class="sxs-lookup"><span data-stu-id="1558d-228">When using a virtual appliance firewall, you must allow traffic on hello ports used for these services.</span></span> <span data-ttu-id="1558d-229">Mer information finns i avsnittet hello [portar som krävs].</span><span class="sxs-lookup"><span data-stu-id="1558d-229">For more information, see hello [Required ports] section.</span></span>

### <span data-ttu-id="1558d-230"><a id="hdinsight-ip"></a>HDInsight med nätverkssäkerhetsgrupper och användardefinierade vägar</span><span class="sxs-lookup"><span data-stu-id="1558d-230"><a id="hdinsight-ip"></a> HDInsight with network security groups and user-defined routes</span></span>

<span data-ttu-id="1558d-231">Om du tänker använda **nätverkssäkerhetsgrupper** eller **användardefinierade vägar** toocontrol nätverkstrafik, utför följande åtgärder innan du installerar HDInsight hello:</span><span class="sxs-lookup"><span data-stu-id="1558d-231">If you plan on using **network security groups** or **user-defined routes** toocontrol network traffic, perform hello following actions before installing HDInsight:</span></span>

1. <span data-ttu-id="1558d-232">Identifiera hello Azure-region som du planerar toouse för HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1558d-232">Identify hello Azure region that you plan toouse for HDInsight.</span></span>

2. <span data-ttu-id="1558d-233">Identifiera hello IP-adresser som krävs av HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1558d-233">Identify hello IP addresses required by HDInsight.</span></span> <span data-ttu-id="1558d-234">Mer information finns i hello [IP-adresser som krävs av HDInsight](#hdinsight-ip) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-234">For more information, see hello [IP Addresses required by HDInsight](#hdinsight-ip) section.</span></span>

3. <span data-ttu-id="1558d-235">Skapa eller ändra hello nätverkssäkerhetsgrupper eller användardefinierade vägar för hello-undernätet som du planerar tooinstall HDInsight till.</span><span class="sxs-lookup"><span data-stu-id="1558d-235">Create or modify hello network security groups or user-defined routes for hello subnet that you plan tooinstall HDInsight into.</span></span>

    * <span data-ttu-id="1558d-236">__Nätverkssäkerhetsgrupper__: Tillåt __inkommande__ trafik på port __443__ från hello IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="1558d-236">__Network security groups__: allow __inbound__ traffic on port __443__ from hello IP addresses.</span></span>
    * <span data-ttu-id="1558d-237">__Användardefinierade vägar__: skapa en väg tooeach IP-adress och ange hello __nästa hopptyp__ too__Internet__.</span><span class="sxs-lookup"><span data-stu-id="1558d-237">__User-defined routes__: create a route tooeach IP address and set hello __Next hop type__ too__Internet__.</span></span>

<span data-ttu-id="1558d-238">Mer information om nätverkssäkerhetsgrupper eller användardefinierade vägar finns hello följande dokumentation:</span><span class="sxs-lookup"><span data-stu-id="1558d-238">For more information on network security groups or user-defined routes, see hello following documentation:</span></span>

* [<span data-ttu-id="1558d-239">Nätverkssäkerhetsgrupp</span><span class="sxs-lookup"><span data-stu-id="1558d-239">Network security group</span></span>](../virtual-network/virtual-networks-nsg.md)

* [<span data-ttu-id="1558d-240">Användardefinierade vägar</span><span class="sxs-lookup"><span data-stu-id="1558d-240">User-defined routes</span></span>](../virtual-network/virtual-networks-udr-overview.md)

#### <a name="forced-tunneling"></a><span data-ttu-id="1558d-241">Tvingad tunneltrafik</span><span class="sxs-lookup"><span data-stu-id="1558d-241">Forced tunneling</span></span>

<span data-ttu-id="1558d-242">Tvingad tunneling är en användardefinierad konfiguration där all trafik från ett undernät är framtvingad tooa specifika nätverk eller plats, till exempel ditt lokala nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-242">Forced tunneling is a user-defined routing configuration where all traffic from a subnet is forced tooa specific network or location, such as your on-premises network.</span></span> <span data-ttu-id="1558d-243">HDInsight har __inte__ stöd Tvingad tunneltrafik.</span><span class="sxs-lookup"><span data-stu-id="1558d-243">HDInsight does __not__ support forced tunneling.</span></span>

## <span data-ttu-id="1558d-244"><a id="hdinsight-ip"></a>Den begärda IP-adresser</span><span class="sxs-lookup"><span data-stu-id="1558d-244"><a id="hdinsight-ip"></a> Required IP addresses</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1558d-245">hello Azure hälsa och hanteringstjänster måste vara kan toocommunicate med HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1558d-245">hello Azure health and management services must be able toocommunicate with HDInsight.</span></span> <span data-ttu-id="1558d-246">Om du använder nätverkssäkerhetsgrupper eller användardefinierade vägar tillåta trafik från hello IP-adresser för dessa tjänster tooreach HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1558d-246">If you use network security groups or user-defined routes, allow traffic from hello IP addresses for these services tooreach HDInsight.</span></span>
>
> <span data-ttu-id="1558d-247">Om du inte använder nätverkssäkerhetsgrupper eller användardefinierade vägar toocontrol trafik, kan du ignorera det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1558d-247">If you do not use network security groups or user-defined routes toocontrol traffic, you can ignore this section.</span></span>

<span data-ttu-id="1558d-248">Om du använder nätverkssäkerhetsgrupper eller användardefinierade vägar, måste du tillåta trafik från hello Azure hälsa och management services tooreach HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1558d-248">If you use network security groups or user-defined routes, you must allow traffic from hello Azure health and management services tooreach HDInsight.</span></span> <span data-ttu-id="1558d-249">Använd hello följande steg toofind hello IP-adresser som får:</span><span class="sxs-lookup"><span data-stu-id="1558d-249">Use hello following steps toofind hello IP addresses that must be allowed:</span></span>

1. <span data-ttu-id="1558d-250">Du måste alltid tillåta trafik från hello efter IP-adresser:</span><span class="sxs-lookup"><span data-stu-id="1558d-250">You must always allow traffic from hello following IP addresses:</span></span>

    | <span data-ttu-id="1558d-251">IP-adress</span><span class="sxs-lookup"><span data-stu-id="1558d-251">IP address</span></span> | <span data-ttu-id="1558d-252">Tillåtna port</span><span class="sxs-lookup"><span data-stu-id="1558d-252">Allowed port</span></span> | <span data-ttu-id="1558d-253">Riktning</span><span class="sxs-lookup"><span data-stu-id="1558d-253">Direction</span></span> |
    | ---- | ----- | ----- |
    | <span data-ttu-id="1558d-254">168.61.49.99</span><span class="sxs-lookup"><span data-stu-id="1558d-254">168.61.49.99</span></span> | <span data-ttu-id="1558d-255">443</span><span class="sxs-lookup"><span data-stu-id="1558d-255">443</span></span> | <span data-ttu-id="1558d-256">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-256">Inbound</span></span> |
    | <span data-ttu-id="1558d-257">23.99.5.239</span><span class="sxs-lookup"><span data-stu-id="1558d-257">23.99.5.239</span></span> | <span data-ttu-id="1558d-258">443</span><span class="sxs-lookup"><span data-stu-id="1558d-258">443</span></span> | <span data-ttu-id="1558d-259">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-259">Inbound</span></span> |
    | <span data-ttu-id="1558d-260">168.61.48.131</span><span class="sxs-lookup"><span data-stu-id="1558d-260">168.61.48.131</span></span> | <span data-ttu-id="1558d-261">443</span><span class="sxs-lookup"><span data-stu-id="1558d-261">443</span></span> | <span data-ttu-id="1558d-262">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-262">Inbound</span></span> |
    | <span data-ttu-id="1558d-263">138.91.141.162</span><span class="sxs-lookup"><span data-stu-id="1558d-263">138.91.141.162</span></span> | <span data-ttu-id="1558d-264">443</span><span class="sxs-lookup"><span data-stu-id="1558d-264">443</span></span> | <span data-ttu-id="1558d-265">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-265">Inbound</span></span> |

2. <span data-ttu-id="1558d-266">Om ditt HDInsight-kluster finns i något av följande regioner hello, måste du tillåta trafik från hello IP-adresser som anges för hello region:</span><span class="sxs-lookup"><span data-stu-id="1558d-266">If your HDInsight cluster is in one of hello following regions, then you must allow traffic from hello IP addresses listed for hello region:</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="1558d-267">Om inte hello Azure-region som du använder visas endast Använd hello fyra IP-adresser från steg 1.</span><span class="sxs-lookup"><span data-stu-id="1558d-267">If hello Azure region you are using is not listed, then only use hello four IP addresses from step 1.</span></span>

    | <span data-ttu-id="1558d-268">Land/region</span><span class="sxs-lookup"><span data-stu-id="1558d-268">Country</span></span> | <span data-ttu-id="1558d-269">Region</span><span class="sxs-lookup"><span data-stu-id="1558d-269">Region</span></span> | <span data-ttu-id="1558d-270">Tillåtna IP-adresser</span><span class="sxs-lookup"><span data-stu-id="1558d-270">Allowed IP addresses</span></span> | <span data-ttu-id="1558d-271">Tillåtna port</span><span class="sxs-lookup"><span data-stu-id="1558d-271">Allowed port</span></span> | <span data-ttu-id="1558d-272">Riktning</span><span class="sxs-lookup"><span data-stu-id="1558d-272">Direction</span></span> |
    | ---- | ---- | ---- | ---- | ----- |
    | <span data-ttu-id="1558d-273">Asien</span><span class="sxs-lookup"><span data-stu-id="1558d-273">Asia</span></span> | <span data-ttu-id="1558d-274">Östasien</span><span class="sxs-lookup"><span data-stu-id="1558d-274">East Asia</span></span> | <span data-ttu-id="1558d-275">23.102.235.122</span><span class="sxs-lookup"><span data-stu-id="1558d-275">23.102.235.122</span></span></br><span data-ttu-id="1558d-276">52.175.38.134</span><span class="sxs-lookup"><span data-stu-id="1558d-276">52.175.38.134</span></span> | <span data-ttu-id="1558d-277">443</span><span class="sxs-lookup"><span data-stu-id="1558d-277">443</span></span> | <span data-ttu-id="1558d-278">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-278">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="1558d-279">Sydostasien</span><span class="sxs-lookup"><span data-stu-id="1558d-279">Southeast Asia</span></span> | <span data-ttu-id="1558d-280">13.76.245.160</span><span class="sxs-lookup"><span data-stu-id="1558d-280">13.76.245.160</span></span></br><span data-ttu-id="1558d-281">13.76.136.249</span><span class="sxs-lookup"><span data-stu-id="1558d-281">13.76.136.249</span></span> | <span data-ttu-id="1558d-282">443</span><span class="sxs-lookup"><span data-stu-id="1558d-282">443</span></span> | <span data-ttu-id="1558d-283">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-283">Inbound</span></span> |
    | <span data-ttu-id="1558d-284">Australien</span><span class="sxs-lookup"><span data-stu-id="1558d-284">Australia</span></span> | <span data-ttu-id="1558d-285">Östra Australien</span><span class="sxs-lookup"><span data-stu-id="1558d-285">Australia East</span></span> | <span data-ttu-id="1558d-286">104.210.84.115</span><span class="sxs-lookup"><span data-stu-id="1558d-286">104.210.84.115</span></span></br><span data-ttu-id="1558d-287">13.75.152.195</span><span class="sxs-lookup"><span data-stu-id="1558d-287">13.75.152.195</span></span> | <span data-ttu-id="1558d-288">443</span><span class="sxs-lookup"><span data-stu-id="1558d-288">443</span></span> | <span data-ttu-id="1558d-289">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-289">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="1558d-290">Sydöstra Australien</span><span class="sxs-lookup"><span data-stu-id="1558d-290">Australia Southeast</span></span> | <span data-ttu-id="1558d-291">13.77.2.56</span><span class="sxs-lookup"><span data-stu-id="1558d-291">13.77.2.56</span></span></br><span data-ttu-id="1558d-292">13.77.2.94</span><span class="sxs-lookup"><span data-stu-id="1558d-292">13.77.2.94</span></span> | <span data-ttu-id="1558d-293">443</span><span class="sxs-lookup"><span data-stu-id="1558d-293">443</span></span> | <span data-ttu-id="1558d-294">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-294">Inbound</span></span> |
    | <span data-ttu-id="1558d-295">Brasilien</span><span class="sxs-lookup"><span data-stu-id="1558d-295">Brazil</span></span> | <span data-ttu-id="1558d-296">Södra Brasilien</span><span class="sxs-lookup"><span data-stu-id="1558d-296">Brazil South</span></span> | <span data-ttu-id="1558d-297">191.235.84.104</span><span class="sxs-lookup"><span data-stu-id="1558d-297">191.235.84.104</span></span></br><span data-ttu-id="1558d-298">191.235.87.113</span><span class="sxs-lookup"><span data-stu-id="1558d-298">191.235.87.113</span></span> | <span data-ttu-id="1558d-299">443</span><span class="sxs-lookup"><span data-stu-id="1558d-299">443</span></span> | <span data-ttu-id="1558d-300">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-300">Inbound</span></span> |
    | <span data-ttu-id="1558d-301">Kanada</span><span class="sxs-lookup"><span data-stu-id="1558d-301">Canada</span></span> | <span data-ttu-id="1558d-302">Östra Kanada</span><span class="sxs-lookup"><span data-stu-id="1558d-302">Canada East</span></span> | <span data-ttu-id="1558d-303">52.229.127.96</span><span class="sxs-lookup"><span data-stu-id="1558d-303">52.229.127.96</span></span></br><span data-ttu-id="1558d-304">52.229.123.172</span><span class="sxs-lookup"><span data-stu-id="1558d-304">52.229.123.172</span></span> | <span data-ttu-id="1558d-305">443</span><span class="sxs-lookup"><span data-stu-id="1558d-305">443</span></span> | <span data-ttu-id="1558d-306">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-306">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="1558d-307">Centrala Kanada</span><span class="sxs-lookup"><span data-stu-id="1558d-307">Canada Central</span></span> | <span data-ttu-id="1558d-308">52.228.37.66</span><span class="sxs-lookup"><span data-stu-id="1558d-308">52.228.37.66</span></span></br><span data-ttu-id="1558d-309">52.228.45.222</span><span class="sxs-lookup"><span data-stu-id="1558d-309">52.228.45.222</span></span> | <span data-ttu-id="1558d-310">443</span><span class="sxs-lookup"><span data-stu-id="1558d-310">443</span></span> | <span data-ttu-id="1558d-311">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-311">Inbound</span></span> |
    | <span data-ttu-id="1558d-312">Kina</span><span class="sxs-lookup"><span data-stu-id="1558d-312">China</span></span> | <span data-ttu-id="1558d-313">Norra Kina</span><span class="sxs-lookup"><span data-stu-id="1558d-313">China North</span></span> | <span data-ttu-id="1558d-314">42.159.96.170</span><span class="sxs-lookup"><span data-stu-id="1558d-314">42.159.96.170</span></span></br><span data-ttu-id="1558d-315">139.217.2.219</span><span class="sxs-lookup"><span data-stu-id="1558d-315">139.217.2.219</span></span> | <span data-ttu-id="1558d-316">443</span><span class="sxs-lookup"><span data-stu-id="1558d-316">443</span></span> | <span data-ttu-id="1558d-317">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-317">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="1558d-318">Östra Kina</span><span class="sxs-lookup"><span data-stu-id="1558d-318">China East</span></span> | <span data-ttu-id="1558d-319">42.159.198.178</span><span class="sxs-lookup"><span data-stu-id="1558d-319">42.159.198.178</span></span></br><span data-ttu-id="1558d-320">42.159.234.157</span><span class="sxs-lookup"><span data-stu-id="1558d-320">42.159.234.157</span></span> | <span data-ttu-id="1558d-321">443</span><span class="sxs-lookup"><span data-stu-id="1558d-321">443</span></span> | <span data-ttu-id="1558d-322">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-322">Inbound</span></span> |
    | <span data-ttu-id="1558d-323">Europa</span><span class="sxs-lookup"><span data-stu-id="1558d-323">Europe</span></span> | <span data-ttu-id="1558d-324">Norra Europa</span><span class="sxs-lookup"><span data-stu-id="1558d-324">North Europe</span></span> | <span data-ttu-id="1558d-325">52.164.210.96</span><span class="sxs-lookup"><span data-stu-id="1558d-325">52.164.210.96</span></span></br><span data-ttu-id="1558d-326">13.74.153.132</span><span class="sxs-lookup"><span data-stu-id="1558d-326">13.74.153.132</span></span> | <span data-ttu-id="1558d-327">443</span><span class="sxs-lookup"><span data-stu-id="1558d-327">443</span></span> | <span data-ttu-id="1558d-328">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-328">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="1558d-329">Västra Europa</span><span class="sxs-lookup"><span data-stu-id="1558d-329">West Europe</span></span>| <span data-ttu-id="1558d-330">52.166.243.90</span><span class="sxs-lookup"><span data-stu-id="1558d-330">52.166.243.90</span></span></br><span data-ttu-id="1558d-331">52.174.36.244</span><span class="sxs-lookup"><span data-stu-id="1558d-331">52.174.36.244</span></span> | <span data-ttu-id="1558d-332">443</span><span class="sxs-lookup"><span data-stu-id="1558d-332">443</span></span> | <span data-ttu-id="1558d-333">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-333">Inbound</span></span> |
    | <span data-ttu-id="1558d-334">Tyskland</span><span class="sxs-lookup"><span data-stu-id="1558d-334">Germany</span></span> | <span data-ttu-id="1558d-335">Centrala Tyskland</span><span class="sxs-lookup"><span data-stu-id="1558d-335">Germany Central</span></span> | <span data-ttu-id="1558d-336">51.4.146.68</span><span class="sxs-lookup"><span data-stu-id="1558d-336">51.4.146.68</span></span></br><span data-ttu-id="1558d-337">51.4.146.80</span><span class="sxs-lookup"><span data-stu-id="1558d-337">51.4.146.80</span></span> | <span data-ttu-id="1558d-338">443</span><span class="sxs-lookup"><span data-stu-id="1558d-338">443</span></span> | <span data-ttu-id="1558d-339">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-339">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="1558d-340">Nordöstra Tyskland</span><span class="sxs-lookup"><span data-stu-id="1558d-340">Germany Northeast</span></span> | <span data-ttu-id="1558d-341">51.5.150.132</span><span class="sxs-lookup"><span data-stu-id="1558d-341">51.5.150.132</span></span></br><span data-ttu-id="1558d-342">51.5.144.101</span><span class="sxs-lookup"><span data-stu-id="1558d-342">51.5.144.101</span></span> | <span data-ttu-id="1558d-343">443</span><span class="sxs-lookup"><span data-stu-id="1558d-343">443</span></span> | <span data-ttu-id="1558d-344">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-344">Inbound</span></span> |
    | <span data-ttu-id="1558d-345">Indien</span><span class="sxs-lookup"><span data-stu-id="1558d-345">India</span></span> | <span data-ttu-id="1558d-346">Indien, centrala</span><span class="sxs-lookup"><span data-stu-id="1558d-346">Central India</span></span> | <span data-ttu-id="1558d-347">52.172.153.209</span><span class="sxs-lookup"><span data-stu-id="1558d-347">52.172.153.209</span></span></br><span data-ttu-id="1558d-348">52.172.152.49</span><span class="sxs-lookup"><span data-stu-id="1558d-348">52.172.152.49</span></span> | <span data-ttu-id="1558d-349">443</span><span class="sxs-lookup"><span data-stu-id="1558d-349">443</span></span> | <span data-ttu-id="1558d-350">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-350">Inbound</span></span> |
    | <span data-ttu-id="1558d-351">Japan</span><span class="sxs-lookup"><span data-stu-id="1558d-351">Japan</span></span> | <span data-ttu-id="1558d-352">Östra Japan</span><span class="sxs-lookup"><span data-stu-id="1558d-352">Japan East</span></span> | <span data-ttu-id="1558d-353">13.78.125.90</span><span class="sxs-lookup"><span data-stu-id="1558d-353">13.78.125.90</span></span></br><span data-ttu-id="1558d-354">13.78.89.60</span><span class="sxs-lookup"><span data-stu-id="1558d-354">13.78.89.60</span></span> | <span data-ttu-id="1558d-355">443</span><span class="sxs-lookup"><span data-stu-id="1558d-355">443</span></span> | <span data-ttu-id="1558d-356">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-356">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="1558d-357">Västra Japan</span><span class="sxs-lookup"><span data-stu-id="1558d-357">Japan West</span></span> | <span data-ttu-id="1558d-358">40.74.125.69</span><span class="sxs-lookup"><span data-stu-id="1558d-358">40.74.125.69</span></span></br><span data-ttu-id="1558d-359">138.91.29.150</span><span class="sxs-lookup"><span data-stu-id="1558d-359">138.91.29.150</span></span> | <span data-ttu-id="1558d-360">443</span><span class="sxs-lookup"><span data-stu-id="1558d-360">443</span></span> | <span data-ttu-id="1558d-361">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-361">Inbound</span></span> |
    | <span data-ttu-id="1558d-362">Korea</span><span class="sxs-lookup"><span data-stu-id="1558d-362">Korea</span></span> | <span data-ttu-id="1558d-363">Centrala Korea</span><span class="sxs-lookup"><span data-stu-id="1558d-363">Korea Central</span></span> | <span data-ttu-id="1558d-364">52.231.39.142</span><span class="sxs-lookup"><span data-stu-id="1558d-364">52.231.39.142</span></span></br><span data-ttu-id="1558d-365">52.231.36.209</span><span class="sxs-lookup"><span data-stu-id="1558d-365">52.231.36.209</span></span> | <span data-ttu-id="1558d-366">433</span><span class="sxs-lookup"><span data-stu-id="1558d-366">433</span></span> | <span data-ttu-id="1558d-367">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-367">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="1558d-368">Sydkorea</span><span class="sxs-lookup"><span data-stu-id="1558d-368">Korea South</span></span> | <span data-ttu-id="1558d-369">52.231.203.16</span><span class="sxs-lookup"><span data-stu-id="1558d-369">52.231.203.16</span></span></br><span data-ttu-id="1558d-370">52.231.205.214</span><span class="sxs-lookup"><span data-stu-id="1558d-370">52.231.205.214</span></span> | <span data-ttu-id="1558d-371">443</span><span class="sxs-lookup"><span data-stu-id="1558d-371">443</span></span> | <span data-ttu-id="1558d-372">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-372">Inbound</span></span>
    | <span data-ttu-id="1558d-373">Storbritannien</span><span class="sxs-lookup"><span data-stu-id="1558d-373">United Kingdom</span></span> | <span data-ttu-id="1558d-374">Storbritannien, västra</span><span class="sxs-lookup"><span data-stu-id="1558d-374">UK West</span></span> | <span data-ttu-id="1558d-375">51.141.13.110</span><span class="sxs-lookup"><span data-stu-id="1558d-375">51.141.13.110</span></span></br><span data-ttu-id="1558d-376">51.141.7.20</span><span class="sxs-lookup"><span data-stu-id="1558d-376">51.141.7.20</span></span> | <span data-ttu-id="1558d-377">443</span><span class="sxs-lookup"><span data-stu-id="1558d-377">443</span></span> | <span data-ttu-id="1558d-378">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-378">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="1558d-379">Storbritannien, södra</span><span class="sxs-lookup"><span data-stu-id="1558d-379">UK South</span></span> | <span data-ttu-id="1558d-380">51.140.47.39</span><span class="sxs-lookup"><span data-stu-id="1558d-380">51.140.47.39</span></span></br><span data-ttu-id="1558d-381">51.140.52.16</span><span class="sxs-lookup"><span data-stu-id="1558d-381">51.140.52.16</span></span> | <span data-ttu-id="1558d-382">443</span><span class="sxs-lookup"><span data-stu-id="1558d-382">443</span></span> | <span data-ttu-id="1558d-383">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-383">Inbound</span></span> |
    | <span data-ttu-id="1558d-384">USA</span><span class="sxs-lookup"><span data-stu-id="1558d-384">United States</span></span> | <span data-ttu-id="1558d-385">Centrala USA</span><span class="sxs-lookup"><span data-stu-id="1558d-385">Central US</span></span> | <span data-ttu-id="1558d-386">13.67.223.215</span><span class="sxs-lookup"><span data-stu-id="1558d-386">13.67.223.215</span></span></br><span data-ttu-id="1558d-387">40.86.83.253</span><span class="sxs-lookup"><span data-stu-id="1558d-387">40.86.83.253</span></span> | <span data-ttu-id="1558d-388">443</span><span class="sxs-lookup"><span data-stu-id="1558d-388">443</span></span> | <span data-ttu-id="1558d-389">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-389">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="1558d-390">Norra centrala USA</span><span class="sxs-lookup"><span data-stu-id="1558d-390">North Central US</span></span> | <span data-ttu-id="1558d-391">157.56.8.38</span><span class="sxs-lookup"><span data-stu-id="1558d-391">157.56.8.38</span></span></br><span data-ttu-id="1558d-392">157.55.213.99</span><span class="sxs-lookup"><span data-stu-id="1558d-392">157.55.213.99</span></span> | <span data-ttu-id="1558d-393">443</span><span class="sxs-lookup"><span data-stu-id="1558d-393">443</span></span> | <span data-ttu-id="1558d-394">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-394">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="1558d-395">Västra centrala USA</span><span class="sxs-lookup"><span data-stu-id="1558d-395">West Central US</span></span> | <span data-ttu-id="1558d-396">52.161.23.15</span><span class="sxs-lookup"><span data-stu-id="1558d-396">52.161.23.15</span></span></br><span data-ttu-id="1558d-397">52.161.10.167</span><span class="sxs-lookup"><span data-stu-id="1558d-397">52.161.10.167</span></span> | <span data-ttu-id="1558d-398">443</span><span class="sxs-lookup"><span data-stu-id="1558d-398">443</span></span> | <span data-ttu-id="1558d-399">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-399">Inbound</span></span> |
    | &nbsp; | <span data-ttu-id="1558d-400">Västra USA 2</span><span class="sxs-lookup"><span data-stu-id="1558d-400">West US 2</span></span> | <span data-ttu-id="1558d-401">52.175.211.210</span><span class="sxs-lookup"><span data-stu-id="1558d-401">52.175.211.210</span></span></br><span data-ttu-id="1558d-402">52.175.222.222</span><span class="sxs-lookup"><span data-stu-id="1558d-402">52.175.222.222</span></span> | <span data-ttu-id="1558d-403">443</span><span class="sxs-lookup"><span data-stu-id="1558d-403">443</span></span> | <span data-ttu-id="1558d-404">Inkommande</span><span class="sxs-lookup"><span data-stu-id="1558d-404">Inbound</span></span> |

    <span data-ttu-id="1558d-405">Information om hello IP-adresserna för toouse för Azure Government, finns hello [Azure Government Intelligence + analys](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1558d-405">For information on hello IP addresses toouse for Azure Government, see hello [Azure Government Intelligence + Analytics](https://docs.microsoft.com/azure/azure-government/documentation-government-services-intelligenceandanalytics) document.</span></span>

3. <span data-ttu-id="1558d-406">Om du använder en anpassad DNS-server med det virtuella nätverket måste du också tillåta åtkomst från __168.63.129.16__.</span><span class="sxs-lookup"><span data-stu-id="1558d-406">If you use a custom DNS server with your virtual network, you must also allow access from __168.63.129.16__.</span></span> <span data-ttu-id="1558d-407">Den här adressen är Azures rekursiv matchning.</span><span class="sxs-lookup"><span data-stu-id="1558d-407">This address is Azure's recursive resolver.</span></span> <span data-ttu-id="1558d-408">Mer information finns i hello [namnmatchning för virtuella datorer och rollen instanser](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1558d-408">For more information, see hello [Name resolution for VMs and Role instances](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md) document.</span></span>

<span data-ttu-id="1558d-409">Mer information finns i hello [styra nätverkstrafiken](#networktraffic) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-409">For more information, see hello [Controlling network traffic](#networktraffic) section.</span></span>

## <span data-ttu-id="1558d-410"><a id="hdinsight-ports"></a>Portar som krävs</span><span class="sxs-lookup"><span data-stu-id="1558d-410"><a id="hdinsight-ports"></a> Required ports</span></span>

<span data-ttu-id="1558d-411">Om du tänker använda ett nätverk **virtuell installation brandväggen** toosecure hello virtuellt nätverk, måste du tillåta utgående trafik på hello följande portar:</span><span class="sxs-lookup"><span data-stu-id="1558d-411">If you plan on using a network **virtual appliance firewall** toosecure hello virtual network, you must allow outbound traffic on hello following ports:</span></span>

* <span data-ttu-id="1558d-412">53</span><span class="sxs-lookup"><span data-stu-id="1558d-412">53</span></span>
* <span data-ttu-id="1558d-413">443</span><span class="sxs-lookup"><span data-stu-id="1558d-413">443</span></span>
* <span data-ttu-id="1558d-414">1433</span><span class="sxs-lookup"><span data-stu-id="1558d-414">1433</span></span>
* <span data-ttu-id="1558d-415">11000-11999</span><span class="sxs-lookup"><span data-stu-id="1558d-415">11000-11999</span></span>
* <span data-ttu-id="1558d-416">14000-14999</span><span class="sxs-lookup"><span data-stu-id="1558d-416">14000-14999</span></span>

<span data-ttu-id="1558d-417">En lista över portar för vissa tjänster finns hello [portar som används av Hadoop-tjänster på HDInsight](hdinsight-hadoop-port-settings-for-services.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1558d-417">For a list of ports for specific services, see hello [Ports used by Hadoop services on HDInsight](hdinsight-hadoop-port-settings-for-services.md) document.</span></span>

<span data-ttu-id="1558d-418">Mer information om brandväggsregler för virtuella installationer finns hello [virtuella utrustningsscenario](../virtual-network/virtual-network-scenario-udr-gw-nva.md) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="1558d-418">For more information on firewall rules for virtual appliances, see hello [virtual appliance scenario](../virtual-network/virtual-network-scenario-udr-gw-nva.md) document.</span></span>

## <span data-ttu-id="1558d-419"><a id="hdinsight-nsg"></a>Exempel: nätverkssäkerhetsgrupper med HDInsight</span><span class="sxs-lookup"><span data-stu-id="1558d-419"><a id="hdinsight-nsg"></a>Example: network security groups with HDInsight</span></span>

<span data-ttu-id="1558d-420">hello exemplen i det här avsnittet visar hur toocreate nätverkssäkerhetsgruppen regler som tillåter HDInsight toocommunicate med hello Azure hanteringstjänster.</span><span class="sxs-lookup"><span data-stu-id="1558d-420">hello examples in this section demonstrate how toocreate network security group rules that allow HDInsight toocommunicate with hello Azure management services.</span></span> <span data-ttu-id="1558d-421">Innan du använder hello exempel justera hello IP-adresser toomatch hello de för hello Azure-region som du använder.</span><span class="sxs-lookup"><span data-stu-id="1558d-421">Before using hello examples, adjust hello IP addresses toomatch hello ones for hello Azure region you are using.</span></span> <span data-ttu-id="1558d-422">Du hittar den här informationen i hello [HDInsight med nätverkssäkerhetsgrupper och användardefinierade vägar](#hdinsight-ip) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-422">You can find this information in hello [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

### <a name="azure-resource-management-template"></a><span data-ttu-id="1558d-423">Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="1558d-423">Azure Resource Management template</span></span>

<span data-ttu-id="1558d-424">hello skapas följande resurshantering ett virtuellt nätverk som begränsar inkommande trafik, men tillåter trafik från hello IP-adresser som krävs av HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1558d-424">hello following Resource Management template creates a virtual network that restricts inbound traffic, but allows traffic from hello IP addresses required by HDInsight.</span></span> <span data-ttu-id="1558d-425">Den här mallen skapar också ett HDInsight-kluster i hello virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-425">This template also creates an HDInsight cluster in hello virtual network.</span></span>

* [<span data-ttu-id="1558d-426">Distribuera en skyddad virtuell Azure-nätverk och ett HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="1558d-426">Deploy a secured Azure Virtual Network and an HDInsight Hadoop cluster</span></span>](https://azure.microsoft.com/resources/templates/101-hdinsight-secure-vnet/)

> [!IMPORTANT]
> <span data-ttu-id="1558d-427">Ändra hello IP-adresser som används i det här exemplet toomatch hello Azure-region som du använder.</span><span class="sxs-lookup"><span data-stu-id="1558d-427">Change hello IP addresses used in this example toomatch hello Azure region you are using.</span></span> <span data-ttu-id="1558d-428">Du hittar den här informationen i hello [HDInsight med nätverkssäkerhetsgrupper och användardefinierade vägar](#hdinsight-ip) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-428">You can find this information in hello [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

### <a name="azure-powershell"></a><span data-ttu-id="1558d-429">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="1558d-429">Azure PowerShell</span></span>

<span data-ttu-id="1558d-430">Använd följande PowerShell-skriptet toocreate ett virtuellt nätverk begränsar inkommande trafik som tillåter trafik från hello IP-adresser för hello Nordeuropa region hello.</span><span class="sxs-lookup"><span data-stu-id="1558d-430">Use hello following PowerShell script toocreate a virtual network that restricts inbound traffic and allows traffic from hello IP addresses for hello North Europe region.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1558d-431">Ändra hello IP-adresser som används i det här exemplet toomatch hello Azure-region som du använder.</span><span class="sxs-lookup"><span data-stu-id="1558d-431">Change hello IP addresses used in this example toomatch hello Azure region you are using.</span></span> <span data-ttu-id="1558d-432">Du hittar den här informationen i hello [HDInsight med nätverkssäkerhetsgrupper och användardefinierade vägar](#hdinsight-ip) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-432">You can find this information in hello [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

```powershell
$vnetName = "Replace with your virtual network name"
$resourceGroupName = "Replace with hello resource group hello virtual network is in"
$subnetName = "Replace with hello name of hello subnet that you plan toouse for HDInsight"
# Get hello Virtual Network object
$vnet = Get-AzureRmVirtualNetwork `
    -Name $vnetName `
    -ResourceGroupName $resourceGroupName
# Get hello region hello Virtual network is in.
$location = $vnet.Location
# Get hello subnet object
$subnet = $vnet.Subnets | Where-Object Name -eq $subnetName
# Create a Network Security Group.
# And add exemptions for hello HDInsight health and management services.
$nsg = New-AzureRmNetworkSecurityGroup `
    -Name "hdisecure" `
    -ResourceGroupName $resourceGroupName `
    -Location $location `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -name "hdirule1" `
        -Description "HDI health and management address 52.164.210.96" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "52.164.210.96" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 300 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 13.74.153.132" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "13.74.153.132" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 301 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.49.99" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.49.99" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 302 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 23.99.5.239" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "23.99.5.239" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 303 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 168.61.48.131" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "168.61.48.131" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 304 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "hdirule2" `
        -Description "HDI health and management 138.91.141.162" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "443" `
        -SourceAddressPrefix "138.91.141.162" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Allow `
        -Priority 305 `
        -Direction Inbound `
    | Add-AzureRmNetworkSecurityRuleConfig `
        -Name "blockeverything" `
        -Description "Block everything else" `
        -Protocol "*" `
        -SourcePortRange "*" `
        -DestinationPortRange "*" `
        -SourceAddressPrefix "Internet" `
        -DestinationAddressPrefix "VirtualNetwork" `
        -Access Deny `
        -Priority 500 `
        -Direction Inbound
# Set hello changes toohello security group
Set-AzureRmNetworkSecurityGroup -NetworkSecurityGroup $nsg
# Apply hello NSG toohello subnet
Set-AzureRmVirtualNetworkSubnetConfig `
    -VirtualNetwork $vnet `
    -Name $subnetName `
    -AddressPrefix $subnet.AddressPrefix `
    -NetworkSecurityGroup $nsg
```

> [!IMPORTANT]
> <span data-ttu-id="1558d-433">Det här exemplet visar hur tooadd regler tooallow inkommande trafik på hello som krävs för IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="1558d-433">This example demonstrates how tooadd rules tooallow inbound traffic on hello required IP addresses.</span></span> <span data-ttu-id="1558d-434">Det innehåller inte en regel toorestrict inkommande åtkomst från andra källor.</span><span class="sxs-lookup"><span data-stu-id="1558d-434">It does not contain a rule toorestrict inbound access from other sources.</span></span>
>
> <span data-ttu-id="1558d-435">hello som följande exempel visar hur tooenable SSH åt från hello Internet:</span><span class="sxs-lookup"><span data-stu-id="1558d-435">hello following example demonstrates how tooenable SSH access from hello Internet:</span></span>
>
> ```powershell
> Add-AzureRmNetworkSecurityRuleConfig -Name "SSH" -Description "SSH" -Protocol "*" -SourcePortRange "*" -DestinationPortRange "22" -SourceAddressPrefix "*" -DestinationAddressPrefix "VirtualNetwork" -Access Allow -Priority 306 -Direction Inbound
> ```

### <a name="azure-cli"></a><span data-ttu-id="1558d-436">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1558d-436">Azure CLI</span></span>

<span data-ttu-id="1558d-437">Använd hello följande steg toocreate ett virtuellt nätverk som begränsar inkommande trafik, men tillåter trafik från hello IP-adresser som krävs av HDInsight.</span><span class="sxs-lookup"><span data-stu-id="1558d-437">Use hello following steps toocreate a virtual network that restricts inbound traffic, but allows traffic from hello IP addresses required by HDInsight.</span></span>

1. <span data-ttu-id="1558d-438">Använd hello efter kommandot toocreate en ny säkerhetsgrupp för nätverk med namnet `hdisecure`.</span><span class="sxs-lookup"><span data-stu-id="1558d-438">Use hello following command toocreate a new network security group named `hdisecure`.</span></span> <span data-ttu-id="1558d-439">Ersätt **RESOURCEGROUPNAME** med hello resursgruppen som innehåller hello Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="1558d-439">Replace **RESOURCEGROUPNAME** with hello resource group that contains hello Azure Virtual Network.</span></span> <span data-ttu-id="1558d-440">Ersätt **plats** med hello plats (region) hello gruppen har skapats i.</span><span class="sxs-lookup"><span data-stu-id="1558d-440">Replace **LOCATION** with hello location (region) that hello group was created in.</span></span>

    ```azurecli
    az network nsg create -g RESOURCEGROUPNAME -n hdisecure -l LOCATION
    ```

    <span data-ttu-id="1558d-441">När hello grupp har skapats, får du information på hello ny grupp.</span><span class="sxs-lookup"><span data-stu-id="1558d-441">Once hello group has been created, you receive information on hello new group.</span></span>

2. <span data-ttu-id="1558d-442">Använd hello följande tooadd regler toohello ny säkerhetsgrupp för nätverk som tillåter inkommande kommunikation på port 443 från hello Azure HDInsight-tjänst för hälsotillstånd och hantering.</span><span class="sxs-lookup"><span data-stu-id="1558d-442">Use hello following tooadd rules toohello new network security group that allow inbound communication on port 443 from hello Azure HDInsight health and management service.</span></span> <span data-ttu-id="1558d-443">Ersätt **RESOURCEGROUPNAME** med hello namnet hello resursgruppen som innehåller hello Azure Virtual Network.</span><span class="sxs-lookup"><span data-stu-id="1558d-443">Replace **RESOURCEGROUPNAME** with hello name of hello resource group that contains hello Azure Virtual Network.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="1558d-444">Ändra hello IP-adresser som används i det här exemplet toomatch hello Azure-region som du använder.</span><span class="sxs-lookup"><span data-stu-id="1558d-444">Change hello IP addresses used in this example toomatch hello Azure region you are using.</span></span> <span data-ttu-id="1558d-445">Du hittar den här informationen i hello [HDInsight med nätverkssäkerhetsgrupper och användardefinierade vägar](#hdinsight-ip) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-445">You can find this information in hello [HDInsight with network security groups and user-defined routes](#hdinsight-ip) section.</span></span>

    ```azurecli
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule1 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "52.164.210.96" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 300 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "13.74.153.132" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 301 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.49.99" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 302 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "23.99.5.239" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 303 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "168.61.48.131" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 304 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule2 --protocol "*" --source-port-range "*" --destination-port-range "443" --source-address-prefix "138.91.141.162" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 305 --direction "Inbound"
    az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n block --protocol "*" --source-port-range "*" --destination-port-range "*" --source-address-prefix "Internet" --destination-address-prefix "VirtualNetwork" --access "Deny" --priority 500 --direction "Inbound"
    ```

3. <span data-ttu-id="1558d-446">tooretrieve hello Unik identifierare för den här nätverkssäkerhetsgruppen använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1558d-446">tooretrieve hello unique identifier for this network security group, use hello following command:</span></span>

    ```azurecli
    az network nsg show -g RESOURCEGROUPNAME -n hdisecure --query 'id'
    ```

    <span data-ttu-id="1558d-447">Det här kommandot returnerar ett värde liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="1558d-447">This command returns a value similar toohello following text:</span></span>

        "/subscriptions/SUBSCRIPTIONID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"

    <span data-ttu-id="1558d-448">Använd dubbla citattecken runt id i hello-kommandot om du inte får hello förväntat resultat.</span><span class="sxs-lookup"><span data-stu-id="1558d-448">Use double-quotes around id in hello command if you don't get hello expected results.</span></span>

4. <span data-ttu-id="1558d-449">Använd hello efter kommandot tooapply hello säkerhet grupp tooa undernät.</span><span class="sxs-lookup"><span data-stu-id="1558d-449">Use hello following command tooapply hello network security group tooa subnet.</span></span> <span data-ttu-id="1558d-450">Ersätt hello __GUID__ och __RESOURCEGROUPNAME__ värden med hello som returnerades från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="1558d-450">Replace hello __GUID__ and __RESOURCEGROUPNAME__ values with hello ones returned from hello previous step.</span></span> <span data-ttu-id="1558d-451">Ersätt __VNETNAME__ och __SUBNETNAME__ med hello virtuella nätverksnamnet och undernät som du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="1558d-451">Replace __VNETNAME__ and __SUBNETNAME__ with hello virtual network name and subnet name that you want toocreate.</span></span>

    ```azurecli
    az network vnet subnet update -g RESOURCEGROUPNAME --vnet-name VNETNAME --name SUBNETNAME --set networkSecurityGroup.id="/subscriptions/GUID/resourceGroups/RESOURCEGROUPNAME/providers/Microsoft.Network/networkSecurityGroups/hdisecure"
    ```

    <span data-ttu-id="1558d-452">När det här kommandot har slutförts kan du installera HDInsight i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-452">Once this command completes, you can install HDInsight into hello Virtual Network.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1558d-453">De här stegen kan du bara öppna toohello HDInsight hälsa och hantering av tjänsten för dataåtkomst på hello Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="1558d-453">These steps only open access toohello HDInsight health and management service on hello Azure cloud.</span></span> <span data-ttu-id="1558d-454">Alla andra åtkomst toohello HDInsight-kluster från utanför hello virtuellt nätverk har blockerats.</span><span class="sxs-lookup"><span data-stu-id="1558d-454">Any other access toohello HDInsight cluster from outside hello Virtual Network is blocked.</span></span> <span data-ttu-id="1558d-455">tooenable åtkomst från utanför hello virtuellt nätverk, måste du lägga till ytterligare Nätverkssäkerhetsgruppen regler.</span><span class="sxs-lookup"><span data-stu-id="1558d-455">tooenable access from outside hello virtual network, you must add additional Network Security Group rules.</span></span>
>
> <span data-ttu-id="1558d-456">hello som följande exempel visar hur tooenable SSH åt från hello Internet:</span><span class="sxs-lookup"><span data-stu-id="1558d-456">hello following example demonstrates how tooenable SSH access from hello Internet:</span></span>
>
> ```azurecli
> az network nsg rule create -g RESOURCEGROUPNAME --nsg-name hdisecure -n hdirule5 --protocol "*" --source-port-range "*" --destination-port-range "22" --source-address-prefix "*" --destination-address-prefix "VirtualNetwork" --access "Allow" --priority 306 --direction "Inbound"
> ```

## <span data-ttu-id="1558d-457"><a id="example-dns"></a>Exempel: DNS-konfiguration</span><span class="sxs-lookup"><span data-stu-id="1558d-457"><a id="example-dns"></a> Example: DNS configuration</span></span>

### <a name="name-resolution-between-a-virtual-network-and-a-connected-on-premises-network"></a><span data-ttu-id="1558d-458">Namnmatchning mellan ett virtuellt nätverk och ett anslutna lokalt nätverk</span><span class="sxs-lookup"><span data-stu-id="1558d-458">Name resolution between a virtual network and a connected on-premises network</span></span>

<span data-ttu-id="1558d-459">Det här exemplet gör hello följande förutsättningar:</span><span class="sxs-lookup"><span data-stu-id="1558d-459">This example makes hello following assumptions:</span></span>

* <span data-ttu-id="1558d-460">Du har ett virtuellt Azure-nätverk som är anslutna tooan lokalt nätverk via en VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="1558d-460">You have an Azure Virtual Network that is connected tooan on-premises network using a VPN gateway.</span></span>

* <span data-ttu-id="1558d-461">hello anpassad DNS-server i hello virtuellt nätverk kör Linux eller Unix som hello-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="1558d-461">hello custom DNS server in hello virtual network is running Linux or Unix as hello operating system.</span></span>

* <span data-ttu-id="1558d-462">[Binda](https://www.isc.org/downloads/bind/) är installerad på hello anpassad DNS-server.</span><span class="sxs-lookup"><span data-stu-id="1558d-462">[Bind](https://www.isc.org/downloads/bind/) is installed on hello custom DNS server.</span></span>

<span data-ttu-id="1558d-463">På hello anpassade DNS-server i hello virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="1558d-463">On hello custom DNS server in hello virtual network:</span></span>

1. <span data-ttu-id="1558d-464">Använda Azure PowerShell eller Azure CLI toofind hello DNS-suffix hello virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="1558d-464">Use either Azure PowerShell or Azure CLI toofind hello DNS suffix of hello virtual network:</span></span>

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. <span data-ttu-id="1558d-465">Använd hello följande text som hello hello på hello anpassade DNS-server för virtuellt nätverk för hello `/etc/bind/named.conf.local` fil:</span><span class="sxs-lookup"><span data-stu-id="1558d-465">On hello custom DNS server for hello virtual network, use hello following text as hello contents of hello `/etc/bind/named.conf.local` file:</span></span>

    ```
    // Forward requests for hello virtual network suffix tooAzure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {168.63.129.16;}; # Azure recursive resolver
    };
    ```

    <span data-ttu-id="1558d-466">Ersätt hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` värdet med hello DNS-suffixet för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="1558d-466">Replace hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` value with hello DNS suffix of your virtual network.</span></span>

    <span data-ttu-id="1558d-467">Den här konfigurationen dirigerar alla DNS-förfrågningar för hello DNS-suffix hello virtuellt nätverk toohello Azure rekursiv matchning.</span><span class="sxs-lookup"><span data-stu-id="1558d-467">This configuration routes all DNS requests for hello DNS suffix of hello virtual network toohello Azure recursive resolver.</span></span>

2. <span data-ttu-id="1558d-468">Använd hello följande text som hello hello på hello anpassade DNS-server för virtuellt nätverk för hello `/etc/bind/named.conf.options` fil:</span><span class="sxs-lookup"><span data-stu-id="1558d-468">On hello custom DNS server for hello virtual network, use hello following text as hello contents of hello `/etc/bind/named.conf.options` file:</span></span>

    ```
    // Clients tooaccept requests from
    // TODO: Add hello IP range of hello joined network toothis list
    acl goodclients {
        10.0.0.0/16; # IP address range of hello virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            # All other requests are sent toohello following
            forwarders {
                192.168.0.1; # Replace with hello IP address of your on-premises DNS server
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform tooRFC1035
            listen-on { any; };
    };
    ```
    
    * <span data-ttu-id="1558d-469">Ersätt hello `10.0.0.0/16` värdet med hello IP-adressintervall för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="1558d-469">Replace hello `10.0.0.0/16` value with hello IP address range of your virtual network.</span></span> <span data-ttu-id="1558d-470">Den här posten kan name resolution begäranden adresser i det här intervallet.</span><span class="sxs-lookup"><span data-stu-id="1558d-470">This entry allows name resolution requests addresses within this range.</span></span>

    * <span data-ttu-id="1558d-471">Lägg till hello IP-adressintervall hello lokalt nätverk toohello `acl goodclients { ... }` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="1558d-471">Add hello IP address range of hello on-premises network toohello `acl goodclients { ... }` section.</span></span>  <span data-ttu-id="1558d-472">posten kan namnmatchning från resurser i hello lokalt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-472">entry allows name resolution requests from resources in hello on-premises network.</span></span>
    
    * <span data-ttu-id="1558d-473">Ersätt värdet för hello `192.168.0.1` med hello IP-adressen för lokala DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="1558d-473">Replace hello value `192.168.0.1` with hello IP address of your on-premises DNS server.</span></span> <span data-ttu-id="1558d-474">Den här posten dirigerar alla andra DNS-förfrågningar toohello lokala DNS-servern.</span><span class="sxs-lookup"><span data-stu-id="1558d-474">This entry routes all other DNS requests toohello on-premises DNS server.</span></span>

3. <span data-ttu-id="1558d-475">toouse hello konfiguration, starta om bindning.</span><span class="sxs-lookup"><span data-stu-id="1558d-475">toouse hello configuration, restart Bind.</span></span> <span data-ttu-id="1558d-476">Till exempel `sudo service bind9 restart`.</span><span class="sxs-lookup"><span data-stu-id="1558d-476">For example, `sudo service bind9 restart`.</span></span>

4. <span data-ttu-id="1558d-477">Lägga till en villkorlig vidarebefordrare toohello lokala DNS-server.</span><span class="sxs-lookup"><span data-stu-id="1558d-477">Add a conditional forwarder toohello on-premises DNS server.</span></span> <span data-ttu-id="1558d-478">Konfigurera hello villkorlig vidarebefordrare toosend begäranden för hello DNS-suffix från steg 1 toohello anpassade DNS-server.</span><span class="sxs-lookup"><span data-stu-id="1558d-478">Configure hello conditional forwarder toosend requests for hello DNS suffix from step 1 toohello custom DNS server.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1558d-479">Hello i dokumentationen för DNS-programvaran för att närmare information om hur tooadd en villkorlig vidarebefordrare.</span><span class="sxs-lookup"><span data-stu-id="1558d-479">Consult hello documentation for your DNS software for specifics on how tooadd a conditional forwarder.</span></span>

<span data-ttu-id="1558d-480">När du har slutfört de här stegen kan du ansluta tooresources i antingen nätverk med fullständigt kvalificerade domännamn (FQDN).</span><span class="sxs-lookup"><span data-stu-id="1558d-480">After completing these steps, you can connect tooresources in either network using fully qualified domain names (FQDN).</span></span> <span data-ttu-id="1558d-481">Du kan nu installera HDInsight i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-481">You can now install HDInsight into hello virtual network.</span></span>

### <a name="name-resolution-between-two-connected-virtual-networks"></a><span data-ttu-id="1558d-482">Namnmatchning mellan två anslutna virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="1558d-482">Name resolution between two connected virtual networks</span></span>

<span data-ttu-id="1558d-483">Det här exemplet gör hello följande förutsättningar:</span><span class="sxs-lookup"><span data-stu-id="1558d-483">This example makes hello following assumptions:</span></span>

* <span data-ttu-id="1558d-484">Du har två virtuella Azure-nätverk som är anslutna med hjälp av en VPN-gateway eller peering.</span><span class="sxs-lookup"><span data-stu-id="1558d-484">You have two Azure Virtual Networks that are connected using either a VPN gateway or peering.</span></span>

* <span data-ttu-id="1558d-485">hello anpassade DNS-servern i båda nätverken kör Linux eller Unix som hello-operativsystem.</span><span class="sxs-lookup"><span data-stu-id="1558d-485">hello custom DNS server in both networks is running Linux or Unix as hello operating system.</span></span>

* <span data-ttu-id="1558d-486">[Binda](https://www.isc.org/downloads/bind/) är installerad på hello anpassade DNS-servrar.</span><span class="sxs-lookup"><span data-stu-id="1558d-486">[Bind](https://www.isc.org/downloads/bind/) is installed on hello custom DNS servers.</span></span>

1. <span data-ttu-id="1558d-487">Använda Azure PowerShell eller Azure CLI toofind hello DNS-suffix för båda virtuella nätverken:</span><span class="sxs-lookup"><span data-stu-id="1558d-487">Use either Azure PowerShell or Azure CLI toofind hello DNS suffix of both virtual networks:</span></span>

    ```powershell
    $resourceGroupName = Read-Input -Prompt "Enter hello resource group that contains hello virtual network used with HDInsight"
    $NICs = Get-AzureRmNetworkInterface -ResourceGroupName $resourceGroupName
    $NICs[0].DnsSettings.InternalDomainNameSuffix
    ```

    ```azurecli-interactive
    read -p "Enter hello name of hello resource group that contains hello virtual network: " RESOURCEGROUP
    az network nic list --resource-group $RESOURCEGROUP --query "[0].dnsSettings.internalDomainNameSuffix"
    ```

2. <span data-ttu-id="1558d-488">Använd hello följande text som hello hello `/etc/bind/named.config.local` fil på hello anpassad DNS-server.</span><span class="sxs-lookup"><span data-stu-id="1558d-488">Use hello following text as hello contents of hello `/etc/bind/named.config.local` file on hello custom DNS server.</span></span> <span data-ttu-id="1558d-489">Gör den här ändringen hello anpassade DNS-servern i båda virtuella nätverken.</span><span class="sxs-lookup"><span data-stu-id="1558d-489">Make this change on hello custom DNS server in both virtual networks.</span></span>

    ```
    // Forward requests for hello virtual network suffix tooAzure recursive resolver
    zone "0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net" {
        type forward;
        forwarders {10.0.0.4;}; # hello IP address of hello DNS server in hello other virtual network
    };
    ```

    <span data-ttu-id="1558d-490">Ersätt hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` värdet med hello DNS-suffixet hello __andra__ virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-490">Replace hello `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` value with hello DNS suffix of hello __other__ virtual network.</span></span> <span data-ttu-id="1558d-491">Den här posten skickar begäranden för hello DNS-suffix hello fjärrnätverket toohello anpassade DNS i nätverket.</span><span class="sxs-lookup"><span data-stu-id="1558d-491">This entry routes requests for hello DNS suffix of hello remote network toohello custom DNS in that network.</span></span>

3. <span data-ttu-id="1558d-492">På hello anpassade DNS-servrar i båda virtuella nätverken använder du följande text som hello hello hello `/etc/bind/named.conf.options` fil:</span><span class="sxs-lookup"><span data-stu-id="1558d-492">On hello custom DNS servers in both virtual networks, use hello following text as hello contents of hello `/etc/bind/named.conf.options` file:</span></span>

    ```
    // Clients tooaccept requests from
    acl goodclients {
        10.1.0.0/16; # hello IP address range of one virtual network
        10.0.0.0/16; # hello IP address range of hello other virtual network
        localhost;
        localnets;
    };

    options {
            directory "/var/cache/bind";

            recursion yes;

            allow-query { goodclients; };

            forwarders {
            168.63.129.16;   # Azure recursive resolver         
            };

            dnssec-validation auto;

            auth-nxdomain no;    # conform tooRFC1035
            listen-on { any; };
    };
    ```
    
    * <span data-ttu-id="1558d-493">Ersätt hello `10.0.0.0/16` och `10.1.0.0/16` värden med hello IP-adressintervall för ditt virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-493">Replace hello `10.0.0.0/16` and `10.1.0.0/16` values with hello IP address ranges of your virtual networks.</span></span> <span data-ttu-id="1558d-494">Den här posten kan resurser i varje nätverk toomake begäranden hello DNS-servrar.</span><span class="sxs-lookup"><span data-stu-id="1558d-494">This entry allows resources in each network toomake requests of hello DNS servers.</span></span>

    <span data-ttu-id="1558d-495">Alla begäranden som inte är för hello DNS-suffix hello virtuella nätverk (till exempel microsoft.com) hanteras av hello Azure rekursiv matchning.</span><span class="sxs-lookup"><span data-stu-id="1558d-495">Any requests that are not for hello DNS suffixes of hello virtual networks (for example, microsoft.com) is handled by hello Azure recursive resolver.</span></span>

4. <span data-ttu-id="1558d-496">toouse hello konfiguration, starta om bindning.</span><span class="sxs-lookup"><span data-stu-id="1558d-496">toouse hello configuration, restart Bind.</span></span> <span data-ttu-id="1558d-497">Till exempel `sudo service bind9 restart` på båda DNS-servrar.</span><span class="sxs-lookup"><span data-stu-id="1558d-497">For example, `sudo service bind9 restart` on both DNS servers.</span></span>

<span data-ttu-id="1558d-498">När du har slutfört de här stegen kan du ansluta tooresources i hello virtuellt nätverk med fullständigt kvalificerade domännamn (FQDN).</span><span class="sxs-lookup"><span data-stu-id="1558d-498">After completing these steps, you can connect tooresources in hello virtual network using fully qualified domain names (FQDN).</span></span> <span data-ttu-id="1558d-499">Du kan nu installera HDInsight i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="1558d-499">You can now install HDInsight into hello virtual network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1558d-500">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1558d-500">Next steps</span></span>

* <span data-ttu-id="1558d-501">Slutpunkt till slutpunkt-exempel på hur du konfigurerar HDInsight tooconnect tooan lokalt nätverk finns i [ansluta HDInsight tooan lokalt nätverk](./connect-on-premises-network.md).</span><span class="sxs-lookup"><span data-stu-id="1558d-501">For an end-to-end example of configuring HDInsight tooconnect tooan on-premises network, see [Connect HDInsight tooan on-premises network](./connect-on-premises-network.md).</span></span>

* <span data-ttu-id="1558d-502">Mer information om virtuella Azure-nätverk finns hello [översikt över Azure Virtual Network](../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1558d-502">For more information on Azure virtual networks, see hello [Azure Virtual Network overview](../virtual-network/virtual-networks-overview.md).</span></span>

* <span data-ttu-id="1558d-503">Mer information om nätverkssäkerhetsgrupper finns [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="1558d-503">For more information on network security groups, see [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

* <span data-ttu-id="1558d-504">Mer information om användardefinierade vägar finns [användardefinierade vägar och IP-vidarebefordring](../virtual-network/virtual-networks-udr-overview.md).</span><span class="sxs-lookup"><span data-stu-id="1558d-504">For more information on user-defined routes, see [USer-defined routes and IP forwarding](../virtual-network/virtual-networks-udr-overview.md).</span></span>