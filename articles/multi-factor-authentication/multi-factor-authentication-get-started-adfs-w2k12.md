---
title: aaaMFA Server med AD FS i Windows Server | Microsoft Docs
description: "Den här artikeln beskriver hur tooget igång med Azure Multi-Factor Authentication och AD FS i Windows Server 2012 R2 och 2016."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 57208068-1e55-45b6-840f-fdcd13723074
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/29/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 656785abcc63a020add765a86670b488a3b84b51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-azure-multi-factor-authentication-server-toowork-with-ad-fs-in-windows-server"></a>Konfigurera Azure-flerfunktionsautentisering toowork med AD FS i Windows Server
Om du använder Active Directory Federation Services (AD FS) och vill toosecure molnet eller lokala resurser kan du konfigurera Azure Multi-Factor Authentication-servern toowork med AD FS. Den här konfigurationen utlöser tvåstegsverifiering för slutpunkter med högt värde.

I den här artikeln diskuterar vi hur du använder Azure Multi-Factor Authentication Server med AD FS i Windows Server 2012 R2 eller Windows Server 2016. Mer information finns i avsnittet om hur för[skydda molnresurser och lokala resurser med Azure Multi-Factor Authentication-Server med AD FS 2.0](multi-factor-authentication-get-started-adfs-adfs2.md).

## <a name="secure-windows-server-ad-fs-with-azure-multi-factor-authentication-server"></a>Skydda Windows Server AD FS med Azure Multi-Factor Authentication Server
När du installerar Azure Multi-Factor Authentication-servern har hello följande alternativ:

* Installera Azure Multi-Factor Authentication-servern lokalt på hello samma server som AD FS
* Installera hello Azure Multi-Factor Authentication adaptern lokalt på hello AD FS-servern och sedan installera Multi-Factor Authentication-servern på en annan dator

Innan du börjar bör du vara medveten om hello följande information:

* Du är inte obligatoriska tooinstall Azure Multi-Factor Authentication-servern på AD FS-servern. Dock måste du installera hello Multi-Factor Authentication adaptern för AD FS på en Windows Server 2012 R2 eller Windows Server 2016 som kör AD FS. Du kan installera hello server på en annan dator om det är en version som stöds och du installerar separat hello AD FS-adaptern på din AD FS-federationsserver. Se hello följande procedurer toolearn hur tooinstall hello kortet separat.
* Om din organisation använder SMS eller mobilapp verifieringsmetoderna hello strängar som definierats i Företagsinställningar innehålla en platshållare <$*$application_name*$>. Du kan ange namnet på ett program som ersätter platshållaren i MFA Server v7.1. I v7.0 eller äldre ersätts platshållaren automatiskt inte när du använder hello AD FS-adaptern. För dessa äldre versioner att ta bort hello platshållare från strängar som hello när du skyddar AD FS.
* hello-konto som du använder toosign i måste ha användaren rättigheter toocreate säkerhetsgrupper i Active Directory-tjänsten.
* Installationsguiden för hello Multi-Factor Authentication AD FS-adapter skapar en säkerhetsgrupp med namnet PhoneFactor Admins i din instans av Active Directory. Hello AD FS-tjänstkontot för toothis för federation tjänstgruppen lägger sedan till. Kontrollera på domänkontrollanten som hello gruppen PhoneFactor Admins faktiskt har skapats och att hello AD FS-tjänstkontot är medlem i den här gruppen. Om det behövs kan du manuellt lägga till hello AD FS-tjänsten konto toohello gruppen PhoneFactor Admins på domänkontrollanten.
* Läs mer om information om hur du installerar hello webbtjänst-SDK med hello användarportalen [distribuera hello användarportalen för Azure Multi-Factor Authentication-servern.](multi-factor-authentication-get-started-portal.md)

