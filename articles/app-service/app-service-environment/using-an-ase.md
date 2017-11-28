---
title: "aaaUse en Azure Apptjänst-miljö"
description: "Hur toocreate, publicera och skala appar i Azure Apptjänst-miljö"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a22450c4-9b8b-41d4-9568-c4646f4cf66b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: 30c89e384efc07c560254856c0ca7d4eb4b1f010
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-an-app-service-environment"></a><span data-ttu-id="291e9-103">Använd en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="291e9-103">Use an App Service environment</span></span> #

## <a name="overview"></a><span data-ttu-id="291e9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="291e9-104">Overview</span></span> ##

<span data-ttu-id="291e9-105">Azure Apptjänst-miljön är en distribution av Azure App Service till ett undernät i kundens virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="291e9-105">Azure App Service Environment is a deployment of Azure App Service into a subnet in a customer’s Azure virtual network.</span></span> <span data-ttu-id="291e9-106">Det består av:</span><span class="sxs-lookup"><span data-stu-id="291e9-106">It consists of:</span></span>

- <span data-ttu-id="291e9-107">**Främre ends**: hello frontwebbservrarna är där HTTP/HTTPS avslutas i en Apptjänst-miljö (ASE).</span><span class="sxs-lookup"><span data-stu-id="291e9-107">**Front ends**: hello front ends are where HTTP/HTTPS terminates in an App Service environment (ASE).</span></span>
- <span data-ttu-id="291e9-108">**Anställda**: hello arbetare är hello-resurser som är värdar för dina appar.</span><span class="sxs-lookup"><span data-stu-id="291e9-108">**Workers**: hello workers are hello resources that host your apps.</span></span>
- <span data-ttu-id="291e9-109">**Databasen**: hello databasen innehåller information som definierar hello-miljö.</span><span class="sxs-lookup"><span data-stu-id="291e9-109">**Database**: hello database holds information that defines hello environment.</span></span>
- <span data-ttu-id="291e9-110">**Lagring**: hello lagring är används toohost hello kunden publicerade appar.</span><span class="sxs-lookup"><span data-stu-id="291e9-110">**Storage**: hello storage is used toohost hello customer-published apps.</span></span>

> [!NOTE]
> <span data-ttu-id="291e9-111">Det finns två versioner av Apptjänstmiljö: ASEv1 och ASEv2.</span><span class="sxs-lookup"><span data-stu-id="291e9-111">There are two versions of App Service Environment: ASEv1 and ASEv2.</span></span> <span data-ttu-id="291e9-112">Du måste hantera hello resurser innan du kan använda dem i ASEv1.</span><span class="sxs-lookup"><span data-stu-id="291e9-112">In ASEv1, you must manage hello resources before you can use them.</span></span> <span data-ttu-id="291e9-113">toolearn hur tooconfigure och hantera ASEv1, se [konfigurerar du en Apptjänst-miljö v1][ConfigureASEv1].</span><span class="sxs-lookup"><span data-stu-id="291e9-113">toolearn how tooconfigure and manage ASEv1, see [Configure an App Service environment v1][ConfigureASEv1].</span></span> <span data-ttu-id="291e9-114">hello resten av den här artikeln fokuserar på ASEv2.</span><span class="sxs-lookup"><span data-stu-id="291e9-114">hello rest of this article focuses on ASEv2.</span></span>
>
>

<span data-ttu-id="291e9-115">Du kan distribuera en ASE (ASEv1 och ASEv2) med en extern eller intern VIP för åtkomst till appen.</span><span class="sxs-lookup"><span data-stu-id="291e9-115">You can deploy an ASE (ASEv1 and ASEv2) with an external or internal VIP for app access.</span></span> <span data-ttu-id="291e9-116">hello-distribution med en extern VIP kallas vanligtvis en extern ASE.</span><span class="sxs-lookup"><span data-stu-id="291e9-116">hello deployment with an external VIP is commonly called an External ASE.</span></span> <span data-ttu-id="291e9-117">hello interna versionen kallas hello ILB ASE eftersom den använder en intern belastningsutjämnare (ILB).</span><span class="sxs-lookup"><span data-stu-id="291e9-117">hello internal version is called hello ILB ASE because it uses an internal load balancer (ILB).</span></span> <span data-ttu-id="291e9-118">toolearn mer om hello ILB ASE finns [skapa och använda en ILB ASE][MakeILBASE].</span><span class="sxs-lookup"><span data-stu-id="291e9-118">toolearn more about hello ILB ASE, see [Create and use an ILB ASE][MakeILBASE].</span></span>

