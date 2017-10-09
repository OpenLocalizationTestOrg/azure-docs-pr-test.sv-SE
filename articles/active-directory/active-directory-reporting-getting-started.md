---
title: "Azure Active Directory Reporting: Komma igång | Microsoft Docs"
description: "Visar hello olika tillgängliga rapporterna i Azure Active Directory reporting"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: 7ac99919-8df5-4424-9298-fc7c025ba949
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: f47875708398391dd7f3efdc56a741fdba273b76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-active-directory-reporting"></a>Komma igång med Azure Active Directory Reporting
## <a name="what-it-is"></a>Vad det är
Azure AD (Active Directory Azure) tillhandahåller säkerhets-, aktivitets- och granskningsrapporter för din katalog. Här är en lista över rapporter hello:

### <a name="security-reports"></a>Säkerhetsrapporter
* Inloggningar från okända källor
* Inloggningar efter flera fel
* Inloggningar från flera geografiska områden
* Inloggningar från IP-adresser med misstänkt aktivitet
* Oregelbunden inloggningsaktivitet
* Inloggningar från potentiellt infekterade enheter
* Användare med avvikande inloggningsaktivitet

### <a name="activity-reports"></a>Aktivitetsrapporter
* Programanvändning: sammanfattning
* Programanvändning: detaljerad
* Instrumentpanel för program
* Kontoetableringsfel
* Enskilda användarenheter
* Aktivitet för enskilda användare
* Aktivitetsrapport för grupper
* Aktivitetsrapport över registrering av lösenordsåterställning
* Lösenordsåterställningsaktivitet

### <a name="audit-reports"></a>Granskningsrapporter
* Kataloggranskningsrapport

> [!TIP]
> Mer dokumentation om Azure AD Reporting finns i [Visa åtkomst- och användningsrapporter](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="how-it-works"></a>Hur det fungerar
### <a name="reporting-pipeline"></a>Rapporteringsflöde
Hej rapporteringsflödet består av tre steg. Varje gång en användare loggar in, eller en autentisering görs, händer följande hello:

* Först hello användaren har autentiserats (lyckas eller misslyckas) och hello resultatet lagras i hello Azure Active Directory service-databaser.
* De senaste inloggningarna bearbetas med jämna mellanrum. I detta läge söker våra säkerhetsalgoritmer och algoritmer för avvikande aktivitet igenom de senaste inloggningarna och letar efter misstänkt aktivitet.
* Efter bearbetningen hello rapporter skrivs, cachelagras och hanteras i hello klassiska Azure-portalen.

### <a name="report-generation-times"></a>Rapportera genereringstider
På grund av toohello stora mängden autentiseringar och logga moduler som bearbetas av hello Azure AD-plattformen, är hello senaste inloggningar som bearbetas i genomsnitt en timme gamla. I sällsynta fall kan det ta upp too8 timmar tooprocess hello senaste inloggningar.

Du kan hitta hello senast bearbetade inloggningen genom att undersöka hello hjälptexten hello överst i varje rapport.

![Hjälptexten hello överst i varje rapport](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [!TIP]
> Mer dokumentation om Azure AD Reporting finns i [Visa åtkomst- och användningsrapporter](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="getting-started"></a>Komma igång
### <a name="sign-into-hello-azure-classic-portal"></a>Logga in på hello klassiska Azure-portalen
Först behöver du toosign till hello [klassiska Azure-portalen](https://manage.windowsazure.com) som en global administratör eller efterlevnadsadministratör. Du måste också vara ett Azure-prenumeration tjänstadministratör eller en medadministratör eller använda hello ”åtkomst tooAzure AD” Azure-prenumeration.

### <a name="navigate-tooreports"></a>Navigera tooReports
tooview rapporter, navigera toohello rapporter fliken hello överst i din katalog.

Om det här är första gången du visar hello rapporter behöver tooagree tooa dialogrutan innan du kan visa hello rapporter. Detta är att den är godkänd för administratörer i din organisation tooview tooensure dessa data, som kan betraktas som privat information i vissa länder.

![Dialogruta](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>Utforska varje rapport
Navigera till varje rapport toosee hello data som samlas in och hello inloggningar som bearbetas. Du hittar en [lista över alla hello rapporter här](active-directory-reporting-guide.md).

![Alla rapporter](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-hello-reports-as-csv"></a>Hämta hello rapporter som CSV-fil
Alla rapporter kan laddas ned som CSV-filer (kommaavgränsade). Du kan använda de här filerna i Excel, PowerBI eller tredje parts analys program toofurther analysera dina data.

toodownload någon rapport som en CSV-fil, navigera toohello rapporten och klicka på ”ladda ned” längst ned hello.

![Knappen Ladda ned](./media/active-directory-reporting-getting-started/downloadButton.png)

> [!TIP]
> Mer dokumentation om Azure AD Reporting finns i [Visa åtkomst- och användningsrapporter](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="next-steps"></a>Nästa steg
### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>Anpassa aviseringar om avvikande inloggningsaktivitet
Navigera toohello ”konfigurera” fliken i din katalog.

Rulla toohello ”meddelanden” avsnittet.

Aktivera eller inaktivera hello ”e-postaviseringar av avvikande inloggningar” avsnittet.

![hello meddelanden](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-hello-azure-ad-reporting-api"></a>Integrera med hello Azure AD Reporting API
Se [komma igång med hello Reporting API](active-directory-reporting-api-getting-started.md).

### <a name="engage-multi-factor-authentication-on-users"></a>Aktivera Multi-Factor Authentication för användare
Välj en användare i en rapport.

Klicka hello ”aktivera MFA” längst ned hello hello-skärmen.

![Hej Multi-Factor Authentication-knappen längst ned hello hello-skärmen](./media/active-directory-reporting-getting-started/mfaButton.png)

> [!TIP]
> Mer dokumentation om Azure AD Reporting finns i [Visa åtkomst- och användningsrapporter](active-directory-view-access-usage-reports.md).
> 
> 

## <a name="learn-more"></a>Läs mer
### <a name="audit-events"></a>Granska händelser
Lär dig mer om vilka händelser som granskas i hello directory i [Azure Active Directory Reporting granskningshändelser](active-directory-reporting-audit-events.md).

### <a name="api-integration"></a>API-integrering
Se [komma igång med hello Reporting API](active-directory-reporting-api-getting-started.md) och hello [API-referensdokumentation](https://msdn.microsoft.com/library/azure/mt126081.aspx).

### <a name="get-in-touch"></a>Kontakta oss
E-post [aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com) för feedback, hjälp eller eventuella frågor.

> [!TIP]
> Mer dokumentation om Azure AD Reporting finns i [Visa åtkomst- och användningsrapporter](active-directory-view-access-usage-reports.md).
> 
> 

