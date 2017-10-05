---
title: "Hantera Azure endpoint åtkomstkontrollistor | PowerShell | Klassiska | Microsoft Docs"
description: "Lär dig att hantera ACL: er med PowerShell"
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
ms.openlocfilehash: c3476908447380ccd7e8b9c0f1c2a55ae763cc1e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-endpoint-access-control-lists-using-powershell-in-the-classic-deployment-model"></a><span data-ttu-id="7411f-103">Hantera åtkomstkontrollistor för slutpunkten med PowerShell i den klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="7411f-103">Manage endpoint access control lists using PowerShell in the classic deployment model</span></span>
<span data-ttu-id="7411f-104">Du kan skapa och hantera nätverket åtkomstkontrollistor (ACL) för slutpunkter med hjälp av Azure PowerShell eller i hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="7411f-104">You can create and manage Network Access Control Lists (ACLs) for endpoints by using Azure PowerShell or in the Management Portal.</span></span> <span data-ttu-id="7411f-105">I det här avsnittet hittar du procedurer för ACL vanliga uppgifter som du kan utföra med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7411f-105">In this topic, you'll find procedures for ACL common tasks that you can complete using PowerShell.</span></span> <span data-ttu-id="7411f-106">Lista över Azure PowerShell cmdlets finns [Azure Management-Cmdlets](http://go.microsoft.com/fwlink/?LinkId=317721).</span><span class="sxs-lookup"><span data-stu-id="7411f-106">For the list of Azure PowerShell cmdlets see [Azure Management Cmdlets](http://go.microsoft.com/fwlink/?LinkId=317721).</span></span> <span data-ttu-id="7411f-107">Mer information om åtkomstkontrollistor finns [vad är ett nätverk åtkomstkontrollista (ACL)?](virtual-networks-acl.md).</span><span class="sxs-lookup"><span data-stu-id="7411f-107">For more information about ACLs, see [What is a Network Access Control List (ACL)?](virtual-networks-acl.md).</span></span> <span data-ttu-id="7411f-108">Om du vill hantera din ACL: er med hjälp av hanteringsportalen, se [så ange slutpunkter till en virtuell dator](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="7411f-108">If you want to manage your ACLs by using the Management Portal, see [How to Set Up Endpoints to a Virtual Machine](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="manage-network-acls-by-using-azure-powershell"></a><span data-ttu-id="7411f-109">Hantera nätverket ACL: er med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7411f-109">Manage Network ACLs by using Azure PowerShell</span></span>
<span data-ttu-id="7411f-110">Du kan använda Azure PowerShell-cmdlets för att skapa, ta bort och konfigurera (uppsättning) nätverket åtkomstkontrollistor (ACL).</span><span class="sxs-lookup"><span data-stu-id="7411f-110">You can use Azure PowerShell cmdlets to create, remove, and configure (set) Network Access Control Lists (ACLs).</span></span> <span data-ttu-id="7411f-111">Innehåller några exempel på några av de sätt som du kan konfigurera en ACL med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7411f-111">We've included a few examples of some of the ways you can configure an ACL using PowerShell.</span></span>

<span data-ttu-id="7411f-112">Du kan använda något av följande om du vill hämta en fullständig lista över ACL PowerShell-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="7411f-112">To retrieve a complete list of the ACL PowerShell cmdlets, you can use either of the following:</span></span>

    Get-Help *AzureACL*
    Get-Command -Noun AzureACLConfig

### <a name="create-a-network-acl-with-rules-that-permit-access-from-a-remote-subnet"></a><span data-ttu-id="7411f-113">Skapa en ACL i nätverket med regler som tillåter åtkomst från ett fjärranslutet undernät</span><span class="sxs-lookup"><span data-stu-id="7411f-113">Create a Network ACL with rules that permit access from a remote subnet</span></span>
<span data-ttu-id="7411f-114">I exemplet nedan kan du skapa en ny ACL som innehåller regler.</span><span class="sxs-lookup"><span data-stu-id="7411f-114">The example below illustrates a way to create a new ACL that contains rules.</span></span> <span data-ttu-id="7411f-115">Denna ACL tillämpas sedan på en virtuell dator-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="7411f-115">This ACL is then applied to a virtual machine endpoint.</span></span> <span data-ttu-id="7411f-116">ACL-regler i exemplet nedan kommer att tillåta åtkomst från ett fjärranslutet undernät.</span><span class="sxs-lookup"><span data-stu-id="7411f-116">The ACL rules in the example below will allow access from a remote subnet.</span></span> <span data-ttu-id="7411f-117">Öppna ett Azure PowerShell ISE för att skapa en ny ACL i nätverket med tillståndet regler för ett fjärranslutet undernät.</span><span class="sxs-lookup"><span data-stu-id="7411f-117">To create a new Network ACL with permit rules for a remote subnet, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="7411f-118">Kopiera och klistra in skriptet nedan, konfigurera skriptet med egna värden och sedan köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="7411f-118">Copy and paste the script below, configuring the script with your own values, and then run the script.</span></span>

