---
title: "aaaLoad belastningsutjämnaren anpassade avsökningar och övervakning hälsostatus | Microsoft Docs"
description: "Lär dig hur toouse anpassade avsökningar för Azure belastningsutjämnare toomonitor instanser bakom belastningsutjämnaren"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3dfcfcd2d5cffa58b160cb38d63acffbd997d452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-load-balancer-probes"></a>Förstå avsökningar av belastningsutjämnare

Azure belastningsutjämnare erbjuder hello kapaciteten toomonitor hello hälsotillståndet för server-instanser med avsökningar. När en avsökning misslyckas toorespond belastningsutjämnaren slutar att skicka nya anslutningar toohello ohälsosamt instansen. hello befintliga anslutningar påverkas inte och nya anslutningar skickas toohealthy instanser.

Molntjänstroller (arbetsroller och webbroller) kan du använda gästagent för avsökning övervakning. Anpassade TCP eller HTTP-avsökningar måste konfigureras när du använder virtuella datorer bakom belastningsutjämnaren.

## <a name="understand-probe-count-and-timeout"></a>Förstå avsökningen antal och timeout

Avsökningen beteende beror på:

* hello antalet lyckade avsökningar som gör att en instans toobe etiketter som upp.
* hello antal misslyckade avsökningar som orsakar en instans toobe etiketter som inaktiv.

hello timeout och frekvens värdet i SuccessFailCount avgöra om en instans är bekräftade toobe körs eller inte körs. I hello Azure-portalen, är hello timeout värdet tootwo gånger hello hello frekvens.

hello avsökningen konfigurationen av alla instanser för belastningsutjämning för en slutpunkt (det vill säga en belastningsutjämnad uppsättning) måste vara hello samma. Det innebär att du inte kan ha en annan avsökningen konfiguration för varje instans av serverroll eller virtuell dator i hello samma värdtjänsten för en viss slutpunkt-kombination. Varje instans måste till exempel ha identiska lokala portar och timeout.

> [!IMPORTANT]
> En belastningsutjämnare avsökning använder hello IP-adressen 168.63.129.16. Den här offentliga IP-adressen underlättar kommunikationen toointernal plattform resurser för hello bring-your-äger-IP-scenariot Azure virtuella. hello virtuella offentliga IP-adressen 168.63.129.16 används i alla regioner och ändras inte. Vi rekommenderar att du IP-adressen i alla lokala Brandväggsprinciper. Det bör inte anses en säkerhetsrisk eftersom endast hello intern Azure-plattformen kan styra ett meddelande från den adressen. Om du inte gör detta är oväntade resultat i en mängd olika scenarier som konfigurerar hello samma IP-adressintervall för 168.63.129.16 och har dubblerats IP-adresser.

## <a name="learn-about-hello-types-of-probes"></a>Lär dig mer om hello typer av avsökningar

### <a name="guest-agent-probe"></a>Gästen agent avsökning

Den här avsökningen är endast tillgängligt för Azure-molntjänster. Belastningsutjämnare använder hello gästagenten i hello virtuella datorn, lyssnar och svarar med ett HTTP-200 OK svar endast när hello-instansen är i tillståndet för hello Ready (det vill säga inte i ett annat tillstånd, till exempel upptagen återvinning eller stoppar).

