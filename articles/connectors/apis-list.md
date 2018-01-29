---
title: "Anslutningsappar för Azure Logic Apps | Microsoft Docs"
description: "Välj mellan alla tillgängliga Microsoft-anslutningsappar för att bygga och skapa logikappar"
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
ms.openlocfilehash: 948b91a9fabc3ab3c4d6708968a88cb9d203b171
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/19/2018
---
# <a name="connectors-list"></a>Lista över anslutningsappar
Du hittar utlösare och åtgärder som definierats av Swagger-beskrivningen för varje anslutningsapp, plus eventuella begränsningar i [Information om anslutningsapp](/connectors/).

Anslutningsappar är en viktig del när du skapar logikappar. Med hjälp av de här anslutningsapparna, kan du utöka dina lokala och molnbaserade program för att göra olika saker med data som du skapar och data du redan har. Anslutningsapparna finns tillgängliga antingen som inbyggda åtgärder eller hanterade anslutningsappar.

**Inbyggda åtgärder**: Logic Apps-motorn innehåller inbyggda åtgärder för att kommunicera med slutpunkter och utföra uppgifter. Du kan till exempel använda åtgärderna för att anropa HTTP-slutpunkter, Azure Functions och Azure API-hanteringsåtgärder samt manipulera meddelanden med dataåtgärder och variabler.

**Hanterade anslutningsappar**: ge åtkomst till API:er för olika tjänster genom att skapa API-anslutningar som Logic Apps-tjänsten är värd för och hanterar. Hanterade anslutningsappar kan delas in i följande kategorier:

* **Standardanslutningsappar**: Automatiskt tillgängliga och ingår när du använder logikappar. Några exempel är Service Bus, Power BI, OneDrive och många fler.

* **Lokala anslutningsappar**: Anslut till serverprogram lokalt med hjälp av den [lokala datagatewayen][gatewaydoc]. Lokala anslutningsappar inkluderar anslutningar till serverprogram som SharePoint Server, SQL Server, Oracle DB, filresurser och andra.

