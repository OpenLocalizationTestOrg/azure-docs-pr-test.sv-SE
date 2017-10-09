---
title: aaaAzure infrastruktur naming riktlinjer - Linux | Microsoft Docs
description: "Läs mer om hello viktiga design och implementeringslösning riktlinjer för namngivning i Azure infrastrukturtjänster."
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
ms.openlocfilehash: 333146e7b2071e43527a5d7dc2ec02ebfb316eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-infrastructure-naming-guidelines-for-linux-vms"></a>Azure-infrastrukturen namngivningen riktlinjer för virtuella Linux-datorer 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Den här artikeln fokuserar på att förstå hur tooapproach namnkonventionerna för alla dina olika Azure-resurser toobuild en logisk och lätt att identifiera en uppsättning resurser i din miljö.

## <a name="implementation-guidelines-for-naming-conventions"></a>Implementeringsriktlinjer för namngivningskonventioner
Beslut:

* Vilka är dina namnkonventionerna för Azure-resurser?

Aktiviteter:

* Definiera hello affix toouse över dina resurser toomaintain konsekvenskontroll.
* Definiera namn tilldelat hello krav för dem toobe globalt unik storage-konto.
* Dokumentet hello naming convention toobe används och distribuera tooall parter ingår tooensure konsekvens mellan distributioner.

## <a name="naming-conventions"></a>Namngivningskonventioner
Du bör ha en god namngivningskonvention på plats innan du skapar något i Azure. En namngivningskonvention garanterar att alla hello resurser har en förutsägbar namn, vilket gör det lägre hello-administrativa belastningen hantering av dessa resurser.

Du kan välja toofollow en specifik uppsättning namnkonventioner som definierats för hela organisationen eller för en viss Azure-prenumeration eller ett konto. Men det är enkelt för enskilda användare i organisationer tooestablish implicit regler när du arbetar med Azure-resurser, behöver du toobe kan tooscale för grupper som arbetar i Azure.

Komma överens om en uppsättning namnkonventioner direkt. Det finns några överväganden om namngivningskonventioner som täcker som anger regler.

## <a name="affixes"></a>Utför
När du visar toodefine en namngivningskonvention är ett beslut om hello affix är på:

* hello namn börjar på hello (prefix)
* hello slutet av hello namn (suffix)

Här är till exempel två möjliga namn för en resursgrupp med hello `rg` fästa:

* Rg WebApp (prefix)
* WebApp-Rg (suffix)

Affix kan hänvisa toodifferent aspekter som beskriver hello resurser. hello visas nedan några exempel som vanligtvis används.

| Aspekt | Exempel | Anteckningar |
|:--- |:--- |:--- |
| Miljö |Dev-, stg, prod |Beroende på hello ändamål och namnet på varje miljö. |
| Plats |usw (USA, västra), Använd (östra USA 2) |Beroende på hello region för hello datacenter eller hello region hello organisation. |
| Azure-komponent, tjänst eller produkt |Rg för resursgrupp, virtuella nätverk för virtuella nätverk |Beroende på hello produkt för vilka hello resursen stöder. |
| Roll |DB-app, web |Beroende på hello roll för hello virtuell dator. |
| Instans |01, 02, 03, osv. |Resurser som har mer än en instans. Till exempel belastningsutjämnade servrar i en molntjänst. |

När din namngivningskonventioner, se till att de klart som utför toouse för varje typ av resurs och i vilket läge (prefix vs suffix).

## <a name="dates"></a>datum
Det är ofta viktiga toodetermine hello datum av hello namn skapas av en resurs. Vi rekommenderar hello ÅÅÅÅMMDD datumformat. Det här formatet blir inte bara hello fullständig datumet registreras, men också att två resurser vars namn skiljer sig bara på hello datum sorteras alfabetiskt och kronologiskt.

## <a name="naming-resources"></a>Namngivning av resurser
Definiera varje typ av resurs i hello namngivningskonvention, som bör ha regler som definierar hur tooassign namn tooeach resurs som har skapats. Dessa regler tillämpas tooall typer av resurser, till exempel:

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

tooensure som hello namn innehåller tillräckligt med information toodetermine toowhich resurs som det refererar bör du använda deskriptiva namn.

## <a name="computer-names"></a>Datornamn
När du skapar en virtuell dator (VM), kräver Azure en VM namnet på upp too64 tecken som används för hello resursnamnet. Azure använder hello samma namn för hello operativsystem i hello VM. Men kan dessa namn inte alltid vara hello samma.

Om en virtuell dator skapas från en avbildning VHD-fil som innehåller ett operativsystem redan hello namn på virtuell dator i Azure kan skilja sig från hello datornamn för Virtuella datorer med operativsystemet. Den här situationen kan lägga till en viss svårt tooVM management som därför inte rekommenderas. Tilldela hello Azure VM resurs hello samma namn som hello datornamn som du tilldelar toohello operativsystemet på den virtuella datorn.

Vi rekommenderar att hello Azure VM-namn är hello samma som hello underliggande operativsystemet datornamn.

## <a name="storage-account-names"></a>Lagringskontonamn
Det här avsnittet gäller inte för[Azure hanterade diskar](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)eftersom du inte skapar ett separat lagringskonto. Storage-konton har särskilda regler för deras namn för ohanterade diskar. Du kan endast använda gemena bokstäver och siffror. Mer information finns i [skapa ett lagringskonto](../../storage/storage-create-storage-account.md#create-a-storage-account). Hej lagringskontonamn, med core.windows.net, bör dessutom vara ett giltigt GUID, unikt DNS-namn. Till exempel om hello lagringskonto anropas mittlagringskonto, hello följande resulterande DNS-namn måste vara unika:

* mystorageaccount.BLOB.Core.Windows.NET
* mystorageaccount.Table.Core.Windows.NET
* mystorageaccount.Queue.Core.Windows.NET

## <a name="next-steps"></a>Nästa steg
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

