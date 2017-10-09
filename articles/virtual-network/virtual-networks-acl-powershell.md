---
title: "aaaManage Azure endpoint åtkomst styra listor | PowerShell | Klassiska | Microsoft Docs"
description: "Lär dig hur toomanage ACL: er med PowerShell"
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c84e40af-f351-4572-b3f0-d572d46bafe7
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: a7ca241ea108a266085bfb689b742d781e58da1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-hello-classic-deployment-model"></a><span data-ttu-id="6b6b6-103">Hantera åtkomstkontrollistor för slutpunkten med PowerShell i hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="6b6b6-103">Manage endpoint access control lists using PowerShell in hello classic deployment model</span></span>
<span data-ttu-id="6b6b6-104">Du kan skapa och hantera nätverket åtkomstkontrollistor (ACL) för slutpunkter med hjälp av Azure PowerShell eller hello-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-104">You can create and manage Network Access Control Lists (ACLs) for endpoints by using Azure PowerShell or in hello Management Portal.</span></span> <span data-ttu-id="6b6b6-105">I det här avsnittet hittar du procedurer för ACL vanliga uppgifter som du kan utföra med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-105">In this topic, you'll find procedures for ACL common tasks that you can complete using PowerShell.</span></span> <span data-ttu-id="6b6b6-106">Hello lista över Azure PowerShell cmdlets finns [Azure Management-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=317721).</span><span class="sxs-lookup"><span data-stu-id="6b6b6-106">For hello list of Azure PowerShell cmdlets see [Azure Management Cmdlets](http://go.microsoft.com/fwlink/?LinkId=317721).</span></span> <span data-ttu-id="6b6b6-107">Mer information om åtkomstkontrollistor finns [vad är ett nätverk åtkomstkontrollista (ACL)?](virtual-networks-acl.md).</span><span class="sxs-lookup"><span data-stu-id="6b6b6-107">For more information about ACLs, see [What is a Network Access Control List (ACL)?](virtual-networks-acl.md).</span></span> <span data-ttu-id="6b6b6-108">Om du vill toomanage din ACL: er med hjälp av hello-hanteringsportalen, se [hur tooSet in slutpunkter tooa virtuella](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="6b6b6-108">If you want toomanage your ACLs by using hello Management Portal, see [How tooSet Up Endpoints tooa Virtual Machine](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="manage-network-acls-by-using-azure-powershell"></a><span data-ttu-id="6b6b6-109">Hantera nätverket ACL: er med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6b6b6-109">Manage Network ACLs by using Azure PowerShell</span></span>
<span data-ttu-id="6b6b6-110">Du kan använda Azure PowerShell-cmdlets toocreate, ta bort och konfigurera (uppsättning) nätverket åtkomstkontrollistor (ACL).</span><span class="sxs-lookup"><span data-stu-id="6b6b6-110">You can use Azure PowerShell cmdlets toocreate, remove, and configure (set) Network Access Control Lists (ACLs).</span></span> <span data-ttu-id="6b6b6-111">Innehåller några exempel på några av hello sätt kan du konfigurera en ACL med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-111">We've included a few examples of some of hello ways you can configure an ACL using PowerShell.</span></span>

<span data-ttu-id="6b6b6-112">tooretrieve en fullständig lista över hello ACL PowerShell-cmdlets, du kan använda något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="6b6b6-112">tooretrieve a complete list of hello ACL PowerShell cmdlets, you can use either of hello following:</span></span>

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a><span data-ttu-id="6b6b6-113">Skapa en ACL i nätverket med regler som tillåter åtkomst från ett fjärranslutet undernät</span><span class="sxs-lookup"><span data-stu-id="6b6b6-113">Create a Network ACL with rules that permit access from a remote subnet</span></span>
<span data-ttu-id="6b6b6-114">hello exemplet nedan visar hur-toocreate en ny ACL som innehåller regler.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-114">hello example below illustrates a way toocreate a new ACL that contains rules.</span></span> <span data-ttu-id="6b6b6-115">Det här används sedan tooa virtuell datorslutpunkt.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-115">This ACL is then applied tooa virtual machine endpoint.</span></span> <span data-ttu-id="6b6b6-116">hello ACL-regler i hello exemplet nedan kommer att tillåta åtkomst från ett fjärranslutet undernät.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-116">hello ACL rules in hello example below will allow access from a remote subnet.</span></span> <span data-ttu-id="6b6b6-117">toocreate en ny nätverket ACL med tillståndet regler för ett fjärranslutet undernät, öppna ett Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-117">toocreate a new Network ACL with permit rules for a remote subnet, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="6b6b6-118">Kopiera och klistra in hello skriptet nedan, konfigurera hello skript med egna värden och sedan köra hello skript.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-118">Copy and paste hello script below, configuring hello script with your own values, and then run hello script.</span></span>

1. <span data-ttu-id="6b6b6-119">Skapa hello nya ACL-objekt i nätverket.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-119">Create hello new network ACL object.</span></span>
   
        $acl1 = New-AzureAclConfig