## <a name="create-a-web-app-in-an-ase"></a><span data-ttu-id="291e9-119">Skapa en webbapp i en ASE</span><span class="sxs-lookup"><span data-stu-id="291e9-119">Create a web app in an ASE</span></span> ##

<span data-ttu-id="291e9-120">toocreate ett webbprogram i en ASE du använda hello samma process som när du skapar den som vanligt, men med några mindre skillnader.</span><span class="sxs-lookup"><span data-stu-id="291e9-120">toocreate a web app in an ASE, you use hello same process as when you create it normally, but with a few small differences.</span></span> <span data-ttu-id="291e9-121">När du skapar en ny programtjänstplan:</span><span class="sxs-lookup"><span data-stu-id="291e9-121">When you create a new App Service plan:</span></span>

- <span data-ttu-id="291e9-122">I stället för att välja en geografisk plats i vilka toodeploy appen, Välj en ASE som din plats.</span><span class="sxs-lookup"><span data-stu-id="291e9-122">Instead of choosing a geographic location in which toodeploy your app, you choose an ASE as your location.</span></span>
- <span data-ttu-id="291e9-123">Alla programtjänstplaner som skapats i en ASE måste finnas i en isolerad prisnivån.</span><span class="sxs-lookup"><span data-stu-id="291e9-123">All App Service plans created in an ASE must be in an Isolated pricing tier.</span></span>

<span data-ttu-id="291e9-124">Om du inte har en ASE, du kan skapa en genom att följa instruktionerna hello i [skapa en Apptjänst-miljö][MakeExternalASE].</span><span class="sxs-lookup"><span data-stu-id="291e9-124">If you don't have an ASE, you can create one by following hello instructions in [Create an App Service environment][MakeExternalASE].</span></span>

<span data-ttu-id="291e9-125">toocreate ett webbprogram i en ASE:</span><span class="sxs-lookup"><span data-stu-id="291e9-125">toocreate a web app in an ASE:</span></span>

1. <span data-ttu-id="291e9-126">Välj **nya** > **webb + mobilt** > **Webbapp**.</span><span class="sxs-lookup"><span data-stu-id="291e9-126">Select **New** > **Web + Mobile** > **Web App**.</span></span>

2. <span data-ttu-id="291e9-127">Ange ett namn för hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="291e9-127">Enter a name for hello web app.</span></span> <span data-ttu-id="291e9-128">Om du redan har valt en apptjänstplan i en ASE visar hello domännamn för hello app hello ASE hello domännamn.</span><span class="sxs-lookup"><span data-stu-id="291e9-128">If you already selected an App Service plan in an ASE, hello domain name for hello app reflects hello domain name of hello ASE.</span></span>

    ![Val av nätverksnamn för Web app][1]

3. <span data-ttu-id="291e9-130">Välj en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="291e9-130">Select a subscription.</span></span>

4. <span data-ttu-id="291e9-131">Ange ett namn för en ny resursgrupp eller välj **använda befintliga** och välj en hello nedrullningsbara listan.</span><span class="sxs-lookup"><span data-stu-id="291e9-131">Enter a name for a new resource group, or select **Use existing** and select one from hello drop-down list.</span></span>

5. <span data-ttu-id="291e9-132">Välj en befintlig programtjänstplan i din ASE eller skapa en ny genom att följa dessa steg:</span><span class="sxs-lookup"><span data-stu-id="291e9-132">Select an existing App Service plan in your ASE, or create a new one by following these steps:</span></span>

    <span data-ttu-id="291e9-133">a.</span><span class="sxs-lookup"><span data-stu-id="291e9-133">a.</span></span> <span data-ttu-id="291e9-134">Välj **skapa nya**.</span><span class="sxs-lookup"><span data-stu-id="291e9-134">Select **Create New**.</span></span>

    <span data-ttu-id="291e9-135">b.</span><span class="sxs-lookup"><span data-stu-id="291e9-135">b.</span></span> <span data-ttu-id="291e9-136">Ange hello namn för din programtjänstplan.</span><span class="sxs-lookup"><span data-stu-id="291e9-136">Enter hello name for your App Service plan.</span></span>

    <span data-ttu-id="291e9-137">c.</span><span class="sxs-lookup"><span data-stu-id="291e9-137">c.</span></span> <span data-ttu-id="291e9-138">Välj din ASE i hello **plats** listrutan.</span><span class="sxs-lookup"><span data-stu-id="291e9-138">Select your ASE in hello **Location** drop-down list.</span></span>

    <span data-ttu-id="291e9-139">d.</span><span class="sxs-lookup"><span data-stu-id="291e9-139">d.</span></span> <span data-ttu-id="291e9-140">Välj en **isolerad** prisnivån.</span><span class="sxs-lookup"><span data-stu-id="291e9-140">Select an **Isolated** pricing tier.</span></span> <span data-ttu-id="291e9-141">Välj **Välj**.</span><span class="sxs-lookup"><span data-stu-id="291e9-141">Select **Select**.</span></span>

    <span data-ttu-id="291e9-142">e.</span><span class="sxs-lookup"><span data-stu-id="291e9-142">e.</span></span> <span data-ttu-id="291e9-143">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="291e9-143">Select **OK**.</span></span>
    
    ![Isolerade prisnivåer][2]

