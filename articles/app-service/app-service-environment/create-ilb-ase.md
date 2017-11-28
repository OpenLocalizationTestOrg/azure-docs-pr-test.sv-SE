---
title: "aaaCreate och använda en intern belastningsutjämnare med Azure Apptjänst-miljön"
description: "Information om hur toocreate och använda en isolerad internet Azure Apptjänst-miljö"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: d019ca6f231c3acfdab4cd380db375a076802f7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a><span data-ttu-id="54027-103">Skapa och använda en intern belastningsutjämnare med en Apptjänst-miljö</span><span class="sxs-lookup"><span data-stu-id="54027-103">Create and use an internal load balancer with an App Service environment</span></span> #

 <span data-ttu-id="54027-104">Azure Apptjänst-miljön är en distribution av Azure App Service till ett undernät i Azure-nätverk (VNet).</span><span class="sxs-lookup"><span data-stu-id="54027-104">Azure App Service Environment is a deployment of Azure App Service into a subnet in an Azure virtual network (VNet).</span></span> <span data-ttu-id="54027-105">Det finns två sätt toodeploy Apptjänst-miljö (ASE):</span><span class="sxs-lookup"><span data-stu-id="54027-105">There are two ways toodeploy an App Service environment (ASE):</span></span> 

- <span data-ttu-id="54027-106">Med en VIP på en extern IP-adress, som kallas ofta för en extern ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-106">With a VIP on an external IP address, often called an External ASE.</span></span>
- <span data-ttu-id="54027-107">Med en VIP på en intern IP-adress kallas ofta för en ILB ASE eftersom hello intern slutpunkt är en intern belastningsutjämnare (ILB).</span><span class="sxs-lookup"><span data-stu-id="54027-107">With a VIP on an internal IP address, often called an ILB ASE because hello internal endpoint is an internal load balancer (ILB).</span></span> 

<span data-ttu-id="54027-108">Den här artikeln beskrivs hur du toocreate en ILB ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-108">This article shows you how toocreate an ILB ASE.</span></span> <span data-ttu-id="54027-109">En översikt över hello ASE finns [introduktion tooApp miljöer][Intro].</span><span class="sxs-lookup"><span data-stu-id="54027-109">For an overview on hello ASE, see [Introduction tooApp Service environments][Intro].</span></span> <span data-ttu-id="54027-110">hur toocreate en extern ASE Se toolearn [skapa en extern ASE][MakeExternalASE].</span><span class="sxs-lookup"><span data-stu-id="54027-110">toolearn how toocreate an External ASE, see [Create an External ASE][MakeExternalASE].</span></span>

## <a name="overview"></a><span data-ttu-id="54027-111">Översikt</span><span class="sxs-lookup"><span data-stu-id="54027-111">Overview</span></span> ##

<span data-ttu-id="54027-112">Du kan distribuera en ASE med en internet-tillgänglig slutpunkt eller en IP-adress på ditt VNet.</span><span class="sxs-lookup"><span data-stu-id="54027-112">You can deploy an ASE with an internet-accessible endpoint or with an IP address in your VNet.</span></span> <span data-ttu-id="54027-113">tooset hello IP-adress tooa VNet-adress, hello ASE måste distribueras med en ILB.</span><span class="sxs-lookup"><span data-stu-id="54027-113">tooset hello IP address tooa VNet address, hello ASE must be deployed with an ILB.</span></span> <span data-ttu-id="54027-114">När du distribuerar din ASE med en ILB, måste du ange:</span><span class="sxs-lookup"><span data-stu-id="54027-114">When you deploy your ASE with an ILB, you must provide:</span></span>

-   <span data-ttu-id="54027-115">En egen domän som du använder när du skapar dina appar.</span><span class="sxs-lookup"><span data-stu-id="54027-115">Your own domain that you use when you create your apps.</span></span>
-   <span data-ttu-id="54027-116">hello-certifikat som används för HTTPS.</span><span class="sxs-lookup"><span data-stu-id="54027-116">hello certificate used for HTTPS.</span></span>
-   <span data-ttu-id="54027-117">DNS-hantering för din domän.</span><span class="sxs-lookup"><span data-stu-id="54027-117">DNS management for your domain.</span></span>

<span data-ttu-id="54027-118">Du kan i RETUR göra saker som:</span><span class="sxs-lookup"><span data-stu-id="54027-118">In return, you can do things such as:</span></span>

-   <span data-ttu-id="54027-119">Värden intranätsprogram på ett säkert sätt i hello moln som du kommer åt via en plats-till-plats eller Azure ExpressRoute VPN.</span><span class="sxs-lookup"><span data-stu-id="54027-119">Host intranet applications securely in hello cloud, which you access through a site-to-site or Azure ExpressRoute VPN.</span></span>
-   <span data-ttu-id="54027-120">Värden appar i hello moln som inte listas i offentliga DNS-servrar.</span><span class="sxs-lookup"><span data-stu-id="54027-120">Host apps in hello cloud that aren't listed in public DNS servers.</span></span>
-   <span data-ttu-id="54027-121">Skapa isolerad internet backend-appar som din frontend appar på ett säkert sätt kan integreras med.</span><span class="sxs-lookup"><span data-stu-id="54027-121">Create internet-isolated back-end apps, which your front-end apps can securely integrate with.</span></span>

### <a name="disabled-functionality"></a><span data-ttu-id="54027-122">Inaktiverade funktioner</span><span class="sxs-lookup"><span data-stu-id="54027-122">Disabled functionality</span></span> ###

<span data-ttu-id="54027-123">Det finns vissa saker som du kan göra när du använder en ILB ASE:</span><span class="sxs-lookup"><span data-stu-id="54027-123">There are some things that you can't do when you use an ILB ASE:</span></span>

