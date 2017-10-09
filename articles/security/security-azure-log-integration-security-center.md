---
title: aaaAzure Log-integrering med security center | Microsoft Docs
description: "Lär dig hur tooget Azure Security center aviseringar arbetar med Log-integration"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ums.workload: na
ms.date: 03/22/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: bcc208d071ec03738215f2aee3b71c7b10927904
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-your-security-center-alerts-in-azure-log-integration"></a>Hur tooget Security Center aviseringar i Azure logganalys-integrering
Den här artikeln innehåller hello steg krävs tooenable hello Azure Log Integration service toopull Säkerhetsvarning information som genererats av Azure Security Center. Du måste ha slutfört hello stegen i hello [Kom igång](security-azure-log-integration-get-started.md) artikel innan du utför hello stegen i den här artikeln.

## <a name="detailed-steps"></a>Detaljerade steg
hello följande steg skapar hello krävs för Azure Active Directory Service Principal och tilldela hello Service Principle läsbehörighet toohello prenumerationen:
1. Öppna hello-kommandotolk och navigera för**c:\Program Files\Microsoft Azure Log-integrering**
2. Kör hello kommando``azlog createazureid``

    Det här kommandot uppmanas du att din Azure-inloggning. hello-kommando skapar sedan en [Azure Active Directory Service Principal](../active-directory/develop/active-directory-application-objects.md) i hello hello Azure AD-klienter som är värdar för Azure-prenumerationer i vilka hello inloggad användare är administratör, en Medadministratör eller en ägare. hello kommandot misslyckas om hello inloggad användare är endast gästanvändaren i hello Azure AD-klient. Autentisering tooAzure görs via Azure Active Directory (AD). Skapa ett huvudnamn för tjänsten för Azlog integrering skapar hello Azure AD-identitet som kommer att få åtkomst till tooread från Azure-prenumerationer.

2. Därefter ska du köra ett kommando som tilldelar reader åtkomst på hello prenumeration toohello tjänstens huvudnamn skapade i steg 2. Om du inte anger en SubscriptionID försöker hello-kommandot tooassign hello service principal reader rollen tooall prenumerationer toowhich du har åtkomst. </br></br>
``azlog authorize <SubscriptionID>`` </br> till exempel </br>
``azlog authorize 0ee55555-0bc4-4a32-a4e8-c29980000000``

    >[!NOTE]
    Varningar kan visas om du kör hello auktorisera kommandot omedelbart efter hello createazureid kommando. Det finns vissa fördröjning mellan när hello Azure AD-kontot skapas och när hello-kontot är tillgängligt för användning. Om du väntar 60 sekunder efter att du kört hello createazureid kommandot toorun hello auktorisera kommandot sedan dessa varningar inte ska visas.

4. Kontrollera hello följande mappar tooconfirm som hello JSON granskningsloggarna finns det:
 * **c:\Users\azlog\AzureResourceManagerJson**
 * **c:\Users\azlog\AzureResourceManagerJsonLD** </br></br>
5. Kontrollera hello följande mappar tooconfirm som Security Center-aviseringar finns i dem:</br></br>
 * **c:\Users\azlog\AzureSecurityCenterJson**
 * **c:\Users\azlog\AzureSecurityCenterJsonLD** </br></br>

Om du stöter på problem under hello installation och konfiguration, öppnar du en [supportbegäran](/azure-supportability/how-to-create-azure-support-request.md)väljer **loggen Integration** som hello-tjänst som du begär support.

## <a name="next-steps"></a>Nästa steg
toolearn mer om Azure Log-integrering finns hello följande dokument:

* [Microsoft Azure Log-integrering för Azure loggar](https://www.microsoft.com/download/details.aspx?id=53324) – Download Center för ytterligare information, systemkrav, och installera instruktioner på Azure logganalys-integration.
* [Introduktion tooAzure loggen integration](security-azure-log-integration-overview.md) – det här dokumentet introducerar tooAzure loggen integration, de viktigaste funktionerna och hur det fungerar.
* [Samarbeta konfigurationssteg](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) – det här blogginlägget visar hur tooconfigure Azure logga toowork integrering med partnerlösningar Splunk, HP ArcSight och IBM QRadar.
* [Azure logganalys Integration vanliga frågor (FAQ)](security-azure-log-integration-faq.md) – det här avsnittet får du svar frågor om Azure logganalys-integration.
* [Integrera Security Center aviseringar med Azure logga Integration](../security-center/security-center-integrating-alerts-with-log-integration.md) – det här dokumentet beskrivs hur toosync Security Center aviseringar, tillsammans med virtuella säkerhetshändelser samlas in av Azure-diagnostik och Azure-granskningsloggarna med din logganalys eller SIEM-lösning.
* [Nya funktioner för Azure-diagnostik och Azure-granskningsloggarna](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) – det här blogginlägget introducerar tooAzure granskningsloggar och andra funktioner som hjälper dig få insikter om hello operations resurserna i Azure.
