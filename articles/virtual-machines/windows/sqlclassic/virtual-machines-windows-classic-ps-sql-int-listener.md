---
title: "aaaConfigure en ILB-lyssnare för Always On-Tillgänglighetsgrupper i Azure | Microsoft Docs"
description: "Den här kursen använder resurser som har skapats med hello klassiska distributionsmodellen och skapar en alltid på tillgänglighetsgruppens lyssnare i Azure som använder en intern belastningsutjämnare."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 291288a0-740b-4cfa-af62-053218beba77
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/02/2017
ms.author: mikeray
ms.openlocfilehash: 2ce9b64fea491c945b58f7641e41fd39d90b078a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-an-ilb-listener-for-always-on-availability-groups-in-azure"></a><span data-ttu-id="db396-103">Konfigurera en ILB-lyssnare för Always On-Tillgänglighetsgrupper i Azure</span><span class="sxs-lookup"><span data-stu-id="db396-103">Configure an ILB listener for Always On availability groups in Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="db396-104">Internt lyssnare</span><span class="sxs-lookup"><span data-stu-id="db396-104">Internal listener</span></span>](../classic/ps-sql-int-listener.md)
> * [<span data-ttu-id="db396-105">Externa lyssnare</span><span class="sxs-lookup"><span data-stu-id="db396-105">External listener</span></span>](../classic/ps-sql-ext-listener.md)
>
>

## <a name="overview"></a><span data-ttu-id="db396-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="db396-106">Overview</span></span>

> [!IMPORTANT]
> <span data-ttu-id="db396-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="db396-107">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="db396-108">Den här artikeln beskriver hello användning av hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="db396-108">This article covers hello use of hello classic deployment model.</span></span> <span data-ttu-id="db396-109">Vi rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="db396-109">We recommend that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="db396-110">tooconfigure en lyssnare för en Always On-tillgänglighetsgrupp i hello Resource Manager-modellen finns [konfigurera belastningsutjämning för en tillgänglighetsgrupp alltid på i Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span><span class="sxs-lookup"><span data-stu-id="db396-110">tooconfigure a listener for an Always On availability group in hello Resource Manager model, see [Configure a load balancer for an Always On availability group in Azure](../sql/virtual-machines-windows-portal-sql-alwayson-int-listener.md).</span></span>

<span data-ttu-id="db396-111">Tillgänglighetsgruppen kan innehålla repliker som är endast till lokala eller endast Azure eller som sträcker sig över både lokalt och Azure för hybrid-konfigurationer.</span><span class="sxs-lookup"><span data-stu-id="db396-111">Your availability group can contain replicas that are on-premises only or Azure only, or that span both on-premises and Azure for hybrid configurations.</span></span> <span data-ttu-id="db396-112">Azure repliker kan finnas inom hello samma region eller över flera regioner som använder flera virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="db396-112">Azure replicas can reside within hello same region or across multiple regions that use multiple virtual networks.</span></span> <span data-ttu-id="db396-113">hello procedurer i den här artikeln förutsätter att du redan har [konfigureras en tillgänglighetsgrupp](../classic/portal-sql-alwayson-availability-groups.md) men ännu inte har konfigurerat en lyssnare.</span><span class="sxs-lookup"><span data-stu-id="db396-113">hello procedures in this article assume that you have already [configured an availability group](../classic/portal-sql-alwayson-availability-groups.md) but have not yet configured a listener.</span></span>

## <a name="guidelines-and-limitations-for-internal-listeners"></a><span data-ttu-id="db396-114">Riktlinjer och begränsningar för interna lyssnare</span><span class="sxs-lookup"><span data-stu-id="db396-114">Guidelines and limitations for internal listeners</span></span>
<span data-ttu-id="db396-115">hello användning av en intern belastningsutjämnare (ILB) med en tillgänglighetsgruppslyssnare i Azure är ämne toohello följande riktlinjer:</span><span class="sxs-lookup"><span data-stu-id="db396-115">hello use of an internal load balancer (ILB) with an availability group listener in Azure is subject toohello following guidelines:</span></span>

