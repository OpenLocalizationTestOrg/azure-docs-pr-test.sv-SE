---
title: "aaaVPN integrering med Azure MFA med hjälp av NPS-tillägg | Microsoft Docs"
description: "Den här artikeln beskrivs integrera din VPN-infrastruktur med Azure MFA med hello Server (NPS)-tillägg för Microsoft Azure."
services: active-directory
keywords: "Azure MFA integrera VPN, Azure Active Directory, NPS-tillägg"
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 5e120f7633121385d9cc5d7bec97ecaa1ec7cf19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-your-vpn-infrastructure-with-azure-multi-factor-authentication-mfa-using-hello-network-policy-server-nps-extension-for-azure"></a>Integrera din VPN-infrastruktur med Azure Multi-Factor Authentication (MFA) använder hello Server (NPS)-tillägg för Azure

## <a name="overview"></a>Översikt

hello Nätverksprincipserver Service (NPS)-tillägget för Azure gör att organisationer toosafeguard RADIUS Remote Authentication Dial-In User Service ()-klientautentisering med hjälp av molnbaserade [Azure Multi-Factor Authentication (MFA)](multi-factor-authentication-get-started-server-rdg.md), som innehåller tvåstegsverifiering.

Den här artikeln innehåller instruktioner för att integrera hello NPS infrastrukturen med Azure MFA med hello NPS-tillägg för Azure tooenable säker tvåstegsverifiering för användare som försöker tooconnect tooyour nätverk via en VPN-anslutning. 

hello nätverksprinciper och Access Services (NPS) ger organisationer hello följande möjligheter:

* Ange centrala platser för hello hantering och kontroll av nätverket begäranden toospecify som kan ansluta, vilka tidpunkter på dagen anslutningar tillåts hello varaktighet för anslutningar och hello säkerhetsnivå som klienter måste använder tooconnect och så vidare. I stället för att ange dessa principer på varje server för VPN- eller Gateway för fjärrskrivbord (RD), kan dessa principer anges en gång på en central plats. hello RADIUS-protokollet används tooprovide hello centraliserad autentisering, auktorisering och redovisning. 
* Skapa och tillämpa hälsopolicys för Network Access Protection (NAP) klienten som avgör om enheter har beviljats obegränsad eller begränsad åtkomst toonetwork resurser.
* Ange en innebär tooenforce autentisering och auktorisering för åtkomst too802.1x-kompatibla trådlösa åtkomstpunkter och Ethernet-växlar.    

