---
title: aaaAzure Log-integrering med Azure Active Directory-granskningsloggar | Microsoft Docs
description: "Lär dig hur tooinstall hello Azure Log integrationstjänsten och integrera loggar från Azure-granskningsloggar"
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
ms.date: 08/08/2017
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 3ee8fa3b8b5e9bd33202e57ed5327cd8d3127f00
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-active-directory-audit-logs"></a>Integrera Azure Active Directory-granskningsloggar

Granskningshändelser för Azure Active Directory (AD Azure) hjälper dig att identifiera Privilegierade åtgärder som har inträffat i Azure Active Directory. Du kan se hello typer av händelser som du kan följa genom att granska [Azure Active Directory-granskningsrapporthändelser](/active-directory/active-directory-reporting-audit-events#list-of-audit-report-events.md).

> [!NOTE]
> Innan du försöker hello stegen i den här artikeln, bör du granska hello [Kom igång](security-azure-log-integration-get-started.md) artikel och slutföra hello stegen.

## <a name="steps-toointegrate-azure-active-directory-audit-logs"></a>Steg toointegrate Azure Active directory-granskningsloggar

1. Öppna hello-kommandotolk och kör det här kommandot:

   ``cd c:\Program Files\Microsoft Azure Log Integration``

2. Kör följande kommando: 
 
   ``azlog createazureid``

   Det här kommandot uppmanas du att din Azure-inloggning. hello kommando skapar sedan ett Azure Active Directory tjänstens huvudnamn i hello Azure AD-klienter som är värdar för hello Azure-prenumerationer som hello inloggade användare är administratör, en medadministratör eller en ägare. hello kommandot misslyckas om hello inloggade användaren är endast gästanvändaren i hello Azure AD-klient. Autentisering tooAzure görs via Azure AD. Skapa ett huvudnamn för tjänsten för Azure Log integrering skapar hello Azure AD identity som ges åtkomst tooread från Azure-prenumerationer.

3. Kör följande kommando tooprovide hello klient-ID. Du måste toobe tillhör hello klient admin rollen toorun hello kommando.

   ``Azlog.exe authorizedirectoryreader tenantId``

   Exempel:

   ``AZLOG.exe authorizedirectoryreader ba2c0000-d24b-4f4e-92b1-48c4469999``

4. Kontrollera att följande mappar tooconfirm som hello JSON för Azure Active Directory-granskningsloggarna hello skapas i dem:

   * **C:\Users\azlog\AzureActiveDirectoryJson**
   * **C:\Users\azlog\AzureActiveDirectoryJsonLD**

hello visar följande videoklipp hello stegen som beskrivs i den här artikeln:

> [!VIDEO https://channel9.msdn.com/Blogs/Azure-Security-Videos/Azure-Log-Integration-Videos-Azure-AD-Integration/player]


> [!NOTE]
> Specifika anvisningar för att hämta hello information i hello JSON-filer till din säkerhetsinformation och Händelsehanteringssystem (SIEM), försäljaren SIEM.

Community-hjälp är tillgänglig via hello [Azure Log Integration MSDN-Forum](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration). Det här forumet kan personer i hello Azure Log-integrering community toosupport varandra med frågor, svar, tips och råd. Dessutom övervakar det här forumet hello Azure Log-integrering team och hjälper när den kan.

Du kan även öppna en [supportbegäran](../azure-supportability/how-to-create-azure-support-request.md). Välj **loggen Integration** som hello-tjänst som du begär support.

## <a name="next-steps"></a>Nästa steg
toolearn mer om Azure Log-integrering finns:

* [Microsoft Azure Log-integrering för Azure loggar](https://www.microsoft.com/download/details.aspx?id=53324): den här Download Center sidan ger information, systemkrav och Installationsinstruktioner för Azure Log-integrering.
* [Introduktion tooAzure loggen Integration](security-azure-log-integration-overview.md): den här artikeln introducerar tooAzure loggen Integration, de viktigaste funktionerna och hur det fungerar.
* [Samarbeta konfigurationssteg](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/): det här blogginlägget visar hur tooconfigure Azure Log-integrering toowork med partnerlösningar Splunk, HP ArcSight och IBM QRadar.
* [Azure Log Integration FAQ](security-azure-log-integration-faq.md): den här artikeln innehåller svar på frågor om Azure Log-integrering.
* [Integrera Security Center-aviseringar med Azure Log-integrering](../security-center/security-center-integrating-alerts-with-log-integration.md): den här artikeln visar hur toosync Security Center aviseringar, tillsammans med virtuella säkerhetshändelser samlas in av Azure-diagnostik och Azure granskningsloggar med din logganalys eller SIEM-lösning.
* [Nya funktioner för Azure-diagnostik och Azure-granskningsloggar](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/): det här blogginlägget beskriver tooAzure granskningsloggar och andra funktioner som hjälper dig få insikter om hello operations resurserna i Azure.
