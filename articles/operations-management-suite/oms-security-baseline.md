---
title: "aaaOperations Management Suite säkerhets- och granska lösningen baslinjen | Microsoft Docs"
description: "Det här dokumentet förklarar hur toouse OMS säkerhet och granska lösningen tooperform en baslinje bedömning av alla övervakade datorer för efterlevnad och säkerhet ändamål."
services: operations-management-suite
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: 
ms.assetid: 17837c8b-3e79-47c0-9b83-a51c6ca44ca6
ms.service: operations-management-suite
ms.custom: oms-security
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: yurid
ms.openlocfilehash: ea52408cb9d2598728fe3826a946067e1c99318f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="baseline-assessment-in-operations-management-suite-security-and-audit-solution"></a>Utvärdering av säkerhetsbaslinjen i säkerhets- och granskningslösningen i Operations Management Suite
Det här dokumentet hjälper dig att toouse [Operations Management Suite (OMS) säkerhets- och granska lösningen](operations-management-suite-overview.md) baslinjen assessment funktioner tooaccess hello säkerhetsstatus för dina övervakade resurser.

## <a name="what-is-baseline-assessment"></a>Vad är en utvärdering av säkerhetsbaslinjen?
Microsoft definierar, i samarbete med branschspecifika och offentliga organisationer i hela världen, en Windows-konfiguration för mycket säkra serverinstallationer. Den här konfigurationen består av en uppsättning registernycklar, granskningsprincipinställningar och säkerhetsprincipinställningar tillsammans med Microsofts rekommenderade värden för dessa inställningar. Den här uppsättningen regler kallas för en säkerhetsbaslinje. Med funktionen Utvärdering av säkerhetsbaslinje i säkerhets- och granskningslösningen i OMS kan du smidigt söka igenom alla dina datorer och kontrollera att efterlevnaden upprätthålls. 

Det finns tre typer av regler:

* **Registerregler**: Kontrollerar att registernycklarna är korrekt angivna.
* **Granskningsprincipregler**: Regler relaterade till granskningsprinciper.
* **Säkerhet principregler**: regler angående hello användarens behörigheter på hello-datorn.