-   <span data-ttu-id="54027-124">Använd IP-baserade SSL.</span><span class="sxs-lookup"><span data-stu-id="54027-124">Use IP-based SSL.</span></span>
-   <span data-ttu-id="54027-125">Tilldela IP-adresser toospecific appar.</span><span class="sxs-lookup"><span data-stu-id="54027-125">Assign IP addresses toospecific apps.</span></span>
-   <span data-ttu-id="54027-126">Köpa och använda ett certifikat med en app via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="54027-126">Buy and use a certificate with an app through hello Azure portal.</span></span> <span data-ttu-id="54027-127">Du kan skaffa certifikat från en certifikatutfärdare och använda dem med dina appar.</span><span class="sxs-lookup"><span data-stu-id="54027-127">You can obtain certificates directly from a certificate authority and use them with your apps.</span></span> <span data-ttu-id="54027-128">Du kan inte hämta dem via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="54027-128">You can't obtain them through hello Azure portal.</span></span>

## <a name="create-an-ilb-ase"></a><span data-ttu-id="54027-129">Skapa en ILB ASE</span><span class="sxs-lookup"><span data-stu-id="54027-129">Create an ILB ASE</span></span> ##

<span data-ttu-id="54027-130">toocreate en ILB ASE:</span><span class="sxs-lookup"><span data-stu-id="54027-130">toocreate an ILB ASE:</span></span>

1. <span data-ttu-id="54027-131">Välj i hello Azure-portalen, **ny** > **webb + mobilt** > **Apptjänstmiljö**.</span><span class="sxs-lookup"><span data-stu-id="54027-131">In hello Azure portal, select **New** > **Web + Mobile** > **App Service Environment**.</span></span>

2. <span data-ttu-id="54027-132">Välj din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="54027-132">Select your subscription.</span></span>

3. <span data-ttu-id="54027-133">Välj eller skapa en Resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="54027-133">Select or create a resource group.</span></span>

4. <span data-ttu-id="54027-134">Välj eller skapa ett VNet.</span><span class="sxs-lookup"><span data-stu-id="54027-134">Select or create a VNet.</span></span>

5. <span data-ttu-id="54027-135">Om du väljer ett existerande VNet, måste toocreate ett undernät toohold hello ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-135">If you select an existing VNet, you need toocreate a subnet toohold hello ASE.</span></span> <span data-ttu-id="54027-136">Kontrollera att tooset ett undernät storlek tillräckligt stor tooaccommodate eventuell tillväxt i din ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-136">Make sure tooset a subnet size large enough tooaccommodate any future growth of your ASE.</span></span> <span data-ttu-id="54027-137">Vi rekommenderar en storlek på `/25`, som har 128-adresser och kan hantera en ASE maximala storlek.</span><span class="sxs-lookup"><span data-stu-id="54027-137">We recommend a size of `/25`, which has 128 addresses and can handle a maximum-sized ASE.</span></span> <span data-ttu-id="54027-138">hello minsta storlek som du kan välja är en `/28`.</span><span class="sxs-lookup"><span data-stu-id="54027-138">hello minimum size you can select is a `/28`.</span></span> <span data-ttu-id="54027-139">När infrastrukturen måste vara storleken skalade tooa Maximalt 11 instanser.</span><span class="sxs-lookup"><span data-stu-id="54027-139">After infrastructure needs, this size can be scaled tooa maximum of 11 instances.</span></span>

    * <span data-ttu-id="54027-140">Utöver hello standard högst 100 instanser i App Service-planer.</span><span class="sxs-lookup"><span data-stu-id="54027-140">Go beyond hello default maximum of 100 instances in your App Service plans.</span></span>

    * <span data-ttu-id="54027-141">Skala nära 100 men med snabbare frontend skalning.</span><span class="sxs-lookup"><span data-stu-id="54027-141">Scale near 100 but with more rapid front-end scaling.</span></span>

6. <span data-ttu-id="54027-142">Välj **virtuell nätverksplats** > **konfiguration av virtuellt nätverk**.</span><span class="sxs-lookup"><span data-stu-id="54027-142">Select **Virtual Network/Location** > **Virtual Network Configuration**.</span></span> <span data-ttu-id="54027-143">Ange hello **VIP typen** för**internt**.</span><span class="sxs-lookup"><span data-stu-id="54027-143">Set hello **VIP Type** too**Internal**.</span></span>

