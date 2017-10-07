---
title: aaaAzure Security Center Troubleshooting Guide | Microsoft Docs
description: "Det här dokumentet hjälper tootroubleshoot problem i Azure Security Center."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 44462de6-2cc5-4672-b1d3-dbb4749a28cd
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: yurid
ms.openlocfilehash: 78b3c49eb66fe3a4f80efbba3a47a87b039c07ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-troubleshooting-guide"></a>Felsökningsguide för Azure Security Center
Den här guiden är för (IT) experter, informationssäkerhetsanalytiker och molnet administratörer vars organisationer som använder Azure Security Center och måste tootroubleshoot Security Center relaterade problem.

>[!NOTE] 
>Från och med tidig juni 2017 använder Security Center hello Microsoft Monitoring Agent toocollect och lagra data. Se [Azure Security Center-plattformen migrering](security-center-platform-migration.md) toolearn mer. hello informationen i den här artikeln representerar Security Center-funktionalitet efter övergången toohello Microsoft Monitoring Agent.
>

## <a name="troubleshooting-guide"></a>Felsökningsguide
Den här guiden förklarar hur problem som rör tootroubleshoot Security Center. De flesta hello felsökning i Security Center sker genom att studera hello [granskningsloggen](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) poster för hello misslyckades komponent. Via granskningsloggarna kan du fastställa:

* Vilka åtgärder som har vidtagits
* Vem som initierat hello åtgärden
* När hello åtgärden uppstod
* hello status för hello åtgärd
* hello värdena för andra egenskaper som kan hjälpa dig undersöka hello åtgärden

hello granskningsloggen innehåller alla skrivåtgärder (PUT, POST, ta bort) utförs på dina resurser, men inte omfattar läsåtgärder (GET).

## <a name="microsoft-monitoring-agent"></a>Microsoft Monitoring Agent
Security Center använder hello Microsoft Monitoring Agent – detta är hello samma agent används av hello Operations Management Suite och Log Analytics-tjänsten – toocollect säkerhetsdata från din virtuella Azure-datorer. När datainsamling har aktiverats och hello agenten installeras på rätt sätt i hello måldatorn ska hello processen nedan körning:

* HealthService.exe

Om du öppnar hello-konsolen (services.msc) visas också hello Microsoft Monitoring Agent-tjänsten körs som visas nedan:

![Tjänster](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig5.png)

toosee vilken version av hello-agenten måste du öppna **Aktivitetshanteraren**, i hello **processer** fliken hitta hello **Microsoft Monitoring Agent-tjänsten**, högerklicka på den och Klicka på **egenskaper**. I hello **information** fliken, ut hello filversion enligt nedan:

![Fil](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig6.png)
   

## <a name="microsoft-monitoring-agent-installation-scenarios"></a>Scenarier för installation av Microsoft Monitoring Agent
Det finns två installationsscenarier som kan ge olika resultat när du installerar hello Microsoft Monitoring Agent på datorn. hello stöds scenarier är:

* **Agent som installeras automatiskt av Security Center**: i det här scenariot kommer du att kan tooview hello aviseringar i platser, Security Center och Sök i loggfilen. Du får e-postaviseringar toohello e-postadress som konfigurerats i hello säkerhetsprincipen för hello prenumeration hello resurs tillhör.
.
* **Agenten installeras manuellt på en virtuell dator finns i Azure**: i det här scenariot om du använder agenter hämtas och installeras manuellt före tooFebruary 2017 kan du kan tooview hello aviseringar i hello Security Center portal endast om du filtrerar på hello prenumerationen hello arbetsytan tillhör. Om du filtret på hello prenumeration hello resurs tillhör, du inte kan toosee några aviseringar. Du får e-meddelanden toohello e-postadress som konfigurerats i hello säkerhetsprincipen för hello prenumeration hello arbetsytan tillhör.

>[!NOTE]
> tooavoid hello beteende förklaras i hello andra, kontrollera att du hämtar hello senaste versionen av hello agent.
> 

## <a name="troubleshooting-monitoring-agent-network-requirements"></a>Felsöka nätverkskrav för övervakningsagenten
För agenter tooconnect tooand registrera med Security Center, måste de ha åtkomst toonetwork resurser, inklusive hello portnummer och URL: er för domänen.

