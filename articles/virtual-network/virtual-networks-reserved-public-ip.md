---
title: aaaManage Azure reserverad IP-adress (klassisk) - PowerShell | Microsoft Docs
description: "Förstå reserverade IP-adresser (klassisk) och hur toomanage dem med hjälp av PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 34652a55-3ab8-4c2d-8fb2-43684033b191
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: c0a77b2ab8b1ab9bef6015c903eb735ea4358a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reserved-ip-addresses-classic"></a>Den reserverade IP-adresser (klassisk)

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Mall](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klassisk)](virtual-networks-reserved-public-ip.md)

IP-adresser i Azure är indelade i två kategorier: dynamiska och reserverade. Offentliga IP-adresser som hanteras av Azure är dynamiska som standard. Detta innebär att hello IP-adress som används för en viss molntjänst (VIP) eller tooaccess som en virtuell dator eller rollen instans direkt (går) kan ändra från tid tootime när resurserna är avstängd eller stoppats (frigjorts).

tooprevent IP-adresser från att ändra, du kan reservera en IP-adress. Reserverade IP-adresser kan endast användas som en VIP säkerställer att hello IP-adress för hello Molntjänsten förblir hello detsamma, även om resurserna är avstängd eller stoppats (frigjorts). Dessutom kan du konvertera befintliga dynamiska IP-adresser används som en VIP tooa reserverad IP-adress.

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Lär dig hur tooreserve en statisk offentlig IP-adress med hjälp av hello [Resource Manager-distributionsmodellen](virtual-network-ip-addresses-overview-arm.md).

toolearn mer om IP-adresser i Azure kan läsa hello [IP-adresser](virtual-network-ip-addresses-overview-classic.md) artikel.

## <a name="when-do-i-need-a-reserved-ip"></a>När behöver en reserverad IP?
* **Du vill tooensure som hello IP är reserverad i din prenumeration**. Om du vill tooreserve en IP-adress som inte har släppts från din prenumeration under några omständigheter, bör du använda en reserverade offentliga IP-adress.  
* **Du vill att din IP-toostay med Molntjänsten även tvärs över stoppas eller frigöra tillstånd (VM)**. Om du vill att din tjänst toobe som nås via en IP-adress som inte ändras även om virtuella datorer i hello cloud service är avstängd eller stoppa (frigjorts).
* **Du vill att utgående trafik från Azure använder en förutsägbar IP-adress tooensure**. Du kan ha din lokala konfigurerad brandvägg tooallow endast trafik från särskilda IP-adresser. Genom att reservera en IP-adress som du vet hello käll-IP-adress och behöver inte tooupdate brandväggsregler på grund av tooan förändring av IP.

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR
1. Kan jag använda en reserverad IP-adress för alla Azure-tjänster? <br>
    Nej. Reserverade IP-adresser kan bara användas för virtuella datorer och instans molntjänstroller exponeras via en VIP.
