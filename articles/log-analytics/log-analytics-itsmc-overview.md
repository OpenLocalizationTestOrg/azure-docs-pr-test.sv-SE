---
title: "aaaIT Service Management-anslutningstjänsten i OMS | Microsoft Docs"
description: "Använd hello IT Service Management-anslutningstjänsten toocentrally övervaka och hantera hello ITSM arbetsobjekt i OMS och att snabbt lösa eventuella problem."
services: log-analytics
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 0b1414d9-b0a7-4e4e-a652-d3a6ff1118c4
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: v-jysur
ms.openlocfilehash: 33ed5d432591b836eb41ba982c66c96f22879444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="centrally-manage-itsm-work-items-using-it-service-management-connector-preview"></a>Centralt hantera ITSM arbetsobjekt med hjälp av IT Service Management-anslutningstjänsten (förhandsgranskning)

![IT Service Management-anslutningstjänsten symbol](./media/log-analytics-itsmc/itsmc-symbol.png)

Du kan använda hello IT Service Management koppling (ITSMC) i OMS logganalys toocentrally övervaka och hantera arbetsuppgifter i din ITSM produkter och tjänster.

hello IT Service Management-anslutningstjänsten integrerar dina befintliga IT Service Management (ITSM) produkter och tjänster med OMS logganalys.  hello-lösningen har dubbelriktad integrering med ITSM produkter och tjänster, där det ger hello OMS användare en alternativet toocreate incidenter, aviseringar och händelser i ITSM lösning. hello-anslutningen också importerar data som incidenter och tjänstbegäranden från ITSM lösning i OMS logganalys.

Med IT Service Management-anslutningstjänsten kan du:

  - Centralt övervaka och hantera arbetsobjekt för ITSM produkter och tjänster som används inom organisationen.
  - Skapa ITSM arbetsuppgifter (t.ex. aviseringen, händelse, incident) i ITSM från OMS-varningar och via loggen sökning.
  - Läs incidenter och tjänstbegäranden från din lösning för ITSM och korrelera med relevanta loggdata i logganalys-arbetsytan.
  - Sök efter eventuella oväntade och ovanliga händelser och lösa dem, även innan hello slutanvändare anropa och rapportera dem toohello supportavdelningen.
  - Importera objekt arbetsdata till logganalys och skapa rapporter för KPI-KPI-indikatorn.  Med dessa rapporter kan du identifiera, utvärdera och arbeta med exempel på viktiga saker, till exempel kontroll av skadlig kod.
  - Visa granskat instrumentpaneler för djupare insikter om incidenter, ändringsbegäranden och berörda system.
  - Felsöka snabbare genom att sammanföra med andra hanteringslösningar i hello logganalys-arbetsytan.


## <a name="prerequisites"></a>Krav

tooimport hello ITSM arbetsobjekt i OMS logganalys hello lösning kräver en anslutning mellan hello IT Service Management-anslutningstjänsten i hello OMS och hello IT SM produkter eller tjänster som du importerar från hello arbetsobjekt.


## <a name="configuration"></a>Konfiguration

Lägg till hello IT Service Management-anslutningstjänsten lösning tooyour OMS arbetsutrymme med hello process som beskrivs i [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).

IT Service Management-anslutningstjänsten panelen som du ser i hello lösningsgalleriet:

![kopplingen sida vid sida](./media/log-analytics-itsmc/itsmc-solutions-tile.png)

Efter lyckad dessutom visas hello IT Service Management-anslutningstjänsten under **OMS** > **inställningar** > **anslutna källor.**

![ITSMC ansluten](./media/log-analytics-itsmc/itsmc-overview-solution-in-connected-sources.png)

> [!NOTE]

> Som standard uppdaterar hello IT Service Management-anslutningstjänsten hello anslutning data en gång i en gång per dygn. toorefresh anslutningsdata direkt för redigering eller mall uppdaterar du gör på hello uppdatering visas knappen Nästa tooyour anslutning.

 ![ITSMC uppdatering](./media/log-analytics-itsmc/itsmc-connection-refresh.png)

## <a name="management-packs"></a>Hanteringspaket
Denna lösning kräver inte några management packs.

