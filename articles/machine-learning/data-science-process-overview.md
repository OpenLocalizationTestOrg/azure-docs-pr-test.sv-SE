---
title: "aaaAzure Team datavetenskap processöversikt | Microsoft Docs"
description: "Tillhandahåller en datavetenskap metoder toodeliver förutsägelseanalys lösningar och intelligent program."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: b1f677bb-eef5-4acb-9b3b-8a5819fb0e78
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: bradsev;
ms.openlocfilehash: 2ba03585a6f6f855faaa3b5c0c75149cad0a88bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="team-data-science-process-overview"></a>Översikt över datavetenskap processen team

hello Team Data vetenskap processen (TDSP) är en flexibel iterativ datavetenskap metoder toodeliver förutsägelseanalyslösningar och intelligent program effektivt. TDSP hjälper till att förbättra gruppsamarbete och utbildning. Den innehåller en destillation av hello metodtips och strukturer från Microsoft och andra i branschen hello som underlättar hello lyckad implementering av datavetenskap initiativ. hello målet är toohelp företag använda fullständigt hello fördelar analytics programmet.

Den här artikeln innehåller en översikt över TDSP och dess huvudsakliga komponenter. Vi ger en allmän beskrivning av hello processen som kan implementeras med en mängd olika verktyg. En mer detaljerad beskrivning av hello projektets aktiviteter och roller som används i hello livscykeln för hello processen erbjuder ytterligare länkade avsnitt. Information om hur tooimplement hello TDSP med en specifik uppsättning verktyg och Microsoft infrastruktur för att vi använder tooimplement hello TDSP i våra tillhandahålls också.

## <a name="key-components-of-hello-tdsp"></a>Viktiga komponenter av hello TDSP

TDSP består av hello följande huvudkomponenter:

- En **datavetenskap livscykel** definition
- En **standardiserade projektstruktur**
- **Infrastruktur och resurser** för datavetenskap projekt
- **Verktyg och hjälpmedel** för projektet körning


## <a name="data-science-lifecycle"></a>Datavetenskap livscykel

hello Team Data vetenskap processen (TDSP) ger en livscykel toostructure hello utveckling av dina datavetenskap projekt. hello livscykel beskrivs hello steg från start toofinish som projekt följer vanligtvis när de utförs.

Om du använder en annan datavetenskap livscykel, exempelvis [SKARPA DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) eller din organisations egna anpassade process, kan du fortfarande använda hello uppgiftsbaserade TDSP hello gäller de utvecklingen livscykler. På en hög nivå har dessa olika metoder mycket gemensamt. 

Den här livscykeln har utformats för datavetenskap projekt som levereras som en del av intelligent program. Dessa program distribuera machine learning eller artificiell intelligence modeller för förutsägelseanalys. Undersökande datavetenskap projekt eller ad hoc-analytics projekt kan också dra nytta av den här processen. Men i sådana fall vissa av stegen hello kanske inte behövs.    

Hej TDSP livscykel består av fem viktiga steg som utförs upprepade gånger:

* **Så här fungerar för företag**
* **Datainsamling och förstå**
* **Modeling**
* **Distribution**
* **Kundens godkännande**

Här är en bild av hello **Team datavetenskap Process livscykel**. 

![TDSP livscykel](./media/data-science-process-overview/tdsp-lifecycle.png) 

hello mål, uppgifter och dokumentation artefakter för varje steg i livscykeln hello i TDSP beskrivs i hello [Team datavetenskap Process livscykel](data-science-process-lifecycle.md) avsnittet. Dessa uppgifter och artefakter är kopplade till Projektroller:

- Lösningsarkitekt
- Projektledare
- Dataexpert
- Projektledare 

hello ger följande diagram en rutnätsvy hello uppgifter (i blått) och artefakter (i grönt) som är associerade med varje steg i livscykeln hello (på hello vågrät axel) för dessa roller (på hello lodrät axel). 

![TDSP-roller-och-uppgifter](./media/data-science-process-overview/tdsp-tasks-by-roles.png)

## <a name="standardized-project-structure"></a>Standardiserade projektstruktur

