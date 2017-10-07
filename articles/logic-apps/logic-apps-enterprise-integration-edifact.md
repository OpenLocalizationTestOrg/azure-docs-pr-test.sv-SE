---
title: "aaaEDIFACT meddelanden för B2B enterprise integration - Azure Logic Apps | Microsoft Docs"
description: "Exchange EDIFACT-meddelanden i EDI-format för B2B enterprise integrering med Azure Logikappar"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 2257d2c8-1929-4390-b22c-f96ca8b291bc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/26/2016
ms.author: LADocs; jonfan
ms.openlocfilehash: 496fadcda58de2d36459202b839b0a63c9e5857c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exchange-edifact-messages-for-enterprise-integration-with-logic-apps"></a>Exchange EDIFACT-meddelanden för enterprise-integrering med logic apps

Innan du kan utbyta EDIFACT-meddelanden för Logikappar i Azure, måste du skapa ett EDIFACT-avtal och spara avtalet i ditt konto för integrering. Här följer hello instruktioner för hur toocreate ett EDIFACT-avtal.

> [!NOTE]
> Den här sidan innehåller hello EDIFACT-funktioner för Azure Logic Apps. Mer information finns i [X12](logic-apps-enterprise-integration-x12.md).

## <a name="before-you-start"></a>Innan du börjar

Här är hello-objekt som du behöver:

* En [integrering konto](../logic-apps/logic-apps-enterprise-integration-accounts.md) som redan har definierat och som är associerade med din Azure-prenumeration  
* Minst två [partners](logic-apps-enterprise-integration-partners.md) som redan har definierats i ditt konto för integrering

> [!NOTE]
> När du skapar ett avtal, hello innehåll i hello-meddelanden som du tar emot eller skickar tooand från hello partner måste matcha hello avtalstyp.

När du [skapa ett konto för integrering](../logic-apps/logic-apps-enterprise-integration-accounts.md) och [lägga till partners](logic-apps-enterprise-integration-partners.md), du kan skapa ett EDIFACT-avtal genom att följa dessa steg.

## <a name="create-an-edifact-agreement"></a>Skapa ett EDIFACT-avtal 