Mer information finns i [Server (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top). 

tooenhance säkerhet och ger hög nivå om överensstämmelse, företag kan integrera NPS med Azure MFA tooensure som användaren använder tvåstegsverifiering toobe kan ansluta toohello virtuell port på hello VPN-servern. Användare toobe beviljas åtkomst, måste de ange sina användarnamn/lösenord tillsammans med information som hello användare har i deras kontroll. Den här informationen måste betrodda och inte enkelt dubbleras, till exempel ett mobiltelefonnummer, fast telefon nummer, program på en mobil enhet och så vidare.

Tidigare toohello tillgängligheten för hello NPS-tillägget för Azure, kunder som önskat tooimplement tvåstegsverifiering för integrerad NPS och Azure MFA miljöer har tooconfigure och upprätthålla en separat MFA-Server i hello lokal miljö som dokumenterade i fjärrskrivbordsgateway och Azure Multi-Factor Authentication-servern med RADIUS.

hello tillgång till hello NPS-tillägg för Azure ger organisationer hello val toodeploy nu ett lokalt baserad MFA-lösning eller en molnbaserad MFA lösning toosecure RADIUS-klientautentisering.
 
## <a name="authentication-flow"></a>Autentiseringsflöde
När en användare ansluter tooa virtuell port på en VPN-server, måste de först autentisera med olika protokoll, vilket tillåter hello användning av en kombination av användarnamn/lösenord och certifikatbaserade autentiseringsmetoder. 

I tillägg tooauthenticating och verifiera identitet, måste användarna ha hello lämpliga behörigheter för fjärråtkomst. I enklare implementationer anges dessa behörighet som tillåter åtkomst direkt i hello Active Directory-användarobjekt. 

 ![Egenskaper för användare](./media/nps-extension-vpn/image1.png)

Enklare implementationer varje VPN-server som beviljar eller nekar åtkomst baserat på principer som definierats på varje lokala VPN-servern.

I större och mer skalbar implementeringar hello principerna för att bevilja eller neka VPN-åtkomst är centraliserad på RADIUS-servrar. I det här fallet fungerar hello VPN-servern som en fjärråtkomstserver (RADIUS-klienten) som vidarebefordrar anslutningsbegäranden och kontot meddelanden tooa RADIUS-servern. tooconnect toohello virtuell port på hello VPN-server, användare måste autentiseras och uppfyller hello villkor definieras centralt på RADIUS-servrar. 

När hello NPS-tillägget för Azure är integrerad med hello NPS, är hello autentiseringsflödet följande:

1. hello VPN-servern tar emot en begäran om autentisering från en VPN-användare som innehåller hello användarnamn och lösenord tooconnect tooa resurs, till exempel en fjärrskrivbordssession. 
2. Fungerar som en RADIUS-klient, VPN-servern konverterar hello begäran tooa RADIUS-åtkomstbegäran och skickar hello-meddelande (lösenord krypteras) toohello RADIUS (NPS) server där hello NPS tillägget installeras. 
3. Hej användarnamnet och lösenordet har verifierats i Active Directory. Om hello användarnamn / lösenord är felaktigt, hello RADIUS-servern skickar ett meddelande om nekad åtkomst. 
4. Om alla villkor som anges i hello NPS anslutningsbegäran och nätverksprinciper uppfylls (till exempel tidpunkt eller grupp medlemskap begränsningar), hello NPS tillägget utlöser en begäran om sekundära autentiseringen med Azure MFA. 
5. Azure MFA kommunicerar med Azure Active Directory, hämtar hello användarinformation och utför hello sekundära autentiseringen använder hello-metoden som konfigurerats av hello användare (textmeddelande, mobilapp och så vidare). 
6. Vid framgången för hello MFA-kontrollen har kommunicerar Azure MFA hello resultatet toohello NPS tillägg.
7. När hello anslutningsförsöket både autentiseras och auktoriseras skickar hello NPS-server där hello tillägget installeras en RADIUS-Access-Accept meddelandet toohello VPN-server (RADIUS-klienten).
8. hello användaren beviljas åtkomst toohello virtuell port på VPN-servern och upprättar en krypterad VPN-tunnel.

## <a name="prerequisites"></a>Krav
Det här avsnittet beskrivs hello krav som är nödvändiga innan integrera Azure MFA med hello Remote Desktop Gateway. Innan du börjar måste du ha hello följande krav är på plats.

* VPN-infrastruktur
* Nätverksprincip och Access Services (NPS)
* Azure MFA-licens
* Windows Server-programvara
* Bibliotek
* Azure AD synkroniserade med lokala AD 
* Azure Active Directory-GUID-ID

### <a name="vpn-infrastructure"></a>VPN-infrastruktur
Den här artikeln förutsätter att du har en fungerande VPN-infrastruktur med hjälp av Microsoft Windows Server 2016 på plats och hello VPN-servern är för närvarande inte konfigurerade tooforward anslutning begäranden tooa RADIUS-server. I den här guiden konfigurerar du hello VPN-infrastruktur toouse en central RADIUS-server.

Om du inte har en fungerande infrastruktur på plats kan du snabbt skapa infrastrukturen genom följande hello riktlinjerna i flera självstudier för VPN-installationen hittar du på hello Microsoft och tredje parters webbplatser. 

### <a name="network-policy-and-access-services-nps-role"></a>Nätverksprincip och Access Services (NPS)

hello NPS-rolltjänsten ger hello RADIUS-server och klient. Den här artikeln förutsätter att du har installerat hello NPS-rollen på en medlemsserver eller domänkontrollant i din miljö. För en VPN-konfiguration i den här guiden konfigurerar du RADIUS. Installera hello NPS-rollen på en server _andra_ än VPN-servern.

Information om hur du installerar NPS-rollen för hello tjänst Windows Server 2012 eller högre, se [installera en NAP-hälsoprincipserver](https://technet.microsoft.com/library/dd296890.aspx). Princip för NAP (Network Access) är föråldrad i Windows Server 2016. En beskrivning av bästa praxis för NPS, inklusive hello rekommendation tooinstall NPS på en domänkontrollant finns [bästa praxis för NPS](https://technet.microsoft.com/library/cc771746).

### <a name="licenses"></a>Licenser

Krävs är en licens för Azure MFA som finns tillgänglig via en Azure AD Premium, Enterprise Mobility plus Security (EMS) eller en MFA-prenumeration. Mer information finns i [hur tooget Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md). I testsyfte kan använda du en utvärderingsprenumeration.

### <a name="software"></a>Programvara

hello NPS tillägget kräver Windows Server 2008 R2 SP1 eller senare med hello NPS-rolltjänsten installerad. Alla hello steg i den här handboken utfördes med Windows Server 2016.

### <a name="libraries"></a>Bibliotek

hello följande två bibliotek krävs:

* [Visual C++ Redistributable-paket för Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
* _Microsoft Azure Active Directory-modulen för Windows PowerShell version 1.1.166.0_ eller högre. Hello senaste versionen och installationsanvisningar finns i [Microsoft Azure Active Directory PowerShell-modulen släpper versionshistorik](https://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx).

Dessa bibliotek paketeras inte med hello NPS tillägget installationsfiler (version 0.9.1.2), trots befintliga dokumentationen som säger annorlunda. Som minimum måste du installera hello Visual C++ Redistributable-paket för Visual Studio 2013. hello Microsoft Azure Active Directory-modulen för Windows PowerShell installeras, om det inte redan finns, via ett konfigurationsskript som du kör som en del av konfigurationsprocessen för hello. Det finns inget behov av tooinstall denna modul i förväg om den inte redan är installerad.

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Azure Active Directory är synkroniserade med lokala Active Directory 

toouse hello NPS-tillägg, lokal användare måste synkroniseras med Azure Active Directory och aktiverats för Multifaktorautentisering. Den här guiden förutsätter att lokala användare är synkroniserade med Azure Active Directory med AD Connect. Instruktioner för att aktivera användare för MFA finns nedan.
Information om Azure AD connect, se [integrera dina lokala kataloger med Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>Azure Active Directory-GUID-ID 
tooinstall hello NPS, måste du tooknow hello GUID för hello Azure Active Directory. Instruktioner för att hitta hello GUID för hello Azure Active Directory finns i nästa avsnitt om hello.

## <a name="configure-radius-for-vpn-connections"></a>Konfigurera RADIUS för VPN-anslutningar

Om du har installerat hello NPS-serverrollen på en medlemsserver kan du behöver tooconfigure tooauthenticate och auktorisera VPN-klienten som begär VPN-anslutningar. 

Det här avsnittet förutsätter att du har installerat hello NPS-rollen men inte har konfigurerat den för användning i din infrastruktur.

>[!NOTE]
>Om du redan har en fungerande VPN-server som använder en centraliserad RADIUS-server för autentisering kan du hoppa över det här avsnittet.
>

### <a name="register-server-in-active-directory"></a>Registrera servern i Active Directory
toofunction korrekt i det här scenariot hello NPS-server måste toobe registreras i Active Directory.

1. Öppna Serverhanteraren.
2. I Serverhanteraren klickar du på **verktyg**, och klicka sedan på **Network Policy Server**. 
3. I hello NPS-konsolträdet högerklickar du på **NPS (lokal)**, och klicka sedan på **registrera servern i Active Directory**. Klicka på **OK** två gånger.

 ![Nätverksprincipserver](./media/nps-extension-vpn/image2.png)

4. Lämna konsolen hello öppen för hello nästa procedur.

### <a name="use-wizard-tooconfigure-radius-server"></a>Använd guiden tooconfigure RADIUS-server
Du kan använda en standard (guidebaserad) eller avancerad konfiguration alternativet tooconfigure hello RADIUS-server. Det här avsnittet förutsätter hello användning av hello guidebaserad standard konfigurationsalternativet.

1. Hello NPS-konsolen, klicka på **NPS (lokal)**.
2. Välj under standardkonfiguration **RADIUS-Server för fjärranslutningar eller VPN-anslutningar**, och klicka sedan på **konfigurera VPN eller fjärranslutning**.

 ![Konfigurera VPN](./media/nps-extension-vpn/image3.png)

3. Hello väljer fjärranslutning eller virtuellt privat nätverk anslutningar typen Välj på sidan **VPN-anslutningar**, och klicka på **nästa**.

 ![Virtuellt privat nätverk](./media/nps-extension-vpn/image4.png)

4. På hello ange fjärranslutning eller VPN-servern klickar du på **Lägg till**.
5. I hello **ny RADIUS-klient** dialogrutan Ange ett eget namn, ange hello omvandlingsbara namnet eller IP-adress för hello VPN-server och ange ett delade hemliga lösenord. Skapa den här delade hemliga lösenordet långa och komplexa. Anteckna det här lösenordet som du behöver för stegen i nästa avsnitt om hello.

 ![Ny RADIUS-klient](./media/nps-extension-vpn/image5.png)

6. Klicka på **OK**, och sedan **nästa**.
7. På hello **konfigurera autentiseringsmetoder** godkänner hello standardvalet (Microsoft krypterad autentisering version 2 (MS-CHAPv2) eller välj ett annat alternativ och klicka på **nästa**.

  >[!NOTE]
  >Om du konfigurerar Extensible Authentication Protocol (EAP), måste du använda MS-CHAPv2 eller PEAP. Inga andra EAP stöds.
 
8. Klicka på sidan Ange användargrupper hello **Lägg till** och välj en lämplig grupp, om sådan finns. Annars lämna hello markeringen tomt toogrant-tooall användare.

 ![Ange användargrupper](./media/nps-extension-vpn/image7.png)

9. Klicka på **Nästa**.
10. Klicka på sidan Ange IP-filter hello **nästa**.
11. Acceptera standardinställningarna för hello hello på sidan Ange inställningar för kryptering och klicka på **nästa**.

 ![Ange kryptering](./media/nps-extension-vpn/image8.png)

12. Hello ange ett resursnamn, lämnar hello namn är tomt, acceptera hello standardinställningen och på **nästa**.

 ![Ange ett resursnamn](./media/nps-extension-vpn/image9.png)

13. Klicka på hello Slutför nya fjärr- eller sidan med VPN-anslutningar och RADIUS-klienter **Slutför**.

 ![Slutföra anslutningar](./media/nps-extension-vpn/image10.png)

### <a name="verify-radius-configuration"></a>Kontrollera RADIUS-konfiguration
Det här avsnittet beskrivs hello-konfiguration som du skapat med hello guiden.

1. Expandera RADIUS-klienter på hello NPS-servern i hello NPS (lokal)-konsolen och välj **RADIUS-klienter**.
2. Högerklicka i informationsfönstret hello hello RADIUS-klient som du skapat med hjälp av guiden och på **egenskaper**. hello egenskaper för RADIUS-klienten (hello VPN-server) bör vara liknande toothose som visas nedan.

 ![VPN-egenskaper](./media/nps-extension-vpn/image11.png)

3. Klicka på **Avbryt**.
4. Expandera på hello NPS-servern i konsolen för hello NPS (lokal) **principer**, och välj **principer för anslutningsbegäran**. Du bör se hello VPN-anslutningar princip som liknar hello bilden nedan.

 ![Anslutningsbegäranden](./media/nps-extension-vpn/image12.png)

5. Enligt principer, Välj **nätverksprinciper**. Du bör en princip för virtuella privata nätverk (VPN) anslutningar som liknar hello bilden nedan.

 ![Egenskaper för nätverk](./media/nps-extension-vpn/image13.png)

## <a name="configure-vpn-server-toouse-radius-authentication"></a>Konfigurera VPN-servern toouse RADIUS-autentisering
I det här avsnittet kan du konfigurera hello VPN-server toouse RADIUS-autentisering. Det här avsnittet förutsätter att du har en fungerande konfiguration för VPN-server men inte har konfigurerat hello VPN-server toouse RADIUS-autentisering. När du har konfigurerat hello VPN-server bekräfta du att konfigurationen fungerar som förväntat.

>[!NOTE]
>Om du redan har en fungerande VPN-serverkonfiguration som använder RADIUS-autentisering kan du hoppa över det här avsnittet.
>

### <a name="configure-authentication-provider"></a>Konfigurera autentiseringsprovider
1. Öppna Serverhanteraren på hello VPN-servern.
2. I Serverhanteraren klickar du på **verktyg**, och sedan **Routning och fjärråtkomst**.
3. Högerklicka i hello konsolen Routning och fjärråtkomst,  **\[servernamn\] (lokal)**, och klicka sedan på **egenskaper**.

 ![Routning och fjärråtkomst](./media/nps-extension-vpn/image14.png)
 
4. I hello **[servernamn} (lokal) egenskaper** dialogrutan klickar du på hello **säkerhet** fliken. 
5. På hello **säkerhet** under autentiseringsprovider fliken klickar du på **RADIUS-autentisering**, och sedan **konfigurera**.

 ![RADIUS-autentisering](./media/nps-extension-vpn/image15.png)
 
6. Klicka på dialogrutan för hello RADIUS-autentisering, **Lägg till**.
7. I hello Lägg till RADIUS-Server i servernamn, Lägg till hello namn eller hello IP-adressen för hello RADIUS-server som du konfigurerade i hello föregående avsnitt.
8. Klicka på delad hemlighet **ändra** och lägga till hello delade hemliga lösenordet du skapade och registrerats tidigare.
9. Ändra hello värde tooa värde mellan i tidsgräns (sekunder) **30** och **60**. Detta är nödvändigt tooallow tillräckligt med tid för toocomplete hello andra autentiseringsfaktor.
 
 ![Lägg till RADIUS-Server](./media/nps-extension-vpn/image16.png)
 
10. Klicka på **OK** tills du är klar stänger du alla dialogrutor.

### <a name="test-vpn-connectivity"></a>Testa en VPN-anslutning
I det här avsnittet kan bekräfta du hello VPN-klienten autentiseras och auktoriseras av hello RADIUS-server när du försöker tooconnect tooVPN virtuell port. Det här avsnittet förutsätter att du använder Windows 10 som en VPN-klient. 

>[!NOTE]
>Om du redan har konfigurerat en VPN-klienten tooconnect toohello VPN-server och hello-inställningarna har sparats, kan du hoppa över hello steg relaterade tooconfiguring och spara en VPN-anslutningsobjektet.
>

1. Klicka på din VPN-klientdatorn **starta**, och sedan **inställningar** (kugghjulsikon).
2. I fönstret Inställningar, klickar du på **nätverk och Internet**.
3. Klicka på **VPN**.
4. Klicka på **lägga till en VPN-anslutning**.
5. I en VPN-anslutning, ange Windows (inbyggt) som hello tillverkare av VPN- och fullständig hello återstående fälten efter behov, och sedan **spara**. 

 ![Lägg till VPN-anslutning](./media/nps-extension-vpn/image17.png)
 
6. Öppna hello **nätverks- och delningscenter** på Kontrollpanelen.
7. Klicka på **ändra Kortinställningar**.

 ![Ändra inställningar för nätverkskort](./media/nps-extension-vpn/image18.png)

8. Högerklicka på hello VPN nätverksanslutningen och klicka på Egenskaper. 

 ![Egenskaper för VPN-nätverk](./media/nps-extension-vpn/image19.png)

9. I egenskapsdialogrutan för hello VPN, klickar du på hello **säkerhet** fliken. 
10. Se till att endast på fliken Säkerhet hello **Microsoft CHAP Version 2 (MS-CHAP v2)** är markerad och klicka på OK.

 ![Tillåt protokoll](./media/nps-extension-vpn/image20.png)

11. Högerklicka på hello VPN-anslutningen och på **Anslut**.
12. På hello inställningar klickar du på **Anslut**.

En lyckad anslutning visas i hello säkerhetsloggen på hello RADIUS-server som händelse-ID 6272 enligt nedan.

 ![Egenskaper för händelse](./media/nps-extension-vpn/image21.png)

## <a name="troubleshoot-guide"></a>Felsöka programguiden
Anta att VPN-konfiguration fungerade innan du har konfigurerat hello VPN-server toouse en centraliserad RADIUS-server för autentisering och auktorisering. I det här fallet är det troligt att hello problemet kan bero på en felaktig konfiguration av hello RADIUS-Server eller hello användning av ett ogiltigt användarnamn eller lösenord. Till exempel om du använder hello alternativt UPN-suffix i hello användarnamn hello inloggningsförsök misslyckas (bör du använda samma kontonamn för bästa resultat hello). 

tootroubleshoot problemen en användbar toostart är tooexamine hello säkerhetshändelseloggar på hello RADIUS-server. toosave tid på att söka efter händelser, kan du använda hello rollbaserad nätverksprincip och åtkomst till servern anpassad vy i Loggboken som visas nedan. Händelse-ID 6273 anger händelser där hello Network Policy Server nekade åtkomst tooa användare. 

 ![Loggboken](./media/nps-extension-vpn/image22.png)
 
## <a name="configure-multi-factor-authentication"></a>Konfigurera Multi-Factor Authentication
Det här avsnittet innehåller instruktioner för att aktivera användare för MFA och konfigurera konton för tvåstegsverifiering. 

### <a name="enable-multi-factor-authentication"></a>Aktivera multifaktorautentisering
I det här avsnittet kan aktivera du Azure AD-konton för MFA. Använd hello **klassiska portalen** tooenable användare för MFA. 

1. Öppna en webbläsare och gå för[https://manage.windowsazure.com](https://manage.windowsazure.com). 
2. Logga in som administratör hello.
3. I hello-portalen i hello vänstra navigeringsfönstret, klickar du på **ACTIVE DIRECTORY**.

 ![Standardkatalogen](./media/nps-extension-vpn/image23.png)

4. Klicka på namnet i kolumnen hello **standardkatalog** (eller en annan katalog, vid behov).
5. På hello Snabbstart klickar du på **konfigurera**.

 ![Konfigurera standard](./media/nps-extension-vpn/image24.png)

6. På sidan Konfigurera hello Bläddra nedåt och på hello multifaktorautentisering under **hantera tjänstinställningar**.

 ![Hantera inställningar för MFA](./media/nps-extension-vpn/image25.png)
 
7. Granska hello standardinställningarna för tjänsten på hello multifaktorautentiseringssidan, och klicka sedan på **användare**. 

 ![MFA-användare](./media/nps-extension-vpn/image26.png)
 
8. På sidan hello användare väljer hello användare som du vill tooenable för MFA och klicka sedan på **aktivera**.

 ![Egenskaper](./media/nps-extension-vpn/image27.png)
 
9. När du uppmanas, klickar du på **aktivera multifaktorautentisering**.

 ![Aktivera MFA](./media/nps-extension-vpn/image28.png)
 
10. Klicka på **Stäng**. 
11. Uppdatera hello-sidan. Hej MFA status är ändrade tooEnabled.

Mer information om hur tooenable användare för Multifaktorautentisering, se [komma igång med Azure Multi-Factor Authentication i molnet hello](multi-factor-authentication-get-started-cloud.md). 

### <a name="configure-accounts-for-two-step-verification"></a>Konfigurera konton för tvåstegsverifiering
När ett konto har aktiverats för Multifaktorautentisering kan användare inte kan toosign i tooresources som omfattas av principen för MFA hello förrän de har konfigurerat en betrodd enhet toouse för hello andra autentiseringsfaktor använt tvåstegsverifiering.

I det här avsnittet kan konfigurera du en betrodd enhet för användning med tvåstegsverifiering. Det finns flera tillgängliga alternativ för du tooconfigure dessa, inklusive hello följande:

* **Mobilapp**. Du kan installera hello Microsoft Authenticator-appen på en Windows Phone, Android eller iOS-enhet. Beroende på din organisations principer kan du är obligatoriska toouse hello app i ett av två lägen: ta emot meddelanden för verifieringar (skickas ett meddelande tooyour enhet) eller Använd verifieringskoden (måste tooenter en verifiering code som uppdaterar var 30: e sekund). 
* **Mobiltelefon samtal eller textmeddelande**. Du kan antingen ta emot en automatisk telefonsamtal eller SMS. Med alternativet telefonsamtal hello hello samtal och tryck på hello # logga tooauthenticate. Med hello text alternativet kan du svara toohello SMS eller ange hello Verifieringskod i hello i inloggningsgränssnittet.
* **Telefonsamtal till arbete**. Den här processen är hello samma som anges för automatisk telefonsamtal ovan.

Följ dessa instruktioner för hur du konfigurerar en enhet toouse hello mobilappen tooreceive push-meddelanden för verifiering.

1. Logga in för[https://aka.ms/mfasetup](https://aka.ms/mfasetup) eller en plats som [https://portal.azure.com](https://portal.azure.com), var du tvungen att tooauthenticate med dina inloggningsuppgifter för MFA-aktiverade. 
2. När du loggar in med ditt användarnamn och lösenord kan visas med en skärm som uppmanar tooset hello konto för ytterligare säkerhetsverifiering.

 ![Ökad säkerhet](./media/nps-extension-vpn/image29.png)

3. Klicka på **ställa in nu**.
4. Hello ytterligare säkerhet kontroll på sidan Välj en Kontakta (telefon för autentisering, arbetstelefon eller mobilapp). Sedan väljer ett land eller region och välj en metod. hello metoden varierar beroende på typ av du väljer. Till exempel om du väljer mobilappar, du kan välja om tooreceive meddelanden för verifiering eller toouse en Verifieringskod. hello stegen som följer förutsätter att du väljer **mobilappen** som hello kontakta typen.

 ![Phone autentisering](./media/nps-extension-vpn/image30.png)

5. Välj mobilappar, klicka på **ta emot meddelanden för verifiering**, och sedan **konfigurera**. 

 ![Verifiering av Mobilappar](./media/nps-extension-vpn/image31.png)
 
6. Om du inte redan gjort det installerar du hello authenticator-mobilappen på din enhet. 
7. Följ anvisningarna hello i hello mobilappen tooscan hello presenteras streckkod eller ange hello information manuellt klicka sedan på **klar**.

 ![Konfigurera Mobilappen](./media/nps-extension-vpn/image32.png)

8. På hello ytterligare säkerhet verifiering klickar du på **kontakta mig** och svar skickas toonotification tooyour enhet.
9. Anger ett kontaktnummer hello ytterligare säkerhet kontroll på sidan om du tappar åtkomst toohello mobila appar och klicka på **nästa**.

 ![Mobiltelefonnummer](./media/nps-extension-vpn/image33.png)
 
10. Klicka på hello ytterligare säkerhetsverifiering **klar**.

hello-enhet är nu konfigurerad tooprovide en annan metod för verifiering. Information om hur du konfigurerar konton för tvåstegsverifiering finns [Konfigurera mitt konto för tvåstegsverifiering](./end-user/multi-factor-authentication-end-user-first-time.md).

## <a name="install-and-configure-nps-extension"></a>Installera och konfigurera NPS-tillägget

Det här avsnittet innehåller anvisningar för hur du konfigurerar VPN-toouse Azure MFA för klientautentisering med hello VPN-servern.

När du installerar och konfigurerar hello NPS tillägget är alla RADIUS-baserad klient-autentisering som bearbetas av den här servern obligatoriska toouse Azure MFA. Om inte alla VPN-användare är registrerade i Azure MFA, du kan ställa in en annan RADIUS-server tooauthenticate användare som inte är konfigurerade toouse MFA. Eller skapa en registerpost som gör att användarna handlingar tooprovide en andra autentiseringsfaktor endast om de har registrerats i MFA. 

Skapa ett nytt strängvärde med namnet _REQUIRE_USER_MATCH i HKLM\SOFTWARE\Microsoft\AzureMfa_, och ange hello värdet tooTRUE eller FALSE. 

 ![Kräv Användarmatchning](./media/nps-extension-vpn/image34.png)
 
Om hello-värdet är uppsättningen tooTRUE eller inte har angetts är alla autentiseringsbegäranden ämne tooan MFA-kontrollen. Om värdet för hello anges tooFALSE utfärdas MFA utmaningar toousers som är registrerade i MFA. Använd endast hello FALSKT inställning för testning eller i produktionsmiljöer under en tid för onboarding.

### <a name="acquire-azure-active-directory-guid-id"></a>Hämta Azure Active Directory-GUID-ID

Som en del av hello konfigurationen av hello NPS-tillägget behöver du toosupply administratörsautentiseringsuppgifter och hello Azure Active Directory-ID för Azure AD-klienten. hello stegen nedan visar hur tooget hello klient-ID.

1. Logga in toohello Azure-portalen på [https://portal.azure.com](https://portal.azure.com) som hello global administratör för hello Azure-klient.
2. I vänster navigeringsfält hello, klickar du på hello **Azure Active Directory** ikon.
3. Klicka på **Egenskaper**.
4. toocopy din katalog-ID toohello Urklipp, Välj hello **kopiera** ikon.
 
 ![Katalog-ID](./media/nps-extension-vpn/image35.png)

### <a name="install-hello-nps-extension"></a>Installera hello NPS-tillägg
hello NPS tillägg måste toobe installeras på en server som har hello nätverksprincip och Access Services (NPS)-rollen installerad och som fungerar som hello RADIUS-server i din design. Installera inte hello NPS tillägg på fjärrservern skrivbordet.

1. Hämta hello NPS tillägget från [https://aka.ms/npsmfa](https://aka.ms/npsmfa). 
2. Kopiera hello installationsprogrammet körbar fil (NpsExtnForAzureMfaInstaller.exe) toohello NPS-servern.
3. Dubbelklicka på hello NPS-server, **NpsExtnForAzureMfaInstaller.exe**. Om du uppmanas, klickar du på **kör**.
4. Granska hello licensvillkor, i hello NPS-tillägg för Azure MFA dialogrutan Kontrollera **jag accepterar licensvillkoren för toohello**, och klicka på **installera**.

 ![NPS-tillägg](./media/nps-extension-vpn/image36.png)
 
5. Klicka i hello NPS-tillägg för Azure MFA dialogrutan **Stäng**.  

 ![Installationen lyckades](./media/nps-extension-vpn/image37.png) 
 
### <a name="configure-certificates-for-use-with-hello-nps-extension-using-a-powershell-script"></a>Konfigurera certifikat för användning med hello NPS-tillägget med hjälp av ett PowerShell-skript
tooensure säker kommunikation och säkerhet, behöver du tooconfigure certifikat för användning av hello NPS-tillägget. hello NPS komponenter inkluderar ett Windows PowerShell-skript som konfigurerar ett självsignerat certifikat för användning med NPS. 

hello skriptet utför hello följande åtgärder:

* Skapar ett självsignerat certifikat
* Associerar den offentliga nyckeln för certifikatet tooservice huvudnamn på Azure AD
* Lagrar hello cert i hello lokala datorns Arkiv
* Ger åtkomst till toohello certifikatets privata nyckel toohello nätverksanvändare
* NPS-tjänsten har startats om

Om du vill toouse dina egna certifikat, du behöver tooassociate hello offentliga för ditt certifikat toohello service på Azure AD och så vidare.
toouse hello skript, ange hello-tillägget med din Azure Active Directory administrativa autentiseringsuppgifter och hello Azure Active Directory klient-ID som du kopierade tidigare. Kör hello skriptet på varje NPS-server där du installerar tillägget för hello NPS.

1. Öppna en administrativ Windows PowerShell-Kommandotolken.
2. I hello PowerShell-Kommandotolken, Skriv _cd ”c:\Program Files\Microsoft\AzureMfa\Config'_, och tryck på **RETUR**.
3. Typen _.\AzureMfsNpsExtnConfigSetup.ps1_, och tryck på **RETUR**. 
 * hello skriptet kontrollerar toosee om hello Azure Active Directory PowerShell-modulen är installerad. Om det inte är installerat installeras hello skript hello-modulen.
 
 ![PowerShell](./media/nps-extension-vpn/image38.png)
 
4. När hello skript verifierar hello installation av hello PowerShell-modulen, visas dialogrutan för hello Azure Active Directory PowerShell-modulen. Hello i dialogrutan Ange autentiseringsuppgifter för Azure AD-administratör och lösenord och klicka på **logga in**. 
 
 ![PowerShell-logga In](./media/nps-extension-vpn/image39.png)
 
5. När du uppmanas, klistra in hello klient-ID som du kopierade tidigare toohello Urklipp och tryck på **RETUR**. 

 ![Klient-ID:t](./media/nps-extension-vpn/image40.png)

6. hello skriptet skapar ett självsignerat certifikat och utför andra ändringar i konfigurationen. hello-utdata är som hello-bild som visas nedan.

 ![Automatiskt signerat certifikat](./media/nps-extension-vpn/image41.png)

7. Starta om hello-servern.
 
### <a name="verify-configuration"></a>Verifiera konfigurationen
tooverify hello konfiguration, behöver du tooestablish en ny VPN-anslutning med VPN-servern. När du har angett dina autentiseringsuppgifter för primär autentisering, väntar hello VPN-anslutningen på hello sekundära autentiseringen toosucceed innan hello anslutningen har upprättats, enligt nedan. 

 ![Verifiera konfigurationen](./media/nps-extension-vpn/image42.png)

Om du har autentiseras med sekundära hello verifieringsmetod som du tidigare har konfigurerat i Azure MFA är anslutna toohello resurs. Om det inte går att genomföra hello sekundära autentiseringen, nekas åtkomst tooresource. 

I hello exemplet nedan hello Authenticator är-appen på en Windows phone används tooprovide hello sekundära autentiseringen.

 ![Verifiera konto](./media/nps-extension-vpn/image43.png)

När du har autentiserats med hello sekundär metod, beviljas åtkomst toohello virtuell port på hello VPN-servern. Eftersom du inte har nödvändig toouse en sekundär autentiseringsmetod med en mobil app på en betrodd enhet, hello logg i processen är dock säkrare än att det skulle använda bara ett användarnamn / lösenord kombination.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Visa loggarna i Loggboken för lyckade inloggningshändelser
tooview hello lyckade inloggningshändelser i hello Windows Loggboken, kan du utfärda hello följande Windows PowerShell-kommandot tooquery hello Windows säkerhetslogg hello NPS-servern.

tooquery lyckade inloggningshändelser i hello viewer säkerhetshändelseloggar, Använd följande kommando, hello
* _Get-WinEvent - loggnamn säkerhet_ | där {$_.ID - eq '6272'} | FL 

 ![Loggboken för säkerhet](./media/nps-extension-vpn/image44.png)
 
Du kan också visa hello säkerhetsloggen eller hello nätverksprincip och åtkomsttjänster anpassad vy, enligt nedan:

 ![Princip för nätverksåtkomst](./media/nps-extension-vpn/image45.png)

På hello server där du installerade hello NPS-tillägg för Azure MFA, hittar du loggarna i Loggboken program specifika toohello tillägg på **program och tjänster Logs\Microsoft\AzureMfa**. 

* _Get-WinEvent - loggnamn säkerhet_ | där {$_.ID - eq '6272'} | FL

 ![Antal händelser](./media/nps-extension-vpn/image46.png)

## <a name="troubleshoot-guide"></a>Felsöka programguiden
Om hello konfigurationen inte fungerar som förväntat, är en bra toostart tootroubleshoot tooverify som hello användaren är konfigurerad toouse Azure MFA. Hello användare ansluta för[https://portal.azure.com](https://portal.azure.com). Om de tillfrågas om sekundära autentiseringen och kan autentisera, vet du att en felaktig konfiguration av Azure MFA.

Om Azure MFA arbetar för hello användare, bör du granska hello relevanta händelseloggar. Dessa inkluderar hello säkerhetshändelse, -Gateway fungerar och Azure MFA loggar som diskuteras i hello föregående avsnitt. 

Nedan visas ett exempel på utdata för säkerhetsloggen visar misslyckade inloggningsförsök-händelser (händelse-ID 6273):

 ![Säkerhetsloggen](./media/nps-extension-vpn/image47.png)

Nedan visas en relaterad händelse från hello AzureMFA loggar:

 ![Azure MFA-loggar](./media/nps-extension-vpn/image48.png)

tooperform avancerad felsökning av alternativ finns hello NPS loggfilerna där hello NPS-tjänsten är installerad. Dessa loggfiler skapas i _%SystemRoot%\System32\Logs_ mapp som kommaavgränsad textfiler. En beskrivning av dessa loggfiler finns i avsnittet [tolka NPS loggfilerna](https://technet.microsoft.com/library/cc771748.aspx). 

hello poster i dessa loggfiler är svåra toointerpret utan att importera dem till ett kalkylblad eller en databas. Du kan hitta ett värde av IAS Parser online tooassist du vid tolkningen hello loggfiler. Nedan visas hello utdata i en sådan nedladdningsbara [shareware-programmet](http://www.deepsoftware.com/iasviewer): 

 ![Shareware-programmet](./media/nps-extension-vpn/image49.png)

Slutligen för ytterligare felsökning av alternativ, kan du använda en protokollanalysator, till exempel Wireshark eller [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). hello visar följande bild från Wireshark hello RADIUS-meddelanden mellan hello VPN-servern och hello NPS-servern.

 ![Microsoft Message Analyzer](./media/nps-extension-vpn/image50.png)

Mer information finns i [integrera din befintliga infrastruktur för NPS med Azure Multi-Factor Authentication](multi-factor-authentication-nps-extension.md).  

## <a name="next-steps"></a>Nästa steg
[Hur tooget Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)

[Fjärrskrivbordsgateway och Azure Multi-Factor Authentication Server med RADIUS](multi-factor-authentication-get-started-server-rdg.md)

[Integrera dina lokala kataloger med Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md)

