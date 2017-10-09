---
title: "aaaConnectors för Logikappar i Azure | Microsoft Docs"
description: "Välj från alla hello tillgängliga Microsoft-hanterade anslutningar toobuild och skapa logikappar"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: f1f1fd50-b7f9-4d13-824a-39678619aa7a
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: mandia; ladocs
ms.openlocfilehash: d681d13d642e6e1512d1f8ab0e1078a194b5da83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connectors-list"></a>Lista över anslutningsappar
> [!TIP]
> Hej [A-Z lista](#az) visar alla tillgängliga hello kopplingar som du kan du använda i dina Logic Apps (i det här avsnittet). [Information om anslutningen](/connectors/) visar en lista över alla utlösare och åtgärder som definierats i hello swagger och visar också några gränser för varje koppling.

Anslutningsappar är en viktig del när du skapar logikappar. Med hjälp av dessa anslutningar, kan du expandera din lokala och molnet program toodo olika saker med data som du skapar och data som du redan har. hello kopplingar är tillgängliga i hello följande kategorier: 

* **Standardanslutningsappar**: Automatiskt tillgängliga och ingår när du använder logikappar. Vissa exempel är Service Bus, Power BI, Oracle Database, OneDrive och många fler.

* **Anslutningsappar för integrationskonton**: De här är tillgängliga när du köper ett integrationskonto. Med sådana anslutningsappar kan du omvandla och validera XML, bearbeta business-to-business-meddelanden med AS2 / X12 / EDIFACT och koda och avkoda flata filer. Om du arbetar med BizTalk Server sedan dessa kopplingar är en bra passar tooexpand BizTalk-arbetsflöden i Azure.  

    BizTalk Server har också en [Logic Apps kortet](https://msdn.microsoft.com/library/mt787163.aspx) som innehåller från en logikapp, skicka och ta emot tooa logikapp.

* **Enterprise-anslutningsappar**: Innehåller MQ och SAP. Tillgängligt för en extra kostnad. 

[Priser för Logic Apps](https://azure.microsoft.com/pricing/details/logic-apps/) och [priser modellen](../logic-apps/logic-apps-pricing.md) innehåller mer information om hello kostnader. 

## <a name="popular-connectors"></a>Populära anslutningsappar
Det finns tusentals program och miljontals körningar som bearbetar data och information med hjälp av dessa anslutningsappar. hello i den följande tabellen listas hello mest populära och vissa Favoriter med våra användare:

| |  |  |  |
| --- | --- | --- | --- |
| [![API Icon][AzureBlobStorageicon]<br/>**Azure Blob<br/>Storage**][AzureBlobStoragedoc] | Om du vill tooautomate alla uppgifter med ditt lagringskonto, bör du titta på den här anslutningen. Stöder CRUD-åtgärder (skapa, läsa, uppdatera, ta bort). | [![API Icon][Azure-Functionsicon]<br/>**Azure Functions**][azure-functionsdoc] | Skapa funktioner som kör anpassade fragment för C# eller node.js och använd sedan använda dessa funktioner i logikapparna.  |
| [![API Icon][Dynamics-365icon]<br/>**Dynamics 365<br/>CRM Online**][Dynamics-365doc] | En av hello de flesta vanliga för kopplingar. Den har utlösare och åtgärder toohelp Automatisera arbetsflöden leads och mycket mer. | [![API Icon][Event-Hubs-icon]<br/>**Event Hubs**][event-hubs-doc] | Använda och publicera händelser i en Event Hub. Du kan till exempel hämta utdata från din logikapp med Händelsehubbar och sedan skicka hello utdata tooa leverantör av realtidsanalys. |
| [![API Icon][FTPicon]<br/>**FTP**][FTPdoc] | Om FTP-servern är tillgänglig från hello internet, och sedan kan du automatisera arbetsflöden toowork med filer och mappar. <br/><br/>SFTP är också tillgänglig i hello SFTP-anslutningen. | [![API Icon][HTTPicon]<br/>**HTTP**][httpdoc] | Använda logic apps toocommunicate med valfri slutpunkt via HTTP. |
| [![API Icon][Office-365-Outlookicon]<br/>**Office 365<br/>Outlook**][office365-outlookdoc] | Mängd utlösare, och många fler åtgärder toouse Office 365 e-post och händelser i dina arbetsflöden. <br/><br/>Denna koppling inkluderar en *godkännande e-post* åtgärd tooapprove semester begäranden utgifter rapporter och så vidare. <br/><br/>Office 365-användare finns också med hello användare av Office 365-kopplingen.| [![API Icon][HTTP-Requesticon]<br/>**Begäran/svar**][HTTP-Requestdoc] | Den här anslutningsappen tillhandahåller en HTTPS-URL. När hello logikapp tar emot en begäran toothis URL, startar hello logikapp. |
| [![API Icon][Salesforceicon]<br/>**Salesforce**][salesforcedoc] | Enkelt logga in med ditt Salesforce-konto tooget åtkomst tooobjects, till exempel Leads, med mera. |  [![API Icon][Service-Busicon]<br/>**Service Bus**][Service-Busdoc] | hello populäraste anslutningen inom logic apps kan det innehåller utlösare och åtgärder toodo asynkrona meddelanden och publicera/prenumerera med köer, prenumerationer och ämnen. |
|  [![API Icon][SharePointicon]<br/>**SharePoint<br/>Online**][SharePointdoc] | Om du gör något med SharePoint och kan dra nytta av automatisering rekommenderar vi att du tittar på den här anslutningsappen. Kan användas med lokal SharePoint och SharePoint Online. | [![API Icon][SQL-Servericon]<br/>**SQL Server**][SQL-Serverdoc] | En av hello används oftast kopplingar, den kan ansluta tooan lokala SQL Server och en Azure SQL Database. | 
| [![API Icon][Twittericon]<br/>**Twitter**][Twitterdoc] | Logga enkelt in med ett Twitter-konto, och påbörja sedan ett arbetsflöde när en ny tweet postas. Spara dessa tweets tooa SQL-databas eller en SharePoint-lista. | | | 

## <a name="integration-account-connectors"></a>Anslutningar för integrationskonton 

hello Enterprise Integration Pack (EIP) innehåller kopplingar som är kända toohello BizTalk Server-communityn. När du köper en [integrering konto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md), du får också hello följande kopplingar: 

|  |  |  |  |
| --- | --- | --- | --- |
| [![API Icon][as2icon]<br/>**AS2-</br>avkodning**][as2decode] | [![API Icon][as2icon]<br/>**AS2-</br>kodning**][as2encode] | [![API Icon][x12icon]<br/>**EDIFACT-</br>avkodning**][EDIFACTdecode] | [![API Icon][x12icon]<br/>**EDIFACT-</br>kodning**][EDIFACTencode] |
[![API Icon][flatfileicon]<br/>**Flatfils</br>kodning**][flatfiledoc] | [![API Icon][flatfiledecodeicon]<br/>**Flatfils</br>avkodning**][flatfiledecodedoc] | [![API Icon][integrationaccounticon]<br/>**Integrations<br/>konto**][integrationaccountdoc] | [![API Icon][xmltransformicon]<br/>**Omvandla<br/>XML**][xmltransformdoc] |
| [![API Icon][x12icon]<br/>**X12-</br>avkodning**][x12decode] | [![API Icon][x12icon]<br/>**X12-</br>kodning**][x12encode] | [![API Icon][xmlvalidateicon]<br/>**XML-<br/>verifiering**][xmlvalidatedoc] | |

## <a name="enterprise-connectors"></a>Enterprise-anslutningsappar

Ansluta tooyour företagsprogram inom dina logic apps.

|  |  |
| --- | --- |
|[![API Icon][MQicon]<br/>**MQ**][mqdoc]|[![API Icon][SAPicon]<br/>**SAP**][sapconnector]|


## <a name="az"></a>Fullständig lista, A–Z

[Information om anslutningen](/connectors/) lista över alla utlösare och åtgärder som definierats i hello swagger och visar också några gränser för varje koppling.

| | | | | | | | | | | | | |
|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [**1**](#1) | [**A**](#a) | [**B**](#b) | [**C**](#c) | [**D**](#d) | [**E**](#e) | [**F**](#f) | [**G**](#g) | [**H**](#h) | [**I**](#i) | [**J**](#j) | [**L**](#l) | [**M**](#m) |
| [**N**](#n) | [**O**](#o) | [**P**](#p) | [**R**](#r) | [**S**](#s) | [**T**](#t) | [**U**](#u) | [**V**](#v) | [**W**](#w) | [**X**](#x) | [**Y**](#y) | [**Z**](#z) | | 

| | |
|---|---|
|<a name="1"></a>10to8-schemaläggning<br/><br/><a name="a"></a>Act!<br/>Adobe Creative Cloud<br/>appFigures<br/>[AS2][as2doc]<br/>Asana<br/>Azure Active Directory (AD)<br/>Azure API Management<br/>Azure Apptjänster<br/>Azure Application<br/>Azure Automation<br/>[Azure Blob Storage][azureblobstoragedoc]<br/>Azure Data Lake<br/>Azure DocumentDB (Cosmos DB)<br/>[Azure Functions][azure-functionsdoc]<br/>[Azure Logic Apps][nested-logic-appdoc]<br/>AzureML<br/>Azure Queues<br/>Azure Resource Manager<br/>[Azure SQL Database][sql-serverdoc]<br/><br/><a name="b"></a>Basecamp 2<br/>Basecamp 3<br/>Batch<br/>Benchmark Email<br/>Bing Search<br/>Bitbucket<br/>Bitly<br/>BizTalk Server<br/>Blogger<br/>Box<br/>Buffer<br/><br/><a name="c"></a>Calendly<br/>Campfire<br/>Capsule CRM<br/>Chatter<br/>Cognito-formulär<br/>Prissättning för Cognitive Services – API för visuellt innehåll<br/>Ansikts-API för Cognitive Services<br/>Cognitive Services LUIS<br/>Cognitive Services-textanalys<br/>Common Data Service<br/>Konvertering av innehåll<br/>Kontroll-avsluta<br/>[Custom APIs / web apps][api/web-appdoc]<br/><br/><a name="d"></a>Dataåtgärder<br/>[DB2][db2doc]<br/>Disqus<br/>DocuSign<br/>Do Until<br/>Dropbox<br/>[Dynamics 365 CRM Online][Dynamics-365doc]<br/>Dynamics 365 for Financials<br/>Dynamics 365 for Operations<br/>Dynamics NAV<br/><br/><a name="e"></a>Easy Redmine<br/>EDIFACT<br/>[Event Hubs][event-hubs-doc]<br/>Eventbrite<br/><br/><a name="f"></a>Facebook<br/>[Filsystem][filesystemdoc]<br/>[Flat fil][flatfiledoc]<br/>FreshBooks<br/>Freshdesk<br/>Freshservice<br/>[FTP][ftpdoc]<br/><br/><a name="g"></a>GitHub<br/>Gmail<br/>Google Calendar<br/>Google Contacts<br/>Google Drive<br/>Google-blad<br/>Google-uppgifter<br/>GoToMeeting<br/>GoToTraining<br/>GoToWebinar<br/><br/><a name="h"></a>Harvest<br/>HelloSign<br/>HipChat<br/>[HTTP][httpdoc]<br/>[HTTP + Swagger][http-swaggerdoc]<br/>[HTTP-webhook][webhookdoc]<br/><br/><a name="i"></a>[Informix][informixdoc]<br/>Infusionsoft<br/>Inoreader<br/>Insightly<br/>Instagram<br/>Instapaper<br/>Integrationskonto<br/>Intercom | <a name="j"></a>JotForm<br/>JIRA<br/><br/><a name="l"></a>LeanKit<br/>LiveChat<br/><br/><a name="m"></a>MailChimp<br/>Mandrill<br/>Medel<br/>Microsoft Forms<br/>Microsoft Teams<br/>Microsoft Translator<br/>[MQ][mqdoc]<br/>MSN Väder<br/>Muhimbi PDF<br/>MySQL<br/><br/><a name="n"></a>Nexmo<br/><br/><a name="o"></a>[Office 365 Outlook][office365-outlookdoc]<br/>Office 365-användare<br/>Office 365 Video<br/>OneDrive<br/>OneDrive för företag<br/>OneNote (företag)<br/>[Oracle Database][oracle-db-doc]<br/>Outlook Customer Manager<br/>Outlook-uppgifter<br/>Outlook.com<br/><br/><a name="p"></a>PagerDuty<br/>Parserr<br/>Paylocity<br/>Pinterest<br/>Pipedrive<br/>Pivotal Tracker<br/>Planner<br/>PostgreSQL<br/>Power BI<br/>Project Online<br/><br/><a name="r"></a>Redmine<br/>[Begäran/svar][http-requestdoc]<br/>RSS<br/><br/><a name="s"></a>[Salesforce][salesforcedoc]<br/>[SAP Application Server][sapconnector]<br/>[SAP Message Server][sapconnector]<br/>[Schema][recurrencedoc]<br/>Omfång<br/>SendGrid<br/>Skicka meddelanden toobatch<br/>[Service Bus][service-busdoc]<br/>SFTP<br/>[SharePoint Online][sharepointdoc]<br/>[SharePoint Server][sharepointserver]<br/>Slack<br/>Smartsheet<br/>SMTP<br/>SparkPost<br/>[SQL Server][sql-serverdoc]<br/>Stripe<br/>SurveyMonkey<br/>Switch Case<br/><br/><a name="t"></a>Teamwork Projects<br/>Teradata<br/>Todoist<br/>Toodledo<br/>[Omvandla XML][xmltransformdoc]<br/>Trello<br/>Twilio<br/>[Twitter][twitterdoc]<br/>Typeform<br/><br/><a name="u"></a>UserVoice<br/><br/><a name="v"></a>Variabler<br/>Vimeo<br/>Visual Studio Team Services<br/><br/><a name="w"></a>WebMerge<br/>WordPress<br/>Wunderlist<br/><br/><a name="x"></a>[X12][x12doc]<br/>[XML-verifiering][xmlvalidatedoc]<br/><br/><a name="y"></a>Yammer<br/>YouTube<br/><br/><a name="z"></a>Zendesk |

> [!TIP]
> Gå tooget igång med Azure Logikappar innan du registrerar dig för ett Azure-konto för[försök Logic Apps](https://tryappservice.azure.com/?appservice=logic). Du kan skapa en kortvarig startlogikapp omedelbart. Inget kreditkort krävs, och du gör inga åtaganden.

## <a name="connectors-as-triggers-and-actions"></a>Anslutningsappar som utlösare och åtgärder

En **utlösare** startar eller kör en instans på din logikapp. Flera anslutningsappar innehåller utlösare som kan meddela din app när specifika händelser äger rum. Hello FTP-anslutningen har till exempel hello `OnUpdatedFile` utlösaren som startar logikappen när en fil har uppdaterats. 

Logic apps är hello följande typer av utlösare:  

* *Avsöka utlösare*: dessa utlösare avsöker din tjänst med en frekvens som är angiven toocheck för nya data. 

    När det finns nya data körs en ny instans av logikappen med hello data som indata. 

* *Push-utlösare*: dessa utlösare lyssnar efter data på en slutpunkt eller för en händelse utlöser toohappen, sedan en ny instans av logikappen.

* *Upprepningsutlösare*: Den här utlösaren instantierar en instans av din logikapp på en app i ett förutbestämt schema.

Anslutningsapparna tillhandahåller också **åtgärder** som du kan använda i ditt arbetsflöde. Din logikapp kan till exempel söka efter data och sedan använda dessa data senare i din logikapp. Mer specifikt kan du söka efter kunddata från en SQL-databas och sedan använda den här kunden data toobuild arbetsflödet. 

> [!TIP]
> I [översikten över anslutningsappar](connectors-overview.md) finns mer information om utlösare och åtgärder. 


## <a name="message-manipulation-actions"></a>Åtgärder för att manipulera meddelanden

Logikappar innehåller inbyggda åtgärder som kan ändra eller manipulera nyttolastdata. hello inbyggda **dataåtgärder** koppling inkluderar hello följande åtgärder: 

| | |
|---|---|
| **Skriva** | Skapa eller generera värden eller objekt toouse senare, eller som du skapar arbetsflödet. Du kan exempelvis skapa en JSON-objekt med värden från flera steg eller beräkna en konstant tooreference senare i en logikapp som körs. |
| **Skapa CSV-tabell**<br/>**Skapa HTML-tabell** | Omvandla en matrisresultatuppsättning till en CSV- eller HTML-tabell. Till exempel lägga till hello CRM ”lista” åtgärd och Lägg till ett filter för poster som lagts till i dag. Skicka hello resultat som en HTML-tabell i ett e-postmeddelande. |
| **Filtrera matris** (fråga) | Filtrera en resultatet set toohello transaktioner som intresserar dig. Till exempel söka efter alla tweets med `#Azure`, och sedan returneras ”filter” Hej tweets tooonly returnerar resultat som är `Tweeted_by_followers > 50`. |
| **Anslut dig** | Anslut dig till en matris med en avgränsare. Till exempel returnerar hello identifiera nyckeln fraser åtgärden en matris av viktiga fraser. Du kan "ansluta" dig till dem med en `,` eller liknande. Så istället för `["Some", "Phrase"]` har du `"Some, Phrase"`. |
| **Parsa JSON** | Tolka och komma åt värden från ett JSON-objekt i hello designer. Till exempel om din Azure-funktion returnerar en JSON-nyttolast kan sedan du parsa den tooaccess hello JSON egenskaper senare i ytterligare ett steg. hello åtgärd validerar också att hello JSON matchar hello angivna schemat vid körning. | 
| **Välj** | Välj vissa egenskaper i en matris för vidare bearbetning. Om du ”listar poster” från SQL och den returnerar 15 kolumner ska du bara välja några av kolumnerna för ytterligare bearbetning. hello-utdata är en matris som endast innehåller hello egenskaper som du väljer. |

## <a name="custom-connectors-and-azure-certification"></a>Anpassade anslutningsappar och Azure-certifiering 

toocall i API: er som kör anpassad kod eller inte är tillgängliga som kopplingar kan du utöka hello Logic Apps plattform av [skapa REST-baserad API Apps som anpassade kopplingar](../logic-apps/logic-apps-create-api-app.md). 

Om du vill toomake din anpassade API Apps offentliga och tillgängliga toouse i Azure, sedan skicka nomineringarna-toohello [Microsoft Azure-certifierad programmet](https://azure.microsoft.com/marketplace/programs/certified/logic-apps/).

## <a name="get-help"></a>Få hjälp

tooask frågor, besvara frågor och se vilka andra användare i Azure Logic Apps gör gå toohello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp förbättra Azure Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Logic Apps användaren feedbackwebbplats](http://aka.ms/logicapps-wish).

Saknar vi ett avsnitt om anslutningsprogram eller någon annan viktig information? Om Ja, hjälpa oss genom att lägga till befintliga artiklar för tooour eller skriva egna. Vår dokumentation är öppen källkod och finns på GitHub. Kom igång på vår [GitHub-lagringsplats](https://github.com/Microsoft/azure-docs). 

## <a name="next-steps"></a>Nästa steg
* [Skapa din första logiska app](../logic-apps/logic-apps-create-a-logic-app.md)
* [Skapa anpassade API:er för logikappar](../logic-apps/logic-apps-create-api-app.md)
* [Övervaka dina logikappar](../logic-apps/logic-apps-monitor-your-logic-apps.md)

<!--Connectors Documentation-->

[api/web-appdoc]: ../logic-apps/logic-apps-custom-hosted-api.md "Integrera logikappar med App Service API Apps"
[azureblobstoragedoc]: ./connectors-create-api-azureblobstorage.md "Hantera filer i blobbehållaren med Azure Blob Storage Connector"
[azure-functionsdoc]: ../logic-apps/logic-apps-azure-functions.md "Integrera logikappar med Azure Functions"
[db2doc]: ./connectors-create-api-db2.md "Ansluta tooIBM DB2 i hello molnet eller lokalt. Uppdatera en rad, hämta en tabell och mycket mer"
[Dynamics-365doc]: ./connectors-create-api-crmonline.md "Anslut tooDynamics CRM Online så att du kan arbeta med CRM Online-data"
[event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Ansluta tooAzure Händelsehubbar. Ta emot och skicka händelser mellan logikappar och händelsehubbar"
[filesystemdoc]: ../logic-apps/logic-apps-using-file-connector.md "Ansluta tooan lokalt filsystem"
[ftpdoc]: ./connectors-create-api-ftp.md "Ansluta tooan FTP / FTPS-server för FTP-aktiviteter, t.ex. ladda upp, hämta, ta bort filer och mycket mer"
[httpdoc]: ./connectors-native-http.md "HTTP-anrop med hello HTTP-anslutningen"
[http-requestdoc]: ./connectors-native-reqres.md "Lägga till åtgärder för HTTP-begäranden och -svar tooyour logikappar"
[http-swaggerdoc]: ./connectors-native-http-swagger.md "Göra HTTP-anrop med HTTP + Swagger-anslutningsappen"
[informixdoc]: ./connectors-create-api-informix.md "Ansluta tooInformix i hello molnet eller lokalt. Läs en rad och lista hello tabeller"
[nested-logic-appdoc]: ../logic-apps/logic-apps-http-endpoint.md "Integrera logikappar med kapslade arbetsflöden"
[office365-outlookdoc]: ./connectors-create-api-office365-outlook.md "Ansluta tooyour Office 365-konto. Skicka och ta emot e-post, hantera din kalender och dina kontakter med mera"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Ansluta tooan Oracle-databasen tooadd, infoga, ta bort rader och mycket mer"
[mqdoc]: ./connectors-create-api-mq.md "Ansluta tooMQ lokal eller Azure, och skicka och ta emot meddelanden"
[recurrencedoc]:  ./connectors-native-recurrence.md "Utlösa återkommande åtgärder för logikappar"
[salesforcedoc]: ./connectors-create-api-salesforce.md "Ansluta tooyour Salesforce-konto. Hantera konton, leads, affärsmöjligheter och mycket mer"
[sapconnector]: ../logic-apps/logic-apps-using-sap-connector.md "Ansluta tooan lokal SAP-systemet"
[service-busdoc]: ./connectors-create-api-servicebus.md "Skicka meddelanden från Service Bus-köer och Service Bus-ämnen och ta emot meddelanden från Service Bus-köer och Service Bus-prenumerationer"
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "Ansluta tooSharePoint Online. Hantera dokument, listobjekt och annat"
[sharepointserver]: ./connectors-create-api-sharepointserver.md "Ansluta tooSharePoint på lokal server. Hantera dokument, listobjekt och annat"
[sql-serverdoc]: ./connectors-create-api-sqlazure.md "Ansluta tooAzure SQL Database eller SQLServer. Skapa, uppdatera, hämta och ta bort poster i en SQL-databastabell."
[twitterdoc]: ./connectors-create-api-twitter.md "Ansluta tooTwitter. Hämta tidslinjer, publicera tweets och mycket mer"
[webhookdoc]: ./connectors-native-webhook.md "Lägg till Webhook åtgärder och utlösare tooyour logikappar"

[as2doc]: ../logic-apps/logic-apps-enterprise-integration-as2.md "Läs mer om AS2 för Enterprise-integration."
[x12doc]: ../logic-apps/logic-apps-enterprise-integration-x12.md "Läs mer om X12 för Enterprise-integration"
[flatfiledoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Läs mer om platta filer för Enterprise-integration."
[flatfiledecodedoc]:../logic-apps/logic-apps-enterprise-integration-flatfile.md "Läs mer om platta filer för Enterprise-integration."
[xmlvalidatedoc]: ../logic-apps/logic-apps-enterprise-integration-xml-validation.md "Läs mer om XML-verifiering för Enterprise-integration."
[xmltransformdoc]: ../logic-apps/logic-apps-enterprise-integration-transform.md "Läs mer om transformeringar för Enterprise-integration."
[as2decode]: ../logic-apps/logic-apps-enterprise-integration-as2-decode.md "Läs mer om AS2-avkodning för Enterprise-integration"
[as2encode]:../logic-apps/logic-apps-enterprise-integration-as2-encode.md "Läs mer om AS2-kodning för Enterprise-integration"
[X12decode]: ../logic-apps/logic-apps-enterprise-integration-X12-decode.md "Läs mer om X12-avkodning för Enterprise-integration"
[X12encode]: ../logic-apps/logic-apps-enterprise-integration-X12-encode.md "Läs mer om X12-kodning för Enterprise-integration"
[EDIFACTdecode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-decode.md "Läs mer om EDIFACT-avkodning för Enterprise-integration"
[EDIFACTencode]: ../logic-apps/logic-apps-enterprise-integration-EDIFACT-encode.md "Läs mer om EDIFACT-kodning för Enterprise-integration"
[integrationaccountdoc]: ../logic-apps/logic-apps-enterprise-integration-metadata.md "Kolla upp scheman, kartor, partner med mera i ditt integrationskonto"


[boxDoc]: ./connectors-create-api-box.md "Ansluta tooBox. Överföra, hämta, ta bort, visa filer och mycket mer "
[delaydoc]: ./connectors-native-delay.md "Utföra fördröjda åtgärder"
[dropboxdoc]: ./connectors-create-api-dropbox.md "Ansluta tooDropbox. Överföra, hämta, ta bort, visa filer och mycket mer "
[facebookdoc]: ./connectors-create-api-facebook.md "Ansluta tooFacebook. Bokför tooa tidslinje, hämta ett Sidflöde och mycket mer"
[githubdoc]: ./connectors-create-api-github.md "Ansluta tooGitHub och spåra problem"
[google-drivedoc]: ./connectors-create-api-googledrive.md "Anslut tooGoogleDrive så att du kan arbeta med dina data"
[google-sheetsdoc]: ./connectors-create-api-googlesheet.md "Anslut tooGoogle blad så att du kan ändra dina blad"
[google-tasksdoc]: ./connectors-create-api-googletasks.md "Ansluter tooGoogle uppgifter så att du kan hantera dina uppgifter"
[google-calendardoc]: ./connectors-create-api-googlecalendar.md "Ansluter tooGoogle kalender och hantera kalendern."
[http-responsedoc]: ./connectors-native-reqres.md "Lägga till åtgärder för HTTP-begäranden och -svar tooyour logikappar"
[instagramdoc]: ./connectors-create-api-instagram.md "Ansluta tooInstagram. Utlösa eller utföra åtgärder vid händelser"
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "Ansluta tooyour MailChimp-konto. Hantera och automatisera e-post"
[mandrilldoc]: ./connectors-create-api-mandrill.md "Ansluta tooMandrill för kommunikation"
[microsoft-translatordoc]: ./connectors-create-api-microsofttranslator.md "Ansluta tooMicrosoft översättare. Översätta text, identifiera språk och annat" 
[office365-usersdoc]: ./connectors-create-api-office365-users.md 
[office365-videodoc]: ./connectors-create-api-office365-video.md "Hämta videoinformation, videolistor, videokanaler och uppspelnings-URL:er för Office 365-videor"
[onedrivedoc]: ./connectors-create-api-onedrive.md "Ansluta tooyour privata Microsoft OneDrive. Överföra, ta bort och lista filer med mera"
[onedrive-for-businessdoc]: ./connectors-create-api-onedriveforbusiness.md "Ansluta tooyour business Microsoft OneDrive. Överföra, ta bort och lista filer och mycket mer"
[outlook.comdoc]: ./connectors-create-api-outlook.md "Ansluta tooyour postlåda i Outlook. Hantera din e-post, dina kalender, dina kontakter och mycket mer"
[project-onlinedoc]: ./connectors-create-api-projectonline.md "Ansluta tooMicrosoft Project Online. Hantera projekt, uppgifter, resurser och mycket mer"
[querydoc]: ./connectors-native-query.md "Välj och filtrera matriser med hello frågeåtgärden"
[rssdoc]: ./connectors-create-api-rss.md "Publicera och hämta flödesobjekt, utlösa åtgärder när ett nytt objekt är publicerade tooan RSS-feed."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "Ansluta tooSendGrid. Skicka e-post och hantera mottagarlistor"
[sftpdoc]: ./connectors-create-api-sftp.md "Ansluta tooyour SFTP-konto. Överföra, hämta och ta bort filer med mera"
[slackdoc]: ./connectors-create-api-slack.md "Ansluta tooSlack och skicka meddelanden tooSlack kanaler"
[smtpdoc]: ./connectors-create-api-smtp.md "Ansluta tooa SMTP-servern och skicka e-post med bifogade filer"
[sparkpostdoc]: ./connectors-create-api-sparkpost.md "Ansluter tooSparkPost för kommunikation"
[trellodoc]: ./connectors-create-api-trello.md "Ansluta tooTrello. Hantera dina projekt och organisera vad du vill med vem du vill"
[twiliodoc]: ./connectors-create-api-twilio.md "Ansluta tooTwilio. Skicka och hämta meddelanden, hämta tillgängliga nummer, hantera inkommande telefonnummer och mycket mer"
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Ansluta tooWunderlist. Hantera uppgifter och uppgiftslistor, håll dig uppdaterad och mycket mer"
[yammerdoc]: ./connectors-create-api-yammer.md "Ansluta tooYammer. Skicka meddelanden, få nya meddelanden och mycket mer"
[youtubedoc]: ./connectors-create-api-youtube.md "Ansluta tooYouTube. Hantera videor och kanaler"


<!--Icon references-->
[appFiguresicon]: ./media/apis-list/appfigures.png
[Asanaicon]: ./media/apis-list/asana.png
[Azure-Automation-icon]: ./media/apis-list/azure-automation.png
[AzureBlobStorageicon]: ./media/apis-list/azureblob.png
[Azure-Data-Lake-icon]: ./media/apis-list/azure-data-lake.png
[Azure-DocumentDBicon]: ./media/apis-list/azure-documentdb.png
[Azure-MLicon]: ./media/apis-list/azureml.png
[Azure-Resource-Manager-icon]: ./media/apis-list/azure-resource-manager.png
[Azure-Queues-icon]: ./media/apis-list/azure-queues.png
[Basecamp-3icon]: ./media/apis-list/basecamp.png
[Bitbucket-icon]: ./media/apis-list/bitbucket.png
[Bitlyicon]: ./media/apis-list/bitly.png
[BizTalk-Servericon]: ./media/apis-list/biztalk.png
[Bloggericon]: ./media/apis-list/blogger.png
[Boxicon]: ./media/apis-list/box.png
[Campfireicon]: ./media/apis-list/campfire.png
[Cognitive-Services-Text-Analyticsicon]: ./media/apis-list/cognitiveservicestextanalytics.png
[DB2icon]: ./media/apis-list/db2.png
[Dropboxicon]: ./media/apis-list/dropbox.png
[Dynamics-365icon]: ./media/apis-list/dynamicscrmonline.png
[Dynamics-365-for-Financialsicon]: ./media/apis-list/madeira.png
[Dynamics-365-for-Operationsicon]: ./media/apis-list/dynamicsax.png
[Easy-Redmineicon]: ./media/apis-list/easyredmine.png
[Event-Hubs-icon]: ./media/apis-list/eventhubs.png
[Facebookicon]: ./media/apis-list/facebook.png
[FTPicon]: ./media/apis-list/ftp.png
[GitHubicon]: ./media/apis-list/github.png
[Google-Calendaricon]: ./media/apis-list/googlecalendar.png
[Google-Driveicon]: ./media/apis-list/googledrive.png
[Google-Sheetsicon]: ./media/apis-list/googlesheet.png
[Google-Tasksicon]: ./media/apis-list/googletasks.png
[HideKeyicon]: ./media/apis-list/hidekey.png
[HipChaticon]: ./media/apis-list/hipchat.png
[Informixicon]: ./media/apis-list/informix.png
[Insightlyicon]: ./media/apis-list/insightly.png
[Instagramicon]: ./media/apis-list/instagram.png
[Instapapericon]: ./media/apis-list/instapaper.png
[JIRAicon]: ./media/apis-list/jira.png
[MailChimpicon]: ./media/apis-list/mailchimp.png
[Mandrillicon]: ./media/apis-list/mandrill.png
[Microsoft-Translatoricon]: ./media/apis-list/microsofttranslator.png
[MQicon]: ./media/apis-list/mq.png
[Office-365-Outlookicon]: ./media/apis-list/office365.png
[Office-365-Usersicon]: ./media/apis-list/office365users.png
[Office-365-Videoicon]: ./media/apis-list/office365video.png
[OneDriveicon]: ./media/apis-list/onedrive.png
[OneDrive-for-Businessicon]: ./media/apis-list/onedriveforbusiness.png
[Oracle-DB-icon]: ./media/apis-list/oracle-db.png
[Outlook.comicon]: ./media/apis-list/outlook.png
[PagerDutyicon]: ./media/apis-list/pagerduty.png
[Pinteresticon]: ./media/apis-list/pinterest.png
[Project-Onlineicon]: ./media/apis-list/projectonline.png
[Redmineicon]: ./media/apis-list/redmine.png
[RSSicon]: ./media/apis-list/rss.png
[Common-Data-Serviceicon]: ./media/apis-list/runtimeservice.png
[Salesforceicon]: ./media/apis-list/salesforce.png
[SAPicon]: ./media/apis-list/sap.png
[SendGridicon]: ./media/apis-list/sendgrid.png
[Service-Busicon]: ./media/apis-list/servicebus.png
[SFTPicon]: ./media/apis-list/sftp.png
[SharePointicon]: ./media/apis-list/sharepointonline.png
[Slackicon]: ./media/apis-list/slack.png
[Smartsheeticon]: ./media/apis-list/smartsheet.png
[SMTPicon]: ./media/apis-list/smtp.png
[SparkPosticon]: ./media/apis-list/sparkpost.png
[SQL-Servericon]: ./media/apis-list/sql.png
[Todoisticon]: ./media/apis-list/todoist.png
[Trelloicon]: ./media/apis-list/trello.png
[Twilioicon]: ./media/apis-list/twilio.png
[Twittericon]: ./media/apis-list/twitter.png
[Vimeoicon]: ./media/apis-list/vimeo.png
[Visual-Studio-Team-Servicesicon]: ./media/apis-list/visualstudioteamservices.png
[WordPressicon]: ./media/apis-list/wordpress.png
[Wunderlisticon]: ./media/apis-list/wunderlist.png
[Yammericon]: ./media/apis-list/yammer.png
[YouTubeicon]: ./media/apis-list/youtube.png

<!-- Primitive Icons -->
[API/Web-Appicon]: ./media/apis-list/api.png
[Azure-Functionsicon]: ./media/apis-list/function.png
[Delayicon]: ./media/apis-list/delay.png
[FileSystemIcon]: ./media/apis-list/filesystem.png
[HTTPicon]: ./media/apis-list/http.png
[HTTP-Requesticon]: ./media/apis-list/request.png
[HTTP-Responseicon]: ./media/apis-list/response.png
[HTTP-Swaggericon]: ./media/apis-list/http_swagger.png
[Nested-Logic-Appicon]: ./media/apis-list/workflow.png
[Recurrenceicon]: ./media/apis-list/recurrence.png
[Queryicon]: ./media/apis-list/query.png
[Webhookicon]: ./media/apis-list/webhook.png

<!-- EIP Icons -->
[as2icon]: ./media/apis-list/as2.png
[x12icon]: ./media/apis-list/x12new.png
[flatfileicon]: ./media/apis-list/flatfileencoding.png
[flatfiledecodeicon]: ./media/apis-list/flatfiledecoding.png
[xmlvalidateicon]: ./media/apis-list/xmlvalidation.png
[xmltransformicon]: ./media/apis-list/xsltransform.png
[integrationaccounticon]: ./media/apis-list/integrationaccount.png
