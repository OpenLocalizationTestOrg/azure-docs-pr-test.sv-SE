---
title: "aaaVerify Azure Traffic Manager-inställningar | Microsoft Docs"
description: "Den här artikeln hjälper dig att kontrollera din Traffic Manager-inställningar"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 2180b640-596e-4fb2-be59-23a38d606d12
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
ms.openlocfilehash: c670be6cf55e140c7ab63d5d526de08e14774d2a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="verify-traffic-manager-settings"></a>Kontrollera inställningarna för Traffic Manager

tootest din Traffic Manager-inställningar du behöver toohave flera klienter på olika platser, från vilken du kör dina tester. Sedan sätta hello slutpunkter i Traffic Manager-profilen ned en i taget.

* Ange hello DNS TTL-värdet låg så att ändringar sprida snabbt (till exempel 30 sekunder).
* Vet hello IP-adresserna för dina Azure-molntjänster och webbplatser i hello-profil som du vill testa.
* Använd verktyg som kan matcha DNS-namn tooan IP-adress och visa den adressen.

Du kontrollerar toosee hello DNS-namn att matcha tooIP adresser hello slutpunkter i din profil. hello namn bör lösas i enlighet med hello trafikroutningsmetod definieras i hello Traffic Manager-profilen. Du kan använda hello verktyg som **nslookup** eller **gräva** tooresolve DNS-namn.

hello kan följande exempel du testa Traffic Manager-profilen.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Kontrollera Traffic Manager-profil med nslookup och ipconfig i Windows

1. Öppna ett kommando eller en Windows PowerShell-Kommandotolken som administratör.
2. Typen `ipconfig /flushdns` tooflush hello cacheminnet för DNS-matchare.
3. Skriv `nslookup <your Traffic Manager domain name>`. Hej exempelvis följande kommando kontrollerar hello domännamn med hello prefix *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    En typisk resultatet visar hello följande information:

    + hello DNS-namn och IP-adressen för hello DNS-server som åt tooresolve det här domännamnet för Traffic Manager.
    + hello Traffic Manager-domännamn du angav på kommandoraden för hello efter ”nslookup” och löser hello IP-adress toowhich hello Traffic Manager-domän. hello andra IP-adressen är hello viktiga en toocheck. Den måste matcha en offentlig virtuella IP-adresser (VIP)-adress för en av hello molntjänster eller webbplatser i hello Traffic Manager-profil som du vill testa.

## <a name="how-tootest-hello-failover-traffic-routing-method"></a>Hur tootest hello redundans trafikroutningsmetoden

1. Lämna alla slutpunkter upp.
2. Med en enda klient begär DNS-matchning för företagets domännamn med hjälp av nslookup eller en liknande funktion.
3. Se till att hello matcha IP-adressen matchar hello primära slutpunkt.
4. Avslutar primära slutpunkten eller ta bort hello övervakning filen så att Traffic Manager tror som hello programmet inte är tillgänglig.
5. Vänta tills hello DNS-Time-to-Live (TTL) av hello Traffic Manager-profil plus en ytterligare två minuter. Till exempel måste din DNS-TTL är 300 sekunder (fem minuter), du vänta tills sju minuter.
6. Rensa din DNS-klienten cache och begäran DNS-matchning med nslookup. I Windows kan tömma du DNS-cache med hello ipconfig/flushdns kommando.
7. Se till att hello matcha IP-adressen matchar din sekundära slutpunkt.
8. Upprepa hello-processen, som i sin tur att stänga av varje slutpunkt. Kontrollera att hello DNS returnerar hello IP-adressen för nästa hello-slutpunkt i hello lista. När alla slutpunkter är nere, bör du hämta hello IP-adressen för primär slutpunkt för hello igen.

## <a name="how-tootest-hello-weighted-traffic-routing-method"></a>Hur tootest hello viktas trafikroutningsmetod

1. Lämna alla slutpunkter upp.
2. Med en enda klient begär DNS-matchning för företagets domännamn med hjälp av nslookup eller en liknande funktion.
3. Se till att hello matcha IP-adressen matchar ett av dina slutpunkter.
4. Rensa din DNS-klientens cacheminne och upprepa steg 2 och 3 för varje slutpunkt. Du bör se olika IP-adresser som returneras för var och en av dina slutpunkter.

## <a name="how-tootest-hello-performance-traffic-routing-method"></a>Hur tootest hello prestanda trafikroutningsmetoden

tooeffectively testa en routningsmetoden för prestanda-trafik, måste du ha klienter som finns i olika delar av hello world. Du kan skapa klienter i olika Azure-regioner som kan använda tootest dina tjänster. Om du har ett globalt nätverk kan du via fjärranslutning logga in tooclients i andra delar av hello world och köra dina tester därifrån.

Du kan också finns kostnadsfria webbaserade DNS-sökning och gräva tjänster som är tillgängliga. Några av dessa verktyg ger du hello möjlighet toocheck DNS-namnmatchningen från olika platser runt hälsningsmeddelande. Gör en sökning efter ”DNS-sökning” exempel. Tjänster från tredje part som Gomez eller presentation kan vara används tooconfirm att dina profiler distribuerar trafik som förväntat.

## <a name="next-steps"></a>Nästa steg

* [Om Traffic Manager-trafikroutningsmetoder](traffic-manager-routing-methods.md)
* [Prestandaöverväganden för Traffic Manager](traffic-manager-performance-considerations.md)
* [Felsök degraderat tillstånd i Traffic Manager](traffic-manager-troubleshooting-degraded.md)