6. <span data-ttu-id="291e9-145">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="291e9-145">Select **Create**.</span></span>

## <a name="how-scale-works"></a><span data-ttu-id="291e9-146">Hur skala fungerar</span><span class="sxs-lookup"><span data-stu-id="291e9-146">How scale works</span></span> ##

<span data-ttu-id="291e9-147">Varje App Service-appen körs i en apptjänstplan.</span><span class="sxs-lookup"><span data-stu-id="291e9-147">Every App Service app runs in an App Service plan.</span></span> <span data-ttu-id="291e9-148">hello behållaren modellen är miljöer håller App Service-planer och programtjänstplaner håller appar.</span><span class="sxs-lookup"><span data-stu-id="291e9-148">hello container model is environments hold App Service plans, and App Service plans hold apps.</span></span> <span data-ttu-id="291e9-149">När du skalar en app du skalar hello App Service-plan och skala därför alla hello appar i hello samma plan.</span><span class="sxs-lookup"><span data-stu-id="291e9-149">When you scale an app, you scale hello App Service plan and thus scale all hello apps in hello same plan.</span></span>

<span data-ttu-id="291e9-150">I ASEv2, när du skalar en apptjänstplan läggs hello behövs infrastruktur automatiskt.</span><span class="sxs-lookup"><span data-stu-id="291e9-150">In ASEv2, when you scale an App Service plan, hello needed infrastructure is automatically added.</span></span> <span data-ttu-id="291e9-151">Det finns en tidsfördröjning tooscale operations medan hello infrastruktur har lagts till.</span><span class="sxs-lookup"><span data-stu-id="291e9-151">There is a time delay tooscale operations while hello infrastructure is added.</span></span> <span data-ttu-id="291e9-152">ASEv1, måste infrastruktur hello behövs läggas innan du kan skapa eller skala upp din programtjänstplan.</span><span class="sxs-lookup"><span data-stu-id="291e9-152">In ASEv1, hello needed infrastructure must be added before you can create or scale out your App Service plan.</span></span> 

<span data-ttu-id="291e9-153">I hello multitenant Apptjänst skalning är vanligtvis omedelbar eftersom en pool av resurser är tillgängliga toosupport den.</span><span class="sxs-lookup"><span data-stu-id="291e9-153">In hello multitenant App Service, scaling is usually immediate because a pool of resources is readily available toosupport it.</span></span> <span data-ttu-id="291e9-154">Det finns ingen sådan buffert i en ASE och resurser vid behov.</span><span class="sxs-lookup"><span data-stu-id="291e9-154">In an ASE, there is no such buffer, and resources are allocated upon need.</span></span>

<span data-ttu-id="291e9-155">Du kan skala upp too100 instanser i en ASE.</span><span class="sxs-lookup"><span data-stu-id="291e9-155">In an ASE, you can scale up too100 instances.</span></span> <span data-ttu-id="291e9-156">De 100 instanserna kan finnas i en enda App Service-plan eller fördelade på flera programtjänstplaner.</span><span class="sxs-lookup"><span data-stu-id="291e9-156">Those 100 instances can be all in one single App Service plan or distributed across multiple App Service plans.</span></span>

## <a name="ip-addresses"></a><span data-ttu-id="291e9-157">IP-adresser</span><span class="sxs-lookup"><span data-stu-id="291e9-157">IP addresses</span></span> ##

