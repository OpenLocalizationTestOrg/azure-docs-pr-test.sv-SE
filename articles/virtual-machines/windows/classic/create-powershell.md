---
title: aaaCreate en virtuell Windows-dator med PowerShell | Microsoft Docs
description: "Skapa Windows-datorer med hjälp av Azure PowerShell och hello klassiska distributionsmodellen."
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
ms.openlocfilehash: 5339f458c1f08f6e1752a53191f19402fab8ea25
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-powershell-and-hello-classic-deployment-model"></a><span data-ttu-id="3b3da-103">Skapa en virtuell Windows-dator med PowerShell och hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="3b3da-103">Create a Windows virtual machine with PowerShell and hello classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3b3da-104">Azure portal – Windows</span><span class="sxs-lookup"><span data-stu-id="3b3da-104">Azure portal - Windows</span></span>](tutorial.md)
> * [<span data-ttu-id="3b3da-105">PowerShell - Windows</span><span class="sxs-lookup"><span data-stu-id="3b3da-105">PowerShell - Windows</span></span>](create-powershell.md)
> 
> 

<br>

> [!IMPORTANT] 
> <span data-ttu-id="3b3da-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3b3da-106">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="3b3da-107">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="3b3da-107">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="3b3da-108">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="3b3da-108">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="3b3da-109">Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3b3da-109">Learn how too[perform these steps using hello Resource Manager model](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

<span data-ttu-id="3b3da-110">Dessa steg visar hur toocustomize en uppsättning Azure PowerShell-kommandon som skapa och förkonfigurera en Windows-baserad Azure virtuella med hjälp av en byggblock-metod.</span><span class="sxs-lookup"><span data-stu-id="3b3da-110">These steps show you how toocustomize a set of Azure PowerShell commands that create and preconfigure a Windows-based Azure virtual machine by using a building block approach.</span></span> <span data-ttu-id="3b3da-111">Du kan använda den här processen tooquickly skapa en kommandouppsättning för en ny Windows-baserad virtuell dator och expandera en befintlig distribution eller toocreate flera anger kommandot som snabbt skapa en anpassad utveckling och testning eller IT-teknikern miljö.</span><span class="sxs-lookup"><span data-stu-id="3b3da-111">You can use this process tooquickly create a command set for a new Windows-based virtual machine and expand an existing deployment or toocreate multiple command sets that quickly build out a custom dev/test or IT pro environment.</span></span>

<span data-ttu-id="3b3da-112">Här följer en Fyll-i-tomrum metod för att skapa uppsättningar med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3b3da-112">These steps follow a fill-in-the-blanks approach for creating Azure PowerShell command sets.</span></span> <span data-ttu-id="3b3da-113">Den här metoden kan vara användbart om du är ny tooPowerShell eller du bara vill tooknow vilka värden toospecify för lyckad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="3b3da-113">This approach can be useful if you are new tooPowerShell or you just want tooknow what values toospecify for successful configuration.</span></span> <span data-ttu-id="3b3da-114">Avancerade PowerShell-användare kan ta hello kommandon och ersätta sina egna värden för variabler som hello (hello rader som börjar med ”$”).</span><span class="sxs-lookup"><span data-stu-id="3b3da-114">Advanced PowerShell users can take hello commands and substitute their own values for hello variables (hello lines beginning with "$").</span></span>

<span data-ttu-id="3b3da-115">Om du inte redan gjort det, Använd hello-instruktioner i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) tooinstall Azure PowerShell på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="3b3da-115">If you haven't done so already, use hello instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) tooinstall Azure PowerShell on your local computer.</span></span> <span data-ttu-id="3b3da-116">Öppna en kommandotolk i Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3b3da-116">Then, open a Windows PowerShell command prompt.</span></span>

