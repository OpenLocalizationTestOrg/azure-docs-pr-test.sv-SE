---
title: aaaUsing Azure AD Connect Health med AD FS | Microsoft Docs
description: "Det här är hello Azure AD Connect Health sida hur toomonitor dina lokala AD FS-infrastruktur."
services: active-directory
documentationcenter: 
author: karavar
manager: femila
editor: curtand
ms.assetid: dc0e53d8-403e-462a-9543-164eaa7dd8b3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0cd26e8762be65e09d22e1f113e5165c4f131715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-ad-fs-using-azure-ad-connect-health"></a>Övervaka AD FS med Azure AD Connect Health
Hej följande dokumentation är specifik toomonitoring AD FS-infrastrukturen med Azure AD Connect Health. Mer information om övervakning av Azure AD Connect (Sync) med Azure AD Connect Health finns i [Använda Azure AD Connect Health för synkronisering](active-directory-aadconnect-health-sync.md). Mer information om övervakning av Active Directory Domain Services med Azure AD Connect Health finns i [Använda Azure AD Connect Health med AD DS](active-directory-aadconnect-health-adds.md).

## <a name="alerts-for-ad-fs"></a>AD FS-aviseringar
hello Azure AD Connect Health-aviseringar avsnittet ger du hello lista över aktiva aviseringar. Varje avisering innehåller relevant information, Lösningssteg och länkar toorelated dokumentation.

Du kan dubbelklicka på ett aktivt eller har lösts aviseringen tooopen ett nytt blad med ytterligare information, vad du kan göra tooresolve hello aviseringen och länkar toorelevant dokumentation. Du kan också visa historiska data om aviseringar som har lösts i hello tidigare.

![Azure AD Connect Health-portalen](./media/active-directory-aadconnect-health/alert2.png)

## <a name="usage-analytics-for-ad-fs"></a>Användningsanalys för AD FS
Azure AD Connect Health-användningsanalys analyserar hello autentiseringstrafiken på dina federationsservrar. Du kan dubbelklicka på hello kryssrutan, tooopen hello användning bladet för användningsanalys, där du ser flera mått och grupperingar.

