---
title: aaaContinuous levereras cloud services med TFS i Azure | Microsoft Docs
description: "Lär dig hur tooset in kontinuerlig leverans för Azure cloud appar. Kodexempel för kommandoraden MSBuild-satser och PowerShell-skript."
services: cloud-services
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 4f3c93c6-5c82-4779-9d19-7404a01e82ca
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/12/2017
ms.author: kraigb
ms.openlocfilehash: c0e5e72ffbd3c05b84ce1733068e92c528bcc4b9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-for-cloud-services-in-azure"></a><span data-ttu-id="8a71c-104">Kontinuerlig leverans för molntjänster i Azure</span><span class="sxs-lookup"><span data-stu-id="8a71c-104">Continuous Delivery for Cloud Services in Azure</span></span>
<span data-ttu-id="8a71c-105">hello process som beskrivs i den här artikeln beskrivs hur du tooset in kontinuerlig leverans för appar i Azure-molnet.</span><span class="sxs-lookup"><span data-stu-id="8a71c-105">hello process described in this article shows you how tooset up continuous delivery for Azure cloud apps.</span></span> <span data-ttu-id="8a71c-106">Den här processen kan du automatiskt skapa paket och distribuera hello paketet tooAzure efter varje kod incheckning.</span><span class="sxs-lookup"><span data-stu-id="8a71c-106">This process enables you to automatically create packages and deploy hello package tooAzure after every code check-in.</span></span> <span data-ttu-id="8a71c-107">hello paketet build process som beskrivs i den här artikeln är likvärdiga toohello **paketet** kommandot i Visual Studio och publishing stegen är likvärdiga toohello **publicera** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8a71c-107">hello package build process described in this article is equivalent toohello **Package** command in Visual Studio, and the publishing steps are equivalent toohello **Publish** command in Visual Studio.</span></span>
<span data-ttu-id="8a71c-108">hello artikeln omfattar hello metoder som du skulle använda toocreate en build-server med kommandoradsverktyget MSBuild-instruktioner och Windows PowerShell-skript och det också visar hur toooptionally konfigurera Visual Studio Team Foundation Server - teamet skapa definitioner toouse hello MSBuild-kommandon och PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="8a71c-108">hello article covers hello methods you would use toocreate a build server with MSBuild command-line statements and Windows PowerShell scripts, and it also demonstrates how toooptionally configure Visual Studio Team Foundation Server - Team Build definitions toouse hello MSBuild commands and PowerShell scripts.</span></span> <span data-ttu-id="8a71c-109">hello process kan anpassas för build-miljön och Azure mål-miljöer.</span><span class="sxs-lookup"><span data-stu-id="8a71c-109">hello process is customizable for your build environment and Azure target environments.</span></span>

<span data-ttu-id="8a71c-110">Du kan också använda Visual Studio Team Services, en version av TFS som finns i Azure, toodo detta enklare.</span><span class="sxs-lookup"><span data-stu-id="8a71c-110">You can also use Visual Studio Team Services, a version of TFS that is hosted in Azure, toodo this more easily.</span></span> 

<span data-ttu-id="8a71c-111">Innan du börjar bör du publicera programmet från Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8a71c-111">Before you start, you should publish your application from Visual Studio.</span></span>
<span data-ttu-id="8a71c-112">Detta säkerställer att alla hello resurser är tillgängliga och initierad när du försöker tooautomate hello publiceringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="8a71c-112">This will ensure that all hello resources are available and initialized when you attempt tooautomate hello publication process.</span></span>

## <a name="1-configure-hello-build-server"></a><span data-ttu-id="8a71c-113">1: Konfigurera hello skapa Server</span><span class="sxs-lookup"><span data-stu-id="8a71c-113">1: Configure hello Build Server</span></span>
<span data-ttu-id="8a71c-114">Innan du kan skapa ett Azure-paket med hjälp av MSBuild, måste du installera hello som krävs för programvaran och verktyg på hello build-servern.</span><span class="sxs-lookup"><span data-stu-id="8a71c-114">Before you can create an Azure package by using MSBuild, you must install hello required software and tools on hello build server.</span></span>

<span data-ttu-id="8a71c-115">Visual Studio är inte obligatoriska toobe installerad på hello build-servern.</span><span class="sxs-lookup"><span data-stu-id="8a71c-115">Visual Studio is not required toobe installed on hello build server.</span></span> <span data-ttu-id="8a71c-116">Om du vill toouse Byggtjänsten för Team Foundation toomanage build-servern, följer du anvisningarna för hello i hello [Byggtjänsten för Team Foundation] [ Team Foundation Build Service] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="8a71c-116">If you want toouse Team Foundation Build Service toomanage your build server, follow hello instructions in hello [Team Foundation Build Service][Team Foundation Build Service] documentation.</span></span>

