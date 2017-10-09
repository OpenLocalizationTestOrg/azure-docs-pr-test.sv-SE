---
title: "aaaManage DNS-poster i Azure DNS med hjälp av Azure PowerShell | Microsoft Docs"
description: "Hantera DNS-postuppsättningar och på Azure DNS-poster när värd för din domän på Azure DNS. Alla PowerShell-kommandon för åtgärder på uppsättningar av poster och poster."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 7136a373-0682-471c-9c28-9e00d2add9c2
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: bfdf116e174d06db0514abdc0ec3f4fc4ee0a079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a>Hantera DNS-poster och postuppsättningar i Azure DNS med hjälp av Azure PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](dns-operations-recordsets-portal.md)
> * [Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-recordsets-cli.md)
> * [PowerShell](dns-operations-recordsets.md)

Den här artikeln visar hur toomanage DNS poster för DNS-zonen med hjälp av Azure PowerShell. DNS-poster kan också hanteras med hjälp av hello plattformsoberoende [Azure CLI](dns-operations-recordsets-cli.md) eller hello [Azure-portalen](dns-operations-recordsets-portal.md).

hello exemplen i den här artikeln förutsätter att du redan har [installerat Azure PowerShell loggat in, och skapa en DNS-zon](dns-operations-dnszones.md).

## <a name="introduction"></a>Introduktion

Innan du skapar DNS-poster i Azure DNS, måste du först toounderstand hur Azure DNS organiserar DNS-poster i DNS-postuppsättningar.

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

Mer information om DNS-poster i Azure DNS finns i [DNS-zoner och poster](dns-zones-records.md).


## <a name="create-a-new-dns-record"></a>Skapa en ny DNS-post