* <span data-ttu-id="db396-116">hello tillgänglighetsgruppens lyssnare stöds på Windows Server 2008 R2, Windows Server 2012 och Windows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="db396-116">hello availability group listener is supported on Windows Server 2008 R2, Windows Server 2012, and Windows Server 2012 R2.</span></span>
* <span data-ttu-id="db396-117">Endast en intern tillgänglighetsgruppens lyssnare stöds för varje molnbaserad tjänst bör eftersom hello-lyssnare har konfigurerats toohello ILB och det finns endast en ILB för varje tjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="db396-117">Only one internal availability group listener is supported for each cloud service, because hello listener is configured toohello ILB, and there is only one ILB for each cloud service.</span></span> <span data-ttu-id="db396-118">Det är dock möjligt toocreate flera externa lyssnare.</span><span class="sxs-lookup"><span data-stu-id="db396-118">However, it is possible toocreate multiple external listeners.</span></span> <span data-ttu-id="db396-119">Mer information finns i [konfigurera en extern lyssnare för Always On-Tillgänglighetsgrupper i Azure](../classic/ps-sql-ext-listener.md).</span><span class="sxs-lookup"><span data-stu-id="db396-119">For more information, see [Configure an external listener for Always On availability groups in Azure](../classic/ps-sql-ext-listener.md).</span></span>

## <a name="determine-hello-accessibility-of-hello-listener"></a><span data-ttu-id="db396-120">Fastställa hello hjälpmedel för hello-lyssnare</span><span class="sxs-lookup"><span data-stu-id="db396-120">Determine hello accessibility of hello listener</span></span>
[!INCLUDE [ag-listener-accessibility](../../../../includes/virtual-machines-ag-listener-determine-accessibility.md)]