<span data-ttu-id="291e9-158">Apptjänsten hello möjlighet tooallocate en dedicerad IP-adress tooan app.</span><span class="sxs-lookup"><span data-stu-id="291e9-158">App Service has hello ability tooallocate a dedicated IP address tooan app.</span></span> <span data-ttu-id="291e9-159">Den här funktionen är tillgänglig när du har konfigurerat IP-baserade SSL, enligt beskrivningen i [binda en befintlig anpassad SSL-certifikat tooAzure webbappar][ConfigureSSL].</span><span class="sxs-lookup"><span data-stu-id="291e9-159">This capability is available after you configure an IP-based SSL, as described in [Bind an existing custom SSL certificate tooAzure web apps][ConfigureSSL].</span></span> <span data-ttu-id="291e9-160">Men i en ASE finns viktiga undantag.</span><span class="sxs-lookup"><span data-stu-id="291e9-160">However, in an ASE, there is a notable exception.</span></span> <span data-ttu-id="291e9-161">Du kan inte lägga till ytterligare IP adresser toobe som används för en IP-baserade SSL i en ASE ILB.</span><span class="sxs-lookup"><span data-stu-id="291e9-161">You can't add additional IP addresses toobe used for an IP-based SSL in an ILB ASE.</span></span>

<span data-ttu-id="291e9-162">I ASEv1 behöver du tooallocate hello IP-adresser som resurser innan du kan använda dem.</span><span class="sxs-lookup"><span data-stu-id="291e9-162">In ASEv1, you need tooallocate hello IP addresses as resources before you can use them.</span></span> <span data-ttu-id="291e9-163">I ASEv2 använder du dem från din app precis som i hello multitenant Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="291e9-163">In ASEv2, you use them from your app just as you do in hello multitenant App Service.</span></span> <span data-ttu-id="291e9-164">Det finns alltid en ledig adress i ASEv2 too30 IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="291e9-164">There is always one spare address in ASEv2 up too30 IP addresses.</span></span> <span data-ttu-id="291e9-165">Varje gång läggs du använder någon annan så att en adress alltid är tillgängliga för användning.</span><span class="sxs-lookup"><span data-stu-id="291e9-165">Each time you use one, another is added so that an address is always readily available for use.</span></span> <span data-ttu-id="291e9-166">Fördröjning är krävs tooallocate en annan IP-adress, vilket förhindrar att lägga till IP-adresser i snabb följd.</span><span class="sxs-lookup"><span data-stu-id="291e9-166">A time delay is required tooallocate another IP address, which prevents adding IP addresses in quick succession.</span></span>

## <a name="front-end-scaling"></a><span data-ttu-id="291e9-167">Frontend skalning</span><span class="sxs-lookup"><span data-stu-id="291e9-167">Front-end scaling</span></span> ##

<span data-ttu-id="291e9-168">I ASEv2, när du skalar upp din App Service-planer arbetare läggs automatiskt toosupport dem.</span><span class="sxs-lookup"><span data-stu-id="291e9-168">In ASEv2, when you scale out your App Service plans, workers are automatically added toosupport them.</span></span> <span data-ttu-id="291e9-169">Varje ASE skapas med två frontwebbservrarna.</span><span class="sxs-lookup"><span data-stu-id="291e9-169">Every ASE is created with two front ends.</span></span> <span data-ttu-id="291e9-170">Dessutom skala hello frontwebbservrarna automatiskt ut med en hastighet av en klientdel för varje 15 instanser i App Service-planer.</span><span class="sxs-lookup"><span data-stu-id="291e9-170">In addition, hello front ends automatically scale out at a rate of one front end for every 15 instances in your App Service plans.</span></span> <span data-ttu-id="291e9-171">Till exempel om du har 15 instanser måste ha du tre frontwebbservrarna.</span><span class="sxs-lookup"><span data-stu-id="291e9-171">For example, if you have 15 instances, then you have three front ends.</span></span> <span data-ttu-id="291e9-172">Om du skalar too30 instanser, finns det fyra front slutar och så vidare.</span><span class="sxs-lookup"><span data-stu-id="291e9-172">If you scale too30 instances, then you have four front ends, and so on.</span></span>

<span data-ttu-id="291e9-173">Det här antalet frontwebbservrarna bör vara mer än tillräckligt för de flesta scenarier.</span><span class="sxs-lookup"><span data-stu-id="291e9-173">This number of front ends should be more than enough for most scenarios.</span></span> <span data-ttu-id="291e9-174">Du kan dock skala ut i en snabbare takt.</span><span class="sxs-lookup"><span data-stu-id="291e9-174">However, you can scale out at a faster rate.</span></span> <span data-ttu-id="291e9-175">Du kan ändra hello förhållandet tooas lite som en klientdel för varje fem instanser.</span><span class="sxs-lookup"><span data-stu-id="291e9-175">You can change hello ratio tooas low as one front end for every five instances.</span></span> <span data-ttu-id="291e9-176">Det finns en avgift för att ändra hello förhållandet.</span><span class="sxs-lookup"><span data-stu-id="291e9-176">There is a charge for changing hello ratio.</span></span> <span data-ttu-id="291e9-177">Mer information finns i [priser för Azure App Service][Pricing].</span><span class="sxs-lookup"><span data-stu-id="291e9-177">For more information, see [Azure App Service pricing][Pricing].</span></span>

