---
title: 'Azure Active Directory Domain Services: Administrationsguide | Microsoft Docs'
description: "Ansluta en Windows virtuella tooa hanterade domänen med hjälp av Azure PowerShell och hello klassiska distributionsmodellen."
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9143b843-7327-43c3-baab-6e24a18db25e
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 21bc5930d84c5368a120f9d81f09320e2a0168fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-windows-server-virtual-machine-tooa-managed-domain-using-powershell"></a><span data-ttu-id="ccf24-103">Ansluta till en Windows Server virtuella tooa hanterade domän med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="ccf24-103">Join a Windows Server virtual machine tooa managed domain using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ccf24-104">Klassiska Azure-portalen – Windows</span><span class="sxs-lookup"><span data-stu-id="ccf24-104">Azure classic portal - Windows</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
> * [<span data-ttu-id="ccf24-105">PowerShell - Windows</span><span class="sxs-lookup"><span data-stu-id="ccf24-105">PowerShell - Windows</span></span>](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> <span data-ttu-id="ccf24-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ccf24-106">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="ccf24-107">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="ccf24-107">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="ccf24-108">Azure AD Domain Services stöder för närvarande inte hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="ccf24-108">Azure AD Domain Services does not currently support hello Resource Manager model.</span></span>
>
>

<span data-ttu-id="ccf24-109">Dessa steg visar hur toocustomize en uppsättning Azure PowerShell-kommandon som skapa och förkonfigurera en Windows-baserad Azure virtuella med hjälp av en byggblock-metod.</span><span class="sxs-lookup"><span data-stu-id="ccf24-109">These steps show you how toocustomize a set of Azure PowerShell commands that create and preconfigure a Windows-based Azure virtual machine by using a building block approach.</span></span> <span data-ttu-id="ccf24-110">De här stegen kan du skapa en Windows-baserad Azure virtuella och ansluta den tooan Azure AD Domain Services-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="ccf24-110">These steps help you build a Windows-based Azure virtual machine and join it tooan Azure AD Domain Services managed domain.</span></span>

<span data-ttu-id="ccf24-111">Här följer en Fyll-i-tomrum metod för att skapa uppsättningar med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ccf24-111">These steps follow a fill-in-the-blanks approach for creating Azure PowerShell command sets.</span></span> <span data-ttu-id="ccf24-112">Den här metoden kan vara användbart om du är ny tooPowerShell eller tooknow toospecify vilka värden för lyckad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ccf24-112">This approach can be useful if you are new tooPowerShell or you want tooknow what values toospecify for successful configuration.</span></span> <span data-ttu-id="ccf24-113">Avancerade PowerShell-användare kan ta hello kommandon och ersätta sina egna värden för variabler som hello (hello rader som börjar med ”$”).</span><span class="sxs-lookup"><span data-stu-id="ccf24-113">Advanced PowerShell users can take hello commands and substitute their own values for hello variables (hello lines beginning with "$").</span></span>

<span data-ttu-id="ccf24-114">Om du inte redan gjort det, Använd hello-instruktioner i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) tooinstall Azure PowerShell på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="ccf24-114">If you haven't done so already, use hello instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) tooinstall Azure PowerShell on your local computer.</span></span> <span data-ttu-id="ccf24-115">Öppna en kommandotolk i Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ccf24-115">Then, open a Windows PowerShell command prompt.</span></span>

## <a name="step-1-add-your-account"></a><span data-ttu-id="ccf24-116">Steg 1: Lägg till ditt konto</span><span class="sxs-lookup"><span data-stu-id="ccf24-116">Step 1: Add your account</span></span>
1. <span data-ttu-id="ccf24-117">I hello PowerShell-Kommandotolken, Skriv **Add-AzureAccount** och på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="ccf24-117">At hello PowerShell prompt, type **Add-AzureAccount** and click **Enter**.</span></span>
2. <span data-ttu-id="ccf24-118">Ange hello e-postadress som är associerad med din Azure-prenumeration och klicka på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="ccf24-118">Type in hello email address associated with your Azure subscription and click **Continue**.</span></span>
3. <span data-ttu-id="ccf24-119">Ange hello lösenord för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="ccf24-119">Type in hello password for your account.</span></span>
4. <span data-ttu-id="ccf24-120">Klicka på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="ccf24-120">Click **Sign in**.</span></span>

