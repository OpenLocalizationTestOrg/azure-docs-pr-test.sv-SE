---
title: "aaaGet igång med hello Azure CDN-biblioteket för .NET | Microsoft Docs"
description: "Lär dig hur toowrite .NET program toomanage Azure CDN med Visual Studio."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 63cf4101-92e7-49dd-a155-a90e54a792ca
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 9753e48c7469072cef6b2ac728e18c78121c97f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-cdn-development"></a>Kom igång med Azure CDN-utveckling
> [!div class="op_single_selector"]
> * [Node.js](cdn-app-dev-node.md)
> * [.NET](cdn-app-dev-net.md)
> 
> 

Du kan använda hello [Azure CDN-biblioteket för .NET](https://msdn.microsoft.com/library/mt657769.aspx) tooautomate skapande och hantering av CDN-profiler och slutpunkter.  Den här kursen går igenom hello skapandet av en enkel .NET-konsolprogram som visar flera hello tillgängliga åtgärder.  Den här kursen är inte avsedd toodescribe alla aspekter av hello Azure CDN-biblioteket för .NET i detalj.

Visual Studio 2015 toocomplete måste den här kursen.  [Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) är gratis.

> [!TIP]
> Hej [slutförda projekt från den här självstudiekursen](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) är tillgänglig för hämtning på MSDN.
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Skapa projektet och Lägg till Nuget-paket
Nu när vi har skapat en resursgrupp för våra CDN-profiler och våra Azure AD behörighet toomanage CDN programprofiler och slutpunkter inom den gruppen, kan vi börja skapa vårt program.

Klicka på i Visual Studio 2015 **filen**, **ny**, **projekt...**  tooopen hello-dialogrutan Nytt projekt.  Expandera **Visual C#**och välj **Windows** hello i fönstret hello vänster.  Klicka på **konsolprogram** i hello mittersta rutan.  Namnge projektet och klicka sedan på **OK**.  

![Nytt projekt](./media/cdn-app-dev-net/cdn-new-project.png)

Vår projektet kommer toouse vissa Azure-bibliotek finns i Nuget-paket.  Lägg till dessa toohello-projektet.

1. Klicka på hello **verktyg** menyn **Nuget Package Manager**, sedan **Pakethanterarkonsolen**.
   
    ![Hantera Nuget-paket](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. Kör följande kommando tooinstall hello hello i hello Package Manager-konsolen, **Active Directory Authentication Library (ADAL)**:
   
    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. Kör följande tooinstall hello hello **Azure CDN bibliotek**:
   
    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Direktiven, konstanter, main-metoden och hjälpmetoder
Det är dags hello grundstrukturen för vårt program som har skrivits.

1. Ersätt hello hello Program.cs fliken i `using` direktiven överst hello med hello följande:
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```
2. Vi behöver toodefine vissa konstanter våra metoder ska använda.  I hello `Program` klass, men innan hello `Main` metoden Lägg till följande hello.  Vara säker på att tooreplace hello platshållare, inklusive hello  **&lt;hakparenteser&gt;**, med egna värden efter behov.
   
    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";
   
    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```
3. Också definiera dessa två variabler på hello klassnivå.  Vi använder dessa senare toodetermine om våra profil och en slutpunkt redan finns.
   
    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. Ersätt hello `Main` metoden på följande sätt:
   
   ```csharp
   static void Main(string[] args)
   {
       //Get a token
       AuthenticationResult authResult = GetAccessToken();
   
       // Create CDN client
       CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
           { SubscriptionId = subscriptionId };
   
       ListProfilesAndEndpoints(cdn);
   
       // Create CDN Profile
       CreateCdnProfile(cdn);
   
       // Create CDN Endpoint
       CreateCdnEndpoint(cdn);
   
       Console.WriteLine();
   
       // Purge CDN Endpoint
       PromptPurgeCdnEndpoint(cdn);
   
       // Delete CDN Endpoint
       PromptDeleteCdnEndpoint(cdn);
   
       // Delete CDN Profile
       PromptDeleteCdnProfile(cdn);
   
       Console.WriteLine("Press Enter tooend program.");
       Console.ReadLine();
   }
   ```
5. Några av våra andra metoder kommer tooprompt hello användare med ”Ja/Nej” frågor.  Lägg till följande metod toomake hello som lite enklare:
   
    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Nu när hello grundstrukturen för vårt program skrivs ska vi skapa hello metoder anropas av hello `Main` metod.

## <a name="authentication"></a>Autentisering
Innan vi kan använda hello Azure CDN bibliotek måste vi behöver tooauthenticate vår tjänst huvudnamn och hämta någon autentiseringstoken.  Den här metoden använder ADAL tooretrieve hello-token.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Om du använder enskilda användarautentisering hello `GetAccessToken` metod ser lite annorlunda ut.

> [!IMPORTANT]
> Använd bara det här kodexemplet om du väljer toohave enskilda användarautentisering i stället för en tjänstens huvudnamn.
> 
> 

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Vara säker på att tooreplace `<redirect URI>` med hello omdirigerings-URI som du angav när du registrerade hello program i Azure AD.

## <a name="list-cdn-profiles-and-endpoints"></a>Lista CDN profiler och slutpunkter
Vi är nu redo tooperform CDN åtgärder.  hello första våra webbmetoden är lista alla hello profiler och slutpunkter i vår resursgruppen och om den hittar en matchning för hello profil och slutpunkt namn anges i vår konstanter gör ned som för senare så att vi inte försöka toocreate dubbletter.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all hello CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's hello name of hello CDN profile we want toocreate!
            profileAlreadyExists = true;
        }

        //List all hello CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // hello unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Skapa CDN-profiler och slutpunkter
Därefter skapar vi en profil.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

När hello profilen har skapats, skapar vi en slutpunkt.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

> [!NOTE]
> hello-exemplet ovan tilldelar hello endpoint ursprung med namnet *Contoso* med ett värdnamn `www.contoso.com`.  Du bör ändra den här toopoint tooyour egna ursprungets värdnamn.
> 
> 

## <a name="purge-an-endpoint"></a>Rensa en slutpunkt
Under förutsättning att hello slutpunkten har skapats, en gemensam uppgift att vi kanske vill tooperform i vårt program Rensa hello innehållet i vår slutpunkt.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

> [!NOTE]
> Hej sträng i hello-exemplet ovan, `/*` anger att jag vill ha toopurge allt i hello rot hello sökväg för slutpunkt.  Detta är likvärdiga toochecking **Rensa alla** i hello Azure portal ”Rensa” dialogrutan. I hello `CreateCdnProfile` -metoden I vår profil skapats som en **Azure CDN från Verizon** profilen med hello kod `Sku = new Sku(SkuName.StandardVerizon)`, så detta kommer att lyckas.  Dock **Azure CDN från Akamai** profiler har inte stöd för **Rensa alla**, så om jag använde en Akamai profil för den här kursen behöver jag tooinclude specifika sökvägar toopurge.
> 
> 

## <a name="delete-cdn-profiles-and-endpoints"></a>Ta bort CDN-profiler och slutpunkter
hello senaste metoder tar bort våra slutpunkt och profil.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-hello-program"></a>Hello-program som körs
Vi kan nu kompilera och köra programmet hello genom att klicka på hello **starta** knappen i Visual Studio.

![Program som körs](./media/cdn-app-dev-net/cdn-program-running-1.png)

När programmet hello når hello ovan kommandotolk, bör du vara kan tooreturn tooyour resursgrupp i hello Azure-portalen och se att hello profilen har skapats.

![Lyckades!](./media/cdn-app-dev-net/cdn-success.png)

Vi kan sedan bekräfta hello prompter toorun hello resten av programmet hello.

![Programmet Slutför](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Nästa steg
toosee hello slutförts projektet från den här genomgången [hämta hello exempel](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

toofind ytterligare dokumentation om hello Azure CDN bibliotek för .NET, visa hello [reference på MSDN](https://msdn.microsoft.com/library/mt657769.aspx).

Hantera dina CDN-resurser med [PowerShell](cdn-manage-powershell.md).

