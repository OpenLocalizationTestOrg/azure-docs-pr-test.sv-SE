---
title: "aaaX12 meddelanden för B2B enterprise integration - Azure Logic Apps | Microsoft Docs"
description: "Exchange X12 meddelanden EDI-format för B2B enterprise integrering med Azure Logikappar"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 7422d2d5-b1c7-4a11-8c9b-0d8cfa463164
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 20a077b299875a16ada66a500d5f1c8f9972d309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exchange-x12-messages-for-enterprise-integration-with-logic-apps"></a>Exchange X12 meddelanden för enterprise-integrering med logic apps

Innan du kan utbyta X12 meddelanden för Logikappar i Azure måste du skapa en X12 avtalet och lagra avtalet i ditt konto för integrering. Här följer hello instruktioner för hur toocreate ett X12 avtal.

> [!NOTE]
> Den här sidan innehåller hello X12 funktioner för Azure Logic Apps. Mer information finns i [EDIFACT](logic-apps-enterprise-integration-edifact.md).

## <a name="before-you-start"></a>Innan du börjar

Här är hello-objekt som du behöver:

* En [integrering konto](../logic-apps/logic-apps-enterprise-integration-accounts.md) som redan har definierat och som är associerade med din Azure-prenumeration
* Minst två [partners](../logic-apps/logic-apps-enterprise-integration-partners.md) som definieras i ditt konto för integrering och konfigurerad med hello X12 identifierare under **Business identiteter**    
* En nödvändig [schemat](../logic-apps/logic-apps-enterprise-integration-schemas.md) för uppladdning av tooyour [integrering konto](../logic-apps/logic-apps-enterprise-integration-accounts.md)

När du [skapa ett konto för integrering](../logic-apps/logic-apps-enterprise-integration-accounts.md), [lägga till partners](logic-apps-enterprise-integration-partners.md), och har en [schemat](../logic-apps/logic-apps-enterprise-integration-schemas.md) som du vill toouse kan du skapa en X12 avtal genom att följa dessa steg.

## <a name="create-an-x12-agreement"></a>Skapa en X12 avtal