<span data-ttu-id="291e9-178">Frontend resurser är hello HTTP/HTTPS-slutpunkt för hello ASE.</span><span class="sxs-lookup"><span data-stu-id="291e9-178">Front-end resources are hello HTTP/HTTPS endpoint for hello ASE.</span></span> <span data-ttu-id="291e9-179">Minnesanvändningen per klientdelen är konsekvent cirka 60 procent med hello frontend standardkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="291e9-179">With hello default front-end configuration, memory usage per front end is consistently around 60 percent.</span></span> <span data-ttu-id="291e9-180">Kundens arbetsbelastningar körs inte på en klientdel.</span><span class="sxs-lookup"><span data-stu-id="291e9-180">Customer workloads don't run on a front end.</span></span> <span data-ttu-id="291e9-181">hello är avgörande för en klientdel med avseende tooscale hello CPU som styrs i första hand av HTTPS-trafik.</span><span class="sxs-lookup"><span data-stu-id="291e9-181">hello key factor for a front end with respect tooscale is hello CPU, which is driven primarily by HTTPS traffic.</span></span>

## <a name="app-access"></a><span data-ttu-id="291e9-182">Åtkomst till appen</span><span class="sxs-lookup"><span data-stu-id="291e9-182">App access</span></span> ##

<span data-ttu-id="291e9-183">I en extern ASE skiljer hello-domän som används när du skapar appar sig från hello multitenant Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="291e9-183">In an External ASE, hello domain that's used when you create apps is different from hello multitenant App Service.</span></span> <span data-ttu-id="291e9-184">Den omfattar hello namnet på hello ASE.</span><span class="sxs-lookup"><span data-stu-id="291e9-184">It includes hello name of hello ASE.</span></span> <span data-ttu-id="291e9-185">Mer information om hur toocreate en extern ASE finns [skapa en Apptjänst-miljö][MakeExternalASE].</span><span class="sxs-lookup"><span data-stu-id="291e9-185">For more information on how toocreate an External ASE, see [Create an App Service environment][MakeExternalASE].</span></span> <span data-ttu-id="291e9-186">hello domännamn i en extern ASE ser ut som *.&lt; asename&gt;. p.azurewebsites.net*.</span><span class="sxs-lookup"><span data-stu-id="291e9-186">hello domain name in an External ASE looks like *.&lt;asename&gt;.p.azurewebsites.net*.</span></span> <span data-ttu-id="291e9-187">Om din ASE heter exempelvis _externa ase_ och du är värd för en app som kallas _contoso_ i den ASE du når den på följande URL: er hello:</span><span class="sxs-lookup"><span data-stu-id="291e9-187">For example, if your ASE is named _external-ase_ and you host an app called _contoso_ in that ASE, you reach it at hello following URLs:</span></span>

- <span data-ttu-id="291e9-188">Contoso.external ase.p.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="291e9-188">contoso.external-ase.p.azurewebsites.net</span></span>
- <span data-ttu-id="291e9-189">Contoso.SCM.external ase.p.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="291e9-189">contoso.scm.external-ase.p.azurewebsites.net</span></span>

<span data-ttu-id="291e9-190">hello URL contoso.scm.external-ase.p.azurewebsites.net är används tooaccess hello Kudu-konsolen eller för att publicera en app med hjälp av web distribuera.</span><span class="sxs-lookup"><span data-stu-id="291e9-190">hello URL contoso.scm.external-ase.p.azurewebsites.net is used tooaccess hello Kudu console or for publishing your app by using web deploy.</span></span> <span data-ttu-id="291e9-191">Mer information om hello Kudu-konsolen finns [Kudu-konsol för Azure App Service][Kudu].</span><span class="sxs-lookup"><span data-stu-id="291e9-191">For information on hello Kudu console, see [Kudu console for Azure App Service][Kudu].</span></span> <span data-ttu-id="291e9-192">Hej Kudu-konsolen ger dig en webbgränssnittet för felsökning, överför filer, redigera filer och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="291e9-192">hello Kudu console gives you a web UI for debugging, uploading files, editing files, and much more.</span></span>