7. <span data-ttu-id="54027-144">Ange ett domännamn.</span><span class="sxs-lookup"><span data-stu-id="54027-144">Enter a domain name.</span></span> <span data-ttu-id="54027-145">Den här domänen är hello som används för appar som har skapats i den här ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-145">This domain is hello one used for apps created in this ASE.</span></span> <span data-ttu-id="54027-146">Det finns vissa begränsningar.</span><span class="sxs-lookup"><span data-stu-id="54027-146">There are some restrictions.</span></span> <span data-ttu-id="54027-147">Det går inte att vara:</span><span class="sxs-lookup"><span data-stu-id="54027-147">It can't be:</span></span>

    * <span data-ttu-id="54027-148">NET</span><span class="sxs-lookup"><span data-stu-id="54027-148">net</span></span>   

    * <span data-ttu-id="54027-149">azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="54027-149">azurewebsites.net</span></span>

    * <span data-ttu-id="54027-150">p.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="54027-150">p.azurewebsites.net</span></span>

    * <span data-ttu-id="54027-151">&lt;asename&gt;. p.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="54027-151">&lt;asename&gt;.p.azurewebsites.net</span></span>

   <span data-ttu-id="54027-152">hello domännamn för appar och hello domännamn som används av din ASE får inte överlappa varandra.</span><span class="sxs-lookup"><span data-stu-id="54027-152">hello custom domain name used for apps and hello domain name used by your ASE can't overlap.</span></span> <span data-ttu-id="54027-153">För en ILB ASE med hello domännamn _contoso.com_, du kan inte använda anpassade domännamn för dina appar som:</span><span class="sxs-lookup"><span data-stu-id="54027-153">For an ILB ASE with hello domain name _contoso.com_, you can't use custom domain names for your apps like:</span></span>

    * <span data-ttu-id="54027-154">www.contoso.com</span><span class="sxs-lookup"><span data-stu-id="54027-154">www.contoso.com</span></span>

    * <span data-ttu-id="54027-155">ABCD.def.contoso.com</span><span class="sxs-lookup"><span data-stu-id="54027-155">abcd.def.contoso.com</span></span>

    * <span data-ttu-id="54027-156">ABCD.contoso.com</span><span class="sxs-lookup"><span data-stu-id="54027-156">abcd.contoso.com</span></span>

   <span data-ttu-id="54027-157">Om du vet hello anpassade domännamn för dina appar, välja en domän för hello ILB ASE som inte står i konflikt med de anpassade domännamn.</span><span class="sxs-lookup"><span data-stu-id="54027-157">If you know hello custom domain names for your apps, choose a domain for hello ILB ASE that won’t have a conflict with those custom domain names.</span></span> <span data-ttu-id="54027-158">I det här exemplet kan du använda något som liknar *contoso internal.com* på din ASE hello domän eftersom som inte står i konflikt med anpassade domännamn som slutar i *. contoso.com*.</span><span class="sxs-lookup"><span data-stu-id="54027-158">In this example, you can use something like *contoso-internal.com* for hello domain of your ASE because that won't conflict with custom domain names that end in *.contoso.com*.</span></span>

8. <span data-ttu-id="54027-159">Välj **OK**, och välj sedan **skapa**.</span><span class="sxs-lookup"><span data-stu-id="54027-159">Select **OK**, and then select **Create**.</span></span>

    ![ASE-generering][1]

<span data-ttu-id="54027-161">På hello **virtuellt nätverk** bladet finns en **virtuella nätverkskonfigurationen** alternativet.</span><span class="sxs-lookup"><span data-stu-id="54027-161">On hello **Virtual Network** blade, there is a **Virtual Network Configuration** option.</span></span> <span data-ttu-id="54027-162">Du kan använda den tooselect en extern VIP eller ett internt VIP.</span><span class="sxs-lookup"><span data-stu-id="54027-162">You can use it tooselect an External VIP or an Internal VIP.</span></span> <span data-ttu-id="54027-163">hello standardvärdet är **externa**.</span><span class="sxs-lookup"><span data-stu-id="54027-163">hello default is **External**.</span></span> <span data-ttu-id="54027-164">Om du väljer **externa**, din ASE använder en internet-tillgängliga VIP.</span><span class="sxs-lookup"><span data-stu-id="54027-164">If you select **External**, your ASE uses an internet-accessible VIP.</span></span> <span data-ttu-id="54027-165">Om du väljer **internt**, din ASE har konfigurerats med en ILB på en IP-adress inom ditt VNet.</span><span class="sxs-lookup"><span data-stu-id="54027-165">If you select **Internal**, your ASE is configured with an ILB on an IP address within your VNet.</span></span>

<span data-ttu-id="54027-166">När du har valt **internt**, hello möjlighet tooadd flera IP-adresser tooyour ASE tas bort.</span><span class="sxs-lookup"><span data-stu-id="54027-166">After you select **Internal**, hello ability tooadd more IP addresses tooyour ASE is removed.</span></span> <span data-ttu-id="54027-167">Du måste i stället tooprovide hello domän hello ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-167">Instead, you need tooprovide hello domain of hello ASE.</span></span> <span data-ttu-id="54027-168">I en ASE med en extern VIP används hello namnet på hello ASE i hello domän för appar som har skapats i den ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-168">In an ASE with an External VIP, hello name of hello ASE is used in hello domain for apps created in that ASE.</span></span>

<span data-ttu-id="54027-169">Om du ställer in **VIP typen** för**internt**, namnet på din ASE används inte i hello domän för hello ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-169">If you set **VIP Type** too**Internal**, your ASE name is not used in hello domain for hello ASE.</span></span> <span data-ttu-id="54027-170">Du ange uttryckligen hello domän.</span><span class="sxs-lookup"><span data-stu-id="54027-170">You specify hello domain explicitly.</span></span> <span data-ttu-id="54027-171">Om din domän är *contoso.corp.net* och du skapar en app i den ASE med namnet *timereporting*, hello URL för appen är timereporting.contoso.corp.net.</span><span class="sxs-lookup"><span data-stu-id="54027-171">If your domain is *contoso.corp.net* and you create an app in that ASE named *timereporting*, hello URL for that app is timereporting.contoso.corp.net.</span></span>


## <a name="create-an-app-in-an-ilb-ase"></a><span data-ttu-id="54027-172">Skapa en app i en ILB ASE</span><span class="sxs-lookup"><span data-stu-id="54027-172">Create an app in an ILB ASE</span></span> ##

<span data-ttu-id="54027-173">Du skapar en app i en ASE ILB i hello samma sätt som du skapar en app i en ASE normalt.</span><span class="sxs-lookup"><span data-stu-id="54027-173">You create an app in an ILB ASE in hello same way that you create an app in an ASE normally.</span></span>

1. <span data-ttu-id="54027-174">Välj i hello Azure-portalen, **ny** > **webb + mobilt** > **Web** eller **Mobile** eller  **API-App**.</span><span class="sxs-lookup"><span data-stu-id="54027-174">In hello Azure portal, select **New** > **Web + Mobile** > **Web** or **Mobile** or **API App**.</span></span>

2. <span data-ttu-id="54027-175">Ange namn på hello hello-appen.</span><span class="sxs-lookup"><span data-stu-id="54027-175">Enter hello name of hello app.</span></span>

