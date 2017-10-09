---
title: "aaaConnect tooDynamics 365 (online) från Azure Logic Apps | Microsoft Docs"
description: "Skapa logik app arbetsflöden som hanterar Dynamics 365 (online) enheter via hello API som tillhandahålls av hello Dynamics 365 connector"
services: logic-apps
cloud: Azure Stack
author: Mattp123
manager: anneta
documentationcenter: 
tags: connectors
ms.assetid: 0dc2abef-7d2c-4a2d-87ca-fad21367d135
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: matp; LADocs
ms.openlocfilehash: 183d7a6b8e5d2c0eecc70da0da3806e06c382df4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-toodynamics-365-from-logic-app-workflows"></a>Ansluta tooDynamics 365 från logik app arbetsflöden

Du kan ansluta tooDynamics 365 (online) och skapa användbart business-flöden som skapar posterna, uppdatera objekt eller returnera en lista över poster med Logic Apps. Med hello Dynamics 365 connector kan du:

* Skapa ditt företag flödet baserat på hello data du får från Dynamics 365 (online).
* Använd åtgärder som får ett svar och sedan göra hello utdata för andra åtgärder. När ett objekt uppdateras i Dynamics 365 (online), kan du skicka ett e-postmeddelande med hjälp av Office 365.

Det här avsnittet visar hur toocreate en logikapp som skapar en uppgift i Dynamics 365 när en ny lead skapas i Dynamics 365.

## <a name="prerequisites"></a>Krav
* Ett Azure-konto.
* En Dynamics 365 (online)-konto.

## <a name="create-a-task-when-a-new-lead-is-created-in-dynamics-365"></a>Skapa en uppgift när en ny lead skapas i Dynamics 365