> [!NOTE]
> toouse användningsanalys med AD FS måste du kontrollera att AD FS-granskning är aktiverat. Mer information finns i [Aktivera granskning för AD FS](active-directory-aadconnect-health-agent-install.md#enable-auditing-for-ad-fs).
>
>

![Azure AD Connect Health-portalen](./media/active-directory-aadconnect-health/report1.png)

tooselect fler mått, ange ett tidsintervall eller toochange hello gruppering, högerklicka på hello användningsanalysen och väljer Redigera diagram. Du kan sedan ange hello tidsintervall, Välj ett annat mått och ändra hello gruppering. Du kan visa hello distribution av hello autentiseringstrafik baserat på olika ”mått” och gruppera varje mått med relevanta ”Gruppera efter” parametrar som beskrivs i följande avsnitt hello:

**Mått: Totalt antal förfrågningar**: Totalt antal förfrågningar som bearbetas av AD FS-servrarna.

|Gruppera efter | Vilka hello grupperingen betyder och varför den är användbar? |
| --- | --- |
| Alla | Visar hello antal Totalt antal begäranden som bearbetas av alla AD FS-servrarna.|
| Program | Grupper hello förfrågningarna baserat på mål hello förlitande part. Den här grupperingen är användbar toounderstand vilka program som tar emot viss procentandel av totalt antal hello-trafik. |
|  Server |Grupper hello förfrågningarna baserat på hello-servern som bearbetade hello-begäran. Den här grupperingen är användbar toounderstand hello belastningsfördelningen av hello totala trafiken.
| Anslut till arbetsplats |Grupper hello antalet förfrågningar baserat på om de kommer från enheter som är ansluten till arbetsplatsen (kända). Den här grupperingen är användbar toounderstand om dina resurser används med enheter som är okänd toohello identitetsinfrastrukturen. |
|  Autentiseringsmetod | Grupper hello förfrågningarna baserat på hello autentiseringsmetod som används för autentisering. Den här grupperingen är användbar toounderstand hello gemensamma autentiseringsmetoden som används för autentisering. Följande är möjliga autentiseringsmetoder för hello <ol> <li>Windows-integrerad autentisering (Windows)</li> <li>Formulärbaserad autentisering (formulär)</li> <li>Enkel inloggning (SSO, Single Sign On)</li> <li>X509-certifikatautentisering (certifikat)</li> <br>Om hello federationsservrar får hello begäran med en SSO-Cookie, räknas begäran som SSO (Single Sign On). I sådana fall om hello cookien är giltig, hello användaren uppmanas inte tooprovide autentiseringsuppgifter och får sömlös åtkomst toohello program. Detta är vanligt om du har flera förlitande parter som skyddas av hello federationsservrar. |
| Nätverksplats | Grupper hello förfrågningarna baserat på hello nätverksplats hello användare. Platsen kan vara antingen intranät eller extranät. Den här grupperingen är användbar tooknow vilken procentandel av hello trafiken kommer från hello intranät och extranät. |


**Mått: Totalt antal kunde inte begära** -hello Totalt antal misslyckade begäranden som bearbetas av federationstjänsten hello. (Det här måttet är bara tillgängligt på AD FS för Windows Server 2012 R2)

|Gruppera efter | Vilka hello grupperingen betyder och varför den är användbar? |
| --- | --- |
| Feltyp | Visar hello antalet fel baserat på fördefinierade feltyper. Den här grupperingen är användbar toounderstand hello vanliga typer av fel. <ul><li>Felaktigt användarnamn eller lösenord: fel på grund av tooincorrect användarnamn eller lösenord.</li> <li>”Utelåsning”: fel på grund av toohello förfrågningarna togs emot från en användare som var utelåst från extranätet </li><li> ”Lösenordet har upphört att gälla”: fel på grund av toousers loggat in med ett utgånget lösenord.</li><li>”Inaktiverat konto”: fel på grund av toousers loggning med ett inaktiverat konto.</li><li>”Enhetsautentisering”: fel på grund av toousers misslyckas tooauthenticate med Enhetsautentisering.</li><li>”Autentisering med användarcertifikat”: fel på grund av tooauthenticate toousers misslyckas på grund av ett ogiltigt certifikat.</li><li>”MFA”: fel på grund av toouser misslyckas tooauthenticate med hjälp av Multifaktorautentisering.</li><li>”Andra autentiseringsuppgifter”: ”Utfärdandeauktorisering”: fel på grund av tooauthorization fel.</li><li>”Utfärdandedelegering”: fel på grund av tooissuance delegering fel.</li><li>”Tokengodkännande”: fel på grund av tooADFS avvisa hello-token från en tredjeparts identitetsprovider.</li><li>”Protokoll”: fel på grund av tooprotocol fel.</li><li>”Okänt”: Samla in allt. Andra fel som inte passar i hello definierats kategorier.</li> |
| Server | Grupper hello felen baserat på hello-servern. Den här grupperingen är användbar toounderstand hello felfördelningen mellan servrar. En ojämn fördelning kan vara ett tecken på att en server har felaktigt tillstånd. |
| Nätverksplats | Grupper hello felen baserat på hello nätverksplats hello-begäranden (intranät eller extranät). Den här grupperingen är användbar toounderstand hello förfrågningar som misslyckas. |
|  Program | Grupper hello felen baserat på mål hello program (förlitande part). Den här grupperingen är användbar toounderstand vilket program ser flest antal fel. |

**Mått: Antal användare** – Det genomsnittliga antalet unika användare som aktivt autentiserar med AD FS

|Gruppera efter | Vilka hello grupperingen betyder och varför den är användbar? |
| --- | --- |
|Alla |Mätvärdet visar antalet Genomsnittligt antal användare som använder hello federationstjänsten i hello valda tidsintervallet. hello användarna grupperas inte. <br>hello medelvärde beror på hello valda tidsintervallet. |
| Program |Grupper hello Genomsnittligt antal användare baserat på hello riktad program (förlitande part). Den här grupperingen är användbar toounderstand hur många användare som använder olika program. |

## <a name="performance-monitoring-for-ad-fs"></a>Prestandaövervakning för AD FS
Azure AD Connect Health-prestandaövervakning tillhandahåller övervakningsinformation för mått. Kryssrutan hello övervakning öppnas ett nytt blad med detaljerad information om hello mått.

![Azure AD Connect Health-portalen](./media/active-directory-aadconnect-health/perf1.png)

Genom att välja hello filteralternativet hello överst på bladet hello kan filtrera du efter server toosee mått för en enskild server. toochange mått, högerklicka på hello övervakning diagrammet under hello övervakning bladet och väljer Redigera diagram (eller hello väljer Redigera diagram knapp). Från hello nya bladet som öppnas du välja ytterligare mått hello listrutan och ange ett tidsintervall för att visa hello prestandadata.

## <a name="reports-for-ad-fs"></a>Rapporter för AD FS
Azure AD Connect Health tillhandahåller rapporter om aktivitet och prestanda för AD FS. Dessa rapporter hjälper administratörer att få insyn i aktiviteter på deras AD FS-servrar.

### <a name="top-50-users-with-failed-usernamepassword-logins"></a>De 50 användarna med flest misslyckade inloggningar med användarnamn/lösenord
En av hello vanliga orsaker till en misslyckad autentiseringsbegäran på en AD FS-server är en begäran med ogiltiga autentiseringsuppgifter, som är ett felaktigt användarnamn eller lösenord. Vanligtvis sker toousers på grund av toocomplex lösenord, bortglömda lösenord eller skrivfel.

Men det finns andra orsaker som kan resultera i ett oväntat antal förfrågningar som hanteras av AD FS-servrarna, till exempel: ett program cachelagrar autentiseringsuppgifter för användare och hello autentiseringsuppgifterna upphör att gälla eller en obehörig användare försöker toosign på ett konto med en serie välkända lösenord. De här två exemplen finns giltiga skäl som kan leda tooa ökning i begäranden.

Azure AD Connect Health för AD FS tillhandahåller en rapport över övre 50 användarna med flest misslyckade inloggningsförsök på grund av tooinvalid användarnamn eller lösenord. Den här rapporten uppnås genom att behandla hello granskningshändelserna som genereras av alla hello AD FS-servrar i hello servergrupper

![Azure AD Connect Health-portalen](./media/active-directory-aadconnect-health-adfs/report1a.png)

I den här rapporten har enkel åtkomst toohello följande uppgifter:

* Totalt antal misslyckade begäranden med fel användarnamn/lösenord i hello senaste 30 dagarna
* Genomsnittligt antal användare som misslyckades med inloggningen på grund av ett felaktigt användarnamn/lösenord varje dag.

Klicka på den här delen kommer du toohello huvudrapporten bladet som innehåller ytterligare information. Det här bladet innehåller ett diagram med trender information toohelp upprätta en baslinje för begäranden med felaktiga användarnamn eller lösenord. Dessutom innehåller den hello lista över övre 50 användare med hello antal misslyckade försök.

hello diagrammet innehåller hello följande information:

* Hej Totalt antal misslyckade inloggningar på grund av tooa felaktigt användarnamn/lösenord per dag.
* Hej Totalt antal unika användare med misslyckade inloggningar per dag.
* Klientens IP-adress för senaste begäran

![Azure AD Connect Health-portalen](./media/active-directory-aadconnect-health-adfs/report3a.png)

hello rapporten innehåller hello följande information:

| Rapportobjekt | Beskrivning |
| --- | --- |
| Användar-ID |Visar hello användar-ID som användes. Det här värdet är vad hello användaren angav, som i vissa fall är hello felaktiga användar-ID som används. |
| Misslyckade försök |Visar hello Totalt antal misslyckade försök för den specifika användar-ID. hello tabellen sorteras med hello mest antal misslyckade försök i fallande ordning. |
| Senaste fel |Visar hello tidsstämpel när hello senaste felet uppstod. |
| IP-adress för senaste fel |Visar hello klient-IP-adress från hello senaste felaktig begäran. |

> [!NOTE]
> Den här rapporten uppdateras automatiskt varannan timme med hello ny information som samlats in inom den tiden. Därför kanske inloggningsförsök inom hello senaste två timmarna inte inkluderas i hello rapporten.
>
>

## <a name="related-links"></a>Relaterade länkar
* [Azure AD Connect Health](active-directory-aadconnect-health.md)
* [Installation av Azure AD Connect Health Agent](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-åtgärder](active-directory-aadconnect-health-operations.md)
* [Använda Azure AD Connect Health för synkronisering](active-directory-aadconnect-health-sync.md)
* [Använda Azure AD Connect Health med AD DS](active-directory-aadconnect-health-adds.md)
* [Vanliga frågor och svar om Azure AD Connect Health](active-directory-aadconnect-health-faq.md)
* [Versionshistorik för Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)