3. <span data-ttu-id="54027-176">Välj hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="54027-176">Select hello subscription.</span></span>

4. <span data-ttu-id="54027-177">Välj eller skapa en Resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="54027-177">Select or create a resource group.</span></span>

5. <span data-ttu-id="54027-178">Välj eller skapa en apptjänstplan.</span><span class="sxs-lookup"><span data-stu-id="54027-178">Select or create an App Service plan.</span></span> <span data-ttu-id="54027-179">Om du vill toocreate en ny programtjänstplan, Välj din ASE som hello plats.</span><span class="sxs-lookup"><span data-stu-id="54027-179">If you want toocreate a new App Service plan, select your ASE as hello location.</span></span> <span data-ttu-id="54027-180">Välj hello arbetspool där du vill att din App Service-plan toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="54027-180">Select hello worker pool where you want your App Service plan toobe created.</span></span> <span data-ttu-id="54027-181">När du skapar hello App Service-plan, Välj din ASE som hello plats och hello arbetspool.</span><span class="sxs-lookup"><span data-stu-id="54027-181">When you create hello App Service plan, select your ASE as hello location and hello worker pool.</span></span> <span data-ttu-id="54027-182">När du anger hello namnet på hello app har hello domän under appnamnet ersatts av hello domän för ditt ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-182">When you specify hello name of hello app, hello domain under your app name is replaced by hello domain for your ASE.</span></span>

6. <span data-ttu-id="54027-183">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="54027-183">Select **Create**.</span></span> <span data-ttu-id="54027-184">Om du vill hello app tooappear på instrumentpanelen, väljer den **PIN-kod toodashboard** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="54027-184">If you want hello app tooappear on your dashboard, select the **Pin toodashboard** check box.</span></span>

    ![Skapa en App Service-plan][2]

    <span data-ttu-id="54027-186">Under **appnamn**, hello domännamn är uppdaterade tooreflect hello domän för ditt ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-186">Under **App name**, hello domain name is updated tooreflect hello domain of your ASE.</span></span>

## <a name="post-ilb-ase-creation-validation"></a><span data-ttu-id="54027-187">Post ILB ASE skapa validering</span><span class="sxs-lookup"><span data-stu-id="54027-187">Post-ILB ASE creation validation</span></span> ##

<span data-ttu-id="54027-188">En ILB ASE är något annorlunda än hello icke - ILB ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-188">An ILB ASE is slightly different than hello non-ILB ASE.</span></span> <span data-ttu-id="54027-189">Som redan anges måste toomanage egna DNS.</span><span class="sxs-lookup"><span data-stu-id="54027-189">As already noted, you need toomanage your own DNS.</span></span> <span data-ttu-id="54027-190">Du har också tooprovide ditt eget certifikat för HTTPS-anslutningar.</span><span class="sxs-lookup"><span data-stu-id="54027-190">You also have tooprovide your own certificate for HTTPS connections.</span></span>

<span data-ttu-id="54027-191">När du har skapat din ASE visar hello domännamn hello domän du angav.</span><span class="sxs-lookup"><span data-stu-id="54027-191">After you create your ASE, hello domain name shows hello domain you specified.</span></span> <span data-ttu-id="54027-192">Ett nytt objekt visas i den **inställningen** menyn kallas **ILB certifikat**.</span><span class="sxs-lookup"><span data-stu-id="54027-192">A new item appears in the **Setting** menu called **ILB Certificate**.</span></span> <span data-ttu-id="54027-193">Hej ASE skapas med ett certifikat som inte anger hello ILB ASE domän.</span><span class="sxs-lookup"><span data-stu-id="54027-193">hello ASE is created with a certificate that doesn't specify hello ILB ASE domain.</span></span> <span data-ttu-id="54027-194">Om du använder hello ASE med certifikatet om webbläsaren det är ogiltigt.</span><span class="sxs-lookup"><span data-stu-id="54027-194">If you use hello ASE with that certificate, your browser tells you that it's invalid.</span></span> <span data-ttu-id="54027-195">Det här certifikatet gör det enklare tootest HTTPS, men du måste tooupload egna certifikat som är bundet tooyour ILB ASE domän.</span><span class="sxs-lookup"><span data-stu-id="54027-195">This certificate makes it easier tootest HTTPS, but you need tooupload your own certificate that's tied tooyour ILB ASE domain.</span></span> <span data-ttu-id="54027-196">Det här steget är nödvändigt oavsett om certifikatet är självsignerat eller förvärvas från en certifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="54027-196">This step is necessary regardless of whether your certificate is self-signed or acquired from a certificate authority.</span></span>

![ILB ASE domännamn][3]

<span data-ttu-id="54027-198">ILB-ASE behöver ett giltigt SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="54027-198">Your ILB ASE needs a valid SSL certificate.</span></span> <span data-ttu-id="54027-199">Använda interna certifikatutfärdare, köpa ett certifikat från en extern utfärdare eller använda ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="54027-199">Use internal certificate authorities, purchase a certificate from an external issuer, or use a self-signed certificate.</span></span> <span data-ttu-id="54027-200">Oavsett hello källa för hello SSL-certifikat måste hello följande certifikat-attribut toobe konfigurerats korrekt:</span><span class="sxs-lookup"><span data-stu-id="54027-200">Regardless of hello source of hello SSL certificate, hello following certificate attributes need toobe configured properly:</span></span>

