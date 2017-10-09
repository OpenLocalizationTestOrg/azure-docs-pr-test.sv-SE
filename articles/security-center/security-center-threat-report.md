---
title: aaaAzure Security Center hot intelligence rapporten | Microsoft Docs
description: "Det här dokumentet hjälper dig att toouse Azure Security Center hot Intelligent rapporter under en undersökning toofind mer information om en säkerhetsavisering."
services: security-center
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 5662e312-e8c2-4736-974e-576eeb333484
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: c888cfac1dd8b057616a6b8e6c6f6b67b552f2e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-threat-intelligence-report"></a>Hotinformationsrapporter i Azure Security Center
Det här dokumentet beskriver hur du kan lära dig mer om ett hot som genererat en säkerhetsvarning med hjälp av hotinformationsrapporter i Azure Security Center.

## <a name="what-is-a-threat-intelligence-report"></a>Vad är en hotinformationsrapport?
Hotidentifiering Security Center fungerar genom att övervaka säkerhetsinformation från Azure-resurser, hello nätverket och anslutna partnerlösningar. Den här informationen kan ofta korrelerar information från flera källor, tooidentify hot analyseras. Den här processen är en del av hello Security Center [identifieringsfunktionerna](security-center-detection-capabilities.md).

När Security Center identifierar ett hot utlöses en [säkerhetsvarning](security-center-managing-and-responding-alerts.md), som innehåller detaljerad information om en viss händelse, inklusive förslag på åtgärder. tooassist incidenter team undersöka och åtgärda hot, Security Center har en hot intelligence-rapport som innehåller information om hello hot som har identifierats, inklusive information som den:

* Angriparens identitet eller associationer (om den här informationen är tillgänglig)
* Angriparens mål
* Aktuella och historiska attackkampanjer (om den här informationen är tillgänglig)
* Angriparens metoder, verktyg och procedurer
* Associerade indikatorer för kompromettering, till exempel URL:er och filhashvärden
* Victimology som är hello branschen och geografiska förekomsten tooassist du för att fastställa om din Azure-resurser som är i fara
* Information om rekommenderade åtgärder

> [!NOTE]
> hello mängden information i någon viss rapport varierar; hello detaljnivå baseras på hello skadlig aktivitet och få information om användningsmönster.
>
>

Security Center har tre typer av hot rapporter som kan variera bl.a toohello attack. hello-rapporter som är tillgängliga är:

* **Aktivitetsgruppsrapport**: Tillhandahåller detaljerad information om angripare, deras mål och metoder.
* **Kampanjrapport**: Fokuserar på information om specifika attackkampanjer.
* **Hot sammanfattningsrapport över**: omfattar alla hello objekt i hello föregående två rapporter.

Den här typen av information är mycket användbar vid hello [incidentsvar](security-center-incident-response.md) processer, där det finns en pågående undersökning toounderstand hello källa för hello attack hello angripare motiveringen och vad toodo toomitigate detta problemet vidarebefordras.

## <a name="how-tooaccess-hello-threat-intelligence-report"></a>Hur tooaccess hello hot intelligence-rapporten?
Du kan granska aktuella aviseringar genom att titta på hello **säkerhetsaviseringar** panelen. Öppna hello Azure-portalen och gör hello nedan toosee mer information om varje avisering:

1. På instrumentpanelen för hello Security Center ser du hello **säkerhetsaviseringar** panelen.
2. Klicka på hello panelen tooopen hello **säkerhetsaviseringar** blad som innehåller mer information om hello aviseringar och klicka på i hello Säkerhetsvarning som du vill tooobtain mer information om.

    ![Säkerhetsaviseringar](./media/security-center-threat-report/security-center-threat-report-fig1.png)
3. I det här fallet hello **misstänkta processen körs** bladet visar hello information om hello avisering som visas i hello bilden nedan:

    ![Utförlig information om säkerhetsaviseringar](./media/security-center-threat-report/security-center-threat-report-fig2.png)
4. hello mängden information som är tillgängliga för varje Säkerhetsvarning varierar bl.a toohello typen av avisering. I hello **rapporter** fält som du har en toohello hot intelligence rapport. Klicka på länken så öppnas ett nytt webbläsarfönster med en PDF-fil.

   ![Val av lagringsutrymme](./media/security-center-threat-report/security-center-threat-report-fig3.png)

Du kan hämta hello PDF-filen för den här rapporten och Läs mer om hello säkerhet problem som har identifierats och vidta åtgärder utifrån hello informationen här.

## <a name="see-also"></a>Se även
I det här dokumentet har vi tittat på hur hotinformationsrapporterna i Azure Security Center kan hjälpa dig när du undersöker säkerhetsaviseringar. toolearn mer om Azure Security Center finns hello följande:

* [Vanliga frågor och svar om Azure Security Center](security-center-faq.md). Sök efter vanliga frågor om hur du använder hello-tjänsten.
* [Använda Azure Security Center vid incidenthantering](security-center-incident-response.md)
* [Identifieringsfunktioner i Azure Security Center](security-center-detection-capabilities.md)
* [Planerings- och bruksanvisning för Azure Security Center](security-center-planning-and-operations-guide.md). Lär dig hur tooplan och förstå hello design överväganden tooadopt Azure Security Center.
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md). Lär dig hur toomanage och svara toosecurity aviseringar.
* [Hantera säkerhetsincidenter i Azure Security Center](security-center-incident.md)
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/). Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.
