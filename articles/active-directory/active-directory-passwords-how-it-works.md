---
title: 'Djupdykning: Azure AD SSPR | Microsoft Docs'
description: "Azure AD Självbetjäning för lösenordsåterställning ingående"
services: active-directory
keywords: 
documentationcenter: 
author: MicrosoftGuyJFlo
manager: femila
ms.reviewer: gahug
ms.assetid: 618c5908-5bf6-4f0d-bf88-5168dfb28a88
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: joflore
ms.custom: it-pro
ms.openlocfilehash: c177192bbe69d179a25d174b06a0813ec28e2615
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="self-service-password-reset-in-azure-ad-deep-dive"></a>Självbetjäning för återställning av lösenord i Azure AD ingående

Hur fungerar SSPR? Vad innebär det alternativet i hello gränssnittet? Fortsätta att läsa toofind mer information om Azure AD-lösenordet för självbetjäning återställa.

## <a name="how-does-hello-password-reset-portal-work"></a>Hur hello lösenordsåterställning portal arbete

När användarna navigerar toohello lösenordsåterställning, portal, inletts ett arbetsflöde toodetermine:

   * Hur hello sidan vara lokaliserade?
   * Gäller hello användarkonto?
   * Vilken organisation hello användaren tillhör?
   * Där hello användarens lösenord hanteras?
   * Är hello användaren licensierad toouse hello funktion?


Läs igenom hello steg nedan toolearn om hello logiken bakom hello lösenord återställa sidan.

