---
title: "Använda Azure Key Vault från ett webbprogram | Microsoft Docs"
description: "Använd den här kursen för att lära dig hur du använder Azure Key Vault från ett webbprogram."
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
ms.openlocfilehash: d095bcfe37baefa90cf79bb48bff3f703ce1dad7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-key-vault-from-a-web-application"></a><span data-ttu-id="3588c-103">Använda Azure Key Vault från webbprogram</span><span class="sxs-lookup"><span data-stu-id="3588c-103">Use Azure Key Vault from a Web Application</span></span>
## <a name="introduction"></a><span data-ttu-id="3588c-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="3588c-104">Introduction</span></span>
<span data-ttu-id="3588c-105">Använd den här kursen för att lära dig hur du använder Azure Key Vault från ett webbprogram i Azure.</span><span class="sxs-lookup"><span data-stu-id="3588c-105">Use this tutorial to help you learn how to use Azure Key Vault from a web application in Azure.</span></span> <span data-ttu-id="3588c-106">Den vägleder dig genom processen att komma åt en hemlighet från ett Azure Key Vault så att den kan användas i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3588c-106">It walks you through the process of accessing a secret from an Azure Key Vault so that it can be used in your web application.</span></span>

<span data-ttu-id="3588c-107">**Uppskattad tidsåtgång:** 15 minuter</span><span class="sxs-lookup"><span data-stu-id="3588c-107">**Estimated time to complete:** 15 minutes</span></span>