1. <span data-ttu-id="7411f-119">Skapa det nya nätverk ACL-objektet.</span><span class="sxs-lookup"><span data-stu-id="7411f-119">Create the new network ACL object.</span></span>
   
        $acl1 = New-AzureAclConfig
2. <span data-ttu-id="7411f-120">Ange en regel som tillåter åtkomst från ett fjärranslutet undernät.</span><span class="sxs-lookup"><span data-stu-id="7411f-120">Set a rule that permits access from a remote subnet.</span></span> <span data-ttu-id="7411f-121">I exemplet nedan som du anger regel *100* (som har högre prioritet än 200 och högre) så att det fjärranslutna undernätet *10.0.0.0/8* åtkomst till den virtuella datorslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7411f-121">In the example below, you set rule *100* (which has priority over rule 200 and higher) to allow the remote subnet *10.0.0.0/8* access to the virtual machine endpoint.</span></span> <span data-ttu-id="7411f-122">Ersätt värdena med dina egna konfigurationskrav.</span><span class="sxs-lookup"><span data-stu-id="7411f-122">Replace the values with your own configuration requirements.</span></span> <span data-ttu-id="7411f-123">Namnet ”SharePoint ACL-config” ska ersättas med det egna namnet som du vill anropa den här regeln.</span><span class="sxs-lookup"><span data-stu-id="7411f-123">The name "SharePoint ACL config" should be replaced with the friendly name that you want to call this rule.</span></span>
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 100 `
            –Action permit –RemoteSubnet "10.0.0.0/8" `
            –Description "SharePoint ACL config"
3. <span data-ttu-id="7411f-124">Upprepa cmdleten, där du ersätter värdena med dina egna konfigurationskrav för ytterligare regler.</span><span class="sxs-lookup"><span data-stu-id="7411f-124">For additional rules, repeat the cmdlet, replacing the values with your own configuration requirements.</span></span> <span data-ttu-id="7411f-125">Se till att ändra den här regeln number ordern så att den ordning som du vill att regler tillämpas.</span><span class="sxs-lookup"><span data-stu-id="7411f-125">Be sure to change the rule number Order to reflect the order in which you want the rules to be applied.</span></span> <span data-ttu-id="7411f-126">Antalet nedre regeln företräde framför högre nummer.</span><span class="sxs-lookup"><span data-stu-id="7411f-126">The lower rule number takes precedence over the higher number.</span></span>
   
        Set-AzureAclConfig –AddRule –ACL $acl1 –Order 200 `
            –Action permit –RemoteSubnet "157.0.0.0/8" `
            –Description "web frontend ACL config"
