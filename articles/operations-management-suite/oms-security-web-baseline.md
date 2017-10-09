---
title: "aaaOperations Management Suite säkerhets- och granska lösningen Web baslinjen | Microsoft Docs"
description: "Det här dokumentet förklarar hur toouse OMS säkerhet och granska lösningen tooperform en web baslinjen bedömning av alla övervakade webbservrar för efterlevnad och säkerhet ändamål."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ROBOTS: NOINDEX
redirect_url: https://www.microsoft.com/cloud-platform/security-and-compliance
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: yurid
ms.openlocfilehash: 8aa87fa404ff97ab549dda3f9bebb75766055963
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="web-baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Utvärdering av säkerhetswebbaslinjen i säkerhets- och granskningslösningen i Operations Management Suite
Det här dokumentet hjälper dig att toouse [Operations Management Suite (OMS) säkerhets- och granska lösningen](operations-management-suite-overview.md) web baslinjen assessment funktioner tooaccess hello säkerhetsstatus för dina övervakade resurser.

## <a name="what-is-web-baseline-assessment"></a>Vad är utvärdering av webbaslinje?
För närvarande tillhandahåller säkerheten i OMS utvärdering av säkerhetsbaslinje för operativsystem. Söks igenom hello operativsysteminställningar servrar per dygn och ger en överblick över potentiellt känsliga inställningar. Läs [bedömning av baslinjen i Operations Management Suite säkerhet och granska lösningen](oms-security-baseline.md) för mer information om det här.

hello målet med hello web baslinjen assessment är toofind sårbara web server-inställningar. Hej tre primära källor för hello web grundläggande konfigurationer är: .NET, ASP.NET och IIS-konfigurationen.  Precis som hello fungerar system baslinjen assessment OMS säkerhet kommer tooscan din webbservrar varje 24hrs och ger en överblick över säkerhetsläget för dem.  I Internet Information Service (IIS), är konfigurationer mycket anpassningsbara, vilket gör att olika webbplats- och nivåer toobe-åsidosätts. hello skanner kontrollerar hello inställningar på varje nivå för program/på platsen i tillägg toohello Standardnivå rot. Detta hjälper dig att tooidentify potentiella säkerhetsproblem inställningar platser och reparera snabbt.


## <a name="web-security-baseline-assessment"></a>Utvärdering av säkerhetswebb-baslinje
För den här förhandsgranskningen kommer den här funktionen toobe som nås med hello OMS sökalternativ. Så hello nedan tooperform hello används fråga:

1. I hello **Microsoft Operations Management Suite** huvudsakliga instrumentpanelen klickar du på **säkerhet och granska** panelen.
2. I hello **säkerhet och granska** instrumentpanelen, klickar du på **loggen Sök** knappen.
3. hello första frågan som du kan använda är hello **Web baslinjen Assessment sammanfattning**. I hello **Begin Sök här** anger den här frågan: typen*= SecurityBaselineSummary BaselineType = web*. hello efter skärmen har ett exempel på utdata:

![Sammanfattning av utvärdering av webbaslinje](./media/oms-security-web-baseline/oms-security-web-baseline-fig1-new.png)

> [!NOTE]
> I den här fråga indikerar varje post en utvärderingssammanfattning på en enskild server.

När du är i hello **loggen Sök**, du kan skriva olika frågor tooobtain mer information om hello web baslinjen assessment. Dessutom toohello föregående fråga, du kan också använda hello följande i den här förhandsgranskningen.

**Utvärdering av webbaslinje**: Varje post motsvarar en utvärdering av en webb-baslinjeregel på en enskild server. Den omfattar alla data för hello regeln, plats, hello förväntat resultat och hello faktiska resultat.

**Fråga**: Type*=SecurityBaseline BaselineType=web*

![Utvärdering av webbaslinje](./media/oms-security-web-baseline/oms-security-web-baseline-fig2.png)

**Visa alla resultat för en specifik server**: frågan visar hur toosee resulterar i en specifik server.

**Fråga**: *Type=SecurityBaseline BaselineType=web Computer=BaselineTestVM1*

![Alla resultat](./media/oms-security-web-baseline/oms-security-web-baseline-fig3.png)

Du kan också använda dessa poster/frågor toocreate egna instrumentpaneler, rapporter och aviseringar. hello-skärmen nedan har en exempel UI-kontroll som du lägger till tooyour instrumentpanelen. Du kan lära dig hur toovisualize dina data med hjälp av OMS Vydesigner [här](https://blogs.technet.microsoft.com/msoms/2016/06/30/oms-view-designer-visualize-your-data-your-way/). hello-skärmen nedan är ett exempel på hur hello panelen ser ut när du har gjort den här anpassningen.

![Exempel-UI](./media/oms-security-web-baseline/oms-security-web-baseline-fig4.png)

> [!NOTE]
> Om du vill ha tooknow hello inställningar som är markerade för hello baslinje, kan du hämta [Excel-kalkylblad](https://gallery.technet.microsoft.com/OMS-Web-Baseline-1e811690) som innehåller dessa inställningar.

## <a name="see-also"></a>Se även
I det här dokumentet har du lärt dig om Utvärdering av säkerhetswebbaslinje i säkerhets- och granskningslösningen i OMS. toolearn mer om OMS-säkerhet finns hello följande artiklar:

* [Översikt över Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Övervakning och svarar tooSecurity varningar i Operations Management Suite säkerhet och granska lösning](oms-security-responding-alerts.md)
* [Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite](oms-security-monitoring-resources.md)

