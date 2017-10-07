---
title: "aaaMigrating BizTalk Server EDI-lösningar tooBizTalk Services tekniska Guide | Microsoft Docs"
description: "Migrera EDI tooMABS; Microsoft Azure-tjänster för BizTalk"
services: biztalk-services
documentationcenter: na
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 61c179fa-3f37-495b-8016-dee7474fd3a6
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 34cca3c939a6a7845a860ead6858287000d03ee7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-biztalk-server-edi-solutions-toobiztalk-services-technical-guide"></a>Migrera BizTalk Server EDI-lösningar tooBizTalk tjänster: tekniska Guide

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Skapad av: Tim Wieman och Nitin Mehrotra

Granskare: Karthik Bharthy

Skrivits: Microsoft Azure BizTalk-tjänst – februari 2014-versionen.

## <a name="introduction"></a>Introduktion
Elektroniska Data Interchange (EDI) är en hello vanligaste innebär som företag utbyta data elektroniskt, kallas även för Business-to-Business eller B2B transaktioner. BizTalk Server har haft EDI-stöd för över tio år, sedan hello inledande BizTalk Server-versionen. Med BizTalk-tjänster fortsätter Microsoft hello stöd för EDI-lösningar på hello Microsoft Azure-plattformen. B2B transaktioner är främst externa tooan organisation och därför är det enklare tooimplement om det har implementerats på en plattform i molnet. Microsoft Azure tillhandahåller den här funktionen via BizTalk-tjänst.

Vissa kunder visar BizTalk-tjänst som en ”greenfield” plattform för nya EDI-lösningar kan ha många kunder aktuella BizTalk Server EDI-lösningar som kan de vill toomigrate tooAzure. Eftersom BizTalk Services EDI arkitektur utifrån hello nyckel samma entiteter som BizTalk Server EDI-arkitektur (handelspartner, enheter, avtal) och det är möjligt toomigrate BizTalk Server EDI artefakter tooBizTalk Services.

