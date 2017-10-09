---
title: aaaPartner integrering i Azure Security Center | Microsoft Docs
description: "Lär dig mer om hur Azure Security Center kan integreras med partners tooenhance övergripande säkerheten för dina Azure-resurser."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: hero-article
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2017
ms.author: yurid
ms.openlocfilehash: 3621335730a076721cb3c23788a47be50aa8fc73
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="partner-integration-in-azure-security-center"></a>Partnerintegrering i Azure Security Center

I den här artikeln beskriver vi hur Azure Security Center kan integreras med partners toohelp du förbättra säkerheten för övergripande. Ger en integrerad upplevelse i Azure Security Center och drar nytta av hello Azure Marketplace för partner och fakturering för certifikatutfärdare.

> [!NOTE] 
> Från och med juni 2017 använder Security Center hello Microsoft Monitoring Agent toocollect och lagra data. Mer information finns i [plattformsmigrering i Azure Security Center](security-center-platform-migration.md). hello informationen i den här artikeln representerar Security Center-funktionalitet efter övergången toohello Microsoft Monitoring Agent.
>

## <a name="why-deploy-partner-solutions-from-security-center"></a>Varför ska du distribuera partnerlösningar från Security Center

Det finns fyra huvudsakliga skälen tooleverage partner integrering i Security Center:

- **Enkel distribution**. Det är mycket enklare att distribuera en partner-lösningen genom följande hello Security Center rekommendation. hello distributionsprocessen kan automatiseras helt med hjälp av en standard-installationen och nätverkstopologi. Kunder kan också välja ett halvautomatiserat alternativ för mer flexibilitet och anpassning.
- **Integrerade identifieringar**. Säkerhetshändelser från partnerlösningar samlas in, aggregeras och visas automatiskt som en del av aviseringarna och incidenterna i Security Center. Dessa händelser är smält med identifieringar från andra källor tooprovide avancerade hotidentifiering funktioner.
- **Enhetlig hälsoövervakning och hantering**. Kunder kan använda integrerad hälsa händelser toomonitor alla partnerlösningar i korthet. Grundläggande hantering är tillgänglig med enkel åtkomst tooadvanced installationen med hjälp av hello partnerlösning.
- **Exportera tooSIEM**. Kunder kan exportera alla Security Center och partner varnar gemensamma händelsen Format (CEF) tooon lokala Security Information and Event Management SIEM ()-system med hjälp av Azure logganalys-integrering (förhandsversion).


## <a name="partners-that-integrate-with-security-center"></a>Partners som integreras med Security Center

För närvarande integrerar Security Center med följande lösning:

- Slutpunktsskydd ([Trend Micro](https://help.deepsecurity.trendmicro.com/azure-marketplace-getting-started-with-deep-security.html), Symantec och [Microsoft Antimalware för Azure Cloud Services och Virtual Machines](https://docs.microsoft.com/azure/security/azure-security-antimalware)) 
- Brandvägg för webbaserade program ([Barracuda](https://www.barracuda.com/products/webapplicationfirewall), [F5](https://support.f5.com/kb/en-us/products/big-ip_asm/manuals/product/bigip-ve-web-application-firewall-microsoft-azure-12-0-0.html), [Imperva](https://www.imperva.com/Products/WebApplicationFirewall-WAF), [Fortinet](https://www.fortinet.com/resources.html?limit=10&search=&document-type=data-sheets) och [Azure Application Gateway](https://azure.microsoft.com/blog/azure-web-application-firewall-waf-generally-available/)) 
- Nästa generations brandvägg ([Check Point](https://www.checkpoint.com/products/vsec-microsoft-azure/), [Barracuda](https://campus.barracuda.com/product/nextgenfirewallf/article/NGF/AzureDeployment/), [Fortinet](http://docs.fortinet.com/d/fortigate-fortios-handbook-the-complete-guide-to-fortios-5.2) och [Cisco](http://www.cisco.com/c/en/us/td/docs/security/firepower/quick_start/azure/ftdv-azure-qsg.html)) 
- Sårbarhetsbedömning ([Qualys](https://www.qualys.com/public-clouds/microsoft-azure/))  

Över tiden, Security Center kommer att expandera hello antalet partners i dessa kategorier och lägga till nya kategorier. 

## <a name="deploy-a-partner-solution"></a>Distribuera en partnerlösning

Baserat på hello installationen av dina Azure-miljön och hello säkerhetsprincip som du har definierat kan Security Center rekommenderar att du distribuerar en partnerlösning. hello Security Center rekommendation hjälper dig att hello med att välja och installera en partnerlösning. hello kan övergripande distributionsupplevelse variera beroende på hello typ av lösningen och partner som du använder. Mer information finns i följande artiklar hello:

- [Installera slutpunktsskydd](security-center-install-endpoint-protection.md)
- [Lägga till en brandvägg för webbappar](security-center-add-web-application-firewall.md)
- [Lägg till en nästa generations brandvägg](security-center-add-next-generation-firewall.md)
- [Sårbarhetsbedömning inte installerad](security-center-vulnerability-assessment-recommendations.md)

## <a name="manage-partner-solutions"></a>Hantera partnerlösningar

Efter distributionen tooview information om hello hälsotillstånd hello lösning och utföra grundläggande hanteringsuppgifter på hello **Security Center** bladet, Välj hello **partnerlösningar** alternativet. Mer information om hur du hanterar partnerlösningar i Security Center finns i [Övervaka partnerlösningar med Azure Security Center](security-center-partner-solutions.md).

![Partnerintegration](./media/security-center-partner-integration/security-center-partner-integration-fig1-new2.png)

> [!NOTE]
> Stöd för Symantec endpoint protection är begränsad toodiscovery. Det finns inga hälsoaviseringar.
>

## <a name="see-also"></a>Se även

I den här artikeln får du lära dig hur toointegrate partnerlösningar i Azure Security Center. toolearn mer om Security Center finns hello följande artiklar:

* [Planerings- och bruksanvisningsguide för Security Center](security-center-planning-and-operations-guide.md)
* [Hanterar och åtgärdar toosecurity aviseringar i Security Center](security-center-managing-and-responding-alerts.md)
* [Säkerhetsaviseringar efter typ i Security Center](security-center-alerts-type.md)
* [Övervakning av säkerhetshälsa i Security Center](security-center-monitoring.md). Lär dig hur toomonitor hello Azure-resursers hälsa.
* [Övervaka partnerlösningar med Security Center](security-center-partner-solutions.md). Lär dig hur toomonitor hello dina partnerlösningars hälsostatus.
* [Vanliga frågor och svar om Azure Security Center](security-center-faq.md). Få svar toofrequently frågor och svar om hello-tjänsten.
* [Azure säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/). Här hittar du blogginlägg om säkerhet och regelefterlevnad i Azure.
