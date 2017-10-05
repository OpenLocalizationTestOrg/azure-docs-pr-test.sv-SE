---
title: "Azure Active Directory certifikatbaserad autentisering - komma igång | Microsoft Docs"
description: "Lär dig hur du konfigurerar certifikatbaserad autentisering i din miljö"
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
ms.openlocfilehash: 8ebc6f2dd7502fd75ffdd4d5d68338382cb1a46b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a><span data-ttu-id="17463-103">Komma igång med certifikatbaserad autentisering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="17463-103">Get started with certificate-based authentication in Azure Active Directory</span></span>

<span data-ttu-id="17463-104">Certifikatbaserad autentisering gör att du ska kunna autentiseras av Azure Active Directory med ett klientcertifikat på en Windows-, Android eller iOS-enhet när du ansluter din Exchange-onlinekonto till:</span><span class="sxs-lookup"><span data-stu-id="17463-104">Certificate-based authentication enables you to be authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

- <span data-ttu-id="17463-105">Mobila Office-program, till exempel Microsoft Outlook och Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="17463-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   

- <span data-ttu-id="17463-106">Exchange ActiveSync (EAS) klienter</span><span class="sxs-lookup"><span data-stu-id="17463-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="17463-107">Konfigurera den här funktionen eliminerar behovet av att ange en kombination av användarnamn och lösenord i vissa e-post och Microsoft Office-program på din mobila enhet.</span><span class="sxs-lookup"><span data-stu-id="17463-107">Configuring this feature eliminates the need to enter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="17463-108">Det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="17463-108">This topic:</span></span>

- <span data-ttu-id="17463-109">Ger dig stegen för att konfigurera och använda certifikatbaserad autentisering för användare av klienter i Office 365 Enterprise, företag, Education och som tillhör amerikanska myndigheter planer.</span><span class="sxs-lookup"><span data-stu-id="17463-109">Provides you with the steps to configure and utilize certificate-based authentication for users of tenants in Office 365 Enterprise, Business, Education, and US Government plans.</span></span> <span data-ttu-id="17463-110">Den här funktionen är tillgängliga i förhandsversionen i Office 365 Kina US Government skydd och US Government Federal planer.</span><span class="sxs-lookup"><span data-stu-id="17463-110">This feature is available in preview in Office 365 China, US Government Defense, and US Government Federal plans.</span></span> 

