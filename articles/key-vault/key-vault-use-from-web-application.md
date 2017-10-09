---
title: "aaaUse Azure Key Vault från ett webbprogram | Microsoft Docs"
description: "Använd den här självstudiekursen toohelp du lära dig hur toouse Azure nyckeln valvet från ett webbprogram."
services: key-vault
documentationcenter: 
author: adhurwit
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 9b7d065e-1979-4397-8298-eeba3aec4792
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: adhurwit
ms.openlocfilehash: d5e2299e60b379c4e234d5cd6be03411c5a5c958
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-key-vault-from-a-web-application"></a><span data-ttu-id="a78ac-103">Använda Azure Key Vault från webbprogram</span><span class="sxs-lookup"><span data-stu-id="a78ac-103">Use Azure Key Vault from a Web Application</span></span>
## <a name="introduction"></a><span data-ttu-id="a78ac-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="a78ac-104">Introduction</span></span>
<span data-ttu-id="a78ac-105">Använd den här självstudiekursen toohelp du lära dig hur toouse Azure nyckeln valvet från ett webbprogram i Azure.</span><span class="sxs-lookup"><span data-stu-id="a78ac-105">Use this tutorial toohelp you learn how toouse Azure Key Vault from a web application in Azure.</span></span> <span data-ttu-id="a78ac-106">Den vägleder dig genom hello processen att komma åt en hemlighet från ett Azure Key Vault så att den kan användas i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a78ac-106">It walks you through hello process of accessing a secret from an Azure Key Vault so that it can be used in your web application.</span></span>

<span data-ttu-id="a78ac-107">**Uppskattad tid toocomplete:** 15 minuter</span><span class="sxs-lookup"><span data-stu-id="a78ac-107">**Estimated time toocomplete:** 15 minutes</span></span>