<span data-ttu-id="3588c-108">Översiktlig information om Azure Key Vault finns i [Vad är Azure Key Vault?](key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="3588c-108">For overview information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3588c-109">Krav</span><span class="sxs-lookup"><span data-stu-id="3588c-109">Prerequisites</span></span>
<span data-ttu-id="3588c-110">För att kunna slutföra den här självstudiekursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="3588c-110">To complete this tutorial, you must have the following:</span></span>

* <span data-ttu-id="3588c-111">En URI för en hemlighet i en Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="3588c-111">A URI to a secret in an Azure Key Vault</span></span>
* <span data-ttu-id="3588c-112">En klient-ID och en Klienthemlighet för ett webbprogram som har registrerats med Azure Active Directory som har åtkomst till ditt Nyckelvalv</span><span class="sxs-lookup"><span data-stu-id="3588c-112">A Client ID and a Client Secret for a web application registered with Azure Active Directory that has access to your Key Vault</span></span>
* <span data-ttu-id="3588c-113">Ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3588c-113">A web application.</span></span> <span data-ttu-id="3588c-114">Vi ska visa stegen för ett ASP.NET MVC-program som distribueras i Azure som en Webbapp.</span><span class="sxs-lookup"><span data-stu-id="3588c-114">We will be showing the steps for an ASP.NET MVC application deployed in Azure as a Web App.</span></span>

> [!NOTE]
> <span data-ttu-id="3588c-115">Det är viktigt att du har slutfört stegen i [Kom igång med Azure Key Vault](key-vault-get-started.md) för den här självstudiekursen så att du har en hemlighet och klient-ID och Klienthemlighet URI för ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3588c-115">It is essential that you have completed the steps listed in [Get Started with Azure Key Vault](key-vault-get-started.md) for this tutorial so that you have the URI to a secret and the Client ID and Client Secret for a web application.</span></span>
> 
> 

<span data-ttu-id="3588c-116">Webbprogram som kommer åt Nyckelvalvet är det som är registrerad i Azure Active Directory och har fått tillgång till ditt Nyckelvalv.</span><span class="sxs-lookup"><span data-stu-id="3588c-116">The web application that will be accessing the Key Vault is the one that is registered in Azure Active Directory and has been given access to your Key Vault.</span></span> <span data-ttu-id="3588c-117">Om detta inte är fallet kan du gå tillbaka till registrera ett program i komma igång-kursen och upprepa stegen.</span><span class="sxs-lookup"><span data-stu-id="3588c-117">If this is not the case, go back to Register an Application in the Get Started tutorial and repeat the steps listed.</span></span>

<span data-ttu-id="3588c-118">Den här kursen är avsedd för webbutvecklare som förstår grunderna för att skapa webbprogram på Azure.</span><span class="sxs-lookup"><span data-stu-id="3588c-118">This tutorial is designed for web developers that understand the basics of creating web applications on Azure.</span></span> <span data-ttu-id="3588c-119">Läs mer om Azure Web Apps [översikt över Web Apps](../app-service-web/app-service-web-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3588c-119">For more information about Azure Web Apps, see [Web Apps overview](../app-service-web/app-service-web-overview.md).</span></span>

## <span data-ttu-id="3588c-120"><a id="packages"></a>Lägg till Nuget-paket</span><span class="sxs-lookup"><span data-stu-id="3588c-120"><a id="packages"></a>Add Nuget Packages</span></span>
<span data-ttu-id="3588c-121">Det finns två paket som ditt webbprogram måste ha installerats.</span><span class="sxs-lookup"><span data-stu-id="3588c-121">There are two packages that your web application needs to have installed.</span></span>

* <span data-ttu-id="3588c-122">Active Directory Authentication Library - innehåller metoder för att interagera med Azure Active Directory och hantera användaridentitet</span><span class="sxs-lookup"><span data-stu-id="3588c-122">Active Directory Authentication Library - contains methods for interacting with Azure Active Directory and managing user identity</span></span>
* <span data-ttu-id="3588c-123">Azure Key Vault Library - innehåller metoder för att interagera med Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="3588c-123">Azure Key Vault Library - contains methods for interacting with Azure Key Vault</span></span>

<span data-ttu-id="3588c-124">Båda dessa paket kan installeras med hjälp av kommandot Install-Package Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="3588c-124">Both of these packages can be installed using the Package Manager Console using the Install-Package command.</span></span>

    // this is currently the latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <span data-ttu-id="3588c-125"><a id="webconfig"></a>Ändra Web.Config</span><span class="sxs-lookup"><span data-stu-id="3588c-125"><a id="webconfig"></a>Modify Web.Config</span></span>
<span data-ttu-id="3588c-126">Det finns tre inställningar för program som behöver läggas till i filen web.config på följande sätt.</span><span class="sxs-lookup"><span data-stu-id="3588c-126">There are three application settings that need to be added to the web.config file as follows.</span></span>

    <!-- ClientId and ClientSecret refer to the web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is the URI for the secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


<span data-ttu-id="3588c-127">Om du inte kommer att vara värd för programmet som en Azure Web App, bör du lägga till faktiska värden för ClientId, Klienthemligheten och hemlighet URI till web.config.</span><span class="sxs-lookup"><span data-stu-id="3588c-127">If you are not going to host your application as an Azure Web App, then you should add the actual ClientId, Client Secret, and Secret URI values to the web.config.</span></span> <span data-ttu-id="3588c-128">Annars lämnar du värdena dummy eftersom vi kommer att lägga till de faktiska värdena i Azure Portal för en extra nivå av säkerhet.</span><span class="sxs-lookup"><span data-stu-id="3588c-128">Otherwise leave these dummy values because we will be adding the actual values in the Azure Portal for an additional level of security.</span></span>

## <span data-ttu-id="3588c-129"><a id="gettoken"></a>Lägg till metoden för att hämta ett åtkomsttoken</span><span class="sxs-lookup"><span data-stu-id="3588c-129"><a id="gettoken"></a>Add Method to Get an Access Token</span></span>
<span data-ttu-id="3588c-130">För att kunna använda Key Vault API behöver du en åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="3588c-130">In order to use the Key Vault API you need an access token.</span></span> <span data-ttu-id="3588c-131">Key Vault klienten hanterar anrop till Key Vault-API men du måste ange den med en funktion som hämtar den åtkomst-token.</span><span class="sxs-lookup"><span data-stu-id="3588c-131">The Key Vault Client handles calls to the Key Vault API but you need to supply it with a function that gets the access token.</span></span>  

<span data-ttu-id="3588c-132">Följande är koden för att hämta en token från Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3588c-132">Following is the code to get an access token from Azure Active Directory.</span></span> <span data-ttu-id="3588c-133">Den här koden kan gå var som helst i programmet.</span><span class="sxs-lookup"><span data-stu-id="3588c-133">This code can go anywhere in your application.</span></span> <span data-ttu-id="3588c-134">Jag vill lägga till en klass med verktyg för webbplatsuppgradering eller EncryptionHelper.</span><span class="sxs-lookup"><span data-stu-id="3588c-134">I like to add a Utils or EncryptionHelper class.</span></span>  

    //add these using statements
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System.Threading.Tasks;
    using System.Web.Configuration;

    //this is an optional property to hold the secret after it is retrieved
    public static string EncryptSecret { get; set; }

    //the method that will be provided to the KeyVaultClient
    public static async Task<string> GetToken(string authority, string resource, string scope)
    {
        var authContext = new AuthenticationContext(authority);
        ClientCredential clientCred = new ClientCredential(WebConfigurationManager.AppSettings["ClientId"],
                    WebConfigurationManager.AppSettings["ClientSecret"]);
        AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

        if (result == null)
            throw new InvalidOperationException("Failed to obtain the JWT token");

        return result.AccessToken;
    }

> [!NOTE]
> <span data-ttu-id="3588c-135">Använda ett klient-ID och Klienthemlighet är det enklaste sättet att autentisera en Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="3588c-135">Using a Client ID and Client Secret is the easiest way to authenticate an Azure AD application.</span></span> <span data-ttu-id="3588c-136">Och använda den i ditt webbprogram för en uppdelning av uppgifter och mer kontroll över din nyckelhantering.</span><span class="sxs-lookup"><span data-stu-id="3588c-136">And using it in your web application allows for a separation of duties and more control over your key management.</span></span> <span data-ttu-id="3588c-137">Men den förlitar sig på att placera Klienthemligheten i inställningarna som kan vara riskabelt som som ger den hemlighet som du vill skydda i inställningarna för några.</span><span class="sxs-lookup"><span data-stu-id="3588c-137">But it does rely on putting the Client Secret in your configuration settings which for some can be as risky as putting the secret that you want to protect in your configuration settings.</span></span> <span data-ttu-id="3588c-138">Nedan visas en diskussion om hur du använder en klient-ID och certifikat i stället för klient-ID och Klienthemlighet för att autentisera Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="3588c-138">See below for a discussion on how to use a Client ID and Certificate instead of Client ID and Client Secret to authenticate the Azure AD application.</span></span>
> 
> 

## <span data-ttu-id="3588c-139"><a id="appstart"></a>Hämta hemligheten på programmet starta</span><span class="sxs-lookup"><span data-stu-id="3588c-139"><a id="appstart"></a>Retrieve the secret on Application Start</span></span>
<span data-ttu-id="3588c-140">Nu måste vi kod för att anropa API för Key Vault och hämta hemligheten.</span><span class="sxs-lookup"><span data-stu-id="3588c-140">Now we need code to call the Key Vault API and retrieve the secret.</span></span> <span data-ttu-id="3588c-141">Följande kod kan placeras var som helst så länge den anropas innan du behöver använda den.</span><span class="sxs-lookup"><span data-stu-id="3588c-141">The following code can be put anywhere as long as it is called before you need to use it.</span></span> <span data-ttu-id="3588c-142">Jag har placerat den här koden i programmet Starta händelsen i Global.asax så att den körs en gång vid start och tillgängliggör hemligheten för programmet.</span><span class="sxs-lookup"><span data-stu-id="3588c-142">I have put this code in the Application Start event in the Global.asax so that it runs once on start and makes the secret available for the application.</span></span>

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

    //I put a variable in a Utils class to hold the secret for general  application use.
    Utils.EncryptSecret = sec.Value;



## <span data-ttu-id="3588c-143"><a id="portalsettings"></a>Lägg till App-inställningar i Azure-portalen (valfritt)</span><span class="sxs-lookup"><span data-stu-id="3588c-143"><a id="portalsettings"></a>Add App Settings in the Azure Portal (optional)</span></span>
<span data-ttu-id="3588c-144">Om du har en Azure Web App kan du nu lägga till de faktiska värdena för AppSettings i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="3588c-144">If you have an Azure Web App you can now add the actual values for the AppSettings in the Azure Portal.</span></span> <span data-ttu-id="3588c-145">Genom att göra de faktiska värdena är inte i web.config men skyddas via portalen där du har separata åtkomstfunktioner för kontrollen.</span><span class="sxs-lookup"><span data-stu-id="3588c-145">By doing this, the actual values will not be in the web.config but protected via the Portal where you have separate access control capabilities.</span></span> <span data-ttu-id="3588c-146">Dessa värden ersätts med de värden som du angav i filen web.config.</span><span class="sxs-lookup"><span data-stu-id="3588c-146">These values will be substituted for the values that you entered in your web.config.</span></span> <span data-ttu-id="3588c-147">Kontrollera att namnen är samma.</span><span class="sxs-lookup"><span data-stu-id="3588c-147">Make sure that the names are the same.</span></span>

![Programinställningar som visas i Azure Portal][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a><span data-ttu-id="3588c-149">Autentisera med ett certifikat i stället för en Klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="3588c-149">Authenticate with a Certificate instead of a Client Secret</span></span>
<span data-ttu-id="3588c-150">Ett annat sätt att autentisera en Azure AD-program är att använda ett klient-ID och ett certifikat i stället för en klient-ID och Klienthemlighet.</span><span class="sxs-lookup"><span data-stu-id="3588c-150">Another way to authenticate an Azure AD application is by using a Client ID and a Certificate instead of a Client ID and Client Secret.</span></span> <span data-ttu-id="3588c-151">Nedan följer stegen för att använda ett certifikat i en Azure Web App:</span><span class="sxs-lookup"><span data-stu-id="3588c-151">Following are the steps to use a Certificate in an Azure Web App:</span></span>

1. <span data-ttu-id="3588c-152">Hämta eller skapa ett certifikat</span><span class="sxs-lookup"><span data-stu-id="3588c-152">Get or Create a Certificate</span></span>
2. <span data-ttu-id="3588c-153">Associera certifikatet med ett Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="3588c-153">Associate the Certificate with an Azure AD application</span></span>
3. <span data-ttu-id="3588c-154">Lägg till kod i ditt webbprogram till att använda certifikat</span><span class="sxs-lookup"><span data-stu-id="3588c-154">Add code to your Web App to use the Certificate</span></span>
4. <span data-ttu-id="3588c-155">Lägga till ett certifikat till ditt webbprogram</span><span class="sxs-lookup"><span data-stu-id="3588c-155">Add a Certificate to your Web App</span></span>

<span data-ttu-id="3588c-156">**Hämta eller skapa ett certifikat** för våra ändamål vi gör ett testcertifikat.</span><span class="sxs-lookup"><span data-stu-id="3588c-156">**Get or Create a Certificate** For our purposes we will make a test certificate.</span></span> <span data-ttu-id="3588c-157">Här är några av kommandon som du kan använda i en utvecklare kommandotolk för att skapa ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="3588c-157">Here are a couple of commands that you can use in a Developer Command Prompt to create a certificate.</span></span> <span data-ttu-id="3588c-158">Ändra katalogen till önskad cert-filer som har skapats.</span><span class="sxs-lookup"><span data-stu-id="3588c-158">Change directory to where you want the cert files created.</span></span>  <span data-ttu-id="3588c-159">Kan också använda aktuellt datum plus 1 år för start- och slutdatum för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="3588c-159">Also, for the beginning and ending date of the certificate, use the current date plus 1 year.</span></span>

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 03/07/2017 -e 03/07/2018 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

<span data-ttu-id="3588c-160">Anteckna slutdatumet och lösenordet för PFX (i det här exemplet: 07/31/2016 och test123).</span><span class="sxs-lookup"><span data-stu-id="3588c-160">Make note of the end date and the password for the .pfx (in this example: 07/31/2016 and test123).</span></span> <span data-ttu-id="3588c-161">Du behöver dem nedan.</span><span class="sxs-lookup"><span data-stu-id="3588c-161">You will need them below.</span></span>

<span data-ttu-id="3588c-162">Mer information om hur du skapar ett testcertifikat finns [så här: skapa dina egna testa certifikat](https://msdn.microsoft.com/library/ff699202.aspx)</span><span class="sxs-lookup"><span data-stu-id="3588c-162">For more information on creating a test certificate, see [How to: Create Your Own Test Certificate](https://msdn.microsoft.com/library/ff699202.aspx)</span></span>

<span data-ttu-id="3588c-163">**Associera certifikatet med ett Azure AD-program** nu när du har ett certifikat måste du koppla den med Azure AD-program.</span><span class="sxs-lookup"><span data-stu-id="3588c-163">**Associate the Certificate with an Azure AD application** Now that you have a certificate, you need to associate it with an Azure AD application.</span></span> <span data-ttu-id="3588c-164">Azure Portal stöder för närvarande kan inte arbetsflöden. Detta kan utföras via PowerShell.</span><span class="sxs-lookup"><span data-stu-id="3588c-164">Presently, the Azure Portal does not support this workflow; this can be completed through PowerShell.</span></span> <span data-ttu-id="3588c-165">Kör följande kommandon för att partner certifikatet med Azure AD-program:</span><span class="sxs-lookup"><span data-stu-id="3588c-165">Run the following commands to assoicate the certificate with the Azure AD application:</span></span>

    $x509 = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $x509.Import("C:\data\KVWebApp.cer")
    $credValue = [System.Convert]::ToBase64String($x509.GetRawCertData())

    # If you used different dates for makecert then adjust these values
    $now = [System.DateTime]::Now
    $yearfromnow = $now.AddYears(1)

    $adapp = New-AzureRmADApplication -DisplayName "KVWebApp" -HomePage "http://kvwebapp" -IdentifierUris "http://kvwebapp" -CertValue $credValue -StartDate $now -EndDate $yearfromnow

    $sp = New-AzureRmADServicePrincipal -ApplicationId $adapp.ApplicationId

    Set-AzureRmKeyVaultAccessPolicy -VaultName 'contosokv' -ServicePrincipalName $sp.ServicePrincipalName -PermissionsToSecrets all -ResourceGroupName 'contosorg'

    # get the thumbprint to use in your app settings
    $x509.Thumbprint

<span data-ttu-id="3588c-166">När du har kört dessa kommandon, kan du se program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3588c-166">After you have run these commands, you can see the application in Azure AD.</span></span> <span data-ttu-id="3588c-167">När du söker kan du kontrollera att du väljer ”mitt företag äger” i stället för ”program företaget använder” i dialogrutan Sök.</span><span class="sxs-lookup"><span data-stu-id="3588c-167">When searching, ensure you select "Applications my company owns" instead of "Applications my company uses" in the search dialog.</span></span>

<span data-ttu-id="3588c-168">Läs mer om Azure AD-program och ServicePrincipal objekt i [program och tjänstens huvudnamn objekt](../active-directory/active-directory-application-objects.md)</span><span class="sxs-lookup"><span data-stu-id="3588c-168">To learn more about Azure AD Application Objects and ServicePrincipal Objects, see [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md)</span></span>

<span data-ttu-id="3588c-169">**Lägg till kod i ditt webbprogram till att använda certifikatet** nu vi lägger till kod i ditt webbprogram för att komma åt cert och användas för autentisering.</span><span class="sxs-lookup"><span data-stu-id="3588c-169">**Add code to your Web App to use the Certificate** Now we will add code to your Web App to access the cert and use it for authentication.</span></span>

<span data-ttu-id="3588c-170">Först är koden för att få åtkomst till certifikatet.</span><span class="sxs-lookup"><span data-stu-id="3588c-170">First there is code to access the cert.</span></span>

    public static class CertificateHelper
    {
        public static X509Certificate2 FindCertificateByThumbprint(string findValue)
        {
            X509Store store = new X509Store(StoreName.My, StoreLocation.CurrentUser);
            try
            {
                store.Open(OpenFlags.ReadOnly);
                X509Certificate2Collection col = store.Certificates.Find(X509FindType.FindByThumbprint,
                    findValue, false); // Don't validate certs, since the test root isn't installed.
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


<span data-ttu-id="3588c-171">Observera att StoreLocation CurrentUser i stället för LocalMachine.</span><span class="sxs-lookup"><span data-stu-id="3588c-171">Note that the StoreLocation is CurrentUser instead of LocalMachine.</span></span> <span data-ttu-id="3588c-172">Och som vi tillhandahåller false till metoden hitta eftersom vi använder ett test-certifikat.</span><span class="sxs-lookup"><span data-stu-id="3588c-172">And that we are supplying 'false' to the Find method because we are using a test cert.</span></span>

<span data-ttu-id="3588c-173">Nästa är kod som använder CertificateHelper och skapar en ClientAssertionCertificate som krävs för autentisering.</span><span class="sxs-lookup"><span data-stu-id="3588c-173">Next is code that uses the CertificateHelper and creates a ClientAssertionCertificate which is needed for authentication.</span></span>

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


<span data-ttu-id="3588c-174">Här är en ny kod att hämta åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="3588c-174">Here is the new code to get the access token.</span></span> <span data-ttu-id="3588c-175">Det här ersätter metoden GetToken.</span><span class="sxs-lookup"><span data-stu-id="3588c-175">This replaces the GetToken method above.</span></span> <span data-ttu-id="3588c-176">Jag har gett den ett annat namn i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="3588c-176">I have given it a different name for convenience.</span></span>

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

<span data-ttu-id="3588c-177">Jag har placera alla den här koden i mitt Web App-projekt verktyg för webbplatsuppgradering klass för enkel användning.</span><span class="sxs-lookup"><span data-stu-id="3588c-177">I have put all of this code into my Web App project's Utils class for ease of use.</span></span>

<span data-ttu-id="3588c-178">Den senaste ändringen av koden är i metoden Application_Start.</span><span class="sxs-lookup"><span data-stu-id="3588c-178">The last code change is in the Application_Start method.</span></span> <span data-ttu-id="3588c-179">Vi måste först anropa metoden GetCert() för att läsa in ClientAssertionCertificate.</span><span class="sxs-lookup"><span data-stu-id="3588c-179">First we need to call the GetCert() method to load the ClientAssertionCertificate.</span></span> <span data-ttu-id="3588c-180">Och vi ändra Återanropsmetoden som vi tillhandahåller när du skapar en ny KeyVaultClient.</span><span class="sxs-lookup"><span data-stu-id="3588c-180">And then we change the callback method that we supply when creating a new KeyVaultClient.</span></span> <span data-ttu-id="3588c-181">Observera att detta ersätter den kod som vi hade ovan.</span><span class="sxs-lookup"><span data-stu-id="3588c-181">Note that this replaces the code that we had above.</span></span>

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


<span data-ttu-id="3588c-182">**Lägga till ett certifikat till ditt webbprogram via Azure Portal** lägger till ett certifikat till ditt webbprogram är en enkel process.</span><span class="sxs-lookup"><span data-stu-id="3588c-182">**Add a Certificate to your Web App through the Azure Portal** Adding a Certificate to your Web App is a simple two-step process.</span></span> <span data-ttu-id="3588c-183">Gå till Azure Portal först och navigera till ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3588c-183">First, go to the Azure Portal and navigate to your Web App.</span></span> <span data-ttu-id="3588c-184">På inställningsbladet för ditt webbprogram, klickar du på posten för ”anpassade domäner och SSL”.</span><span class="sxs-lookup"><span data-stu-id="3588c-184">On the Settings blade for your Web App, click on the entry for "Custom domains and SSL".</span></span> <span data-ttu-id="3588c-185">I bladet som öppnar du kommer att kunna ladda upp det certifikat som du skapade ovan KVWebApp.pfx, se till att du kommer ihåg lösenordet för pfx.</span><span class="sxs-lookup"><span data-stu-id="3588c-185">On the blade that opens you will be able to upload the Certificate that you created above, KVWebApp.pfx, make sure that you remember the password for the pfx.</span></span>

![Lägga till ett certifikat till en Webbapp i Azure-portalen][2]

<span data-ttu-id="3588c-187">Det sista som du behöver göra är att lägga till en programinställning ditt webbprogram som har namnet webbplatsen\_BELASTNINGEN\_certifikat och värdet *.</span><span class="sxs-lookup"><span data-stu-id="3588c-187">The last thing that you need to do is to add an Application Setting to your Web App that has the name WEBSITE\_LOAD\_CERTIFICATES and a value of *.</span></span> <span data-ttu-id="3588c-188">Se till att alla certifikat har lästs in.</span><span class="sxs-lookup"><span data-stu-id="3588c-188">This will ensure that all Certificates are loaded.</span></span> <span data-ttu-id="3588c-189">Om du vill läsa in de certifikat som du har överfört kan du ange en kommaavgränsad lista över sina tumavtryck.</span><span class="sxs-lookup"><span data-stu-id="3588c-189">If you wanted to load only the Certificates that you have uploaded, then you can enter a comma-separated list of their thumbprints.</span></span>

<span data-ttu-id="3588c-190">Läs mer om att lägga till ett certifikat till en Webbapp i [med hjälp av certifikat i Azure webbplatser program](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)</span><span class="sxs-lookup"><span data-stu-id="3588c-190">To learn more about adding a Certificate to a Web App, see [Using Certificates in Azure Websites Applications](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)</span></span>

<span data-ttu-id="3588c-191">**Lägga till ett certifikat till Key Vault som en hemlighet** i stället för att ladda upp certifikatet till tjänsten Web App direkt, kan du lagra den på Key Vault som en hemlighet och distribuera den därifrån.</span><span class="sxs-lookup"><span data-stu-id="3588c-191">**Add a Certificate to Key Vault as a secret** Instead of uploading your certificate to the Web App service directly, you can store it in Key Vault as a secret and deploy it from there.</span></span> <span data-ttu-id="3588c-192">Detta är en tvåstegsprocess som beskrivs i följande blogginlägg [distribuera Azure-certifikat för webbprogram via Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)</span><span class="sxs-lookup"><span data-stu-id="3588c-192">This is a two-step process that is outlined in the following blog post, [Deploying Azure Web App Certificate through Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)</span></span>

## <span data-ttu-id="3588c-193"><a id="next"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3588c-193"><a id="next"></a>Next steps</span></span>
<span data-ttu-id="3588c-194">Programmering referenser finns [Azure Key Vault C# klienten API-referens för](https://msdn.microsoft.com/library/azure/dn903628.aspx).</span><span class="sxs-lookup"><span data-stu-id="3588c-194">For programming references, see [Azure Key Vault C# Client API Reference](https://msdn.microsoft.com/library/azure/dn903628.aspx).</span></span>

<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
