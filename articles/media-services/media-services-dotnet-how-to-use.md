---
title: "aaaHow tooSet in datorn för Media Services-utveckling med .NET"
description: "Läs mer om hello krav för Media Services med hello Media Services SDK för .NET. Lär dig också hur toocreate en app i Visual Studio."
services: media-services
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.assetid: ec2804c7-c656-4fbf-b3e4-3f0f78599a7f
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2017
ms.author: juliako
ms.openlocfilehash: a5a2af3211d8156fd7dea99831fb769df4130d41
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="media-services-development-with-net"></a><span data-ttu-id="54161-104">Media Services-utveckling med .NET</span><span class="sxs-lookup"><span data-stu-id="54161-104">Media Services development with .NET</span></span>
[!INCLUDE [media-services-selector-setup](../../includes/media-services-selector-setup.md)]

<span data-ttu-id="54161-105">Det här avsnittet beskrivs hur toostart utveckla Media Services-program med hjälp av .NET.</span><span class="sxs-lookup"><span data-stu-id="54161-105">This topic discusses how toostart developing Media Services applications using .NET.</span></span>

<span data-ttu-id="54161-106">Hej **Azure Media Services .NET SDK** biblioteket kan du tooprogram mot Media Services med hjälp av .NET.</span><span class="sxs-lookup"><span data-stu-id="54161-106">hello **Azure Media Services .NET SDK** library enables you tooprogram against Media Services using .NET.</span></span> <span data-ttu-id="54161-107">det även enklare toodevelop med .NET, hello toomake **Azure Media Services .NET SDK-tilläggen** bibliotek har angetts.</span><span class="sxs-lookup"><span data-stu-id="54161-107">toomake it even easier toodevelop with .NET, hello **Azure Media Services .NET SDK Extensions** library is provided.</span></span> <span data-ttu-id="54161-108">Det här biblioteket innehåller en uppsättning tilläggsmetoder och hjälpfunktioner som förenklar .NET-kod.</span><span class="sxs-lookup"><span data-stu-id="54161-108">This library contains a set of extension methods and helper functions that simplify your .NET code.</span></span> <span data-ttu-id="54161-109">Båda bibliotek är tillgängligt via **NuGet** och **GitHub**.</span><span class="sxs-lookup"><span data-stu-id="54161-109">Both libraries are available through **NuGet** and **GitHub**.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54161-110">Krav</span><span class="sxs-lookup"><span data-stu-id="54161-110">Prerequisites</span></span>
* <span data-ttu-id="54161-111">Ett Media Services-konto i en ny eller befintlig Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="54161-111">A Media Services account in a new or existing Azure subscription.</span></span> <span data-ttu-id="54161-112">Avsnittet hello [hur tooCreate Media Services-konto](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="54161-112">See hello topic [How tooCreate a Media Services Account](media-services-portal-create-account.md).</span></span>
* <span data-ttu-id="54161-113">Operativsystem: Windows 10, Windows 7, Windows 2008 R2 eller Windows 8.</span><span class="sxs-lookup"><span data-stu-id="54161-113">Operating Systems: Windows 10, Windows 7, Windows 2008 R2, or Windows 8.</span></span>
* <span data-ttu-id="54161-114">.NET framework 4.5.</span><span class="sxs-lookup"><span data-stu-id="54161-114">.NET Framework 4.5.</span></span>
* <span data-ttu-id="54161-115">Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54161-115">Visual Studio.</span></span>

## <a name="create-and-configure-a-visual-studio-project"></a><span data-ttu-id="54161-116">Skapa och konfigurera ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="54161-116">Create and configure a Visual Studio project</span></span>
<span data-ttu-id="54161-117">Det här avsnittet beskrivs hur du toocreate ett projekt i Visual Studio och ställa in det för Media Services-utveckling.</span><span class="sxs-lookup"><span data-stu-id="54161-117">This section shows you how toocreate a project in Visual Studio and set it up for Media Services development.</span></span>  <span data-ttu-id="54161-118">I det här fallet hello projektet är ett Windows C#-konsolprogram, men hello samma konfigurationsstegen som visas här gäller tooother typer av projekt som du kan skapa för Media Services-program (till exempel en Windows Forms-program eller ASP.NET-webbprogram).</span><span class="sxs-lookup"><span data-stu-id="54161-118">In this case, hello project is a C# Windows console application, but hello same setup steps shown here apply tooother types of projects you can create for Media Services applications (for example, a Windows Forms application or an ASP.NET Web application).</span></span>

<span data-ttu-id="54161-119">Det här avsnittet visas hur toouse **NuGet** tooadd Media Services .NET SDK-tillägg och andra beroende bibliotek.</span><span class="sxs-lookup"><span data-stu-id="54161-119">This section shows how toouse **NuGet** tooadd Media Services .NET SDK extensions and other dependent libraries.</span></span>

<span data-ttu-id="54161-120">Alternativt kan du hämta hello senaste Media Services .NET SDK bits från GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) eller [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), skapa hello lösning och lägga till hello referenser toohello klientprojekt.</span><span class="sxs-lookup"><span data-stu-id="54161-120">Alternatively, you can get hello latest Media Services .NET SDK bits from GitHub ([github.com/Azure/azure-sdk-for-media-services](https://github.com/Azure/azure-sdk-for-media-services) or [github.com/Azure/azure-sdk-for-media-services-extensions](https://github.com/Azure/azure-sdk-for-media-services-extensions)), build hello solution, and add hello references toohello client project.</span></span> <span data-ttu-id="54161-121">Alla nödvändiga beroenden för hello hämta har hämtat och extraherat automatiskt.</span><span class="sxs-lookup"><span data-stu-id="54161-121">All hello necessary dependencies get downloaded and extracted automatically.</span></span>

1. <span data-ttu-id="54161-122">Skapa ett nytt C#-konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="54161-122">Create a new C# Console Application in Visual Studio.</span></span> <span data-ttu-id="54161-123">Ange hello **namn**, **plats**, och **lösningsnamn**, och klicka sedan på OK.</span><span class="sxs-lookup"><span data-stu-id="54161-123">Enter hello **Name**, **Location**, and **Solution name**, and then click OK.</span></span>
2. <span data-ttu-id="54161-124">Skapa hello lösning.</span><span class="sxs-lookup"><span data-stu-id="54161-124">Build hello solution.</span></span>
3. <span data-ttu-id="54161-125">Använd **NuGet** tooinstall och Lägg till **Azure Media Services .NET SDK-tilläggen** (**windowsazure.mediaservices.extensions**).</span><span class="sxs-lookup"><span data-stu-id="54161-125">Use **NuGet** tooinstall and add **Azure Media Services .NET SDK Extensions** (**windowsazure.mediaservices.extensions**).</span></span> <span data-ttu-id="54161-126">När du installerar det här paketet installeras även **Media Services .NET SDK** och lägger till alla andra nödvändiga beroenden.</span><span class="sxs-lookup"><span data-stu-id="54161-126">Installing this package, also installs **Media Services .NET SDK** and adds all other required dependencies.</span></span>
   
    <span data-ttu-id="54161-127">Se till att du har hello senaste versionen av NuGet installerad.</span><span class="sxs-lookup"><span data-stu-id="54161-127">Ensure that you have hello newest version of NuGet installed.</span></span> <span data-ttu-id="54161-128">Mer information och installationsanvisningar finns i [NuGet](http://nuget.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="54161-128">For more information and installation instructions, see [NuGet](http://nuget.codeplex.com/).</span></span>
4. <span data-ttu-id="54161-129">Högerklicka på hello namnet på hello projektet i Solution Explorer och välj Hantera NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="54161-129">In Solution Explorer, right-click hello name of hello project and choose Manage NuGet packages.</span></span>
   
    <span data-ttu-id="54161-130">hello dialogruta hantera NuGet-paket.</span><span class="sxs-lookup"><span data-stu-id="54161-130">hello Manage NuGet Packages dialog box appears.</span></span>
5. <span data-ttu-id="54161-131">Välj Azure Media Services .NET SDK-tilläggen hello Online Gallery och Sök efter Azure MediaServices tillägg, och klicka hello installation.</span><span class="sxs-lookup"><span data-stu-id="54161-131">In hello Online gallery, search for Azure MediaServices Extensions, choose Azure Media Services .NET SDK Extensions, and then click hello Install button.</span></span>
   
    <span data-ttu-id="54161-132">hello-projektet har ändrats och refererar till toohello Media Services .NET SDK-tillägg, Media Services .NET SDK och andra beroende sammansättningar har lagts till.</span><span class="sxs-lookup"><span data-stu-id="54161-132">hello project is modified and references toohello Media Services .NET SDK Extensions,  Media Services .NET SDK, and other dependent assemblies are added.</span></span>
6. <span data-ttu-id="54161-133">toopromote en tydligare utvecklingsmiljö överväga att aktivera NuGet-Paketåterställning.</span><span class="sxs-lookup"><span data-stu-id="54161-133">toopromote a cleaner development environment, consider enabling NuGet Package Restore.</span></span> <span data-ttu-id="54161-134">Mer information finns i [NuGet-Paketåterställning ”](http://docs.nuget.org/consume/package-restore).</span><span class="sxs-lookup"><span data-stu-id="54161-134">For more information, see [NuGet Package Restore"](http://docs.nuget.org/consume/package-restore).</span></span>
7. <span data-ttu-id="54161-135">Lägg till en referens för**System.Configuration** sammansättning.</span><span class="sxs-lookup"><span data-stu-id="54161-135">Add a reference too**System.Configuration** assembly.</span></span> <span data-ttu-id="54161-136">Den här sammansättningen innehåller hello System.Configuration. **ConfigurationManager** klassen som används tooaccess configuration-filer (till exempel App.config).</span><span class="sxs-lookup"><span data-stu-id="54161-136">This assembly contains hello System.Configuration.**ConfigurationManager** class that is used tooaccess configuration files (for example, App.config).</span></span>
   
    <span data-ttu-id="54161-137">tooadd referenser hello hanterar referenser dialogrutan, högerklicka på hello projektnamnet i hello Solution Explorer.</span><span class="sxs-lookup"><span data-stu-id="54161-137">tooadd references using hello Manage References dialog, right-click hello project name in hello Solution Explorer.</span></span> <span data-ttu-id="54161-138">Välj sedan Lägg till och referenser.</span><span class="sxs-lookup"><span data-stu-id="54161-138">Then, select Add and References.</span></span>
   
    <span data-ttu-id="54161-139">hello visas hanterar referenser.</span><span class="sxs-lookup"><span data-stu-id="54161-139">hello Manage References dialog appears.</span></span>
8. <span data-ttu-id="54161-140">Under .NET framework-sammansättningar, hitta Markera hello System.Configuration sammansättningen och tryck på OK.</span><span class="sxs-lookup"><span data-stu-id="54161-140">Under .NET framework assemblies, find and select hello System.Configuration assembly and press OK.</span></span>
9. <span data-ttu-id="54161-141">Öppna hello App.config-filen och Lägg till en *appSettings* toohello fil.</span><span class="sxs-lookup"><span data-stu-id="54161-141">Open hello App.config file and add an *appSettings* section toohello file.</span></span>     
   
    <span data-ttu-id="54161-142">Ange hello-värden som är nödvändiga tooconnect toohello Media Services API.</span><span class="sxs-lookup"><span data-stu-id="54161-142">Set hello values that are needed tooconnect toohello Media Services API.</span></span> <span data-ttu-id="54161-143">Mer information finns i [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="54161-143">For more information, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

    <span data-ttu-id="54161-144">Om du använder [användarautentisering](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) config-fil kommer förmodligen ha värden för din Azure AD-klient domän och hello AMS REST API-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="54161-144">If you are using [user authentication](media-services-use-aad-auth-to-access-ams-api.md#types-of-authentication) your config file will probably have values for your Azure AD tenant domain and hello AMS REST API endpoint.</span></span>
    
    >[!Important]
    ><span data-ttu-id="54161-145">De flesta kodexempel i hello Azure Media Services dokumentationen inställd och använda en användare (interaktiva) typ av autentisering tooconnect toohello AMS API.</span><span class="sxs-lookup"><span data-stu-id="54161-145">Most code samples in hello Azure Media Services documentation set, use a user (interactive) type of authentication tooconnect toohello AMS API.</span></span> <span data-ttu-id="54161-146">Den här autentiseringsmetoden fungerar bra för hantering och övervakning av interna appar: mobila appar, Windows-appar och konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="54161-146">This authentication method will work well for management or monitoring native apps: mobile apps, Windows apps, and Console applications.</span></span> <span data-ttu-id="54161-147">Den här autentiseringsmetoden är inte lämpligt för server, webbtjänster, API: er för typ av program.</span><span class="sxs-lookup"><span data-stu-id="54161-147">This authentication method is not suitable for server, web services, APIs type of applications.</span></span>  <span data-ttu-id="54161-148">Mer information finns i [åtkomst hello AMS API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="54161-148">For more information, see [Access hello AMS API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span>

        <configuration>
        ...
            <appSettings>
              <add key="AADTenantDomain" value="YourAADTenantDomain" />
              <add key="MediaServiceRESTAPIEndpoint" value="YourRESTAPIEndpoint" />
            </appSettings>

        </configuration>

10. <span data-ttu-id="54161-149">Skriv över befintliga hello **med** instruktioner hello början av hello Program.cs-filen med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="54161-149">Overwrite hello existing **using** statements at hello beginning of hello Program.cs file with hello following code.</span></span>
           
        using System;
        using System.Configuration;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
        using System.Threading;
        using System.Collections.Generic;
        using System.Linq;

<span data-ttu-id="54161-150">Nu är du redo toostart utvecklingen av ett Media Services-program.</span><span class="sxs-lookup"><span data-stu-id="54161-150">At this point, you are ready toostart developing a Media Services application.</span></span>    

## <a name="example"></a><span data-ttu-id="54161-151">Exempel</span><span class="sxs-lookup"><span data-stu-id="54161-151">Example</span></span>

<span data-ttu-id="54161-152">Här är en liten exempel som ansluter toohello AMS API och visar alla tillgängliga Media-processorer.</span><span class="sxs-lookup"><span data-stu-id="54161-152">Here is a small example that connects toohello AMS API and lists all available Media Processors.</span></span>
    
    class Program
    {
        // Read values from hello App.config file.
        private static readonly string _AADTenantDomain =
            ConfigurationManager.AppSettings["AADTenantDomain"];
        private static readonly string _RESTAPIEndpoint =
            ConfigurationManager.AppSettings["MediaServiceRESTAPIEndpoint"];
    
        private static CloudMediaContext _context = null;
        static void Main(string[] args)
        {
            var tokenCredentials = new AzureAdTokenCredentials(_AADTenantDomain, AzureEnvironments.AzureCloudEnvironment);
            var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
            _context = new CloudMediaContext(new Uri(_RESTAPIEndpoint), tokenProvider);
    
            // List all available Media Processors
            foreach (var mp in _context.MediaProcessors)
            {
                Console.WriteLine(mp.Name);
            }
    
        }

##<a name="next-steps"></a><span data-ttu-id="54161-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54161-153">Next steps</span></span>

<span data-ttu-id="54161-154">Nu [kan du ansluta toohello AMS API](media-services-use-aad-auth-to-access-ams-api.md) och starta [utveckla](media-services-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="54161-154">Now [you can connect toohello AMS API](media-services-use-aad-auth-to-access-ams-api.md) and start [developing](media-services-dotnet-get-started.md).</span></span>


## <a name="media-services-learning-paths"></a><span data-ttu-id="54161-155">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="54161-155">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="54161-156">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="54161-156">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

