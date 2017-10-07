---
title: aaaGetting Started - hotet Modeling verktyget - Azure | Microsoft Docs
description: "Det här är en djupare översikt syntaxmarkering hello hot Modeling verktyget i åtgärden."
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 75ef139071e8abd0e743aa17b443a6e353f29372
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-hello-threat-modeling-tool"></a>Komma igång med hello hot Modeling verktyget

hello moln- och Enterprise-säkerhetsverktyg släpptes hello hot Modeling verktyget Preview tidigare i år som ett kostnadsfritt  **[Klicka på Hämta](https://aka.ms/tmtpreview)**. hello ändring i leveransmekanismen gör att vi toopush hello senaste förbättringar och felkorrigeringar toocustomers varje gång de öppnar hello verktyg, vilket gör det enklare toomaintain och användning.
Den här artikeln tar dig igenom hello processen för att komma igång med hello Microsoft SDL hot modeling metod och visar hur toouse hello verktyget toodevelop bra hot modeller som ett stamnät av säkerhet.

Den här artikeln bygger på befintliga kunskaper i hello SDL hot modeling metod. En snabböversikt finns för**[hot Modeling webbprogram](https://msdn.microsoft.com/library/ms978516.aspx)**  och en arkiverad version av  **[upptäcka säkerhet fel med hello STRIDE metoden](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy)**  MSDN-artikeln publiceras i 2006.

Sammanfatta tooquickly, hello sättet är att skapa ett diagram, identifiera hot, minimera dem och verifiera varje lösning. Här är ett diagram som visar den här processen:

![SDL-processen](./media/azure-security-threat-modeling-tool/sdlapproach.png)

## <a name="starting-hello-threat-modeling-process"></a>Starta hello hot modellera processen

När du startar hello hot Modeling verktyget Lägg märke till några saker som visas i bild hello:

![Tom startsida](./media/azure-security-threat-modeling-tool/tmtstart.png)

### <a name="threat-model-section"></a>Hot modellen avsnitt

| Komponent                                   | Information                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Feedback, förslag och problem med knappen** | Tar du hello  **[MSDN-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=sdlprocess)**  för allt som rör SDL. Den ger dig en möjlighet tooread via vad andra användare gör, tillsammans med lösningar och rekommendationer. Om du fortfarande inte hittar det du söker kan e- tmtextsupport@microsoft.com för vår support-teamet toohelp du                                                                                                                            |
| **Skapa en modell**                          | Öppnar början för du toodraw diagrammet. Se till att tooselect vilken mall som du vill att toouse för din modell                                                                                                                                                                                                                                                                                                                                                                       |
| **Mall för nya modeller**                 | Du måste välja vilken mall toouse innan du skapar en modell. Vår Huvudmall är hello Azure hot modellen mallen, som innehåller Azure-specifika stenciler, hot och åtgärder. Välj hello SDL TM Knowledge Base hello nedrullningsbara menyn för allmän modeller. Om du vill använda toocreate en egen mall eller skicka en ny för alla användare? Kolla in våra  **[mall databasen](https://github.com/Microsoft/threat-modeling-templates)**  GitHub Page toolearn mer                              |
| **Öppna en modell**                            | <p>Öppnar sparat hot modeller. hello nyligen öppnade modeller funktionen är bra om du behöver tooopen senaste filerna. När du hovrar över hello markeringen hittar du 2 sätt tooopen modeller:</p><p><ul><li>Öppna från den här datorn – klassiska sätt att öppna en fil som använder lokal lagring</li><li>Öppna från OneDrive-team kan använda mappar i OneDrive toosave och dela sina hot modeller i en enda plats toohelp att öka produktiviteten och samarbete</li></ul></p> |
| **Kom igång-Guide**                   | Öppnas hello  **[hot Modeling verktyget](./azure-security-threat-modeling-tool.md)**  huvudsida                                                                                                                                                                                                                                                                                                                                                                                            |

### <a name="template-section"></a>Template-sektion

| Komponent               | Information                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Skapa en ny mall** | Öppnar en tom mall för du toobuild på. Om du inte har omfattande kunskaper i att skapa mallar från början, rekommenderar vi att du toobuild från befintliga |
| **Öppna mall**       | Öppnar befintliga mallar för du toomake ändringar för                                                                                                             |

hello hot Modeling verktyget teamet arbetar ständigt tooimprove verktyget funktioner och erfarenheter. Några mindre ändringar kan göras för hello kursen hello år, men alla viktiga ändringar kräver omskrivningar i hello guide. Se tooit ofta tooensure du hämta senaste hello-meddelanden.

## <a name="building-a-model"></a>Skapa en modell

I det här avsnittet så vi:

- Cristina (utvecklare)
- Ricardo (Programhanteraren) och
- Ashish (en tester)

De kommer via hello arbetet med att utveckla sina första hotmodell.

> Ricardo: Hej Cristina, kapitel hello hot modelldiagram och vill toomake att vi hello information rätt. Kan du hjälpa mig titta igenom?
> Cristina: absolut. Låt oss ta en titt.
> Ricardo delar sina skärmen med Cristina öppnar hello-verktyget.

![Grundläggande Hotmodell](./media/azure-security-threat-modeling-tool/basictmt.png)

> Cristina: Ok, verkar enkla, men kan du hjälpa mig via den?
> Ricardo: till! Här är hello uppdelning:
> - Vår mänsklig användaren ritas som en extern enhet, en ruta
> - De skickar kommandon tooour webbserver – hello cirkel
> - hello webbservern samråd med en databas (två parallella linjer)

Vad Ricardo har visat Cristina är en DFD kort för  **[dataflödesdiagram](https://en.wikipedia.org/wiki/Data_flow_diagram)**. hello hot Modeling verktyget kan användarna toospecify förtroendegränser, visas med röd hello streckade linjer, tooshow där olika enheter finns i kontrollen. IT-administratörer kräver en Active Directory-system för autentisering, så hello Active Directory är utanför deras kontroll.

> Cristina: Verkar rätt toome. Nyheter om hello hot?
> Ricardo: Jag visar.

## <a name="analyzing-threats"></a>Analysera hot

När han klickar på hello analysvyn från hello ikonen Menyvalet (fil med förstoringsglas), han tas tooa lista över genererade hot hello hot Modeling verktyget hitta baserat på hello standardmallen som använder hello SDL-metoden anropas  **[ STRIDE (-förfalskning, manipulering, avslöjande av information, Denial of Service och rättighetsökning)](https://en.wikipedia.org/wiki/STRIDE_(security))**. hello idé är att programvaran kommer under en förutsägbar uppsättning hot, som finns med hjälp av dessa 6 kategorier.

Den här metoden fungerar som säkra hemmet genom att säkerställa att varje dörr- och har en mekanism för låsning på plats innan du lägger till ett larmsystem eller jaga efter hello tjuv.

![Grundläggande hot](./media/azure-security-threat-modeling-tool/basicthreats.png)

Ricardo börjar genom att välja hello första objektet på hello-listan. Här är vad som händer:

Först förbättrad hello interaktion mellan hello två stencilerna

![Interaktion](./media/azure-security-threat-modeling-tool/interaction.png)

Andra, ytterligare information om hello hot visas i egenskapsfönstret för hello hot

![Interaktion Info](./media/azure-security-threat-modeling-tool/interactioninfo.png)

hello genereras hot kan han förstå potentiella design-fel. Hej STRIDE kategorisering ger honom en uppfattning om potentiella angreppsmetoder, medan hello ytterligare beskrivning anger att exakt vad har felaktigt, tillsammans med potentiella sätt toomitigate den. Han kan använda redigerbart fält toowrite anteckningar i hello motivering information eller ändra prioritet klassificeringar beroende på företagets bugg fältet.

Azure-mallar har ytterligare information toohelp användarna att förstå inte bara vad som är fel, utan också hur toofix den genom att lägga till beskrivningar, exempel och länkar tooAzure-specifika-dokumentationen.

hello beskrivning gjorts honom inser hello viktigt för att lägga till en användare för autentisering mekanism tooprevent förfalskas, avslöja hello första hot toobe arbetat med. Några minuter till hello diskussion med Cristina, förstå de hello vikten av implementera åtkomstkontroll och roller. Ricardo fylls i vissa snabba anteckningar toomake till dessa genomfördes.

Eftersom Ricardo försattes i hello hot under avslöjande av Information, insåg han hello plan för åtkomstkontroll krävs vissa skrivskyddad konton för granskning och rapportgenerering. Han funderat över om detta ska vara ett nytt hot, men hello åtgärder har hello detsamma, så han anges detta hello hot.
Han tänkte lite mer om avslöjande av information och insåg hello band tänkte tooneed kryptering, ett jobb för hello operations-teamet.

Inte tillämpligt toohello design förfallodatum tooexisting åtgärder eller säkerhet garanterar kan ändras för hot ”saknas” från hello Status i listrutan. Det finns tre alternativ: inte startat – standardvalet måste undersökningen – används toofollow för artiklar och Mitigated – när den fullständiga har arbetat med.

## <a name="reports--sharing"></a>Rapporter & delning

När Ricardo passerar hello lista med Cristina och lägger till viktiga anteckningar, åtgärder/skäl, prioritet och status ändras, he väljer rapporter -> Skapa fullständig rapport -> Spara rapporten, som visar en bra rapport för honom toogo via med kollegor tooensure hello rätt säkerhet arbete har implementerats.

![Interaktion Info](./media/azure-security-threat-modeling-tool/report.png)

Om Ricardo vill tooshare hello-fil i stället, kan han enkelt göra det genom att spara i sin organisation OneDrive-konto. När han har som han kopiera hello Dokumentlänk och dela den med sin kollegor. 

## <a name="threat-modeling-meetings"></a>Hot modellering möten

När Ricardo skickas hans hot modellen toohis kollega med OneDrive, Ashish hello tester, var underwhelmed. Visat sig som Ricardo och Cristina missade ganska många viktiga specialfall, som lätt kan äventyras. Hans angående är ett komplement toothreat modeller.

I detta scenario när Ashish tog över hello hotmodell han kallas för två hot modellering möten: ett möte toosynchronize om hello processen och gå igenom hello diagram och en andra möte för hot granskning och signering.

I hello första möte att Ashish 10 minuter går alla via hello SDL hot modellera processen. Han hämtas in hello hot modelldiagram och igång förklarar i detalj. En viktig komponent saknas har identifierats inom fem minuter.

Några minuter senare, Ashish och Ricardo gick in i en utökad beskrivning av hur hello webbservern har skapats. Det var inte hello bästa sättet för en möte tooproceed, men alla kommit överens om slutligen att identifiera hello diskrepans tidigt frånkopplades toosave dem tiden i hello framtiden.

I hello andra möte hello team har gått igenom hello hot beskrivs några sätt tooaddress dem och signerade av på hello hotmodell. De markerat hello dokumentet till källkontroll och fortsätter med utveckling.

## <a name="thinking-about-assets"></a>Om du tänker tillgångar

Vissa läsare som har hot modelleras eventuellt se att vi inte har diskuterats tillgångar alls. Vi har upptäckt att många programmerare tar lättare att förstå sina program än de förstår hello begreppet tillgångar och vilka tillgångar en angripare kan vara intresserad av.

Om du ska toothreat modellen ett hus, du kan börja med att tänka på att din familj, oersättliga foton och värdefulla illustrationer. Kanske kan du börja med att tänka igenom som kan logga in och hello aktuella säkerhetssystem. Eller så kan du starta genom att beakta hello fysiska funktioner, till exempel hello pool eller hello främre porch. Dessa är detsamma toothinking om tillgångar, angripare eller programvara design. Någon av dessa tre metoder fungerar.

hello metoden toothreat modellering vi visas här är betydligt enklare än vad Microsoft har gjort i hello tidigare. Påträffades att hello programvara Designmetoden fungerar bra för många grupper. Vi hoppas som innehåller din.

## <a name="next-steps"></a>Nästa steg

Skicka frågor, kommentarer och frågor tootmtextsupport@microsoft.com. **[Hämta](https://aka.ms/tmtpreview)**  hello hot Modeling verktyget tooget igång.
