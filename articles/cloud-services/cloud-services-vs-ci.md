---
title: aaaContinuous levereras cloud services med Visual Studio Online | Microsoft Docs
description: "Lär dig hur tooset in kontinuerlig leverans för Azure cloud appar utan att spara diagnostik lagring viktiga toohello service configuration-filer"
services: cloud-services
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 148b2959-c5db-4e4a-a7e9-fccb252e7e8a
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: cawa
ms.openlocfilehash: dc87d049e46daf8b8a26ee4450ebd9b7910f287b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="securely-save-cloud-services-diagnostics-storage-key-and-setup-continuous-integration-and-deployment-tooazure-using-visual-studio-online"></a><span data-ttu-id="3798f-103">Securely spara Cloud Services Diagnostics Lagringsnyckel och installationsprogrammet kontinuerlig integrering och distribution tooAzure med hjälp av Visual Studio Online</span><span class="sxs-lookup"><span data-stu-id="3798f-103">Securely Save Cloud Services Diagnostics Storage Key and Setup Continuous Integration and Deployment tooAzure using Visual Studio Online</span></span>
 <span data-ttu-id="3798f-104">Det är en gemensam praxis tooopen källa projekt Nuförtiden.</span><span class="sxs-lookup"><span data-stu-id="3798f-104">It is a common practice tooopen source projects nowadays.</span></span> <span data-ttu-id="3798f-105">Spara programmet hemligheter i konfigurationsfiler finns inte längre säker hantering som säkerhetsproblem exponeras från hemligheter hamnar från offentliga källa kontroller.</span><span class="sxs-lookup"><span data-stu-id="3798f-105">Saving application secrets in configuration files is no longer safe practice as security vulnerabilities are exposed from secrets being leaked from public source controls.</span></span> <span data-ttu-id="3798f-106">Lagra hemlighet som oformaterad text i en fil i en kontinuerlig Integration pipeline inte är säker delade antingen eftersom servrar kan vara resurser på hello molnmiljö.</span><span class="sxs-lookup"><span data-stu-id="3798f-106">Storing secret as plaintext in a file in a Continuous Integration pipeline is not secure either since build servers could be shared resources on hello Cloud environment.</span></span> <span data-ttu-id="3798f-107">Den här artikeln förklarar hur Visual Studio och Visual Studio Online minskar hello säkerhetsfrågor under utveckling och kontinuerlig Integration process.</span><span class="sxs-lookup"><span data-stu-id="3798f-107">This article explains how Visual Studio and Visual Studio Online mitigates hello security concerns during development and Continuous Integration process.</span></span>

## <a name="remove-diagnostics-storage-key-secret-in-project-configuration-file"></a><span data-ttu-id="3798f-108">Ta bort nyckeln hemlig för diagnostik lagring i konfigurationsfilen för projektet</span><span class="sxs-lookup"><span data-stu-id="3798f-108">Remove Diagnostics Storage Key Secret in Project Configuration File</span></span>
<span data-ttu-id="3798f-109">Cloud Services-tillägget för diagnostik kräver Azure storage för att spara diagnostik resultatet.</span><span class="sxs-lookup"><span data-stu-id="3798f-109">Cloud Services diagnostics extension requires Azure storage for saving diagnostics results.</span></span> <span data-ttu-id="3798f-110">Tidigare hello lagringsanslutningssträng har angetts i hello molntjänster (.cscfg) konfigurationsfiler och gick att checka in toosource kontroll.</span><span class="sxs-lookup"><span data-stu-id="3798f-110">Formerly hello storage connection string is specified in hello Cloud Services configuration (.cscfg) files and could be checked in toosource control.</span></span> <span data-ttu-id="3798f-111">Vi ändrat hello beteende tooonly store en partiell anslutningssträngen med hello nyckel ersättas med en token i hello senaste Azure SDK-versionen.</span><span class="sxs-lookup"><span data-stu-id="3798f-111">In hello latest Azure SDK release we changed hello behavior tooonly store a partial connection string with hello key replaced by a token.</span></span> <span data-ttu-id="3798f-112">hello följande steg beskriver hur hello nya molntjänster verktygsuppsättning fungerar:</span><span class="sxs-lookup"><span data-stu-id="3798f-112">hello following steps describe how hello new Cloud Services tooling works:</span></span>

