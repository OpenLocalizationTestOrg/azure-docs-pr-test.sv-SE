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
# <a name="create-a-windows-virtual-machine-with-powershell-and-hello-classic-deployment-model"></a>Skapa en virtuell Windows-dator med PowerShell och hello klassiska distributionsmodellen
> [!div class="op_single_selector"]
> * [Azure portal – Windows](tutorial.md)
> * [PowerShell - Windows](create-powershell.md)
> 
> 

<br>

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Lär dig hur för[utföra dessa steg med hello Resource Manager-modellen](../../virtual-machines-windows-ps-create.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Dessa steg visar hur toocustomize en uppsättning Azure PowerShell-kommandon som skapa och förkonfigurera en Windows-baserad Azure virtuella med hjälp av en byggblock-metod. Du kan använda den här processen tooquickly skapa en kommandouppsättning för en ny Windows-baserad virtuell dator och expandera en befintlig distribution eller toocreate flera anger kommandot som snabbt skapa en anpassad utveckling och testning eller IT-teknikern miljö.

Här följer en Fyll-i-tomrum metod för att skapa uppsättningar med Azure PowerShell. Den här metoden kan vara användbart om du är ny tooPowerShell eller du bara vill tooknow vilka värden toospecify för lyckad konfiguration. Avancerade PowerShell-användare kan ta hello kommandon och ersätta sina egna värden för variabler som hello (hello rader som börjar med ”$”).

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

## <a name="step-3-determine-hello-imagefamily"></a>Steg 3: Bestäm hello ImageFamily
Därefter behöver du toodetermine hello ImageFamily eller etikettvärde för hello viss bild motsvarande toohello virtuell Azure-dator du vill toocreate. Du kan hämta hello lista över tillgängliga ImageFamily värden med det här kommandot.

    Get-AzureVMImage | select ImageFamily -Unique

Här följer några exempel på ImageFamily värden för Windows-baserade datorer:

* Windows Server 2012 R2 Datacenter
* Windows Server 2008 R2 SP1
* Windows Server 2016 Technical Preview 4
* SQL Server 2012 SP1 Enterprise på Windows Server 2012

Om du hittar hello bilden som du letar efter, öppnar du en ny instans av hello textredigerare på ditt val eller hello PowerShell Integrated Scripting Environment (ISE). Kopiera följande hello till hello ny textfil eller hello PowerShell ISE ersätter hello ImageFamily värde.

    $family="<ImageFamily value>"
    $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

I vissa fall är hello avbildningens namn i hello etikettegenskapen i stället för hello ImageFamily värde. Om du inte kan hitta hello-avbildning som du söker efter med hello ImageFamily egenskapen lista hello avbildningar av egenskapen etikett med det här kommandot.

    Get-AzureVMImage | select Label -Unique

Om du hittar hello höger bild med det här kommandot kan du öppna en ny instans av hello textredigerare hello PowerShell ISE eller val. Kopiera följande hello till hello ny textfil eller hello PowerShell ISE ersätter hello etikettvärde.

    $label="<Label value>"
    $image = Get-AzureVMImage | where { $_.Label -eq $label } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

## <a name="step-4-build-your-command-set"></a>Steg 4: Skapa dina kommandot set
Skapa hello resten av kommandot genom att kopiera hello rätt antal block nedan till dina nya textfil eller hello ISE fylla i hello variabelvärden och tar bort Hej < och > tecken. Se hello två [exempel](#examples) hello slutet av den här artikeln för en uppfattning av hello slutresultatet.

Starta ditt kommando anges genom att välja ett av dessa två kommandon (krävs).

Alternativ 1: Ange ett namn för virtuell dator och en storlek.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

Alternativ 2: Ange ett namn, storlek och namn på tillgänglighetsuppsättning.

    $vmname="<machine name>"
    $vmsize="<Specify one: Small, Medium, Large, ExtraLarge, A5, A6, A7, A8, A9>"
    $availset="<set name>"
    $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image -AvailabilitySetName $availset

Hej InstanceSize värden för D-, DS- eller G-serien virtuella datorer, se [virtuell dator och Molntjänststorlekar för Azure](https://msdn.microsoft.com/library/azure/dn197896.aspx).

> [!NOTE]
> Om du har ett Enterprise-avtal med Software Assurance och avser tootake nytta av hello Windows Server [Hybrid Använd förmån](https://azure.microsoft.com/pricing/hybrid-use-benefit/), lägga till den **- LicenseType** parametern toohello  **Nya AzureVMConfig** cmdlet, skicka hello värdet **Windows_Server** hello vanliga användningsfall.  Kontrollera att du använder en bild som du har överfört; Du kan inte använda en standardiserad avbildning från hello galleriet med hello Hybrid använda förmånen.
> 
> 

Du kan också ange hello lokalt administratörskonto och lösenord för en fristående Windows-dator.

    $cred=Get-Credential -Message "Type hello name and password of hello local administrator account."
    $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.Username -Password $cred.GetNetworkCredential().Password

Välj ett starkt lösenord. toocheck dess styrkan finns [lösenord layout: med starka lösenord](https://www.microsoft.com/security/pc-security/password-checker.aspx).

Du kan också ange tooadd hello Windows datorn tooan befintliga Active Directory-domän, hello lokalt administratörskonto och lösenord, hello domän och hello namnet och lösenordet för ett domänkonto.

    $cred1=Get-Credential –Message "Type hello name and password of hello local administrator account."
    $cred2=Get-Credential –Message "Now type hello name (not including hello domain) and password of an account that has permission tooadd hello machine toohello domain."
    $domaindns="<FQDN of hello domain that hello machine is joining>"
    $domacctdomain="<domain of hello account that has permission tooadd hello machine toohello domain>"
    $vm1 | Add-AzureProvisioningConfig -AdminUsername $cred1.Username -Password $cred1.GetNetworkCredential().Password -WindowsDomain -Domain $domacctdomain -DomainUserName $cred2.Username -DomainPassword $cred2.GetNetworkCredential().Password -JoinDomain $domaindns

Ytterligare före konfigurationsalternativ för Windows-baserade virtuella datorer, finns hello syntax för hello **Windows** och **WindowsDomain** parameteruppsättningar [ Lägg till AzureProvisioningConfig](/powershell/module/azure/add-azureprovisioningconfig).

Du kan också tilldela hello virtuell dator en specifik IP-adress, som kallas en statisk DIP.

    $vm1 | Set-AzureStaticVNetIP -IPAddress <IP address>

Du kan kontrollera att en specifik IP-adress är tillgänglig med:

    Test-AzureStaticVNetIP –VNetName <VNet name> –IPAddress <IP address>

Du kan också tilldela hello virtuella tooa specifika undernät i Azure-nätverk.

    $vm1 | Set-AzureSubnet -SubnetNames "<name of hello subnet>"

Du kan också lägga till en enda disk toohello virtuell dator.

    $disksize=<size of hello disk in GB>
    $disklabel="<hello label on hello disk>"
    $lun=<Logical Unit Number (LUN) of hello disk>
    $hcaching="<Specify one: ReadOnly, ReadWrite, None>"
    $vm1 | Add-AzureDataDisk -CreateNew -DiskSizeInGB $disksize -DiskLabel $disklabel -LUN $lun -HostCaching $hcaching

För en Active Directory-domänkontrollanten, ange $hcaching för ”None”.

Du kan också lägga till hello virtuella tooan befintlig belastningsutjämnad uppsättning för externa trafiken.

    $protocol="<Specify one: tcp, udp>"
    $localport=<port number of hello internal port>
    $pubport=<port number of hello external port>
    $endpointname="<name of hello endpoint>"
    $lbsetname="<name of hello existing load-balanced set>"
    $probeprotocol="<Specify one: tcp, http>"
    $probeport=<TCP or HTTP port number of probe traffic>
    $probepath="<URL path for probe traffic>"
    $vm1 | Add-AzureEndpoint -Name $endpointname -Protocol $protocol -LocalPort $localport -PublicPort $pubport -LBSetName $lbsetname -ProbeProtocol $probeprotocol -ProbePort $probeport -ProbePath $probepath

Slutligen ska du välja en av dessa nödvändiga kommando för att skapa hello virtuella dator.

Alternativ 1: Skapa hello virtuell dator i en befintlig molntjänst.

    New-AzureVM –ServiceName "<short name of hello cloud service>" -VMs $vm1

hello är kort för hello Molntjänsten hello namnet som visas i hello listan med molntjänster i hello Azure-portalen eller hello listan över resursgrupper i hello Azure-portalen.

Alternativ 2: Skapa hello virtuell dator i en befintlig molntjänst och virtuella nätverk.

    $svcname="<short name of hello cloud service>"
    $vnetname="<name of hello virtual network>"
    New-AzureVM –ServiceName $svcname -VMs $vm1 -VNetName $vnetname

## <a name="step-5-run-your-command-set"></a>Steg 5: Kör din kommandot set
Granska hello Azure PowerShell-kommandot set du skapat i en textredigerare eller hello PowerShell ISE som består av flera block med kommandon från steg 4. Se till att du har angett alla hello behövs variabler och att de har rätt hello-värden. Kontrollera också att du har tagit bort alla Hej < och > tecken.

Om du använder en textredigerare, kopiera hello kommandot Ange toohello Urklipp och högerklicka sedan på dina öppna Windows PowerShell-Kommandotolken. Detta utfärda hello kommandouppsättning som en serie av PowerShell-kommandon och skapa din virtuella Azure-datorn. Alternativt kan köra hello-kommando i hello PowerShell ISE.

Om du skapar den här virtuella datorn igen eller en liknande, kan du:

* Spara det här kommandot som angetts som en PowerShell-skriptfil (*.ps1).
* Spara det här kommandot som angetts som en Azure Automation-runbook i hello **Automation-konton** avsnitt i hello Azure-portalen.

## <a id="examples"></a>Exempel
Här är två exempel på användning av hello stegen ovan toobuild Azure PowerShell-kommando anger som skapar Windows-baserade virtuella Azure-datorer.

### <a name="example-1"></a>Exempel 1
Jag behöver en PowerShell kommandouppsättning toocreate hello inledande virtuell dator för en Active Directory-domänkontrollant som:

* Använder hello Windows Server 2012 R2 Datacenter-avbildning.
* Har hello namn AZDC1.
* Är en fristående dator.
* Har en ytterligare datadisk på 20 GB.
* Har hello statisk IP-adress 192.168.244.4.
* Är i hello BackEnd-undernät i hello AZDatacenter virtuellt nätverk.
* Är i hello Azure leksaksladan Molntjänsten.

Här är hello motsvarande Azure PowerShell-kommandot set toocreate den här virtuella datorn med tomma rader mellan varje block för läsbarhet.

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

### <a name="example-2"></a>Exempel 2
Jag behöver en PowerShell kommandouppsättning toocreate en virtuell dator för en line-of-business-server som:

* Använder hello Windows Server 2012 R2 Datacenter-avbildning.
* Har hello namn LOB1.
* Är medlem i domänen corp.contoso.com för hello.
* Har en ytterligare datadisk 200 GB.
* Finns i hello klientdel undernät av hello AZDatacenter virtuella nätverk.
* Är i hello Azure leksaksladan Molntjänsten.

Här är hello motsvarande Azure PowerShell-kommandot set toocreate den här virtuella datorn.

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


## <a name="next-steps"></a>Nästa steg
Om du behöver en OS-disk som är större än 127 GB, kan du [expanderar hello OS enhet](../../virtual-machines-windows-expand-os-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

