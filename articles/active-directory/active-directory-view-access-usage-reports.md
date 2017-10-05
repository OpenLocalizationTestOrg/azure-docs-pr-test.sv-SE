---
title: "Visa åtkomst-och användningsdata | Microsoft Docs"
description: "Beskriver hur du kan visa åtkomst- och rapporter för att få insyn i integritet och säkerheten för din organisations katalog."
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: a074bc4e-cf3f-4ad1-8cc6-4199d2e09ce4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: 038ac79ebf61c6429fbf7ca21eefe9414bcfc03a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="view-your-access-and-usage-reports"></a>Visa åtkomst- och användningsrapporterna
*Den här dokumentationen är en del av [Azure Active Directory Reporting-guiden](active-directory-reporting-guide.md).*

Du kan använda Azure Active Directory-åtkomst och användningsrapporter insyn i integritet och säkerheten för din organisations katalog. Med den här informationen kan fastställa en katalogadministratör bättre där möjliga säkerhetsrisker kan ligga så att de kan på lämpligt sätt att minska riskerna.

I Azure-hanteringsportalen kategoriseras rapporter på följande sätt:

* Avvikelseidentifiering rapporter – innehåller logga in händelser som vi avvikande. Vårt mål är att se till att du är medveten om sådan aktivitet och gör att du ska kunna fastställa om en händelse är misstänkt.
* Integrerad rapporter från Application – tillhandahåller insikter om hur molnprogram används i din organisation. Azure Active Directory möjliggör integrering med tusentals molnprogram.
* Felrapporter – ange fel som kan uppstå vid etablering av konton till externa program.
* Användarspecifika rapporter – visa enheten/inloggning aktivitetsdata för en viss användare.
* Aktivitetsloggar – innehåller en post på alla granskade händelser inom de senaste 24 timmarna, senaste 7 dagarna eller senaste 30 dagarna, samt ändringar av aktiviteten och aktiviteten för återställning och registrering av lösenord.