Mer information finns i [konfigurera hello tjänstdefinitionsfilen (csdef) för hälsoavsökningar](https://msdn.microsoft.com/library/azure/ee758710.aspx) eller [komma igång med en Internetriktade belastningsutjämnare för molntjänster](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Vad gör en gäst agent avsökning Markera en instans som ohälsosamt?

Om hello gästagenten misslyckas toorespond med HTTP 200 OK hello hello belastningen belastningsutjämnaren märken instans som inte svarar och stoppar skickar trafik toothat instans. hello belastningsutjämnaren fortsätter tooping hello-instans. Om hello gästagenten svarar med en HTTP-200, skickar hello belastningsutjämnaren trafik toothat instans igen.

När du använder en webbroll körs hello webbplats koden i w3wp.exe som inte övervakas av hello Azure-strukturen eller gästagenten. Detta innebär att fel i w3wp.exe (till exempel HTTP 500-svar) inte är rapporterat toohello gästagenten och hello belastningsutjämnaren träder inte i den instansen av rotationen.

### <a name="http-custom-probe"></a>Anpassade HTTP-avsökningen

hello anpassade HTTP-belastningsutjämnaren avsökningen åsidosätter hello standard gäst agent avsökningen, vilket innebär att du kan skapa egna anpassade logik toodetermine hello hälsotillstånd hello rollinstans. hello belastningsutjämnaren avsökningar slutpunkten var 15: e sekund som standard. hello instans anses toobe i hello belastningen belastningsutjämnaren rotation om svarar den med en HTTP-200 inom tidsgränsen för hello (31 sekunder som standard).

Detta kan vara användbart om du vill tooimplement egna logik tooremove instanser från belastningen belastningsutjämnaren rotation. Du kan till exempel bestämma tooremove en instans om det är mer än 90% CPU och returnerar status-200. Om du har web-roller som använder w3wp.exe detta betyder också att du får automatisk övervakning av din webbplats, eftersom fel i webbplats koden returnerar en icke-200 status toohello belastningsutjämningsavsökning.

> [!NOTE]
> anpassade hello HTTP-avsökningen stöder relativa sökvägar och HTTP-protokollet. HTTPS stöds inte.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Vad gör en anpassad HTTP-avsökningen Markera en instans som ohälsosamt?

* hello http-programmet returnerar ett HTTP-svarskoden än 200 (till exempel 403, 404 och 500). Det här är en positiv bekräftelse som programmet hello instans ska tas bort från tjänsten direkt.
* hello HTTP-servern svarar inte på alla efter hello tidsgränsen. Beroende på hello timeout-värde som anges detta kan innebära att flera avsökningen begär gå obehandlade innan hello avsökningen hämtar markerad som inte körs (det vill säga innan SuccessFailCount avsökningar skickas).
* hello servern stängs hello anslutning via en TCP-återställning.

### <a name="tcp-custom-probe"></a>Anpassade TCP-avsökning

TCP-avsökningar initiera en anslutning genom att utföra en trevägs handskakning med hello definierats port.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Vad gör en anpassad TCP-avsökning Markera en instans som ohälsosamt?

* hello TCP-servern svarar inte på alla efter hello tidsgränsen. När hello avsökning har markerats som inte kör beror på hello antal misslyckade avsökningen begäranden som har konfigurerade toogo obehandlade innan du markerar hello avsökningen som inte körs.
* hello avsökningen tar emot en TCP Återställ från hälsningspaket rollinstansen.

Mer information om hur du konfigurerar en HTTP-hälsoavsökningen eller en TCP-avsökning finns [komma igång med att skapa en Internetriktade belastningsutjämnare i Resource Manager med hjälp av PowerShell](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Lägga till Felfri instanser till belastningen belastningsutjämnaren rotation

TCP- och HTTP-avsökningar betraktas felfritt och markera hello rollinstans som felfri när:

* hello belastningsutjämnaren hämtar en positivt avsökningen hello första gången hello VM startar.
* hello talet SuccessFailCount (beskrivs ovan) anger hello värdet för lyckad avsökningar som är nödvändiga toomark hello rollinstans som felfritt. Om en rollinstans har tagits bort, måste hello antal lyckades, efterföljande avsökningar lika med eller överstiger hello värdet för SuccessFailCount toomark hello-rollinstans som körs.

> [!NOTE]
> Om hello hälsotillståndet för en rollinstans fluktuerar, väntar hello belastningsutjämnaren längre innan du sätter hälsningspaket rollinstansen tillbaka i hello felfritt tillstånd. Detta görs via principen tooprotect hello användar- och hello infrastruktur.

## <a name="use-log-analytics-for-load-balancer"></a>Använd logganalys för belastningsutjämnare

Du kan använda [logga analytics för belastningsutjämnaren](load-balancer-monitor-log.md) toocheck hello avsökningen hälsa status och avsökning antal. Loggning kan användas med Power BI eller Azure-åtgärdsinformationen tooprovide statistik om hälsostatus för belastningsutjämnaren.
