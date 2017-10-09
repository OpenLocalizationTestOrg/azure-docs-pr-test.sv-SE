---
title: "aaaHow tooConfigure en Apptjänstmiljö v1"
description: "Konfiguration, hantering och övervakning av hello Apptjänstmiljö v1"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: b5a1da49-4cab-460d-b5d2-edd086ec32f4
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: f9539a72517276d8a1e340a408841561e8b8f56d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-an-app-service-environment-v1"></a>Konfigurera en App Service miljö v1

> [!NOTE]
> Den här artikeln handlar om hello Apptjänstmiljö v1.  Det finns en nyare version av hello Apptjänst-miljö som är enklare toouse och körs på mer kraftfulla infrastruktur. Mer om hello nya versionen börjar med hello toolearn [introduktion toohello Apptjänstmiljö](../app-service/app-service-environment/intro.md).
> 

## <a name="overview"></a>Översikt
På en hög nivå, en Azure Apptjänst-miljö som består av flera huvudkomponenter:

* Beräkningsresurser som körs i hello Apptjänstmiljö värdtjänsten
* Lagring
* En databas
* En Classic(V1) eller resursen Manager(V2) Azure Virtual Network (VNet) 
* Ett undernät med hello Apptjänstmiljö värdbaserad tjänst som körs i den

### <a name="compute-resources"></a>Beräkna resurser
Du kan använda hello beräkningsresurser för dina fyra resurspooler.  Varje App Service miljö (ASE) har en uppsättning frontwebbservrarna och tre möjliga arbetarpooler. Du behöver inte toouse alla tre arbetarpooler--om du vill kan du bara använda en eller två.

hello värdar i hello resurspooler (frontwebbservrarna och personer) är inte tillgänglig direkt tootenants. Använda Remote Desktop Protocol (RDP) tooconnect toothem det går inte att, ändra deras etablering eller fungera som en administratör på dem.

Du kan ange resource pool antal och storlek. I en ASE har fyra storleksalternativ som är märkta P1 via P4. Mer information om dessa storlekar och deras prisnivå finns [priser för Apptjänst](../app-service/app-service-value-prop-what-is.md).
Ändra hello kvantitet eller storlek kallas en skalningsåtgärd.  Endast en skalningsåtgärden kan vara pågår i taget.

**Främre ends**: hello frontwebbservrarna är hello HTTP/HTTPS-slutpunkter för dina appar som lagras i din ASE. Du kör inte arbetsbelastningar i hello frontwebbservrarna.

* En ASE som börjar med två P2s som är tillräcklig för arbetsbelastningar för utveckling och testning och låg nivå produktionsarbetsbelastningar. Vi rekommenderar starkt P3s för måttlig tooheavy produktionsarbetsbelastningar.
* För måttlig tooheavy produktionsarbetsbelastningar rekommenderar vi att du har minst fyra P3s tooensure finns tillräcklig frontwebbservrarna körs när schemalagt underhåll sker. Schemalagt underhållsaktiviteter kommer att få ned en klientdelen i taget. Detta minskar den totala tillgängliga frontend kapaciteten under underhållsaktiviteter.
* Frontwebbservrarna kan ta upp tooan timme tooprovision. 
* För ytterligare skala finjusteringar bör du övervaka hello prosessorprocent, minnesprocent och aktiva begäranden mått för hello frontend-pool. Om hello CPU eller minne procenttal ovan 70 procent när du kör P3s, lägga till fler frontwebbservrarna. Om hello aktiva begäranden värdet medelvärden ut too15, 000 too20, 000 förfrågningar per klient, bör du också lägga till flera frontwebbservrarna. hello är övergripande mål tookeep CPU och minne procenttal nedan 70% och aktiva begäranden som beräknar medelvärdet ut toobelow 15 000 förfrågningar per klient när du kör P3s.  

**Anställda**: hello arbetare är där dina appar faktiskt kör. När du skalar upp din App Service-planer, som använder upp arbetare i hello associerat arbetspool.

* Du kan inte direkt lägga till anställda. De kan ta tooan timme tooprovision.
* Skalning hello storleken på en beräkningsresurser för alla poolen tar < 1 timme per uppdateringsdomän. Det finns 20 update domäner i en ASE. Om du har skalats hello beräkna storleken på en arbetspool med 10 instanser kan det ta upp too10 timmar toocomplete.
* Om du ändrar hello storleken på hello beräkningsresurser som används i en arbetspool kommer kallstart hello-appar som körs i den poolen, worker.