* **Anslutningsappar för integrationskonton**: De här är tillgängliga när du köper ett integrationskonto. Med sådana anslutningsappar kan du omvandla och validera XML, bearbeta business-to-business-meddelanden med AS2 / X12 / EDIFACT och koda och avkoda flata filer. Om du arbetar med BizTalk Server är de här anslutningsapparna en bra metod för att utöka dina BizTalk-arbetsflöden till Azure.  

    BizTalk Server har också en [Logic Apps-adapter](https://msdn.microsoft.com/library/mt787163.aspx) för mottagning från en logikapp och sändning till en logikapp.

* **Enterprise-anslutningsappar**: Innehåller MQ och SAP. Tillgängligt för en extra kostnad. 

Mer information om kostnader finns i [prisinformationen](https://azure.microsoft.com/pricing/details/logic-apps/) och [prissättningsmodellen](../logic-apps/logic-apps-pricing.md) för Logic Apps. 

## <a name="popular-connectors"></a>Populära anslutningsappar
Det finns tusentals program och miljontals körningar som bearbetar data och information med hjälp av dessa anslutningsappar. 

### <a name="built-in-actions"></a>Inbyggda åtgärder
Logic Apps-motorn innehåller åtgärder som kan manipulera data, kommunicera över HTTP och styra flödet för logikappsdefinitionen. Några av dessa åtgärder är:

| |  |  |  |
| --- | --- | --- | --- |
| [![API-ikon][HTTPicon]<br/>**HTTP**][httpdoc] | Använd logikappar för att kommunicera med valfri slutpunkt över HTTP.| [![API-ikon][Azure-Functionsicon]<br/>**Azure Functions**][azure-functionsdoc] | Skapa funktioner som kör anpassade fragment för C# eller node.js och använd sedan använda dessa funktioner i logikapparna.  |
| [![API-ikonen][HTTP-Requesticon]<br/>**Begäran**][HTTP-Requestdoc] | Ger en anropsbar HTTPS-URL som vanligtvis används som en webhook i andra program. När logikappen tar emot en begäran till denna URL startar logikappen. | [![API-ikon][Recurrenceicon]<br/>**Schemalägg**][recurrencedoc] | Starta logikappar baserat på enkla eller avancerade återkommande scheman. Skapa till exempel scheman som är så enkla som upprepa varje dag till upprepa varje timme den sista fredagen i varje månad mellan 09.00 och 17.00. |
| [![API-ikon][CallLogicApp-icon]<br/>**Anropa<br/>Logikapp**][nested-logic-appdoc] | Anropa en kapslad logikapp. Alla logikappar med en begäransutlösare kan anropas som en kapslad logikapp.| [![API-ikon][API/Web-Appicon]<br/>**API-app**][api/web-appdoc] | Anropa en API-app i App Service. API-appar med swagger renderas precis som alla andra förstklassiga åtgärder.|

### <a name="standard-connectors"></a>Anslutningsappar av standardtyp
I följande tabell anges de populäraste och några favoriter hos våra användare:

| |  |  |  |
| --- | --- | --- | --- |
| [![API-ikon][AzureBlobStorageicon]<br/>**Azure Blob<br/>Storage**][AzureBlobStoragedoc] | Om du vill automatisera alla aktiviteter med ditt lagringskonto bör du titta på den här anslutningsappen. Stöder CRUD-åtgärder (skapa, läsa, uppdatera, ta bort). | [![API-ikon][Dynamics-365icon]<br/>**Dynamics 365<br/>CRM Online**][Dynamics-365doc] | Detta är ett av de mest efterfrågade anslutningsprogrammen. Den har utlösare och åtgärder för att automatisera arbetsflöden med leads och mycket mer. |
| [![API-ikon][Event-Hubs-icon]<br/>**Event Hubs**][event-hubs-doc] | Använda och publicera händelser i en Event Hub. Du kan till exempel hämta utdata från din logikapp med Event Hubs och sedan skicka dem till en leverantör av realtidsanalys. | [![API-ikon][FTPicon]<br/>**FTP**][FTPdoc] | Om FTP-servern är tillgänglig från internet kan du automatisera arbetsflöden att arbeta med filer och mappar. <br/><br/>Det finns också SFTP med SFTP-anslutningsappen. |
| [![API-ikon][Office-365-Outlookicon]<br/>**Office 365<br/>Outlook**][office365-outlookdoc] | Massor av utlösare och många fler åtgärder för att använda e-post och händelser för Office 365 i dina arbetsflöden. <br/><br/>Den här anslutningsappen innehåller ett *e-postmeddelande med godkännande* för att godkänna semesteransökningar, utgiftsrapporter och så vidare. <br/><br/>Office 365-användare är också tillgängliga med anslutningsappen för Office 365-användare.| [![API-ikon][Salesforceicon]<br/>**Salesforce**][salesforcedoc] | Logga enkelt in med ditt Salesforce-konto för att få åtkomst till objekt som leads med mera. | 
| [![API-ikon][Service-Busicon]<br/>**Service Bus**][Service-Busdoc] | Den populäraste anslutningsappen i Logic Apps, den innehåller utlösare och åtgärder för att göra asynkrona meddelanden och publicera/prenumerera på köer, prenumerationer och avsnitt. |  [![API-ikon][SharePointicon]<br/>**SharePoint<br/>Online**][SharePointdoc] | Om du gör något med SharePoint och kan dra nytta av automatisering rekommenderar vi att du tittar på den här anslutningsappen. Kan användas med lokal SharePoint och SharePoint Online. |
| [![API-ikon][SQL-Servericon]<br/>**SQL Server**][SQL-Serverdoc] | En av de mest använda anslutningsapparna. Den kan ansluta till en lokal SQL Server och en Azure SQL Database. | [![API-ikon][Twittericon]<br/>**Twitter**][Twitterdoc] | Logga enkelt in med ett Twitter-konto, och påbörja sedan ett arbetsflöde när en ny tweet postas. Spara sedan dessa tweets till en SQL Database eller SharePoint-lista. | 

### <a name="on-premises-connectors"></a>Lokala anslutningsappar 

Lokala anslutnings appar ger åtkomst till data i lokala servrar.  Om du vill skapa en anslutning till en lokal server så krävs en [lokal datagateway][gatewaydoc] som erbjuder en säker kommunikationskanal utan nätverksinfrastruktur som behöver konfigureras.  Några av anslutningsapparna är:

|  |  |  |  |
| --- | --- | --- | --- |
| [![API-ikon][db2icon]<br/>**DB2**][db2doc] | [![API-ikon][oracle-DB-icon]<br/>**Oracle DB**][oracle-db-doc] | [![API-ikon][sharepointicon]<br/>**SharePoint</br> Server**][sharepointserver] | [![API-ikon][filesystem-icon]<br/>**File</br> System**][filesystemdoc] |
[![API-ikon][sql-servericon]<br/>**SQL</br> Server**][sql-serverdoc] | ![API-ikon][Biztalk-Servericon]<br/>**BizTalk</br> Server**| |

### <a name="integration-account-connectors"></a>Anslutningar för integrationskonton 

Enterprise-integrationspaketet (EIP) innehåller anslutningsappar som är välkända för BizTalk Server-communityn. När du köper ett [integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) får du också följande anslutningsappar: 

|  |  |  |  |
| --- | --- | --- | --- |
| [![API-ikon][as2icon]<br/>**AS2</br>-avkodning**][as2decode] | [![API-ikon][as2icon]<br/>**AS2</br>-avkodning**][as2encode] | [![API-ikon][x12icon]<br/>**EDIFACT</br>-avkodning**][EDIFACTdecode] | [![API-ikon][x12icon]<br/>**EDIFACT</br>-kodning**][EDIFACTencode] |
[![API-ikon][flatfileicon]<br/>**Flat fil</br>-kodning**][flatfiledoc] | [![API-ikon][flatfiledecodeicon]<br/>**Flat fil</br>-avkodning**][flatfiledecodedoc] | [![API-ikon][integrationaccounticon]<br/>**Integrationskonto<br/>**][integrationaccountdoc] | [![API-ikon][xmltransformicon]<br/>**Transformera<br/>XML**][xmltransformdoc] |
| [![API-ikon][x12icon]<br/>**X12</br>-avkodning**][x12decode] | [![API-ikon][x12icon]<br/>**X12</br>-kodning**][x12encode] | [![API-ikon][xmlvalidateicon]<br/>**XML<br/>-verifiering**][xmlvalidatedoc] | |

### <a name="enterprise-connectors"></a>Enterprise-anslutningsappar

Anslut dina företagsprogram med dina logikappar.

|  |  |
| --- | --- |
|[![API-ikon][MQicon]<br/>**MQ**][mqdoc]|[![API-ikon][SAPicon]<br/>**SAP**][sapconnector]|

> [!TIP]
> För att komma igång med Azure Logic Apps innan du registrerar dig för ett Azure-konto går du till [Prova Logic Apps](https://tryappservice.azure.com/?appservice=logic). Du kan skapa en kortvarig startlogikapp omedelbart. Inga kreditkort krävs. Inga åtaganden.

## <a name="connectors-as-triggers-and-actions"></a>Anslutningsappar som utlösare och åtgärder

En **utlösare** startar eller kör en instans på din logikapp. Flera anslutningsappar innehåller utlösare som kan meddela din app när specifika händelser äger rum. FTP-anslutningsappen har till exempel utlösaren `OnUpdatedFile` som meddelar din logikapp när en fil har uppdaterats. 

Logic Apps innehåller följande typer av utlösare:  

* *Sökningsutlösare*: Dessa utlösare avsöker din tjänst med en angiven frekvens för att söka efter nya data. 

    När det finns nya data körs en ny instans av din logikapp med data som indata. 

* *Push-utlösare*: de här utlösarna lyssnar efter data på en slutpunkt eller efter att en händelse ska inträffa och utlöser därefter en ny instans av din logikapp.

* *Upprepningsutlösare*: Den här utlösaren instantierar en instans av din logikapp på en app i ett förutbestämt schema.

Anslutningsapparna tillhandahåller också **åtgärder** som du kan använda i ditt arbetsflöde. Din logikapp kan till exempel söka efter data och sedan använda dessa data senare i din logikapp. Mer specifikt kan du låsa upp kunddata från en SQL Database och sean använda dessa kunddata för att bygga ditt arbetsflöde. 

> [!TIP]
> I [översikten över anslutningsappar](connectors-overview.md) finns mer information om utlösare och åtgärder. 


## <a name="message-manipulation-actions"></a>Åtgärder för att manipulera meddelanden

Logikappar innehåller inbyggda åtgärder som kan ändra eller manipulera nyttolastdata. Den inbyggda anslutningsappen **Dataåtgärder** innehåller följande åtgärder: 

| | |
|---|---|
| **Skriva** | Skapa eller generera värden eller objekt att använda senare eller när du skapar ditt arbetsflöde. Du kan exempelvis redigera ett JSON-objekt med värden från flera steg, eller beräkna en konstant för senare referens i en logikappkörning. |
| **Skapa CSV-tabell**<br/>**Skapa HTML-tabell** | Omvandla en matrisresultatuppsättning till en CSV- eller HTML-tabell. Lägg exempelvis till en åtgärd för att lista poster för CRM och lägg till ett filter för poster som lagts till idag. Skicka sedan resultatet som en HTML-tabell i ett e-postmeddelande. |
| **Filtrera matris** (fråga) | Filtrera ett resultat på de poster som intresserar dig. Du kan exempelvis söka på alla tweets med `#Azure` och sedan ”filtrera” de returnerade tweetsen så att de endast visar resultat som är `Tweeted_by_followers > 50`. |
| **Anslut dig** | Anslut dig till en matris med en avgränsare. Till exempel returnerar åtgärden för att identifiera nyckelfraser en matris med nyckelfraser. Du kan "ansluta" dig till dem med en `,` eller liknande. Så istället för `["Some", "Phrase"]` har du `"Some, Phrase"`. |
| **Parsa JSON** | Parsa ut och få åtkomst till värden från ett JSON-objekt i designern. Om till exempel Azure-funktionen returnerar en JSON-nyttolast kan du parsa den för att få åtkomst till JSON-egenskaperna senare i ett annat steg. Åtgärden validerar också att JSON matchar det angivna schemat vid körning. | 
| **Välj** | Välj vissa egenskaper i en matris för vidare bearbetning. Om du ”listar poster” från SQL och den returnerar 15 kolumner ska du bara välja några av kolumnerna för ytterligare bearbetning. Utdata är en matris som endast innehåller de egenskaper du väljer. |

## <a name="custom-connectors-and-azure-certification"></a>Anpassade anslutningsappar och Azure-certifiering 

För att anropa API:er som kör anpassad kod eller som inte finns tillgängliga som anslutningsappar, kan du utöka Logic Apps-plattformen genom att [skapa REST-baserade API-appar](../logic-apps/logic-apps-create-api-app.md). Du kan också skapa dina egna [anpassade anslutningsappar](../logic-apps/custom-connector-overview.md) som kan göras tillgängliga för alla logikappar i din prenumeration.

Om du vill göra dina anpassade API-appar allmänt tillgängliga för användning i Azure, kan du [skicka in dina anslutningsappar för Microsoft-certifiering](../logic-apps/custom-connector-submit-certification.md).

## <a name="get-help"></a>Få hjälp

I [Azure Logic Apps-forumet](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) kan du ställa frågor, besvara frågor och sedan vad andra Azure Logic Apps-användare håller på med.

På [webbplatsen för Logic Apps-användarfeedback](http://aka.ms/logicapps-wish) kan du hjälpa till med att förbättra Azure Logic Apps och anslutningsapparna genom att rösta på förslag eller komma med egna förslag på förbättringar.

Saknar vi ett avsnitt om anslutningsprogram eller någon annan viktig information? Om Ja, hjälpa oss genom att bidra till våra befintliga avsnitt eller skriv ett nytt. Vår dokumentation är öppen källkod och finns på GitHub. Kom igång på vår [GitHub-lagringsplats](https://github.com/Microsoft/azure-docs). 

## <a name="next-steps"></a>Nästa steg
* [Skapa din första logiska app](../logic-apps/quickstart-create-first-logic-app-workflow.md)
* [Skapa anpassade API:er för logikappar](../logic-apps/logic-apps-create-api-app.md)
* [Övervaka dina logikappar](../logic-apps/logic-apps-monitor-your-logic-apps.md)

<!--Connectors Documentation-->

[gatewaydoc]: ../logic-apps/logic-apps-gateway-connection.md "Anslut till datakällor lokalt från logikappar med lokala datagatewayer"
[api/web-appdoc]: ../logic-apps/logic-apps-custom-hosted-api.md "Integrera logikappar med App Service API Apps"
[azureblobstoragedoc]: ./connectors-create-api-azureblobstorage.md "Hantera filer i blobbehållaren med Azure Blob Storage Connector"
[azure-functionsdoc]: ../logic-apps/logic-apps-azure-functions.md "Integrera logikappar med Azure Functions"
[db2doc]: ./connectors-create-api-db2.md "Anslut till IBM DB2 i molnet eller lokalt. Uppdatera en rad, hämta en tabell och mycket mer"
[Dynamics-365doc]: ./connectors-create-api-crmonline.md "Ansluta till Dynamics CRM Online så att du kan arbeta med CRM Online-data"
[event-hubs-doc]: ./connectors-create-api-azure-event-hubs.md "Anslut till Azure Event Hubs. Ta emot och skicka händelser mellan logikappar och händelsehubbar"
[filesystemdoc]: ../logic-apps/logic-apps-using-file-connector.md "Ansluta till ett lokalt filsystem"
[ftpdoc]: ./connectors-create-api-ftp.md "Ansluta till en FTP-/FTPS-server för FTP-aktiviteter, till exempel överföring, hämtning och borttagning av filer och mycket mer"
[httpdoc]: ./connectors-native-http.md "Göra HTTP-anrop med HTTP-anslutningsappen"
[http-requestdoc]: ./connectors-native-reqres.md "Lägga till åtgärder för HTTP-begäranden och svar i logikappar"
[http-swaggerdoc]: ./connectors-native-http-swagger.md "Göra HTTP-anrop med HTTP + Swagger-anslutningsappen"
[informixdoc]: ./connectors-create-api-informix.md "Anslut till Informix i molnet eller lokalt. Läsa en rad, lista tabeller och annat"
[nested-logic-appdoc]: ../logic-apps/logic-apps-http-endpoint.md "Integrera logikappar med kapslade arbetsflöden"
[office365-outlookdoc]: ./connectors-create-api-office365-outlook.md "Anslut till ditt Office 365-konto. Skicka och ta emot e-post, hantera din kalender och dina kontakter med mera"
[oracle-db-doc]: ./connectors-create-api-oracledatabase.md "Anslut till en Oracle-databas för att lägga till, infoga och ta bort rader med mera"
[mqdoc]: ./connectors-create-api-mq.md "Ansluta till MQ lokalt eller Azure och skicka och ta emot meddelanden"
[recurrencedoc]:  ./connectors-native-recurrence.md "Utlösa återkommande åtgärder för logikappar"
[salesforcedoc]: ./connectors-create-api-salesforce.md "Anslut till ditt Salesforce-konto. Hantera konton, leads, affärsmöjligheter och mycket mer"
[sapconnector]: ../logic-apps/logic-apps-using-sap-connector.md "Ansluta till ett lokalt SAP-system"
[service-busdoc]: ./connectors-create-api-servicebus.md "Skicka meddelanden från Service Bus-köer och Service Bus-ämnen och ta emot meddelanden från Service Bus-köer och Service Bus-prenumerationer"
[sharepointdoc]: ./connectors-create-api-sharepointonline.md "Anslut till SharePoint Online. Hantera dokument, listobjekt och annat"
[sharepointserver]: ./connectors-create-api-sharepointserver.md "Anslut till lokala SharePoint-servrar. Hantera dokument, listobjekt och annat"
[sql-serverdoc]: ./connectors-create-api-sqlazure.md "Anslut till Azure SQL Database eller SQL Server. Skapa, uppdatera, hämta och ta bort poster i en SQL-databastabell."
[twitterdoc]: ./connectors-create-api-twitter.md "Anslut till Twitter. Hämta tidslinjer, publicera tweets och mycket mer"
[webhookdoc]: ./connectors-native-webhook.md "Lägga till webhook-åtgärder och utlösare i logikappar"

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


[boxDoc]: ./connectors-create-api-box.md "Anslut till Box. Överföra, hämta, ta bort, visa filer och mycket mer "
[delaydoc]: ./connectors-native-delay.md "Utföra fördröjda åtgärder"
[dropboxdoc]: ./connectors-create-api-dropbox.md "Anslut till Dropbox. Överföra, hämta, ta bort, visa filer och mycket mer "
[facebookdoc]: ./connectors-create-api-facebook.md "Anslut till Facebook. Publicera på en tidslinje, hämta ett sidflöde och mycket mer"
[githubdoc]: ./connectors-create-api-github.md "Ansluta till GitHub och spåra problem"
[google-drivedoc]: ./connectors-create-api-googledrive.md "Ansluta till Google Drive så att du kan arbeta med dina data"
[google-sheetsdoc]: ./connectors-create-api-googlesheet.md "Ansluta till Google Kalkylark så att du kan göra ändringar i kalkylblad"
[google-tasksdoc]: ./connectors-create-api-googletasks.md "Ansluter till Google Tasks så att du kan hantera uppgifter"
[google-calendardoc]: ./connectors-create-api-googlecalendar.md "Ansluter till Google Kalender och kan hantera kalendern."
[http-responsedoc]: ./connectors-native-reqres.md "Lägga till åtgärder för HTTP-begäranden och svar i logikappar"
[instagramdoc]: ./connectors-create-api-instagram.md "Anslut till Instagram. Utlösa eller utföra åtgärder vid händelser"
[mailchimpdoc]: ./connectors-create-api-mailchimp.md "Anslut till ditt MailChimp-konto. Hantera och automatisera e-post"
[mandrilldoc]: ./connectors-create-api-mandrill.md "Ansluta till Mandrill för kommunikation"
[microsoft-translatordoc]: ./connectors-create-api-microsofttranslator.md "Anslut till Microsoft Translator. Översätta text, identifiera språk och annat" 
[office365-usersdoc]: ./connectors-create-api-office365-users.md 
[office365-videodoc]: ./connectors-create-api-office365-video.md "Hämta videoinformation, videolistor, videokanaler och uppspelnings-URL:er för Office 365-videor"
[onedrivedoc]: ./connectors-create-api-onedrive.md "Anslut till ditt personliga Microsoft OneDrive. Överföra, ta bort och lista filer med mera"
[onedrive-for-businessdoc]: ./connectors-create-api-onedriveforbusiness.md "Anslut till företagets Microsoft OneDrive. Överföra, ta bort och lista filer och mycket mer"
[outlook.comdoc]: ./connectors-create-api-outlook.md "Anslut till din Outlook-postlåda. Hantera din e-post, dina kalender, dina kontakter och mycket mer"
[project-onlinedoc]: ./connectors-create-api-projectonline.md "Anslut till Microsoft Project Online. Hantera projekt, uppgifter, resurser och mycket mer"
[querydoc]: ./connectors-native-query.md "Välja och filtrera matriser med frågeåtgärden"
[rssdoc]: ./connectors-create-api-rss.md "Publicera och hämta flödesobjekt, utlös åtgärder när ett nytt objekt publiceras på en RSS-feed."
[sendgriddoc]: ./connectors-create-api-sendgrid.md "Anslut till SendGrid. Skicka e-post och hantera mottagarlistor"
[sftpdoc]: ./connectors-create-api-sftp.md "Anslut till ditt SFTP-konto. Överföra, hämta och ta bort filer med mera"
[slackdoc]: ./connectors-create-api-slack.md "Ansluta till Slack och skicka meddelanden till Slack-kanaler"
[smtpdoc]: ./connectors-create-api-smtp.md "Ansluta till en SMTP-server och skicka e-post med bifogade filer"
[sparkpostdoc]: ./connectors-create-api-sparkpost.md "Ansluter till SparkPost för kommunikation"
[trellodoc]: ./connectors-create-api-trello.md "Anslut till Trello. Hantera dina projekt och organisera vad du vill med vem du vill"
[twiliodoc]: ./connectors-create-api-twilio.md "Anslut till Twilio. Skicka och hämta meddelanden, hämta tillgängliga nummer, hantera inkommande telefonnummer och mycket mer"
[wunderlistdoc]: ./connectors-create-api-wunderlist.md "Anslut till Wunderlist. Hantera uppgifter och uppgiftslistor, håll dig uppdaterad och mycket mer"
[yammerdoc]: ./connectors-create-api-yammer.md "Anslut till Yammer. Skicka meddelanden, få nya meddelanden och mycket mer"
[youtubedoc]: ./connectors-create-api-youtube.md "Anslut till YouTube. Hantera videor och kanaler"


<!--Icon references-->
[appFiguresicon]: ./media/apis-list/appfigures.png
[AppServices-icon]: ./media/apis-list/AppServices.png
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
[FileSystem-icon]: ./media/apis-list/filesystem.png
[FileSystemIcon]: ./media/apis-list/filesystem.png
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
[API/Web-Appicon]: ./media/apis-list/appservices.png
[Azure-Functionsicon]: ./media/apis-list/function.png
[CallLogicApp-icon]: ./media/apis-list/calllogicapp.png
[Delayicon]: ./media/apis-list/delay.png
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