## <a name="connected-sources"></a>Anslutna källor

hello som följande ITSM produkter och tjänster stöds av hello IT Service Management-anslutningstjänsten:

- [System Center Service Manager (SCSM)](log-analytics-itsmc-connections.md#connect-system-center-service-manager-to-it-service-management-connector-in-oms)

- [ServiceNow](log-analytics-itsmc-connections.md#connect-servicenow-to-it-service-management-connector-in-oms)

- [Provance](log-analytics-itsmc-connections.md#connect-provance-to-it-service-management-connector-in-oms)  

- [Cherwell](log-analytics-itsmc-connections.md#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="using-hello-solution"></a>Med hello-lösning

När du har anslutit hello OMS IT Service Management-anslutningstjänsten med ITSM-tjänsten startar hello logganalys services hello datainsamlingen från hello anslutna ITSM produkter eller tjänster.

> [!NOTE]
> - Data som importeras av IT Service Management-anslutningstjänsten lösning visas i logganalys som händelser med namnet **ServiceDesk_CL**.
- Händelsemeddelandet innehåller ett fält med namnet **ServiceDeskWorkItemType_s**. som ta dess värde som incident eller ändringsbegäran, beroende på hello arbetsobjekt data i hello **ServiceDesk_CL** händelse.

## <a name="input-data"></a>Indata
Arbetsobjekt som har importerats från hello ITSM produkter och tjänster.

hello visas följande information exempel på data som samlas in av hello IT Service Management-anslutningstjänsten:

> [!NOTE]
> Beroende på hello typ av arbetsobjekt importeras till Log Analytics **ServiceDesk_CL** innehåller hello följande fält:

**Arbetsobjekt:** **incidenter**  
ServiceDeskWorkItemType_s = ”Incident”

**Fält**

- ServiceDeskConnectionName
- ID för skrivbord
- Status
- Angelägenhetsgrad
- Påverkan
- Prioritet
- Eskalering
- Skapat av
- Löst av
- Stängts av
- Källa
- Tilldelad till
- Kategori
- Rubrik
- Beskrivning
- Skapad datum
- Stängningsdatum
- Lösningsdatum
- Senast ändrad datum
- Dator


**Arbetsobjekt:** **ändringsbegäranden**

ServiceDeskWorkItemType_s = ”ändra begäran”

**Fält**
- ServiceDeskConnectionName
- ID för skrivbord
- Skapat av
- Stängts av
- Källa
- Tilldelad till
- Rubrik
- Typ
- Kategori
- Status
- Eskalering
- Konflikt Status
- Angelägenhetsgrad
- Prioritet
- Risk
- Påverkan
- Tilldelad till
- Skapad datum
- Stängningsdatum
- Senast ändrad datum
- Begärt datum
- Planerat startdatum
- Planerat slutdatum
- Startdatum för arbete
- Slutdatum för arbete
- Beskrivning
- Dator

## <a name="output-data-for-a-servicenow-incident"></a>Utdata för en ServiceNow-incident

| OMS-fält | ITSM fält |
|:--- |:--- |
| ServiceDeskId_s| Tal |
| IncidentState_s | Status |
| Urgency_s |Angelägenhetsgrad |
| Impact_s |Påverkan|
| Priority_s | Prioritet |
| CreatedBy_s | Öppnas av |
| ResolvedBy_s | Löst av|
| ClosedBy_s  | Stängts av |
| Source_s| Kontakta typ |
| AssignedTo_s | Tilldelade för |
| Category_s | Kategori |
| Title_s|  Kort beskrivning |
| Description_s|  Anteckningar |
| CreatedDate_t|  Öppna |
| ClosedDate_t| Stängd|
| ResolvedDate_t|Löst|
| Dator  | konfigurationsobjekt |

## <a name="output-data-for-a-servicenow-change-request"></a>Utdata för en ServiceNow ändringsbegäran

| OMS-fält | ITSM fält |
|:--- |:--- |
| ServiceDeskId_s| Tal |
| CreatedBy_s | Begärd av |
| ClosedBy_s | Stängts av |
| AssignedTo_s | Tilldelade för |
| Title_s|  Kort beskrivning |
| Type_s|  Typ |
| Category_s|  Catgory |
| CRState_s|  Status|
| Urgency_s|  Angelägenhetsgrad |
| Priority_s| Prioritet|
| Risk_s| Risk|
| Impact_s| Påverkan|
| RequestedDate_t  | Begärt efter datum |
| ClosedDate_t | Stängningsdatum |
| PlannedStartDate_t  |     Planerat startdatum |
| PlannedEndDate_t  |   Planerat slutdatum |
| WorkStartDate_t  | Verkligt startdatum |
| WorkEndDate_t | Verkligt slutdatum|
| Description_s | Beskrivning |
| Dator  | Konfigurationsobjekt |

**Exempel logganalys skärmen för ITSM data:**

![Log Analytics skärmen](./media/log-analytics-itsmc/itsmc-overview-sample-log-analytics.png)

## <a name="it-service-management-connector--integration-with-other-oms-solutions"></a>IT Service Management-anslutningstjänsten – integrering med andra OMS-lösningar

IT Service Management-anslutningstjänsten stöder för närvarande integrering med hello Tjänstkarta lösning.

Tjänstkarta upptäcker automatiskt hello programkomponenter i Windows och Linux-system och kartor hello kommunikation mellan tjänster. Det gör att du tooview dina servrar som du betrakta dem – som sammanlänkade system som levererar kritiska tjänster. Tjänstkarta visar anslutningar mellan servrar, processer och portar över en TCP-ansluten arkitektur med ingen konfiguration krävs för andra än installation av en agent. Mer information: [Tjänstkarta](../operations-management-suite/operations-management-suite-service-map.md).

Du kan visa hello service supportavdelningen som har skapats i hello ITSM lösningar som visas i följande exempel hello med den här integreringen:

![Integrerad lösning ](./media/log-analytics-itsmc/itsmc-overview-integrated-solutions.png)
## <a name="create-itsm-work-items-for-oms-alerts"></a>Skapa ITSM arbetsobjekt för OMS-aviseringar

Du kan skapa tillhörande arbetsobjekt i hello anslutna ITSM källor för hello OMS-aviseringar.  toodo detta, Använd hello nedan:

1. Från **loggen Sök** och köra en sökning frågan tooview loggdata. Frågeresultatet är hello källan för arbetsobjekt.
2. I **loggen Sök**, klickar du på **avisering** tooopen hello **lägga till Varningsregeln** sidan.

    ![Log Analytics skärmen](./media/log-analytics-itsmc/itsmc-work-items-for-oms-alerts.png)

3. På hello **lägga till Varningsregeln** fönster, ange information om hello som krävs för **namn**, **allvarlighetsgrad**, **sökfråga**, och  **Varna kriterier** (fönstret/Tidsmått för mätning).
4. Välj **Ja** för **ITSM åtgärder**.
5. Välja ITSM anslutning hello **Välj anslutning** lista.
6. Ange hello information som krävs.
7. toocreate ett separat arbetsobjekt för varje loggpost för den här aviseringen, Välj hello **skapa enskilda arbetsobjekt för varje loggpost** kryssrutan.

    Eller

    Låt kryssrutan avmarkerad toocreate endast en arbetsuppgiften för valfritt antal loggposter i den här aviseringen.

7. Klicka på **Spara**.

hello OMS aviseringen kommer att skapas under **aviseringar**. Hej motsvarande ITSM anslutning arbete objekt skapas när hello angivna aviseringen är uppfyllt.

## <a name="create-itsm-work-items-from-oms-logs"></a>Skapa ITSM arbetsobjekt från OMS-loggar

Du kan skapa arbetsobjekt i hello anslutna ITSM källor med hjälp av OMS loggen sökning. toodo detta, Använd hello nedan:

1. Från **loggen Sök**söka hello krävs data, välja hello information och klicka på **skapa arbetsobjekt**.

    Hej **skapa ITSM arbetsobjekt** visas:

    ![Log Analytics skärmen](media/log-analytics-itsmc/itsmc-work-items-from-oms-logs.png)

2.   Lägg till hello följande information:

  - **Rubrik för arbetsobjekt**: titel hello arbetsobjekt.
  - **Beskrivning av arbetsuppgift**: beskrivning för hello nya arbetsuppgiften.
  - **Påverkas datorn**: namnet på hello dator där den här loggdata hittades.
  - **Välj anslutning**: ITSM anslutning som du vill toocreate arbetsuppgiften.
  - **Arbetsobjektet**: typ av arbetsobjekt.

3. toouse en befintlig mall för arbetsobjekt för en incident, klickar du på **Ja** under **generera arbetsobjekt utifrån hello mallen** alternativ och klickar sedan på **skapa**.

    Eller:

    Klicka på **nr** om du vill tooprovide din anpassade värden.

4. Ange hello lämpliga värden i hello **kontakta typen**, **inverkan**, **angelägenhetsgrad**, **kategori**, och **underkategori**  textrutor och klicka sedan på **skapa**.

hello arbetsobjekt skapas i hello ITSM som du kan också visa i OMS.

## <a name="troubleshoot-itsm-connections-in-oms"></a>Felsöka ITSM anslutningar i OMS
1.  Om anslutningen misslyckas från Gränssnittet för anslutna källa och du får hello **fel spara anslutningen** visas, hello följande:
 - Vid ServiceNow, Cherwell och Provance anslutningar, se till att du korrekt angivna hello användarnamn/lösenord och klient-ID: T/klienthemlighet för varje hello anslutningar. Om hello problemet kvarstår bör du kontrollera om du har tillräckliga privilegier i hello motsvarande ITSM produkten toomake hello-anslutningen.
 - Om Service Manager, kontrollera att hello webbprogrammet har distribuerats och hybridanslutningen har skapats. tooverify hello finns har anslutning med hello lokal Service Manager-datorn, gå hello Web app-URL som anges i dokumentationen för hello för att göra hello [hybridanslutning](log-analytics-itsmc-connections.md#configure-the-hybrid-connection).

2.  Om inte komma har synkroniserats data från ServiceNow i OMS, se till att den hello ServiceNow-instansen inte är i viloläge. Detta kan senare inträffa i hello ServiceNow Dev instanser vid inaktivitet. Annan, rapporten hello problemet.
3.  Om aviseringar komma skickas från OMS men arbetsobjekt komma skapas inte i ITSM produkt eller konfigurationsobjekt inte får skapas/länkade toowork objekt eller en allmän information, hello följande:
 -  IT Service Management-anslutningstjänsten lösning i OMS-portalen kan vara används tooget en sammanfattning av anslutningar/artiklar/arbetsdatorer osv. Klicka på hello felmeddelande hello status-bladet, navigera för**loggen Sök** och visa hello-anslutning som har hello fel med hjälp av hello information i hello felmeddelande.
 - Du kan visa hello fel/relaterad information direkt i hello **loggen Sök** sidan med *typ = ServiceDeskLog_CL*.

## <a name="troubleshoot-service-manager-web-app-deployment"></a>Felsöka Service Manager Web App-distribution
1.  Vid problem med web app-distribution, se till att du har tillräckliga behörigheter i hello prenumeration nämns toocreate/distribuera resurser.
2.  Om **objektreferens har angetts tooinstance objektets** felmeddelande när du kör hello [skriptet](log-analytics-itsmc-service-manager-script.md) se till att du har angett giltiga värden under **Användarkonfiguration**avsnitt.
3.  Se till att hello begärda resursprovidern har registrerats i hello prenumeration om du inte toocreate service bus relay-namnrymd. Om du inte har registrerats, skapar du det manuellt från hello Azure-portalen. Du kan också skapa den när [skapar hello hybridanslutning](log-analytics-itsmc-connections.md#configure-the-hybrid-connection) från hello Azure-portalen.


## <a name="contact-us"></a>Kontakta oss

För alla frågor eller feedback om hello IT Service Management-anslutningstjänsten, kontaktar du oss på [ omsitsmfeedback@microsoft.com ](mailto:omsitsmfeedback@microsoft.com).

## <a name="next-steps"></a>Nästa steg
[Lägg till ITSM produkter och tjänster tooIT Service Management-anslutningstjänsten](log-analytics-itsmc-connections.md).
