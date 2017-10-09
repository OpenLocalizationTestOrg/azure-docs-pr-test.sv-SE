---
title: Anslut Active Directory med Azure Active Directory. | Microsoft Docs
description: "Azure AD Connect integrerar dina lokala kataloger med Azure Active Directory. Detta gör att du tooprovide en gemensam identitet för Office 365 och Azure SaaS-program som är integrerade med Azure AD."
keywords: "Introduktion tooAzure AD Connect, Azure AD Connect översikt vad är Azure AD Connect, installera active directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: e49f2af4b67e9ed3ad093888541da7c82af0e052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-on-premises-directories-with-azure-active-directory"></a>Integrerar dina lokala kataloger med Azure Active Directory
Azure AD Connect integrerar dina lokala kataloger med Azure Active Directory. Detta gör att du tooprovide en gemensam identitet för dina användare för Office 365 och Azure SaaS-program som är integrerade med Azure AD. Det här avsnittet får du stegvisa hello planering, distribution och användningsstegen. Det är en samling länkar toohello avsnitt relaterade toothis område.

> [!IMPORTANT]
> [Azure AD Connect är hello bästa sätt tooconnect din lokala katalog med Azure AD och Office 365. Det här är en bra tidpunkt tooupgrade tooAzure AD Connect från Windows Azure Active Directory Sync (DirSync) eller Azure AD Sync eftersom dessa verktyg är nu föråldrade och slutet av stödet för 13 April 2017.](active-directory-aadconnect-dirsync-deprecated.md)
> 
> 

![Vad är Azure AD Connect?](media/active-directory-aadconnect/arch.png)

## <a name="why-use-azure-ad-connect"></a>Varför ska jag använda Azure AD Connect?
Om du integrerar dina lokala kataloger med Azure AD kan du hjälpa dina användare att bli mer produktiva genom att tillhandahålla en gemensam identitet för åtkomst både till molnet och lokala resurser. Användare och företag kan dra nytta av hello följande:

* Använda en enda identitet tooaccess lokala program du kan och molntjänster som Office 365.
* Enkel verktyget tooprovide en enkel distributionsupplevelse för synkronisering och inloggning.
* Ger hello senaste funktionerna för dina scenarier. Azure AD Connect ersätter äldre versioner av identitetsintegrationsverktyg som DirSync och Azure AD Sync. Mer information finns i [Jämförelse av katalogintegreringsverktyg för hybrididentitet](../active-directory-hybrid-identity-design-considerations-tools-comparison.md).

### <a name="how-azure-ad-connect-works"></a>Hur Azure AD Connect fungerar
Azure Active Directory Connect består av tre huvudkomponenter: hello synkroniseringstjänster, Active Directory Federation Services hello tillvalskomponenter och hello övervakningskomponenten [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md) .

<center>![Azure AD Connect Stack](./media/active-directory-aadconnect-how-it-works/AADConnectStack2.png)
</center>

* Synkronisering – Den här komponenten är ansvarig för att skapa användare, grupper och andra objekt. Det är också ansvarig för att kontrollera identitetsinformationen för dina lokala användare och grupper matchar hello molnet.
* AD FS - Federation är en valfri del av Azure AD Connect och kan vara används tooconfigure en hybridmiljö med hjälp av en lokal AD FS-infrastruktur. Detta kan användas av organisationer tooaddress komplexa distributioner, till exempel domänanslutning, verkställandet av AD-inloggning princip och smartkort eller 3 part MFA.
* Hälsoövervakning – Azure AD Connect Health kan tillhandahålla robust övervakning och ger en central plats i hello Azure portal tooview den här aktiviteten. Mer information finns i [Azure Active Directory Connect Health](../connect-health/active-directory-aadconnect-health.md).

## <a name="install-azure-ad-connect"></a>Installera Azure AD Connect
Du hittar hello download för Azure AD Connect på [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=615771).

