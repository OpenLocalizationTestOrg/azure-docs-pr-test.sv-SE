---
title: aaaMigrate tooResource Manager med PowerShell | Microsoft Docs
description: "Den här artikeln innehåller stegvisa hello stöds av plattformen migrering av IaaS-resurser som virtuella datorer (VM), virtuella nätverk (Vnet) och storage-konton från klassiska tooAzure Resource Manager (ARM) med hjälp av Azure PowerShell-kommandon"
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2b3dff9b-2e99-4556-acc5-d75ef234af9c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: b5b2987da292f1c241be71a354b0c2e1a96a07c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-iaas-resources-from-classic-tooazure-resource-manager-by-using-azure-powershell"></a>Migrera IaaS-resurser från klassiska tooAzure Resource Manager med hjälp av Azure PowerShell
Dessa steg visar hur toouse Azure PowerShell-kommandon toomigrate infrastruktur som en tjänst (IaaS)-resurser från hello klassisk distribution modellen toohello Azure Resource Manager-distributionsmodellen.

Om du vill kan du migrera resurser med hjälp av hello [Azure-kommandoradsgränssnittet (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).

* Bakgrundsinformation om migreringsscenarier som stöds finns i [plattform som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-overview.md).
* Detaljerad information och en genomgång för migrering finns i [tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md).
* [Granska de vanligaste migreringsfelen](migration-classic-resource-manager-errors.md)

<br>
Här är en flödesschema tooidentify hello ordning där steg måste toobe körs under en migreringsprocessen

![Skärmbild som visar hello migreringssteg](media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-plan-for-migration"></a>Steg 1: Planera för migrering
Här följer några metodtips som vi rekommenderar medan du utvärderar migrera IaaS-resurser från klassiska tooResource Manager:

* Läs igenom hello [stöds inte funktioner och konfigurationer som stöds och](migration-classic-resource-manager-overview.md). Om du har virtuella datorer som använder icke-stödda konfigurationer eller funktioner, rekommenderar vi att du väntar hello configuration-funktionen stöd toobe meddelats. Du kan också den passar dina behov, ta bort funktionen eller flytta från den tooenable konfigurationsmigreringen.
* Om du har automatiserat skript som distribuerar din infrastruktur och dina program idag, försök toocreate en liknande inställningar med hjälp av dessa skript för migrering. Du kan också ställa in exempel miljöer med hjälp av hello Azure-portalen.

> [!IMPORTANT]
> Programgatewayer stöds inte för migrering från klassiska tooResource Manager. toomigrate ett klassiskt virtuellt nätverk med en Programgateway ta bort hello gateway innan du kör ett förbereda åtgärden toomove hello nätverk. När du har slutfört migreringen hello återansluta hello gateway i Azure Resource Manager.
>
>ExpressRoute-gatewayer som ansluter tooExpressRoute kretsar i en annan prenumeration som inte kan migreras automatiskt. I sådana fall kan ta bort hello ExpressRoute-gateway, migrera hello virtuella nätverk och återskapa hello gateway. Se [migrera ExpressRoute-kretsar och associerade virtuella nätverk från hello klassiska toohello Resource Manager-distributionsmodellen](../../expressroute/expressroute-migration-classic-resource-manager.md) för mer information.
>
>

## <a name="step-2-install-hello-latest-version-of-azure-powershell"></a>Steg 2: Installera hello senaste versionen av Azure PowerShell
Det finns två huvudsakliga alternativ tooinstall Azure PowerShell: [PowerShell-galleriet](https://www.powershellgallery.com/profiles/azure-sdk/) eller [installationsprogram för webbplattform (WebPI)](http://aka.ms/webpi-azps). WebPI tar emot uppdateringar varje månad. PowerShell-galleriet tar emot uppdateringar kontinuerligt. Den här artikeln är baserad på Azure PowerShell version 2.1.0.

Installationsanvisningar finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-hello-subscription-in-azure-portal"></a>Steg 3: Kontrollera att du är administratör för hello prenumeration i Azure-portalen
tooperform migreringen, du måste läggas till som medadministratör för prenumerationen hello i hello [Azure-portalen](https://portal.azure.com).

1. Logga in på hello [Azure-portalen](https://portal.azure.com).
2. På navmenyn hello väljer **prenumeration**. Om du inte ser det, väljer **fler tjänster**.
3. Hitta hello lämpliga prenumerationspost titta sedan på hello **Mina ROLLEN** fältet. För en medadministratör hello-värdet bör vara _kontoadministratören_.

Om du inte kan tooadd en medadministratör Kontakta sedan en tjänstadministratör eller en medadministratör för hello prenumeration tooget själv lagts till.   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a>Steg 4: Ange din prenumeration och registrera dig för migrering
Först, starta PowerShell-Kommandotolken. För migrering, behöver du tooset upp din miljö för både klassiska och Resource Manager.

Logga in tooyour konto för hello Resource Manager-modellen.

```powershell
    Login-AzureRmAccount
```

Hämta hello tillgängliga prenumerationer med hjälp av hello följande kommando:

```powershell
    Get-AzureRMSubscription | Sort Name | Select Name
```

Ange din Azure-prenumeration för hello aktuell session. Det här exemplet anger hello prenumeration för standardnamnet för**min Azure-prenumeration**. Ersätt hello exempel prenumerationsnamn med din egen.

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> Registreringen är en enstaka steg, men du måste göra detta en gång innan du försöker migrera. Utan registrering, kan du se hello följande felmeddelande:
>
> *BadRequest: Prenumerationen har inte registrerats för migrering.*
>
>

Registrera med resursprovidern för hello migrering med hjälp av hello följande kommando:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Vänta 5 minuter för hello registrering toofinish. Du kan kontrollera hello status för hello godkännandet med hjälp av hello följande kommando:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Se till att RegistrationState `Registered` innan du fortsätter.

Nu logga in tooyour konto för hello klassiska modellen.

```powershell
    Add-AzureAccount
```

Hämta hello tillgängliga prenumerationer med hjälp av hello följande kommando:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Ange din Azure-prenumeration för hello aktuell session. Det här exemplet anger hello standardabonnemang för**min Azure-prenumeration**. Ersätt hello exempel prenumerationsnamn med din egen.

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-cores-in-hello-azure-region-of-your-current-deployment-or-vnet"></a>Steg 5: Kontrollera att du har tillräckligt med Azure Resource Manager-dator kärnor i hello Azure-regionen för din aktuella distributionen eller virtuella nätverk
Du kan använda hello följande PowerShell-kommandot toocheck hello aktuellt antal kärnor som du har i Azure Resource Manager. toolearn mer om grundläggande kvoter finns [gränser och hello Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).

Det här exemplet kontrollerar hello tillgänglighet i hello **västra USA** region. Ersätt hello exempel regionnamn med din egen.

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-6-run-commands-toomigrate-your-iaas-resources"></a>Steg 6: Kör kommandon toomigrate IaaS-resurser
> [!NOTE]
> Alla hello-åtgärder som beskrivs här är idempotent. Om du har problem med än en funktion som inte stöds eller ett konfigurationsfel rekommenderar vi att du gör hello förbereda, avbrytas eller genomföras igen. hello plattform försök görs hello åtgärden igen.
>
>

## <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Steg 6.1: Alternativ 1 – migrera virtuella datorer i en tjänst i molnet (inte i ett virtuellt nätverk)
Hämta hello lista över molntjänster med hjälp av följande kommando hello och välj hello molnbaserad tjänst som du vill toomigrate. Om hello virtuella datorer i hello Molntjänsten i ett virtuellt nätverk eller om de har webb-eller arbetsroller returnerar hello kommando ett felmeddelande.

```powershell
    Get-AzureService | ft Servicename
```

Hämta hello distributionens namn för hello-Molntjänsten. I det här exemplet hello tjänstnamnet är **Mina tjänsten**. Ersätt hello exempel tjänstnamn med egna namn.

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Förbered hello virtuella datorer i hello Molntjänsten för migrering. Du har två alternativ toochoose från.

* **Alternativ 1. Migrera hello VMs tooa plattform skapa virtuellt nätverk**

    Verifiera först om du kan migrera hello Molntjänsten med hello följande kommandon:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    hello Visar föregående kommando alla varningar och fel som blockerar migrering. Om verifieringen lyckas, så du kan flytta på toohello **Förbered** steg:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* **Alternativ 2. Migrera tooan befintligt virtuellt nätverk i hello Resource Manager-distributionsmodellen**

    Det här exemplet anger hello resursgruppens namn för**myResourceGroup**, hello virtuella nätverksnamnet för**myVirtualNetwork** och hello undernätsnamn för**mySubNet**. Ersätt hello namn i hello exemplet med hello namnen på de egna resurser.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Verifiera först om du kan migrera hello virtuellt nätverk med hello följande kommando:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    hello Visar föregående kommando alla varningar och fel som blockerar migrering. Om verifieringen lyckas, kan du fortsätta med hello följande förberedelsesteget:

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

När hello Förberedelseåtgärden lyckas med något av föregående alternativ hello, fråga hello migrering tillstånd hello virtuella datorer. Se till att de är i hello `Prepared` tillstånd.

Det här exemplet anger hello namn på virtuell dator för**myVM**. Ersätt hello Exempelnamn med ditt eget namn på virtuell dator.

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

Kontrollera hello-konfigurationen för hello förberedd resurser med hjälp av PowerShell eller hello Azure-portalen. Om du inte är klar för migrering och du vill toogo tillbaka toohello gamla tillstånd, använder du hello följande kommando:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Om hello förberedda konfigurationen ser bra ut du gå vidare och genomför hello resurser med hjälp av hello följande kommando:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

## <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a>Steg 6.1: Alternativ 2 – migrera virtuella datorer i ett virtuellt nätverk

toomigrate virtuella datorer i ett virtuellt nätverk som du migrerar hello virtuellt nätverk. att migrera automatiskt hello virtuella datorer med hello virtuellt nätverk. Välj hello virtuella nätverk som du vill toomigrate.
> [!NOTE]
> [Migrera klassiska virtuell dator](migrate-single-classic-to-resource-manager.md) genom att skapa en ny Resource Manager virtuell dator med hanterade diskar med hjälp av hello VHD-(OS och data) filer för hello virtuell dator.
<br>

> [!NOTE]
> hello virtuella nätverksnamnet kan skilja sig från vad som anges i hello nya portalen. hello nya Azure-portalen visar hello namn som `[vnet-name]` men hello faktiska virtuella nätverksnamnet är av typen `Group [resource-group-name] [vnet-name]`. Innan du migrerar sökning hello faktiska virtuella nätverksnamnet hello kommandot `Get-AzureVnetSite | Select -Property Name` eller hello gamla Azure-portalen. 

Det här exemplet anger hello virtuella nätverksnamnet för**myVnet**. Ersätt hello Exempelnamn för virtuellt nätverk med din egen.

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> Om hello virtuellt nätverk innehåller webb- eller arbetsroller eller virtuella datorer med konfigurationer som inte stöds, kan du få ett felmeddelande för verifiering.
>
>

Verifiera först om du kan migrera hello virtuellt nätverk med hjälp av hello följande kommando:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

hello Visar föregående kommando alla varningar och fel som blockerar migrering. Om verifieringen lyckas, kan du fortsätta med hello följande förberedelsesteget:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Kontrollera hello-konfigurationen för hello förbereda virtuella datorer med hjälp av Azure PowerShell eller hello Azure-portalen. Om du inte är klar för migrering och du vill toogo tillbaka toohello gamla tillstånd, använder du hello följande kommando:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Om hello förberedda konfigurationen ser bra ut du gå vidare och genomför hello resurser med hjälp av hello följande kommando:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

## <a name="step-62-migrate-a-storage-account"></a>Steg 6.2 Migrera ett lagringskonto
När du är klar migrera hello virtuella datorer, rekommenderar vi att du migrerar hello storage-konton.

Utför föregående nödvändiga kontroller innan du migrerar hello storage-konto:

* **Migrera den klassiska virtuella datorer vars diskar lagras i hello storage-konto**

    Föregående kommando returnerar RoleName och DiskName egenskaper för alla hello klassiska Virtuella diskar i hello storage-konto. RoleName är hello namn hello virtuella toowhich en disk som är ansluten. Om föregående kommando returnerar diskar Se till att virtuella datorer toowhich diskarna är anslutna migreras innan du migrerar hello storage-konto.
    ```powershell
     $storageAccountName = 'yourStorageAccountName'
      Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
      DiskName | Format-List -Property RoleName, DiskName

    ```
* **Ta bort beräkningsfunktionen klassiska Virtuella diskar som lagras i hello storage-konto**

    Hitta beräkningsfunktionen klassiska Virtuella diskar i hello storage-konto med hjälp av följande kommando:

    ```powershell
        $storageAccountName = 'yourStorageAccountName'
        Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

    ```
    Om ovanstående kommando returnerar diskar sedan ta bort dessa diskar med hjälp av följande kommando:

    ```powershell
       Remove-AzureDisk -DiskName 'yourDiskName'
    ```
* **Ta bort VM-avbildningar lagras i hello storage-konto**

    Föregående kommando returnerar alla hello VM-avbildningar med OS-disken som lagras i hello storage-konto.
     ```powershell
        Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                } | Select-Object -Property ImageName, ImageLabel
     ```
     Föregående kommando returnerar alla hello VM-avbildningar med datadiskar som lagras i hello storage-konto.
     ```powershell

        Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                         -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                        } | Select-Object -Property ImageName, ImageLabel
     ```
    Ta bort alla hello VM-avbildningar som returneras av ovan kommandon med hjälp av föregående kommando:
    ```powershell
    Remove-AzureVMImage -ImageName 'yourImageName'
    ```

Verifiera varje lagringskonto för migrering med hjälp av följande kommando hello. I det här exemplet hello lagringskontonamnet är **Mittlagringskonto**. Ersätt hello Exempelnamn med hello namnet på ditt eget lagringskonto.

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
```

Nästa steg är tooPrepare hello lagringskonto för migrering

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Kontrollera hello-konfigurationen för hello förberedd storage-konto med hjälp av Azure PowerShell eller hello Azure-portalen. Om du inte är klar för migrering och du vill toogo tillbaka toohello gamla tillstånd, använder du hello följande kommando:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Om hello förberedda konfigurationen ser bra ut du gå vidare och genomför hello resurser med hjälp av hello följande kommando:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Nästa steg
* [Översikt över plattformar som stöds migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Tekniska ingående om plattformen stöds från klassiska tooAzure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Planera för migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Använd CLI toomigrate IaaS-resurser från klassiska tooAzure Resource Manager](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Community-verktyg för att hjälpa till med migrering av IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Granska de vanligaste migreringsfelen](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Granska hello vanliga mest frågor om migrera IaaS-resurser från klassiska tooAzure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
