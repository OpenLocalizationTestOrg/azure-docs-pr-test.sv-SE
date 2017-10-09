---
title: "aaaRemote fjärrskrivbordsgateway integrering med Azure MFA NPS-tillägg | Microsoft Docs"
description: "Den här artikeln beskrivs integrera din infrastruktur för Remote Desktop Gateway med Azure MFA med hello Server (NPS)-tillägg för Microsoft Azure."
services: active-directory
keywords: "Azure MFA integrera servern för fjärrskrivbordsgateway, Azure Active Directory, NPS-tillägg"
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
ms.openlocfilehash: ae5f6864416582bd82b787005ca787988d23a813
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#  <a name="integrate-your-remote-desktop-gateway-infrastructure-using-hello-network-policy-server-nps-extension-and-azure-ad"></a>Integrera din infrastruktur för Remote Desktop Gateway med tillägget för hello-Server (NPS) och Azure AD

Den här artikeln innehåller information för att integrera med Azure Multi-Factor Authentication (MFA) för din infrastruktur för Remote Desktop Gateway med hello Server (NPS)-tillägg för Microsoft Azure. 

hello Nätverksprincipserver Service (NPS)-tillägget för Azure kan kunderna toosafeguard RADIUS Remote Authentication Dial-In User Service () klientautentisering med hjälp av Azure är molnbaserad [Multi-Factor Authentication (MFA)](multi-factor-authentication.md). Den här lösningen ger tvåstegsverifiering för att lägga till ett andra säkerhetslager säkerhet toouser inloggningar och transaktioner.

Den här artikeln innehåller stegvisa instruktioner för att integrera hello NPS infrastrukturen med Azure MFA med hello NPS-tillägg för Azure. Detta gör att säkra verifiering för användare som försöker toolog på tooa Remote Desktop Gateway. 

hello nätverksprinciper och Access Services (NPS) ger organisationer hello möjlighet toodo hello följande:
* Definiera centrala platser för hello hantering och kontroll av begäranden för nätverk genom att ange vem som kan ansluta, vilka tidpunkter på dagen anslutningar tillåts hello varaktighet för anslutningar och hello säkerhetsnivå som klienter måste använder tooconnect och så vidare. I stället för att ange dessa principer på varje server för VPN- eller Gateway för fjärrskrivbord (RD) kan dessa principer anges en gång på en central plats. hello RADIUS-protokollet ger hello centraliserad autentisering, auktorisering och redovisning. 
* Skapa och tillämpa hälsopolicys för Network Access Protection (NAP) klienten som avgör om enheter har beviljats obegränsad eller begränsad åtkomst toonetwork resurser.
* Ange en innebär tooenforce autentisering och auktorisering för åtkomst too802.1x-kompatibla trådlösa åtkomstpunkter och Ethernet-växlar.    

Normalt organisationer använder NPS (RADIUS) toosimplify och centralisera hello hantering av VPN-principer. Men många organisationer också använda NPS toosimplify och centralisera hello hantering av Anslutningsutjämning för Fjärrskrivbord anslutning auktoriseringsprinciper (RD cap). 

Organisationer kan även integreras i NPS Azure MFA tooenhance och ger en hög nivå av kompatibilitet. Detta säkerställer att användare upprättar tvåstegsverifiering verifiering toolog på toohello Remote Desktop Gateway. Användare toobe beviljas åtkomst, måste de ange sina användarnamn/lösenord tillsammans med information som hello användare har i deras kontroll. Den här informationen måste betrodda och inte enkelt dubbleras, till exempel ett mobiltelefonnummer, fast telefon nummer, program på en mobil enhet och så vidare.

Tidigare toohello tillgängligheten för hello NPS-tillägget för Azure, kunder som önskat tooimplement tvåstegsverifiering för integrerad NPS och Azure MFA miljöer har tooconfigure och upprätthålla en separat MFA-Server i hello lokal miljö som dokumenterade i [Remote Desktop Gateway och Azure Multi-Factor Authentication-servern med hjälp av RADIUS](multi-factor-authentication-get-started-server-rdg.md).

hello tillgång till hello NPS-tillägg för Azure ger organisationer hello val toodeploy nu ett lokalt baserad MFA-lösning eller en molnbaserad MFA lösning toosecure RADIUS-klientautentisering.

## <a name="authentication-flow"></a>Autentiseringsflöde

