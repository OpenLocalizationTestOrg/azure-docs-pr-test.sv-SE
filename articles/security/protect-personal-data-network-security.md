---
title: "aaaProtect personliga data med Azure network säkerhetsfunktioner | Microsoft Docs"
description: "Skydda personliga data med Azure funktioner för nätverkssäkerhet"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: be112a9408d327ccedf871656afe800fc7f775e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protect-personal-data-with-network-security-features-azure-application-gateway-and-network-security-groups"></a>Skydda dina personliga data med funktioner för nätverkssäkerhet: Azure Application Gateway och Nätverkssäkerhetsgrupper

Den här artikeln innehåller information och procedurer som hjälper dig att använda Azure Application Gateway och Nätverkssäkerhetsgrupper tooprotect personliga data.

En viktig del i flera lager säkerhet strategi tooprotect hello sekretessnivå personliga data är skydd mot vanliga säkerhetsproblem kryphål, till exempel SQL injection eller globala webbplatsskript. Att hålla oönskad nätverkstrafik i ditt virtuella Azure-nätverket skyddar mot potentiella hot av känsliga data och Microsoft Azure tillhandahåller verktyg toohelp skydda dina data mot angripare.

## <a name="scenario"></a>Scenario

Ett stort kryssning företag, sitt säte i hello USA utökar dess operations toooffer färdvägar i hello Medelhavet, Adriatiska havet, baltiska havet samt hello brittiska staterna. För att främja dessa åtgärder, har det fått flera mindre cruise rader i Italien, Tyskland, Danmark och hello Storbritannien

hello företag använder Microsoft Azure toostore företagsdata i hello molnet och köra program på virtuella datorer som bearbetar och komma åt dessa data. Dessa data innehåller personligt identifierbar information, till exempel namn, adresser, telefonnummer och kreditkortsinformation av dess globala kundbas. Den innehåller också traditionella personal information, till exempel adresser, telefonnummer, skatt identifikationsnummer och medicinska information om företagets anställda på alla platser. hello kryssning rad har också en stor databas med medlemmar av trafik och förmåner som innehåller personuppgifter tootrack relationer med aktuella och tidigare kunder.

Företagets anställda åtkomst hello nätverket från fjärranslutna kontor och resa agenter finns runt hello world hello företagets har åtkomst till företagsresurser för toosome och använder webbaserade program som finns i Azure Virtual Machines toointeract med den.

## <a name="problem-statement"></a>Problembeskrivning

hello företag måste skydda hello sekretessen för kunders och anställdas personliga data från angripare som utnyttjar programvara säkerhetsrisker toorun skadlig kod som kan uttsätta personliga data lagras eller används av hello företagets molnbaserade program.

## <a name="company-goal"></a>Företagets mål

hello företagets mål tooensure som obehöriga personer inte kan komma åt företagets Azure virtuella nätverk och hello program och data som finns där genom att utnyttja vanliga säkerhetsproblem. 

## <a name="solutions"></a>Lösningar

Microsoft Azure tillhandahåller säkerhet mekanismer toohelp förhindra oönskad trafik från att ange virtuella Azure-nätverk. Kontroll av inkommande och utgående trafik utförs traditionellt genom brandväggar. Du kan använda hello Programgateway med hello Brandvägg för webbaserade program och Nätverkssäkerhetsgrupp grupper (NSG), som fungerar som en enkel distribuerade brandväggen i Azure. Dessa verktyg kan du toodetect och blockera oönskad nätverkstrafik.

### <a name="application-gatewayweb-application-firewall"></a>Brandvägg för programmet Gateway/webbaserade program

