---
title: aaaFind aktivitetsrapporter i hello Azure-portalen | Microsoft Docs
description: "Lär dig hur toofind Azure Active Directory-aktivitet rapporter i hello Azure-portalen."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: d93521f8-dc21-4feb-aaff-4bb300f04812
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/19/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: f8d7a968403e10ccc5319f27fedad38b1553ded0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="find-activity-reports-in-hello-azure-portal"></a>Hitta aktivitetsrapporter i hello Azure-portalen

Om du flyttar från hello Azure klassiska portal toohello Azure-portalen, får du en ny titt på Azure Active Directory (AD Azure) aktivitetsloggar. I en senaste [blogginlägget](https://blogs.technet.microsoft.com/enterprisemobility/2016/11/08/azuread-weve-just-turned-on-detailed-auditing-and-sign-in-logs-in-the-new-azure-portal/), förklarar vi hur du kan se aktiviteten loggar hello gäller hello-resurs som du arbetar med i hello Azure-portalen. I den här artikeln beskriver vi hur toofind rapporter som du använde i hello klassiska Azure-portalen i hello Azure-portalen.

## <a name="whats-new"></a>Nyheter

Rapporter i hello klassiska Azure-portalen är uppdelade i kategorier:

1.  Säkerhetsrapporter
2.  Aktivitetsrapporter
3.  Integrerad apprapporter

### <a name="activity-and-integrated-app-reports"></a>Aktiviteten och integrerad apprapporter

För sammanhangsberoende rapportering i hello Azure-portalen, kombineras befintliga rapporter till en enda vy. En enda, underliggande API ger hello toohello datavy.

Detta visas på hello toosee **Azure Active Directory** bladet under **AKTIVITETEN**väljer **granskningsloggar**.

![Granskningsloggar](./media/active-directory-reporting-migration/482.png "Granskningsloggar")

följande rapporter hello konsolideras i den här vyn:

-   Granska rapporten
-   Lösenordsåterställningsaktivitet
-   Lösenordsåterställningsaktivitet för registrering
-   Självbetjäning grupper aktivitet
-   Ändringar av Office365 namn
-   Kontot etablering aktivitet
-   Status för förnyelse av lösenord
-   Kontoetableringsfel


hello programanvändning rapporten har förbättrats och ingår i hello **inloggningar** vyn. Detta visas på hello toosee **Azure Active Directory** bladet under **AKTIVITETEN**väljer **inloggningar**.

![Inloggningar visa](./media/active-directory-reporting-migration/483.png "inloggningar visa")

Hej **inloggningar** vyn innehåller alla användarinloggningar. Du kan använda denna information tooget användning programinformation. Du kan också visa information om användning i hello **företagsprogram** översikt i hello **hantera** avsnitt.

![Företagsprogram](./media/active-directory-reporting-migration/484.png "företagsprogram")

## <a name="access-a-specific-report"></a>Åtkomst till en rapport

Även om hello Azure-portalen erbjuder en enda vy kan titta du också på specifika rapporter.

### <a name="audit-logs"></a>Granskningsloggar

I svaret toocustomer feedback kan i hello Azure-portalen, du använda avancerad filtrering tooaccess hello data som du vill. Är ett filter som du kan använda en *aktivitetskategorin*, som visar hello olika typer av aktiviteten loggar i Azure AD. toonarrow resulterar toowhat som du söker efter kan du välja en kategori.

Om du är intresserad av att endast aktiviteter relaterade tooself service lösenordsåterställning kan du till exempel välja hello **Self-service lösenordshantering** kategori. hello-kategorier som du ser baseras på hello-resurs som du arbetar med.  

![Kategori alternativen på hello Filter granskningsloggar sidan](./media/active-directory-reporting-migration/06.png "kategori alternativen på sidan för hello-Filter granskningsloggar")

Aktivitetskategorier inkluderar:

- Kärnkatalog
- Hantering av lösenord för självbetjäning
- Självbetjäning, grupphantering
- Kontoetablering

### <a name="application-usage"></a>Programanvändning

tooview information om programanvändning för alla appar eller för en enda app under **AKTIVITETEN**väljer **inloggningar**. toonarrow hello resultat kan du filtrera efter användarnamn eller programnamn.

![Filter inloggning händelser sidan](./media/active-directory-reporting-migration/07.png "filtrera inloggning händelser sida")

### <a name="security-reports"></a>Säkerhetsrapporter

#### <a name="azure-ad-anomalous-activity-reports"></a>Azure AD avvikande aktivitetsrapporter

Azure AD avvikande aktivitetssäkerhet rapporter från hello klassiska Azure-portalen har konsoliderade tooprovide du med en enda central vy. Den här vyn visar alla riskhändelser som säkerhetsrelaterade att Azure AD kan identifiera och rapportera om.

hello i den följande tabellen listas hello Azure AD avvikande aktivitet säkerhetsrapporter och motsvarande risk händelsetyper i hello Azure-portalen.

| Azure AD avvikande Aktivitetsrapport |  Identity protection risk händelsetyp|
| :--- | :--- |
| Används med läckta autentiseringsuppgifter | Läckta autentiseringsuppgifter |
| Oregelbunden inloggningsaktivitet | Omöjlig resa tooatypical platser |
| Inloggningar från potentiellt infekterade enheter | Inloggningar från angripna enheter|
| Inloggningar från okända källor | Inloggningar från anonyma IP-adresser |
| Inloggningar från IP-adresser med misstänkt aktivitet | Inloggningar från IP-adresser med misstänkt aktivitet |
| - | Inloggningar från okända platser |

hello följande Azure AD avvikande aktivitetssäkerhet rapporterar inte ingår som riskhändelser i hello Azure-portalen:

* Inloggningar efter flera fel
* Inloggningar från flera geografiska områden

Dessa rapporter är kvar i hello klassiska Azure-portalen, men de kommer att inaktualiseras någon gång hello framtida.

Mer information finns i avsnittet om [Azure Active Directory-riskhändelser](active-directory-identity-protection-risk-events.md).  


#### <a name="detected-risk-events"></a>Identifierade riskhändelser

I hello Azure-portalen, kan du komma åt rapporter om identifierad riskhändelser på hello **Azure Active Directory** bladet under **säkerhet**. Identifierade riskhändelser spåras i hello följande rapporter:   

- Användare i riskzonen
- Riskfyllda inloggningar

![Säkerhetsrapporter](./media/active-directory-reporting-migration/04.png "säkerhetsrapporter")

Mer information om säkerhetsrapporter finns:

- [Användare på Risk säkerhetsrapporten hello Azure Active Directory-portalen](active-directory-reporting-security-user-at-risk.md)
- [Riskfyllda inloggningar rapporten i hello Azure Active Directory-portalen](active-directory-reporting-security-risky-sign-ins.md)


## <a name="activity-reports-in-hello-azure-classic-portal-vs-hello-azure-portal"></a>Aktivitetsrapporter i hello klassiska Azure-portalen kontra hello Azure-portalen

hello tabell i det här avsnittet innehåller befintliga rapporter i hello klassiska Azure-portalen. Här beskrivs också hur du kan få hello samma information i hello Azure-portalen.

alla tooview granska data på hello **Azure Active Directory** bladet under **AKTIVITETEN**, gå för**granskningsloggar**.

![Granskningsloggar](./media/active-directory-reporting-migration/61.png "Granskningsloggar")

| Klassisk Azure-portal                 | toofind i hello Azure-portalen                                                         |
| ---                                  | ---                                                                        |
| Granskningsloggar                           | För **aktivitetskategorin**väljer **Core Directory**.                       |
| Lösenordsåterställningsaktivitet              | För **aktivitetskategorin**väljer **Self-service lösenordshantering**. |
| Lösenordsåterställningsaktivitet för registrering | För **aktivitetskategorin**väljer **Self-service lösenordshantering**.     |
| Självbetjäning grupper aktivitet         | För **aktivitetskategorin**väljer **Self-service Group Management**.        |
| Kontot etablering aktivitet        | För **aktivitetskategorin**väljer **konto Användaretablering**.         |
| Status för förnyelse av lösenord             | För **aktivitetskategorin**väljer **automatisk förnyelse av App lösenord**.      |
| Kontoetableringsfel          | För **aktivitetskategorin**väljer **konto Användaretablering**.        |
| Ändringar av Office365 namn         | För **aktivitetskategorin**väljer **Self-service lösenordshantering**. För **aktivitet resurstypen**väljer **gruppen**. För **Aktivitetskälla**väljer **O365 grupper**.|

tooview hello **programanvändning** rapport om hello **Azure Active Directory** bladet under **hantera**väljer **företagsprogram**, och välj sedan **inloggningar**.


![Enterprise program inloggningar rapporten](./media/active-directory-reporting-migration/199.png "Enterprise program inloggningar rapport")

## <a name="next-steps"></a>Nästa steg

En översikt över rapportering finns i hello [Azure Active Directory reporting](active-directory-reporting-azure-portal.md).