* <span data-ttu-id="54027-201">**Ämne**: det här attributet måste anges too*.your-rot-domain-här.</span><span class="sxs-lookup"><span data-stu-id="54027-201">**Subject**: This attribute must be set too*.your-root-domain-here.</span></span>
* <span data-ttu-id="54027-202">**Alternativt ämnesnamn**: det här attributet måste innehålla både **.your-rot-domain-här* och **.scm.your-rot-domain-här*.</span><span class="sxs-lookup"><span data-stu-id="54027-202">**Subject Alternative Name**: This attribute must include both **.your-root-domain-here* and **.scm.your-root-domain-here*.</span></span> <span data-ttu-id="54027-203">SSL-anslutningar toohello SCM/Kudu-plats som är associerade med varje app använder en adress i formatet hello *your-app-name.scm.your-root-domain-here*.</span><span class="sxs-lookup"><span data-stu-id="54027-203">SSL connections toohello SCM/Kudu site associated with each app use an address of hello form *your-app-name.scm.your-root-domain-here*.</span></span>

<span data-ttu-id="54027-204">Konvertera/spara hello SSL-certifikatet som en .pfx-fil.</span><span class="sxs-lookup"><span data-stu-id="54027-204">Convert/save hello SSL certificate as a .pfx file.</span></span> <span data-ttu-id="54027-205">hello .pfx-filen måste innehålla alla mellanliggande och root certifikat.</span><span class="sxs-lookup"><span data-stu-id="54027-205">hello .pfx file must include all intermediate and root certificates.</span></span> <span data-ttu-id="54027-206">Skydda den med ett lösenord.</span><span class="sxs-lookup"><span data-stu-id="54027-206">Secure it with a password.</span></span>

<span data-ttu-id="54027-207">Om du vill toocreate ett självsignerat certifikat, kan du använda hello PowerShell-kommandon här.</span><span class="sxs-lookup"><span data-stu-id="54027-207">If you want toocreate a self-signed certificate, you can use hello PowerShell commands here.</span></span> <span data-ttu-id="54027-208">Vara säker på att toouse ILB ASE domännamnet i stället för *internal.contoso.com*:</span><span class="sxs-lookup"><span data-stu-id="54027-208">Be sure toouse your ILB ASE domain name instead of *internal.contoso.com*:</span></span> 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

<span data-ttu-id="54027-209">hello-certifikat som genereras av dessa PowerShell-kommandon flaggas av webbläsare eftersom hello certifikatet inte har skapats av en certifikatutfärdare i din webbläsare förtroendekedja för.</span><span class="sxs-lookup"><span data-stu-id="54027-209">hello certificate that these PowerShell commands generate is flagged by browsers because hello certificate wasn't created by a certificate authority that's in your browser's chain of trust.</span></span> <span data-ttu-id="54027-210">tooget ett certifikat som har förtroende för din webbläsare, skaffa den från en kommersiell certifikatutfärdare i din webbläsare förtroendekedja för.</span><span class="sxs-lookup"><span data-stu-id="54027-210">tooget a certificate that your browser trusts, procure it from a commercial certificate authority in your browser's chain of trust.</span></span> 

![Ställ in ILB certificate][4]

<span data-ttu-id="54027-212">tooupload egna certifikat och testa åtkomst:</span><span class="sxs-lookup"><span data-stu-id="54027-212">tooupload your own certificates and test access:</span></span>

1. <span data-ttu-id="54027-213">När du har skapat hello ASE gå toohello ASE UI.</span><span class="sxs-lookup"><span data-stu-id="54027-213">After hello ASE is created, go toohello ASE UI.</span></span> <span data-ttu-id="54027-214">Välj **ASE** > **inställningar** > **ILB certifikat**.</span><span class="sxs-lookup"><span data-stu-id="54027-214">Select **ASE** > **Settings** > **ILB Certificate**.</span></span>

2. <span data-ttu-id="54027-215">tooset hello ILB certifikat, Välj hello certifikatets PFX-fil och ange hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="54027-215">tooset hello ILB certificate, select hello certificate .pfx file and enter hello password.</span></span> <span data-ttu-id="54027-216">Det här steget tar några tid tooprocess.</span><span class="sxs-lookup"><span data-stu-id="54027-216">This step takes some time tooprocess.</span></span> <span data-ttu-id="54027-217">Det visas ett meddelande om att en överföringen pågår.</span><span class="sxs-lookup"><span data-stu-id="54027-217">A message appears stating that an upload operation is in progress.</span></span>

3. <span data-ttu-id="54027-218">Hämta hello ILB-adressen för din ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-218">Get hello ILB address for your ASE.</span></span> <span data-ttu-id="54027-219">Välj **ASE** > **egenskaper** > **virtuella IP-adressen**.</span><span class="sxs-lookup"><span data-stu-id="54027-219">Select **ASE** > **Properties** > **Virtual IP Address**.</span></span>

4. <span data-ttu-id="54027-220">Skapa en webbapp i din ASE efter hello ASE skapas.</span><span class="sxs-lookup"><span data-stu-id="54027-220">Create a web app in your ASE after hello ASE is created.</span></span>

5. <span data-ttu-id="54027-221">Skapa en virtuell dator om du inte har något i det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="54027-221">Create a VM if you don't have one in that VNet.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="54027-222">Försök inte toocreate den här virtuella datorn i hello samma undernät som hello ASE eftersom den kommer att misslyckas eller orsaka problem.</span><span class="sxs-lookup"><span data-stu-id="54027-222">Don't try toocreate this VM in hello same subnet as hello ASE because it will fail or cause problems.</span></span>
    >
    >

