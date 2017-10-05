---
title: "Förstå utgående anslutningar i Azure | Microsoft Docs"
description: "Den här artikeln förklarar hur Azure kan användas för virtuella datorer att kommunicera med offentliga Internet-tjänster."
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
ms.openlocfilehash: 03cb14b5710b6dd17599a3c4eab21380c76c2b40
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="understanding-outbound-connections-in-azure"></a>Förstå utgående anslutningar i Azure

En virtuell dator (VM) i Azure kan kommunicera med slutpunkter utanför Azure i offentliga IP-adressutrymme. När en virtuell dator initierar ett utgående flöde till ett mål i offentliga IP-adressutrymme, Azure mappar privata IP-adressen för den virtuella datorn till en offentlig IP-adress och kan returnera trafik till den virtuella datorn.

Azure tillhandahåller tre olika metoder för att uppnå utgående anslutning. Varje metod har egna funktioner och begränsningar. Granska varje metod noggrant om du vill välja vilket som uppfyller dina behov.

| Scenario | Metod | Obs! |
| --- | --- | --- |
| Standalone VM (någon belastningsutjämnare, ingen offentlig IP på instansnivå adress) |Standard SNAT |Azure associerar en offentlig IP-adress för SNAT |
| Utjämning av nätverksbelastning VM (ingen offentlig IP på instansnivå adress på den virtuella datorn) |SNAT med belastningsutjämnaren |Azure använder en offentlig IP-adress i belastningsutjämnaren för SNAT |
| Virtuell dator med offentlig IP på instansnivå adress (med eller utan belastningsutjämnare) |SNAT används inte |Azure använder offentlig IP-adress som tilldelats den virtuella datorn |

