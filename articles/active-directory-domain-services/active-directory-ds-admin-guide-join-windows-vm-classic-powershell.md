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
# <a name="join-a-windows-server-virtual-machine-tooa-managed-domain-using-powershell"></a>Ansluta till en Windows Server virtuella tooa hanterade domän med hjälp av PowerShell
> [!div class="op_single_selector"]
> * [Klassiska Azure-portalen – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell - Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Azure AD Domain Services stöder för närvarande inte hello Resource Manager-modellen.
>
>

Dessa steg visar hur toocustomize en uppsättning Azure PowerShell-kommandon som skapa och förkonfigurera en Windows-baserad Azure virtuella med hjälp av en byggblock-metod. De här stegen kan du skapa en Windows-baserad Azure virtuella och ansluta den tooan Azure AD Domain Services-hanterad domän.

Här följer en Fyll-i-tomrum metod för att skapa uppsättningar med Azure PowerShell. Den här metoden kan vara användbart om du är ny tooPowerShell eller tooknow toospecify vilka värden för lyckad konfiguration. Avancerade PowerShell-användare kan ta hello kommandon och ersätta sina egna värden för variabler som hello (hello rader som börjar med ”$”).

Om du inte redan gjort det, Använd hello-instruktioner i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) tooinstall Azure PowerShell på den lokala datorn. Öppna en kommandotolk i Windows PowerShell.

## <a name="step-1-add-your-account"></a>Steg 1: Lägg till ditt konto
1. I hello PowerShell-Kommandotolken, Skriv **Add-AzureAccount** och på **RETUR**.
2. Ange hello e-postadress som är associerad med din Azure-prenumeration och klicka på **Fortsätt**.
3. Ange hello lösenord för ditt konto.
4. Klicka på **logga in**.

## <a name="step-2-set-your-subscription-and-storage-account"></a>Steg 2: Ange din prenumeration och storage-konto
Ange din Azure-prenumeration och storage-konto genom att köra dessa kommandon i Kommandotolken för hello Windows PowerShell. Ersätt allt inom hello citattecken, inklusive Hej < och > tecken, med hello rätt namn.

    $subscr="<subscription name>"
    $staccount="<storage account name>"
    Select-AzureSubscription -SubscriptionName $subscr –Current
    Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

Du kan hämta hello rätt prenumerationsnamn från hello SubscriptionName-egenskapen för hello utdata från hello **Get-AzureSubscription** kommando. Du kan hämta hello rätt lagringskontonamnet från hello etikettegenskap hello utdata från hello **Get-AzureStorageAccount** kommandot när du har kört hello **Välj AzureSubscription** kommando.

## <a name="step-3-step-by-step-walkthrough---provision-hello-virtual-machine-and-join-it-toohello-managed-domain"></a>Steg 3: Steg för steg - etablera hello virtuell dator och ansluta den toohello hanterad domän
Här är hello motsvarande Azure PowerShell-kommandot set toocreate den här virtuella datorn med tomma rader mellan varje block för läsbarhet.

Ange information om hello Windows virtuella toobe etableras.

    $family="Windows Server 2012 R2 Datacenter"
    $vmname="Contoso100-test"
    $vmsize="ExtraSmall"

Hej InstanceSize värden för D-, DS- eller G-serien virtuella datorer, se [virtuell dator och Molntjänststorlekar för Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

Ange information om hello-hanterad domän.

    $domaindns="contoso100.com"
    $domacctdomain="contoso100"

Ange hello namn hello-Molntjänsten.

    $svcname="Contoso100-test"

Ange hello namn hello virtuellt nätverk toowhich hello VM ska anslutas. Se till att hello AAD-DS hanterade domänen är tillgänglig i det här virtuella nätverket.

    $vnetname="MyPreviewVnet"

Välj hello VM avbildningen toobe används tooprovision hello VM.

    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

Konfigurera hello VM - namnet på VM, instans storlek och avbildningen toobe används.

    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Skaffa autentiseringsuppgifter för lokal administratör för hello VM. Välj en stark lokala administratörslösenordet.

    $localadmincred=Get-Credential –Message "Type hello name and password of hello local administrator account."

Skaffa autentiseringsuppgifter för ett konto som tillhör too'AAD DC administratörer toojoin VM toohello hanterade domän. Ange inte hello domännamn – för instans i vårt exempel anger vi ”bob” hello användarnamn.

    $domainadmincred=Get-Credential –Message "Now type hello name (DO NOT INCLUDE hello DOMAIN) and password of an account in hello AAD DC Administrators group, that has permission tooadd hello machine toohello domain."

Konfigurera hello VM - Ange domänen koppling krav & autentiseringsuppgifter som krävs.

    $vm1 | Add-AzureProvisioningConfig -AdminUsername $localadmincred.Username -Password $localadmincred.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $domainadmincred.Username -DomainPassword $domainadmincred.GetNetworkCredential().Password -JoinDomain $domaindns

Ange ett undernät för hello VM.

    $vm1 | Set-AzureSubnet -SubnetNames "Subnet-1"

Valfritt: Punkt toohello IP-adress hello domän. Det här steget är inte nödvändigt om du ställer in hello IP-adresser för hello Azure AD Domain Services-hanterad domän toobe hello DNS-servrar för hello virtuellt nätverk.

    $dns = New-AzureDns -Name 'contoso100-dc1' -IPAddress '10.0.0.4'

Nu kan domänanslutna etablera hello Windows VM.

    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname -Location "Central US" -DnsSettings $dns

<br>

## <a name="script-tooprovision-a-windows-vm-and-automatically-join-it-tooan-aad-domain-services-managed-domain"></a>Skript tooprovision en virtuell Windows-dator och ansluta automatiskt tooan AAD Domain Services-hanterad domän
Den här uppsättningen för PowerShell-kommando skapar en virtuell dator för en line-of-business-server som:

* Använder hello Windows Server 2012 R2 Datacenter-avbildning.
* Är en extra liten virtuell dator.
* Har hello namn Contoso100-test.
* Är automatiskt domän kopplade toohello contoso100 hanterade domän.
* Har lagts till toohello samma virtuella nätverk som hello hanterade domän.

Här är hello fullständigt exempelprogram skriptet toocreate hello Windows-dator och ansluta automatiskt toohello Azure AD Domain Services-hanterad domän.

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

## <a name="related-content"></a>Relaterat innehåll
* [Azure AD Domain Services - komma igång-guide](active-directory-ds-getting-started.md)
* [Administrera en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-domain.md)
