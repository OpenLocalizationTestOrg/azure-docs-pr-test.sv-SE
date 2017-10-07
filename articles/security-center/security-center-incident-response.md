---
title: aaaRespond toosecurity incidenter med Azure Security Center | Microsoft Docs
description: "Det här dokumentet förklarar hur toouse Azure Security Center för ett scenario för incidenter."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 8af12f1c-4dce-4212-8ac4-170d4313492d
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: aaf50c0c7e774d03d517c3fd11686dbae48dd29b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-security-center-for-an-incident-response"></a>Använda Azure Security Center vid incidenthantering
Många organisationer lär dig hur toorespond toosecurity incidenter efter drabbas av ett angrepp. tooreduce kostnader och skada, är det viktigt toohave en incidenter planera på plats innan en attack äger rum. Azure Security Center kan användas i olika faser av en incidenthantering.

## <a name="incident-response-planning"></a>Planering av incidenthantering
En effektiv plan beror på tre kärnfunktionerna: som kan tooprotect, upptäcka och åtgärda toothreats. Skydda handlar om att förhindra incidenter, identifieringen är om hur du identifierar hot tidigt och svaret är om att ta bort hello angripare och återställa system toomitigate hello påverkan av en överträdelse.

Den här artikeln använder etapper av incidenter för hello från hello [Microsoft Azure Security Response i hello molnet](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678) artikel, som visas i följande diagram hello:

![Livscykel för incidenthantering](./media/security-center-incident-response/security-center-incident-response-fig1.png)

Du kan använda Security Center under hello identifiera, utvärdering och diagnostisera steg. Här följer exempel på hur Security Center kan vara användbar vid hello tre inledande incidenter steg:

* **Identifiera**: granska hello första tecken på att en händelse undersökning.
  * Exempel: granska hello första kontroll att en säkerhetsvarning med hög prioritet har aktiverats i instrumentpanelen för hello Security Center.
* **Utvärdera**: utföra hello inledande assessment tooobtain mer information om hello misstänkt aktivitet.
  * Exempel: hämta mer information om hello Säkerhetsvarning.
* **Diagnostisera**: Utföra en teknisk undersökning och identifiera strategier för inneslutning, åtgärder och bugglösning.
  * Exempel: Följ hello steg beskrivs av Security Center i den särskilda säkerhetsaviseringen.

hello scenariot nedan visar hur tooleverage Security Center under hello identifiera, utvärdering och diagnostisera/svara led i en säkerhetsincident. I Security Center är en [säkerhetsincident](security-center-incident.md) en sammanställning av alla aviseringar för en resurs som överensstämmer med särskilda [händelsekedje](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/)mönster. Incidenter visas i hello [säkerhetsaviseringar](security-center-managing-and-responding-alerts.md) panelen och bladet. En incident visar hello lista över relaterade aviseringar, vilket gör att du tooobtain mer information om varje förekomst. Security Center innehåller också fristående säkerhetsaviseringar som kan vara används tootrack ned en misstänkt aktivitet.

## <a name="scenario"></a>Scenario
Contoso nyligen har migreras några av sina lokala resurser tooAzure, inklusive vissa baseras på virtuella datorer av branschspecifika arbetsbelastningar och SQL-databaser. För närvarande har Contosos team som ansvarar för datorsäkerheten, CSIRT (Computer Security Incident Response Team) svårt att undersöka säkerhetsproblem på grund av bristande integrering av säkerhetsfunktionerna i de aktuella incidenthanteringsverktygen. Den här brist på integrering introducerar ett problem under hello identifiera steget (för många falska positiva identifieringar) samt under hello utvärdering och diagnostisera steg. Som en del av migreringen, de valt tooopt i för Security Center toohelp dem som löser problemet.

Hej första fasen av migreringen slutförts när de publicerats så alla resurser som åtgärdas alla hello säkerhetsrekommendationer från Security Center. Contoso CSIRT är hello centrum för att hantera datorn säkerhetsincidenter. hello team består av en grupp personer med ansvar för att hantera en säkerhetsincident. hello gruppmedlemmar har tydligt definierat uppgifter tooensure områden med svar är återstår som upptäckts.

För hello syftet med det här scenariot vi toofocus på hello följande personer som ingår i Contoso CSIRT hello roller:

![Livscykel för incidenthantering](./media/security-center-incident-response/security-center-incident-response-fig2.png)

Johanna, som arbetar med säkerheten. Hennes ansvarsområden är följande:

* Övervaka och svara toosecurity hot runt hello klockan.
* Växande toohello molnansvarig eller säkerhetsanalytiker efter behov.