> [!NOTE]
> * Vissa avancerade användningsrapporter avvikelseidentifiering och resurser är bara tillgängliga när du aktiverar [Azure Active Directory Premium](active-directory-get-started-premium.md). Avancerade rapporter kan du förbättra säkerheten för åtkomst, svara på potentiella hot och få åtkomst till analytics på enhetsanvändning för åtkomst och program.
> * Azure Active Directory Premium och Basic är tillgängliga för kunder i Kina genom den globala instansen av Azure Active Directory. Azure Active Directory Premium och Basic stöds inte för närvarande i Microsoft Azure-tjänsten som drivs av 21Vianet i Kina. Om du vill ha mer information kontaktar du oss via [Azure Active Directory-forumet](https://feedback.azure.com/forums/169401-azure-active-directory/).
> 
> 

## <a name="reports"></a>Rapporter
| Rapport | Beskrivning |
| --- | --- |
| **Avvikande aktivitetsrapporter** | |
| [Inloggningar från okända källor](active-directory-reporting-sign-ins-from-unknown-sources.md) |Kan indikera ett försök att logga in utan som spåras. |
| [Inloggningar efter flera fel](active-directory-reporting-sign-ins-after-multiple-failures.md) |Kan indikera en lyckad nyckelsökningsangrepp. |
| [Inloggningar från flera geografiska områden](active-directory-reporting-sign-ins-from-multiple-geographies.md) |Kan indikera att flera användare loggar in med samma konto. |
| [Inloggningar från IP-adresser med misstänkt aktivitet](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md) |Kan indikera en lyckad inloggning efter en varaktig intrång försök. |
| [Inloggningar från eventuellt infekterade enheter](active-directory-reporting-sign-ins-from-possibly-infected-devices.md) |Kan indikera ett försök att logga in från potentiellt infekterade enheter. |
| [Onormal inloggningsaktivitet](active-directory-reporting-irregular-sign-in-activity.md) |Kan indikera händelser avvikande till användarnas inloggning mönster. |
| [Användare med avvikande inloggningsaktivitet](active-directory-reporting-users-with-anomalous-sign-in-activity.md) |Anger användare vars konton har komprometterats. |
| Används med läckta autentiseringsuppgifter |Används med läckta autentiseringsuppgifter |
| **Aktivitetsloggar** | |
| Granska rapporten |Granskade händelser i katalogen |
| Lösenordsåterställningsaktivitet |Ger en detaljerad vy av lösenordsåterställning som sker i din organisation. |
| Lösenordsåterställningsaktivitet för registrering |Innehåller en detaljerad vy av lösenordet för återställning registreringar som sker i din organisation. |
| Självbetjäningsportalen grupper aktivitet |En aktivitetslogg för alla gruppen självbetjäningsportalen aktivitet i din katalog |
| **Integrerade program** | |
| Programanvändning |Innehåller en sammanfattning av användning för alla SaaS-program som är integrerade med din katalog. |
| Kontot etablering aktivitet |Innehåller en historik över försöker etablera konton till externa program. |
| Status för förnyelse av lösenord |En detaljerad översikt över automatisk förnyelse lösenordsstatus för SaaS-program. |
| Kontoetableringsfel |Anger en påverkan på användarnas åtkomst till externa program. |
| **Rights management** | |
| RMS-användning |Innehåller en översikt för användning av Rights Management |
| Mest aktiva RMS-användare |Visar en lista över övre 1000 aktiva användare som RMS-skyddade filer |
| Användning av RMS-enhet |Visar en lista över enheter som används för att komma åt RMS-skyddade filer |
| Användning av RMS-aktiverade program |Ger användning av RMS-aktiverat program |

## <a name="report-editions"></a>Rapporten versioner
| Rapport | Kostnadsfri | Basic | Premium |
| --- | --- | --- | --- |
| **Avvikande aktivitetsrapporter** | | | |
| Inloggningar från okända källor |✓ |✓ |✓ |
| Inloggningar efter flera fel |✓ |✓ |✓ |
| Inloggningar från flera geografiska områden |✓ |✓ |✓ |
| Inloggningar från IP-adresser med misstänkt aktivitet | | |✓ |
| Inloggningar från eventuellt infekterade enheter | | |✓ |
| Onormal inloggningsaktivitet | | |✓ |
| Användare med avvikande inloggningsaktivitet | | |✓ |
| Används med läckta autentiseringsuppgifter | | |✓ |
| **Aktivitetsloggar** | | | |
| Granska rapporten |✓ |✓ |✓ |
| Lösenordsåterställningsaktivitet | | |✓ |
| Lösenordsåterställningsaktivitet för registrering | | |✓ |
| Självbetjäningsportalen grupper aktivitet | | |✓ |
| **Integrerade program** | | | |
| Programanvändning | | |✓ |
| Kontot etablering aktivitet |✓ |✓ |✓ |
| Status för förnyelse av lösenord | | |✓ |
| Kontoetableringsfel |✓ |✓ |✓ |
| **Rights management** | | | |
| RMS-användning | | |Endast RMS |
| Mest aktiva RMS-användare | | |Endast RMS |
| Användning av RMS-enhet | | |Endast RMS |
| Användning av RMS-aktiverade program | | |Endast RMS |

## <a name="anomalous-activity-reports"></a>Avvikande aktivitetsrapporter
<p>Avvikande inloggning aktivitetsrapporter flagga misstänkt tecken i aktiviteten till Office 365, Azure-hanteringsportalen, Azure AD-åtkomstpanelen, Sharepoint Online, Dynamics CRM Online och andra Microsoft online services.</p>

<p>Alla dessa rapporter, förutom rapporten ”inloggningar efter flera fel” flaggan också misstänkta <i>federerad</i> inloggningar till dessa tjänster, oavsett federation-providern. </p>

<p>Följande rapporter är tillgängliga: </p><ul>

<li>[Inloggningar från okända källor](active-directory-reporting-sign-ins-from-unknown-sources.md).</li>

<li>[Inloggningar efter flera fel](active-directory-reporting-sign-ins-after-multiple-failures.md).</li>

<li>[Inloggningar från flera områden](active-directory-reporting-sign-ins-from-multiple-geographies.md).</li>

<li>[Inloggningar från IP-adresser med misstänkt aktivitet](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md).</li>

<li>[Onormal inloggningsaktivitet](active-directory-reporting-irregular-sign-in-activity.md).</li>

<li>[Inloggningar från eventuellt infekterade enheter](active-directory-reporting-sign-ins-from-possibly-infected-devices.md).</li>

<li>[Användare med avvikande inloggningsaktivitet](active-directory-reporting-users-with-anomalous-sign-in-activity.md).</li>

<li>Används med läckta autentiseringsuppgifter</li></ul>

## <a name="activity-logs"></a>Aktivitetsloggar
### <a name="audit-report"></a>Granska rapporten
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Visar en förteckning över alla granskade händelser inom de senaste 24 timmarna, senaste 7 dagarna eller senaste 30 dagarna. <br /> Mer information finns i [Azure Active Directory Granskningsrapporthändelser](active-directory-reporting-audit-events.md) |Directory > fliken rapporter |

![Granska rapporten](./media/active-directory-view-access-usage-reports/auditReport.PNG)

### <a name="password-reset-activity"></a>Lösenordsåterställningsaktivitet
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Visar alla försök som har inträffat i din organisation för lösenordsåterställning. |Directory > fliken rapporter |

![Lösenordsåterställningsaktivitet](./media/active-directory-view-access-usage-reports/passwordResetActivity.PNG)

### <a name="password-reset-registration-activity"></a>Lösenordsåterställningsaktivitet för registrering
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Visar alla registreringar som har inträffat i din organisation för lösenordsåterställning. |Directory > fliken rapporter |

![Lösenordsåterställningsaktivitet för registrering](./media/active-directory-view-access-usage-reports/passwordResetRegistrationActivity.PNG)

### <a name="self-service-groups-activity"></a>Självbetjäningsportalen grupper aktivitet
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Visar alla aktiviteter för självbetjäning hanterade grupper i katalogen. |Directory > användare > <i>användaren</i> > fliken enheter |

![Självbetjäningsportalen grupper aktivitet](./media/active-directory-view-access-usage-reports/selfServiceGroupsActivity.PNG)

## <a name="integrated-applications-reports"></a>Integrerade program rapporter
### <a name="application-usage-summary"></a>Programanvändning: sammanfattning
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Använd den här rapporten när du vill se användning för alla SaaS-program i din katalog. Den här rapporten baseras på antalet gånger som användare har klickat på programmet i åtkomstpanelen. |Directory > fliken rapporter |

Den här rapporten innehåller logga moduler att *alla* program som din katalog har åtkomst till, inklusive förintegrerade Microsoft-program.

Förintegrerade Microsoft-program finns Office 365, Sharepoint, Azure-hanteringsportalen och andra.

![Översikt över programanvändning](./media/active-directory-view-access-usage-reports/applicationUsage.PNG)

### <a name="application-usage-detailed"></a>Programanvändning: detaljerad
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Använd den här rapporten om du vill se hur mycket ett visst SaaS-program används. Den här rapporten baseras på antalet gånger som användare har klickat på programmet i åtkomstpanelen. |Directory > fliken rapporter |

### <a name="application-dashboard"></a>Instrumentpanel för program
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Den här rapporten visar kumulativa logga moduler tillämpningen av användare i din organisation, under ett tidsintervall som är markerade. Diagrammet på instrumentpanelssidan hjälper dig att identifiera trender för alla användning av programmet. |Directory > program > fliken instrumentpanelen |

## <a name="error-reports"></a>Felrapporter
### <a name="account-provisioning-errors"></a>Kontoetableringsfel
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Används för att övervaka fel som uppstår under synkroniseringen av konton från SaaS-program till Azure Active Directory. |Directory > fliken rapporter |

![Kontoetableringsfel](./media/active-directory-view-access-usage-reports/accountProvisioningErrors.PNG)

## <a name="user-specific-reports"></a>Användarspecifika rapporter
### <a name="devices"></a>Enheter
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Använd den här rapporten om du vill visa IP-adress och geografiska placeringen av enheter som en specifik användare har använt för att få åtkomst till Azure Active Directory. |Directory > användare > <i>användaren</i> > fliken enheter |

### <a name="activity"></a>Aktivitet
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Visar inloggningssidan i aktiviteten för en användare. Rapporten innehåller information som program som har loggat in, enhet som används, IP-adress och plats. Vi samlar inte in historiken för användare som loggar in med ett Microsoft-konto. |Directory > användare > <i>användaren</i> > fliken aktivitet |

#### <a name="sign-in-events-included-in-the-user-activity-report"></a>Logga in händelser i rapporten användaraktivitet
Endast vissa typer av inloggning händelser visas i rapporten användaraktivitet.

| Händelsetyp | Med? |
| --- | --- |
| Inloggningar till den [åtkomst till Kontrollpanelen](http://myapps.microsoft.com/) |Ja |
| Inloggningar till den [Azure-hanteringsportalen](https://manage.windowsazure.com/) |Ja |
| Inloggningar till den [Microsoft Azure-portalen](https://portal.azure.com/) |Ja |
| Inloggningar till den [Office 365-portalen](http://portal.office.com/) |Ja |
| Inloggningar till en intern App som Outlook (se undantagsfelet nedan) |Ja |
| Inloggning till en federerad/etablerats via åtkomstpanelen, t.ex. Salesforce |Ja |
| Inloggning till en lösenordsbaserad app via åtkomstpanelen som Twitter |Ja |
| Inloggning till en anpassad app som har lagts till i katalogen |Ingen (kommer snart) |
| Inloggning till en Azure AD Application Proxy-app som har lagts till katalogen |Ingen (kommer snart) |

> Obs: Om du vill minska mängden brus i den här rapporten inloggningar av den [Microsoft Online Services-inloggningsassistent](http://community.office365.com/en-us/w/sso/534.aspx) visas inte.
> 
> 

## <a name="things-to-consider-if-you-suspect-security-breach"></a>Saker att tänka på om du misstänker säkerhetsintrång
Om du misstänker att ett användarkonto kan vara drabbade eller alla typer av misstänkt användaraktivitet som kan leda till att ett intrång av dina katalogdata i molnet, du kanske vill en eller flera av följande åtgärder:

* Kontakta användaren för att verifiera aktiviteten
* Återställ användarens lösenord
* [Aktivera Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md) för ytterligare säkerhet

## <a name="view-or-download-a-report"></a>Visa eller hämta en rapport
1. I den klassiska Azure-portalen klickar du på **Active Directory**, klicka på namnet på din organisations katalog och klicka sedan på **rapporter**.
2. Klicka på den rapport som du vill visa och/eller ladda ned på sidan rapporter.
   
   > [!NOTE]
   > Om du har använt funktionen reporting i Azure Active Directory visas ett meddelande om att delta i. Om du godkänner villkoren klickar du på bockmarkeringen för att fortsätta.
   > 
   > 
3. Klicka på listrutan bredvid intervall och välj sedan något av följande tidsintervall som ska användas när rapporten skapas:
   
   * Senaste 24 timmarna
   * Senaste 7 dagarna
   * Senaste 30 dagarna
4. Klicka på bockmarkeringen för att köra rapporten.
   * Upp till 1000 händelser visas i den klassiska Azure-portalen.
5. Om det är tillämpligt, klickar du på **hämta** att ladda ned rapporten till en komprimerad fil formatet fil med kommaavgränsade värden (CSV) för offline visning och arkivering.
   * Upp till 75 000 händelser tas med i den hämta filen.
   * Mer information finns i [Azure AD Reporting API](active-directory-reporting-api-getting-started.md).

## <a name="ignore-an-event"></a>Ignorera en händelse
Om du visar några rapporter för avvikelseidentifiering kan eventuellt du se att du kan ignorera olika händelser som visas i rapporter. Om du vill ignorera en händelse bara markera händelse i rapporten och klicka sedan på **Ignorera**. Den **Ignorera** tar permanent bort den markerade händelsen i rapporten och kan bara användas av licensierade globala administratörer.

## <a name="automatic-email-notifications"></a>Automatisk e-postaviseringar
Mer information om Azure AD reporting meddelanden, kan du ta en titt [Azure Active Directory Reporting meddelanden](active-directory-reporting-notifications.md).

## <a name="whats-next"></a>Nästa steg
* [Komma igång med Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Anpassa inloggnings- och åtkomstpanelsidorna till ditt företag](active-directory-add-company-branding.md)