<span data-ttu-id="291e9-193">I en ILB ASE bestämmer du hello domän vid tidpunkten för distribution.</span><span class="sxs-lookup"><span data-stu-id="291e9-193">In an ILB ASE, you determine hello domain at deployment time.</span></span> <span data-ttu-id="291e9-194">Mer information om hur toocreate en ILB ASE, se [skapa och använda en ILB ASE][MakeILBASE].</span><span class="sxs-lookup"><span data-stu-id="291e9-194">For more information on how toocreate an ILB ASE, see [Create and use an ILB ASE][MakeILBASE].</span></span> <span data-ttu-id="291e9-195">Om du anger domännamnet för hello _ilb ase.info_, hello appar i den ASE använda denna domän under skapande av app.</span><span class="sxs-lookup"><span data-stu-id="291e9-195">If you specify hello domain name _ilb-ase.info_, hello apps in that ASE use that domain during app creation.</span></span> <span data-ttu-id="291e9-196">För hello app med namnet _contoso_, hello URL: er är:</span><span class="sxs-lookup"><span data-stu-id="291e9-196">For hello app named _contoso_, hello URLs are:</span></span>

- <span data-ttu-id="291e9-197">Contoso.ilb ase.info</span><span class="sxs-lookup"><span data-stu-id="291e9-197">contoso.ilb-ase.info</span></span>
- <span data-ttu-id="291e9-198">Contoso.SCM.ilb ase.info</span><span class="sxs-lookup"><span data-stu-id="291e9-198">contoso.scm.ilb-ase.info</span></span>

## <a name="publishing"></a><span data-ttu-id="291e9-199">Publicering</span><span class="sxs-lookup"><span data-stu-id="291e9-199">Publishing</span></span> ##

<span data-ttu-id="291e9-200">Precis som med hello multitenant Apptjänst i en ASE kan du publicera med:</span><span class="sxs-lookup"><span data-stu-id="291e9-200">As with hello multitenant App Service, in an ASE you can publish with:</span></span>

- <span data-ttu-id="291e9-201">Web-distribution.</span><span class="sxs-lookup"><span data-stu-id="291e9-201">Web deployment.</span></span>
- <span data-ttu-id="291e9-202">FTP.</span><span class="sxs-lookup"><span data-stu-id="291e9-202">FTP.</span></span>
- <span data-ttu-id="291e9-203">Kontinuerlig integrering.</span><span class="sxs-lookup"><span data-stu-id="291e9-203">Continuous integration.</span></span>
- <span data-ttu-id="291e9-204">Dra och släpp i hello Kudu-konsolen.</span><span class="sxs-lookup"><span data-stu-id="291e9-204">Drag and drop in hello Kudu console.</span></span>
- <span data-ttu-id="291e9-205">IDE-miljö, till exempel för Visual Studio, Eclipse eller IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="291e9-205">An IDE, such as Visual Studio, Eclipse, or IntelliJ IDEA.</span></span>

<span data-ttu-id="291e9-206">Med en extern ASE hello dessa publiceringsalternativ alla fungerar samma.</span><span class="sxs-lookup"><span data-stu-id="291e9-206">With an External ASE, these publishing options all behave hello same.</span></span> <span data-ttu-id="291e9-207">Mer information finns i [distribution i Azure App Service][AppDeploy].</span><span class="sxs-lookup"><span data-stu-id="291e9-207">For more information, see [Deployment in Azure App Service][AppDeploy].</span></span> 

