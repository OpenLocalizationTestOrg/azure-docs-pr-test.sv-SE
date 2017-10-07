---
title: "problem med aaaConnectivity och nätverk för Microsoft Azure Cloud Services FAQ | Microsoft Docs"
description: "Den här artikeln innehåller vanliga frågor om anslutning och nätverk för Microsoft Azure Cloud Services hello."
services: cloud-services
documentationcenter: 
author: genlin
manager: cshepard
editor: 
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: genli
ms.openlocfilehash: e725dbbf585a76807362c59299d0a31f511afd3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connectivity-and-networking-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Problem med anslutningen och nätverk för Azure Cloud Services: vanliga frågor (FAQ)

Den här artikeln innehåller vanliga frågor om problem med anslutningen och nätverk för [Microsoft Azure Cloud Services](https://azure.microsoft.com/services/cloud-services). Du kan också läsa hello [Cloud Services VM-storlek sidan](cloud-services-sizes-specs.md) storlek information.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Det går inte att reservera en IP-adress i en multi-VIP-molntjänst
Kontrollera först att den virtuella dator hello-instansen som du försöker tooreserve hello IP för är aktiverad. Därefter kontrollerar du att du använder reserverade IP-adresser för både hello mellanlagring och produktion distributioner. **Inte** ändra hello inställningar medan hello distribution uppgraderas.

## <a name="how-do-i-remote-desktop-when-i-have-an-nsg"></a>Hur gör jag Fjärrskrivbord när jag har en NSG?
Lägg till regler toohello NSG som tillåter trafik på portarna **3389** och **20000**.  Fjärrskrivbord använder port **3389**.  Molnet tjänstinstanser belastningsutjämnas, så du inte kan styra vilken instans tooconnect till direkt.  Hej *RemoteForwarder* och *RemoteAccess* agenter hantera RDP-trafik och Tillåt hello klienten toosend en RDP-cookie och ange en enskild instans tooconnect till.  Hej *RemoteForwarder* och *RemoteAccess* agenter kräver att port **20000** öppnas, där blockeras om du har en NSG.

## <a name="can-i-ping-a-cloud-service"></a>Kan jag pinga en tjänst i molnet?

Nej, inte genom att använda hello normal ”pinga” / ICMP-protokollet. hello ICMP-protokollet är inte tillåtet via hello Azure belastningsutjämnare.

tootest anslutning, rekommenderar vi att du gör en port-ping. Medan Ping.exe använder ICMP, kan andra verktyg, till exempel PSPing, Nmap och telnet tootest-anslutningen tooa viss TCP-port.

Mer information finns i [använder port pingar i stället för ICMP-tootest Virtuella Azure-anslutningen](https://blogs.msdn.microsoft.com/mast/2014/06/22/use-port-pings-instead-of-icmp-to-test-azure-vm-connectivity/).

## <a name="how-do-i-prevent-receiving-thousands-of-hits-from-unknown-ip-addresses-that-indicate-some-sort-of-malicious-attack-toohello-cloud-service"></a>Hur förhindrar mottagning tusentals träffar från okända IP-adresser som indikerar någon form av skadlig kod toohello Molntjänsten?
Azure implementerar en multilayer network security tooprotect dess plattformstjänster mot distribuerade denial of service (DDoS)-attacker. hello Azure DDoS defense system är en del av Azures kontinuerlig övervakning processen, som kontinuerligt förbättras genom intrång tester. Den här DDoS-skydd är utformat toowithstand inte bara attacker från hello utanför utan också från andra Azure-klienter. Mer information finns i [nätverkssäkerhet för Microsoft Azure](http://download.microsoft.com/download/C/A/3/CA3FC5C0-ECE0-4F87-BF4B-D74064A00846/AzureNetworkSecurity_v3_Feb2015.pdf).

Du kan också skapa ett startade uppgiften tooselectively block vissa specifika IP-adresser. Mer information finns i [blockera en specifik IP-adress](cloud-services-startup-tasks-common.md#block-a-specific-ip-address).

## <a name="when-i-try-toordp-toomy-cloud-service-instance-i-get-hello-message-hello-user-account-has-expired"></a>När jag försöker tooRDP toomy cloud service-instans hello-meddelande, ”Hej användarkontot har upphört att gälla”.
Du kan få hello felmeddelandet ”det här användarkontot har upphört att gälla” när du kringgå hello förfallodatum som konfigurerats i RDP-inställningar. Du kan ändra hello upphör att gälla från hello-portalen genom att följa dessa steg:
1. Logga in toohello Azure-hanteringskonsolen (https://manage.windowsazure.com), navigera tooyour Molntjänsten och välj hello **konfigurera** fliken.
2. Välj **Remote**.
3. Ändra hello ”upphör att gälla”-datum och sedan spara hello konfiguration.

Nu bör du kunna tooRDP tooyour datorn.

## <a name="why-is-loadbalancer-not-balancing-traffic-equally"></a>Varför är LoadBalancer inte NLB trafiken jämnt?
Information om hur interna load balancer fungerar finns i [Azure belastningsutjämnare nya distribution läget](https://azure.microsoft.com/blog/azure-load-balancer-new-distribution-mode/).

hello distribution algoritm som används är 5-tuppel (käll-IP, källport, mål-IP, målport protokolltyp) hash-toomap trafik tooavailable servrar. Det ger varaktighet endast inom en transportsession. Paket i hello samma TCP eller UDP-sessionen kommer att vara riktad toohello-instans samma datacenter IP (DIP) bakom hello belastningsutjämnade slutpunkt. När hello klient stänger och öppnar nytt hello-anslutning eller startar en ny session från hello samma käll-IP, hello källport ändras och gör hello trafik toogo tooa annan DIP slutpunkt.