- För proxy-servrar behöver du tooensure som hello lämpliga proxyserver resurser konfigureras i agentinställningarna för. Den här artikeln för mer information om [hur toochange hello proxyinställningar](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-windows-agents#configure-proxy-settings).
- För brandväggar som begränsar åtkomst toohello Internet, behöver du tooconfigure din brandvägg toopermit åtkomst tooOMS. Ingen åtgärd krävs i agentinställningarna.

hello följande tabell visar resurser som krävs för kommunikation.

| Agentresurs | Portar | Kringgå HTTPS-kontroll |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Ja |
| *.oms.opinsights.azure.com | 443 | Ja |
| *.blob.core.windows.net | 443 | Ja |
| *.azure-automation.net | 443 | Ja |

Om du får problem med onboarding med hello agent, se till att tooread hello artikel [hur tootroubleshoot Operations Management Suite onboarding utfärdar](https://support.microsoft.com/en-us/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues).


## <a name="troubleshooting-endpoint-protection-not-working-properly"></a>Felsökning om Endpoint Protection inte fungerar korrekt

Hej gästagenten är hello överordnade processen allt hello [Microsoft Antimalware](../security/azure-security-antimalware.md) har tillägget. När hello gäst-agenten misslyckas, misslyckas även hello Microsoft Antimalware som körs som en underordnad process för hello gästagenten.  I scenarier som som är rekommenderade tooverify hello följande alternativ:

- Om hello målet VM är en anpassad avbildning och hello skapare av hello VM aldrig installerat gästagenten.
- Om hello målet är en Linux-VM i stället för en Windows-VM sedan installera hello Windows-versionen av hello-tillägget för program mot skadlig kod på en Linux-VM misslyckas. Hej gästagenten för Linux har specifika krav när det gäller OS-versionen och nödvändiga paketen och dessa krav inte uppfylls hello VM-agenten fungerar inte om det antingen. 
- Om hello VM har skapats med en äldre version av gästagenten. Om det var ska du tänka på att vissa gamla agenter inte kunde uppdateras automatiskt själva toohello nyare version och detta kan leda toothis problem. Använd alltid hello senaste versionen av gästagenten om hur du skapar dina egna avbildningar.
- Vissa program från tredje part administration kan inaktivera hello gästagenten eller blockera åtkomst toocertain sökvägar. Om du har från tredje part installerat på den virtuella datorn, kontrollera att agentens hello är på hello undantagslistan.
- Vissa inställningar för brandväggen eller en Nätverkssäkerhetsgrupp (NSG) kan blockera nätverket trafik tooand från gästagenten.
- Vissa åtkomstkontrollistor (ACL) förhindrar åtkomst till disken.
- Otillräckligt diskutrymme kan blockera hello gästagenten från att fungera korrekt. 

Som standard hello användargränssnittet för Microsoft-program mot skadlig kod har inaktiverats, [att aktivera Microsoft program mot skadlig kod användargränssnittet på Azure Resource Manager virtuella datorer efter distribution](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/) för mer information om hur tooenable den om du behöver.

## <a name="troubleshooting-problems-loading-hello-dashboard"></a>Felsökning av problem som läser in hello instrumentpanelen

Om det uppstår problem som läser in hello Security Center instrumentpanelen säkerställa att hello som registrerar hello prenumeration tooSecurity Center (d.v.s. hello första användare som också öppna Security Center med hello prenumeration) och hello-användare som vill tooturn på insamling av data ska vara *ägare* eller *deltagare* på hello prenumeration. Från den tidpunkt då om även användare med *Reader* på hello prenumeration visas hello instrumentpanelen/aviseringar/rekommendation/princip.

## <a name="contacting-microsoft-support"></a>Kontakta Microsoft Support
Vissa problem kan identifieras med hello instruktionerna i den här artikeln, andra du kan också hitta beskrivs i hello Security Center offentliga [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter). Om du behöver ytterligare felsökning kan du öppna en ny supportbegäran med hjälp av **Azure Portal** enligt nedan: 

![Microsoft Support](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## <a name="see-also"></a>Se även
I det här dokumentet du lärt dig hur tooconfigure säkerhetsprinciper i Azure Security Center. toolearn mer om Azure Security Center finns hello följande:

* [Planera för Azure Security Center och handboken](security-center-planning-and-operations-guide.md) – Lär dig hur tooplan och förstå hello design överväganden tooadopt Azure Security Center.
* [Övervakning av säkerhetshälsa i Azure Security Center](security-center-monitoring.md) – Lär dig hur toomonitor hello Azure-resursers hälsa
* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar
* [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md) – Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure

