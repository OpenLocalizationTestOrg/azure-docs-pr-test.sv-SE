---
title: "Azure ML-arbetsstationen viktig information för sprint 3 januari 2018"
description: "Det här dokumentet beskriver uppdateringar för sprint 3-versionen av Azure ML"
services: machine-learning
author: raymondlaghaeian
ms.author: raymondl
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 01/22/2018
ms.openlocfilehash: f75fcec3b722563949b6553f17c4f3db3e223675
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/25/2018
---
# <a name="sprint-3---january-2018"></a>Sprint 3 – januari 2018 

#### <a name="version-number-01171218263"></a>Versionsnummer: 0.1.1712.18263

>Här är hur du kan [hittar du versionsnumret](https://docs.microsoft.com/en-us/azure/machine-learning/preview/known-issues-and-troubleshooting-guide).

Välkommen till den fjärde uppdateringen Azure Machine Learning-arbetsstationen. Följande är uppdateringar och förbättringar i den här sprint. Många av dessa uppdateringar görs direkt följd av användarfeedback. 

## <a name="notable-new-features-and-changes"></a>Viktiga nya funktioner och ändringar
- Uppdateringar till stacken autentisering tvingar val för inloggning och kontot vid start

## <a name="detailed-updates"></a>Detaljerad uppdateringar
Nedan följer en lista över detaljerade uppdateringar under varje komponent i Azure Machine Learning i den här sprint.

### <a name="workbench"></a>Workbench
- Möjlighet att installera eller avinstallera appen från Lägg till/ta bort program
- Uppdateringar till stacken autentisering tvingar val för inloggning och kontot vid start
- Förbättrad upplevelse för enkel inloggning (SSO) i Windows
- Användare som hör till flera klienter med andra autentiseringsuppgifter ska nu kunna logga in på arbetsstationen

#### <a name="ui"></a>UI
- Allmänna förbättringar och korrigeringarna

### <a name="notebooks"></a>Bärbara datorer
- Allmänna förbättringar och korrigeringarna

### <a name="data-preparation"></a>Förberedelse av data 
- Förbättrad automatisk-förslag vid utförandet av exempel omvandlingar
- Förbättrad algoritm för mönster frekvens inspector
- Möjlighet att skicka exempeldata och feedback när du utför med hjälp av exempel transformationer ![bild av skicka feedbacklänk på härledd kolumn transformeringen](media/release-notes-sprint-3/SendFeedbackFromDeriveColumn.png)
- Spark Runtime-förbättringar
- Scala har ersatt Pyspark
- Fast oförmåga att stänga Data är inte tillämpligt för tid serien Inspector 
- Fast låser sig tiden för körningen Data Prep för HDI

### <a name="model-management-cli-updates"></a>Uppdaterar modellen Management CLI 
  - Ägarskapet för prenumerationen är inte längre krävs för att etablera resurser. Deltagare åtkomst till resursgruppen räcker att ställa in distributionsmiljön.
  - Aktiverade lokala miljön kostnadsfritt konfigurera prenumerationer 
