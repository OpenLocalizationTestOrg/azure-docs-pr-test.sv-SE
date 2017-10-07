---
title: 'Azure AD Connect: Anpassad installation | Microsoft Docs'
description: "Det här dokumentet beskriver hello anpassade installationsalternativen för Azure AD Connect. Använd dessa instruktioner tooinstall Active Directory via Azure AD Connect."
services: active-directory
keywords: "vad är Azure AD Connect, installera Active Directory, komponenter som krävs för Azure AD"
documentationcenter: 
author: billmath
manager: femila
ms.assetid: 6d42fb79-d9cf-48da-8445-f482c4c536af
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/02/2017
ms.author: billmath
ms.openlocfilehash: c49724fab78c5a1688662043f69506fff6f82104
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="custom-installation-of-azure-ad-connect"></a>Anpassad installation av Azure AD Connect
Azure AD Connect **anpassade inställningar** används när du vill ha fler alternativ för hello installation. Den används om du har flera skogar eller om du vill tooconfigure valfria funktioner som inte omfattas av snabbinstallationen hello. Den används i samtliga fall där hello [ **Snabbinstallation** ](active-directory-aadconnect-get-started-express.md) inte uppfyller dina distributions- eller -topologi.

Kontrollera innan du börjar installera Azure AD Connect, för[hämta Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) och krav för fullständig hello stegen i [Azure AD Connect: maskin- och](active-directory-aadconnect-prerequisites.md). Kontrollera också att du har nödvändiga konton tillgängliga. Mer information finns i [Azure AD Connect: Konton och behörigheter](active-directory-aadconnect-accounts-permissions.md).