2. Hur många reserverade IP-adresser kan ha? <br>
    Mer information finns i hello [Azure begränsar](../azure-subscription-service-limits.md#networking-limits) artikel.
3. Finns det en avgift för reserverade IP-adresser? <br>
    Ibland. Prisinformation, finns hello [reserverade IP-adress prisinformation](http://go.microsoft.com/fwlink/?LinkID=398482) sidan.
4. Hur jag för att reservera en IP-adress? <br>
    Du kan använda PowerShell hello [Azure Management REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx), eller hello [Azure-portalen](https://portal.azure.com) tooreserve en IP-adress i en Azure-region. En reserverad IP-adress är associerade tooyour prenumeration.
5. Kan jag använda en reserverad IP med tillhörighetsgrupp-baserade Vnet? <br>
    Nej. Reserverade IP-adresser stöds endast i regionala virtuella nätverk. Reserverade IP-adresser stöds inte för Vnet som är associerade med tillhörighetsgrupper. Mer information om hur du kopplar ett VNet med en region eller affinitetsgrupp finns hello [om Regional Vnet och Tillhörighetsgrupper](virtual-networks-migrate-to-regional-vnet.md) artikel.

## <a name="manage-reserved-vips"></a>Hantera reserverade virtuella IP-adresser

Se till att du har installerat och konfigurerat PowerShell genom att slutföra hello stegen i hello [installerar och konfigurerar PowerShell](/powershell/azure/overview) artikel. 

Innan du kan använda reserverade IP-adresser måste du lägga till den tooyour prenumeration. toocreate en reserverad IP från hello-pool med offentliga IP-adresser i hello *centrala USA* plats, kör hello följande kommando:

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
```

Observera att du kan ange vilka IP som reserverats. tooview vilka IP-adresser är reserverade i din prenumeration, kör följande PowerShell-kommando hello och Observera hello värden för *ReservedIPName* och *adressen*:

```powershell
Get-AzureReservedIP
```

Förväntad utdata:

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

>[!NOTE]
>När du skapar en reserverad IP-adress med PowerShell kan ange du inte en resurs grupp toocreate hello reserverade IP-adress i. Azure platser till en resursgrupp med namnet *standard-nätverk* automatiskt. Om du skapar hello reserverad IP med hello [Azure-portalen](http://portal.azure.com), du kan ange valfri resursgrupp som du väljer. Om du skapar hello reserverad IP i en resursgrupp än *standard-nätverk* dock när du refererar till hello reserverade IP-Adressen med kommandon som `Get-AzureReservedIP` och `Remove-AzureReservedIP`, måste du referera hello namn *Gruppnamn resursgruppnamn-reserverad-ip*.  Till exempel om du skapar en reserverad IP med namnet *myReservedIP* i en resursgrupp med namnet *myResourceGroup*, måste du referera hello namnet på hello reserverade IP-adress som *grupp myResourceGroup myReservedIP*.   

När en IP-adress är reserverat förblir associerade tooyour prenumeration tills du tar bort den. toodelete en reserverad IP kör hello följande PowerShell-kommando:

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

## <a name="reserve-hello-ip-address-of-an-existing-cloud-service"></a>Reservera hello IP-adressen för en befintlig molntjänst
Du kan reservera hello IP-adressen för en befintlig molntjänst genom att lägga till hello `-ServiceName` parameter. tooreserve hello IP-adressen för en molnbaserad tjänst *TestService* i hello *centrala USA* plats, kör hello följande PowerShell-kommando:

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
```

## <a name="associate-a-reserved-ip-tooa-new-cloud-service"></a>Associera en reserverad IP-tooa ny molntjänst
hello följande skript skapar en ny reserverad IP-adress och sedan kopplar den tooa ny molntjänst med namnet *TestService*.

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```

> [!NOTE]
> När du skapar en reserverad IP-toouse med en molnbaserad tjänst kan du fortfarande hänvisar toohello VM med *VIP:&lt;portnummer >* för inkommande kommunikation. Reservera en IP-innebär inte kan du ansluta toohello VM direkt. hello reserverad IP-adress tilldelas toohello cloud service som hello VM har distribuerats till. Om du vill tooconnect tooa VM efter IP direkt har tooconfigure en offentlig IP på instansnivå. En offentlig IP på instansnivå är en typ av offentlig IP-adress (kallas en går) som är tilldelad direkt tooyour VM. Det går inte att reservera. Mer information finns i hello [instansnivå offentliga IP-går](virtual-networks-instance-level-public-ip.md) artikel.
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a>Ta bort en reserverad IP-adress från en aktiv distribution
tooremove en reserverad IP läggas till tooa ny molntjänst, kör hello följande PowerShell-kommando:

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

> [!NOTE]
> Om du tar bort en reserverad IP-adress från en aktiv distribution tas inte bort hello reservation från prenumerationen. Det gör helt enkelt hello IP toobe används av en annan resurs i din prenumeration.
> 

## <a name="associate-a-reserved-ip-tooa-running-deployment"></a>Associera en reserverad IP-tooa kör distribution
hello följande kommandon skapar en molntjänst med namnet *TestService2* med en ny virtuell dator med namnet *TestVM2*. hello befintlig reserverad IP med namnet *MyReservedIP* är associerade toohello Molntjänsten.

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

## <a name="associate-a-reserved-ip-tooa-cloud-service-by-using-a-service-configuration-file"></a>Koppla en reserverad IP-tooa molntjänst genom att använda en konfigurationsfil för tjänsten
Du kan även associera en reserverad IP-tooa molntjänst med hjälp av en tjänst-konfigurationsfil (CSCFG). hello xml med följande exempel visar hur tooconfigure en cloud service toouse ett reserverat VIP med namnet *MyReservedIP*:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Nästa steg
* Förstå hur [IP-adressering](virtual-network-ip-addresses-overview-classic.md) fungerar i hello klassiska distributionsmodellen.
* Lär dig mer om [reserverade privata IP-adresser](virtual-networks-reserved-private-ip.md).
* Lär dig mer om [instans nivå offentliga IP-går adresser](virtual-networks-instance-level-public-ip.md).

