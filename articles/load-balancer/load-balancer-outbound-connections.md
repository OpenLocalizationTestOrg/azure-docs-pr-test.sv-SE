---
title: "aaaUnderstanding utgående anslutningar i Azure | Microsoft Docs"
description: "Den här artikeln förklarar hur Azure kan användas för virtuella datorer toocommunicate med offentliga Internet-tjänster."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 5/31/2017
ms.author: kumud
ms.openlocfilehash: ffe59971b934a16c9f6f78e3b15869a0072320d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-outbound-connections-in-azure"></a>Förstå utgående anslutningar i Azure

En virtuell dator (VM) i Azure kan kommunicera med slutpunkter utanför Azure i offentliga IP-adressutrymme. När en virtuell dator initierar ett utgående flödet tooa mål i offentliga IP-adressutrymme, Azure mappar hello VM privat IP-adress tooa offentliga IP-adress och kan returnera trafik tooreach hello VM.

Azure tillhandahåller tre olika metoder för tooachieve utgående anslutning. Varje metod har egna funktioner och begränsningar. Granska varje metod noggrant toochoose vilket som uppfyller dina behov.

| Scenario | Metod | Obs! |
| --- | --- | --- |
| Standalone VM (någon belastningsutjämnare, ingen offentlig IP på instansnivå adress) |Standard SNAT |Azure associerar en offentlig IP-adress för SNAT |
| Utjämning av nätverksbelastning VM (ingen offentlig IP på instansnivå adress på den virtuella datorn) |SNAT använda hello belastningsutjämnare |Azure använder en offentlig IP-adressen för hello belastningsutjämnare för SNAT |
| Virtuell dator med offentlig IP på instansnivå adress (med eller utan belastningsutjämnare) |SNAT används inte |Azure använder hello offentlig IP-adress tilldelas toohello VM |

