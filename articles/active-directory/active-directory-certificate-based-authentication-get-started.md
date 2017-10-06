---
title: "aaaAzure certifikatbaserad autentisering i Active Directory - komma igång | Microsoft Docs"
description: "Lär dig hur tooconfigure certifikatbaserad autentisering i din miljö"
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: c6ad7640-8172-4541-9255-770f39ecce0e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 3c73bdf56018c0716085c923a61e9560dbe4004c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a>Komma igång med certifikatbaserad autentisering i Azure Active Directory

Certifikatbaserad autentisering kan du toobe autentiseras av Azure Active Directory med ett klientcertifikat på en Windows-, Android eller iOS-enhet när du ansluter din Exchange-onlinekonto till: 

- Mobila Office-program, till exempel Microsoft Outlook och Microsoft Word   

- Exchange ActiveSync (EAS) klienter 

Konfigurera den här funktionen eliminerar hello måste tooenter användarnamnet och lösenordet till vissa e-post och Microsoft Office-program på din mobila enhet. 

Det här avsnittet:

- Ger du med hello steg tooconfigure använda certifikatbaserad autentisering för användare av klienter i Office 365 Enterprise-, Business-, Education och USA: S regering planer. Den här funktionen är tillgängliga i förhandsversionen i Office 365 Kina US Government skydd och US Government Federal planer. 

