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
# <a name="use-azure-key-vault-from-a-web-application"></a>Använda Azure Key Vault från webbprogram
## <a name="introduction"></a>Introduktion
Använd den här självstudiekursen toohelp du lära dig hur toouse Azure nyckeln valvet från ett webbprogram i Azure. Den vägleder dig genom hello processen att komma åt en hemlighet från ett Azure Key Vault så att den kan användas i ditt webbprogram.

**Uppskattad tid toocomplete:** 15 minuter

Översiktlig information om Azure Key Vault finns i [Vad är Azure Key Vault?](key-vault-whatis.md)

## <a name="prerequisites"></a>Krav
toocomplete den här självstudien måste du ha hello följande:

* En URI tooa hemlighet i en Azure Key Vault
* En klient-ID och en Klienthemlighet för ett webbprogram som har registrerats med Azure Active Directory som har åtkomst tooyour Key Vault
* Ett webbprogram. Vi ska visa hello steg för ett ASP.NET MVC-program som distribueras i Azure som en Webbapp.

> [!NOTE]
> Det är viktigt att du har slutfört stegen i hello [Kom igång med Azure Key Vault](key-vault-get-started.md) för den här självstudiekursen så att du har hello URI tooa hemlighet och hello klient-ID och Klienthemlighet för ett webbprogram.
> 
> 

hello webbprogram som kommer åt hello Key Vault är hello något som är registrerad i Azure Active Directory och har fått åtkomst tooyour Key Vault. Om detta inte är fallet hello gå tillbaka tooRegister ett program i hello komma igång-kursen och upprepa hello stegen.

Den här kursen är avsedd för webbutvecklare som förstår hello grunderna för att skapa webbprogram på Azure. Läs mer om Azure Web Apps [översikt över Web Apps](../app-service-web/app-service-web-overview.md).

## <a id="packages"></a>Lägg till Nuget-paket
Det finns två paket som ditt webbprogram måste toohave installerad.

* Active Directory Authentication Library - innehåller metoder för att interagera med Azure Active Directory och hantera användaridentitet
* Azure Key Vault Library - innehåller metoder för att interagera med Azure Key Vault

Båda dessa paket kan installeras med hello kommandot hello Install-Package Package Manager-konsolen.

    // this is currently hello latest stable version of ADAL
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.16.204221202

    Install-Package Microsoft.Azure.KeyVault


## <a id="webconfig"></a>Ändra Web.Config
Det finns tre inställningar för program som behöver toobe tillagda toohello web.config-filen på följande sätt.

    <!-- ClientId and ClientSecret refer toohello web application registration with Azure Active Directory -->
    <add key="ClientId" value="clientid" />
    <add key="ClientSecret" value="clientsecret" />

    <!-- SecretUri is hello URI for hello secret in Azure Key Vault -->
    <add key="SecretUri" value="secreturi" />


Om du inte kommer toohost ditt program som en Azure Web App, bör du lägga till hello faktiska ClientId, Klienthemligheten och hemlighet URI värden toohello web.config. Annars lämnar du värdena dummy eftersom vi kommer att lägga till hello faktiska värden i hello Azure-portalen för en extra nivå av säkerhet.

## <a id="gettoken"></a>Lägg till metoden tooGet en åtkomst-Token
I ordning toouse hello Key Vault API behöver du en åtkomst-token. hello Key Vault-klienten hanterar anrop toohello Key Vault-API, men du måste toosupply den med en funktion som hämtar hello åtkomst-token.  

Följande är hello koden tooget en åtkomst-token från Azure Active Directory. Den här koden kan gå var som helst i programmet. Jag som tooadd ett verktyg för webbplatsuppgradering eller EncryptionHelper klass.  

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
> Använda ett klient-ID och Klienthemlighet är hello enklaste sättet tooauthenticate ett Azure AD-program. Och använda den i ditt webbprogram för en uppdelning av uppgifter och mer kontroll över din nyckelhantering. Men den förlitar sig på att placera hello Klienthemlighet i inställningarna som kan vara riskabelt som som ger hello hemlighet som du vill tooprotect i inställningarna för några. Nedan visas en diskussion om hur toouse klient-ID och certifikat i stället för klient-ID och Klienthemlighet tooauthenticate hello Azure AD-program.
> 
> 

## <a id="appstart"></a>Hämta hello hemligheten på programmet starta
Vi behöver nu code toocall hello Key Vault-API och hämta hello hemlighet. hello följande kod kan placeras var som helst så länge den anropas innan du behöver toouse den. Jag har placerat den här koden i hello programmet Starta händelsen i hello Global.asax så att den körs en gång vid start och gör hello hemlighet som är tillgängliga för hello program.

    //add these using statements
    using Microsoft.Azure.KeyVault;
    using System.Web.Configuration;

    // I put my GetToken method in a Utils class. Change for wherever you placed your method.
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

    var sec = await kv.GetSecretAsync(WebConfigurationManager.AppSettings["SecretUri"]);

    //I put a variable in a Utils class toohold hello secret for general  application use.
    Utils.EncryptSecret = sec.Value;



## <a id="portalsettings"></a>Lägg till App-inställningar i hello Azure-portalen (valfritt)
Du kan nu lägga hello faktiska värden för hello AppSettings i hello Azure-portalen om du har en Azure-Webbapp. Genom att göra hello faktiska värden är inte i hello web.config men skyddas via hello Portal där du har separata åtkomstfunktioner för kontrollen. Dessa värden ersätts för hello-värden som du angav i filen web.config. Kontrollera att hello namn är hello samma.

![Programinställningar som visas i Azure Portal][1]

