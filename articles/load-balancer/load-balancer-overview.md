---
title: "aaaAzure belastningsutjämnaren översikt | Microsoft Docs"
description: "Översikt över Azure belastningsutjämnare funktioner, arkitektur och implementering. Lär dig hur hello belastningsutjämnaren fungerar och utnyttja i hello molnet."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
ms.assetid: 0f313dc0-f007-4cee-b2b9-f542357925a3
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 2376a02f7cbbbed6a90f216419c0c3d30f594272
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-load-balancer-overview"></a>Översikt över belastningsutjämnaren i Azure

Azure belastningsutjämnare ger hög tillgänglighet och nätverksprestanda tooyour program. Det är en belastningsutjämnare för lager 4 (TCP, UDP) som distribuerar inkommande trafik mellan felfri instanser av tjänster som definierats i en belastningsutjämnad uppsättning.

Azure belastningsutjämnare kan konfigureras för att:

* Läsa belastningsutjämna inkommande Internet-trafik toovirtual datorer. Den här konfigurationen kallas [mot Internet för belastningsutjämning](load-balancer-internet-overview.md).
* Läs in Utjämna trafiken mellan virtuella datorer i ett virtuellt nätverk, mellan virtuella datorer i molntjänster eller mellan lokala datorer och virtuella datorer i ett virtuellt nätverk mellan platser. Den här konfigurationen kallas [intern belastningsutjämning](load-balancer-internal-overview.md).
* Vidarebefordra externa trafiken tooa specifik virtuell dator.

Alla resurser i molnet hello måste en offentlig IP-adress toobe kan nås från hello Internet. hello moln-infrastruktur i Azure använder icke-dirigerbara IP-adresser för dess resurser. Azure använder network address translation (NAT) med offentliga IP-adresser toocommunicate toohello Internet.

## <a name="azure-deployment-models"></a>Azure distributionsmodeller

Det är viktigt toounderstand hello skillnaderna mellan hello Azure klassiska och Resource Manager [distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md). Azure belastningsutjämnare konfigureras på olika sätt i varje modell.

### <a name="azure-classic-deployment-model"></a>Azure klassiska distributionsmodellen

Virtuella datorer distribueras i ett moln-tjänsten kan vara grupperad toouse belastningsutjämning. I den här modellen är en offentlig IP-adress och ett fullständigt domännamn, (FQDN) som har tilldelats tooa Molntjänsten. hello belastningsutjämnaren port översättning och hello-nätverkstrafik om belastningsutjämning med hjälp av hello offentliga IP-adressen för hello-Molntjänsten.

Belastningsutjämnad trafik definieras av slutpunkter. Port översättning slutpunkter ha en 1: 1-relation mellan hello tilldelade offentlig port hello offentlig IP-adress och hello lokal port tilldelade toohello-tjänsten på en specifik virtuell dator. Läsa in belastningsutjämning slutpunkter ha en en-till-många-relation mellan hello offentlig IP-adress och hello lokala portar som tilldelats toohello tjänster på hello virtuella datorer i hello-Molntjänsten.

![Azure belastningsutjämnare i hello klassiska distributionsmodellen](./media/load-balancer-overview/asm-lb.png)

Bild 1 – Azure belastningsutjämnare i hello klassiska distributionsmodellen

hello domänetikett hello offentliga IP-adress som hello belastningen belastningsutjämnare använder för denna distributionsmodell är \<molntjänstnamnet\>. cloudapp.net. hello följande bild visar hello Azure belastningsutjämnare i den här modellen.

### <a name="azure-resource-manager-deployment-model"></a>Azure Resource Manager-distributionsmodellen

I hello Resource Manager-modellen har ingen måste toocreate en tjänst i molnet. hello belastningsutjämnaren skapas tooexplicitly vidarebefordra trafik mellan flera virtuella datorer.

En offentlig IP-adress är en enskild resurs som har en domänetikett (DNS-namn) för. hello offentliga IP-adressen är kopplad till hello belastningsutjämningsresurs. Regler för inläsning av belastningsutjämning och inkommande NAT-regler använder hello offentlig IP-adress som hello Internet-slutpunkt för hello resurser som tar emot belastningsutjämnad trafik.

