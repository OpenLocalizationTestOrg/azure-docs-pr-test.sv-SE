---
title: "Kom igång med fjärråtkomst övervakningslösning - Azure | Microsoft Docs"
description: "Den här kursen använder simulerade scenarier för att införa den fjärranslutna förkonfigurerade övervakningslösning. Dessa scenarier skapas när du distribuerar fjärråtkomst övervakning förkonfigurerade lösningen för första gången."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 12/12/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: e57ad43cc9a82e3944e93c6500ad5740818d10cc
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/13/2017
---
# <a name="explore-the-capabilities-of-the-remote-monitoring-preconfigured-solution"></a>Utforska funktionerna i fjärråtkomst övervakning förkonfigurerade lösningen

Den här kursen visar de viktigaste funktionerna i den fjärranslutna övervakningslösning. För att införa dessa funktioner demonstreras kursen vanliga kundscenarier med en simulerad IoT-program för företaget Contoso.

I kursen får du förstår typiska IoT-scenarier ger remote övervakningslösning out box.

I den här guiden får du lära dig hur man:

>[!div class="checklist"]
> * Visualisera och filtrera enheter på instrumentpanelen
> * Svara på ett larm
> * Uppdatera den inbyggda programvaran i dina enheter
> * Ordna dina tillgångar

## <a name="prerequisites"></a>Krav

Den här kursen behöver en distribuerad instans av den fjärranslutna övervakningslösning i din Azure-prenumeration.

Om du inte har distribuerat remote övervakningslösning ännu, bör du genomföra den [Distribuera fjärråtkomst övervakning förkonfigurerade lösningen](iot-suite-remote-monitoring-deploy.md) kursen.

## <a name="the-contoso-sample-iot-deployment"></a>Contoso IoT exempeldistribution

Du kan använda Contoso IoT exempeldistribution för att förstå grundläggande scenarier fjärrdatabasen övervakningslösning ger out box. Dessa scenarier bygger på verkliga IoT-distributioner. Troligen kommer du vill anpassa remote övervakningslösning för att uppfylla dina specifika krav, men Contoso-exempel hjälper dig att lära dig grunderna.

> [!NOTE]
> Om du använde CLI för att distribuera förkonfigurerade lösningen filen `deployment-{your deployment name}-output.json` innehåller information om distribution till exempel URL för åtkomst av distribuerade exemplet.

Contoso-exempel etablerar en uppsättning simulerade enheter och regler för att fungera på dem. När du förstår de grundläggande scenarierna kan du utforska av lösningen funktioner i [utför avancerad övervakning med hjälp av den fjärranslutna övervakningslösning](iot-suite-remote-monitoring-monitor.md).

Contoso är ett företag som hanterar en mängd olika tillgångar i olika miljöer. Contoso planerar att använda molnbaserade IoT-program att övervaka och hantera flera resurser från en centraliserad program. Följande avsnitt innehåller en sammanfattning av den första konfigurationen av Contoso-exempel:

> [!NOTE]
> Contoso-demonstrationen är bara ett sätt att etablera simulerade enheter och skapa regler. Andra alternativ för etablering omfattar att skapa egna anpassade enheter. Mer information om hur du skapar dina egna enheter och regler finns [hantera och konfigurera dina enheter](iot-suite-remote-monitoring-manage.md) och [identifiera problem med tröskelvärdesbaserad regler](iot-suite-remote-monitoring-automate.md).

### <a name="contoso-devices"></a>Contoso-enheter

Contoso använder olika typer av smarta enheter. Dessa enheter uppfylla olika roller för det företag som skickar olika telemetri dataströmmar. Dessutom varje typ av enhet har olika enhetens egenskaper och metoder som stöds.

I följande tabell visas en sammanfattning av de etablerade enhetstyper:

