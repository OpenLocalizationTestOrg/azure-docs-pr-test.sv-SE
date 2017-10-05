---
title: "Felsöka roller som inte går att starta | Microsoft Docs"
description: "Här följer några vanliga orsaker till varför en tjänst i molnet roll kan inte startas. Det finns också lösningar på problemen."
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
ms.openlocfilehash: 7d956192e8b9c3688b8b6f0108bd9296f66fbd62
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a><span data-ttu-id="f11db-104">Felsöka Cloud Service-roller som inte går att starta</span><span class="sxs-lookup"><span data-stu-id="f11db-104">Troubleshoot Cloud Service roles that fail to start</span></span>
<span data-ttu-id="f11db-105">Här följer några vanliga problem och lösningar som rör Azure Cloud Services roller som inte går att starta.</span><span class="sxs-lookup"><span data-stu-id="f11db-105">Here are some common problems and solutions related to Azure Cloud Services roles that fail to start.</span></span>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a><span data-ttu-id="f11db-106">DLL-filer som saknas eller beroenden</span><span class="sxs-lookup"><span data-stu-id="f11db-106">Missing DLLs or dependencies</span></span>
<span data-ttu-id="f11db-107">Sluta att svara roller och roller som reglering mellan **initierar**, **upptagen**, och **stoppar** tillstånd kan orsakas av DLL-filer eller sammansättningar saknas.</span><span class="sxs-lookup"><span data-stu-id="f11db-107">Unresponsive roles and roles that are cycling between **Initializing**, **Busy**, and **Stopping** states can be caused by missing DLLs or assemblies.</span></span>

<span data-ttu-id="f11db-108">Symtom på DLL: er eller sammansättningar saknas kan vara:</span><span class="sxs-lookup"><span data-stu-id="f11db-108">Symptoms of missing DLLs or assemblies can be:</span></span>

* <span data-ttu-id="f11db-109">Din rollinstans Bläddra igenom **initierar**, **upptagen**, och **stoppar** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="f11db-109">Your role instance is cycling through **Initializing**, **Busy**, and **Stopping** states.</span></span>
* <span data-ttu-id="f11db-110">Rollinstansen har flyttats till **klar** men om du navigerar till ditt webbprogram, visas inte sidan.</span><span class="sxs-lookup"><span data-stu-id="f11db-110">Your role instance has moved to **Ready** but if you navigate to your web application, the page does not appear.</span></span>

<span data-ttu-id="f11db-111">Det finns flera metoder för att undersöka problemen.</span><span class="sxs-lookup"><span data-stu-id="f11db-111">There are several recommended methods for investigating these issues.</span></span>

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a><span data-ttu-id="f11db-112">Diagnostisera problem med saknas DLL-filen i en webbroll</span><span class="sxs-lookup"><span data-stu-id="f11db-112">Diagnose missing DLL issues in a web role</span></span>
<span data-ttu-id="f11db-113">När du navigerar till en webbplats som har distribuerats på en webbplats roll och webbläsaren visar ett serverfel som liknar följande, det kan tyda på att en DLL-fil saknas.</span><span class="sxs-lookup"><span data-stu-id="f11db-113">When you navigate to a website that is deployed in a web role, and the browser displays a server error similar to the following, it may indicate that a DLL is missing.</span></span>

