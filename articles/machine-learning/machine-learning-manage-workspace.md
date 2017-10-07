---
title: aaaManage Machine Learning-arbetsytan | Microsoft Docs
description: "Hantera åtkomst tooAzure Machine Learning arbetsytor och distribuera och hantera ml – API-webbtjänster"
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
ms.openlocfilehash: 7bd378d82aa46f4340ca974c7dc5a612c089cdca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-an-azure-machine-learning-workspace"></a><span data-ttu-id="be0d9-103">Hantera en Azure Machine Learning-arbetsyta</span><span class="sxs-lookup"><span data-stu-id="be0d9-103">Manage an Azure Machine Learning workspace</span></span>

> [!NOTE]
> <span data-ttu-id="be0d9-104">Information om hur du hanterar Web services hello Machine Learning-webbtjänster portal finns [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="be0d9-104">For information on managing Web services in hello Machine Learning Web Services portal, see [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span>
> 
> 

<span data-ttu-id="be0d9-105">Du kan hantera Machine Learning arbetsytor i antingen hello Azure-portalen eller hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="be0d9-105">You can manage Machine Learning workspaces in either hello Azure portal or hello Azure classic portal.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="use-hello-azure-portal"></a><span data-ttu-id="be0d9-106">Använd hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="be0d9-106">Use hello Azure portal</span></span>

<span data-ttu-id="be0d9-107">toomanage en arbetsyta i hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="be0d9-107">toomanage a workspace in hello Azure portal:</span></span>

1. <span data-ttu-id="be0d9-108">Logga in toohello [Azure-portalen](https://portal.azure.com/) med ett administratörskonto för Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="be0d9-108">Sign in toohello [Azure portal](https://portal.azure.com/) using an Azure subscription administrator account.</span></span>
2. <span data-ttu-id="be0d9-109">Ange i sökrutan överst hello på hello sidan hello ”datorn learning arbetsytor” och välj sedan **Machine Learning arbetsytor**.</span><span class="sxs-lookup"><span data-stu-id="be0d9-109">In hello search box at hello top of hello page, enter "machine learning workspaces" and then select **Machine Learning Workspaces**.</span></span>
3. <span data-ttu-id="be0d9-110">Klicka på hello-arbetsyta som du vill toomanage.</span><span class="sxs-lookup"><span data-stu-id="be0d9-110">Click hello workspace you want toomanage.</span></span>

<span data-ttu-id="be0d9-111">Dessutom toohello standard management resursinformation och alternativ som är tillgängliga, kan du:</span><span class="sxs-lookup"><span data-stu-id="be0d9-111">In addition toohello standard resource management information and options available, you can:</span></span>

- <span data-ttu-id="be0d9-112">Visa **egenskaper** – den här sidan visar hello arbetsytan och resursen och du kan ändra hello prenumeration och resursgrupp som den här arbetsytan är kopplad till.</span><span class="sxs-lookup"><span data-stu-id="be0d9-112">View **Properties** - This page displays hello workspace and resource information, and you can change hello subscription and resource group that this workspace is connected with.</span></span>
- <span data-ttu-id="be0d9-113">**Omsynkronisering Lagringsnycklar** -hello arbetsytan underhåller nycklar toohello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="be0d9-113">**Resync Storage Keys** - hello workspace maintains keys toohello storage account.</span></span> <span data-ttu-id="be0d9-114">Om hello storage-konto ändras nycklar och du kan klicka på **omsynkronisering nycklar** toosynchronize hello nycklar med hello-arbetsyta.</span><span class="sxs-lookup"><span data-stu-id="be0d9-114">If hello storage account changes keys, then you can click **Resync keys** toosynchronize hello keys with hello workspace.</span></span>

<span data-ttu-id="be0d9-115">toomanage hello webbtjänster som är associerade med den här arbetsytan kan använda hello Machine Learning-webbtjänster portal.</span><span class="sxs-lookup"><span data-stu-id="be0d9-115">toomanage hello web services associated with this workspace, use hello Machine Learning Web Services portal.</span></span> <span data-ttu-id="be0d9-116">Se [hantera en webbtjänst med hjälp av hello Azure Machine Learning-webbtjänster portal](machine-learning-manage-new-webservice.md) fullständig information.</span><span class="sxs-lookup"><span data-stu-id="be0d9-116">See [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md) for complete information.</span></span>

> [!NOTE]
> <span data-ttu-id="be0d9-117">toodeploy eller hantera nya webbtjänster som du måste tilldelas en deltagare eller administratörsroll hello prenumeration toowhich hello webbtjänsten har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="be0d9-117">toodeploy or manage New web services you must be assigned a contributor or administrator role on hello subscription toowhich hello web service is deployed.</span></span> <span data-ttu-id="be0d9-118">Om du bjuda in en annan användare tooa maskininlärning arbetsytan måste du tilldela dem tooa rollen deltagare eller administratör på hello prenumeration innan de kan distribuera eller hantera webbtjänster.</span><span class="sxs-lookup"><span data-stu-id="be0d9-118">If you invite another user tooa machine learning workspace, you must assign them tooa contributor or administrator role on hello subscription before they can deploy or manage web services.</span></span> 
> 
><span data-ttu-id="be0d9-119">Mer information om inställningen åtkomstbehörighet finns [visa åtkomst-tilldelningar för användare och grupper i hello Azure portal – förhandsversion](../active-directory/role-based-access-control-manage-assignments.md).</span><span class="sxs-lookup"><span data-stu-id="be0d9-119">For more information on setting access permissions, see [View access assignments for users and groups in hello Azure portal - Public preview](../active-directory/role-based-access-control-manage-assignments.md).</span></span>

## <a name="use-hello-azure-classic-portal"></a><span data-ttu-id="be0d9-120">Använd hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="be0d9-120">Use hello Azure classic portal</span></span>

<span data-ttu-id="be0d9-121">Du kan hantera dina Machine Learning arbetsytor att med hello klassiska Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="be0d9-121">Using hello Azure classic portal, you can manage your Machine Learning workspaces to:</span></span>

* <span data-ttu-id="be0d9-122">Övervaka hur hello arbetsytan används</span><span class="sxs-lookup"><span data-stu-id="be0d9-122">Monitor how hello workspace is being used</span></span>
* <span data-ttu-id="be0d9-123">Konfigurera hello arbetsytan tooallow eller nekar åtkomst</span><span class="sxs-lookup"><span data-stu-id="be0d9-123">Configure hello workspace tooallow or deny access</span></span>
* <span data-ttu-id="be0d9-124">Hantera webbtjänster som skapats i hello arbetsyta</span><span class="sxs-lookup"><span data-stu-id="be0d9-124">Manage Web services created in hello workspace</span></span>
* <span data-ttu-id="be0d9-125">Ta bort hello-arbetsytan</span><span class="sxs-lookup"><span data-stu-id="be0d9-125">Delete hello workspace</span></span>

<span data-ttu-id="be0d9-126">Dessutom innehåller hello instrumentpanelen en översikt över arbetsytan-användning och en snabb överblick över din arbetsyta information.</span><span class="sxs-lookup"><span data-stu-id="be0d9-126">In addition, hello dashboard tab provides an overview of your workspace usage and a quick glance of your workspace information.</span></span>  

> [!TIP]
> <span data-ttu-id="be0d9-127">I Azure Machine Learning Studio, på hello **WEB SERVICES** fliken, du kan lägga till, uppdatera eller ta bort en Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="be0d9-127">In Azure Machine Learning Studio, on hello **WEB SERVICES** tab, you can add, update, or delete a Machine Learning Web service.</span></span>
> 
> 

<span data-ttu-id="be0d9-128">toomanage en arbetsyta i hello klassiska Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="be0d9-128">toomanage a workspace in hello Azure classic portal:</span></span>

1. <span data-ttu-id="be0d9-129">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) med Microsoft Azure-konto, Använd hello-konto som är kopplad till hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="be0d9-129">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) using your Microsoft Azure account - use hello account that's associated with hello Azure subscription.</span></span>
2. <span data-ttu-id="be0d9-130">Klicka på hello Microsoft Azure-tjänster Kontrollpanelen **MASKININLÄRNING**.</span><span class="sxs-lookup"><span data-stu-id="be0d9-130">In hello Microsoft Azure services panel, click **MACHINE LEARNING**.</span></span>
3. <span data-ttu-id="be0d9-131">Klicka på hello-arbetsyta som du vill toomanage.</span><span class="sxs-lookup"><span data-stu-id="be0d9-131">Click hello workspace you want toomanage.</span></span>

<span data-ttu-id="be0d9-132">hello arbetsytssidan har tre flikar:</span><span class="sxs-lookup"><span data-stu-id="be0d9-132">hello workspace page has three tabs:</span></span>

* <span data-ttu-id="be0d9-133">**INSTRUMENTPANELEN** -gör att du tooview arbetsytan användning och information</span><span class="sxs-lookup"><span data-stu-id="be0d9-133">**DASHBOARD** - Allows you tooview workspace usage and information</span></span>
* <span data-ttu-id="be0d9-134">**Konfigurera** -tillåter toomanage åtkomst toohello arbetsytan</span><span class="sxs-lookup"><span data-stu-id="be0d9-134">**CONFIGURE** - Allows you toomanage access toohello workspace</span></span>
* <span data-ttu-id="be0d9-135">**WEBBTJÄNSTER** -tillåter toomanage webbtjänster som har publicerats från arbetsytan</span><span class="sxs-lookup"><span data-stu-id="be0d9-135">**WEB SERVICES** - Allows you toomanage Web services that have been published from this workspace</span></span>

### <a name="toomonitor-how-hello-workspace-is-being-used"></a><span data-ttu-id="be0d9-136">toomonitor hur hello arbetsytan används</span><span class="sxs-lookup"><span data-stu-id="be0d9-136">toomonitor how hello workspace is being used</span></span>
<span data-ttu-id="be0d9-137">Klicka på hello **INSTRUMENTPANELEN** fliken.</span><span class="sxs-lookup"><span data-stu-id="be0d9-137">Click hello **DASHBOARD** tab.</span></span>

<span data-ttu-id="be0d9-138">Du kan visa övergripande användning av arbetsytan och få en snabb överblick över arbetsytan information från hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="be0d9-138">From hello dashboard, you can view overall usage of your workspace and get a quick glance of workspace information.</span></span>

* <span data-ttu-id="be0d9-139">Hej **COMPUTE** diagrammet visar hello beräkningsresurser som används av hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="be0d9-139">hello **COMPUTE** chart shows hello compute resources being used by hello workspace.</span></span> <span data-ttu-id="be0d9-140">Du kan ändra hello visa toodisplay relativa eller absoluta värden och du kan ändra hello tidsram som visas i diagrammet hello.</span><span class="sxs-lookup"><span data-stu-id="be0d9-140">You can change hello view toodisplay relative or absolute values, and you can change hello timeframe displayed in hello chart.</span></span>
* <span data-ttu-id="be0d9-141">**Användningsöversikten** visar Azure-lagring som används av hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="be0d9-141">**Usage overview** displays Azure storage being used by hello workspace.</span></span>
* <span data-ttu-id="be0d9-142">**Snabböversikten** innehåller en översikt över arbetsyteinformation och länkar.</span><span class="sxs-lookup"><span data-stu-id="be0d9-142">**Quick glance** provides a summary of workspace information and useful links.</span></span>

> [!NOTE]
> <span data-ttu-id="be0d9-143">Hej **inloggning tooML Studio** länken öppnar Machine Learning Studio med hello Account du för närvarande är inloggad på.</span><span class="sxs-lookup"><span data-stu-id="be0d9-143">hello **Sign-in tooML Studio** link opens Machine Learning Studio using hello Microsoft Account you are currently signed into.</span></span> <span data-ttu-id="be0d9-144">Hej Account du använde toosign i toohello Azure klassiska portal toocreate en arbetsyta har automatiskt inte behörighet tooopen arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="be0d9-144">hello Microsoft Account you used toosign in toohello Azure classic portal toocreate a workspace does not automatically have permission tooopen that workspace.</span></span> <span data-ttu-id="be0d9-145">tooopen en arbetsyta, måste du vara inloggad i toohello Account som har definierats som hello ägare av hello arbetsytan eller måste tooreceive en inbjudan från hello ägare toojoin hello arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="be0d9-145">tooopen a workspace, you must be signed in toohello Microsoft Account that was defined as hello owner of hello workspace, or you need tooreceive an invitation from hello owner toojoin hello workspace.</span></span>
> 
> 

### <a name="toogrant-or-suspend-access-for-users"></a><span data-ttu-id="be0d9-146">toogrant eller pausar åtkomst för användare</span><span class="sxs-lookup"><span data-stu-id="be0d9-146">toogrant or suspend access for users</span></span>
<span data-ttu-id="be0d9-147">Klicka på hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="be0d9-147">Click hello **CONFIGURE** tab.</span></span>

<span data-ttu-id="be0d9-148">Från fliken för hello-konfiguration kan du:</span><span class="sxs-lookup"><span data-stu-id="be0d9-148">From hello configuration tab you can:</span></span>

* <span data-ttu-id="be0d9-149">Pausa åtkomst toohello Machine Learning-arbetsytan genom att klicka på NEKA.</span><span class="sxs-lookup"><span data-stu-id="be0d9-149">Suspend access toohello Machine Learning workspace by clicking DENY.</span></span> <span data-ttu-id="be0d9-150">Användare kommer inte längre att kunna tooopen hello arbetsyta i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="be0d9-150">Users will no longer be able tooopen hello workspace in Machine Learning Studio.</span></span> <span data-ttu-id="be0d9-151">toorestore åtkomst till, klickar du på TILLÅT.</span><span class="sxs-lookup"><span data-stu-id="be0d9-151">toorestore access, click ALLOW.</span></span>

<span data-ttu-id="be0d9-152">toomanage ytterligare konton som har åtkomst toohello arbetsyta i Machine Learning Studio klickar du på **inloggning tooML Studio** i hello **INSTRUMENTPANELEN** fliken (se anmärkning för hello som föregår rörande  **Sign-in tooML Studio**).</span><span class="sxs-lookup"><span data-stu-id="be0d9-152">toomanage additional accounts who have access toohello workspace in Machine Learning Studio, click **Sign-in tooML Studio** in hello **DASHBOARD** tab (see hello preceeding note regarding **Sign-in tooML Studio**).</span></span> <span data-ttu-id="be0d9-153">Detta öppnar hello arbetsyta i Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="be0d9-153">This opens hello workspace in Machine Learning Studio.</span></span> <span data-ttu-id="be0d9-154">Klicka på hello från den här **inställningar** fliken och sedan **användare**.</span><span class="sxs-lookup"><span data-stu-id="be0d9-154">From here, click hello **SETTINGS** tab and then **USERS**.</span></span> <span data-ttu-id="be0d9-155">Du kan klicka på **bjuda in fler användare** toogive användare åtkomst toohello arbetsytan, eller välj en användare och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="be0d9-155">You can click **INVITE MORE USERS** toogive users access toohello workspace, or select a user and click **REMOVE**.</span></span>

### <a name="toomanage-web-services-in-this-workspace"></a><span data-ttu-id="be0d9-156">toomanage webbtjänster i den här arbetsytan</span><span class="sxs-lookup"><span data-stu-id="be0d9-156">toomanage web services in this workspace</span></span>
<span data-ttu-id="be0d9-157">Klicka på hello **WEB SERVICES** fliken.</span><span class="sxs-lookup"><span data-stu-id="be0d9-157">Click hello **WEB SERVICES** tab.</span></span>

<span data-ttu-id="be0d9-158">Detta visar en lista över webbtjänster som publicerats från den här arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="be0d9-158">This displays a list of web services published from this workspace.</span></span>
<span data-ttu-id="be0d9-159">toomanage en webbtjänst, klicka på hello i hello listan tooopen hello-webbtjänstsida.</span><span class="sxs-lookup"><span data-stu-id="be0d9-159">toomanage a web service, click hello name in hello list tooopen hello Web service page.</span></span>

<span data-ttu-id="be0d9-160">En webbtjänst kan ha en eller flera slutpunkter som definierats.</span><span class="sxs-lookup"><span data-stu-id="be0d9-160">A Web service may have one or more endpoints defined.</span></span>

* <span data-ttu-id="be0d9-161">Du kan definiera flera slutpunkter i tillägg toohello ”standard” slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="be0d9-161">You can define more endpoints in addition toohello "Default" endpoint.</span></span> <span data-ttu-id="be0d9-162">tooadd Hej slutpunkt, klickar du på **hantera slutpunkter** längst hello hello instrumentpanelen tooopen hello Azure Machine Learning-webbtjänster portal.</span><span class="sxs-lookup"><span data-stu-id="be0d9-162">tooadd hello endpoint, click **Manage Endpoints** at hello bottom of hello dashboard tooopen hello Azure Machine Learning Web Services portal.</span></span>
* <span data-ttu-id="be0d9-163">toodelete slutpunkt (du inte kan ta bort hello ”standard” endpoint), markerar du kryssrutan för hello hello början av hello endpoint raden och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="be0d9-163">toodelete an endpoint (you cannot delete hello "Default" endpoint), click hello check box at hello beginning of hello endpoint row, and click **DELETE**.</span></span> <span data-ttu-id="be0d9-164">Detta tar bort hello endpoint från hello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="be0d9-164">This removes hello endpoint from hello Web service.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="be0d9-165">Om ett program använder hello webbtjänstslutpunkt när hello slutpunkt har tagits bort, får hello programmet ett fel hello nästa gång den försöker tooaccess hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="be0d9-165">If an application is using hello web service endpoint when hello endpoint is deleted, hello application will receive an error hello next time it tries tooaccess hello service.</span></span>
  > 
  > 

<span data-ttu-id="be0d9-166">Klicka på en Web service endpoint tooopen hello namn den.</span><span class="sxs-lookup"><span data-stu-id="be0d9-166">Click hello name of a Web service endpoint tooopen it.</span></span> 

<span data-ttu-id="be0d9-167">Du kan visa övergripande användning av webbtjänsten under en viss tidsperiod hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="be0d9-167">From hello dashboard, you can view overall usage of your Web service over a period of time.</span></span> <span data-ttu-id="be0d9-168">Du kan välja hello period tooview från hello Period listrutan i hello övre högra hörnet av hello användning diagram.</span><span class="sxs-lookup"><span data-stu-id="be0d9-168">You can select hello period tooview from hello Period dropdown menu in hello upper right of hello usage charts.</span></span> <span data-ttu-id="be0d9-169">hello instrumentpanelen visar hello följande information:</span><span class="sxs-lookup"><span data-stu-id="be0d9-169">hello dashboard shows hello following information:</span></span>

* <span data-ttu-id="be0d9-170">**Begäranden över tid** visar ett steg diagram över hello antal begäranden via hello tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="be0d9-170">**Requests Over Time** displays a step graph of hello number of requests over hello selected time period.</span></span> <span data-ttu-id="be0d9-171">Det hjälper dig identifiera om det uppstår toppar i användning.</span><span class="sxs-lookup"><span data-stu-id="be0d9-171">It can help identify if you are experiencing spikes in usage.</span></span>
* <span data-ttu-id="be0d9-172">**Begäran och svar begäranden** visar hello Totalt antal anrop för begäran och svar hello-tjänsten har tagit emot och hello tidsperioden och hur många av dem misslyckades.</span><span class="sxs-lookup"><span data-stu-id="be0d9-172">**Request-Response Requests** displays hello total number of Request-Response calls hello service has received over hello selected time period and how many of them failed.</span></span>
* <span data-ttu-id="be0d9-173">**Genomsnittlig tid för begäran och svar Compute** visar en Genomsnittlig tid för hello behövs tooexecute hello emot begäranden.</span><span class="sxs-lookup"><span data-stu-id="be0d9-173">**Average Request-Response Compute Time** displays an average of hello time needed tooexecute hello received requests.</span></span>
* <span data-ttu-id="be0d9-174">**Batch-begäranden** visar hello Totalt antal gruppbegäranden hello-tjänsten har tagit emot och hello tidsperiod och hur många av dem misslyckades.</span><span class="sxs-lookup"><span data-stu-id="be0d9-174">**Batch Requests** displays hello total number of Batch Requests hello service has received over hello selected time period and how many of them failed.</span></span>
* <span data-ttu-id="be0d9-175">**Genomsnittlig svarstid för jobbet** visar en Genomsnittlig tid för hello behövs tooexecute hello emot begäranden.</span><span class="sxs-lookup"><span data-stu-id="be0d9-175">**Average Job Latency** displays an average of hello time needed tooexecute hello received requests.</span></span>
* <span data-ttu-id="be0d9-176">**Fel** visar hello sammanlagda antalet fel som har inträffat på anrop toohello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="be0d9-176">**Errors** displays hello aggregate number of errors that have occurred on calls toohello web service.</span></span>
* <span data-ttu-id="be0d9-177">**Services kostnader** visar hello kostnader för hello faktureringsplan som associeras med hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="be0d9-177">**Services Costs** displays hello charges for hello billing plan associated with hello service.</span></span>

<span data-ttu-id="be0d9-178">Hello Konfigurera sidan kan du uppdatera hello följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="be0d9-178">From hello Configure page, you can update hello following properties:</span></span>

* <span data-ttu-id="be0d9-179">**Beskrivning** kan du tooenter en beskrivning för hello webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="be0d9-179">**Description** allows you tooenter a description for hello Web service.</span></span> <span data-ttu-id="be0d9-180">Beskrivningen är ett obligatoriskt fält.</span><span class="sxs-lookup"><span data-stu-id="be0d9-180">Description is a required field.</span></span>
* <span data-ttu-id="be0d9-181">**Loggning av** kan du tooenable eller inaktivera felloggning på hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="be0d9-181">**Logging** allows you tooenable or disable error logging on hello endpoint.</span></span> <span data-ttu-id="be0d9-182">Mer information om loggning finns i Aktivera [loggning för Machine Learning-webbtjänster](machine-learning-web-services-logging.md).</span><span class="sxs-lookup"><span data-stu-id="be0d9-182">For more information on Logging, see Enable [logging for Machine Learning web services](machine-learning-web-services-logging.md).</span></span>
* <span data-ttu-id="be0d9-183">**Aktivera exempeldata** kan du tooprovide exempeldata som du kan använda tootest hello svar på begäranden tjänsten.</span><span class="sxs-lookup"><span data-stu-id="be0d9-183">**Enable Sample data** allows you tooprovide sample data that you can use tootest hello Request-Response service.</span></span> <span data-ttu-id="be0d9-184">Om du har skapat hello-webbtjänsten i Machine Learning Studio hello exempeldata hämtas från hello data din används tootrain din modell.</span><span class="sxs-lookup"><span data-stu-id="be0d9-184">If you created hello web service in Machine Learning Studio, hello sample data is taken from hello data your used tootrain your model.</span></span> <span data-ttu-id="be0d9-185">Om du har skapat hello service programmässigt hämtas hello data från hello exempeldata som du angav som en del av hello JSON-paketet.</span><span class="sxs-lookup"><span data-stu-id="be0d9-185">If you created hello service programmatically, hello data is taken from hello example data you provided as part of hello JSON package.</span></span>