För användare beviljas toobe toonetwork kommer åt resurser via en Gateway för fjärrskrivbord, måste de uppfylla hello villkor som anges i en RD auktoriseringsprincip för klientanslutning (RD CAP) och en värdserver för resursen auktoriseringsprincip (Värddator FJÄRRSKRIVBORDSANSLUTNING). Auktoriseringsprinciper för fjärrskrivbordsanslutning ange vem som är auktoriserade tooconnect tooRD Gateways. Auktoriseringsprinciper för fjärrskrivbordsresurser ange hello nätverksresurser, till exempel fjärrskrivbord eller Fjärrprogram, hello användaren tillåts tooconnect toothrough hello RD Gateway. 

En fjärrskrivbordsgateway kan vara konfigurerade toouse en central princip store för auktoriseringsprinciper för fjärrskrivbordsanslutning. Auktoriseringsprinciper för fjärrskrivbordsresurser kan inte använda en central princip när de bearbetas på hello RD Gateway. Ett exempel på en RD Gateway som konfigurerats toouse en central princip store för auktoriseringsprinciper för fjärrskrivbordsanslutning är en RADIUS-klient tooanother NPS-server som fungerar som hello centrala principarkivet.

När hello NPS-tillägget för Azure är integrerad med hello NPS och fjärrskrivbordsgateway är hello autentiseringsflödet följande:

1. hello Remote Desktop Gateway-servern tar emot en autentiseringsbegäran från en användare av fjärrskrivbord tooconnect tooa resurs, till exempel en fjärrskrivbordssession. Fungerar som en RADIUS-klient, hello fjärrskrivbordsgatewayserver konverterar hello begäran tooa RADIUS-åtkomstbegäran och skickar hello meddelandet toohello RADIUS (NPS) server där hello NPS-tillägg har installerats. 
2. Hej användarnamnet och lösenordet har verifierats i Active Directory och hello användaren autentiseras.
3. Om alla hello villkor som anges i hello NPS anslutningsbegäran och hello nätverksprinciper uppfylls (till exempel tidpunkt eller grupp medlemskap begränsningar), hello NPS tillägget utlöser en begäran om sekundära autentiseringen med Azure MFA. 
4. Azure MFA kommunicerar med Azure AD, hämtar hello användarinformation och utför hello sekundära autentiseringen använder hello-metoden som konfigurerats av hello användare (textmeddelande, mobilapp och så vidare). 
5. Vid framgången för hello MFA-kontrollen har kommunicerar Azure MFA hello resultatet toohello NPS tillägg.
6. hello NPS-server där hello tillägget installeras skickar meddelandet RADIUS nekad för hello auktoriseringsprincip för Fjärrskrivbordsanslutning princip toohello Remote Desktop Gateway-servern.
7. hello användaren beviljas åtkomst toohello begärde nätverksresurs genom hello RD Gateway.

## <a name="prerequisites"></a>Krav
Det här avsnittet beskrivs hello krav som är nödvändiga innan integrera Azure MFA med hello Remote Desktop Gateway. Innan du börjar måste du ha hello följande krav är på plats.  

* Remote Desktop Services (RDS)-infrastruktur
* Azure MFA-licens
* Windows Server-programvara
* Nätverksprincip och Access Services (NPS)
* Azure AD synkroniserade med lokala AD 
* Azure Active Directory-GUID-ID

