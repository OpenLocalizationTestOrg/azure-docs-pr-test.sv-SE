---
title: "Azure AD SSPR med tillbakaskrivning av lösenord | Microsoft Docs"
description: "Med hjälp av Azure AD och Azure AD Connect toowriteback lösenord tooon lokal katalog"
services: active-directory
keywords: "Hantering av Active directory-lösenord, lösenordshantering, Azure AD self service för lösenordsåterställning"
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: 6a1dd964a51b4f3b5b0be303b722ab6deb4a6110
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="password-writeback-overview"></a>Översikt över tillbakaskrivning av lösenord

Tillbakaskrivning av lösenord gör du tooconfigure Azure AD toowrite lösenord tillbaka tooyou lokala Active Directory. Tas bort hello måste tooset in och hantera en komplicerad lokala självbetjäning lösenord Återställ lösningen och ger ett bekvämt sätt molnbaserade för dina användare tooreset sina lokala lösenord oavsett var de befinner. Tillbakaskrivning av lösenord är en del av [Azure Active Directory Connect](./connect/active-directory-aadconnect.md) som kan aktiveras och används av aktuella Premium-prenumeranter [Azure Active Directory-versioner](active-directory-editions.md).

Tillbakaskrivning av lösenord innehåller följande funktioner hello