1.  [Logga in tooAzure](https://portal.azure.com).

2.  Skriv i hello Azure search `Logic apps`, och tryck på RETUR.

      ![Sök efter Logic Apps](./media/connectors-create-api-crmonline/find-logic-apps.png)

3.  Under **logikappar**, klickar du på **Lägg till**.

      ![Lägg till LogicApp](./media/connectors-create-api-crmonline/add-logic-app.png)

4.  toocreate hello logikapp, en fullständig hello **namn**, **prenumeration**, **resursgruppen**, och **plats** fält och klicka sedan på  **Skapa**.

5.  Välj hello nya logikappen. När du får hello **distributionen lyckades** meddelande, klicka på **uppdatera**.

6.  Under **utvecklingsverktyg**, klickar du på **logik App Designer**. I hello mall klickar du på **tom Logikapp**.

7.  Skriv i sökrutan hello `Dynamics 365`. Från hello Dynamics 365 utlöser listan, Välj **Dynamics 365 – när en post skapas**.

8.  Om du tillfrågas toosign i tooDynamics 365 kan du göra det nu.

9.  Ange hello följande information i hello utlösaren information:

  * **Organisationens namn**. Välj hello Dynamics 365 instans som du vill hello logik app toolisten till.

  * **Entitetsnamnet**. Välj hello entitet som du vill toolisten till. Den här händelsen fungerar som en utlösare toostart hello logikapp. 
  I den här genomgången **leder** är markerad.

  * **Hur ofta ska toocheck objekt?** Dessa värden anger hur ofta hello logikapp söker efter uppdateringar relaterade toohello utlösare. hello standardinställningen är toocheck för uppdateringar tre minuters mellanrum.

    * **Frekvensen**. Välj sekunder, minuter, timmar eller dagar.

    * **Intervallet**. Ange hello antal sekunder, minuter, timmar eller dagar som du vill toopass innan hello nästa kontroll.

      ![Logik Apputlösare information](./media/connectors-create-api-crmonline/trigger-details.png)

10. Klicka på **nytt steg**, och klicka sedan på **lägga till en åtgärd**.

11. Skriv i sökrutan hello `Dynamics 365`. Välj hello åtgärder listan **Dynamics 365 – skapa en ny post**.

12. Ange hello följande information:

    * **Organisationens namn**. Välj hello Dynamics 365 instans där du vill att hello flödet toocreate hello post. 
    Observera att den här instansen inte har toobe hello samma instans där hello händelsen utlöses från.

    * **Entitetsnamnet**. Välj hello entitet som du vill toocreate en post när hello händelsen utlöses. 
    I den här genomgången **uppgifter** är markerad.

13. Klicka i hello **ämne** som visas. Du kan välja något av dessa fält hello dynamiskt innehåll listan som visas:

    * **Efternamn**. Att välja det här fältet infogas hello efternamn för hello lead hello ämnesfältet för hello aktivitet när posten för hello uppgift har skapats.
    * **Avsnittet**. Att välja fältet fältet infogningar hello avsnittet för hello lead till hello ämnesfältet för hello aktivitet när hello uppgiftspost skapas. 
    Klicka på **avsnittet** tooadd som toohello **ämne** rutan.

      ![Logik App skapa nya registrera information](./media/connectors-create-api-crmonline/create-record-details.png)

14. Hello logik App Designer i verktygsfältet klickar du på **spara**.

    ![Logik App Designer verktygsfältet spara](./media/connectors-create-api-crmonline/designer-toolbar-save.png)

15. toostart hello Logikapp, klickar du på **kör**.

    ![Logik App Designer verktygsfältet spara](./media/connectors-create-api-crmonline/designer-toolbar-run.png)

16. Nu skapa en lead-post i Dynamics 365 för försäljning och se din flödet i praktiken!

## <a name="set-advanced-options-for-a-logic-app-step"></a>Ange avancerade alternativ för en logik app steg

hur toofilter data i en app steg logik Klicka toospecify **visa avancerade alternativ** steget och sedan lägga till ett filter eller ordning av frågan.

Du kan till exempel använder en filter frågan tooget endast aktiva konton och sortera efter hello kontonamn. tooperform detta uppgift, ange hello OData-filterfråga `statuscode eq 1`, och välj **kontonamn** hello dynamiskt innehåll listan. Mer information: [MSDN: $filter](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_1) och [$orderby](https://msdn.microsoft.com/library/gg309461.aspx#Anchor_2).

![Logikapp avancerade alternativ](./media/connectors-create-api-crmonline/advanced-options.png)

### <a name="best-practices-when-using-advanced-options"></a>Rekommenderade metoder när du använder avancerade alternativ

När du lägger till tooa värdefältet matcha du fälttypen hello om du skriver ett värde eller välj ett värde från hello dynamiska innehållslistan.

Typ av fält  |Hur toouse  |Där toofind  |Namn  |Datatyp  
---------|---------|---------|---------|---------
Textfält|Textfält kräver en rad med text eller dynamiskt innehåll som är ett fält av typen text. Exempel inkluderar hello kategori och underkategori fält.|Inställningar > anpassningar > Anpassa hello System > enheter > aktivitet > fält |category |Rad med Text        
Heltalsfält | Vissa fält kräver heltal eller dynamiskt innehåll som är av typen Integer. Exempel: procent färdigt och varaktighet. |Inställningar > anpassningar > Anpassa hello System > enheter > aktivitet > fält |värdet |Heltal         
Datumfält | Vissa fält kräver ett datum som anges i formatet åååå-mm-dd eller dynamiskt innehåll som är ett fält av typen date. Exempel är skapad, startdatum, verklig Start senast på håller tid, Verkligt slut och förfallodatum. | Inställningar > anpassningar > Anpassa hello System > enheter > aktivitet > fält |createdon |Datum och tid
Ange fälten som kräver både en post-ID och sökning |Vissa fält som refererar till en annan entitetspost kräver både hello post-ID och hello sökning. |Inställningar > anpassningar > Anpassa hello System > enheter > konto > fält  | accountid  | Primär nyckel

### <a name="more-examples-of-fields-that-require-both-a-record-id-and-lookup-type"></a>Fler exempel på fält som kräver en post-ID och ett sökning skriver
Expandera på hello föregående tabell följer fler exempel på fält som inte fungerar med värden som väljs i hello dynamiska innehållslistan. I stället kräver dessa fält båda ID och sökning posttyp anges i hello fält i PowerApps.  
* Ägare och typen ägare. fältet för hello ägare måste vara ett giltigt användar- eller post-ID. hello typen ägare måste vara antingen **systemusers** eller **team**.
* Kund och kunden. hello kund-fältet måste vara ett giltigt konto eller kontakta post-ID. hello typen ägare måste vara antingen **konton** eller **kontakter**.
* Angående och för typen. hello om fältet måste vara en giltig post-ID, till exempel ett konto eller kontakta post-ID. hello angående typ måste vara hello sökning typ för hello post som **konton** eller **kontakter**.

hello läggs följande aktivitet Skapa åtgärd exempel en kontopost som motsvarar toohello-ID för att lägga till den toohello om fältet för hello-aktivitet.

![Flödet recordId och typ för kontot](./media/connectors-create-api-crmonline/recordid-type-account.png)

Det här exemplet ger också hello uppgiften tooa specifika användare baserat på hello användarens post-ID.

![Flödet recordId och typ för kontot](./media/connectors-create-api-crmonline/recordid-type-user.png)

toofind en post har ID, se följande avsnitt hello: *hitta hello post-ID*

## <a name="find-hello-record-id"></a>Hitta hello post-ID

1. Öppna en post, till exempel en kontopost.

2. Hello åtgärder i verktygsfältet klickar du på **Pop-ut** ![popout post](./media/connectors-create-api-crmonline/popout-record.png).
Du kan också hello åtgärder toocopy hello fullständiga URL: en till e-postprogram, klicka på verktygsfältet **e-post en länk**.

   hello post-ID visas mellan hello 7b och %7 %d tecken i hello URL-kodning.

   ![Flödet recordId och typ för kontot](./media/connectors-create-api-crmonline/recordid.png)

## <a name="troubleshooting"></a>Felsökning
tootroubleshoot ett steg som inte i en logikapp, visa hello statusinformation om hello-händelse.

1. Under **Logikappar**, Välj din logikapp och klicka sedan på **översikt**. 

   hello sammanfattningen visas samt hello kör status för hello logikapp. 

   ![Logikapp Körstatus](./media/connectors-create-api-crmonline/tshoot1.png)

2. tooview mer information om eventuella misslyckade körs klickar du på hello misslyckades händelse. tooexpand ett steg som inte klicka på steget.

   ![Expandera steg som misslyckades](./media/connectors-create-api-crmonline/tshoot2.png)

   hello steg information visas och kan hjälpa dig att felsöka hello orsaken till hello felet.

   ![Det gick inte steg-information](./media/connectors-create-api-crmonline/tshoot3.png)

Läs mer om hur du felsöker logikappar [diagnostisera fel i logik app](../logic-apps/logic-apps-diagnosing-failures.md).

## <a name="connector-specific-details"></a>Connector-specifik information

Visa alla utlösare och åtgärder som definierats i hello swagger och även se några gränser i hello [connector information](/connectors/crm/). 

## <a name="next-steps"></a>Nästa steg
Utforska hello andra tillgängliga kopplingar i Logic Apps på vår [API: er listan](apis-list.md).