<span data-ttu-id="a78ac-108">Översiktlig information om Azure Key Vault finns i [Vad är Azure Key Vault?](key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="a78ac-108">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a78ac-109">Krav</span><span class="sxs-lookup"><span data-stu-id="a78ac-109">Prerequisites</span></span>
<span data-ttu-id="a78ac-110">toocomplete den här självstudien måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="a78ac-110">toocomplete this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="a78ac-111">En URI tooa hemlighet i en Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a78ac-111">A URI tooa secret in an Azure Key Vault</span></span>
* <span data-ttu-id="a78ac-112">En klient-ID och en Klienthemlighet för ett webbprogram som har registrerats med Azure Active Directory som har åtkomst tooyour Key Vault</span><span class="sxs-lookup"><span data-stu-id="a78ac-112">A Client ID and a Client Secret for a web application registered with Azure Active Directory that has access tooyour Key Vault</span></span>
* <span data-ttu-id="a78ac-113">Ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a78ac-113">A web application.</span></span> <span data-ttu-id="a78ac-114">Vi ska visa hello steg för ett ASP.NET MVC-program som distribueras i Azure som en Webbapp.</span><span class="sxs-lookup"><span data-stu-id="a78ac-114">We will be showing hello steps for an ASP.NET MVC application deployed in Azure as a Web App.</span></span>

> [!NOTE]
> <span data-ttu-id="a78ac-115">Det är viktigt att du har slutfört stegen i hello [Kom igång med Azure Key Vault](key-vault-get-started.md) för den här självstudiekursen så att du har hello URI tooa hemlighet och hello klient-ID och Klienthemlighet för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a78ac-115">It is essential that you have completed hello steps listed in [Get Started with Azure Key Vault](key-vault-get-started.md) for this tutorial so that you have hello URI tooa secret and hello Client ID and Client Secret for a web application.</span></span>
> 
> 

<span data-ttu-id="a78ac-116">hello webbprogram som kommer åt hello Key Vault är hello något som är registrerad i Azure Active Directory och har fått åtkomst tooyour Key Vault.</span><span class="sxs-lookup"><span data-stu-id="a78ac-116">hello web application that will be accessing hello Key Vault is hello one that is registered in Azure Active Directory and has been given access tooyour Key Vault.</span></span> <span data-ttu-id="a78ac-117">Om detta inte är fallet hello gå tillbaka tooRegister ett program i hello komma igång-kursen och upprepa hello stegen.</span><span class="sxs-lookup"><span data-stu-id="a78ac-117">If this is not hello case, go back tooRegister an Application in hello Get Started tutorial and repeat hello steps listed.</span></span>

<span data-ttu-id="a78ac-118">Den här kursen är avsedd för webbutvecklare som förstår hello grunderna för att skapa webbprogram på Azure.</span><span class="sxs-lookup"><span data-stu-id="a78ac-118">This tutorial is designed for web developers that understand hello basics of creating web applications on Azure.</span></span> <span data-ttu-id="a78ac-119">Läs mer om Azure Web Apps [översikt över Web Apps](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a78ac-119">For more information about Azure Web Apps, see [Web Apps overview](../app-service-web/app-service-web-overview.md).</span></span>

## <span data-ttu-id="a78ac-120"><a id="packages"></a>Lägg till Nuget-paket</span><span class="sxs-lookup"><span data-stu-id="a78ac-120"><a id="packages"></a>Add Nuget Packages</span></span>
<span data-ttu-id="a78ac-121">Det finns två paket som ditt webbprogram måste toohave installerad.</span><span class="sxs-lookup"><span data-stu-id="a78ac-121">There are two packages that your web application needs toohave installed.</span></span>

* <span data-ttu-id="a78ac-122">Active Directory Authentication Library - innehåller metoder för att interagera med Azure Active Directory och hantera användaridentitet</span><span class="sxs-lookup"><span data-stu-id="a78ac-122">Active Directory Authentication Library - contains methods for interacting with Azure Active Directory and managing user identity</span></span>
* <span data-ttu-id="a78ac-123">Azure Key Vault Library - innehåller metoder för att interagera med Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a78ac-123">Azure Key Vault Library - contains methods for interacting with Azure Key Vault</span></span>

<span data-ttu-id="a78ac-124">Båda dessa paket kan installeras med hello kommandot hello Install-Package Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="a78ac-124">Both of these packages can be installed using hello Package Manager Console using hello Install-Package command.</span></span>

    // this is currently hello latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <span data-ttu-id="a78ac-125"><a id="webconfig"></a>Ändra Web.Config</span><span class="sxs-lookup"><span data-stu-id="a78ac-125"><a id="webconfig"></a>Modify Web.Config</span></span>
<span data-ttu-id="a78ac-126">Det finns tre inställningar för program som behöver toobe tillagda toohello web.config-filen på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="a78ac-126">There are three application settings that need toobe added toohello web.config file as follows.</span></span>

    <!-- ClientId and ClientSecret refer toohello web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is hello URI for hello secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


<span data-ttu-id="a78ac-127">Om du inte kommer toohost ditt program som en Azure Web App, bör du lägga till hello faktiska ClientId, Klienthemligheten och hemlighet URI värden toohello web.config. Annars lämnar du värdena dummy eftersom vi kommer att lägga till hello faktiska värden i hello Azure-portalen för en extra nivå av säkerhet.</span><span class="sxs-lookup"><span data-stu-id="a78ac-127">If you are not going toohost your application as an Azure Web App, then you should add hello actual ClientId, Client Secret, and Secret URI values toohello web.config. Otherwise leave these dummy values because we will be adding hello actual values in hello Azure Portal for an additional level of security.</span></span>

## <span data-ttu-id="a78ac-128"><a id="gettoken"></a>Lägg till metoden tooGet en åtkomst-Token</span><span class="sxs-lookup"><span data-stu-id="a78ac-128"><a id="gettoken"></a>Add Method tooGet an Access Token</span></span>
<span data-ttu-id="a78ac-129">I ordning toouse hello Key Vault API behöver du en åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="a78ac-129">In order toouse hello Key Vault API you need an access token.</span></span> <span data-ttu-id="a78ac-130">hello Key Vault-klienten hanterar anrop toohello Key Vault-API, men du måste toosupply den med en funktion som hämtar hello åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="a78ac-130">hello Key Vault Client handles calls toohello Key Vault API but you need toosupply it with a function that gets hello access token.</span></span>  

<span data-ttu-id="a78ac-131">Följande är hello koden tooget en åtkomst-token från Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="a78ac-131">Following is hello code tooget an access token from Azure Active Directory.</span></span> <span data-ttu-id="a78ac-132">Den här koden kan gå var som helst i programmet.</span><span class="sxs-lookup"><span data-stu-id="a78ac-132">This code can go anywhere in your application.</span></span> <span data-ttu-id="a78ac-133">Jag som tooadd ett verktyg för webbplatsuppgradering eller EncryptionHelper klass.</span><span class="sxs-lookup"><span data-stu-id="a78ac-133">I like tooadd a Utils or EncryptionHelper class.</span></span>  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property toohold hello secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //hello method that will be provided toohello KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed tooobtain hello JWT token");

        return result.AccessToken;
    }

> [!NOTE]
> <span data-ttu-id="a78ac-134">Använda ett klient-ID och Klienthemlighet är hello enklaste sättet tooauthenticate ett Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="a78ac-134">Using a Client ID and Client Secret is hello easiest way tooauthenticate an Azure AD application.</span></span> <span data-ttu-id="a78ac-135">Och använda den i ditt webbprogram för en uppdelning av uppgifter och mer kontroll över din nyckelhantering.</span><span class="sxs-lookup"><span data-stu-id="a78ac-135">And using it in your web application allows for a separation of duties and more control over your key management.</span></span> <span data-ttu-id="a78ac-136">Men den förlitar sig på att placera hello Klienthemlighet i inställningarna som kan vara riskabelt som som ger hello hemlighet som du vill tooprotect i inställningarna för några.</span><span class="sxs-lookup"><span data-stu-id="a78ac-136">But it does rely on putting hello Client Secret in your configuration settings which for some can be as risky as putting hello secret that you want tooprotect in your configuration settings.</span></span> <span data-ttu-id="a78ac-137">Nedan visas en diskussion om hur toouse klient-ID och certifikat i stället för klient-ID och Klienthemlighet tooauthenticate hello Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="a78ac-137">See below for a discussion on how toouse a Client ID and Certificate instead of Client ID and Client Secret tooauthenticate hello Azure AD application.</span></span>
> 
> 

## <span data-ttu-id="a78ac-138"><a id="appstart"></a>Hämta hello hemligheten på programmet starta</span><span class="sxs-lookup"><span data-stu-id="a78ac-138"><a id="appstart"></a>Retrieve hello secret on Application Start</span></span>
<span data-ttu-id="a78ac-139">Vi behöver nu code toocall hello Key Vault-API och hämta hello hemlighet.</span><span class="sxs-lookup"><span data-stu-id="a78ac-139">Now we need code toocall hello Key Vault API and retrieve hello secret.</span></span> <span data-ttu-id="a78ac-140">hello följande kod kan placeras var som helst så länge den anropas innan du behöver toouse den.</span><span class="sxs-lookup"><span data-stu-id="a78ac-140">hello following code can be put anywhere as long as it is called before you need toouse it.</span></span> <span data-ttu-id="a78ac-141">Jag har placerat den här koden i hello programmet Starta händelsen i hello Global.asax så att den körs en gång vid start och gör hello hemlighet som är tillgängliga för hello program.</span><span class="sxs-lookup"><span data-stu-id="a78ac-141">I have put this code in hello Application Start event in hello Global.asax so that it runs once on start and makes hello secret available for hello application.</span></span>

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

    //I put a variable in a Utils class toohold hello secret for general  application use.
    Utils.EncryptSecret = sec.Value;



## <span data-ttu-id="a78ac-142"><a id="portalsettings"></a>Lägg till App-inställningar i hello Azure-portalen (valfritt)</span><span class="sxs-lookup"><span data-stu-id="a78ac-142"><a id="portalsettings"></a>Add App Settings in hello Azure Portal (optional)</span></span>
<span data-ttu-id="a78ac-143">Du kan nu lägga hello faktiska värden för hello AppSettings i hello Azure-portalen om du har en Azure-Webbapp.</span><span class="sxs-lookup"><span data-stu-id="a78ac-143">If you have an Azure Web App you can now add hello actual values for hello AppSettings in hello Azure Portal.</span></span> <span data-ttu-id="a78ac-144">Genom att göra hello faktiska värden är inte i hello web.config men skyddas via hello Portal där du har separata åtkomstfunktioner för kontrollen.</span><span class="sxs-lookup"><span data-stu-id="a78ac-144">By doing this, hello actual values will not be in hello web.config but protected via hello Portal where you have separate access control capabilities.</span></span> <span data-ttu-id="a78ac-145">Dessa värden ersätts för hello-värden som du angav i filen web.config. Kontrollera att hello namn är hello samma.</span><span class="sxs-lookup"><span data-stu-id="a78ac-145">These values will be substituted for hello values that you entered in your web.config. Make sure that hello names are hello same.</span></span>

![Programinställningar som visas i Azure Portal][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a><span data-ttu-id="a78ac-147">Autentisera med ett certifikat i stället för en Klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="a78ac-147">Authenticate with a Certificate instead of a Client Secret</span></span>
<span data-ttu-id="a78ac-148">Ett annat sätt tooauthenticate ett Azure AD-program är att använda ett klient-ID och ett certifikat i stället för en klient-ID och Klienthemlighet.</span><span class="sxs-lookup"><span data-stu-id="a78ac-148">Another way tooauthenticate an Azure AD application is by using a Client ID and a Certificate instead of a Client ID and Client Secret.</span></span> <span data-ttu-id="a78ac-149">Följande är hello steg toouse ett certifikat i en Azure Web App:</span><span class="sxs-lookup"><span data-stu-id="a78ac-149">Following are hello steps toouse a Certificate in an Azure Web App:</span></span>

1. <span data-ttu-id="a78ac-150">Hämta eller skapa ett certifikat</span><span class="sxs-lookup"><span data-stu-id="a78ac-150">Get or Create a Certificate</span></span>
2. <span data-ttu-id="a78ac-151">Associera hello certifikat med ett Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="a78ac-151">Associate hello Certificate with an Azure AD application</span></span>
3. <span data-ttu-id="a78ac-152">Lägg till kod tooyour Web App toouse hello certifikat</span><span class="sxs-lookup"><span data-stu-id="a78ac-152">Add code tooyour Web App toouse hello Certificate</span></span>
4. <span data-ttu-id="a78ac-153">Lägg till ett certifikat tooyour Web App</span><span class="sxs-lookup"><span data-stu-id="a78ac-153">Add a Certificate tooyour Web App</span></span>

<span data-ttu-id="a78ac-154">**Hämta eller skapa ett certifikat** för våra ändamål vi gör ett testcertifikat.</span><span class="sxs-lookup"><span data-stu-id="a78ac-154">**Get or Create a Certificate** For our purposes we will make a test certificate.</span></span> <span data-ttu-id="a78ac-155">Här är några av kommandon som du kan använda i en kommandotolk för utvecklare toocreate ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="a78ac-155">Here are a couple of commands that you can use in a Developer Command Prompt toocreate a certificate.</span></span> <span data-ttu-id="a78ac-156">Ändra katalogen toowhere som du vill hello cert filer som har skapats.</span><span class="sxs-lookup"><span data-stu-id="a78ac-156">Change directory toowhere you want hello cert files created.</span></span>  <span data-ttu-id="a78ac-157">För hello inledande och avslutande hello certifikatet, Använd hello aktuellt datum plus 1 år.</span><span class="sxs-lookup"><span data-stu-id="a78ac-157">Also, for hello beginning and ending date of hello certificate, use hello current date plus 1 year.</span></span>

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 03/07/2017 -e 03/07/2018 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

<span data-ttu-id="a78ac-158">Anteckna hello slutdatum och .pfx hello hello lösenord (i det här exemplet: 07/31/2016 och test123).</span><span class="sxs-lookup"><span data-stu-id="a78ac-158">Make note of hello end date and hello password for hello .pfx (in this example: 07/31/2016 and test123).</span></span> <span data-ttu-id="a78ac-159">Du behöver dem nedan.</span><span class="sxs-lookup"><span data-stu-id="a78ac-159">You will need them below.</span></span>

<span data-ttu-id="a78ac-160">Mer information om hur du skapar ett testcertifikat finns [så här: skapa dina egna testa certifikat](https://msdn.microsoft.com/library/ff699202.aspx)</span><span class="sxs-lookup"><span data-stu-id="a78ac-160">For more information on creating a test certificate, see [How to: Create Your Own Test Certificate](https://msdn.microsoft.com/library/ff699202.aspx)</span></span>

<span data-ttu-id="a78ac-161">**Associera hello certifikat med ett Azure AD-program** nu när du har ett certifikat måste tooassociate den med Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="a78ac-161">**Associate hello Certificate with an Azure AD application** Now that you have a certificate, you need tooassociate it with an Azure AD application.</span></span> <span data-ttu-id="a78ac-162">Hello Azure Portal stöder för närvarande kan inte arbetsflöden. Detta kan utföras via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="a78ac-162">Presently, hello Azure Portal does not support this workflow; this can be completed through PowerShell.</span></span> <span data-ttu-id="a78ac-163">Kör följande kommandon tooassoicate hello certifikat med hello Azure AD-programmet hello:</span><span class="sxs-lookup"><span data-stu-id="a78ac-163">Run hello following commands tooassoicate hello certificate with hello Azure AD application:</span></span>

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $x509.Import("C:\data\KVWebApp.cer")
    $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    # If you used different dates for makecert then adjust these values
    $now = [System.DateTime]::Now
    $yearfromnow = $now.AddYears(1)

    $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $now -EndDate $yearfromnow

    $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get hello thumbprint toouse in your app settings
    $x509.Thumbprint

<span data-ttu-id="a78ac-164">När du har kört dessa kommandon, kan du se hello program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a78ac-164">After you have run these commands, you can see hello application in Azure AD.</span></span> <span data-ttu-id="a78ac-165">När du söker kan se till att du väljer ”mitt företag äger” i stället för ”program företaget använder” i dialogrutan för hello Sök.</span><span class="sxs-lookup"><span data-stu-id="a78ac-165">When searching, ensure you select "Applications my company owns" instead of "Applications my company uses" in hello search dialog.</span></span>

<span data-ttu-id="a78ac-166">toolearn mer om Azure AD-program och ServicePrincipal objekt, se [program och tjänstens huvudnamn objekt](../active-directory/active-directory-application-objects.md)</span><span class="sxs-lookup"><span data-stu-id="a78ac-166">toolearn more about Azure AD Application Objects and ServicePrincipal Objects, see [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md)</span></span>

<span data-ttu-id="a78ac-167">**Lägg till kod tooyour Web App toouse hello certifikat** nu ska du lägga till kod tooyour Web App tooaccess hello cert och användas för autentisering.</span><span class="sxs-lookup"><span data-stu-id="a78ac-167">**Add code tooyour Web App toouse hello Certificate** Now we will add code tooyour Web App tooaccess hello cert and use it for authentication.</span></span>

<span data-ttu-id="a78ac-168">Det finns först kod tooaccess hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="a78ac-168">First there is code tooaccess hello cert.</span></span>

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since hello test root isn't installed.
                if (col == null || col.Count == 0)
                    return null;
                return col[0];
            }
            finally
            {
                store.Close();
            }
        }
    }


