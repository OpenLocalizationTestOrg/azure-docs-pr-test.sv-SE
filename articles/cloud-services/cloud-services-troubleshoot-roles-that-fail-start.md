---
title: aaaTroubleshoot roller som inte toostart | Microsoft Docs
description: "Här följer några vanliga orsaker till varför en tjänst i molnet roll misslyckas toostart. Det finns också lösningar toothese problem."
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 674b2faf-26d7-4f54-99ea-a9e02ef0eb2f
ms.service: cloud-services
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 7/26/2017
ms.author: v-six
ms.openlocfilehash: e2fbecb08a10984add79dfc74e73de6869bb314f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-cloud-service-roles-that-fail-toostart"></a><span data-ttu-id="d275c-104">Felsöka Cloud Service-roller som inte toostart</span><span class="sxs-lookup"><span data-stu-id="d275c-104">Troubleshoot Cloud Service roles that fail toostart</span></span>
<span data-ttu-id="d275c-105">Här följer några vanliga problem och lösningar relaterade tooAzure molntjänster roller som inte toostart.</span><span class="sxs-lookup"><span data-stu-id="d275c-105">Here are some common problems and solutions related tooAzure Cloud Services roles that fail toostart.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a><span data-ttu-id="d275c-106">DLL-filer som saknas eller beroenden</span><span class="sxs-lookup"><span data-stu-id="d275c-106">Missing DLLs or dependencies</span></span>
<span data-ttu-id="d275c-107">Sluta att svara roller och roller som reglering mellan **initierar**, **upptagen**, och **stoppar** tillstånd kan orsakas av DLL-filer eller sammansättningar saknas.</span><span class="sxs-lookup"><span data-stu-id="d275c-107">Unresponsive roles and roles that are cycling between **Initializing**, **Busy**, and **Stopping** states can be caused by missing DLLs or assemblies.</span></span>

<span data-ttu-id="d275c-108">Symtom på DLL: er eller sammansättningar saknas kan vara:</span><span class="sxs-lookup"><span data-stu-id="d275c-108">Symptoms of missing DLLs or assemblies can be:</span></span>

* <span data-ttu-id="d275c-109">Din rollinstans Bläddra igenom **initierar**, **upptagen**, och **stoppar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="d275c-109">Your role instance is cycling through **Initializing**, **Busy**, and **Stopping** states.</span></span>
* <span data-ttu-id="d275c-110">Rollinstansen har flyttats för**klar** men hello sidan visas inte om du navigerar tooyour webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d275c-110">Your role instance has moved too**Ready** but if you navigate tooyour web application, hello page does not appear.</span></span>

<span data-ttu-id="d275c-111">Det finns flera metoder för att undersöka problemen.</span><span class="sxs-lookup"><span data-stu-id="d275c-111">There are several recommended methods for investigating these issues.</span></span>

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a><span data-ttu-id="d275c-112">Diagnostisera problem med saknas DLL-filen i en webbroll</span><span class="sxs-lookup"><span data-stu-id="d275c-112">Diagnose missing DLL issues in a web role</span></span>
<span data-ttu-id="d275c-113">När du navigerar tooa webbplats som distribueras i en webbroll och hello webbläsaren visar en server fel liknande toohello följande, kan det indikera att en DLL-fil saknas.</span><span class="sxs-lookup"><span data-stu-id="d275c-113">When you navigate tooa website that is deployed in a web role, and hello browser displays a server error similar toohello following, it may indicate that a DLL is missing.</span></span>