4. <span data-ttu-id="7411f-127">Därefter kan du antingen skapa en ny slutpunkt (Lägg till) eller ange ACL för en befintlig slutpunkt (anges).</span><span class="sxs-lookup"><span data-stu-id="7411f-127">Next, you can either create a new endpoint (Add) or set the ACL for an existing endpoint (Set).</span></span> <span data-ttu-id="7411f-128">I det här exemplet ska vi lägga till en ny virtuell dator-slutpunkt som kallas ”web” och uppdatera virtuell datorslutpunkt med ACL-inställningar.</span><span class="sxs-lookup"><span data-stu-id="7411f-128">In this example, we will add a new virtual machine endpoint called "web" and update the virtual machine endpoint with the ACL settings.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Add-AzureEndpoint –Name "web" –Protocol tcp –Localport 80 - PublicPort 80 –ACL $acl1 `
        | Update-AzureVM
5. <span data-ttu-id="7411f-129">Sedan kombinera cmdletar och kör skriptet.</span><span class="sxs-lookup"><span data-stu-id="7411f-129">Next, combine the cmdlets and run the script.</span></span> <span data-ttu-id="7411f-130">I det här exemplet kombinerade cmdlets skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="7411f-130">For this example, the combined cmdlets would look like this:</span></span>
   
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

### <a name="remove-a-network-acl-rule-that-permits-access-from-a-remote-subnet"></a><span data-ttu-id="7411f-131">Ta bort en nätverket ACL-regel som tillåter åtkomst från ett fjärranslutet undernät</span><span class="sxs-lookup"><span data-stu-id="7411f-131">Remove a Network ACL rule that permits access from a remote subnet</span></span>
<span data-ttu-id="7411f-132">Exemplet nedan visar ett sätt att ta bort en regel för Portåtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="7411f-132">The example below illustrates a way to remove a network ACL rule.</span></span>  <span data-ttu-id="7411f-133">Ta bort en nätverket regeln med tillståndet regler för ett fjärranslutet undernät, öppna ett Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="7411f-133">To remove a Network ACL rule with permit rules for a remote subnet, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="7411f-134">Kopiera och klistra in skriptet nedan, konfigurera skriptet med egna värden och sedan köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="7411f-134">Copy and paste the script below, configuring the script with your own values, and then run the script.</span></span>

1. <span data-ttu-id="7411f-135">Första steget är att hämta nätverket ACL-objektet för den virtuella datorslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7411f-135">First step is to get the Network ACL object for the virtual machine endpoint.</span></span> <span data-ttu-id="7411f-136">Sedan tar du bort regeln för Portåtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="7411f-136">You'll then remove the ACL rule.</span></span> <span data-ttu-id="7411f-137">I det här fallet bort vi den efter regeln-ID.</span><span class="sxs-lookup"><span data-stu-id="7411f-137">In this case, we are removing it by rule ID.</span></span> <span data-ttu-id="7411f-138">Detta tar endast bort regel-ID 0 från ACL.</span><span class="sxs-lookup"><span data-stu-id="7411f-138">This will only remove the rule ID 0 from the ACL.</span></span> <span data-ttu-id="7411f-139">ACL-objekt tas inte bort från den virtuella datorslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7411f-139">It does not remove the ACL object from the virtual machine endpoint.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Get-AzureAclConfig –EndpointName "web" `
        | Set-AzureAclConfig –RemoveRule –ID 0 –ACL $acl1
2. <span data-ttu-id="7411f-140">Därefter måste du använda nätverket ACL-objekt till den virtuella datorslutpunkten och uppdatera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7411f-140">Next, you must apply the Network ACL object to the virtual machine endpoint and update the virtual machine.</span></span>
   
        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Set-AzureEndpoint –ACL $acl1 –Name "web" `
        | Update-AzureVM

### <a name="remove-a-network-acl-from-a-virtual-machine-endpoint"></a><span data-ttu-id="7411f-141">Ta bort en ACL i nätverket från en virtuell datorslutpunkt</span><span class="sxs-lookup"><span data-stu-id="7411f-141">Remove a Network ACL from a virtual machine endpoint</span></span>
<span data-ttu-id="7411f-142">I vissa fall kanske du vill ta bort ett nätverk ACL-objekt från den virtuella datorslutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7411f-142">In certain scenarios, you might want to remove a Network ACL object from a virtual machine endpoint.</span></span> <span data-ttu-id="7411f-143">För att göra det, öppnar du en Azure PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="7411f-143">To do that, open an Azure PowerShell ISE.</span></span> <span data-ttu-id="7411f-144">Kopiera och klistra in skriptet nedan, konfigurera skriptet med egna värden och sedan köra skriptet.</span><span class="sxs-lookup"><span data-stu-id="7411f-144">Copy and paste the script below, configuring the script with your own values, and then run the script.</span></span>

        Get-AzureVM –ServiceName $serviceName –Name $vmName `
        | Remove-AzureAclConfig –EndpointName "web" `
        | Update-AzureVM

## <a name="next-steps"></a><span data-ttu-id="7411f-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7411f-145">Next steps</span></span>
[<span data-ttu-id="7411f-146">Vad är ett nätverk åtkomstkontrollista (ACL)?</span><span class="sxs-lookup"><span data-stu-id="7411f-146">What is a Network Access Control List (ACL)?</span></span>](virtual-networks-acl.md)

