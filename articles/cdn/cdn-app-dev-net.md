---
title: "Kom igång med Azure CDN-biblioteket för .NET | Microsoft Docs"
description: "Lär dig hur du skriver .NET-program för att hantera Azure CDN med Visual Studio."
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
ms.openlocfilehash: 5379586355ece98af6295236d6cbd09cb31c742b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-cdn-development"></a><span data-ttu-id="76249-103">Kom igång med Azure CDN-utveckling</span><span class="sxs-lookup"><span data-stu-id="76249-103">Get started with Azure CDN development</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="76249-104">Node.js</span><span class="sxs-lookup"><span data-stu-id="76249-104">Node.js</span></span>](cdn-app-dev-node.md)
> * [<span data-ttu-id="76249-105">.NET</span><span class="sxs-lookup"><span data-stu-id="76249-105">.NET</span></span>](cdn-app-dev-net.md)
> 
> 

<span data-ttu-id="76249-106">Du kan använda den [Azure CDN-biblioteket för .NET](https://msdn.microsoft.com/library/mt657769.aspx) att automatisera skapande och hantering av CDN-profiler och slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="76249-106">You can use the [Azure CDN Library for .NET](https://msdn.microsoft.com/library/mt657769.aspx) to automate creation and management of CDN profiles and endpoints.</span></span>  <span data-ttu-id="76249-107">Den här vägledningen visar genom att skapa en enkel .NET-konsolprogram som visar flera av de tillgängliga åtgärderna.</span><span class="sxs-lookup"><span data-stu-id="76249-107">This tutorial walks through the creation of a simple .NET console application that demonstrates several of the available operations.</span></span>  <span data-ttu-id="76249-108">Den här kursen är inte avsedd att beskriva alla aspekter av Azure CDN-biblioteket för .NET i detalj.</span><span class="sxs-lookup"><span data-stu-id="76249-108">This tutorial is not intended to describe all aspects of the Azure CDN Library for .NET in detail.</span></span>

<span data-ttu-id="76249-109">Du måste Visual Studio 2015 att slutföra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="76249-109">You need Visual Studio 2015 to complete this tutorial.</span></span>  <span data-ttu-id="76249-110">[Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) är gratis.</span><span class="sxs-lookup"><span data-stu-id="76249-110">[Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) is freely available for download.</span></span>

> [!TIP]
> <span data-ttu-id="76249-111">Den [slutförda projekt från den här självstudiekursen](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) är tillgänglig för hämtning på MSDN.</span><span class="sxs-lookup"><span data-stu-id="76249-111">The [completed project from this tutorial](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) is available for download on MSDN.</span></span>
> 
> 

[!INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a><span data-ttu-id="76249-112">Skapa projektet och Lägg till Nuget-paket</span><span class="sxs-lookup"><span data-stu-id="76249-112">Create your project and add Nuget packages</span></span>
<span data-ttu-id="76249-113">Nu när vi har skapat en resursgrupp för våra CDN-profiler och våra Azure AD-programmet behörighet att hantera CDN profiler och slutpunkter inom den gruppen, kan vi börja skapa vårt program.</span><span class="sxs-lookup"><span data-stu-id="76249-113">Now that we've created a resource group for our CDN profiles and given our Azure AD application permission to manage CDN profiles and endpoints within that group, we can start creating our application.</span></span>

<span data-ttu-id="76249-114">Klicka på i Visual Studio 2015 **filen**, **ny**, **projekt...**  att öppna dialogrutan Nytt projekt.</span><span class="sxs-lookup"><span data-stu-id="76249-114">From within Visual Studio 2015, click **File**, **New**, **Project...** to open the new project dialog.</span></span>  <span data-ttu-id="76249-115">Expandera **Visual C#**och välj **Windows** i rutan till vänster.</span><span class="sxs-lookup"><span data-stu-id="76249-115">Expand **Visual C#**, then select **Windows** in the pane on the left.</span></span>  <span data-ttu-id="76249-116">Klicka på **konsolprogram** i mitten av fönstret.</span><span class="sxs-lookup"><span data-stu-id="76249-116">Click **Console Application** in the center pane.</span></span>  <span data-ttu-id="76249-117">Namnge projektet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="76249-117">Name your project, then click **OK**.</span></span>  

![Nytt projekt](./media/cdn-app-dev-net/cdn-new-project.png)

<span data-ttu-id="76249-119">Projektet kommer att använda vissa Azure bibliotek i Nuget-paket.</span><span class="sxs-lookup"><span data-stu-id="76249-119">Our project is going to use some Azure libraries contained in Nuget packages.</span></span>  <span data-ttu-id="76249-120">Lägg till dem i projektet.</span><span class="sxs-lookup"><span data-stu-id="76249-120">Let's add those to the project.</span></span>

1. <span data-ttu-id="76249-121">Klicka på den **verktyg** menyn **Nuget Package Manager**, sedan **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="76249-121">Click the **Tools** menu, **Nuget Package Manager**, then **Package Manager Console**.</span></span>
   
    ![Hantera Nuget-paket](./media/cdn-app-dev-net/cdn-manage-nuget.png)
2. <span data-ttu-id="76249-123">I Package Manager-konsolen kör du följande kommando för att installera den **Active Directory Authentication Library (ADAL)**:</span><span class="sxs-lookup"><span data-stu-id="76249-123">In the Package Manager Console, execute the following command to install the **Active Directory Authentication Library (ADAL)**:</span></span>
   
    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`
3. <span data-ttu-id="76249-124">Kör följande för att installera den **Azure CDN bibliotek**:</span><span class="sxs-lookup"><span data-stu-id="76249-124">Execute the following to install the **Azure CDN Management Library**:</span></span>
   
    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a><span data-ttu-id="76249-125">Direktiven, konstanter, main-metoden och hjälpmetoder</span><span class="sxs-lookup"><span data-stu-id="76249-125">Directives, constants, main method, and helper methods</span></span>
<span data-ttu-id="76249-126">Det är dags grundstrukturen för vårt program som har skrivits.</span><span class="sxs-lookup"><span data-stu-id="76249-126">Let's get the basic structure of our program written.</span></span>

1. <span data-ttu-id="76249-127">Fliken Program.cs i ersätter den `using` direktiven överst med följande:</span><span class="sxs-lookup"><span data-stu-id="76249-127">Back in the Program.cs tab, replace the `using` directives at the top with the following:</span></span>
   
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
2. <span data-ttu-id="76249-128">Vi behöver definiera vissa konstanter våra metoder ska använda.</span><span class="sxs-lookup"><span data-stu-id="76249-128">We need to define some constants our methods will use.</span></span>  <span data-ttu-id="76249-129">I den `Program` class, men innan den `Main` metoden Lägg till följande.</span><span class="sxs-lookup"><span data-stu-id="76249-129">In the `Program` class, but before the `Main` method, add the following.</span></span>  <span data-ttu-id="76249-130">Se till att ersätta platshållare, inklusive den  **&lt;hakparenteser&gt;**, med egna värden efter behov.</span><span class="sxs-lookup"><span data-stu-id="76249-130">Be sure to replace the placeholders, including the **&lt;angle brackets&gt;**, with your own values as needed.</span></span>
   
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
3. <span data-ttu-id="76249-131">Också på klassnivå, definiera dessa två variabler.</span><span class="sxs-lookup"><span data-stu-id="76249-131">Also at the class level, define these two variables.</span></span>  <span data-ttu-id="76249-132">Vi använder dessa senare för att avgöra om våra profil och en slutpunkt redan finns.</span><span class="sxs-lookup"><span data-stu-id="76249-132">We'll use these later to determine if our profile and endpoint already exist.</span></span>
   
    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```
4. <span data-ttu-id="76249-133">Ersätt den `Main` metoden på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="76249-133">Replace the `Main` method as follows:</span></span>
   
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
   
       Console.WriteLine("Press Enter to end program.");
       Console.ReadLine();
   }
   ```
5. <span data-ttu-id="76249-134">Några av våra andra metoder kommer att uppmana användaren med frågor som ”Ja/Nej”.</span><span class="sxs-lookup"><span data-stu-id="76249-134">Some of our other methods are going to prompt the user with "Yes/No" questions.</span></span>  <span data-ttu-id="76249-135">Lägg till följande metod för att se att lite enklare:</span><span class="sxs-lookup"><span data-stu-id="76249-135">Add the following method to make that a little easier:</span></span>
   
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

<span data-ttu-id="76249-136">Nu när den grundläggande strukturen i vårt program skrivs ska vi skapa metoderna anropas av den `Main` metoden.</span><span class="sxs-lookup"><span data-stu-id="76249-136">Now that the basic structure of our program is written, we should create the methods called by the `Main` method.</span></span>

## <a name="authentication"></a><span data-ttu-id="76249-137">Autentisering</span><span class="sxs-lookup"><span data-stu-id="76249-137">Authentication</span></span>
<span data-ttu-id="76249-138">Innan vi kan använda Azure CDN-hanteringsmodul behöver vi verifiera vår huvudnamn för tjänsten och hämta någon autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="76249-138">Before we can use the Azure CDN Management Library, we need to authenticate our service principal and obtain an authentication token.</span></span>  <span data-ttu-id="76249-139">Den här metoden använder ADAL för att hämta token.</span><span class="sxs-lookup"><span data-stu-id="76249-139">This method uses ADAL to retrieve the token.</span></span>

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

<span data-ttu-id="76249-140">Om du använder autentisering av enskilda användare, den `GetAccessToken` metoden ser lite annorlunda ut.</span><span class="sxs-lookup"><span data-stu-id="76249-140">If you are using individual user authentication, the `GetAccessToken` method will look slightly different.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="76249-141">Använd bara det här kodexemplet om du väljer att ha enskilda användarautentisering i stället för en tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="76249-141">Only use this code sample if you are choosing to have individual user authentication instead of a service principal.</span></span>
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

<span data-ttu-id="76249-142">Se till att ersätta `<redirect URI>` med omdirigerings-URI som du angav när du registrerade programmet i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="76249-142">Be sure to replace `<redirect URI>` with the redirect URI you entered when you registered the application in Azure AD.</span></span>

## <a name="list-cdn-profiles-and-endpoints"></a><span data-ttu-id="76249-143">Lista CDN profiler och slutpunkter</span><span class="sxs-lookup"><span data-stu-id="76249-143">List CDN profiles and endpoints</span></span>
<span data-ttu-id="76249-144">Vi är nu redo att utföra åtgärder för CDN.</span><span class="sxs-lookup"><span data-stu-id="76249-144">Now we're ready to perform CDN operations.</span></span>  <span data-ttu-id="76249-145">Det första våra webbmetoden är lista alla profiler och slutpunkter i vår resursgruppen och om den hittar en matchning för namnen på profil och slutpunkt har angetts i vår konstanter gör du anteckna som för senare så att vi inte att skapa dubbletter.</span><span class="sxs-lookup"><span data-stu-id="76249-145">The first thing our method does is list all the profiles and endpoints in our resource group, and if it finds a match for the profile and endpoint names specified in our constants, makes a note of that for later so we don't try to create duplicates.</span></span>

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a><span data-ttu-id="76249-146">Skapa CDN-profiler och slutpunkter</span><span class="sxs-lookup"><span data-stu-id="76249-146">Create CDN profiles and endpoints</span></span>
<span data-ttu-id="76249-147">Därefter skapar vi en profil.</span><span class="sxs-lookup"><span data-stu-id="76249-147">Next, we'll create a profile.</span></span>

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

<span data-ttu-id="76249-148">När profilen har skapats, skapar vi en slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="76249-148">Once the profile is created, we'll create an endpoint.</span></span>

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
> <span data-ttu-id="76249-149">Exemplet ovan tilldelar slutpunkten ursprung med namnet *Contoso* med ett värdnamn `www.contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="76249-149">The example above assigns the endpoint an origin named *Contoso* with a hostname `www.contoso.com`.</span></span>  <span data-ttu-id="76249-150">Du bör ändra så att den pekar till dina egna ursprungsvärdnamn.</span><span class="sxs-lookup"><span data-stu-id="76249-150">You should change this to point to your own origin's hostname.</span></span>
> 
> 

## <a name="purge-an-endpoint"></a><span data-ttu-id="76249-151">Rensa en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="76249-151">Purge an endpoint</span></span>
<span data-ttu-id="76249-152">Under förutsättning att slutpunkten har skapats, en gemensam uppgift som vi kan utföra i vårt program rensa innehållet i vår slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="76249-152">Assuming the endpoint has been created, one common task that we might want to perform in our program is purging the content in our endpoint.</span></span>

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
> <span data-ttu-id="76249-153">I exemplet ovan, strängen `/*` anger att jag vill rensa allt i roten av sökväg för slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="76249-153">In the example above, the string `/*` denotes that I want to purge everything in the root of the endpoint path.</span></span>  <span data-ttu-id="76249-154">Detta motsvarar kontrollerar **Rensa alla** i Azure portal ”Rensa” dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="76249-154">This is equivalent to checking **Purge All** in the Azure portal's "purge" dialog.</span></span> <span data-ttu-id="76249-155">I den `CreateCdnProfile` -metoden I vår profil skapats som en **Azure CDN från Verizon** profilen genom att använda koden `Sku = new Sku(SkuName.StandardVerizon)`, så detta kommer att lyckas.</span><span class="sxs-lookup"><span data-stu-id="76249-155">In the `CreateCdnProfile` method, I created our profile as an **Azure CDN from Verizon** profile using the code `Sku = new Sku(SkuName.StandardVerizon)`, so this will be successful.</span></span>  <span data-ttu-id="76249-156">Dock **Azure CDN från Akamai** profiler har inte stöd för **Rensa alla**, så om jag använde en Akamai profil för den här självstudiekursen jag måste du inkludera specifika sökvägar för att rensa.</span><span class="sxs-lookup"><span data-stu-id="76249-156">However, **Azure CDN from Akamai** profiles do not support **Purge All**, so if I was using an Akamai profile for this tutorial, I would need to include specific paths to purge.</span></span>
> 
> 

## <a name="delete-cdn-profiles-and-endpoints"></a><span data-ttu-id="76249-157">Ta bort CDN-profiler och slutpunkter</span><span class="sxs-lookup"><span data-stu-id="76249-157">Delete CDN profiles and endpoints</span></span>
<span data-ttu-id="76249-158">Senaste metoderna tar bort våra slutpunkt och profil.</span><span class="sxs-lookup"><span data-stu-id="76249-158">The last methods will delete our endpoint and profile.</span></span>

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

## <a name="running-the-program"></a><span data-ttu-id="76249-159">Programmet körs</span><span class="sxs-lookup"><span data-stu-id="76249-159">Running the program</span></span>
<span data-ttu-id="76249-160">Vi kan nu kompilera och köra programmet genom att klicka på den **starta** knappen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="76249-160">We can now compile and run the program by clicking the **Start** button in Visual Studio.</span></span>

![Program som körs](./media/cdn-app-dev-net/cdn-program-running-1.png)

<span data-ttu-id="76249-162">När programmet har nått ovan uppmaningen, bör du kunna återgå till din resursgrupp i Azure portal och se att profilen har skapats.</span><span class="sxs-lookup"><span data-stu-id="76249-162">When the program reaches the above prompt, you should be able to return to your resource group in the Azure portal and see that the profile has been created.</span></span>

![Lyckades!](./media/cdn-app-dev-net/cdn-success.png)

<span data-ttu-id="76249-164">Vi kan sedan bekräfta anvisningarna för att köra resten av programmet.</span><span class="sxs-lookup"><span data-stu-id="76249-164">We can then confirm the prompts to run the rest of the program.</span></span>

![Programmet Slutför](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a><span data-ttu-id="76249-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="76249-166">Next Steps</span></span>
<span data-ttu-id="76249-167">Se slutförda projektet från den här genomgången [hämta exempelfilerna](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).</span><span class="sxs-lookup"><span data-stu-id="76249-167">To see the completed project from this walkthrough, [download the sample](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).</span></span>

<span data-ttu-id="76249-168">Ytterligare dokumentation om Azure CDN Management biblioteket för .NET visar den [reference på MSDN](https://msdn.microsoft.com/library/mt657769.aspx).</span><span class="sxs-lookup"><span data-stu-id="76249-168">To find additional documentation on the Azure CDN Management Library for .NET, view the [reference on MSDN](https://msdn.microsoft.com/library/mt657769.aspx).</span></span>

<span data-ttu-id="76249-169">Hantera dina CDN-resurser med [PowerShell](cdn-manage-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="76249-169">Manage your CDN resources with [PowerShell](cdn-manage-powershell.md).</span></span>