1.  Logga in toohello [Azure-portalen](http://portal.azure.com "Azure-portalen"). Välj hello vänstra menyn **fler tjänster**.

    > [!TIP]
    > Om du inte ser **fler tjänster**, du kanske tooexpand hello menyn först. Överst hello hello komprimerad-menyn, Välj **menyn Visa**.

    ![På vänster-menyn väljer du ”fler tjänster”](./media/logic-apps-enterprise-integration-edifact/edifact-0.png)

2. I sökrutan hello skriver du ”integration” för filtret. Markera i hello resultat **Integrationskonton**.

    ![Filtrera efter ”integration”, Välj ”Integrationskonton”](./media/logic-apps-enterprise-integration-edifact/edifact-1-3.png)

3. I hello **Integrationskonton** bladet som öppnas väljer hello integration konto där du vill toocreate hello avtal.
Om du inte ser några integrationskonton [skapa en första](../logic-apps/logic-apps-enterprise-integration-accounts.md "om integrationskonton").  

    ![Välj integration konto där toocreate hello avtal](./media/logic-apps-enterprise-integration-edifact/edifact-1-4.png)

4. Välj hello **avtal** panelen. Om du inte har ett avtal sida vid sida, Lägg till hello panelen först.   

    ![Välj ikonen ”avtal”](./media/logic-apps-enterprise-integration-edifact/edifact-1-5.png)

5. I hello avtal bladet som öppnas väljer du **Lägg till**.

    ![Välj ”Lägg till”](./media/logic-apps-enterprise-integration-edifact/edifact-agreement-2.png)

6. Under **Lägg till**, ange en **namn** för ditt avtal. För **avtalstyp**väljer **EDIFACT**. Välj hello **värden Partner**, **Värdidentiteten**, **gäst Partner**, och **gäst identitet** för ditt avtal.

    ![Ange avtalsuppgifter](./media/logic-apps-enterprise-integration-edifact/edifact-1.png)

    | Egenskap | Beskrivning |
    | --- | --- |
    | Namn |Namnet på hello avtal |
    | Avtalstyp | Ska vara EDIFACT |
    | Värd-Partner |Ett avtal måste både värden och gästen partner. hello värden partner representerar hello organisation som konfigurerar hello avtal. |
    | Värdidentiteten |En identifierare för hello värden partner |
    | Gästen Partner |Ett avtal måste både värden och gästen partner. hello gäst partner representerar hello organisation som har att göra affärer med hello värden partner. |
    | Gästen identitet |En identifierare för hello gäst partner |
    | Ta emot inställningarna |Dessa egenskaper gäller tooall har tagits emot av ett avtal. |
    | Skicka inställningar |Dessa egenskaper gäller tooall meddelanden som skickas av ett avtal. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Konfigurera hur ditt avtal handtag mottagna meddelanden

Nu när du har angett hello avtal egenskaper kan konfigurera du hur detta avtal identifierar och hanterar inkommande meddelanden som tagits emot från din partner via det här avtalet.

1.  Under **Lägg till**väljer **tar emot inställningar**.
Konfigurera dessa egenskaper baserat på ditt avtal med hello-partner som utbyter meddelanden med dig. Egenskapsbeskrivningar finns hello tabellerna i det här avsnittet.

    **Ta emot inställningarna** är indelad i följande avsnitt: identifierare, bekräftelse, scheman, kontroll siffror, verifiering och interna inställningar.

    ![Konfigurera ”ta emot inställningar”](./media/logic-apps-enterprise-integration-edifact/edifact-2.png)  

2. När du är klar gör att toosave dina inställningar genom att välja **OK**.

Nu ditt avtal är klar toohandle inkommande valda meddelanden som uppfyller tooyour inställningar.

### <a name="identifiers"></a>Identifierare

| Egenskap | Beskrivning |
| --- | --- |
| UNB6.1 (mottagande referens lösenord) |Ange ett alfanumeriskt värde mellan 1 och 14 tecken. |
| UNB6.2 (mottagande referens kvalificerare) |Ange ett alfanumeriskt värde med minst ett tecken och högst två tecken. |

### <a name="acknowledgments"></a>Bekräftelser

| Egenskap | Beskrivning |
| --- | --- |
| Inleverans av meddelande (CONTRL) |Välj den här kryssrutan tooreturn tekniska (CONTRL) bekräftelse toohello interchange avsändare. hello bekräftelse skickas toohello interchange avsändaren baserat på hello skicka inställningar för hello avtal. |
| Bekräftelse (CONTRL) |Välj den här kryssrutan tooreturn en funktionell (CONTRL) bekräftelse toohello interchange avsändaren hello bekräftelse skickas toohello interchange avsändaren baserat på hello skicka inställningar för hello avtal. |

### <a name="schemas"></a>Scheman

| Egenskap | Beskrivning |
| --- | --- |
| UNH2.1 (TYP) |Välj en uppsättning transaktionstyp. |
| UNH2.2 (VERSION) |Ange versionsnummer för hello-meddelande. (Minimum, ett tecken; maximalt tre tecken). |
| UNH2.3 (VERSION) |Ange versionsnumret för hello-meddelande. (Minimum, ett tecken; maximalt tre tecken). |
| UNH2.5 (ASSOCIERADE TILLDELADE KOD) |Ange hello tilldelade kod. (Maximalt sex tecken. Måste vara alfanumeriskt). |
| UNG2.1 (APP AVSÄNDAR-ID) |Ange ett alfanumeriskt värde med minst ett tecken och högst 35 tecken. |
| UNG2.2 (APP AVSÄNDAREN KOD KVALIFICERARE) |Ange ett alfanumeriskt värde med högst fyra tecken. |
| SCHEMAT |Välj hello överfört tidigare schema som du vill använda toouse från ditt konto med associerade integrering. |

### <a name="control-numbers"></a>Kontrollen siffror
| Egenskap | Beskrivning |
| --- | --- |
| Tillåt inte Interchange kontrollen antal dubbletter |dubbla externa utbyten tooblock, Välj den här egenskapen. Om markerad kontrollerar hello EDIFACT avkoda åtgärd att hello interchange kontrollen (UNB5) för hello emot utbyte inte matchar några tidigare bearbetade interchange kontroll. Om en matchning hittas, bearbetas inte hello utbyte. |
| Leta efter UNB5-dubletter varje (dagar) |Om du väljer toodisallow dubbla interchange kontrollen siffror, kan du ange hello antal dagar när tooperform hello söka genom att ge hello lämpligt värde för den här inställningen. |
| Tillåt inte dubletter av gruppkontrollnummer |tooblock interchanges med dubblettgrupp kontroll-nummer (UNG5), Välj den här egenskapen. |
| Tillåt inte dubletter av transaktionsuppsättningsnummer |tooblock interchanges med dubbla transaktion set kontroll-nummer (UNH1), väljer den här egenskapen. |
| EDIFACT bekräftelse kontrollen tal |toodesignate hello transaktion som referensnummer för användning i en bekräftelse, ange ett värde för hello prefix, ett intervall av referensnummer och ett suffix. |

### <a name="validations"></a>Verifieringar

När du slutför varje validering rad läggs en annan automatiskt. Om du inte anger några regler använder verifieringen hello ”standard” rad.

| Egenskap | Beskrivning |
| --- | --- |
| Meddelandetyp |Välj hello EDI-meddelandetypen. |
| EDI-validering |Validerar EDI-datatyper som definieras av hello schemat EDI egenskaper, längd, tom dataelement och avslutande avgränsare. |
| Utökad validering |Om hello datatypen inte EDI, validering på hello data elementet krav tillåts och upprepning, uppräkningar och elementet längd dataverifiering (min/max). |
| Tillåt inledande/efterföljande nollor |Behåll ytterligare inledande eller avslutande noll och utrymme tecken. Ta inte bort dessa tecken. |
| Ta bort inledande/efterföljande nollor |Ta bort inledande eller efterföljande noll och blanksteg. |
| Avslutande avgränsare princip |Generera avslutande avgränsare. <p>Välj **inte tillåtet** tooprohibit avslutande avgränsare och avgränsare i hello emot utbyte. Om hello interchange har avslutande avgränsare och avgränsare, deklareras hello interchange inte giltig. <p>Välj **valfritt** tooaccept interchanges med eller utan avslutande avgränsare och avgränsare. <p>Välj **obligatoriska** när hello emot interchange måste ha avslutande avgränsare och avgränsare. |

### <a name="internal-settings"></a>Internt inställningar

| Egenskap | Beskrivning |
| --- | --- |
| Skapa tomma XML-taggar om avslutande avgränsare är tillåtna |Välj den här kryssrutan toohave hello interchange avsändaren innehåller tomma XML-taggar för avslutande avgränsare. |
| Dela upp interchange i transaktionsuppsättningar – inaktivera transaktionsuppsättningar vid fel|Parsar varje transaktion som angetts i ett utbyte i ett annat XML-dokument genom att använda hello lämpliga kuvert toohello transaktion uppsättningen. Pausa endast hello transaktion uppsättningar som inte kan valideras. |
| Dela upp interchange i transaktionsuppsättningar – inaktivera interchange vid fel|Parsar varje transaktion som i ett utbyte i ett annat XML-dokument genom att tillämpa lämpliga hello-kuvertet. Pausa hello hela interchange när en eller flera uppsättningar av transaktionen i hello interchange inte kan valideras. | 
| Bevara Interchange – inaktivera transaktion anger vid fel |Lämnar hello interchange intakta, skapar ett XML-dokument för hello hela gruppbaserad utbyte. Pausa endast hello transaktion uppsättningar som inte kan valideras, medan tooprocess anger alla andra transaktioner. |
| Bevara interchange – inaktivera interchange vid fel |Lämnar hello interchange intakta, skapar ett XML-dokument för hello hela gruppbaserad utbyte. Pausa hello hela interchange när en eller flera uppsättningar av transaktionen i hello interchange inte kan valideras. |

## <a name="configure-how-your-agreement-sends-messages"></a>Konfigurera hur ditt avtal skickar meddelanden

Du kan konfigurera hur detta avtal identifierar och hanterar utgående meddelanden som du skickar tooyour partners via det här avtalet.

1.  Under **Lägg till**väljer **skicka inställningar**.
Konfigurera dessa egenskaper baserat på ditt avtal med din partner som utbyter meddelanden med dig. Egenskapsbeskrivningar finns hello tabellerna i det här avsnittet.

    **Skicka inställningar** är indelad i följande avsnitt: identifierare, bekräftelse, scheman, kuvert, teckenuppsättningar och avgränsare, kontroll siffror och verifieringar.

    ![Konfigurera ”skicka inställningar”](./media/logic-apps-enterprise-integration-edifact/edifact-3.png)    

2. När du är klar gör att toosave dina inställningar genom att välja **OK**.

Ditt avtal är nu redo toohandle utgående meddelanden som uppfyller tooyour valda inställningar.

### <a name="identifiers"></a>Identifierare

| Egenskap | Beskrivning |
| --- | --- |
| UNB1.2 (Syntax version) |Välj ett värde mellan **1** och **4**. |
| UNB2.3 (omvänd routingadress för avsändare) |Ange ett alfanumeriskt värde med minst ett tecken och högst 14 tecken. |
| UNB3.3 (omvänd routingadress för mottagare) |Ange ett alfanumeriskt värde med minst ett tecken och högst 14 tecken. |
| UNB6.1 (mottagande referens lösenord) |Ange ett alfanumeriskt värde med minst en och högst 14 tecken. |
| UNB6.2 (mottagande referens kvalificerare) |Ange ett alfanumeriskt värde med minst ett tecken och högst två tecken. |
| UNB7 (program referens-ID) |Ange ett alfanumeriskt värde med minst ett tecken och högst 14 tecken |

### <a name="acknowledgment"></a>Bekräftelse
| Egenskap | Beskrivning |
| --- | --- |
| Inleverans av meddelande (CONTRL) |Markera kryssrutan om hello finns partner förväntar sig tooreceive en teknisk (CONTRL) bekräftelse. Den här inställningen anger att hello värd-partner som skickar hello-meddelande, begär en bekräftelse från hello gäst partner. |
| Bekräftelse (CONTRL) |Markera kryssrutan om hello finns partner förväntar sig tooreceive en funktionell (CONTRL) bekräftelse. Den här inställningen anger att hello värd-partner som skickar hello-meddelande, begär en bekräftelse från hello gäst partner. |
| Skapa SG1/SG4-slinga för godkända transaktionsuppsättningar |Om du väljer toorequest funktionella bekräftelse, Välj den här kryssrutan tooforce generationens SG1/SG4 slingor i funktionella CONTRL bekräftelser för godkända transaktionen uppsättningar. |

### <a name="schemas"></a>Scheman
| Egenskap | Beskrivning |
| --- | --- |
| UNH2.1 (TYP) |Välj en uppsättning transaktionstyp. |
| UNH2.2 (VERSION) |Ange versionsnummer för hello-meddelande. |
| UNH2.3 (VERSION) |Ange versionsnumret för hello-meddelande. |
| SCHEMAT |Välj hello schemat toouse. Scheman finns i ditt konto för integrering. tooaccess dina scheman länka först logikappen integrering konto tooyour. |

### <a name="envelopes"></a>Kuvert
| Egenskap | Beskrivning |
| --- | --- |
| UNB8 (bearbetar prioritetskod) |Ange en alfabetisk värde som inte är mer än ett tecken lång. |
| UNB10 (avtal för kommunikation) |Ange ett alfanumeriskt värde med minst ett tecken och högst 40 tecken. |
| UNB11 (testa Indicator) |Välj den här kryssrutan tooindicate som hello interchange genereras är testdata |
| Tillämpa UNA-segment (tjänststrängråd) |Välj den här kryssrutan toogenerate ett UNA segment för hello interchange toobe skickas. |
| Tillämpa UNG-segment (funktionsgrupprubrik) |Välj den här kryssrutan toocreate gruppering segment i hello funktionella grupphuvud i hello meddelanden som skickas toohello gäst partner. hello är följande värden används toocreate hello UNG segment: <p>För **UNG1**, ange ett alfanumeriskt värde med minst ett tecken och högst sex tecken. <p>För **UNG2.1**, ange ett alfanumeriskt värde med minst ett tecken och högst 35 tecken. <p>För **UNG2.2**, ange ett alfanumeriskt värde med högst fyra tecken. <p>För **UNG3.1**, ange ett alfanumeriskt värde med minst ett tecken och högst 35 tecken. <p>För **UNG3.2**, ange ett alfanumeriskt värde med högst fyra tecken. <p>För **UNG6**, ange ett alfanumeriskt värde med minst en och högst tre tecken. <p>För **UNG7.1**, ange ett alfanumeriskt värde med minst ett tecken och högst tre tecken. <p>För **UNG7.2**, ange ett alfanumeriskt värde med minst ett tecken och högst tre tecken. <p>För **UNG7.3**, ange ett alfanumeriskt värde med minst 1 tecken och högst 6 tecken. <p>För **UNG8**, ange ett alfanumeriskt värde med minst ett tecken och högst 14 tecken. |

### <a name="character-sets-and-separators"></a>Teckenuppsättningar och avgränsare

Andra än hello teckenuppsättning, kan du ange en annan uppsättning avgränsare toobe används för varje meddelandetyp. Om en teckenuppsättning inte anges för ett visst meddelande-schema används hello standardinställda teckenuppsättningen.

| Egenskap | Beskrivning |
| --- | --- |
| UNB1.1 (System Identifier) |Välj hello EDIFACT-tecknet ange toobe som tillämpats på hello utgående utbyte. |
| Schemat |Välj ett schema hello nedrullningsbara listan. När du har slutfört varje rad läggs automatiskt en ny rad. För hello valda schemat ange väljer hello avgränsare som du vill toouse, baserat på hello följande beskrivningar av avgränsare. |
| Indatatypen |Välj en Indatatyp hello nedrullningsbara listan. |
| Komponenten avgränsare |tooseparate sammansatta dataelement, ange ett enskilt tecken. |
| Data elementet avgränsare |tooseparate enkel dataelement i sammansatta dataelement, ange ett enskilt tecken. |
| Segmentera Begränsare |tooindicate hello slutet av en EDI-segment, ange ett enskilt tecken. |
| Suffix |Välj hello tecken som används med hello segment-ID. Om du har angett ett suffix hello får segment Begränsare dataelementet vara tomt. Om hello segment Begränsare lämnas tomt, måste du ange ett suffix. |

### <a name="control-numbers"></a>Kontrollen siffror
| Egenskap | Beskrivning |
| --- | --- |
| UNB5 (Interchange kontrollen tal) |Ange ett prefix, ett intervall med värden för hello interchange kontroll och ett suffix. Dessa värden är används toogenerate ett utgående utbyte. hello prefix och suffix är valfria, medan hello kontrollen tal krävs. hello kontrollnumret ökas för varje nytt meddelande; hello prefix och suffix förblir hello samma. |
| UNG5 (gruppen kontrollen tal) |Ange ett prefix, ett intervall med värden för hello interchange kontroll och ett suffix. Dessa värden används toogenerate hello gruppnumret kontroll. hello prefix och suffix är valfria, medan hello kontrollen tal krävs. hello kontrollnumret ökas för varje nytt meddelande tills hello maxvärdet har nåtts. hello prefix och suffix förblir hello samma. |
| UNH1 (meddelandenummer sidhuvud referens) |Ange ett prefix, ett intervall med värden för hello interchange kontroll och ett suffix. Dessa värden används toogenerate hello meddelandet sidhuvud referensnummer. hello prefix och suffix är valfria, medan hello referensnumret måste anges. hello referensnummer ökas för varje nytt meddelande; hello prefix och suffix förblir hello samma. |

### <a name="validations"></a>Verifieringar

När du slutför varje validering rad läggs en annan automatiskt. Om du inte anger några regler använder verifieringen hello ”standard” rad.

| Egenskap | Beskrivning |
| --- | --- |
| Meddelandetyp |Välj hello EDI-meddelandetypen. |
| EDI-validering |Validerar EDI-datatyper som definieras av hello EDI-egenskaperna för hello schemat, längd, tom dataelement och avslutande avgränsare. |
| Utökad validering |Om hello datatypen inte EDI, validering på hello data elementet krav tillåts och upprepning, uppräkningar och elementet längd dataverifiering (min/max). |
| Tillåt inledande/efterföljande nollor |Behåll ytterligare inledande eller avslutande noll och utrymme tecken. Ta inte bort dessa tecken. |
| Ta bort inledande/efterföljande nollor |Ta bort inledande eller avslutande noll tecken. |
| Avslutande avgränsare princip |Generera avslutande avgränsare. <p>Välj **inte tillåtet** tooprohibit avslutande avgränsare och avgränsare i hello skickas utbyte. Om hello interchange har avslutande avgränsare och avgränsare, deklareras hello interchange inte giltig. <p>Välj **valfritt** toosend interchanges med eller utan avslutande avgränsare och avgränsare. <p>Välj **obligatoriska** om hello skickas interchange måste ha avslutande avgränsare och avgränsare. |

## <a name="find-your-created-agreement"></a>Hitta din skapade avtal

1.  När du är klar med inställningen alla dina avtal egenskaper på hello **Lägg till** bladet välj **OK** toofinish skapar ditt avtal och returnerar tooyour integrering konto bladet.

    Ditt nya avtal nu visas i din **avtal** lista.

2.  Du kan också visa dina avtal i ditt Kontoöversikt för integrering. Välj på ditt kontoblad integration **översikt**och välj hello **avtal** panelen. 

    ![Välj ”avtal” panelen tooview alla avtal](./media/logic-apps-enterprise-integration-edifact/edifact-4.png)   

## <a name="view-swagger-file"></a>Visa Swagger-fil
tooview hello Swagger information för hello EDIFACT-anslutningen finns [EDIFACT](/connectors/edifact/).

## <a name="learn-more"></a>Läs mer
* [Mer information om hello Enterprise-Integrationspaket](logic-apps-enterprise-integration-overview.md "Lär dig mer om Enterprise-Integrationspaket")  