Sam är säkerhetsanalytiker och hans ansvar omfattar:

* Undersöka attacker.
* Åtgärda aviseringar.
* Arbeta med toodetermine för ägare av arbetsbelastning och tillämpa åtgärder.

Som du ser Johanna och Sam har olika ansvarsområdena och de måste fungera tillsammans tooshare Security Center information.

## <a name="recommended-solution"></a>Rekommenderad lösning
Eftersom Johanna och Sam har olika roller, kommer de att använda olika områden av Security Center tooobtain relevant information för dagliga aktiviteter. Johanna använder **Säkerhetsaviseringar** som en del av sin dagliga övervakning.

![Säkerhetsaviseringar](./media/security-center-incident-response/security-center-incident-response-fig3.png)

Johanna använder säkerhetsaviseringar under hello identifiera och bedöm steg. När Johanna har slutförts hello inledande kontrollen kan hon eskalera hello problemet tooSam om det krävs ytterligare undersökning. Nu använder Sam hello information som angavs av Security Center ibland tillsammans med andra datakällor toomove toohello diagnostisera steget.

## <a name="how-tooimplement-this-solution"></a>Hur tooimplement den här lösningen
toosee hur du använder Azure Security Center i ett scenario för incidenter, vi gör Johannas hello identifiera och bedöm stegvis, och sedan se Sam har toodiagnose hello problemet.

### <a name="detect-and-assess-incident-response-stages"></a>Identifiera och utvärdera stegen vid incidenthantering
Johanna inloggad toohello Azure-portalen och fungerar i hello Security Center-konsolen. Som en del av sin dagligen övervakningsaktiviteter, igång hon granska högprioriterade aviseringar genom att utföra hello följande steg:

1. Klicka på hello **säkerhetsaviseringar** panelen och åtkomst hello **säkerhetsaviseringar** bladet.
    ![Bladet Säkerhetsaviseringar](./media/security-center-incident-response/security-center-incident-response-fig4.png)

   > [!NOTE]
   > Hello syftet med det här scenariot är Jenny pågående tooperform en utvärdering på hello skadliga SQL aktivitet avisering som visas i föregående bild hello.
   >
   >
2. Klicka på hello **skadliga SQL-aktivitet** varning och granska hello angripna resurser i hello **skadliga SQL-aktivitet** bladet: ![Incident information](./media/security-center-incident-response/security-center-incident-response-fig5.png)

    I det här bladet Jenny kan anteckna om hello angripna resurser, hur många gånger angrepp har inträffat och när det har upptäckts.
3. Klicka på hello **angripna resursen** tooobtain mer information om det här angreppet.

När du har läst hello beskrivning finner Jenny att detta inte är ett falsklarm och att hon ska hantera det här fallet tooSam.

### <a name="diagnose-incident-response-stage"></a>Steget att diagnosticera incidenthantering
SAM tar emot hello fallet från Jenny och börjar granska hello steg som föreslås i Security Center.

![Livscykel för incidenthantering](./media/security-center-incident-response/security-center-incident-response-fig6.png)

### <a name="additional-resources"></a>Ytterligare resurser
hello incidenter team kan också dra nytta av hello [Security Center Power BI](security-center-powerbi.md) kapaciteten toosee olika typer av rapporter. Dessa rapporter kan hjälpa dem under ytterligare undersökning toovisualize, analysera och filtrera rekommendationer och säkerhetsaviseringar. För företag som använder sina säkerhetsinformation och hanteringslösningen för händelse (SIEM) under undersökningen hello, kan de också [integrera Security Center med sitt lösning](security-center-integrating-alerts-with-log-integration.md). Du kan också integrera Azure granskningsloggar och händelser för virtuell dator (VM)-säkerhet med hjälp av hello [Azure logganalys integration verktyget](https://blogs.msdn.microsoft.com/azuresecurity/2016/07/21/microsoft-azure-log-integration-preview/). tooinvestigate en attack, du kan använda den här informationen tillsammans med hello-information som innehåller Security Center.

## <a name="conclusion"></a>Slutsats
Montera ett team innan något inträffar är mycket viktigt tooyour organisation och påverkar positivt hur incidenter hanteras. Med hello rätt verktyg toomonitor resurser hjälper den här team tootake korrekt steg tooremediate en säkerhetsincident. Security Center [identifieringsfunktionerna](security-center-detection-capabilities.md) kan hjälpa IT tooquickly svara toosecurity incidenter och åtgärda säkerhetsproblem.
