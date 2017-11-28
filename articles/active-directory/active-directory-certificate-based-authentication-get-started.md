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
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a><span data-ttu-id="ea1a6-103">Komma igång med certifikatbaserad autentisering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ea1a6-103">Get started with certificate-based authentication in Azure Active Directory</span></span>

<span data-ttu-id="ea1a6-104">Certifikatbaserad autentisering kan du toobe autentiseras av Azure Active Directory med ett klientcertifikat på en Windows-, Android eller iOS-enhet när du ansluter din Exchange-onlinekonto till:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-104">Certificate-based authentication enables you toobe authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

- <span data-ttu-id="ea1a6-105">Mobila Office-program, till exempel Microsoft Outlook och Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="ea1a6-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   

- <span data-ttu-id="ea1a6-106">Exchange ActiveSync (EAS) klienter</span><span class="sxs-lookup"><span data-stu-id="ea1a6-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="ea1a6-107">Konfigurera den här funktionen eliminerar hello måste tooenter användarnamnet och lösenordet till vissa e-post och Microsoft Office-program på din mobila enhet.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-107">Configuring this feature eliminates hello need tooenter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="ea1a6-108">Det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-108">This topic:</span></span>

- <span data-ttu-id="ea1a6-109">Ger du med hello steg tooconfigure använda certifikatbaserad autentisering för användare av klienter i Office 365 Enterprise-, Business-, Education och USA: S regering planer.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-109">Provides you with hello steps tooconfigure and utilize certificate-based authentication for users of tenants in Office 365 Enterprise, Business, Education, and US Government plans.</span></span> <span data-ttu-id="ea1a6-110">Den här funktionen är tillgängliga i förhandsversionen i Office 365 Kina US Government skydd och US Government Federal planer.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-110">This feature is available in preview in Office 365 China, US Government Defense, and US Government Federal plans.</span></span> 

