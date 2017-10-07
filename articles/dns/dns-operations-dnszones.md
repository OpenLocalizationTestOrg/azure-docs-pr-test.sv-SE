---
title: aaaManage DNS-zoner i Azure DNS - PowerShell | Microsoft Docs
description: "Du kan hantera DNS-zoner med hjälp av Azure Powershell. Den här artikeln beskriver hur tooupdate, ta bort och skapa DNS-zoner på Azure DNS"
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: a67992ab-8166-4052-9b28-554c5a39e60c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: gwallace
ms.openlocfilehash: 261b89f72213aa9784034d47ff9d1c55a4e80d65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-dns-zones-using-powershell"></a>Hur toomanage DNS-zoner med hjälp av PowerShell

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Den här artikeln visar hur toomanage DNS-zoner med hjälp av Azure PowerShell. Du kan också hantera DNS-zoner med hello plattformsoberoende [Azure CLI](dns-operations-dnszones-cli.md) eller hello Azure-portalen.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a>Skapa en DNS-zon

En DNS-zon skapas med hjälp av hello `New-AzureRmDnsZone` cmdlet.

hello följande exempel skapas en DNS-zon som kallas *contoso.com* i hello resursgrupp med namnet *MyResourceGroup*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

hello följande exempel visas hur toocreate ett DNS zonen med två [Azure Resource Manager taggar](dns-zones-records.md#tags), *projekt = demo* och *env = test*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

## <a name="get-a-dns-zone"></a>Hämta en DNS-zon

tooretrieve en DNS-zon använder hello `Get-AzureRmDnsZone` cmdlet. Den här åtgärden returnerar ett DNS-zonen objektet motsvarande tooan befintlig zon i Azure DNS. hello objekt innehåller information om hello zonen (till exempel hello Antal uppsättningar av poster), men innehåller inte hello postuppsättningar själva (se `Get-AzureRmDnsRecordSet`).

```powershell
Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-8ec2-f4879750d201
Tags                  : {project, env}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org.,
                        ns4-01.azure-dns.info.}
NumberOfRecordSets    : 2
MaxNumberOfRecordSets : 5000
```

## <a name="list-dns-zones"></a>Lista över DNS-zoner

Genom att utelämna hello zonnamnet från `Get-AzureRmDnsZone`, kan du räkna upp alla zoner i en resursgrupp. Den här åtgärden returnerar en matris med zonen objekt.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

Genom att utelämna både hello zonnamnet och hello resursgruppens namn från `Get-AzureRmDnsZone`, kan du räkna upp alla zoner i hello Azure-prenumeration.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a>Uppdatera en DNS-zon

Ändrar tooa DNS-zonresurs kan göras med hjälp av `Set-AzureRmDnsZone`. Denna cmdlet inte uppdatera alla hello DNS-postuppsättningar i zonen hello (se [hur tooManage DNS-poster](dns-operations-recordsets.md)). Den används endast tooupdate egenskaper för hello zonresurs sig själv. hello skrivbar zonegenskaperna är för närvarande begränsad toohello [Azure Resource Manager-taggar' för hello zonresurs](dns-zones-records.md#tags).

Använd någon av följande två sätt tooupdate hello en DNS-zon:

### <a name="specify-hello-zone-using-hello-zone-name-and-resource-group"></a>Ange hello zon med hjälp av hello namn och en resurs i zoner

Den här metoden ersätter hello befintliga zonen taggar med hello värden som har angetts.

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-hello-zone-using-a-zone-object"></a>Ange hello zon med ett $zone-objekt

Den här metoden hämtar hello befintlig zon objekt, ändrar hello taggar och genomför ändringar som hello. På så sätt kan kan befintliga taggar bevaras.

```powershell
# Get hello zone object
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

# Remove an existing tag
$zone.Tags.Remove("project")

# Add a new tag
$zone.Tags.Add("status","approved")

# Commit changes
Set-AzureRmDnsZone -Zone $zone
```

När du använder `Set-AzureRmDnsZone` med ett $zone-objekt [Etag kontrollerar](dns-zones-records.md#etags) används tooensure samtidiga ändringar inte att skrivas över. Du kan använda valfri hello `-Overwrite` växla toosuppress kontrollerna.

## <a name="delete-a-dns-zone"></a>Ta bort en DNS-zon

DNS-zoner kan tas bort med hello `Remove-AzureRmDnsZone` cmdlet.

> [!NOTE]
> En DNS-zon också tar du bort DNS-poster i zonen hello. Den här åtgärden kan inte ångras. Om hello DNS-zon används, misslyckas tjänster med hjälp av hello zon när hello zon tas bort.
>
>tooprotect mot oavsiktlig zonen radering finns [hur tooprotect DNS zoner och registrerar](dns-protect-zones-recordsets.md).


Använd någon av följande två sätt toodelete hello en DNS-zon:

### <a name="specify-hello-zone-using-hello-zone-name-and-resource-group-name"></a>Ange hello zon med hjälp av hello zonnamnet och resursgruppens namn

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-hello-zone-using-a-zone-object"></a>Ange hello zon med ett $zone-objekt

Du kan ange hello zonen toobe tas bort med hjälp av en `$zone` objektet som returnerades av `Get-AzureRmDnsZone`.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

hello zonen objekt kan även skickas i stället för att skickas som en parameter:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

Precis som med `Set-AzureRmDnsZone`, att ange hello zon med hjälp av en `$zone` objekt aktiverar Etag kontrollerar tooensure samtidiga ändringar inte tas bort. Använd hello `-Overwrite` växla toosuppress kontrollerna.

## <a name="confirmation-prompts"></a>Konfigurera meddelanden

Hej `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, och `Remove-AzureRmDnsZone` cmdlets alla stöder konfigurera meddelanden.

Båda `New-AzureRmDnsZone` och `Set-AzureRmDnsZone` fråga efter bekräftelse om hello `$ConfirmPreference` PowerShell inställningsvariabeln har värdet `Medium` eller lägre. På grund av toohello potentiellt hög inverkan av att ta bort en DNS-zon hello `Remove-AzureRmDnsZone` cmdlet uppmanas att bekräfta om hello `$ConfirmPreference` PowerShell variabel har ett värde annat än `None`.

Eftersom hello standardvärdet för `$ConfirmPreference` är `High`, endast `Remove-AzureRmDnsZone` uppmanas att bekräfta som standard.

Du kan åsidosätta hello aktuella `$ConfirmPreference` inställningen med hello `-Confirm` parameter. Om du anger `-Confirm` eller `-Confirm:$True` , hello cmdlet uppmanar dig att bekräfta innan den körs. Om du anger `-Confirm:$False` , hello cmdleten visas inte efter bekräftelse.

Mer information om `-Confirm` och `$ConfirmPreference`, se [om variabler för](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Nästa steg

Lär dig hur för[hantera uppsättningar av poster och poster](dns-operations-recordsets.md) i DNS-zonen.
<br>
Lär dig hur för[Delegera din domän tooAzure DNS](dns-domain-delegation.md).
<br>
Granska hello [Azure DNS-PowerShell referensdokumentationen](/powershell/module/azurerm.dns).