hello snabbast toochange hello compute resursstorleken på en arbetspool som inte kör några appar är att:

* Skala ned hello antalet anställda too2.  hello lägsta skala ned storlek i hello-portalen är 2. Det tar några minuter toodeallocate dina instanser. 
* Välj hello ny beräkning storlek och antalet instanser. Härifrån kan tar det upp too2 timmar toocomplete.

Om dina appar kräver en större storlek för beräknings-resurs, kan du dra nytta av hello tidigare vägledning. Du kan fylla i en annan worker-pool med anställda hello önskad storlek och flytta dina appar över toothat poolen istället för att ändra hello storleken på hello worker poolen som är värd för dessa appar.

* Skapa hello ytterligare instanser av hello behövs beräkna storleken i en annan worker-pool. Detta tar upp tooan timme toocomplete.
* Tilldela om din App Service-planer som är värd för hello-appar som behöver en större storlek toohello nyligen konfigurerade arbetspool. Det här är en snabb åtgärd som ska vidtas på mindre än en minut toocomplete.  
* Skala ned hello första worker poolen om du inte behöver de instanserna som inte används längre. Den här åtgärden tar några minuter toocomplete.

**Autoskalning**: en hello verktyg som hjälper dig att toomanage din beräknings-resursanvändningen är autoskalning. Du kan använda autoskalning för frontend eller arbetarpooler. Du kan göra sådant som ökar din instanser av valfri pooltyp hello morgonen och minska i hello kväll. Eller kanske du kan lägga till instanser när hello antalet anställda som är tillgängliga i en arbetspool sjunker under ett visst tröskelvärde.

Om du vill ha tooset Autoskala regler runt beräkning resource pool mått och Kom ihåg hello tid att etablera krävs. Mer information om autoskalning Apptjänstmiljöer finns [hur tooconfigure Autoskala i en Apptjänst-miljö][ASEAutoscale].

### <a name="storage"></a>Lagring
Varje ASE har konfigurerats med 500 GB lagringsutrymme. Detta utrymme används över alla hello appar i hello ASE. Detta utrymme är en del av hello ASE och för närvarande vara inte avstängda toouse ditt lagringsutrymme. Om du gör justeringar tooyour virtuella nätverksroutning eller säkerhetsgrupp, toostill måste tillåta åtkomst tooAzure lagring-- eller hello ASE fungerar inte.

### <a name="database"></a>Databas
hello-databasen innehåller information om hello som definierar hello miljö, samt hello information om hello-appar som körs på den. Detta är en del av hello lagras i Azure-prenumeration för. Det är inte något du har en direkt möjlighet toomanipulate. Om du gör justeringar tooyour virtuella nätverksroutning eller säkerhetsgrupp, toostill måste tillåta åtkomst tooSQL Azure-- eller hello ASE fungerar inte.

### <a name="network"></a>Nätverk
Hej VNet som används med din ASE kan vara en som du gjorde när du skapade hello ASE eller en som du har gjort i förväg. När du skapar hello undernät under skapande av ASE tvingas hello ASE toobe i hello samma resursgrupp som hello virtuellt nätverk. Om du behöver hello resursgruppens namn används av din ASE toobe än ditt VNet, måste toocreate din ASE med hjälp av en resource manager-mall.

Det finns vissa begränsningar i hello virtuella nätverk som används för en ASE:

* hello virtuella nätverk måste vara ett regionalt virtuellt nätverk.
* Det måste toobe ett undernät med 8 eller flera adresser där hello ASE distribueras.
* När ett undernät är används toohost en ASE, kan inte hello adressintervall hello undernät ändras. Därför rekommenderar vi att hello-undernätet innehåller minst 64 adresser tooaccommodate eventuell ASE tillväxt.
* Det kan finnas ingenting i hello undernät men hello ASE.

Till skillnad från hello värdbaserad tjänst som innehåller hello ASE, hello [virtuellt nätverk] [ virtualnetwork] och undernät som finns under användarkontroll.  Du kan administrera ditt virtuella nätverk via hello virtuella nätverk Användargränssnittet eller PowerShell.  En ASE kan distribueras i ett klassiskt eller Resource Manager VNet.  hello portal och API-upplevelser är något annorlunda mellan klassisk och Resource Manager VNets men hello ASE upplevelse är hello samma.