<span data-ttu-id="291e9-208">hello största skillnaden med publicering är med avseende tooan ILB ASE.</span><span class="sxs-lookup"><span data-stu-id="291e9-208">hello major difference with publishing is with respect tooan ILB ASE.</span></span> <span data-ttu-id="291e9-209">Med en ILB ASE hello publishing slutpunkter alla bara är tillgängliga via hello ILB.</span><span class="sxs-lookup"><span data-stu-id="291e9-209">With an ILB ASE, hello publishing endpoints are all available only through hello ILB.</span></span> <span data-ttu-id="291e9-210">Hej ILB finns i en privat IP-adress i hello ASE undernät i hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="291e9-210">hello ILB is on a private IP in hello ASE subnet in hello virtual network.</span></span> <span data-ttu-id="291e9-211">Om du inte har network access toohello ILB kan du publicera alla appar på den ASE.</span><span class="sxs-lookup"><span data-stu-id="291e9-211">If you don’t have network access toohello ILB, you can't publish any apps on that ASE.</span></span> <span data-ttu-id="291e9-212">Enligt beskrivningen i [skapa och använda en ILB ASE][MakeILBASE], behöver du tooconfigure DNS för hello appar i hello system.</span><span class="sxs-lookup"><span data-stu-id="291e9-212">As noted in [Create and use an ILB ASE][MakeILBASE], you need tooconfigure DNS for hello apps in hello system.</span></span> <span data-ttu-id="291e9-213">Som innehåller hello SCM slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="291e9-213">That includes hello SCM endpoint.</span></span> <span data-ttu-id="291e9-214">Om de inte är rätt definierad, kan du publicera.</span><span class="sxs-lookup"><span data-stu-id="291e9-214">If they're not defined properly, you can't publish.</span></span> <span data-ttu-id="291e9-215">Din IDEs måste också toohave network access toohello ILB i ordning toopublish direkt tooit.</span><span class="sxs-lookup"><span data-stu-id="291e9-215">Your IDEs also need toohave network access toohello ILB in order toopublish directly tooit.</span></span>

<span data-ttu-id="291e9-216">Internet-baserade CI system, t.ex GitHub och Visual Studio Team Services fungerar inte med en ILB ASE eftersom hello publishing slutpunkt inte är tillgänglig Internet.</span><span class="sxs-lookup"><span data-stu-id="291e9-216">Internet-based CI systems, such as GitHub and Visual Studio Team Services, don't work with an ILB ASE because hello publishing endpoint is not Internet accessible.</span></span> <span data-ttu-id="291e9-217">Du måste i stället toouse en CI-system som använder en pull-modell, till exempel Dropbox.</span><span class="sxs-lookup"><span data-stu-id="291e9-217">Instead, you need toouse a CI system that uses a pull model, such as Dropbox.</span></span>

<span data-ttu-id="291e9-218">hello publishing slutpunkter för appar i en ASE ILB använda hello domän som hello ILB ASE har skapats med.</span><span class="sxs-lookup"><span data-stu-id="291e9-218">hello publishing endpoints for apps in an ILB ASE use hello domain that hello ILB ASE was created with.</span></span> <span data-ttu-id="291e9-219">Du kan se den i hello app publiceringsprofilen och hello app portalbladet (i **översikt** > **Essentials** och även i **egenskaper**).</span><span class="sxs-lookup"><span data-stu-id="291e9-219">You can see it in hello app's publishing profile and in hello app's portal blade (in **Overview** > **Essentials** and also in **Properties**).</span></span> 

## <a name="pricing"></a><span data-ttu-id="291e9-220">Prissättning</span><span class="sxs-lookup"><span data-stu-id="291e9-220">Pricing</span></span> ##

<span data-ttu-id="291e9-221">hello priser SKU kallas **isolerad** skapades för ASEv2.</span><span class="sxs-lookup"><span data-stu-id="291e9-221">hello pricing SKU called **Isolated** was created for use only with ASEv2.</span></span> <span data-ttu-id="291e9-222">Alla programtjänstplaner som finns i ASEv2 finns i hello isolerad priser SKU.</span><span class="sxs-lookup"><span data-stu-id="291e9-222">All App Service plans that are hosted in ASEv2 are in hello Isolated pricing SKU.</span></span> <span data-ttu-id="291e9-223">Isolerade priser för Apptjänst-planen kan variera per region.</span><span class="sxs-lookup"><span data-stu-id="291e9-223">Isolated App Service plan rates can vary per region.</span></span> 

<span data-ttu-id="291e9-224">Dessutom toohello pris för din Apptjänst-planer finns det en platt för ASE sig själv.</span><span class="sxs-lookup"><span data-stu-id="291e9-224">In addition toohello price for your App Service plans, there is a flat rate for ASE itself.</span></span> <span data-ttu-id="291e9-225">hello debiteras inte ändras med hello storlek på din ASE och betalar för hello ASE infrastruktur på en standard skalning av ytterligare 1 frontend för varje 15 App Service-plan instanser.</span><span class="sxs-lookup"><span data-stu-id="291e9-225">hello flat rate doesn't change with hello size of your ASE and pays for hello ASE infrastructure at a default scaling rate of 1 additional front-end for every 15 App Service plan instances.</span></span>  