| Enhetstyp        | Telemetri                                  | Egenskaper                                  | Taggar                    | Metoder                                                                                      |
| ------------------ | ------------------------------------------ | ------------------------------------------- | ----------------------- | -------------------------------------------------------------------------------------------- |
| Kylaggregat            | Tryck på temperatur, fuktighet            | Typ, version på inbyggd programvara, modell               | Plats, våning, plats | Starta om, inbyggd programvara uppdateringen nödfall ventilen versionen ökad belastning                          |
| Prototyper enhet | Temperatur, tryck, geografiska plats        | Typ, version på inbyggd programvara, modell               | Plats, läge          | Starta om, Firmware-uppdatering, flytta enheten, Stop-enhet, temperatur versionen, temperatur öka |
| Motorn             | Tanken bränsle genom kylvätska sensor Vibration | Typ, version på inbyggd programvara, modell               | Plats, våning, plats | Starta om, Firmware-uppdatering tom tanken, Fill tanken                                              |
| Lastbil              | Geografiska plats, hastighet, last temperatur     | Typ, version på inbyggd programvara, modell               | Plats, Läs in          | Lägre last temperatur, öka last temperatur, Firmware-uppdatering                         |
| Snabba           | Våning Vibration, temperatur              | Typ, version på inbyggd programvara, modell, geografiska plats | Plats, plats        | Stoppa snabba Start snabba Firmware-uppdatering                                               |

> [!NOTE]
> Contoso-demo exempel tillhandahåller två enheter per typ. En fungerar på rätt sätt inom de gränser som har definierats som vanligt av Contoso för varje typ, och det har någon typ av fel. I nästa avsnitt du lär dig mer om de regler som Contoso definierar för enheter. De här reglerna definiera gränser för korrekt beteende.

### <a name="contoso-rules"></a>Contoso-regler

Operatörer på Contoso vet de tröskelvärden som avgör om en enhet fungerar korrekt. Till exempel fungerar en kylaggregat inte korrekt om trycket rapporterar är större än 250 PSI. Följande tabell visar tröskelvärdet regler på Contoso definierar för varje typ av enhet:

| Regelnamn | Beskrivning | Tröskelvärde | Allvarsgrad | Berörda enheter |
| --------- | ----------- | --------- | -------- | ---------------- |
| Kylaggregat tryck för hög | Aviseringar om chillers nå högre än normalt tryck nivåer   |P > 250 psi       | Kritiskt | Chillers            |
| Prototyper enheten temp för hög  | Aviseringar om prototyper enheter nå högre än normal temperatur nivåer  |T > 80&deg; F |Kritiskt | Prototyper enheter |
| Motorn tanken tom  | Aviseringar om motorn bränsletanken går tom                     | F < 5 gallon | Information     | Motorer             |
| Högre än normal last temperatur | Aviseringar om lastbils last temperatur är högre än normalt                 | T < 45&deg; F |Varning  | Lastbilar              |
| Snabba vibration stoppades      | Aviseringar om snabba slutar helt (baserat på vibration nivå)                     | V < 0,1 mm |Varning  | Hissar           |

### <a name="operate-the-contoso-sample-deployment"></a>Fungera exempeldistribution Contoso

Nu har du sett den första installationen i Contoso-exemplet. I följande avsnitt beskrivs tre scenarier i Contoso-exempel som visar hur en operatör kan använda förkonfigurerade lösningen.

## <a name="respond-to-a-pressure-alarm"></a>Svara på ett larm för hög belastning

Det här scenariot visar hur du identifierar och svara på ett larm som utlöses av en kylaggregat enhet. Kylaggregat finns i Redmond, skapa 43 Våning 2.

Som operatör kan se du i instrumentpanelen för att det finns ett larm som rör en kylaggregat tryck. Du kan Panorera och Zooma i kartan för att se mer information.

1. På den **instrumentpanelen** sidan den **System larm** rutnät som du kan se den **kylaggregat tryck för hög** larm. Kylaggregat markeras på kartan:

    ![Instrumentpanelen visar trycket larm och enheten på kartan](media/iot-suite-remote-monitoring-explore/dashboardalarm.png)

1. Om du vill visa information om enhet och telemetri, klickar du på den markerade kylaggregat på kartan. Telemetrin visar en insamling tryck:

    ![Välj enhet på kartan för att visa detaljer](media/iot-suite-remote-monitoring-explore/dashboarddetail.png)

1. Stäng **enheten detalj**.

1. Navigera till den **Underhåll** väljer **Underhåll** på navigeringsmenyn.

