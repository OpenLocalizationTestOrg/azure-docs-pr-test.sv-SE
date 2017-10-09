---
title: "aaaGet igång med Azure DNS med hjälp av PowerShell | Microsoft Docs"
description: "Lär dig hur toocreate ett DNS-zonen och posten i Azure DNS. Detta är en stegvis guide toocreate och hantera din första DNS-zonen och posten med hjälp av PowerShell."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 0f9dead1e4b44fcc74c84a024c41cdfaeb02b5d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-powershell"></a>Komma igång med Azure DNS med PowerShell

> [!div class="op_single_selector"]
> * [Azure Portal](dns-getstarted-portal.md)
> * [PowerShell](dns-getstarted-powershell.md)
> * [Azure CLI 1.0](dns-getstarted-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-cli.md)

Den här artikeln vägleder dig genom hello steg toocreate din första DNS-zon och en post med hjälp av Azure PowerShell. Du kan också utföra dessa steg med hello Azure-portalen eller hello plattformsoberoende Azure CLI.

En DNS-zon är används toohost hello DNS-poster för en viss domän. toostart som värd för din domän i Azure DNS, behöver du toocreate en DNS-zon för domännamnet. Varje DNS-post för din domän skapas sedan i den här DNS-zonen. Slutligen toopublish DNS-zonen toohello Internet, behöver du tooconfigure hello namnservrar för hello domän. Dessa steg beskrivs nedan.

Dessa instruktioner förutsätter att du redan har installerat och inloggad tooAzure PowerShell. Mer information finns [hur toomanage DNS zoner med hjälp av PowerShell](dns-operations-dnszones.md).

## <a name="create-hello-resource-group"></a>Skapa hello resursgrupp

Innan du skapar hello DNS-zonen skapas en resursgrupp toocontain hello DNS-zon. hello följande visar hello-kommando.

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a>Skapa en DNS-zon

En DNS-zon skapas med hjälp av hello `New-AzureRmDnsZone` cmdlet. hello följande exempel skapas en DNS-zon som kallas *contoso.com* i hello resursgrupp med namnet *MyResourceGroup*. Använd hello exempel toocreate en DNS-zon ersätter hello värden för din egen.

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a>Skapa en DNS-post

Du skapar postuppsättningar med hello `New-AzureRmDnsRecordSet` cmdlet. hello följande exempel skapas en post med hello relativa namnet ”www” i hello DNS-zonen ”contoso.com” i resursgruppen ”MyResourceGroup”. hello fullständigt kvalificerade namnet på postuppsättningen hello är ”www.contoso.com”. hello-posttypen är ”A”, med IP-adress ”1.2.3.4” och hello TTL-värde är 3 600 sekunder.

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

För andra posttyper postuppsättningar med fler än en post och toomodify befintliga poster finns [hantera DNS-poster och postuppsättningar med hjälp av Azure PowerShell](dns-operations-recordsets.md). 


## <a name="view-records"></a>Visa poster

toolist hello DNS-poster i zonen, Använd:

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a>Uppdatera namnservrar

När du är nöjd att din DNS-zonen och poster har ställts in korrekt behöver tooconfigure ditt domännamn toouse hello Azure DNS-namnservrar. Detta gör att andra användare i hello Internet toofind DNS-poster.

hello namnservrar för zonen ges av hello `Get-AzureRmDnsZone` cmdlet:

```powershell
Get-AzureRmDnsZone -ZoneName contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

Dessa namnservrar ska konfigureras med hello domännamnsregistratorn (där du har köpt hello domännamn). Din registrator kommer att erbjuda hello alternativet tooset in hello namnservrar för hello domän. Mer information finns i [Delegera din domän tooAzure DNS](dns-domain-delegation.md).

## <a name="delete-all-resources"></a>Ta bort alla resurser

toodelete alla resurser skapas i den här artikeln tar hello följande steg:

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a>Nästa steg

toolearn mer om Azure DNS finns [översikt över Azure DNS](dns-overview.md).

toolearn mer information om hur du hanterar DNS-zoner i Azure DNS finns [hantera DNS-zoner i Azure DNS med hjälp av PowerShell](dns-operations-dnszones.md).

toolearn mer information om hur du hanterar DNS-poster i Azure DNS finns [hantera DNS-poster och registrera anger i Azure DNS med hjälp av PowerShell](dns-operations-recordsets.md).

