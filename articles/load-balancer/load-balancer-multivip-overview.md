---
title: "aaaMultiple VIP för belastningsutjämnaren i Azure | Microsoft Docs"
description: "Översikt över flera VIP: er på Azure belastningsutjämnare"
services: load-balancer
documentationcenter: na
author: chkuhtz
manager: narayan
editor: 
ms.assetid: 748e50cd-3087-4c2e-a9e1-ac0ecce4f869
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/11/2016
ms.author: chkuhtz
ms.openlocfilehash: e186e1248e7d187e7efd0668dc3244465ce3e156
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="multiple-vips-for-azure-load-balancer"></a>Flera virtuella ip-adresser för Azure Load Balancer

Azure belastningsutjämnare kan utjämna tooload tjänster på flera portar, flera IP-adresser eller båda. Du kan använda belastning offentliga och interna belastningsutjämnare definitioner tooload saldo flödar över en uppsättning virtuella datorer.

Den här artikeln beskriver hello grunderna i den här möjligheten, viktiga begrepp och begränsningar. Om du avser endast tooexpose tjänster en IP-adress, kan du hitta förenklad instruktioner för [offentliga](load-balancer-get-started-internet-portal.md) eller [interna](load-balancer-get-started-ilb-arm-portal.md) läsa in belastningsutjämning konfigurationer. Lägga till flera VIP: er är inkrementell tooa enskild VIP-konfiguration. Du kan expandera en förenklad konfiguration när som helst med hello begrepp i den här artikeln.

När du definierar en Azure belastningsutjämnare ansluten en frontend och backend-konfiguration med regler. Hej hälsoavsökningen som hello regeln är används toodetermine hur nya flöden skickas tooa nod i hello serverdelspool. hello klientdel definieras av en virtuell IP (VIP), vilket är ett 3-tuppel som består av en IP-adress (offentlig eller intern), en transportprotokollet (UDP eller TCP) och ett portnummer. En DIP är en IP-adress på en virtuell NIC som Azure ansluten tooa VM i hello serverdelspool.

hello följande tabell innehåller några exempel klientdel konfigurationer:

| VIP | IP-adress | Protokollet | port |
| --- | --- | --- | --- |
| 1 |65.52.0.1 |TCP |80 |
| 2 |65.52.0.1 |TCP |*8080* |
| 3 |65.52.0.1 |*UDP* |80 |
| 4 |*65.52.0.2* |TCP |80 |

hello tabell visar fyra olika frontends. Frontends #1, #2 och #3 är en enskild VIP med flera regler. hello samma IP-adress används men hello port eller protokoll är olika för varje klientdel. Frontends #1 och #4 är ett exempel på flera VIP: er, där hello samma frontend-protokoll och port återanvänds över flera VIP: er.

Azure belastningsutjämnare ger flexibilitet för att definiera hello regler för belastningsutjämning. En regel förklarar hur en adress och port på hello klientdel är mappade toohello mål-adress och port på hello backend. Huruvida backend portar återanvänds över regler beror på hello typ av hello regel. Varje regel har specifika krav som kan påverka konfiguration och avsökning design av värd. Det finns två typer av regler:

1. återanvända hello standardregel med ingen backend-port
2. hello flytande IP regeln där återanvänds backend-portar

Azure belastningsutjämnare kan du toomix både regeltyper på hello samma konfiguration av belastningsutjämning. hello belastningsutjämnare kan använda dem samtidigt för en viss virtuell dator eller en kombination, så länge du följer hello begränsningarna för hello regeln. Vilken typ av du väljer beror på hello kraven för dina program och hello komplexitet stöder denna konfiguration. Du bör utvärdera vilka regeltyper lämpar sig bäst för ditt scenario.

Vi utforska dessa scenarier ytterligare genom att börja med hello standardbeteendet.

## <a name="rule-type-1-no-backend-port-reuse"></a>Regeltyp #1: ingen backend port återanvändning

![MultiVIP-bild](./media/load-balancer-multivip-overview/load-balancer-multivip.png)

I det här scenariot hello klientdel VIP konfigureras enligt följande:

| VIP | IP-adress | Protokollet | port |
| --- | --- | --- | --- |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

hello DIP är hello målet hello inkommande flödet. Varje virtuell dator visar hello desired-tjänsten på en unik port på en DIP i hello serverdelspool. Den här tjänsten är associerad med hello klientdel via en regeldefinition av.

Vi definierar två regler:

| Regel | Mappa klientdel | toobackend pool |
| --- | --- | --- |
| 1 |![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 |![backend](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP1:80, ![backend](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) DIP2:80 |
| 2 |![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 |![backend](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP1:81, ![backend](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) DIP2:81 |

hello fullständig mappning i Azure belastningsutjämnare är nu på följande sätt:

| Regel | VIP-IP-adress | Protokollet | port | Mål | port |
| --- | --- | --- | --- | --- | --- |
| ![Regeln](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |DIP IP-adress |80 |
| ![Regeln](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |DIP IP-adress |81 |

Varje regel måste generera ett flöde med en unik kombination av IP-adress och målport. Genom att variera hello målporten hello flödet, kan flera regler flöden toohello leverera samma DIP på olika portar.

Hälsoavsökningar är alltid dirigerad toohello DIP av en virtuell dator. Du måste se till att du att din avsökningen återspeglar hello hälsotillstånd hello VM.

## <a name="rule-type-2-backend-port-reuse-by-using-floating-ip"></a>Regeltyp #2: återanvändning av backend-port med hjälp av flytande IP

Azure belastningsutjämnare ger hello flexibilitet tooreuse hello klientdelsport över flera VIP: er oavsett hello regeltyp används. Dessutom kan vissa Programscenarier föredrar eller kräver hello samma port toobe som används av flera programinstanser på en enda virtuell dator i serverdelspoolen hello. Vanliga exempel port återanvändning av omfattar kluster för hög tillgänglighet, virtuella installationer och exponera flera TLS slutpunkter utan omkryptering.

Om du vill tooreuse hello serverdelsport över flera regler måste du aktivera flytande IP i hello Regeldefinitionen.

Flytande IP är en del av känt som direkt Server returnera (DSR). DSR består av två delar: en flödet topologi och en IP-mappning schemat. På en plattformsnivå körs alltid Azure belastningsutjämnare i en topologi för flödet av DSR oavsett om flytande IP är aktiverat eller inte. Det innebär att hello utgående en del av ett flöde alltid korrekt omskrivet tooflow direkt tillbaka toohello ursprung.

Med regeltypen för hello standard exponerar Azure en traditionell IP-adress mappning schema för användarvänlighet för belastningsutjämning. Aktivera flytande IP ändrar hello adressmappning schemat tooallow för ytterligare flexibilitet som beskrivs nedan.

hello följande diagram visar den här konfigurationen:

![MultiVIP-bild](./media/load-balancer-multivip-overview/load-balancer-multivip-dsr.png)

I det här scenariot har varje virtuell dator i serverdelspoolen hello tre nätverksgränssnitt:

* DIP: ett virtuellt nätverkskort som är associerade med hello VM (IP-konfiguration av Azures NIC resurs)
* VIP1: en loopback-gränssnittet i gästoperativsystemet som är konfigurerade med IP-adressen för VIP1
* VIP2: en loopback-gränssnittet i gästoperativsystemet som är konfigurerade med IP-adressen för VIP2

> [!IMPORTANT]
> hello konfigurationen av hello logiska gränssnitt utförs inom hello gästoperativsystemet. Den här konfigurationen är inte utföras eller som hanteras av Azure. Utan den här konfigurationen fungerar inte hello regler. Hälsotillstånd avsökningen definitioner använda hello DIP av hello VM i stället för hello logiska VIP. Din tjänst måste därför ange avsökningen svar på en DIP-port som visar hello status för hello-tjänst som erbjuds för hello logiska VIP.

Vi antar hello samma frontend-konfiguration som tidigare hello:

| VIP | IP-adress | Protokollet | port |
| --- | --- | --- | --- |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |*65.52.0.2* |TCP |80 |

Vi definierar två regler:

| Regel | Mappa klientdel | toobackend pool |
| --- | --- | --- |
| 1 |![Regeln](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 |![backend](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) VIP1:80 (i VM1 och VM2) |
| 2 |![Regeln](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 |![backend](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) VIP2:80 (i VM1 och VM2) |

hello följande tabell visar hello fullständig mappning i hello belastningsutjämnare:

| Regel | VIP-IP-adress | Protokollet | port | Mål | port |
| --- | --- | --- | --- | --- | --- |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-green.png) 1 |65.52.0.1 |TCP |80 |samma som VIP (65.52.0.1) |samma som VIP (80) |
| ![VIP](./media/load-balancer-multivip-overview/load-balancer-rule-purple.png) 2 |65.52.0.2 |TCP |80 |samma som VIP (65.52.0.2) |samma som VIP (80) |

hello mål av hello inkommande flödet är hello VIP-adressen på hello loopback-gränssnittet i hello VM. Varje regel måste generera ett flöde med en unik kombination av IP-adress och målport. Genom att variera hello målets IP-adress i hello flöde, port återanvändning är möjligt på hello samma virtuella dator. Tjänsten är utsatta toohello belastningsutjämnaren genom att binda den toohello VIPS IP-adressen och porten för hello respektive loopback-gränssnitt.

Observera att det här exemplet inte ändrar hello målport. Även om det är ett flytande IP-scenario stöder Azure belastningsutjämnare också definiera ett mål för regel toorewrite hello serverdelsport och toomake den skiljer sig från hello frontend-målport.

hello regeltyp flytande IP är hello grunden för flera belastningsutjämnare configuration belastningsmönster. Ett exempel som är tillgänglig för närvarande är hello [SQL AlwaysOn med flera lyssnare](../virtual-machines/windows/sql/virtual-machines-windows-portal-sql-ps-alwayson-int-listener.md) konfiguration. Över tiden, kommer vi dokumentera flera av dessa scenarier.

## <a name="limitations"></a>Begränsningar

* Konfigurationer med flera VIP stöds bara med IaaS-VM.
* Programmet måste använda hello DIP för utgående flöden med hello flytande IP-regel. Om programmet Binder toohello VIP-adressen konfigurerad på hello loopback-gränssnittet i hello gäst-OS, sedan SNAT är inte tillgängliga toorewrite hello utgående flödet och hello flödet misslyckas.
* Offentliga IP-adresser påverka faktureringen. Mer information finns i [priser för IP-adress](https://azure.microsoft.com/pricing/details/ip-addresses/)
* Prenumerationsbegränsningar gäller. Mer information finns i [gränser](../azure-subscription-service-limits.md#networking-limits) mer information.