### <a name="1-open-hello-role-designer"></a><span data-ttu-id="3798f-113">1. Öppna hello rollen designer</span><span class="sxs-lookup"><span data-stu-id="3798f-113">1. Open hello Role designer</span></span>
* <span data-ttu-id="3798f-114">Dubbelklicka på eller högerklicka på en molntjänster rollen tooopen roll designer</span><span class="sxs-lookup"><span data-stu-id="3798f-114">Double click or right click on a Cloud Services role tooopen Role designer</span></span>

![Öppna rollen designer][0]

### <a name="2-under-diagnostics-section-a-new-check-box-dont-remove-storage-key-secret-from-project-is-added"></a><span data-ttu-id="3798f-116">2. Under avsnittet för diagnostik nytt ”inte ta bort nyckeln för säkerhetslagring hemliga från projektet” har lagts till</span><span class="sxs-lookup"><span data-stu-id="3798f-116">2. Under diagnostics section, a new check box “Don’t remove storage key secret from project” is added</span></span>
* <span data-ttu-id="3798f-117">Om du använder hello lokala lagringsemulatorn den här kryssrutan är inaktiverad eftersom det inte finns några hemliga toomanage för hello lokala anslutningssträngen, som är UseDevelopmentStorage = true.</span><span class="sxs-lookup"><span data-stu-id="3798f-117">If you are using hello local storage emulator, this checkbox is disabled because there is no secret toomanage for hello local connection string, which is UseDevelopmentStorage=true.</span></span>

![Lokal lagring emulatorn anslutningssträngen är inte hemliga][1]

* <span data-ttu-id="3798f-119">Om du skapar ett nytt projekt som standard är den här kryssrutan avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="3798f-119">If you are creating a new project, by default this checkbox is unchecked.</span></span> <span data-ttu-id="3798f-120">Detta resulterar i hello lagring viktiga avsnitt i anslutningssträngen för lagring av hello som valts som ersätts med en token.</span><span class="sxs-lookup"><span data-stu-id="3798f-120">This results in hello storage key section of hello selected storage connection string being replaced with a token.</span></span> <span data-ttu-id="3798f-121">hello värdet för hello-token kommer att hittas under hello den aktuella användarens centrala AppData mapp, till exempel: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService</span><span class="sxs-lookup"><span data-stu-id="3798f-121">hello value of hello token will be found under hello current user’s AppData Roaming folder, for example: C:\Users\contosouser\AppData\Roaming\Microsoft\CloudService</span></span>

> <span data-ttu-id="3798f-122">Observera hello user\AppData mappen är åtkomst kontrolleras av användarens inloggning och anses vara en säker plats toostore development hemligheter.</span><span class="sxs-lookup"><span data-stu-id="3798f-122">Note that hello user\AppData folder is Access Controlled by user sign-in and is considered a secure place toostore development secrets.</span></span>
> 
> 

![Nyckeln för säkerhetslagring sparas under användarprofil][2]

### <a name="3-select-a-diagnostics-storage-account"></a><span data-ttu-id="3798f-124">3. Välj ett lagringskonto för diagnostik</span><span class="sxs-lookup"><span data-stu-id="3798f-124">3. Select a diagnostics storage account</span></span>
* <span data-ttu-id="3798f-125">Välj ett lagringskonto från hello dialogrutan startas genom att klicka på ”...” Hej</span><span class="sxs-lookup"><span data-stu-id="3798f-125">Select a storage account from hello dialog launched by clicking hello “…”</span></span> <span data-ttu-id="3798f-126">till.</span><span class="sxs-lookup"><span data-stu-id="3798f-126">button.</span></span> <span data-ttu-id="3798f-127">Observera hur anslutningssträngen för lagring av hello genereras inte hello lagringskontonyckel.</span><span class="sxs-lookup"><span data-stu-id="3798f-127">Notice how hello storage connection string generated will not have hello storage account key.</span></span>
* <span data-ttu-id="3798f-128">Till exempel: DefaultEndpointsProtocol = https; AccountName = contosostorage; AccountKey = $(*clouddiagstrg.key*)</span><span class="sxs-lookup"><span data-stu-id="3798f-128">For example: DefaultEndpointsProtocol=https;AccountName=contosostorage;AccountKey=$(*clouddiagstrg.key*)</span></span>

