---
title: "aaaView åtkomst-och användningsdata | Microsoft Docs"
description: "Förklarar hur tooview åtkomst och användning rapporterar toogain insyn i hello integritet och säkerheten för din organisations katalog."
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
ms.openlocfilehash: 1c18fd2a327ae8b67f62ce2754f643bdb03514a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-your-access-and-usage-reports"></a>Visa åtkomst- och användningsrapporterna
*Den här dokumentationen är en del av hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).*

Du kan använda Azure Active Directory-åtkomst och användning rapporter toogain insyn i hello integritet och säkerhet för din organisations katalog. Med den här informationen kan fastställa en katalogadministratör bättre där möjliga säkerhetsrisker kan ligga så att de kan rätt toomitigate riskerna.

I hello Azure-hanteringsportalen kategoriseras rapporter i hello följande sätt:

* Avvikelseidentifiering rapporterar – innehåller logga in händelser som påträffades toobe avvikande. Vårt mål är toomake du känner till denna aktivitet och aktivera toobe kan toomake fastställa om en händelse är misstänkt.
* Integrerad rapporter från Application – tillhandahåller insikter om hur molnprogram används i din organisation. Azure Active Directory möjliggör integrering med tusentals molnprogram.
* Felrapporter – ange fel som kan uppstå vid etablering av konton tooexternal program.
* Användarspecifika rapporter – visa enheten/inloggning aktivitetsdata för en viss användare.
* Aktivitetsloggar – innehåller en post på alla granskade händelser inom hello sista 24 timmar, senaste 7 dagarna eller senaste 30 dagarna, samt ändringar av aktiviteten och aktiviteten för återställning och registrering av lösenord.

