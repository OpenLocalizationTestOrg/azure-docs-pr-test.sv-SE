---
title: "Självstudier: Bearbeta EDIFACT fakturor med Azure BizTalk-tjänst | Microsoft Docs"
description: "Hur toocreate och konfigurera hello rutan kopplingen eller API-app och använda den i en logikapp i Azure App Service"
services: biztalk-services
documentationcenter: .net,nodejs,java
author: msftman
manager: erikre
editor: 
ms.assetid: 7e0b93fa-3e2b-4a9c-89ef-abf1d3aa8fa9
ms.service: biztalk-services
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/31/2016
ms.author: deonhe
ms.openlocfilehash: 4ca021c9084b9b6540c93ef15fc3d44be0966970
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-process-edifact-invoices-using-azure-biztalk-services"></a>Självstudier: Processen EDIFACT fakturor med hjälp av Azure BizTalk-tjänst

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Du kan använda hello BizTalk-Services-portalen tooconfigure och distribuera X12 och EDIFACT-avtal. I den här självstudiekursen kommer titta vi på hur toocreate ett EDIFACT-avtal för att utbyta fakturor mellan affärspartner. Den här självstudiekursen skrivs runt en slutpunkt till slutpunkt affärslösning som inbegriper två handelspartners Northwind och Contoso att utbyta EDIFACT-meddelanden.  

