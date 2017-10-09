---
title: aaaConfigure Azure MFA | Microsoft Docs
description: "Det här är hello Azure Multi-Factor authentication sida som beskriver vilka toodo bredvid med MFA.  Detta inkluderar rapporter, bedrägerivarning, engångsförbikoppling, anpassade röstmeddelanden som cachelagring, tillförlitliga IP-adresser och app-lösenord."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: kgremban
ms.openlocfilehash: 7f6d0b0975a2c1da2de9b52e978b84475c79b218
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Konfigurera Azure Multi-Factor Authentication-inställningar
Den här artikeln hjälper dig att hantera Azure Multi-Factor Authentication nu när du är igång.  Den omfattar olika avsnitt som hjälper dig tooget hello mesta möjliga av Azure Multi-Factor Authentication.  Inte alla dessa funktioner är tillgängliga i alla versioner av Azure Multi-Factor Authentication.

| Funktion | Beskrivning | 
|:--- |:--- |
| [Bedrägerivarning](#fraud-alert) |Bedrägerivarning kan konfigureras och ställa in så att användarna kan rapportera bedrägliga försök tooaccess sina resurser. |
| [Engångsförbikoppling](#one-time-bypass) |En engångsförbikoppling kan en användare tooauthenticate en gång genom att ”kringgå” multifaktorautentisering. |
| [Anpassade röstmeddelanden](#custom-voice-messages) |Anpassade röstmeddelanden Tillåt toouse egna inspelningar eller med multifaktorautentisering. |
| [Cachelagring](#caching-in-azure-multi-factor-authentication) |Cachelagring kan du tooset en viss tid så att efterföljande autentiseringsförsök lyckas automatiskt. |
| [Tillförlitliga IP-adresser](#trusted-ips) |Administratörer av en hanterad eller extern klient kan använda betrodda IP-adresser toobypass tvåstegsverifiering för användare som loggar in från hello lokalt intranät. |
| [Applösenord](#app-passwords) |Ett applösenord kan ett program som inte är medveten om MFA toobypass multifaktorautentisering och fortsätta arbeta. |
| [Kom ihåg Multi-Factor Authentication för sparade enheter och webbläsare](#remember-multi-factor-authentication-for-devices-that-users-trust) |Tillåter tooremember enheter för ett visst antal dagar efter att en användare har loggat in med MFA. |
| [Valbar verifieringsmetoderna](#selectable-verification-methods) |Låter dig toochoose hello autentiseringsmetoder som är tillgängliga för användare toouse. |

## <a name="access-hello-azure-mfa-management-portal"></a>Komma åt hello Azure MFA-hanteringsportalen

hello-funktioner som beskrivs i den här artikeln har konfigurerats i hello Azure Multi-Factor Authentication-hanteringsportalen. Det finns två sätt tooaccess hello MFA-hanteringsportalen via hello klassiska Azure-portalen. hello är först genom att hantera en leverantör av Multifaktorautent. hello är andra via hello MFA-tjänstinställningar. 

### <a name="use-an-auth-provider"></a>Använd en leverantör av Multifaktorautent

Använd den här metoden tooaccess hello-hanteringsportalen om du använder en leverantör av Multifaktorautent för förbrukningsbaserad MFA.

tooaccess Hej MFA-hanteringsportalen via en Azure leverantör av Multifaktorautent, logga in på hello klassiska Azure-portalen som administratör och välj hello Active Directory-alternativet. Klicka på hello **Flerfunktionsautentiseringsleverantörer** fliken, Välj din katalog och på hello **hantera** knappen längst ned hello.

### <a name="use-hello-mfa-service-settings-page"></a>Använd sidan för hello MFA inställningar 

Om du har en leverantör av Multifaktorautent eller ett Azure MFA, Azure AD Premium eller Enterprise Mobility + Security licens, Använd den här metoden tooaccess hello MFA-tjänsten inställningssidan.

tooaccess hello MFA-hanteringsportalen via hello sidan för MFA inställningar, logga in på hello klassiska Azure-portalen som administratör och väljer alternativet för hello Active Directory. Klicka på din katalog och klicka sedan på hello **konfigurera** fliken. Markera under hello multifaktorautentisering avsnittet **hantera tjänstinställningar**. Hello längst ned på sidan för hello MFA inställningar klickar du på hello **gå toohello portal** länk.


## <a name="fraud-alert"></a>Bedrägerivarning
Bedrägerivarning kan konfigureras och ställa in så att användarna kan rapportera bedrägliga försök tooaccess sina resurser.  Användare kan rapportera bedrägeri med hello mobila appar eller via telefonen.

### <a name="set-up-fraud-alert"></a>Ställ in bedrägerivarning
1. Navigera toohello MFA-hanteringsportalen per hello instruktioner hello överst i den här sidan.
2. Klicka på hello Azure Multi-Factor Authentication-hanteringsportalen, **inställningar** under hello konfigurationsavsnittet.
3. Kontrollera hello under hello bedrägeriförsök avsnitt i hello inställningssidan **användarna toosubmit Bedrägerivarningar** kryssrutan.
4. Välj **spara** tooapply ändringarna. 

### <a name="configuration-options"></a>Konfigurationsalternativ

- **Blockera användare när bedrägeri rapporteras** – om en användare rapporter bedrägeri, sitt konto blockeras.
- **Code tooReport bedrägeri under inledande hälsning** -användare vanligtvis trycka på # tooconfirm tvåstegsverifiering. Om de vill tooreport bedrägeri anger de en kod innan du trycker på #. Den här koden är **0** som standard, men du kan anpassa den.

> [!NOTE]
> Microsofts standard rösthälsning Instruera användarna toopress 0# toosubmit en bedrägerivarning. Om du vill toouse en kod än 0, bör du registrera och överföra din egen anpassade rösthälsning med lämpliga instruktioner.

![Bedrägeri aviseringsalternativ – skärmbild](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="how-users-report-fraud"></a>Hur användare rapportera bedrägeri 
Bedrägerivarning rapporteras på två sätt.  Antingen via hello mobila appar eller via telefon hello.  

#### <a name="report-fraud-with-hello-mobile-app"></a>Rapportera bedrägeri med hello mobila appar
1. När en verifiering skickas tooyour telefon, väljer du den tooopen hello Microsoft Authenticator-appen.
2. Välj **neka** på hello-meddelande. 
3. Välj **rapportera bedrägeri**.
4. Stäng hello app.

#### <a name="report-fraud-with-a-phone"></a>Rapportera bedrägeri med en telefon
1. När det gäller ett verifieringssamtal i tooyour phone, svara på samtalet.  
2. tooreport bedrägeri, ange hello bedrägerikod (standardvärdet är 0) och hello #-tecken. Du meddelas att en bedrägerivarning har skickats.
3. Avsluta hello samtal.

### <a name="view-fraud-reports"></a>Visa bedrägeri rapporter
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Välj Active Directory hello vänster.
3. Vid hello översta Välj **Flerfunktionsautentiseringsleverantörer**. Nu visas en lista över dina Flerfunktionsautentiseringsleverantörer.
4. Välj din leverantör av Multifaktorautent och klicka på **hantera** på hello hello sidans nederkant. hello Azure Multi-Factor Authentication-hanteringsportalen öppnas.
5. Klicka på hello Azure Multi-Factor Authentication-hanteringsportalen, under Visa en rapport **bedrägeriförsök**.
6. Ange datumintervall för hello som du vill tooview i hello rapporten. Du kan också ange användarnamn och telefonnummer hello användarens status.
7. Klicka på **Run** (Kör). Detta öppnar en rapport över bedrägerivarningar. Klicka på **exportera tooCSV** om du inte vill tooexport hello rapporten.

## <a name="one-time-bypass"></a>Engångsförbikoppling
En engångsförbikoppling kan en användare tooauthenticate en gång utan att utföra tvåstegsverifiering. Hej förbikopplingen är tillfällig och upphör att gälla efter ett angivet antal sekunder. I situationer där hello-mobilappen eller phone inte tar emot ett meddelande eller telefonsamtal, aktiverar du en engångsförbikoppling så hello användare kan komma åt hello önskad resurs.

### <a name="create-a-one-time-bypass"></a>Skapa en engångsförbikoppling
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Navigera toohello MFA-hanteringsportalen per hello instruktioner hello överst i den här sidan.
3. I hello Azure Multi-Factor Authentication-hanteringsportalen, om du ser hello namnet på din eller Azure MFA-leverantör på hello lämnas med en  **+**  nästa tooit klickar du på hello  **+**  finns i olika replikeringsgrupper för MFA-servern och hello Azure-standard grupp. Välj lämplig hello-grupp.
4. Under Administration av användaren väljer **Engångsförbikoppling**.
5. På hello Engångsförbikoppling klickar du på **nya Engångsförbikoppling**.

  ![Molnet](./media/multi-factor-authentication-whats-next/create1.png)

6. Ange hello användarnamn och hello antalet sekunder som hello kringgå kommer finnas och hello orsaken till hello kringgå. Klicka på **kringgå**.
7. hello träder tidsgränsen i kraft omedelbart, så hello användaren måste toosign i innan hello enstaka kringgå upphör att gälla. 

### <a name="view-hello-one-time-bypass-report"></a>Visa hello enstaka kringgå rapport
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Välj Active Directory hello vänster.
3. Vid hello översta Välj **Flerfunktionsautentiseringsleverantörer**. Nu visas en lista över dina Flerfunktionsautentiseringsleverantörer.
4. Välj din leverantör av Multifaktorautent och klicka på **hantera** på hello hello sidans nederkant. hello Azure Multi-Factor Authentication-hanteringsportalen öppnas.
5. Klicka på hello Azure Multi-Factor Authentication-hanteringsportalen under Visa en rapport i vänster hello **Engångsförbikoppling**.
6. Ange datumintervall för hello som du vill tooview i hello rapporten. Du kan också ange användarnamn och telefonnummer hello användarens status.
7. Klicka på **Run** (Kör). Detta öppnar en rapport över förbikopplingar. Klicka på **exportera tooCSV** om du inte vill tooexport hello rapporten.

## <a name="custom-voice-messages"></a>Anpassade röstmeddelanden
Anpassade röstmeddelanden Tillåt toouse egna inspelningar eller helg för tvåstegsverifiering. Dessa kan användas i tillägg tooor tooreplace hello Microsoft poster.

Innan du börjar ska du vara medveten om följande hello:

* hello stöds filformat är .wav eller .mp3.
* hello storleksbegränsningen är 5 MB.
* Autentiseringsmeddelanden måste vara kortare än 20 sekunder. Något längre tid än det kan leda till hello verifiering toofail eftersom hello användaren inte svarar innan hello meddelandet slutförs, orsakar hello verifiering tootime ut.

### <a name="set-up-a-custom-message"></a>Konfigurera ett anpassat meddelande

Det finns två delar toocreating anpassade meddelandet. Först du överför hello-meddelande och sedan du aktivera den för användarna.

tooupload ditt anpassade meddelande:

1. Skapa en anpassad röstmeddelande med något av hello stöds.
2. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
3. Navigera toohello MFA-hanteringsportalen per hello instruktioner hello överst i den här sidan.
4. Klicka på hello Azure Multi-Factor Authentication-hanteringsportalen, **röstmeddelanden** under hello konfigurationsavsnittet.
5. På hello konfigurera: röst meddelanden sidan, klickar du på **Nytt röstmeddelande**.
   ![Molnet](./media/multi-factor-authentication-whats-next/custom1.png)
6. På hello konfigurera: nya röstmeddelanden sidan, klickar du på **hantera ljudfiler**.
   ![Molnet](./media/multi-factor-authentication-whats-next/custom2.png)
7. På hello konfigurera: låter sidan filer genom att klicka på **överför ljudfilen**.
   ![Molnet](./media/multi-factor-authentication-whats-next/custom3.png)
8. På hello konfigurera: Överför ljud-fil klickar du på **Bläddra** och navigerar tooyour röstmeddelande, klickar på **öppna**.
9. Lägg till en beskrivning och klicka på **överför**.
10. När denna är klar, ett meddelande som bekräftar att du har överfört hello-filen.

tooturn hello-meddelande på för dina användare:

1. Klicka på vänster hello **röstmeddelanden**.
2. Under hello röstmeddelanden avsnittet, klickar du på **Nytt röstmeddelande**.
3. Välj ett språk från hello språket i listrutan.
4. Ange hello programmet i rutan om det här meddelandet är för ett visst program.
5. Hello meddelandetypen listrutan, Välj hello meddelandet typen toobe åsidosätts med ditt nya anpassade meddelande.
6. Hello ljudfilen listrutan, Välj hello ljudfilen som du överfört hello första del.
7. Klicka på **Skapa**. Ett meddelande bekräftar att du har skapat ett röstmeddelande.
    ![Molnet](./media/multi-factor-authentication-whats-next/custom5.png)</center>

## <a name="caching-in-azure-multi-factor-authentication"></a>Cachelagring i Azure Multi-Factor Authentication
Cachelagring kan du tooset en viss tid så att efterföljande autentiseringsförsök inom denna tidsperiod lyckas automatiskt. Det här används främst när lokalt system som VPN skickar flera förfrågningar för verifiering när hello första begäran pågår fortfarande. Detta gör att hello efterföljande förfrågningar toosucceed automatiskt när användaren hello lyckas hello första kontroll pågår. 

Cachelagring är inte avsedda toobe som används för inloggningar tooAzure AD.

### <a name="set-up-caching"></a>Konfigurera cachelagring 
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Navigera toohello MFA-hanteringsportalen per hello instruktioner hello överst i den här sidan.
3. Klicka på hello Azure Multi-Factor Authentication-hanteringsportalen, **cachelagring** under hello konfigurationsavsnittet.
4. På hello konfigurera cachelagring sidan klickar du på **nytt cacheminne**.
5. Välj hello cachetyp och hello cachelagringssekunder. Klicka på **Skapa**.

<center>![Moln](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Tillförlitliga IP-adresser
Tillförlitliga IP-adresser är en funktion i Azure MFA som administratörer av en hanterad eller extern klient kan använda toobypass tvåstegsverifiering för användare som loggar in från hello lokalt intranät. Den här funktionen är tillgänglig med hello fullständiga versionen av Azure Multi-Factor Authentication inte hello gratisversionen för administratörer. Mer information om hur tooget hello fullständiga versionen av Azure Multi-Factor Authentication finns [Azure Multi-Factor Authentication](multi-factor-authentication.md).

| Typ av Azure AD-klient | Alternativen för tillförlitlig IP |
|:--- |:--- |
| Hanterad |<li>Specifik IP-adressintervall – administratörer kan ange ett intervall med IP-adresser som kan kringgå tvåstegsverifiering för användare som loggar in från hello företagets intranät.</li> |
| Federerad |<li>Alla externa användare – alla federerade användare registrerar i från inuti hello organisation kringgå tvåstegsverifiering med hjälp av ett anspråk som utfärdats av AD FS.</li><br><li>Specifik IP-adressintervall – administratörer kan ange ett intervall med IP-adresser som kan kringgå tvåstegsverifiering för användare som loggar in från hello företagets intranät. |

För att kringgå fungerar bara från i företagets intranät. Om du har valt alla federerade användare, och en användare loggar in från utanför hello företags intranät, har användaren tooauthenticate med tvåstegsverifiering även om hello användaren anger ett AD FS-anspråk. 

**Slutanvändarens upplevelse i corpnet:**

När tillförlitliga IP-adresser är inaktiverat tvåstegsverifiering krävs för webbläsaren flöden och applösenord krävs för äldre klientappar. 

När tillförlitliga IP-adresser aktiveras tvåstegsverifiering är *inte* krävs för webbläsaren flöden och applösenord är *inte* krävs för äldre appar rich-klient, förutsatt att hello användaren inte har redan skapat ett applösenord. När ett applösenord används kan fortfarande det krävs. 

**Slutanvändarens upplevelse utanför corpnet:**

Om betrodda IP-adresser är aktiverad eller inte kan tvåstegsverifiering krävs för webbläsaren flöden och applösenord krävs för äldre klientappar. 

### <a name="tooenable-trusted-ips"></a>tooenable tillförlitliga IP-adresser
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Navigera toohello MFA tjänstinställningar sida per hello instruktioner hello början av den här artikeln.
3. Hello-tjänsten på sidan Inställningar under betrodda IP-adresser, har du två alternativ:
   
   * **För förfrågningar från externa användare som kommer från intranätet** – hello kryssrutan. Alla externa användare som loggar in från hello företagsnätverk kringgå tvåstegsverifiering med hjälp av ett anspråk som utfärdats av AD FS.
   * **För begäranden från en viss mängd offentliga IP-adresser** – ange hello IP-adresser i hello textrutan med CIDR-notering. Till exempel: xxx.xxx.xxx.0/24 för IP-adresser i hello intervallet xxx.xxx.xxx.1 – xxx.xxx.xxx.254 eller xxx.xxx.xxx.xxx/32 för en enda IP-adress. Du kan ange upp too50 IP-adressintervall. Användare som loggar in från dessa IP-adresser kringgå tvåstegsverifiering.
4. Klicka på **Spara**.
5. När hello-uppdateringarna har tillämpats, klickar du på **Stäng**.

![Tillförlitliga IP-adresser](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>Applösenord
Vissa appar, t.ex. Office 2010 eller senare och Apple Mail stöder inte tvåstegsverifiering. De är inte konfigurerade tooaccept en andra verifiering. toouse dessa appar du behöver toouse ”applösenord” istället för lösenordet traditionella. Hej applösenord kan hello programmet toobypass tvåstegsverifiering och fortsätta arbeta.

> [!NOTE]
> Modern autentisering för hello Office 2013-klienter
> 
> Office 2013-klienter (inklusive Outlook) och nyare stöd för modern autentiseringsprotokoll och kan vara aktiverad toowork med tvåstegsverifiering. När du har aktiverat, krävs inte applösenord för dessa klienter.  Mer information finns i [Office 2013 modern autentisering offentlig förhandsgranskning av](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

### <a name="important-things-tooknow-about-app-passwords"></a>Viktiga saker tooknow om applösenord
hello följande är ett viktigt lista över saker som du bör känna till om applösenord.

* Applösenord behöver bör bara toobe anges en gång per app. Användare inte har tookeep reda på dem och ange dem varje gång.
* hello faktiska lösenordet genereras automatiskt och tillhandahålls inte av hello användare. Detta beror på att hello automatiskt genererade lösenordet är svårare för en angripare tooguess och är säkrare.
* Det finns en gräns på 40 lösenord per användare. 
* Appar som cachelagrar lösenord och använder den i lokala scenarier kan bli felaktiga eftersom hello applösenordet inte känns igen utanför hello organisations-id. Ett exempel är Exchange e-postmeddelanden som är lokalt men hello arkiverade e-post finns i hello moln. hello fungerar samma lösenord inte.
* När multifaktorautentisering har aktiverats på en användares konto, applösenord kan användas med de flesta icke-webbläsarklienter som Outlook och Lync, men administrativa åtgärder kan utföras med hjälp av applösenord med icke-webbläsarprogram, till exempel Windows PowerShell även om användaren har ett administratörskonto.  Se till att du skapar ett tjänstkonto med ett starkt lösenord toorun PowerShell-skript och aktivera inte kontot för tvåstegsverifiering.

> [!WARNING]
> Applösenord fungerar inte i hybridmiljöer där klienter kommunicera med både lokalt och molnet slutpunkter för automatisk upptäckt. Detta beror på att domänlösenord är obligatoriska tooauthenticate lokala och applösenord är obligatoriska tooauthenticate hello moln.

### <a name="naming-guidance-for-app-passwords"></a>Riktlinjer för Applösenord för namngivning
Applösenord bör avspegla hello enheten som används. Till exempel om du har en bärbar dator som har icke-webbläsarappar, till exempel Outlook, Word och Excel, skapa ett applösenord med namnet bärbar och använda det applösenordet i dessa program. Skapa sedan en annan applösenord med namnet skrivbordet för hello samma program på din dator. 

Microsoft rekommenderar att skapa ett applösenord per enhet, inte ett applösenord per program.

### <a name="federated-sso-app-passwords"></a>Applösenord för federerade (SSO)
Azure AD stöder federation (enkel inloggning) med lokala Windows Server Active Directory Domain Services (AD DS). Om din organisation är federerat med Azure AD och du kommer toobe med Azure Multi-Factor Authentication och sedan hello är följande information om applösenord viktiga för dig. Det här avsnittet gäller endast toofederated (SSO) kunder.

* Applösenord verifieras av Azure AD och därför kringgå federation. Federation används bara aktivt när du ställer in applösenord.
* För federerade (SSO) användare går vi aldrig toohello identitetsprovider (IdP) till skillnad från hello passiva flödet. hello lösenord lagras i organisations hello-id. Om hello användaren lämnar företaget hello, har denna information tooflow tooorganizational id via DirSync i realtid. Inaktivering/borttagning av konto kan ta toothree timmar toosync, vilket fördröjer inaktivering/borttagning av Applösenord i Azure AD.
* Inställningar för lokal klientåtkomstkontroll respekteras inte av applösenord.
* Ingen lokal autentisering loggning/granskning kapaciteten är tillgänglig för Applösenord.
* Vissa avancerade arkitektur Designer kan kräva en kombination av organisationens användarnamn och lösenord och lösenord när du använder tvåstegsverifiering med klienter, beroende på var de autentiseras. För klienter som autentiseras mot en lokal infrastruktur, använder du en organisations användarnamn och lösenord. För klienter som autentiseras mot Azure AD, använder du hello applösenord.

  Anta att du har en arkitektur som består av följande hello:

  * Du federerar din lokala Active Directory med Azure AD-instans
  * Du använder Exchange online
  * Du använder Lync som är specifikt lokalt
  * Du använder Azure Multi-Factor Authentication

  ![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

  I så fall måste du göra hello följande:

  * När du loggar – i tooLync, använda din organisationer användarnamn och lösenord.
  * När försök tooaccess hello adressboken via ett Outlook-klienten som ansluter tooExchange online kan du använda ett applösenord.

### <a name="allow-app-password-creation"></a>Tillåt att appen lösenord skapas
Användare kan inte skapa applösenord som standard. Den här funktionen måste vara aktiverat. tooallow användare hello möjlighet toocreate applösenord använder hello nedan:

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Navigera toohello MFA tjänstinställningar sida per hello instruktioner hello början av den här artikeln.
3. Välj hello knappen bredvid för**Tillåt användare toocreate app lösenord toosign till icke-webbläsarappar**.

![Skapa Applösenord](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Skapa applösenord
Användare kan skapa applösenord vid första registreringen. De möjlighet hello slutet av registreringsprocessen för hello som gör att de toocreate applösenord.

Användare kan också skapa applösenord efter registreringen genom att ändra deras inställningar i hello Azure-portalen eller hello Office 365-portalen. Mer information och detaljerade anvisningar för dina användare kan se [vad är applösenord i Azure Multi-Factor Authentication](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Kom ihåg Multifaktorautentisering för enheter som har förtroende för användare
Komma ihåg Multifaktorautentisering för enheter och webbläsare att användare förtroende är en kostnadsfri funktion för alla MFA-användare. Det gör att du toogive användare hello alternativet tooby pass MFA för ett visst antal dagar efter en lyckad logga in med MFA. Detta kan förbättra användbarhet genom att minimera hello antalet gånger som en användare kan utföra tvåstegsverifiering på hello samma enhet.

Men om ett konto eller en enhet har komprometterats bör kan komma ihåg MFA för betrodda enheter påverka säkerheten. Om ett företagskonto har komprometterats eller en betrodd enhet tappas bort eller blir stulen, bör du [Återställ Multifaktorautentisering på alla enheter](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Återkallar den här åtgärden hello betrodda status från alla enheter och hello användaren är obligatoriska tooperform tvåstegsverifiering igen. Du kan också instruera din användare toorestore MFA på sina egna enheter med hello instruktioner i [hantera inställningar för tvåstegsverifiering](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted)

### <a name="how-it-works"></a>Hur det fungerar

Kom ihåg Multi-Factor Authentication fungerar genom att ange en beständig cookie hello webbläsare när en användare checkar hello ”fråga inte igen för **X** dagar” kryssrutan vid inloggning. hello användaren att inte uppmanas MFA igen från den Browser tills hello cookien förfaller. Om hello användare öppnar en annan webbläsare på hello samma enhet eller raderar sitt cookies, de kan inte ange tooverify igen. 

Hej ”fråga inte igen för **X** dagar” kryssrutan inte visas i icke-webbläsarappar, oavsett om de stöder modern autentisering. De här apparna använda uppdaterings-tokens som ger nya åtkomsttoken varje timme. När en uppdateringstoken verifieras var Azure AD-kontroller som hello senaste gången tvåstegsverifiering har utfört inom hello konfigurerade antalet dagar. 

Därför kan komma ihåg Multifaktorautentisering på betrodda enheter minskar hello Antal autentiseringar i webbprogram (som normalt fråga varje gång) men ökar hello Antal autentiseringar för modern autentisering klienter (vilket normalt fråga efter 90 dagar).

> [!NOTE]
>Den här funktionen är inte kompatibel med hello ”jag vill förbli inloggad”-funktionen i AD FS när användare utför tvåstegsverifiering för AD FS via hello Azure MFA-Server eller en MFA-lösning från tredje part. Om användarna Markera ”jag vill förbli inloggad” på AD FS och även markera sin enhet som betrodd för MFA, de inte kan tooverify när hello ”Kom ihåg MFA” antal dagar upphör att gälla. Azure AD begär en ny tvåstegsverifiering, men AD FS returnerar en token med hello ursprungliga MFA anspråk och datumet i stället för att utföra tvåstegsverifiering igen. Anger av en kontroll skapas mellan Azure AD och AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Aktivera Kom ihåg multifaktorautentisering
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Navigera toohello MFA tjänstinställningar sida per hello instruktioner hello början av den här artikeln.
3. På sidan för hello inställningar under hantering av Enhetsinställningar för användaren markerar hello **användarna tooremember multifaktorautentisering på enheter de litar** rutan.
   ![Kom ihåg enheter](./media/multi-factor-authentication-whats-next/remember.png)
4. Ange hello antalet dagar som du vill tooallow hello betrodda enheter toobypass tvåstegsverifiering. hello standardvärdet är 14 dagar.
5. Klicka på **Spara**.
6. Klicka på **Stäng**.

### <a name="mark-a-device-as-trusted"></a>Markera en enhet som betrodd

När du aktiverar den här funktionen kan användare kan markera en enhet som betrodda när de loggar in genom att kontrollera **fråga inte igen**.

![Fråga inte igen – skärmbild](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Valbar verifieringsmetoderna
Du kan välja vilka verifieringsmetoderna är tillgängliga för användarna. hello tabellen nedan innehåller en kort översikt över varje metod.

När användarna registrerar sina konton för MFA, kan de välja sin önskade verifieringsmetod utanför hello-alternativ som du har aktiverat. hello vägledning för sina registreringen ingår i [Konfigurera mitt konto för tvåstegsverifiering](multi-factor-authentication-end-user-first-time.md)

| Metod | Beskrivning |
|:--- |:--- |
| Anropa toophone |Placerar en automatiserad röstsamtal. hello användaren svar hello anropa och trycka på # hello phone tangentbordet tooauthenticate. Det här telefonnumret är inte synkroniserade tooon lokala Active Directory. |
| Text meddelandet toophone |Skickar ett SMS med verifieringskoden. hello användaren är begärd tooeither svar toohello SMS med verifieringskoden hello eller tooenter hello Verifieringskod i hello i inloggningsgränssnittet. |
| Avisering via mobilapp |Skickar en push notification tooyour telefon eller en registrerad enhet. hello användaren visar hello-meddelande och väljer **Kontrollera** toocomplete verifiering. <br>hello Microsoft Authenticator-appen är tillgänglig för [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), och [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
| Verifieringskod från mobilapp |hello Microsoft Authenticator-appen genererar en ny OATH-Verifieringskod var 30: e sekund. hello användaren anger den här koden i hello i inloggningsgränssnittet.<br>hello Microsoft Authenticator-appen är tillgänglig för [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), och [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |

### <a name="how-tooenabledisable-authentication-methods"></a>Hur tooenable och inaktivera autentiseringsmetoder
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Navigera toohello MFA tjänstinställningar sida per hello instruktioner hello början av den här artikeln.
3. På sidan för hello inställningar, under Alternativ för verifiering, Markera/avmarkera hello-alternativ som du vill toouse.
   ![Alternativ för verifiering](./media/multi-factor-authentication-whats-next/authmethods.png)
4. Klicka på **Spara**.
5. Klicka på **Stäng**.