| Lösning | Scenario |
| --- | --- |
| Innan du börjar – [Maskinvara och krav](active-directory-aadconnect-prerequisites.md) |<li>Steg toocomplete innan du börjar tooinstall Azure AD Connect.</li> |
| [Standardinställningar](active-directory-aadconnect-get-started-express.md) |<li>Om du har en enda skog AD sedan detta bör hello alternativet toouse.</li> <li>Användaren loggar in med hello samma lösenord med hjälp av Lösenordssynkronisering.</li> |
| [Anpassade inställningar](active-directory-aadconnect-get-started-custom.md) |<li>Används när du har flera skogar. Har stöd för många lokala [topologier](active-directory-aadconnect-topologies.md).</li> <li>Anpassa ditt inloggningsalternativ, t.ex. AD FS för federation eller använd en identitetsprovider från en tredje part.</li> <li>Anpassa synkroniseringsfunktioner, t.ex. filtrering och tillbakaskrivning.</li> |
| [Uppgradera från DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) |<li>Används när du har en befintlig DirSync-server som redan körs.</li> |
| [Uppgradera från Azure AD Sync eller Azure AD Connect](active-directory-aadconnect-upgrade-previous-version.md) |<li>Det finns flera olika metoder beroende på dina preferenser.</li> |

[Efter installationen](active-directory-aadconnect-whats-next.md) bör du kontrollera den fungerar som förväntat och tilldela licenser toohello användare.

### <a name="next-steps-tooinstall-azure-ad-connect"></a>Nästa steg tooInstall Azure AD Connect
|Avsnitt |Länk|  
| --- | --- |
|Ladda ned Azure AD Connect | [Ladda ned Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771)|
|Installera med standardinställningar | [Snabbinstallation av Azure AD Connect](./active-directory-aadconnect-get-started-express.md)|
|Installera med anpassade inställningar | [Anpassad installation av Azure AD Connect](./active-directory-aadconnect-get-started-custom.md)|
|Uppgradera från DirSync | [Uppgradera från Azure AD-synkroniseringsverktyg (DirSync)](./active-directory-aadconnect-dirsync-upgrade-get-started.md)|
|Efter installationen | [Kontrollera hello installationen och tilldela licenser](active-directory-aadconnect-whats-next.md)|