Du kan använda Nätverkssäkerhetsgrupp grupper (NSG) tooblock åtkomst om du inte vill att en VM-toocommunicate med slutpunkter utanför Azure i offentliga IP-adressutrymme. Med hjälp av NSG: er beskrivs i detalj i [förhindrar anslutning för offentliga](#preventing-public-connectivity).

## <a name="standalone-vm-with-no-instance-level-public-ip-address"></a>Standalone VM med ingen offentlig IP på instansnivå adress

I det här scenariot hello VM ingår inte i en pool med Azure belastningsutjämnare och har inte en instans nivå offentliga IP-går-adress som tilldelats tooit. När hello VM skapar ett utgående flöde, översätter Azure hello privata IP-källadress av hello utgående flödet tooa offentliga IP-adress. hello offentlig IP-adress som används för detta utgående flöde kan inte konfigureras och räknas inte mot hello prenumeration offentliga IP-resursgräns. Azure använder källa Network Address Translation (SNAT) tooperform denna funktion. Tillfälliga portar för hello offentliga IP-adressen är används toodistinguish enskilda flöden som härrör från hello VM. SNAT tilldelas tillfälliga portar när flöden skapas. I den här kontexten är hello tillfälliga portar som används för SNAT refererad tooas SNAT portar.

Det finns en begränsad resurs som kan vara förbrukat SNAT portar. Det är viktigt toounderstand hur de används. En SNAT port används per flöde tooa enda målets IP-adress. För flera flöden toohello samma IP-måladress, varje flöde förbrukar en enda SNAT port. Detta säkerställer att hello flöden är unik när kommer från hello samma offentliga IP-adress toohello samma mål-IP-adress. Flera flöden varje tooa olika IP-måladress använda en enda SNAT port per mål. hello mål-IP-adress gör hello flöden unikt.

Du kan använda [Log Analytics för belastningsutjämnaren](load-balancer-monitor-log.md) och [avisering händelseloggar toomonitor för SNAT port uttömning meddelanden](load-balancer-monitor-log.md#alert-event-log). När SNAT port resurserna är uttömda misslyckas utgående flöden tills SNAT portar släpps av befintliga flöden. Belastningsutjämnare använder en 4-minuters timeout för inaktivitet för återtagning SNAT portar.

## <a name="load-balanced-vm-with-no-instance-level-public-ip-address"></a>Utjämning av nätverksbelastning virtuell dator med ingen offentlig IP på instansnivå adress

I det här scenariot ingår hello VM i en pool med Azure belastningsutjämnare.  hello VM saknar en offentlig IP-adress som tilldelats tooit. hello belastningsutjämnaren resursen måste konfigureras med en regel toolink hello offentliga IP-klientdel med hello serverdelspool.  Om du inte slutföra den här konfigurationen, hello beteendet är enligt beskrivningen i hello ovan för [Standalone VM med ingen offentlig IP på instansnivå](load-balancer-outbound-connections.md#standalone-vm-with-no-instance-level-public-ip-address).

När hello belastningsutjämnade VM skapar ett utgående flöde, översätter Azure hello privata IP-källadress hello utgående flödet toohello offentliga IP-adressen för hello offentliga belastningsutjämnare klientdel. Azure använder källa Network Address Translation (SNAT) tooperform denna funktion. Tillfälliga portar för hello belastningsutjämnaren offentliga IP-adressen är används toodistinguish enskilda flöden som härrör från hello VM. SNAT tilldelas tillfälliga portar när utgående flöden skapas. I den här kontexten är hello tillfälliga portar som används för SNAT refererad tooas SNAT portar.

Det finns en begränsad resurs som kan vara förbrukat SNAT portar. Det är viktigt toounderstand hur de används. En SNAT port används per flöde tooa enda målets IP-adress. För flera flöden toohello samma IP-måladress, varje flöde förbrukar en enda SNAT port. Detta säkerställer att hello flöden är unik när kommer från hello samma offentliga IP-adress toohello samma mål-IP-adress. Flera flöden varje tooa olika IP-måladress använda en enda SNAT port per mål. hello mål-IP-adress gör hello flöden unikt.

Du kan använda [Log Analytics för belastningsutjämnaren](load-balancer-monitor-log.md) och [avisering händelseloggar toomonitor för SNAT port uttömning meddelanden](load-balancer-monitor-log.md#alert-event-log). När SNAT port resurserna är uttömda misslyckas utgående flöden tills SNAT portar släpps av befintliga flöden. Belastningsutjämnare använder en 4-minuters timeout för inaktivitet för återtagning SNAT portar.

## <a name="vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer"></a>Virtuell dator med en offentlig IP på instansnivå adress (med eller utan belastningsutjämnare)

I det här scenariot har hello VM en instans nivå offentliga IP-går tilldelade tooit. Det spelar ingen roll om hello VM är belastningsutjämnad eller inte. Källan Network Address Translation (SNAT) används inte när en går används. hello VM använder hello går för alla utgående flöden. Om ditt program initierar många utgående flöden och SNAT resursöverbelastning uppstår, bör du tilldela en går tooavoid SNAT begränsningar.

## <a name="discovering-hello-public-ip-used-by-a-given-vm"></a>Identifiera hello offentliga IP-Adressen används av en viss virtuell dator

Det finns många sätt toodetermine hello offentliga källans IP-adress för en utgående anslutning. OpenDNS är en tjänst som kan du visa hello offentliga IP-adressen för den virtuella datorn. Med hello nslookup-kommando kan skicka du en DNS-fråga för hello namnet myip.opendns.com toohello OpenDNS lösare. hello källans IP-adress som har använt toosend hello frågan returnerar hello-tjänsten. När du kör hello följande fråga från den virtuella datorn, är hello svaret hello offentlig IP-adress som används för den virtuella datorn.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-public-connectivity"></a>Förhindrar en offentlig anslutning

Ibland är det oönskade för en VM-toobe tillåts toocreate ett utgående flöde eller det kan finnas en krav toomanage som mål kan nås med utgående flöden. I så fall måste du använda [Nätverkssäkerhetsgrupp grupper (NSG)](../virtual-network/virtual-networks-nsg.md) toomanage hello mål som hello VM kan nå. När du tillämpar en NSG tooa belastningsutjämnade virtuella datorn, måste du toopay uppmärksamhet toohello [standard taggar](../virtual-network/virtual-networks-nsg.md#default-tags) och [standard regler](../virtual-network/virtual-networks-nsg.md#default-rules).

Du måste se till att hello VM kan ta emot hälsa avsökningen begäranden från Azure belastningsutjämnare. Om en NSG block hälsa avsökningen begäranden från hello AZURE_LOADBALANCER Standardtaggen, hälsoavsökningen din VM misslyckas och hello VM markeras. Belastningsutjämnaren slutar att skicka nya flöden toothat VM.

## <a name="limitations"></a>Begränsningar

Om [flera (offentliga) IP-adresser som är kopplade till en belastningsutjämnare](load-balancer-multivip-overview.md), någon av dessa offentliga IP-adresser är en kandidat för utgående flöden.

Azure använder en algoritm toodetermine hello antalet SNAT tillgängliga portar baserat på hello storleken på hello poolen.  Detta kan inte konfigureras just nu.

Det är viktigt toorememember som hello antalet tillgängliga portar för SNAT inte översätter direkt toonumber anslutningar. Finns ovanför för närmare information om när och hur tilldelas SNAT portar och toomanage icke förnybara resursen.
