---
title: "aaaWeb baslinjen Assessment i Operations Management Suite säkerhet och granska lösningen baslinjen | Microsoft Docs"
description: "Det här dokumentet förklarar hur toouse web bedömning av baslinjen i OMS-säkerhet och granska lösningen tooperform en baslinje bedömning av alla övervakade webbservrar för efterlevnad och säkerhet ändamål."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: yurid
ms.openlocfilehash: dafa9d3d93fae31748306b60ee40b285dd59c802
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Utvärdering av säkerhetswebbaslinjen i säkerhets- och granskningslösningen i Operations Management Suite
Det här dokumentet hjälper dig att använda OMS säkerhet och Audit web baslinjen assessment funktioner tooaccess hello säkerhetsstatus för dina övervakade resurser.

## <a name="what-is-web-baseline-assessment"></a>Vad är utvärdering av webbaslinjen?
För närvarande tillhandahåller säkerheten i OMS utvärdering av säkerhetsbaslinje för operativsystem. Den genomsöker hello OS-inställningarna för dina servrar per dygn och ger en överblick över potentiellt känsliga inställningar. Läs [bedömning av baslinjen i Operations Management Suite säkerhet och granska lösningen](https://docs.microsoft.com/azure/operations-management-suite/oms-security-baseline) för mer information om det här.

hello målet med hello Web baslinjen assessment är toofind sårbara web server-inställningar. Hej tre primära källor för hello web grundläggande konfigurationer är: .NET, ASP.NET och IIS-konfigurationen.  Precis som hello fungerar system baslinjen assessment OMS säkerhet kommer tooscan din webbservrar varje 24hrs och ger en överblick över säkerhetsläget för dem.  I Internet Information Service (IIS), är konfigurationer mycket anpassningsbara, vilket gör att olika webbplats- och nivåer toobe-åsidosätts. hello skanner kontrollerar hello inställningar på varje nivå för program/på platsen i tillägg toohello Standardnivå rot. Detta hjälper dig att tooidentify potentiellt känsliga inställningar och åtgärda snabbt tillsammans med våra rekommendationer för dessa inställningar.

>[!NOTE] 
>Du kan hämta hello vanliga Configuration identifierare och Baslinjeregler som används av OMS säkerhet i den här [sidan](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335?redir=0).


## <a name="web-security-baseline-assessment"></a>Utvärdering av säkerhetsbaslinjen för webben

För den här förhandsversionen kan hello-funktionen nås via hello OMS sökalternativ och hello OMS säkerhet och granska instrumentpanelen. Så hello nedan tooperform hello används fråga:

1. I hello **Microsoft Operations Management Suite** huvudinstrumentpanelen, klickar du på **säkerhet och granska** panelen.
2. I hello **säkerhet och granska** instrumentpanel, du kan se hello Web baslinjen perspektiv nästa toohello OS baslinjen perspektiv.
   
    ![Utvärdering av webbaslinjen i säkerhets- och granskningslösningen i OMS](./media/oms-security-web-baseline/oms-security-web-baseline-fig5.png)

3. hello vänster visar hello antalet webbservrar jämfört med toohello baslinje, hello genomsnittliga procentandelen av regler som skickats på alla servrar som hello utvärderas och hello listan över servrar som analyserades.
4. hello högra fönstret visar hello unika regler som misslyckades på grund av *allvarlighetsgrad*, och *RuleType*. Klicka på någon av reglerna för hello högra fönstret visas hello detaljer för regeln. Ett exempel visas i hello under bilden. hello-regel som anges under *inställning för regel*. Hej *AzId* fält som är en unik identifierare för varje regel som används av Microsoft för att spåra hello baslinjeregler. Dessutom toothat användare kan se hello *förväntat resultat* (Microsofts rekommenderat värde), och annan information om hello säkerhet effekten av hello regeln.
    
    ![Fråga](./media/oms-security-web-baseline/oms-security-web-baseline-fig6.png)

5. Du kan skapa egna frågor tooreview hello resultat. 

hello första frågan som du kan använda är hello **Web baslinjen Assessment sammanfattning**. I hello **Begin Sök här** anger den här frågan: *typ = SecurityBaselineSummary BaselineType = Web*. hello följande är ett exempel på utdata:

![Frågeresultat](./media/oms-security-web-baseline/oms-security-web-baseline-fig7.png)

>[!NOTE] 
>I den här fråga indikerar varje post en utvärderingssammanfattning på en enskild server.

När du är i hello **loggen Sök**, du kan skriva olika frågor tooobtain mer information om hello web baslinjen assessment. Dessutom toohello föregående fråga, du kan också använda hello följande i den här förhandsgranskningen:

**Utvärdering av webbaslinje**: Varje post motsvarar en utvärdering av en webb-baslinjeregel på en enskild server. Den omfattar alla data för en misslyckad regel hello *SitePath* på vilka hello regeln utvärderades, hello *förväntat resultat*, och hello *faktiska resultat*.

Fråga: *Type=SecurityBaseline BaselineType=Web AnalyzeResult=Failed*

![Frågeresultat 2](./media/oms-security-web-baseline/oms-security-web-baseline-fig8.png)

**Visa alla resultat för en specifik server**: frågan visar hur toosee resulterar i en viss server: frågan: *typ = SecurityBaseline BaselineType = Web datorn = BaselineTestVM1*

![Frågeresultat 3](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Du kan använda dessa poster/frågor toocreate egna instrumentpaneler, rapporter och aviseringar. Här är exempel UI kontroll som du lägger till tooyour instrumentpanelen. Du kan lära dig hur toovisualize dina data med hjälp av OMS Vydesigner [här](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/). hello-skärmen nedan är ett exempel på hur hello panelen ser ut när du har gjort den här anpassningen.

![instrumentpanel](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

## <a name="see-also"></a>Se även
I det här dokumentet har du lärt dig hur du utvärderar webbaslinjen i säkerhets- och granskningslösningen i OMS. toolearn mer om OMS-säkerhet finns hello följande artiklar:

* [Översikt över Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Övervakning och svarar tooSecurity varningar i Operations Management Suite säkerhet och granska lösning](oms-security-responding-alerts.md)
* [Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite](oms-security-monitoring-resources.md)