<span data-ttu-id="db396-121">Den här artikeln handlar om hur du skapar en lyssnare som använder en ILB.</span><span class="sxs-lookup"><span data-stu-id="db396-121">This article focuses on creating a listener that uses an ILB.</span></span> <span data-ttu-id="db396-122">Om du behöver en offentlig eller externa lyssnare finns hello versionen av den här artikeln beskrivs ställa in en [externa lyssnare](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="db396-122">If you need an public or external listener, see hello version of this article that discusses setting up an [external listener](../classic/ps-sql-ext-listener.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="create-load-balanced-vm-endpoints-with-direct-server-return"></a><span data-ttu-id="db396-123">Skapa belastningsutjämnade slutpunkter för virtuell dator med direkt servern returnerade</span><span class="sxs-lookup"><span data-stu-id="db396-123">Create load-balanced VM endpoints with direct server return</span></span>
<span data-ttu-id="db396-124">Du först skapa en ILB genom att köra skriptet hello senare i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="db396-124">You first create an ILB by running hello script later in this section.</span></span>

<span data-ttu-id="db396-125">Skapa en slutpunkt för Utjämning av nätverksbelastning för varje virtuell dator som är värd för en Azure replik.</span><span class="sxs-lookup"><span data-stu-id="db396-125">Create a load-balanced endpoint for each VM that hosts an Azure replica.</span></span> <span data-ttu-id="db396-126">Om du har repliker i flera områden, varje replik för området måste vara i samma molntjänst i hello hello samma virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="db396-126">If you have replicas in multiple regions, each replica for that region must be in hello same cloud service in hello same Azure virtual network.</span></span> <span data-ttu-id="db396-127">Skapa tillgänglighet grupp repliker som sträcker sig över flera Azure-regioner måste du konfigurera flera virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="db396-127">Creating availability group replicas that span multiple Azure regions requires configuring multiple virtual networks.</span></span> <span data-ttu-id="db396-128">Mer information om hur du konfigurerar mellan virtuell nätverksanslutning finns [konfigurera nätverksanslutningen för virtuella nätverk toovirtual](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span><span class="sxs-lookup"><span data-stu-id="db396-128">For more information on configuring cross virtual network connectivity, see [Configure virtual network toovirtual network connectivity](../../../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md).</span></span>

1. <span data-ttu-id="db396-129">Gå tooeach VM som är värd för en replik tooview hello information i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="db396-129">In hello Azure portal, go tooeach VM that hosts a replica tooview hello details.</span></span>

2. <span data-ttu-id="db396-130">Klicka på hello **slutpunkter** för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="db396-130">Click hello **Endpoints** tab for each VM.</span></span>

3. <span data-ttu-id="db396-131">Kontrollera att hello **namn** och **offentlig Port** hello lyssnare slutpunkt som du vill toouse inte som redan används.</span><span class="sxs-lookup"><span data-stu-id="db396-131">Verify that hello **Name** and **Public Port** of hello listener endpoint that you want toouse are not already in use.</span></span> <span data-ttu-id="db396-132">I hello exemplet i det här avsnittet är hello namnet *MyEndpoint*, och hello port är *1433*.</span><span class="sxs-lookup"><span data-stu-id="db396-132">In hello example in this section, hello name is *MyEndpoint*, and hello port is *1433*.</span></span>

4. <span data-ttu-id="db396-133">På din lokala klient, ladda ned och installera hello senaste [PowerShell-modulen](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="db396-133">On your local client, download and install hello latest [PowerShell module](https://azure.microsoft.com/downloads/).</span></span>

5. <span data-ttu-id="db396-134">Starta Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db396-134">Start Azure PowerShell.</span></span>  
    <span data-ttu-id="db396-135">Öppnar en ny PowerShell-session med hello Azure administrativa inlästa moduler.</span><span class="sxs-lookup"><span data-stu-id="db396-135">A new PowerShell session opens, with hello Azure administrative modules loaded.</span></span>

6. <span data-ttu-id="db396-136">Kör `Get-AzurePublishSettingsFile`.</span><span class="sxs-lookup"><span data-stu-id="db396-136">Run `Get-AzurePublishSettingsFile`.</span></span> <span data-ttu-id="db396-137">Denna cmdlet dirigerar tooa webbläsare toodownload en publicera inställningar tooa lokala katalog.</span><span class="sxs-lookup"><span data-stu-id="db396-137">This cmdlet directs you tooa browser toodownload a publish settings file tooa local directory.</span></span> <span data-ttu-id="db396-138">Du kan uppmanas att dina inloggningsuppgifter för din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="db396-138">You might be prompted for your sign-in credentials for your Azure subscription.</span></span>

7. <span data-ttu-id="db396-139">Kör följande hello `Import-AzurePublishSettingsFile` kommandot med hello sökvägen hello Publicera fil med inställningar som du hämtade:</span><span class="sxs-lookup"><span data-stu-id="db396-139">Run hello following `Import-AzurePublishSettingsFile` command with hello path of hello publish settings file that you downloaded:</span></span>

        Import-AzurePublishSettingsFile -PublishSettingsFile <PublishSettingsFilePath>

    <span data-ttu-id="db396-140">När hello publicera inställningsfilen importeras, kan du hantera Azure-prenumerationen i hello PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="db396-140">After hello publish settings file is imported, you can manage your Azure subscription in hello PowerShell session.</span></span>

8. <span data-ttu-id="db396-141">För *ILB*, tilldela en statisk IP-adress.</span><span class="sxs-lookup"><span data-stu-id="db396-141">For *ILB*, assign a static IP address.</span></span> <span data-ttu-id="db396-142">Undersök hello aktuella konfiguration av virtuellt nätverk genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="db396-142">Examine hello current virtual network configuration by running hello following command:</span></span>

        (Get-AzureVNetConfig).XMLConfiguration
9. <span data-ttu-id="db396-143">Obs hello *undernät* namn för hello undernät som innehåller hello virtuella datorer som är värdar för hello repliker.</span><span class="sxs-lookup"><span data-stu-id="db396-143">Note hello *Subnet* name for hello subnet that contains hello VMs that host hello replicas.</span></span> <span data-ttu-id="db396-144">Det här namnet används i hello $SubnetName parameter i hello skript.</span><span class="sxs-lookup"><span data-stu-id="db396-144">This name is used in hello $SubnetName parameter in hello script.</span></span>

10. <span data-ttu-id="db396-145">Obs hello *VirtualNetworkSite* namn och hello startar *AddressPrefix* för hello undernät som innehåller hello virtuella datorer som är värdar för hello repliker.</span><span class="sxs-lookup"><span data-stu-id="db396-145">Note hello *VirtualNetworkSite* name and hello starting *AddressPrefix* for hello subnet that contains hello VMs that host hello replicas.</span></span> <span data-ttu-id="db396-146">Leta efter en tillgänglig IP-adress genom att skicka båda värdena toohello `Test-AzureStaticVNetIP` kommandot och genom att undersöka hello *AvailableAddresses*.</span><span class="sxs-lookup"><span data-stu-id="db396-146">Look for an available IP address by passing both values toohello `Test-AzureStaticVNetIP` command and by examining hello *AvailableAddresses*.</span></span> <span data-ttu-id="db396-147">Till exempel om hello virtuellt nätverk kallas *MyVNet* och har ett undernät-adressintervall som börjar vid *172.16.0.128*, hello följande kommando visar tillgängliga adresser:</span><span class="sxs-lookup"><span data-stu-id="db396-147">For example, if hello virtual network is named *MyVNet* and has a subnet address range that starts at *172.16.0.128*, hello following command would list available addresses:</span></span>

        (Test-AzureStaticVNetIP -VNetName "MyVNet"-IPAddress 172.16.0.128).AvailableAddresses
11. <span data-ttu-id="db396-148">Välj en av hello tillgängliga adresser och använda den i hello $ILBStaticIP parametern hello skriptet i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="db396-148">Select one of hello available addresses, and use it in hello $ILBStaticIP parameter of hello script in hello next step.</span></span>

12. <span data-ttu-id="db396-149">Kopiera följande PowerShell-skriptet tooa textredigerare hello och ange hello variabelvärden toosuit din miljö.</span><span class="sxs-lookup"><span data-stu-id="db396-149">Copy hello following PowerShell script tooa text editor, and set hello variable values toosuit your environment.</span></span> <span data-ttu-id="db396-150">Standardvärden har angetts för vissa parametrar.</span><span class="sxs-lookup"><span data-stu-id="db396-150">Defaults have been provided for some parameters.</span></span>  

    <span data-ttu-id="db396-151">Befintliga distributioner som använder tillhörighetsgrupper kan inte lägga till en ILB.</span><span class="sxs-lookup"><span data-stu-id="db396-151">Existing deployments that use affinity groups cannot add an ILB.</span></span> <span data-ttu-id="db396-152">Läs mer om krav för ILB [översikt över intern belastningsutjämnare](../../../load-balancer/load-balancer-internal-overview.md).</span><span class="sxs-lookup"><span data-stu-id="db396-152">For more information about ILB requirements, see [Internal load balancer overview](../../../load-balancer/load-balancer-internal-overview.md).</span></span>

    <span data-ttu-id="db396-153">Även om tillgänglighetsgruppen sträcker sig över Azure-regioner, måste du köra hello skriptet en gång i varje datacenter för hello Molntjänsten och noder som tillhör det datacentret.</span><span class="sxs-lookup"><span data-stu-id="db396-153">Also, if your availability group spans Azure regions, you must run hello script once in each datacenter for hello cloud service and nodes that reside in that datacenter.</span></span>

        # Define variables
        $ServiceName = "<MyCloudService>" # hello name of hello cloud service that contains hello availability group nodes
        $AGNodes = "<VM1>","<VM2>","<VM3>" # all availability group nodes containing replicas in hello same cloud service, separated by commas
        $SubnetName = "<MySubnetName>" # subnet name that hello replicas use in hello virtual network
        $ILBStaticIP = "<MyILBStaticIPAddress>" # static IP address for hello ILB in hello subnet
        $ILBName = "AGListenerLB" # customize hello ILB name or use this default value

        # Create hello ILB
        Add-AzureInternalLoadBalancer -InternalLoadBalancerName $ILBName -SubnetName $SubnetName -ServiceName $ServiceName -StaticVNetIPAddress $ILBStaticIP

        # Configure a load-balanced endpoint for each node in $AGNodes by using ILB
        ForEach ($node in $AGNodes)
        {
            Get-AzureVM -ServiceName $ServiceName -Name $node | Add-AzureEndpoint -Name "ListenerEndpoint" -LBSetName "ListenerEndpointLB" -Protocol tcp -LocalPort 1433 -PublicPort 1433 -ProbePort 59999 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ILBName -DirectServerReturn $true | Update-AzureVM
        }

13. <span data-ttu-id="db396-154">När du har angett hello variabler, kopiera hello skript från hello text editor tooyour PowerShell-session toorun den.</span><span class="sxs-lookup"><span data-stu-id="db396-154">After you have set hello variables, copy hello script from hello text editor tooyour PowerShell session toorun it.</span></span> <span data-ttu-id="db396-155">Om hello uppmaningen visas fortfarande  **>>** , tryck på RETUR igen toomake att hello skriptet börjar köras.</span><span class="sxs-lookup"><span data-stu-id="db396-155">If hello prompt still shows **>>**, press Enter again toomake sure hello script starts running.</span></span>

## <a name="verify-that-kb2854082-is-installed-if-necessary"></a><span data-ttu-id="db396-156">Kontrollera att KB2854082 installeras vid behov</span><span class="sxs-lookup"><span data-stu-id="db396-156">Verify that KB2854082 is installed if necessary</span></span>
[!INCLUDE [kb2854082](../../../../includes/virtual-machines-ag-listener-kb2854082.md)]

## <a name="open-hello-firewall-ports-in-availability-group-nodes"></a><span data-ttu-id="db396-157">Öppna portar i brandväggen hello tillgänglighet grupp noder</span><span class="sxs-lookup"><span data-stu-id="db396-157">Open hello firewall ports in availability group nodes</span></span>
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-open-firewall.md)]

## <a name="create-hello-availability-group-listener"></a><span data-ttu-id="db396-158">Skapa hello tillgänglighetsgruppens lyssnare</span><span class="sxs-lookup"><span data-stu-id="db396-158">Create hello availability group listener</span></span>

<span data-ttu-id="db396-159">Skapa hello tillgänglighetsgruppens lyssnare i två steg.</span><span class="sxs-lookup"><span data-stu-id="db396-159">Create hello availability group listener in two steps.</span></span> <span data-ttu-id="db396-160">Först skapar hello klienten åtkomst punkt klusterresurs och konfigurera beroenden.</span><span class="sxs-lookup"><span data-stu-id="db396-160">First, create hello client access point cluster resource and configure  dependencies.</span></span> <span data-ttu-id="db396-161">Konfigurera andra hello klusterresurser i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="db396-161">Second, configure hello cluster resources in PowerShell.</span></span>

### <a name="create-hello-client-access-point-and-configure-hello-cluster-dependencies"></a><span data-ttu-id="db396-162">Skapa hello klientåtkomstpunkt och konfigurera hello klustret beroenden</span><span class="sxs-lookup"><span data-stu-id="db396-162">Create hello client access point and configure hello cluster dependencies</span></span>
[!INCLUDE [firewall](../../../../includes/virtual-machines-ag-listener-create-listener.md)]

### <a name="configure-hello-cluster-resources-in-powershell"></a><span data-ttu-id="db396-163">Konfigurera hello klusterresurser i PowerShell</span><span class="sxs-lookup"><span data-stu-id="db396-163">Configure hello cluster resources in PowerShell</span></span>
1. <span data-ttu-id="db396-164">För ILB, måste du använda hello IP-adressen för hello ILB som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="db396-164">For ILB, you must use hello IP address of hello ILB that was created earlier.</span></span> <span data-ttu-id="db396-165">tooobtain den här IP-adressen i PowerShell, Använd hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="db396-165">tooobtain this IP address in PowerShell, use hello following script:</span></span>

        # Define variables
        $ServiceName="<MyServiceName>" # hello name of hello cloud service that contains hello AG nodes
        (Get-AzureInternalLoadBalancer -ServiceName $ServiceName).IPAddress

2. <span data-ttu-id="db396-166">Kopiera hello PowerShell-skript för ditt operativsystem tooa textredigering på en av hello virtuella datorer, och ange sedan hello variabler toohello värden som du antecknade tidigare.</span><span class="sxs-lookup"><span data-stu-id="db396-166">On one of hello VMs, copy hello PowerShell script for your operating system tooa text editor, and then set hello variables toohello values you noted earlier.</span></span>

    <span data-ttu-id="db396-167">För Windows Server 2012 eller senare, Använd hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="db396-167">For Windows Server 2012 or later, use hello following script:</span></span>

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP address resource name
        $ILBIP = “<X.X.X.X>” # hello IP address of hello ILB

        Import-Module FailoverClusters

        Get-ClusterResource $IPResourceName | Set-ClusterParameter -Multiple @{"Address"="$ILBIP";"ProbePort"="59999";"SubnetMask"="255.255.255.255";"Network"="$ClusterNetworkName";"EnableDhcp"=0}

    <span data-ttu-id="db396-168">För Windows Server 2008 R2, använder du hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="db396-168">For Windows Server 2008 R2, use hello following script:</span></span>

        # Define variables
        $ClusterNetworkName = "<MyClusterNetworkName>" # hello cluster network name (Use Get-ClusterNetwork on Windows Server 2012 of higher toofind hello name)
        $IPResourceName = "<IPResourceName>" # hello IP address resource name
        $ILBIP = “<X.X.X.X>” # hello IP address of hello ILB

        Import-Module FailoverClusters

        cluster res $IPResourceName /priv enabledhcp=0 address=$ILBIP probeport=59999  subnetmask=255.255.255.255

3. <span data-ttu-id="db396-169">När du har angetts hello variabler, öppna ett upphöjt Windows PowerShell-fönster, klistra in hello skript från hello textredigerare till din PowerShell-session toorun den.</span><span class="sxs-lookup"><span data-stu-id="db396-169">After you have set hello variables, open an elevated Windows PowerShell window, paste hello script from hello text editor into your PowerShell session toorun it.</span></span> <span data-ttu-id="db396-170">Om hello uppmaningen visas fortfarande  **>>** , tryck på RETUR igen toomake till att hello skriptet börjar köras.</span><span class="sxs-lookup"><span data-stu-id="db396-170">If hello prompt still shows **>>**, Press Enter again toomake sure that hello script starts running.</span></span>

4. <span data-ttu-id="db396-171">Upprepa föregående steg för varje virtuell hello.</span><span class="sxs-lookup"><span data-stu-id="db396-171">Repeat hello preceding steps for each VM.</span></span>  
    <span data-ttu-id="db396-172">Det här skriptet konfigurerar hello IP-adressresurs med hello IP-adress för hello Molntjänsten och anger andra parametrar, till exempel hello avsökningsport.</span><span class="sxs-lookup"><span data-stu-id="db396-172">This script configures hello IP address resource with hello IP address of hello cloud service and sets other parameters, such as hello probe port.</span></span> <span data-ttu-id="db396-173">När hello IP-adressresurs är online, svarar den toohello avsökning på hello avsökningsport från hello belastningsutjämnade slutpunkt som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="db396-173">When hello IP address resource is brought online, it can respond toohello polling on hello probe port from hello load-balanced endpoint that you created earlier.</span></span>

## <a name="bring-hello-listener-online"></a><span data-ttu-id="db396-174">Ta hello-lyssnare</span><span class="sxs-lookup"><span data-stu-id="db396-174">Bring hello listener online</span></span>
[!INCLUDE [Bring-Listener-Online](../../../../includes/virtual-machines-ag-listener-bring-online.md)]

## <a name="follow-up-items"></a><span data-ttu-id="db396-175">Uppföljning av objekt</span><span class="sxs-lookup"><span data-stu-id="db396-175">Follow-up items</span></span>
[!INCLUDE [Follow-up](../../../../includes/virtual-machines-ag-listener-follow-up.md)]

## <a name="test-hello-availability-group-listener-within-hello-same-virtual-network"></a><span data-ttu-id="db396-176">Testa hello tillgänglighetsgruppens lyssnare (hello i samma virtuella nätverk)</span><span class="sxs-lookup"><span data-stu-id="db396-176">Test hello availability group listener (within hello same virtual network)</span></span>
[!INCLUDE [Test-Listener-Within-VNET](../../../../includes/virtual-machines-ag-listener-test.md)]

## <a name="next-steps"></a><span data-ttu-id="db396-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="db396-177">Next steps</span></span>
[!INCLUDE [Listener-Next-Steps](../../../../includes/virtual-machines-ag-listener-next-steps.md)]
