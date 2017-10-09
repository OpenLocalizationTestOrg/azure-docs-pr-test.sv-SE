---
title: "aaaDelegate din domän tooAzure DNS | Microsoft Docs"
description: "Förstå hur toochange domän delegering och Använd Azure DNS namn servrar tooprovide domän värdar."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: gwallace
ms.openlocfilehash: f780bdaa416150e5e3afe6c6845dc75ba54b6203
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="delegate-a-domain-tooazure-dns"></a>Delegera en domän tooAzure DNS

Azure DNS kan du toohost en DNS-zon och hantera hello DNS-poster för en domän i Azure. För DNS-frågor för en domän tooreach Azure DNS hello domän har toobe delegerad tooAzure DNS från hello överordnad domän. Kom ihåg Azure DNS är inte hello domänregistrator. Den här artikeln förklarar hur toodelegate din domän tooAzure DNS.

För domäner som har köpt från ett register, erbjuder din registrator hello alternativet tooset upp dessa NS-poster. Du har inte tooown domän-toocreate en DNS-zon med det domännamnet i Azure DNS. Du behöver dock tooown hello domän tooset in hello delegering tooAzure DNS med hello register.

Anta att du köper hello domän 'contoso.net' och skapa en zon med hello name 'contoso.net' i Azure DNS. Hello ägare av hello domän erbjuder din registrator du hello alternativet tooconfigure hello adresser (det vill säga hello NS-poster) för din domän. hello registrator lagrar dessa NS-poster i hello överordnade domänen i det här fallet '.net'. Klienter runt hello world kan sedan vara riktad tooyour domän i Azure DNS-zonen när tooresolve DNS-poster i 'contoso.net'.

## <a name="create-a-dns-zone"></a>Skapa en DNS-zon

1. Logga in toohello Azure-portalen
1. Hej hubbmenyn, klicka på och klicka på **New > nätverk >** och klicka sedan på **DNS-zonen** tooopen hello skapa DNS-zonen bladet.

    ![DNS-zon](./media/dns-domain-delegation/dns.png)

1. På hello **skapa DNS-zonen** bladet ange hello följande värden, och klicka sedan på **skapa**:

   | **Inställning** | **Värde** | **Detaljer** |
   |---|---|---|
   |**Namn**|contoso.net|hello namnet på hello DNS-zonen|
   |**Prenumeration**|[Din prenumeration]|Välj en prenumeration toocreate hello Programgateway i.|
   |**Resursgrupp**|**Skapa ny:** contosoRG|Skapa en resursgrupp. hello resursgruppens namn måste vara unikt inom hello-prenumeration som du har valt. Mer om resursgrupper läsa hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) översiktsartikel.|
   |**Plats**|Västra USA||

> [!NOTE]
> hello resursgruppen refererar toohello platsen för hello resursgruppen och har ingen inverkan på hello DNS-zon. hello DNS-zonen plats är alltid ”globala” och visas inte.

## <a name="retrieve-name-servers"></a>Hämta namnservrar

Innan du kan delegera din DNS-zonen tooAzure DNS, måste du först tooknow hello servernamn för zonen. Azure DNS allokerar namnservrar från en pool varje gång en zon skapas.

1. Med hello DNS-zon som skapats i hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**. Klicka på hello **contoso.net** DNS-zonen i hello **alla resurser** bladet. Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange **contoso.net** i hello filtrera efter namn... rutan Programgateway tooeasily åtkomst hello. 

