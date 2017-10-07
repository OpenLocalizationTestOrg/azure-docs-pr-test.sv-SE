---
title: "aaaWhat är en åtkomstkontrollista i Azure-nätverk?"
description: "Lär dig mer om åtkomstkontrollistor i Azure"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 83d66c84-8f6b-4388-8767-cd2de3e72d76
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: a2681af035d162362771e6df9864bbe838f16352
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-an-endpoint-access-control-list"></a>Vad är en slutpunkt för åtkomstkontrollista?

> [!IMPORTANT]
> Azure har två olika [distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) för att skapa och arbeta med resurser: Resource Manager och klassisk. Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. 

En slutpunkt åtkomstkontrollista (ACL) är en säkerhetsförbättring av för din Azure-distribution. En ACL ger hello möjlighet tooselectively bevilja eller neka trafik för en virtuell dator-slutpunkten. Funktionen paket filtrering ger ett ytterligare lager av säkerhet. Du kan ange nätverk ACL: er för endast slutpunkter. Du kan inte ange en ACL för ett virtuellt nätverk eller ett specifikt undernät som ingår i ett virtuellt nätverk. Det rekommenderas toouse nätverkssäkerhetsgrupper (NSG: er) i stället för ACL: er när det är möjligt. toolearn mer information om NSG: er, se [nätverk Säkerhetsöversikt grupp](virtual-networks-nsg.md)