### <a name="4----debugging-hello-project"></a><span data-ttu-id="3798f-129">4.    Felsökning hello-projekt</span><span class="sxs-lookup"><span data-stu-id="3798f-129">4.    Debugging hello project</span></span>
* <span data-ttu-id="3798f-130">F5 toostart felsökning i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3798f-130">F5 toostart debugging in Visual Studio.</span></span> <span data-ttu-id="3798f-131">Allt bör fungera i hello samma sätt som tidigare.</span><span class="sxs-lookup"><span data-stu-id="3798f-131">Everything should work in hello same way as before.</span></span>
  <span data-ttu-id="3798f-132">![Starta felsökning lokalt][3]</span><span class="sxs-lookup"><span data-stu-id="3798f-132">![Start debugging locally][3]</span></span>

### <a name="5-publish-project-from-visual-studio"></a><span data-ttu-id="3798f-133">5. Publicera projekt från Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3798f-133">5. Publish project from Visual Studio</span></span>
* <span data-ttu-id="3798f-134">Starta hello dialogrutan Publicera och fortsätt inloggningen instruktioner toopublish hello applicaion tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3798f-134">Launch hello publish dialog and proceed with sign-in instructions toopublish hello applicaion tooAzure.</span></span>

### <a name="6-additional-information"></a><span data-ttu-id="3798f-135">6. Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="3798f-135">6. Additional information</span></span>
> <span data-ttu-id="3798f-136">Obs: hello inställningar panelen i hello rollen designer förblir eftersom den är för tillfället.</span><span class="sxs-lookup"><span data-stu-id="3798f-136">Note: hello Settings panel in hello role designer will stay as it is for now.</span></span> <span data-ttu-id="3798f-137">Om du vill toouse hello hemliga hanteringsfunktionen för diagnostik gå toohello konfigurationer fliken.</span><span class="sxs-lookup"><span data-stu-id="3798f-137">If you want toouse hello secret management feature for diagnostics, go toohello Configurations tab.</span></span>
> 
> 

![Lägg till-inställningar][4]

> <span data-ttu-id="3798f-139">Obs: Om aktiverad, hello Application Insights nyckeln lagras som klartext.</span><span class="sxs-lookup"><span data-stu-id="3798f-139">Note: If enabled, hello Application Insights key will be stored as plain text.</span></span> <span data-ttu-id="3798f-140">hello-nyckeln används endast för Överför innehållet så att inga känsliga data är i fara komprometteras.</span><span class="sxs-lookup"><span data-stu-id="3798f-140">hello key is only used for upload contents so no sensitive data will be at risk being compromised.</span></span>
> 
> 

## <a name="build-and-publish-a-cloud-services-project-using-visual-studio-online-task-templates"></a><span data-ttu-id="3798f-141">Skapa och publicera en Cloud Services-projekt med hjälp av Visual Studio online uppgiftsmallar</span><span class="sxs-lookup"><span data-stu-id="3798f-141">Build and Publish a Cloud Services Project using Visual Studio online Task Templates</span></span>
* <span data-ttu-id="3798f-142">hello följande steg visar hur toosetup kontinuerlig Integration för molntjänster projekt med Visual Studio online uppgifter:</span><span class="sxs-lookup"><span data-stu-id="3798f-142">hello following steps illustrates how toosetup Continuous Integration for Cloud Services project using Visual Studio online tasks:</span></span>
  ### <a name="1----obtain-a-vso-account"></a><span data-ttu-id="3798f-143">1.    Hämta ett VSO konto</span><span class="sxs-lookup"><span data-stu-id="3798f-143">1.    Obtain a VSO account</span></span>
* <span data-ttu-id="3798f-144">[Skapa Visual Studio Online-konto] [ Create Visual Studio Online account] om du inte redan har en</span><span class="sxs-lookup"><span data-stu-id="3798f-144">[Create Visual Studio Online account][Create Visual Studio Online account] if you don’t have one already</span></span>
* <span data-ttu-id="3798f-145">[Skapa grupprojekt] [ Create team project] i Visual Studio-onlinekonto</span><span class="sxs-lookup"><span data-stu-id="3798f-145">[Create team project][Create team project] in your Visual Studio online account</span></span>

### <a name="2----setup-source-control-in-visual-studio"></a><span data-ttu-id="3798f-146">2.    Konfigurera källkontroll i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3798f-146">2.    Setup source control in Visual Studio</span></span>
* <span data-ttu-id="3798f-147">Ansluta tooa grupprojekt</span><span class="sxs-lookup"><span data-stu-id="3798f-147">Connect tooa team project</span></span>

![Ansluta tooteam-projekt][5]

![Välj team projekt tooconnect till][6]

* <span data-ttu-id="3798f-150">Lägg till ditt projekt toosource kontroll</span><span class="sxs-lookup"><span data-stu-id="3798f-150">Add your project toosource control</span></span>

