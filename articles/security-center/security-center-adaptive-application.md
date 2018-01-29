---
title: Anpassningsbara programkontroller i Azure Security Center | Microsoft Docs
description: "Det här dokumentet hjälper dig att använda anpassningsbar programkontroll i Azure Security Center för att vitlista godkända program som körs i virtuella Azure-datorer."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/02/2018
ms.author: yurid
ms.openlocfilehash: b54dfc454cfcdefb56bfda9b242412e0f213b50b
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/02/2018
---
# <a name="adaptive-application-controls-in-azure-security-center-preview"></a>Anpassningsbara programkontroller i Azure Security Center (förhandsversion)
Lär dig att konfigurera programkontroll i Azure Security Center med den här genomgången.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Vad är anpassningsbara programkontroller i Security Center?
Med hjälp av anpassningsbara programkontroller kan du kontrollera vilka program som kan köras på din virtuella dator i Azure, vilket bland annat hjälper dig skydda dina virtuella datorer mot skadlig kod. Security Center använder Machine Learning för att analysera processerna som körs i den virtuella datorn och hjälper dig att tillämpa vitlisteregler med den här intelligensen. Den här funktionen förenklar processen avsevärt för att konfigurera och underhålla programs vitlistor, så att du kan:

- Blockera eller varna vid försök att köra skadliga program, inklusive de som annars kan missas av program mot skadlig kod
- Uppfylla organisationens säkerhetsprincip som anger att bara licensierade program får användas.
- Undvika oönskad programvara som ska användas i din miljö.
- Undvik att gamla program som inte stöds kan köras.
- Förhindra vissa verktyg som inte tillåts i din organisation.
- Aktivera IT för att kontrollera åtkomsten till känsliga data via appanvändning.

## <a name="how-to-enable-adaptive-application-controls"></a>Hur fungerar anpassningsbara programkontroller?
Anpassningsbara programkontroller hjälper dig att definiera en uppsättning program som ska tillåtas att köras på konfigurerade resursgrupper. Den här funktionen är endast tillgänglig för Windows-datorer (alla versioner, klassisk eller Azure Resource Manager). Följ stegen nedan för att konfigurera listan över tillåtna program i Security Center:

1.  Öppna instrumentpanelen för **Security Center** och klicka på **Anpassningsbara programkontroller** under avsnittet **Avancerat molnskydd** i det vänstra navigeringsfönstret.

    ![Skydd](./media/security-center-adaptive-application/security-center-adaptive-application-fig1-new.png)