En privat eller offentlig IP-adress har tilldelats toohello nätverk gränssnittet resurs kopplade tooa virtuella datorn. När ett nätverksgränssnitt har lagts till tooa belastningsutjämnaren backend-IP-adresspool, är hello belastningsutjämnare kan toosend belastningsutjämnad nätverkstrafik baserat på hello belastningsutjämnade regler som har skapats.

hello visar följande bild hello Azure belastningsutjämnare i den här modellen:

![Azure belastningsutjämnare i Resource Manager](./media/load-balancer-overview/arm-lb.png)

Bild 2 – Azure belastningsutjämnare i Resource Manager

hello belastningsutjämnare kan hanteras via Resource Manager-baserade mallar, API: er och verktyg. toolearn mer om hanteraren för filserverresurser, se hello [översikt över Resource Manager](../azure-resource-manager/resource-group-overview.md).

## <a name="load-balancer-features"></a>Läsa in belastningsutjämning funktioner

* Hash-baserad distribution

    Azure belastningsutjämnare använder en algoritm för hash-baserad distribution. Som standard använder en 5-tuppel-hash som består av käll-IP, källport, mål-IP, målport och protokollet typen toomap trafik tooavailable servrar. Det ger endast varaktighet *inom* en transportsession. Paket i hello samma TCP eller UDP-sessionen kommer att vara riktad toohello samma instans bakom hello belastningsutjämnade slutpunkt. När hello klient stänger och öppnar en ny anslutning hello eller startar en ny session från hello samma käll-IP, hello port ändringar av datakällan. Detta kan orsaka hello trafik toogo tooa annan slutpunkt i olika datacenter.

    Mer information finns i [belastningsdistributionsläget nätverksbelastningsutjämning](load-balancer-distribution-mode.md). hello visar följande bild hello hash-baserad distribution:

    ![Hash-baserad distribution](./media/load-balancer-overview/load-balancer-distribution.png)

    Bild 3 - hash-baserad distribution

* Vidarebefordrade portar

    Azure belastningsutjämnare ger dig kontroll över hur inkommande kommunikation hanteras. Detta meddelande innehåller trafik som initieras från Internet-värdar, virtuella datorer i andra molntjänster eller virtuella nätverk. Den här kontrollen representeras av en slutpunkt (kallas även en slutpunkt för indata).

    En slutpunkt för indata lyssnar på en offentlig port och vidarebefordrar trafik tooan Intern port. Du kan mappa hello samma portar för en intern eller extern slutpunkt eller Använd en annan port för dem. Du kan till exempel ha en web server konfigurerad toolisten tooport 81 medan hello offentlig slutpunkt mappningen är port 80. hello skapandet av en offentlig slutpunkt utlöser hello skapandet av en load belastningsutjämnaren instans.

    När skapas med hello Azure-portalen, skapas hello portal automatiskt slutpunkter toohello virtuell dator för hello Remote Desktop Protocol (RDP) och remote trafiken i Windows PowerShell-sessionen. Du kan använda dessa slutpunkter tooremotely administrera hello virtuella datorn över hello Internet.

* Automatisk omkonfiguration

    Azure belastningsutjämnare konfigurerar omedelbart om sig själv när du skalar delar upp eller ned. Omkonfigureringen inträffar exempelvis när du ökar hello instanser för web/worker-roller i en molntjänst eller när du lägger till ytterligare virtuella datorer i hello ange samma Utjämning av nätverksbelastning.