## <a name="sample-based-on-this-tutorial"></a>Exempel baserat på den här självstudiekursen
Den här självstudiekursen skrivs runt ett prov, **skicka EDIFACT fakturor med BizTalk-tjänst**, som är tillgängliga toodownload från hello [MSDN Code Gallery](http://go.microsoft.com/fwlink/?LinkId=401005). Du kan använda hello exemplet och gå igenom den här självstudiekursen toounderstand hur hello exemplet skapades. Eller så kan du använda den här självstudiekursen toocreate din egen lösning grunden. Den här kursen är riktad mot hello andra sättet så att du förstår hur den här lösningen har skapats. Dessutom så mycket som möjligt hello kursen är konsekvent med hello exemplet och använder hello samma namn för artefakter (till exempel scheman, transformeringar) som används i hello exemplet.  

> [!NOTE]
> Eftersom den här lösningen skickar ett meddelande från en EAI bridge tooan EDI bridge, återanvänds hello [BizTalk Services Bridge länkning exempel](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104) exempel.  
> 
> 

## <a name="what-does-hello-solution-do"></a>Vad är hello-lösningen?
I den här lösningen får Northwind EDIFACT fakturor från Contoso. Dessa fakturor är inte i ett standard EDI-format. Innan du skickar hello faktura tooNorthwind, så måste det vara omvandlade tooan EDIFACT (kallas även INVOIC) dokument. Erhåller, måste Northwind bearbeta hello EDIFACT faktura och returnera ett meddelande (kallas även CONTRL) tooContoso för kontrollen.

![][1]  

tooachieve scenariot business Contoso använder hello-funktioner som tillhandahålls med Microsoft Azure BizTalk-tjänst.

* Contoso använder EAI bryggor tootransform hello ursprungliga faktura tooEDIFACT INVOIC.
* Hej EAI bridge skickar sedan hello meddelandet tooan EDI skicka brygga som distribueras som en del av ett avtal som konfigurerats i hello BizTalk-Services-portalen.
* hello EDI skicka bridge bearbetar hello EDIFACT INVOIC och dirigerar den tooNorthwind.
* När du har fått hello faktura returnerar Northwind en CONTRL meddelande toohello EDI får brygga som distribueras som en del av hello avtal.  

> [!NOTE]
> Den här lösningen visar eventuellt också hur toouse batchbearbetning toosend hello fakturor i batchar, i stället för att skicka varje faktura separat.  
> 
> 

toocomplete hello scenariot kan vi använda Service Bus-köer toosend faktura från Contoso tooNorthwind eller ta emot bekräftelse från exempeldatabasen. Dessa köer kan skapas med ett klientprogram som är tillgänglig för hämtning och ingår i hello exempel paket som är tillgängliga som en del av den här kursen.  

## <a name="prerequisites"></a>Krav
* Du måste ha en Service Bus-namnrymd. Anvisningar om hur du skapar ett namnområde finns [hur man: skapa eller ändra en Service Bus-tjänsten Namespace](https://msdn.microsoft.com/library/azure/hh674478.aspx). Låt oss anta att du redan har ett namnområde för Service Bus etableras, kallas **edifactbts**.
* Du måste ha en prenumeration BizTalk-tjänst. Instruktioner finns i [skapa en BizTalk Service med hjälp av Azure klassiska portal](http://go.microsoft.com/fwlink/?LinkID=302280). För den här självstudiekursen kommer vi förutsätter att du har en BizTalk-tjänst-prenumeration som kallas **contosowabs**.
* Registrera prenumerationen BizTalk-tjänst för hello BizTalk-Services-portalen. Instruktioner finns i [registrerar en BizTalk Tjänstdistribution på hello BizTalk-Services-portalen](https://msdn.microsoft.com/library/hh689837.aspx)
* Du måste ha Visual Studio installerat.
* Du måste ha BizTalk Services SDK är installerat. Du kan hämta hello SDK från [http://go.microsoft.com/fwlink/?LinkId=235057](http://go.microsoft.com/fwlink/?LinkId=235057)  

## <a name="step-1-create-hello-service-bus-queues"></a>Steg 1: Skapa hello Service Bus-köer
Den här lösningen använder Service Bus-köer tooexchange meddelanden mellan affärspartner. Contoso och Northwind skickar du meddelanden toohello köer från där hello EAI-och/eller EDI bryggor använda dem. Du behöver tre Service Bus-köer för den här lösningen:

* **northwindreceive** – Northwind tar emot hello faktura från Contoso via den här kön.
* **contosoreceive** – Contoso tar emot hello bekräftelse från exempeldatabasen via den här kön.
* **avbruten** – alla avbrutna meddelanden är routade toothis kön. Meddelanden gör uppehåll om de inte under bearbetning.

Du kan skapa dessa Service Bus-köer med hjälp av ett klientprogram i hello exempel paketet.  

1. Hello plats dit du hämtade hello exempel, öppna **självstudiekursen skickar fakturor med BizTalk Services EDI Bridges.sln**.
2. Tryck på **F5** toobuild och starta hello **kursen klienten** program.
3. Ange hello Service Bus ACS namnområde, Utfärdarens namn och nyckeln för utfärdaren i hello-skärmen.
   
   ![][2]  
4. En meddelanderuta uppmanas att tre köer kommer att skapas i Service Bus-namnrymd. Klicka på **OK**.
5. Lämna hello kursen klienten körs. Öppna hello, klicka på **Service Bus** > ***Service Bus-namnrymd*** > **köer**, och kontrollera att hello tre köer skapas.  

## <a name="step-2-create-and-deploy-trading-partner-agreement"></a>Steg 2: Skapa och distribuera handelspartneravtal
Skapa ett handelspartneravtal mellan Contoso och Northwind. Ett handelspartneravtal definierar ett handel avtal mellan hello två affärspartner, till exempel vilka schemat toouse meddelande som messaging protocol toouse osv. Ett handelspartneravtal innehåller två EDI-bryggor, en toosend meddelanden tootrading partners (kallas hello **EDI skicka bridge**) och en tooreceive meddelanden från handelspartner (kallas hello **EDI får bridge**).

Hello gäller den här lösningen är hello EDI skicka bridge motsvarar toohello Skicka sida av hello avtal och är används toosend hello EDIFACT faktura från Contoso tooNorthwind. På liknande sätt hello EDI får bridge motsvarar toohello mottagarsidan hello avtal och är används tooreceive bekräftelser från exempeldatabasen.  

### <a name="create-hello-trading-partners"></a>Skapa hello handelspartner
toostart med, skapa handelspartner för Contoso och Northwind.  

1. I hello BizTalk-Services-portalen på hello **Partners** klickar du på **Lägg till**.
2. Hello nya partner på sidan Ange **Contoso** som Partnernamn och klicka sedan på **spara**.
3. Upprepa hello steg toocreate hello andra partner **Northwind**.  

### <a name="create-hello-agreement"></a>Skapa hello avtal
Handelspartneravtal skapas mellan företag-profiler för handelspartner. Den här lösningen använder hello standard partner profiler som skapas automatiskt när vi skapade hello partners.  

1. I hello BizTalk-Services-portalen klickar du på **avtal** > **Lägg till**.
2. I hello nya avtalet **allmänna inställningar** anger hello värden som visas i hello bilden nedan, och klicka sedan på **Fortsätt**.
   
   ![][3]  
   
   När du klickar på **Fortsätt**, två flikar **tar emot inställningar** och **skicka inställningar** blir tillgängliga.
3. Skapa hello skicka avtal mellan Contoso och Northwind. Det här avtalet styr hur Contoso skickar hello EDIFACT faktura tooNorthwind.
   
   1. Klicka på **skicka inställningar**.
   2. Behåll hello standardvärdena på hello **inkommande URL**, **transformera**, och **Batching** flikar.
   3. På hello **protokollet** under hello fliken **scheman** avsnittet laddar du upp hello **EFACT_D93A_INVOIC.xsd** schema. Det här schemat är tillgänglig med hello exempel paketet.
      
      ![][4]  
   4. På hello **Transport** anger hello information för hello Service Bus-köer. För hello sändningssidan avtalet, vi använder hello **northwindreceive** kö toosend hello EDIFACT faktura tooNorthwind och hello **avbruten** kö tooroute alla meddelanden som inte under bearbetning och Pausa. Du har skapat dessa köer i **steg 1: skapa hello Service Bus-köer** (i det här avsnittet).
      
      ![][5]  
      
      Under **Transport Inställningar > transporttypen** och **upphävande Meddelandeinställningar > transporttypen**väljer Azure Service Bus och ange värden för hello enligt hello bild.
4. Skapa hello får avtal mellan Contoso och Northwind. Det här avtalet styr hur Contoso tar emot hello bekräftelse från exempeldatabasen.
   
   1. Klicka på **ta emot inställningarna**.
   2. Behåll hello standardvärdena på hello **Transport** och **transformera** flikar.
   3. På hello **protokollet** under hello fliken **scheman** avsnittet laddar du upp hello **EFACT_4.1_CONTRL.xsd** schema. Det här schemat är tillgänglig med hello exempel paketet.
   4. På hello **väg** fliken, skapa ett filter tooensure som endast bekräftelser från exempeldatabasen är routade tooContoso. Under **vägar**, klickar du på **Lägg till** toocreate hello routning filter.
      
      ![][6]  
      
      1. Ange värden för **Regelnamn**, **väg regeln**, och **väg mål** enligt hello bild.
      2. Klicka på **Spara**.
   5. På hello **väg** igen och ange var pausat bekräftelser (bekräftelser som inte under bearbetning) dirigeras till. Ange hello transport typen tooAzure Service Bus, vidarebefordra måltypen för**kön**, autentisering skriver för**signatur för delad åtkomst** (SAS), ange hello SAS-anslutningssträng för hello Service Bus namnområde, och ange sedan hello könamnet som **avbruten**.
5. Klicka slutligen på **distribuera** toodeploy hello avtal. Obs hello slutpunkter där hello skicka och ta emot avtal distribueras.
   
   * På hello **skicka inställningar** fliken, under **inkommande URL**, Observera hello slutpunkt. toosend ett meddelande från Contoso tooNorthwind med hello EDI skicka bridge, måste du skicka ett meddelande toothis slutpunkt.
   * På hello **tar emot inställningar** fliken, under **Transport**, Observera hello slutpunkt. toosend ett meddelande från exempeldatabasen tooContoso med hello EDI få bridge, måste du skicka ett meddelande toothis slutpunkt.  

## <a name="step-3-create-and-deploy-hello-biztalk-services-project"></a>Steg 3: Skapa och distribuera hello BizTalk-Services-projekt
I föregående steg hello distribuerade hello EDI skicka och ta emot avtal tooprocess EDIFACT fakturor och bekräftelser. Dessa avtal kan endast bearbeta meddelanden som uppfyller toohello standard EDIFACT meddelandet schemat. Per hello scenario för den här lösningen skickar Contoso dock en faktura tooNorthwind i ett egna interna schema. Så innan hello-meddelande skickas toohello EDI skicka bridge, måste den konverteras från hello interna schema toohello standard EDIFACT faktura-schema. hello BizTalk Services EAI projekt gör detta.

hello BizTalk-tjänst-projektet **InvoiceProcessingBridge**, att transformeringar hello meddelandet ingår även som en del av hello-exempel som du hämtat. hello projektet innehåller följande artefakter hello:

* **INHOUSEINVOICE. XSD** – schemat för hello interna fakturan som skickas tooNorthwind.
* **EFACT_D93A_INVOIC. XSD** – schemat för hello standard EDIFACT faktura.
* **EFACT_4.1_CONTRL. XSD** – schemat för hello EDIFACT bekräftelse att Northwind skickar tooContoso.
* **INHOUSEINVOICE_to_D93AINVOIC. TRFM** – hello transformeringen som mappar hello interna faktura schema toohello standard EDIFACT faktura-schema.  

### <a name="create-hello-biztalk-services-project"></a>Skapa hello BizTalk-Services-projekt
1. Expandera hello InvoiceProcessingBridge projekt i hello Visual Studio-lösning, och öppna hello **MessageFlowItinerary.bcs** fil.
2. Klicka någonstans på hello arbetsytan och ange hello **BizTalk-tjänst-URL** hello egenskapen rutan toospecify prenumerationsnamn din BizTalk-tjänst. Till exempel `https://contosowabs.biztalk.windows.net`.
   
   ![][7]  
3. Dra från verktygslådan hello en **Xml One-Way Bridge** toohello arbetsytan. Ange hello **entitetsnamnet** och **relativ adress** egenskaper för hello överbrygga för**ProcessInvoiceBridge**. Dubbelklicka på **ProcessInvoiceBridge** tooopen hello bridge configuration yta.
4. Inom hello **meddelandetyper** klickar du på hello plus (**+**) knappen toospecify hello schemat för inkommande hello-meddelande. Eftersom hello inkommande meddelande för hello EAI bridge är alltid hello interna faktura, ställa in detta också**INHOUSEINVOICE**.
   
   ![][8]  
5. Klicka på hello **XML-transformeringen** form, och hello i egenskapsrutan för hello **Maps** egenskap, klickar du på ellipsknappen hello (**...** ) knappen. I hello **Maps markeringen** dialogrutan, Välj hello **INHOUSEINVOICE_to_D93AINVOIC** transformeringsfilen och klicka sedan på **OK**.
   
   ![][9]  
6. Gå tillbaka för**MessageFlowItinerary.bcs**, och dra från verktygslådan hello en **tvåvägs externa tjänstslutpunkten** toohello höger i hello **ProcessInvoiceBridge**. Ange dess **entitetsnamnet** egenskapen för**EDIBridge**.
7. Hello Solution Explorer, expandera hello **MessageFlowItinerary.bcs** och dubbelklicka på hello **EDIBridge.config** fil. Ersätt hello innehållet i hello **EDIBridge.config** med hello följande.
   
   > [!NOTE]
   > Varför behöver tooedit hello .config-filen? hello externa tjänstslutpunkten vi lagt till toohello bridge designerytan representerar hello EDI bryggor som vi tidigare har distribuerats. EDI bryggor är dubbelriktad bryggor med en skicka och ta emot sida. Hej är EAI bridge vi lagt till toohello bridge designer dock en enkelriktad brygga. Så toohandle hello annat meddelande exchange mönster av hello två platslänkbryggor, vi använder en anpassad bridge beteende genom att inkludera dess konfiguration i hello .config-fil. Dessutom hanterar hello beteende också hello toohello EDI skicka bridge autentiseringsslutpunkten. Detta beteende är tillgänglig som en separat exempelprogram på [BizTalk Services Bridge länkning exempel - EAI tooEDI](http://code.msdn.microsoft.com/BizTalk-Bridge-chaining-2246b104). Den här lösningen återanvänder hello exempel.  
   > 
   > 
   
   ```
   <?xml version="1.0" encoding="utf-8"?>
   <configuration>
     <system.serviceModel>
       <extensions>
         <behaviorExtensions>
           <add name="BridgeAuthentication" 
                 type="Microsoft.BizTalk.Bridge.Behaviour.BridgeBehaviorElement, Microsoft.BizTalk.Bridge.Behaviour, Version=1.0.0.0, Culture=neutral, PublicKeyToken=ae58f69b69495c05" />
         </behaviorExtensions>
       </extensions>
       <behaviors>
         <endpointBehaviors>
           <behavior name="BridgeAuthenticationConfiguration">
             <!-- Enter hello ACS namespace, issuer name and issuer secret of hello BizTalk Services deployment -->
             <BridgeAuthentication acsnamespace="[YOUR ACS NAMESPACE]" 
                                   issuername="owner" 
                                   issuersecret="[YOUR ACS SECRET]" />
             <webHttp />
           </behavior>
         </endpointBehaviors>
       </behaviors>
       <bindings>
         <webHttpBinding>
           <binding name="BridgeBindingConfiguration">
             <security mode="Transport" />
           </binding>
         </webHttpBinding>
       </bindings>
       <client>
         <clear />
         <!--
           Go BizTalk Portal > Agreement > Send Settings > Inbound URL
           Copy hello Endpoint URL and paste it in hello below address field
         -->
         <endpoint name="TwoWayExternalServiceEndpointReference1" 
                   address="[YOUR EDI BRIDGE SEND URI]" 
                   behaviorConfiguration="BridgeAuthenticationConfiguration" 
                   binding="webHttpBinding" 
                   bindingConfiguration="BridgeBindingConfiguration" 
                   contract="System.ServiceModel.Routing.IRequestReplyRouter" />
       </client>
     </system.serviceModel>
   </configuration>
   
   ```
8. Uppdatera hello EDIBridge.config filen tooinclude konfigurationsinformation
   
   * Under  *<behaviors>* anger hello ACS-namnområde och nyckel som är associerad med hello BizTalk-abonnemang.
   * Under  *<client>* , ange hello slutpunkt där hello EDI skicka avtal har distribuerats.
   
   Spara ändringarna och stänga hello konfigurationsfilen.
9. Hello verktygslådan, klicka på hello **Connector** och koppling hello **ProcessInvoiceBridge** och **EDIBridge** komponenter. Välj hello-koppling och egenskaper i rutan Ange **filtervillkor** för**matcha alla**. Detta säkerställer att alla meddelanden som bearbetas av hello EAI bridge dirigeras toohello EDI bridge.
   
   ![][10]  
10. Spara ändringarna toohello lösning.  

### <a name="deploy-hello-project"></a>Distribuera hello-projekt
1. Hämta och installera hello SSL-certifikat för prenumerationen BizTalk-tjänst på hello datorn där du skapade hello BizTalk-Services-projekt. Från, under BizTalk-tjänst klickar du på **instrumentpanelen**, och klicka sedan på **hämta SSL-certifikat**. Dubbelklicka på hello certifikatet och följ hello fråga toocomplete hello installation. Kontrollera att du installerar hello certifikatet under **betrodda rotcertifikatutfärdare** certifikatarkiv.
2. I Visual Studio Solution Explorer högerklickar du på hello **InvoiceProcessingBridge** projektet och klicka sedan på **distribuera**.
3. Ange värden för hello enligt hello bild och klicka sedan på **distribuera**. Du kan hämta hello ACS-autentiseringsuppgifter för BizTalk-tjänst genom att klicka på **anslutningsinformationen** hello BizTalk-tjänst instrumentpanel.
   
   ![][11]  
   
   Kopiera från hello utdatafönstret hello slutpunkt där hello EAI bridge distribueras, till exempel `https://contosowabs.biztalk.windows.net/default/ProcessInvoiceBridge`. Du behöver den här slutpunkts-URL senare.  

## <a name="step-4-test-hello-solution"></a>Steg 4: Testa hello-lösning
I det här avsnittet vi titta på hur tootest hello lösning med hjälp av hello **kursen klienten** program som ingår i hello exempel.  

1. Tryck på F5 toostart hello i Visual Studio **kursen klienten**.
2. hello-skärmen måste ha hello värden förinställd från hello steg där vi skapade hello Service Bus-köer. Klicka på **Nästa**.
3. Ange autentiseringsuppgifter för ACS för BizTalk-abonnemang och hello slutpunkter i hello nästa fönstret där EAI- och EDI (får) bryggor distribueras.
   
   Du hade kopierade hello EAI bridge slutpunkt i hello föregående steg. För EDI får bridge slutpunkt i hello BizTalk-Services-portalen kan gå toohello avtal > få Inställningar > Transport > slutpunkt.
   
   ![][12]  
4. Hello nästa på i fönstret under Contoso, hello **skicka interna faktura** knappen. Öppna dialogrutan i hello fil, öppna hello INHOUSEINVOICE.txt filen. Granska hello innehållet hello-filen och klicka sedan på **OK** toosend hello faktura.
   
   ![][13]  
5. Några få sekunder hello tas faktura emot på Northwind. Klicka på hello **Visa meddelande** länk toosee hello faktura tas emot av Northwind. Observera hur hello fakturan som tagits emot av Northwind finns i standard EDIFACT-schema när hello sändas av Contoso var ett interna schema.
   
   ![][14]  
6. Välj hello faktura och klicka sedan på **skicka bekräftelse**. Observera att hello interchange ID är samma i hello mottagna faktura och hello bekräftelse skickas hello dialogrutan som öppnas. Klicka på OK i hello **skicka bekräftelse** dialogrutan.
   
   ![][15]  
7. Hello bekräftelse har tagits emot på Contoso inom några sekunder.
   
   ![][16]  

## <a name="step-5-optional-send-edifact-invoice-in-batches"></a>Steg 5 (valfritt): skicka EDIFACT faktura i batchar
BizTalk-tjänster EDI bryggor stöder också massbearbetning av utgående meddelanden. Den här funktionen är användbart för att ta emot partners som föredrar tooreceive en grupp med meddelanden (uppfyller vissa villkor) i stället för enskilda meddelanden.

hello viktigaste aspekt när du arbetar med batchar är hello faktiska versionen av hello batch, kallas även hello versionen kriterier. hello versionen villkor kan baseras på hur hello mottagande partnern vill tooreceive meddelanden. Om batchbearbetning är aktiverad, skickar inte hello EDI bridge hello utgående meddelande toohello får partner tills hello versionen villkor är uppfyllt. Till exempel batching villkor baserat på meddelandet storlek levererar en batch endast när n meddelanden grupperas. Ett batch-villkor kan också vara tidsbaserade, så att en batch skickas med en viss tid varje dag. I den här lösningen försök vi hello meddelandestorlek baserade villkor.

1. Klicka på hello avtal som du skapade tidigare i hello BizTalk-Services-portalen. Klicka på Skicka Inställningar > batchbearbetning > Lägg till Batch.
2. Batch-namn, ange **InvoiceBatch**, ange en beskrivning och klicka sedan på **nästa**.
3. Ange ett batch-villkor som definierar vilka meddelanden måste vara batchar. I den här lösningen batch vi alla meddelanden. Så, Välj hello använda avancerade alternativ för definitioner och ange **1 = 1**. Detta är ett villkor som alltid är true och därför alla meddelanden som ska grupperas. Klicka på **Nästa**.
   
   ![][17]  
4. Ange ett villkor för batch-versionen. Hello listrutan, Välj **MessageCountBased**, och för **antal**, ange **3**. Det innebär att en grupp med tre meddelanden ska skickas tooNorthwind. Klicka på **Nästa**.
   
   ![][18]  
5. Granska hello sammanfattning och klicka sedan på **spara**. Klicka på **distribuera** tooredeploy hello avtal.
6. Gå tillbaka toohello **kursen klienten**, klickar du på **skicka interna faktura**, och följ hello prompter toosend hello faktura. Du ser att någon faktura tas emot på Northwind eftersom hello batchstorlek inte är uppfyllt. Upprepa detta två gånger, så att du har tre faktura meddelanden som skickas tooNorthwind. Det här uppfyller hello batch release villkor 3 meddelanden och du bör nu se en faktura på Northwind.

<!--Image references-->
[1]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-1.PNG  
[2]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-2.PNG  
[3]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-3.PNG
[4]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-4.PNG  
[5]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-5.PNG  
[6]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-6.PNG  
[7]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-7.PNG  
[8]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-8.PNG
[9]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-9.PNG  
[10]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-10.PNG  
[11]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-11.PNG  
[12]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-12.PNG  
[13]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-13.PNG
[14]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-14.PNG  
[15]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-15.PNG  
[16]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-16.PNG  
[17]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-17.PNG  
[18]: ./media/biztalk-process-edifact-invoice/process-edifact-invoices-with-auzure-bts-18.PNG