På den **Underhåll** kan du visa information om regeln som utlöste kylaggregat trycket larm.

1. Listan över meddelanden visar antalet gånger som larmet har utlösts, bekräftelser och öppna och stänga larm:

    ![Underhåll visar listan över larm som har utlösts](media/iot-suite-remote-monitoring-explore/maintenancealarmlist.png)

1. Första larmet i listan är det senaste. Klicka på den **kylaggregat trycket** larm visa associerade enheter och telemetri. Telemetrin visar en insamling minnesbelastning för kylaggregat:

    ![Underhåll visar telemetri för valda larm](media/iot-suite-remote-monitoring-explore/maintenancetelemetry.png)

Nu har du identifierat problemet som utlöste larm och den associerade enheten. Som operatör är nästa steg att bekräfta larm och åtgärda problemet.

1. För att indikera att du nu arbetar med larm, ändra den **larm status** till **godkänd**:

    ![Välj och bekräfta larm](media/iot-suite-remote-monitoring-explore/maintenanceacknowledge.png)

1. För att fungera på kylaggregat markerar du den och väljer sedan **schema**. Välj **EmergencyValveRelease**, lägga till ett jobbnamn **ChillerPressureRelease**, och välj **tillämpa**. Dessa inställningar skapar ett jobb som körs omedelbart:

    ![Välj enheten och schemalägga en åtgärd](media/iot-suite-remote-monitoring-explore/maintenanceschedule.png)

1. Om du vill visa jobbstatus återgå till den **Underhåll** sidan och visa en lista över jobb i den **jobb** vyn. Du kan se jobbet kördes för att frisläppa ventilen trycket på kylaggregat:

    ![Status för jobben i vyn jobb](media/iot-suite-remote-monitoring-explore/maintenancerunningjob.png)

Kontrollera slutligen att telemetri värdena från kylaggregat är tillbaka till normal.

1. Om du vill visa rutnätet larm, navigera till den **instrumentpanelen** sidan.

1. Att visa enheter telemetri, markerar enheten för det ursprungliga larmet på kartan och bekräfta att du är tillbaka till normal.

1. Om du vill stänga incidenten, navigera till den **Underhåll** väljer larm, och ange status till **stängd**:

    ![Välj och Stäng larm](media/iot-suite-remote-monitoring-explore/maintenanceclose.png)

## <a name="update-device-firmware"></a>Uppdatera enhetens inbyggda programvara

Contoso testar en ny typ av enhet i fältet. Som en del av den tester, som du behöver se till att den inbyggda programvaran uppdaterar fungerar korrekt. Följande steg visar hur du använder fjärråtkomst övervakningslösning för att uppdatera den inbyggda programvaran på flera enheter.

Använd för att utföra nödvändiga enhetens hanteringsuppgifter i **enheter** sidan. Starta genom att filtrera för alla prototyper enheter:

1. Navigera till den **enheter** sidan. Välj den **prototyper enheter** filter i den **filter** listrutan:

    ![Använd ett filter på enhetssidan](media/iot-suite-remote-monitoring-explore/devicesprototypingfilter.png)

    > [!TIP]
    > Klicka på **hantera** att hantera de tillgängliga filtren.

1. Välj något av prototyper enheter:

    ![Välj en enhet på enhetssidan](media/iot-suite-remote-monitoring-explore/devicesselect.png)

1. Klicka på den **schema** knappen och välj sedan **Firmware-uppdatering**. Ange värden för **jobbnamn** och **Firmware URI**. Välj **tillämpa** att schemalägga jobbet ska köras nu:

    ![Schemalägga firmware-uppdatering på enheten](media/iot-suite-remote-monitoring-explore/devicesschedulefirmware.png)

    > [!NOTE]
    > Med de simulerade enheterna kan du använda valfri URL som du vill som den **Firmware URI** värde. Simulerade enheter inte kommer åt URL: en.

1. Observera hur många enheter som påverkar jobbet och välja **Verkställ**:

    ![Skicka Uppdateringsjobbet inbyggd programvara](media/iot-suite-remote-monitoring-explore/devicessubmitupdate.png)

Du kan använda den **Underhåll** att spåra jobbet när den körs.