- Förutsätter att du redan har en [infrastruktur för offentliga nycklar (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) och [AD FS](connect/active-directory-aadconnectfed-whatis.md) konfigurerats.    


## <a name="requirements"></a>Krav

tooconfigure certifikatbaserad autentisering hello måste följande vara sant:  

- Certifikatbaserad autentisering (CBA) stöds endast för federerat miljöer för webbläsarprogram eller interna klienter som använder modern autentisering (ADAL). hello ett undantag är Exchange Active Sync (EAS) för EXO som kan användas för både federerade och hanterade konton. 

- hello rotcertifikatutfärdare och alla mellanliggande certifikatutfärdare måste konfigureras i Azure Active Directory.  

- Varje certifikatutfärdare måste ha en lista certifikat (CRL) som kan refereras via en Internetuppkopplad URL.  

- Du måste ha minst en certifikatutfärdare som konfigurerats i Azure Active Directory. Du kan hitta relaterade steg i hello [konfigurera hello certifikatutfärdare](#step-2-configure-the-certificate-authorities) avsnitt.  

- För Exchange ActiveSync-klienter har hello klientcertifikatet hello användarens dirigerbara e-post-adressen i Exchange online i antingen hello huvudnamn eller hello RFC822 namn-värde i fältet för hello Alternativt ämnesnamn. Azure Active Directory mappar hello RFC822 värdet toohello proxyadress attribut i hello katalog.  

- Klientenheten måste ha åtkomst tooat minst en certifikatutfärdare som utfärdar certifikat.  

- Ett klientcertifikat för klientautentisering måste ha utfärdats tooyour klienten.  




## <a name="step-1-select-your-device-platform"></a>Steg 1: Välj din enhetsplattform

Som ett första steg behöver för hello enhetsplattform som du är intresserad, du tooreview hello följande:

- stöd för mobila hello Office-program 
- hello specifik implementeringskrav  

hello relaterad information finns för hello följande enhetsplattformar:

- [Android](active-directory-certificate-based-authentication-android.md)
- [iOS](active-directory-certificate-based-authentication-ios.md)


## <a name="step-2-configure-hello-certificate-authorities"></a>Steg 2: Konfigurera hello certifikatutfärdare 

tooconfigure överför din certifikatutfärdare i Azure Active Directory, för varje certifikatutfärdare hello följande: 

* Hej offentliga delen av hello certifikat i *.cer* format 
* hello finns mot URL: er där hello Internet listor över återkallade certifikat (CRL)

hello-schemat för en certifikatutfärdare som ser ut som följer: 

    class TrustedCAsForPasswordlessAuth 
    { 
       CertificateAuthorityInformation[] certificateAuthorities;    
    } 

    class CertificateAuthorityInformation 

    { 
        CertAuthorityType authorityType; 
        X509Certificate trustedCertificate; 
        string crlDistributionPoint; 
        string deltaCrlDistributionPoint; 
        string trustedIssuer; 
        string trustedIssuerSKI; 
    }                

    enum CertAuthorityType 
    { 
        RootAuthority = 0, 
        IntermediateAuthority = 1 
    } 

Du kan använda hello hello konfiguration, [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0):  

1. Starta Windows PowerShell med administratörsbehörighet. 
2. Installera hello Azure AD-modulen. Du behöver tooinstall Version [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) eller högre.  
   
        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33 

Som ett första konfigurationssteg måste du tooestablish en anslutning med din klient. Så snart det finns en anslutning tooyour klient, kan du granska, lägga till, ta bort och ändra hello betrodda certifikatutfärdare som har definierats i din katalog. 

### <a name="connect"></a>Anslut

tooestablish en anslutning till din klient, Använd hello [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet:

    Connect-AzureAD 


### <a name="retrieve"></a>Hämta 

tooretrieve hello betrodda certifikatutfärdare som har definierats i din katalog använder hello [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet. 

    Get-AzureADTrustedCertificateAuthority 
 

### <a name="add"></a>Lägg till

toocreate en betrodd certifikatutfärdare använder hello [ny AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet och ange hello **crlDistributionPoint** attributet tooa rätt värde: 
   
    $cert=Get-Content -Encoding byte "[LOCATION OF hello CER FILE]" 
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
    $new_ca.AuthorityType=0 
    $new_ca.TrustedCertificate=$cert 
    $new_ca.crlDistributionPoint=”<CRL Distribution URL>”
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 


### <a name="remove"></a>Ta bort

tooremove en betrodd certifikatutfärdare använder hello [ta bort AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:
   
    $c=Get-AzureADTrustedCertificateAuthority 
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiy"></a>Modfiy

toomodify en betrodd certifikatutfärdare använder hello [Set AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:

    $c=Get-AzureADTrustedCertificateAuthority 
    $c[0].AuthorityType=1 
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 


## <a name="step-3-configure-revocation"></a>Steg 3: Konfigurera återkallade certifikat

toorevoke ett klientcertifikat, Azure Active Directory hämtar listan över återkallade certifikat (CRL) från URL: er hello överförs som en del av information om certifikatutfärdare och cachelagrar hello-certifikat. hello publicera senaste tidsstämpel (**giltighetsdatum** egenskapen) i hello CRL används tooensure hello CRL fortfarande är giltigt. hello CRL är regelbundet refererade toorevoke åtkomst toocertificates som är en del av hello-listan.

Om det krävs mer instant återkallas (till exempel om en användare förlorar en enhet), kan hello Autentiseringstoken för hello användare vara ogiltig. tooinvalidate hello auktorisering token, Ställ in hello **StsRefreshTokenValidFrom** för den aktuella användaren med Windows PowerShell. Du måste uppdatera hello **StsRefreshTokenValidFrom** för varje användare som du vill toorevoke åtkomst för.

tooensure som kvarstår hello återkallade certifikat, måste du ange hello **giltighetsdatum** av hello CRL tooa datum efter hello-värde som anges av **StsRefreshTokenValidFrom** och kontrollera hello certifikatet i fråga finns i hello CRL.

Hej följande steg disposition hello processen för att uppdatera och ogiltigförklara hello autentiseringstoken genom att ange hello **StsRefreshTokenValidFrom** fältet. 

**tooconfigure återkallade certifikat:** 

1. Anslut med autentiseringsuppgifterna toohello MSOL administrationstjänsten: 
   
        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

2. Hämta hello aktuella StsRefreshTokensValidFrom värdet för en användare: 
   
        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 

3. Konfigurera ett nytt StsRefreshTokensValidFrom värde för hello användaren lika toohello aktuell tidsstämpel: 
   
        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

hello datum som du anger måste finnas i framtida hello. Om hello datumet inte är i hello framtida hello **StsRefreshTokensValidFrom** egenskapen har inte angetts. Om hello datum i hello framtida **StsRefreshTokensValidFrom** anges toohello aktuell tid (inte hello datum som anges av kommandot Set-MsolUser). 


## <a name="step-4-test-your-configuration"></a>Steg 4: Testa din konfiguration

### <a name="testing-your-certificate"></a>Testa ditt certifikat

Som ett första konfigurationstest bör du försöka toosign i för[Outlook Web Access](https://outlook.office365.com) eller [SharePoint Online](https://microsoft.sharepoint.com) med din **på enhetens webbläsare**.

Om din inloggning lyckades, vet som:

- hello användarcertifikat har etablerat tooyour testenhet
- AD FS är korrekt konfigurerad  


### <a name="testing-office-mobile-applications"></a>Testa mobila Office-program

**tootest certifikatbaserad autentisering på din mobila Office-program:** 

1. Installera ett mobila Office-program (t.ex. OneDrive) på enheten test.
3. Starta hello program. 
4. Ange ditt användarnamn och välj sedan hello användarcertifikat som du vill toouse. 

Du bör vara loggat in. 

### <a name="testing-exchange-activesync-client-applications"></a>Testa program för Exchange ActiveSync-klienten

tooaccess Exchange ActiveSync (EAS) via en EAS-profil som innehåller hello klientcertifikat certifikatbaserad autentisering måste vara tillgängliga toohello program. 

hello EAS-profil måste innehålla hello följande information:

- hello användaren certifikat toobe används för autentisering 

- hello EAS-slutpunkt (till exempel outlook.office365.com)

EAS-profil kan konfigureras och placeras på hello enhet via hello användning av hantering av mobila enheter (MDM), till exempel Intune eller genom att manuellt hello certifikat hello EAS-profil på hello enhet.  

### <a name="testing-eas-client-applications-on-android"></a>Testa EAS klientprogram på Android

**tootest autentisering med datorcertifikat:**  

1. Konfigurera en EAS-profil i hello-programmet som uppfyller hello kraven ovan.  
2. Öppna hello program och kontrollera att du synkroniserar e-post. 