hello virtuella nätverk som används toohost en ASE kan använda antingen privata RFC1918 IP-adresser eller använda offentliga IP-adresser.  Om du vill toouse ett IP-adressintervall som inte omfattas av RFC1918 (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) måste du toocreate din VNet och undernät toobe som används av din ASE i ASE skapas.

Eftersom den här funktionen placerar hello Azure App Service i ditt virtuella nätverk, innebär det att dina appar som finns i din ASE kan nu komma åt resurser som är tillgängliga via ExpressRoute eller plats-till-plats virtuella privata nätverk (VPN) direkt. hello-appar som är inom din Apptjänst-miljön kräver inte ytterligare nätverk funktioner tooaccess resurser tillgängliga toohello virtuella nätverk som är värd för din Apptjänst-miljö. Det innebär att du inte behöver tooresources för toouse-VNET-integrering eller Hybridanslutningar tooget i eller anslutna tooyour virtuellt nätverk. Du kan använda båda dessa funktioner fortfarande om tooaccess resurser i nätverk som inte är ansluten tooyour virtuellt nätverk.

Exempelvis kan du använda toointegrate VNET-integrering med ett virtuellt nätverk som finns i din prenumeration, men inte är anslutna toohello virtuellt nätverk som din ASE är i. Du kan också fortfarande använda Hybridanslutningar tooaccess resurser som ingår i andra nätverk, precis som vanligt.  

Om du har ditt virtuella nätverk som konfigurerats med en ExpressRoute-VPN, bör du vara medveten om några av hello routning behov med en ASE. Det finns vissa konfigurationer, användardefinierad väg (UDR) som inte är kompatibla med en ASE. Mer information om hur du kör en ASE i ett virtuellt nätverk med ExpressRoute finns [körs en Apptjänst-miljö i ett virtuellt nätverk med ExpressRoute][ExpressRoute].

#### <a name="securing-inbound-traffic"></a>Att säkra inkommande trafik
Det finns två huvudsakliga sätt toocontrol inkommande trafik tooyour ASE.  Du kan använda Network Security Groups(NSGs) toocontrol vilka IP adresser kan komma åt din ASE som beskrivs här [hur toocontrol inkommande trafik i en Apptjänst-miljö](app-service-app-service-environment-control-inbound-traffic.md) och du kan också konfigurera din ASE med en intern belastning Balancer(ILB).  De här funktionerna kan också användas tillsammans om du vill toorestrict åtkomst med hjälp av NSG: er tooyour ILB ASE.

När du skapar en ASE skapas en VIP på ditt VNet.  Det finns två VIP typer externa och interna.  När du skapar en ASE med en extern VIP kommer dina appar i din ASE vara tillgänglig via en internet-dirigerbara IP-adress. När du väljer internt din ASE konfigureras med en ILB och kommer inte att direkt internet som är tillgänglig.  En ILB ASE kräver fortfarande en extern VIP men används bara för Azure åtkomst för hantering och underhåll.  

Under skapande av ILB ASE ange hello underdomäner som används av hello ILB ASE och kommer att ha toomanage egna DNS för hello underdomän som du anger.  Eftersom du ställer in hello underdomännamn måste också toomanage hello certifikatet som används för HTTPS-åtkomst.  Efter ASE uppmanas du att skapa tooprovide hello certifikat.  läsa toolearn mer information om hur du skapar och använder en ILB ASE [med en Apptjänst-miljö med en intern belastningsutjämnare][ILBASE]. 

## <a name="portal"></a>Portalen
Du kan hantera och övervaka din Apptjänst-miljö med hjälp av hello Användargränssnittet i hello Azure-portalen. Om du har en ASE, är du förmodligen toosee hello Apptjänst symbolen på din sidopanelen. Den här symbolen är används toorepresent Apptjänstmiljöer i hello Azure-portalen:

![Symbolen för App-miljö][1]

tooopen hello-användargränssnitt som visar en lista över dina Apptjänstmiljöer kan du använda hello ikonen eller välj hello-ikonen (”>” symbol) längst ned hello hello sidopanelen tooselect Apptjänstmiljöer. Genom att välja en av hello ASEs visas, öppna hello Gränssnittselement som inte används toomonitor och hantera den.

![Gränssnittet för att övervaka och hantera din Apptjänst-miljö][2]