Det här dokumentet beskrivs några av hello skillnader ingår i migrera BizTalk Server EDI artefakter tooBizTalk tjänster. Det här dokumentet förutsätter kunskaper om BizTalk Server EDI-bearbetning och handel Partner avtal. Mer information om BizTalk Server EDI finns [handel Partner med hjälp av BizTalk hanteringsservern](https://msdn.microsoft.com/library/bb259970.aspx).

## <a name="which-version-of-biztalk-server-edi-artifacts-can-be-migrated-toobiztalk-services"></a>Vilken version av BizTalk Server EDI artefakter kan migreras tooBizTalk tjänster?
BizTalk Server EDI-modul för hello förbättrats avsevärt för BizTalk Server 2010, när den var remodeled tooinclude partner, profiler och avtal. BizTalk-tjänster använder hello samma modell tooorganize hello handelspartner och hello business-avdelningar inom de handelspartner. Därför kan migrera EDI artefakter från BizTalk Server 2010 och senare versioner tooBizTalk tjänster, är det mycket mer direkt framåt. toomigrate EDI artefakter som är associerade med versioner tidigare tooBizTalk Server 2010 måste du först uppgradera tooBizTalk Server 2010 och sedan migrera EDI-artefakter tooBizTalk tjänster.

## <a name="scenariosmessage-flow"></a>Scenarier/Message-flöde
Som med BizTalk Server är EDI bearbetas i BizTalk-tjänst uppbyggd kring en lösning för handel Partner (TPM). hello TPM-lösningen har hello följande huvudkomponenter:

* Handelspartner, som representerar organisationen i en B2B-transaktion.
* Profiler som representerar avdelningar inom en handelspartner.
* Handel partner (eller avtal), som representerar hello business avtal mellan två parter-profiler.

hello följande bild visar hello likheter, samt skillnader mellan en BizTalk Server EDI-lösning och BizTalk Services EDI-lösning:

![][EDImessageflow]

Hej viktiga skillnader och likheter mellan en EDI-lösningen flöde i BizTalk Server och BizTalk-tjänsterna är:

* Precis som BizTalk-servern använder ett EDIReceive pipeline tooreceive EDI-meddelandet och en EDISend pipeline toosend EDI-meddelandet, använder en EDI får bridge tooreceive och EDI skicka bridge toosend ut EDI-meddelanden i BizTalk-tjänst. BizTalk Server hello pipelines är kopplade till ett avtal med skicka eller ta emot portar. Hello avtal själva anger hello skicka eller ta emot bridge i BizTalk-tjänst.
* BizTalk Server när hello EDIReceive pipeline processer hello EDI-meddelandet är hello-meddelande dumpade tooa SQL Server-databas. sedan hämtar hello-meddelande från hello SQL Server-databas hello EdiSend pipeline, bearbetar den och skickar sedan ut toohello handelspartner.
  
    I BizTalk-tjänst när hello EDI meddelande bridge processer hello EDI, dirigerar hello meddelandet tooan extern process. hello extern process kan köras på Microsoft Azure eller lokalt. hello extern process ska vidarebefordra hello meddelandet toohello EDI skicka bridge; hello skicka bridge emot natur inte hello-meddelande. Hello EDI skicka bridge dirigerar hello meddelandet toohello handelspartner efter bearbetning hello-meddelande.

BizTalk-tjänsterna tillhandahåller ett enkelt att använda konfigurationen upplevelse tooquickly skapa och distribuera en B2B-avtal mellan handelspartner utan att konfigurera alla Microsoft Azure Compute instanser (webb- eller Arbetarroll roller), alla Microsoft Azure SQL-databaser eller något Microsoft Azure storage-konton. Mer komplicerade scenarier kräver binda i arbetsflöden eller annan bearbetning för tjänsten ”runt hello kanter” en handel partneravtalet, det vill säga före eller efter handel Partner avtal EDI bridge bearbetning. Detaljerade inträffar hello följande händelser under en EDI meddelandebehandling i BizTalk-tjänst.

1. En EDI-meddelandet tas emot från handelspartner Fabrikam.  För att ta emot EDI-meddelanden från handelspartner stöder BizTalk-tjänst transportprotokoll som FTP, SFTP, AS2 och HTTP/S.
2. hello handel partner avtal mottagarsidan bearbetning Disassemblerar hello EDI-meddelandeformat tooXML.  Du kan vidarebefordra hello upp EDI meddelandet (i XML-format) tooService Bus-slutpunkter som en Service Bus Relay-slutpunkt, Service Bus-ämne, Service Bus-kö eller en brygga BizTalk-tjänst.
3. hello kan nedmonterade XML-meddelanden sedan tas emot från hello slutpunkt för ytterligare anpassade bearbetning.  Dessa slutpunkter kunde bearbetas av komponenten lokalt eller en Microsoft Azure Compute instans toofurther processen hello-meddelande i en Windows-arbetsflödet (WF) eller Windows Communication Foundation (WCF)-tjänst, till exempel.
4. hello ”Skicka sida bearbetning” av hello handelspartneravtal sedan monterar hello XML-meddelande till EDI-format och skickar den tootrading partner Contoso.  För att skicka meddelanden för EDI tootrading partners, stöder BizTalk Services hello samma protokoll som används för att ta emot EDI-meddelanden.

Det här dokumentet ytterligare innehåller konceptuell information om migrera vissa hello olika BizTalk Server EDI artefakter tooBizTalk Services.

## <a name="sendreceive-ports-tootrading-partners"></a>Skicka och ta emot portar tooTrading Partners
I BizTalk Server skapar du få platser och ta emot portar tooreceive EDI/XML-meddelanden från handelspartner och du ställer in skicka portar toosend EDI/XML-meddelanden tootrading partner. Du binder sedan upp dessa portar tooa handel partneravtalet via hello BizTalk Server-administrationskonsolen. Hej platser där du får meddelanden från handelspartner och där du skickar meddelanden tootrading partner har konfigurerats som en del av hello handel partneravtalet (som en del av Transport inställningar) i hello BizTalk-Services-portalen i BizTalk-tjänst .  Så har du verkligen inte hello begreppet ”skicka portar” och ”ta emot platser” sig självt, i BizTalk-tjänst. Mer information finns i [skapa avtal](https://msdn.microsoft.com/library/windowsazure/hh689908.aspx).

## <a name="pipelines-bridges"></a>Pipelines (bryggor)
I BizTalk Server EDI är pipelines meddelandet bearbetning enheter som även kan innehålla anpassade logik för behandling av specifika funktioner som krävs av programmet hello. BizTalk-tjänster skulle hello motsvarande EDI-brygga. Men i BizTalk-tjänst för tillfället hello EDI bryggor ”stängs”.  Det vill säga kan du lägga till egna anpassade aktiviteter tooan EDI bridge. Alla anpassade bearbetning måste göras utanför hello EDI bridge i ditt program före eller efter hello-meddelande anger hello brygga som konfigurerats som en del av hello partneravtalet för handel. EAI bryggor har hello alternativet toodo anpassad bearbetning. Om du vill anpassad bearbetning, kan du använda EAI bryggor före eller efter hello-meddelande bearbetas av hello EDI bridge. Mer information finns i [hur tooInclude anpassad kod i bryggor](https://msdn.microsoft.com/library/azure/dn232389.aspx).

Du kan infoga ett Publicera/prenumerera-flöde med anpassad kod och/eller med hjälp av Service Bus-meddelanden köer och ämnen innan hello handel partneravtalet tar emot hello-meddelande eller när hello avtal bearbetar hello-meddelande och skickar det tooa Service Bus-slutpunkten.

Se **scenarier/Message flödet** i det här avsnittet för hello meddelandet flödet mönster.

## <a name="agreements"></a>Avtal
Om du är bekant med hello BizTalk Server 2010 Trading Partner avtal används för EDI bearbetning letar BizTalk-tjänst handelspartneravtal insatt. De flesta hello avtal inställningar är hello samma och Använd hello samma terminologi. I vissa fall hello avtal inställningar är mycket enklare jämfört med toohello samma inställningar i BizTalk-servern. Microsoft Azure BizTalk Services stöder X12-, EDIFACT- och AS2-transport.

Microsoft Azure BizTalk Services tillhandahåller också en **TPM datamigrering** verktyget toomigrate handelspartner och avtal från BizTalk Server handel Partner modulen tooBizTalk Services-portalen. hello TPM datamigreringsverktyget är tillgängligt som en del av ett verktyg för paket, som kan hämtas från hello [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057). hello paketet innehåller också en viktig information som innehåller instruktioner om hur toouse hello verktyget och grundläggande information om felsökning hello verktyget.

## <a name="schemas"></a>Scheman
BizTalk-tjänst ger EDI-scheman som kan användas i BizTalk-Services-lösningar.  Dessutom kan BizTalk Server EDI-scheman också användas med BizTalk-tjänst eftersom hello rotnoden i hello EDI-schemat är samma mellan BizTalk Server samt BizTalk-tjänst. Därför kommer du att kunna toodirectly vidta BizTalk Server EDI-scheman och Använd dem i hello EDI-lösningar som du utvecklar med BizTalk-tjänst. Du kan också hämta hello scheman från hello [MABS SDK](http://go.microsoft.com/fwlink/p/?LinkId=235057).

## <a name="maps-transforms"></a>Maps (transformeringar)
Maps i BizTalk Server kallas transformeringar i BizTalk-tjänst. Migrera mappningar från BizTalk Server tooBizTalk tjänster kan vara någon av hello mer komplicerade uppgifter tooachieve (beroende på kartan komplexitet). hello mappning verktyget för BizTalk-tjänst skiljer sig från hello BizTalk-mappning. Även om hello mapper ser ut Hej främst samma, hello underliggande mappningsformat är olika. Hej functoids (kallas **kartan Operations** i BizTalk-tjänst) tillgängliga toohello användare skiljer sig också.  Du kan i praktiken direkt använda en BizTalk-karta i BizTalk-tjänst. Inte alla hello functoids som är tillgängliga i BizTalk Server är också tillgängliga som kartan åtgärder i BizTalk-tjänst.

### <a name="new-transform-operations"></a>Nya omvandlingsåtgärder
Hello lista över kartan omvandlingsåtgärder tillgängliga kan verka detsamma hello BizTalk Server mapper, ha BizTalk Services omvandlar sätt hello samma uppgifter. Exempelvis BizTalk Services transformeringar har **Liståtgärder** tillgängliga. Detta är inte tillgänglig i hello BizTalk-mappning.  Hej **Liståtgärder** aktivera toocreate och fungerar på en ”lista”, där en lista är en uppsättning objekt (även kallat ”rader”) och varje objekt kan ha flera medlemmar (även kallat ”kolumner”).  Du kan sortera hello listan, Välj objekt baserat på ett villkor, osv.

Ett annat exempel de nya funktionerna i BizTalk Services omvandlar är hello **Loop Operations**.  Det är svårt toocreate kapslade loopar i hello BizTalk Server-mappning.  Därmed läggs hello Loop kartan operations för hello BizTalk Services transformeringar.

Har ett annat exempel är hello **If-Then-Else** operation i kartan.  Gör en if-then-else åtgärd var möjligt i hello BizTalk mapper, men det krävs flera functoids tooaccomplish till synes enkelt.

### <a name="migrating-biztalk-server-maps"></a>Migrera BizTalk Server mappar
BizTalk-tjänst för Microsoft Azure tillhandahåller en verktyget toomigrate BizTalk Server mappar tooBizTalk Services transformeringar. Hej **BTMMigrationTool** är tillgänglig som en del av hello **verktyg** paketet som medföljer hello [BizTalk Services SDK-hämtningen](http://go.microsoft.com/fwlink/p/?LinkId=235057). Mer information om verktyget hello finns [konvertera en BizTalk kartan tooa BizTalk Services transformera](https://msdn.microsoft.com/library/windowsazure/hh949812.aspx).

Du kan också titta på ett prov av Sandro Pereira, BizTalk MVP på hur för[migrera BizTalk Server maps tooBizTalk Services transformeringar](http://social.technet.microsoft.com/wiki/contents/articles/23220.migrating-biztalk-server-maps-to-windows-azure-biztalk-services-wabs-maps.aspx).

## <a name="orchestrations"></a>Orkestreringarna
Om du behöver toomigrate BizTalk Server orchestration bearbetning tooMicrosoft Azure måste hello orkestreringarna skrivas om eftersom Microsoft Azure inte stöder körs BizTalk Server orkestreringarna toobe.  Du kan skriva om hello orchestration-funktioner i en tjänst i Windows Workflow Foundation 4.0 (WF4).  Detta är en fullständig omarbetning eftersom det inte finns för närvarande inga migrering från BizTalk Server orkestreringarna tooWF4. Här följer några resurser för Windows-arbetsflödet:

* [*Hur toointegrate ett arbetsflöde för WCF-tjänst med Service Bus-köer och ämnen* ](https://msdn.microsoft.com/library/azure/hh709041.aspx) av Paolo Salvatori. 
* [*Skapa appar med Windows Workflow Foundation och Azure* session](http://go.microsoft.com/fwlink/p/?LinkId=237314) från hello Build 2011 konferensrum.
* [*Windows Workflow Foundation Developer Center* ](http://go.microsoft.com/fwlink/p/?LinkId=237315) på MSDN.
* [*Dokumentation för Windows Workflow Foundation 4 (WF4)* ](https://msdn.microsoft.com/library/dd489441.aspx) på MSDN.

## <a name="other-considerations"></a>Andra överväganden
Följande är några säkerhetsaspekter som du måste göra när du använder BizTalk-tjänst.

### <a name="fallback-agreements"></a>Fallback avtal
BizTalk Server EDI-bearbetningen har hello begreppet ”reserv avtalen”.  BizTalk-tjänst har **inte** hittills har ett begrepp som reserv avtal.  BizTalk-dokumentationen avsnitten [hello rollen avtal vid bearbetning av EDI](http://go.microsoft.com/fwlink/p/?LinkId=237317) och [reserv avtal egenskaper eller konfigurera globala](https://msdn.microsoft.com/library/bb245981.aspx) information om hur du använder reserv avtal i BizTalk Server.

### <a name="routing-toomultiple-destinations"></a>Routning toomultiple mål
BizTalk-tjänst bryggor i det aktuella tillståndet har inte stöd för routning meddelanden toomultiple mål med hjälp av ett Publicera-prenumerera modellen. Du kan i stället vidarebefordra meddelanden från en BizTalk-tjänst bridge tooa Service Bus-ämne, som sedan kan ha flera prenumerationer tooreceive hello-meddelande på mer än en slutpunkt.

## <a name="conclusion"></a>Slutsats
Microsoft Azure BizTalk-tjänster uppdateras vid regelbundna milstolpar tooadd flera funktioner och möjligheter. Varje uppdatering ser vi fram emot toosupporting ökat funktioner toofacilitate skapar slutpunkt till slutpunkt-lösningar med hjälp av BizTalk-tjänster och andra tekniker för Azure.

## <a name="see-also"></a>Se även
[Utveckla företagsprogram med Azure](https://msdn.microsoft.com/library/azure/hh674490.aspx)

[EDImessageflow]: ./media/biztalk-migrating-to-edi-guide/IC719455.png