- <span data-ttu-id="ea1a6-111">Förutsätter att du redan har en [infrastruktur för offentliga nycklar (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) och [AD FS](connect/active-directory-aadconnectfed-whatis.md) konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-111">Assumes that you already have a [public key infrastructure (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) and [AD FS](connect/active-directory-aadconnectfed-whatis.md) configured.</span></span>    


## <a name="requirements"></a><span data-ttu-id="ea1a6-112">Krav</span><span class="sxs-lookup"><span data-stu-id="ea1a6-112">Requirements</span></span>

<span data-ttu-id="ea1a6-113">tooconfigure certifikatbaserad autentisering hello måste följande vara sant:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-113">tooconfigure certificate-based authentication, hello following must be true:</span></span>  

- <span data-ttu-id="ea1a6-114">Certifikatbaserad autentisering (CBA) stöds endast för federerat miljöer för webbläsarprogram eller interna klienter som använder modern autentisering (ADAL).</span><span class="sxs-lookup"><span data-stu-id="ea1a6-114">Certificate-based authentication (CBA) is only supported for Federated environments for browser applications or native clients using modern authentication (ADAL).</span></span> <span data-ttu-id="ea1a6-115">hello ett undantag är Exchange Active Sync (EAS) för EXO som kan användas för både federerade och hanterade konton.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-115">hello one exception is Exchange Active Sync (EAS) for EXO which can be used for both, federated and managed accounts.</span></span> 

- <span data-ttu-id="ea1a6-116">hello rotcertifikatutfärdare och alla mellanliggande certifikatutfärdare måste konfigureras i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-116">hello root certificate authority and any intermediate certificate authorities must be configured in Azure Active Directory.</span></span>  

- <span data-ttu-id="ea1a6-117">Varje certifikatutfärdare måste ha en lista certifikat (CRL) som kan refereras via en Internetuppkopplad URL.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-117">Each certificate authority must have a certificate revocation list (CRL) that can be referenced via an Internet facing URL.</span></span>  

- <span data-ttu-id="ea1a6-118">Du måste ha minst en certifikatutfärdare som konfigurerats i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-118">You must have at least one certificate authority configured in Azure Active Directory.</span></span> <span data-ttu-id="ea1a6-119">Du kan hitta relaterade steg i hello [konfigurera hello certifikatutfärdare](#step-2-configure-the-certificate-authorities) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-119">You can find related steps in hello [Configure hello certificate authorities](#step-2-configure-the-certificate-authorities) section.</span></span>  

- <span data-ttu-id="ea1a6-120">För Exchange ActiveSync-klienter har hello klientcertifikatet hello användarens dirigerbara e-post-adressen i Exchange online i antingen hello huvudnamn eller hello RFC822 namn-värde i fältet för hello Alternativt ämnesnamn.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-120">For Exchange ActiveSync clients, hello client certificate must have hello user’s routable email address in Exchange online in either hello Principal Name or hello RFC822 Name value of hello Subject Alternative Name field.</span></span> <span data-ttu-id="ea1a6-121">Azure Active Directory mappar hello RFC822 värdet toohello proxyadress attribut i hello katalog.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-121">Azure Active Directory maps hello RFC822 value toohello Proxy Address attribute in hello directory.</span></span>  

- <span data-ttu-id="ea1a6-122">Klientenheten måste ha åtkomst tooat minst en certifikatutfärdare som utfärdar certifikat.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-122">Your client device must have access tooat least one certificate authority that issues client certificates.</span></span>  

- <span data-ttu-id="ea1a6-123">Ett klientcertifikat för klientautentisering måste ha utfärdats tooyour klienten.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-123">A client certificate for client authentication must have been issued tooyour client.</span></span>  




## <a name="step-1-select-your-device-platform"></a><span data-ttu-id="ea1a6-124">Steg 1: Välj din enhetsplattform</span><span class="sxs-lookup"><span data-stu-id="ea1a6-124">Step 1: Select your device platform</span></span>

<span data-ttu-id="ea1a6-125">Som ett första steg behöver för hello enhetsplattform som du är intresserad, du tooreview hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-125">As a first step, for hello device platform you care about, you need tooreview hello following:</span></span>

- <span data-ttu-id="ea1a6-126">stöd för mobila hello Office-program</span><span class="sxs-lookup"><span data-stu-id="ea1a6-126">hello Office mobile applications support</span></span> 
- <span data-ttu-id="ea1a6-127">hello specifik implementeringskrav</span><span class="sxs-lookup"><span data-stu-id="ea1a6-127">hello specific implementation requirements</span></span>  

<span data-ttu-id="ea1a6-128">hello relaterad information finns för hello följande enhetsplattformar:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-128">hello related information exists for hello following device platforms:</span></span>

- [<span data-ttu-id="ea1a6-129">Android</span><span class="sxs-lookup"><span data-stu-id="ea1a6-129">Android</span></span>](active-directory-certificate-based-authentication-android.md)
- [<span data-ttu-id="ea1a6-130">iOS</span><span class="sxs-lookup"><span data-stu-id="ea1a6-130">iOS</span></span>](active-directory-certificate-based-authentication-ios.md)


## <a name="step-2-configure-hello-certificate-authorities"></a><span data-ttu-id="ea1a6-131">Steg 2: Konfigurera hello certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="ea1a6-131">Step 2: Configure hello certificate authorities</span></span> 

<span data-ttu-id="ea1a6-132">tooconfigure överför din certifikatutfärdare i Azure Active Directory, för varje certifikatutfärdare hello följande:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-132">tooconfigure your certificate authorities in Azure Active Directory, for each certificate authority, upload hello following:</span></span> 

* <span data-ttu-id="ea1a6-133">Hej offentliga delen av hello certifikat i *.cer* format</span><span class="sxs-lookup"><span data-stu-id="ea1a6-133">hello public portion of hello certificate, in *.cer* format</span></span> 
* <span data-ttu-id="ea1a6-134">hello finns mot URL: er där hello Internet listor över återkallade certifikat (CRL)</span><span class="sxs-lookup"><span data-stu-id="ea1a6-134">hello Internet facing URLs where hello Certificate Revocation Lists (CRLs) reside</span></span>

<span data-ttu-id="ea1a6-135">hello-schemat för en certifikatutfärdare som ser ut som följer:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-135">hello schema for a certificate authority looks as follows:</span></span> 

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

<span data-ttu-id="ea1a6-136">Du kan använda hello hello konfiguration, [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0):</span><span class="sxs-lookup"><span data-stu-id="ea1a6-136">For hello configuration, you can use hello [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0):</span></span>  

1. <span data-ttu-id="ea1a6-137">Starta Windows PowerShell med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-137">Start Windows PowerShell with administrator privileges.</span></span> 
2. <span data-ttu-id="ea1a6-138">Installera hello Azure AD-modulen.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-138">Install hello Azure AD module.</span></span> <span data-ttu-id="ea1a6-139">Du behöver tooinstall Version [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) eller högre.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-139">You need tooinstall Version [2.0.0.33 ](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) or higher.</span></span>  
   
        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33 

<span data-ttu-id="ea1a6-140">Som ett första konfigurationssteg måste du tooestablish en anslutning med din klient.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-140">As a first configuration step, you need tooestablish a connection with your tenant.</span></span> <span data-ttu-id="ea1a6-141">Så snart det finns en anslutning tooyour klient, kan du granska, lägga till, ta bort och ändra hello betrodda certifikatutfärdare som har definierats i din katalog.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-141">As soon as a connection tooyour tenant exists, you can review, add, delete and modify hello trusted certificate authorities that are defined in your directory.</span></span> 

### <a name="connect"></a><span data-ttu-id="ea1a6-142">Anslut</span><span class="sxs-lookup"><span data-stu-id="ea1a6-142">Connect</span></span>

<span data-ttu-id="ea1a6-143">tooestablish en anslutning till din klient, Använd hello [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-143">tooestablish a connection with your tenant, use hello [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0) cmdlet:</span></span>

    Connect-AzureAD 


### <a name="retrieve"></a><span data-ttu-id="ea1a6-144">Hämta</span><span class="sxs-lookup"><span data-stu-id="ea1a6-144">Retrieve</span></span> 

<span data-ttu-id="ea1a6-145">tooretrieve hello betrodda certifikatutfärdare som har definierats i din katalog använder hello [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-145">tooretrieve hello trusted certificate authorities that are defined in your directory, use hello [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet.</span></span> 

    Get-AzureADTrustedCertificateAuthority 
 

### <a name="add"></a><span data-ttu-id="ea1a6-146">Lägg till</span><span class="sxs-lookup"><span data-stu-id="ea1a6-146">Add</span></span>

<span data-ttu-id="ea1a6-147">toocreate en betrodd certifikatutfärdare använder hello [ny AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet och ange hello **crlDistributionPoint** attributet tooa rätt värde:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-147">toocreate a trusted certificate authority, use hello [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet and set hello **crlDistributionPoint** attribute tooa correct value:</span></span> 
   
    $cert=Get-Content -Encoding byte "[LOCATION OF hello CER FILE]" 
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation 
    $new_ca.AuthorityType=0 
    $new_ca.TrustedCertificate=$cert 
    $new_ca.crlDistributionPoint=”<CRL Distribution URL>”
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca 


### <a name="remove"></a><span data-ttu-id="ea1a6-148">Ta bort</span><span class="sxs-lookup"><span data-stu-id="ea1a6-148">Remove</span></span>

<span data-ttu-id="ea1a6-149">tooremove en betrodd certifikatutfärdare använder hello [ta bort AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-149">tooremove a trusted certificate authority, use hello [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span></span>
   
    $c=Get-AzureADTrustedCertificateAuthority 
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2] 


### <a name="modfiy"></a><span data-ttu-id="ea1a6-150">Modfiy</span><span class="sxs-lookup"><span data-stu-id="ea1a6-150">Modfiy</span></span>

<span data-ttu-id="ea1a6-151">toomodify en betrodd certifikatutfärdare använder hello [Set AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-151">toomodify a trusted certificate authority, use hello [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0) cmdlet:</span></span>

    $c=Get-AzureADTrustedCertificateAuthority 
    $c[0].AuthorityType=1 
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0] 


## <a name="step-3-configure-revocation"></a><span data-ttu-id="ea1a6-152">Steg 3: Konfigurera återkallade certifikat</span><span class="sxs-lookup"><span data-stu-id="ea1a6-152">Step 3: Configure revocation</span></span>

<span data-ttu-id="ea1a6-153">toorevoke ett klientcertifikat, Azure Active Directory hämtar listan över återkallade certifikat (CRL) från URL: er hello överförs som en del av information om certifikatutfärdare och cachelagrar hello-certifikat.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-153">toorevoke a client certificate, Azure Active Directory fetches hello certificate revocation list (CRL) from hello URLs uploaded as part of certificate authority information and caches it.</span></span> <span data-ttu-id="ea1a6-154">hello publicera senaste tidsstämpel (**giltighetsdatum** egenskapen) i hello CRL används tooensure hello CRL fortfarande är giltigt.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-154">hello last publish timestamp (**Effective Date** property) in hello CRL is used tooensure hello CRL is still valid.</span></span> <span data-ttu-id="ea1a6-155">hello CRL är regelbundet refererade toorevoke åtkomst toocertificates som är en del av hello-listan.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-155">hello CRL is periodically referenced toorevoke access toocertificates that are a part of hello list.</span></span>

<span data-ttu-id="ea1a6-156">Om det krävs mer instant återkallas (till exempel om en användare förlorar en enhet), kan hello Autentiseringstoken för hello användare vara ogiltig.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-156">If a more instant revocation is required (for example, if a user loses a device), hello authorization token of hello user can be invalidated.</span></span> <span data-ttu-id="ea1a6-157">tooinvalidate hello auktorisering token, Ställ in hello **StsRefreshTokenValidFrom** för den aktuella användaren med Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-157">tooinvalidate hello authorization token, set hello **StsRefreshTokenValidFrom** field for this particular user using Windows PowerShell.</span></span> <span data-ttu-id="ea1a6-158">Du måste uppdatera hello **StsRefreshTokenValidFrom** för varje användare som du vill toorevoke åtkomst för.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-158">You must update hello **StsRefreshTokenValidFrom** field for each user you want toorevoke access for.</span></span>

<span data-ttu-id="ea1a6-159">tooensure som kvarstår hello återkallade certifikat, måste du ange hello **giltighetsdatum** av hello CRL tooa datum efter hello-värde som anges av **StsRefreshTokenValidFrom** och kontrollera hello certifikatet i fråga finns i hello CRL.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-159">tooensure that hello revocation persists, you must set hello **Effective Date** of hello CRL tooa date after hello value set by **StsRefreshTokenValidFrom** and ensure hello certificate in question is in hello CRL.</span></span>

<span data-ttu-id="ea1a6-160">Hej följande steg disposition hello processen för att uppdatera och ogiltigförklara hello autentiseringstoken genom att ange hello **StsRefreshTokenValidFrom** fältet.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-160">hello following steps outline hello process for updating and invalidating hello authorization token by setting hello **StsRefreshTokenValidFrom** field.</span></span> 

<span data-ttu-id="ea1a6-161">**tooconfigure återkallade certifikat:**</span><span class="sxs-lookup"><span data-stu-id="ea1a6-161">**tooconfigure revocation:**</span></span> 

1. <span data-ttu-id="ea1a6-162">Anslut med autentiseringsuppgifterna toohello MSOL administrationstjänsten:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-162">Connect with admin credentials toohello MSOL service:</span></span> 
   
        $msolcred = get-credential 
        connect-msolservice -credential $msolcred 

2. <span data-ttu-id="ea1a6-163">Hämta hello aktuella StsRefreshTokensValidFrom värdet för en användare:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-163">Retrieve hello current StsRefreshTokensValidFrom value for a user:</span></span> 
   
        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com` 
        $user.StsRefreshTokensValidFrom 

3. <span data-ttu-id="ea1a6-164">Konfigurera ett nytt StsRefreshTokensValidFrom värde för hello användaren lika toohello aktuell tidsstämpel:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-164">Configure a new StsRefreshTokensValidFrom value for hello user equal toohello current timestamp:</span></span> 
   
        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

<span data-ttu-id="ea1a6-165">hello datum som du anger måste finnas i framtida hello.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-165">hello date you set must be in hello future.</span></span> <span data-ttu-id="ea1a6-166">Om hello datumet inte är i hello framtida hello **StsRefreshTokensValidFrom** egenskapen har inte angetts.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-166">If hello date is not in hello future, hello **StsRefreshTokensValidFrom** property is not set.</span></span> <span data-ttu-id="ea1a6-167">Om hello datum i hello framtida **StsRefreshTokensValidFrom** anges toohello aktuell tid (inte hello datum som anges av kommandot Set-MsolUser).</span><span class="sxs-lookup"><span data-stu-id="ea1a6-167">If hello date is in hello future, **StsRefreshTokensValidFrom** is set toohello current time (not hello date indicated by Set-MsolUser command).</span></span> 


## <a name="step-4-test-your-configuration"></a><span data-ttu-id="ea1a6-168">Steg 4: Testa din konfiguration</span><span class="sxs-lookup"><span data-stu-id="ea1a6-168">Step 4: Test your configuration</span></span>

### <a name="testing-your-certificate"></a><span data-ttu-id="ea1a6-169">Testa ditt certifikat</span><span class="sxs-lookup"><span data-stu-id="ea1a6-169">Testing your certificate</span></span>

<span data-ttu-id="ea1a6-170">Som ett första konfigurationstest bör du försöka toosign i för[Outlook Web Access](https://outlook.office365.com) eller [SharePoint Online](https://microsoft.sharepoint.com) med din **på enhetens webbläsare**.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-170">As a first configuration test, you should try toosign in too[Outlook Web Access](https://outlook.office365.com) or [SharePoint Online](https://microsoft.sharepoint.com) using your **on-device browser**.</span></span>

<span data-ttu-id="ea1a6-171">Om din inloggning lyckades, vet som:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-171">If your sign-in is successful, then you know that:</span></span>

- <span data-ttu-id="ea1a6-172">hello användarcertifikat har etablerat tooyour testenhet</span><span class="sxs-lookup"><span data-stu-id="ea1a6-172">hello user certificate has been provisioned tooyour test device</span></span>
- <span data-ttu-id="ea1a6-173">AD FS är korrekt konfigurerad</span><span class="sxs-lookup"><span data-stu-id="ea1a6-173">AD FS is configured correctly</span></span>  


### <a name="testing-office-mobile-applications"></a><span data-ttu-id="ea1a6-174">Testa mobila Office-program</span><span class="sxs-lookup"><span data-stu-id="ea1a6-174">Testing Office mobile applications</span></span>

<span data-ttu-id="ea1a6-175">**tootest certifikatbaserad autentisering på din mobila Office-program:**</span><span class="sxs-lookup"><span data-stu-id="ea1a6-175">**tootest certificate-based authentication on your mobile Office application:**</span></span> 

1. <span data-ttu-id="ea1a6-176">Installera ett mobila Office-program (t.ex. OneDrive) på enheten test.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-176">On your test device, install an Office mobile application (e.g., OneDrive).</span></span>
3. <span data-ttu-id="ea1a6-177">Starta hello program.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-177">Launch hello application.</span></span> 
4. <span data-ttu-id="ea1a6-178">Ange ditt användarnamn och välj sedan hello användarcertifikat som du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-178">Enter your user name, and then select hello user certificate you want toouse.</span></span> 

<span data-ttu-id="ea1a6-179">Du bör vara loggat in.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-179">You should be successfully signed in.</span></span> 

### <a name="testing-exchange-activesync-client-applications"></a><span data-ttu-id="ea1a6-180">Testa program för Exchange ActiveSync-klienten</span><span class="sxs-lookup"><span data-stu-id="ea1a6-180">Testing Exchange ActiveSync client applications</span></span>

<span data-ttu-id="ea1a6-181">tooaccess Exchange ActiveSync (EAS) via en EAS-profil som innehåller hello klientcertifikat certifikatbaserad autentisering måste vara tillgängliga toohello program.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-181">tooaccess Exchange ActiveSync (EAS) via certificate-based authentication, an EAS profile containing hello client certificate must be available toohello application.</span></span> 

<span data-ttu-id="ea1a6-182">hello EAS-profil måste innehålla hello följande information:</span><span class="sxs-lookup"><span data-stu-id="ea1a6-182">hello EAS profile must contain hello following information:</span></span>

- <span data-ttu-id="ea1a6-183">hello användaren certifikat toobe används för autentisering</span><span class="sxs-lookup"><span data-stu-id="ea1a6-183">hello user certificate toobe used for authentication</span></span> 

- <span data-ttu-id="ea1a6-184">hello EAS-slutpunkt (till exempel outlook.office365.com)</span><span class="sxs-lookup"><span data-stu-id="ea1a6-184">hello EAS endpoint (for example, outlook.office365.com)</span></span>

<span data-ttu-id="ea1a6-185">EAS-profil kan konfigureras och placeras på hello enhet via hello användning av hantering av mobila enheter (MDM), till exempel Intune eller genom att manuellt hello certifikat hello EAS-profil på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-185">An EAS profile can be configured and placed on hello device through hello utilization of Mobile device management (MDM) such as Intune or by manually placing hello certificate in hello EAS profile on hello device.</span></span>  

### <a name="testing-eas-client-applications-on-android"></a><span data-ttu-id="ea1a6-186">Testa EAS klientprogram på Android</span><span class="sxs-lookup"><span data-stu-id="ea1a6-186">Testing EAS client applications on Android</span></span>

<span data-ttu-id="ea1a6-187">**tootest autentisering med datorcertifikat:**</span><span class="sxs-lookup"><span data-stu-id="ea1a6-187">**tootest certificate authentication:**</span></span>  

1. <span data-ttu-id="ea1a6-188">Konfigurera en EAS-profil i hello-programmet som uppfyller hello kraven ovan.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-188">Configure an EAS profile in hello application that satisfies hello requirements above.</span></span>  
2. <span data-ttu-id="ea1a6-189">Öppna hello program och kontrollera att du synkroniserar e-post.</span><span class="sxs-lookup"><span data-stu-id="ea1a6-189">Open hello application, and verify that mail is synchronizing.</span></span> 

