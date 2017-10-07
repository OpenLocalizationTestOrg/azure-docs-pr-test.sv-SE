---
title: aaaIntegrating Azure Security Center-aviseringar med Azure logga integration | Microsoft Docs
description: "Den här artikeln hjälper dig att komma igång med Security Center-aviseringar integrera med Azure logganalys-integration."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: d2d088d3-d38d-47ff-a062-c78e0fd59226
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/23/2017
ms.author: terrylan
ms.openlocfilehash: 2649036ee990bf0f48fa0cb35c7495ac932c29ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrating-azure-security-center-alerts-with-azure-log-integration"></a>Integrera Azure Security Center-aviseringar med Azure logganalys-integration
Förlitar sig på en lösning för Security Information and Event Management (SIEM) som startpunkt för triaging och undersöker säkerhetsaviseringar hello många säkerhetsåtgärder och incidenter team. Du kan integrera Azure Security Center-aviseringar med Azure Log-integrering med SIEM-lösning.

Azure logganalys-integration stöder för närvarande HP ArcSight Splunk och IBM QRadar.

## <a name="what-logs-can-i-integrate"></a>Vilka loggar kan integrera?
Azure ger utförlig loggning för varje tjänst. Dessa loggar är kategoriserade som:

* **Kontroll och hantering loggar** som ger inblick i hello Azure Resource Manager skapa-, UPDATE- och DELETE-åtgärder. Dessa plan kontrollhändelserna exponeras i hello Azure aktivitetsloggar
* **Plan för dataloggar** som ger inblick i hello händelser som utlöses när du använder en Azure-resurs. Ett exempel är hello Windows-händelseloggen, där du kan hämta säkerhetsinformation för händelse från hello Event Viewer säkerhetskanal. Data plan händelser (som genereras av en virtuell dator eller en Azure-tjänst) är anslutna med Azure diagnostikloggar.

Azure logganalys-integration stöder för närvarande hello integrering av:

* Azure VM-loggar
* Azure-granskningsloggar
* Azure Security Center-aviseringar

## <a name="install-azure-log-integration"></a>Installera Azure logganalys-integrering
Hämta [Azure logganalys integration](https://www.microsoft.com/download/details.aspx?id=53324).

hello Azure integration Loggtjänsten samlar in telemetridata från hello-dator som är installerad.  I insamlade telemetridata är:

* Undantag som uppstår vid körning av Azure logganalys-integrering
* Mått om hello antalet frågor och händelser som har bearbetats
* Statistik om vilka Azlog.exe kommandoradsalternativ som används

> [!NOTE]
> Du kan stänga av insamling av telemetridata genom att avmarkera det här alternativet.
>
>

## <a name="integrate-azure-audit-logs-and-security-center-alerts"></a>Integrera Azure-granskningsloggarna och Security Center-aviseringar
1. Öppna hello-kommandotolk och **cd** till **c:\Program Files\Microsoft Azure Log integrering**.
2. Kör hello **azlog createazureid** kommandot toocreate en [Azure Active Directory Service Principal](../active-directory/active-directory-application-objects.md) i hello Azure Active Directory (AD) hello klienter som är värdar för Azure-prenumerationer.

    Du ombeds ange inloggningsinformation Azure.

   > [!NOTE]
   > Du måste vara hello prenumeration ägare eller en Medadministratör för hello prenumeration.
   >
   >

    Autentisering tooAzure görs via Azure AD.  Skapa ett huvudnamn för tjänsten för Azure logganalys integrering skapar hello Azure AD identity som ges åtkomst tooread från Azure-prenumerationer.
3. Kör hello **azlog auktorisera <SubscriptionID>**  kommandot tooassign Reader åtkomst på hello prenumeration toohello tjänstens huvudnamn skapade i steg 2. Om du inte anger en **SubscriptionID**, hello tjänstens huvudnamn är tilldelade hello Reader rollen tooall prenumerationer toowhich du har åtkomst.

   > [!NOTE]
   > Varningar kan visas om du kör hello **auktorisera** kommandot omedelbart efter hello **createazureid** kommando. Det finns vissa fördröjning mellan när hello Azure AD-kontot skapas och när hello-kontot är tillgängligt för användning. Om du väntar cirka 10 sekunder efter att du kört hello **createazureid** kommandot toorun hello **auktorisera** kommandot sedan dessa varningar inte ska visas.
   >
   >
4. Kontrollera hello följande mappar tooconfirm som hello JSON granskningsloggarna finns det:

   * **c:\Users\azlog\AzureResourceManagerJson**
   * **c:\Users\azlog\AzureResourceManagerJsonLD**
5. Kontrollera hello följande mappar tooconfirm som Security Center-aviseringar finns i dem:

   * **c:\Users\azlog\ AzureSecurityCenterJson**
   * **c:\Users\azlog\AzureSecurityCenterJsonLD**
6. Konfigurera hello SIEM vidarebefordrare connector toohello rätt mapp. hello proceduren varierar beroende på hello SIEM som du använder.

## <a name="next-steps"></a>Nästa steg
toolearn mer om Azure aktivitetsloggar och egenskapsdefinitioner, se:

* [Granska driften med Resource Manager](../azure-resource-manager/resource-group-audit.md)

toolearn mer om Security Center finns hello följande:

* [Hantera och svarar toosecurity aviseringar i Azure Security Center](security-center-managing-and-responding-alerts.md) – Lär dig hur toomanage och svara toosecurity aviseringar.
* [Vanliga frågor om Azure Security Center](security-center-faq.md) – finns vanliga frågor om hur du använder hello-tjänsten.
* [Azures säkerhetsblogg](http://blogs.msdn.com/b/azuresecurity/) – hello Azure security nyheter och information.
