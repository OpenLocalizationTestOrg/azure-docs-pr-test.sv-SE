---
title: Hantera en arbetsyta i Machine Learning | Microsoft Docs
description: "Hantera åtkomst till Azure Machine Learning arbetsytor och distribuera och hantera ml – API-tjänster"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: daf3d413-7a77-4beb-9a7a-6b4bdf717719
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye
ms.openlocfilehash: 94800f51baf83311c33490cada5f991ff2101da9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a><span data-ttu-id="69898-103">Hantera en Azure Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="69898-103">Manage an Azure Machine Learning workspace</span></span>

> [!NOTE]
> <span data-ttu-id="69898-104">Information om hur du hanterar webbtjänster i Machine Learning-webbtjänster portal finns [hantera en webbtjänst med hjälp av Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="69898-104">For information on managing Web services in the Machine Learning Web Services portal, see [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span>
> 
> 

<span data-ttu-id="69898-105">Du kan hantera Machine Learning arbetsytor i Azure-portalen eller den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="69898-105">You can manage Machine Learning workspaces in either the Azure portal or the Azure classic portal.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="use-the-azure-portal"></a><span data-ttu-id="69898-106">Använda Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="69898-106">Use the Azure portal</span></span>

<span data-ttu-id="69898-107">Hantera en arbetsyta i Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="69898-107">To manage a workspace in the Azure portal:</span></span>

1. <span data-ttu-id="69898-108">Logga in på den [Azure-portalen](https://portal.azure.com/) med ett administratörskonto för Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="69898-108">Sign in to the [Azure portal](https://portal.azure.com/) using an Azure subscription administrator account.</span></span>
2. <span data-ttu-id="69898-109">Ange ”dator learning arbetsytor” i sökrutan överst på sidan och välj sedan **Machine Learning arbetsytor**.</span><span class="sxs-lookup"><span data-stu-id="69898-109">In the search box at the top of the page, enter "machine learning workspaces" and then select **Machine Learning Workspaces**.</span></span>
3. <span data-ttu-id="69898-110">Klicka på arbetsytan som du vill hantera.</span><span class="sxs-lookup"><span data-stu-id="69898-110">Click the workspace you want to manage.</span></span>

<span data-ttu-id="69898-111">Förutom standard resursinformation för hantering och alternativ som är tillgängliga kan du:</span><span class="sxs-lookup"><span data-stu-id="69898-111">In addition to the standard resource management information and options available, you can:</span></span>

- <span data-ttu-id="69898-112">Visa **egenskaper** – den här sidan visar arbetsytan och resursen och du kan ändra gruppen prenumeration och resurs som den här arbetsytan är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="69898-112">View **Properties** - This page displays the workspace and resource information, and you can change the subscription and resource group that this workspace is connected with.</span></span>
- <span data-ttu-id="69898-113">**Omsynkronisering Lagringsnycklar** -arbetsytan underhåller nycklar till lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="69898-113">**Resync Storage Keys** - The workspace maintains keys to the storage account.</span></span> <span data-ttu-id="69898-114">Om lagringskontot ändras nycklar och du kan klicka på **omsynkronisering nycklar** att synkronisera nycklar med arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="69898-114">If the storage account changes keys, then you can click **Resync keys** to synchronize the keys with the workspace.</span></span>

<span data-ttu-id="69898-115">Använd portalen Machine Learning-webbtjänster för att hantera de webbtjänster som är associerade med den här arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="69898-115">To manage the web services associated with this workspace, use the Machine Learning Web Services portal.</span></span> <span data-ttu-id="69898-116">Se [hantera en webbtjänst med hjälp av Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md) fullständig information.</span><span class="sxs-lookup"><span data-stu-id="69898-116">See [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md) for complete information.</span></span>

> [!NOTE]
> <span data-ttu-id="69898-117">Om du vill distribuera eller hantera nya webbtjänster måste du tilldelas rollen deltagare eller administratör på den prenumeration som webbtjänsten har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="69898-117">To deploy or manage New web services you must be assigned a contributor or administrator role on the subscription to which the web service is deployed.</span></span> <span data-ttu-id="69898-118">Om du bjuda in en annan användare till en machine learning-arbetsytan måste du tilldela dem en roll för deltagare eller administratör för prenumerationen innan de kan distribuera eller hantera webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="69898-118">If you invite another user to a machine learning workspace, you must assign them to a contributor or administrator role on the subscription before they can deploy or manage web services.</span></span> 
> 
><span data-ttu-id="69898-119">Mer information om inställningen åtkomstbehörighet finns [visa åtkomst-tilldelningar för användare och grupper i Azure portal – förhandsversion](../active-directory/role-based-access-control-manage-assignments.md).</span><span class="sxs-lookup"><span data-stu-id="69898-119">For more information on setting access permissions, see [View access assignments for users and groups in the Azure portal - Public preview](../active-directory/role-based-access-control-manage-assignments.md).</span></span>

## <a name="use-the-azure-classic-portal"></a><span data-ttu-id="69898-120">Använd den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="69898-120">Use the Azure classic portal</span></span>

<span data-ttu-id="69898-121">Med den klassiska Azure-portalen kan hantera du arbetsytorna Machine Learning till:</span><span class="sxs-lookup"><span data-stu-id="69898-121">Using the Azure classic portal, you can manage your Machine Learning workspaces to:</span></span>

* <span data-ttu-id="69898-122">Övervaka hur arbetsytan används</span><span class="sxs-lookup"><span data-stu-id="69898-122">Monitor how the workspace is being used</span></span>
* <span data-ttu-id="69898-123">Konfigurera arbetsyta om du vill tillåta eller neka åtkomst</span><span class="sxs-lookup"><span data-stu-id="69898-123">Configure the workspace to allow or deny access</span></span>
* <span data-ttu-id="69898-124">Hantera webbtjänster som skapats i arbetsytan</span><span class="sxs-lookup"><span data-stu-id="69898-124">Manage Web services created in the workspace</span></span>
* <span data-ttu-id="69898-125">Ta bort arbetsytan</span><span class="sxs-lookup"><span data-stu-id="69898-125">Delete the workspace</span></span>

<span data-ttu-id="69898-126">Dessutom innehåller en översikt över arbetsytan-användning och en snabb överblick över din arbetsyta information fliken instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="69898-126">In addition, the dashboard tab provides an overview of your workspace usage and a quick glance of your workspace information.</span></span>  

> [!TIP]
> <span data-ttu-id="69898-127">I Azure Machine Learning Studio på den **WEB SERVICES** fliken, du kan lägga till, uppdatera eller ta bort en Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="69898-127">In Azure Machine Learning Studio, on the **WEB SERVICES** tab, you can add, update, or delete a Machine Learning Web service.</span></span>
> 
> 

<span data-ttu-id="69898-128">Hantera en arbetsyta i den klassiska Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="69898-128">To manage a workspace in the Azure classic portal:</span></span>

1. <span data-ttu-id="69898-129">Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com/) med Microsoft Azure-konto – Använd det konto som är kopplad till den Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="69898-129">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) using your Microsoft Azure account - use the account that's associated with the Azure subscription.</span></span>
2. <span data-ttu-id="69898-130">Klicka på panelen för Microsoft Azure-tjänster **MASKININLÄRNING**.</span><span class="sxs-lookup"><span data-stu-id="69898-130">In the Microsoft Azure services panel, click **MACHINE LEARNING**.</span></span>
3. <span data-ttu-id="69898-131">Klicka på arbetsytan som du vill hantera.</span><span class="sxs-lookup"><span data-stu-id="69898-131">Click the workspace you want to manage.</span></span>

<span data-ttu-id="69898-132">Arbetsytssidan har tre flikar:</span><span class="sxs-lookup"><span data-stu-id="69898-132">The workspace page has three tabs:</span></span>

* <span data-ttu-id="69898-133">**INSTRUMENTPANELEN** -gör att du kan visa arbetsytan användnings- och information</span><span class="sxs-lookup"><span data-stu-id="69898-133">**DASHBOARD** - Allows you to view workspace usage and information</span></span>
* <span data-ttu-id="69898-134">**Konfigurera** -kan du hantera åtkomst till arbetsytan</span><span class="sxs-lookup"><span data-stu-id="69898-134">**CONFIGURE** - Allows you to manage access to the workspace</span></span>
* <span data-ttu-id="69898-135">**WEBBTJÄNSTER** -låter dig hantera webbtjänster som har publicerats från arbetsytan</span><span class="sxs-lookup"><span data-stu-id="69898-135">**WEB SERVICES** - Allows you to manage Web services that have been published from this workspace</span></span>

### <a name="to-monitor-how-the-workspace-is-being-used"></a><span data-ttu-id="69898-136">Att övervaka hur arbetsytan används</span><span class="sxs-lookup"><span data-stu-id="69898-136">To monitor how the workspace is being used</span></span>
<span data-ttu-id="69898-137">Klicka på den **INSTRUMENTPANELEN** fliken.</span><span class="sxs-lookup"><span data-stu-id="69898-137">Click the **DASHBOARD** tab.</span></span>

<span data-ttu-id="69898-138">Du kan visa övergripande användning av arbetsytan och få en snabb överblick över arbetsytan information från instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="69898-138">From the dashboard, you can view overall usage of your workspace and get a quick glance of workspace information.</span></span>

* <span data-ttu-id="69898-139">Den **COMPUTE** diagrammet visar de beräkningsresurser som används av arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="69898-139">The **COMPUTE** chart shows the compute resources being used by the workspace.</span></span> <span data-ttu-id="69898-140">Du kan ändra vy för att visa relativa eller absoluta värden och du kan ändra den tidsperiod som visas i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="69898-140">You can change the view to display relative or absolute values, and you can change the timeframe displayed in the chart.</span></span>
* <span data-ttu-id="69898-141">**Användningsöversikten** visar Azure-lagring som används av arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="69898-141">**Usage overview** displays Azure storage being used by the workspace.</span></span>
* <span data-ttu-id="69898-142">**Snabböversikten** innehåller en översikt över arbetsyteinformation och länkar.</span><span class="sxs-lookup"><span data-stu-id="69898-142">**Quick glance** provides a summary of workspace information and useful links.</span></span>

> [!NOTE]
> <span data-ttu-id="69898-143">Den **logga in på ML Studio** länken öppnar Machine Learning Studio med Account du för närvarande är inloggad på.</span><span class="sxs-lookup"><span data-stu-id="69898-143">The **Sign-in to ML Studio** link opens Machine Learning Studio using the Microsoft Account you are currently signed into.</span></span> <span data-ttu-id="69898-144">Account du använde för att logga in på den klassiska Azure-portalen för att skapa en arbetsyta har inte behörighet att öppna arbetsytan automatiskt.</span><span class="sxs-lookup"><span data-stu-id="69898-144">The Microsoft Account you used to sign in to the Azure classic portal to create a workspace does not automatically have permission to open that workspace.</span></span> <span data-ttu-id="69898-145">Om du vill öppna en arbetsyta, måste du vara inloggad på Account som har definierats som ägare till arbetsytan eller måste du få en inbjudan från ägare att ansluta till arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="69898-145">To open a workspace, you must be signed in to the Microsoft Account that was defined as the owner of the workspace, or you need to receive an invitation from the owner to join the workspace.</span></span>
> 
> 

### <a name="to-grant-or-suspend-access-for-users"></a><span data-ttu-id="69898-146">Att bevilja eller pausa åtkomst för användare</span><span class="sxs-lookup"><span data-stu-id="69898-146">To grant or suspend access for users</span></span>
<span data-ttu-id="69898-147">Klicka på den **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="69898-147">Click the **CONFIGURE** tab.</span></span>

<span data-ttu-id="69898-148">På konfigurationsfliken kan du:</span><span class="sxs-lookup"><span data-stu-id="69898-148">From the configuration tab you can:</span></span>

* <span data-ttu-id="69898-149">Inaktivera åtkomst till Machine Learning-arbetsytan genom att klicka på NEKA.</span><span class="sxs-lookup"><span data-stu-id="69898-149">Suspend access to the Machine Learning workspace by clicking DENY.</span></span> <span data-ttu-id="69898-150">Användare kommer inte längre att kunna öppna arbetsytan i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="69898-150">Users will no longer be able to open the workspace in Machine Learning Studio.</span></span> <span data-ttu-id="69898-151">För att återställa åtkomsten, klickar du på TILLÅT.</span><span class="sxs-lookup"><span data-stu-id="69898-151">To restore access, click ALLOW.</span></span>

<span data-ttu-id="69898-152">Om du vill hantera ytterligare konton som har åtkomst till arbetsytan i Machine Learning Studio klickar du på **logga in på ML Studio** i den **INSTRUMENTPANELEN** fliken (se anmärkning som föregår angående **logga in på ML Studio**).</span><span class="sxs-lookup"><span data-stu-id="69898-152">To manage additional accounts who have access to the workspace in Machine Learning Studio, click **Sign-in to ML Studio** in the **DASHBOARD** tab (see the preceeding note regarding **Sign-in to ML Studio**).</span></span> <span data-ttu-id="69898-153">Då öppnas arbetsytan i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="69898-153">This opens the workspace in Machine Learning Studio.</span></span> <span data-ttu-id="69898-154">Härifrån kan du klicka på den **inställningar** fliken och sedan **användare**.</span><span class="sxs-lookup"><span data-stu-id="69898-154">From here, click the **SETTINGS** tab and then **USERS**.</span></span> <span data-ttu-id="69898-155">Du kan klicka på **bjuda in fler användare** att ge användare åtkomst till arbetsytan, eller välj en användare och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="69898-155">You can click **INVITE MORE USERS** to give users access to the workspace, or select a user and click **REMOVE**.</span></span>

### <a name="to-manage-web-services-in-this-workspace"></a><span data-ttu-id="69898-156">Hantera webbtjänster i den här arbetsytan</span><span class="sxs-lookup"><span data-stu-id="69898-156">To manage web services in this workspace</span></span>
<span data-ttu-id="69898-157">Klicka på den **WEB SERVICES** fliken.</span><span class="sxs-lookup"><span data-stu-id="69898-157">Click the **WEB SERVICES** tab.</span></span>

<span data-ttu-id="69898-158">Detta visar en lista över webbtjänster som publicerats från den här arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="69898-158">This displays a list of web services published from this workspace.</span></span>
<span data-ttu-id="69898-159">Klicka på namnet i listan för att öppna tjänstsidan för att hantera en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="69898-159">To manage a web service, click the name in the list to open the Web service page.</span></span>

<span data-ttu-id="69898-160">En webbtjänst kan ha en eller flera slutpunkter som definierats.</span><span class="sxs-lookup"><span data-stu-id="69898-160">A Web service may have one or more endpoints defined.</span></span>

* <span data-ttu-id="69898-161">Du kan definiera flera slutpunkter förutom ”Default”-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="69898-161">You can define more endpoints in addition to the "Default" endpoint.</span></span> <span data-ttu-id="69898-162">Om du vill lägga till slutpunkten, klickar du på **hantera slutpunkter** längst ned i instrumentpanelen för att öppna Azure Machine Learning-webbtjänster-portalen.</span><span class="sxs-lookup"><span data-stu-id="69898-162">To add the endpoint, click **Manage Endpoints** at the bottom of the dashboard to open the Azure Machine Learning Web Services portal.</span></span>
* <span data-ttu-id="69898-163">Ta bort en slutpunkt (du inte kan ta bort slutpunkten för ”Default”) och klicka på kryssrutan i början av raden slutpunkt och på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="69898-163">To delete an endpoint (you cannot delete the "Default" endpoint), click the check box at the beginning of the endpoint row, and click **DELETE**.</span></span> <span data-ttu-id="69898-164">Detta tar bort slutpunkten från webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="69898-164">This removes the endpoint from the Web service.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="69898-165">Om ett program använder webbtjänstslutpunkt när slutpunkten har tagits bort, får programmet ett fel nästa gång den försöker komma åt tjänsten.</span><span class="sxs-lookup"><span data-stu-id="69898-165">If an application is using the web service endpoint when the endpoint is deleted, the application will receive an error the next time it tries to access the service.</span></span>
  > 
  > 

<span data-ttu-id="69898-166">Klicka på namnet på en slutpunkt för webbtjänsten så att den öppnas.</span><span class="sxs-lookup"><span data-stu-id="69898-166">Click the name of a Web service endpoint to open it.</span></span> 

<span data-ttu-id="69898-167">Du kan visa övergripande användning av webbtjänsten under en viss tidsperiod från instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="69898-167">From the dashboard, you can view overall usage of your Web service over a period of time.</span></span> <span data-ttu-id="69898-168">Du kan välja vilken om du vill visa från listrutan Period i det övre högra hörnet för användning.</span><span class="sxs-lookup"><span data-stu-id="69898-168">You can select the period to view from the Period dropdown menu in the upper right of the usage charts.</span></span> <span data-ttu-id="69898-169">Instrumentpanelen visar följande information:</span><span class="sxs-lookup"><span data-stu-id="69898-169">The dashboard shows the following information:</span></span>

* <span data-ttu-id="69898-170">**Begäranden över tid** visar ett steg diagram över hur många begäranden för den valda tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="69898-170">**Requests Over Time** displays a step graph of the number of requests over the selected time period.</span></span> <span data-ttu-id="69898-171">Det hjälper dig identifiera om det uppstår toppar i användning.</span><span class="sxs-lookup"><span data-stu-id="69898-171">It can help identify if you are experiencing spikes in usage.</span></span>
* <span data-ttu-id="69898-172">**Begäran och svar begäranden** visar det totala antalet anrop för begäran och svar tjänsten har tagit emot och den valda tidsperioden och hur många av dem misslyckades.</span><span class="sxs-lookup"><span data-stu-id="69898-172">**Request-Response Requests** displays the total number of Request-Response calls the service has received over the selected time period and how many of them failed.</span></span>
* <span data-ttu-id="69898-173">**Genomsnittlig tid för begäran och svar Compute** visar ett genomsnitt av tiden som krävs för att köra den mottagna begäranden.</span><span class="sxs-lookup"><span data-stu-id="69898-173">**Average Request-Response Compute Time** displays an average of the time needed to execute the received requests.</span></span>
* <span data-ttu-id="69898-174">**Batch-begäranden** visar det totala antalet gruppbegäranden tjänsten har tagit emot och den valda tidsperioden och hur många av dem misslyckades.</span><span class="sxs-lookup"><span data-stu-id="69898-174">**Batch Requests** displays the total number of Batch Requests the service has received over the selected time period and how many of them failed.</span></span>
* <span data-ttu-id="69898-175">**Genomsnittlig svarstid för jobbet** visar ett genomsnitt av tiden som krävs för att köra den mottagna begäranden.</span><span class="sxs-lookup"><span data-stu-id="69898-175">**Average Job Latency** displays an average of the time needed to execute the received requests.</span></span>
* <span data-ttu-id="69898-176">**Fel** visar det sammanlagda antalet fel som har inträffat på anrop till webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="69898-176">**Errors** displays the aggregate number of errors that have occurred on calls to the web service.</span></span>
* <span data-ttu-id="69898-177">**Services kostnader** visar avgifterna för den faktureringsplan som är associerad med tjänsten.</span><span class="sxs-lookup"><span data-stu-id="69898-177">**Services Costs** displays the charges for the billing plan associated with the service.</span></span>

<span data-ttu-id="69898-178">Du kan använda sidan Konfigurera för att uppdatera följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="69898-178">From the Configure page, you can update the following properties:</span></span>

* <span data-ttu-id="69898-179">**Beskrivning** kan du ange en beskrivning för webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="69898-179">**Description** allows you to enter a description for the Web service.</span></span> <span data-ttu-id="69898-180">Beskrivningen är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="69898-180">Description is a required field.</span></span>
* <span data-ttu-id="69898-181">**Loggning av** kan du aktivera eller inaktivera felloggning på slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="69898-181">**Logging** allows you to enable or disable error logging on the endpoint.</span></span> <span data-ttu-id="69898-182">Mer information om loggning finns i Aktivera [loggning för Machine Learning-webbtjänster](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="69898-182">For more information on Logging, see Enable [logging for Machine Learning web services](machine-learning-web-services-logging.md).</span></span>
* <span data-ttu-id="69898-183">**Aktivera exempeldata** kan du ange exempeldata som du kan använda för att testa tjänsten begäran och svar.</span><span class="sxs-lookup"><span data-stu-id="69898-183">**Enable Sample data** allows you to provide sample data that you can use to test the Request-Response service.</span></span> <span data-ttu-id="69898-184">Om du har skapat webbtjänsten i Machine Learning Studio hämtas exempeldata från data din används för att träna din modell.</span><span class="sxs-lookup"><span data-stu-id="69898-184">If you created the web service in Machine Learning Studio, the sample data is taken from the data your used to train your model.</span></span> <span data-ttu-id="69898-185">Om du har skapat tjänsten programmässigt hämtas data från de exempel informationen som en del av JSON-paketet.</span><span class="sxs-lookup"><span data-stu-id="69898-185">If you created the service programmatically, the data is taken from the example data you provided as part of the JSON package.</span></span>

