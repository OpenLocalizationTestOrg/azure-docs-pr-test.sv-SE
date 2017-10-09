---
title: "aaaMove appar från BizTalk-tjänst tooAzure Logic Apps | Microsoft Docs"
description: Flytta eller migrera Azure BizTalk Services MABS tooLogic appar
services: logic-apps
documentationcenter: 
author: jonfancey
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: ladocs; jonfan; mandia
ms.openlocfilehash: b3b065b90a37002f72305b0fc866c24231fb5f9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-from-biztalk-services-toologic-apps"></a>Flytta från BizTalk-tjänst tooLogic appar

Microsoft Azure BizTalk-tjänster (MABS) tas ur bruk. Använd det här avsnittet toomove din MABS integrering lösningar tooAzure Logic Apps. 

## <a name="overview"></a>Översikt

BizTalk-tjänster består av två underordnade tjänster:

1.  Microsoft BizTalk Services Hybridanslutningar
2.  EAI- och EDI bridge-baserade integrering

Om du letar toomove hybridanslutningar sedan [Hybridanslutningar för Azure App Service](../app-service/app-service-hybrid-connections.md) beskrivs hello ändringar och funktioner i den här tjänsten. Azure Hybridanslutningar ersätter Hybridanslutningar för BizTalk-tjänster. Azure Hybridanslutningar är tillgänglig i Azure App Service och erbjuds i hello Azure-portalen. Azure Hybridanslutningar innehåller också en ny Hybridanslutningshanteraren toomanage befintlig BizTalk-tjänst hybridanslutningar och nya hybridanslutningar som du skapar i hello portal. Azure App Service-Hybridanslutningar är allmänt tillgänglig (GA).

Logic Apps är hello ersättning för EAI- och EDI bridge-baserade-integration. Logic Apps innehåller alla hello samma funktioner som BizTalk-tjänst med mera. Logic Apps tillhandahåller skalbar molnlagring förbrukningsbaserad arbetsflöde och orchestration funktioner som gör att du tooquickly och enkelt skapa komplexa integration lösningar använder en webbläsare, eller använda verktyg i Visual Studio.

hello följande tabell innehåller en mappning av BizTalk-tjänst funktioner tooLogic appar.

| BizTalk Services   | Logic Apps            | Syfte                  |
| ------------------ | --------------------- | ---------------------------- |
| koppling          | koppling             | Skicka och ta emot data   |
| Platslänksbrygga             | Logikapp             | Pipeline-processor           |
| Validera fas     | XML-verifiering åtgärd      | Validera ett XML-dokument mot ett schema             |
| Utöka fas       | Data-token      | Befordra egenskaper i meddelanden eller för beslut om routning             |
| Transformera fas    | Transformera åtgärd      | Konvertera XML-meddelanden från ett format tooanother             |
| Avkoda fas       | Flat fil avkoda åtgärd      | Konvertera från flat fil tooXML             |
| Koda fas       |  Flat fil koda åtgärd      | Konvertera från XML-filen för tooflat             |
| Meddelandet Inspector       |  Azure Functions eller API Apps      | Köra anpassad kod i din integreringar             |
| Väg-åtgärd      |  Villkor eller växel      | Väg meddelanden tooone av hello angetts kopplingar             |

Det finns ett antal olika typer av artefakt i BizTalk-tjänst.

## <a name="connectors"></a>Anslutningar
Kopplingar i BizTalk-tjänst Tillåt bryggor toosend och ta emot data, inklusive dubbelriktat bryggor som aktiverad interaktioner som HTTP-baserade frågor och svar. I Logic Apps används hello samma terminologi. Kopplingar i logikappar hantera hello samma syfte, och även innehålla över 140 som kan ansluta tooa stor mängd olika tekniker och tjänster, både lokalt med hjälp av hello lokala Data Gateway (ersätta hello BizTalk Adapter-tjänst som används av BizTalk-tjänst) och molntjänster SaaS och PaaS som OneDrive, Office 365, Dynamics CRM och mycket mer.

Källor i BizTalk-tjänst är begränsat tooFTP, SFTP och Service Bus-kö eller prenumeration på artikeln.

![](media/logic-apps-move-from-mabs/sources.png)

Varje brygga har en HTTP-slutpunkt som standard, som är konfigurerad med hello Runtime-adress och hello relativ adress egenskaperna för hello bridge. tooachieve hello samma med Logic Apps, Använd hello [förfrågan och svar](../connectors/connectors-native-reqres.md) åtgärder.