<span data-ttu-id="a78ac-169">Observera att hello StoreLocation CurrentUser i stället för LocalMachine.</span><span class="sxs-lookup"><span data-stu-id="a78ac-169">Note that hello StoreLocation is CurrentUser instead of LocalMachine.</span></span> <span data-ttu-id="a78ac-170">Och som vi tillhandahåller 'false' toohello hitta metod eftersom vi använder ett test-certifikat.</span><span class="sxs-lookup"><span data-stu-id="a78ac-170">And that we are supplying 'false' toohello Find method because we are using a test cert.</span></span>

<span data-ttu-id="a78ac-171">Nästa är kod som använder hello CertificateHelper och skapar en ClientAssertionCertificate som krävs för autentisering.</span><span class="sxs-lookup"><span data-stu-id="a78ac-171">Next is code that uses hello CertificateHelper and creates a ClientAssertionCertificate which is needed for authentication.</span></span>

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


<span data-ttu-id="a78ac-172">Här är hello ny kod tooget hello åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="a78ac-172">Here is hello new code tooget hello access token.</span></span> <span data-ttu-id="a78ac-173">Det här ersätter hello GetToken metoden ovan.</span><span class="sxs-lookup"><span data-stu-id="a78ac-173">This replaces hello GetToken method above.</span></span> <span data-ttu-id="a78ac-174">Jag har gett den ett annat namn i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="a78ac-174">I have given it a different name for convenience.</span></span>

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

