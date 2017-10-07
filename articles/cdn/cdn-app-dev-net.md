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
# <a name="get-started-with-azure-cdn-development"></a><span data-ttu-id="aa2ed-103">Kom igång med Azure CDN-utveckling</span><span class="sxs-lookup"><span data-stu-id="aa2ed-103">Get started with Azure CDN development</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="aa2ed-104">Node.js</span><span class="sxs-lookup"><span data-stu-id="aa2ed-104">Node.js</span></span>](cdn-app-dev-node.md)
> * [<span data-ttu-id="aa2ed-105">.NET</span><span class="sxs-lookup"><span data-stu-id="aa2ed-105">.NET</span></span>](cdn-app-dev-net.md)
> 
> 

<span data-ttu-id="aa2ed-106">Du kan använda hello [Azure CDN-biblioteket för .NET](https://msdn.microsoft.com/library/mt657769.aspx) tooautomate skapande och hantering av CDN-profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-106">You can use hello [Azure CDN Library for .NET](https://msdn.microsoft.com/library/mt657769.aspx) tooautomate creation and management of CDN profiles and endpoints.</span></span>  <span data-ttu-id="aa2ed-107">Den här kursen går igenom hello skapandet av en enkel .NET-konsolprogram som visar flera hello tillgängliga åtgärder.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-107">This tutorial walks through hello creation of a simple .NET console application that demonstrates several of hello available operations.</span></span>  <span data-ttu-id="aa2ed-108">Den här kursen är inte avsedd toodescribe alla aspekter av hello Azure CDN-biblioteket för .NET i detalj.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-108">This tutorial is not intended toodescribe all aspects of hello Azure CDN Library for .NET in detail.</span></span>

<span data-ttu-id="aa2ed-109">Visual Studio 2015 toocomplete måste den här kursen.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-109">You need Visual Studio 2015 toocomplete this tutorial.</span></span>  <span data-ttu-id="aa2ed-110">[Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) är gratis.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-110">[Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) is freely available for download.</span></span>

> [!TIP]
> <span data-ttu-id="aa2ed-111">Hej [slutförda projekt från den här självstudiekursen](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) är tillgänglig för hämtning på MSDN.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-111">hello [completed project from this tutorial](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) is available for download on MSDN.</span></span>
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a><span data-ttu-id="aa2ed-112">Skapa projektet och Lägg till Nuget-paket</span><span class="sxs-lookup"><span data-stu-id="aa2ed-112">Create your project and add Nuget packages</span></span>
<span data-ttu-id="aa2ed-113">Nu när vi har skapat en resursgrupp för våra CDN-profiler och våra Azure AD behörighet toomanage CDN programprofiler och slutpunkter inom den gruppen, kan vi börja skapa vårt program.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-113">Now that we've created a resource group for our CDN profiles and given our Azure AD application permission toomanage CDN profiles and endpoints within that group, we can start creating our application.</span></span>

<span data-ttu-id="aa2ed-114">Klicka på i Visual Studio 2015 **filen**, **ny**, **projekt...**  tooopen hello-dialogrutan Nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-114">From within Visual Studio 2015, click **File**, **New**, **Project...** tooopen hello new project dialog.</span></span>  <span data-ttu-id="aa2ed-115">Expandera **Visual C#**och välj **Windows** hello i fönstret hello vänster.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-115">Expand **Visual C#**, then select **Windows** in hello pane on hello left.</span></span>  <span data-ttu-id="aa2ed-116">Klicka på **konsolprogram** i hello mittersta rutan.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-116">Click **Console Application** in hello center pane.</span></span>  <span data-ttu-id="aa2ed-117">Namnge projektet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-117">Name your project, then click **OK**.</span></span>  

![Nytt projekt](./media/cdn-app-dev-net/cdn-new-project.png)

<span data-ttu-id="aa2ed-119">Vår projektet kommer toouse vissa Azure-bibliotek finns i Nuget-paket.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-119">Our project is going toouse some Azure libraries contained in Nuget packages.</span></span>  <span data-ttu-id="aa2ed-120">Lägg till dessa toohello-projektet.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-120">Let's add those toohello project.</span></span>

1. <span data-ttu-id="aa2ed-121">Klicka på hello **verktyg** menyn **Nuget Package Manager**, sedan **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-121">Click hello **Tools** menu, **Nuget Package Manager**, then **Package Manager Console**.</span></span>
   
    ![Hantera Nuget-paket](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. <span data-ttu-id="aa2ed-123">Kör följande kommando tooinstall hello hello i hello Package Manager-konsolen, **Active Directory Authentication Library (ADAL)**:</span><span class="sxs-lookup"><span data-stu-id="aa2ed-123">In hello Package Manager Console, execute hello following command tooinstall hello **Active Directory Authentication Library (ADAL)**:</span></span>
   
    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. <span data-ttu-id="aa2ed-124">Kör följande tooinstall hello hello **Azure CDN bibliotek**:</span><span class="sxs-lookup"><span data-stu-id="aa2ed-124">Execute hello following tooinstall hello **Azure CDN Management Library**:</span></span>
   
    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a><span data-ttu-id="aa2ed-125">Direktiven, konstanter, main-metoden och hjälpmetoder</span><span class="sxs-lookup"><span data-stu-id="aa2ed-125">Directives, constants, main method, and helper methods</span></span>
<span data-ttu-id="aa2ed-126">Det är dags hello grundstrukturen för vårt program som har skrivits.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-126">Let's get hello basic structure of our program written.</span></span>

1. <span data-ttu-id="aa2ed-127">Ersätt hello hello Program.cs fliken i `using` direktiven överst hello med hello följande:</span><span class="sxs-lookup"><span data-stu-id="aa2ed-127">Back in hello Program.cs tab, replace hello `using` directives at hello top with hello following:</span></span>
   
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
2. <span data-ttu-id="aa2ed-128">Vi behöver toodefine vissa konstanter våra metoder ska använda.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-128">We need toodefine some constants our methods will use.</span></span>  <span data-ttu-id="aa2ed-129">I hello `Program` klass, men innan hello `Main` metoden Lägg till följande hello.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-129">In hello `Program` class, but before hello `Main` method, add hello following.</span></span>  <span data-ttu-id="aa2ed-130">Vara säker på att tooreplace hello platshållare, inklusive hello  **&lt;hakparenteser&gt;**, med egna värden efter behov.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-130">Be sure tooreplace hello placeholders, including hello **&lt;angle brackets&gt;**, with your own values as needed.</span></span>
   
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
3. <span data-ttu-id="aa2ed-131">Också definiera dessa två variabler på hello klassnivå.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-131">Also at hello class level, define these two variables.</span></span>  <span data-ttu-id="aa2ed-132">Vi använder dessa senare toodetermine om våra profil och en slutpunkt redan finns.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-132">We'll use these later toodetermine if our profile and endpoint already exist.</span></span>
   
    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. <span data-ttu-id="aa2ed-133">Ersätt hello `Main` metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="aa2ed-133">Replace hello `Main` method as follows:</span></span>
   
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
5. <span data-ttu-id="aa2ed-134">Några av våra andra metoder kommer tooprompt hello användare med ”Ja/Nej” frågor.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-134">Some of our other methods are going tooprompt hello user with "Yes/No" questions.</span></span>  <span data-ttu-id="aa2ed-135">Lägg till följande metod toomake hello som lite enklare:</span><span class="sxs-lookup"><span data-stu-id="aa2ed-135">Add hello following method toomake that a little easier:</span></span>
   
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

<span data-ttu-id="aa2ed-136">Nu när hello grundstrukturen för vårt program skrivs ska vi skapa hello metoder anropas av hello `Main` metod.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-136">Now that hello basic structure of our program is written, we should create hello methods called by hello `Main` method.</span></span>

## <a name="authentication"></a><span data-ttu-id="aa2ed-137">Autentisering</span><span class="sxs-lookup"><span data-stu-id="aa2ed-137">Authentication</span></span>
<span data-ttu-id="aa2ed-138">Innan vi kan använda hello Azure CDN bibliotek måste vi behöver tooauthenticate vår tjänst huvudnamn och hämta någon autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-138">Before we can use hello Azure CDN Management Library, we need tooauthenticate our service principal and obtain an authentication token.</span></span>  <span data-ttu-id="aa2ed-139">Den här metoden använder ADAL tooretrieve hello-token.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-139">This method uses ADAL tooretrieve hello token.</span></span>

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

<span data-ttu-id="aa2ed-140">Om du använder enskilda användarautentisering hello `GetAccessToken` metod ser lite annorlunda ut.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-140">If you are using individual user authentication, hello `GetAccessToken` method will look slightly different.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="aa2ed-141">Använd bara det här kodexemplet om du väljer toohave enskilda användarautentisering i stället för en tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-141">Only use this code sample if you are choosing toohave individual user authentication instead of a service principal.</span></span>
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

<span data-ttu-id="aa2ed-142">Vara säker på att tooreplace `<redirect URI>` med hello omdirigerings-URI som du angav när du registrerade hello program i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-142">Be sure tooreplace `<redirect URI>` with hello redirect URI you entered when you registered hello application in Azure AD.</span></span>

## <a name="list-cdn-profiles-and-endpoints"></a><span data-ttu-id="aa2ed-143">Lista CDN profiler och slutpunkter</span><span class="sxs-lookup"><span data-stu-id="aa2ed-143">List CDN profiles and endpoints</span></span>
<span data-ttu-id="aa2ed-144">Vi är nu redo tooperform CDN åtgärder.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-144">Now we're ready tooperform CDN operations.</span></span>  <span data-ttu-id="aa2ed-145">hello första våra webbmetoden är lista alla hello profiler och slutpunkter i vår resursgruppen och om den hittar en matchning för hello profil och slutpunkt namn anges i vår konstanter gör ned som för senare så att vi inte försöka toocreate dubbletter.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-145">hello first thing our method does is list all hello profiles and endpoints in our resource group, and if it finds a match for hello profile and endpoint names specified in our constants, makes a note of that for later so we don't try toocreate duplicates.</span></span>

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

## <a name="create-cdn-profiles-and-endpoints"></a><span data-ttu-id="aa2ed-146">Skapa CDN-profiler och slutpunkter</span><span class="sxs-lookup"><span data-stu-id="aa2ed-146">Create CDN profiles and endpoints</span></span>
<span data-ttu-id="aa2ed-147">Därefter skapar vi en profil.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-147">Next, we'll create a profile.</span></span>

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

<span data-ttu-id="aa2ed-148">När hello profilen har skapats, skapar vi en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-148">Once hello profile is created, we'll create an endpoint.</span></span>

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
> <span data-ttu-id="aa2ed-149">hello-exemplet ovan tilldelar hello endpoint ursprung med namnet *Contoso* med ett värdnamn `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-149">hello example above assigns hello endpoint an origin named *Contoso* with a hostname `www.contoso.com`.</span></span>  <span data-ttu-id="aa2ed-150">Du bör ändra den här toopoint tooyour egna ursprungets värdnamn.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-150">You should change this toopoint tooyour own origin's hostname.</span></span>
> 
> 

## <a name="purge-an-endpoint"></a><span data-ttu-id="aa2ed-151">Rensa en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="aa2ed-151">Purge an endpoint</span></span>
<span data-ttu-id="aa2ed-152">Under förutsättning att hello slutpunkten har skapats, en gemensam uppgift att vi kanske vill tooperform i vårt program Rensa hello innehållet i vår slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-152">Assuming hello endpoint has been created, one common task that we might want tooperform in our program is purging hello content in our endpoint.</span></span>

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
> <span data-ttu-id="aa2ed-153">Hej sträng i hello-exemplet ovan, `/*` anger att jag vill ha toopurge allt i hello rot hello sökväg för slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-153">In hello example above, hello string `/*` denotes that I want toopurge everything in hello root of hello endpoint path.</span></span>  <span data-ttu-id="aa2ed-154">Detta är likvärdiga toochecking **Rensa alla** i hello Azure portal ”Rensa” dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-154">This is equivalent toochecking **Purge All** in hello Azure portal's "purge" dialog.</span></span> <span data-ttu-id="aa2ed-155">I hello `CreateCdnProfile` -metoden I vår profil skapats som en **Azure CDN från Verizon** profilen med hello kod `Sku = new Sku(SkuName.StandardVerizon)`, så detta kommer att lyckas.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-155">In hello `CreateCdnProfile` method, I created our profile as an **Azure CDN from Verizon** profile using hello code `Sku = new Sku(SkuName.StandardVerizon)`, so this will be successful.</span></span>  <span data-ttu-id="aa2ed-156">Dock **Azure CDN från Akamai** profiler har inte stöd för **Rensa alla**, så om jag använde en Akamai profil för den här kursen behöver jag tooinclude specifika sökvägar toopurge.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-156">However, **Azure CDN from Akamai** profiles do not support **Purge All**, so if I was using an Akamai profile for this tutorial, I would need tooinclude specific paths toopurge.</span></span>
> 
> 

## <a name="delete-cdn-profiles-and-endpoints"></a><span data-ttu-id="aa2ed-157">Ta bort CDN-profiler och slutpunkter</span><span class="sxs-lookup"><span data-stu-id="aa2ed-157">Delete CDN profiles and endpoints</span></span>
<span data-ttu-id="aa2ed-158">hello senaste metoder tar bort våra slutpunkt och profil.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-158">hello last methods will delete our endpoint and profile.</span></span>

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

## <a name="running-hello-program"></a><span data-ttu-id="aa2ed-159">Hello-program som körs</span><span class="sxs-lookup"><span data-stu-id="aa2ed-159">Running hello program</span></span>
<span data-ttu-id="aa2ed-160">Vi kan nu kompilera och köra programmet hello genom att klicka på hello **starta** knappen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-160">We can now compile and run hello program by clicking hello **Start** button in Visual Studio.</span></span>

![Program som körs](./media/cdn-app-dev-net/cdn-program-running-1.png)

<span data-ttu-id="aa2ed-162">När programmet hello når hello ovan kommandotolk, bör du vara kan tooreturn tooyour resursgrupp i hello Azure-portalen och se att hello profilen har skapats.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-162">When hello program reaches hello above prompt, you should be able tooreturn tooyour resource group in hello Azure portal and see that hello profile has been created.</span></span>

![Lyckades!](./media/cdn-app-dev-net/cdn-success.png)

<span data-ttu-id="aa2ed-164">Vi kan sedan bekräfta hello prompter toorun hello resten av programmet hello.</span><span class="sxs-lookup"><span data-stu-id="aa2ed-164">We can then confirm hello prompts toorun hello rest of hello program.</span></span>

![Programmet Slutför](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a><span data-ttu-id="aa2ed-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aa2ed-166">Next Steps</span></span>
<span data-ttu-id="aa2ed-167">toosee hello slutförts projektet från den här genomgången [hämta hello exempel](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).</span><span class="sxs-lookup"><span data-stu-id="aa2ed-167">toosee hello completed project from this walkthrough, [download hello sample](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).</span></span>

<span data-ttu-id="aa2ed-168">toofind ytterligare dokumentation om hello Azure CDN bibliotek för .NET, visa hello [reference på MSDN](https://msdn.microsoft.com/library/mt657769.aspx).</span><span class="sxs-lookup"><span data-stu-id="aa2ed-168">toofind additional documentation on hello Azure CDN Management Library for .NET, view hello [reference on MSDN](https://msdn.microsoft.com/library/mt657769.aspx).</span></span>

<span data-ttu-id="aa2ed-169">Hantera dina CDN-resurser med [PowerShell](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="aa2ed-169">Manage your CDN resources with [PowerShell](cdn-manage-powershell.md).</span></span>