> [!NOTE]
> * Vissa avancerade användningsrapporter avvikelseidentifiering och resurser är bara tillgängliga när du aktiverar [Azure Active Directory Premium](active-directory-get-started-premium.md). Avancerade rapporter kan du förbättra säkerheten för åtkomst, svara toopotential hot och få åtkomst till tooanalytics på enhetsanvändning för åtkomst och program.
> * Azure Active Directory Premium och Basic är tillgängliga för kunder i Kina hello globala instansen av Azure Active Directory. Azure Active Directory Premium och Basic stöds inte för närvarande i hello Microsoft Azure-tjänsten som drivs av 21Vianet i Kina. Mer information kontaktar du oss på hello [Azure Active Directory-forumet](https://feedback.azure.com/forums/169401-azure-active-directory/).
> 
> 

## <a name="reports"></a>Rapporter
| Rapport | Beskrivning |
| --- | --- |
| **Avvikande aktivitetsrapporter** | |
| [Inloggningar från okända källor](active-directory-reporting-sign-ins-from-unknown-sources.md) |Kan indikera ett försök toosign i utan som spåras. |
| [Inloggningar efter flera fel](active-directory-reporting-sign-ins-after-multiple-failures.md) |Kan indikera en lyckad nyckelsökningsangrepp. |
| [Inloggningar från flera geografiska områden](active-directory-reporting-sign-ins-from-multiple-geographies.md) |Kan tyda på att flera användare loggar in med hello samma konto. |
| [Inloggningar från IP-adresser med misstänkt aktivitet](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md) |Kan indikera en lyckad inloggning efter en varaktig intrång försök. |
| [Inloggningar från eventuellt infekterade enheter](active-directory-reporting-sign-ins-from-possibly-infected-devices.md) |Kan indikera ett försök toosign i från potentiellt infekterade enheter. |
| [Onormal inloggningsaktivitet](active-directory-reporting-irregular-sign-in-activity.md) |Kan indikera händelser avvikande toousers' inloggning mönster. |
| [Användare med avvikande inloggningsaktivitet](active-directory-reporting-users-with-anomalous-sign-in-activity.md) |Anger användare vars konton har komprometterats. |
| Används med läckta autentiseringsuppgifter |Används med läckta autentiseringsuppgifter |
| **Aktivitetsloggar** | |
| Granska rapporten |Granskade händelser i katalogen |
| Lösenordsåterställningsaktivitet |Ger en detaljerad vy av lösenordsåterställning som sker i din organisation. |
| Lösenordsåterställningsaktivitet för registrering |Innehåller en detaljerad vy av lösenordet för återställning registreringar som sker i din organisation. |
| Självbetjäningsportalen grupper aktivitet |Innehåller en aktivitet loggen tooall gruppen självbetjäningsportalen aktivitet i din katalog |
| **Integrerade program** | |
| Programanvändning |Innehåller en sammanfattning av användning för alla SaaS-program som är integrerade med din katalog. |
| Kontot etablering aktivitet |Innehåller en historik över försök tooprovision konton tooexternal program. |
| Status för förnyelse av lösenord |En detaljerad översikt över automatisk förnyelse lösenordsstatus för SaaS-program. |
| Kontoetableringsfel |Anger en påverkan toousers åtkomst tooexternal program. |
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
<p>hello avvikande inloggning aktivitetsrapporter flaggan misstänkta inloggning aktiviteten tooOffice365, Azure-hanteringsportalen, Azure AD-åtkomstpanelen, Sharepoint Online, Dynamics CRM Online och andra Microsoft online services.</p>

<p>Alla dessa rapporter, förutom hello ”inloggningar efter flera fel” rapporten flaggan också misstänkta <i>federerad</i> logga moduler toohello ovannämnda tjänster, oavsett hello federationsleverantör. </p>

<p>hello följande rapporter är tillgängliga: </p><ul>

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
| Visar en förteckning över alla granskade händelser inom hello senaste 24 timmarna, senaste 7 dagarna eller senaste 30 dagarna. <br /> Mer information finns i [Azure Active Directory Granskningsrapporthändelser](active-directory-reporting-audit-events.md) |Directory > fliken rapporter |

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
| Visar alla aktiviteter för hello självbetjäning hanterade grupper i katalogen. |Directory > användare > <i>användaren</i> > fliken enheter |

![Självbetjäningsportalen grupper aktivitet](./media/active-directory-view-access-usage-reports/selfServiceGroupsActivity.PNG)

## <a name="integrated-applications-reports"></a>Integrerade program rapporter
### <a name="application-usage-summary"></a>Programanvändning: sammanfattning
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Använd den här rapporten när du vill toosee användning för alla hello SaaS-program i din katalog. Den här rapporten är baserad på hello antalet gånger som användare har klickat på hello programmet hello åtkomstpanelen. |Directory > fliken rapporter |

Den här rapporten innehåller inloggningar för*alla* program som din katalog har åtkomst till, inklusive förintegrerade Microsoft-program.

Förintegrerade Microsoft-program finns Office 365, Sharepoint, hello Azure-hanteringsportalen och andra.

![Översikt över programanvändning](./media/active-directory-view-access-usage-reports/applicationUsage.PNG)

### <a name="application-usage-detailed"></a>Programanvändning: detaljerad
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Använd den här rapporten om du vill toosee hur mycket ett visst SaaS-program används. Den här rapporten är baserad på hello antalet gånger som användare har klickat på hello programmet hello åtkomstpanelen. |Directory > fliken rapporter |

### <a name="application-dashboard"></a>Instrumentpanel för program
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Den här rapporten visar kumulativa logga moduler toohello program av användare i din organisation, under ett tidsintervall som är markerade. hello diagram på hello instrumentpanelssida hjälper dig att identifiera trender för alla användning av programmet. |Directory > program > fliken instrumentpanelen |

## <a name="error-reports"></a>Felrapporter
### <a name="account-provisioning-errors"></a>Kontoetableringsfel
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Använd den här toomonitor fel som uppstår under hello synkronisering av konton från SaaS-program tooAzure Active Directory. |Directory > fliken rapporter |

![Kontoetableringsfel](./media/active-directory-view-access-usage-reports/accountProvisioningErrors.PNG)

## <a name="user-specific-reports"></a>Användarspecifika rapporter
### <a name="devices"></a>Enheter
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Använd den här rapporten om du vill toosee hello IP-adress och geografiska placeringen för enheter som en specifik användare har använt tooaccess Azure Active Directory. |Directory > användare > <i>användaren</i> > fliken enheter |

### <a name="activity"></a>Aktivitet
| Beskrivning | Rapportens plats |
|:--- |:--- |
| Visar hello tecken i aktiviteten för en användare. hello rapporten innehåller information som hello programmet inloggad, enhet som används, IP-adress och plats. Vi samlar inte in hello historik för användare som loggar in med ett Microsoft-konto. |Directory > användare > <i>användaren</i> > fliken aktivitet |

#### <a name="sign-in-events-included-in-hello-user-activity-report"></a>Logga in händelser i hello användaraktivitet rapport
Endast vissa typer av inloggning händelser visas i hello användaraktivitet rapporten.

| Händelsetyp | Med? |
| --- | --- |
| Logga moduler toohello [åtkomstpanelen](http://myapps.microsoft.com/) |Ja |
| Logga moduler toohello [Azure-hanteringsportalen](https://manage.windowsazure.com/) |Ja |
| Logga moduler toohello [Microsoft Azure Portal](https://portal.azure.com/) |Ja |
| Logga moduler toohello [Office 365-portalen](http://portal.office.com/) |Ja |
| Logga moduler tooa interna program som Outlook (se undantagsfelet nedan) |Ja |
| Signera moduler tooa federerad/etablerats via hello åtkomstpanelen, t.ex. Salesforce |Ja |
| Logga moduler tooa lösenordsbaserade appen via hello åtkomstpanelen som Twitter |Ja |
| Logga in på moduler tooa anpassad app som har lagts till toohello directory |Ingen (kommer snart) |
| Signera moduler tooan Azure AD Application Proxy-app som har lagts till toohello directory |Ingen (kommer snart) |

> Obs: tooreduce hello mycket brus i den här rapporten inloggningar av hello [Microsoft Online Services-inloggningsassistent](http://community.office365.com/en-us/w/sso/534.aspx) visas inte.
> 
> 

## <a name="things-tooconsider-if-you-suspect-security-breach"></a>Saker tooconsider om du misstänker säkerhetsintrång
Om du misstänker att ett användarkonto kan vara drabbade eller alla typer av misstänkt användaraktivitet som kan orsaka tooa säkerhetsintrång av dina katalogdata i hello moln du tooconsider en eller flera av hello följande åtgärder:

* Kontakta hello användaraktivitet tooverify hello
* Återställa hello användares lösenord
* [Aktivera Multi-Factor authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md) för ytterligare säkerhet

## <a name="view-or-download-a-report"></a>Visa eller hämta en rapport
1. I hello klassiska Azure-portalen klickar du på **Active Directory**hello namnet på din organisations katalog och klicka sedan på **rapporter**.
2. På hello rapporter klickar du på hello-rapport som du vill tooview och/eller ladda ned.
   
   > [!NOTE]
   > Om detta är hello första gången du använder hello reporting-funktionen i Azure Active Directory, visas ett meddelande tooOpt i. Om du godkänner villkoren klickar du på hello markerat ikonen toocontinue.
   > 
   > 
3. Klicka på nästa tooInterval för hello nedrullningsbara menyn och välj sedan något av följande tidsintervall som ska användas när rapporten skapas hello:
   
   * Senaste 24 timmarna
   * Senaste 7 dagarna
   * Senaste 30 dagarna
4. Klicka på hello markerat ikonen toorun hello rapport.
   * Konfigurera too1000 visas händelser i hello klassiska Azure-portalen.
5. Om det är tillämpligt, klickar du på **hämta** toodownload hello tooa komprimerade rapportfilen formatet fil med kommaavgränsade värden (CSV) för offline visning och arkivering.
   * Konfigurera too75 inkluderas 000 händelser i hello ned filen.
   * Mer information finns på hello [Azure AD Reporting API](active-directory-reporting-api-getting-started.md).

## <a name="ignore-an-event"></a>Ignorera en händelse
Om du visar några rapporter för avvikelseidentifiering kan eventuellt du se att du kan ignorera olika händelser som visas i rapporter. tooignore en händelse, bara markera hello händelse i hello rapporten och klicka sedan på **Ignorera**. Hej **Ignorera** tar permanent bort hello markerade händelsen från hello rapporten och kan bara användas av licensierade globala administratörer.

## <a name="automatic-email-notifications"></a>Automatisk e-postaviseringar
Mer information om Azure AD reporting meddelanden, kan du ta en titt [Azure Active Directory Reporting meddelanden](active-directory-reporting-notifications.md).

## <a name="whats-next"></a>Nästa steg
* [Komma igång med Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Lägga till företagsanpassning tooyour inloggnings- och åtkomstpanel-sidor](active-directory-add-company-branding.md)