6. <span data-ttu-id="54027-223">Ange hello DNS för din ASE-domän.</span><span class="sxs-lookup"><span data-stu-id="54027-223">Set hello DNS for your ASE domain.</span></span> <span data-ttu-id="54027-224">Du kan använda jokertecken med din domän i din DNS.</span><span class="sxs-lookup"><span data-stu-id="54027-224">You can use a wildcard with your domain in your DNS.</span></span> <span data-ttu-id="54027-225">toodo några enkla tester, redigera hello hosts-filen på din Virtuella tooset hello web app name toohello VIP IP-adress:</span><span class="sxs-lookup"><span data-stu-id="54027-225">toodo some simple tests, edit hello hosts file on your VM tooset hello web app name toohello VIP IP address:</span></span>

    <span data-ttu-id="54027-226">a.</span><span class="sxs-lookup"><span data-stu-id="54027-226">a.</span></span> <span data-ttu-id="54027-227">Om din ASE har hello domännamn _. ilbase.com_ och du skapar hello webbprogram med namnet _mytestapp_, riktar på _mytestapp.ilbase.com_. Sedan ställer du in _mytestapp.ilbase.com_ tooresolve toohello ILB-adress.</span><span class="sxs-lookup"><span data-stu-id="54027-227">If your ASE has hello domain name _.ilbase.com_ and you create hello web app named _mytestapp_, it's addressed at _mytestapp.ilbase.com_. You then set _mytestapp.ilbase.com_ tooresolve toohello ILB address.</span></span> <span data-ttu-id="54027-228">(För Windows hello hosts-filen finns på _C:\Windows\System32\drivers\etc\_.)</span><span class="sxs-lookup"><span data-stu-id="54027-228">(On Windows, hello hosts file is at _C:\Windows\System32\drivers\etc\_.)</span></span>

    <span data-ttu-id="54027-229">b.</span><span class="sxs-lookup"><span data-stu-id="54027-229">b.</span></span> <span data-ttu-id="54027-230">tootest web distribution publicering eller åtkomst toohello, avancerad konsol, skapa en post för _mytestapp.scm.ilbase.com_.</span><span class="sxs-lookup"><span data-stu-id="54027-230">tootest web deployment publishing or access toohello advanced console, create a record for _mytestapp.scm.ilbase.com_.</span></span>

7. <span data-ttu-id="54027-231">Använda en webbläsare på den virtuella datorn och gå till http://mytestapp.ilbase.com. (Eller toowhatever din webbprogrammets namn är med domänen.)</span><span class="sxs-lookup"><span data-stu-id="54027-231">Use a browser on that VM and go to http://mytestapp.ilbase.com. (Or go toowhatever your web app name is with your domain.)</span></span>

8. <span data-ttu-id="54027-232">Använda en webbläsare på den virtuella datorn och gå till https://mytestapp.ilbase.com. Om du använder ett självsignerat certifikat kan acceptera hello bristande säkerhet.</span><span class="sxs-lookup"><span data-stu-id="54027-232">Use a browser on that VM and go to https://mytestapp.ilbase.com. If you use a self-signed certificate, accept hello lack of security.</span></span>

    <span data-ttu-id="54027-233">hello IP-adressen för din ILB anges under **IP-adresser**.</span><span class="sxs-lookup"><span data-stu-id="54027-233">hello IP address for your ILB is listed under **IP addresses**.</span></span> <span data-ttu-id="54027-234">Den här listan har också hello IP-adresser som används av hello externa VIP och för av inkommande hanteringstrafik.</span><span class="sxs-lookup"><span data-stu-id="54027-234">This list also has hello IP addresses used by hello external VIP and for inbound management traffic.</span></span>

    ![ILB IP-adress][5]

### <a name="web-jobs-functions-and-hello-ilb-ase"></a><span data-ttu-id="54027-236">Web jobb, funktioner och hello ILB ASE</span><span class="sxs-lookup"><span data-stu-id="54027-236">Web jobs, Functions and hello ILB ASE</span></span>

<span data-ttu-id="54027-237">Web jobb och funktioner stöds i en ASE ILB men hello portal toowork med dem, måste du ha åtkomst toohello SCM nätverksplats.</span><span class="sxs-lookup"><span data-stu-id="54027-237">Both Functions and web jobs are supported on an ILB ASE but for hello portal toowork with them, you must have network access toohello SCM site.</span></span>  <span data-ttu-id="54027-238">Detta innebär att din webbläsare måste antingen vara på en värd som är antingen i eller ansluten toohello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="54027-238">This means your browser must either be on a host that is either in or connected toohello virtual network.</span></span>  

<span data-ttu-id="54027-239">När du använder Azure Functions på en ILB ASE kan få du ett felmeddelande som säger ”vi inte kan tooretrieve dina funktioner just nu.</span><span class="sxs-lookup"><span data-stu-id="54027-239">When you use Azure Functions on an ILB ASE, you might get an error message that says "We are not able tooretrieve your functions right now.</span></span> <span data-ttu-id="54027-240">Försök igen senare ”.</span><span class="sxs-lookup"><span data-stu-id="54027-240">Please try again later."</span></span> <span data-ttu-id="54027-241">Det här felet uppstår eftersom hello funktioner UI utnyttjar hello SCM plats över HTTPS och hello rotcertifikat är inte i hello webbläsare förtroendekedja för.</span><span class="sxs-lookup"><span data-stu-id="54027-241">This error occurs because hello Functions UI leverages hello SCM site over HTTPS and hello root certificate is not in hello browser chain of trust.</span></span> <span data-ttu-id="54027-242">Web jobb har liknande problem.</span><span class="sxs-lookup"><span data-stu-id="54027-242">Web jobs has a similar problem.</span></span> <span data-ttu-id="54027-243">tooavoid det här problemet kan du göra något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="54027-243">tooavoid this problem you can do one of hello following:</span></span>