Hej [Brandvägg för webbaserade program](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (Brandvägg) komponent i hello [Azure Programgateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-introduction) skyddar webbprogram som är mer och mer riktad mot angrepp som drar nytta av vanliga kända säkerhetsproblem. En centraliserad Brandvägg skyddar mot webbattacker och säkerhetshanteringen utan några ändringar i programmet.

Azure Brandvägg adresser olika attack kategorier inklusive SQL injection, över flera webbplatser, HTTP-protokollet överträdelser och avvikelser, robotar, robotar, skannrar, vanliga felinställningar för programmet, http-Denial of Service och andra vanliga attacker som kommandot injection HTTP-begäran smuggling HTTP-svar dela och Fjärrlagring inkludering attacker. 

Du kan skapa en Programgateway med Brandvägg eller Lägg till Brandvägg tooan befintliga Programgateway. I båda fallen kräver Azure Programgateway sitt eget undernät.

#### <a name="how-do-i-create-an-application-gateway-with-waf"></a>Hur skapar jag en Programgateway med Brandvägg? 

toocreate en ny Programgateway med Brandvägg är aktiverad, hello följande:

1. Loggen i toohello Azure-portalen och hello **Favoriter** hello portalen klickar du på **ny**

2. I hello **ny** bladet, klickar du på **nätverk**.

3. Klicka på **Programgateway**.

4. Navigera toohello Azure-portalen **klickar du på nytt \> nätverk \> Application Gateway.**

   ![Skapa programgatewayer](media/protect-netsec/app-gateway-01.png)

5. I hello **grunderna** bladet som visas, ange hello värden för hello följande fält: namn, tjänstnivån (Standard eller Brandvägg) SKU-storleken (små, medelstora eller stora) instansen antalet (2 för hög tillgänglighet), prenumeration, resursgrupp, och Plats.

6. I hello **inställningar** bladet som visas under **för virtuella nätverk**, klickar du på **Välj ett virtuellt nätverk**. Det här steget öppnas ange hello Välj virtuellt nätverk-bladet.

7. Klicka på **Skapa nytt** tooopen hello **skapa virtuellt nätverk** bladet.

8. Ange hello följande värden: namn, adressutrymme, undernätsnamn, adressintervallet i undernätet. Klicka på **OK**.

9. På hello **inställningar** bladet under **Frontend-IP-konfiguration**, Välj hello IP-adresstypen.

10. Klicka på **Välj en offentlig IP-adress** sedan **Skapa nytt.**

11. Acceptera standardvärdet för hello och på **OK.**

12. På hello **inställningar** bladet under **Lyssnarkonfigurationen**, Välj toouse HTTP eller HTTPS enligt **protokollet**. toouse HTTPS krävs ett certifikat.

13. Konfigurera specifika inställningar för hello Brandvägg: **brandväggen status** (**aktiverad**) och **brandväggsläge** (**förebyggande**). Om du väljer **identifiering** som hello läge, loggas endast trafik.

14. Granska hello **sammanfattning** och klickar på **OK**. Hello Programgateway är nu i kö och skapas.

När hello Programgateway har skapats kan du navigera tooit hello-portalen och fortsätta konfigurationen av hello Programgateway.

![skapade Programgateway](media/protect-netsec/adatum-app-gateway.png)

#### <a name="how-do-i-add-waf-tooan-existing-application"></a>Hur lägger jag till Brandvägg tooan befintliga program?

tooupdate ett befintligt program gateway toosupport Brandvägg i förebyggande läge hello följande:

1. I hello Azure-portalen **Favoriter** rutan klickar du på **alla resurser**.

2. Klicka på hello befintliga Programgateway i hello **alla resurser** bladet. 
>[!NOTE]
Obs: Om hello-prenumeration som du har valt redan har flera resurser i den, kan du ange hello namn i hello filtrera efter namn... rutan tooeasily åtkomst hello DNS-zon.
3. Klicka på **Brandvägg för webbaserade program** och uppdatera inställningar för Programgateway hello: **uppgradera tooWAF nivå** (markerad) **brandväggen status** (aktiverat)  **Brandväggsläge** (förhindra). Du måste också tooconfigure hello regeluppsättning och konfigurera inaktiverade regler.

Mer detaljerad information om hur toocreate en ny Programgateway med Brandvägg och hur tooadd Brandvägg tooan befintliga Programgateway, se [skapa en Programgateway med Brandvägg för webbaserade program med hjälp av hello-portalen.](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal)

### <a name="network-security-groups"></a>Nätverkssäkerhetsgrupper

En [nätverkssäkerhetsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) innehåller en lista över säkerhetsregler som tillåter eller nekar nätverket trafik tooresources ansluten för[Azure Virtual Networks](https://docs.microsoft.com/azure/virtual-network/) (VNet). NSG: er kan vara associerade toosubnets eller enskilda virtuella datorer. När en NSG är associerad tooa undernät, tillämpas hello reglerna tooall resurser anslutna toohello undernät. Trafik kan ytterligare begränsas av också koppla en NSG tooa VM eller nätverkskort.

NSG: er innehåller fyra egenskaper: namn, Region, resursgruppen och regler.
>[!Note]
Även om en NSG finns i en resursgrupp, det kan vara associerade tooresources i valfri resursgrupp, så länge hello resursen är en del av hello samma Azure-region som hello NSG.

NSG-regler innehåller nio egenskaper: namn, protokoll (TCP, UDP eller \*, som inkluderar ICMP samt UDP och TCP), källa portintervall, Målportintervall, källadress-prefix, måladress-prefix, riktning (inkommande eller utgående), prioritet ( mellan 100 och 4096) och komma åt typen (Tillåt eller neka). Alla NSG: er innehåller en uppsättning standardregler som kan tas bort eller åsidosätts av hello regler som du skapar.

#### <a name="how-do-i-implement-nsgs"></a>Hur jag implementera NSG: er?

Implementera NSG: er kräver planering och det finns flera designöverväganden som du behöver tootake i beräkningen. Dessa inkluderar begränsning hello antalet NSG: er per prenumeration och regler per NSG; Utformning av VNet och undernät, särskilda regler, ICMP-trafik, isolering av nivåer med undernät och belastningsutjämnare.

Mer information i Planera och implementera NSG: er och ett exempelscenario för distribution finns [filtrera nätverkstrafik med nätverkssäkerhetsgrupper.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg)

#### <a name="how-do-i-create-rules-in-an-nsg"></a>Hur skapar regler i en NSG?

toocreate regler för inkommande trafik i en befintlig NSG, hello följande:

1. Klicka på **Bläddra**, och sedan **Nätverkssäkerhetsgrupper**.

2. Hello listan med NSG: er, på **NSG-klientdel**, och sedan **inkommande säkerhetsregler.**

3. På hello listan över ingående säkerhetsregler **Lägg till.**

4. Ange hello-värden i hello följande fält: namn, prioritet, källa, protokoll, källa intervallet, mål, mål portintervall och åtgärden.

hello nya regeln visas i hello NSG efter några sekunder.

![Nätverkssäkerhetsregler](media/protect-netsec/inbound-security.png)

Mer information om hur toocreate NSG: er i undernät, skapa regler och koppla en NSG till ett frontend och backend-undernät, finns [skapa nätverkssäkerhetsgrupper med hello Azure-portalen.](https://docs.microsoft.com/azure/virtual-network/virtual-networks-create-nsg-arm-pportal)

## <a name="next-steps"></a>Nästa steg

[Azure nätverkssäkerhet](https://azure.microsoft.com/blog/azure-network-security/)

[Säkerhetsmetoder för Azure-nätverk](https://docs.microsoft.com/azure/security/azure-security-network-security-best-practices)

[Hämta information om en nätverkssäkerhetsgrupp](https://docs.microsoft.com/rest/api/network/virtualnetwork/get-information-about-a-network-security-group)

[Brandvägg för webbaserade program (Brandvägg)](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
