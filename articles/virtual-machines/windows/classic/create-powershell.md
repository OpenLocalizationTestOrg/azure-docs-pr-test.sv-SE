---
title: Skapa en Windows VM med PowerShell | Microsoft Docs
description: "Skapa Windows-datorer med hjälp av Azure PowerShell och den klassiska distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 42c0d4be-573c-45d1-b9b0-9327537702f7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 75c6cf17ee269ae169d9f2f748d0985ca07e454e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell-and-the-classic-deployment-model"></a><span data-ttu-id="30f8c-103">Skapa en Windows-dator med PowerShell och den klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="30f8c-103">Create a Windows virtual machine with PowerShell and the classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="30f8c-104">Azure portal – Windows</span><span class="sxs-lookup"><span data-stu-id="30f8c-104">Azure portal - Windows</span></span>](tutorial.md)
> * [<span data-ttu-id="30f8c-105">PowerShell - Windows</span><span class="sxs-lookup"><span data-stu-id="30f8c-105">PowerShell - Windows</span></span>](create-powershell.md)
> 
> 

<br>

> [!IMPORTANT] 
> <span data-ttu-id="30f8c-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="30f8c-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="30f8c-107">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="30f8c-107">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="30f8c-108">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="30f8c-108">Microsoft recommends that most new deployments use the Resource Manager model.</span></span> <span data-ttu-id="30f8c-109">Lär dig hur du [utför dessa steg med hjälp av Resource Manager-modellen](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="30f8c-109">Learn how to [perform these steps using the Resource Manager model](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="30f8c-110">Dessa steg visar hur du anpassar en uppsättning Azure PowerShell-kommandon som kan skapa och förkonfigurera en Windows-baserad Azure virtuella med hjälp av en byggblock-metod.</span><span class="sxs-lookup"><span data-stu-id="30f8c-110">These steps show you how to customize a set of Azure PowerShell commands that create and preconfigure a Windows-based Azure virtual machine by using a building block approach.</span></span> <span data-ttu-id="30f8c-111">Du kan använda den här processen att snabbt skapa en kommandouppsättning för en ny Windows-baserad virtuell dator och expandera en befintlig distribution eller skapa flera uppsättningar som snabbt skapar en anpassad utveckling och testning eller IT-teknikern miljö.</span><span class="sxs-lookup"><span data-stu-id="30f8c-111">You can use this process to quickly create a command set for a new Windows-based virtual machine and expand an existing deployment or to create multiple command sets that quickly build out a custom dev/test or IT pro environment.</span></span>

<span data-ttu-id="30f8c-112">Här följer en Fyll-i-tomrum metod för att skapa uppsättningar med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="30f8c-112">These steps follow a fill-in-the-blanks approach for creating Azure PowerShell command sets.</span></span> <span data-ttu-id="30f8c-113">Den här metoden kan vara användbart om du är nybörjare på PowerShell eller om du bara vill veta vilka värden du anger för lyckad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="30f8c-113">This approach can be useful if you are new to PowerShell or you just want to know what values to specify for successful configuration.</span></span> <span data-ttu-id="30f8c-114">Avancerade PowerShell-användare kan ta kommandon och ersätta deras egna värden för variabler (rader som börjar med ”$”).</span><span class="sxs-lookup"><span data-stu-id="30f8c-114">Advanced PowerShell users can take the commands and substitute their own values for the variables (the lines beginning with "$").</span></span>

<span data-ttu-id="30f8c-115">Om du inte redan har gjort så, följ instruktionerna i [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview) att installera Azure PowerShell på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="30f8c-115">If you haven't done so already, use the instructions in [How to install and configure Azure PowerShell](/powershell/azure/overview) to install Azure PowerShell on your local computer.</span></span> <span data-ttu-id="30f8c-116">Öppna en kommandotolk i Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="30f8c-116">Then, open a Windows PowerShell command prompt.</span></span>

## <a name="step-1-add-your-account"></a><span data-ttu-id="30f8c-117">Steg 1: Lägg till ditt konto</span><span class="sxs-lookup"><span data-stu-id="30f8c-117">Step 1: Add your account</span></span>
1. <span data-ttu-id="30f8c-118">I PowerShell-Kommandotolken skriver **Add-AzureAccount** och på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="30f8c-118">At the PowerShell prompt, type **Add-AzureAccount** and click **Enter**.</span></span> 
2. <span data-ttu-id="30f8c-119">Skriv e-postadressen som är associerade med din Azure-prenumeration och klicka på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="30f8c-119">Type in the email address associated with your Azure subscription and click **Continue**.</span></span> 
3. <span data-ttu-id="30f8c-120">Skriv in lösenordet för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="30f8c-120">Type in the password for your account.</span></span> 
4. <span data-ttu-id="30f8c-121">Klicka på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="30f8c-121">Click **Sign in**.</span></span>

## <a name="step-2-set-your-subscription-and-storage-account"></a><span data-ttu-id="30f8c-122">Steg 2: Ange din prenumeration och storage-konto</span><span class="sxs-lookup"><span data-stu-id="30f8c-122">Step 2: Set your subscription and storage account</span></span>
<span data-ttu-id="30f8c-123">Ange din Azure-prenumeration och storage-konto genom att köra dessa kommandon i Kommandotolken för Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="30f8c-123">Set your Azure subscription and storage account by running these commands at the Windows PowerShell command prompt.</span></span> <span data-ttu-id="30f8c-124">Ersätt allt inom citattecken, inklusive den < och > tecken, med rätt namn.</span><span class="sxs-lookup"><span data-stu-id="30f8c-124">Replace everything within the quotes, including the < and > characters, with the correct names.</span></span>

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

<span data-ttu-id="30f8c-125">Du kan hämta rätt prenumerationsnamn från egenskapen SubscriptionName för utdata från den **Get-AzureSubscription** kommando.</span><span class="sxs-lookup"><span data-stu-id="30f8c-125">You can get the correct subscription name from the SubscriptionName property of the output of the **Get-AzureSubscription** command.</span></span> <span data-ttu-id="30f8c-126">Du kan hämta rätt lagringskontonamnet från egenskapen Label för utdata från den **Get-AzureStorageAccount** kommando när du kör den **Välj AzureSubscription** kommando.</span><span class="sxs-lookup"><span data-stu-id="30f8c-126">You can get the correct storage account name from the Label property of the output of the **Get-AzureStorageAccount** command after you run the **Select-AzureSubscription** command.</span></span>

## <a name="step-3-determine-the-imagefamily"></a><span data-ttu-id="30f8c-127">Steg 3: Bestäm ImageFamily</span><span class="sxs-lookup"><span data-stu-id="30f8c-127">Step 3: Determine the ImageFamily</span></span>
<span data-ttu-id="30f8c-128">Därefter måste du bestämma ImageFamily eller etiketten för den specifika bilden som motsvarar den Azure-dator som du vill skapa.</span><span class="sxs-lookup"><span data-stu-id="30f8c-128">Next, you need to determine the ImageFamily or Label value for the specific image corresponding to the Azure virtual machine you want to create.</span></span> <span data-ttu-id="30f8c-129">Du kan hämta listan över tillgängliga ImageFamily värden med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="30f8c-129">You can get the list of available ImageFamily values with this command.</span></span>

    Get-AzureVMImage | select ImageFamily -Unique

<span data-ttu-id="30f8c-130">Här följer några exempel på ImageFamily värden för Windows-baserade datorer:</span><span class="sxs-lookup"><span data-stu-id="30f8c-130">Here are some examples of ImageFamily values for Windows-based computers:</span></span>

* <span data-ttu-id="30f8c-131">Windows Server 2012 R2 Datacenter</span><span class="sxs-lookup"><span data-stu-id="30f8c-131">Windows Server 2012 R2 Datacenter</span></span>
* <span data-ttu-id="30f8c-132">Windows Server 2008 R2 SP1</span><span class="sxs-lookup"><span data-stu-id="30f8c-132">Windows Server 2008 R2 SP1</span></span>
* <span data-ttu-id="30f8c-133">Windows Server 2016 Technical Preview 4</span><span class="sxs-lookup"><span data-stu-id="30f8c-133">Windows Server 2016 Technical Preview 4</span></span>
* <span data-ttu-id="30f8c-134">SQL Server 2012 SP1 Enterprise på Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="30f8c-134">SQL Server 2012 SP1 Enterprise on Windows Server 2012</span></span>

<span data-ttu-id="30f8c-135">Om du hittar den bild som du söker efter, öppnar du en ny instans av textredigerare ditt val eller PowerShell Integrated Scripting Environment (ISE).</span><span class="sxs-lookup"><span data-stu-id="30f8c-135">If you find the image you are looking for, open a fresh instance of the text editor of your choice or the PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="30f8c-136">Kopiera följande till ny textfil eller PowerShell ISE ersätter ImageFamily-värdet.</span><span class="sxs-lookup"><span data-stu-id="30f8c-136">Copy the following into the new text file or the PowerShell ISE, substituting the ImageFamily value.</span></span>

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

<span data-ttu-id="30f8c-137">I vissa fall är avbildningens namn i egenskapen Label i stället för ImageFamily-värdet.</span><span class="sxs-lookup"><span data-stu-id="30f8c-137">In some cases, the image name is in the Label property instead of the ImageFamily value.</span></span> <span data-ttu-id="30f8c-138">Om du inte kan hitta den bild som du söker efter med egenskapen ImageFamily lista avbildningar av egenskapen etikett med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="30f8c-138">If you didn't find the image that you are looking for using the ImageFamily property, list the images by their Label property with this command.</span></span>

    Get-AzureVMImage | select Label -Unique

<span data-ttu-id="30f8c-139">Om du hittar rätt avbildningen med det här kommandot kan du öppna en ny instans av textredigerare ditt val eller PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="30f8c-139">If you find the right image with this command, open a fresh instance of the text editor of your choice or the PowerShell ISE.</span></span> <span data-ttu-id="30f8c-140">Kopiera följande till ny textfil eller PowerShell ISE ersätter värdet etikett.</span><span class="sxs-lookup"><span data-stu-id="30f8c-140">Copy the following into the new text file or the PowerShell ISE, substituting the Label value.</span></span>

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a><span data-ttu-id="30f8c-141">Steg 4: Skapa dina kommandot set</span><span class="sxs-lookup"><span data-stu-id="30f8c-141">Step 4: Build your command set</span></span>
<span data-ttu-id="30f8c-142">Skapa resten av kommandot genom kopiering lämplig uppsättning block nedan i den nya textfilen eller av ISE och fylla i variabelvärdena och tar bort den < och > tecken.</span><span class="sxs-lookup"><span data-stu-id="30f8c-142">Build the rest of your command set by copying the appropriate set of blocks below into your new text file or the ISE and then filling in the variable values and removing the < and > characters.</span></span> <span data-ttu-id="30f8c-143">Finns två [exempel](#examples) i slutet av den här artikeln för en uppfattning av slutresultatet.</span><span class="sxs-lookup"><span data-stu-id="30f8c-143">See the two [examples](#examples) at the end of this article for an idea of the final result.</span></span>

<span data-ttu-id="30f8c-144">Starta ditt kommando anges genom att välja ett av dessa två kommandon (krävs).</span><span class="sxs-lookup"><span data-stu-id="30f8c-144">Start your command set by choosing one of these two command blocks (required).</span></span>

<span data-ttu-id="30f8c-145">Alternativ 1: Ange ett namn för virtuell dator och en storlek.</span><span class="sxs-lookup"><span data-stu-id="30f8c-145">Option 1: Specify a virtual machine name and a size.</span></span>

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

<span data-ttu-id="30f8c-146">Alternativ 2: Ange ett namn, storlek och namn på tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="30f8c-146">Option 2: Specify a name, size, and availability set name.</span></span>

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

<span data-ttu-id="30f8c-147">InstanceSize-värden för D-, DS- eller G-serien virtuella datorer finns i [virtuell dator och Molntjänststorlekar för Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="30f8c-147">For the InstanceSize values for D-, DS-, or G-series virtual machines, see [Virtual Machine and Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="30f8c-148">Om du har ett Enterprise-avtal med Software Assurance och vill dra nytta av Windows Server [Hybrid Använd förmån](https://azure.microsoft.com/pricing/hybrid-use-benefit/), lägga till den **- LicenseType** parametern till den  **Nya AzureVMConfig** cmdlet, skicka värdet **Windows_Server** för vanliga användningsfall.</span><span class="sxs-lookup"><span data-stu-id="30f8c-148">If you have an Enterprise Agreement with Software Assurance, and intend to take advantage of the Windows Server [Hybrid Use Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/), add the **-LicenseType** parameter to the **New-AzureVMConfig** cmdlet, passing the value **Windows_Server** for the typical use case.</span></span>  <span data-ttu-id="30f8c-149">Kontrollera att du använder en bild som du har överfört; Du kan inte använda en standardiserad avbildning från galleriet med Hybrid använda förmånen.</span><span class="sxs-lookup"><span data-stu-id="30f8c-149">Be sure you are using an image you have uploaded; you cannot use a standard image from the Gallery with the Hybrid Use Benefit.</span></span>
> 
> 

<span data-ttu-id="30f8c-150">Du kan också ange det lokala administratörskontot och lösenord för en fristående Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="30f8c-150">Optionally, for a standalone Windows computer, specify the local administrator account and password.</span></span>

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

<span data-ttu-id="30f8c-151">Välj ett starkt lösenord.</span><span class="sxs-lookup"><span data-stu-id="30f8c-151">Choose a strong password.</span></span> <span data-ttu-id="30f8c-152">Du kan kontrollera dess styrkan [lösenord layout: med starka lösenord](https://www.microsoft.com/security/pc-security/password-checker.aspx).</span><span class="sxs-lookup"><span data-stu-id="30f8c-152">To check its strength, see [Password Checker: Using Strong Passwords](https://www.microsoft.com/security/pc-security/password-checker.aspx).</span></span>

<span data-ttu-id="30f8c-153">Du kan också ange det lokala administratörskontot och lösenordet och domänen, och namn och lösenord för ett domänkonto för att lägga till Windows-dator till en befintlig Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="30f8c-153">Optionally, to add the Windows computer to an existing Active Directory domain, specify the local administrator account and password, the domain, and the name and password of a domain account.</span></span>

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="<FQDN of the domain that the machine is joining>"
    $domacctdomain="<domain of the account that has permission to add the machine to the domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

<span data-ttu-id="30f8c-154">För ytterligare före konfigurationsalternativ för Windows-baserade virtuella datorer, se syntaxen för den **Windows** och **WindowsDomain** parameteruppsättningar [Lägg till AzureProvisioningConfig ](/powershell/module/azure/add-azureprovisioningconfig).</span><span class="sxs-lookup"><span data-stu-id="30f8c-154">For additional pre-configuration options for Windows-based virtual machines, see the syntax for the **Windows** and **WindowsDomain** parameter sets in [Add-AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).</span></span>

<span data-ttu-id="30f8c-155">Du kan också tilldela den virtuella datorn en specifik IP-adress, som kallas en statisk DIP.</span><span class="sxs-lookup"><span data-stu-id="30f8c-155">Optionally, assign the virtual machine a specific IP address, known as a static DIP.</span></span>

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

<span data-ttu-id="30f8c-156">Du kan kontrollera att en specifik IP-adress är tillgänglig med:</span><span class="sxs-lookup"><span data-stu-id="30f8c-156">You can verify that a specific IP address is available with:</span></span>

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

<span data-ttu-id="30f8c-157">Du kan också tilldela den virtuella datorn till ett specifikt undernät i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="30f8c-157">Optionally, assign the virtual machine to a specific subnet in an Azure virtual network.</span></span>

    $vm1 | Set-AzureSubnet -SubnetNames "<name of the subnet>"

<span data-ttu-id="30f8c-158">Du kan också lägga till en enda datadisk till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="30f8c-158">Optionally, add a single data disk to the virtual machine.</span></span>

    $disksize=<size of the disk in GB>
    $disklabel="<the label on the disk>"
    $lun=<Logical Unit Number (LUN) of the disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

<span data-ttu-id="30f8c-159">Ange $hcaching till ”None” för en Active Directory-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="30f8c-159">For an Active Directory domain controller, set $hcaching to "None".</span></span>

<span data-ttu-id="30f8c-160">Du kan också lägga till den virtuella datorn till en befintlig belastningsutjämnad uppsättning för externa trafiken.</span><span class="sxs-lookup"><span data-stu-id="30f8c-160">Optionally, add the virtual machine to an existing load-balanced set for external traffic.</span></span>

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of the internal port>
    $pubport=<port number of the external port>
    $endpointname="<name of the endpoint>"
    $lbsetname="<name of the existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

<span data-ttu-id="30f8c-161">Slutligen ska du välja en av dessa nödvändiga kommando för att skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="30f8c-161">Finally, choose one of these required command blocks for creating the virtual machine.</span></span>

<span data-ttu-id="30f8c-162">Alternativ 1: Skapa den virtuella datorn i en befintlig molntjänst.</span><span class="sxs-lookup"><span data-stu-id="30f8c-162">Option 1: Create the virtual machine in an existing cloud service.</span></span>

    New-AzureVM –ServiceName "<short name of the cloud service>" -VMs $vm1

<span data-ttu-id="30f8c-163">Det korta namnet för Molntjänsten är det namn som visas i listan över molntjänster i Azure-portalen eller i listan över resursgrupper i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="30f8c-163">The short name of the cloud service is the name that appears in the list of Cloud Services in the Azure portal or in the list of Resource Groups in the Azure portal.</span></span>

<span data-ttu-id="30f8c-164">Alternativ 2: Skapa den virtuella datorn i en befintlig molntjänst och virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="30f8c-164">Option 2: Create the virtual machine in an existing cloud service and virtual network.</span></span>

    $svcname="<short name of the cloud service>"
    $vnetname="<name of the virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a><span data-ttu-id="30f8c-165">Steg 5: Kör din kommandot set</span><span class="sxs-lookup"><span data-stu-id="30f8c-165">Step 5: Run your command set</span></span>
<span data-ttu-id="30f8c-166">Granska uppsättningen Azure PowerShell-kommando du skapat i en textredigerare eller PowerShell ISE som består av flera block med kommandon från steg 4.</span><span class="sxs-lookup"><span data-stu-id="30f8c-166">Review the Azure PowerShell command set you built in your text editor or the PowerShell ISE consisting of multiple blocks of commands from step 4.</span></span> <span data-ttu-id="30f8c-167">Se till att du har angett alla nödvändiga variabler och att de har rätt värden.</span><span class="sxs-lookup"><span data-stu-id="30f8c-167">Ensure that you have specified all the needed variables and that they have the correct values.</span></span> <span data-ttu-id="30f8c-168">Kontrollera också att du har tagit bort alla de < och > tecken.</span><span class="sxs-lookup"><span data-stu-id="30f8c-168">Also make sure that you have removed all the < and > characters.</span></span>

<span data-ttu-id="30f8c-169">Om du använder en textredigerare, kopiera kommandot till Urklipp och högerklickar på dina öppna Windows PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="30f8c-169">If you are using a text editor, copy the command set to the clipboard and then right-click your open Windows PowerShell command prompt.</span></span> <span data-ttu-id="30f8c-170">Detta utfärda kommandot som en serie av PowerShell-kommandon och skapa din virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="30f8c-170">This will issue the command set as a series of PowerShell commands and create your Azure virtual machine.</span></span> <span data-ttu-id="30f8c-171">Du kan också köra kommandot i PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="30f8c-171">Alternately, run the command set in the PowerShell ISE.</span></span>

<span data-ttu-id="30f8c-172">Om du skapar den här virtuella datorn igen eller en liknande, kan du:</span><span class="sxs-lookup"><span data-stu-id="30f8c-172">If you will be creating this virtual machine again or a similar one, you can:</span></span>

* <span data-ttu-id="30f8c-173">Spara det här kommandot som angetts som en PowerShell-skriptfil (*.ps1).</span><span class="sxs-lookup"><span data-stu-id="30f8c-173">Save this command set as a PowerShell script file (*.ps1).</span></span>
* <span data-ttu-id="30f8c-174">Spara det här kommandot som angetts som en Azure Automation-runbook i den **Automation-konton** på Azure portal.</span><span class="sxs-lookup"><span data-stu-id="30f8c-174">Save this command set as an Azure Automation runbook in the **Automation Accounts** section of the Azure portal.</span></span>

## <span data-ttu-id="30f8c-175"><a id="examples"></a>Exempel</span><span class="sxs-lookup"><span data-stu-id="30f8c-175"><a id="examples"></a>Examples</span></span>
<span data-ttu-id="30f8c-176">Här är två exempel på hur stegen ovan för att skapa Azure PowerShell-kommando anger som skapar Windows-baserade virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="30f8c-176">Here are two examples of using the steps above to build Azure PowerShell command sets that create Windows-based Azure virtual machines.</span></span>

### <a name="example-1"></a><span data-ttu-id="30f8c-177">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="30f8c-177">Example 1</span></span>
<span data-ttu-id="30f8c-178">Jag behöver en PowerShell kommandouppsättning skapa den första virtuella datorn för en Active Directory-domänkontrollant som:</span><span class="sxs-lookup"><span data-stu-id="30f8c-178">I need a PowerShell command set to create the initial virtual machine for an Active Directory domain controller that:</span></span>

* <span data-ttu-id="30f8c-179">Använder Windows Server 2012 R2 Datacenter-avbildning.</span><span class="sxs-lookup"><span data-stu-id="30f8c-179">Uses the Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="30f8c-180">Har namnet AZDC1.</span><span class="sxs-lookup"><span data-stu-id="30f8c-180">Has the name AZDC1.</span></span>
* <span data-ttu-id="30f8c-181">Är en fristående dator.</span><span class="sxs-lookup"><span data-stu-id="30f8c-181">Is a standalone computer.</span></span>
* <span data-ttu-id="30f8c-182">Har en ytterligare datadisk på 20 GB.</span><span class="sxs-lookup"><span data-stu-id="30f8c-182">Has an additional data disk of 20 GB.</span></span>
* <span data-ttu-id="30f8c-183">Har den statiska IP-adressen 192.168.244.4.</span><span class="sxs-lookup"><span data-stu-id="30f8c-183">Has the static IP address 192.168.244.4.</span></span>
* <span data-ttu-id="30f8c-184">Är i BackEnd-undernät för det virtuella nätverket AZDatacenter.</span><span class="sxs-lookup"><span data-stu-id="30f8c-184">Is in the BackEnd subnet of the AZDatacenter virtual network.</span></span>
* <span data-ttu-id="30f8c-185">Är i Azure-leksaksladan Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="30f8c-185">Is in the Azure-TailspinToys cloud service.</span></span>

<span data-ttu-id="30f8c-186">Det här är den motsvarande Azure PowerShell-kommando som vill skapa den virtuella datorn med tomma rader mellan varje block för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="30f8c-186">Here is the corresponding Azure PowerShell command set to create this virtual machine, with blank lines between each block for readability.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type the name and password of the local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

    $vm1 | Set-AzureSubnet -SubnetNames "BackEnd"

    $vm1 | Set-AzureStaticVNetIP -IPAddress 192.168.244.4

    $disksize=20
    $disklabel="DCData"
    $lun=0
    $hcaching="None"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

### <a name="example-2"></a><span data-ttu-id="30f8c-187">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="30f8c-187">Example 2</span></span>
<span data-ttu-id="30f8c-188">Jag behöver en PowerShell kommandouppsättning skapa en virtuell dator för en line-of-business-server som:</span><span class="sxs-lookup"><span data-stu-id="30f8c-188">I need a PowerShell command set to create a virtual machine for a line-of-business server that:</span></span>

* <span data-ttu-id="30f8c-189">Använder Windows Server 2012 R2 Datacenter-avbildning.</span><span class="sxs-lookup"><span data-stu-id="30f8c-189">Uses the Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="30f8c-190">Har namnet LOB1.</span><span class="sxs-lookup"><span data-stu-id="30f8c-190">Has the name LOB1.</span></span>
* <span data-ttu-id="30f8c-191">Är medlem i domänen corp.contoso.com.</span><span class="sxs-lookup"><span data-stu-id="30f8c-191">Is a member of the corp.contoso.com domain.</span></span>
* <span data-ttu-id="30f8c-192">Har en ytterligare datadisk 200 GB.</span><span class="sxs-lookup"><span data-stu-id="30f8c-192">Has an additional data disk of 200 GB.</span></span>
* <span data-ttu-id="30f8c-193">Är i undernätet FrontEnd för det virtuella nätverket AZDatacenter.</span><span class="sxs-lookup"><span data-stu-id="30f8c-193">Is in the FrontEnd subnet of the AZDatacenter virtual network.</span></span>
* <span data-ttu-id="30f8c-194">Är i Azure-leksaksladan Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="30f8c-194">Is in the Azure-TailspinToys cloud service.</span></span>

<span data-ttu-id="30f8c-195">Det här är den motsvarande Azure PowerShell-kommando som vill skapa den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="30f8c-195">Here is the corresponding Azure PowerShell command set to create this virtual machine.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type the name and password of the local administrator account."
    $cred2=Get-Credential –Message "Now type the name (not including the domain) and password of an account that has permission to add the machine to the domain."
    $domaindns="corp.contoso.com"
    $domacctdomain="CORP"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "FrontEnd"

    $disksize=200
    $disklabel="LOBData"
    $lun=0
    $hcaching="ReadWrite"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

    $svcname="Azure-TailspinToys"
    $vnetname="AZDatacenter"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname


## <a name="next-steps"></a><span data-ttu-id="30f8c-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="30f8c-196">Next steps</span></span>
<span data-ttu-id="30f8c-197">Om du behöver en OS-disk som är större än 127 GB, kan du [Expandera enhetens OS](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="30f8c-197">If you need an OS disk that is larger than 127 GB, you can [expand the OS drive](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

