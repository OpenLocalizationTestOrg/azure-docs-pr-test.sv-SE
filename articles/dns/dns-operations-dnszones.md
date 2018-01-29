---
title: Hantera DNS-zoner i Azure DNS - PowerShell | Microsoft Docs
description: "Du kan hantera DNS-zoner med hjälp av Azure Powershell. Den här artikeln beskriver hur du uppdaterar, ta bort och skapa DNS-zoner på Azure DNS"
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
ms.date: 09/22/2016
ms.author: gwallace
ms.openlocfilehash: 3f28e70bb6ef46f53375d256a520db40fcb71ad0
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-manage-dns-zones-using-powershell"></a>Hur du hanterar DNS-zoner med hjälp av PowerShell

> [!div class="op_single_selector"]
> * [Portal](dns-operations-dnszones-portal.md)
> * [PowerShell](dns-operations-dnszones.md)
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)

Den här artikeln visar hur du hanterar DNS-zoner med hjälp av Azure PowerShell. Du kan också hantera DNS-zoner med flera plattformar [Azure CLI](dns-operations-dnszones-cli.md) eller Azure-portalen.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

[!INCLUDE [dns-powershell-setup](../../includes/dns-powershell-setup-include.md)]


## <a name="create-a-dns-zone"></a>Skapa en DNS-zon

En DNS-zon skapas med hjälp av cmdleten `New-AzureRmDnsZone`.

I följande exempel skapas en DNS-zon som kallas *contoso.com* i resursgruppen kallas *MyResourceGroup*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

I följande exempel visas hur du skapar en DNS-zon med två [Azure Resource Manager taggar](dns-zones-records.md#tags), *projekt = demo* och *env = test*:

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

Azure DNS stöder nu också privata DNS-zoner (för närvarande en förhandsvisningsfunktion).  Ett exempel på hur du skapar en privat DNS-zon finns [Kom igång med privata Azure DNS-zoner med hjälp av PowerShell](./private-dns-getstarted-powershell.md).

## <a name="get-a-dns-zone"></a>Hämta en DNS-zon

Använd för att hämta en DNS-zon på `Get-AzureRmDnsZone` cmdlet. Den här åtgärden returnerar ett objekt för DNS-zon som motsvarar en befintlig zon i Azure DNS. Objektet innehåller information om zonen (till exempel antal uppsättningar av poster), men innehåller inte postuppsättningar själva (se `Get-AzureRmDnsRecordSet`).

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

Genom att utelämna zonnamnet från `Get-AzureRmDnsZone`, kan du räkna upp alla zoner i en resursgrupp. Den här åtgärden returnerar en matris med zonen objekt.

```powershell
$zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup
```

Genom att utelämna både zonnamnet och resursgruppens namn från `Get-AzureRmDnsZone`, kan du räkna upp alla zoner i Azure-prenumeration.

```powershell
$zoneList = Get-AzureRmDnsZone
```

## <a name="update-a-dns-zone"></a>Uppdatera en DNS-zon

En DNS-zon resurs kan ändras med hjälp av `Set-AzureRmDnsZone`. Denna cmdlet inte uppdatera alla DNS-postuppsättningar i zonen (se [hur du hanterar DNS-poster](dns-operations-recordsets.md)). Den används endast för att uppdatera egenskaper för resursen zonen. Skrivbar zonegenskaperna är för tillfället begränsad till den [Azure Resource Manager-taggar ”för resursen zonen](dns-zones-records.md#tags).

Använd någon av följande två sätt att uppdatera en DNS-zon:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Ange zonen med namn och resursen zoner

Den här metoden ersätter befintliga zonens taggarna med de angivna värdena.

```powershell
Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @{ project="demo"; env="test" }
```

### <a name="specify-the-zone-using-a-zone-object"></a>Ange zonen med ett $zone-objekt

Den här metoden hämtar det befintliga objektet i zonen, ändrar taggar och genomför ändringarna. På så sätt kan kan befintliga taggar bevaras.

```powershell
# Get the zone object
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

# Remove an existing tag
$zone.Tags.Remove("project")

# Add a new tag
$zone.Tags.Add("status","approved")

# Commit changes
Set-AzureRmDnsZone -Zone $zone
```

När du använder `Set-AzureRmDnsZone` med ett $zone-objekt [Etag kontrollerar](dns-zones-records.md#etags) används för att kontrollera samtidiga ändringar inte att skrivas över. Du kan använda det valfria `-Overwrite` växel för att ignorera dessa kontroller.

## <a name="delete-a-dns-zone"></a>Ta bort en DNS-zon

DNS-zoner kan tas bort med den `Remove-AzureRmDnsZone` cmdlet.

> [!NOTE]
> En DNS-zon också tar du bort DNS-poster i zonen. Den här åtgärden kan inte ångras. Om DNS-zonen misslyckas tjänster med hjälp av zonen när zonen tas bort.
>
>För att skydda mot oavsiktlig zonen borttagning finns [Skydda DNS-zoner och poster](dns-protect-zones-recordsets.md).


Använd någon av följande två sätt att ta bort en DNS-zon:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Ange zonen med zonnamnet och resursgruppens namn

```powershell
Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
```

### <a name="specify-the-zone-using-a-zone-object"></a>Ange zonen med ett $zone-objekt

Du kan ange zonen som ska tas bort med hjälp av en `$zone` objektet som returnerades av `Get-AzureRmDnsZone`.

```powershell
$zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
Remove-AzureRmDnsZone -Zone $zone
```

Objektet zon kan även skickas i stället för att skickas som en parameter:

```powershell
Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone

```

Precis som med `Set-AzureRmDnsZone`, ange en zon med hjälp av en `$zone` objekt gör det möjligt för Etag-kontroller för att säkerställa samtidiga ändringar inte tas bort. Använd den `-Overwrite` växel för att ignorera dessa kontroller.

## <a name="confirmation-prompts"></a>Konfigurera meddelanden

Den `New-AzureRmDnsZone`, `Set-AzureRmDnsZone`, och `Remove-AzureRmDnsZone` cmdlets alla stöder konfigurera meddelanden.

Båda `New-AzureRmDnsZone` och `Set-AzureRmDnsZone` fråga efter bekräftelse om de `$ConfirmPreference` PowerShell inställningsvariabeln har värdet `Medium` eller lägre. På grund av potentiellt hög effekten av att ta bort en DNS-zon på `Remove-AzureRmDnsZone` cmdlet uppmanas att bekräfta om den `$ConfirmPreference` PowerShell variabel har ett värde annat än `None`.

Eftersom standardvärdet för `$ConfirmPreference` är `High`, endast `Remove-AzureRmDnsZone` uppmanas att bekräfta som standard.

Du kan åsidosätta aktuellt `$ConfirmPreference` inställningen med hjälp av den `-Confirm` parameter. Om du anger `-Confirm` eller `-Confirm:$True` , så uppmanas du att bekräfta innan den körs. Om du anger `-Confirm:$False` , cmdleten visas inte efter bekräftelse.

Mer information om `-Confirm` och `$ConfirmPreference`, se [om variabler för](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Nästa steg

Lär dig hur du [hantera uppsättningar av poster och poster](dns-operations-recordsets.md) i DNS-zonen.
<br>
Lär dig hur du [Delegera din domän till Azure DNS](dns-domain-delegation.md).
<br>
Granska de [Azure DNS-PowerShell referensdokumentationen](/powershell/module/azurerm.dns).