Om din nya posten har hello samma namn och som en befintlig post, du behöver för[lägga till den befintliga postuppsättning för toohello](#add-a-record-to-an-existing-record-set). Om din nya posten har ett annat namn och typ tooall befintliga poster, måste toocreate en ny uppsättning av poster. 

### <a name="create-a-records-in-a-new-record-set"></a>Skapa ”A” poster i en ny uppsättning av poster

Du skapar postuppsättningar med hello `New-AzureRmDnsRecordSet` cmdlet. När du skapar en postuppsättning måste toospecify hello postuppsättningens namn, hello zonen, hello tid toolive (TTL), hello posttyp och hello poster toobe skapades.

hello parametrar för att lägga till poster tooa postuppsättning varierar beroende på hello typ av hello uppsättningen av poster. Till exempel när du använder en postuppsättning av typen ”A”, måste toospecify hello IP-adressen med hello parametern `-IPv4Address`. Andra parametrar används för andra typer av poster. Se [ytterligare posttyp exempel](#additional-record-type-examples) mer information.

hello skapas följande exempel en postuppsättning med hello relativa namnet www om du i hello DNS-zonen ”contoso.com”. hello fullständigt kvalificerade namnet på postuppsättningen hello är ”www.contoso.com”. hello-posttypen är ”A” och hello TTL-värde är 3 600 sekunder. hello postuppsättning innehåller en post med IP-adressen '1.2.3.4'.

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

toocreate en postuppsättning på hello apex om du för en zon (i det här fallet ”contoso.com”), hello används postuppsättningsnamnet ”@” (utan citattecken):

```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

Om du behöver en post som innehåller fler än en post toocreate först skapa en lokal matris och lägga till hello poster och sedan skicka hello matris för`New-AzureRmDnsRecordSet` på följande sätt:

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

[Postuppsättning metadata](dns-zones-records.md#tags-and-metadata) kan vara används tooassociate programspecifika data med varje postuppsättningen, som nyckel / värde-par. hello följande exempel visas hur toocreate en postuppsättning med två metadataposter ”Avd = ekonomi ' och ' miljö = produktion”.

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

Azure DNS stöder också 'empty-postuppsättningar som kan fungera som en platshållare tooreserve ett DNS-namn innan du skapar DNS-poster. Tom postuppsättningar är synliga i hello Azure DNS-kontrollplan, men visas på hello Azure DNS-namnservrar. hello följande exempel skapar en tom postuppsättning:

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a>Skapa poster för andra typer

Att ha sett i detalj hur toocreate ”A” poster, hello följande exempel visas hur toocreate poster av andra post typer som stöds av Azure DNS.

I varje fall visar vi hur toocreate en post som innehåller en post. hello tidigare exempel ”A” poster kan vara anpassats toocreate postuppsättningar av andra typer som innehåller flera poster med metadata, eller toocreate tom postuppsättningar.

Vi inte ger ett exempel toocreate ett SOA-postuppsättning eftersom SOAs skapas och tas bort tillsammans med varje DNS-zon och kan inte skapas eller tas bort separat. Dock [hello SOA kan ändras, som visas i en senare exempel](#to-modify-an-SOA-record).

### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Skapa en AAAA-postuppsättning med en post

```powershell
New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a>Skapa en CNAME-postuppsättning med en post

> [!NOTE]
> hello DNS-standarden inte tillåter CNAME-poster på hello apex för en zon (`-Name '@'`), eller tillåter att postuppsättningar som innehåller fler än en post.
> 
> Mer information finns i [CNAME-poster](dns-zones-records.md#cname-records).


```powershell
New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a>Skapa en MX-postuppsättning med en post

I det här exemplet använder vi hello postuppsättningsnamnet ”@” toocreate en MX-post vid hello zonens apex (i det här fallet ”contoso.com”).


```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a>Skapa en NS-postuppsättning med en post

```powershell
New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a>Skapa en PTR-postuppsättning med en post

I det här fallet ”min-arpa-zone.com' representerar hello ARPA zon för omvänd sökning som motsvarar IP-adressintervall. Varje PTR-post i den här zonen motsvarar tooan IP-adress inom IP-intervallet. hello-postnamnet ”10” är hello sista oktetten hello IP-adress inom den här IP-adressintervall som representeras av den här posten.

```powershell
New-AzureRmDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a>Skapa en SRV-postuppsättning med en post

När du skapar en [SRV-postuppsättning](dns-zones-records.md#srv-records), ange hello  *\_service* och  *\_protokollet* i hello postuppsättning namn. Det finns inget behov av tooinclude ”@” i hello postuppsättning namn när du skapar en SRV-post på hello zonens apex.

```powershell
New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a>Skapa en TXT-postuppsättning med en post

hello följande exempel visas hur toocreate en TXT-posten. Mer information om hello stränglängden som stöds i TXT-poster finns [TXT-poster](dns-zones-records.md#txt-records).

```powershell
New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a>Hämta en uppsättning poster

tooretrieve befintliga postuppsättningen, Använd `Get-AzureRmDnsRecordSet`. Denna cmdlet returnerar ett lokalt objekt som representerar hello postuppsättningen i Azure DNS.

Precis som med `New-AzureRmDnsRecordSet`, hello postuppsättning namn måste vara en *relativa* namn, vilket innebär att den inte får innehålla hello zonnamnet. Du måste också toospecify hello posttyp och hello zon som innehåller hello uppsättningen av poster.

hello följande exempel visas hur tooretrieve en postuppsättning. I det här exemplet hello zon anges med hjälp av hello `-ZoneName` och `-ResourceGroupName` parametrar.

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Du kan också också ange hello zon med hjälp av en zon-objekt som överförts med hello `-Zone` parameter.

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a>Lista över postuppsättningar

Du kan också använda `Get-AzureRmDnsZone` toolist postuppsättningar i en zon för genom att utelämna hello `-Name` och/eller `-RecordType` parametrar.

hello följande exempel returneras alla postuppsättningar i zonen hello:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

hello följande exempel visar hur alla uppsättningar av en viss typ kan hämtas genom att ange hello posttyp när utelämna hello post anges namn:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

tooretrieve alla postuppsättningar med ett angivet namn över posttyper, behöver du tooretrieve alla postuppsättningar och sedan hello filterresultaten:

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

I alla hello exemplen ovan hello zon kan vara anges antingen med hjälp av hello `-ZoneName` och `-ResourceGroupName`parametrar (som visas), eller genom att ange ett objekt i zonen:

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzureRmDnsRecordSet -Zone $zone
```

## <a name="add-a-record-tooan-existing-record-set"></a>Lägg till en post tooan befintliga uppsättningen av poster

tooadd en befintlig post poster tooan ange, följ hello följande tre steg:

1. Hämta hello befintliga postuppsättning

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Lägg till hello nya poster toohello lokala postuppsättning. Detta är en åtgärd som är offline.

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Bekräfta hello ändra tillbaka toohello Azure DNS-tjänsten. 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

Med hjälp av `Set-AzureRmDnsRecordSet` *ersätter* hello befintliga posten i Azure DNS (och alla poster som den innehåller) med hello postuppsättning anges. [ETag kontrollerar](dns-zones-records.md#etags) används tooensure samtidiga ändringar inte att skrivas över. Du kan använda valfri hello `-Overwrite` växla toosuppress kontrollerna.

Den här sekvensen av åtgärder kan också vara *skickas*, vilket innebär att du skickar hello postuppsättning objekt genom att använda hello pipe i stället för att skicka som en parameter:

```powershell
Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

hello exemplen ovan visar hur tooadd en ”A” poster tooan befintlig post inställd av typen ”A”. En liknande sekvens av åtgärder är används tooadd poster toorecord uppsättningar av andra typer som ersätter hello `-Ipv4Address` -parametern för `Add-AzureRmDnsRecordConfig` med andra parametrar specifika tooeach posttyp. hello parametrar för varje posttyp är hello samma som för hello `New-AzureRmDnsRecordConfig` cmdlet, som visas i [ytterligare posttyp exempel](#additional-record-type-examples) ovan.

Postuppsättningar av typen 'CNAME' eller 'SOA-' får inte innehålla fler än en post. Den här begränsningen uppstår från hello DNS-standarden. Det är inte en begränsning i Azure DNS.

## <a name="remove-a-record-from-an-existing-record-set"></a>Ta bort en post från en befintlig post

hello processen tooremove en post från en postuppsättning är liknande toohello processen tooadd en post tooan befintliga postuppsättningen:

1. Hämta hello befintliga postuppsättning

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. Ta bort hello-posten från hello lokala postuppsättning-objekt. Detta är en åtgärd som är offline. hello-post som tas bort måste vara en exakt matchning med en befintlig post över alla parametrar.

    ```powershell
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. Bekräfta hello ändra tillbaka toohello Azure DNS-tjänsten. Använd hello valfria `-Overwrite` växla toosuppress [Etag kontrollerar](dns-zones-records.md#etags) för samtidiga ändringar.

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $Rs
    ```

Med hello ovan sekvens tooremove hello sista posten från en uppsättning poster tas inte bort hello postuppsättningen, i stället de lämnar en tom postuppsättning. tooremove en postuppsättning helt, se [ta bort en postuppsättning](#delete-a-record-set).

På liknande sätt postuppsättning tooadding poster tooa, hello sekvens med åtgärder tooremove en postuppsättning kan också skickas:

```powershell
Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

Olika posttyper stöds genom att ange hello lämpliga typspecifika parametrar för`Remove-AzureRmDnsRecordSet`. hello parametrar för varje posttyp är hello samma som för hello `New-AzureRmDnsRecordConfig` cmdlet, som visas i [ytterligare posttyp exempel](#additional-record-type-examples) ovan.


## <a name="modify-an-existing-record-set"></a>Ändra en befintlig postuppsättning

hello stegen för att ändra en befintlig postuppsättning är liknande toohello steg du ta när du lägger till eller ta bort poster från en postuppsättning:

1. Hämta hello befintliga-postuppsättning med hjälp av `Get-AzureRmDnsRecordSet`.
2. Ändra hello lokala postuppsättning objekt genom att:
    * Tillägg eller borttagning av poster
    * Ändra hello parametrar av befintliga poster
    * Ändra hello post ange metadata och toolive TTL (time)
3. Genomför ändringarna med hjälp av hello `Set-AzureRmDnsRecordSet` cmdlet. Detta *ersätter* hello befintliga posten i Azure DNS med hello postuppsättning anges.

När du använder `Set-AzureRmDnsRecordSet`, [Etag kontrollerar](dns-zones-records.md#etags) används tooensure samtidiga ändringar inte att skrivas över. Du kan använda valfri hello `-Overwrite` växla toosuppress kontrollerna.

### <a name="tooupdate-a-record-in-an-existing-record-set"></a>Ange tooupdate en post i en befintlig post

I det här exemplet ändrar vi hello IP-adressen för en befintlig ”A” post:

```powershell
$rs = Get-AzureRmDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-an-soa-record"></a>toomodify en SOA-post

Du kan inte lägga till eller ta bort poster från hello skapas automatiskt SOA-postuppsättning på zonens apex hello (`-Name "@"`, inklusive citattecken). Men du kan ändra någon av parametrarna hello inom hello SOA-post (utom ”värd”) och hello postuppsättning TTL-värde.

följande exempel visar hur hello toochange hello *e-post* -egenskapen för hello SOA-post:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-ns-records-at-hello-zone-apex"></a>toomodify NS-poster på hello zonens apex

hello NS-postuppsättning på zonens apex hello skapas automatiskt med varje DNS-zon. Den innehåller hello namnen på hello Azure DNS-namnet servrar tilldelade toohello zon.

Du kan lägga till ytterligare namn servrar toothis NS postuppsättningen, toosupport samordna värd domäner med mer än en DNS-leverantör. Du kan också ändra hello TTL-värde och metadata för den här postuppsättningen. Du kan inte ta bort eller ändra hello förifyllda Azure DNS-namnservrar.

Observera att detta gäller endast toohello NS postuppsättning på zonens apex hello. Andra NS-postuppsättningar i zonen (som används toodelegate underordnade zoner) kan ändras utan begränsning.

hello som följande exempel visar hur tooadd en ytterligare toohello NS namnserverposten ange på hello zonens apex:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-record-set-metadata"></a>toomodify postuppsättning metadata

[Postuppsättning metadata](dns-zones-records.md#tags-and-metadata) kan vara används tooassociate programspecifika data med varje postuppsättningen, som nyckel / värde-par.

hello följande exempel visas hur toomodify hello metadata för en befintlig post angetts:

```powershell
# Get hello record set
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"

# Add 'dept=finance' name-value pair
$rs.Metadata.Add('dept', 'finance') 

# Remove metadata item named 'environment'
$rs.Metadata.Remove('environment')  

# Commit changes
Set-AzureRmDnsRecordSet -RecordSet $rs
```


## <a name="delete-a-record-set"></a>Ta bort en uppsättning poster

Postuppsättningar kan tas bort med hjälp av hello `Remove-AzureRmDnsRecordSet` cmdlet. En postuppsättning också tar bort alla poster inom hello uppsättningen av poster.

> [!NOTE]
> Du kan inte ta bort hello SOA- och NS postuppsättningar vid basdomänen hello (`-Name '@'`).  Azure DNS skapas de automatiskt när hello zonen skapades och tar bort dem automatiskt när hello zon tas bort.

hello följande exempel visas hur toodelete en postuppsättning. I det här exemplet anges hello postuppsättningens namn, postuppsättning typ, zonnamnet och resursgruppen varje explicit.

```powershell
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

Du kan också hello uppsättningen av poster kan anges med namn och typ och hello zonen angivna med hjälp av ett objekt:

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

Som ett tredje alternativ, kan du ange hello-postuppsättning sig själv med en postuppsättning-objekt:

```powershell
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -RecordSet $rs
```

När du anger hello postuppsättning toobe tas bort med hjälp av en postuppsättning objektet [Etag kontrollerar](dns-zones-records.md#etags) används tooensure samtidiga ändringar tas inte bort. Du kan använda valfri hello `-Overwrite` växla toosuppress kontrollerna.

hello postuppsättning objekt kan även skickas i stället för att skickas som en parameter:

```powershell
Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzureRmDnsRecordSet
```

## <a name="confirmation-prompts"></a>Konfigurera meddelanden

Hej `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, och `Remove-AzureRmDnsRecordSet` cmdlets alla stöder konfigurera meddelanden.

Varje cmdlet uppmanas att bekräfta om hello `$ConfirmPreference` PowerShell inställningsvariabeln har värdet `Medium` eller lägre. Eftersom hello standardvärdet för `$ConfirmPreference` är `High`, dessa frågor inte anges när du använder hello standardinställningarna för PowerShell.

Du kan åsidosätta hello aktuella `$ConfirmPreference` inställningen med hello `-Confirm` parameter. Om du anger `-Confirm` eller `-Confirm:$True` , hello cmdlet uppmanar dig att bekräfta innan den körs. Om du anger `-Confirm:$False` , hello cmdleten visas inte efter bekräftelse. 

Mer information om `-Confirm` och `$ConfirmPreference`, se [om variabler för](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).

## <a name="next-steps"></a>Nästa steg

Lär dig mer om [zoner och poster i Azure DNS](dns-zones-records.md).
<br>
Lär dig hur för[skydda zoner och poster](dns-protect-zones-recordsets.md) när du använder Azure DNS.
<br>
Granska hello [Azure DNS-PowerShell referensdokumentationen](/powershell/module/azurerm.dns).