* Tjänstövervakning

    Azure belastningsutjämnare kan avsöka hello hälsotillstånd hello olika server-instanser. När en avsökning misslyckas toorespond stoppar hello belastningsutjämnaren skickar nya anslutningar toohello ohälsosamt instanser. Befintliga anslutningar påverkas inte.

    Tre typer av avsökningar stöds:

    + **Gästen agent avsökningen (på plattform som en tjänst endast virtuella datorer):** hello belastningsutjämnare använder hello gästagenten inuti hello virtuella datorn. Hej gästagenten lyssnar och svarar med ett HTTP-200 OK svar endast när hello-instansen är i tillståndet för hello ready (d.v.s. hello-instansen är inte i ett tillstånd som upptagen återvinning eller stoppar). Om hello agenten misslyckas toorespond med en HTTP-200 OK hello hello belastningen belastningsutjämnaren märken instans som inte svarar och stoppar skickar trafik toothat instans. hello belastningsutjämnaren fortsätter tooping hello-instans. Om hello gästagenten svarar med en HTTP-200, skickar hello belastningsutjämnaren trafik toothat instans igen. När du använder en webbroll körs webbplats koden i w3wp.exe som inte övervakas av hello Azure-strukturen eller gästagenten. Detta innebär att fel i w3wp.exe (t.ex. HTTP 500-svar) inte är rapporterat toohello gästagenten och hello belastningsutjämnaren inte vet tootake instansen utanför rotation.
    + **Anpassade HTTP-avsökningen:** den här avsökningen åsidosätter hello standard (gästagenten) avsökning. Du kan använda den toocreate egna anpassade logik toodetermine hello hälsotillstånd hello rollinstans. hello belastningsutjämnaren ska regelbundet söka slutpunkten (var 15 sekunder, som standard). hello instans anses toobe i rotation om den svarar med en ACK TCP eller HTTP 200 inom tidsgränsen för hello (standard 31 sekunder). Detta är användbart för att implementera din egen logik tooremove instanser från hello belastningsutjämnaren rotation. Du kan till exempel konfigurera hello instans tooreturn status-200 om hello-instans är över 90% CPU. Web-roller som använder w3wp.exe du också få automatisk övervakning av din webbplats, eftersom fel i koden webbplats returnera en icke-200 status toohello avsökning.
    + **Anpassade TCP-avsökning:** den här avsökningen förlitar sig på lyckad TCP session etablering tooa definierats avsökningsport.

    Mer information finns i hello [LoadBalancerProbe schemat](https://msdn.microsoft.com/library/azure/jj151530.aspx).

* Källan NAT

    All utgående trafik toohello Internet som kommer från din tjänst genomgår datakällan NAT (SNAT) med hjälp av hello samma VIP-adressen som hello inkommande trafik. SNAT ger viktiga fördelar:

    + Enkel uppgradering och katastrofåterställning återställning för tjänster, kan eftersom hello VIP kan vara dynamiskt mappad tooanother hello service-instans.
    + Det underlättar åtkomstkontrollistan (ACL) för åtkomstkontrollhantering. ACL: er som uttrycks i VIP ändras inte services skala upp, ner eller hämta omdistribueras.

    Hej belastningsutjämningskonfigurationen stöder fullständig kon NAT för UDP. Fullständig kon NAT är en typ av NAT-enhet där hello port tillåter inkommande anslutningar från en extern värd (i svaret tooan utgående begäran).

    För varje ny utgående anslutning som initierar en virtuell dator, har också en utgående port allokerats av hello belastningsutjämnaren. hello extern värd ser trafik med en virtuell IP-VIP-allokerad port. Scenarier som kräver ett stort antal utgående anslutningar, rekommenderas för toouse [offentlig IP på instansnivå](../virtual-network/virtual-networks-instance-level-public-ip.md) adresser så att hello virtuella datorer har en dedikerad utgående IP-adress för SNAT. Detta minskar risken för hello om porten konsumtion.

    Se [utgående anslutningar](load-balancer-outbound-connections.md) mer information om det här avsnittet.

### <a name="support-for-multiple-load-balanced-ip-addresses-for-virtual-machines"></a>Stöd för flera belastningsutjämnad IP-adresser för virtuella datorer
Du kan tilldela fler än en belastningsutjämnad offentliga IP-adress tooa uppsättning virtuella datorer. Med den här möjligheten du kan vara värd för flera SSL-webbplatser och/eller flera SQL Server AlwaysOn Availability Group-lyssnare på hello samma uppsättning virtuella datorer. Mer information finns i [flera VIP: er per Molntjänsten](load-balancer-multivip.md).

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="limitations"></a>Begränsningar

Läs in Belastningsutjämnarens serverdelspooler kan innehålla alla VM SKU förutom grundläggande nivån.

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om [Internetriktade belastningsutjämnare](load-balancer-internet-overview.md)

- Lär dig mer om [översikt över interna belastningsutjämnare](load-balancer-internal-overview.md)

- Skapa en [Internetriktade belastningsutjämnare](load-balancer-get-started-internet-portal.md)

- Lär dig mer om hello andra nyckeln [nätverk](../networking/networking-overview.md) Azure