## <a name="step-1-add-your-account"></a><span data-ttu-id="3b3da-117">Steg 1: Lägg till ditt konto</span><span class="sxs-lookup"><span data-stu-id="3b3da-117">Step 1: Add your account</span></span>
1. <span data-ttu-id="3b3da-118">I hello PowerShell-Kommandotolken, Skriv **Add-AzureAccount** och på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="3b3da-118">At hello PowerShell prompt, type **Add-AzureAccount** and click **Enter**.</span></span> 
2. <span data-ttu-id="3b3da-119">Ange hello e-postadress som är associerad med din Azure-prenumeration och klicka på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="3b3da-119">Type in hello email address associated with your Azure subscription and click **Continue**.</span></span> 
3. <span data-ttu-id="3b3da-120">Ange hello lösenord för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="3b3da-120">Type in hello password for your account.</span></span> 
4. <span data-ttu-id="3b3da-121">Klicka på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="3b3da-121">Click **Sign in**.</span></span>

## <a name="step-2-set-your-subscription-and-storage-account"></a><span data-ttu-id="3b3da-122">Steg 2: Ange din prenumeration och storage-konto</span><span class="sxs-lookup"><span data-stu-id="3b3da-122">Step 2: Set your subscription and storage account</span></span>
<span data-ttu-id="3b3da-123">Ange din Azure-prenumeration och storage-konto genom att köra dessa kommandon i Kommandotolken för hello Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3b3da-123">Set your Azure subscription and storage account by running these commands at hello Windows PowerShell command prompt.</span></span> <span data-ttu-id="3b3da-124">Ersätt allt inom hello citattecken, inklusive Hej < och > tecken, med hello rätt namn.</span><span class="sxs-lookup"><span data-stu-id="3b3da-124">Replace everything within hello quotes, including hello < and > characters, with hello correct names.</span></span>

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

<span data-ttu-id="3b3da-125">Du kan hämta hello rätt prenumerationsnamn från hello SubscriptionName-egenskapen för hello utdata från hello **Get-AzureSubscription** kommando.</span><span class="sxs-lookup"><span data-stu-id="3b3da-125">You can get hello correct subscription name from hello SubscriptionName property of hello output of hello **Get-AzureSubscription** command.</span></span> <span data-ttu-id="3b3da-126">Du kan hämta hello rätt lagringskontonamnet från hello etikettegenskap hello utdata från hello **Get-AzureStorageAccount** kommandot när du har kört hello **Välj AzureSubscription** kommando.</span><span class="sxs-lookup"><span data-stu-id="3b3da-126">You can get hello correct storage account name from hello Label property of hello output of hello **Get-AzureStorageAccount** command after you run hello **Select-AzureSubscription** command.</span></span>

## <a name="step-3-determine-hello-imagefamily"></a><span data-ttu-id="3b3da-127">Steg 3: Bestäm hello ImageFamily</span><span class="sxs-lookup"><span data-stu-id="3b3da-127">Step 3: Determine hello ImageFamily</span></span>
<span data-ttu-id="3b3da-128">Därefter behöver du toodetermine hello ImageFamily eller etikettvärde för hello viss bild motsvarande toohello virtuell Azure-dator du vill toocreate.</span><span class="sxs-lookup"><span data-stu-id="3b3da-128">Next, you need toodetermine hello ImageFamily or Label value for hello specific image corresponding toohello Azure virtual machine you want toocreate.</span></span> <span data-ttu-id="3b3da-129">Du kan hämta hello lista över tillgängliga ImageFamily värden med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="3b3da-129">You can get hello list of available ImageFamily values with this command.</span></span>

    Get-AzureVMImage | select ImageFamily -Unique

<span data-ttu-id="3b3da-130">Här följer några exempel på ImageFamily värden för Windows-baserade datorer:</span><span class="sxs-lookup"><span data-stu-id="3b3da-130">Here are some examples of ImageFamily values for Windows-based computers:</span></span>