ACL: er kan konfigureras med hjälp av PowerShell eller hello Azure-portalen. tooconfigure ett ACL-nätverk med hjälp av PowerShell, se [hantera åtkomstkontrollistor för slutpunkter med hjälp av PowerShell](virtual-networks-acl-powershell.md). tooconfigure ett ACL-nätverk med hjälp av hello Azure portal, se [hur tooset slutpunkter tooa virtuella datorn](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Med nätverks-ACL: er kan du göra hello följande:

* Tillåta eller neka inkommande trafik baserat på fjärrundernät IPv4-adressintervallet tooa virtuella slutpunkten för indata.
* Svartlistat IP-adresser
* Skapa flera regler per virtuell datorslutpunkt
* Använda regeln ordning tooensure hello rätt uppsättning regler tillämpas på en slutpunkt för den angivna virtuella datorn (lägsta toohighest)
* Ange en ACL för en specifik fjärrundernät IPv4-adress.

Se hello [Azure begränsar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) artikel för ACL-gränser.

## <a name="how-acls-work"></a>Så här fungerar ACL: er
En ACL är ett objekt som innehåller en lista över regler. När du skapar en ACL och tillämpa den tooa virtuell datorslutpunkt sker filtrering av nätverkspaket på hello värdnoden på den virtuella datorn. Det innebär att hello trafik från fjärranslutna IP-adresser filtreras efter hello värdnoden för matchande ACL-regler i stället för på den virtuella datorn. Detta förhindrar att den virtuella datorn utgifter hello värdefull CPU-cykler för filtrering av nätverkspaket.

När en virtuell dator skapas placeras standard ACL i plats tooblock all inkommande trafik. Men om en slutpunkt har skapats för (port 3389), sedan hello standard ACL är ändrade tooallow all inkommande trafik för denna slutpunkt. Inkommande trafik från alla fjärrundernät tillåts sedan toothat slutpunkt och ingen brandvägg etablering krävs. Alla portar blockeras för inkommande trafik såvida inte slutpunkter har skapats för dessa portar. Utgående trafik tillåts som standard.

**Standard ACL exempeltabell**

| **Regel #** | **Fjärranslutet undernät** | **Slutpunkt** | **Tillåt/neka** |
| --- | --- | --- | --- |
| 100 |0.0.0.0/0 |3389 |Tillåt |

## <a name="permit-and-deny"></a>Tillåt och neka
Du kan selektivt tillåter eller nekar nätverkstrafik för en virtuell dator indataslutpunkten genom att skapa regler som anger ”Tillåt” eller ”neka”. Det är viktigt toonote som standard när en slutpunkt skapas all trafik tillåts toohello slutpunkt. Därför är det viktigt toounderstand hur toocreate tillstånd/neka regler och placera dem i hello rätt rangordning om du vill granulär kontroll över hello nätverk trafik som du väljer tooallow tooreach hello virtuell datorslutpunkt.

Punkter tooconsider:

1. **Inga ACL –** som standard när en slutpunkt som vi bevilja alla för hello slutpunkt.
2. **Tillåt -** när du lägger till en eller flera ”Tillåt” intervall du neka alla intervall som standard. Endast paket från hello tillåtna IP-intervall kommer att kunna toocommunicate med hello virtuell datorslutpunkt.
3. **Neka -** när du lägger till en eller flera ”neka” adressintervall tillåter du alla andra områden av trafik som standard.
4. **Kombinationen av Tillåt och neka -** kan du använda en kombination av ”tillstånd” och ”neka” om du vill toocarve ut en specifik IP-adress vara toobe beviljas eller nekas.

## <a name="rules-and-rule-precedence"></a>Regler och regeln företräde
ACL: er för nätverket kan ställas in på en specifik virtuell dator slutpunkter. Du kan till exempel ange ett nätverk ACL för en RDP-slutpunkt skapas på en virtuell dator som låser ned åtkomst för vissa IP-adresser. hello tabellen nedan visar ett sätt toogrant åtkomst toopublic virtuella IP-adresser (VIP) i en viss intervallet toopermit åtkomstbehörighet för RDP. Alla andra fjärranslutna IP-adresser att nekas. Vi följer en *lägsta företräde* regel ordning.

### <a name="multiple-rules"></a>Flera regler
I hello exemplet nedan om du vill tooallow åtkomst toohello RDP-slutpunkt endast från två offentliga IPv4-adressintervall (65.0.0.0/8 och 159.0.0.0/8), du kan åstadkomma detta genom att ange två *tillåter* regler. I det här fallet eftersom RDP skapas som standard för en virtuell dator, kanske vill du toolock ned toohello RDP åtkomstport baserat på ett fjärranslutet undernät. hello exemplet nedan visar ett sätt toogrant åtkomst toopublic virtuella IP-adresser (VIP) i en viss intervallet toopermit åtkomstbehörighet för RDP. Alla andra fjärranslutna IP-adresser att nekas. Detta fungerar eftersom nätverket ACL: er kan ställas in för en specifik virtuell dator-slutpunkten och åtkomst nekas som standard.

**Exempel – flera regler**

| **Regel #** | **Fjärranslutet undernät** | **Slutpunkt** | **Tillåt/neka** |
| --- | --- | --- | --- |
| 100 |65.0.0.0/8 |3389 |Tillåt |
| 200 |159.0.0.0/8 |3389 |Tillåt |

### <a name="rule-order"></a>Regeln ordning
Eftersom flera regler kan anges för en slutpunkt, måste det finnas en sätt tooorganize regler i ordning toodetermine som regeln företräde. hello regeln ordning anger företräde. Nätverk ACL: er Följ en *lägsta företräde* regel ordning. I hello exemplet nedan beviljas selektivt hello slutpunkt på port 80 åtkomst tooonly vissa IP-adressintervall. tooconfigure, har vi en neka-regel (regel \# 100) för adresser i hello 175.1.0.1/24 utrymme. En andra regel anges sedan med prioritet 200 att tillåter åtkomst till tooall andra adresser under 175.0.0.0/8.

**Exempel – prioritet för regeln**

| **Regel #** | **Fjärranslutet undernät** | **Slutpunkt** | **Tillåt/neka** |
| --- | --- | --- | --- |
| 100 |175.1.0.1/24 |80 |Neka |
| 200 |175.0.0.0/8 |80 |Tillåt |

## <a name="network-acls-and-load-balanced-sets"></a>Nätverks-ACL: er och läsa in belastningsutjämnade uppsättningar
ACL: er för nätverket kan anges för en belastningsutjämnad uppsättning slutpunkt. Om du anger en ACL för en belastningsutjämnad uppsättning, är hello nätverket ACL tillämpade tooall virtuella datorer i den belastningsutjämnade uppsättningen. Till exempel om en belastningsutjämnad uppsättning skapas med ”Port 80” och hello belastningsutjämnade uppsättningen innehåller 3 virtuella datorer hello nätverket ACL som skapats på slutpunkten ”Port 80” på en virtuell dator kommer automatiskt gäller toohello andra virtuella datorer.

![Nätverks-ACL: er och läsa in belastningsutjämnade uppsättningar](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Nästa steg
[Hantera åtkomstkontrollistor för slutpunkter med hjälp av PowerShell](virtual-networks-acl-powershell.md)