![Lägga till projektet toosource kontroll][7]

![Mappa kontrollen projektmappen tooa källa][8]

* <span data-ttu-id="3798f-153">Kontrollera i projektet från Explorer-teamet</span><span class="sxs-lookup"><span data-stu-id="3798f-153">Check in your project from Team Explorer</span></span>

![Kontrollera i projektet toosource kontroll][9]

### <a name="3----configure-build-process"></a><span data-ttu-id="3798f-155">3.    Konfigurera skapandeprocess</span><span class="sxs-lookup"><span data-stu-id="3798f-155">3.    Configure Build process</span></span>
* <span data-ttu-id="3798f-156">Bläddra tooyour grupprojekt och Lägg till en ny build-process mallar</span><span class="sxs-lookup"><span data-stu-id="3798f-156">Browse tooyour team project and add a new build process Templates</span></span>

![Lägg till en ny version][10]

* <span data-ttu-id="3798f-158">Välj build-aktivitet</span><span class="sxs-lookup"><span data-stu-id="3798f-158">Select build task</span></span>

![Lägga till en build-aktivitet][11]

![Välj mall för Visual Studio-Skapa aktivitet][12]

* <span data-ttu-id="3798f-161">Redigera build uppgiften indata.</span><span class="sxs-lookup"><span data-stu-id="3798f-161">Edit build task input.</span></span> <span data-ttu-id="3798f-162">Anpassa hello build parametrar enligt tooyour måste du</span><span class="sxs-lookup"><span data-stu-id="3798f-162">Please customize hello build parameters according tooyour need</span></span>

![Konfigurera build-aktivitet][13]

`/t:Publish /p:TargetProfile=$(targetProfile) /p:DebugType=None /p:SkipInvalidConfigurations=true /p:OutputPath=bin\ /p:PublishDir="$(build.artifactstagingdirectory)\\"`

* <span data-ttu-id="3798f-164">Konfigurera build variabler</span><span class="sxs-lookup"><span data-stu-id="3798f-164">Configure build variables</span></span>

![Konfigurera build variabler][14]

* <span data-ttu-id="3798f-166">Lägg till en aktivitet tooupload build släpp</span><span class="sxs-lookup"><span data-stu-id="3798f-166">Add a task tooupload build drop</span></span>

![Välj publicera build släpp aktivitet][15]

![Konfigurera publicera build släpp aktivitet][16]

* <span data-ttu-id="3798f-169">Kör hello build</span><span class="sxs-lookup"><span data-stu-id="3798f-169">Run hello build</span></span>

![Ny version av kön][17]

![Visa build sammanfattning][18]

* <span data-ttu-id="3798f-172">Om hello build lyckas visas en liknande toobelow resultat</span><span class="sxs-lookup"><span data-stu-id="3798f-172">if hello build is successful you will see a result similar toobelow</span></span>

![Skapa resultat][19]

### <a name="4----configure-release-process"></a><span data-ttu-id="3798f-174">4.    Konfigurera Release-processen</span><span class="sxs-lookup"><span data-stu-id="3798f-174">4.    Configure Release process</span></span>
* <span data-ttu-id="3798f-175">Skapa en ny version</span><span class="sxs-lookup"><span data-stu-id="3798f-175">Create a new release</span></span>

![Skapa ny version][20]

* <span data-ttu-id="3798f-177">Välj hello uppgift för distribution av Azure Cloud Services</span><span class="sxs-lookup"><span data-stu-id="3798f-177">Select hello Azure Cloud Services Deployment task</span></span>

![Välj Azure Cloud Services uppgift för distribution][21]

* <span data-ttu-id="3798f-179">Som hello lagringskontonyckel inte är markerat i toosource kontrollen behöver vi toospecify hello hemlig nyckel för att ställa in diagnostik tillägg.</span><span class="sxs-lookup"><span data-stu-id="3798f-179">As hello storage account key is not checked in toosource control, we need toospecify hello secret key for setting diagnostics extensions.</span></span> <span data-ttu-id="3798f-180">Expandera hello **avancerade alternativ för att skapa nya tjänsten** avsnittet och redigera hello **diagnostik Lagringskontonycklar** parametern indata.</span><span class="sxs-lookup"><span data-stu-id="3798f-180">Expand hello **Advanced Options for Creating New Service** section and edit hello **Diagnostics Storage Account Keys** parameter input.</span></span> <span data-ttu-id="3798f-181">Den här indata använder flera rader med nyckel/värde-par i hello-format för **[RoleName]:$(StorageAccountKey)**</span><span class="sxs-lookup"><span data-stu-id="3798f-181">This input takes in multiple lines of key value pair in hello format of **[RoleName]:$(StorageAccountKey)**</span></span>