![Serverfel i programmet '/'.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a><span data-ttu-id="f11db-115">Diagnostisera problem genom att stänga av anpassade fel</span><span class="sxs-lookup"><span data-stu-id="f11db-115">Diagnose issues by turning off custom errors</span></span>
<span data-ttu-id="f11db-116">Mer information om fel kan visas genom att konfigurera web.config för webbrollen att ange anpassade felläget till avslaget och omdistribuera tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f11db-116">More complete error information can be viewed by configuring the web.config for the web role to set the custom error mode to Off and redeploying the service.</span></span>

<span data-ttu-id="f11db-117">Visa fullständig fel utan att använda Fjärrskrivbord:</span><span class="sxs-lookup"><span data-stu-id="f11db-117">To view more complete errors without using Remote Desktop:</span></span>

1. <span data-ttu-id="f11db-118">Öppna lösningen i Microsoft Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f11db-118">Open the solution in Microsoft Visual Studio.</span></span>
2. <span data-ttu-id="f11db-119">I den **Solution Explorer**letar du upp filen web.config och öppna den.</span><span class="sxs-lookup"><span data-stu-id="f11db-119">In the **Solution Explorer**, locate the web.config file and open it.</span></span>
3. <span data-ttu-id="f11db-120">Leta upp avsnittet system.web i web.config-filen och Lägg till följande rad:</span><span class="sxs-lookup"><span data-stu-id="f11db-120">In the web.config file, locate the system.web section and add the following line:</span></span>

    ```xml
    <customErrors mode="Off" />
    ```
4. <span data-ttu-id="f11db-121">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="f11db-121">Save the file.</span></span>
5. <span data-ttu-id="f11db-122">Paketera och distribuera om tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f11db-122">Repackage and redeploy the service.</span></span>

<span data-ttu-id="f11db-123">När tjänsten är nytt, visas ett felmeddelande med namnet på sammansättningen eller DLL-filen saknas.</span><span class="sxs-lookup"><span data-stu-id="f11db-123">Once the service is redeployed, you will see an error message with the name of the missing assembly or DLL.</span></span>

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a><span data-ttu-id="f11db-124">Diagnostisera problem genom att visa felet via fjärranslutning</span><span class="sxs-lookup"><span data-stu-id="f11db-124">Diagnose issues by viewing the error remotely</span></span>
<span data-ttu-id="f11db-125">Du kan använda Fjärrskrivbord för att få åtkomst till rollen och visa fullständig felinformation via fjärranslutning.</span><span class="sxs-lookup"><span data-stu-id="f11db-125">You can use Remote Desktop to access the role and view more complete error information remotely.</span></span> <span data-ttu-id="f11db-126">Använd följande steg för att granska felen med hjälp av fjärrskrivbord:</span><span class="sxs-lookup"><span data-stu-id="f11db-126">Use the following steps to view the errors by using Remote Desktop:</span></span>

1. <span data-ttu-id="f11db-127">Kontrollera att Azure SDK 1.3 eller senare är installerad.</span><span class="sxs-lookup"><span data-stu-id="f11db-127">Ensure that Azure SDK 1.3 or later is installed.</span></span>
2. <span data-ttu-id="f11db-128">Under distributionen av lösningen med hjälp av Visual Studio, välja att ”konfigurera anslutning till fjärrskrivbord...”.</span><span class="sxs-lookup"><span data-stu-id="f11db-128">During the deployment of the solution by using Visual Studio, choose to “Configure Remote Desktop connections…”.</span></span> <span data-ttu-id="f11db-129">Mer information om hur du konfigurerar fjärrskrivbordsanslutningen finns [med hjälp av fjärrskrivbord med Azure-roller](../vs-azure-tools-remote-desktop-roles.md).</span><span class="sxs-lookup"><span data-stu-id="f11db-129">For more information on configuring the Remote Desktop connection, see [Using Remote Desktop with Azure Roles](../vs-azure-tools-remote-desktop-roles.md).</span></span>
3. <span data-ttu-id="f11db-130">I Microsoft Azure klassiska portal när instansen visar statusen **klar**, klicka på någon av rollinstanserna.</span><span class="sxs-lookup"><span data-stu-id="f11db-130">In the Microsoft Azure classic portal, once the instance shows a status of **Ready**, click one of the role instances.</span></span>
4. <span data-ttu-id="f11db-131">Klicka på den **Anslut** ikonen i den **fjärråtkomst** område i menyfliksområdet.</span><span class="sxs-lookup"><span data-stu-id="f11db-131">Click the **Connect** icon in the **Remote Access** area of the ribbon.</span></span>
5. <span data-ttu-id="f11db-132">Logga in på den virtuella datorn med hjälp av autentiseringsuppgifterna som angavs vid konfiguration av fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="f11db-132">Sign in to the virtual machine by using the credentials that were specified during the Remote Desktop configuration.</span></span>
6. <span data-ttu-id="f11db-133">Öppna ett kommandofönster.</span><span class="sxs-lookup"><span data-stu-id="f11db-133">Open a command window.</span></span>
7. <span data-ttu-id="f11db-134">Skriv `IPconfig`.</span><span class="sxs-lookup"><span data-stu-id="f11db-134">Type `IPconfig`.</span></span>
8. <span data-ttu-id="f11db-135">Anteckna värdet för IPV4-adress.</span><span class="sxs-lookup"><span data-stu-id="f11db-135">Note the IPV4 Address value.</span></span>
9. <span data-ttu-id="f11db-136">Öppna Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="f11db-136">Open Internet Explorer.</span></span>
10. <span data-ttu-id="f11db-137">Ange adressen och namnet på webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="f11db-137">Type the address and the name of the web application.</span></span> <span data-ttu-id="f11db-138">Till exempel `http://<IPV4 Address>/default.aspx`.</span><span class="sxs-lookup"><span data-stu-id="f11db-138">For example, `http://<IPV4 Address>/default.aspx`.</span></span>

<span data-ttu-id="f11db-139">Gå till webbplatsen returnerar nu mer utförlig felmeddelanden:</span><span class="sxs-lookup"><span data-stu-id="f11db-139">Navigating to the website will now return more explicit error messages:</span></span>

* <span data-ttu-id="f11db-140">Serverfel i programmet '/'.</span><span class="sxs-lookup"><span data-stu-id="f11db-140">Server Error in '/' Application.</span></span>
* <span data-ttu-id="f11db-141">Beskrivning: Ett ohanterat undantag uppstod under körningen av den aktuella webbegäran.</span><span class="sxs-lookup"><span data-stu-id="f11db-141">Description: An unhandled exception occurred during the execution of the current web request.</span></span> <span data-ttu-id="f11db-142">Granska stack-spårning för mer information om felet och var det uppstod i koden.</span><span class="sxs-lookup"><span data-stu-id="f11db-142">Please review the stack trace for more information about the error and where it originated in the code.</span></span>
* <span data-ttu-id="f11db-143">Undantagsinformation: System.IO.FIleNotFoundException: Det gick inte att läsa in filen eller sammansättningen ' Microsoft.WindowsAzure.StorageClient, Version = 1.1.0.0 kultur = neutral, PublicKeyToken = 31bf856ad364e35' eller en av dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="f11db-143">Exception Details: System.IO.FIleNotFoundException: Could not load file or assembly ‘Microsoft.WindowsAzure.StorageClient, Version=1.1.0.0, Culture=neutral, PublicKeyToken=31bf856ad364e35’ or one of its dependencies.</span></span> <span data-ttu-id="f11db-144">Det går inte att hitta den angivna filen.</span><span class="sxs-lookup"><span data-stu-id="f11db-144">The system cannot find the file specified.</span></span>

<span data-ttu-id="f11db-145">Exempel:</span><span class="sxs-lookup"><span data-stu-id="f11db-145">For example:</span></span>

![Explicit serverfel i programmet '/'](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a><span data-ttu-id="f11db-147">Diagnostisera problem genom att använda beräkningsemulatorn</span><span class="sxs-lookup"><span data-stu-id="f11db-147">Diagnose issues by using the compute emulator</span></span>
<span data-ttu-id="f11db-148">Du kan använda Microsoft Azure-beräkningsemulatorn för att diagnostisera och felsöka problem med saknade beroenden och web.config-fel.</span><span class="sxs-lookup"><span data-stu-id="f11db-148">You can use the Microsoft Azure compute emulator to diagnose and troubleshoot issues of missing dependencies and web.config errors.</span></span>

<span data-ttu-id="f11db-149">För bästa resultat i den här metoden för diagnos bör du använda en dator eller virtuell dator som har en ren installation av Windows.</span><span class="sxs-lookup"><span data-stu-id="f11db-149">For best results in using this method of diagnosis, you should use a computer or virtual machine that has a clean installation of Windows.</span></span> <span data-ttu-id="f11db-150">Använd Windows Server 2008 R2 x64 för att simulera Azure-miljön bäst.</span><span class="sxs-lookup"><span data-stu-id="f11db-150">To best simulate the Azure environment, use Windows Server 2008 R2 x64.</span></span>

1. <span data-ttu-id="f11db-151">Installera den fristående versionen av den [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="f11db-151">Install the standalone version of the [Azure SDK](https://azure.microsoft.com/downloads/).</span></span>
2. <span data-ttu-id="f11db-152">Skapa molntjänstprojektet på utveckling-datorn.</span><span class="sxs-lookup"><span data-stu-id="f11db-152">On the development machine, build the cloud service project.</span></span>
3. <span data-ttu-id="f11db-153">Navigera till mappen bin\debug på molntjänstprojektet i Utforskaren.</span><span class="sxs-lookup"><span data-stu-id="f11db-153">In Windows Explorer, navigate to the bin\debug folder of the cloud service project.</span></span>
4. <span data-ttu-id="f11db-154">Kopiera filen .csx mapp- och .cscfg på den dator som du använder för att felsöka problem.</span><span class="sxs-lookup"><span data-stu-id="f11db-154">Copy the .csx folder and .cscfg file to the computer that you are using to debug the issues.</span></span>
5. <span data-ttu-id="f11db-155">Öppna en Azure SDK-kommandotolksfönster och Skriv på ren dator `csrun.exe /devstore:start`.</span><span class="sxs-lookup"><span data-stu-id="f11db-155">On the clean machine, open an Azure SDK Command Prompt window and type `csrun.exe /devstore:start`.</span></span>
6. <span data-ttu-id="f11db-156">I Kommandotolken, Skriv `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.</span><span class="sxs-lookup"><span data-stu-id="f11db-156">At the command prompt, type `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.</span></span>
7. <span data-ttu-id="f11db-157">När rollen startar visas detaljerad felinformation i Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="f11db-157">When the role starts, you will see detailed error information in Internet Explorer.</span></span> <span data-ttu-id="f11db-158">Du kan också använda standard Windows felsökningsverktyg för att diagnosticera problemet.</span><span class="sxs-lookup"><span data-stu-id="f11db-158">You can also use standard Windows troubleshooting tools to further diagnose the problem.</span></span>

## <a name="diagnose-issues-by-using-intellitrace"></a><span data-ttu-id="f11db-159">Diagnostisera problem med hjälp av IntelliTrace</span><span class="sxs-lookup"><span data-stu-id="f11db-159">Diagnose issues by using IntelliTrace</span></span>
<span data-ttu-id="f11db-160">Du kan använda för arbetaren och web-roller som använder .NET Framework 4 [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), som finns i Microsoft Visual Studio Enterprise.</span><span class="sxs-lookup"><span data-stu-id="f11db-160">For worker and web roles that use .NET Framework 4, you can use [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), which is available in Microsoft Visual Studio Enterprise.</span></span>

<span data-ttu-id="f11db-161">Följ dessa steg om du vill distribuera tjänsten med IntelliTrace aktiverad:</span><span class="sxs-lookup"><span data-stu-id="f11db-161">Follow these steps to deploy the service with IntelliTrace enabled:</span></span>

1. <span data-ttu-id="f11db-162">Bekräfta att Azure SDK 1.3 eller senare är installerat.</span><span class="sxs-lookup"><span data-stu-id="f11db-162">Confirm that Azure SDK 1.3 or later is installed.</span></span>
2. <span data-ttu-id="f11db-163">Distribuera lösningen med hjälp av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f11db-163">Deploy the solution by using Visual Studio.</span></span> <span data-ttu-id="f11db-164">Under distributionen kan du kontrollera den **aktivera IntelliTrace för .NET 4 roller** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="f11db-164">During deployment, check the **Enable IntelliTrace for .NET 4 roles** check box.</span></span>
3. <span data-ttu-id="f11db-165">När instansen startar, öppnar du den **Server Explorer**.</span><span class="sxs-lookup"><span data-stu-id="f11db-165">Once the instance starts, open the **Server Explorer**.</span></span>
4. <span data-ttu-id="f11db-166">Expandera den **Azure\\molntjänster** nod och leta upp distributionen.</span><span class="sxs-lookup"><span data-stu-id="f11db-166">Expand the **Azure\\Cloud Services** node and locate the deployment.</span></span>
5. <span data-ttu-id="f11db-167">Expandera distributionen tills du ser rollinstanserna.</span><span class="sxs-lookup"><span data-stu-id="f11db-167">Expand the deployment until you see the role instances.</span></span> <span data-ttu-id="f11db-168">Högerklicka på någon av instanserna.</span><span class="sxs-lookup"><span data-stu-id="f11db-168">Right-click on one of the instances.</span></span>
6. <span data-ttu-id="f11db-169">Välj **visa IntelliTrace-loggar**.</span><span class="sxs-lookup"><span data-stu-id="f11db-169">Choose **View IntelliTrace logs**.</span></span> <span data-ttu-id="f11db-170">Den **IntelliTrace sammanfattning** öppnas.</span><span class="sxs-lookup"><span data-stu-id="f11db-170">The **IntelliTrace Summary** will open.</span></span>
7. <span data-ttu-id="f11db-171">Leta upp avsnittet undantag sammanfattningen.</span><span class="sxs-lookup"><span data-stu-id="f11db-171">Locate the exceptions section of the summary.</span></span> <span data-ttu-id="f11db-172">Om det finns undantag, avsnittet är märkt **Undantagsdata**.</span><span class="sxs-lookup"><span data-stu-id="f11db-172">If there are exceptions, the section will be labeled **Exception Data**.</span></span>
8. <span data-ttu-id="f11db-173">Expandera den **Undantagsdata** och leta efter **System.IO.FileNotFoundException** fel som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="f11db-173">Expand the **Exception Data** and look for **System.IO.FileNotFoundException** errors similar to the following:</span></span>

![Undantagsdata saknas filen eller sammansättningen](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a><span data-ttu-id="f11db-175">Adressen saknas DLL: er och sammansättningar</span><span class="sxs-lookup"><span data-stu-id="f11db-175">Address missing DLLs and assemblies</span></span>
<span data-ttu-id="f11db-176">Följ dessa steg för att åtgärda saknade DLL och sammansättningen fel:</span><span class="sxs-lookup"><span data-stu-id="f11db-176">To address missing DLL and assembly errors, follow these steps:</span></span>

1. <span data-ttu-id="f11db-177">Öppna lösningen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f11db-177">Open the solution in Visual Studio.</span></span>
2. <span data-ttu-id="f11db-178">I **Solution Explorer**öppnar den **referenser** mapp.</span><span class="sxs-lookup"><span data-stu-id="f11db-178">In **Solution Explorer**, open the **References** folder.</span></span>
3. <span data-ttu-id="f11db-179">Klicka på den sammansättning som identifieras i Windows.</span><span class="sxs-lookup"><span data-stu-id="f11db-179">Click the assembly identified in the error.</span></span>
4. <span data-ttu-id="f11db-180">I den **egenskaper** rutan hitta **kopiera lokala egenskapen** och ange värdet **SANT**.</span><span class="sxs-lookup"><span data-stu-id="f11db-180">In the **Properties** pane, locate **Copy Local property** and set the value to **True**.</span></span>
5. <span data-ttu-id="f11db-181">Distribuera om Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="f11db-181">Redeploy the cloud service.</span></span>

<span data-ttu-id="f11db-182">När du har kontrollerat att alla fel har åtgärdats kan du distribuera tjänsten utan att kontrollera den **aktivera IntelliTrace för .NET 4 roller** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="f11db-182">Once you have verified that all errors have been corrected, you can deploy the service without checking the **Enable IntelliTrace for .NET 4 roles** check box.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f11db-183">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f11db-183">Next steps</span></span>
<span data-ttu-id="f11db-184">Visa mer [felsökning artiklar](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) för molntjänster.</span><span class="sxs-lookup"><span data-stu-id="f11db-184">View more [troubleshooting articles](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) for cloud services.</span></span>

<span data-ttu-id="f11db-185">Information om hur du felsöker cloud service rollen problem med hjälp av Azure PaaS datorn diagnostikdata finns [Kevin Williamson bloggserie](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span><span class="sxs-lookup"><span data-stu-id="f11db-185">To learn how to troubleshoot cloud service role issues by using Azure PaaS computer diagnostics data, see [Kevin Williamson's blog series](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).</span></span>