* <span data-ttu-id="3b3da-131">Windows Server 2012 R2 Datacenter</span><span class="sxs-lookup"><span data-stu-id="3b3da-131">Windows Server 2012 R2 Datacenter</span></span>
* <span data-ttu-id="3b3da-132">Windows Server 2008 R2 SP1</span><span class="sxs-lookup"><span data-stu-id="3b3da-132">Windows Server 2008 R2 SP1</span></span>
* <span data-ttu-id="3b3da-133">Windows Server 2016 Technical Preview 4</span><span class="sxs-lookup"><span data-stu-id="3b3da-133">Windows Server 2016 Technical Preview 4</span></span>
* <span data-ttu-id="3b3da-134">SQL Server 2012 SP1 Enterprise på Windows Server 2012</span><span class="sxs-lookup"><span data-stu-id="3b3da-134">SQL Server 2012 SP1 Enterprise on Windows Server 2012</span></span>

<span data-ttu-id="3b3da-135">Om du hittar hello bilden som du letar efter, öppnar du en ny instans av hello textredigerare på ditt val eller hello PowerShell Integrated Scripting Environment (ISE).</span><span class="sxs-lookup"><span data-stu-id="3b3da-135">If you find hello image you are looking for, open a fresh instance of hello text editor of your choice or hello PowerShell Integrated Scripting Environment (ISE).</span></span> <span data-ttu-id="3b3da-136">Kopiera följande hello till hello ny textfil eller hello PowerShell ISE ersätter hello ImageFamily värde.</span><span class="sxs-lookup"><span data-stu-id="3b3da-136">Copy hello following into hello new text file or hello PowerShell ISE, substituting hello ImageFamily value.</span></span>

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

<span data-ttu-id="3b3da-137">I vissa fall är hello avbildningens namn i hello etikettegenskapen i stället för hello ImageFamily värde.</span><span class="sxs-lookup"><span data-stu-id="3b3da-137">In some cases, hello image name is in hello Label property instead of hello ImageFamily value.</span></span> <span data-ttu-id="3b3da-138">Om du inte kan hitta hello-avbildning som du söker efter med hello ImageFamily egenskapen lista hello avbildningar av egenskapen etikett med det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="3b3da-138">If you didn't find hello image that you are looking for using hello ImageFamily property, list hello images by their Label property with this command.</span></span>

    Get-AzureVMImage | select Label -Unique

