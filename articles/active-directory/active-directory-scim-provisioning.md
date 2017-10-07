---
title: "aaaUsing System för Identitetshantering domäner automatiskt etablera användare och grupper från Azure Active Directory tooapplications | Microsoft Docs"
description: "Azure Active Directory kan automatiskt etablera användare och grupper tooany program eller identitet butik som är fronted av en webbtjänst med hello-gränssnitt som definierats i hello SCIM protokollspecifikation"
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 4d86f3dc-e2d3-4bde-81a3-4a0e092551c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;oldportal
ms.openlocfilehash: 43045c97e68d0d22db598dcb5ec23481c4e97718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-system-for-cross-domain-identity-management-tooautomatically-provision-users-and-groups-from-azure-active-directory-tooapplications"></a>Med hjälp av System för Identitetshantering i domänerna tooautomatically etablera användare och grupper från Azure Active Directory tooapplications

## <a name="overview"></a>Översikt
Azure Active Directory (AD Azure) automatiskt kan etablera användare och grupper tooany program eller identitet butik som är fronted av en webbtjänst med hello-gränssnitt som definierats i hello [System för domäner Identity Management (SCIM) 2.0 specifikation för protokollet](https://tools.ietf.org/html/draft-ietf-scim-api-19). Azure Active Directory kan skicka begäranden toocreate, ändra eller ta bort tilldelade användare och grupper toohello webbtjänsten. hello-webbtjänsten kan översätta dessa begäranden till åtgärder på hello mål identitet store. 

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln. 



![][0]
*Bild 1: Etablering från Azure Active Directory tooan identitet store via en webbtjänst*

Den här funktionen kan användas tillsammans med hello ”ta med din egen app” funktioner i Azure AD tooenable enkel inloggning och automatisk användaretablering för program som tillhandahåller eller fronted av en SCIM-webbtjänst.

Det finns två användningsfall för att använda SCIM i Azure Active Directory:

* **Etablering av användare och grupper tooapplications som stöder SCIM** program som stöder SCIM 2.0 och använder OAuth ägar-token för autentisering fungerar med Azure AD utan konfiguration.
* **Skapa din egen lösning för etablering för program som stöder andra API-baserad etablering** för icke-SCIM program, kan du skapa en SCIM slutpunkt tootranslate mellan hello Azure AD SCIM slutpunkten och eventuella API hello programmet stöder för användaretablering. toohelp som du utvecklar en slutpunkt för SCIM vi ger Common Language Infrastructure (CLI) bibliotek och kodexempel som visar hur toodo ger en SCIM slutpunkt och översätta SCIM meddelanden.  

## <a name="provisioning-users-and-groups-tooapplications-that-support-scim"></a>Etablering av användare och grupper tooapplications som stöder SCIM
Azure AD kan vara konfigurerade tooautomatically etablera tilldelade användare och grupper tooapplications som implementerar en [System för domäner Identity Management 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) webbtjänsten och acceptera OAuth ägar-token för autentisering . Inom hello SCIM 2.0-specifikationen måste program uppfylla följande villkor:

* Har stöd för att skapa användare och/eller grupper, enligt avsnittet 3.3 i hello SCIM-protokollet.  
* Har stöd för ändring av användare och/eller grupper med patch-förfrågningar enligt avsnitt 3.5.2 i hello SCIM-protokollet.  
* Har stöd för hämtning av en känd resurs enligt punkt 3.4.1 i hello SCIM protokoll.  
* Har stöd för frågor till användare och/eller grupper, enligt avsnittet 3.4.2 hello SCIM-protokollet.  Som standard tillfrågas användare av externalId och grupper är efterfrågas av displayName.  
* Har stöd för frågor till användaren genom ID och manager enligt punkt 3.4.2 i hello SCIM-protokollet.  
* Har stöd för frågor grupper efter ID och av medlem enligt punkt 3.4.2 i hello SCIM-protokollet.  
* Accepterar OAuth ägar-token för auktorisering enligt avsnittet 2.1 av hello SCIM-protokollet.

Kontrollera med leverantören av program eller program leverantörens dokumentation för rapporter för kompatibilitet med dessa krav.

### <a name="getting-started"></a>Komma igång
Program som stöder hello SCIM profil beskrivs i den här artikeln kan vara anslutna tooAzure Active Directory funktionen hello ”icke-galleriet programmet” hello Azure AD application Gallery. När ansluten, Azure AD körs synkroniseringsprocessen var tjugonde minut där frågor till hello programmets SCIM slutpunkt för tilldelade användare och grupper och skapar eller ändrar dem enligt toohello tilldelningsinformation.

**tooconnect ett program som stöder SCIM:**

1. Logga in för[hello Azure-portalen](https://portal.azure.com). 
2. Bläddra för ** Azure Active Directory > företagsprogram och välj **nytt program > alla > icke-galleriet programmet**.
3. Ange ett namn för ditt program och klicka på **Lägg till** ikonen toocreate ett app-objekt.
    
  ![][1]
  *Bild 2: Azure AD application gallery*
    
4. Välj hello i resulterande hello-skärmen **etablering** fliken i hello vänstra kolumnen.
5. I hello **etablering läge** väljer du **automatisk**.
    
  ![][2]
  *Bild 3: Konfigurera allokering i hello Azure-portalen*
    
6. I hello **klient URL** anger hello URL för hello programmets SCIM slutpunkt. Exempel: https://api.contoso.com/scim/v2/
7. Om hello SCIM slutpunkten kräver en OAuth ägar-token från en utfärdare än Azure AD och sedan kopiera hello krävs OAuth ägar-token i hello valfria **hemlighet Token** fältet. Om det här fältet är tomt, med en OAuth ägar-token som utfärdas från Azure AD med varje begäran med Azure AD. Appar som använder Azure AD som en identitetsleverantör kan verifiera den här Azure AD-utfärdade token.
8. Klicka på hello **Testanslutningen** knappen toohave Azure Active Directory försök tooconnect toohello SCIM slutpunkt. Om hello försök misslyckas, visas information om felet.  
9. Om det lyckas hello försök tooconnect toohello program, klicka sedan på **spara** toosave hello-administratörsautentiseringsuppgifter.
10. I hello **mappningar** avsnittet finns det två valbar uppsättningar attributmappning: en för användarobjekt och en för gruppobjekt. Markera varje en tooreview hello attribut som synkroniseras från Azure Active Directory tooyour app. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användare och grupper i din app för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.

    >[!NOTE]
    >Alternativt kan du inaktivera synkronisering av gruppobjekt genom att inaktivera hello ”grupper” mappning. 

11. Under **inställningar**, hello **omfång** fältet definierar vilka användare som är och eller grupper som ska synkroniseras. Att välja ”Sync endast har tilldelats användare och grupper” (rekommenderas) kommer bara synkronisera användare och grupper som tilldelas i hello **användare och grupper** fliken.
12. När konfigurationen är klar kan du ändra hello **Status för etablering** för**på**.
13. Klicka på **spara** toostart hello etablering Azure AD-tjänsten. 
14. Om synkroniserar endast tilldelas användare och grupper (rekommenderas), vara säker på att tooselect hello **användare och grupper** och tilldelar hello användare och/eller grupper som du vill toosync.

När hello inledande synkroniseringen har startat, kan du använda hello **granskningsloggar** fliken toomonitor pågår, som visar alla åtgärder som utförs av hello etableras på din app. Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

>[!NOTE]
>hello inledande synkronisering tar längre tid tooperform än efterföljande synkroniseringar som sker ungefär var tjugonde minut så länge hello-tjänsten körs. 


## <a name="building-your-own-provisioning-solution-for-any-application"></a>Skapa din egen lösning för etablering för alla program
Du kan aktivera enkel inloggning och automatisk användaretablering för nästan alla program som innehåller en REST- eller SOAP användaretablering API genom att skapa en SCIM-webbtjänst som samverkar med Azure Active Directory.

Här är hur det fungerar:

1. Azure AD innehåller ett vanligt språk infrastrukturbibliotek med namnet [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/). Systemintegrerare och utvecklare kan använda det här biblioteket toocreate och distribuera en SCIM-baserade webbtjänstslutpunkt kan ansluta Azure AD tooany programmets identitet store.
2. Mappningar implementeras i hello web service toomap hello standardiserade användaren schemat toohello användarschema och protokoll som krävs av hello program.
3. hello slutpunkts-URL är registrerad i Azure AD som en del av ett anpassat program i hello programgalleriet.
4. Användare och grupper är tilldelade toothis program i Azure AD. Vid tilldelning av placeras de i en kö toobe synkroniseras toohello målprogrammet. hello synkroniseringsprocessen hantering hello kön körs var tjugonde minut.

### <a name="code-samples"></a>Kodexempel
toomake detta bearbeta lättare, en uppsättning [kodexempel](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) tillhandahålls som skapar en slutpunkt för webbtjänsten SCIM och visa Automatisk etablering. Ett exempel är av en provider som underhåller en fil med rader av kommaavgränsade värden som representerar användare och grupper.  hello andra är av en provider som körs på hello Amazon Web Services identitets- och åtkomsthantering service.  

**Förutsättningar**

* Visual Studio 2013 eller senare
* [Azure SDK för .NET](https://azure.microsoft.com/downloads/)
* Windows-dator som har stöd för hello ASP.NET framework 4.5 toobe används som hello SCIM slutpunkt. Den här datorn måste vara tillgänglig från hello moln
* [En Azure-prenumeration med en utvärderingsversion eller licensierad version av Azure AD Premium](https://azure.microsoft.com/services/active-directory/)
* hello Amazon AWS urvalet kräver bibliotek från hello [AWS Toolkit för Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html). Mer information finns i hello viktigt fil som ingår i hello exempel.

### <a name="getting-started"></a>Komma igång
hello enklaste sättet tooimplement en SCIM slutpunkten som kan acceptera etablering begäranden från Azure AD är toobuild och distribuera hello-kodexempel som matar ut filen med hello etablerade användare tooa fil med kommaavgränsade värden (CSV).

**toocreate en exempel SCIM slutpunkt:**

1. Hämta hello exempel kodpaketet på [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2. Packa hello paketet och placera den på din Windows-dator på en plats, till exempel C:\AzureAD-BYOA-Provisioning-Samples\.
3. Starta hello FileProvisioningAgent lösningen i Visual Studio i den här mappen.
4. Välj **Verktyg > Library Package Manager > Package Manager-konsolen**, och kör följande kommandon för hello FileProvisioningAgent tooresolve hello lösning referenser hello:
  ```` 
   Install-Package Microsoft.SystemForCrossDomainIdentityManagement
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   Install-Package Microsoft.Owin.Diagnostics
   Install-Package Microsoft.Owin.Host.SystemWeb
  ````
5. Skapa hello FileProvisioningAgent projekt.
6. Starta hello kommandotolk program i Windows (som administratör) och använda hello **cd** kommandot toochange hello directory tooyour **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** mapp.
7. Kör hello följande kommando, ersätter < ip-adress > med hello IP-adress eller domännamn namnet hello Windows-dator:
  ````   
   FileAgnt.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. I under **Windows-inställningar > nätverk och Internet-inställningar**väljer hello **Windows-brandväggen > Avancerade inställningar**, och skapa en **inkommande regel** som tillåter inkommande åtkomst tooport 9000.
9. Om hello Windows-datorn är bakom en router, hello router behov toobe konfigurerats tooperform Network Access Translation mellan dess port 9000 som är exponerade toohello internet och port 9000 på hello Windows-dator. Detta krävs för Azure AD toobe kan tooaccess den här slutpunkten i hello molnet.

**tooregister hello exempel SCIM slutpunkt i Azure AD:**

1. Logga in för[hello Azure-portalen](https://portal.azure.com). 
2. Bläddra för ** Azure Active Directory > företagsprogram och välj **nytt program > alla > icke-galleriet programmet**.
3. Ange ett namn för ditt program och klicka på **Lägg till** ikonen toocreate ett app-objekt. hello programobjektet skapas är avsedda toorepresent hello mål app du vill etablera tooand implementera enkel inloggning för, och inte bara hello SCIM slutpunkt.
4. Välj hello i resulterande hello-skärmen **etablering** fliken i hello vänstra kolumnen.
5. I hello **etablering läge** väljer du **automatisk**.
    
  ![][2]
  *Bild 4: Konfigurera allokering i hello Azure-portalen*
    
6. I hello **klient URL** anger hello internet-exponerade URL och portnummer för för SCIM slutpunkten. Det är något som http://testmachine.contoso.com:9000 eller http://<ip-address>:9000/, där < ip-adress > är hello internet exponeras IP adress.  
7. Om hello SCIM slutpunkten kräver en OAuth ägar-token från en utfärdare än Azure AD och sedan kopiera hello krävs OAuth ägar-token i hello valfria **hemlighet Token** fältet. Om det här fältet är tomt, innehåller en OAuth ägar-token som utfärdas från Azure AD med varje begäran Azure AD. Appar som använder Azure AD som en identitetsleverantör kan verifiera den här Azure AD-utfärdade token.
8. Klicka på hello **Testanslutningen** knappen toohave Azure Active Directory försök tooconnect toohello SCIM slutpunkt. Om hello försök misslyckas, visas information om felet.  
9. Om det lyckas hello försök tooconnect toohello program, klicka sedan på **spara** toosave hello-administratörsautentiseringsuppgifter.
10. I hello **mappningar** avsnittet finns det två valbar uppsättningar attributmappning: en för användarobjekt och en för gruppobjekt. Markera varje en tooreview hello attribut som synkroniseras från Azure Active Directory tooyour app. Hej attribut som valts som **matchande** egenskaper är används toomatch hello användare och grupper i din app för uppdateringsåtgärder. Välj hello spara knappen toocommit ändringar.
11. Under **inställningar**, hello **omfång** fältet definierar vilka användare som är och eller grupper som ska synkroniseras. Att välja ”Sync endast har tilldelats användare och grupper” (rekommenderas) kommer bara synkronisera användare och grupper som tilldelas i hello **användare och grupper** fliken.
12. När konfigurationen är klar kan du ändra hello **Status för etablering** för**på**.
13. Klicka på **spara** toostart hello etablering Azure AD-tjänsten. 
14. Om synkroniserar endast tilldelas användare och grupper (rekommenderas), vara säker på att tooselect hello **användare och grupper** och tilldelar hello användare och/eller grupper som du vill toosync.

När hello inledande synkroniseringen har startat, kan du använda hello **granskningsloggar** fliken toomonitor pågår, som visar alla åtgärder som utförs av hello etableras på din app. Mer information om hur tooread hello Azure AD-etablering loggar finns [rapportering om automatisk konto användaretablering](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

hello sista steget vid verifiering av hello exempel är tooopen hello TargetFile.csv fil i hello \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug mapp på din Windows-dator. När hello etableringsprocessen körs med den här filen hello detaljer om allt tilldelade och etableras användare och grupper.

### <a name="development-libraries"></a>Utvecklingsbibliotek
toodevelop egna webbtjänst som överensstämmer med toohello SCIM-specifikationen först bekanta dig med hello efter bibliotek som föreskrivs av Microsoft toohelp påskynda hello utvecklingsprocessen: 

1. Common Language Infrastructure (CLI) bibliotek erbjuds för användning med språk som är baserat på infrastrukturen, till exempel C#. En av dessa bibliotek [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), deklarerar ett gränssnitt, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, som visas i följande illustration hello: A utvecklare som använder hello bibliotek skulle implementera gränssnittet med en klass som kan refereras till, Allmänt, som en provider. hello bibliotek aktivera hello developer toodeploy en webbtjänst som överensstämmer med toohello SCIM-specifikationen. hello-webbtjänsten kan finnas antingen i Internet Information Services eller alla körbara vanlig infrastruktur för språk-sammansättningen. Begäran översätts till anrop toohello providern metoder som skulle vara programmerad av hello developer toooperate på vissa Identitetslagret.
  
  ![][3]
  
2. [Express route-hanterare](http://expressjs.com/guide/routing.html) är tillgängliga för parsning av node.js begärandeobjekt som motsvarar anrop (som definieras av hello SCIM specification) tooa node.js-webbtjänsten.   

### <a name="building-a-custom-scim-endpoint"></a>Skapa en anpassad SCIM slutpunkt
Använda hello CLI bibliotek, värd utvecklare som använder dessa bibliotek sina tjänster i alla körbara vanlig infrastruktur för språk-sammansättningen eller i Internet Information Services. Här är exempelkod som värd för en tjänst i en körbar sammansättning på hello adressen http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Den här tjänsten måste ha en HTTP-adress och servern certifikat för serverautentisering för vilka hello rotcertifikatutfärdaren är en av följande hello: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Ett certifikat för serverautentisering kan vara bundna tooa port på en Windows-värd med hello network shell-verktyget: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

Här är hello-värdet som angetts för hello certhash argumentet hello certifikatets hello tumavtryck medan hello-värdet som angetts för hello appid argumentet är en godtycklig globalt unik identifierare.  

toohost hello-tjänsten i IIS, en utvecklare skulle skapa en CLA kod bibliotekssammansättning med en klass som heter Start i hello standardnamnområdet hello sammansättningen.  Här är ett exempel på sådan klass: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a>Hantering av slutpunkten autentisering
Begäranden från Azure Active Directory innehåller en OAuth 2.0-ägartoken.   Alla mottagande hello tjänstbegäran ska autentisera hello utfärdaren som Azure Active Directory uppdrag hello förväntades Azure Active Directory-klient för åtkomst toohello Azure Active Directory Graph-webbtjänsten.  I hello token identifieras hello utfärdaren av ett iss-anspråk som ”iss”: ”https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/”.  I det här exemplet hello basadressen för hello anspråksvärdet https://sts.windows.net, identifierar Azure Active Directory som hello utfärdare, medan hello relativ adress segment, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, är en unik identifierare för hello Azure Active Directory innehavaren för vilken hello token har utfärdats.  Om hello token har utfärdats för att komma åt hello Azure Active Directory Graph webbtjänsten sedan hello identifierare för att tjänsten bör 00000002-0000-0000-c000-000000000000, vara i hello värdet av hello-token eller anspråk.  

Utvecklare som använder hello CLA bibliotek som tillhandahålls av Microsoft för att skapa en SCIM-tjänst kan autentisera begäranden från Azure Active Directory med hello Microsoft.Owin.Security.ActiveDirectory paketet genom att följa dessa steg: 

1. I en provider, implementera hello Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior egenskapen genom att låta den returnera en metoden toobe anropas när hello-tjänsten har startats: 

  ````
    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }
  ````

2. Lägg till följande kod toothat metoden toohave hello alla begäran tooany hello-tjänstens slutpunkter autentiserad som försetts med en token som utfärdas av Azure Active Directory för en angiven klient för åtkomst toohello Azure AD Graph-webbtjänsten: 

  ````
    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute hello appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }
  ````


## <a name="user-and-group-schema"></a>Användar- och schema
Azure Active Directory kan etablera två typer av resurser tooSCIM webbtjänster.  Dessa typer av resurser är användare och grupper.  

Resurser för användare identifieras av hello schema-ID, urn: ietf:params:scim:schemas:extension:enterprise:2.0:User som ingår i den här protokollspecifikation: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  hello standardmappningen hello attribut för användare i Azure Active Directory toohello attribut för urn: ietf:params:scim:schemas:extension:enterprise:2.0:User resurser finns i tabell 1 nedan.  

Gruppera resurser identifieras av hello schemaidentifierare http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  Tabell 2 nedan visar hello standardmappningen hello attribut för resursgrupper i Azure Active Directory-attribut för toohello http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  

### <a name="table-1-default-user-attribute-mapping"></a>Tabell 1: Standard användaren attributmappning
| Azure Active Directory-användare | urn: ietf:params:scim:schemas:extension:enterprise:2.0:User |
| --- | --- |
| IsSoftDeleted |Aktiv |
| Visningsnamn |Visningsnamn |
| Fax TelephoneNumber |phoneNumbers typ eq ”fax” .value |
| givenName |name.givenName |
| Befattning |Rubrik |
| E-post |e-postmeddelanden typen eq ”arbete” .value |
| mailNickname |externalId |
| Manager |Manager |
| mobila |phoneNumbers typ eq ”mobil” .value |
| objekt-ID |id |
| Postnummer |adresser typen eq ”arbete” .postalCode |
| Proxy-adresser |e-postmeddelanden [Ange eq ”andra”]. Värdet |
| fysisk-leverans – OfficeName |[Ange eq ”andra”] adresser. Formaterad |
| StreetAddress |adresser typen eq ”arbete” .streetAddress |
| Efternamn |name.familyName |
| Telefonnummer |phoneNumbers typ eq ”arbete” .value |
| användaren huvudkontot |Användarnamn |

### <a name="table-2-default-group-attribute-mapping"></a>Tabell 2: Standard grupp attributmappning
| Azure Active Directory-grupp | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| --- | --- |
| Visningsnamn |externalId |
| E-post |e-postmeddelanden typen eq ”arbete” .value |
| mailNickname |Visningsnamn |
| Medlemmar |Medlemmar |
| objekt-ID |id |
| proxyAddresses |e-postmeddelanden [Ange eq ”andra”]. Värdet |

## <a name="user-provisioning-and-de-provisioning"></a>Användaretablering och avetablering
Följande bild visar hälsningsmeddelande att Azure Active Directory skickar tooa SCIM service toomanage hello livscykeln för en användare i en annan identitet store hello. hello diagram visar även hur en SCIM tjänst som implementeras med hjälp av hello CLI bibliotek som tillhandahålls av Microsoft för skapande av dessa tjänster översätta dessa begäranden till anrop toohello metoder för en provider.  

![][4]
*Bild 5: Användaretablering och avetablering sekvens*

1. Azure Active Directory-frågor hello tjänst för en användare med ett attributvärde för externalId matchar hello mailNickname attributvärde för en användare i Azure AD. hello frågan uttrycks som en begäran om Hypertext Transfer Protocol (HTTP) som det här exemplet där jyoung är ett exempel på en mailNickname för en användare i Azure Active Directory: 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  Om hello-tjänsten har skapats med hello vanlig infrastruktur för språket bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster, översätts hello begäran till en anropet toohello frågemetoden av hello-leverantör.  Här är hello signaturen för metoden: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);
  ````
  Här är hello definition av hello Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters gränssnitt: 
  ````
    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }

    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }
  ````
  I följande exempel på en fråga för en användare med ett angivet värde för hello externalId attributet hello, är värden hello-argument som skickas toohello frågemetoden: 
  * parametrar. AlternateFilters.Count: 1
  * parametrar. AlternateFilters.ElementAt(0). AttributePath: ”externalId”
  * parametrar. AlternateFilters.ElementAt(0). Jämförelseoperator: ComparisonOperator.Equals
  * parametrar. AlternateFilter.ElementAt(0). ComparisonValue: ”jyoung”
  * correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. Begärande-ID ”] 

2. Om hello svar tooa frågan toohello webbtjänst för en användare med ett externalId-attributvärde som matchar hello mailNickname attributvärde för en användare inte returnerar några användare begär Azure Active Directory som hello tillhandahållande en användare motsvarande toohello något i Azure Active Directory.  Här är ett exempel på en sådan begäran: 
  ````
    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}
  ````
  hello vanlig infrastruktur för språket bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster kan översätta begäran till en anropet toohello Create-metoden för hello-leverantör.  hello Create-metoden har den här signaturen: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  Hello-värdet för hello resurs argumentet är en instans av hello Microsoft.SystemForCrossDomainIdentityManagement i begäran-tooprovision en användare. Core2EnterpriseUser klass som definierats i hello Microsoft.SystemForCrossDomainIdentityManagement.Schemas-biblioteket.  Om hello begäran tooprovision hello användare lyckas, sedan är hello implementering av hello-metoden förväntade tooreturn en instans av hello Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser-klass med hello värdet för hello ID-egenskapen angetts toohello Unik identifierare för hello nyetablerade användare.  

3. tooupdate en känd tooexist i en identity-butik fronted genom en SCIM Azure Active Directory fortsätter genom att begära hello aktuell status för den användaren från hello-tjänsten med en begäran som användare: 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  I en tjänst som skapats med hello vanlig infrastruktur för språket bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster översätts hello-begäran till en anropet toohello hämta-metoden i hello-leverantör.  Här är hello signaturen för hello hämta-metoden: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);

    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }
  ````
  I en begäran tooretrieve hello aktuell status för en användare hello exempelvis är hello värdena för hello egenskaper för hello-objekt som angetts för hello värde för argumentet för hello-parametrar följande: 
  
  * ID: ”54D382A4-2050-4C03-94D1-E769F1D15682”
  * SchemaIdentifier: ”urn: ietf:params:scim:schemas:extension:enterprise:2.0:User”