1.  Logga in toohello [Azure-portalen](http://portal.azure.com "Azure-portalen"). Välj hello vänstra menyn **fler tjänster**. 

    > [!TIP]
    > Om du inte ser **fler tjänster**, du kanske tooexpand hello menyn först. Överst hello hello komprimerad-menyn, Välj **menyn Visa**.

    ![På vänster-menyn väljer du ”fler tjänster”](./media/logic-apps-enterprise-integration-x12/account-1.png)

2.  Skriv ”integration” i sökrutan hello som filter. Markera i hello resultat **Integrationskonton**.  

    ![Filtrera efter ”integration”, Välj ”Integrationskonton”](./media/logic-apps-enterprise-integration-x12/account-2.png)

3. I hello **Integrationskonton** bladet som öppnas väljer hello integration konto där du vill tooadd hello avtal.
Om du inte ser några integrationskonton [skapa en första](../logic-apps/logic-apps-enterprise-integration-accounts.md "om integrationskonton").

    ![Välj integration konto där toocreate hello avtal](./media/logic-apps-enterprise-integration-x12/account-3.png)

4. Välj **översikt**och välj hello **avtal** panelen. Om du inte har ett avtal sida vid sida, Lägg till hello panelen först. 

    ![Välj ikonen ”avtal”](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

5. I hello avtal bladet som öppnas väljer du **Lägg till**.

    ![Välj ”Lägg till”](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)     

6. Under **Lägg till**, ange en **namn** för ditt avtal. Hello avtalstyp Välj **X12**. Välj hello **värden Partner**, **Värdidentiteten**, **gäst Partner**, och **gäst identitet** för ditt avtal. Mer information för egenskapen finns hello tabellen i det här steget.

    ![Ange avtalsuppgifter](./media/logic-apps-enterprise-integration-x12/x12-1.png)  

    | Egenskap | Beskrivning |
    | --- | --- |
    | Namn |Namnet på hello avtal |
    | Avtalstyp | Ska vara X12 |
    | Värd-Partner |Ett avtal måste både värden och gästen partner. hello värden partner representerar hello organisation som konfigurerar hello avtal. |
    | Värdidentiteten |En identifierare för hello värden partner |
    | Gästen Partner |Ett avtal måste både värden och gästen partner. hello gäst partner representerar hello organisation som har att göra affärer med hello värden partner. |
    | Gästen identitet |En identifierare för hello gäst partner |
    | Ta emot inställningarna |Dessa egenskaper gäller tooall har tagits emot av ett avtal. |
    | Skicka inställningar |Dessa egenskaper gäller tooall meddelanden som skickas av ett avtal. |  

  > [!NOTE]
  > Lösning av X12 avtal beror på matchar hello avsändaren kvalificerare och identifierare, och hello mottagaren kvalificerare och identifieraren som definierats i hello partner och inkommande meddelande. Ändrar värdena för din partner, uppdatera hello avtal för.

## <a name="configure-how-your-agreement-handles-received-messages"></a>Konfigurera hur ditt avtal handtag mottagna meddelanden

Nu när du har angett hello avtal egenskaper kan konfigurera du hur detta avtal identifierar och hanterar inkommande meddelanden som tagits emot från din partner via det här avtalet.

1.  Under **Lägg till**väljer **tar emot inställningar**.
Konfigurera dessa egenskaper baserat på ditt avtal med hello-partner som utbyter meddelanden med dig. Egenskapsbeskrivningar finns hello tabellerna i det här avsnittet.

    **Ta emot inställningarna** är indelad i följande avsnitt: identifierare, bekräftelse, scheman, kuvert, kontroll siffror, verifieringar och interna inställningar.

2. När du är klar gör att toosave dina inställningar genom att välja **OK**.

Nu ditt avtal är klar toohandle inkommande valda meddelanden som uppfyller tooyour inställningar.

### <a name="identifiers"></a>Identifierare

![Ange ID-egenskaper](./media/logic-apps-enterprise-integration-x12/x12-2.png)  

| Egenskap | Beskrivning |
| --- | --- |
| ISA1 (auktorisering kvalificerare) |Välj hello auktorisering kvalificerare värde hello nedrullningsbara listan. |
| ISA2 |Valfri. Ange värdet för auktorisering information. Om hello-värde som du angav för ISA1 är än 00 ange minst ett alfanumeriskt tecken och högst 10. |
| ISA3 (Security kvalificerare) |Välj hello kvalificerare säkerhetsvärde hello nedrullningsbara listan. |
| ISA4 |Valfri. Ange hello säkerhetsvärde information. Om hello-värde som du angav för ISA3 är än 00 ange minst ett alfanumeriskt tecken och högst 10. |

### <a name="acknowledgment"></a>Bekräftelse

![Ange egenskaper för bekräftelse](./media/logic-apps-enterprise-integration-x12/x12-3.png) 

| Egenskap | Beskrivning |
| --- | --- |
| TA1 förväntades |Returnerar en teknisk bekräftelse toohello interchange avsändare |
| MI förväntades |Returnerar en funktionell bekräftelse toohello interchange avsändare. Välj beroende om du vill hello 997 eller 999 bekräftelser, på hello schemaversionen |
| Inkludera AK2/IK2 Loop |Aktiverar generering av AK2 loopar i funktionella bekräftelser för godkända transaktionen uppsättningar |

### <a name="schemas"></a>Scheman

Välj ett schema för varje transaktionstyp (ST1) och avsändaren program (GS2). hello får pipeline Disassemblerar hello inkommande meddelande genom att matcha hello värden för ST1 och GS2 i inkommande hello-meddelande med hello värden som anges här och hello schemat för inkommande hello-meddelande med hello-schema som du anger här.

![Välj schema](./media/logic-apps-enterprise-integration-x12/x12-33.png) 

| Egenskap | Beskrivning |
| --- | --- |
| Version |Välj hello X12 version |
| Transaktionstyp (ST01) |Välj hello transaktionstyp |
| Avsändaren program (GS02) |Välj hello avsändaren program |
| Schemat |Välj hello schemafilen som du vill toouse. Scheman läggs tooyour integrering konto. |

> [!NOTE]
> Konfigurera hello krävs [schemat](../logic-apps/logic-apps-enterprise-integration-schemas.md) som är överförda tooyour [integrering konto](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Kuvert

![Ange hello avgränsare i en transaktion: Välj Standard identifierare eller upprepning avgränsare](./media/logic-apps-enterprise-integration-x12/x12-34.png)

| Egenskap | Beskrivning |
| --- | --- |
| ISA11 användning |Anger hello avgränsare toouse i en transaktion: <p>Välj **Standard identifierare** toouse en punkt (.) för decimalform i stället för hello decimalform hello inkommande dokument i hello EDI får pipeline. <p>Välj **upprepning avgränsare** toospecify hello avgränsare för upprepade förekomster av en enkel dataelement eller en upprepad datastruktur. Till exempel används vanligtvis hello cirkumflex (^) som hello upprepning avgränsare. Du kan bara använda hello cirkumflex för HIPAA scheman. |

### <a name="control-numbers"></a>Kontrollen siffror

![Välj hur toohandle styr antalet dubbletter](./media/logic-apps-enterprise-integration-x12/x12-35.png) 

| Egenskap | Beskrivning |
| --- | --- |
| Tillåt inte Interchange kontrollen antal dubbletter |Blockera dubbla externa utbyten. Kontrollerar hello interchange kontrollnumret (ISA13) för hello emot interchange kontroll. Om en matchning hittas hello får pipeline inte bearbeta hello utbyte. Du kan ange hello antal dagar för att utföra hello kontroll genom att ge ett värde för *Sök efter duplicerade ISA13 varje (dagar)*. |
| Tillåt inte dubletter av gruppkontrollnummer |Blockera interchanges med dubbla grupp kontrollen nummer. |
| Tillåt inte dubletter av transaktionsuppsättningsnummer |Blockera interchanges med dubbla transaktion set kontrollen nummer. |

### <a name="validations"></a>Verifieringar

![Ange egenskaper för verifiering för mottagna meddelanden](./media/logic-apps-enterprise-integration-x12/x12-36.png) 

När du slutför varje validering rad läggs en annan automatiskt. Om du inte anger några regler använder verifieringen hello ”standard” rad.

| Egenskap | Beskrivning |
| --- | --- |
| Meddelandetyp |Välj hello EDI-meddelandetypen. |
| EDI-validering |Validerar EDI-datatyper som definieras av hello schemat EDI egenskaper, längd, tom dataelement och avslutande avgränsare. |
| Utökad validering |Om hello datatypen inte EDI, validering på hello data elementet krav tillåts och upprepning, uppräkningar och elementet längd dataverifiering (min/max). |
| Tillåt inledande/efterföljande nollor |Behåll ytterligare inledande eller avslutande noll och utrymme tecken. Ta inte bort dessa tecken. |
| Ta bort inledande/efterföljande nollor |Ta bort inledande eller efterföljande noll och blanksteg. |
| Avslutande avgränsare princip |Generera avslutande avgränsare. <p>Välj **inte tillåtet** tooprohibit avslutande avgränsare och avgränsare i hello emot utbyte. Om hello interchange har avslutande avgränsare och avgränsare, deklareras hello interchange inte giltig. <p>Välj **valfritt** tooaccept interchanges med eller utan avslutande avgränsare och avgränsare. <p>Välj **obligatoriska** när hello interchange måste ha avslutande avgränsare och avgränsare. |

### <a name="internal-settings"></a>Internt inställningar

![Välj inställningar för internt](./media/logic-apps-enterprise-integration-x12/x12-37.png) 

| Egenskap | Beskrivning |
| --- | --- |
| Konvertera underförstådda decimalformat ”Nn” tooa bas 10 numeriskt värde |Konverterar ett EDI-nummer som har angetts med hello format ”Nn” till ett numeriskt värde bas 10 |
| Skapa tomma XML-taggar om avslutande avgränsare är tillåtna |Välj den här kryssrutan toohave hello interchange avsändaren innehåller tomma XML-taggar för avslutande avgränsare. |
| Dela upp interchange i transaktionsuppsättningar – inaktivera transaktionsuppsättningar vid fel|Parsar varje transaktion som angetts i ett utbyte i ett annat XML-dokument genom att använda hello lämpliga kuvert toohello transaktion uppsättningen. Pausar hello-transaktioner om hello verifieringen misslyckas. |
| Dela upp interchange i transaktionsuppsättningar – inaktivera interchange vid fel|Parsar varje transaktion som i ett utbyte i ett annat XML-dokument genom att tillämpa lämpliga hello-kuvertet. Pausar hela interchange när en eller flera uppsättningar av transaktionen i hello interchange inte kan valideras. | 
| Bevara Interchange – inaktivera transaktion anger vid fel |Lämnar hello interchange intakta, skapar ett XML-dokument för hello hela gruppbaserad utbyte. Pausar endast hello transaktion uppsättningar som inte kan valideras medan tooprocess anger alla andra transaktioner. |
| Bevara interchange – inaktivera interchange vid fel |Lämnar hello interchange intakta, skapar ett XML-dokument för hello hela gruppbaserad utbyte. Pausar hello hela interchange när en eller flera uppsättningar av transaktionen i hello interchange inte kan valideras. |

## <a name="configure-how-your-agreement-sends-messages"></a>Konfigurera hur ditt avtal skickar meddelanden

Du kan konfigurera hur detta avtal identifierar och hanterar utgående meddelanden som du skickar tooyour partner via det här avtalet.

1.  Under **Lägg till**väljer **skicka inställningar**.
Konfigurera dessa egenskaper baserat på ditt avtal med din partner som utbyter meddelanden med dig. Egenskapsbeskrivningar finns hello tabellerna i det här avsnittet.

    **Skicka inställningar** är indelad i följande avsnitt: identifierare, bekräftelse, scheman, kuvert, teckenuppsättningar och avgränsare, kontroll siffror och validering.

2. När du är klar gör att toosave dina inställningar genom att välja **OK**.

Ditt avtal är nu redo toohandle utgående meddelanden som uppfyller tooyour valda inställningar.

### <a name="identifiers"></a>Identifierare

![Ange ID-egenskaper](./media/logic-apps-enterprise-integration-x12/x12-4.png)  

| Egenskap | Beskrivning |
| --- | --- |
| Auktorisering kvalificerare (ISA1) |Välj hello auktorisering kvalificerare värde hello nedrullningsbara listan. |
| ISA2 |Ange värdet för auktorisering information. Om det här värdet är än 00, ange minst ett alfanumeriskt tecken och högst 10. |
| Säkerhet kvalificerare (ISA3) |Välj hello kvalificerare säkerhetsvärde hello nedrullningsbara listan. |
| ISA4 |Ange hello säkerhetsvärde information. Om det här värdet är än 00 för textrutan för hello-värde (ISA4), anger du minst ett alfanumeriskt värde och högst 10. |

### <a name="acknowledgment"></a>Bekräftelse

![Ange egenskaper för bekräftelse](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Egenskap | Beskrivning |
| --- | --- |
| TA1 förväntades |Returnera en teknisk bekräftelse (TA1) toohello interchange avsändare. Den här inställningen anger att hello värden partner som skickar begäranden för hello-meddelande en bekräftelse från hello gäst partner i hello avtal. Dessa bekräftelser förväntas av hello värden partner baserat på hello tar emot inställningar av hello avtal. |
| MI förväntades |Returnera en funktionell bekräftelse (MI) toohello interchange avsändare. Välj om du vill hello 997 eller 999 bekräftelser, baserat på hello schema-versioner som du arbetar med. Dessa bekräftelser förväntas av hello värden partner baserat på hello tar emot inställningar av hello avtal. |
| MI Version |Välj hello MI version |

### <a name="schemas"></a>Scheman

![Välj schemat toouse](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Egenskap | Beskrivning |
| --- | --- |
| Version |Välj hello X12 version |
| Transaktionstyp (ST01) |Välj hello transaktionstyp |
| SCHEMAT |Välj hello schemat toouse. Scheman finns i ditt konto för integrering. Om du väljer schemat först konfigurerar den automatiskt version och transaktionen typ  |

> [!NOTE]
> Konfigurera hello krävs [schemat](../logic-apps/logic-apps-enterprise-integration-schemas.md) som är överförda tooyour [integrering konto](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Kuvert

![Ange hello avgränsare i en transaktion: Välj Standard identifierare eller upprepning avgränsare](./media/logic-apps-enterprise-integration-x12/x12-6.png) 

| Egenskap | Beskrivning |
| --- | --- |
| ISA11 användning |Anger hello avgränsare toouse i en transaktion: <p>Välj **Standard identifierare** toouse en punkt (.) för decimalform i stället för hello decimalform hello inkommande dokument i hello EDI får pipeline. <p>Välj **upprepning avgränsare** toospecify hello avgränsare för upprepade förekomster av en enkel dataelement eller en upprepad datastruktur. Till exempel används vanligtvis hello cirkumflex (^) som hello upprepning avgränsare. Du kan bara använda hello cirkumflex för HIPAA scheman. |

### <a name="control-numbers"></a>Kontrollen siffror

![Ange egenskaper för kontrollen tal](./media/logic-apps-enterprise-integration-x12/x12-8.png) 

| Egenskap | Beskrivning |
| --- | --- |
| Versionsnumret för kontrollen (ISA12) |Välj hello version av hello X12 standard |
| Användning-indikatorn (ISA15) |Välj hello kontexten för ett utbyte.  hello värden information produktionsdata, eller testa data |
| Schemat |Genererar hello GS och ST segment för en X12-kodade utbyte den skickar toohello skicka Pipeline |
| GS1 |Valfritt, väljer du ett värde för hello funktionell kod från hello listrutan |
| GS2 |Valfritt program avsändaren |
| GS3 |Valfritt program mottagare |
| GS4 |Valfritt, Välj CCYYMMDD eller ååmmdd |
| GS5 |Valfritt, Välj HHMM, HHMMSS eller HHMMSSdd |
| GS7 |Valfritt, väljer du ett värde för hello ansvarar agency hello nedrullningsbara listan |
| GS8 |Valfritt, version av hello dokument |
| Interchange kontrollnumret (ISA13) |Krävs, ange ett värdeintervall för hello interchange kontrollnummer. Ange ett numeriskt värde med minst 1 och högst 999999999 |
| Gruppnummer kontrollen (GS06) |Krävs, ange ett intervall med nummer för hello gruppnummer för kontrollen. Ange ett numeriskt värde med minst 1 och högst 999999999 |
| Transaktionsnumret Set kontrollen (ST02) |Krävs, ange ett intervall med nummer för hello ange Transaktionskontroll nummer. Ange ett intervall med numeriska värden med minst 1 och högst 999999999 |
| Prefix |Valfritt, avsedda för hello antal transaktion set kontrollen nummer som används i bekräftelse. Ange ett numeriskt värde för hello mellersta två fält och ett alfanumeriskt värde (om du vill) för hello prefix och suffix fält. hello mellersta är obligatoriska och innehålla hello lägsta och högsta värde för hello kontroll |
| Suffix |Valfritt, avsedda för hello antal transaktion set kontrollen nummer som används i en bekräftelse. Ange ett numeriskt värde för hello mellersta två fält och ett alfanumeriskt värde (om du vill) för hello prefix och suffix fält. hello mellersta är obligatoriska och innehålla hello lägsta och högsta värde för hello kontroll |

### <a name="character-sets-and-separators"></a>Teckenuppsättningar och avgränsare

Andra än hello teckenuppsättning, kan du ange en annan uppsättning avgränsare för varje meddelandetyp. Om en teckenuppsättning har inte angetts för ett visst meddelande-schema, används hello standardinställda teckenuppsättningen.

![Ange avgränsare för meddelandetyper](./media/logic-apps-enterprise-integration-x12/x12-9.png) 

| Egenskap | Beskrivning |
| --- | --- |
| Tecknet Set toobe används |toovalidate hello egenskaper, Välj hello X12 teckenuppsättningen. hello alternativ är grundläggande och utökad UTF8. |
| Schemat |Välj ett schema hello nedrullningsbara listan. När du har slutfört varje rad läggs automatiskt en ny rad. För hello valda schemat ange väljer hello avgränsare som du vill toouse, baserat på hello följande beskrivningar av avgränsare. |
| Indatatypen |Välj en Indatatyp hello nedrullningsbara listan. |
| Komponenten avgränsare |tooseparate sammansatta dataelement, ange ett enskilt tecken. |
| Data elementet avgränsare |tooseparate enkel dataelement i sammansatta dataelement, ange ett enskilt tecken. |
| Ersättning tecken |Ange ett annat tecken som används för att ersätta alla tecken i hello nyttolasten vid generering av hälsningsmeddelande utgående X12. |
| Segmentera Begränsare |tooindicate hello slutet av en EDI-segment, ange ett enskilt tecken. |
| Suffix |Välj hello tecken som används med hello segment-ID. Om du har angett ett suffix hello får segment Begränsare dataelementet vara tomt. Om hello segment Begränsare lämnas tomt, måste du ange ett suffix. |

> [!TIP]
> tooprovide specialtecken värden, redigera hello avtal som JSON och ange hello ASCII-värde för hello specialtecken.

### <a name="validation"></a>Validering

![Ange egenskaper för verifiering för att skicka meddelanden](./media/logic-apps-enterprise-integration-x12/x12-10.png) 

När du slutför varje validering rad läggs en annan automatiskt. Om du inte anger några regler använder verifieringen hello ”standard” rad.

| Egenskap | Beskrivning |
| --- | --- |
| Meddelandetyp |Välj hello EDI-meddelandetypen. |
| EDI-validering |Validerar EDI-datatyper som definieras av hello schemat EDI egenskaper, längd, tom dataelement och avslutande avgränsare. |
| Utökad validering |Om hello datatypen inte EDI, validering på hello data elementet krav tillåts och upprepning, uppräkningar och elementet längd dataverifiering (min/max). |
| Tillåt inledande/efterföljande nollor |Behåll ytterligare inledande eller avslutande noll och utrymme tecken. Ta inte bort dessa tecken. |
| Ta bort inledande/efterföljande nollor |Ta bort inledande eller avslutande noll tecken. |
| Avslutande avgränsare princip |Generera avslutande avgränsare. <p>Välj **inte tillåtet** tooprohibit avslutande avgränsare och avgränsare i hello skickas utbyte. Om hello interchange har avslutande avgränsare och avgränsare, deklareras hello interchange inte giltig. <p>Välj **valfritt** toosend interchanges med eller utan avslutande avgränsare och avgränsare. <p>Välj **obligatoriska** om hello skickas interchange måste ha avslutande avgränsare och avgränsare. |

## <a name="find-your-created-agreement"></a>Hitta din skapade avtal

1.  När du är klar med inställningen alla dina avtal egenskaper på hello **Lägg till** bladet välj **OK** toofinish skapar ditt avtal och returnerar tooyour integrering konto bladet.

    Ditt nya avtal nu visas i din **avtal** lista.

2.  Du kan också visa dina avtal i ditt Kontoöversikt för integrering. Välj på ditt kontoblad integration **översikt**och välj hello **avtal** panelen.

    ![Välj ”avtal” panelen tooview alla avtal](./media/logic-apps-enterprise-integration-x12/x12-1-5.png)   

## <a name="view-hello-swagger"></a>Visa hello swagger
Se hello [swagger information](/connectors/x12/). 

## <a name="learn-more"></a>Läs mer
* [Mer information om hello Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")  