Med alla projekt som delar en katalogstruktur och använda mallar för projektdokument enkelt hello team medlemmar toofind information om deras projekt. All kod och dokument som lagras i en version av kontrollsystemet (VC: er) som Git eller TFS Subversion tooenable gruppsamarbete. Uppgifter och funktioner i en flexibel följer upp system som Jira projekt kan Rally, Visual Studio Team Services spåras, närmare spårning hello-koden för enskilda funktioner. Sådana spårning kan också team tooobtain bättre kostnad uppskattningar. TDSP rekommenderar att du skapar en separat databas för varje projekt på hello VC: er för versionshantering, informationssäkerhet och samarbete. hello standardiserade struktur för alla projekt som hjälper dig att bygga institutionella knowledge över hello organisationen.

Vi ger mallar för hello mappstrukturen och nödvändiga dokument i standardplatserna. Den här mappstrukturen organiserar hello-filer som innehåller koden för datagranskning och extrahering av funktionen och som registrerar modellen iterationer. Mallarna gör det enklare för team medlemmar toounderstand arbete som utförs av andra och tooadd tooteams för nya medlemmar. Det är enkelt tooview och uppdatera mallar i markdown-format. Använda mallar tooprovide checklistor med viktiga frågor för varje projekt tooinsure att hello problemet är väl definierade och att produkterna uppfyller hello kvalitet förväntades. Exempel:

- ett projekt auktoriserad toodocument hello affärsproblem och omfattning hello-projekt
- hello datastruktur rapporter toodocument och statistik för hello rådata
- modellen rapporter toodocument hello härledda funktioner
- modellen prestandamått, till exempel ROC kurvor eller mus


![TDSP kataloger](./media/data-science-process-overview/tdsp-dir-structure.png)

hello katalogstruktur kan klonas från [Github](https://github.com/Azure/Azure-TDSP-ProjectTemplate).

## <a name="infrastructure-and-resources-for-data-science-projects"></a>Infrastruktur och resurser för datavetenskap projekt

TDSP innehåller rekommendationer för att hantera delade analyser och lagringsinfrastruktur som:

- molnet filsystem för lagring av datauppsättningar, 
- databaser
- stordata (Hadoop eller Spark) kluster 
- Machine learning-tjänster. 

infrastruktur för hello analytics och lagring kan vara i hello molnet eller lokalt. Detta lagras rådata och bearbetade datauppsättningar. Den här infrastrukturen kan reproduceras analys. Det förhindrar också duplicering, vilket kan leda tooinconsistencies och onödiga infrastrukturkostnader. Verktygen finns angivna tooprovision hello delade resurser, följa upp dem och möjliggöra varje team medlem tooconnect toothose resurser på ett säkert sätt. Det är också en bra idé har projektmedlemmar skapar en konsekvent beräknings-miljö. Olika gruppmedlemmar kan replikera och validera experiment.

Här är ett exempel på ett team arbetar på flera projekt och dela olika moln analytics-infrastrukturkomponenter.

![TDSP-infrastruktur](./media/data-science-process-overview/tdsp-analytics-infra.png)


## <a name="tools-and-utilities-for-project-execution"></a>Verktyg och hjälpmedel för projektet körning

Introduktion till processer i de flesta organisationer är svårt. Verktyg som hjälp lägre hello barriärer tooand öka hello konsekvenskontroll av antagandet tooimplement hello datavetenskap processen och livscykel. TDSP ger en inledande uppsättning verktyg och skript toojump start införandet av TDSP i ett team. Det hjälper också till att automatisera vissa av hello vanliga aktiviteter i hello datavetenskap livscykel, till exempel datagranskning och baslinjen modellering. Det är en väldefinierad struktur för enskilda användare toocontribute delade verktyg och hjälpmedel i deras team delad kod databas. Dessa resurser kan sedan användas av andra projekt i hello team eller hello organisationen. TDSP planerar också tooenable hello avgifter för verktyg och hjälpmedel toohello hela gruppen. Hej TDSP verktyg kan klonas från [Github](https://github.com/Azure/Azure-TDSP-Utilities).


## <a name="next-steps"></a>Nästa steg

[: Team vetenskap roller och aktiviteter](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/roles-tasks.md) beskrivs hello personal roller och förknippade aktiviteter för en datavetenskap team som standardiserar på den här processen. 