2. Sidan **Anpassningsbara programkontroller** visas.

    ![kontroller](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

4. I avsnittet Resursgrupper finns tre flikar:
    * **Rekommenderas**:  lista över resursgrupper som programkontroll rekommenderas för. Security Center använder Machine Learning för att identifiera virtuella datorer som är bra kandidater för programkontroll baserat på om de virtuella datorerna konsekvent kör samma program.
    * **Konfigurerad**: lista över resursgrupper som innehåller de virtuella datorer som konfigurerades med programkontroll. 
    * **Ingen rekommendation**: lista över resursgrupper som innehåller virtuella datorer utan rekommendationer för programkontroll. Till exempel virtuella datorer där program alltid byts ut och inte har uppnått ett stabilt tillstånd.

### <a name="configure-a-new-application-control-policy"></a>Konfigurera en ny princip för programkontroll
Klicka på fliken **Rekommenderas** för att visa en lista över resursgrupper med rekommendationer för programkontroll:

![Rekommenderas](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

Listan innehåller:
- **NAMN**: Namnet på prenumerationen och resursgruppen
- **Virtuella datorer**: antalet virtuella datorer i resursgruppen
- **TILLSTÅND**: Rekommendationernas tillstånd, i de flesta fall öppet
- **ALLVARLIGHETSGRAD**: Rekommendationernas allvarlighetsgrad

Välj en resursgrupp för att öppna alternativet för att **skapa kontrollregler för program**:

![Kontrollregler för program](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

I **Välj virtuella datorer** läser du listan med rekommenderade virtuella datorer och avmarkerar dem du inte vill använda programkontroll för. I **Välj processer för reglerna för lista över tillåtna** läser du listan med rekommenderade virtuella datorer och avmarkerar dem du inte vill använda. Listan innehåller:

- **NAMN**: den fullständiga programsökvägen
- **PROCESSER**: Hur många program som finns på varje sökväg
- **GEMENSAM**: "Ja" betyder att dessa processer har körts på de flesta virtuella datorerna i den här resursgruppen.
- **EXPLOATERBAR**: En varningsikon anger om programmen skulle kunna användas av en angripare för att kringgå listan över tillåtna program. Vi rekommenderar att du granskar programmen innan du godkänner dem. 

När du är klar med dina val klickar du på knappen **Skapa**. Som standard aktiverar Security Center alltid programkontroll i läget *Granska*. När du har validerat att listan inte har haft någon negativ inverkan på din arbetsbelastning kan du ändra till läget *Tvinga*.

Security Center förlitar sig på minst två veckors data för att skapa en baslinje och fylla i unika rekommendationer för varje grupp med virtuella datorer. Nya Security Center-kunder på standardnivån bör vara beredda på att deras grupp av virtuella datorer först visas på fliken *Ingen rekommendation*.

> [!NOTE]
> Som ett säkerhetsregelverk försöker Security Center alltid att skapa en utgivarregel för programmen som ska vitlistas, och endast om ett program inte har någon utgivarinformation (alltså inte signerad) skapas en sökvägsregel för den specifika EXE-filens fullständiga sökväg.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Redigera och övervaka en grupp som har konfigurerats med programkontroll

Om du vill redigera och övervaka en grupp som har konfigurerats med programkontroll klickar du på **KONFIGURERAD** under **Resursgrupper**:

![Resursgrupper](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

Listan innehåller:

- **NAMN**: Namnet på prenumerationen och resursgruppen
- **Virtuella datorer**: antalet virtuella datorer i resursgruppen
- **LÄGE**: I granskningsläget loggas försök att köra program som inte tillåts, och blockeringen hindrar dem från att köras
- **ALLVARLIGHETSGRAD**: Rekommendationernas allvarlighetsgrad

Välj en resursgrupp att göra ändringar i på sidan för att **redigera principer för programkontroll**.

![Skydd](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

Under **Skyddsläge** kan du välja mellan följande alternativ:
- **Granska**: i det här läget tvingar inte programkontrollösningen reglerna, utan granskar endast aktiviteten för de skyddade virtuella datorerna. Det här rekommenderas för scenarier där du först vill observera det övergripande beteendet innan en app blockeras mot att köras i den virtuella måldatorn.
- **Tvinga**: i det här läget tvingar programkontrollösningen reglerna, och ser till att program som inte är tillåtna blockeras. 

Som vi nämnde tidigare blir alltid en ny princip för programkontroll som standard konfigurerad i läget *Granska*. Under **Policy extension** (Principtillägg) kan du lägga till egna programsökvägar du vill lägga till på vitlistan. När du lägger till sökvägarna skapar Security Center lämpliga regler för programmen utöver de regler som redan finns. I avsnittet **Issues** (Ärenden) listas eventuella aktuella överträdelser.

![Problem](./media/security-center-adaptive-application/security-center-adaptive-application-fig7.png)

Listan innehåller:

- **ÄRENDEN**: eventuella överträdelser som har loggats, vilket kan vara följande:
    - **ViolationsBlocked**: när lösningen är aktiverad i läget Tvinga och ett program som inte tillåts försöker köras.
    - **ViolationsAudited**: när lösningen är aktiverad i läget Granska och ett program som inte tillåts körs.
    - **RulesViolatedManually**: när en användare har försökt att konfigurera regler manuellt på de virtuella datorerna och inte via ASC-hanteringsportalen.
- **ANTAL VIRTUELLA DATORER**: antalet virtuella datorer med den här ärendetypen.

Om du klickar på var och en av dessa rader omdirigeras du till sidan med [Azure-aktivitetsloggen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) där du kan se information om alla virtuella datorer med den här typen av överträdelser. Om du klickar på de tre punkterna i slutet av varje rad kan du ta bort den specifika posten. I avsnittet **Konfigurerade virtuella datorer** visas de virtuella datorer som reglerna gäller. 

![Konfigurerade virtuella datorer](./media/security-center-adaptive-application/security-center-adaptive-application-fig8.png)

**Utgivare av regler för lista över tillåtna** anger programmen för vilka en utgivarregel skapades baserat på den certifikatinformation som hittades för varje program. Läs [Understanding Publisher Rules in Applocker](https://docs.microsoft.com/windows/device-security/applocker/understanding-the-publisher-rule-condition-in-applocker) (Förstå publiceringsregler i Applocker) för att få mer information.

![Regler för lista över tillåtna](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

Om du klickar på de tre punkterna i slutet av varje rad kan du ta bort den specifika regeln. I **Sökväg för regler för lista över tillåtna** anges hela programsökvägen (inklusive den körbara filen) för programmen som inte är signerade med ett digitalt certifikat, men som fortfarande finns i reglerna för lista över tillåtna. 

> [!NOTE]
> Som standard försöker Security Center som ett säkerhetsregelverk alltid att skapa en utgivarregel för EXE-filerna som ska vitlistas, och endast om en EXE-fil inte har någon utgivarinformation (alltså inte signerad) skapas en sökvägsregel för den specifika EXE-filens fullständiga sökväg.

![Sökväg för regler för lista över tillåtna](./media/security-center-adaptive-application/security-center-adaptive-application-fig10.png)

Listan innehåller:
- **NAMN**: fullständig korrigering för den körbara filen
- **EXPLOATERBAR**: sant anger att program skulle kunna användas av en angripare för att kringgå listan över tillåtna program.  

Om du klickar på de tre punkterna i slutet av varje rad kan du ta bort den specifika regeln. När du har gjort ändringarna kan du klicka på knappen **Spara**, eller om du bestämmer dig för att inte tillämpa ändringarna klickar du på **Ignorera**.

### <a name="not-recommended-list"></a>Listan Rekommenderas inte

Security Center rekommenderar enbart Lägga till program i listan över tillåtna för virtuella datorer som kör en stabil uppsättning program. Rekommendationer skapas inte om program på den associerade virtuella datorn ändras hela tiden. 

![Rekommendation](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

Listan innehåller:
- **NAMN**: Namnet på prenumerationen och resursgruppen.
- **Virtuella datorer**: antalet virtuella datorer i resursgruppen.

## <a name="next-steps"></a>Nästa steg
I det här dokumentet har du lärt dig att använda anpassningsbar programkontroll i Azure Security Center för att vitlista godkända program som körs i virtuella Azure-datorer. I följande avsnitt kan du lära dig mer om Azure Security Center:

* [Hantera och åtgärda säkerhetsaviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Lär dig hur du hanterar aviseringar och åtgärdar säkerhetsincidenter i Security Center.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md). Lär dig att övervaka hälsotillståndet för dina Azure-resurser.
* [Förstå säkerhetsaviseringar i Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Läs mer om de olika typerna av säkerhetsaviseringar.
* [Felsökningsguide för Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Lär dig hur du felsöker vanliga problem i Security Center. 
* [Vanliga frågor och svar om Azure Security Center](security-center-faq.md). Här finns vanliga frågor om att använda tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/). Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.

