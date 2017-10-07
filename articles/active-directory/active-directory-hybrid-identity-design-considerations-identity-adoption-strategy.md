---
title: "designöverväganden för aaaAzure Active Directory hybrid identity - definiera en strategi för hybrid identity införande | Microsoft Docs"
description: "Med villkorlig åtkomstkontroll kontrollerar hello särskilda villkor som du väljer när du autentiserar användaren hello och innan åtkomst toohello program i Azure Active Directory. När dessa villkor är uppfyllda, hello användare autentiseras och tillåtet åtkomst toohello program."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: b92fa5a9-c04c-4692-b495-ff64d023792c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 9ffca675d0c714392adfcbbc4dcfad12fccbac78
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="define-a-hybrid-identity-adoption-strategy"></a>Definiera en strategi för införandet en hybrid identity
I den här uppgiften definierar du hello hybrid identity införandestrategin för hybrid identity lösning toomeet hello företagets krav som beskrivs i:

* [Fastställa affärsbehov](active-directory-hybrid-identity-design-considerations-business-needs.md)
* [Ange krav för directory-synkronisering](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
* [Ange krav för multifaktorautentisering](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Definiera behov affärsstrategi
hello första uppgiften affärsbehov avgörande hello organisationer.  Detta kan vara mycket bred och scope krypning kan inträffa om du inte är försiktig.  I början av hello enkelhet men komma ihåg tooplan för en design som ska anpassas och underlätta förändringen i hello framtida.  Oavsett om det är en enkel design eller en extremt komplexa, är Azure Active Directory hello Microsoft Identity plattform som har stöd för Office 365, Microsoft Online Services och anpassade program i molnet.

## <a name="define-an-integration-strategy"></a>Definiera en strategi för integrering
Microsoft har tre huvudsakliga integrationsscenarier är molnidentiteter, synkroniserade identiteter och federerade identiteter.  Du bör planera på införandet av en av dessa strategier för integrering.  Hej strategi som du väljer kan variera och hello beslut att välja en kan innehålla vilken användarupplevelse som du vill tooprovide, har du några befintliga infrastruktur för hello redan på plats och vad som är mest kostnadseffektiva hello.  

![](./media/hybrid-id-design-considerations/integration-scenarios.png)

hello-scenarier som definierades i hello ovan bild är:

* **Molnet identiteter**: dessa är identiteter som finns endast i hello molnet.  I fallet hello Azure AD, skulle de finns i Azure AD-katalogen.
* **Synkroniserade**: dessa är identiteter som finns lokalt och i hello molnet.  Med Azure AD Connect användarna antingen skapas eller kopplas till befintlig Azure AD-konton.  hello har användarens lösenords-hash synkroniserats från hello lokala miljö toohello moln i något som kallas lösenords-hash.  När med synkroniserade är hello vara bra att om en användare har inaktiverats i hello lokala miljö, kan det ta upp too3 timmar för detta konto status tooshow i Azure AD.  Detta är på grund av toohello synkronisering tidsintervall.
* **Federerad**: dessa identiteter finns både lokalt och i hello molnet.  Med Azure AD Connect användarna antingen skapas eller kopplas till befintlig Azure AD-konton.  

> [!NOTE]
> Mer information om alternativ för synkronisering av hello [integrera dina lokala identiteter med Azure Active Directory](connect/active-directory-aadconnect.md).
> 
> 

hello i den följande tabellen hjälper dig att fastställa hello fördelarna och nackdelarna med vart och ett av följande strategier hello:

| Strategi | Fördelar | Nackdelar |
| --- | --- | --- |
| **Molnidentiteter** |Enklare toomanage för små företag. <br> Inget tooinstall på-lokal-ingen ytterligare maskinvara krävs<br>Enkelt inaktiveras om hello användaren lämnar företaget hello |Användare behöver toosign i vid åtkomst av arbetsbelastningar i hello moln <br> Lösenord kanske eller kanske inte hello samma för molnet och lokala identiteter |
| **Synkroniserade** |Lokala lösenordet autentiserar både lokalt och molnet kataloger <br>Enklare toomanage för små, medelstora eller stora organisationer <br>Användare kan ha enkel inloggning (SSO) för vissa resurser <br> Microsoft önskad metod för synkronisering <br> Enklare toomanage |Vissa kunder kan vara ovilliga toosynchronize sina kataloger med hello molnet på grund av ett visst företag polis |
| **Federerad** |Användare kan ha enkel inloggning (SSO) <br>Om en användare avslutas eller lämnar, hello konto kan inaktiveras omedelbart och behörighet återkallats<br> Har stöd för avancerade scenarier som får inte vara åstadkommas med synkroniserade |Fler steg toosetup och konfigurera <br> Högre Underhåll <br> Kan kräva ytterligare maskinvara för hello STS-infrastruktur <br> Kan kräva ytterligare maskinvara tooinstall hello federationsserver. Det krävs ytterligare programvara om AD FS används <br> Kräv omfattande inställningar för enkel inloggning <br> Kritisk felpunkt om hello federation-servern är avstängd, inte användare tooauthenticate |

### <a name="client-experience"></a>Klientupplevelse
hello-strategi som du använder styr hello inloggning användarupplevelsen.  hello följande tabeller innehåller information om vad hello-användare kan förvänta sina uppstår toobe.  Observera att inte alla federerade identitetsleverantörer stöder enkel inloggning i samtliga scenarier.

**Ansluten till domänen och privata nätverksprogram**:

|  | Synkroniserade identiteter | Federerade identiteter |
| --- | --- | --- |
| Webbläsare |Formulärbaserad autentisering |enkel inloggning krävs på, ibland toosupply organisations-ID |
| Outlook |Fråga efter autentiseringsuppgifter |Fråga efter autentiseringsuppgifter |
| Skype för företag (Lync) |Fråga efter autentiseringsuppgifter |enkel inloggning på för Lync, ange autentiseringsuppgifter för Exchange |
| SkyDrive Pro |Fråga efter autentiseringsuppgifter |enkel inloggning |
| Office Pro Plus prenumeration |Fråga efter autentiseringsuppgifter |enkel inloggning |

**Externa eller ej betrodda källor**:

|  | Synkroniserade identiteter | Federerade identiteter |
| --- | --- | --- |
| Webbläsare |Formulärbaserad autentisering |Formulärbaserad autentisering |
| Outlook, Skype för företag (Lync) Skydrive Pro, Office-prenumeration |Fråga efter autentiseringsuppgifter |Fråga efter autentiseringsuppgifter |
| Exchange ActiveSync |Fråga efter autentiseringsuppgifter |enkel inloggning på för Lync, ange autentiseringsuppgifter för Exchange |
| Mobilappar |Fråga efter autentiseringsuppgifter |Fråga efter autentiseringsuppgifter |

Om du har konstaterat från aktivitet 1 att du har en 3 part IdP eller är pågående toouse en tooprovide federation med Azure AD, behöver du toobe medveten om följande hello stöds funktioner:

* SAML 2.0-providern som är godkända för hello SP-Lite profil stöder autentisering tooAzure AD och associerade program
* Har stöd för passiv autentisering, vilket underlättar auth tooOWA, SPO, osv.
* Exchange Online-klienter kan användas via hello SAML 2.0 förbättrad klienten profil (ECP)

Du måste också vara medveten om vilka funktioner är inte tillgängliga:

* Aktiva klienter bryts utan stöd för WS-Trust/Federation
  * Det innebär att inga Lync-klienten, OneDrive-klient, Office-prenumeration, Office Mobile tidigare tooOffice 2016
* Övergången av Office toopassive autentisering ger toosupport ren SAML 2.0 IdPs, men stöd kommer fortfarande att klienten av klient

> [!NOTE]
> Senaste uppdaterade listan finns hello hello artikel http://aka.ms/ssoproviders.
> 
> 

## <a name="define-synchronization-strategy"></a>Definiera en strategi för synkronisering
I den här uppgiften definierar du hello verktyg som kommer att använda toosynchronize hello organisationens lokala data toohello molnet och vad du bör använda topologi.  Eftersom de flesta organisationer använder Active Directory, har information om hur du använder Azure AD Connect tooaddress hello frågor ovan angetts i viss detalj.  Det finns information om hur du använder FIM 2010 R2 eller toohelp MIM 2016 planerar den här strategin för miljöer som inte har Active Directory.  Dock framtida versioner av Azure AD Connect har stöd för LDAP-kataloger, så beroende på tidslinjen, den här informationen kan vara kan tooassist.

### <a name="synchronization-tools"></a>Synkroniseringsverktyg
Hello års har flera synkroniseringsverktyg fanns och används för olika scenarier.  Azure AD Connect är för närvarande hello gå tootool för alla scenarier som stöds.  AAD Sync och DirSync är också fortfarande runt och kan även finnas i din miljö nu. 

> [!NOTE]
> Hello senaste information om funktioner för hello stöds av varje verktyg finns [katalogintegreringsverktyg](active-directory-hybrid-identity-design-considerations-tools-comparison.md) artikel.  
> 
> 

### <a name="supported-topologies"></a>Topologier som stöds
När du definierar en strategi för synkronisering måste hello-topologi som används bestämmas. Beroende på hello är information som fastställdes i steg 2 kan du bestämma vilken topologi hello rätt en toouse. hello enkel skog, enkel topologi för Azure AD är de vanligaste hello och består av en Active Directory-skog och en enda instans av Azure AD.  Det här händer toobe som används i en majoritet av hello scenarier och hello förväntades topologi när du använder Azure AD Connect Snabbinstallation enligt hello bilden nedan.

![](./media/hybrid-id-design-considerations/single-forest.png)Enkel skog scenariot det är väldigt vanligt för små och även stora organisationer toohave flera skogar, som visas i bild 5.

> [!NOTE]
> Mer information om hello olika lokalt och Azure AD-topologier med Azure AD Connect-synkronisering hello artikeln [topologier för Azure AD Connect](connect/active-directory-aadconnect-topologies.md).
> 
> 

![](./media/hybrid-id-design-considerations/multi-forest.png) 

Scenario med flera skogar

Om det här fallet hello hello sedan flera forest enda bör Azure AD-topologi övervägas om hello följande är sant:

* Användare har bara 1 identitet i alla skogar – hello som unikt identifierar användare nedan beskriver detta i detalj.
* hello användaren autentiseras toohello skogen som sin identitet finns
* Källfästpunkten (ändras id) och UPN hämtas från den här skogen
* Alla skogar som är tillgängliga för Azure AD Connect – detta innebär att den behöver inte toobe domänanslutna och kan placeras i en DMZ om detta förenklar detta.
* Användare har en postlåda
* hello-skogen som är värd för en användares postlåda har hello bästa kvalitet för attribut som visas i hello Exchange globala adresslistan (GAL)
* Om det inte finns någon postlåda på hello användaren och alla skogar kan vara används toocontribute dessa värden
* Om du har en länkad postlåda och det finns också ett annat konto i en annan skog används toosign i.

> [!NOTE]
> Objekt som finns i både lokalt och i molnet hello är ”ansluten” via en unik identifierare. Det här unika identifierare finns hello gäller katalogsynkronisering är refererad tooas hello SourceAnchor. I hello sammanhang för enkel inloggning är refererad tooas hello ImmutableId. [Designbegreppen för Azure AD Connect](connect/active-directory-aadconnect-design-concepts.md#sourceanchor) för mer information om hello användning av SourceAnchor.
> 
> 

Om hello ovan inte är uppfyllda och du har mer än en aktiv konto eller mer än en postlåda, Välj en Azure AD Connect och ignorera hello andra.  Om du har länkat postlådor, men inga andra konto, dessa konton inte exporterade tooAzure AD och användaren är inte en medlem av en grupp.  Detta skiljer sig från hur den var i hello tidigare med DirSync och stöds avsiktlig toobetter dessa scenarier med flera skogar. Ett scenario med flera skogar visas i hello bilden nedan.

![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 

**Flera skogar flera Azure AD-scenarier**

Det rekommenderas toohave som en enda katalog i Azure AD för en organisation, men det är det sparas en 1:1-relation mellan en Azure AD Connect sync-server och Azure AD-katalog stöds.  För varje instans av Azure AD behöver du en installation av Azure AD Connect.  Dessutom Azure AD avsiktligt är isolerad och kan toosee användare i en annan instans kan inte användare i en instans av Azure AD.

Det är möjligt och stöds tooconnect en lokal instans av Active Directory toomultiple Azure AD-kataloger som visas i hello bilden nedan:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 

**En skog filtrering scenario**

I ordning toodo måste hello följande vara sant:

* Azure AD Connect sync-servrar måste konfigureras för att filtrera så att de har ett ömsesidigt uteslutande uppsättning objekt.  Detta har gjort, till exempel av omfång varje server tooa viss domän eller Organisationsenhet.
* En DNS-domän kan bara registreras i en enda Azure AD-katalog så hello UPN hello användare i hello lokala AD måste använda separata namnområden
* Användare i en instans av Azure AD kommer bara att kunna toosee användare från deras instans.  De kommer inte att kunna toosee användare i hello andra instanser
* Endast en av hello Azure AD-kataloger kan du aktivera Exchange hybrid med hello lokala AD
* Ömsesidig ensamrätt gäller även toowrite igen.  Det gör att vissa återskrivning funktioner stöds inte med den här topologin eftersom dessa förutsätter en enda lokal konfiguration.  Detta omfattar:
  * Gruppera återskrivning med standardkonfiguration
  * Tillbakaskrivning av enhet

Tänk på att följande hello stöds inte och bör inte väljas som en implementering:

* Det är inte stöds toohave flera servrar för Azure AD Connect sync ansluter toohello samma Azure AD directory även om de är konfigurerade toosynchronize ömsesidigt uteslutande uppsättning objekt
* Det finns inte stöd för toosync hello samma användare toomultiple Azure AD-kataloger. 
* Det är också stöds inte toomake en konfigurationsändring toomake användare i en Azure AD tooappear som kontakter i en annan Azure AD-katalog. 
* Det är också stöds inte toomodify Azure AD Connect sync tooconnect toomultiple Azure AD-kataloger.
* Azure AD-kataloger är avsiktligt isolerat. Det är som inte stöds toochange hello konfigurationen av Azure AD Connect sync tooread data från en annan Azure AD-katalog i ett försök toobuild en gemensam och enhetlig GAL mellan hello kataloger. Det är också stöds inte tooexport användare som kontaktar tooanother lokala AD med hjälp av Azure AD Connect-synkronisering.

> [!NOTE]
> Om din organisation begränsar datorer i nätverket från att ansluta toohello Internet, den här artikeln innehåller hello slutpunkter (FQDN: er, IPv4 och IPv6-adressintervall) som ska inkluderas i din utgående Tillåt listor och Internet Explorer zonen Betrodda platser för klienten datorer tooensure datorerna kan använda Office 365. Mer information finns [Office 365-URL: er och IP-adressintervall](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).
> 
> 

## <a name="define-multi-factor-authentication-strategy"></a>Definiera en strategi för multifaktorautentisering
I den här uppgiften definierar du hello multifaktorautentisering strategi toouse.  Azure Multi-Factor Authentication finns i två olika versioner.  En är en molnbaserad och hello andra är lokalt baserad med hello Azure MFA-Server.  Utifrån hello utvärdering du ovan du kan bestämma vilken lösning som är hello din strategi för stämmer överens.  Använd hello tabellen nedan toodetermine vilket designalternativ för som bäst uppfyller företagets säkerhetskrav:

Multi-Factor designalternativen:

| Tillgångsinformation toosecure | MFA i molnet hello | MFA lokalt |
| --- | --- | --- |
| Microsoft-appar |Ja |Ja |
| SaaS-appar i hello app-galleriet |Ja |Ja |
| IIS-program publicerade via Azure AD App Proxy |Ja |Ja |
| IIS-program som inte publicerats via hello Azure AD App-Proxy |Nej |Ja |
| Fjärråtkomst som VPN, Fjärrskrivbordsgateway |Nej |Ja |

Trots att du kan ha regleras på en lösning för din strategi, måste du ändå toouse hello utvärdering ovan på var användarna finns.  Detta kan orsaka hello lösning toochange.  Använd hello tabellen nedan tooassist du fastställa detta:

| Användarplats | Alternativet föredragna designen |
| --- | --- |
| Azure Active Directory |Flera FactorAuthentication i hello moln |
| Azure AD och lokalt AD med federation med AD FS |Båda |
| Azure AD och lokala AD med Azure AD Connect inga Lösenordssynkronisering |Båda |
| Azure AD och lokala med Azure AD Connect med Lösenordssynkronisering |Båda |
| Lokala AD |Multi-Factor Authentication Server |

> [!NOTE]
> Du bör också se till att hello multifaktorautentisering designalternativ som du har valt stöder hello-funktioner som krävs för din design.  Mer information finns [väljer hello Multi-Factor säkerhetslösning för du](../multi-factor-authentication/multi-factor-authentication-get-started.md#what-am-i-trying-to-secure).
> 
> 

## <a name="multi-factor-auth-provider"></a>Leverantör av Multifaktorautent
Multifaktorautentisering är som standard för globala administratörer som har en Azure Active Directory-klient. Men om du vill tooextend multifaktorautentisering tooall användare och/eller vill tooyour globala administratörer toobe kan tootake nytta funktioner, till exempel hello-hanteringsportalen, anpassade helg och rapporter, måste sedan du köpa och konfigurera Multi-Factor Authentication Provider.

> [!NOTE]
> Du bör också se till att hello multifaktorautentisering designalternativ som du har valt stöder hello-funktioner som krävs för din design. 
> 
> 

## <a name="next-steps"></a>Nästa steg
[Fastställa kraven på dataskydd](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Se även
[Översikt över design-överväganden](active-directory-hybrid-identity-design-considerations-overview.md)