Du kan använda Nätverkssäkerhetsgrupp grupper (NSG) för att blockera åtkomst om du inte vill att en virtuell dator för att kommunicera med slutpunkter utanför Azure i offentliga IP-adressutrymme. Med hjälp av NSG: er beskrivs i detalj i [förhindrar anslutning för offentliga](#preventing-public-connectivity).

## <a name="standalone-vm-with-no-instance-level-public-ip-address"></a>Standalone VM med ingen offentlig IP på instansnivå adress

I det här scenariot, den virtuella datorn är inte en del av en pool med Azure belastningsutjämnare och har inte en instans nivå offentliga IP-går-adress som tilldelats till den. När den virtuella datorn skapar ett utgående flöde, översätter Azure privata källans IP-adress för utgående begränsas till en offentlig IP-källadress. Den offentliga IP-adressen som används för detta utgående flöde kan inte konfigureras och räknas inte mot prenumerationens offentliga IP-resursgräns. Azure använder källa Network Address Translation (SNAT) för att utföra den här funktionen. Tillfälliga portar för den offentliga IP-adressen används för att särskilja enskilda flöden av den virtuella datorn har sitt ursprung. SNAT tilldelas tillfälliga portar när flöden skapas. I den här kontexten kallas tillfälliga portarna som används för SNAT SNAT portar.

Det finns en begränsad resurs som kan vara förbrukat SNAT portar. Det är viktigt att förstå hur de används. En SNAT port används per flöde till en enda mål-IP-adress. Varje flöde förbrukar en enskild port SNAT för flera flöden med samma mål-IP-adress. Detta säkerställer att flöden är unika när kommer från samma offentliga IP-adress till samma IP-adressen för målet. Flera flöden, till en annan IP-måladress använder en enskild port SNAT per mål. Den IP-adressen gör flödena unikt.

Du kan använda [Log Analytics för belastningsutjämnaren](load-balancer-monitor-log.md) och [avisering händelseloggar för att övervaka SNAT port uttömning meddelanden](load-balancer-monitor-log.md#alert-event-log). När SNAT port resurserna är uttömda misslyckas utgående flöden tills SNAT portar släpps av befintliga flöden. Belastningsutjämnare använder en 4-minuters timeout för inaktivitet för återtagning SNAT portar.

## <a name="load-balanced-vm-with-no-instance-level-public-ip-address"></a>Utjämning av nätverksbelastning virtuell dator med ingen offentlig IP på instansnivå adress

I det här fallet är den virtuella datorn tillhör en pool med Azure belastningsutjämnare.  Den virtuella datorn har inte en offentlig IP-adress som tilldelats. Resursen belastningsutjämnaren måste konfigureras med en regel för att länka den offentliga IP-klientdelen med serverdelspoolen.  Om du inte slutföra den här konfigurationen, beteendet är enligt beskrivningen i avsnittet ovan för [Standalone VM med ingen offentlig IP på instansnivå](load-balancer-outbound-connections.md#standalone-vm-with-no-instance-level-public-ip-address).

När den belastningsutjämnade virtuella datorn skapar ett utgående flöde, översätter Azure privata källans IP-adress för utgående offentliga belastningsutjämnare klientdel offentliga IP-adressen. Azure använder källa Network Address Translation (SNAT) för att utföra den här funktionen. Tillfälliga portar för Belastningsutjämnarens offentliga IP-adressen används för att särskilja enskilda flöden av den virtuella datorn har sitt ursprung. SNAT tilldelas tillfälliga portar när utgående flöden skapas. I den här kontexten kallas tillfälliga portarna som används för SNAT SNAT portar.

Det finns en begränsad resurs som kan vara förbrukat SNAT portar. Det är viktigt att förstå hur de används. En SNAT port används per flöde till en enda mål-IP-adress. Varje flöde förbrukar en enskild port SNAT för flera flöden med samma mål-IP-adress. Detta säkerställer att flöden är unika när kommer från samma offentliga IP-adress till samma IP-adressen för målet. Flera flöden, till en annan IP-måladress använder en enskild port SNAT per mål. Den IP-adressen gör flödena unikt.

Du kan använda [Log Analytics för belastningsutjämnaren](load-balancer-monitor-log.md) och [avisering händelseloggar för att övervaka SNAT port uttömning meddelanden](load-balancer-monitor-log.md#alert-event-log). När SNAT port resurserna är uttömda misslyckas utgående flöden tills SNAT portar släpps av befintliga flöden. Belastningsutjämnare använder en 4-minuters timeout för inaktivitet för återtagning SNAT portar.

## <a name="vm-with-an-instance-level-public-ip-address-with-or-without-load-balancer"></a>Virtuell dator med en offentlig IP på instansnivå adress (med eller utan belastningsutjämnare)

Den virtuella datorn har en instans nivå offentliga IP-går kopplade till den i det här scenariot. Det spelar ingen roll om den virtuella datorn är belastningsutjämnad eller inte. Källan Network Address Translation (SNAT) används inte när en går används. Den virtuella datorn använder går för alla utgående flöden. Om ditt program initierar många utgående flöden och SNAT resursöverbelastning uppstår, bör du tilldela en går att undvika SNAT begränsningar.

## <a name="discovering-the-public-ip-used-by-a-given-vm"></a>Identifiering av offentliga IP-Adressen som används av en viss virtuell dator

Det finns många sätt att avgöra den offentliga IP-källadressen för en utgående anslutning. OpenDNS är en tjänst som kan visa den offentliga IP-adressen på den virtuella datorn. Använda nslookup-kommando, kan du skicka en DNS-fråga för namnet myip.opendns.com till OpenDNS matcharen. Källans IP-adress som används för att skicka frågan returnerar tjänsten. När du kör följande fråga från den virtuella datorn är svaret offentliga IP-Adressen som används för den virtuella datorn.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-public-connectivity"></a>Förhindrar en offentlig anslutning

Ibland är den oönskade en virtuell dator för att kunna skapa ett utgående flöde eller det kan finnas ett krav för att hantera vilka destinationer kan nås med utgående flöden. I så fall måste du använda [Nätverkssäkerhetsgrupp grupper (NSG)](../virtual-network/virtual-networks-nsg.md) att hantera mål som den virtuella datorn kan nå. När du kopplar en NSG till en belastningsutjämnad VM, måste du vara uppmärksam på den [standard taggar](../virtual-network/virtual-networks-nsg.md#default-tags) och [standard regler](../virtual-network/virtual-networks-nsg.md#default-rules).

Du måste se till att den virtuella datorn kan ta emot hälsa avsökningen begäranden från Azure-belastningsutjämnaren. Om en NSG blockerar hälsa avsökningen begäranden från Standardetiketten AZURE_LOADBALANCER, din VM hälsoavsökningen misslyckas och den virtuella datorn markeras. Belastningsutjämnaren slutar att skicka nya flödar till den virtuella datorn.

## <a name="limitations"></a>Begränsningar

Om [flera (offentliga) IP-adresser som är kopplade till en belastningsutjämnare](load-balancer-multivip-overview.md), någon av dessa offentliga IP-adresser är en kandidat för utgående flöden.

Azure använder en algoritm för att bestämma antalet tillgängliga portar för SNAT baserat på storleken på poolen.  Detta kan inte konfigureras just nu.

Det är viktigt att rememember antalet tillgängliga portar för SNAT inte översätter direkt till antalet anslutningar. Närmare information om när och hur tilldelas SNAT portar och hur du hanterar icke förnybara resursen se ovan.