- <span data-ttu-id="17463-111">Förutsätter att du redan har en [infrastruktur för offentliga nycklar (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) och [AD FS](connect/active-directory-aadconnectfed-whatis.md) konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="17463-111">Assumes that you already have a [public key infrastructure (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) and [AD FS](connect/active-directory-aadconnectfed-whatis.md) configured.</span></span>    


## <a name="requirements"></a><span data-ttu-id="17463-112">Krav</span><span class="sxs-lookup"><span data-stu-id="17463-112">Requirements</span></span>

<span data-ttu-id="17463-113">Följande om du vill konfigurera certifikatbaserad autentisering, den måste vara sant:</span><span class="sxs-lookup"><span data-stu-id="17463-113">To configure certificate-based authentication, the following must be true:</span></span>  

- <span data-ttu-id="17463-114">Certifikatbaserad autentisering (CBA) stöds endast för federerat miljöer för webbläsarprogram eller interna klienter som använder modern autentisering (ADAL).</span><span class="sxs-lookup"><span data-stu-id="17463-114">Certificate-based authentication (CBA) is only supported for Federated environments for browser applications or native clients using modern authentication (ADAL).</span></span> <span data-ttu-id="17463-115">Det enda undantaget är Exchange Active Sync (EAS) för EXO som kan användas för både federerade och hanterade konton.</span><span class="sxs-lookup"><span data-stu-id="17463-115">The one exception is Exchange Active Sync (EAS) for EXO which can be used for both, federated and managed accounts.</span></span> 

- <span data-ttu-id="17463-116">Rotcertifikatutfärdaren och alla mellanliggande certifikatutfärdare måste konfigureras i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="17463-116">The root certificate authority and any intermediate certificate authorities must be configured in Azure Active Directory.</span></span>  

- <span data-ttu-id="17463-117">Varje certifikatutfärdare måste ha en lista certifikat (CRL) som kan refereras via en Internetuppkopplad URL.</span><span class="sxs-lookup"><span data-stu-id="17463-117">Each certificate authority must have a certificate revocation list (CRL) that can be referenced via an Internet facing URL.</span></span>  

- <span data-ttu-id="17463-118">Du måste ha minst en certifikatutfärdare som konfigurerats i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="17463-118">You must have at least one certificate authority configured in Azure Active Directory.</span></span> <span data-ttu-id="17463-119">Du kan hitta relaterade stegen i den [konfigurera certificate myndigheter](#step-2-configure-the-certificate-authorities) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="17463-119">You can find related steps in the [Configure the certificate authorities](#step-2-configure-the-certificate-authorities) section.</span></span>  

- <span data-ttu-id="17463-120">För Exchange ActiveSync-klienter, måste klientcertifikatet ha användarens dirigerbara e-postadress i Exchange online i huvudnamn eller RFC822 namn-värde i fältet Alternativt ämnesnamn.</span><span class="sxs-lookup"><span data-stu-id="17463-120">For Exchange ActiveSync clients, the client certificate must have the user’s routable email address in Exchange online in either the Principal Name or the RFC822 Name value of the Subject Alternative Name field.</span></span> <span data-ttu-id="17463-121">Azure Active Directory mappar RFC822 värdet till attributet proxyadress i katalogen.</span><span class="sxs-lookup"><span data-stu-id="17463-121">Azure Active Directory maps the RFC822 value to the Proxy Address attribute in the directory.</span></span>  

- <span data-ttu-id="17463-122">Klientenheten måste ha tillgång till minst en certifikatutfärdare som utfärdar certifikat.</span><span class="sxs-lookup"><span data-stu-id="17463-122">Your client device must have access to at least one certificate authority that issues client certificates.</span></span>  

- <span data-ttu-id="17463-123">Ett klientcertifikat för klientautentisering måste har utfärdats till klienten.</span><span class="sxs-lookup"><span data-stu-id="17463-123">A client certificate for client authentication must have been issued to your client.</span></span>  




## <a name="step-1-select-your-device-platform"></a><span data-ttu-id="17463-124">Steg 1: Välj din enhetsplattform</span><span class="sxs-lookup"><span data-stu-id="17463-124">Step 1: Select your device platform</span></span>

<span data-ttu-id="17463-125">Som ett första steg för den enhetsplattform som du bryr dig om du behöver för att granska följande:</span><span class="sxs-lookup"><span data-stu-id="17463-125">As a first step, for the device platform you care about, you need to review the following:</span></span>

- <span data-ttu-id="17463-126">Stöd för mobila Office-program</span><span class="sxs-lookup"><span data-stu-id="17463-126">The Office mobile applications support</span></span> 
- <span data-ttu-id="17463-127">Krav för specifik implementering</span><span class="sxs-lookup"><span data-stu-id="17463-127">The specific implementation requirements</span></span>  

<span data-ttu-id="17463-128">Det finns relaterad information för följande enhetsplattformar:</span><span class="sxs-lookup"><span data-stu-id="17463-128">The related information exists for the following device platforms:</span></span>

- [<span data-ttu-id="17463-129">Android</span><span class="sxs-lookup"><span data-stu-id="17463-129">Android</span></span>](active-directory-certificate-based-authentication-android.md)
- [<span data-ttu-id="17463-130">iOS</span><span class="sxs-lookup"><span data-stu-id="17463-130">iOS</span></span>](active-directory-certificate-based-authentication-ios.md)


## <a name="step-2-configure-the-certificate-authorities"></a><span data-ttu-id="17463-131">Steg 2: Konfigurera på certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="17463-131">Step 2: Configure the certificate authorities</span></span> 

<span data-ttu-id="17463-132">Om du vill konfigurera din certifikatutfärdare i Azure Active Directory för varje certifikatutfärdare, överför du följande:</span><span class="sxs-lookup"><span data-stu-id="17463-132">To configure your certificate authorities in Azure Active Directory, for each certificate authority, upload the following:</span></span> 

* <span data-ttu-id="17463-133">Den offentliga delen av certifikatet i *.cer* format</span><span class="sxs-lookup"><span data-stu-id="17463-133">The public portion of the certificate, in *.cer* format</span></span> 
* <span data-ttu-id="17463-134">Internet facing URL: er där den listor över återkallade certifikat (CRL) finns</span><span class="sxs-lookup"><span data-stu-id="17463-134">The Internet facing URLs where the Certificate Revocation Lists (CRLs) reside</span></span>

<span data-ttu-id="17463-135">Schemat för en certifikatutfärdare som ser ut som följer:</span><span class="sxs-lookup"><span data-stu-id="17463-135">The schema for a certificate authority looks as follows:</span></span> 

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

<span data-ttu-id="17463-136">Konfiguration, kan du använda den [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0):</span><span class="sxs-lookup"><span data-stu-id="17463-136">For the configuration, you can use the [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0):</span></span>  

1. <span data-ttu-id="17463-137">Starta Windows PowerShell med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="17463-137">Start Windows PowerShell with administrator privileges.</span></span> 
2. <span data-ttu-id="17463-138">Installera Azure AD-modulen.</span><span class="sxs-lookup"><span data-stu-id="17463-138">Install the Azure AD module.</span></span> <span data-ttu-id="17463-139">Du måste installera Version [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) eller högre.</span><span class="sxs-lookup"><span data-stu-id="17463-139">You need to install Version [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) or higher.</span></span>  
   
        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33 

<span data-ttu-id="17463-140">Som ett första konfigurationssteg måste du upprätta en anslutning till din klient.</span><span class="sxs-lookup"><span data-stu-id="17463-140">As a first configuration step, you need to establish a connection with your tenant.</span></span> <span data-ttu-id="17463-141">Så snart det finns en anslutning till din klient, kan du granska, lägga till, ta bort och ändra betrodda certifikatutfärdare som har definierats i din katalog.</span><span class="sxs-lookup"><span data-stu-id="17463-141">As soon as a connection to your tenant exists, you can review, add, delete and modify the trusted certificate authorities that are defined in your directory.</span></span> 

### <a name="connect"></a><span data-ttu-id="17463-142">Anslut</span><span class="sxs-lookup"><span data-stu-id="17463-142">Connect</span></span>

<span data-ttu-id="17463-143">För att upprätta en anslutning till din klient använder de [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="17463-143">To establish a connection with your tenant, use the [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet:</span></span>

    Connect-AzureAD 


### <a name="retrieve"></a><span data-ttu-id="17463-144">Hämta</span><span class="sxs-lookup"><span data-stu-id="17463-144">Retrieve</span></span> 

<span data-ttu-id="17463-145">Använd för att hämta de betrodda certifikatutfärdare som har definierats i din katalog i [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="17463-145">To retrieve the trusted certificate authorities that are defined in your directory, use the [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet.</span></span> 

    Get-AzureADTrustedCertificateAuthority 
 

### <a name="add"></a><span data-ttu-id="17463-146">Lägg till</span><span class="sxs-lookup"><span data-stu-id="17463-146">Add</span></span>

<span data-ttu-id="17463-147">Så här skapar du en betrodd certifikatutfärdare på [ny AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet och ange den **crlDistributionPoint** attribut till ett korrekt värde:</span><span class="sxs-lookup"><span data-stu-id="17463-147">To create a trusted certificate authority, use the [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet and set the **crlDistributionPoint** attribute to a correct value:</span></span> 
   
    $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]" 
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
    $new_ca.AuthorityType=0 
    $new_ca.TrustedCertificate=$cert 
    $new_ca.crlDistributionPoint=”<CRL Distribution URL>”
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 


### <a name="remove"></a><span data-ttu-id="17463-148">Ta bort</span><span class="sxs-lookup"><span data-stu-id="17463-148">Remove</span></span>

<span data-ttu-id="17463-149">Ta bort en betrodd certifikatutfärdare med den [ta bort AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="17463-149">To remove a trusted certificate authority, use the [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span></span>
   
    $c=Get-AzureADTrustedCertificateAuthority 
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiy"></a><span data-ttu-id="17463-150">Modfiy</span><span class="sxs-lookup"><span data-stu-id="17463-150">Modfiy</span></span>

<span data-ttu-id="17463-151">Om du vill ändra en betrodd certifikatutfärdare, använder den [Set AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="17463-151">To modify a trusted certificate authority, use the [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span></span>

    $c=Get-AzureADTrustedCertificateAuthority 
    $c[0].AuthorityType=1 
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 


## <a name="step-3-configure-revocation"></a><span data-ttu-id="17463-152">Steg 3: Konfigurera återkallade certifikat</span><span class="sxs-lookup"><span data-stu-id="17463-152">Step 3: Configure revocation</span></span>

<span data-ttu-id="17463-153">Om du vill återkalla ett certifikat för Azure Active Directory hämtar listan över återkallade certifikat (CRL) från URL: er som överförs som en del av information om certifikatutfärdare och cachelagrar den.</span><span class="sxs-lookup"><span data-stu-id="17463-153">To revoke a client certificate, Azure Active Directory fetches the certificate revocation list (CRL) from the URLs uploaded as part of certificate authority information and caches it.</span></span> <span data-ttu-id="17463-154">Senaste publicera tidsstämpel (**giltighetsdatum** egenskapen) i listan över återkallade certifikat för att kontrollera listan över återkallade certifikat är fortfarande giltig.</span><span class="sxs-lookup"><span data-stu-id="17463-154">The last publish timestamp (**Effective Date** property) in the CRL is used to ensure the CRL is still valid.</span></span> <span data-ttu-id="17463-155">Listan över återkallade certifikat refereras med jämna mellanrum för att återkalla åtkomst till certifikat som ingår i listan.</span><span class="sxs-lookup"><span data-stu-id="17463-155">The CRL is periodically referenced to revoke access to certificates that are a part of the list.</span></span>

<span data-ttu-id="17463-156">Om det krävs mer instant återkallas (till exempel om en användare förlorar en enhet), kan vara ogiltig Autentiseringstoken för användaren.</span><span class="sxs-lookup"><span data-stu-id="17463-156">If a more instant revocation is required (for example, if a user loses a device), the authorization token of the user can be invalidated.</span></span> <span data-ttu-id="17463-157">För att ogiltigförklara autentiseringstoken, ange den **StsRefreshTokenValidFrom** för den aktuella användaren med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="17463-157">To invalidate the authorization token, set the **StsRefreshTokenValidFrom** field for this particular user using Windows PowerShell.</span></span> <span data-ttu-id="17463-158">Du måste uppdatera de **StsRefreshTokenValidFrom** för varje användare som du vill återkalla åtkomst för.</span><span class="sxs-lookup"><span data-stu-id="17463-158">You must update the **StsRefreshTokenValidFrom** field for each user you want to revoke access for.</span></span>

<span data-ttu-id="17463-159">Att säkerställa att återkallningen kvarstår, måste du ange den **giltighetsdatum** av listan över återkallade certifikat till ett datum efter värdet som angetts av **StsRefreshTokenValidFrom** och se till att det aktuella certifikatet finns i listan över återkallade certifikat.</span><span class="sxs-lookup"><span data-stu-id="17463-159">To ensure that the revocation persists, you must set the **Effective Date** of the CRL to a date after the value set by **StsRefreshTokenValidFrom** and ensure the certificate in question is in the CRL.</span></span>

<span data-ttu-id="17463-160">Följande steg beskriver processen för att uppdatera och ogiltigförklara autentiseringstoken genom att ange den **StsRefreshTokenValidFrom** fältet.</span><span class="sxs-lookup"><span data-stu-id="17463-160">The following steps outline the process for updating and invalidating the authorization token by setting the **StsRefreshTokenValidFrom** field.</span></span> 

<span data-ttu-id="17463-161">**Så här konfigurerar du återkallade certifikat:**</span><span class="sxs-lookup"><span data-stu-id="17463-161">**To configure revocation:**</span></span> 

1. <span data-ttu-id="17463-162">Anslut med administratörsautentiseringsuppgifter för tjänsten MSOL:</span><span class="sxs-lookup"><span data-stu-id="17463-162">Connect with admin credentials to the MSOL service:</span></span> 
   
        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

2. <span data-ttu-id="17463-163">Hämta det aktuella StsRefreshTokensValidFrom-värdet för en användare:</span><span class="sxs-lookup"><span data-stu-id="17463-163">Retrieve the current StsRefreshTokensValidFrom value for a user:</span></span> 
   
        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 

3. <span data-ttu-id="17463-164">Konfigurera ett nytt StsRefreshTokensValidFrom värde för den användare som är lika med aktuell tidsstämpel:</span><span class="sxs-lookup"><span data-stu-id="17463-164">Configure a new StsRefreshTokensValidFrom value for the user equal to the current timestamp:</span></span> 
   
        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

<span data-ttu-id="17463-165">Det datum som du anger måste vara i framtiden.</span><span class="sxs-lookup"><span data-stu-id="17463-165">The date you set must be in the future.</span></span> <span data-ttu-id="17463-166">Om datumet inte är i framtiden kan den **StsRefreshTokensValidFrom** egenskapen har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="17463-166">If the date is not in the future, the **StsRefreshTokensValidFrom** property is not set.</span></span> <span data-ttu-id="17463-167">Om datumet är i framtiden, **StsRefreshTokensValidFrom** anges till den aktuella tiden (inte det datum som anges av kommandot Set-MsolUser).</span><span class="sxs-lookup"><span data-stu-id="17463-167">If the date is in the future, **StsRefreshTokensValidFrom** is set to the current time (not the date indicated by Set-MsolUser command).</span></span> 


## <a name="step-4-test-your-configuration"></a><span data-ttu-id="17463-168">Steg 4: Testa din konfiguration</span><span class="sxs-lookup"><span data-stu-id="17463-168">Step 4: Test your configuration</span></span>

### <a name="testing-your-certificate"></a><span data-ttu-id="17463-169">Testa ditt certifikat</span><span class="sxs-lookup"><span data-stu-id="17463-169">Testing your certificate</span></span>

<span data-ttu-id="17463-170">Som ett första konfigurationstest bör du försöker logga in på [Outlook Web Access](https://outlook.office365.com) eller [SharePoint Online](https://microsoft.sharepoint.com) med din **på enhetens webbläsare**.</span><span class="sxs-lookup"><span data-stu-id="17463-170">As a first configuration test, you should try to sign in to [Outlook Web Access](https://outlook.office365.com) or [SharePoint Online](https://microsoft.sharepoint.com) using your **on-device browser**.</span></span>

<span data-ttu-id="17463-171">Om din inloggning lyckades, vet som:</span><span class="sxs-lookup"><span data-stu-id="17463-171">If your sign-in is successful, then you know that:</span></span>

- <span data-ttu-id="17463-172">Användarcertifikatet har etablerats till testenhet</span><span class="sxs-lookup"><span data-stu-id="17463-172">The user certificate has been provisioned to your test device</span></span>
- <span data-ttu-id="17463-173">AD FS är korrekt konfigurerad</span><span class="sxs-lookup"><span data-stu-id="17463-173">AD FS is configured correctly</span></span>  


### <a name="testing-office-mobile-applications"></a><span data-ttu-id="17463-174">Testa mobila Office-program</span><span class="sxs-lookup"><span data-stu-id="17463-174">Testing Office mobile applications</span></span>

<span data-ttu-id="17463-175">**Så här testar certifikatbaserad autentisering på din mobila Office-program:**</span><span class="sxs-lookup"><span data-stu-id="17463-175">**To test certificate-based authentication on your mobile Office application:**</span></span> 

1. <span data-ttu-id="17463-176">Installera ett mobila Office-program (t.ex. OneDrive) på enheten test.</span><span class="sxs-lookup"><span data-stu-id="17463-176">On your test device, install an Office mobile application (e.g., OneDrive).</span></span>
3. <span data-ttu-id="17463-177">Starta programmet.</span><span class="sxs-lookup"><span data-stu-id="17463-177">Launch the application.</span></span> 
4. <span data-ttu-id="17463-178">Ange ditt användarnamn och välj sedan användarcertifikatet för som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="17463-178">Enter your user name, and then select the user certificate you want to use.</span></span> 

<span data-ttu-id="17463-179">Du bör vara loggat in.</span><span class="sxs-lookup"><span data-stu-id="17463-179">You should be successfully signed in.</span></span> 

### <a name="testing-exchange-activesync-client-applications"></a><span data-ttu-id="17463-180">Testa program för Exchange ActiveSync-klienten</span><span class="sxs-lookup"><span data-stu-id="17463-180">Testing Exchange ActiveSync client applications</span></span>

<span data-ttu-id="17463-181">För att komma åt Exchange ActiveSync (EAS) via certifikatbaserad autentisering måste det finnas en EAS-profil som innehåller klientcertifikatet till programmet.</span><span class="sxs-lookup"><span data-stu-id="17463-181">To access Exchange ActiveSync (EAS) via certificate-based authentication, an EAS profile containing the client certificate must be available to the application.</span></span> 

<span data-ttu-id="17463-182">EAS-profil måste innehålla följande information:</span><span class="sxs-lookup"><span data-stu-id="17463-182">The EAS profile must contain the following information:</span></span>

- <span data-ttu-id="17463-183">Användarcertifikatet som ska användas för autentisering</span><span class="sxs-lookup"><span data-stu-id="17463-183">The user certificate to be used for authentication</span></span> 

- <span data-ttu-id="17463-184">EAS-slutpunkt (till exempel outlook.office365.com)</span><span class="sxs-lookup"><span data-stu-id="17463-184">The EAS endpoint (for example, outlook.office365.com)</span></span>

<span data-ttu-id="17463-185">EAS-profil kan konfigureras och placeras på enheten via användning av hantering av mobila enheter (MDM), till exempel Intune eller genom att manuellt certifikatet EAS-profil på enheten.</span><span class="sxs-lookup"><span data-stu-id="17463-185">An EAS profile can be configured and placed on the device through the utilization of Mobile device management (MDM) such as Intune or by manually placing the certificate in the EAS profile on the device.</span></span>  

### <a name="testing-eas-client-applications-on-android"></a><span data-ttu-id="17463-186">Testa EAS klientprogram på Android</span><span class="sxs-lookup"><span data-stu-id="17463-186">Testing EAS client applications on Android</span></span>

<span data-ttu-id="17463-187">**Att testa autentisering med datorcertifikat:**</span><span class="sxs-lookup"><span data-stu-id="17463-187">**To test certificate authentication:**</span></span>  

1. <span data-ttu-id="17463-188">Konfigurera en EAS-profil i programmet som uppfyller kraven ovan.</span><span class="sxs-lookup"><span data-stu-id="17463-188">Configure an EAS profile in the application that satisfies the requirements above.</span></span>  
2. <span data-ttu-id="17463-189">Öppna programmet och kontrollera att du synkroniserar e-post.</span><span class="sxs-lookup"><span data-stu-id="17463-189">Open the application, and verify that mail is synchronizing.</span></span> 