2. <span data-ttu-id="6b6b6-120">Ange en regel som tillåter åtkomst från ett fjärranslutet undernät.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-120">Set a rule that permits access from a remote subnet.</span></span> <span data-ttu-id="6b6b6-121">I hello exemplet nedan kan du ange regel *100* (som har högre prioritet än 200 och högre) tooallow hello fjärrundernät *10.0.0.0/8* komma åt toohello virtuell datorslutpunkt.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-121">In hello example below, you set rule *100* (which has priority over rule 200 and higher) tooallow hello remote subnet *10.0.0.0/8* access toohello virtual machine endpoint.</span></span> <span data-ttu-id="6b6b6-122">Ersätt hello värden med din egen konfigurationskrav.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-122">Replace hello values with your own configuration requirements.</span></span> <span data-ttu-id="6b6b6-123">hello namnet ”SharePoint ACL-config” ska ersättas med hello eget namn som du vill toocall den här regeln.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-123">hello name "SharePoint ACL config" should be replaced with hello friendly name that you want toocall this rule.</span></span>
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. <span data-ttu-id="6b6b6-124">Upprepa hello cmdlet, ersätter hello värden med din egen konfigurationskrav för ytterligare regler.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-124">For additional rules, repeat hello cmdlet, replacing hello values with your own configuration requirements.</span></span> <span data-ttu-id="6b6b6-125">Vara säker på att toochange hello regel nummer ordning tooreflect hello order som du vill hello regler toobe tillämpas.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-125">Be sure toochange hello rule number Order tooreflect hello order in which you want hello rules toobe applied.</span></span> <span data-ttu-id="6b6b6-126">hello lägre nummer i regeln företräde framför hello högre nummer.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-126">hello lower rule number takes precedence over hello higher number.</span></span>
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. <span data-ttu-id="6b6b6-127">Därefter kan du antingen skapa en ny slutpunkt (Lägg till) eller ange hello ACL för en befintlig slutpunkt (anges).</span><span class="sxs-lookup"><span data-stu-id="6b6b6-127">Next, you can either create a new endpoint (Add) or set hello ACL for an existing endpoint (Set).</span></span> <span data-ttu-id="6b6b6-128">I det här exemplet ska vi lägga till en ny virtuell dator-slutpunkt som kallas ”web” och uppdatera hello virtuella slutpunkten med hello ACL-inställningar.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-128">In this example, we will add a new virtual machine endpoint called "web" and update hello virtual machine endpoint with hello ACL settings.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. <span data-ttu-id="6b6b6-129">Sedan kombinera hello-cmdlets och köra hello-skript.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-129">Next, combine hello cmdlets and run hello script.</span></span> <span data-ttu-id="6b6b6-130">I det här exemplet hello kombinerade cmdlets skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="6b6b6-130">For this example, hello combined cmdlets would look like this:</span></span>
   
        $acl1 = New-AzureAclConfig
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "Sharepoint ACL config"
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        |Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        |Update-AzureVM

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a><span data-ttu-id="6b6b6-131">Ta bort en nätverket ACL-regel som tillåter åtkomst från ett fjärranslutet undernät</span><span class="sxs-lookup"><span data-stu-id="6b6b6-131">Remove a Network ACL rule that permits access from a remote subnet</span></span>
<span data-ttu-id="6b6b6-132">hello exemplet nedan sätt-tooremove en regel för Portåtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-132">hello example below illustrates a way tooremove a network ACL rule.</span></span>  <span data-ttu-id="6b6b6-133">tooremove ett nätverk regeln med tillståndet regler för ett fjärranslutet undernät, öppna ett Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-133">tooremove a Network ACL rule with permit rules for a remote subnet, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="6b6b6-134">Kopiera och klistra in hello skriptet nedan, konfigurera hello skript med egna värden och sedan köra hello skript.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-134">Copy and paste hello script below, configuring hello script with your own values, and then run hello script.</span></span>

1. <span data-ttu-id="6b6b6-135">Första steget är tooget hello nätverket ACL-objekt för hello virtuella slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-135">First step is tooget hello Network ACL object for hello virtual machine endpoint.</span></span> <span data-ttu-id="6b6b6-136">Sedan tar du bort hello regeln.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-136">You'll then remove hello ACL rule.</span></span> <span data-ttu-id="6b6b6-137">I det här fallet bort vi den efter regeln-ID.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-137">In this case, we are removing it by rule ID.</span></span> <span data-ttu-id="6b6b6-138">Detta tar endast bort hello regel-ID 0 från hello ACL.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-138">This will only remove hello rule ID 0 from hello ACL.</span></span> <span data-ttu-id="6b6b6-139">Hello ACL-objekt tas inte bort från hello virtuella slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-139">It does not remove hello ACL object from hello virtual machine endpoint.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. <span data-ttu-id="6b6b6-140">Därefter måste du tillämpa hello nätverket ACL objektet toohello virtuell datorslutpunkt och uppdatera hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-140">Next, you must apply hello Network ACL object toohello virtual machine endpoint and update hello virtual machine.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a><span data-ttu-id="6b6b6-141">Ta bort en ACL i nätverket från en virtuell datorslutpunkt</span><span class="sxs-lookup"><span data-stu-id="6b6b6-141">Remove a Network ACL from a virtual machine endpoint</span></span>
<span data-ttu-id="6b6b6-142">I vissa fall kanske du vill tooremove ett nätverk ACL-objekt från en virtuell dator-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-142">In certain scenarios, you might want tooremove a Network ACL object from a virtual machine endpoint.</span></span> <span data-ttu-id="6b6b6-143">toodo som öppnas i ett Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-143">toodo that, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="6b6b6-144">Kopiera och klistra in hello skriptet nedan, konfigurera hello skript med egna värden och sedan köra hello skript.</span><span class="sxs-lookup"><span data-stu-id="6b6b6-144">Copy and paste hello script below, configuring hello script with your own values, and then run hello script.</span></span>

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="6b6b6-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6b6b6-145">Next steps</span></span>
[<span data-ttu-id="6b6b6-146">Vad är ett nätverk åtkomstkontrollista (ACL)?</span><span class="sxs-lookup"><span data-stu-id="6b6b6-146">What is a Network Access Control List (ACL)?</span></span>](virtual-networks-acl.md)