1. Hämta hello namnservrar från hello DNS-zonen bladet. I det här exemplet hello zonen 'contoso.net' har tilldelats namnservrar ' ns1-01.azure-dns.com', 'ns2-01.azure-DNS-.net', ' ns3-01.azure-dns.org', och ' ns4-01.azure-dns.info':

 ![DNS-namnserver](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS skapas automatiskt auktoritära NS-poster i zonen som innehåller hello tilldelade namnservrar.  toosee hello namnserver namn via Azure PowerShell eller Azure CLI, du behöver tooretrieve dessa poster.

hello dessutom följande exempel hello steg tooretrieve hello namnservrar för en zon i Azure DNS med PowerShell och Azure CLI.

### <a name="powershell"></a>PowerShell

```powershell
# hello record name "@" is used toorefer toorecords at hello top of hello zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

följande exempel hello är hello svar.

```
Name              : @
ZoneName          : contoso.net
ResourceGroupName : contosorg
Ttl               : 172800
Etag              : 03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5
RecordType        : NS
Records           : {ns1-07.azure-dns.com., ns2-07.azure-dns.net., ns3-07.azure-dns.org.,
                    ns4-07.azure-dns.info.}
Metadata          :
```

### <a name="azure-cli"></a>Azure CLI

```azurecli
az network dns record-set show --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

följande exempel hello är hello svar.

```json
{
  "etag": "03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosoRG/providers/Microsoft.Network/dnszones/contoso.net/NS/@",
  "metadata": null,
  "name": "@",
  "nsRecords": [
    {
      "nsdname": "ns1-07.azure-dns.com."
    },
    {
      "nsdname": "ns2-07.azure-dns.net."
    },
    {
      "nsdname": "ns3-07.azure-dns.org."
    },
    {
      "nsdname": "ns4-07.azure-dns.info."
    }
  ],
  "resourceGroup": "contosoRG",
  "ttl": 172800,
  "type": "Microsoft.Network/dnszones/NS"
}
```

## <a name="delegate-hello-domain"></a>Delegera hello domän

Nu när du har hello namnservrar och hello DNS-zonen skapas måste toobe uppdateras med hello Azure DNS-namnservrar hello överordnade domänen. Varje register har sina egna DNS management tools toochange hello namnserverposter för en domän. Hej domänregistrators DNS-hantering på sidan Redigera hello NS-poster och Ersätt hello NS-poster med hello som skapats i Azure DNS.

När delegera en domän tooAzure DNS, måste du använda hello servernamn tillhandahålls av Azure DNS. Det rekommenderas toouse alla fyra namn servernamn, oavsett hello namnet på din domän. Domän-delegering kräver inte hello name server name toouse hello samma toppnivådomänen som din domän.

Du bör inte använda ”sammanlänkande poster' toopoint toohello Azure DNS-namnet IP-adresser, eftersom dessa IP-adresser kan ändras i framtiden. Delegering med hjälp av namnservernamn i din egen zon, även kallat ”vanity name servers”, stöds inte i Azure DNS.

## <a name="verify-name-resolution-is-working"></a>Kontrollera att namnmatchningen fungerar

När du har slutfört hello delegering kan du kontrollera att namnmatchning fungerar genom att använda ett verktyg som till exempel ”nslookup” tooquery hello SOA-post för zonen (som skapas automatiskt när hello zon skapas).

Du har inte toospecify hello Azure DNS-namnservrar hittar om hello delegering har ställts in korrekt hello normal DNS lösningsprocessen hello namnservrar automatiskt.

```
nslookup -type=SOA contoso.com
```

hello följande är ett exempelsvar från hello föregående kommando:

```
Server: ns1-04.azure-dns.com
Address: 208.76.47.4

contoso.com
primary name server = ns1-04.azure-dns.com
responsible mail addr = msnhst.microsoft.com
serial = 1
refresh = 900 (15 mins)
retry = 300 (5 mins)
expire = 604800 (7 days)
default TTL = 300 (5 mins)
```

## <a name="delegate-sub-domains-in-azure-dns"></a>Delegera underdomäner i Azure DNS

Om du vill tooset in en separat underordnad zon kan delegera du en underordnad domän i Azure DNS. Till exempel att ställa in och delegerad 'contoso.net' i Azure DNS du anta att du vill tooset in en separat underordnad zon 'partners.contoso.net'.

1. Skapa partners.contoso.net' hello underordnade zonen' i Azure DNS.
2. Leta upp hello auktoritära NS-poster i hello underordnade zonen tooobtain hello namnservrar värd hello underordnade zonen i Azure DNS.
3. Delegera hello underordnade zonen genom att konfigurera NS-poster i hello överordnade zonen pekar toohello underordnad zon.

### <a name="create-a-dns-zone"></a>Skapa en DNS-zon

1. Logga in toohello Azure-portalen
1. Hej hubbmenyn, klicka på och klicka på **New > nätverk >** och klicka sedan på **DNS-zonen** tooopen hello skapa DNS-zonen bladet.

    ![DNS-zon](./media/dns-domain-delegation/dns.png)

1. På hello **skapa DNS-zonen** bladet ange hello följande värden, och klicka sedan på **skapa**:

   | **Inställning** | **Värde** | **Detaljer** |
   |---|---|---|
   |**Namn**|partners.contoso.net|hello namnet på hello DNS-zonen|
   |**Prenumeration**|[Din prenumeration]|Välj en prenumeration toocreate hello Programgateway i.|
   |**Resursgrupp**|**Använd befintlig:** contosoRG|Skapa en resursgrupp. hello resursgruppens namn måste vara unikt inom hello-prenumeration som du har valt. Mer om resursgrupper läsa hello toolearn [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) översiktsartikel.|
   |**Plats**|Västra USA||

> [!NOTE]
> hello resursgruppen refererar toohello platsen för hello resursgruppen och har ingen inverkan på hello DNS-zon. hello DNS-zonen plats är alltid ”globala” och visas inte.

### <a name="retrieve-name-servers"></a>Hämta namnservrar

1. Med hello DNS-zon som skapats i hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**. Klicka på hello **partners.contoso.net** DNS-zonen i hello **alla resurser** bladet. Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange **partners.contoso.net** i hello filtrera efter namn... rutan tooeasily åtkomst hello DNS-zon.

1. Hämta hello namnservrar från hello DNS-zonen bladet. I det här exemplet hello zonen 'contoso.net' har tilldelats namnservrar ' ns1-01.azure-dns.com', 'ns2-01.azure-DNS-.net', ' ns3-01.azure-dns.org', och ' ns4-01.azure-dns.info':

 ![DNS-namnserver](./media/dns-domain-delegation/viewzonens500.png)

Azure DNS skapas automatiskt auktoritära NS-poster i zonen som innehåller hello tilldelade namnservrar.  toosee hello namnserver namn via Azure PowerShell eller Azure CLI, du behöver tooretrieve dessa poster.

### <a name="create-name-server-record-in-parent-zone"></a>Skapa namnserverpost i överordnad zon

1. Navigera toohello **contoso.net** DNS-zonen i hello Azure-portalen.
1. Klicka på **+ Postuppsättning**
1. På hello **lägga till postuppsättning** bladet ange hello följande värden, och klicka sedan på **OK**:

   | **Inställning** | **Värde** | **Detaljer** |
   |---|---|---|
   |**Namn**|partner|hello namnet på hello underordnade DNS-zonen|
   |**Typ**|NS|Använd NS för namnserverposter.|
   |**TTL**|1|Tid toolive.|
   |**TTL-enhet**|Timmar|Anger tid toolive enhet toohours|
   |**NAMNSERVER**|{namnservrar från zonen partners.contoso.net}|Ange alla 4 i hello namnservrar från partners.contoso.net zon. |

   ![DNS-namnserver](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a>Delegera underdomäner i Azure DNS med andra verktyg

hello innehåller följande exempel hello steg toodelegate underdomäner i Azure DNS med PowerShell och CLI:

#### <a name="powershell"></a>PowerShell

hello följande PowerShell-exempel visar hur det fungerar. hello samma steg kan utföras via hello Azure-portalen eller via hello plattformsoberoende Azure CLI.

```powershell
# Create hello parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve hello authoritative NS records from hello child zone as shown in hello next example. This contains hello name servers assigned toohello child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create hello corresponding NS record set in hello parent zone toocomplete hello delegation. hello record set name in hello parent zone matches hello child zone name, in this case "partners".
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

Använd `nslookup` tooverify allt har konfigurerats korrekt genom att leta upp hello SOA-post på hello underordnad zon.

```
nslookup -type=SOA partners.contoso.com
```

```
Server: ns1-08.azure-dns.com
Address: 208.76.47.8

partners.contoso.com
    primary name server = ns1-08.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)
```

#### <a name="azure-cli"></a>Azure CLI

```azurecli
#!/bin/bash

# Create hello parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

Hämta hello namnservrar för hello `partners.contoso.net` zon från hello-utdata.

```
{
  "etag": "00000003-0000-0000-418f-250de2b2d201",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosorg/providers/Microsoft.Network/dnszones/partners.contoso.net",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "partners.contoso.net",
  "nameServers": [
    "ns1-09.azure-dns.com.",
    "ns2-09.azure-dns.net.",
    "ns3-09.azure-dns.org.",
    "ns4-09.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "resourceGroup": "contosorg",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Skapa hello postuppsättning och NS-poster för varje namnserver.

```azurecli
#!/bin/bash

# Create hello record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create a ns record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a>Ta bort alla resurser

toodelete alla resurser skapas i den här artikeln, fullständig hello följande steg:

1. I hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**. Klicka på hello **contosorg** resursgrupp i hello bladet för alla resurser. Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange **contosorg** i hello **filtrera efter namn...** rutan tooeasily åtkomst hello resursgruppens namn.
1. I hello **contosorg** bladet, klickar du på hello **ta bort** knappen.
1. hello portalen måste du tootype hello namnet på hello resurs grupp tooconfirm som du vill toodelete den. Typen *contosorg* hello resursgruppens namn, sedan klickar du på **ta bort**. Tar bort en resursgrupp alla resurser inom hello resursgrupp, så alltid att tooconfirm hello innehållet i en resursgrupp innan den tas bort. hello portal tar bort alla resurser som ingår i hello resursgrupp och sedan tar bort hello resursgruppen sig själv. Den här processen tar flera minuter.

## <a name="next-steps"></a>Nästa steg

[Hantera DNS-zoner](dns-operations-dnszones.md)

[Hantera DNS-poster](dns-operations-recordsets.md)