Om anpassade inställningar inte matchar din topologi, till exempel tooupgrade DirSync, se [relaterad dokumentation](#related-documentation) för andra scenarier.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Installation av Azure AD Connect med anpassade inställningar
### <a name="express-settings"></a>Standardinställningar
Klicka på den här sidan **anpassa** toostart en installation med anpassade inställningar.

### <a name="install-required-components"></a>Installera nödvändiga komponenter
När du installerar hello synkroniseringstjänster, kan du låta hello valfria konfigurationsavsnittet avmarkerad och Azure AD Connect ställer automatiskt in allt. Konfigurera en SQL Server 2012 Express LocalDB-instans, skapar hello relevanta grupper och tilldela behörigheter. Om du vill toochange hello standardinställningar kan använda du hello följande tabell toounderstand hello valfri konfigurationsalternativ som är tillgängliga.

![Nödvändiga komponenter](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

| Valfri konfiguration | Beskrivning |
| --- | --- |
| Använda en befintlig SQL-server |Kan du toospecify hello SQL Server och hello instansnamn. Välj det här alternativet om du redan har en databasserver som du vill att toouse. Ange hello instansnamnet följt av ett kommatecken och portnummer nummer i **instansnamn** om SQL Server inte har aktiverad. |
| Använda ett befintligt tjänstkonto |Som standard använder Azure AD Connect ett virtuella tjänstkonto för hello synkronisering services toouse. Om du använder en fjärransluten SQLServer eller använder en proxyserver som kräver autentisering måste toouse en **Hanterat tjänstkonto** eller använda ett tjänstkonto i hello domän och känna hello lösenordet. Ange hello konto toouse i de fallen. Kontrollera att hello-användaren som kör hello installationen är en SA i SQL så kan du skapa en inloggning för hello-tjänstkontot. Se [Azure AD Connect: Konton och behörigheter](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) |
| Ange anpassade synkroniseringsgrupper |Som standard skapar Azure AD Connect fyra grupper lokala toohello server när hello synkroniseringstjänsterna installeras. Dessa grupper är: gruppen Administratörer, gruppen Ansvariga för, gruppen Bläddra och hello gruppen för återställning av lösenord. Du kan ange dina egna grupper här. hello-grupper måste vara lokal på hello-servern och går inte att hitta i hello domän. |

### <a name="user-sign-in"></a>Användarinloggning
När du har installerat hello nödvändiga komponenter, du uppmanas tooselect användarna med enkel inloggning. hello följande tabell ger en kort beskrivning av hello tillgängliga alternativ. En fullständig beskrivning av hello inloggning metoder finns [användarinloggning](active-directory-aadconnect-user-signin.md).

![Användarinloggning](./media/active-directory-aadconnect-get-started-custom/usersignin2.png)

| Alternativ för enkel inloggning | Beskrivning |
| --- | --- |
| Lösenordssynkronisering |Användare ska kunna toosign tooMicrosoft molntjänster, till exempel Office 365, med hjälp av hello samma lösenord som de använder i sina lokala nätverk. hello användarnas lösenord är synkroniserade tooAzure AD som lösenordshasher och autentiseringen sker i hello molnet. Mer information finns i [Lösenordssynkronisering](active-directory-aadconnectsync-implement-password-synchronization.md). |
|Direktautentisering (förhandsversion)|Användare ska kunna toosign tooMicrosoft molntjänster, till exempel Office 365, med hjälp av hello samma lösenord som de använder i sina lokala nätverk.  hello användare lösenord skickas via toohello lokala Active Directory-styrenhet toobe verifieras.
| Federation med AD FS |Användare ska kunna toosign tooMicrosoft molntjänster, till exempel Office 365, med hjälp av hello samma lösenord som de använder i sina lokala nätverk.  omdirigeras användarna hello tootheir lokala AD FS-instans toosign i och autentiseringen sker lokalt. |
| Konfigurera inte |Ingen av funktionerna installeras eller konfigureras. Välj det här alternativet om du redan har en federationsserver från en annan tillverkare eller en annan befintlig lösning på plats. |
|Aktivera enkel inloggning|Det här alternativet är tillgängligt med både Lösenordssynkronisering och direkt-autentisering och ger enkel inloggning på upplevelse för användare av fjärrskrivbord på hello företagsnätverket.  Mer information finns i avsnittet om [enkel inloggning](active-directory-aadconnect-sso.md). </br>Obs för AD FS-kunder som det här alternativet är inte tillgänglig eftersom AD FS redan erbjudanden hello samma nivå av enkel inloggning.</br>(om Tereftalsyra inte har publicerats på hello samtidigt)
|Inloggningsalternativ|Det här alternativet är tillgängligt för kunder för synkronisering av lösenord och ger en enkel inloggning upplevelse för användare av fjärrskrivbord på hello företagsnätverket.  </br>Mer information finns i avsnittet om [enkel inloggning](active-directory-aadconnect-sso.md). </br>Obs för AD FS-kunder som det här alternativet är inte tillgänglig eftersom AD FS redan erbjudanden hello samma nivå av enkel inloggning.


### <a name="connect-tooazure-ad"></a>Ansluta tooAzure AD
Ange ett globalt administratörskonto och lösenord på hello Anslut tooAzure AD skärmen. Om du har valt **Federation med AD FS** på föregående sida hello inte loggar in med ett konto i en domän som du planerar tooenable för federation. En rekommendation är toouse ett konto i hello standard **onmicrosoft.com** domän, som medföljer Azure AD-katalogen.

Det här kontot är bara använda toocreate ett tjänstkonto i Azure AD och används inte när hello-guiden har slutförts.  
![Användarinloggning](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Om ditt globala administratörskonto har MFA är aktiverat, måste tooprovide hello lösenordet igen i hello inloggning popup och fullständig hello MFA-kontrollen. hello kontrollen kan bestå i att uppge en Verifieringskod eller ett telefonsamtal.  
![Användarinloggning i MFA](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

hello globala administratörskonto kan också ha [Privileged Identity Management](../active-directory-privileged-identity-management-getting-started.md) aktiverat.

Om du får ett fel och har problem med anslutningen läser du [Felsöka anslutningsproblem](active-directory-aadconnect-troubleshoot-connectivity.md).

## <a name="pages-under-hello-section-sync"></a>Sidor under synkroniseringsavsnittet för hello

### <a name="connect-your-directories"></a>Anslut dina kataloger
tooconnect tooyour Active Directory Domain Service, Azure AD Connect måste hello skogens namn och autentiseringsuppgifter för ett konto med tillräcklig behörighet.

![Anslut katalog](./media/active-directory-aadconnect-get-started-custom/connectdir01.png)

Efter att ange hello skogsnamnet och klicka **Lägg till katalog**, en dialogruta visas och du uppmanas med hello följande alternativ:

| Alternativ | Beskrivning |
| --- | --- |
| Använda befintligt konto | Välj det här alternativet om du vill toobe används Azure AD Connect för att ansluta toohello AD-skog under katalogsynkronisering tooprovide en befintlig AD DS-konto. Du kan ange hello domändelen i NetBios- eller FQDN-format, dvs, FABRIKAM\syncuser eller fabrikam.com\syncuser. Det här kontot kan vara ett vanligt användarkonto eftersom det bara behöver standardläsbehörighet hello. Beroende på scenario kan du dock behöva fler behörigheter. Mer information finns i [Azure AD Connect: Konton och behörigheter](active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account). |
| Skapa ett nytt konto | Välj det här alternativet om du vill att Azure AD Connect guiden toocreate hello AD DS-konto krävs av Azure AD Connect för att ansluta toohello AD-skog under katalogsynkronisering. När det här alternativet är markerat, ange hello användarnamn och lösenord för ett företagsadministratörskonto. Hej företagsadministratörskonto som används av Azure AD Connect-guiden toocreate hello krävs AD DS-konto. Du kan ange hello domändelen i NetBios- eller FQDN-format, d.v.s. FABRIKAM\administrator eller fabrikam.com\administrator. |

![Anslut katalog](./media/active-directory-aadconnect-get-started-custom/connectdir02.png)


### <a name="azure-ad-sign-in-configuration"></a>Inloggningskonfiguration för Azure AD
Den här sidan kan du tooreview hello UPN-domäner som finns i lokala AD DS och som har verifierats i Azure AD. Den här sidan kan du också tooconfigure hello attributet toouse för hello userPrincipalName.

![Overifierade domäner](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Granska varje domän som markerats med **Inte tillagd** och **Inte verifierad**. Kontrollera att de domäner som du använder har verifierats i Azure AD. Klicka på symbolen för hello uppdatera när du har verifierat dina domäner. Mer information finns i [lägga till och verifiera hello domän](../active-directory-add-domain.md)

**UserPrincipalName** -hello attributet userPrincipalName är hello attributet som användare använder när de loggar in tooAzure AD och Office 365. hello domäner används, även kallat hello UPN-suffixet, bör verifieras i Azure AD innan hello användarna synkroniseras. Microsoft rekommenderar tookeep hello standard attributet userPrincipalName. Om det här attributet är icke-dirigerbart och inte kan verifieras, så det är möjligt tooselect ett annat attribut. Du kan till exempel välja email som hello attribut hålla hello inloggnings-ID. Om du använder ett annat attribut än userPrincipalName kallas det för ett **Alternativt ID**. hello Alternativt ID attributvärdet måste följa hello RFC822 som standard. Ett alternativt ID kan användas med både lösenordssynkronisering och federation.

>[!NOTE]
> Du måste ha minst en verifierad domän i ordning toocontinue hello guiden när du aktiverar direkt-autentisering.

> [!WARNING]
> Det går inte att använda ett alternativt ID med alla Office 365-arbetsbelastningar. Mer information finns för[konfigurera alternativa inloggnings-ID](https://technet.microsoft.com/library/dn659436.aspx).
>
>

### <a name="domain-and-ou-filtering"></a>Domän- och organisationsenhetsfiltrering
Som standard synkroniseras alla domäner och organisationsenheter. Om det finns vissa domäner och organisationsenheter som du inte vill toosynchronize tooAzure AD kan avmarkera du dessa domäner och organisationsenheter.  
![Filtrering av domän-OU](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png)  
Den här sidan i guiden hello konfigurerar domän- och OU-baserade filtrering. Om du planerar toomake ändringar sedan se [domänbaserade filtreringen](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) och [ou-baserade filtrering](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) innan du gör dessa ändringar. Vissa organisationsenheter är av yttersta vikt för hello funktioner och får inte vara omarkerat.

Om du använder OU-baserad filtrering med en Azure AD Connect-version som är äldre än 1.1.524.0 synkroniseras nya organisationsenheter som läggs till senare som standard. Om du vill att hello beteende som nya organisationsenheter inte ska synkroniseras och sedan kan du konfigurera den när hello-guiden har slutförts med [ou-baserade filtrering](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering). För Azure AD Connect version 1.1.524.0 eller efter ange du om du vill att den nya organisationsenheter toobe synkroniseras eller inte.

Om du planerar toouse [gruppbaserade filtrering](#sync-filtering-based-on-groups), kontrollera hello Organisationsenhet med hello grupp ingår och inte filtreras med OU-filtrering. OU-filtrering utvärderas före gruppbaserad filtrering.

Det är också möjligt att vissa domäner inte kan nås på grund av toofirewall begränsningar. Dessa domäner är omarkerade som standard och visas med en varning.  
![Domäner som inte kan nås](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Om du ser den här varningen, se till att dessa domäner verkligen inte kan nås och hello varning förväntas.

### <a name="uniquely-identifying-your-users"></a>Identifiera användarna unikt

#### <a name="select-how-users-should-be-identified-in-your-on-premises-directories"></a>Välj hur användare ska identifieras i dina lokala kataloger
hello kan funktionen matchande mellan skogar du toodefine hur användare från AD DS-skogar representeras i Azure AD. En användare kan antingen representeras endast en gång i alla skogar eller ha en kombination av aktiverade och inaktiverade konton. hello-användare kan också vara representerad som en kontakt i vissa skogar.

![Unik](./media/active-directory-aadconnect-get-started-custom/unique.png)

| Inställning | Beskrivning |
| --- | --- |
| [Dina användare representeras endast en gång över alla skogar](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Alla användare skapas som enskilda objekt i Azure AD. hello-objekt är inte anslutna i metaversum hello. |
| [E-postattribut](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Det här alternativet kopplar ihop användare och kontakter om hello e-postattributet har samma värde i olika skogar hello. Använd det här alternativet om dina kontakter har skapats med hjälp av GALSync. Om det här alternativet är valt, användarobjekt vars e-postattribut inte fylls inte synkroniserade tooAzure AD. |
| [ObjectSID och msExchangeMasterAccountSID/ msRTCSIP-OriginatorSid](active-directory-aadconnect-topologies.md#multiple-forests-single-azure-ad-tenant) |Det här alternativet kopplar ihop en aktiverad användare i en kontoskog med en inaktiverad användare i en resursskog. I Exchange kallas den här konfigurationen för en länkad postlåda. Det här alternativet kan också användas om du bara använder Lync och Exchange finns inte i hello resursskog. |
| sAMAccountName och MailNickName |Det här alternativet kopplar ihop attribut om det är förväntat hello inloggnings-ID för hello användaren kan hittas. |
| Ett specifikt attribut |Det här alternativet kan du tooselect ett eget attribut. Om det här alternativet är valt, användarobjekt vars (markerade) attributet inte fyllts inte synkroniserade tooAzure AD. **Begränsning:** och se till att toopick ett attribut som redan finns i hello metaversum. Om du väljer ett anpassat attribut (inte i hello metaversum) slutföras hello guiden inte. |

#### <a name="select-how-users-should-be-identified-with-azure-ad---source-anchor"></a>Välj hur användare ska identifieras med Azure AD – källfästpunkt
hello attributet sourceAnchor är ett attribut som inte kan ändras under hello livslängd för ett användarobjekt. Det är hello primära nyckeln som länkar hello lokala användare med hello användare i Azure AD.

| Inställning | Beskrivning |
| --- | --- |
| Låt Azure hantera hello källfästpunkten för mig | Välj det här alternativet om du vill att Azure AD toopick hello attribut för dig. Om du väljer det här alternativet, Azure AD Connect-guiden tillämpar hello sourceAnchor attributet markeringen logik som beskrivs i artikel [Azure AD Connect: Designbegreppen - med msDS-ConsistencyGuid som sourceAnchor](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). hello Guiden informerar vilket attribut har plockats som hello Källfästpunkten attribut när anpassade installationen är klar. |
| Ett specifikt attribut | Välj det här alternativet om du vill toospecify ett befintligt AD-attribut som hello sourceAnchor attribut. |

Eftersom hello-attributet inte kan ändras, måste du planera för en bra attributet toouse. En bra kandidat är objectGUID. Det här attributet ändras inte om inte hello användarkontot flyttas mellan skogar/domäner. I en miljö med flera skogar där du flyttar konton mellan skogar, användas ett annat attribut, till exempel ett attribut med employeeID hello. Undvik attribut som kan ändras när en person gifter sig eller får nya uppgifter. Du kan inte använda attribut med ett @-sign. Det betyder att du inte kan använda email eller userPrincipalName. hello-attributet är också skiftlägeskänsligt så när du flyttar ett objekt mellan skogar, se till att toopreserve hello gemener/versaler. Binära attribut är base64-kodade, men andra attributtyper är kvar i kodat tillstånd. I federationsscenarier och i vissa Azure AD-gränssnitt kallas det här attributet även för immutableID. Mer information om källfästpunkten hello kan hittas i hello [designbegreppen](active-directory-aadconnect-design-concepts.md#sourceanchor).

### <a name="sync-filtering-based-on-groups"></a>Synkroniseringsfiltrering baserat på grupper
hello funktionen för gruppfiltrering kan du toosync bara en liten del av objekt för en pilot. toouse denna funktion, skapa en grupp för detta ändamål i din lokala Active Directory. Lägg sedan till användare och grupper som ska vara synkroniserade tooAzure AD som direkta medlemmar. Du kan senare lägga till och ta bort användare toothis toomaintain hello listan över objekt som ska finnas i Azure AD. Alla objekt som du vill toosynchronize måste vara direkta medlemmar i gruppen hello. Användare, grupper, kontakter och datorer/enheter måste vara direkta medlemmar. Kapslade gruppmedlemskap stöds inte. När du lägger till en grupp som en medlem, endast hello själva gruppen har lagts till och inte dess medlemmar.

![Synkronisera filtrering](./media/active-directory-aadconnect-get-started-custom/filter2.png)

> [!WARNING]
> Den här funktionen är endast avsedda toosupport en pilotdistribution. Använd den inte i en fullständig produktionsdistribution.
>
>

I en komplett Produktionsdistribution blir ska det toobe hårda toomaintain en enda grupp med alla objekt toosynchronize. I stället bör du använda någon av hello metoder i [konfigurera filtrering](active-directory-aadconnectsync-configure-filtering.md).

### <a name="optional-features"></a>Valfria funktioner
Den här sidan kan du tooselect hello valfria funktioner för dina specifika scenarier.

![Valfria funktioner](./media/active-directory-aadconnect-get-started-custom/optional.png)

> [!WARNING]
> Om du har för närvarande DirSync eller Azure AD Sync active, aktiveras inte hello tillbakaskrivning av funktionerna i Azure AD Connect.
>
>

| Valfria funktioner | Beskrivning |
| --- | --- |
| Exchange-hybridinstallation |hello Exchange-hybridinstallation tillåter hello Exchange-postlådor existerar både lokalt och i Office 365. Azure AD Connect synkroniserar en specifik uppsättning [attribut](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) från Azure AD tillbaka till din lokala katalog. |
| Gemensamma mappar för Exchange-e-post | hello offentliga mappar för Exchange-e-funktionen kan du toosynchronize e-postaktiverade offentlig mapp objekt från din lokala Active Directory tooAzure AD. |
| Filtrering av Azure AD-appar och -attribut |Genom att aktivera Azure AD-app och attributfiltrering gör skräddarsys hello uppsättningen synkroniserade attribut. Det här alternativet lägger till två flera sidor toohello konfigurationsguiden. Mer information finns i [Filtrering av Azure AD-appar och -attribut](#azure-ad-app-and-attribute-filtering). |
| Lösenordssynkronisering |Om du valde federation som hello inloggning lösning kan du aktivera det här alternativet. Lösenordssynkronisering kan sedan användas som ett reservalternativ. Mer information finns i [Lösenordssynkronisering](active-directory-aadconnectsync-implement-password-synchronization.md). </br></br>Om du valde direkt autentisering är det här alternativet aktiverat som standard tooensure stöd för äldre klienter och som ett reservalternativ. Mer information finns i [Lösenordssynkronisering](active-directory-aadconnectsync-implement-password-synchronization.md).|
| Tillbakaskrivning av lösenord |Genom att aktivera tillbakaskrivning av lösenord skrivs lösenordsändringar som skapats i Azure AD tillbaka tooyour lokala katalog. Mer information finns i [Komma igång med lösenordshantering](../active-directory-passwords-getting-started.md). |
| Tillbakaskrivning av grupp |Om du använder hello **Office 365-grupper** funktion, och du kan ha dessa grupper vara representerade i din lokala Active Directory. Det här alternativet är endast tillgänglig om Exchange finns i din lokala Active Directory. Mer information finns i [Tillbakaskrivning av grupp](active-directory-aadconnect-feature-preview.md#group-writeback). |
| Tillbakaskrivning av enheter |Tillåter toowriteback enhetsobjekt i Azure AD tooyour lokala Active Directory för scenarier med villkorlig åtkomst. Mer information finns i [Aktivera tillbakaskrivning av enheter i Azure AD Connect](active-directory-aadconnect-feature-device-writeback.md). |
| Synkronisering av katalogtilläggsattribut |Genom att aktivera synkronisering är attribut som angetts synkroniserade tooAzure AD. Mer information finns i [Katalogtillägg](active-directory-aadconnectsync-feature-directory-extensions.md). |

### <a name="azure-ad-app-and-attribute-filtering"></a>Filtrering av Azure AD-appar och -attribut
Om du vill toolimit vilka attribut toosynchronize tooAzure AD, börjar du med att välja vilka tjänster som du använder. Om du gör konfigurationsändringar på den här sidan har en ny tjänst toobe valt uttryckligen genom att köra installationsguiden för hello.

![Valfria funktioner – appar](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Baserat på hello tjänster du valde i föregående steg i hello visar denna sida alla attribut som synkroniseras. Den här listan är en kombination av alla typer av objekt som synkroniseras. Om det finns vissa specifika attribut som du behöver toonot synkronisera kan avmarkera du dessa attribut.

![Valfria funktioner – attribut](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

> [!WARNING]
> Funktionaliteten kan påverkas om du tar bort attribut. Bästa praxis och rekommendationer finns i [attribut som synkroniseras](active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).
>
>

### <a name="directory-extension-attribute-sync"></a>Synkronisering av katalogtilläggsattribut
Du kan utöka hello schemat i Azure AD med anpassade attribut som läggs till av din organisation eller andra attribut i Active Directory. toouse funktionen väljer **katalogtilläggsattribut** på hello **valfria funktioner** sidan. Du kan markera fler attribut toosync på den här sidan.

![Katalogtillägg](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Mer information finns i [Katalogtillägg](active-directory-aadconnectsync-feature-directory-extensions.md).

### <a name="enabling-single-sign-on-sso"></a>Aktivera enkel inloggning (SSO)
Konfigurera enkel inloggning för användning med Lösenordssynkronisering eller direkt-autentisering är det enkelt att du bara behöver toocomplete en gång för varje skog som synkroniserade tooAzure AD. Konfigurationen omfattar följande två steg:

1.  Skapa hello nödvändiga datorkonto i din lokala Active Directory.
2.  Konfigurera hello intranätzonen för hello klientdatorer toosupport enkel inloggning.

#### <a name="create-hello-computer-account-in-active-directory"></a>Skapa hello datorkonto i Active Directory
För varje skog som har lagts till i Azure AD Connect, behöver du toosupply domänadministratörsbehörighet så att hello datorkontot kan skapas i varje skog. hello autentiseringsuppgifter är endast använda toocreate hello-konto och inte lagras eller användas för alla andra åtgärder. Lägg bara till hello autentiseringsuppgifter på hello **aktivera enkel inloggning** hello Azure AD Connect-guiden som visas:

![Aktivera enkel inloggning](./media/active-directory-aadconnect-get-started-custom/enablesso.png)

>[!NOTE]
>Du kan hoppa över en viss skog om du inte vill att toouse för enkel inloggning med den skogen.

#### <a name="configure-hello-intranet-zone-for-client-machines"></a>Konfigurera hello intranätzonen för klientdatorer.
tooensure att hello klienten inloggningar automatiskt i hello intranät zonen du behöver tooensure att två URL: er är en del av hello intranätzonen. Detta säkerställer att hello domänansluten dator skickas automatiskt en Kerberos-biljett tooAzure AD när den är ansluten toohello företagsnätverket.
På en dator har som hello verktyg för hantering av Grupprincip.

1.  Öppna hello verktyg för hantering av Grupprincip
2.  Redigera hello Grupprincip som kommer att tillämpas tooall användare. Till exempel hello domänens standardprincip.
3.  Navigera för**användaren Datorkonfiguration\Administrativa mallar\Windows-komponenter\Internet Explorer\Internet Panel\Security kontrollsida** och välj **plats tooZone Tilldelningslista** per hello-bild nedan.
4.  Aktivera hello princip och ange följande två objekt i dialogrutan för hello hello.

        Value: `https://autologon.microsoftazuread-sso.com`  
        Data: 1  
        Value: `https://aadg.windows.net.nsatc.net`  
        Data: 1

5.  Det bör se ut ungefär toohello följande:  
![Intranätszoner](./media/active-directory-aadconnect-get-started-custom/sitezone.png)

6.  Klicka på **OK** två gånger.

## <a name="configuring-federation-with-ad-fs"></a>Konfigurera federation med AD FS
Du kan konfigurera AD FS med Azure AD Connect med bara några klickningar. hello följande krävs innan hello-konfigurationen.

* En Windows Server 2012 R2-server för hello federationsservern med fjärrhantering aktiverat
* En Windows Server 2012 R2-server för hello webbprogramproxyservern med fjärrhantering aktiverat
* Ett SSL-certifikat för hello federation service namn som du avser toouse (till exempel sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>Förutsättningar för AD FS-konfiguration
tooconfigure AD FS grupp med Azure AD Connect, kontrollera att WinRM är aktiverat på hello fjärrservrar. Dessutom kan gå igenom hello portkraven som anges i [tabell 3 – Azure AD Connect och federationsservrar/WAP](active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-ad-fs-federation-serverswap).

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Skapa en ny AD FS-servergrupp eller använd en befintlig AD FS-servergrupp
Du kan använda en befintlig AD FS-servergrupp eller välja toocreate en ny AD FS-servergrupp. Om du väljer toocreate en ny är obligatoriska tooprovide hello SSL-certifikat. Om hello SSL-certifikatet är lösenordsskyddat, uppmanas hello lösenord.

![AD FS-servergrupp](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Om du väljer toouse en befintlig AD FS-servergrupp, tas du direkt toohello konfigurera hello förtroende mellan AD FS och AD Azure.

### <a name="specify-hello-ad-fs-servers"></a>Ange hello AD FS-servrar
Ange hello-servrar som du vill tooinstall AD FS på. Du kan lägga till en eller flera servrar baserat på dina kapacitetsplaneringsbehov. Anslut alla servrar tooActive Directory innan du utför den här konfigurationen. Microsoft rekommenderar att du installerar en enskild AD FS-server för test- och pilotdistributioner. Sedan lägger du till och distribuera flera servrar toomeet dina skalningsbehov genom att köra Azure AD Connect igen efter den första konfigurationen.

> [!NOTE]
> Kontrollera att alla dina servrar är domänansluten tooan AD-domänen innan du gör den här konfigurationen.
>
>

![AD FS-servrar](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-hello-web-application-proxy-servers"></a>Ange hello Web Application Proxy-servrar
Ange hello-servrar som du vill ha som ditt webbprogram proxyservrar. Hej webbprogramproxyservern distribueras i DMZ (mot ett extranät) och stöder autentiseringsbegäranden från extranätet hello. Du kan lägga till en eller flera servrar baserat på dina kapacitetsplaneringsbehov. Microsoft rekommenderar att du installerar en enskild webbprogramproxyserver för test- och pilotdistributioner. Sedan lägger du till och distribuera flera servrar toomeet dina skalningsbehov genom att köra Azure AD Connect igen efter den första konfigurationen. Vi rekommenderar att du har ett motsvarande antal proxyautentisering servrar toosatisfy från hello intranät.

> [!NOTE]
> <li> Om hello-konto som du använder inte är en lokal administratör på hello AD FS-servrar, sedan tillfrågas du om autentiseringsuppgifter som administratör.</li>
> <li> Kontrollera att det finns HTTP/HTTPS-anslutning mellan hello Azure AD Connect-servern och hello webbprogramproxyservern innan du kör det här steget.</li>
> <li> Se till att det finns HTTP/HTTPS-anslutning mellan hello Webbappservern och hello AD FS-servern tooallow autentisering begäranden tooflow via.</li>
>

![Webbapp](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

Du kan ange tooenter autentiseringsuppgifter så att hello webbappservern kan upprätta en säker anslutning toohello AD FS-server. Dessa autentiseringsuppgifter måste toobe lokal administratör på hello AD FS-servern.

![Proxy](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-hello-service-account-for-hello-ad-fs-service"></a>Ange hello tjänstkonto för hello AD FS-tjänsten
hello AD FS-tjänsten kräver en domän konto tooauthenticate användare och söka efter användarinformation i Active Directory-tjänsten. Två typer av tjänstkonton stöds:

* **Grupphanterat tjänstkonto** – Introducerades i Active Directory Domain Services med Windows Server 2012. Den här typen av konto tillhandahåller tjänster, till exempel AD FS, ett konto utan att behöva tooupdate hello kontolösenord regelbundet. Använd det här alternativet om du redan har Windows Server 2012-domänkontrollanter i hello-domän som AD FS-servrarna tillhör.
* **Domänanvändarkonto** -den här typen av konto kräver att du tooprovide ett lösenord och regelbundet uppdatera hello lösenord när hello lösenordet ändras eller upphör att gälla. Använd det här alternativet endast när du inte har Windows Server 2012-domänkontrollanter i hello-domän som AD FS-servrarna tillhör.

Om du har valt Grupphanterat tjänstkonto och funktionen aldrig har använts i Active Directory uppmanas du att ange autentiseringsuppgifter som företagsadministratör. Autentiseringsuppgifterna är används tooinitiate hello KeyStore och aktiverar hello i Active Directory.

> [!NOTE]
> Azure AD Connect utförs en kontroll toodetect om hello AD FS-tjänsten är redan registrerad som ett SPN i hello domän.  AD DS kan inte dubbla SPN toobe registrerade på samma gång.  Om det finns en dubbel SPN, blir inte kan tooproceed ytterligare förrän hello SPN har tagits bort.

![AD FS-tjänstkonto](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-hello-azure-ad-domain-that-you-wish-toofederate"></a>Välj hello Azure AD-domän som du vill toofederate
Den här konfigurationen är används toosetup hello federationsrelationen mellan AD FS och Azure AD. Den konfigurerar AD FS tooissue säkerhet token tooAzure AD och konfigurerar Azure AD tootrust hello token från den här specifika instansen av AD FS. Den här sidan kan du bara tooconfigure en enda domän i hello första installationen. Du kan konfigurera flera domäner senare genom att köra Azure AD Connect igen.

![Azure AD-domän](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-hello-azure-ad-domain-selected-for-federation"></a>Kontrollera hello Azure AD-domänen som valts för federation
När du väljer hello domän toobe federerade innehåller Azure AD Connect nödvändig information tooverify en overifierade domän. Se [Lägg till och verifiera hello domän](../active-directory-add-domain.md) för hur toouse informationen.

![Azure AD-domän](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

> [!NOTE]
> AD Connect försöker tooverify hello domän under hello konfigurera steget. Om du fortsätter tooconfigure utan att lägga till hello nödvändiga DNS-posterna är hello guiden inte kan toocomplete hello konfiguration.
>
>

## <a name="configure-and-verify-pages"></a>Konfigurera och verifiera sidor
hello konfiguration sker på den här sidan.

> [!NOTE]
> Innan du fortsätter installationen, och om du har konfigurerat federation, kontrollerar du att du har konfigurerat [namnmatchning för federationsservrar](active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers).
>
>

![Redo tooconfigure](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Mellanlagringsläge
Det är möjligt toosetup en ny synkroniseringsserver parallellt med mellanlagringsläget. Det är bara stöds toohave en synkroniseringsserver som exporterar tooone katalog i hello molnet. Men om du vill toomove från en annan server, till exempel en som kör DirSync, kan du aktivera Azure AD Connect i mellanlagringsläge. När aktiverad hello sync motorn importera och synkronisera data som vanligt, men exporterar inte något tooAzure AD eller AD. hello funktioner lösenord synkronisering och tillbakaskrivning av lösenord är inaktiverade när mellanlagringsläge är aktiverat.

![Mellanlagringsläge](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

I fristående läge är möjliga toomake nödvändiga ändringar toohello Synkroniseringsmotorn och se vad som är om toobe exporteras. När hello konfigurationen ser bra ut kör hello installationsguiden igen och inaktiverar mellanlagringsläge. Data är nu exporterade tooAzure AD från den här servern. Se till att toodisable hello andra servern på samma gång så att endast en server exporterar aktivt hello.

Mer information finns i [Mellanlagringsläge](active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-your-federation-configuration"></a>Verifiera federationkonfigurationen
Azure AD Connect verifierar hello DNS-inställningarna för dig när du klickar på knappen för hello verifiera.

**Anslutningskontroller för intranät**

* Lös federation FQDN: Azure AD Connect kontrollerar om hello federation FQDN kan matchas av DNS-tooensure anslutning. Om Azure AD Connect inte kan lösa hello FQDN, att hello verifieringen misslyckas. Kontrollera att det finns en DNS-post för hello federationstjänsten FQDN ordning toosuccessfully fullständig hello verifieringen.
* DNS A-post: Azure AD Connect kontrollerar om det finns en A-post för federationstjänsten. Hello saknas en A-post misslyckas hello verifieringen. Skapa en A-post och inte CNAME-post för din federationsserver FQDN i ordning toosuccessfully Slutför hello verifieringen.

**Anslutningskontroller för extranät**

* Lös federation FQDN: Azure AD Connect kontrollerar om hello federation FQDN kan matchas av DNS-tooensure anslutning.

![Slutför](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Verifiera](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Dessutom utför hello följande verifieringssteg:

* Kontrollera att du kan logga in från en webbläsare från en domänansluten dator hello intranätet: Anslut toohttps://myapps.microsoft.com och kontrollera hello logga in med ditt inloggade konto. hello inbyggda AD DS-administratörskontot synkroniseras inte och kan inte användas för verifiering.
* Kontrollera att du kan logga in från en enhet från hello extranät. Anslut toohttps://myapps.microsoft.com på en hemdator eller en mobil enhet, och ange dina autentiseringsuppgifter.
* Verifiera inloggningen på en rich-klient. Anslut toohttps://testconnectivity.microsoft.com, Välj hello **Office 365** och välj hello **Office 365 enkel inloggning Test**.

## <a name="next-steps"></a>Nästa steg
När hello-installationen har slutförts, logga ut och logga in igen tooWindows innan du använder Synchronization Service Manager eller Synchronization Rule Editor.

Nu när du har Azure AD Connect installeras kan du [verifiera hello installationen och tilldela licenser](active-directory-aadconnect-whats-next.md).

Mer information om dessa funktioner, som aktiverades med installationen hello: [förhindra oavsiktliga borttagningar](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) och [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).

Mer information om dessa vanliga avsnitt: [Schemaläggaren och hur tootrigger synkronisera](active-directory-aadconnectsync-feature-scheduler.md).

Läs mer om hur du [integrerar dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md).