* **Noll fördröjning feedback** -tillbakaskrivning av lösenord är en synkron åtgärd. Användarna meddelas omedelbart om lösenordet inte uppfyller principen eller har inte toobe återställa eller ändras av någon anledning.
* **Har stöd för återställa lösenord för användare som använder AD FS eller andra tekniker för federation** -med tillbakaskrivning av lösenord, så länge hello federerade användarkonton synkroniseras till din Azure AD-klient är kan toomanage sina lokala AD lösenord från hello molnet.
* **Har stöd för återställa lösenord för användare som använder [lösenordshashsynkronisering](./connect/active-directory-aadconnectsync-implement-password-synchronization.md)**  – när hello lösenord återställa tjänsten känner av att ett synkroniserade användarkonto har aktiverats för lösenordshashsynkronisering återställs både det här kontot lokalt och molnet lösenord samtidigt.
* **Stöder ändring av lösenord från hello åtkomst panelen och Office 365** – när federerade eller lösenord synkroniseras användare komma toochange lösenord har upphört att gälla eller ej upphört att gälla, vi skriva dessa lösenord tillbaka tooyour lokala AD-miljö.
* **Stöd för skrivning tillbaka lösenord när en administratör återställer dem från hello Azure-portalen** – när en administratör återställer ett lösenord i hello [Azure-portalen](https://portal.azure.com)om användaren är federerat eller lösenord synkroniseras, vi ställer hello lösenord Hej administratör väljer på din lokala AD samt. Detta stöds för närvarande inte i hello administrationsportalen för Office.
* **Tillämpar dina lokala AD lösenordsprinciper** – när en användare återställer sitt lösenord vi Kontrollera att den uppfyller dina lokala AD-princip innan du genomför den toothat directory. Detta inkluderar historik, komplexitet, ålder, lösenordsfilter och eventuella andra begränsningar för lösenord du har definierat i din lokala AD.
* **Kräver inte några inkommande brandväggsregler** -tillbakaskrivning av lösenord använder en Azure Service Bus relay som en underliggande kommunikationskanalen, vilket innebär att du inte har tooopen alla ingående portar i brandväggen för den här funktionen toowork.
* **Stöds inte för användarkonton som finns inom skyddade grupper i din lokala Active Directory** – för mer information om skyddade grupper, se [skyddade konton och grupper i Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

## <a name="how-password-writeback-works"></a>Så här fungerar tillbakaskrivning av lösenord

När en federerad eller lösenord hash synkroniserade användare kommer tooreset eller ändra sina lösenord i hello molnet hello följande inträffar:

1. Vi kontrollerar toosee vilken typ av lösenord hello användaren har.
    * Om vi ser hanteras hello lösenord lokalt
        * Vi kontrollerar om hello tillbakaskrivning av tjänsten är igång och körs, om det är vi ge hello användaren fortsätta
        * Om hello tillbakaskrivning av tjänsten inte är igång, meddela vi hello användaren att deras lösenord inte kan återställas nu
2. Därefter hello användaren skickar hello lämpliga autentiseringsgater och når skärmen för hello återställning av lösenord.
3. hello användare väljer ett nytt lösenord och bekräftar den.
4. När du klickar på Skicka kryptera vi hello lösenord i klartext med en symmetrisk nyckel som skapades under installationsprocessen för hello tillbakaskrivning.
5. Efter kryptering hello lösenord inkludera vi den i en nyttolast som skickas via en HTTPS kanal tooyour klient-specifika service bus relay (som vi också ställa in automatiskt under installationsprocessen för hello tillbakaskrivning). Den här relay skyddas av ett slumpmässigt genererat lösenord som känner av endast lokala installationen.
6. När hello-meddelande når service bus, hello slutpunkten för återställning av lösenord automatiskt väcker och ser att det finns en väntande begäran återställning.
7. hello-tjänsten sedan letar efter hello användaren i fråga med hjälp av hello fästpunktsattributet för molnet. För den här toosucceed sökning:

    * hello användarobjektet måste finnas i hello AD anslutarplats
    * hello användarobjektet måste vara länkade toohello motsvarande MV-objekt
    * hello användarobjektet måste vara länkade toohello motsvarande AAD-koppling objekt.
    * hello länk från AD-koppling objektet tooMV måste ha hello synkroniseringsregeln `Microsoft.InfromADUserAccountEnabled.xxx` på hello länk. <br> <br>
    När hello anropet kommer från hello molnet, hello synkronisering motorn använder hello cloudAnchor attributet toolook hello AAD-koppling utrymme objekt följer hello länken tillbaka toohello MV-objekt och följer hello länka tillbaka toohello AD-objekt. Eftersom det kan finnas flera AD-objekt (flera skogar) för samma användare, hello Synkroniseringsmotorn förlitar sig på hello hello `Microsoft.InfromADUserAccountEnabled.xxx` länk toopick hello korrigera en.

    > [!Note]
    > Azure AD Connect måste vara kan toocommunicate med hello PDC-emulatorn för toowork för tillbakaskrivning av lösenord på grund av denna logik. Om du behöver tooenable detta manuellt, kan du ansluta Azure AD Connect toohello PDC-emulatorn genom att högerklicka på hello **egenskaper** av koppling hello Active Directory-synkronisering, sedan välja **konfigurera katalogpartitioner**. Därifrån kan du leta efter hello **inställningarna för domänkontrollanter anslutning** avsnittet och markera kryssrutan hello med titeln **endast använder prioriterade domänkontrollanter**. Även om hello primär Domänkontrollant inte är en PDC-emulator, försöker Azure AD Connect tooconnect toohello PDC för tillbakaskrivning av lösenord.

8. När hello användarkonto har hittats, försöker vi tooreset hello lösenordet direkt i hello lämplig AD-skog.
9. Om det lyckas hello lösenord set-åtgärd meddela vi hello användaren sitt lösenord har ändrats.
    > [!NOTE]
    > I hello fallet när hello användarens lösenord är synkroniserade tooAzure AD med hjälp av Lösenordssynkronisering, ökar risken att hello lokala lösenordsprincip är lägre än hello molnet lösenordsprincip. I det här fallet kräva vi dock oavsett hello lokalt-princip är och istället låta lösenord hash-synkronisering toosynchronize hello hash för lösenordet. Detta säkerställer att din princip för lokal tillämpas i hello molnet, oavsett om du använder lösenord synkronisering eller federation tooprovide enkel inloggning.

10. Om hello lösenord misslyckas, vi returnera hello fel toohello användare och låter dem försök igen.
    * hello misslyckas på grund av följande hello
        * hello-tjänsten är inaktiv
        * hello lösenord de markerade uppfyller inte organisationens principer
        * Vi kunde inte hitta hello användare i hello lokala AD

    Vi har ett särskilt meddelande för många av dessa fall och meddela hello användaren vad de kan göra tooresolve hello problemet.

## <a name="configuring-password-writeback"></a>Konfigurera tillbakaskrivning av lösenord

Vi rekommenderar att du använder funktionen för automatisk uppdatering av hello av [Azure AD Connect](./connect/active-directory-aadconnect-get-started-express.md) om du vill toouse tillbakaskrivning av lösenord.

DirSync och Azure AD Sync är inte längre stöds sätt för att aktivera lösenord tillbakaskrivning hello artikel [uppgradera från DirSync och Azure AD Sync](connect/active-directory-aadconnect-dirsync-deprecated.md) har mer information toohelp med övergången.

hello stegen nedan förutsätter att du redan har konfigurerat Azure AD Connect i din miljö med hello [Express](./connect/active-directory-aadconnect-get-started-express.md) eller [anpassade](./connect/active-directory-aadconnect-get-started-custom.md) inställningar.

1. tooconfigure och aktivera tillbakaskrivning av lösenord logga in tooyour Azure AD Connect-servern och starta hello **Azure AD Connect** konfigurationsguiden.
2. Klicka på välkomstskärmen hello **konfigurera**.
3. På hello ytterligare uppgifter skärmen klickar du på **anpassa synkroniseringsalternativ** och välj sedan **nästa**.
4. Ange autentiseringsuppgifter för en Global administratör hello Anslut tooAzure AD skärmen och välj **nästa**.
5. På hello ansluta dina kataloger och domän och Organisationsenhet Förfrågningsfiltrering kontrolleras som du kan välja **nästa**.
6. På skärmen för valfria funktioner hello kryssrutan hello bredvid för**tillbakaskrivning av lösenord** och på **nästa**.
   ![Aktivera tillbakaskrivning av lösenord i Azure AD Connect][Writeback]
7. Klicka på klar tooconfigure hello-skärmen **konfigurera** och vänta tills hello processen toocomplete.
8. När du ser att konfigurationen slutförts kan du klicka på **avsluta**

## <a name="licensing-requirements-for-password-writeback"></a>Licensieringskrav för tillbakaskrivning av lösenord

Mer information om licensiering, se avsnittet hello [licenser som krävs för tillbakaskrivning av lösenord](active-directory-passwords-licensing.md#licenses-required-for-password-writeback) eller hello följande platser

* [Platsen för Azure Active Directory-priser](https://azure.microsoft.com/pricing/details/active-directory/)
* [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security)
* [Säker produktiva Enterprise](https://www.microsoft.com/secure-productive-enterprise/default.aspx)

### <a name="on-premises-authentication-modes-supported-for-password-writeback"></a>Lokala autentiseringslägen som stöds för tillbakaskrivning av lösenord

Tillbakaskrivning av lösenord fungerar för hello följande användartyper lösenord:

* **Endast molnbaserad användare**: tillbakaskrivning av lösenord är inte giltigt i den här situationen eftersom det inte finns några lokala lösenord
* **Lösenord synkroniseras användare**: tillbakaskrivning av lösenord som stöds
* **Federerade användare**: tillbakaskrivning av lösenord som stöds
* **Direkt-autentisering användare**: tillbakaskrivning av lösenord som stöds

### <a name="user-and-admin-operations-supported-for-password-writeback"></a>Användare och administrativa åtgärder som stöds för tillbakaskrivning av lösenord

Lösenord skrivs tillbaka i alla hello följande situationer:

* **Slutanvändarens de åtgärder som stöds**
  * Alla slutanvändare självbetjäning frivillig ändra lösenord
  * Alla slutanvändare självbetjäning kraft ändra lösenord för åtgärden (t ex lösenordet upphör att gälla)
  * Alla slutanvändare Självbetjäning för lösenordsåterställning härstammar från hello [portalen för återställning av lösenord](https://passwordreset.microsoftonline.com)
* **Administratören de åtgärder som stöds**
  * En administratör självbetjäning frivillig ändra lösenord
  * En administratör självbetjäning kraft ändra lösenord för åtgärden (t ex lösenordet upphör att gälla)
  * En administratör Självbetjäning för lösenordsåterställning härstammar från hello [portalen för återställning av lösenord](https://passwordreset.microsoftonline.com)
  * Alla administratörsstartade slutanvändarens lösenordsåterställning från hello [klassiska Azure-portalen](https://manage.windowsazure.com)
  * Alla administratörsstartade slutanvändarens lösenordsåterställning från hello [Azure-portalen](https://portal.azure.com)

### <a name="user-and-admin-operations-not-supported-for-password-writeback"></a>Användar- och Admin åtgärder stöds inte för tillbakaskrivning av lösenord

Lösenord är **inte** skriven i någon av följande situationer hello:

* **Stöds inte slutanvändaråtgärder**
  * Slutanvändare återställa sina egna lösenord med hjälp av PowerShell v1, v2 eller hello Azure AD Graph API
* **Åtgärder som inte stöds**
  * Alla administratörsstartade slutanvändarens lösenordsåterställning från hello [hanteringsportalen för Office](https://portal.office.com)
  * Alla administratörsstartade slutanvändarens lösenordsåterställning från PowerShell v1, v2 eller hello Azure AD Graph API

När vi arbetar tooremove dessa begränsningar kan har vi inte en specifik tidslinje vi kan dela ännu.

## <a name="password-writeback-security-model"></a>Säkerhetsmodellen med tillbakaskrivning av lösenord

Tillbakaskrivning av lösenord är ett mycket säkert tjänst.  tooensure din information skyddas, vi aktivera en 4 nivåer säkerhetsmodell som beskrivs nedan.

* **Klient-specifika service bus relay**
  * När du ställer in hello-tjänsten kan konfigurera vi en klient-specifika service bus relay som skyddas av ett slumpmässigt genererat starkt lösenord som Microsoft har aldrig åtkomst till.
* **Krypteringsnyckel för låst, kryptografiskt starka lösenord**
  * När du har skapat hello service bus relay skapa vi en stark symmetriska nyckel som vi använder tooencrypt hello lösenord som den kommer över hello överföring. Den här nyckeln finns bara i ditt företag hemliga arkivet i hello moln, vilket är låst och granskas precis som alla lösenord i hello directory kraftigt.
* **Industry standard TLS**
  1. När ett lösenord återställa eller ändra åtgärden genomförs i hello molnet, vi ta hello lösenord i klartext och kryptera med den offentliga nyckeln.
  2. Vi sedan placera som i en HTTPS-meddelandet som skickas via en krypterad kanal med hjälp av Microsofts SSL-certifikat tooyour service bus relay.
  3. När hello-meddelande anländer till Service Bus, lokal agent vaknar in och autentiserar tooService Bus med hjälp av hello starkt lösenord som skapades tidigare.
  4. Lokal agent hämtar krypterade hello-meddelande, dekrypterar den med hjälp av hello privata nyckeln vi genereras.
  5. Lokal agent försöker sedan tooset hello lösenord via hello AD DS SetPassword API.
     * Det här steget är vad gör att vi tooenforce din AD lokala lösenordsprincip (komplexitet, ålder, historik, filter, etc.) i hello molnet.
* **Förfallodatum för meddelandet** 
  * Om hello-meddelande placeras i Service Bus eftersom din lokala-tjänsten har stoppats, avslutas och tas bort efter flera minuter tooincrease säkerhet ytterligare.

### <a name="password-writeback-encryption-details"></a>Krypteringsinformation med tillbakaskrivning av lösenord

Hej krypteringssteg en begäran om lösenordsåterställning går igenom när användaren har skickat det, innan den tas emot i din lokala miljö, tooensure högsta tillåtna tillförlitlighet och säkerhet beskrivs nedan.

* **Steg 1 – lösenordskryptering med 2048-bitars RSA-nyckel** – när en användare skickar ett lösenord toobe skrivs tillbaka tooon lokala först hello skickade själva lösenordet krypteras med en 2048-bitars RSA-nyckel.
* **Steg 2 – paket-nivå kryptering med AES-GCM** - sedan hello-paketet (lösenord + metadata som krävs) har krypterats med AES-GCM. Detta förhindrar alla med direktåtkomst toohello underliggande ServiceBus kanal visning/manipulera hello innehållet.
* **Steg 3 – All kommunikation som sker över TLS / SSL** -alla hello kommunikation med ServiceBus sker i SSL/TLS-kanal. Detta skyddar hello innehållet från obehörig 3 parter.
* **Automatisk nyckelförnyelse var sjätte månad** - automatiskt var sjätte månad eller varje gång tillbakaskrivning av lösenord är inaktiverad / aktiveras på Azure AD Connect vi förnyade alla dessa nycklar tooensure högsta tillåtna säkerhets- och säkerhet.

### <a name="password-writeback-bandwidth-usage"></a>Bandbreddsanvändning med tillbakaskrivning av lösenord

Tillbakaskrivning av lösenord är en långsam-tjänst som skickar begäranden bakåt toohello lokal agent endast under hello följande omständigheter:

1. Två meddelanden när du aktiverar eller inaktiverar hello-funktionen via Azure AD Connect.
2. Ett meddelande skickas en gång var femte minut som ett pulsslag för så länge hello-tjänsten körs.
3. Två meddelanden skickas varje gång ett nytt lösenord har skickats
    * Första meddelandet som en begäran tooperform hello-åtgärd
    * Andra meddelande som innehåller hello resultatet av hello och skickas i hello följande omständigheter:
        * Varje gång ett nytt lösenord skickas när en användare lösenordsåterställning via självbetjäning.
        * Ett nytt lösenord skickas under en användarlösenord ändras igen.
        * Varje gång ett nytt lösenord skickas under en admin-initierad användaren lösenordsåterställning (från endast hello Azure admin portaler).

#### <a name="message-size-and-bandwidth-considerations"></a>Överväganden för meddelande storlek och bandbredd

hello storlek på hello-meddelande som beskrivs ovan är vanligtvis under 1 kb, även under extrema belastning, hello lösenord tillbakaskrivning själva tjänsten förbrukar några kbit / s bandbredd. Eftersom varje meddelande skickas i realtid, bara vid behov av en uppdateringsåtgärd för lösenord och eftersom hello meddelandestorlek är så liten hello bandbreddsanvändning för hello tillbakaskrivning kapaciteten effektivt är för liten toohave riktigt mätbara påverkan.

## <a name="next-steps"></a>Nästa steg

hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering
* [**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här
* [**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Tekniska ingående** ](active-directory-passwords-how-it-works.md) -gå bakom hello gardinen toounderstand hur det fungerar
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? -Svar tooquestions du alltid vill ha tooask
* [**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR

[Writeback]: ./media/active-directory-passwords-writeback/enablepasswordwriteback.png "Aktivera tillbakaskrivning av lösenord i Azure AD Connect"