### <a name="install-azure-multi-factor-authentication-server-locally-on-hello-ad-fs-server"></a>Installera Azure Multi-Factor Authentication-servern lokalt på hello AD FS-server
1. Hämta och installera Azure Multi-Factor Authentication Server på AD FS-servern. Mer information om installationen finns i [Komma igång med Azure Multi-Factor Authentication Server](multi-factor-authentication-get-started-server.md).
2. I hanteringskonsolen för hello Azure Multi-Factor Authentication-servern klickar du på hello **AD FS** ikon. Välj alternativ för hello **Tillåt användarregistrering** och **användarna tooselect metoden**.
3. Välj eventuella ytterligare alternativ som du vill att toospecify för din organisation.
4. Klicka på **Installera AD FS-adapter**.
   
   <center>![Moln](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>

5. Om hello Active Directory-fönstret visas, betyder det två saker. Datorn är domänansluten tooa domän och hello Active Directory-konfigurationen för säker kommunikation mellan hello AD FS-adaptern och hello Multi-Factor Authentication-tjänsten är ofullständig. Klicka på **nästa** tooautomatically slutföra den här konfigurationen eller markera hello **hoppa över automatisk konfiguration av Active Directory och konfigurera inställningarna manuellt** kryssrutan. Klicka på **Nästa**.
6. Om hello lokal grupp windows visas, betyder det två saker. Datorn är inte kopplade tooa domän och hello lokala gruppens konfiguration för säker kommunikation mellan hello AD FS-adaptern och hello Multi-Factor Authentication-tjänsten är ofullständig. Klicka på **nästa** tooautomatically slutföra den här konfigurationen eller markera hello **hoppa över automatisk konfiguration av lokal grupp och konfigurera inställningarna manuellt** kryssrutan. Klicka på **Nästa**.
7. I installationsguiden för hello klickar du på **nästa**. Azure Multi-Factor Authentication-servern skapar hello PhoneFactor Admins-gruppen och lägger till hello AD FS-tjänsten konto toohello gruppen PhoneFactor Admins.
   <center>![Moln](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. På hello **starta installationsprogrammet** klickar du på **nästa**.
9. Klicka i hello Multi-Factor Authentication AD FS kortet installer **nästa**.
10. Klicka på **Stäng** när hello installationen är klar.
11. När hello nätverkskort har installerats, måste du registrera den med AD FS. Öppna Windows PowerShell och kör följande kommando hello:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
    <center>![Moln](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. toouse nätverkskortet nyregistrerade redigera hello globala autentiseringsprincipen i AD FS. I hello AD FS-hanteringskonsolen gå toohello **autentiseringsprinciper** nod. I hello **Multifaktorautentisering** klickar du på hello **redigera** länka nästa toohello **globala inställningar** avsnitt. I hello **redigera Global autentiseringsprincip** väljer **Multifaktorautentisering** som ytterligare autentiseringsmetod och klicka sedan på **OK**. hello adaptern registreras som WindowsAzureMultiFactorAuthentication. Starta om hello AD FS-tjänsten för hello registrering tootake effekt.

<center>![Moln](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

Nu har Multi-Factor Authentication-Server ställts in toobe en ytterligare autentisering providern toouse med AD FS.

## <a name="install-a-standalone-instance-of-hello-ad-fs-adapter-by-using-hello-web-service-sdk"></a>Installera en fristående instans av hello AD FS-adapter med hello webbtjänst-SDK
1. Installera hello webbtjänst-SDK på hello-server som kör Multi-Factor Authentication-servern.
2. Kopiera hello följande filer från hello \program\multi-Factor-Factor Authentication-servern toohello katalogserver när du planerar tooinstall hello AD FS-adaptern:
   * MultiFactorAuthenticationAdfsAdapterSetup64.msi
   * Register-MultiFactorAuthenticationAdfsAdapter.ps1
   * Unregister-MultiFactorAuthenticationAdfsAdapter.ps1
   * MultiFactorAuthenticationAdfsAdapter.config
3. Kör installationsfilen för hello sedan filerna MultiFactorAuthenticationAdfsAdapterSetup64.msi.
4. Klicka i hello Multi-Factor Authentication AD FS kortet installer **nästa** toostart hello installation.
5. Klicka på **Stäng** när hello installationen är klar.

## <a name="edit-hello-multifactorauthenticationadfsadapterconfig-file"></a>Redigera hello MultiFactorAuthenticationAdfsAdapter.config fil
Följ dessa steg tooedit hello MultiFactorAuthenticationAdfsAdapter.config fil:

1. Ange hello **UseWebServiceSdk** nod för**SANT**.  
2. Hello-värdet för **in WebServiceSdkUrl** toohello URL för hello Multi-Factor Authentication webbtjänst-SDK. Till exempel: *https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx*, där *certificatename* är hello namnet på ditt certifikat.  
3. Redigera hello Register-MultiFactorAuthenticationAdfsAdapter.ps1 skript genom att lägga till *- ConfigurationFilePath &lt;sökväg&gt;*  toohello slutet av hello `Register-AdfsAuthenticationProvider` kommando, där  *&lt;sökväg&gt;*  är hello fullständig sökväg toohello MultiFactorAuthenticationAdfsAdapter.config fil.

### <a name="configure-hello-web-service-sdk-with-a-username-and-password"></a>Konfigurera hello webbtjänst-SDK med ett användarnamn och lösenord
Det finns två alternativ för att konfigurera hello webbtjänst-SDK. hello först är med ett användarnamn och lösenord är hello andra med ett klientcertifikat. Följ dessa steg för hello första alternativet, eller kan du gå vidare för hello andra.  

1. Hello-värdet för **in WebServiceSdkUsername** tooan konto som är medlem i hello säkerhetsgruppen PhoneFactor Admins. Använd hello &lt;domän&gt;&#92;&lt; användarnamnet&gt; format.  
2. Hello-värdet för **in WebServiceSdkPassword** toohello rätt kontolösenord.

### <a name="configure-hello-web-service-sdk-with-a-client-certificate"></a>Konfigurera hello webbtjänst-SDK med ett klientcertifikat
Om du inte vill toouse ett användarnamn och lösenord, följer du dessa steg tooconfigure hello webbtjänst-SDK med ett klientcertifikat.

1. Hämta ett certifikat från en certifikatutfärdare för hello-server som kör hello webbtjänst-SDK. Lär dig hur för[Hämta klientcertifikat](https://technet.microsoft.com/library/cc770328.aspx).  
2. Importera hello klienten certifikat toohello lokala datorns personliga certifikatarkiv på hello-server som kör hello webbtjänst-SDK. Kontrollera att hello certifikatutfärdarens offentliga certifikat i certifikatarkivet för betrodda rotcertifikat.  
3. Exportera hello offentliga och privata nycklar hello klienten certifikatet tooa .pfx-fil.  
4. Exportera hello offentliga nyckeln i Base64-format tooa .cer-fil.  
5. I Serverhanteraren Kontrollera hello webbserver (IIS) \Web Server\Security\IIS autentisering av klientcertifikatsmappning funktionen är installerad. Om den inte är installerad väljer **Lägg till roller och funktioner** tooadd den här funktionen.  
6. I IIS-hanteraren, dubbelklicka på **Konfigurationsredigeraren** i hello-webbplats som innehåller hello webbtjänst-SDK virtuell katalog. Det är viktigt tooselect hello webbplats, inte hello virtuell katalog.  
7. Gå toohello **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** avsnitt.  
8. Ange aktiverad för**SANT**.  
9. Ställ in oneToOneCertificateMappingsEnabled för**SANT**.  
10. Klicka på hello **...**  knappen Nästa toooneToOneMappings och klicka sedan på hello **Lägg till** länk.  
11. Öppna hello Base64 .cer-fil som du exporterade tidigare. Ta bort *-----BEGIN CERTIFICATE-----*, *-----END CERTIFICATE-----* och eventuella radbrytningar. Kopiera hello resulterande strängen.  
12. Ange certifikatet toohello strängen som du kopierade i föregående steg hello.  
13. Ange aktiverad för**SANT**.  
14. Ange användarnamn tooan konto som är medlem i hello säkerhetsgruppen PhoneFactor Admins. Använd hello &lt;domän&gt;&#92;&lt; användarnamnet&gt; format.  
15. Ange hello lösenord toohello rätt kontolösenord och stäng sedan Redigeraren för säkerhetskonfiguration.  
16. Klicka på hello **tillämpa** länk.  
17. Dubbelklicka i hello webbtjänst-SDK virtuella katalogen **autentisering**.  
18. Kontrollera att ASP.NET Impersonation and Basic Authentication är inställda för**aktiverad**, och alla andra objekt för**inaktiverade**.  
19. Dubbelklicka i hello webbtjänst-SDK virtuella katalogen **SSL-inställningar**.  
20. Ställ in klientcertifikat för**acceptera**, och klicka sedan på **tillämpa**.  
21. Kopiera hello PFX-filen som du exporterade tidigare toohello-server som kör hello AD FS-adaptern.  
22. Importera hello .pfx-filen toohello lokala datorns personliga certifikatarkiv.  
23. Högerklicka och välj **hantera privata nycklar**, och sedan bevilja läsbehörighet toohello konto du använde toosign i toohello AD FS-tjänsten.  
24. Öppna hello klienten certifikat och kopiera hello tumavtrycket från hello **information** fliken.  
25. I filen MultiFactorAuthenticationAdfsAdapter.config hello anger **WebServiceSdkCertificateThumbprint** toohello sträng kopierade i föregående steg i hello.  

Slutligen tooregister hello-kort, kör hello \program\multi-Factor-Factor Authentication Server\Register-multifactorauthenticationadfsadapter.ps1 i PowerShell. hello adaptern registreras som WindowsAzureMultiFactorAuthentication. Starta om hello AD FS-tjänsten för hello registrering tootake effekt.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Skydda Azure AD-resurser med hjälp av AD FS
toosecure din molnresursen, konfigurera en regel för anspråk så att Active Directory Federation Services avger hello multipleauthn anspråk när en användare utför tvåstegsverifiering har. Denna begäran har skickats på tooAzure AD. Följ den här proceduren toowalk hello stegen:

1. Öppna AD FS-hantering.
2. Hello vänster markerar **förtroende för förlitande part**.
3. Högerklicka på **Microsoft Office 365 Identity Platform** och välj **Redigera anspråksregler...**

   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip1.png)

4. För Utfärdande av transformeringsregler klickar du på **Lägg till regel.**

   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip2.png)

5. På Hej guiden Lägg till transformera anspråk, Välj **släpp igenom eller filtrera ett inkommande anspråk** hello listrutan och klicka på **nästa**.

   ![Molnet](./media/multi-factor-authentication-get-started-adfs-cloud/trustedip3.png)

6. Namnge din regel.
7. Välj **autentiseringsmetodernas referenser** hello inkommande anspråkstyp.
8. Välj **Släpp igenom alla anspråksvärden**.
    ![Guiden Lägg till anspråksregel för transformering](./media/multi-factor-authentication-get-started-adfs-cloud/configurewizard.png)
9. Klicka på **Slutför**. Stäng hello AD FS-hanteringskonsol.

## <a name="related-topics"></a>Relaterade ämnen
Hjälp med felsökning finns hello [Azure Multi-Factor Authentication vanliga frågor och svar](multi-factor-authentication-faq.md)