## <a name="xml-processing-and-bridges"></a>XML-bearbetning och bryggor
En brygga i BizTalk-tjänst är detsamma tooa process-pipelinen. En brygga kan ta data från en koppling och göra några arbeta med hello data och skicka den tooanother system. Logic Apps hello samma genom att stödja hello samma pipeline-baserade interaktion mönster som BizTalk-tjänst och innehåller också ett antal andra integration mönster. Hej [XML-Request-Reply Bridge](https://msdn.microsoft.com/library/azure/hh689781.aspx) i BizTalk Services kallas en VETER pipeline som består av faser som kan:

* V Validera
* (E) utöka
* (T) transformera
* (E) utöka
* (R) väg

Som det visas i följande bild hello hello bearbetning delas mellan begäran och svar och tillåter kontroll över hello begäran och hello svar sökvägar separat (till exempel med hjälp av olika kartor för varje):

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Dessutom XML enkelriktade bridge läggs avkoda och koda faser i hello början och slutet av bearbetning och hello direkt bridge innehåller ett enda Enrich steg.

### <a name="message-processing-and-decodingencoding"></a>Bearbetning av meddelandet och avkoda/kodning
I BizTalk-tjänst tar emot XML-meddelanden med olika typer och fastställa hello matchande schemat för hello-meddelande togs emot. Detta görs i hello **meddelandetyper** steget i hello får process-pipelinen. Sedan används hello avkoda steget hello upptäckte meddelandet typ toodecode den med hjälp av hello angivna schemat. Om hello-schemat är en platt fil-schemat, konverterar hello inkommande platt fil tooXML. 

Logic Apps tillhandahåller liknande funktioner. Du får en platt fil över en mängd olika protokoll som använder olika connector hello utlösare (filsystem, FTP-, HTTP och så vidare) och använda hello [Flat fil avkoda](../logic-apps/logic-apps-enterprise-integration-flatfile.md) åtgärd tooconvert hello inkommande data tooXML. Du kan flytta dina befintliga flat fil-scheman direkt toologic appar utan att något ändras och sedan ladda upp scheman tooyour integrering konto.

### <a name="validation"></a>Validering
När hello inkommande data är konverterade tooXML (eller om XML var hello-meddelandeformat togs emot), kör verifieringen toodetermine om hello-meddelande följer tooyour XSD-schemat. toodo detta i Logikappar, Använd hello [XML-verifiering](../logic-apps/logic-apps-enterprise-integration-xml-validation.md) åtgärd. Igen och du kan använda hello samma scheman från BizTalk-tjänst utan några ändringar.

### <a name="transform-messages"></a>Transformera meddelanden
I BizTalk-tjänst konverterar hello transformeringen mellanlagra en XML-baserade meddelande format tooanother. Detta görs genom att använda en karta med hello TRFM-baserade mapper. Hello processen påminner om i Logic Apps. hello Transform-åtgärden körs en karta från ditt konto för integrering. hello största skillnaden är att mappningar i Logic Apps XSLT-format. XSLT innehåller hello möjlighet tooreuse befintliga XSLT som du redan har, inklusive maps som skapats för BizTalk Server som innehåller functoids. 

### <a name="routing-rules"></a>Regler för Routning
BizTalk-tjänst gör ett beslut om routning på vilka endpoint-koppling toosend inkommande meddelanden/data. hello möjlighet tooselect en av ett antal förkonfigurerade slutpunkter är möjligt med hjälp av hello routning filteralternativet:

![](media/logic-apps-move-from-mabs/route-filter.png)

Logic Apps ger funktioner för mer avancerad logik med [villkoret](../logic-apps/logic-apps-use-logic-app-features.md) och [växel](../logic-apps/logic-apps-switch-case.md), aktivera avancerade Kontrollflöde och routning. Konvertera routning filter i BizTalk-tjänst bäst uppnås med hjälp av en **villkoret** *om* det finns två alternativ. Om det finns fler än två, använder du en **växla**.

### <a name="enrich"></a>Utöka
Hej Enrich delen i BizTalk-tjänst som bearbetar lägger till egenskaper toohello Meddelandekontext som är associerade med hello mottagna data. Till exempel uppgradera en egenskapen toouse för routning (diskuteras nedan) från en databassökning, eller genom att extrahera ett värde med ett XPath-uttryck. Logic Apps ger åtkomst tooall kontextuella utdata från föregående åtgärder, vilket gör det enkelt tooreplicate hello samma beteende. Till exempel använder hello `Get Row` SQL-anslutningsåtgärd du returnera data från en SQL Server-databas och Använd hello data i en beslut åtgärd för routning. På samma sätt egenskaper på inkommande Service Bus meddelanden i kön av en utlösare är adresserbara samt XPath med hello xpath arbetsflödet definition language uttryck.

### <a name="use-custom-code"></a>Använd anpassad kod
BizTalk-tjänst ger hello möjlighet för[köra anpassad kod](https://msdn.microsoft.com/library/azure/dn232389.aspx) på din egen sammansättningar. Det har implementerats av hello [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector.aspx) gränssnitt. Varje steg i hello bridge innehåller två egenskaper (på Ange Inspector och på Avsluta Inspector) som tillhandahåller hello .net-typ du skapade som implementerar det här gränssnittet. Anpassad kod kan du tooperform mer komplexa bearbetning på hello data, samt återanvändning befintlig kod i sammansättningar som utför vanliga affärslogik. 

Logic Apps tillhandahåller två sätt tooexecute anpassad kod: Azure Functions och API Apps. Azure Functions kan skapas och anropas från logikappar. Se [Lägg till och kör anpassad kod för logic apps via Azure Functions](../logic-apps/logic-apps-azure-functions.md). Använd API Apps, en del av Azure App Service toocreate egna utlösare och åtgärder. Lär dig mer om [skapar en anpassad API toouse med Logic Apps](../logic-apps/logic-apps-create-api-app.md). 

Om du har anpassade kod i assmeblies som anropas från BizTalk-tjänst kan du antingen flytta detta code tooAzure funktioner eller skapa anpassade API: er med API Apps; beroende på vad du implementera. Till exempel om du har kod som omsluter en annan tjänst att Logic Apps inte har en koppling sedan skapa en API-App och använda hello åtgärder API-appen finns i din logikapp. Om du har hjälpfunktioner eller bibliotek, är förmodligen hello bäst med Azure Functions.

### <a name="edi-processing-and-trading-partner-management"></a>EDI bearbetning och hantering av handelspartners
BizTalk-tjänster omfattar EDI och B2B bearbetning med stöd för AS2 tillämplighet instruktionen 2, X12 och EDIFACT. Det gör även Logic Apps. I BizTalk Services din skapa EDI bryggor och skapa/hantera handelspartner och avtal i hello dedikerade spårning och Management-portalen.

I Logic Apps den här funktionen ingår hello [Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md). Det här består av hello Integration konto och B2B åtgärder för EDI och B2B-bearbetning. Hej [integrering konto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) är används toocreate och hantera [handelspartner](../logic-apps/logic-apps-enterprise-integration-partners.md) och [avtal](../logic-apps/logic-apps-enterprise-integration-agreements.md). Du kan associera en eller flera logic apps toohello konto när du skapar ett konto för integrering. När associerat, kan du använda hello B2B åtgärder tooaccess handel partnerinformation i din logikapp. Det finns hello följande åtgärder:

* AS2 koda
* AS2 avkoda
* Koda X12
* Avkoda X12
* EDIFACT koda
* EDIFACT avkoda

Till skillnad från BizTalk-tjänst fristående dessa åtgärder från hello transportprotokoll. Så när du skapar dina logic apps har du mer flexibilitet för vilka kopplingar du använder toosend och ta emot data. Till exempel det är möjligt tooreceive X12 filer som bifogade filer från e-post och bearbeta dessa filer i en logikapp. 

## <a name="manage-and-monitor"></a>Hantera och övervaka
Samt hantering av handelspartners hello dedikerade portal BizTalk-tjänst under förutsättning att spåra funktioner toomonitor och felsöka problem. 

Logic Apps ger bättre spårning och övervakning av funktioner i hello [Azure-portalen](../logic-apps/logic-apps-monitor-your-logic-apps.md), och med hello [Operations Management Suite B2B-lösning](../logic-apps/logic-apps-monitor-b2b-message.md), även en mobilapp för att hålla koll på saker När du är på hello flytta.

## <a name="high-availability"></a>Hög tillgänglighet
tooachieve med hög tillgänglighet (HA) i BizTalk-tjänster du använder mer än en instans i en viss region tooshare hello belastningen. Hög tillgänglighet i region med logic apps är inbyggda och kommer utan extra kostnad. En process för säkerhetskopiering och återställning krävs för out av region katastrofåterställning för B2B bearbetning i BizTalk-tjänst. I Logic Apps mellan region aktiv/passiv [DR kapaciteten](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md) tillhandahålls; som tillåter hello synkronisering av B2B-data över Integrationskonton i olika regioner för verksamhetskontinuitet.

## <a name="next"></a>Nästa
* [Vad är Logic Apps?](logic-apps-what-are-logic-apps.md)
* [Skapa din första logikapp](logic-apps-create-a-logic-app.md), eller kom snabbt igång med en [färdig mall](logic-apps-use-logic-app-templates.md)  
* [Visa alla hello tillgängliga kopplingar](../connectors/apis-list.md) du kan använda i en logikapp