- <span data-ttu-id="54027-244">Lägg till hello certifikatarkivet tooyour betrott certifikat.</span><span class="sxs-lookup"><span data-stu-id="54027-244">Add hello certificate tooyour trusted certificate store.</span></span> <span data-ttu-id="54027-245">Då hämtas kant och Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="54027-245">This unblocks Edge and Internet Explorer.</span></span>
- <span data-ttu-id="54027-246">Använd Chrome och webbplatsen toohello SCM först, godkänna hello obetrodda certifikat och sedan toohello portal.</span><span class="sxs-lookup"><span data-stu-id="54027-246">Use Chrome and go toohello SCM site first, accept hello untrusted certificate and then go toohello portal.</span></span>
- <span data-ttu-id="54027-247">Använda ett kommersiella certifikat som finns i din webbläsare förtroendekedja för.</span><span class="sxs-lookup"><span data-stu-id="54027-247">Use a commercial certificate that is in your browser chain of trust.</span></span>  <span data-ttu-id="54027-248">Är det bästa alternativet för hello.</span><span class="sxs-lookup"><span data-stu-id="54027-248">This is hello best option.</span></span>  

## <a name="dns-configuration"></a><span data-ttu-id="54027-249">DNS-konfiguration</span><span class="sxs-lookup"><span data-stu-id="54027-249">DNS configuration</span></span> ##

<span data-ttu-id="54027-250">När du använder en extern VIP hanteras hello DNS av Azure.</span><span class="sxs-lookup"><span data-stu-id="54027-250">When you use an External VIP, hello DNS is managed by Azure.</span></span> <span data-ttu-id="54027-251">Alla appar som skapats i en ASE läggs automatiskt tooAzure DNS, vilket är en offentlig DNS.</span><span class="sxs-lookup"><span data-stu-id="54027-251">Any app created in your ASE is automatically added tooAzure DNS, which is a public DNS.</span></span> <span data-ttu-id="54027-252">I en ASE ILB, måste du hantera din egen DNS.</span><span class="sxs-lookup"><span data-stu-id="54027-252">In an ILB ASE, you must manage your own DNS.</span></span> <span data-ttu-id="54027-253">För en viss domän som _contoso.net_, behöver du toocreate DNS A-poster i din DNS punkt tooyour ILB adressen för:</span><span class="sxs-lookup"><span data-stu-id="54027-253">For a given domain such as _contoso.net_, you need toocreate DNS A records in your DNS that point tooyour ILB address for:</span></span>

- <span data-ttu-id="54027-254">*. contoso.net</span><span class="sxs-lookup"><span data-stu-id="54027-254">*.contoso.net</span></span>
- <span data-ttu-id="54027-255">*. scm.contoso.net</span><span class="sxs-lookup"><span data-stu-id="54027-255">*.scm.contoso.net</span></span>

<span data-ttu-id="54027-256">Om din ILB ASE domän används för flera saker utanför den här ASE behöva toomanage DNS på grundval av per programnamn.</span><span class="sxs-lookup"><span data-stu-id="54027-256">If your ILB ASE domain is used for multiple things outside this ASE, you might need toomanage DNS on a per-app-name basis.</span></span> <span data-ttu-id="54027-257">Den här metoden en utmaning eftersom du behöver tooadd namnen på alla nya app i din DNS när du skapar den.</span><span class="sxs-lookup"><span data-stu-id="54027-257">This method is challenging because you need tooadd each new app name into your DNS when you create it.</span></span> <span data-ttu-id="54027-258">Därför rekommenderar vi att du använder en särskild domän.</span><span class="sxs-lookup"><span data-stu-id="54027-258">For this reason, we recommend that you use a dedicated domain.</span></span>

## <a name="publish-with-an-ilb-ase"></a><span data-ttu-id="54027-259">Publicera med en ILB ASE</span><span class="sxs-lookup"><span data-stu-id="54027-259">Publish with an ILB ASE</span></span> ##

<span data-ttu-id="54027-260">Det finns två slutpunkter för varje app som har skapats.</span><span class="sxs-lookup"><span data-stu-id="54027-260">For every app that's created, there are two endpoints.</span></span> <span data-ttu-id="54027-261">Du har på en ILB ASE  *&lt;appnamn >.&lt; ILB ASE domän >* och  *&lt;appnamn > .scm.&lt; ILB ASE domän >*.</span><span class="sxs-lookup"><span data-stu-id="54027-261">In an ILB ASE, you have *&lt;app name>.&lt;ILB ASE Domain>* and *&lt;app name>.scm.&lt;ILB ASE Domain>*.</span></span> 

<span data-ttu-id="54027-262">hello SCM platsnamn tar toohello Kudu-konsolen som kallas hello **avancerade portal**, inom hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="54027-262">hello SCM site name takes you toohello Kudu console, called hello **Advanced portal**, within hello Azure portal.</span></span> <span data-ttu-id="54027-263">Hej Kudu-konsolen kan du visa miljövariabler, utforska hello disk, använda en konsol och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="54027-263">hello Kudu console lets you view environment variables, explore hello disk, use a console, and much more.</span></span> <span data-ttu-id="54027-264">Mer information finns i [Kudu-konsol för Azure App Service][Kudu].</span><span class="sxs-lookup"><span data-stu-id="54027-264">For more information, see [Kudu console for Azure App Service][Kudu].</span></span> 

<span data-ttu-id="54027-265">Hej multitenant Apptjänst och en extern ASE är det enkel inloggning mellan hello Azure-portalen och hello Kudu-konsolen.</span><span class="sxs-lookup"><span data-stu-id="54027-265">In hello multitenant App Service and in an External ASE, there's single sign-on between hello Azure portal and hello Kudu console.</span></span> <span data-ttu-id="54027-266">För hello ILB ASE behöver du dock toouse din publishing autentiseringsuppgifter toosign in hello Kudu-konsolen.</span><span class="sxs-lookup"><span data-stu-id="54027-266">For hello ILB ASE, however, you need toouse your publishing credentials toosign into hello Kudu console.</span></span>