<span data-ttu-id="a78ac-175">Jag har placera alla den här koden i mitt Web App-projekt verktyg för webbplatsuppgradering klass för enkel användning.</span><span class="sxs-lookup"><span data-stu-id="a78ac-175">I have put all of this code into my Web App project's Utils class for ease of use.</span></span>

<span data-ttu-id="a78ac-176">hello senaste kodändring är i hello Application_Start metod.</span><span class="sxs-lookup"><span data-stu-id="a78ac-176">hello last code change is in hello Application_Start method.</span></span> <span data-ttu-id="a78ac-177">Vi måste först toocall hello GetCert() metoden tooload hello ClientAssertionCertificate.</span><span class="sxs-lookup"><span data-stu-id="a78ac-177">First we need toocall hello GetCert() method tooload hello ClientAssertionCertificate.</span></span> <span data-ttu-id="a78ac-178">Och vi ändra hello-metod som vi tillhandahåller när du skapar en ny KeyVaultClient.</span><span class="sxs-lookup"><span data-stu-id="a78ac-178">And then we change hello callback method that we supply when creating a new KeyVaultClient.</span></span> <span data-ttu-id="a78ac-179">Observera att detta ersätter hello koden som vi hade ovan.</span><span class="sxs-lookup"><span data-stu-id="a78ac-179">Note that this replaces hello code that we had above.</span></span>

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