<span data-ttu-id="3b3da-139">Om du hittar hello höger bild med det här kommandot kan du öppna en ny instans av hello textredigerare hello PowerShell ISE eller val.</span><span class="sxs-lookup"><span data-stu-id="3b3da-139">If you find hello right image with this command, open a fresh instance of hello text editor of your choice or hello PowerShell ISE.</span></span> <span data-ttu-id="3b3da-140">Kopiera följande hello till hello ny textfil eller hello PowerShell ISE ersätter hello etikettvärde.</span><span class="sxs-lookup"><span data-stu-id="3b3da-140">Copy hello following into hello new text file or hello PowerShell ISE, substituting hello Label value.</span></span>

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a><span data-ttu-id="3b3da-141">Steg 4: Skapa dina kommandot set</span><span class="sxs-lookup"><span data-stu-id="3b3da-141">Step 4: Build your command set</span></span>
<span data-ttu-id="3b3da-142">Skapa hello resten av kommandot genom att kopiera hello rätt antal block nedan till dina nya textfil eller hello ISE fylla i hello variabelvärden och tar bort Hej < och > tecken.</span><span class="sxs-lookup"><span data-stu-id="3b3da-142">Build hello rest of your command set by copying hello appropriate set of blocks below into your new text file or hello ISE and then filling in hello variable values and removing hello < and > characters.</span></span> <span data-ttu-id="3b3da-143">Se hello två [exempel](#examples) hello slutet av den här artikeln för en uppfattning av hello slutresultatet.</span><span class="sxs-lookup"><span data-stu-id="3b3da-143">See hello two [examples](#examples) at hello end of this article for an idea of hello final result.</span></span>

<span data-ttu-id="3b3da-144">Starta ditt kommando anges genom att välja ett av dessa två kommandon (krävs).</span><span class="sxs-lookup"><span data-stu-id="3b3da-144">Start your command set by choosing one of these two command blocks (required).</span></span>

<span data-ttu-id="3b3da-145">Alternativ 1: Ange ett namn för virtuell dator och en storlek.</span><span class="sxs-lookup"><span data-stu-id="3b3da-145">Option 1: Specify a virtual machine name and a size.</span></span>

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

<span data-ttu-id="3b3da-146">Alternativ 2: Ange ett namn, storlek och namn på tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="3b3da-146">Option 2: Specify a name, size, and availability set name.</span></span>

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

<span data-ttu-id="3b3da-147">Hej InstanceSize värden för D-, DS- eller G-serien virtuella datorer, se [virtuell dator och Molntjänststorlekar för Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b3da-147">For hello InstanceSize values for D-, DS-, or G-series virtual machines, see [Virtual Machine and Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="3b3da-148">Om du har ett Enterprise-avtal med Software Assurance och avser tootake nytta av hello Windows Server [Hybrid Använd förmån](https://azure.microsoft.com/pricing/hybrid-use-benefit/), lägga till den **- LicenseType** parametern toohello  **Nya AzureVMConfig** cmdlet, skicka hello värdet **Windows_Server** hello vanliga användningsfall.</span><span class="sxs-lookup"><span data-stu-id="3b3da-148">If you have an Enterprise Agreement with Software Assurance, and intend tootake advantage of hello Windows Server [Hybrid Use Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/), add the **-LicenseType** parameter toohello **New-AzureVMConfig** cmdlet, passing hello value **Windows_Server** for hello typical use case.</span></span>  <span data-ttu-id="3b3da-149">Kontrollera att du använder en bild som du har överfört; Du kan inte använda en standardiserad avbildning från hello galleriet med hello Hybrid använda förmånen.</span><span class="sxs-lookup"><span data-stu-id="3b3da-149">Be sure you are using an image you have uploaded; you cannot use a standard image from hello Gallery with hello Hybrid Use Benefit.</span></span>
> 
> 

<span data-ttu-id="3b3da-150">Du kan också ange hello lokalt administratörskonto och lösenord för en fristående Windows-dator.</span><span class="sxs-lookup"><span data-stu-id="3b3da-150">Optionally, for a standalone Windows computer, specify hello local administrator account and password.</span></span>

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

<span data-ttu-id="3b3da-151">Välj ett starkt lösenord.</span><span class="sxs-lookup"><span data-stu-id="3b3da-151">Choose a strong password.</span></span> <span data-ttu-id="3b3da-152">toocheck dess styrkan finns [lösenord layout: med starka lösenord](https://www.microsoft.com/security/pc-security/password-checker.aspx).</span><span class="sxs-lookup"><span data-stu-id="3b3da-152">toocheck its strength, see [Password Checker: Using Strong Passwords](https://www.microsoft.com/security/pc-security/password-checker.aspx).</span></span>

<span data-ttu-id="3b3da-153">Du kan också ange tooadd hello Windows datorn tooan befintliga Active Directory-domän, hello lokalt administratörskonto och lösenord, hello domän och hello namnet och lösenordet för ett domänkonto.</span><span class="sxs-lookup"><span data-stu-id="3b3da-153">Optionally, tooadd hello Windows computer tooan existing Active Directory domain, specify hello local administrator account and password, hello domain, and hello name and password of a domain account.</span></span>

    $cred1=Get-Credential –Message "Type hello name and password of hello local administrator account."
    $cred2=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account that has permission tooadd hello machine toohello domain."
    $domaindns="<FQDN of hello domain that hello machine is joining>"
    $domacctdomain="<domain of hello account that has permission tooadd hello machine toohello domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

<span data-ttu-id="3b3da-154">Ytterligare före konfigurationsalternativ för Windows-baserade virtuella datorer, finns hello syntax för hello **Windows** och **WindowsDomain** parameteruppsättningar [ Lägg till AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).</span><span class="sxs-lookup"><span data-stu-id="3b3da-154">For additional pre-configuration options for Windows-based virtual machines, see hello syntax for hello **Windows** and **WindowsDomain** parameter sets in [Add-AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).</span></span>

<span data-ttu-id="3b3da-155">Du kan också tilldela hello virtuell dator en specifik IP-adress, som kallas en statisk DIP.</span><span class="sxs-lookup"><span data-stu-id="3b3da-155">Optionally, assign hello virtual machine a specific IP address, known as a static DIP.</span></span>

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

<span data-ttu-id="3b3da-156">Du kan kontrollera att en specifik IP-adress är tillgänglig med:</span><span class="sxs-lookup"><span data-stu-id="3b3da-156">You can verify that a specific IP address is available with:</span></span>

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

<span data-ttu-id="3b3da-157">Du kan också tilldela hello virtuella tooa specifika undernät i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="3b3da-157">Optionally, assign hello virtual machine tooa specific subnet in an Azure virtual network.</span></span>

    $vm1 | Set-AzureSubnet -SubnetNames "<name of hello subnet>"

<span data-ttu-id="3b3da-158">Du kan också lägga till en enda disk toohello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="3b3da-158">Optionally, add a single data disk toohello virtual machine.</span></span>

    $disksize=<size of hello disk in GB>
    $disklabel="<hello label on hello disk>"
    $lun=<Logical Unit Number (LUN) of hello disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

<span data-ttu-id="3b3da-159">För en Active Directory-domänkontrollanten, ange $hcaching för ”None”.</span><span class="sxs-lookup"><span data-stu-id="3b3da-159">For an Active Directory domain controller, set $hcaching too"None".</span></span>

<span data-ttu-id="3b3da-160">Du kan också lägga till hello virtuella tooan befintlig belastningsutjämnad uppsättning för externa trafiken.</span><span class="sxs-lookup"><span data-stu-id="3b3da-160">Optionally, add hello virtual machine tooan existing load-balanced set for external traffic.</span></span>

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of hello internal port>
    $pubport=<port number of hello external port>
    $endpointname="<name of hello endpoint>"
    $lbsetname="<name of hello existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

<span data-ttu-id="3b3da-161">Slutligen ska du välja en av dessa nödvändiga kommando för att skapa hello virtuella dator.</span><span class="sxs-lookup"><span data-stu-id="3b3da-161">Finally, choose one of these required command blocks for creating hello virtual machine.</span></span>

<span data-ttu-id="3b3da-162">Alternativ 1: Skapa hello virtuell dator i en befintlig molntjänst.</span><span class="sxs-lookup"><span data-stu-id="3b3da-162">Option 1: Create hello virtual machine in an existing cloud service.</span></span>

    New-AzureVM –ServiceName "<short name of hello cloud service>" -VMs $vm1

<span data-ttu-id="3b3da-163">hello är kort för hello Molntjänsten hello namnet som visas i hello listan med molntjänster i hello Azure-portalen eller hello listan över resursgrupper i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3b3da-163">hello short name of hello cloud service is hello name that appears in hello list of Cloud Services in hello Azure portal or in hello list of Resource Groups in hello Azure portal.</span></span>

<span data-ttu-id="3b3da-164">Alternativ 2: Skapa hello virtuell dator i en befintlig molntjänst och virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="3b3da-164">Option 2: Create hello virtual machine in an existing cloud service and virtual network.</span></span>

    $svcname="<short name of hello cloud service>"
    $vnetname="<name of hello virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a><span data-ttu-id="3b3da-165">Steg 5: Kör din kommandot set</span><span class="sxs-lookup"><span data-stu-id="3b3da-165">Step 5: Run your command set</span></span>
<span data-ttu-id="3b3da-166">Granska hello Azure PowerShell-kommandot set du skapat i en textredigerare eller hello PowerShell ISE som består av flera block med kommandon från steg 4.</span><span class="sxs-lookup"><span data-stu-id="3b3da-166">Review hello Azure PowerShell command set you built in your text editor or hello PowerShell ISE consisting of multiple blocks of commands from step 4.</span></span> <span data-ttu-id="3b3da-167">Se till att du har angett alla hello behövs variabler och att de har rätt hello-värden.</span><span class="sxs-lookup"><span data-stu-id="3b3da-167">Ensure that you have specified all hello needed variables and that they have hello correct values.</span></span> <span data-ttu-id="3b3da-168">Kontrollera också att du har tagit bort alla Hej < och > tecken.</span><span class="sxs-lookup"><span data-stu-id="3b3da-168">Also make sure that you have removed all hello < and > characters.</span></span>

<span data-ttu-id="3b3da-169">Om du använder en textredigerare, kopiera hello kommandot Ange toohello Urklipp och högerklicka sedan på dina öppna Windows PowerShell-Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="3b3da-169">If you are using a text editor, copy hello command set toohello clipboard and then right-click your open Windows PowerShell command prompt.</span></span> <span data-ttu-id="3b3da-170">Detta utfärda hello kommandouppsättning som en serie av PowerShell-kommandon och skapa din virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="3b3da-170">This will issue hello command set as a series of PowerShell commands and create your Azure virtual machine.</span></span> <span data-ttu-id="3b3da-171">Alternativt kan köra hello-kommando i hello PowerShell ISE.</span><span class="sxs-lookup"><span data-stu-id="3b3da-171">Alternately, run hello command set in hello PowerShell ISE.</span></span>

<span data-ttu-id="3b3da-172">Om du skapar den här virtuella datorn igen eller en liknande, kan du:</span><span class="sxs-lookup"><span data-stu-id="3b3da-172">If you will be creating this virtual machine again or a similar one, you can:</span></span>

* <span data-ttu-id="3b3da-173">Spara det här kommandot som angetts som en PowerShell-skriptfil (*.ps1).</span><span class="sxs-lookup"><span data-stu-id="3b3da-173">Save this command set as a PowerShell script file (*.ps1).</span></span>
* <span data-ttu-id="3b3da-174">Spara det här kommandot som angetts som en Azure Automation-runbook i hello **Automation-konton** avsnitt i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3b3da-174">Save this command set as an Azure Automation runbook in hello **Automation Accounts** section of hello Azure portal.</span></span>

## <span data-ttu-id="3b3da-175"><a id="examples"></a>Exempel</span><span class="sxs-lookup"><span data-stu-id="3b3da-175"><a id="examples"></a>Examples</span></span>
<span data-ttu-id="3b3da-176">Här är två exempel på användning av hello stegen ovan toobuild Azure PowerShell-kommando anger som skapar Windows-baserade virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="3b3da-176">Here are two examples of using hello steps above toobuild Azure PowerShell command sets that create Windows-based Azure virtual machines.</span></span>

### <a name="example-1"></a><span data-ttu-id="3b3da-177">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="3b3da-177">Example 1</span></span>
<span data-ttu-id="3b3da-178">Jag behöver en PowerShell kommandouppsättning toocreate hello inledande virtuell dator för en Active Directory-domänkontrollant som:</span><span class="sxs-lookup"><span data-stu-id="3b3da-178">I need a PowerShell command set toocreate hello initial virtual machine for an Active Directory domain controller that:</span></span>

* <span data-ttu-id="3b3da-179">Använder hello Windows Server 2012 R2 Datacenter-avbildning.</span><span class="sxs-lookup"><span data-stu-id="3b3da-179">Uses hello Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="3b3da-180">Har hello namn AZDC1.</span><span class="sxs-lookup"><span data-stu-id="3b3da-180">Has hello name AZDC1.</span></span>
* <span data-ttu-id="3b3da-181">Är en fristående dator.</span><span class="sxs-lookup"><span data-stu-id="3b3da-181">Is a standalone computer.</span></span>
* <span data-ttu-id="3b3da-182">Har en ytterligare datadisk på 20 GB.</span><span class="sxs-lookup"><span data-stu-id="3b3da-182">Has an additional data disk of 20 GB.</span></span>
* <span data-ttu-id="3b3da-183">Har hello statisk IP-adress 192.168.244.4.</span><span class="sxs-lookup"><span data-stu-id="3b3da-183">Has hello static IP address 192.168.244.4.</span></span>
* <span data-ttu-id="3b3da-184">Är i hello BackEnd-undernät i hello AZDatacenter virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="3b3da-184">Is in hello BackEnd subnet of hello AZDatacenter virtual network.</span></span>
* <span data-ttu-id="3b3da-185">Är i hello Azure leksaksladan Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b3da-185">Is in hello Azure-TailspinToys cloud service.</span></span>

<span data-ttu-id="3b3da-186">Här är hello motsvarande Azure PowerShell-kommandot set toocreate den här virtuella datorn med tomma rader mellan varje block för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="3b3da-186">Here is hello corresponding Azure PowerShell command set toocreate this virtual machine, with blank lines between each block for readability.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="AZDC1"
    $vmsize="Medium"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
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

### <a name="example-2"></a><span data-ttu-id="3b3da-187">Exempel 2</span><span class="sxs-lookup"><span data-stu-id="3b3da-187">Example 2</span></span>
<span data-ttu-id="3b3da-188">Jag behöver en PowerShell kommandouppsättning toocreate en virtuell dator för en line-of-business-server som:</span><span class="sxs-lookup"><span data-stu-id="3b3da-188">I need a PowerShell command set toocreate a virtual machine for a line-of-business server that:</span></span>

* <span data-ttu-id="3b3da-189">Använder hello Windows Server 2012 R2 Datacenter-avbildning.</span><span class="sxs-lookup"><span data-stu-id="3b3da-189">Uses hello Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="3b3da-190">Har hello namn LOB1.</span><span class="sxs-lookup"><span data-stu-id="3b3da-190">Has hello name LOB1.</span></span>
* <span data-ttu-id="3b3da-191">Är medlem i domänen corp.contoso.com för hello.</span><span class="sxs-lookup"><span data-stu-id="3b3da-191">Is a member of hello corp.contoso.com domain.</span></span>
* <span data-ttu-id="3b3da-192">Har en ytterligare datadisk 200 GB.</span><span class="sxs-lookup"><span data-stu-id="3b3da-192">Has an additional data disk of 200 GB.</span></span>
* <span data-ttu-id="3b3da-193">Finns i hello klientdel undernät av hello AZDatacenter virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="3b3da-193">Is in hello FrontEnd subnet of hello AZDatacenter virtual network.</span></span>
* <span data-ttu-id="3b3da-194">Är i hello Azure leksaksladan Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="3b3da-194">Is in hello Azure-TailspinToys cloud service.</span></span>

<span data-ttu-id="3b3da-195">Här är hello motsvarande Azure PowerShell-kommandot set toocreate den här virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3b3da-195">Here is hello corresponding Azure PowerShell command set toocreate this virtual machine.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1
    $vmname="LOB1"
    $vmsize="Large"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $cred1=Get-Credential –Message "Type hello name and password of hello local administrator account."
    $cred2=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account that has permission tooadd hello machine toohello domain."
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


## <a name="next-steps"></a><span data-ttu-id="3b3da-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3b3da-196">Next steps</span></span>
<span data-ttu-id="3b3da-197">Om du behöver en OS-disk som är större än 127 GB, kan du [expanderar hello OS enhet](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3b3da-197">If you need an OS disk that is larger than 127 GB, you can [expand hello OS drive](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