1. Om du vill visa en lista över jobb, navigera till den **Underhåll** och klickar på **jobb**.

1. Leta upp den händelse som rör jobbet som du skapade. Kontrollera att uppdateringen firmware initierades korrekt.

Du kan skapa ett filter för att verifiera versionen av inbyggd programvara uppdateras korrekt.

1. Om du vill skapa ett filter, navigera till den **enheter** och välja **hantera filter**:

    ![Hantera enhetsfilter](media/iot-suite-remote-monitoring-explore/devicesmanagefilters.png)

1. Skapa ett filter som innehåller endast enheter med den nya versionen av inbyggd programvara:

    ![Skapa enhetsfilter](media/iot-suite-remote-monitoring-explore/devicescreatefilter.png)

1. Gå tillbaka till den **enheter** sidan och kontrollera att enheten har den nya versionen av inbyggd programvara.

## <a name="organize-your-assets"></a>Ordna dina tillgångar

Contoso har två olika teamen till fältet aktiviteter:

* Skapande av Smart team hanterar chillers och hissar motorer.
* Smart Vehicle team hanterar lastbilar och prototyper enheter.

Om du vill göra det lättare som en operatör att ordna och hantera dina enheter, vill du tagga dem med lämplig Teamnamn.

Du kan skapa taggnamn ska användas med enheter.

1. Om du vill visa alla enheter, navigera till den **enheter** och väljer den **alla enheter** filter:

    ![Visa alla enheter](media/iot-suite-remote-monitoring-explore/devicesalldevices.png)

1. Välj den **lastbilar** och **prototyper** enheter. Välj **taggen**:

    ![Välj enheter som prototyp och lastbil](media/iot-suite-remote-monitoring-explore/devicesmultiselect.png)

1. Välj **taggen** och sedan skapa en ny text-tagg som kallas **FieldService** med ett värde **ConnectedVehicle**. Välj ett namn för jobbet. Klicka på **Verkställ**:

    ![Lägg till tagg prototyp och lastbil enheter](media/iot-suite-remote-monitoring-explore/devicesaddtag.png)

1. Välj den **kylaggregat**, **snabba**, och **motorn** enheter. Välj **taggen**:

    ![Välj kylaggregat-motorn och snabba enheter](media/iot-suite-remote-monitoring-explore/devicesmultiselect2.png)

1. Välj **taggen** och sedan skapa en ny text-tagg som kallas **FieldService** med ett värde **SmartBuilding**. Välj ett namn för jobbet. Klicka på **spara**:

    ![Lägg till tagg kylaggregat-motorn och snabba enheter](media/iot-suite-remote-monitoring-explore/devicesaddtag2.png)

Du kan använda taggvärden för att skapa filter.

1. På den **enheter** väljer **hantera filter**:

    ![Hantera enhetsfilter](media/iot-suite-remote-monitoring-explore/devicesmanagefilters.png)

1. Skapa ett nytt filter som använder taggnamnet **FieldService** och värdet **SmartBuilding**. Spara filter som **Smart byggnad**.

1. Skapa ett nytt filter som använder taggnamnet **FieldService** och värdet **ConnectedVehicle**. Spara filter som **anslutna fordon**.

Contoso-operatorn kan nu fråga enheter utifrån operativsystem teamet utan att behöva ändra något på enheter.

## <a name="next-steps"></a>Nästa steg

I kursen får du lärt dig att:

>[!div class="checklist"]
> * Visualisera och filtrera enheter på instrumentpanelen
> * Svara på ett larm
> * Uppdatera den inbyggda programvaran i dina enheter
> * Ordna dina tillgångar

Nu när du har gjort den fjärranslutna övervakningslösning är föreslagna nästa steg att lära dig om de avancerade funktionerna i den fjärranslutna övervakningslösning:

* [Övervaka enheterna](./iot-suite-remote-monitoring-monitor.md).
* [Hantera dina enheter](./iot-suite-remote-monitoring-manage.md).
* [Automatisera din lösning med regler](./iot-suite-remote-monitoring-automate.md).
* [Underhålla din lösning](./iot-suite-remote-monitoring-maintain.md).