<span data-ttu-id="a78ac-180">**Lägg till certifikat tooyour webbprogram via hello Azure Portal** att lägga till ett certifikat tooyour Web App är en enkel process.</span><span class="sxs-lookup"><span data-stu-id="a78ac-180">**Add a Certificate tooyour Web App through hello Azure Portal** Adding a Certificate tooyour Web App is a simple two-step process.</span></span> <span data-ttu-id="a78ac-181">Först gå toohello Azure-portalen och navigera tooyour Web App.</span><span class="sxs-lookup"><span data-stu-id="a78ac-181">First, go toohello Azure Portal and navigate tooyour Web App.</span></span> <span data-ttu-id="a78ac-182">Klicka på hello post för ”anpassade domäner och SSL” på hello inställningar-bladet för din Webbapp.</span><span class="sxs-lookup"><span data-stu-id="a78ac-182">On hello Settings blade for your Web App, click on hello entry for "Custom domains and SSL".</span></span> <span data-ttu-id="a78ac-183">På hello bladet som öppnar du är kan tooupload hello certifikatet som du skapade ovan KVWebApp.pfx, Kom ihåg att hello lösenord för hello pfx.</span><span class="sxs-lookup"><span data-stu-id="a78ac-183">On hello blade that opens you will be able tooupload hello Certificate that you created above, KVWebApp.pfx, make sure that you remember hello password for hello pfx.</span></span>