4. Om ett referensattribut toobe uppdaterats sedan Azure Active Directory-frågor hello service toodetermine oavsett hello aktuella värdet för hello referensattribut i hello Identitetslagret fronted av matchar hello-tjänst redan hello värdet för attributet i Azure Active Directory. För användare är hello endast attribut för vilka hello efterfrågas aktuella värdet på det här sättet hello manager attribut. Här är ett exempel på en begäran toodetermine om hello manager attribut för en viss användare-objekt har ett visst värde: 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  Hej värdet för Frågeparametern för hello attribut-id, innebär det att om det finns ett användarobjekt som motsvarar hello uttryck som hello värdet för Frågeparametern för hello filter och sedan hello-tjänsten är förväntade toorespond med urn: ietf:params:scim:schemas: kärnor: 2.0:User eller urn: ietf:params:scim:schemas:extension:enterprise:2.0:User resurs, inklusive endast hello värdet för den resurs-id-attribut.  Hej värdet för hello **id** attributet är känd toohello begärande. Den ingår i hello värdet för Frågeparametern för hello filter; hello syftet med ber om den är faktiskt toorequest en minimal representation av en resurs som uppfyller hello filteruttrycket som en indikation på om huruvida varje sådan objekt finns.   

  Om hello-tjänsten har skapats med hello vanlig infrastruktur för språket bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster, översätts hello begäran till en anropet toohello frågemetoden av hello-leverantör. hello-värdet för hello egenskaper för hello-objekt som angetts för hello värde för argumentet för hello-parametrar är följande: 
  
  * parametrar. AlternateFilters.Count: 2
  * parametrar. AlternateFilters.ElementAt(x). AttributePath: ”id”
  * parametrar. AlternateFilters.ElementAt(x). Jämförelseoperator: ComparisonOperator.Equals
  * parametrar. AlternateFilter.ElementAt(x). ComparisonValue: ”54D382A4-2050-4C03-94D1-E769F1D15682”
  * parametrar. AlternateFilters.ElementAt(y). AttributePath: ”manager”
  * parametrar. AlternateFilters.ElementAt(y). Jämförelseoperator: ComparisonOperator.Equals
  * parametrar. AlternateFilter.ElementAt(y). ComparisonValue: ”2819c223-7f76-453a-919d-413861904646”
  * parametrar. RequestedAttributePaths.ElementAt(0): ”id”
  * parametrar. SchemaIdentifier: ”urn: ietf:params:scim:schemas:extension:enterprise:2.0:User”

  Här, hello index x hello värdet kan vara 0 hello värdet för hello index y kan vara 1, eller hello värdet för x kan vara 1 och hello värdet för y kan vara 0, beroende på hello ordning hello uttryck av hello filter frågeparameter.   