## <a name="authenticate-with-a-certificate-instead-of-a-client-secret"></a>Autentisera med ett certifikat i stället för en Klienthemlighet
Ett annat sätt tooauthenticate ett Azure AD-program är att använda ett klient-ID och ett certifikat i stället för en klient-ID och Klienthemlighet. Följande är hello steg toouse ett certifikat i en Azure Web App:

1. Hämta eller skapa ett certifikat
2. Associera hello certifikat med ett Azure AD-program
3. Lägg till kod tooyour Web App toouse hello certifikat
4. Lägg till ett certifikat tooyour Web App

**Hämta eller skapa ett certifikat** för våra ändamål vi gör ett testcertifikat. Här är några av kommandon som du kan använda i en kommandotolk för utvecklare toocreate ett certifikat. Ändra katalogen toowhere som du vill hello cert filer som har skapats.  För hello inledande och avslutande hello certifikatet, Använd hello aktuellt datum plus 1 år.

    makecert -sv mykey.pvk -n "cn=KVWebApp" KVWebApp.cer -b 03/07/2017 -e 03/07/2018 -r
    pvk2pfx -pvk mykey.pvk -spc KVWebApp.cer -pfx KVWebApp.pfx -po test123

Anteckna hello slutdatum och .pfx hello hello lösenord (i det här exemplet: 07/31/2016 och test123). Du behöver dem nedan.

Mer information om hur du skapar ett testcertifikat finns [så här: skapa dina egna testa certifikat](https://msdn.microsoft.com/library/ff699202.aspx)

**Associera hello certifikat med ett Azure AD-program** nu när du har ett certifikat måste tooassociate den med Azure AD-program. Hello Azure Portal stöder för närvarande kan inte arbetsflöden. Detta kan utföras via PowerShell. Kör följande kommandon tooassoicate hello certifikat med hello Azure AD-programmet hello:

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

När du har kört dessa kommandon, kan du se hello program i Azure AD. När du söker kan se till att du väljer ”mitt företag äger” i stället för ”program företaget använder” i dialogrutan för hello Sök.

toolearn mer om Azure AD-program och ServicePrincipal objekt, se [program och tjänstens huvudnamn objekt](../active-directory/active-directory-application-objects.md)

**Lägg till kod tooyour Web App toouse hello certifikat** nu ska du lägga till kod tooyour Web App tooaccess hello cert och användas för autentisering.

Det finns först kod tooaccess hello certifikat.

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


Observera att hello StoreLocation CurrentUser i stället för LocalMachine. Och som vi tillhandahåller 'false' toohello hitta metod eftersom vi använder ett test-certifikat.

Nästa är kod som använder hello CertificateHelper och skapar en ClientAssertionCertificate som krävs för autentisering.

    public static ClientAssertionCertificate AssertionCert { get; set; }

    public static void GetCert()
    {
        var clientAssertionCertPfx = CertificateHelper.FindCertificateByThumbprint(WebConfigurationManager.AppSettings["thumbprint"]);
        AssertionCert = new ClientAssertionCertificate(WebConfigurationManager.AppSettings["clientid"], clientAssertionCertPfx);
    }


Här är hello ny kod tooget hello åtkomsttoken. Det här ersätter hello GetToken metoden ovan. Jag har gett den ett annat namn i informationssyfte.

    public static async Task<string> GetAccessToken(string authority, string resource, string scope)
    {
        var context = new AuthenticationContext(authority, TokenCache.DefaultShared);
        var result = await context.AcquireTokenAsync(resource, AssertionCert);
        return result.AccessToken;
    }

Jag har placera alla den här koden i mitt Web App-projekt verktyg för webbplatsuppgradering klass för enkel användning.

hello senaste kodändring är i hello Application_Start metod. Vi måste först toocall hello GetCert() metoden tooload hello ClientAssertionCertificate. Och vi ändra hello-metod som vi tillhandahåller när du skapar en ny KeyVaultClient. Observera att detta ersätter hello koden som vi hade ovan.

    Utils.GetCert();
    var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetAccessToken));


**Lägg till certifikat tooyour webbprogram via hello Azure Portal** att lägga till ett certifikat tooyour Web App är en enkel process. Först gå toohello Azure-portalen och navigera tooyour Web App. Klicka på hello post för ”anpassade domäner och SSL” på hello inställningar-bladet för din Webbapp. På hello bladet som öppnar du är kan tooupload hello certifikatet som du skapade ovan KVWebApp.pfx, Kom ihåg att hello lösenord för hello pfx.

![Att lägga till ett certifikat tooa webbprogram i hello Azure-portalen][2]

hello sista du behöver toodo är tooadd en tillämpningsinställning tooyour webbprogram som har hello namn webbplats\_BELASTNINGEN\_certifikat och värdet *. Se till att alla certifikat har lästs in. Om du vill använda tooload endast hello certifikat som du har laddat upp och du kan ange en kommaavgränsad lista över sina tumavtryck.

toolearn mer information om hur du lägger till ett certifikat tooa Web App finns [med hjälp av certifikat i Azure webbplatser program](https://azure.microsoft.com/blog/2014/10/27/using-certificates-in-azure-websites-applications/)

**Lägga till ett certifikat tooKey valvet som en hemlighet** i stället för att ladda upp dina certifikat toohello Web App service direkt, kan du lagra den på Key Vault som en hemlighet och distribuera den därifrån. Detta är en tvåstegsprocess som beskrivs i följande blogginlägget hello [distribuera Azure-certifikat för webbprogram via Key Vault](https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/)

## <a id="next"></a>Nästa steg
Programmering referenser finns [Azure Key Vault C# klienten API-referens för](https://msdn.microsoft.com/library/azure/dn903628.aspx).

<!--Image references-->
[1]: ./media/key-vault-use-from-web-application/PortalAppSettings.png
[2]: ./media/key-vault-use-from-web-application/PortalAddCertificate.png