## <a name="step-2-set-your-subscription-and-storage-account"></a><span data-ttu-id="ccf24-121">Steg 2: Ange din prenumeration och storage-konto</span><span class="sxs-lookup"><span data-stu-id="ccf24-121">Step 2: Set your subscription and storage account</span></span>
<span data-ttu-id="ccf24-122">Ange din Azure-prenumeration och storage-konto genom att köra dessa kommandon i Kommandotolken för hello Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ccf24-122">Set your Azure subscription and storage account by running these commands at hello Windows PowerShell command prompt.</span></span> <span data-ttu-id="ccf24-123">Ersätt allt inom hello citattecken, inklusive Hej < och > tecken, med hello rätt namn.</span><span class="sxs-lookup"><span data-stu-id="ccf24-123">Replace everything within hello quotes, including hello < and > characters, with hello correct names.</span></span>

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

<span data-ttu-id="ccf24-124">Du kan hämta hello rätt prenumerationsnamn från hello SubscriptionName-egenskapen för hello utdata från hello **Get-AzureSubscription** kommando.</span><span class="sxs-lookup"><span data-stu-id="ccf24-124">You can get hello correct subscription name from hello SubscriptionName property of hello output of hello **Get-AzureSubscription** command.</span></span> <span data-ttu-id="ccf24-125">Du kan hämta hello rätt lagringskontonamnet från hello etikettegenskap hello utdata från hello **Get-AzureStorageAccount** kommandot när du har kört hello **Välj AzureSubscription** kommando.</span><span class="sxs-lookup"><span data-stu-id="ccf24-125">You can get hello correct storage account name from hello Label property of hello output of hello **Get-AzureStorageAccount** command after you run hello **Select-AzureSubscription** command.</span></span>

## <a name="step-3-step-by-step-walkthrough---provision-hello-virtual-machine-and-join-it-toohello-managed-domain"></a><span data-ttu-id="ccf24-126">Steg 3: Steg för steg - etablera hello virtuell dator och ansluta den toohello hanterad domän</span><span class="sxs-lookup"><span data-stu-id="ccf24-126">Step 3: Step-by-step walkthrough - provision hello virtual machine and join it toohello managed domain</span></span>
<span data-ttu-id="ccf24-127">Här är hello motsvarande Azure PowerShell-kommandot set toocreate den här virtuella datorn med tomma rader mellan varje block för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="ccf24-127">Here is hello corresponding Azure PowerShell command set toocreate this virtual machine, with blank lines between each block for readability.</span></span>

<span data-ttu-id="ccf24-128">Ange information om hello Windows virtuella toobe etableras.</span><span class="sxs-lookup"><span data-stu-id="ccf24-128">Specify information about hello Windows virtual machine toobe provisioned.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