1. <span data-ttu-id="8a71c-117">Hello build-servern installerar hello [.NET Framework 4.5.2][.NET Framework 4.5.2], som innehåller MSBuild.</span><span class="sxs-lookup"><span data-stu-id="8a71c-117">On hello build server, install hello [.NET Framework 4.5.2][.NET Framework 4.5.2], which includes MSBuild.</span></span>
2. <span data-ttu-id="8a71c-118">Installera hello senaste [Azure redigering av verktyg för .NET](https://azure.microsoft.com/develop/net/).</span><span class="sxs-lookup"><span data-stu-id="8a71c-118">Install hello latest [Azure Authoring Tools for .NET](https://azure.microsoft.com/develop/net/).</span></span>
3. <span data-ttu-id="8a71c-119">Installera hello [Azure-bibliotek för .NET](http://go.microsoft.com/fwlink/?LinkId=623519).</span><span class="sxs-lookup"><span data-stu-id="8a71c-119">Install hello [Azure Libraries for .NET](http://go.microsoft.com/fwlink/?LinkId=623519).</span></span>
4. <span data-ttu-id="8a71c-120">Kopiera hello Microsoft.WebApplication.targets fil från en toohello för installation av Visual Studio skapa server.</span><span class="sxs-lookup"><span data-stu-id="8a71c-120">Copy hello Microsoft.WebApplication.targets file from a Visual Studio installation toohello build server.</span></span>

   <span data-ttu-id="8a71c-121">Den här filen finns på en dator med Visual Studio installerat i hello directory C:\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications.</span><span class="sxs-lookup"><span data-stu-id="8a71c-121">On a computer with Visual Studio installed, this file is located in hello directory C:\\Program Files(x86)\\MSBuild\\Microsoft\\VisualStudio\\v14.0\\WebApplications.</span></span> <span data-ttu-id="8a71c-122">Du bör kopiera den toohello samma katalog på hello build-servern.</span><span class="sxs-lookup"><span data-stu-id="8a71c-122">You should copy it toohello same directory on hello build server.</span></span>
5. <span data-ttu-id="8a71c-123">Installera hello [Azure Tools för Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="8a71c-123">Install hello [Azure Tools for Visual Studio](https://www.visualstudio.com/features/azure-tools-vs.aspx).</span></span>

## <a name="2-build-a-package-using-msbuild-commands"></a><span data-ttu-id="8a71c-124">2: skapa ett paket med hjälp av MSBuild-kommandon</span><span class="sxs-lookup"><span data-stu-id="8a71c-124">2: Build a Package using MSBuild Commands</span></span>
<span data-ttu-id="8a71c-125">Det här avsnittet beskrivs hur tooconstruct en MSBuild kommando som skapar ett Azure-paket.</span><span class="sxs-lookup"><span data-stu-id="8a71c-125">This section describes how tooconstruct an MSBuild command that builds an Azure package.</span></span> <span data-ttu-id="8a71c-126">Kör det här steget på hello build server tooverify att allt är korrekt konfigurerat och att hello MSBuild-kommandot har vad du vill toodo.</span><span class="sxs-lookup"><span data-stu-id="8a71c-126">Run this step on hello build server tooverify that everything is configured correctly and that hello MSBuild command does what you want it toodo.</span></span> <span data-ttu-id="8a71c-127">Du kan antingen lägga till den här kommandoraden tooexisting skapa skript i hello build-server eller så kan du använda hello kommandoraden i en TFS skapa Definition, enligt beskrivningen i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="8a71c-127">You can either add this command line tooexisting build scripts on hello build server, or you can use hello command line in a TFS Build Definition, as described in hello next section.</span></span> <span data-ttu-id="8a71c-128">Mer information om kommandoradsparametrar och MSBuild finns [MSBuild Kommandoradsreferens](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="8a71c-128">For more information about command-line parameters and MSBuild, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span></span>

1. <span data-ttu-id="8a71c-129">Om Visual Studio är installerat på hello build-server, leta upp och välj **Kommandotolken Visual Studio** i hello **Visual Studio Tools** -mappen i Windows.</span><span class="sxs-lookup"><span data-stu-id="8a71c-129">If Visual Studio is installed on hello build server, locate and choose **Visual Studio Command Prompt** in hello **Visual Studio Tools** folder in Windows.</span></span>

   <span data-ttu-id="8a71c-130">Om Visual Studio inte är installerat på hello build-servern, öppna en kommandotolk och kontrollera att MSBuild.exe är tillgängligt på sökvägen.</span><span class="sxs-lookup"><span data-stu-id="8a71c-130">If Visual Studio is not installed on hello build server, open a command prompt and make sure that MSBuild.exe is accessible on the path.</span></span> <span data-ttu-id="8a71c-131">MSBuild installeras med hello .NET Framework i hello sökvägen % WINDIR %\\Microsoft.NET\\Framework\\*Version*.</span><span class="sxs-lookup"><span data-stu-id="8a71c-131">MSBuild is installed with hello .NET Framework in hello path %WINDIR%\\Microsoft.NET\\Framework\\*Version*.</span></span> <span data-ttu-id="8a71c-132">Om du vill lägga till MSBuild.exe toohello PATH-miljövariabeln när du har .NET Framework 4 har installerats, exempelvis följande kommando vid kommandotolken hello hello:</span><span class="sxs-lookup"><span data-stu-id="8a71c-132">For example, to add MSBuild.exe toohello PATH environment variable when you have .NET Framework 4 installed, type hello following command at hello command prompt:</span></span>

       set PATH=%PATH%;"C:\Windows\Microsoft.NET\Framework\v4.0.30319"
2. <span data-ttu-id="8a71c-133">Navigera toohello mapp som innehåller filen Azure-projekt som du vill toobuild Kommandotolken hello.</span><span class="sxs-lookup"><span data-stu-id="8a71c-133">At hello command prompt, navigate toohello folder containing the Azure project file that you want toobuild.</span></span>
3. <span data-ttu-id="8a71c-134">Kör MSBuild med hello/target: publicera alternativet som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="8a71c-134">Run MSBuild with hello /target:Publish option as in hello following example:</span></span>

       MSBuild /target:Publish

   <span data-ttu-id="8a71c-135">Det här alternativet kan vara förkortas /t: publicera.</span><span class="sxs-lookup"><span data-stu-id="8a71c-135">This option can be abbreviated as /t:Publish.</span></span> <span data-ttu-id="8a71c-136">Hej /t:Publish alternativ i MSBuild ska inte förväxlas med hello publicera kommandon i Visual Studio när du har hello Azure SDK har installerats.</span><span class="sxs-lookup"><span data-stu-id="8a71c-136">hello /t:Publish option in MSBuild should not be confused with hello Publish commands available in Visual Studio when you have hello Azure SDK installed.</span></span> <span data-ttu-id="8a71c-137">Hej /t: publicera alternativet endast versioner hello Azure-paket.</span><span class="sxs-lookup"><span data-stu-id="8a71c-137">hello /t:Publish option only builds hello Azure packages.</span></span> <span data-ttu-id="8a71c-138">Den distribuerar inte hello-paket som hello publicera kommandon i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8a71c-138">It does not deploy hello packages as hello Publish commands in Visual Studio do.</span></span>

   <span data-ttu-id="8a71c-139">Alternativt kan ange du hello projektnamn som en MSBuild-parameter.</span><span class="sxs-lookup"><span data-stu-id="8a71c-139">Optionally, you can specify hello project name as an MSBuild parameter.</span></span> <span data-ttu-id="8a71c-140">Om inget anges används hello aktuell katalog.</span><span class="sxs-lookup"><span data-stu-id="8a71c-140">If not specified, hello current directory is used.</span></span> <span data-ttu-id="8a71c-141">Mer information om MSBuild kommandoradsalternativ finns [MSBuild Kommandoradsreferens](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span><span class="sxs-lookup"><span data-stu-id="8a71c-141">For more information about MSBuild command line options, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311%28v=vs.140%29.aspx).</span></span>
4. <span data-ttu-id="8a71c-142">Leta upp hello utdata.</span><span class="sxs-lookup"><span data-stu-id="8a71c-142">Locate hello output.</span></span> <span data-ttu-id="8a71c-143">Som standard det här kommandot skapar en katalog i relationen toohello rotmapp för hello projekt som *ProjectDir*\\bin\\*Configuration* \\ App.publish\\.</span><span class="sxs-lookup"><span data-stu-id="8a71c-143">By default, this command creates a directory in relation toohello root folder for hello project, such as *ProjectDir*\\bin\\*Configuration*\\app.publish\\.</span></span> <span data-ttu-id="8a71c-144">När du skapar ett Azure-projekt kan generera du två filer, hello paketfilen sig själv och hello tillhörande konfigurationsfil:</span><span class="sxs-lookup"><span data-stu-id="8a71c-144">When you build an Azure project, you generate two files, hello package file itself and hello accompanying configuration file:</span></span>

   * <span data-ttu-id="8a71c-145">Project.cspkg</span><span class="sxs-lookup"><span data-stu-id="8a71c-145">Project.cspkg</span></span>
   * <span data-ttu-id="8a71c-146">ServiceConfiguration. *TargetProfile*.cscfg</span><span class="sxs-lookup"><span data-stu-id="8a71c-146">ServiceConfiguration.*TargetProfile*.cscfg</span></span>

   <span data-ttu-id="8a71c-147">Som standard varje Azure-projekt inkluderar en service konfigurationsfilen (.cscfg-filen) för lokala (felsökning) versioner och ett annat för molnet (mellanlagring eller produktion) versioner, men du kan lägga till eller ta bort service configuration-filer efter behov.</span><span class="sxs-lookup"><span data-stu-id="8a71c-147">By default, each Azure project includes one service configuration file (.cscfg file) for local (debugging) builds and another for cloud (staging or production) builds, but you can add or remove service configuration files as needed.</span></span> <span data-ttu-id="8a71c-148">När du skapar ett paket i Visual Studio kan blir du ombedd vilka service configuration file tooinclude tillsammans med hello paketet.</span><span class="sxs-lookup"><span data-stu-id="8a71c-148">When you build a package within Visual Studio, you will be asked which service configuration file tooinclude alongside hello package.</span></span>
5. <span data-ttu-id="8a71c-149">Ange hello service konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="8a71c-149">Specify hello service configuration file.</span></span> <span data-ttu-id="8a71c-150">När du skapar ett paket med hjälp av MSBuild med hello lokal tjänst-konfigurationsfilen som standard.</span><span class="sxs-lookup"><span data-stu-id="8a71c-150">When you build a package by using MSBuild, hello local service configuration file is included by default.</span></span> <span data-ttu-id="8a71c-151">tooinclude en annan tjänstkonfigurationsfil egenskapen TargetProfile för hello MSBuild-kommandot, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="8a71c-151">tooinclude a different service configuration file, set the TargetProfile property of hello MSBuild command, as in hello following example:</span></span>

       MSBuild /t:Publish /p:TargetProfile=Cloud
6. <span data-ttu-id="8a71c-152">Ange hello plats för hello utdata.</span><span class="sxs-lookup"><span data-stu-id="8a71c-152">Specify hello location for hello output.</span></span> <span data-ttu-id="8a71c-153">Ange hello sökväg med hjälp av /p:PublishDir =*Directory* \\ alternativ, inklusive hello avslutande omvänt avgränsare, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="8a71c-153">Set hello path by using the /p:PublishDir=*Directory*\\ option, including hello trailing backslash separator, as in hello following example:</span></span>

       MSBuild /target:Publish /p:PublishDir=\\myserver\drops\

   <span data-ttu-id="8a71c-154">När du har skapats och testa en lämplig MSBuild rad toobuild projekt och kombineras till ett Azure-paket, du kan lägga till den här kommandoraden tooyour build-skript.</span><span class="sxs-lookup"><span data-stu-id="8a71c-154">Once you've constructed and tested an appropriate MSBuild command line toobuild your projects and combine them into an Azure package, you can add this command line tooyour build scripts.</span></span> <span data-ttu-id="8a71c-155">Om din build-servern använder anpassade skript, beror den här processen på specifika anpassade build-processen.</span><span class="sxs-lookup"><span data-stu-id="8a71c-155">If your build server uses custom scripts, this process will depend on the specifics of your custom build process.</span></span> <span data-ttu-id="8a71c-156">Om du använder TFS som en kompileringsmiljö, kan du följa hello instruktioner i hello nästa steg tooadd hello Azure-paketet build tooyour build.</span><span class="sxs-lookup"><span data-stu-id="8a71c-156">If you are using TFS as a build environment, then you can follow hello instructions in hello next step tooadd hello Azure package build tooyour build process.</span></span>

## <a name="3-build-a-package-using-tfs-team-build"></a><span data-ttu-id="8a71c-157">3: skapa ett paket med TFS-teamet skapa</span><span class="sxs-lookup"><span data-stu-id="8a71c-157">3: Build a Package using TFS Team Build</span></span>
<span data-ttu-id="8a71c-158">Om du har servern TFS (Team Foundation) som en build domänkontrollant och hello skapa server har angetts som en TFS build-dator och du kan du konfigurera en automatisk build för Azure-paket.</span><span class="sxs-lookup"><span data-stu-id="8a71c-158">If you have Team Foundation Server (TFS) set up as a build controller and hello build server set up as a TFS build machine, then you can optionally set up an automated build for your Azure package.</span></span> <span data-ttu-id="8a71c-159">Mer information om hur tooset in och använda Team Foundation server som ett build-system, se [skala ut ditt system][Scale out your build system].</span><span class="sxs-lookup"><span data-stu-id="8a71c-159">For information on how tooset up and use Team Foundation server as a build system, see [Scale out your build system][Scale out your build system].</span></span> <span data-ttu-id="8a71c-160">I synnerhet följande procedur förutsätter att du har konfigurerat build-server enligt beskrivningen i [distribuera och konfigurera en build-server][Deploy and configure a build server], och att du har skapat ett grupprojekt skapas ett moln Service-projekt i hello grupprojekt.</span><span class="sxs-lookup"><span data-stu-id="8a71c-160">In particular, the following procedure assumes that you have configured your build server as described in [Deploy and configure a build server][Deploy and configure a build server], and that you have created a team project, created a cloud service project in hello team project.</span></span>

<span data-ttu-id="8a71c-161">tooconfigure TFS toobuild Azure paket, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="8a71c-161">tooconfigure TFS toobuild Azure packages, perform hello following steps:</span></span>

1. <span data-ttu-id="8a71c-162">I Visual Studio på utvecklingsdatorn hello Visa-menyn, Välj **Team Explorer**, eller välj Ctrl +\\, Ctrl + M.</span><span class="sxs-lookup"><span data-stu-id="8a71c-162">In Visual Studio on your development computer, on hello View menu, choose **Team Explorer**, or choose Ctrl+\\, Ctrl+M.</span></span> <span data-ttu-id="8a71c-163">I fönstret Team Explorer expanderar du hello **bygger** nod eller välj hello **bygger** , och väljer **ny skapa Definition**.</span><span class="sxs-lookup"><span data-stu-id="8a71c-163">In the Team Explorer window, expand hello **Builds** node or choose hello **Builds** page, and choose **New Build Definition**.</span></span>

   ![Nytt skapa Definition alternativ][0]
2. <span data-ttu-id="8a71c-165">Välj hello **utlösaren** och ange hello önskad villkor för när du vill att hello paketet toobe inbyggda.</span><span class="sxs-lookup"><span data-stu-id="8a71c-165">Choose hello **Trigger** tab, and specify hello desired conditions for when you want hello package toobe built.</span></span> <span data-ttu-id="8a71c-166">Till exempel ange **kontinuerlig Integration** toobuild hello paketet när ett källkontroll checka in inträffar.</span><span class="sxs-lookup"><span data-stu-id="8a71c-166">For example, specify **Continuous Integration** toobuild hello package whenever a source control check-in occurs.</span></span>
3. <span data-ttu-id="8a71c-167">Välj hello **inställningar för datakälla** fliken och kontrollera att din projektmapp visas i hello **kontroll källmapp** kolumn, och hello status är **Active**.</span><span class="sxs-lookup"><span data-stu-id="8a71c-167">Choose hello **Source Settings** tab, and make sure your project folder is listed in hello **Source Control Folder** column, and hello status is **Active**.</span></span>
4. <span data-ttu-id="8a71c-168">Välj hello **skapa standardvärden** och kontrollera hello namnet på hello build-servern under Build-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="8a71c-168">Choose hello **Build Defaults** tab, and under Build controller, verify hello name of hello build server.</span></span>  <span data-ttu-id="8a71c-169">Dessutom välja hello **kopiera build utdata toohello följande avlämningsmapp** och ange önskade hello målplatsen.</span><span class="sxs-lookup"><span data-stu-id="8a71c-169">Also, choose hello option **Copy build output toohello following drop folder** and specify hello desired drop location.</span></span>
5. <span data-ttu-id="8a71c-170">Välj hello **processen** fliken. Hello Process, Välj fliken hello standardmallen under **skapa**, Välj hello projektet om den inte redan är valt, och expandera hello **Avancerat** avsnitt i hello **skapa**avsnitt i hello rutnätet.</span><span class="sxs-lookup"><span data-stu-id="8a71c-170">Choose hello **Process** tab. On hello Process tab, choose hello default template, under **Build**, choose hello project if it is not already selected, and expand hello **Advanced** section in hello **Build** section of hello grid.</span></span>
6. <span data-ttu-id="8a71c-171">Välj **MSBuild-argument**, och ange hello lämpliga MSBuild kommandoradsargument som beskrivs i steg 2 ovan.</span><span class="sxs-lookup"><span data-stu-id="8a71c-171">Choose **MSBuild Arguments**, and set hello appropriate MSBuild command line arguments as described in Step 2 above.</span></span> <span data-ttu-id="8a71c-172">Ange till exempel **/t: Publicera /p:PublishDir =\\\\minserver\\släpper\\**  toobuild ett paket och kopiera hello-paket filer toohello plats \\ \\minserver\\släpper\\:</span><span class="sxs-lookup"><span data-stu-id="8a71c-172">For example, enter **/t:Publish /p:PublishDir=\\\\myserver\\drops\\** toobuild a package and copy hello package files toohello location \\\\myserver\\drops\\:</span></span>

   ![MSBuild-argument][2]

   > [!NOTE]
   > <span data-ttu-id="8a71c-174">Kopiera hello filer tooa offentlig resurs gör det enklare toomanually distribuerar hello-paket på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="8a71c-174">Copying hello files tooa public share makes it easier toomanually deploy hello packages from your development computer.</span></span>
7. <span data-ttu-id="8a71c-175">Testa hello lyckats build-steg genom att kontrollera i ändra tooyour projekt eller Köa en ny version.</span><span class="sxs-lookup"><span data-stu-id="8a71c-175">Test hello success of your build step by checking in a change tooyour project, or queue up a new build.</span></span> <span data-ttu-id="8a71c-176">tooqueue in en ny version i teamet Explorer, högerklicka på **alla skapa definitioner** och välj sedan **kön nya skapa**.</span><span class="sxs-lookup"><span data-stu-id="8a71c-176">tooqueue up a new build, in the Team Explorer, right-click **All Build Definitions,** and then choose **Queue New Build**.</span></span>

## <a name="4-publish-a-package-using-a-powershell-script"></a><span data-ttu-id="8a71c-177">4: publicera ett paket med hjälp av ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="8a71c-177">4: Publish a Package using a PowerShell Script</span></span>
<span data-ttu-id="8a71c-178">Det här avsnittet beskrivs hur tooconstruct ett Windows PowerShell-skript som ska publicera hello Cloud app-paket utdata tooAzure med valfria parametrar.</span><span class="sxs-lookup"><span data-stu-id="8a71c-178">This section describes how tooconstruct a Windows PowerShell script that will publish hello Cloud app package output tooAzure using optional parameters.</span></span> <span data-ttu-id="8a71c-179">Det här skriptet kan anropas efter hello build steg i din anpassade build-automation.</span><span class="sxs-lookup"><span data-stu-id="8a71c-179">This script can be called after hello build step in your custom build automation.</span></span> <span data-ttu-id="8a71c-180">Det kan också anropas från processmall arbetsflödesaktiviteter i Visual Studio TFS Team skapa.</span><span class="sxs-lookup"><span data-stu-id="8a71c-180">It can also be called from Process Template workflow activities in Visual Studio TFS Team Build.</span></span>

1. <span data-ttu-id="8a71c-181">Installera hello [Azure PowerShell-cmdlets] [ Azure PowerShell cmdlets] (v0.6.1 eller högre).</span><span class="sxs-lookup"><span data-stu-id="8a71c-181">Install hello [Azure PowerShell cmdlets][Azure PowerShell cmdlets] (v0.6.1 or higher).</span></span>
   <span data-ttu-id="8a71c-182">Under installationsfasen för hello cmdlet, välja tooinstall som en snapin-modul.</span><span class="sxs-lookup"><span data-stu-id="8a71c-182">During hello cmdlet setup phase, choose tooinstall as a snap-in.</span></span> <span data-ttu-id="8a71c-183">Observera att den versionen som stöds officiellt ersätter hello äldre version som erbjuds via CodePlex, även om tidigare versioner av hello har numrerade 2.x.x.</span><span class="sxs-lookup"><span data-stu-id="8a71c-183">Note that this officially supported version replaces hello older version offered through CodePlex, although hello previous versions were numbered 2.x.x.</span></span>
2. <span data-ttu-id="8a71c-184">Starta Azure PowerShell med hjälp av hello Start-menyn eller startsidan.</span><span class="sxs-lookup"><span data-stu-id="8a71c-184">Start Azure PowerShell using hello Start menu or Start page.</span></span> <span data-ttu-id="8a71c-185">Om du startar i det här sättet kommer att laddas hello Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="8a71c-185">If you start in this way, hello Azure PowerShell cmdlets will be loaded.</span></span>
3. <span data-ttu-id="8a71c-186">I hello PowerShell-Kommandotolken, kontrollera att PowerShell-cmdlets för hello laddas genom att ange hello partiella kommando `Get-Azure` och sedan trycka på hello tabbtangenten för instruktionen slutfördes.</span><span class="sxs-lookup"><span data-stu-id="8a71c-186">At hello PowerShell prompt, verify that hello PowerShell cmdlets are loaded by entering hello partial command `Get-Azure` and then pressing hello Tab key for statement completion.</span></span>

   <span data-ttu-id="8a71c-187">Om du trycker på tabbtangenten hello upprepade gånger, bör du se olika Azure PowerShell-kommandon.</span><span class="sxs-lookup"><span data-stu-id="8a71c-187">If you press hello Tab key repeatedly, you should see various Azure PowerShell commands.</span></span>
4. <span data-ttu-id="8a71c-188">Kontrollera att du kan ansluta tooyour Azure-prenumeration genom att importera din prenumerationsinformation från hello .publishsettings-fil.</span><span class="sxs-lookup"><span data-stu-id="8a71c-188">Verify that you can connect tooyour Azure subscription by importing your subscription information from hello .publishsettings file.</span></span>

   `Import-AzurePublishSettingsFile c:\scripts\WindowsAzure\default.publishsettings`

   <span data-ttu-id="8a71c-189">Ange hello kommando</span><span class="sxs-lookup"><span data-stu-id="8a71c-189">Then enter hello command</span></span>

   `Get-AzureSubscription`

   <span data-ttu-id="8a71c-190">Detta visar information om din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8a71c-190">This shows information about your subscription.</span></span> <span data-ttu-id="8a71c-191">Kontrollera att allt är korrekt.</span><span class="sxs-lookup"><span data-stu-id="8a71c-191">Verify that everything is correct.</span></span>
5. <span data-ttu-id="8a71c-192">Spara hello skript-mall hello slutet av den här artikeln till mappen skript c:\\skript\\WindowsAzure\\**PublishCloudService.ps1**.</span><span class="sxs-lookup"><span data-stu-id="8a71c-192">Save hello script template provided at hello end of this article to your scripts folder as c:\\scripts\\WindowsAzure\\**PublishCloudService.ps1**.</span></span>
6. <span data-ttu-id="8a71c-193">Granska hello parametrar avsnitt av hello skriptet.</span><span class="sxs-lookup"><span data-stu-id="8a71c-193">Review hello parameters section of hello script.</span></span> <span data-ttu-id="8a71c-194">Lägg till eller ändra alla standardvärden.</span><span class="sxs-lookup"><span data-stu-id="8a71c-194">Add or modify any default values.</span></span> <span data-ttu-id="8a71c-195">Dessa värden kan alltid åsidosättas genom att passera i explicit parametrar.</span><span class="sxs-lookup"><span data-stu-id="8a71c-195">These values can always be overridden by passing in explicit parameters.</span></span>
7. <span data-ttu-id="8a71c-196">Se till att det finns giltiga Molntjänsten och lagringskonton som skapats i din prenumeration som kan vara mål för hello publicera skript.</span><span class="sxs-lookup"><span data-stu-id="8a71c-196">Ensure there are valid cloud service and storage accounts created in your subscription that can be targeted by hello publish script.</span></span> <span data-ttu-id="8a71c-197">Storage-konto (blob storage) kommer att använda tooupload och tillfälligt lagra hello distribution av paket och config-filen medan distributionen skapas.</span><span class="sxs-lookup"><span data-stu-id="8a71c-197">The storage account (blob storage) will be used tooupload and temporarily store hello deployment package and config file while the deployment is being created.</span></span>

   * <span data-ttu-id="8a71c-198">toocreate en ny molntjänst, kan du anropa den här skript eller Använd hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8a71c-198">toocreate a new cloud service, you can call this script or use hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8a71c-199">Hej molntjänstnamnet kommer att användas som ett prefix i ett fullständigt kvalificerat domännamn och därför det måste vara unikt.</span><span class="sxs-lookup"><span data-stu-id="8a71c-199">hello cloud service name will be used as a prefix in a fully qualified domain name and hence it must be unique.</span></span>

         New-AzureService -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
   * <span data-ttu-id="8a71c-200">toocreate ett nytt lagringskonto, kan du anropa den här skript eller Använd hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8a71c-200">toocreate a new storage account, you can call this script or use hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="8a71c-201">Hej lagringskontonamn används som ett prefix i ett fullständigt kvalificerat domännamn och därför det måste vara unikt.</span><span class="sxs-lookup"><span data-stu-id="8a71c-201">hello storage account name will be used as a prefix in a fully qualified domain name and hence it must be unique.</span></span> <span data-ttu-id="8a71c-202">Du kan försöka med hello samma namn som Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="8a71c-202">You can try using hello same name as the cloud service.</span></span>

         New-AzureStorageAccount -ServiceName "mytestcloudservice" -Location "North Central US" -Label "mytestcloudservice"
8. <span data-ttu-id="8a71c-203">Anropa hello skript direkt från Azure PowerShell eller tråd in det här skriptet tooyour värden build automation toooccur efter hello paketet build.</span><span class="sxs-lookup"><span data-stu-id="8a71c-203">Call hello script directly from Azure PowerShell, or wire up this script tooyour host build automation toooccur after hello package build.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="8a71c-204">hello-skriptet kommer alltid att ta bort eller ersätta din befintliga distributioner som standard om de upptäcks.</span><span class="sxs-lookup"><span data-stu-id="8a71c-204">hello script will always delete or replace your existing deployments by default if they are detected.</span></span> <span data-ttu-id="8a71c-205">Detta är nödvändigt för att aktivera kontinuerlig leverans från automation när inga användare att fråga är möjligt.</span><span class="sxs-lookup"><span data-stu-id="8a71c-205">This is necessary to enable continuous delivery from automation where no user prompting is possible.</span></span>
   >
   >

   <span data-ttu-id="8a71c-206">**Exempelscenario 1:** kontinuerlig distribution toohello mellanlagring miljö för en tjänst:</span><span class="sxs-lookup"><span data-stu-id="8a71c-206">**Example scenario 1:** continuous deployment toohello staging environment of a service:</span></span>

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Staging -serviceName mycloudservice -storageAccountName mystoragesaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   <span data-ttu-id="8a71c-207">Detta är vanligtvis följas upp av test kör verifieringen och en VIP-växling.</span><span class="sxs-lookup"><span data-stu-id="8a71c-207">This is typically followed up by test run verification and a VIP swap.</span></span> <span data-ttu-id="8a71c-208">hello VIP-växlingen kan göras via hello [Azure-portalen](https://portal.azure.com) eller med cmdleten hello Move-distribution.</span><span class="sxs-lookup"><span data-stu-id="8a71c-208">hello VIP swap can be done via hello [Azure portal](https://portal.azure.com) or by using hello Move-Deployment cmdlet.</span></span>

   <span data-ttu-id="8a71c-209">**Exempelscenario 2:** kontinuerlig distribution toohello produktionsmiljön för en dedikerad test-tjänst</span><span class="sxs-lookup"><span data-stu-id="8a71c-209">**Example scenario 2:** continuous deployment toohello production environment of a dedicated test service</span></span>

       PowerShell c:\scripts\windowsazure\PublishCloudService.ps1 -environment Production -enableDeploymentUpgrade 1 -serviceName mycloudservice -storageAccountName mystorageaccount -packageLocation c:\drops\app.publish\ContactManager.Azure.cspkg -cloudConfigLocation c:\drops\app.publish\ServiceConfiguration.Cloud.cscfg -subscriptionDataFile c:\scripts\default.publishsettings

   <span data-ttu-id="8a71c-210">**Fjärrskrivbord:**</span><span class="sxs-lookup"><span data-stu-id="8a71c-210">**Remote Desktop:**</span></span>

   <span data-ttu-id="8a71c-211">Om Fjärrskrivbord har aktiverats i Azure-projekt behöver tooperform ytterligare enstaka steg tooensure hello rätt moln Tjänstcertifikatet har överförts tooall molntjänster som mål för det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="8a71c-211">If Remote Desktop is enabled in your Azure project you will need tooperform additional one-time steps tooensure hello correct Cloud Service Certificate is uploaded tooall cloud services targeted by this script.</span></span>

   <span data-ttu-id="8a71c-212">Leta upp hello certifikat-tumavtryck värden förväntades av rollerna.</span><span class="sxs-lookup"><span data-stu-id="8a71c-212">Locate hello certificate thumbprint values expected by your roles.</span></span> <span data-ttu-id="8a71c-213">Tumavtryck för värden visas under hello certifikat i molnet config-fil (d.v.s. ServiceConfiguration.Cloud.cscfg).</span><span class="sxs-lookup"><span data-stu-id="8a71c-213">The thumbprint values are visible in hello Certificates section of the cloud config file (i.e. ServiceConfiguration.Cloud.cscfg).</span></span> <span data-ttu-id="8a71c-214">Det är också synliga i dialogrutan för hello konfiguration av fjärrskrivbord i Visual Studio när du visa alternativ och visa hello valt certifikat.</span><span class="sxs-lookup"><span data-stu-id="8a71c-214">It is also visible in hello Remote Desktop Configuration dialog in Visual Studio when you Show Options and view hello selected certificate.</span></span>

       <Certificates>
             <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="C33B6C432C25581601B84C80F86EC2809DC224E8" thumbprintAlgorithm="sha1" />
       </Certificates>

   <span data-ttu-id="8a71c-215">Överför certifikat för fjärrskrivbord som ett enstaka installationen steg med hjälp av hello följande cmdlet-skript:</span><span class="sxs-lookup"><span data-stu-id="8a71c-215">Upload Remote Desktop certificates as a one-time setup step using hello following cmdlet script:</span></span>

       Add-AzureCertificate -serviceName <CLOUDSERVICENAME> -certToDeploy (get-item cert:\CurrentUser\MY\<THUMBPRINT>)

   <span data-ttu-id="8a71c-216">Exempel:</span><span class="sxs-lookup"><span data-stu-id="8a71c-216">For example:</span></span>

       Add-AzureCertificate -serviceName 'mytestcloudservice' -certToDeploy (get-item cert:\CurrentUser\MY\C33B6C432C25581601B84C80F86EC2809DC224E8

   <span data-ttu-id="8a71c-217">Du kan också exportera hello certifikatfil PFX med privata nyckeln och överför certifikat tooeach mål molnbaserad tjänst med hjälp av den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8a71c-217">Alternatively you can export hello certificate file PFX with private key and upload certificates tooeach target cloud service using the [Azure portal](https://portal.azure.com).</span></span>

   <!---
   Fixing broken links for Azure content migration from ACOM tooDOCS. I'm unable toofind a replacement links, so I'm commenting out this reference for now. hello author can investigate in hello future. "Read hello following article toolearn more: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx.
   -->
   <span data-ttu-id="8a71c-218">**Uppgradera jämfört med distributionen. Ta bort distributionen -\> ny distribution**</span><span class="sxs-lookup"><span data-stu-id="8a71c-218">**Upgrade Deployment vs. Delete Deployment -\> New Deployment**</span></span>

   <span data-ttu-id="8a71c-219">hello skript kommer som standard att utföra en Uppgraderingsdistribution ($enableDeploymentUpgrade = 1) när ingen parameter som skickas eller värdet 1 skickas explicit.</span><span class="sxs-lookup"><span data-stu-id="8a71c-219">hello script will by default perform an Upgrade Deployment ($enableDeploymentUpgrade = 1) when no parameter is passed in or the value 1 is passed explicitly.</span></span> <span data-ttu-id="8a71c-220">Detta har fördelen med att göra kortare tid än en fullständig distribution för enskild instanser.</span><span class="sxs-lookup"><span data-stu-id="8a71c-220">For single instances this has the advantage of taking less time than a full deployment.</span></span> <span data-ttu-id="8a71c-221">För instanser som kräver hög tillgänglighet har också hello fördelen att lämna vissa instanser körs samtidigt som andra uppgraderas (går din uppdateringsdomän), plus din VIP tas inte bort.</span><span class="sxs-lookup"><span data-stu-id="8a71c-221">For instances that require high availability this also has hello advantage of leaving some instances running while others are upgraded (walking your update domain), plus your VIP will not be deleted.</span></span>

   <span data-ttu-id="8a71c-222">Uppgradera distributionen kan inaktiveras i hello skript ($enableDeploymentUpgrade = 0) eller genom att skicka *- enableDeploymentUpgrade 0* som en parameter som ändrar skriptet beteende toofirst bort alla befintliga distributionen och sedan skapa en ny distribution.</span><span class="sxs-lookup"><span data-stu-id="8a71c-222">Upgrade Deployment can be disabled in hello script ($enableDeploymentUpgrade = 0) or by passing *-enableDeploymentUpgrade 0* as a parameter, which alters the script behavior toofirst delete any existing deployment and then create a new deployment.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="8a71c-223">hello-skriptet kommer alltid att ta bort eller ersätta din befintliga distributioner som standard om de upptäcks.</span><span class="sxs-lookup"><span data-stu-id="8a71c-223">hello script will always delete or replace your existing deployments by default if they are detected.</span></span> <span data-ttu-id="8a71c-224">Detta är nödvändigt för att aktivera kontinuerlig leverans från automation om ingen användare/operator fråga är möjligt.</span><span class="sxs-lookup"><span data-stu-id="8a71c-224">This is necessary to enable continuous delivery from automation where no user/operator prompting is possible.</span></span>
   >
   >

## <a name="5-publish-a-package-using-tfs-team-build"></a><span data-ttu-id="8a71c-225">5: publicera ett paket med TFS-teamet skapa</span><span class="sxs-lookup"><span data-stu-id="8a71c-225">5: Publish a Package using TFS Team Build</span></span>
<span data-ttu-id="8a71c-226">Det här valfria steget ansluter TFS Team skapa toohello skript skapade i steg 4, som hanterar publicering av hello paketet build tooAzure.</span><span class="sxs-lookup"><span data-stu-id="8a71c-226">This optional step connects TFS Team Build toohello script created in step 4, which handles publishing of hello package build tooAzure.</span></span> <span data-ttu-id="8a71c-227">Detta innebär att ändra hello processmall som används av build-definition så att det körs en publicera aktivitet hello slutet av hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="8a71c-227">This entails modifying hello Process Template used by your build definition so that it runs a Publish activity at hello end of hello workflow.</span></span> <span data-ttu-id="8a71c-228">hello publicera aktivitet körs i PowerShell-kommandot Skicka parametrar från hello build.</span><span class="sxs-lookup"><span data-stu-id="8a71c-228">hello Publish activity will execute your PowerShell command passing in parameters from hello build.</span></span> <span data-ttu-id="8a71c-229">Utdata från hello MSBuild mål och publicera skript ska skickas till hello standardversion utdata.</span><span class="sxs-lookup"><span data-stu-id="8a71c-229">Output of hello MSBuild targets and publish script will be piped into hello standard build output.</span></span>

1. <span data-ttu-id="8a71c-230">Redigera hello skapa Definition ansvarar för kontinuerlig distribution.</span><span class="sxs-lookup"><span data-stu-id="8a71c-230">Edit hello Build Definition responsible for continuous deploy.</span></span>
2. <span data-ttu-id="8a71c-231">Välj hello **processen** fliken.</span><span class="sxs-lookup"><span data-stu-id="8a71c-231">Select hello **Process** tab.</span></span>
3. <span data-ttu-id="8a71c-232">Följ [instruktionerna](http://msdn.microsoft.com/library/dd647551.aspx) tooadd en aktivitet projekt för hello skapa processmall ladda ned hello standardmallen, Lägg till den hello projektet och kontrollera i.</span><span class="sxs-lookup"><span data-stu-id="8a71c-232">Follow [these instructions](http://msdn.microsoft.com/library/dd647551.aspx) tooadd an Activity project for hello build process template, download hello default template, add it to hello project and check it in.</span></span> <span data-ttu-id="8a71c-233">Ge hello build processmall ett nytt namn, till exempel AzureBuildProcessTemplate.</span><span class="sxs-lookup"><span data-stu-id="8a71c-233">Give hello build process template a new name, such as AzureBuildProcessTemplate.</span></span>
4. <span data-ttu-id="8a71c-234">Returnera toohello **processen** fliken och använda **Visa detaljer** tooshow en lista över tillgängliga build processmallar.</span><span class="sxs-lookup"><span data-stu-id="8a71c-234">Return toohello **Process** tab, and use **Show Details** tooshow a list of available build process templates.</span></span> <span data-ttu-id="8a71c-235">Välj hello **ny...**  knappen och navigera toohello projekt du lagt till och checkats in.</span><span class="sxs-lookup"><span data-stu-id="8a71c-235">Choose hello **New...** button, and navigate toohello project you just added and checked in.</span></span> <span data-ttu-id="8a71c-236">Leta upp hello-mallen som du just skapade och välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="8a71c-236">Locate hello template you just created and choose **OK**.</span></span>
5. <span data-ttu-id="8a71c-237">Öppna hello valt processmall för redigering.</span><span class="sxs-lookup"><span data-stu-id="8a71c-237">Open hello selected Process Template for editing.</span></span> <span data-ttu-id="8a71c-238">Du kan öppna direkt i hello Workflow designer eller hello XML-redigerare toowork med hello XAML.</span><span class="sxs-lookup"><span data-stu-id="8a71c-238">You can open directly in hello Workflow designer or in hello XML editor toowork with hello XAML.</span></span>
6. <span data-ttu-id="8a71c-239">Lägg till hello efter nya argumentlista som separata poster hello argument-fliken i hello Arbetsflödesdesignern.</span><span class="sxs-lookup"><span data-stu-id="8a71c-239">Add hello following list of new arguments as separate line items in hello arguments tab of hello workflow designer.</span></span> <span data-ttu-id="8a71c-240">Alla argument bör ha riktningen = i och Skriv = sträng.</span><span class="sxs-lookup"><span data-stu-id="8a71c-240">All arguments should have direction=In and type=String.</span></span> <span data-ttu-id="8a71c-241">Dessa kommer att använda tooflow parametrar från hello build-definition i hello-arbetsflöde, vilken sedan get används toocall hello publicera skript.</span><span class="sxs-lookup"><span data-stu-id="8a71c-241">These will be used tooflow parameters from hello build definition into hello workflow, which then get used toocall hello publish script.</span></span>

       SubscriptionName
       StorageAccountName
       CloudConfigLocation
       PackageLocation
       Environment
       SubscriptionDataFileLocation
       PublishScriptLocation
       ServiceName

   ![Listan med argument][3]

   <span data-ttu-id="8a71c-243">hello motsvarande XAML ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="8a71c-243">hello corresponding XAML looks like this:</span></span>

       <Activity  _ />
         <x:Members>
           <x:Property Name="BuildSettings" Type="InArgument(mtbwa:BuildSettings)" />
           <x:Property Name="TestSpecs" Type="InArgument(mtbwa:TestSpecList)" />
           <x:Property Name="BuildNumberFormat" Type="InArgument(x:String)" />
           <x:Property Name="CleanWorkspace" Type="InArgument(mtbwa:CleanWorkspaceOption)" />
           <x:Property Name="RunCodeAnalysis" Type="InArgument(mtbwa:CodeAnalysisOption)" />
           <x:Property Name="SourceAndSymbolServerSettings" Type="InArgument(mtbwa:SourceAndSymbolServerSettings)" />
           <x:Property Name="AgentSettings" Type="InArgument(mtbwa:AgentSettings)" />
           <x:Property Name="AssociateChangesetsAndWorkItems" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateWorkItem" Type="InArgument(x:Boolean)" />
           <x:Property Name="DropBuild" Type="InArgument(x:Boolean)" />
           <x:Property Name="MSBuildArguments" Type="InArgument(x:String)" />
           <x:Property Name="MSBuildPlatform" Type="InArgument(mtbwa:ToolPlatform)" />
           <x:Property Name="PerformTestImpactAnalysis" Type="InArgument(x:Boolean)" />
           <x:Property Name="CreateLabel" Type="InArgument(x:Boolean)" />
           <x:Property Name="DisableTests" Type="InArgument(x:Boolean)" />
           <x:Property Name="GetVersion" Type="InArgument(x:String)" />
           <x:Property Name="PrivateDropLocation" Type="InArgument(x:String)" />
           <x:Property Name="Verbosity" Type="InArgument(mtbw:BuildVerbosity)" />
           <x:Property Name="Metadata" Type="mtbw:ProcessParameterMetadataCollection" />
           <x:Property Name="SupportedReasons" Type="mtbc:BuildReason" />
           <x:Property Name="SubscriptionName" Type="InArgument(x:String)" />
           <x:Property Name="StorageAccountName" Type="InArgument(x:String)" />
           <x:Property Name="CloudConfigLocation" Type="InArgument(x:String)" />
           <x:Property Name="PackageLocation" Type="InArgument(x:String)" />
           <x:Property Name="Environment" Type="InArgument(x:String)" />
           <x:Property Name="SubscriptionDataFileLocation" Type="InArgument(x:String)" />
           <x:Property Name="PublishScriptLocation" Type="InArgument(x:String)" />
           <x:Property Name="ServiceName" Type="InArgument(x:String)" />
         </x:Members>

         <this:Process.MSBuildArguments>
7. <span data-ttu-id="8a71c-244">Lägg till en ny sekvens hello slutet av körs på agenten:</span><span class="sxs-lookup"><span data-stu-id="8a71c-244">Add a new sequence at hello end of Run On Agent:</span></span>

   1. <span data-ttu-id="8a71c-245">Starta genom att lägga till en instruktion om aktiviteten toocheck efter en giltigt skriptfil.</span><span class="sxs-lookup"><span data-stu-id="8a71c-245">Start by adding an If Statement activity toocheck for a valid script file.</span></span> <span data-ttu-id="8a71c-246">Ange hello villkorsvärdet toothis:</span><span class="sxs-lookup"><span data-stu-id="8a71c-246">Set hello condition toothis value:</span></span>

          Not String.IsNullOrEmpty(PublishScriptLocation)
   2. <span data-ttu-id="8a71c-247">Hello sedan fall av hello om instruktionen, lägga till en ny sekvensaktivitet.</span><span class="sxs-lookup"><span data-stu-id="8a71c-247">In hello Then case of hello If Statement, add a new Sequence activity.</span></span> <span data-ttu-id="8a71c-248">Ange hello visa namnet too'Start publicera '</span><span class="sxs-lookup"><span data-stu-id="8a71c-248">Set hello display name too'Start publish'</span></span>
   3. <span data-ttu-id="8a71c-249">Lägg till följande lista över nya variabler som separata poster på fliken variabler av hello workflow designer med hello Start publicera sekvens fortfarande markerat.</span><span class="sxs-lookup"><span data-stu-id="8a71c-249">With hello Start publish sequence still selected, add the following list of new variables as separate line items in the variables tab of hello workflow designer.</span></span> <span data-ttu-id="8a71c-250">Alla variabler ska ha variabeltyp = sträng och omfånget = Start publicera.</span><span class="sxs-lookup"><span data-stu-id="8a71c-250">All variables should have Variable type =String and Scope=Start publish.</span></span> <span data-ttu-id="8a71c-251">Dessa kommer att använda tooflow parametrar från hello build-definition i arbetsflödet, vilken sedan get används toocall hello publicera skript.</span><span class="sxs-lookup"><span data-stu-id="8a71c-251">These will be used tooflow parameters from hello build definition into the workflow, which then get used toocall hello publish script.</span></span>

      * <span data-ttu-id="8a71c-252">SubscriptionDataFilePath av typen String</span><span class="sxs-lookup"><span data-stu-id="8a71c-252">SubscriptionDataFilePath, of type String</span></span>
      * <span data-ttu-id="8a71c-253">PublishScriptFilePath av typen String</span><span class="sxs-lookup"><span data-stu-id="8a71c-253">PublishScriptFilePath, of type String</span></span>

        ![Nya variabler][4]
   4. <span data-ttu-id="8a71c-255">Om du använder TFS 2012 eller tidigare, lägga till en ConvertWorkspaceItem aktivitet hello början av hello ny sekvens.</span><span class="sxs-lookup"><span data-stu-id="8a71c-255">If you are using TFS 2012 or earlier, add a ConvertWorkspaceItem activity at hello beginning of hello new Sequence.</span></span> <span data-ttu-id="8a71c-256">Om du använder TFS 2013 eller senare kan du lägga till en GetLocalPath aktivitet hello början av hello ny sekvens.</span><span class="sxs-lookup"><span data-stu-id="8a71c-256">If you are using TFS 2013 or later, add a GetLocalPath activity at hello beginning of hello new sequence.</span></span> <span data-ttu-id="8a71c-257">För en ConvertWorkspaceItem ange hello egenskaper på följande sätt: riktning = ServerToLocal, visningsnamn = 'Konvertera publicera skript filename', indata = PublishScriptLocation, resultat = PublishScriptFilePath, arbetsytan = 'Arbetsytan'.</span><span class="sxs-lookup"><span data-stu-id="8a71c-257">For a ConvertWorkspaceItem, set hello properties as follows: Direction=ServerToLocal, DisplayName='Convert publish script filename', Input=' PublishScriptLocation', Result='PublishScriptFilePath', Workspace='Workspace'.</span></span> <span data-ttu-id="8a71c-258">Ange hello egenskapen IncomingPath too'PublishScriptLocation för en GetLocalPath aktivitet ', och resultatet too'PublishScriptFilePath hello ”.</span><span class="sxs-lookup"><span data-stu-id="8a71c-258">For a GetLocalPath activity, set hello property IncomingPath too'PublishScriptLocation', and hello Result too'PublishScriptFilePath'.</span></span> <span data-ttu-id="8a71c-259">Den här aktiviteten konverterar hello sökvägen toohello publicera skript från platser för TFS-server (om tillämpligt) tooa standard lokal disk sökväg.</span><span class="sxs-lookup"><span data-stu-id="8a71c-259">This activity converts hello path toohello publish script from TFS server locations (if applicable) tooa standard local disk path.</span></span>
   5. <span data-ttu-id="8a71c-260">Om du använder TFS 2012 eller tidigare, lägga till en annan ConvertWorkspaceItem aktivitet hello slutet av hello ny sekvens.</span><span class="sxs-lookup"><span data-stu-id="8a71c-260">If you are using TFS 2012 or earlier, add another ConvertWorkspaceItem activity at hello end of hello new Sequence.</span></span> <span data-ttu-id="8a71c-261">Riktning = ServerToLocal DisplayName = konvertera prenumerationen filnamn, indata = SubscriptionDataFileLocation, resultat = SubscriptionDataFilePath, arbetsytan = 'Arbetsytan'.</span><span class="sxs-lookup"><span data-stu-id="8a71c-261">Direction=ServerToLocal, DisplayName='Convert subscription filename', Input=' SubscriptionDataFileLocation', Result= 'SubscriptionDataFilePath', Workspace='Workspace'.</span></span> <span data-ttu-id="8a71c-262">Om du använder TFS 2013 eller senare, lägga till en annan GetLocalPath.</span><span class="sxs-lookup"><span data-stu-id="8a71c-262">If you are using TFS 2013 or later, add another GetLocalPath.</span></span> <span data-ttu-id="8a71c-263">IncomingPath = SubscriptionDataFileLocation, och resultatet = SubscriptionDataFilePath.</span><span class="sxs-lookup"><span data-stu-id="8a71c-263">IncomingPath='SubscriptionDataFileLocation', and Result='SubscriptionDataFilePath.'</span></span>
   6. <span data-ttu-id="8a71c-264">Lägga till en InvokeProcess aktivitet hello slutet av hello ny sekvens.</span><span class="sxs-lookup"><span data-stu-id="8a71c-264">Add an InvokeProcess activity at hello end of hello new Sequence.</span></span>
      <span data-ttu-id="8a71c-265">Den här aktiviteten anrop PowerShell.exe med hello argument skickades av hello skapa Definition.</span><span class="sxs-lookup"><span data-stu-id="8a71c-265">This activity calls PowerShell.exe with hello arguments passed in by hello Build Definition.</span></span>

      + <span data-ttu-id="8a71c-266">Argument = String.Format (”-filen” ”{0}” ”- serviceName {1} - storageAccountName {2} - anges i packageLocation” ”{3}” ”- cloudConfigLocation” ”{4}” ”- subscriptionDataFile” ”{5}” ”- selectedSubscription {6}-miljön” ”{7}” ””, PublishScriptFilePath, ServiceName, StorageAccountName, anges i PackageLocation, CloudConfigLocation, SubscriptionDataFilePath, SubscriptionName, miljön)</span><span class="sxs-lookup"><span data-stu-id="8a71c-266">Arguments = String.Format(" -File ""{0}"" -serviceName {1}  -storageAccountName {2} -packageLocation ""{3}""  -cloudConfigLocation ""{4}"" -subscriptionDataFile ""{5}""  -selectedSubscription {6} -environment ""{7}""",  PublishScriptFilePath, ServiceName, StorageAccountName,  PackageLocation, CloudConfigLocation,  SubscriptionDataFilePath, SubscriptionName, Environment)</span></span>
      + <span data-ttu-id="8a71c-267">DisplayName = Execute publicera skript</span><span class="sxs-lookup"><span data-stu-id="8a71c-267">DisplayName = Execute publish script</span></span>
      + <span data-ttu-id="8a71c-268">FileName = ”PowerShell” (citattecken för hello)</span><span class="sxs-lookup"><span data-stu-id="8a71c-268">FileName = "PowerShell" (include hello quotes)</span></span>
      + <span data-ttu-id="8a71c-269">OutputEncoding = System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)</span><span class="sxs-lookup"><span data-stu-id="8a71c-269">OutputEncoding=  System.Text.Encoding.GetEncoding(System.Globalization.CultureInfo.InstalledUICulture.TextInfo.OEMCodePage)</span></span>
   7. <span data-ttu-id="8a71c-270">I hello **hantera standardutdata** avsnittet textruta för InvokeProcess genom att ange hello textruta värdet too'data'.</span><span class="sxs-lookup"><span data-stu-id="8a71c-270">In hello **Handle Standard Output** section textbox of the InvokeProcess, set hello textbox value too'data'.</span></span> <span data-ttu-id="8a71c-271">Här är en variabel toostore hello standardutdata data.</span><span class="sxs-lookup"><span data-stu-id="8a71c-271">This is a variable toostore hello standard output data.</span></span>
   8. <span data-ttu-id="8a71c-272">Lägga till en WriteBuildMessage aktivitet under hello **hantera standardutdata** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8a71c-272">Add a WriteBuildMessage activity just below hello **Handle Standard Output** section.</span></span> <span data-ttu-id="8a71c-273">Ange hello vikten = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' och hello Message = ”data”.</span><span class="sxs-lookup"><span data-stu-id="8a71c-273">Set hello Importance = 'Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High' and hello Message='data'.</span></span> <span data-ttu-id="8a71c-274">Detta säkerställer att hello standard utdata från skriptet hämta skrivs toohello build-utdata.</span><span class="sxs-lookup"><span data-stu-id="8a71c-274">This ensures hello standard output of the script will get written toohello build output.</span></span>
   9. <span data-ttu-id="8a71c-275">I hello **hantera Felutdata** avsnittet textruta för InvokeProcess genom att ange hello textruta värdet too'data'.</span><span class="sxs-lookup"><span data-stu-id="8a71c-275">In hello **Handle Error Output** section textbox of the InvokeProcess, set hello textbox value too'data'.</span></span> <span data-ttu-id="8a71c-276">Här är en variabel toostore hello standardfel data.</span><span class="sxs-lookup"><span data-stu-id="8a71c-276">This is a variable toostore hello standard error data.</span></span>
   10. <span data-ttu-id="8a71c-277">Lägga till en WriteBuildError aktivitet under hello **hantera Felutdata** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8a71c-277">Add a WriteBuildError activity just below hello **Handle Error Output** section.</span></span> <span data-ttu-id="8a71c-278">Ange hello Message = ”data”.</span><span class="sxs-lookup"><span data-stu-id="8a71c-278">Set hello Message='data'.</span></span> <span data-ttu-id="8a71c-279">Detta säkerställer att hello standardfel hello skriptet hämta skrivs utdata för toohello build-fel.</span><span class="sxs-lookup"><span data-stu-id="8a71c-279">This ensures hello standard errors of hello script will get written toohello build error output.</span></span>
   11. <span data-ttu-id="8a71c-280">Korrigera eventuella fel identifieras med blå utropstecken märken.</span><span class="sxs-lookup"><span data-stu-id="8a71c-280">Correct any errors, indicated by blue exclamation marks.</span></span> <span data-ttu-id="8a71c-281">Hovra över utropstecken märken tooget en ledtråd om hello felet.</span><span class="sxs-lookup"><span data-stu-id="8a71c-281">Hover over the exclamation marks tooget a hint about hello error.</span></span> <span data-ttu-id="8a71c-282">Spara hello arbetsflöde om du vill rensa fel.</span><span class="sxs-lookup"><span data-stu-id="8a71c-282">Save hello workflow to clear errors.</span></span>

   <span data-ttu-id="8a71c-283">hello slutresultatet av hello publicera arbetsflödet aktiviteter ser ut så här i hello-designer:</span><span class="sxs-lookup"><span data-stu-id="8a71c-283">hello final result of hello publish workflow activities will look like this in hello designer:</span></span>

   ![Arbetsflödesaktiviteter][5]

   <span data-ttu-id="8a71c-285">hello slutresultatet av hello publicera arbetsflödet aktiviteter ser ut så här i XAML:</span><span class="sxs-lookup"><span data-stu-id="8a71c-285">hello final result of hello publish workflow activities will look like this in XAML:</span></span>

       <If Condition="[Not String.IsNullOrEmpty(PublishScriptLocation)]" sap2010:WorkflowViewState.IdRef="If_1">
           <If.Then>
             <Sequence DisplayName="Start Publish" sap2010:WorkflowViewState.IdRef="Sequence_4">
               <Sequence.Variables>
                 <Variable x:TypeArguments="x:String" Name="SubscriptionDataFilePath" />
                 <Variable x:TypeArguments="x:String" Name="PublishScriptFilePath" />
               </Sequence.Variables>
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert publish script filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_1" Input="[PublishScriptLocation]" Result="[PublishScriptFilePath]" Workspace="[Workspace]" />
               <mtbwa:ConvertWorkspaceItem DisplayName="Convert subscription filename" sap2010:WorkflowViewState.IdRef="ConvertWorkspaceItem_2" Input="[SubscriptionDataFileLocation]" Result="[SubscriptionDataFilePath]" Workspace="[Workspace]" />
               <mtbwa:InvokeProcess Arguments="[String.Format(&quot; -File &quot;&quot;{0}&quot;&quot; -serviceName {1}&#xD;&#xA;            -storageAccountName {2} -packageLocation &quot;&quot;{3}&quot;&quot;&#xD;&#xA;            -cloudConfigLocation &quot;&quot;{4}&quot;&quot; -subscriptionDataFile &quot;&quot;{5}&quot;&quot;&#xD;&#xA;            -selectedSubscription {6} -environment &quot;&quot;{7}&quot;&quot;&quot;,&#xD;&#xA;            PublishScriptFilePath, ServiceName, StorageAccountName,&#xD;&#xA;            PackageLocation, CloudConfigLocation,&#xD;&#xA;            SubscriptionDataFilePath, SubscriptionName, Environment)]" DisplayName="'Execute Publish Script'" FileName="[PowerShell]" sap2010:WorkflowViewState.IdRef="InvokeProcess_1">
                 <mtbwa:InvokeProcess.ErrorDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildError Message="{x:Null}" sap2010:WorkflowViewState.IdRef="WriteBuildError_1" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.ErrorDataReceived>
                 <mtbwa:InvokeProcess.OutputDataReceived>
                   <ActivityAction x:TypeArguments="x:String">
                     <ActivityAction.Argument>
                       <DelegateInArgument x:TypeArguments="x:String" Name="data" />
                     </ActivityAction.Argument>
                     <mtbwa:WriteBuildMessage sap2010:WorkflowViewState.IdRef="WriteBuildMessage_2" Importance="[Microsoft.TeamFoundation.Build.Client.BuildMessageImportance.High]" Message="[data]" mva:VisualBasic.Settings="Assembly references and imported namespaces serialized as XML namespaces" />
                   </ActivityAction>
                 </mtbwa:InvokeProcess.OutputDataReceived>
               </mtbwa:InvokeProcess>
             </Sequence>
           </If.Then>
         </If>
       </Sequence>
8. <span data-ttu-id="8a71c-286">Spara hello build processen mall arbetsflödet och checka In den här filen.</span><span class="sxs-lookup"><span data-stu-id="8a71c-286">Save hello build process template workflow and Check In this file.</span></span>
9. <span data-ttu-id="8a71c-287">Redigera definition av hello build (Stäng den om den redan är öppen), och välj hello **ny** knappen om du inte ser hello mall i hello lista över processmallar ännu.</span><span class="sxs-lookup"><span data-stu-id="8a71c-287">Edit hello build definition (close it if it is already open), and select hello **New** button if you do not yet see hello new template in hello list of Process Templates.</span></span>
10. <span data-ttu-id="8a71c-288">Ange egenskapsvärden för hello-parametern i hello övrigt avsnittet enligt följande:</span><span class="sxs-lookup"><span data-stu-id="8a71c-288">Set hello parameter property values in hello Misc section as follows:</span></span>

    1. <span data-ttu-id="8a71c-289">CloudConfigLocation = ”c:\\släpper\\app.publish\\ServiceConfiguration.Cloud.cscfg' *det här värdet är härledd från: ($PublishDir)ServiceConfiguration.Cloud.cscfg*</span><span class="sxs-lookup"><span data-stu-id="8a71c-289">CloudConfigLocation ='c:\\drops\\app.publish\\ServiceConfiguration.Cloud.cscfg' *This value is derived from: ($PublishDir)ServiceConfiguration.Cloud.cscfg*</span></span>
    2. <span data-ttu-id="8a71c-290">Anges i PackageLocation = ' c:\\släpper\\app.publish\\ContactManager.Azure.cspkg' *det här värdet är härledd från: ($PublishDir)($ProjectName) .cspkg*</span><span class="sxs-lookup"><span data-stu-id="8a71c-290">PackageLocation = 'c:\\drops\\app.publish\\ContactManager.Azure.cspkg' *This value is derived from: ($PublishDir)($ProjectName).cspkg*</span></span>
    3. <span data-ttu-id="8a71c-291">PublishScriptLocation = ”c:\\skript\\WindowsAzure\\PublishCloudService.ps1'</span><span class="sxs-lookup"><span data-stu-id="8a71c-291">PublishScriptLocation = 'c:\\scripts\\WindowsAzure\\PublishCloudService.ps1'</span></span>
    4. <span data-ttu-id="8a71c-292">ServiceName = 'mycloudservicename' *Använd hello lämpliga molntjänstnamnet här*</span><span class="sxs-lookup"><span data-stu-id="8a71c-292">ServiceName = 'mycloudservicename' *Use hello appropriate cloud service name here*</span></span>
    5. <span data-ttu-id="8a71c-293">Miljö = ”mellanlagring”</span><span class="sxs-lookup"><span data-stu-id="8a71c-293">Environment = 'Staging'</span></span>
    6. <span data-ttu-id="8a71c-294">StorageAccountName = 'mystorageaccountname' *Använd hello lämpliga lagringskontonamnet här*</span><span class="sxs-lookup"><span data-stu-id="8a71c-294">StorageAccountName = 'mystorageaccountname' *Use hello appropriate storage account name here*</span></span>
    7. <span data-ttu-id="8a71c-295">SubscriptionDataFileLocation = ”c:\\skript\\WindowsAzure\\Subscription.xml'</span><span class="sxs-lookup"><span data-stu-id="8a71c-295">SubscriptionDataFileLocation = 'c:\\scripts\\WindowsAzure\\Subscription.xml'</span></span>
    8. <span data-ttu-id="8a71c-296">SubscriptionName = 'default'</span><span class="sxs-lookup"><span data-stu-id="8a71c-296">SubscriptionName = 'default'</span></span>

    ![Parametern egenskapsvärden][6]
11. <span data-ttu-id="8a71c-298">Spara hello ändringar toohello skapa Definition.</span><span class="sxs-lookup"><span data-stu-id="8a71c-298">Save hello changes toohello Build Definition.</span></span>
12. <span data-ttu-id="8a71c-299">Kö ett Build-tooexecute både hello paketet build och publicera.</span><span class="sxs-lookup"><span data-stu-id="8a71c-299">Queue a Build tooexecute both hello package build and publish.</span></span> <span data-ttu-id="8a71c-300">Om du har en utlösare ange tooContinuous Integration, körs det här problemet på varje incheckning.</span><span class="sxs-lookup"><span data-stu-id="8a71c-300">If you have a trigger set tooContinuous Integration, you will execute this behavior on every check-in.</span></span>

### <a name="publishcloudserviceps1-script-template"></a><span data-ttu-id="8a71c-301">PublishCloudService.ps1 skript mall</span><span class="sxs-lookup"><span data-stu-id="8a71c-301">PublishCloudService.ps1 script template</span></span>
```
Param(  $serviceName = "",
        $storageAccountName = "",
        $packageLocation = "",
        $cloudConfigLocation = "",
        $environment = "Staging",
        $deploymentLabel = "ContinuousDeploy too$servicename",
        $timeStampFormat = "g",
        $alwaysDeleteExistingDeployments = 1,
        $enableDeploymentUpgrade = 1,
        $selectedsubscription = "default",
        $subscriptionDataFile = ""
     )


function Publish()
{
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot -ErrorVariable a -ErrorAction silentlycontinue
    if ($a[0] -ne $null)
    {
        Write-Output "$(Get-Date -f $timeStampFormat) - No deployment is detected. Creating a new deployment. "
    }
    #check for existing deployment and then either upgrade, delete + deploy, or cancel according too$alwaysDeleteExistingDeployments and $enableDeploymentUpgrade boolean variables
    if ($deployment.Name -ne $null)
    {
        switch ($alwaysDeleteExistingDeployments)
        {
            1
            {
                switch ($enableDeploymentUpgrade)
                {
                    1  #Update deployment inplace (usually faster, cheaper, won't destroy VIP)
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Upgrading deployment."
                        UpgradeDeployment
                    }
                    0  #Delete then create new deployment
                    {
                        Write-Output "$(Get-Date -f $timeStampFormat) - Deployment exists in $servicename.  Deleting deployment."
                        DeleteDeployment
                        CreateNewDeployment

                    }
                } # switch ($enableDeploymentUpgrade)
            }
            0
            {
                Write-Output "$(Get-Date -f $timeStampFormat) - ERROR: Deployment exists in $servicename.  Script execution cancelled."
                exit
            }
        } #switch ($alwaysDeleteExistingDeployments)
    } else {
            CreateNewDeployment
    }
}

function CreateNewDeployment()
{
    write-progress -id 3 -activity "Creating New Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: In progress"

    $opstat = New-AzureDeployment -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Creating New Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Creating New Deployment: Complete, Deployment ID: $completeDeploymentID"

    StartInstances
}

function UpgradeDeployment()
{
    write-progress -id 3 -activity "Upgrading Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: In progress"

    # perform Update-Deployment
    $setdeployment = Set-AzureDeployment -Upgrade -Slot $slot -Package $packageLocation -Configuration $cloudConfigLocation -label $deploymentLabel -ServiceName $serviceName -Force

    $completeDeployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $completeDeploymentID = $completeDeployment.deploymentid

    write-progress -id 3 -activity "Upgrading Deployment" -completed -Status "Complete"
    Write-Output "$(Get-Date -f $timeStampFormat) - Upgrading Deployment: Complete, Deployment ID: $completeDeploymentID"
}

function DeleteDeployment()
{

    write-progress -id 2 -activity "Deleting Deployment" -Status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: In progress"

    #WARNING - always deletes with force
    $removeDeployment = Remove-AzureDeployment -Slot $slot -ServiceName $serviceName -Force

    write-progress -id 2 -activity "Deleting Deployment: Complete" -completed -Status $removeDeployment
    Write-Output "$(Get-Date -f $timeStampFormat) - Deleting Deployment: Complete"

}

function StartInstances()
{
    write-progress -id 4 -activity "Starting Instances" -status "In progress"
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: In progress"

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $runstatus = $deployment.Status

    if ($runstatus -ne 'Running')
    {
        $run = Set-AzureDeployment -Slot $slot -ServiceName $serviceName -Status Running
    }
    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $oldStatusStr = @("") * $deployment.RoleInstanceList.Count

    while (-not(AllInstancesRunning($deployment.RoleInstanceList)))
    {
        $i = 1
        foreach ($roleInstance in $deployment.RoleInstanceList)
        {
            $instanceName = $roleInstance.InstanceName
            $instanceStatus = $roleInstance.InstanceStatus

            if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
            {
                $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
                Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
            }

            write-progress -id (4 + $i) -activity "Starting Instance '$instanceName'" -status "$instanceStatus"
            $i = $i + 1
        }

        sleep -Seconds 1

        $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    }

    $i = 1
    foreach ($roleInstance in $deployment.RoleInstanceList)
    {
        $instanceName = $roleInstance.InstanceName
        $instanceStatus = $roleInstance.InstanceStatus

        if ($oldStatusStr[$i - 1] -ne $roleInstance.InstanceStatus)
        {
            $oldStatusStr[$i - 1] = $roleInstance.InstanceStatus
            Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instance '$instanceName': $instanceStatus"
        }

        $i = $i + 1
    }

    $deployment = Get-AzureDeployment -ServiceName $serviceName -Slot $slot
    $opstat = $deployment.Status

    write-progress -id 4 -activity "Starting Instances" -completed -status $opstat
    Write-Output "$(Get-Date -f $timeStampFormat) - Starting Instances: $opstat"
}

function AllInstancesRunning($roleInstanceList)
{
    foreach ($roleInstance in $roleInstanceList)
    {
        if ($roleInstance.InstanceStatus -ne "ReadyRole")
        {
            return $false
        }
    }

    return $true
}

#configure powershell with Azure 1.7 modules
Import-Module Azure

#configure powershell with publishsettings for your subscription
$pubsettings = $subscriptionDataFile
Import-AzurePublishSettingsFile $pubsettings
Set-AzureSubscription -CurrentStorageAccountName $storageAccountName -SubscriptionName $selectedsubscription
Select-AzureSubscription $selectedsubscription

#set remaining environment variables for Azure cmdlets
$subscription = Get-AzureSubscription $selectedsubscription
$subscriptionname = $subscription.subscriptionname
$subscriptionid = $subscription.subscriptionid
$slot = $environment

#main driver - publish & write progress tooactivity log
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script started."
Write-Output "$(Get-Date -f $timeStampFormat) - Preparing deployment of $deploymentLabel for $subscriptionname with Subscription ID $subscriptionid."

Publish

$deployment = Get-AzureDeployment -slot $slot -serviceName $servicename
$deploymentUrl = $deployment.Url

Write-Output "$(Get-Date -f $timeStampFormat) - Created Cloud Service with URL $deploymentUrl."
Write-Output "$(Get-Date -f $timeStampFormat) - Azure Cloud Service deploy script finished."
```

## <a name="next-steps"></a><span data-ttu-id="8a71c-302">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8a71c-302">Next steps</span></span>
<span data-ttu-id="8a71c-303">tooenable fjärrfelsökning när du använder kontinuerlig leverans finns [aktivera fjärrfelsökning när du använder kontinuerlig leverans toopublish tooAzure](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).</span><span class="sxs-lookup"><span data-stu-id="8a71c-303">tooenable remote debugging when using continuous delivery, see [Enable remote debugging when using continuous delivery toopublish tooAzure](cloud-services-virtual-machines-dotnet-continuous-delivery-remote-debugging.md).</span></span>

[Team Foundation Build Service]: https://msdn.microsoft.com/library/ee259687.aspx
[.NET Framework 4]: https://www.microsoft.com/download/details.aspx?id=17851
[.NET Framework 4.5]: https://www.microsoft.com/download/details.aspx?id=30653
[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42643
[Scale out your build system]: https://msdn.microsoft.com/library/dd793166.aspx
[Deploy and configure a build server]: https://msdn.microsoft.com/library/ms181712.aspx
[Azure PowerShell cmdlets]: /powershell/azureps-cmdlets-docs
[hello .publishsettings file]: https://manage.windowsazure.com/download/publishprofile.aspx?wa=wsignin1.0
[0]: ./media/cloud-services-dotnet-continuous-delivery/tfs-01bc.png
[2]: ./media/cloud-services-dotnet-continuous-delivery/tfs-02.png
[3]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-03.png
[4]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-04.png
[5]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-05.png
[6]: ./media/cloud-services-dotnet-continuous-delivery/common-task-tfs-06.png