1. Användaren klickar på hello kan inte komma åt ditt kontolänk eller försätts direkt för[https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2. Baserat på hello webbläsarens språk hello upplevelse återges i hello språket. hello upplevelse för återställning av lösenord är lokaliserade till hello samma språk som har stöd för Office 365.
3. Användaren anger ett användar-id och skickar en captcha.
4. Azure AD verifierar om hello användaren är kan toouse funktionen hello följande:
   * Kontrollerar hello användaren har den här funktionen är aktiverad och en Azure AD-licens.
     * Om hello användaren inte har den här funktionen är aktiverad eller tilldelats en licens, hello användaren har och deras administratör tooreset svar toocontact sitt lösenord.
   * Kontrollerar hello användaren har hello rätt challenge data som definierats på sitt konto i enlighet med systemadministratören.
     * Om principen kräver bara en utmaning säkerställs hello användaren har hello lämpliga data har definierats för minst en av hello utmaningar aktiveras med Hej administratör princip.
       * Om hello användaren inte har konfigurerats och sedan hello användaren är uppmanade toocontact sina administratören tooreset sitt lösenord.
     * Om hello principen kräver två utmaningar, är det säkerställas hello användaren har hello lämpliga data har definierats för minst två av hello utmaningar aktiveras med Hej administratör princip.
       * Om hello användaren inte har konfigurerats, så vi hello användaren är uppmanade toocontact sina administratören tooreset sitt lösenord.
   * Kontrollerar om hello användarens lösenord hanteras lokalt (federerad eller synkroniseras lösenords-hash).
     * Om tillbakaskrivning distribueras och hello användarens lösenord hanteras lokalt hello användare tillåts tooproceed tooauthenticate och återställa sina lösenord.
     * Om tillbakaskrivning inte har distribuerats och hello användarens lösenord hanteras lokalt och sedan hello användaren tillfrågas toocontact sina administratören tooreset sitt lösenord.
5. Om det fastställs att hello-användaren kan toosuccessfully återställa sina lösenord och sedan leds hello användaren via hello återställningen.

## <a name="authentication-methods"></a>Autentiseringsmetoder

Om Self-Service lösenord återställa (SSPR) är aktiverat, måste du välja minst en av följande alternativ för autentiseringsmetoder hello. Vi rekommenderar starkt att välja minst två autentiseringsmetoder så att användarna har du mer flexibilitet.

* E-post
* Mobiltelefon
* Arbetstelefon
* Säkerhetsfrågor

### <a name="what-fields-are-used-in-hello-directory-for-authentication-data"></a>Vilka fält som används i hello directory för autentiseringsdata

* Arbetstelefon motsvarar tooOffice telefon
    * Användare kan tooset det här fältet själva definieras av administratören
* Mobiltelefon motsvarar tooeither telefon för autentisering (inte synligt offentligt) eller mobiltelefon (synligt offentligt)
    * hello tjänsten söker efter telefon för autentisering först, sedan faller tillbaka tooMobile Phone om de inte installerats
* Den alternativa e-postadressen motsvarar tooeither autentisering e-post som (inte synligt offentligt) eller alternativa e-post
    * hello service ser ut för e-post för autentisering först och sedan misslyckas tillbaka tooAlternate e-post

Som standard synkroniseras endast hello molnet attribut arbetstelefon och mobiltelefon är tooyour molnkatalog från din lokala katalog för autentiseringsdata.

Användare kan bara återställa sitt lösenord om de har data som finns i hello autentiseringsmetoder att hello-administratör har aktiverat och kräver.

Om användarna inte vill att deras nummer toobe mobiltelefon synliga i hello directory men fortfarande som toouse den för återställning av lösenord, Administratörer bör inte fylla det i hello directory och hello användaren bör fyller sina **telefon för autentisering**  attributet via hello [registreringsportalen för lösenordsåterställning](http://aka.ms/ssprsetup). Administratörer kan se den här informationen i hello användarprofil men publiceras inte någon annanstans. Om ett Azure-administratörskonto registrerar sina telefonnummer för autentisering, fylls i hello mobiltelefon fältet och är synligt.

### <a name="number-of-authentication-methods-required"></a>Antal autentiseringsmetoder krävs

Det här alternativet avgör hello minsta antalet tillgängliga hello autentiseringsmetoder som en användare måste gå igenom tooreset eller låsa upp sitt lösenord och kan ställas in tooeither 1 eller 2.

Användarna kan välja toosupply flera autentiseringsmetoder om de har aktiverats av Hej administratör.

Om en användare inte har hello minsta krävs metoder som är registrerad visas en felsida som hänvisar dem toorequest en administratör tooreset sitt lösenord.

### <a name="how-secure-are-my-security-questions"></a>Hur säker är min säkerhetsfrågor

Om du använder säkerhetsfrågor, rekommenderar vi dem används med en annan metod som de kan vara mindre säkert än andra metoder eftersom vissa personer vet svaren hello tooanother användare frågor.

> [!NOTE] 
> Säkerhetsfrågor lagras säkert och privat för en användare i hello directory och bara ska besvaras av användare under registreringen. Det går inte en administratör tooread eller ändra en användare frågor och svar.
>

### <a name="security-question-localization"></a>Översättning av säkerhet fråga

Alla fördefinierade frågor som följer är lokaliserade till hello fullständig uppsättning med Office 365 språk baserat på hello webbläsare Användarplats.

* I vilken stad träffade du din första make/maka/partner?
* I vilken stad träffades dina föräldrar?
* I vilken stad bor ditt närmsta syskon?
* I vilken stad föddes din pappa?
* I vilken stad hade du ditt första jobb?
* I vilken stad föddes din mamma?
* Vilken stad befann du dig i på nyårsafton 2000?
* Vad är hello efternamn hette din favoritlärare i hög * skola?
* Vad är ett av de universitet du har använt hello namn toobut inte deltar?
* Vad är hello namn hello plats där du hade din första bröllopsmottagning?
* Vilket är din pappas mellannamn?
* Vilken är din favoriträtt?
* Vad hette din mormor i för- och efternamn?
* Vilket är din mammas mellannamn?
* Vad är ditt äldsta syskon födelsedag månad och år? (t.ex. November 1985)
* Vilket är ditt äldsta syskons mellannamn?
* Vad hette din farfar i för- och efternamn?
* Vilket är ditt yngsta syskons mellannamn?
* Vilken skola gick du i när du gick i sjätte klass?
* Vad har hello först och efternamn din bästa barndomskompis?
* Vad har hello först och efternamn din första kärlek?
* Vad hette hello senaste hette din favoritlärare i grundskolan?
* Hello märke och vilken modell var din första bil eller motorcykel?
* Vad hette hello hello första skolan som du gick i?
* Vad hette hello hello sjukhus där du föddes?
* Vad hette hello hello barndomshem din första?
* Vad hette hello din barndomshjälte?
* Vad hette hello din favoritgosedjur?
* Vad hette hello ditt första husdjur?
* Vad hade du för smeknamn som barn?
* Vilken var din favoritsport i gymnasiet?
* Vilket var ditt första jobb?
* Vad har hello fyra sista siffrorna i telefonnummer du hade som barn?
* När du var barn vad drömde du toobe vuxen?
* Vem är hello mest berömda person som du har träffat?

### <a name="custom-security-questions"></a>Anpassade säkerhetsfrågor

Anpassade säkerhetsfrågor är inte lokaliserade för olika språk. Alla anpassade frågor visas i hello samma språk som de anges i hello administrativa användargränssnittet även om hello webbläsare Användarplats är olika. Använd hello fördefinierade frågor om du behöver lokaliserade frågor.

en anpassad säkerhetsfråga hello maxlängd är 200 tecken.

### <a name="security-question-requirements"></a>Fråga säkerhetskrav

* Minsta svaret tecken är 3 tecken
* Maximal svaret tecken är 40 tecken
* Användare kan inte svara på hello samma fråga mer än en gång
* Användare kan inte ange hello samma besvara toomore än en fråga
* Alla teckenuppsättningen kanske används toodefine frågor och svar inklusive Unicode-tecken
* hello antalet frågor som definierats måste vara större än eller lika med toohello antalet frågor krävs tooregister

## <a name="registration"></a>Registrering

### <a name="require-users-tooregister-when-signing-in"></a>Kräv användare tooregister när du loggar in

Aktivera det här alternativet måste en användare som har aktiverats för lösenord återställs toocomplete hello registreringen för lösenordsåterställning om de logga in tooapplications med hjälp av Azure AD toosign i som de som följer:

* Office 365
* Azure Portal
* Åtkomstpanel
* Federerade program
* Anpassade program med Azure AD

Inaktiverar den här funktionen kan fortfarande användare toomanually registrera kontaktuppgifter genom att besöka [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) eller genom att klicka på hello **registrera dig för lösenordsåterställning** länken under hello fliken profil i hello åtkomstpanelen.

> [!NOTE]
> Användare kan ignorera hello registreringsportalen för lösenordsåterställning genom att klicka på Avbryt eller stänger ett fönster hello men uppmanas varje gång de loggar in förrän de har slutfört registreringen.
>

### <a name="number-of-days-before-users-are-asked-tooreconfirm-their-authentication-information"></a>Antal dagar innan användarna uppmanas ange tooreconfirm sina autentiseringsinformation

Det här alternativet avgör hello tidsperiod mellan inställningen och reconfirming autentiseringsinformationen och är endast tillgängligt om du aktiverar hello **kräver användare tooregister när du loggar in** alternativet.

Giltiga värden är 0-730 dagar med 0 vilket innebär att fråga inte användare tooreconfirm sin autentiseringsinformation

## <a name="notifications"></a>Meddelanden

### <a name="notify-users-on-password-resets"></a>Meddela användare om lösenordsåterställning

Om det här alternativet anges tooyes får ett e-postmeddelande till dem att lösenordet har ändrats via hello SSPR portal tootheir primära och alternativa-e-postadresser på filen i Azure AD med hello-användare som återställning av lösenordet. Ingen annan får ett meddelande om återställning av den här händelsen.

### <a name="notify-all-admins-when-other-admins-reset-their-passwords"></a>Meddela alla administratörer när andra administratörer återställa sina lösenord

Om det här alternativet anges tooyes, sedan **alla administratörer** får ett e-tootheir primära e-postadress på filen i Azure AD meddela dem om att en annan administratör har ändrat sitt lösenord med hjälp av SSPR.

Exempel: Det finns fyra administratörer i en miljö. Administratören ”A” återställa sina lösenord med hjälp av SSPR. Administratörer B och C D får ett e-postmeddelande Varna dem om det fortfarande händer.

## <a name="on-premises-integration"></a>Lokal integrering

Om du har installerat, konfigurerats och aktiverats Azure AD Connect har du ytterligare alternativ för lokal integreringar.

### <a name="write-back-passwords-tooyour-on-premises-directory"></a>Skriv tillbaka lösenord tooyour lokal katalog

Styr huruvida tillbakaskrivning av lösenord är aktiverat för den här katalogen och om tillbakaskrivning finns på, anger hello status hello lokala tillbakaskrivning av tjänsten. Detta är användbart om du vill tootemporarily inaktivera hello tillbakaskrivning av lösenord utan att konfigurera Azure AD Connect igen.

* Om hello växlar är uppsättningen tooyes tillbakaskrivning ska aktiveras och federerad och lösenord hash-synkroniserade användarna kommer att kunna tooreset sina lösenord.
* Om hello växlar är uppsättningen toono tillbakaskrivning ska vara inaktiverade och federerad och lösenord hash-synkroniserade användare kommer inte att kunna tooreset sina lösenord.

### <a name="allow-users-toounlock-accounts-without-resetting-their-password"></a>Tillåt att användare toounlock konton utan att återställa sina lösenord

Anger huruvida användare som besöker hello-portalen för återställning av lösenord ska vara angivna hello alternativet toounlock sina lokala Active Directory-konton utan att återställa sina lösenord. Standard Azure AD kommer alltid att låsas upp konton när du utför en återställning av lösenord, den här inställningen kan du tooseparate de två åtgärderna. 

* Om anges för ”yes”, och användarna vara angivna hello alternativet tooreset sina lösenord och låsa upp kontot hello eller toounlock utan att återställa lösenordet för hello.
* Om inställningen för ”Nej”, användare kommer bara att kunna tooperform en kombinerad lösenordsåterställning och kontot låsa upp igen.

## <a name="network-requirements"></a>Nätverkskrav

### <a name="firewall-rules"></a>Brandväggsregler

[Lista över URL: er för Microsoft Office och IP-adresser](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)

För Azure AD Connect version 1.1.443.0 och senare, du behöver utgående HTTPS åtkomst toohello följande
* passwordreset.microsoftonline.com
* servicebus.Windows.NET

För mer detaljerade åtkomst, du kan hitta hello uppdatera listan över Microsoft Azure Datacenter IP-intervall som uppdateras varje onsdag och placera i kraft hello följande måndag [här](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="idle-connection-timeouts"></a>Timeout för inaktiv anslutning

hello Azure AD Connect-verktyget skickar regelbundet pingar/keepalive-överföringar tooServiceBus slutpunkter tooensure att hello anslutningar förblir aktiv. Bör hello verktyget identifierar att för många anslutningar avslutats, ökas automatiskt hello frekvensen av pingar toohello slutpunkt. hello lägsta ”pinga intervall' således toois 1 ping var 60: e sekund men vi rekommenderar starkt att proxyservrar/brandväggar tillåter inaktiva anslutningar toopersist minst 2-3 minuter. * För äldre versioner rekommenderar vi fyra minuter eller mer.

## <a name="active-directory-permissions"></a>Active Directory-behörigheter

hello konto har angetts i hello Azure AD Connect-verktyget måste ha Återställ lösenord, ändra lösenord, skrivbehörighet på lockoutTime och skrivbehörighet på pwdLastSet, utökade rättigheter på antingen hello rotobjektet av **varje domän** i den skogen **eller** på hello användaren organisationsenheter som du vill att toobe i omfånget för SSPR.

Om du inte är säker avser vilka konto hello ovan, öppna hello Azure Active Directory Connect Konfigurationsgränssnittet och klickar på hello Visa aktuella konfigurationsalternativet. hello-konto som du behöver tooadd behörighet toois som anges under ”synkroniseras kataloger”

Dessa behörigheter kan hello MA-tjänstkontot för varje skog toomanage lösenord för användarkonton i skogen. **Om du inte tooassign dessa behörigheter sedan, även om tillbakaskrivning visas toobe som konfigurerats på rätt sätt, stöter användarna på fel vid försök toomanage sina lokala lösenord från hello molnet.**

> [!NOTE]
> Det kan ta upp tooan timme eller mer för dessa behörigheter tooreplicate tooall objekt i katalogen.
>

tooset hello lämpliga behörigheter för toooccur för tillbakaskrivning av lösenord

1. Öppna Active Directory-användare och datorer med ett konto som har administratörsbehörighet för hello lämplig domän
2. Kontrollera att avancerade funktioner är aktiverat hello Visa-menyn
3. Högerklicka på hello-objekt som representerar hello roten hello domän i hello till vänster och välj Egenskaper
    * Klicka på fliken för hello-säkerhet
    * Klicka på Avancerat.
4. Klicka på Lägg till från fliken hello behörighet
5. Välj hello konto att behörigheter som tillämpas för (från Azure AD Connect-installation)
6. Välj underordnade objekt i hello gäller toodrop listrutan
7. Under behörigheter kryssrutorna hello för hello följande
    * Unexpire lösenord
    * Återställ lösenord
    * Ändra lösenord
    * Skriva lockoutTime
    * Skriva pwdLastSet
8. Klicka på Använd/OK via tooapply och avsluta alla öppna dialogrutor.

## <a name="how-does-password-reset-work-for-b2b-users"></a>Hur lösenordsåterställning för B2B-användare?
Återställning av lösenord och ändra stöds helt med alla B2B-konfigurationer.  Läsa nedan för hello tre explicit B2B fall stöds av lösenordsåterställning.

1. **Användare från en partnerorganisationen med en befintlig Azure AD-klient** - om hello organisation du samarbetar med har en befintlig Azure AD-klient vi **respektera oavsett principer för återställning av lösenord är aktiverade i den klienten**. För lösenord återställa toowork, hello partner organisation bara behov toomake till Azure AD SSPR är aktiverad, som är utan extra kostnad för O365 kunder, och kan aktiveras genom att följa stegen hello i vår [komma igång med lösenordshantering ](https://azure.microsoft.com/documentation/articles/active-directory-passwords-getting-started/#enable-users-to-reset-or-change-their-aad-passwords) guide.
2. **Användare som har registrerat med [självbetjäning registrering](active-directory-self-service-signup.md)**  - om hello organisation du samarbetar med används hello [självbetjäning anmälan](active-directory-self-service-signup.md) funktionen tooget till en klient, som vi låter dem återställa med de registrerade hello e-post.
3. **B2B användare** -B2B användare skapas med hjälp av hello ny [Azure AD B2B-funktioner](active-directory-b2b-what-is-azure-ad-b2b.md) kommer att kunna tooreset sina lösenord med hello e-postmeddelande de registrerade under hello inbjudan.

tootest detta, gå toohttp://passwordreset.microsoftonline.com med något av dessa partner-användare. Så länge som de har en alternativ e-postadress eller autentisering e-definitionen för lösenordsåterställning fungerar som förväntat.

## <a name="next-steps"></a>Nästa steg

hello följande länkar ger ytterligare information om lösenordsåterställning med hjälp av Azure AD

* [**Snabbstart** ](active-directory-passwords-getting-started.md) – Kom igång med självbetjäningsfunktionen för återställning av lösenord i Azure AD 
* [**Licensiering**](active-directory-passwords-licensing.md) – Konfigurera Azure AD-licensiering
* [**Data** ](active-directory-passwords-data.md) – förstå hello data som krävs och hur de används för lösenordshantering
* [**Distributionen** ](active-directory-passwords-best-practices.md) -planera och distribuera SSPR tooyour användare som använder hello vägledning finns här
* [**Princip**](active-directory-passwords-policy.md) – Förstå och ange principer för Azure AD-lösenord
* [**Tillbakaskrivning av lösenord**](active-directory-passwords-writeback.md) – Hur fungerar tillbakaskrivning av lösenord tillsammans med din lokala katalog?
* [**Anpassa** ](active-directory-passwords-customize.md) -anpassa hello utseende och känslan av hello SSPR upplevelse för ditt företag.
* [**Rapportering**](active-directory-passwords-reporting.md) – Identifiera om, när och var dina användare kommer åt SSPR-funktioner
* [**Vanliga frågor och svar**](active-directory-passwords-faq.md) – Hur gör man? Varför? Vad? Var? Vem? När? -Svar tooquestions du alltid vill ha tooask
* [**Felsöka** ](active-directory-passwords-troubleshoot.md) – Lär dig hur tooresolve vanliga problem att vi se med SSPR

