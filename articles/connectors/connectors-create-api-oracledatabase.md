---
title: aaaAdd hello Oracle-databas connector i dina Logic Apps i Azure | Microsoft Docs
description: "Använd hello Oracle-databas kopplingen i en logikapp"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: mandia; ladocs
ms.openlocfilehash: 8a802a6c4782e210ff71848614152cb46ba5d651
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-oracle-database-connector"></a>Kom igång med hello Oracle Database connector

Med hello Oracle Database connector kan skapa du organisationens arbetsflöden som använder data i den befintliga databasen. Den här anslutningen kan ansluta tooan lokal Oracle-databas eller en virtuell Azure-dator med Oracle-databas har installerats. Med den här anslutningen kan du:

* Skapa ditt arbetsflöde genom att lägga till en ny databas för kunden tooa kunder eller uppdatera en ordning i en order-databas.
* Använd åtgärder tooget en rad med data, infoga en ny rad och även ta bort. Till exempel när en post har skapats i Dynamics CRM Online (en utlösare), sedan infoga en rad i en Oracle-databas (en åtgärd). 

Det här avsnittet visar hur toouse hello connector för Oracle-databas i en logikapp.

## <a name="prerequisites"></a>Krav

* Oracle-versioner stöds: 
    * Oracle 9 och senare
    * Oracle-klientprogramvaran 8.1.7 eller senare

* Installera hello lokala datagateway. [Ansluta tooon lokala data från logikappar](../logic-apps/logic-apps-gateway-connection.md) visar hello steg. hello-gateway är obligatoriska tooconnect tooan lokal Oracle-databas eller en virtuell Azure-dator med Oracle DB installerad. 

    > [!NOTE]
    > hello lokala datagateway fungerar som en brygga och ger en säker dataöverföring mellan lokala data (data som inte är i hello moln) och dina logic apps. hello samma gateway kan användas med flera tjänster och flera datakällor. Därför kanske behöver du bara tooinstall hello gateway en gång.

* Installera hello Oracle-klient på hello datorn där du installerade hello lokala datagateway. Tänk tooinstall hello 64-bitars Oracle Data Provider för .NET från Oracle:  

  [64-bitars ODAC 12c version 4 (12.1.0.2.4) för Windows x64](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > Om hello Oracle-klienten inte är installerad, uppstår ett fel när du toocreate eller Använd hello-anslutning. Se hello vanliga fel i det här avsnittet.


## <a name="add-hello-connector"></a>Lägg till hello-koppling

> [!IMPORTANT]
> Den här anslutningen har inte utlösare. Det finns endast åtgärder. Så när du skapar din logikapp, lägga till en annan utlösare toostart din logikapp som **schema - upprepning**, eller **begäranden / svar - svar**. 

1. I hello [Azure-portalen](https://portal.azure.com), skapa en tom logikapp.

2. Hello början av din logikapp, Välj hello **begäranden / svar - begäran** utlösare: 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. Välj **Spara**. När du sparar genereras automatiskt en begärd URL. 

4. Välj **nytt steg**, och välj **lägga till en åtgärd**. Skriv i `oracle` toosee hello tillgängliga åtgärder: 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > Det är också hello snabbaste sättet toosee hello utlösare och åtgärder som är tillgängliga för alla anslutningar. Ange i en del av hello Kopplingsnamn `oracle`. hello designer visas alla utlösare och åtgärder. 

5. Välj någon av hello åtgärder, exempelvis **Oracle-databasen - Get-raden**. Välj **Anslut via lokala datagateway**. Ange servernamnet för hello Oracle, autentiseringsmetod, användarnamn, lösenord och väljer hello gateway:

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. När du är ansluten, markera en tabell från listan hello och ange hello rad-ID tooyour tabell. Du behöver tooknow hello identifierare toohello tabell. Om du inte vet administratören Oracle DB och få hello utdata från `select * from yourTableName`. Det här ger du hello identifierbar information som du behöver tooproceed.

    I följande exempel hello, returneras jobbdata från en databas för personal: 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. I nästa steg, du kan använda någon av hello andra kopplingar toobuild arbetsflödet. Om du vill ha tootest hämta data från Oracle och skicka ett e-postmeddelande med hello Oracle data med hjälp av en av hello skicka e-kopplingar, sådan Office 365 eller Gmail. Använd dynamiska hello-token från hello Oracle tabell toobuild hello `Subject` och `Body` av din e-post:

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. **Spara** din logikapp och välj sedan **kör**. Stäng designern för hello och titta på hello körs historik för hello status. Välj raden för hello-meddelande om den inte. hello designer öppnas och visar vilka steg misslyckades och visar hello felinformation. Om det lyckas, bör du få ett e-postmeddelande med hello information som du har lagt till.


### <a name="workflow-ideas"></a>Arbetsflöde för uppslag

* Du vill toomonitor hello #oracle hashtaggar och placera hello tweets i en databas så att de kan efterfrågas och används i andra program. Lägg till hello i en logikapp `Twitter - When a new tweet is posted` utlösa och ange hello **#oracle** hashtaggar. Lägg sedan till hello `Oracle Database - Insert row` åtgärder, och välj din tabell:

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* Meddelanden skickas tooa Service Bus-kö. Du vill tooget dessa meddelanden och placera dem i en databas. Lägg till hello i en logikapp `Service Bus - when a message is received in a queue` utlösare och välj hello kön. Lägg sedan till hello `Oracle Database - Insert row` åtgärder, och välj din tabell:

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a>Vanliga fel

#### <a name="error-cannot-reach-hello-gateway"></a>**Fel**: Det går inte att nå hello Gateway

**Orsak**: hello lokala datagateway är inte kan tooconnect toohello moln. 

**Minskning**: Kontrollera att din gateway körs på hello lokala dator där du har installerat och att den kan ansluta toohello internet.  Vi rekommenderar att du inte installerar hello gateway på en dator som kan stängas av eller i viloläge. Du kan också starta om hello lokala data gateway-tjänsten (PBIEgwService).

#### <a name="error-hello-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-please-visit-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-tooinstall-hello-official-provider"></a>**Fel**: hello-providern som används är inaktuell: ' System.Data.OracleClient kräver Oracle klientprogramversionen 8.1.7 eller senare ”.. Besök [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) tooinstall hello officiella leverantören.

**Orsak**: hello Oracle-klient SDK inte har installerats på datorn hello där hello lokala datagateway körs.  

**Lösning**: hämta och installera hello Oracle klient-SDK på hello samma dator som hello lokala datagateway.

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a>**Fel**: tabellen [tabellnamn] definierar inte några viktiga kolumner

**Orsak**: hello tabellen inte har någon primärnyckel.  

**Lösning**: hello Oracle Database connector kräver att en tabell med en primärnyckelkolumn används.

#### <a name="currently-not-supported"></a>För närvarande stöds inte

* Vyer och lagrade procedurer 
* En tabell med sammansatta nycklar
* Kapslade objekttyper i tabeller
 
## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/oracle/). 

## <a name="get-some-help"></a>Få hjälp

Hej [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) är utmärkt placera tooask frågor, besvara frågor och se vad andra användare med Logic Apps gör. 

Du kan förbättra Logic Apps och kopplingar genom röstning och skicka dina idéer på [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish). 


## <a name="next-steps"></a>Nästa steg
[Skapa en logikapp](../logic-apps/logic-apps-create-a-logic-app.md), och utforska hello tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).