<span data-ttu-id="291e9-226">Om hello standard skala antalet 1 klientdelen för varje 15 App Service-plan instanser inte är tillräckligt snabbt, kan du justera hello förhållandet som framför-servrar har lagts till eller hello storleken på hello framför-servrar.</span><span class="sxs-lookup"><span data-stu-id="291e9-226">If hello default scale rate of 1 front end for every 15 App Service plan instances is not fast enough, you can adjust hello ratio at which front-ends are added or hello size of hello front-ends.</span></span>  <span data-ttu-id="291e9-227">När du justerar hello förhållande eller storlek, betalar för hello frontend kärnor inte skulle att lägga till som standard.</span><span class="sxs-lookup"><span data-stu-id="291e9-227">When you adjust hello ratio or size, you pay for hello front-end cores that would not be added by default.</span></span>  

<span data-ttu-id="291e9-228">Till exempel om du ändrar hello skala förhållandet too10 en klientdel har lagts till för varje 10 instanser i App Service-planer.</span><span class="sxs-lookup"><span data-stu-id="291e9-228">For example, if you adjust hello scale ratio too10, a front end is added for every 10 instances in your App Service plans.</span></span> <span data-ttu-id="291e9-229">hello fasta avgifter omfattar en skala andel en klientdel för varje 15 instanser.</span><span class="sxs-lookup"><span data-stu-id="291e9-229">hello flat fee covers a scale rate of one front end for every 15 instances.</span></span> <span data-ttu-id="291e9-230">Med en skala förhållandet mellan 10 betala en avgift för hello tredje klientdelen som har lagts till för hello 10 App Service-plan instanser.</span><span class="sxs-lookup"><span data-stu-id="291e9-230">With a scale ratio of 10, you pay a fee for hello third front end that's added for hello 10 App Service plan instances.</span></span> <span data-ttu-id="291e9-231">Du behöver inte toopay för den när du når 15 instanser eftersom den har lagts till automatiskt.</span><span class="sxs-lookup"><span data-stu-id="291e9-231">You don't need toopay for it when you reach 15 instances because it was added automatically.</span></span>

<span data-ttu-id="291e9-232">Om du justeras hello storleken på hello framför ends too2 kärnor men inte ändra hello förhållandet sedan du betalar för hello extra kärnor.</span><span class="sxs-lookup"><span data-stu-id="291e9-232">If you adjusted hello size of hello front-ends too2 cores but do not adjust hello ratio then you pay for hello extra cores.</span></span>  <span data-ttu-id="291e9-233">En ASE skapas med 2 framför-upphör kommer det även under hello automatisk skalning tröskelvärde du ska betala för extra 2 kärnor om du har ökat hello storlek too2 core framför-servrar.</span><span class="sxs-lookup"><span data-stu-id="291e9-233">An ASE is created with 2 front-ends, so even below hello automatic scaling threshold you would pay for 2 extra cores if you increased hello size too2 core front-ends.</span></span>

<span data-ttu-id="291e9-234">Mer information finns i [priser för Azure App Service][Pricing].</span><span class="sxs-lookup"><span data-stu-id="291e9-234">For more information, see [Azure App Service pricing][Pricing].</span></span>

## <a name="delete-an-ase"></a><span data-ttu-id="291e9-235">Ta bort en ASE</span><span class="sxs-lookup"><span data-stu-id="291e9-235">Delete an ASE</span></span> ##

<span data-ttu-id="291e9-236">toodelete en ASE:</span><span class="sxs-lookup"><span data-stu-id="291e9-236">toodelete an ASE:</span></span> 

1. <span data-ttu-id="291e9-237">Använd **ta bort** hello överst i hello **Apptjänstmiljö** bladet.</span><span class="sxs-lookup"><span data-stu-id="291e9-237">Use **Delete** at hello top of hello **App Service Environment** blade.</span></span> 

2. <span data-ttu-id="291e9-238">Ange hello namnet på din ASE tooconfirm som du vill toodelete den.</span><span class="sxs-lookup"><span data-stu-id="291e9-238">Enter hello name of your ASE tooconfirm that you want toodelete it.</span></span> <span data-ttu-id="291e9-239">När du tar bort en ASE kan du ta bort alla hello innehållsfiler i den.</span><span class="sxs-lookup"><span data-stu-id="291e9-239">When you delete an ASE, you delete all of hello content within it as well.</span></span> 

    ![Ta bort ASE][3]

<!--Image references-->
[1]: ./media/using_an_app_service_environment/usingase-appcreate.png
[2]: ./media/using_an_app_service_environment/usingase-pricingtiers.png
[3]: ./media/using_an_app_service_environment/usingase-delete.png


<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
