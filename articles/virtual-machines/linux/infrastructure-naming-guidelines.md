---
title: Azure-infrastrukturen naming riktlinjer - Linux | Microsoft Docs
description: "Läs mer om viktiga design och implementeringslösning riktlinjer för namngivning av i Azure infrastrukturtjänster."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 18ee4fb1-8297-49a1-8d3f-097880be67c7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1b086f0972c02d569a7219820a3d596960b6014b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-infrastructure-naming-guidelines-for-linux-vms"></a>Azure-infrastrukturen namngivningen riktlinjer för virtuella Linux-datorer 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på att förstå hur du hanterar namnkonventionerna för alla olika Azure-resurser att skapa en logisk och lätt att identifiera uppsättning resurser i din miljö.

## <a name="implementation-guidelines-for-naming-conventions"></a>Implementeringsriktlinjer för namngivningskonventioner
Beslut:

* Vilka är dina namnkonventionerna för Azure-resurser?

Aktiviteter:

* Definiera affix att använda i dina resurser för att upprätthålla enhetliga.
* Definiera lagringskontonamn angivna krav att vara globalt unika.
* Dokumentera namngivningskonvention ska användas och distribueras till alla de parter som ingår för att garantera konsekvens över distributioner.

## <a name="naming-conventions"></a>Namngivningskonventioner
Du bör ha en god namngivningskonvention på plats innan du skapar något i Azure. En namngivningskonvention garanterar att alla resurser som har en förutsägbar namn, som hjälper dig att minska det administrativa arbetet hantering av dessa resurser.

Du kan välja att följa en specifik uppsättning namnkonventioner som definierats för hela organisationen eller för en viss Azure-prenumeration eller ett konto. Även om det är enkelt för enskilda användare i organisationer att fastställa implicit regler när du arbetar med Azure-resurser som du behöver kunna skala för grupper som arbetar i Azure.

Komma överens om en uppsättning namnkonventioner direkt. Det finns några överväganden om namngivningskonventioner som täcker som anger regler.

## <a name="affixes"></a>Utför
När du visar om du vill definiera en namngivningskonvention är ett beslut om Affixet är på:

* I början av namn (prefix)
* I slutet av namn (suffix)

Här är till exempel två möjliga namn för en resursgrupp med hjälp av den `rg` fästa:

* Rg WebApp (prefix)
* WebApp-Rg (suffix)

Affix kan referera till olika aspekter som beskriver resurserna. I följande tabell visas några exempel som vanligtvis används.

| Aspekt | Exempel | Anteckningar |
|:--- |:--- |:--- |
| Miljö |Dev-, stg, prod |Beroende på syftet med och namnet på varje miljö. |
| Plats |usw (USA, västra), Använd (östra USA 2) |Beroende på region i datacenter eller region för organisationen. |
| Azure-komponent, tjänst eller produkt |Rg för resursgrupp, virtuella nätverk för virtuella nätverk |Beroende på den produkt som resursen ger stöd. |
| Roll |DB-app, web |Beroende på vilken roll för den virtuella datorn. |
| Instans |01, 02, 03, osv. |Resurser som har mer än en instans. Till exempel belastningsutjämnade servrar i en molntjänst. |

När din namngivningskonventioner, kontrollera att de klart vilka affix ska användas för varje typ av resursen och vilken plats (prefix vs suffix).

## <a name="dates"></a>datum
Ofta är det viktigt att fastställa skapandedatum från namnet på en resurs. Vi rekommenderar ÅÅÅÅMMDD datumformat. Detta format garanterar som är inte bara hela datumet registreras, men också att två resurser vars namn skiljer sig bara sorteras alfabetiskt och kronologiskt.

## <a name="naming-resources"></a>Namngivning av resurser
Definiera varje typ av resurs i namngivningskonvention som ska ha regler som definierar hur du tilldelar namn till varje resurs som har skapats. Dessa regler tillämpas för alla typer av resurser, till exempel:

* Prenumerationer
* Konton
* Lagringskonton
* Virtuella nätverk
* Undernät
* Tillgänglighetsuppsättningar
* Resursgrupper
* Virtuella datorer
* Slutpunkter
* Nätverkssäkerhetsgrupper
* Roller

Ger tillräckligt med information för att avgöra vilken resurs som det refererar bör du använda beskrivande namn för att se till att namnet.

## <a name="computer-names"></a>Datornamn
När du skapar en virtuell dator (VM), kräver Azure ett VM-namn med upp till 64 tecken som ska användas för resursnamnet. Azure använder samma namn för operativsystemet installerat på den virtuella datorn. Men kan dessa namn inte alltid densamma.

Om en virtuell dator skapas från en avbildning VHD-fil som innehåller redan ett operativsystem på det Virtuella datornamnet i Azure kan skilja sig från datornamnet för den Virtuella datorns operativsystem. Den här situationen kan lägga till en viss svårt att VM-hantering som därför inte rekommenderas. Tilldela den Virtuella Azure-resursen med samma namn som datornamnet som du tilldelar till operativsystemet på den virtuella datorn.

Vi rekommenderar att Azure VM-namn är samma som det underliggande operativsystemet datornamnet.

## <a name="storage-account-names"></a>Lagringskontonamn
Det här avsnittet gäller inte för [Azure hanterade diskar](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)eftersom du inte skapar ett separat lagringskonto. Storage-konton har särskilda regler för deras namn för ohanterade diskar. Du kan endast använda gemena bokstäver och siffror. Mer information finns i [skapa ett lagringskonto](../../storage/storage-create-storage-account.md#create-a-storage-account). Lagringskontonamnet med core.windows.net, bör dessutom vara ett giltigt GUID, unikt DNS-namn. Exempelvis om lagringskontot anropas mittlagringskonto, måste följande resulterande DNS-namn vara unika:

* mystorageaccount.BLOB.Core.Windows.NET
* mystorageaccount.Table.Core.Windows.NET
* mystorageaccount.Queue.Core.Windows.NET

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