### <a name="learn-more-about-install-azure-ad-connect"></a>Mer information om installationen av Azure AD Connect
Du bör också tooprepare för [operativa](active-directory-aadconnectsync-operations.md) frågor. Du kanske vill toohave vänteläge server så att du enkelt kan växla om det finns en [katastrofåterställning](active-directory-aadconnectsync-operations.md#disaster-recovery). Om du planerar toomake ofta konfigurationsändringar bör du planera för en [mellanlagringsläge](active-directory-aadconnectsync-operations.md#staging-mode) server.

|Avsnitt |Länk|  
| --- | --- |
|Topologier som stöds | [Topologier för Azure AD Connect](active-directory-aadconnect-topologies.md)|
|Designbegrepp | [Designbegrepp för Azure AD Connect](active-directory-aadconnect-design-concepts.md)|
|Konton som används för installation | [Mer information om Azure AD Connect-autentiseringsuppgifter och -behörigheter](./active-directory-aadconnect-accounts-permissions.md)|
|Driftplanering | [Azure AD Connect-synkronisering: Driftåtgärder och saker att tänka på](active-directory-aadconnectsync-operations.md)|
|Alternativ för användarinloggning | [Alternativ för användarinloggning i Azure AD Connect](active-directory-aadconnect-user-signin.md)|

## <a name="configure-sync-features"></a>Konfigurera synkroniseringsfunktioner
Azure AD Connect har flera funktioner som du kan aktivera om du vill eller som är aktiverade som standard. Vissa funktioner kan ibland kräva ytterligare konfiguration i vissa scenarier och topologier.

[Filtrering](active-directory-aadconnectsync-configure-filtering.md) används när du vill toolimit vilka objekt som är synkroniserade tooAzure AD. Som standard synkroniseras alla användare, kontakter, grupper och Windows 10-datorer. Du kan ändra hello filtrering baserat på domäner, organisationsenheter eller attribut.

[Lösenordssynkronisering](active-directory-aadconnectsync-implement-password-synchronization.md) synkroniserar hello lösenordshashen i Active Directory tooAzure AD. hello slutanvändare kan använda hello samma lösenord lokalt och i molnet hello men endast hantera den på en plats. Du kan också använda en egen lösenordsprincip eftersom den använder din lokala Active Directory som hello utfärdare.

[Tillbakaskrivning av lösenord](../active-directory-passwords-getting-started.md) Tillåt toochange din användare och återställa sina lösenord i hello molnet och har din lokala lösenordsprincip som tillämpas.

[Tillbakaskrivning av enheter](active-directory-aadconnect-feature-device-writeback.md) kan en enhet som har registrerats i Azure AD toobe skrivs tillbaka tooon lokala Active Directory så att den kan användas för villkorlig åtkomst.

Hej [förhindra oavsiktliga borttagningar](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) funktionen är aktiverad som standard och skyddar din molnkatalog mot flera samtidiga borttagningar hello samtidigt. Som standard tillåts 500 borttagningar per körning. Du kan ändra den här inställningen beroende på storleken på din organisation.

[Automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md) är aktiverad som standard för installationer med standardinställningar och säkerställer Azure AD Connect är alltid in toodate med hello senaste versionen.

### <a name="next-steps-tooconfigure-sync-features"></a>Nästa steg tooconfigure sync-funktioner
|Avsnitt |Länk|  
| --- | --- |
|Konfigurera filtrering | [Azure AD Connect-synkronisering: Konfigurera filtrering](active-directory-aadconnectsync-configure-filtering.md)|
|Lösenordssynkronisering | [Azure AD Connect-synkronisering: Implementera lösenordssynkronisering](active-directory-aadconnectsync-implement-password-synchronization.md)|
|Tillbakaskrivning av lösenord | [Komma igång med lösenordshantering](../active-directory-passwords-getting-started.md)|
|Tillbakaskrivning av enheter | [Aktivera tillbakaskrivning av enheter i Azure AD Connect](active-directory-aadconnect-feature-device-writeback.md)|
|Förhindra oavsiktliga borttagningar | [Azure AD Connect-synkronisering: Förhindra oavsiktliga borttagningar](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)|
|Automatisk uppgradering | [Azure AD Connect: Automatisk uppgradering](active-directory-aadconnect-feature-automatic-upgrade.md)|

## <a name="customize-azure-ad-connect-sync"></a>Anpassa synkroniseringen av Azure AD Connect
Azure AD Connect-synkronisering levereras med en standardkonfiguration som är avsedda toowork för de flesta kunder och topologier. Men det finns alltid situationer där hello standardkonfigurationen inte fungerar och måste justeras. Det är stöds toomake ändringarna som beskrivs i det här avsnittet och i länkade avsnitt.

Om du inte har arbetat med en synkroniseringstopologi innan du vill toostart toounderstand hello grunderna och hello termer som används som beskrivs i hello [tekniska begrepp](active-directory-aadconnectsync-technical-concepts.md). Azure AD Connect är hello utveckling av MIIS2003, ILM2007 och FIM2010. Även om vissa saker är identiska, har mycket ändrats.

Hej [standardkonfigurationen](active-directory-aadconnectsync-understanding-default-configuration.md) förutsätter att det kan finnas mer än en skog i hello konfiguration. I dessa topologier kan ett användarobjekt representeras som en kontakt i en annan skog. hello-användare kan också ha en länkad postlåda i en annan resursskog. hello funktionssätt hello standardkonfigurationen beskrivs i [användare och kontakter](active-directory-aadconnectsync-understanding-users-and-contacts.md).

Hej konfigurationsmodellen i synkroniseringsverktyget kallas [deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md). hello avancerade attributflödena använder [funktioner](active-directory-aadconnectsync-functions-reference.md) tooexpress attributet transformationer. Du kan se och granska hello hela-konfigurationen med hjälp av verktyg som medföljer Azure AD Connect. Om du behöver toomake konfigurationsändringar, se till att du följer hello [metodtips](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) så att det är enklare tooadopt nya versioner.

### <a name="next-steps-toocustomize-azure-ad-connect-sync"></a>Nästa steg toocustomize Azure AD Connect-synkronisering
|Avsnitt |Länk|  
| --- | --- |
|Alla artiklar om Azure AD Connect-synkronisering | [Azure AD Connect-synkronisering](active-directory-aadconnectsync-whatis.md)|
|Tekniska begrepp | [Azure AD Connect-synkronisering: Tekniska begrepp](active-directory-aadconnectsync-technical-concepts.md)|
|Förstå hello standardkonfigurationen | [Azure AD Connect-synkronisering: Förstå hello standardkonfigurationen](active-directory-aadconnectsync-understanding-default-configuration.md)|
|Förstå användare och kontakter | [Azure AD Connect-synkronisering: Förstå användare och kontakter](active-directory-aadconnectsync-understanding-users-and-contacts.md)|
|Deklarativ etablering | [Azure AD Connect-synkronisering: Förstå uttryck för deklarativ etablering](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)|
|Ändra hello standardkonfigurationen | [Metodtips för att ändra hello standardkonfigurationen](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)|

## <a name="configure-federation-features"></a>Konfigurera federationsfunktioner
AD FS kan vara konfigurerade toosupport [flera domäner](active-directory-aadconnect-multiple-domains.md). Du kan till exempel ha flera toppdomäner som du behöver toouse för federation.

Om AD FS-servern inte har konfigurerat tooautomatically uppdatera certifikat från Azure AD eller om du använder en ADFS-lösning, sedan du meddelas när du har för[uppdatera certifikat](active-directory-aadconnect-o365-certs.md).

### <a name="next-steps-tooconfigure-federation-features"></a>Nästa steg tooconfigure federationsfunktioner
|Avsnitt |Länk|  
| --- | --- |
|Alla AD FS-artiklar | [Azure AD Connect och federation](active-directory-aadconnectfed-whatis.md)|
|Konfigurera AD FS med underdomäner | [Stöd för flera domäner för federering med Azure AD](active-directory-aadconnect-multiple-domains.md)|
|Hantera AD FS-servergrupp | [Hantering och anpassning av AD FS med Azure AD Connect](active-directory-aadconnect-federation-management.md)|
|Uppdatera federationscertifikat manuellt | [Förnya federationscertifikat för Office 365 och Azure AD](active-directory-aadconnect-o365-certs.md)|

## <a name="more-information-and-references"></a>Mer information och referenser
|Avsnitt |Länk|  
| --- | --- |
|Versionshistorik | [Versionshistorik](active-directory-aadconnect-version-history.md)|
|Jämföra DirSync, Azure ADSync och Azure AD Connect | [Jämförelse av katalogintegreringsverktyg](../active-directory-hybrid-identity-design-considerations-tools-comparison.md)|
|Kompatibilitetslista för Azure AD för andra lösningar än ADFS-lösningar | [Kompatibilitetslista för Azure AD-federation](active-directory-aadconnect-federation-compatibility.md)|
|Konfigurera en SAML 2.0-identitetsprovider (IdP)|[Använda en SAML 2.0-identitetsprovider (IdP) för enkel inloggning](active-directory-aadconnect-federation-saml-idp.md)|
|Attribut som synkroniseras | [Attribut som synkroniseras](active-directory-aadconnectsync-attributes-synchronized.md)|
|Övervaka med hjälp av Azure AD Connect Health | [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health.md)|
|Vanliga frågor och svar | [Vanliga frågor och svar om Azure AD Connect](active-directory-aadconnect-faq.md)|

**Ytterligare resurser**

Ignite 2015-presentation om hur du utökar dina lokala kataloger toohello moln.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3862/player]
> 
> 