5. Här är ett exempel på en begäran från Azure Active Directory tooan SCIM service tooupdate en användare: 
  ````
    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
  ````
  hello infrastruktur för Microsoft Common Language bibliotek för att implementera SCIM tjänster kan översätta hello-begäran till en anropet toohello uppdateringsmetoden av hello-leverantör. Här är hello signaturen för hello metoden Update: 
  ````
    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }

    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }

      public string Value
      { get; set; }
    }
  ````
    I en begäran tooupdate användaren hello exempelvis har hello-objekt som angetts för hello värde för hello korrigering argumentet egenskapsvärdena: 
  
  * ResourceIdentifier.Identifier: ”54D382A4-2050-4C03-94D1-E769F1D15682”
  * ResourceIdentifier.SchemaIdentifier: ”urn: ietf:params:scim:schemas:extension:enterprise:2.0:User”
  * (PatchRequest som PatchRequest2). Operations.Count: 1
  * (PatchRequest som PatchRequest2). Operations.ElementAt(0). OperationName: OperationName.Add
  * (PatchRequest som PatchRequest2). Operations.ElementAt(0). Path.AttributePath: ”manager”
  * (PatchRequest som PatchRequest2). Operations.ElementAt(0). Value.Count: 1
  * (PatchRequest som PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Referens: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
  * (PatchRequest som PatchRequest2). Operations.ElementAt(0). Value.ElementAt(0). Värde: 2819c223-7f76-453a-919d-413861904646

6. toode etablera en användare från en butik identitet fronted av SCIM-tjänsten, Azure AD skickar en begäran som: 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  Om hello-tjänsten har skapats med hello vanlig infrastruktur för språket bibliotek som tillhandahålls av Microsoft för att implementera SCIM tjänster, översätts hello-begäran till en anropet toohello Delete-metoden för hello-leverantör.   Den här metoden har den här signaturen: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  hello-objekt som angetts för hello värde för hello resourceIdentifier argumentet har egenskapsvärdena i hello exempel på en begäran toode-etablera en användare: 
  
  * ResourceIdentifier.Identifier: ”54D382A4-2050-4C03-94D1-E769F1D15682”
  * ResourceIdentifier.SchemaIdentifier: ”urn: ietf:params:scim:schemas:extension:enterprise:2.0:User”

## <a name="group-provisioning-and-de-provisioning"></a>Gruppen etablering och avetablering
Följande bild visar hälsningsmeddelande att Azure AcD skickar tooa SCIM service toomanage hello livscykeln för en grupp i en annan identitet store hello.  Dessa meddelanden skiljer sig från hälsningsmeddelande som rör toousers på tre sätt: 

* hello schemat för en gruppresurs identifieras som http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.  
* Begäranden tooretrieve grupper fastställa hello medlemmar attributet är toobe uteslutas från alla resurser som finns i svaret toohello begäran.  
* Begäranden toodetermine om ett referensattribut har ett visst värde är begäranden om hello medlemmar attribut.  

![][5]
*Bild 6: Grupp etablering och avetablering sekvens*

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Automatisera användaren etablering/avetablering tooSaaS appar](active-directory-saas-app-provisioning.md)
* [Anpassa attributmappning för Användaretablering](active-directory-saas-customizing-attribute-mappings.md)
* [Skriva uttryck för attributmappning](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Omfångsfilter för Användaretablering](active-directory-saas-scoping-filters.md)
* [Kontot etablering meddelanden](active-directory-saas-account-provisioning-notifications.md)
* [Lista över självstudier om hur tooIntegrate SaaS-appar](active-directory-saas-tutorial-list.md)

<!--Image references-->
[0]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[1]: ./media/active-directory-scim-provisioning/scim-figure-2a.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2b.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
