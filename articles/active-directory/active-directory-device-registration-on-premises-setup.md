---
title: "aaaSetting konfigurerar lokal villkorlig åtkomst med enhetsregistrering för Azure Active Directory | Microsoft Docs"
description: "En stegvis guide tooenabling villkorlig åtkomst tooon lokala program med hjälp av Active Directory Federation Services (AD FS) i Windows Server 2012 R2."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 6ae9df8b-31fe-4d72-9181-cf50cfebbf05
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 808e3b96873102aebae667153f588dcdb205e884
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="setting-up-on-premises-conditional-access-by-using-azure-active-directory-device-registration"></a>Konfigurera lokal villkorlig åtkomst med hjälp av Azure Active Directory-enhetsregistrering
När du behöver användare tooworkplace-ansluta sina personliga enheter toohello enhetsregistreringstjänsten för Azure Active Directory (AD Azure) kan sina enheter markeras som kända tooyour organisation. Följande är en stegvis guide för att aktivera villkorlig åtkomst tooon lokala program med hjälp av Active Directory Federation Services (AD FS) i Windows Server 2012 R2.

> [!NOTE]
> En licens för Office 365 eller Azure AD Premium-licens krävs när du använder enheter som är registrerade i Azure Active Directory device registrering service policyer. Dessa inkluderar principer som tillämpas av AD FS i lokala resurser.
> 
> Mer information om hello scenarier för villkorlig åtkomst för lokala resurser, se [ansluta tooworkplace från valfri enhet för SSO och sömlös andra faktor-autentisering över företagsprogram](https://technet.microsoft.com/library/dn280945.aspx).

Dessa funktioner är tillgängliga toocustomers som köper en Azure Active Directory Premium-licens.

## <a name="supported-devices"></a>Enheter som stöds
* Windows 7-domänanslutna enheter
* Windows 8.1 personliga och domänanslutna enheter
* iOS 6 och senare för hello Safari som webbläsare
* Android 4.0 eller senare, Samsung GS3 eller senare telefoner, Samsung Galaxy anmärkning 2 eller senare surfplattor

## <a name="scenario-prerequisites"></a>Krav för scenario
* Prenumerationen tooOffice 365 eller Azure Active Directory Premium
* En Azure Active Directory-klient
* Windows Server Active Directory (Windows Server 2008 eller senare)
* Uppdaterade schemat i Windows Server 2012 R2
* Licens för Azure Active Directory Premium
* Windows Server 2012 R2 Federation Services, konfigurerad för SSO tooAzure AD
* Windows Server 2012 R2 Web Application Proxy 
* Microsoft Azure Active Directory Connect (Azure AD Connect) [(hämta Azure AD Connect)](http://www.microsoft.com/en-us/download/details.aspx?id=47594)
* Verifierad domän

## <a name="known-issues-in-this-release"></a>Kända problem i den här versionen
* Principer för enhetsbaserad villkorlig åtkomst måste enheten objektet tillbakaskrivning tooActive Directory från Azure Active Directory. Det kan ta upp toothree timmar för enheten objekt toobe skrivs tillbaka tooActive Directory.
* iOS 7-enheter uppmanas alltid hello användaren tooselect certifikat vid autentisering av klientcertifikat.
* Vissa versioner av iOS 8 innan iOS 8.3 inte fungerar.

## <a name="scenario-assumptions"></a>Scenariot antaganden
Det här scenariot förutsätter att du har en hybridmiljö som består av en Azure AD-klient och en lokal Active Directory. Dessa klienter ska anslutas med Azure AD Connect, med en verifierad domän och med AD FS för enkel inloggning. Använd följande checklista toohelp du konfigurera din miljö enligt toohello krav hello.

## <a name="checklist-prerequisites-for-conditional-access-scenario"></a>Checklista: Krav för villkorlig åtkomst scenario
Anslut din Azure AD-klient med din lokala Active Directory-instans.

## <a name="configure-azure-active-directory-device-registration-service"></a>Konfigurera enhetsregistreringstjänsten för Azure Active Directory
Använd den här guiden toodeploy och konfigurera enhetsregistreringstjänsten för hello Azure Active Directory för din organisation.

Den här guiden förutsätter att du har konfigurerat Windows Server Active Directory och prenumererar tooMicrosoft Azure Active Directory. Se hello krav som beskrivs ovan.

toodeploy hello enhetsregistreringstjänsten för Azure Active Directory med Azure Active Directory klient fullständig hello uppgifter i hello följande checklista i ordning. När en referenslänk tar tooa konceptuellt avsnitt kan du returnera toothis checklista efteråt, så att du kan fortsätta med hello återstående aktiviteter. Vissa uppgifter inkluderar ett scenario validering steg som hjälper dig att bekräfta om hello steg har slutförts.

## <a name="part-1-enable-azure-active-directory-device-registration"></a>Del 1: Aktivera Azure Active Directory-enhetsregistrering
Gör så hello i hello checklista tooenable och konfigurera hello enhetsregistreringstjänsten för Azure Active Directory.

| Aktivitet | Referens | 
| --- | --- |
| Aktivera registrering av enheten i din Azure Active Directory-klient tooallow enheter toojoin hello arbetsplats. Som standard aktiveras inte Azure Multi-Factor Authentication för hello-tjänsten. Vi rekommenderar dock att du använder multi-Factor Authentication när du registrerar en enhet. Se till att AD FS är konfigurerat för en flerfunktionsautentiseringsleverantör innan du aktiverar Multifaktorautentisering i registreringstjänsten för Active Directory. |[Aktivera enhetsregistrering för Azure Active Directory](active-directory-device-registration-get-started.md)| 
|Enheter som identifierar din enhetsregistreringstjänsten för Azure Active Directory genom att söka efter välkända DNS-poster. Konfigurera företagets DNS så att enheter kan identifiera din enhetsregistreringstjänsten för Azure Active Directory. |[Konfigurera identifiering av Azure Active Directory device registration](active-directory-device-registration-get-started.md)| 


## <a name="part-2-deploy-and-configure-windows-server-2012-r2-active-directory-federation-services-and-set-up-a-federation-relationship-with-azure-ad"></a>Del 2: Distribuera och konfigurera Windows Server 2012 R2 Active Directory Federation Services och ställer in en federationsrelation med Azure AD

| Aktivitet | Referens |
| --- | --- |
| Distribuera Active Directory Domain Services med hello schematillägg för Windows Server 2012 R2. Du behöver inte tooupgrade någon av din domän domänkontrollanter tooWindows Server 2012 R2. hello schemat uppgraderingen är hello endast krav. |[Uppgradera schemat Active Directory Domain Services](#upgrade-your-active-directory-domain-services-schema) |
| Enheter som identifierar din enhetsregistreringstjänsten för Azure Active Directory genom att söka efter välkända DNS-poster. Konfigurera företagets DNS så att enheter kan identifiera din enhetsregistreringstjänsten för Azure Active Directory. |[Förbereda dina Active Directory-stöd för enheter](#prepare-your-active-directory-to-support-devices) |

## <a name="part-3-enable-device-writeback-in-azure-ad"></a>Del 3: Aktivera tillbakaskrivning av enhet i Azure AD
| Aktivitet | Referens |
| --- | --- |
| Slutföra två tillhör ”aktivera tillbakaskrivning av enheter i Azure AD Connect”. När du har det returnerade toothis guide. |[Aktivera tillbakaskrivning av enheter i Azure AD Connect](#upgrade-your-active-directory-domain-services-schema) |

## <a name="optional-part-4-enable-multi-factor-authentication"></a>[Valfritt] Del 4: Aktivera Multi-Factor Authentication
Vi rekommenderar starkt att du konfigurerar en hello flera alternativ för Multifaktorautentisering. Om du vill toorequire Multifaktorautentisering finns [väljer hello Multifaktorautentisering säkerhetslösning för du](../multi-factor-authentication/multi-factor-authentication-get-started.md). Den innehåller en beskrivning av varje lösning och länkar toohelp som du konfigurerar hello lösning av ditt val.

## <a name="part-5-verification"></a>Del 5: verifiering
hello distributionen är nu klar och du kan testa vissa scenarier. Använd hello följande länkar tooexperiment med hello-tjänsten och bekanta dig med dess funktioner.

| Aktivitet | Referens |
| --- | --- |
| Ansluta till vissa enheter tooyour arbetsplats med hjälp av enhetsregistreringstjänsten för Azure Active Directory. Du kan ansluta till iOS-, Windows- och Android-enheter. |[Ansluta enheter tooyour arbetsplats med enhetsregistreringstjänsten för Azure Active Directory](#join-devices-to-your-workplace-using-azure-active-directory-device-registration) |
| Visa och aktivera eller inaktivera registrerade enheter med hjälp av hello-administratörsportalen. I den här uppgiften visa vissa registrerade enheter med hjälp av hello-administratörsportalen. |[Azure Active Directory device översikt över enhetsregistreringstjänst](active-directory-device-registration-get-started.md) |
| Kontrollera att enhetsobjekt skrivs tillbaka från Azure Active Directory tooWindows Server Active Directory. |[Verifiera registrerade enheter skrivs tillbaka tooActive Directory](#verify-registered-devices-are-written-back-to-active-directory) |
| Nu när användarna kan registrera sina enheter, kan du skapa program åtkomstprinciper i AD FS som endast tillåta registrerade enheter. I det här steget skapar du en åtkomstregel för programmet och ett anpassat meddelande om nekad åtkomst. |[Skapa en programåtkomstprincip och ett anpassat meddelande om nekad åtkomst](#create-an-application-access-policy-and-custom-access-denied-message) |

## <a name="integrate-azure-active-directory-with-on-premises-active-directory"></a>Integrera Azure Active Directory med lokala Active Directory
Det här steget hjälper dig att integrera din Azure AD-klient med din lokala Active Directory med hjälp av Azure AD Connect. Även om hello åtgärder är tillgängliga i hello klassiska Azure-portalen, anteckna särskilda instruktioner som anges i det här avsnittet.

1. Logga in toohello klassiska Azure-portalen med ett konto som är en global administratör i Azure AD.
2. Hello vänster, Välj **Active Directory**.
3. På hello **Directory** , Välj din katalog.
4. Välj hello **katalogintegrering** fliken.
5. Under hello **distribuera och hantera** , följer du steg 1 till 3 toointegrate Azure Active Directory med din lokala katalog.
   
   1. Lägga till domäner.
   2. Installera och köra Azure AD Connect med hjälp av hello instruktionerna på [anpassad installation av Azure AD Connect](connect/active-directory-aadconnect-get-started-custom.md).
   3. Verifiera och hantera katalogsynkronisering. Instruktioner för enkel inloggning finns i det här steget.
   
   Dessutom konfigurera federation med AD FS som beskrivs i [anpassad installation av Azure AD Connect](connect/active-directory-aadconnect-get-started-custom.md).

## <a name="upgrade-your-active-directory-domain-services-schema"></a>Uppgradera schemat Active Directory Domain Services
> [!NOTE]
> När du har uppgraderat din Active Directory-schemat kan hello process inte ångras. Vi rekommenderar att du först uppgraderar hello i en testmiljö.
> 

1. Logga in tooyour domänkontrollanten med ett konto som har både Företagsadministratörer och schema-administratörsbehörighet.
2. Kopiera hello **[media] \support\adprep** katalog och underkataloger tooone av Active Directory-domänkontrollanter (där **[media]** är installationsmediet för hello sökvägen toohello Windows Server 2012 R2 ).
4. Gå från en kommandotolk toohello **adprep** katalog och kör **adprep.exe/forestprep**. Följ hello anvisningarna toocomplete hello schemat uppgraderingen.

## <a name="prepare-your-active-directory-toosupport-devices"></a>Förbereda dina Active Directory toosupport enheter
> [!NOTE]
> Det här är en engångsåtgärd som att du måste köra tooprepare dina Active Directory-skog toosupport enheter. toocomplete den här proceduren måste du vara inloggad med administratörsbehörighet för företag och Active Directory-skogen måste ha hello Windows Server 2012 R2-schemat.
> 


### <a name="prepare-your-active-directory-forest"></a>Förbered Active Directory-skog
1. På din federationsserver, öppna ett Windows PowerShell-kommandofönster och skriv **initiera ADDeviceRegistration**. 
2. När du tillfrågas om **ServiceAccountName**, ange hello namn för hello-tjänstkonto som du valde som hello tjänstkonto för AD FS. Om det är ett konto för gMSA anger hello konto i hello **domain\accountname$** format. Använd hello format för ett domänkonto **domain\accountname**.

### <a name="enable-device-authentication-in-ad-fs"></a>Aktivera enhetsautentisering i AD FS
1. Öppna hello AD FS-hanteringskonsolen på din federationsserver och gå för**AD FS** > **autentiseringsprinciper**.
2. På hello **åtgärder** väljer **redigera Global primär autentisering**.
3. Kontrollera **aktivera enhetsautentisering**, och välj sedan **OK**.
4. Om du regelbundet som standard AD FS tar du bort oanvända enheter från Active Directory. Inaktivera den här uppgiften när du använder enhetsregistreringstjänsten för Azure Active Directory så att enheter kan hanteras i Azure.

### <a name="disable-unused-device-cleanup"></a>Inaktivera oanvända enheten rensning
På din federationsserver, öppna ett Windows PowerShell-kommandofönster och skriv **Set-AdfsDeviceRegistration - MaximumInactiveDays 0**.

### <a name="prepare-azure-ad-connect-for-device-writeback"></a>Förbereda Azure AD Connect för tillbakaskrivning av enhet.
Slutföra del 1: förbereda Azure AD Connect.

## <a name="join-devices-tooyour-workplace-by-using-azure-active-directory-device-registration-service"></a>Ansluta enheter tooyour arbetsplats med hjälp av enhetsregistreringstjänsten för Azure Active Directory

### <a name="join-an-ios-device-by-using-azure-active-directory-device-registration"></a>Ansluta en iOS-enhet med hjälp av Azure Active Directory-enhetsregistrering
Azure Active Directory-enhetsregistrering använder hello luftburna registreringen för iOS-enheter. Den här processen startar när hello användaren ansluter toohello profil registrerings-URL med Safari. hello URL-format är:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/"yourdomainname"

I det här fallet `yourdomainname` hello domännamn som du har konfigurerat med Azure Active Directory. Till exempel om ditt domännamn är contoso.com, är hello URL följande:

    https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com

Det finns många olika sätt toocommunicate denna URL tooyour användare. En metod som rekommenderas publicera URL: en i ett anpassat program meddelande om nekad åtkomst i AD FS. Den här informationen ingår i nästa avsnitt av hello [skapa en programåtkomstprincip och ett anpassat meddelande om nekad åtkomst](#create-an-application-access-policy-and-custom-access-denied-message).

### <a name="join-a-windows-81-device-by-using-azure-active-directory-device-registration"></a>Ansluta till en Windows 8.1-enhet med hjälp av Azure Active Directory-enhetsregistrering
1. På din Windows 8.1-enhet väljer **datorinställningar** > **nätverk** > **arbetsplats**.
2. Ange ditt användarnamn i UPN-format. till exempel  **dan@contoso.com** .
3. Välj **ansluta**.
4. När du uppmanas logga in med dina autentiseringsuppgifter. hello-enhet är nu ansluten.

### <a name="join-a-windows-7-device-by-using-azure-active-directory-device-registration"></a>Ansluta till en Windows 7-enhet med hjälp av Azure Active Directory-enhetsregistrering
tooregister Windows 7-domänanslutna enheter behöver du toodeploy hello programpaket för enhetsregistrering. hello programpaket kallas Workplace Join för Windows 7 och dess tillgängliga för hämtning på hello [Microsoft Connect-webbplatsen](https://connect.microsoft.com/site1164). 

Instruktioner om hur toouse hello paketet finns i [hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory](active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="verify-that-registered-devices-are-written-back-tooactive-directory"></a>Kontrollera att registrerade enheter skrivs tillbaka tooActive Directory
Du kan visa och verifiera att din enhetsobjekt har skrivits tillbaka tooyour Active Directory med hjälp av LDP.exe eller ADSI-redigering. Båda är tillgängliga med hello verktyg för Active Directory-administratör.

Som standard enhetsobjekt som skrivs från Azure Active Directory placeras i hello samma domän som AD FS-gruppen.

    CN=RegisteredDevices,defaultNamingContext

## <a name="create-an-application-access-policy-and-custom-access-denied-message"></a>Skapa en programåtkomstprincip och ett anpassat meddelande om nekad åtkomst
Överväg att hello följande scenario: du skapar ett program förlitande part i AD FS och konfigurera en regel för auktorisering av utfärdande som tillåter endast registrerade enheter. Endast enheter som är registrerade tillåts nu tooaccess hello program. 

toomake det enkelt för användare toogain åtkomst toohello programmet du konfigurera ett anpassat meddelande om nekad åtkomst som innehåller instruktioner för hur toojoin sin enhet. Användarna har nu en sömlös sätt tooregister sina enheter så att de kan komma åt ett program.

hello följande steg visar hur tooimplement det här scenariot.

> [!NOTE]
> Det här avsnittet förutsätter att du redan har konfigurerat ett förtroende för förlitande part för programmet i AD FS.
> 

1. Öppna hello AD FS MMC-verktyg och välj sedan **AD FS** > **förtroenden** > **förtroende för förlitande part**.
2. Leta upp hello programmet toowhich nya Åtkomstregeln gäller. Högerklicka på programmet hello och välj sedan **redigera Anspråksregler**.
3. Välj hello **auktoriseringsregler** och välj sedan **Lägg till regel**.
4. Från hello **anspråksregel** mall listrutan, Välj **bevilja eller neka användare baserat på ett inkommande anspråk**. Välj sedan **nästa**.
5. I hello **Regelnamn för anspråk** anger **tillåta åtkomst från registrerade enheter**.
6. Från hello **inkommande Anspråkstyp** listrutan, Välj **är registrerad användare**.
7. I hello **inkommande anspråksvärde** anger **SANT**.
8. Välj hello **bevilja åtkomst toousers med detta inkommande anspråk** knappen.
9. Välj **Slutför**, och välj sedan **tillämpa**.
10. Ta bort alla regler som är mer Tillåtande än hello regeln som du skapade. Till exempel ta bort hello standardregel **Tillåt åtkomst tooall användare**.

Tillämpningsprogrammet är nu konfigurerad tooallow åtkomst till endast när hello användare kommer från en enhet som de registrerade och anslutna toohello arbetsplats. För mer avancerade åtkomstprinciper finns [hantera risker med ytterligare Multifaktorautentisering för känsliga program](https://technet.microsoft.com/library/dn280949.aspx).

Därefter konfigurerar du ett anpassat felmeddelande för ditt program. hello felmeddelande låter användarna veta att de måste ansluta deras enhet toohello arbetsplats innan de kan komma åt programmet hello. Du kan skapa meddelande om nekad åtkomst ett anpassat program med hjälp av anpassade HTML- och PowerShell.

På din federationsserver, öppna ett PowerShell-kommandofönster och skriv sedan följande kommando hello. Ersätt delar av hello kommando med objekt som är specifika tooyour system:

    Set-AdfsRelyingPartyWebContent -Name "relying party trust name" -ErrorPageAuthorizationErrorMessage
Du måste registrera enheten innan du kan komma åt det här programmet.

**Om du använder en iOS-enhet, Välj den här länken toojoin enhet**:

    a href='https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/yourdomain.com

Anslut den här iOS-enhet tooyour arbetsplats.

Om du använder en Windows 8.1-enhet kan du ansluta enheten genom att välja **datorinställningar**> **nätverk** > **arbetsplats**.

I hello föregående kommandon, **förlitande part förtroende namnet** är hello namnet på ditt program för förlitande part objekt i AD FS.
Och **yourdomain.com** hello domännamn som du har konfigurerat med Azure Active Directory (t.ex, contoso.com).
Vara säker på att tooremove eventuella radbrytningar (eventuella) från hello HTML innehåll som du skickar toohello **Set AdfsRelyingPartyWebContent** cmdlet.

När användare åtkomst till programmet från en enhet som inte är registrerat med hello enhetsregistreringstjänsten för Azure Active Directory, visas nu en sida som ser ut ungefär toohello följande skärmbild.

![Skärmbild av ett fel när användare inte har registrerat sin enhet med Azure AD](./media/active-directory-conditional-access/error-azureDRS-device-not-registered.gif)