<span data-ttu-id="ccf24-129">Hej InstanceSize värden för D-, DS- eller G-serien virtuella datorer, se [virtuell dator och Molntjänststorlekar för Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="ccf24-129">For hello InstanceSize values for D-, DS-, or G-series virtual machines, see [Virtual Machine and Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span>

<span data-ttu-id="ccf24-130">Ange information om hello-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="ccf24-130">Provide information about hello managed domain.</span></span>

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

<span data-ttu-id="ccf24-131">Ange hello namn hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="ccf24-131">Specify hello name of hello cloud service.</span></span>

    $svcname="Contoso100-test"

<span data-ttu-id="ccf24-132">Ange hello namn hello virtuellt nätverk toowhich hello VM ska anslutas.</span><span class="sxs-lookup"><span data-stu-id="ccf24-132">Specify hello name of hello virtual network toowhich hello VM should be joined.</span></span> <span data-ttu-id="ccf24-133">Se till att hello AAD-DS hanterade domänen är tillgänglig i det här virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="ccf24-133">Ensure that hello AAD-DS managed domain is available in this virtual network.</span></span>

    $vnetname="MyPreviewVnet"

<span data-ttu-id="ccf24-134">Välj hello VM avbildningen toobe används tooprovision hello VM.</span><span class="sxs-lookup"><span data-stu-id="ccf24-134">Select hello VM image toobe used tooprovision hello VM.</span></span>

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

<span data-ttu-id="ccf24-135">Konfigurera hello VM - namnet på VM, instans storlek och avbildningen toobe används.</span><span class="sxs-lookup"><span data-stu-id="ccf24-135">Configure hello VM - set VM name, instance size & image toobe used.</span></span>

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

<span data-ttu-id="ccf24-136">Skaffa autentiseringsuppgifter för lokal administratör för hello VM.</span><span class="sxs-lookup"><span data-stu-id="ccf24-136">Obtain local administrator credentials for hello VM.</span></span> <span data-ttu-id="ccf24-137">Välj en stark lokala administratörslösenordet.</span><span class="sxs-lookup"><span data-stu-id="ccf24-137">Choose a strong local administrator password.</span></span>

    $localadmincred=Get-Credential –Message "Type hello name and password of hello local administrator account."

<span data-ttu-id="ccf24-138">Skaffa autentiseringsuppgifter för ett konto som tillhör too'AAD DC administratörer toojoin VM toohello hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="ccf24-138">Obtain credentials for a user account belonging too'AAD DC Administrators' group toojoin VM toohello managed domain.</span></span> <span data-ttu-id="ccf24-139">Ange inte hello domännamn – för instans i vårt exempel anger vi ”bob” hello användarnamn.</span><span class="sxs-lookup"><span data-stu-id="ccf24-139">Do not specify hello domain name - for instance, in our example, we specify 'bob' as hello user name.</span></span>

    $domainadmincred=Get-Credential –Message "Now type hello name (DO NOT INCLUDE hello DOMAIN) and password of an account in hello AAD DC Administrators group, that has permission tooadd hello machine toohello domain."

<span data-ttu-id="ccf24-140">Konfigurera hello VM - Ange domänen koppling krav & autentiseringsuppgifter som krävs.</span><span class="sxs-lookup"><span data-stu-id="ccf24-140">Configure hello VM - specify domain join requirement & required credentials.</span></span>

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

<span data-ttu-id="ccf24-141">Ange ett undernät för hello VM.</span><span class="sxs-lookup"><span data-stu-id="ccf24-141">Set a subnet for hello VM.</span></span>

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

<span data-ttu-id="ccf24-142">Valfritt: Punkt toohello IP-adress hello domän.</span><span class="sxs-lookup"><span data-stu-id="ccf24-142">Optional: Point toohello IP address of hello domain.</span></span> <span data-ttu-id="ccf24-143">Det här steget är inte nödvändigt om du ställer in hello IP-adresser för hello Azure AD Domain Services-hanterad domän toobe hello DNS-servrar för hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="ccf24-143">If you set hello IP addresses of hello Azure AD Domain Services managed domain toobe hello DNS servers for hello virtual network, this step is not required.</span></span>

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

<span data-ttu-id="ccf24-144">Nu kan domänanslutna etablera hello Windows VM.</span><span class="sxs-lookup"><span data-stu-id="ccf24-144">Now, provision hello domain-joined Windows VM.</span></span>

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-tooprovision-a-windows-vm-and-automatically-join-it-tooan-aad-domain-services-managed-domain"></a><span data-ttu-id="ccf24-145">Skript tooprovision en virtuell Windows-dator och ansluta automatiskt tooan AAD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="ccf24-145">Script tooprovision a Windows VM and automatically join it tooan AAD Domain Services managed domain</span></span>
<span data-ttu-id="ccf24-146">Den här uppsättningen för PowerShell-kommando skapar en virtuell dator för en line-of-business-server som:</span><span class="sxs-lookup"><span data-stu-id="ccf24-146">This PowerShell command set creates a virtual machine for a line-of-business server that:</span></span>

* <span data-ttu-id="ccf24-147">Använder hello Windows Server 2012 R2 Datacenter-avbildning.</span><span class="sxs-lookup"><span data-stu-id="ccf24-147">Uses hello Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="ccf24-148">Är en extra liten virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ccf24-148">Is an extra small virtual machine.</span></span>
* <span data-ttu-id="ccf24-149">Har hello namn Contoso100-test.</span><span class="sxs-lookup"><span data-stu-id="ccf24-149">Has hello name Contoso100-test.</span></span>
* <span data-ttu-id="ccf24-150">Är automatiskt domän kopplade toohello contoso100 hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="ccf24-150">Is automatically domain joined toohello contoso100 managed domain.</span></span>
* <span data-ttu-id="ccf24-151">Har lagts till toohello samma virtuella nätverk som hello hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="ccf24-151">Is added toohello same virtual network as hello managed domain.</span></span>

<span data-ttu-id="ccf24-152">Här är hello fullständigt exempelprogram skriptet toocreate hello Windows-dator och ansluta automatiskt toohello Azure AD Domain Services-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="ccf24-152">Here is hello full sample script toocreate hello Windows virtual machine and automatically join it toohello Azure AD Domain Services managed domain.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type hello name and password of hello local administrator account."

    $domainadmincred=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account in hello AAD DC Administrators group, that has permission tooadd hello machine toohello domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a><span data-ttu-id="ccf24-153">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="ccf24-153">Related Content</span></span>
* [<span data-ttu-id="ccf24-154">Azure AD Domain Services - komma igång-guide</span><span class="sxs-lookup"><span data-stu-id="ccf24-154">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="ccf24-155">Administrera en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="ccf24-155">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
