---
title: "aaaCreate anpassade DNS-poster för en webbapp | Microsoft Docs"
description: "Hur toocreate domänen DNS-poster för webbapp med Azure DNS."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 6c16608c-4819-44e7-ab88-306cf4d6efe5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/16/2016
ms.author: gwallace
ms.openlocfilehash: 070c808a55bab922eb624d99ae5c275d8eaa5aaa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Skapa DNS-poster för ett webbprogram i en anpassad domän

Du kan använda Azure DNS toohost en anpassad domän för ditt webbprogram. Till exempel du skapar en Azure webbapp och du vill att dina användare tooaccess den genom att antingen använda contoso.com eller www.contoso.com som ett fullständigt domännamn.

toodo, toocreate två poster:

* En rot ”A” poster peka toocontoso.com
* ”CNAME”-posten för hello www namn som pekar toohello en post

Tänk på att om du skapar en A-post för en webbapp i Azure hello en post måste uppdateras manuellt om hello underliggande IP-adress för hello web app-ändringar.

## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar måste du först skapa en DNS-zon i Azure DNS och delegera hello zonen i din registrator tooAzure DNS.

1. toocreate en DNS-zon gör hello i [skapa en DNS-zon](dns-getstarted-create-dnszone.md).
2. toodelegate din DNS-tooAzure DNS, så hello i [DNS-delegering i domänen](dns-domain-delegation.md).

När du skapar en zon och delegera den tooAzure DNS kan skapar du sedan poster för domänen.

## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. Skapa en A-post för den anpassade domänen

En A-post är används toomap namn tooits IP-adress. I följande exempel hello tilldelas vi som en A-post tooan IPv4-adress:

### <a name="step-1"></a>Steg 1

Skapa en A-post och tilldela tooa variabeln $rs

```powershell
$rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600
```

### <a name="step-2"></a>Steg 2

Lägg till hello IPv4 toohello tidigare skapade poster värdet ”@” Hej $rs används som tilldelats. hello IPv4-värdet som tilldelas blir hello IP-adressen för din webbapp.

toofind hello IP-adressen för en webbapp gör hello i [konfigurera ett anpassat domännamn i Azure App Service](../app-service-web/app-service-web-tutorial-custom-domain.md).

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address "<your web app IP address>"
```

### <a name="step-3"></a>Steg 3

Genomför ändringar hello toohello postuppsättning. Använd `Set-AzureRMDnsRecordSet` tooupload hello ändras toohello postuppsättning tooAzure DNS:

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. Skapa en CNAME-post för den anpassade domänen

Om din domän hanteras redan av Azure DNS (se [DNS-delegering i domänen](dns-domain-delegation.md), kan du använda följande hello exempel toocreate en CNAME-post för contoso.azurewebsites.net hello.

### <a name="step-1"></a>Steg 1

Öppna PowerShell och skapa en ny CNAME-postuppsättning och tilldelar tooa variabeln $rs. Det här exemplet skapar en postuppsättning typen CNAME med en ”toolive” 600 sekunder i DNS-zonen med namnet ”contoso.com”.

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600
```

följande exempel hello är hello svar.

```
Name              : www
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a>Steg 2

När du har skapat hello CNAME-postuppsättning måste toocreate ett Aliasvärde som pekar toohello webbprogram.

Med hjälp av hello som tidigare tilldelats variabeln ”$rs” kan du använda hello PowerShell-kommandot nedan toocreate hello-alias för hello web app contoso.azurewebsites.net.

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"
```

följande exempel hello är hello svar.

```
    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a>Steg 3

Genomför hello ändringar med hjälp av hello `Set-AzureRMDnsRecordSet` cmdlet:

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

Du kan validera hello posten skapades korrekt genom att fråga hello ”www.contoso.com” med nslookup, enligt nedan:

```
PS C:\> nslookup
Default Server:  Default
Address:  192.168.0.1

> www.contoso.com
Server:  default server
Address:  192.168.0.1

Non-authoritative answer:
Name:    <instance of web app service>.cloudapp.net
Address:  <ip of web app service>
Aliases:  www.contoso.com
contoso.azurewebsites.net
<instance of web app service>.vip.azurewebsites.windows.net
```

## <a name="create-an-awverify-record-for-web-apps"></a>Skapa en ”awverify” post för webbprogram

Om du väljer toouse en A-post för ditt webbprogram, måste du gå via en verifiering processen tooensure du egna hello anpassade domäner. Den här kontrollen görs genom att skapa en särskild CNAME-post med namnet ”awverify”. Det här avsnittet gäller enbart tooA poster.

### <a name="step-1"></a>Steg 1

Skapa hello ”awverify” post. I hello exemplet nedan skapar vi hello ”aweverify” post för contoso.com tooverify ägarskap för hello anpassade domäner.

```powershell
$rs = New-AzureRMDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "myresourcegroup" -Name "awverify" -RecordType "CNAME" -Ttl 600
```

följande exempel hello är hello svar.

```
Name              : awverify
ZoneName          : contoso.com
ResourceGroupName : myresourcegroup
Ttl               : 600
Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
RecordType        : CNAME
Records           : {}
Tags              : {}
```

### <a name="step-2"></a>Steg 2

När hello postuppsättning ”awverify” har skapats kan du tilldela hello CNAME-postuppsättning alias. I hello exemplet nedan, kommer vi tilldela hello CNAMe postuppsättning alias tooawverify.contoso.azurewebsites.net.

```powershell
Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"
```

följande exempel hello är hello svar.

```
    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}
```

### <a name="step-3"></a>Steg 3

Genomför hello ändringar med hjälp av hello `Set-AzureRMDnsRecordSet cmdlet`som visas i hello kommandot nedan.

```powershell
Set-AzureRMDnsRecordSet -RecordSet $rs
```

## <a name="next-steps"></a>Nästa steg

Gör så hello i [konfigurera ett anpassat domännamn för Apptjänst](../app-service-web/web-sites-custom-domain-name.md) tooconfigure din web app toouse anpassade domäner.