![Serverfel i programmet '/'.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a><span data-ttu-id="d275c-115">Diagnostisera problem genom att stänga av anpassade fel</span><span class="sxs-lookup"><span data-stu-id="d275c-115">Diagnose issues by turning off custom errors</span></span>
<span data-ttu-id="d275c-116">Mer information om fel kan visas genom att konfigurera hello web.config för hello web rollen tooset hello anpassade fel läge tooOff och omdistribuera hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d275c-116">More complete error information can be viewed by configuring hello web.config for hello web role tooset hello custom error mode tooOff and redeploying hello service.</span></span>

<span data-ttu-id="d275c-117">tooview slutföra fler fel utan att använda Fjärrskrivbord:</span><span class="sxs-lookup"><span data-stu-id="d275c-117">tooview more complete errors without using Remote Desktop:</span></span>

1. <span data-ttu-id="d275c-118">Öppna hello lösning i Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d275c-118">Open hello solution in Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="d275c-119">I hello **Solution Explorer**letar du upp hello web.config-filen och öppna den.</span><span class="sxs-lookup"><span data-stu-id="d275c-119">In hello **Solution Explorer**, locate hello web.config file and open it.</span></span>
3. <span data-ttu-id="d275c-120">Leta upp hello system.web-avsnittet i filen web.config för hello och Lägg till följande rad hello:</span><span class="sxs-lookup"><span data-stu-id="d275c-120">In hello web.config file, locate hello system.web section and add hello following line:</span></span>

    ```xml
    <customErrors mode="Off" />
    ```
4. <span data-ttu-id="d275c-121">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="d275c-121">Save hello file.</span></span>
5. <span data-ttu-id="d275c-122">Paketera och distribuera hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d275c-122">Repackage and redeploy hello service.</span></span>

<span data-ttu-id="d275c-123">När hello-tjänsten är nytt, visas ett felmeddelande med hello namnet hello sammansättningen eller DLL-filen saknas.</span><span class="sxs-lookup"><span data-stu-id="d275c-123">Once hello service is redeployed, you will see an error message with hello name of hello missing assembly or DLL.</span></span>

## <a name="diagnose-issues-by-viewing-hello-error-remotely"></a><span data-ttu-id="d275c-124">Diagnostisera problem genom att visa hello fel via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="d275c-124">Diagnose issues by viewing hello error remotely</span></span>
<span data-ttu-id="d275c-125">Du kan använda Fjärrskrivbord tooaccess hello-rollen och visa fullständig felinformation via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="d275c-125">You can use Remote Desktop tooaccess hello role and view more complete error information remotely.</span></span> <span data-ttu-id="d275c-126">Använd följande steg tooview hello fel med hjälp av fjärrskrivbord hello:</span><span class="sxs-lookup"><span data-stu-id="d275c-126">Use hello following steps tooview hello errors by using Remote Desktop:</span></span>

1. <span data-ttu-id="d275c-127">Kontrollera att Azure SDK 1.3 eller senare är installerad.</span><span class="sxs-lookup"><span data-stu-id="d275c-127">Ensure that Azure SDK 1.3 or later is installed.</span></span>
2. <span data-ttu-id="d275c-128">Under hello distribution av hello lösning med hjälp av Visual Studio, välja för ”konfigurera anslutningar till fjärrskrivbord...”.</span><span class="sxs-lookup"><span data-stu-id="d275c-128">During hello deployment of hello solution by using Visual Studio, choose too“Configure Remote Desktop connections…”.</span></span> <span data-ttu-id="d275c-129">Mer information om hur du konfigurerar hello fjärrskrivbordsanslutning finns [med hjälp av fjärrskrivbord med Azure-roller](../vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="d275c-129">For more information on configuring hello Remote Desktop connection, see [Using Remote Desktop with Azure Roles](../vs-azure-tools-remote-desktop-roles.md).</span></span>
3. <span data-ttu-id="d275c-130">I hello Microsoft klassiska Azure-portalen när hello instans visar statusen **klar**, klicka på någon av hello rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="d275c-130">In hello Microsoft Azure classic portal, once hello instance shows a status of **Ready**, click one of hello role instances.</span></span>
4. <span data-ttu-id="d275c-131">Klicka på hello **Anslut** ikon i hello **fjärråtkomst** område i hello menyfliksområdet.</span><span class="sxs-lookup"><span data-stu-id="d275c-131">Click hello **Connect** icon in hello **Remote Access** area of hello ribbon.</span></span>
5. <span data-ttu-id="d275c-132">Logga in toohello virtuell dator med hjälp av hello-autentiseringsuppgifter som angavs under hello fjärrskrivbord konfiguration.</span><span class="sxs-lookup"><span data-stu-id="d275c-132">Sign in toohello virtual machine by using hello credentials that were specified during hello Remote Desktop configuration.</span></span>
6. <span data-ttu-id="d275c-133">Öppna ett kommandofönster.</span><span class="sxs-lookup"><span data-stu-id="d275c-133">Open a command window.</span></span>
7. <span data-ttu-id="d275c-134">Skriv `IPconfig`.</span><span class="sxs-lookup"><span data-stu-id="d275c-134">Type `IPconfig`.</span></span>
8. <span data-ttu-id="d275c-135">Anteckna värdet för hello IPV4-adress.</span><span class="sxs-lookup"><span data-stu-id="d275c-135">Note hello IPV4 Address value.</span></span>
9. <span data-ttu-id="d275c-136">Öppna Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="d275c-136">Open Internet Explorer.</span></span>
10. <span data-ttu-id="d275c-137">Ange hello-adress och hello namn på hello webbprogram.</span><span class="sxs-lookup"><span data-stu-id="d275c-137">Type hello address and hello name of hello web application.</span></span> <span data-ttu-id="d275c-138">Till exempel `http://<IPV4 Address>/default.aspx`.</span><span class="sxs-lookup"><span data-stu-id="d275c-138">For example, `http://<IPV4 Address>/default.aspx`.</span></span>

<span data-ttu-id="d275c-139">Navigera toohello returnerar webbplats nu mer utförlig felmeddelanden:</span><span class="sxs-lookup"><span data-stu-id="d275c-139">Navigating toohello website will now return more explicit error messages:</span></span>

* <span data-ttu-id="d275c-140">Serverfel i programmet '/'.</span><span class="sxs-lookup"><span data-stu-id="d275c-140">Server Error in '/' Application.</span></span>
* <span data-ttu-id="d275c-141">Beskrivning: Ett ohanterat undantag uppstod under hello körning av hello aktuella webbegäran.</span><span class="sxs-lookup"><span data-stu-id="d275c-141">Description: An unhandled exception occurred during hello execution of hello current web request.</span></span> <span data-ttu-id="d275c-142">Granska hello stackspårning för mer information om felet hello och ursprung i hello kod.</span><span class="sxs-lookup"><span data-stu-id="d275c-142">Please review hello stack trace for more information about hello error and where it originated in hello code.</span></span>
* <span data-ttu-id="d275c-143">Undantagsinformation: System.IO.FIleNotFoundException: Det gick inte att läsa in filen eller sammansättningen ' Microsoft.WindowsAzure.StorageClient, Version = 1.1.0.0 kultur = neutral, PublicKeyToken = 31bf856ad364e35' eller en av dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="d275c-143">Exception Details: System.IO.FIleNotFoundException: Could not load file or assembly ‘Microsoft.WindowsAzure.StorageClient, Version=1.1.0.0, Culture=neutral, PublicKeyToken=31bf856ad364e35’ or one of its dependencies.</span></span> <span data-ttu-id="d275c-144">hello går inte att hitta angivna hello-filen.</span><span class="sxs-lookup"><span data-stu-id="d275c-144">hello system cannot find hello file specified.</span></span>

<span data-ttu-id="d275c-145">Exempel:</span><span class="sxs-lookup"><span data-stu-id="d275c-145">For example:</span></span>

![Explicit serverfel i programmet '/'](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-hello-compute-emulator"></a><span data-ttu-id="d275c-147">Diagnostisera problem med hjälp av hello beräkningsemulatorn</span><span class="sxs-lookup"><span data-stu-id="d275c-147">Diagnose issues by using hello compute emulator</span></span>
<span data-ttu-id="d275c-148">Du kan använda hello Microsoft Azure compute emulator toodiagnose och felsökning av problem med saknade beroenden och web.config-fel.</span><span class="sxs-lookup"><span data-stu-id="d275c-148">You can use hello Microsoft Azure compute emulator toodiagnose and troubleshoot issues of missing dependencies and web.config errors.</span></span>

<span data-ttu-id="d275c-149">För bästa resultat i den här metoden för diagnos bör du använda en dator eller virtuell dator som har en ren installation av Windows.</span><span class="sxs-lookup"><span data-stu-id="d275c-149">For best results in using this method of diagnosis, you should use a computer or virtual machine that has a clean installation of Windows.</span></span> <span data-ttu-id="d275c-150">toobest simulera hello Azure-miljön kan använda Windows Server 2008 R2 x64.</span><span class="sxs-lookup"><span data-stu-id="d275c-150">toobest simulate hello Azure environment, use Windows Server 2008 R2 x64.</span></span>

1. <span data-ttu-id="d275c-151">Installera hello fristående versionen av hello [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="d275c-151">Install hello standalone version of hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="d275c-152">Skapa hello molntjänstprojekt på hello utvecklingsdator.</span><span class="sxs-lookup"><span data-stu-id="d275c-152">On hello development machine, build hello cloud service project.</span></span>
3. <span data-ttu-id="d275c-153">Navigera toohello hello molntjänstprojekt bin\debug mapp i Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="d275c-153">In Windows Explorer, navigate toohello bin\debug folder of hello cloud service project.</span></span>
4. <span data-ttu-id="d275c-154">Kopiera hello .csx mapp- och .cscfg-filen toohello datorn att du använder toodebug hello problem.</span><span class="sxs-lookup"><span data-stu-id="d275c-154">Copy hello .csx folder and .cscfg file toohello computer that you are using toodebug hello issues.</span></span>
5. <span data-ttu-id="d275c-155">Öppna en Azure SDK-kommandotolksfönster och Skriv på hello ren maskin, `csrun.exe /devstore:start`.</span><span class="sxs-lookup"><span data-stu-id="d275c-155">On hello clean machine, open an Azure SDK Command Prompt window and type `csrun.exe /devstore:start`.</span></span>
6. <span data-ttu-id="d275c-156">I hello kommandotolk, Skriv `run csrun <path too.csx folder> <path too.cscfg file> /launchBrowser`.</span><span class="sxs-lookup"><span data-stu-id="d275c-156">At hello command prompt, type `run csrun <path too.csx folder> <path too.cscfg file> /launchBrowser`.</span></span>
7. <span data-ttu-id="d275c-157">När hello roll startar visas detaljerad felinformation i Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="d275c-157">When hello role starts, you will see detailed error information in Internet Explorer.</span></span> <span data-ttu-id="d275c-158">Du kan också använda standard Windows felsökningsverktyg toofurther diagnostisera hello problem.</span><span class="sxs-lookup"><span data-stu-id="d275c-158">You can also use standard Windows troubleshooting tools toofurther diagnose hello problem.</span></span>

## <a name="diagnose-issues-by-using-intellitrace"></a><span data-ttu-id="d275c-159">Diagnostisera problem med hjälp av IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="d275c-159">Diagnose issues by using IntelliTrace</span></span>
<span data-ttu-id="d275c-160">Du kan använda för arbetaren och web-roller som använder .NET Framework 4 [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), som finns i Microsoft Visual Studio Enterprise.</span><span class="sxs-lookup"><span data-stu-id="d275c-160">For worker and web roles that use .NET Framework 4, you can use [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), which is available in Microsoft Visual Studio Enterprise.</span></span>

<span data-ttu-id="d275c-161">Följ dessa steg toodeploy hello-tjänsten med IntelliTrace aktiverad:</span><span class="sxs-lookup"><span data-stu-id="d275c-161">Follow these steps toodeploy hello service with IntelliTrace enabled:</span></span>

1. <span data-ttu-id="d275c-162">Bekräfta att Azure SDK 1.3 eller senare är installerat.</span><span class="sxs-lookup"><span data-stu-id="d275c-162">Confirm that Azure SDK 1.3 or later is installed.</span></span>
2. <span data-ttu-id="d275c-163">Distribuera hello lösning med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d275c-163">Deploy hello solution by using Visual Studio.</span></span> <span data-ttu-id="d275c-164">Under distributionen, kontrollera hello **aktivera IntelliTrace för .NET 4 roller** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="d275c-164">During deployment, check hello **Enable IntelliTrace for .NET 4 roles** check box.</span></span>
3. <span data-ttu-id="d275c-165">När hello instans startar öppna hello **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="d275c-165">Once hello instance starts, open hello **Server Explorer**.</span></span>
4. <span data-ttu-id="d275c-166">Expandera hello **Azure\\molntjänster** nod och leta upp hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="d275c-166">Expand hello **Azure\\Cloud Services** node and locate hello deployment.</span></span>
5. <span data-ttu-id="d275c-167">Expandera hello-distribution tills du ser hello rollinstanser.</span><span class="sxs-lookup"><span data-stu-id="d275c-167">Expand hello deployment until you see hello role instances.</span></span> <span data-ttu-id="d275c-168">Högerklicka på någon av hello instanser.</span><span class="sxs-lookup"><span data-stu-id="d275c-168">Right-click on one of hello instances.</span></span>
6. <span data-ttu-id="d275c-169">Välj **visa IntelliTrace-loggar**.</span><span class="sxs-lookup"><span data-stu-id="d275c-169">Choose **View IntelliTrace logs**.</span></span> <span data-ttu-id="d275c-170">Hej **IntelliTrace sammanfattning** öppnas.</span><span class="sxs-lookup"><span data-stu-id="d275c-170">hello **IntelliTrace Summary** will open.</span></span>
7. <span data-ttu-id="d275c-171">Leta upp hello undantag avsnitt i hello sammanfattning.</span><span class="sxs-lookup"><span data-stu-id="d275c-171">Locate hello exceptions section of hello summary.</span></span> <span data-ttu-id="d275c-172">Om det finns undantag, hello-avsnittet är märkt **Undantagsdata**.</span><span class="sxs-lookup"><span data-stu-id="d275c-172">If there are exceptions, hello section will be labeled **Exception Data**.</span></span>
8. <span data-ttu-id="d275c-173">Expandera hello **Undantagsdata** och leta efter **System.IO.FileNotFoundException** fel liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="d275c-173">Expand hello **Exception Data** and look for **System.IO.FileNotFoundException** errors similar toohello following:</span></span>

![Undantagsdata saknas filen eller sammansättningen](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a><span data-ttu-id="d275c-175">Adressen saknas DLL: er och sammansättningar</span><span class="sxs-lookup"><span data-stu-id="d275c-175">Address missing DLLs and assemblies</span></span>
<span data-ttu-id="d275c-176">tooaddress saknas DLL-fil och montering fel, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="d275c-176">tooaddress missing DLL and assembly errors, follow these steps:</span></span>

1. <span data-ttu-id="d275c-177">Öppna hello lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d275c-177">Open hello solution in Visual Studio.</span></span>
2. <span data-ttu-id="d275c-178">I **Solution Explorer**öppnar hello **referenser** mapp.</span><span class="sxs-lookup"><span data-stu-id="d275c-178">In **Solution Explorer**, open hello **References** folder.</span></span>
3. <span data-ttu-id="d275c-179">Klicka på hello sammansättningen som identifierats i hello-fel.</span><span class="sxs-lookup"><span data-stu-id="d275c-179">Click hello assembly identified in hello error.</span></span>
4. <span data-ttu-id="d275c-180">I hello **egenskaper** rutan hitta **kopiera lokala egenskapen** och ange hello värdet för**SANT**.</span><span class="sxs-lookup"><span data-stu-id="d275c-180">In hello **Properties** pane, locate **Copy Local property** and set hello value too**True**.</span></span>
5. <span data-ttu-id="d275c-181">Omdistribuera hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="d275c-181">Redeploy hello cloud service.</span></span>

<span data-ttu-id="d275c-182">När du har kontrollerat att alla fel har åtgärdats, kan du distribuera hello tjänst utan att kontrollera hello **aktivera IntelliTrace för .NET 4 roller** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="d275c-182">Once you have verified that all errors have been corrected, you can deploy hello service without checking hello **Enable IntelliTrace for .NET 4 roles** check box.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d275c-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d275c-183">Next steps</span></span>
<span data-ttu-id="d275c-184">Visa mer [felsökning artiklar](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) för molntjänster.</span><span class="sxs-lookup"><span data-stu-id="d275c-184">View more [troubleshooting articles](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) for cloud services.</span></span>

<span data-ttu-id="d275c-185">toolearn hur tootroubleshoot moln rolltjänst problem med hjälp av Azure PaaS diagnostikdata för datorn, se [Kevin Williamson bloggserie](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span><span class="sxs-lookup"><span data-stu-id="d275c-185">toolearn how tootroubleshoot cloud service role issues by using Azure PaaS computer diagnostics data, see [Kevin Williamson's blog series](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span></span>
