---
title: "aaaMonitoring resurser i Operations Management Suite säkerhet och granska lösningen | Microsoft Docs"
description: "Det här dokumentet hjälper toouse OMS säkerhet och granska funktioner toomonitor dina resurser och identifiera säkerhetsproblem."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: d6752120-821f-4aa7-a049-25bf5a653b95
ms.service: operations-management-suite
ms.custom: oms-security
ms.topic: article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: 932b946ae1ffa3b979c02f419702d42d46abf7ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Övervaka resurser i Operations Management Suite säkerhet och granska lösning
Det här dokumentet hjälper dig att använda OMS säkerhet och granska funktioner toomonitor dina resurser och identifiera säkerhetsproblem.

## <a name="what-is-oms"></a>Vad är OMS?
Microsoft Operations Management Suite (OMS) är Microsofts molnbaserade IT-hanteringslösning som hjälper dig att hantera och skydda dina lokala och molnet infrastruktur. Mer information om OMS artikeln hello [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Övervaka resurser
När det är möjligt, så ska tooprevent säkerhetsincidenter inträffar på hello första plats. Det är dock möjligt tooprevent alla säkerhetsincidenter. När en säkerhetsincident sker behöver tooensure att dess konsekvenser minimeras.  Det finns tre viktiga rekommendationer som kan använda toominimize hello nummer och hello konsekvenserna av säkerhet:

* Regelbundet bedöma säkerhetsrisker i din miljö.
* Kontrollera regelbundet alla datorer och nätverk enheter tooensure som de har alla hello senaste korrigeringsprogram som är installerade.
* Kontrollera regelbundet alla loggar och mekanismer för loggning, inklusive händelseloggar för operativsystem, specifika programloggarna och intrångsidentifiering systemloggar för identifiering.

OMS säkerhet och granska lösningen möjliggör IT tooactively övervaka alla resurser som hjälper dig att minimera hello konsekvenserna av säkerhet. OMS säkerhet och granska har säkerhetsdomäner som kan användas för att övervaka resurser. hello säkerhetsdomäner ger snabb åtkomst tooa alternativ, för säkerhet övervakning hello följande domäner beskrivs mer detaljerat:

* kontroll av skadlig kod
* Kontroll av uppdateringar
* Identitet och åtkomst

> [!NOTE]
> en översikt över alla dessa alternativ, läsa [komma igång med Operations Management Suite säkerhet och granska lösningen](oms-security-getting-started.md).
> 
> 

### <a name="monitoring-system-protection"></a>Övervakning av Systemskydd
I en skydd på djupet metoden var skyddslager är viktigt för hello övergripande säkerhetstillståndet hos din tillgång. Datorer med identifierat hot och datorer med otillräckligt skydd visas i hello kontroll av skadlig kod panelen under säkerhetsdomäner. Du kan identifiera en plan tooapply skydd toohello servrar som behöver den med hjälp av hello information på hello kontroll av skadlig kod. tooaccess hello det här alternativet-Följ stegen nedan:

1. I hello **Microsoft Operations Management Suite** huvudsakliga instrumentpanelen klickar du på **säkerhet och granska** panelen.
   
    ![Säkerhet och granskning](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)
2. I hello **säkerhet och granska** instrumentpanelen, klickar du på **program mot skadlig kod Assessment** under **säkerhetsdomäner**. Hej **program mot skadlig kod Assessment** instrumentpanelen visas enligt nedan:

![kontroll av skadlig kod](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

Du kan använda hello **kontroll av skadlig kod** instrumentpanelen tooidentify hello följande säkerhetsproblem:

* **Aktiva hot**: datorer som har komprometterats och ha active hot i hello system.
* **Åtgärdad hot**: datorer som har drabbats men hello hot har reparerats.
* **Inaktuell signatur**: datorer som har skadlig kod skydd är aktiverat men hello signatur är för gammal.
* **Inget realtidsskydd**: datorer som inte har installerat program mot skadlig kod.

### <a name="monitoring-updates"></a>övervakning av uppdateringar
Använda hello de senaste säkerhetsuppdateringarna är en bra säkerhetsrutin bör tas upp i din strategi för uppdateringen. Microsoft Monitoring Agent-tjänsten (HealthService.exe) läser information om uppdateringar från övervakade datorer och sedan skickar uppdaterad information toohello OMS tjänsten i hello molnet för bearbetning. hello tjänsten Microsoft Monitoring Agent har konfigurerats som en automatisk tjänst och den bör köras alltid i hello måldatorn.

![övervakning av uppdateringar](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Logik är tillämpade toohello-data för uppdateringen och hello Molntjänsten innehåller hello-data. Om uppdateringar som saknas påträffas, visas de på hello **uppdateringar** instrumentpanelen. Du kan använda hello **uppdateringar** instrumentpanelen toowork med saknade uppdateringar och utveckla en plan tooapply dem toohello servrar där det behövs. Gör hello nedan tooaccess hello **uppdateringar** instrumentpanelen:

1. I hello **Microsoft Operations Management Suite** huvudsakliga instrumentpanelen klickar du på **säkerhet och granska** panelen.
2. I hello **säkerhet och granska** instrumentpanelen, klickar du på **uppdatering Assessment** under **säkerhetsdomäner**. hello Update instrumentpanelen visas enligt nedan:

![Utvärdering av uppdateringar](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

Du kan utföra en uppdatering assessment toounderstand hello aktuell status för dina datorer och adress hello viktigaste hot i den här instrumentpanelen. Med hjälp av hello **kritiska uppdateringar eller säkerhetsuppdateringar** sida vid sida, som IT-administratörer kommer att kunna tooaccess detaljerad information om hello-uppdateringar som saknas enligt nedan:

![sökresultat](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

Den här rapporten innehåller viktig information som kan vara används tooidentify hello typ av hot som det här systemet är sårbar för, och som innehåller hello Microsoft KB-artiklar som är associerade med hello säkerhetsuppdatering och hello MS Bulletin som innehåller mer information om hello säkerhetsproblem.

### <a name="monitoring-identity-and-access"></a>Övervakning av identiteter och åtkomst
Med användare som arbetar var som helst, med olika enheter och komma åt mängder med molnet och lokala appar, är det viktigt att skydda sina autentiseringsuppgifter. Attacker för stöld av autentiseringsuppgifter är de som ursprungligen angriparen åtkomst tooa reguljära användarens autentiseringsuppgifter tooaccess ett system inom hello nätverk. Många gånger angreppet inledande är bara ett sätt tooget åtkomst toohello nätverk, hello slutmålet är toodiscover privilegium konton. 

Angripare kvar i hello nätverk med gratis tooling tooextract autentiseringsuppgifter från hello-sessioner för andra konton. Beroende på hello systemkonfiguration, kan dessa autentiseringsuppgifter extraheras i hello form av hashvärden, biljetter och även textlösenord.  

> [!NOTE]
> datorer som är direkt exponerade toohello Internet visas många misslyckades försöker att försök toologin med alla slags välkända användarnamn (t.ex. administratör). I de flesta fall är det OK om hello välkända användarnamn inte används och hello lösenord är tillräckligt starkt.
> 
> 

Det är möjligt tooidentify dessa användare innan de angripa ett konto med privilegier. Du kan utnyttja **OMS säkerhets- och granska lösningen** toomonitor identitet och åtkomst. Gör hello nedan tooaccess hello **identitets- och** instrumentpanelen:

1. I hello **Microsoft Operations Management Suite** huvudinstrumentpanelen klickar du på säkerhet och granska panelen.
2. I hello **säkerhet och granska** instrumentpanelen, klickar du på **identitets- och** under **säkerhetsdomäner**. Hej **identitets- och** instrumentpanelen visas enligt nedan:

![identitet och åtkomst](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Som en del av din strategi för vanlig övervakning, måste du inkludera identitet övervakning. IT-administratören bör se ut om det finns ett specifikt användarnamn som har många försök. Detta kan indikera antingen angripare som införskaffade hello verkliga användarnamn och försök toobrute kraft eller ett automatiskt verktyg som använder hårdkodade lösenord som upphört att gälla.

Den här instrumentpanelen aktivera IT tooquickly identifiera potentiella hot relaterade tooidentity och åtkomst toocompany's resurser. Det är särskilt viktigt tooalso identifiera eventuella trender, till exempel i hello inloggningar över tid panelen visas över tidsperiod hur många gånger ett misslyckat inloggningsförsök har utförts. I det här fallet hello datorn **filserver** emot 35 inloggningsförsök. Du kan utforska mer information om den här datorn genom att klicka på den. 

![Mer information](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

hello rapporten genereras för den här datorn ger värdefull information om det här mönstret. Har upptäckt att hello **konto** kolumnen ger du hello användarkonto som har använt tootry tooaccess hello system, hello **TIMEGENERATED** kolumnen ger du hello tidsintervall i vilka hello försök gjordes och Hej **LOGONTYPENAME** kolumnen ger du hello plats där det här försöket har utförts. Om dessa försök utförts lokalt i hello system av ett program, hello **processen** kolumn skulle visar hello processnamn. Hello processens namn finns redan i scenarier där hello inloggningsförsöket kommer från ett program, och nu kan du utföra ytterligare undersökning i hello målsystemet.

## <a name="see-also"></a>Se även
I det här dokumentet du lärt dig hur toouse OMS säkerhet och granska lösningen toomonitor dina resurser. toolearn mer om OMS-säkerhet finns hello följande artiklar:

* [Översikt över Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Komma igång med Operations Management Suite säkerhet och granska lösningen](oms-security-getting-started.md)
* [Övervakning och svarar tooSecurity varningar i Operations Management Suite säkerhet och granska lösning](oms-security-responding-alerts.md)