![Att lägga till ett certifikat tooa webbprogram i hello Azure-portalen][2]

<span data-ttu-id="a78ac-185">hello sista du behöver toodo är tooadd en tillämpningsinställning tooyour webbprogram som har hello namn webbplats\_BELASTNINGEN\_certifikat och värdet *.</span><span class="sxs-lookup"><span data-stu-id="a78ac-185">hello last thing that you need toodo is tooadd an Application Setting tooyour Web App that has hello name WEBSITE\_LOAD\_CERTIFICATES and a value of *.</span></span> <span data-ttu-id="a78ac-186">Se till att alla certifikat har lästs in.</span><span class="sxs-lookup"><span data-stu-id="a78ac-186">This will ensure that all Certificates are loaded.</span></span> <span data-ttu-id="a78ac-187">Om du vill använda tooload endast hello certifikat som du har laddat upp och du kan ange en kommaavgränsad lista över sina tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="a78ac-187">If you wanted tooload only hello Certificates that you have uploaded, then you can enter a comma-separated list of their thumbprints.</span></span>

<span data-ttu-id="a78ac-188">toolearn mer information om hur du lägger till ett certifikat tooa Web App finns [med hjälp av certifikat i Azure webbplatser program](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)</span><span class="sxs-lookup"><span data-stu-id="a78ac-188">toolearn more about adding a Certificate tooa Web App, see [Using Certificates in Azure Websites Applications](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)</span></span>

<span data-ttu-id="a78ac-189">**Lägga till ett certifikat tooKey valvet som en hemlighet** i stället för att ladda upp dina certifikat toohello Web App service direkt, kan du lagra den på Key Vault som en hemlighet och distribuera den därifrån.</span><span class="sxs-lookup"><span data-stu-id="a78ac-189">**Add a Certificate tooKey Vault as a secret** Instead of uploading your certificate toohello Web App service directly, you can store it in Key Vault as a secret and deploy it from there.</span></span> <span data-ttu-id="a78ac-190">Detta är en tvåstegsprocess som beskrivs i följande blogginlägget hello [distribuera Azure-certifikat för webbprogram via Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)</span><span class="sxs-lookup"><span data-stu-id="a78ac-190">This is a two-step process that is outlined in hello following blog post, [Deploying Azure Web App Certificate through Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)</span></span>

## <span data-ttu-id="a78ac-191"><a id="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a78ac-191"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="a78ac-192">Programmering referenser finns [Azure Key Vault C# klienten API-referens för](https://msdn.microsoft.com/library/azure/dn903628.aspx).</span><span class="sxs-lookup"><span data-stu-id="a78ac-192">For programming references, see [Azure Key Vault C# Client API Reference](https://msdn.microsoft.com/library/azure/dn903628.aspx).</span></span>

<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