<span data-ttu-id="54027-267">Internet-baserade CI system, t.ex GitHub och Visual Studio Team Services fungerar inte med en ILB ASE eftersom hello publishing slutpunkten inte komma åt internet.</span><span class="sxs-lookup"><span data-stu-id="54027-267">Internet-based CI systems, such as GitHub and Visual Studio Team Services, don't work with an ILB ASE because hello publishing endpoint isn't internet accessible.</span></span> <span data-ttu-id="54027-268">Du måste i stället toouse en CI-system som använder en pull-modell, till exempel Dropbox.</span><span class="sxs-lookup"><span data-stu-id="54027-268">Instead, you need toouse a CI system that uses a pull model, such as Dropbox.</span></span>

<span data-ttu-id="54027-269">hello publishing slutpunkter för appar i en ASE ILB använda hello domän som hello ILB ASE har skapats med.</span><span class="sxs-lookup"><span data-stu-id="54027-269">hello publishing endpoints for apps in an ILB ASE use hello domain that hello ILB ASE was created with.</span></span> <span data-ttu-id="54027-270">Den här domänen visas hello app publiceringsprofilen och i hello app portalbladet (**översikt** > **Essentials** och även **egenskaper**).</span><span class="sxs-lookup"><span data-stu-id="54027-270">This domain appears in hello app's publishing profile and in hello app's portal blade (**Overview** > **Essentials** and also **Properties**).</span></span> <span data-ttu-id="54027-271">Om du har en ILB ASE med hello underdomän *contoso.net* och en app med namnet *MinTest*, använda *mytest.contoso.net* för FTP och *mytest.scm.contoso.net*  för web-distribution.</span><span class="sxs-lookup"><span data-stu-id="54027-271">If you have an ILB ASE with hello subdomain *contoso.net* and an app named *mytest*, use *mytest.contoso.net* for FTP and *mytest.scm.contoso.net* for web deployment.</span></span>

## <a name="couple-an-ilb-ase-with-a-waf-device"></a><span data-ttu-id="54027-272">Koppla en ILB ASE med en Brandvägg-enhet</span><span class="sxs-lookup"><span data-stu-id="54027-272">Couple an ILB ASE with a WAF device</span></span> ##

<span data-ttu-id="54027-273">Azure Apptjänst innehåller många säkerhetsåtgärder som skyddar hello system.</span><span class="sxs-lookup"><span data-stu-id="54027-273">Azure App Service provides many security measures that protect hello system.</span></span> <span data-ttu-id="54027-274">De kan också hjälpa toodetermine om en app har över.</span><span class="sxs-lookup"><span data-stu-id="54027-274">They also help toodetermine whether an app was hacked.</span></span> <span data-ttu-id="54027-275">hello bästa skydd för ett webbprogram är toocouple en Värdplattformen, till exempel Azure App Service med en brandvägg för webbaserade program (Brandvägg).</span><span class="sxs-lookup"><span data-stu-id="54027-275">hello best protection for a web application is toocouple a hosting platform, such as Azure App Service, with a web application firewall (WAF).</span></span> <span data-ttu-id="54027-276">Eftersom hello ILB ASE har ett isolerat nätverk programslutpunkten, är den lämplig för sådan användning.</span><span class="sxs-lookup"><span data-stu-id="54027-276">Because hello ILB ASE has a network-isolated application endpoint, it's appropriate for such a use.</span></span>

<span data-ttu-id="54027-277">Mer om hur tooconfigure din ILB ASE med en Brandvägg enhet, se toolearn [konfigurera brandväggar för webbaserade program med App Service-miljö][ASEWAF].</span><span class="sxs-lookup"><span data-stu-id="54027-277">toolearn more about how tooconfigure your ILB ASE with a WAF device, see [Configure a web application firewall with your App Service environment][ASEWAF].</span></span> <span data-ttu-id="54027-278">Den här artikeln visar hur toouse Barracuda virtuell utrustning med din ASE.</span><span class="sxs-lookup"><span data-stu-id="54027-278">This article shows how toouse a Barracuda virtual appliance with your ASE.</span></span> <span data-ttu-id="54027-279">Ett annat alternativ är toouse Azure Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="54027-279">Another option is toouse Azure Application Gateway.</span></span> <span data-ttu-id="54027-280">Programgateway använder hello OWASP core regler toosecure alla program som placeras bakom det.</span><span class="sxs-lookup"><span data-stu-id="54027-280">Application Gateway uses hello OWASP core rules toosecure any applications placed behind it.</span></span> <span data-ttu-id="54027-281">Mer information om Application Gateway finns [introduktion toohello Azure web application firewall][AppGW].</span><span class="sxs-lookup"><span data-stu-id="54027-281">For more information about Application Gateway, see [Introduction toohello Azure web application firewall][AppGW].</span></span>

## <a name="get-started"></a><span data-ttu-id="54027-282">Kom igång</span><span class="sxs-lookup"><span data-stu-id="54027-282">Get started</span></span> ##

<span data-ttu-id="54027-283">Alla artiklar och hur tooinstructions för ASEs är tillgängliga i den [viktigt för apptjänstmiljöer][ASEReadme].</span><span class="sxs-lookup"><span data-stu-id="54027-283">All articles and how-tooinstructions for ASEs are available in the [README for App Service environments][ASEReadme].</span></span>

* <span data-ttu-id="54027-284">tooget igång med ASEs, se [introduktion tooApp miljöer][Intro].</span><span class="sxs-lookup"><span data-stu-id="54027-284">tooget started with ASEs, see [Introduction tooApp Service environments][Intro].</span></span>
* <span data-ttu-id="54027-285">Mer information om hello Azure Apptjänst-plattformen finns [Azure App Service](http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/).</span><span class="sxs-lookup"><span data-stu-id="54027-285">For more information about hello Azure App Service platform, see [Azure App Service](http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/).</span></span>
 
<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[3]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate.png
[4]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate2.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

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