> [!NOTE]
> Läs [Använd OMS säkerhet tooassess hello säkerhet Konfigurationsbaslinje](https://blogs.technet.microsoft.com/msoms/2016/08/12/use-oms-security-to-assess-the-security-configuration-baseline/) för en kort översikt över den här funktionen.
> 
> 

## <a name="security-baseline-assessment"></a>Utvärdering av säkerhetsbaslinje
Du kan granska baslinje i assessment för alla datorer som övervakas av OMS säkerhet och granska hjälp hello instrumentpanelen i security.  Kör hello följande steg tooaccess hello säkerhet baslinjen assessment instrumentpanelen:

1. I hello **Microsoft Operations Management Suite** huvudinstrumentpanelen, klickar du på **säkerhet och granska** panelen.
2. I hello **säkerhet och granska** instrumentpanelen, klickar du på **baslinjen Assessment** under **säkerhetsdomäner**. Hej **utvärdering av säkerheten baslinjen** instrumentpanelen visas i följande bild hello:
   
    ![Utvärdering av säkerhetsbaslinjen i säkerhets- och granskningslösningen i OMS](./media/oms-security-baseline/oms-security-baseline-fig1.png)

Den här instrumentpanelen är uppdelat i tre huvudområden:

* **Datorer jämfört med toobaseline**: det här avsnittet ger en översikt över hello antal datorer som användes och hello procentandelen som har klarat hello assessment. Det ger även hello översta 10-datorer och hello procentandel resultat för hello assessment.
* **Krävs regler Status**: det här avsnittet finns hello avsiktshantering toobring medvetenhet om hello misslyckades regler efter allvarlighetsgrad och kunde inte regler av typen. Söker toohello första diagrammet kan du snabbt identifiera om de flesta hello misslyckades regler är kritisk, eller inte. Det ger även en lista över hello översta 10 misslyckade regler och deras allvarlighetsgrad. hello andra diagrammet visar hello typ av regel som misslyckades under hello assessment. 
* **Datorer som saknar baslinjen assessment**: det här avsnittet listas hello-datorer som inte komma åt på grund av inkompatibilitet mellan toooperating-system eller fel. 

### <a name="accessing-computers-compared-toobaseline"></a>Åtkomst till datorer jämfört med toobaseline
Helst vara alla dina datorer är kompatibel med hello säkerhet baslinjen assessment. I vissa fall kan det dock hända att så inte är fallet. Som en del av hanteringen för hello säkerhet är det viktigt tooinclude granska hello-datorer som inte kunde toopass alla security assessment tester. Ett snabbt sätt toovisualize som är genom att välja alternativet hello **datorer nås** finns i hello **datorer jämfört med toobaseline** avsnitt. Du bör se hello loggen Sök resultatet visar hello lista med datorer som visas i följande skärmbild hello:

![Resultat från Dator som öppnats](./media/oms-security-baseline/oms-security-baseline-fig2.png)

hello sökresultat visas i tabellformat där hello första kolumnen har hello datornamn och hello andra färgen har hello antalet regler som har misslyckats. tooretrieve hello information om hello typ av regel som har misslyckats, klicka i hello antalet misslyckade regler förutom hello datornamn. Du bör se en resultat liknande toohello som visas i följande bild hello:

![Resultatdetaljer från Dator som öppnats](./media/oms-security-baseline/oms-security-baseline-fig3.png)

I det här sökresultatet har hello Totalt används regler, hello antal kritiska regler som har misslyckats, hello Varningsregler och hello information misslyckades regler.

### <a name="accessing-required-rules-status"></a>Kontrollera statusen för obligatoriska regler
När du har fått hello information om hello procentandel antal datorer som har klarat hello assessment kanske tooobtain mer information om vilka regler som misslyckas bl.a toohello allvarlighetsgrad. Den här visualiseringen hjälper tooprioritize vilka datorer som ska åtgärdas första tooensure de är kompatibla i hello nästa assessment. Hovra över hello viktig del av hello diagram i hello **misslyckades regler efter allvarlighetsgrad** panelen under **krävs regler status** och klicka på den. Du bör se en resultat liknande toohello följande skärm:

![Misslyckade regler efter allvarlighetsgrad](./media/oms-security-baseline/oms-security-baseline-fig4.png) 

I resultatet för den här loggen visas hello typ av baslinjen regeln som misslyckades, hello beskrivning av den här regeln och hello Common Configuration Enumeration (CCE)-ID för denna regel. Dessa attribut måste vara tillräckligt med tooperform en korrigerande åtgärd toofix problemet i hello måldatorn.

> [!NOTE]
> Mer information om CCE åt hello [National Vulnerability Database](https://nvd.nist.gov/cce/index.cfm).
> 
> 

### <a name="accessing-computers-missing-baseline-assessment"></a>Kontrollera datorer utan utvärdering av säkerhetsbaslinje
OMS stöder hello domänmedlem och domänkontrollanten baslinjen profil i Windows Server 2008 R2 upp tooWindows Server 2012 R2. Baslinjen för Windows Server 2016 är inte klar än och läggs till så fort den publiceras. Alla andra operativsystem som genomsöks via OMS säkerhet och granska baslinjen bedömning visas under hello **datorer som saknar baslinjen assessment** avsnitt.

## <a name="see-also"></a>Se även
I det här dokumentet har du lärt dig om Utvärdering av säkerhetsbaslinje i säkerhets- och granskningslösningen i OMS. toolearn mer om OMS-säkerhet finns hello följande artiklar:

* [Översikt över Operations Management Suite (OMS)](operations-management-suite-overview.md)
* [Övervakning och svarar tooSecurity varningar i Operations Management Suite säkerhet och granska lösning](oms-security-responding-alerts.md)
* [Övervaka resurser i säkerhets- och granskningslösningen i Operations Management Suite](oms-security-monitoring-resources.md)

