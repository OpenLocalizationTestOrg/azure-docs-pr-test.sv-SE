---
title: 'Azure Active Directory Domain Services: Administrationsguide | Microsoft Docs'
description: "Anslut en virtuell Windows-dator till en hanterad domän med hjälp av Azure PowerShell och den klassiska distributionsmodellen."
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
ms.openlocfilehash: 1bb1546fb616131a1e1868a0d0610c4cad5d73e2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain-using-powershell"></a><span data-ttu-id="96be8-103">Anslut en virtuell dator med Windows Server till en hanterad domän med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="96be8-103">Join a Windows Server virtual machine to a managed domain using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="96be8-104">Klassiska Azure-portalen – Windows</span><span class="sxs-lookup"><span data-stu-id="96be8-104">Azure classic portal - Windows</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
> * [<span data-ttu-id="96be8-105">PowerShell - Windows</span><span class="sxs-lookup"><span data-stu-id="96be8-105">PowerShell - Windows</span></span>](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> <span data-ttu-id="96be8-106">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="96be8-106">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="96be8-107">Den här artikeln beskriver den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="96be8-107">This article covers using the classic deployment model.</span></span> <span data-ttu-id="96be8-108">Azure AD Domain Services stöder för närvarande inte Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="96be8-108">Azure AD Domain Services does not currently support the Resource Manager model.</span></span>
>
>

<span data-ttu-id="96be8-109">Dessa steg visar hur du anpassar en uppsättning Azure PowerShell-kommandon som kan skapa och förkonfigurera en Windows-baserad Azure virtuella med hjälp av en byggblock-metod.</span><span class="sxs-lookup"><span data-stu-id="96be8-109">These steps show you how to customize a set of Azure PowerShell commands that create and preconfigure a Windows-based Azure virtual machine by using a building block approach.</span></span> <span data-ttu-id="96be8-110">De här stegen kan du skapa en Windows-baserad Azure virtuella och ansluta den till en Azure AD Domain Services-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="96be8-110">These steps help you build a Windows-based Azure virtual machine and join it to an Azure AD Domain Services managed domain.</span></span>

<span data-ttu-id="96be8-111">Här följer en Fyll-i-tomrum metod för att skapa uppsättningar med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="96be8-111">These steps follow a fill-in-the-blanks approach for creating Azure PowerShell command sets.</span></span> <span data-ttu-id="96be8-112">Den här metoden kan vara användbart om du är nybörjare på PowerShell eller om du vill veta vilka värden som du vill ange för lyckad konfiguration.</span><span class="sxs-lookup"><span data-stu-id="96be8-112">This approach can be useful if you are new to PowerShell or you want to know what values to specify for successful configuration.</span></span> <span data-ttu-id="96be8-113">Avancerade PowerShell-användare kan ta kommandon och ersätta deras egna värden för variabler (rader som börjar med ”$”).</span><span class="sxs-lookup"><span data-stu-id="96be8-113">Advanced PowerShell users can take the commands and substitute their own values for the variables (the lines beginning with "$").</span></span>

<span data-ttu-id="96be8-114">Om du inte redan har gjort så, följ instruktionerna i [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview) att installera Azure PowerShell på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="96be8-114">If you haven't done so already, use the instructions in [How to install and configure Azure PowerShell](/powershell/azure/overview) to install Azure PowerShell on your local computer.</span></span> <span data-ttu-id="96be8-115">Öppna en kommandotolk i Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="96be8-115">Then, open a Windows PowerShell command prompt.</span></span>

## <a name="step-1-add-your-account"></a><span data-ttu-id="96be8-116">Steg 1: Lägg till ditt konto</span><span class="sxs-lookup"><span data-stu-id="96be8-116">Step 1: Add your account</span></span>
1. <span data-ttu-id="96be8-117">I PowerShell-Kommandotolken skriver **Add-AzureAccount** och på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="96be8-117">At the PowerShell prompt, type **Add-AzureAccount** and click **Enter**.</span></span>
2. <span data-ttu-id="96be8-118">Skriv e-postadressen som är associerade med din Azure-prenumeration och klicka på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="96be8-118">Type in the email address associated with your Azure subscription and click **Continue**.</span></span>
3. <span data-ttu-id="96be8-119">Skriv in lösenordet för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="96be8-119">Type in the password for your account.</span></span>
4. <span data-ttu-id="96be8-120">Klicka på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="96be8-120">Click **Sign in**.</span></span>

## <a name="step-2-set-your-subscription-and-storage-account"></a><span data-ttu-id="96be8-121">Steg 2: Ange din prenumeration och storage-konto</span><span class="sxs-lookup"><span data-stu-id="96be8-121">Step 2: Set your subscription and storage account</span></span>
<span data-ttu-id="96be8-122">Ange din Azure-prenumeration och storage-konto genom att köra dessa kommandon i Kommandotolken för Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="96be8-122">Set your Azure subscription and storage account by running these commands at the Windows PowerShell command prompt.</span></span> <span data-ttu-id="96be8-123">Ersätt allt inom citattecken, inklusive den < och > tecken, med rätt namn.</span><span class="sxs-lookup"><span data-stu-id="96be8-123">Replace everything within the quotes, including the < and > characters, with the correct names.</span></span>

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

<span data-ttu-id="96be8-124">Du kan hämta rätt prenumerationsnamn från egenskapen SubscriptionName för utdata från den **Get-AzureSubscription** kommando.</span><span class="sxs-lookup"><span data-stu-id="96be8-124">You can get the correct subscription name from the SubscriptionName property of the output of the **Get-AzureSubscription** command.</span></span> <span data-ttu-id="96be8-125">Du kan hämta rätt lagringskontonamnet från egenskapen Label för utdata från den **Get-AzureStorageAccount** kommando när du kör den **Välj AzureSubscription** kommando.</span><span class="sxs-lookup"><span data-stu-id="96be8-125">You can get the correct storage account name from the Label property of the output of the **Get-AzureStorageAccount** command after you run the **Select-AzureSubscription** command.</span></span>

## <a name="step-3-step-by-step-walkthrough---provision-the-virtual-machine-and-join-it-to-the-managed-domain"></a><span data-ttu-id="96be8-126">Steg 3: Steg för steg - etablera den virtuella datorn och ansluta den till den hanterade domänen</span><span class="sxs-lookup"><span data-stu-id="96be8-126">Step 3: Step-by-step walkthrough - provision the virtual machine and join it to the managed domain</span></span>
<span data-ttu-id="96be8-127">Det här är den motsvarande Azure PowerShell-kommando som vill skapa den virtuella datorn med tomma rader mellan varje block för läsbarhet.</span><span class="sxs-lookup"><span data-stu-id="96be8-127">Here is the corresponding Azure PowerShell command set to create this virtual machine, with blank lines between each block for readability.</span></span>

<span data-ttu-id="96be8-128">Ange information om Windows-dator som ska etableras.</span><span class="sxs-lookup"><span data-stu-id="96be8-128">Specify information about the Windows virtual machine to be provisioned.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

<span data-ttu-id="96be8-129">InstanceSize-värden för D-, DS- eller G-serien virtuella datorer finns i [virtuell dator och Molntjänststorlekar för Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span><span class="sxs-lookup"><span data-stu-id="96be8-129">For the InstanceSize values for D-, DS-, or G-series virtual machines, see [Virtual Machine and Cloud Service Sizes for Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).</span></span>

<span data-ttu-id="96be8-130">Ange information om den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="96be8-130">Provide information about the managed domain.</span></span>

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

<span data-ttu-id="96be8-131">Ange namnet på Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="96be8-131">Specify the name of the cloud service.</span></span>

    $svcname="Contoso100-test"

<span data-ttu-id="96be8-132">Ange namnet på det virtuella nätverket som den virtuella datorn ska anslutas.</span><span class="sxs-lookup"><span data-stu-id="96be8-132">Specify the name of the virtual network to which the VM should be joined.</span></span> <span data-ttu-id="96be8-133">Kontrollera att den hanterade AAD-DS-domänen är tillgänglig i det här virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="96be8-133">Ensure that the AAD-DS managed domain is available in this virtual network.</span></span>

    $vnetname="MyPreviewVnet"

<span data-ttu-id="96be8-134">Välj VM-avbildning som används för att etablera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="96be8-134">Select the VM image to be used to provision the VM.</span></span>

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

<span data-ttu-id="96be8-135">Konfigurera VM - namn på VM, instansstorleken & bilden som ska användas.</span><span class="sxs-lookup"><span data-stu-id="96be8-135">Configure the VM - set VM name, instance size & image to be used.</span></span>

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

<span data-ttu-id="96be8-136">Hämta lokal administratörsbehörighet för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="96be8-136">Obtain local administrator credentials for the VM.</span></span> <span data-ttu-id="96be8-137">Välj en stark lokala administratörslösenordet.</span><span class="sxs-lookup"><span data-stu-id="96be8-137">Choose a strong local administrator password.</span></span>

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

<span data-ttu-id="96be8-138">Skaffa autentiseringsuppgifter för ett användarkonto som hör till ' AAD DC-administratörsgruppen för att koppla en VM till den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="96be8-138">Obtain credentials for a user account belonging to 'AAD DC Administrators' group to join VM to the managed domain.</span></span> <span data-ttu-id="96be8-139">Ange inte namnet på en domän - exempelvis i vårt exempel kan vi ange ”bob” som användarnamn.</span><span class="sxs-lookup"><span data-stu-id="96be8-139">Do not specify the domain name - for instance, in our example, we specify 'bob' as the user name.</span></span>

    $domainadmincred=Get-Credential –Message "Now type the name (DO NOT INCLUDE THE DOMAIN) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

<span data-ttu-id="96be8-140">Konfigurera den virtuella datorn – Ange domänen koppling krav & autentiseringsuppgifter som krävs.</span><span class="sxs-lookup"><span data-stu-id="96be8-140">Configure the VM - specify domain join requirement & required credentials.</span></span>

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

<span data-ttu-id="96be8-141">Ange ett undernät för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="96be8-141">Set a subnet for the VM.</span></span>

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

<span data-ttu-id="96be8-142">Valfritt: Peka på IP-adressen för domänen.</span><span class="sxs-lookup"><span data-stu-id="96be8-142">Optional: Point to the IP address of the domain.</span></span> <span data-ttu-id="96be8-143">Det här steget är inte nödvändigt om du ställer in IP-adresserna för Azure AD Domain Services hanterade domänen för att DNS-servrarna för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="96be8-143">If you set the IP addresses of the Azure AD Domain Services managed domain to be the DNS servers for the virtual network, this step is not required.</span></span>

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

<span data-ttu-id="96be8-144">Nu kan etablera domänanslutna Windows VM.</span><span class="sxs-lookup"><span data-stu-id="96be8-144">Now, provision the domain-joined Windows VM.</span></span>

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-to-provision-a-windows-vm-and-automatically-join-it-to-an-aad-domain-services-managed-domain"></a><span data-ttu-id="96be8-145">Skript för att etablera en virtuell Windows-dator och automatiskt ansluta den till en AAD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="96be8-145">Script to provision a Windows VM and automatically join it to an AAD Domain Services managed domain</span></span>
<span data-ttu-id="96be8-146">Den här uppsättningen för PowerShell-kommando skapar en virtuell dator för en line-of-business-server som:</span><span class="sxs-lookup"><span data-stu-id="96be8-146">This PowerShell command set creates a virtual machine for a line-of-business server that:</span></span>

* <span data-ttu-id="96be8-147">Använder Windows Server 2012 R2 Datacenter-avbildning.</span><span class="sxs-lookup"><span data-stu-id="96be8-147">Uses the Windows Server 2012 R2 Datacenter image.</span></span>
* <span data-ttu-id="96be8-148">Är en extra liten virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="96be8-148">Is an extra small virtual machine.</span></span>
* <span data-ttu-id="96be8-149">Har namnet Contoso100-test.</span><span class="sxs-lookup"><span data-stu-id="96be8-149">Has the name Contoso100-test.</span></span>
* <span data-ttu-id="96be8-150">Automatiskt är domänanslutna till contoso100 hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="96be8-150">Is automatically domain joined to the contoso100 managed domain.</span></span>
* <span data-ttu-id="96be8-151">Har lagts till i samma virtuella nätverk som den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="96be8-151">Is added to the same virtual network as the managed domain.</span></span>

<span data-ttu-id="96be8-152">Det här är det fullständiga exempelskriptet för att skapa Windows-dator och ansluta den automatiskt till den hanterade domänen med Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="96be8-152">Here is the full sample script to create the Windows virtual machine and automatically join it to the Azure AD Domain Services managed domain.</span></span>

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

    $svcname="Contoso100-test"
    $vnetname="MyPreviewVnet"

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

    $localadmincred=Get-Credential –Message "Type the name and password of the local administrator account."

    $domainadmincred=Get-Credential –Message "Now type the name (not including the domain) and password of an account in the AAD DC Administrators group, that has permission to add the machine to the domain."

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="related-content"></a><span data-ttu-id="96be8-153">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="96be8-153">Related Content</span></span>
* [<span data-ttu-id="96be8-154">Azure AD Domain Services - komma igång-guide</span><span class="sxs-lookup"><span data-stu-id="96be8-154">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="96be8-155">Administrera en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="96be8-155">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