### <a name="remote-desktop-services-rds-infrastructure"></a>Remote Desktop Services (RDS)-infrastruktur
Du måste ha en fungerande Remote Desktop Services (RDS)-infrastruktur på plats. Om du inte gör det, kan du snabbt skapa infrastrukturen i Azure med hjälp av följande hello Snabbstartsguide mall: [skapa Remote Desktop Sessionssamling distribution](https://github.com/Azure/azure-quickstart-templates/tree/ad20c78b36d8e1246f96bb0e7a8741db481f957f/rds-deployment). 

Om du vill skapa toomanually en lokal RDS infrastruktur snabbt för testning, följ hello steg toodeploy en. 
**Lär dig mer**: [distribuera RDS med Azure Snabbstart](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-in-azure) och [grundläggande RDS-distribution för infrastruktur](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure). 

### <a name="licenses"></a>Licenser
Krävs är en licens för Azure MFA som finns tillgänglig via en Azure AD Premium, Enterprise Mobility plus Security (EMS) eller en MFA-prenumeration. Mer information finns i [hur tooget Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md). I testsyfte kan använda du en utvärderingsprenumeration.

### <a name="software"></a>Programvara
hello NPS tillägget kräver Windows Server 2008 R2 SP1 eller senare med hello NPS-rolltjänsten installerad. Alla hello stegen i det här avsnittet utfördes med Windows Server 2016.

### <a name="network-policy-and-access-services-nps-role"></a>Nätverksprincip och Access Services (NPS)
hello NPS-rolltjänsten tillhandahåller hello RADIUS-servern och klienten funktioner samt nätverksprincip Access-tjänsten för hälsotillstånd. Den här rollen måste installeras på minst två datorer i din infrastruktur: hello Remote Desktop Gateway och en annan medlemsserver eller domänkontrollant. Som standard finns hello roll redan på hello dator är konfigurerad som hello Remote Desktop Gateway.  Du måste sedan även installera hello NPS-rollen på minst på en annan dator, till exempel en domänkontrollant eller medlemsserver.

Information om hur du installerar hello NPS-rollen service i Windows Server 2012 eller tidigare, se [installera en NAP-hälsoprincipserver](https://technet.microsoft.com/library/dd296890.aspx). En beskrivning av bästa praxis för NPS, inklusive hello rekommendation tooinstall NPS på en domänkontrollant finns [bästa praxis för NPS](https://technet.microsoft.com/library/cc771746).

### <a name="azure-active-directory-synched-with-on-premises-active-directory"></a>Azure Active Directory är synkroniserade med lokala Active Directory 
toouse hello NPS-tillägg, lokal användare måste synkroniseras med Azure AD och aktiverats för MFA. Det här avsnittet förutsätter att lokala användare är synkroniserade med Azure AD med hjälp av AD Connect. Information om Azure AD connect, se [integrera dina lokala kataloger med Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md). 

### <a name="azure-active-directory-guid-id"></a>Azure Active Directory-GUID-ID
tooinstall NPS, måste tooknow hello GUID för hello Azure AD. Instruktioner för att hitta hello GUID för hello Azure AD finns nedan.

## <a name="configure-multi-factor-authentication"></a>Konfigurera Multi-Factor Authentication 
Det här avsnittet innehåller instruktioner för att integrera Azure MFA med hello Remote Desktop Gateway. Du måste konfigurera hello Azure MFA-tjänsten innan användare kan registrera sina enheter Multi-Factor eller program som en administratör.

Gör så hello i [komma igång med Azure Multi-Factor Authentication i molnet hello](multi-factor-authentication-get-started-cloud.md) tooenable MFA för Azure AD-användare. 

### <a name="configure-accounts-for-two-step-verification"></a>Konfigurera konton för tvåstegsverifiering
När ett konto har aktiverats för MFA, du kan inte logga in tooresources som styrs av hello MFA-principen förrän du har konfigurerat en betrodd enhet toouse för hello andra autentiseringsfaktor har autentiseras med tvåstegsverifiering.

Gör så hello i [vad Azure Multi-Factor Authentication innebär för mig?](./end-user/multi-factor-authentication-end-user.md) toounderstand och konfigurera dina enheter för MFA med ditt användarkonto.

## <a name="install-and-configure-nps-extension"></a>Installera och konfigurera NPS-tillägget
Det här avsnittet innehåller instruktioner för att konfigurera RDS infrastruktur toouse Azure MFA för klientautentisering med hello Remote Desktop Gateway.

### <a name="acquire-azure-active-directory-guid-id"></a>Hämta Azure Active Directory-GUID-ID

Som en del av hello konfigurationen av hello NPS-tillägget behöver du toosupply administratörsautentiseringsuppgifter och hello Azure AD-ID för Azure AD-klienten. hello följande steg visar hur tooget hello klient-ID.

1. Logga in toohello [Azure-portalen](https://portal.azure.com) som hello global administratör för hello Azure-klient.
2. I vänster navigeringsfält hello, väljer du hello **Azure Active Directory** ikon.
3. Välj **egenskaper**.
4. Klicka på hello i hello egenskapsbladet bredvid hello katalog-ID, **kopiera** ikonen som visas nedan, toocopy hello ID tooclipboard.

 ![Egenskaper](./media/nps-extension-remote-desktop-gateway/image1.png)

### <a name="install-hello-nps-extension"></a>Installera hello NPS-tillägg
Installera tillägget för hello NPS på en server som har hello nätverksprincip och Access Services (NPS)-rollen installerad. Detta fungerar som hello RADIUS-server för din design. 

>[!Important]
> Var noga med att du inte installerar hello NPS tillägg på servern för fjärrskrivbordsgateway.
> 

1. Hämta hello [NPS tillägget](https://aka.ms/npsmfa). 
2. Kopiera hello installationsprogrammet körbar fil (NpsExtnForAzureMfaInstaller.exe) toohello NPS-servern.
3. Dubbelklicka på hello NPS-server, **NpsExtnForAzureMfaInstaller.exe**. Om du uppmanas, klickar du på **kör**.
4. Granska hello licensvillkor, i hello NPS-tillägg för Azure MFA dialogrutan Kontrollera **jag accepterar licensvillkoren för toohello**, och klicka på **installera**.
 
  ![Azure MFA-inställningar](./media/nps-extension-remote-desktop-gateway/image2.png)

5. Klicka på Stäng i hello NPS-tillägg för Azure MFA-dialogruta. 

  ![NPS-tillägget för Azure MFA](./media/nps-extension-remote-desktop-gateway/image3.png)

### <a name="configure-certificates-for-use-with-hello-nps-extension-using-a-powershell-script"></a>Konfigurera certifikat för användning med hello NPS-tillägget med hjälp av ett PowerShell-skript
Sedan måste tooconfigure certifikat för säker kommunikation för tooensure hello NPS-tillägget och säkerhet. hello NPS komponenter inkluderar ett Windows PowerShell-skript som konfigurerar ett självsignerat certifikat för användning med NPS. 

hello skriptet utför hello följande åtgärder:

* Skapar ett självsignerat certifikat
* Associerar den offentliga nyckeln för certifikatet tooservice huvudnamn på Azure AD
* Lagrar hello cert i hello lokala datorns Arkiv
* Ger åtkomst till toohello certifikatets privata nyckel toohello nätverksanvändare
* NPS-tjänsten har startats om

Om du vill toouse dina egna certifikat, du behöver tooassociate hello offentliga för ditt certifikat toohello service på Azure AD och så vidare.

toouse hello skript, ange hello-tillägget med dina administratörsautentiseringsuppgifter för Azure AD och hello Azure AD-klient-ID som du kopierade tidigare. Kör hello skriptet på varje NPS-server där du installerade hello NPS-tillägget. Sedan hello följande:

1. Öppna en administrativ Windows PowerShell-Kommandotolken.
2. I hello PowerShell-Kommandotolken, Skriv **cd ”c:\Program Files\Microsoft\AzureMfa\Config'**, och tryck på **RETUR**.
3. Typen _.\AzureMfsNpsExtnConfigSetup.ps1_, och tryck på **RETUR**. hello skriptet kontrollerar toosee om hello Azure Active Directory PowerShell-modulen är installerad. Om inte har installerats installeras hello skript hello-modulen.

  ![Azure AD PowerShell](./media/nps-extension-remote-desktop-gateway/image4.png)
  
4. När hello skript verifierar hello installation av hello PowerShell-modulen, visas dialogrutan för hello Azure Active Directory PowerShell-modulen. Hello i dialogrutan Ange autentiseringsuppgifter för Azure AD-administratör och lösenord och klicka på **logga In**.

  ![Öppna Powershell-konto](./media/nps-extension-remote-desktop-gateway/image5.png)

5. När du uppmanas, klistra in hello klient-ID som du kopierade tidigare toohello Urklipp och tryck på **RETUR**.

  ![Ange klient-ID](./media/nps-extension-remote-desktop-gateway/image6.png)

6. hello skriptet skapar ett självsignerat certifikat och utför andra ändringar i konfigurationen. hello utdata ska vara som hello-bild som visas nedan.

  ![Självsignerat certifikat](./media/nps-extension-remote-desktop-gateway/image7.png)

## <a name="configure-nps-components-on-remote-desktop-gateway"></a>Konfigurera NPS-komponenter på servern för fjärrskrivbordsgateway
I det här avsnittet konfigurerar du auktoriseringsprinciper för hello Remote Desktop Gateway-anslutningen och andra inställningar för RADIUS.

Hej autentiseringsflödet kräver att RADIUS-meddelanden utväxlas mellan hello fjärrskrivbordsgateway och hello NPS-server där hello NPS-servern är installerad. Det innebär att du måste konfigurera RADIUS-klientinställningarna på både servern för fjärrskrivbordsgateway och hello NPS-server där hello NPS-tillägg har installerats. 

### <a name="configure-remote-desktop-gateway-connection-authorization-policies-toouse-central-store"></a>Konfigurera servern för fjärrskrivbordsgateway anslutning principer toouse centrala auktoriseringsarkiv
Auktoriseringsprinciper för fjärrskrivbordsanslutning (RD cap) Ange hello krav för anslutande tooa Remote Desktop Gateway-server. Auktoriseringsprinciper för fjärrskrivbordsanslutning lagras lokalt (standard) eller de kan lagras i en central auktoriseringsprincip för fjärrskrivbordsanslutning som kör NPS. tooconfigure integration av Azure MFA med Fjärrskrivbordstjänster, behöver du toospecify hello användning av en central store.

1. Öppna på servern för fjärrskrivbordsgateway hello **Serverhanteraren**. 
2. På menyn hello **verktyg**, peka för**Fjärrskrivbordstjänster**, och klicka sedan på **hanteraren för fjärrskrivbordsgateway**.

  ![Fjärrskrivbordstjänster](./media/nps-extension-remote-desktop-gateway/image8.png)

3. Hello RD Gateway Manager, högerklicka på  **\[servernamn\] (lokal)**, och klicka på **egenskaper**.

  ![Servernamn](./media/nps-extension-remote-desktop-gateway/image9.png)

4. Välj hello i hello egenskapsdialogrutan **auktoriseringsprincip för Fjärrskrivbordsanslutning** Store-fliken.
5. Välj fliken auktoriseringsprincip för fjärrskrivbordsanslutning hello **Central Server som kör NPS**. 
6. I hello **ange namn eller IP-adress för hello-server som kör NPS** fält, typen hello IP-adress eller servernamn för hello server där du installerade hello NPS-tillägget.

  ![Ange namn eller IP-adress](./media/nps-extension-remote-desktop-gateway/image10.png)
  
7. Klicka på **Lägg till**.
8. I hello **delad hemlighet** dialogrutan, ange en delad hemlighet och klicka sedan på **OK**. Se till att du registrerar den här delad hemlighet och lagra hello-post på ett säkert sätt.

 >[!NOTE]
 >Delad hemlighet används tooestablish förtroende mellan hello RADIUS-servrar och klienter. Skapa ett långa och komplexa lösenord.
 >

 ![Delad hemlighet](./media/nps-extension-remote-desktop-gateway/image11.png)

9. Klicka på **OK** tooclose hello dialogrutan.

### <a name="configure-radius-timeout-value-on-remote-desktop-gateway-nps"></a>Konfigurera RADIUS-timeout-värde på Remote Desktop Gateway NPS
tooensure det är tid toovalidate användarnas autentiseringsuppgifter, utföra tvåstegsverifiering, svar och svara tooRADIUS meddelanden, är det nödvändigt tooadjust hello RADIUS-timeout-värde.

1. På hello servern för fjärrskrivbordsgateway i Serverhanteraren klickar du på **verktyg**, och klicka sedan på **Network Policy Server**. 
2. I hello **NPS (lokal)** konsolen, expandera **RADIUS-klienter och servrar**, och välj **fjärranslutna RADIUS-Server**.

 ![Fjärr-RADIUS-Server](./media/nps-extension-remote-desktop-gateway/image12.png)

3. Dubbelklicka på i informationsfönstret hello **TS GATEWAY-SERVERGRUPP**.

 >[!NOTE]
 >RADIUS-Server gruppen skapades när du har konfigurerat hello central server för NPS-policys. hello RD Gateway vidarebefordrar RADIUS-meddelanden toothis server eller grupp av servrar, om mer än en i hello grupp.
 >

4. I hello **gruppegenskaper för TS GATEWAY-SERVER** dialogrutan, Välj hello IP-adress eller namnet på hello NPS-server som du har konfigurerat toostore auktoriseringsprinciper för fjärrskrivbordsanslutning och klicka sedan på **redigera**. 

 ![TS Gateway-servergrupp](./media/nps-extension-remote-desktop-gateway/image13.png)

5. I hello **redigera RADIUS-Server** dialogrutan, Välj hello **belastningsutjämning** fliken.
6. I hello **belastningsutjämning** på fliken hello **antal sekunder utan svar innan begäran anses som utelämnats** fältet, ändra standardvärdet för hello från 3 tooa värde mellan 30 och 60 sekunder.
7. I hello **antalet sekunder mellan begäranden när servern anses vara otillgänglig** fältet, ändra hello standardvärdet 30 sekunder tooa värde som är lika tooor som är större än hello värdet du angav i föregående steg i hello.

 ![Redigera RADIUS-Server](./media/nps-extension-remote-desktop-gateway/image14.png)

8.  Klicka på OK två gånger tooclose hello dialogrutor.

### <a name="verify-connection-request-policies"></a>Kontrollera principer för anslutningsbegäran 
Är som standard när du konfigurerar hello RD Gateway toouse en central princip store för anslutningen auktoriseringsprinciper hello RD Gateway konfigurerade tooforward CAP begäranden toohello NPS-servern. hello NPS-server med hello Azure MFA-tillägg installeras processer hello RADIUS-åtkomstbegäran. hello följande steg visar hur tooverify hello standard princip för anslutningsbegäran. 

1. På hello RD Gateway i hello NPS (lokal)-konsolen, expandera **principer**, och välj **principer för anslutningsbegäran**.
2. Högerklicka på **ansluta principer för anslutningsbegäran**, och dubbelklicka på **AUKTORISERINGSPRINCIP för TS GATEWAY**.
3. I hello **AUKTORISERINGSPRINCIP för TS GATEWAY egenskaper** dialogrutan klickar du på hello **inställningar** fliken.
4. På **inställningar** klickar du på fliken under Vidarebefordra anslutningsbegäran **autentisering**. RADIUS-klienten är konfigurerad tooforward begäranden för autentisering.

 ![Autentiseringsinställningar](./media/nps-extension-remote-desktop-gateway/image15.png)
 
5. Klicka på **Avbryt**. 

## <a name="configure-nps-on-hello-server-where-hello-nps-extension-is-installed"></a>Konfigurera NPS på hello servern där hello NPS-tillägget har installerats
hello NPS-server där hello NPS tillägget är installerade måste toobe kan tooexchange RADIUS-meddelanden med hello NPS-server på hello Remote Desktop Gateway. exchange-tooenable det här meddelandet måste du tooconfigure hello NPS-komponenter på hello servern där hello NPS-tillägg-tjänsten är installerad. 

### <a name="register-server-in-active-directory"></a>Registrera servern i Active Directory
toofunction korrekt i det här scenariot hello NPS-server måste toobe registreras i Active Directory.

1. Öppna **Serverhanteraren**.
2. I Serverhanteraren klickar du på **verktyg**, och klicka sedan på **Network Policy Server**. 
3. I hello NPS-konsolträdet högerklickar du på **NPS (lokal)**, och klicka sedan på **registrera servern i Active Directory**. 
4. Klicka på **OK** två gånger.

 ![Registrera servern i AD](./media/nps-extension-remote-desktop-gateway/image16.png)

5. Lämna konsolen hello öppen för hello nästa procedur.

### <a name="create-and-configure-radius-client"></a>Skapa och konfigurera RADIUS-klient 
hello Remote Desktop Gateway måste toobe som konfigurerats som en RADIUS-klient toohello NPS-server. 

1. På hello NPS-server där hello NPS tillägget installeras i hello **NPS (lokal)** konsolen och högerklicka på **RADIUS-klienter** och på **ny**.

 ![Ny RADIUS-klienter](./media/nps-extension-remote-desktop-gateway/image17.png)

2. I hello **ny RADIUS-klient** dialogrutan Ange ett eget namn som _Gateway_, och hello IP-adress eller DNS-namnet på hello Remote Desktop Gateway-servern. 
3. I hello **delad hemlighet** och hello **Bekräfta delad hemlighet** ange hello samma hemlighet som du använde tidigare.

 ![Namn och adress](./media/nps-extension-remote-desktop-gateway/image18.png)

4. Klicka på **OK** dialogrutan för tooclose hello ny RADIUS-klient.

### <a name="configure-network-policy"></a>Konfigurera principen för nätverk
Återkalla hello NPS-server med hello Azure MFA-tillägget är hello avsedda centrala principarkiv för hello anslutning auktorisering princip (CAP). Du måste därför tooimplement ett tak för hello NPS-server tooauthorize giltiga anslutningar förfrågningar.  

1. Expandera i hello NPS (lokal) konsolen **principer**, och klicka på **nätverksprinciper**.
2. Högerklicka på **anslutningar tooother klientåtkomstservrar**, och klicka på **duplicera princip**. 

 ![Duplicera princip](./media/nps-extension-remote-desktop-gateway/image19.png)

3. Högerklicka på **kopiera anslutningar tooother klientåtkomstservrar**, och klicka på **egenskaper**.

 ![Egenskaper för nätverk](./media/nps-extension-remote-desktop-gateway/image20.png)

4. I hello **kopiera anslutningar tooother klientåtkomstservrar** dialogrutan i principens namn anger du ett lämpligt namn som **RDG_CAP**. Kontrollera **principen aktiverad**, och välj **bevilja åtkomst**. Alternativt, Välj typ av nätverksåtkomst **fjärrskrivbordsgateway**, eller så kan du lämna den som **Ospecificerad**.

 ![Kopia av anslutningar](./media/nps-extension-remote-desktop-gateway/image21.png)

5.  Klicka på hello **begränsningar** och kontrollera **Tillåt klienter tooconnect utan att förhandla om autentiseringsmetod**.

 ![Tillåt att klienter tooconnect](./media/nps-extension-remote-desktop-gateway/image22.png)

6. Du kan också klicka på hello **villkor** fliken och Lägg till villkor som måste uppfyllas för hello anslutning toobe auktoriserad, till exempel medlemskap i en specifik Windows-grupp.

 ![Villkor](./media/nps-extension-remote-desktop-gateway/image23.png)

7. Klicka på **OK**. När du tillfrågas tooview hello motsvarande hjälpavsnitt, klickar du på **nr**.
8. Kontrollera att den nya principen är hello överst i listan hello att hello principen är aktiverad och att den ger åtkomst.

 ![Nätverksprinciper](./media/nps-extension-remote-desktop-gateway/image24.png)

## <a name="verify-configuration"></a>Verifiera konfigurationen
tooverify hello konfiguration, behöver du toolog på hello Remote Desktop Gateway med en lämplig RDP-klient. Vara säker på att toouse ett konto som tillåts av din anslutning auktoriseringsprinciper och är aktiverat för Azure MFA. 

Visa i hello bilden nedan kan du använda hello **webbåtkomst för fjärrskrivbord** sidan.

![Webbåtkomst till fjärrskrivbord](./media/nps-extension-remote-desktop-gateway/image25.png)

När du har angett dina autentiseringsuppgifter för primär autentisering, visas hello Remote Desktop ansluta dialogrutan statusen initierar anslutning till enligt nedan. 

Om du har autentiseras med sekundära hello autentiseringsmetoden som du tidigare har konfigurerat i Azure MFA är anslutna toohello resurs. Om det inte går att genomföra hello sekundära autentiseringen, nekas åtkomst tooresource. 

![Initiera anslutning](./media/nps-extension-remote-desktop-gateway/image26.png)

I hello exemplet nedan hello Authenticator är-appen på en Windows phone används tooprovide hello sekundära autentiseringen.

![Konton](./media/nps-extension-remote-desktop-gateway/image27.png)

När du har autentiserats med hello sekundära autentiseringsmetoden, loggas i hello servern för fjärrskrivbordsgateway som vanligt. Men eftersom du är obligatoriska toouse en sekundär autentiseringsmetod med en mobil app på en betrodd enhet är hello inloggningsprocessen säkrare än den annars skulle vara.

### <a name="view-event-viewer-logs-for-successful-logon-events"></a>Visa loggarna i Loggboken för lyckade inloggningshändelser
tooview hello lyckad inloggning händelser i hello Windows Loggboken, kan du utfärda följande Windows PowerShell-kommandot tooquery hello Windows Terminal Services och säkerhetsloggar Windows hello.

tooquery lyckad inloggning händelser i hello Gateway operativa loggar _(händelse loggboken\program- och tjänster Logs\Microsoft\Windows\TerminalServices-Gateway\Operational_, använda hello följande kommandon:

* _Get-WinEvent - loggnamn Microsoft-Windows-TerminalServices-Gateway/Operational_ | där {$_.ID - eq '300'} | FL 
* Detta kommando visar Windows-händelser som visar hello användaren uppfyllda resurskraven auktorisering princip (RD RAP) och har beviljats åtkomst.

![Visa Loggboken](./media/nps-extension-remote-desktop-gateway/image28.png)

* _Get-WinEvent - loggnamn Microsoft-Windows-TerminalServices-Gateway/Operational_ | där {$_.ID - eq ”200”} | FL
* Detta kommando visar hello-händelser som visas när användaren uppfyller kraven för anslutningen auktorisering princip.

![Anslutningsverifiering](./media/nps-extension-remote-desktop-gateway/image29.png)

Du kan också visa den här loggen och filter på händelse-ID, 300 och 200. tooquery lyckade inloggningshändelser i hello viewer säkerhetshändelseloggar, Använd hello följande kommando:

* _Get-WinEvent - loggnamn säkerhet_ | där {$_.ID - eq '6272'} | FL 
* Det här kommandot kan köras på antingen hello centrala NPS eller hello RD Gateway-servern. 

![Lyckade inloggningshändelser](./media/nps-extension-remote-desktop-gateway/image30.png)

Du kan också visa hello säkerhetsloggen eller hello nätverksprincip och åtkomsttjänster anpassad vy, enligt nedan:

![Nätverksprincip och åtkomsttjänster](./media/nps-extension-remote-desktop-gateway/image31.png)

På hello server där du installerade hello NPS-tillägg för Azure MFA, hittar du loggarna i Loggboken program specifika toohello tillägg på _program och tjänster Logs\Microsoft\AzureMfa_. 

![Loggar i Loggboken program](./media/nps-extension-remote-desktop-gateway/image32.png)

## <a name="troubleshoot-guide"></a>Felsöka programguiden

Om hello konfigurationen inte fungerar som förväntat, är hello första plats toostart tootroubleshoot tooverify som hello användaren är konfigurerad toouse Azure MFA. Hello användare ansluta toohello [Azure-portalen](https://portal.azure.com). Om användaren uppmanas att sekundära kontroll och kan autentisera, vet du att en felaktig konfiguration av Azure MFA.

Om Azure MFA arbetar för hello användare, bör du granska hello relevanta händelseloggar. Dessa inkluderar hello säkerhetshändelse, -Gateway fungerar och Azure MFA loggar som diskuteras i hello föregående avsnitt. 

Nedan visas ett exempel på utdata för säkerhetsloggen visar misslyckade inloggningsförsök-händelser (händelse-ID 6273).

![Misslyckade inloggningsförsök händelse](./media/nps-extension-remote-desktop-gateway/image33.png)

Nedan visas en relaterad händelse från hello AzureMFA loggar:

![Azure MFA-logg](./media/nps-extension-remote-desktop-gateway/image34.png)

tooperform avancerad felsökning av alternativ finns hello NPS loggfilerna där hello NPS-tjänsten är installerad. Dessa loggfiler skapas i _%SystemRoot%\System32\Logs_ mapp som kommaavgränsad textfiler. 

En beskrivning av dessa loggfiler finns i avsnittet [tolka NPS loggfilerna](https://technet.microsoft.com/library/cc771748.aspx). hello poster i dessa loggfiler kan vara svårt toointerpret utan att importera dem till ett kalkylblad eller en databas. Du hittar flera IAS-Parser online tooassist du vid tolkningen hello loggfiler. 

hello bilden nedan visar hello utdata i en sådan nedladdningsbara [shareware-programmet](http://www.deepsoftware.com/iasviewer). 

![Shareware-app](./media/nps-extension-remote-desktop-gateway/image35.png)

Slutligen för ytterligare felsökning av alternativ, kan du använda en protokollanalysator sådana [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). 

hello bilden nedan från Microsoft Message Analyzer visar nätverkstrafik filtreras i RADIUS-protokollet som innehåller hello användarnamn **Contoso\AliceC**.

![Microsoft Message Analyzer](./media/nps-extension-remote-desktop-gateway/image36.png)

## <a name="next-steps"></a>Nästa steg
[Hur tooget Azure Multi-Factor Authentication](multi-factor-authentication-versions-plans.md)

[Fjärrskrivbordsgateway och Azure Multi-Factor Authentication Server med RADIUS](multi-factor-authentication-get-started-server-rdg.md)

[Integrera dina lokala kataloger med Azure Active Directory](../active-directory/connect/active-directory-aadconnect.md)