> <span data-ttu-id="3798f-182">Obs: om din diagnostik storage-konto är under hello samma prenumeration som där du ska publicera hello molntjänster programmet, du inte tooenter hello nyckel i hello distribution uppgiften indata; hello distribution hämtar programmässigt hello storage-informationen från prenumerationen</span><span class="sxs-lookup"><span data-stu-id="3798f-182">Note: if your diagnostics storage account is under hello same subscription as where you will publish hello Cloud Services application, you don't have tooenter hello key in hello deployment task input; hello deployment will programmatically obtain hello storage information from your subscription</span></span>
> 
> 

![Konfigurera Cloud Services distribution aktivitet][22]

* <span data-ttu-id="3798f-184">Använd hemlighet Skapa variabler toosave Lagringsnycklar.</span><span class="sxs-lookup"><span data-stu-id="3798f-184">Use secret build variables toosave storage Keys.</span></span> <span data-ttu-id="3798f-185">indata för en variabel som hemlighet klickar du på hello låsikon hello höger på hello toomask variabler</span><span class="sxs-lookup"><span data-stu-id="3798f-185">toomask a variable as secret click on hello lock icon on hello right side of hello Variables input</span></span>

![Spara lagringsnycklar i hemlighet Skapa variabler][23]

* <span data-ttu-id="3798f-187">Skapa en version och distribuera ditt projekt tooAzure</span><span class="sxs-lookup"><span data-stu-id="3798f-187">Create a release and deploy your project tooAzure</span></span>

![Skapa ny version][24]

## <a name="next-steps"></a><span data-ttu-id="3798f-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3798f-189">Next steps</span></span>
<span data-ttu-id="3798f-190">toolearn mer information om hur du anger diagnostik tillägg för Azure Cloud Services, se [aktivera diagnostik i Azure Cloud Services med hjälp av PowerShell][Enable diagnostics in Azure Cloud Services using PowerShell]</span><span class="sxs-lookup"><span data-stu-id="3798f-190">toolearn more about setting diagnostics extensions for Azure Cloud Services, please see [Enable diagnostics in Azure Cloud Services using PowerShell][Enable diagnostics in Azure Cloud Services using PowerShell]</span></span>

[Create Visual Studio Online account]:https://www.visualstudio.com/team-services/
[Create team project]: https://www.visualstudio.com/it-it/docs/setup-admin/team-services/connect-to-visual-studio-team-services
[Enable diagnostics in Azure Cloud Services using PowerShell]:https://azure.microsoft.com/en-us/documentation/articles/cloud-services-diagnostics-powershell/

[0]: ./media/cloud-services-vs-ci/vs-01.png
[1]: ./media/cloud-services-vs-ci/vs-02.png
[2]: ./media/cloud-services-vs-ci/file-01.png
[3]: ./media/cloud-services-vs-ci/vs-03.png
[4]: ./media/cloud-services-vs-ci/vs-04.png
[5]: ./media/cloud-services-vs-ci/vs-05.png
[6]: ./media/cloud-services-vs-ci/vs-06.png
[7]: ./media/cloud-services-vs-ci/vs-07.png
[8]: ./media/cloud-services-vs-ci/vs-08.png
[9]: ./media/cloud-services-vs-ci/vs-09.png
[10]: ./media/cloud-services-vs-ci/vso-01.png
[11]: ./media/cloud-services-vs-ci/vso-02.png
[12]: ./media/cloud-services-vs-ci/vso-03.png
[13]: ./media/cloud-services-vs-ci/vso-04.png
[14]: ./media/cloud-services-vs-ci/vso-05.png
[15]: ./media/cloud-services-vs-ci/vso-06.png
[16]: ./media/cloud-services-vs-ci/vso-07.png
[17]: ./media/cloud-services-vs-ci/vso-08.png
[18]: ./media/cloud-services-vs-ci/vso-09.png
[19]: ./media/cloud-services-vs-ci/vso-10.png
[20]: ./media/cloud-services-vs-ci/vso-11.png
[21]: ./media/cloud-services-vs-ci/vso-12.png
[22]: ./media/cloud-services-vs-ci/vso-13.png
[23]: ./media/cloud-services-vs-ci/vso-14.png
[24]: ./media/cloud-services-vs-ci/vso-15.png