Det här första bladet visar några egenskaper för din ASE tillsammans med ett mått diagram per resurspoolen. Några av hello egenskaper som visas i hello **Essentials** block är också hyperlänkar öppnas hello bladet som är kopplade till den. Du kan till exempel välja hello **virtuellt nätverk** namn tooopen in hello användargränssnitt som är associerade med hello virtuella nätverk som din ASE körs i. **Apptjänstplaner** och **appar** öppna varje bladen som innehåller dessa artiklar som finns i din ASE.  

### <a name="monitoring"></a>Övervakning
hello diagram Tillåt toosee olika prestandamått i varje resurspool. Du kan övervaka hello Genomsnittlig CPU och minne för frontend hello-pool. Du kan övervaka hello antal som används och hello antal som är tillgängligt för worker pooler.

Flera Apptjänst planer kan göra användning av hello arbetare i en arbetspool. hello arbetsbelastning distribueras inte i hello samma sätt som med hello-frontservrar så hello processor- och minnesanvändning inte ger mycket hello sätt användbar information. Det är viktigare tootrack hur många personer som du har använt och är tillgängliga, särskilt om du hanterar systemet för andra toouse.  

Du kan också använda alla hello mått som kan spåras i hello diagram tooset in aviseringar. Konfigurera aviseringar här fungerar hello samma som någon annanstans i App Service. Du kan ange en avisering från antingen hello **aviseringar** UI del eller från vidaresökning i några mått UI och välja **Lägg till avisering**.

![Mått UI][3]

hello är som beskrivs bara hello Apptjänstmiljö mått. Det finns också mätvärden som är tillgängliga när hello App Service-plan nivå. Detta är där övervakning CPU och minne blir mycket bra.

I en ASE är alla hello App Service-planer dedikerade App Service-planer. Som innebär att hello appar som körs på hello värdar allokerade toothat programtjänstplanen hello appar i den här programtjänstplanen. toosee information om din programtjänstplan ta fram din programtjänstplan från någon av hello listor i hello ASE Användargränssnittet eller från **Bläddra apptjänstplaner** (som listar alla).   

### <a name="settings"></a>Inställningar
Inom hello ASE bladet finns en **inställningar** avsnitt som innehåller flera viktiga funktioner:

**Inställningar för** > **egenskaper**: hello **inställningar** blad öppnas automatiskt när du skapar ASE-bladet. Hello upp är **egenskaper**. Det finns ett antal objekt i här som är redundant toowhat som du ser i **Essentials**, men det är mycket användbar är **virtuella IP-adressen**, samt **utgående IP-adresser**.

![Inställningsbladet och egenskaper][4]

**Inställningar för** > **IP-adresser**: när du skapar en IP-Secure Sockets Layer (SSL)-app i din ASE, måste en SSL IP-adress. I ordning tooobtain en måste din ASE SSL IP-adresser som den äger som kan allokeras. När en ASE skapas, den har en SSL IP-adress för detta ändamål, men du kan lägga till fler. Det finns en kostnad för ytterligare IP SSL-adresser, som visas i [priser för Apptjänst] [ AppServicePricing] (i hello-avsnittet i SSL-anslutningar). hello ytterligare priset är hello IP SSL pris.

**Inställningar för** > **Front End poolen** / **Arbetarpooler**: var och en av dessa resource pool blad erbjuder hello möjlighet toosee information endast om den resurspoolen , dessutom styr tooproviding toofully skala den resurspoolen.  

grundläggande hello-bladet för varje resurspool innehåller ett diagram med mått för den resurspoolen. Precis som med hello diagram från hello ASE bladet, kan du gå till hello diagram och ställa in aviseringar efter behov. Ställer in en avisering från hello ASE bladet för en specifik resurspool hello samma sak som att göra det från hello resurspool. Från hello arbetspool **inställningar** bladet du har åtkomst tooall hello appar eller apptjänstplaner som körs i den här arbetspool.

![Inställningar för Worker UI][5]

### <a name="portal-scale-capabilities"></a>Portalen skala funktioner
Det finns tre skalningsåtgärder:

* Ändra hello antalet IP-adresser i hello ASE som är tillgängliga för IP SSL-användning.
* Ändra hello storlek på hello beräkningsresurser som används i en resurspool.
* Ändra hello antalet beräkningsresurser som används i en resurspool, antingen manuellt eller via autoskalning.

I hello portal finns tre sätt toocontrol hur många servrar som du har i din resurspooler:

* Skalningsåtgärden från hello ASE huvudblad hello överst. Du kan göra flera skala konfigurationsändringar toohello frontend och arbetarpooler. De tillämpas alla som en enda åtgärd.
* En manuell skalningsåtgärden från hello enskilda resurspool **skala** bladet är under **inställningar**.
* Autoskalning som du skapar från hello enskilda resurspool **skala** bladet.

toouse hello skalningsåtgärden på hello ASE bladet dra hello skjutreglaget toohello kvantitet du vill använda och spara. Det här Gränssnittet stöder också ändra hello storlek.  

![Skala UI][6]

toouse hello manuell eller automatiska funktioner i en specifik resurspool gå för**inställningar** > **Front End poolen** / **Arbetarpooler** som det behövs. Öppna sedan in hello poolen som du vill toochange. Gå för**inställningar** > **skala ut** eller **inställningar** > **skala upp**. Hej **skala ut** bladet kan du toocontrol instans kvantitet. **Skala upp** kan du toocontrol resursstorlek.  

![Inställningar för Användargränssnittet][7]

## <a name="fault-tolerance-considerations"></a>Överväganden för feltolerans
Du kan konfigurera en Apptjänstmiljö toouse in too55 totala beräkningsresurser. För dessa 55 beräkningsresurser kan endast 50 vara används toohost arbetsbelastningar. hello anledningen har två syften. Det finns minst 2 frontend beräkningsresurser.  Som lämnar in too53 toosupport hello arbetspool allokering. I ordning tooprovide feltolerans behöver du toohave en ytterligare beräkningsresurser som allokeras enligt toohello följande regler:

* Varje arbetspool måste minst 1 ytterligare beräkningsresurser som inte är tillgängliga toobe som tilldelats en arbetsbelastning.
* Om hello antalet beräkningsresurser i en arbetspool går över ett visst värde, måste en annan beräkningsresurser anges för feltolerans. Detta gäller inte hello i hello frontend-pool.

Inom en enskild arbetspool är hello feltolerans kraven att resurser för ett givet värde för X tilldelat tooa arbetspool:

* Om X är mellan 2 och 20, är hello användbara beräkningsresurser som du kan använda för arbetsbelastningar X-1.
* Om X är mellan 21 och 40, är hello användbara beräkningsresurser som du kan använda för arbetsbelastningar X-2.
* Om X är mellan 41 och 53, är hello användbara beräkningsresurser som du kan använda för arbetsbelastningar X-3.

hello minsta storleken har 2 frontservrar och 2 anställda.  Här följer några exempel tooclarify med hello ovan instruktioner sedan:  

* Om du har 30 arbetare i en enda pool kan 28 av dem vara används toohost arbetsbelastningar.
* Om du har 2 anställda i en enda pool vara 1 används toohost arbetsbelastningar.
* Om du har 20 arbetare i en enda pool kan 19 vara används toohost arbetsbelastningar.  
* Om du har 21 arbetare i en enda pool fortfarande endast 19 kan använda toohost arbetsbelastningar.  

hello feltolerans aspekt är viktigt, men du måste tookeep det i åtanke när du skalar ovan vissa tröskelvärden. Om du vill tooadd mer kapacitet från 20 instanser och sedan gå too22 eller högre eftersom 21 inte lägga till några mer kapacitet. samma sak gäller kommer ovan 40, där hello nästa nummer som lägger till kapacitet är 42 hello.  

## <a name="deleting-an-app-service-environment"></a>Ta bort Apptjänst-miljö
Om du vill toodelete en Apptjänst-miljö, bara använder hello **ta bort** åtgärd hello överst på bladet för hello Apptjänst-miljö. När du gör det, kommer du att ange tooenter hello namnet på din Apptjänstmiljö tooconfirm att du verkligen toodo detta. Observera att när du tar bort en Apptjänst-miljö kan du ta bort alla hello innehållsfiler i den.  

![Ta bort Apptjänst-miljö UI][9]  

## <a name="getting-started"></a>Komma igång
tooget igång med Apptjänstmiljöer, se [hur toocreate en Apptjänstmiljö](app-service-web-how-to-create-an-app-service-environment.md).

Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service](../app-service/app-service-value-prop-what-is.md).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-web-configure-an-app-service-environment/ase-icon.png
[2]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-aseblade.png
[3]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolchart.png
[4]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-properties.png
[5]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolblade.png
[6]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-scalecommand.png
[7]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-poolscale.png
[8]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-pricingtiers.png
[9]: ./media/app-service-web-configure-an-app-service-environment/aseconfig-deletease.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
