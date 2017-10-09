---
title: aaaProtect din API med Azure API Management | Microsoft Docs
description: "Lär dig hur tooprotect din API med kvoter och begränsning (hastighetsbegränsning) principer."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 450dc368-d005-401d-ae64-3e1a2229b12f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 3113fd277d434da0c051b8b90fd629a102bf4867
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a><span data-ttu-id="638c8-103">Skydda ditt API med frekvensbegränsningar med hjälp av Azure API Management</span><span class="sxs-lookup"><span data-stu-id="638c8-103">Protect your API with rate limits using Azure API Management</span></span>
<span data-ttu-id="638c8-104">Den här guiden visar hur lätt det är tooadd skydd för din serverdel API genom att konfigurera hastighet gränsen och kvot principer med Azure API Management.</span><span class="sxs-lookup"><span data-stu-id="638c8-104">This guide shows you how easy it is tooadd protection for your backend API by configuring rate limit and quota policies with Azure API Management.</span></span>

<span data-ttu-id="638c8-105">I den här självstudiekursen skapar du en ”kostnadsfri utvärderingsversion” API-produkt som gör att utvecklare toomake too10 anrop per minut och in tooa högst 200 anrop per vecka tooyour API: et med hello [gränsen anropet frekvensen per prenumeration](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) och [ Ange kvot per prenumeration](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) principer.</span><span class="sxs-lookup"><span data-stu-id="638c8-105">In this tutorial, you will create a "Free Trial" API product that allows developers toomake up too10 calls per minute and up tooa maximum of 200 calls per week tooyour API using hello [Limit call rate per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) policies.</span></span> <span data-ttu-id="638c8-106">Du kommer sedan publicerar hello API och testa hello hastighet gränsen principen.</span><span class="sxs-lookup"><span data-stu-id="638c8-106">You will then publish hello API and test hello rate limit policy.</span></span>

<span data-ttu-id="638c8-107">För mer avancerade scenarier med hjälp av hello begränsning [hastighet gränsen av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) och [kvoten av nyckeln](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) principer, se [avancerade begäran begränsning med Azure API Management](api-management-sample-flexible-throttling.md).</span><span class="sxs-lookup"><span data-stu-id="638c8-107">For more advanced throttling scenarios using hello [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) and [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) policies, see [Advanced request throttling with Azure API Management](api-management-sample-flexible-throttling.md).</span></span>

## <span data-ttu-id="638c8-108"><a name="create-product"></a>toocreate en produkt</span><span class="sxs-lookup"><span data-stu-id="638c8-108"><a name="create-product"> </a>toocreate a product</span></span>
<span data-ttu-id="638c8-109">I det här steget ska du skapa en produkt för en kostnadsfri utvärdering som inte kräver prenumerationsgodkännande.</span><span class="sxs-lookup"><span data-stu-id="638c8-109">In this step, you will create a Free Trial product that does not require subscription approval.</span></span>

> [!NOTE]
> <span data-ttu-id="638c8-110">Om du redan har en produkt som har konfigurerats och vill toouse den för den här självstudiekursen, kan du hoppa vidare för[konfigurera anropa hastighet principer för begränsning av och kvot] [ Configure call rate limit and quota policies] och följ hello kursen därifrån med hjälp av produkten i stället för hello kostnadsfri utvärderingsversion av produkten.</span><span class="sxs-lookup"><span data-stu-id="638c8-110">If you already have a product configured and want toouse it for this tutorial, you can jump ahead too[Configure call rate limit and quota policies][Configure call rate limit and quota policies] and follow hello tutorial from there using your product in place of hello Free Trial product.</span></span>
> 
> 

<span data-ttu-id="638c8-111">tooget har startats klickar du på **Publisher portal** i hello Azure-portalen för API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="638c8-111">tooget started, click **Publisher portal** in hello Azure Portal for your API Management service.</span></span>

![Utgivarportalen][api-management-management-console]

> <span data-ttu-id="638c8-113">Om du inte har skapat en instans för API Management-tjänsten finns [skapa en instans för API Management-tjänsten] [ Create an API Management service instance] i hello [hantera din första API i Azure API Management] [ Manage your first API in Azure API Management] kursen.</span><span class="sxs-lookup"><span data-stu-id="638c8-113">If you have not yet created an API Management service instance, see [Create an API Management service instance][Create an API Management service instance] in hello [Manage your first API in Azure API Management][Manage your first API in Azure API Management] tutorial.</span></span>
> 
> 

<span data-ttu-id="638c8-114">Klicka på **produkter** i hello **API Management** menyn på hello vänstra toodisplay hello **produkter** sidan.</span><span class="sxs-lookup"><span data-stu-id="638c8-114">Click **Products** in hello **API Management** menu on hello left toodisplay hello **Products** page.</span></span>

![Lägg till produkt][api-management-add-product]

<span data-ttu-id="638c8-116">Klicka på **Lägg till produkten** toodisplay hello **Lägg till ny produkt** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="638c8-116">Click **Add product** toodisplay hello **Add new product** dialog box.</span></span>

![Lägg till ny produkt][api-management-new-product-window]

<span data-ttu-id="638c8-118">I hello **rubrik** skriver **kostnadsfri utvärderingsversion**.</span><span class="sxs-lookup"><span data-stu-id="638c8-118">In hello **Title** box, type **Free Trial**.</span></span>

<span data-ttu-id="638c8-119">I hello **beskrivning** rutan, typen hello följande text: **prenumeranter kommer att kunna toorun 10 anrop per minut in tooa högst 200 anrop i veckan då åtkomst nekas.**</span><span class="sxs-lookup"><span data-stu-id="638c8-119">In hello **Description** box, type hello following text: **Subscribers will be able toorun 10 calls/minute up tooa maximum of 200 calls/week after which access is denied.**</span></span>

<span data-ttu-id="638c8-120">Produkter i API Management kan skyddas eller vara öppna.</span><span class="sxs-lookup"><span data-stu-id="638c8-120">Products in API Management can be protected or open.</span></span> <span data-ttu-id="638c8-121">Skyddade produkter måste vara prenumererade toobefore som de kan användas.</span><span class="sxs-lookup"><span data-stu-id="638c8-121">Protected products must be subscribed toobefore they can be used.</span></span> <span data-ttu-id="638c8-122">Öppna produkter kan användas utan en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="638c8-122">Open products can be used without a subscription.</span></span> <span data-ttu-id="638c8-123">Se till att **kräver prenumeration** är valda toocreate en skyddad produkt som kräver en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="638c8-123">Ensure that **Require subscription** is selected toocreate a protected product that requires a subscription.</span></span> <span data-ttu-id="638c8-124">Det här är standardinställningen för hello.</span><span class="sxs-lookup"><span data-stu-id="638c8-124">This is hello default setting.</span></span>

<span data-ttu-id="638c8-125">Om du vill att en administratör tooreview och godkänna eller avvisa prenumeration försöker toothis produkten, Välj **kräver godkännande för prenumerationen**.</span><span class="sxs-lookup"><span data-stu-id="638c8-125">If you want an administrator tooreview and accept or reject subscription attempts toothis product, select **Require subscription approval**.</span></span> <span data-ttu-id="638c8-126">Om hello inte är markerad, att prenumeration försök automatiskt godkänd.</span><span class="sxs-lookup"><span data-stu-id="638c8-126">If hello check box is not selected, subscription attempts will be auto-approved.</span></span> <span data-ttu-id="638c8-127">I det här exemplet godkänns automatiskt prenumerationer, så att inte markerar hello.</span><span class="sxs-lookup"><span data-stu-id="638c8-127">In this example, subscriptions are automatically approved, so do not select hello box.</span></span>

<span data-ttu-id="638c8-128">tooallow developer konton toosubscribe flera gånger toohello ny produkt, Välj hello **tillåter flera samtidiga prenumerationer** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="638c8-128">tooallow developer accounts toosubscribe multiple times toohello new product, select hello **Allow multiple simultaneous subscriptions** check box.</span></span> <span data-ttu-id="638c8-129">I den här självstudiekursen ska vi inte använda flera samtidiga prenumerationer och lämnar därför kryssrutan avmarkerad.</span><span class="sxs-lookup"><span data-stu-id="638c8-129">This tutorial does not utilize multiple simultaneous subscriptions, so leave it unchecked.</span></span>

<span data-ttu-id="638c8-130">När alla värden anges, klickar du på **spara** toocreate hello produkten.</span><span class="sxs-lookup"><span data-stu-id="638c8-130">After all values are entered, click **Save** toocreate hello product.</span></span>

![Produkten läggs till][api-management-product-added]

<span data-ttu-id="638c8-132">Som standard är nya produkter synliga toousers i hello **administratörer** grupp.</span><span class="sxs-lookup"><span data-stu-id="638c8-132">By default, new products are visible toousers in hello **Administrators** group.</span></span> <span data-ttu-id="638c8-133">Vi tooadd hello **utvecklare** grupp.</span><span class="sxs-lookup"><span data-stu-id="638c8-133">We are going tooadd hello **Developers** group.</span></span> <span data-ttu-id="638c8-134">Klicka på **kostnadsfri utvärderingsversion**, och klicka sedan på hello **synlighet** fliken.</span><span class="sxs-lookup"><span data-stu-id="638c8-134">Click **Free Trial**, and then click hello **Visibility** tab.</span></span>

> <span data-ttu-id="638c8-135">Grupper finns i API Management används toomanage hello synligheten för produkter toodevelopers.</span><span class="sxs-lookup"><span data-stu-id="638c8-135">In API Management, groups are used toomanage hello visibility of products toodevelopers.</span></span> <span data-ttu-id="638c8-136">Produkter ge synlighet toogroups och utvecklare kan visa och prenumerera toohello produkter som är synliga toohello grupper som de tillhör.</span><span class="sxs-lookup"><span data-stu-id="638c8-136">Products grant visibility toogroups, and developers can view and subscribe toohello products that are visible toohello groups in which they belong.</span></span> <span data-ttu-id="638c8-137">Mer information finns i [hur toocreate och använda grupper i Azure API Management][How toocreate and use groups in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="638c8-137">For more information, see [How toocreate and use groups in Azure API Management][How toocreate and use groups in Azure API Management].</span></span>
> 
> 

![Lägga till en utvecklargrupp][api-management-add-developers-group]

<span data-ttu-id="638c8-139">Välj hello **utvecklare** kryssrutan och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="638c8-139">Select hello **Developers** check box, and then click **Save**.</span></span>

## <span data-ttu-id="638c8-140"><a name="add-api"></a>tooadd API toohello produkten</span><span class="sxs-lookup"><span data-stu-id="638c8-140"><a name="add-api"> </a>tooadd an API toohello product</span></span>
<span data-ttu-id="638c8-141">I det här steget i självstudiekursen hello vi lägga till hello Echo API toohello nya kostnadsfri utvärderingsversion av produkten.</span><span class="sxs-lookup"><span data-stu-id="638c8-141">In this step of hello tutorial, we will add hello Echo API toohello new Free Trial product.</span></span>

> <span data-ttu-id="638c8-142">Varje instans för API Management-tjänsten har redan konfigurerats med en Echo-API som kan använda tooexperiment med och lär dig mer om API-hantering.</span><span class="sxs-lookup"><span data-stu-id="638c8-142">Each API Management service instance comes pre-configured with an Echo API that can be used tooexperiment with and learn about API Management.</span></span> <span data-ttu-id="638c8-143">Mer information finns i [Hantera ditt första API i Azure API Management][Manage your first API in Azure API Management].</span><span class="sxs-lookup"><span data-stu-id="638c8-143">For more information, see [Manage your first API in Azure API Management][Manage your first API in Azure API Management].</span></span>
> 
> 

<span data-ttu-id="638c8-144">Klicka på **produkter** från hello **API Management** menyn på hello vänster och klicka sedan på **kostnadsfri utvärderingsversion** tooconfigure hello produkten.</span><span class="sxs-lookup"><span data-stu-id="638c8-144">Click **Products** from hello **API Management** menu on hello left, and then click **Free Trial** tooconfigure hello product.</span></span>

![Konfigurera produkten][api-management-configure-product]

<span data-ttu-id="638c8-146">Klicka på **lägga till API tooproduct**.</span><span class="sxs-lookup"><span data-stu-id="638c8-146">Click **Add API tooproduct**.</span></span>

![Lägg till API tooproduct][api-management-add-api]

<span data-ttu-id="638c8-148">Välj **Echo API** och klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="638c8-148">Select **Echo API**, and then click **Save**.</span></span>

![Lägg till Echo API][api-management-add-echo-api]

## <span data-ttu-id="638c8-150"><a name="policies"></a>tooconfigure anropa hastighet principer för begränsning av och kvot</span><span class="sxs-lookup"><span data-stu-id="638c8-150"><a name="policies"> </a>tooconfigure call rate limit and quota policies</span></span>
<span data-ttu-id="638c8-151">Hastighetsbegränsningar och kvoter konfigureras i hello redigeraren.</span><span class="sxs-lookup"><span data-stu-id="638c8-151">Rate limits and quotas are configured in hello policy editor.</span></span> <span data-ttu-id="638c8-152">hello två principer som vi kommer att lägga till i den här kursen är hello [gränsen anropet frekvensen per prenumeration](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) och [Set kvot per prenumeration](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) principer.</span><span class="sxs-lookup"><span data-stu-id="638c8-152">hello two policies we will be adding in this tutorial are hello [Limit call rate per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) and [Set usage quota per subscription](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) policies.</span></span> <span data-ttu-id="638c8-153">Dessa principer tillämpas hello produkten definitionsområdet.</span><span class="sxs-lookup"><span data-stu-id="638c8-153">These policies must be applied at hello product scope.</span></span>

<span data-ttu-id="638c8-154">Klicka på **principer** under hello **API Management** menyn hello vänster.</span><span class="sxs-lookup"><span data-stu-id="638c8-154">Click **Policies** under hello **API Management** menu on hello left.</span></span> <span data-ttu-id="638c8-155">I hello **produkten** klickar du på **kostnadsfri utvärderingsversion**.</span><span class="sxs-lookup"><span data-stu-id="638c8-155">In hello **Product** list, click **Free Trial**.</span></span>

![Princip för produkt][api-management-product-policy]

<span data-ttu-id="638c8-157">Klicka på **Lägg till princip** tooimport hello Principmall och börja skapa hello hastighet gränsen och kvot principer.</span><span class="sxs-lookup"><span data-stu-id="638c8-157">Click **Add Policy** tooimport hello policy template and begin creating hello rate limit and quota policies.</span></span>

![Lägg till princip][api-management-add-policy]

<span data-ttu-id="638c8-159">Hastigheten med vilken gränsen och kvot principer är inkommande principer, så position hello markören i hello inkommande element.</span><span class="sxs-lookup"><span data-stu-id="638c8-159">Rate limit and quota policies are inbound policies, so position hello cursor in hello inbound element.</span></span>

![Principredigerare][api-management-policy-editor-inbound]

<span data-ttu-id="638c8-161">Rulla igenom hello lista över principer och hitta hello **gränsen anropet frekvensen per prenumeration** post i principen.</span><span class="sxs-lookup"><span data-stu-id="638c8-161">Scroll through hello list of policies and locate hello **Limit call rate per subscription** policy entry.</span></span>

![Principrapporter][api-management-limit-policies]

<span data-ttu-id="638c8-163">Efter hello markören är placerad i hello **inkommande** principelement, klicka på hello pilen bredvid **gränsen anropet frekvensen per prenumeration** tooinsert mall för Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="638c8-163">After hello cursor is positioned in hello **inbound** policy element, click hello arrow beside **Limit call rate per subscription** tooinsert its policy template.</span></span>

```xml
<rate-limit calls="number" renewal-period="seconds">
<api name="name" calls="number">
<operation name="name" calls="number" />
</api>
</rate-limit>
```

<span data-ttu-id="638c8-164">Hello principen kan inställningsgränser för hello produkten API: er och åtgärder som du kan se från kodutdrag hello.</span><span class="sxs-lookup"><span data-stu-id="638c8-164">As you can see from hello snippet, hello policy allows setting limits for hello product's APIs and operations.</span></span> <span data-ttu-id="638c8-165">I den här självstudiekursen kommer vi inte använda den här funktionen, så ta bort hello **api** och **åtgärden** element från hello **gräns för överföringshastigheten** elementet så att endast hello yttre **gräns för överföringshastigheten** förblir element som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="638c8-165">In this tutorial we will not use that capability, so delete hello **api** and **operation** elements from hello **rate-limit** element, such that only hello outer **rate-limit** element remains, as shown in hello following example.</span></span>

```xml
<rate-limit calls="number" renewal-period="seconds">
</rate-limit>
```

<span data-ttu-id="638c8-166">I hello kostnadsfri utvärderingsversion produkten hello högsta tillåtna anropet frekvensen är 10 anrop per minut, så Skriv **10** som hello värde hello **anrop** attribut, och **60** för hello **förnyelseperioden** attribut.</span><span class="sxs-lookup"><span data-stu-id="638c8-166">In hello Free Trial product, hello maximum allowable call rate is 10 calls per minute, so type **10** as hello value for hello **calls** attribute, and **60** for hello **renewal-period** attribute.</span></span>

```xml
<rate-limit calls="10" renewal-period="60">
</rate-limit>
```

<span data-ttu-id="638c8-167">tooconfigure hello **Set kvot per prenumeration** princip, position markören direkt under hello nyligen tillagda **gräns för överföringshastigheten** element i hello **inkommande** element, leta upp och klickar på hello pilen toohello till vänster i **Set kvot per prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="638c8-167">tooconfigure hello **Set usage quota per subscription** policy, position your cursor immediately below hello newly added **rate-limit** element within hello **inbound** element, and then locate and click hello arrow toohello left of **Set usage quota per subscription**.</span></span>

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
<api name="name" calls="number" bandwidth="kilobytes">
<operation name="name" calls="number" bandwidth="kilobytes" />
</api>
</quota>
```

<span data-ttu-id="638c8-168">På liknande sätt toohello **Set kvot per prenumeration** princip, **Set kvot per prenumeration** principen kan ange versaler för för hello produkten API: er och åtgärder.</span><span class="sxs-lookup"><span data-stu-id="638c8-168">Similarly toohello **Set usage quota per subscription** policy, **Set usage quota per subscription** policy allows setting caps for on hello product's APIs and operations.</span></span> <span data-ttu-id="638c8-169">I den här självstudiekursen kommer vi inte använda den här funktionen, så ta bort hello **api** och **åtgärden** element från hello **kvot** element, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="638c8-169">In this tutorial we will not use that capability, so delete hello **api** and **operation** elements from hello **quota** element, as shown in hello following example.</span></span>

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
</quota>
```

<span data-ttu-id="638c8-170">Kvoter kan baseras på hello antal anrop per intervall, bandbredd eller båda.</span><span class="sxs-lookup"><span data-stu-id="638c8-170">Quotas can be based on hello number of calls per interval, bandwidth, or both.</span></span> <span data-ttu-id="638c8-171">I den här självstudiekursen vi inte begränsa utifrån bandbredd, så ta bort hello **bandbredd** attribut.</span><span class="sxs-lookup"><span data-stu-id="638c8-171">In this tutorial, we are not throttling based on bandwidth, so delete hello **bandwidth** attribute.</span></span>

```xml
<quota calls="number" renewal-period="seconds">
</quota>
```

<span data-ttu-id="638c8-172">I hello kostnadsfri utvärderingsversion produkten är hello kvoten 200 anrop per vecka.</span><span class="sxs-lookup"><span data-stu-id="638c8-172">In hello Free Trial product, hello quota is 200 calls per week.</span></span> <span data-ttu-id="638c8-173">Ange **200** som hello värde hello **anrop** attributet och sedan ange **604800** som hello värde hello **förnyelseperioden** attribut.</span><span class="sxs-lookup"><span data-stu-id="638c8-173">Specify **200** as hello value for hello **calls** attribute, and then specify **604800** as hello value for hello **renewal-period** attribute.</span></span>

```xml
<quota calls="200" renewal-period="604800">
</quota>
```

> <span data-ttu-id="638c8-174">Principintervall anges i sekunder.</span><span class="sxs-lookup"><span data-stu-id="638c8-174">Policy intervals are specified in seconds.</span></span> <span data-ttu-id="638c8-175">toocalculate hello-intervall för en vecka, multiplicera hello antal dagar (7) av hello antal timmar under en dag (24) av hello antalet minuter under en timme (60) av hello antal sekunder per minut (60): 7 * 24 * 60 * 60 = 604800.</span><span class="sxs-lookup"><span data-stu-id="638c8-175">toocalculate hello interval for a week, you can multiply hello number of days (7) by hello number of hours in a day (24) by hello number of minutes in an hour (60) by hello number of seconds in a minute (60): 7 * 24 * 60 * 60 = 604800.</span></span>
> 
> 

<span data-ttu-id="638c8-176">När du har konfigurerat hello princip måste den matcha hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="638c8-176">When you have finished configuring hello policy, it should match hello following example.</span></span>

```xml
<policies>
    <inbound>
        <rate-limit calls="10" renewal-period="60">
        </rate-limit>
        <quota calls="200" renewal-period="604800">
        </quota>
        <base />

</inbound>
<outbound>

    <base />

    </outbound>
</policies>
```

<span data-ttu-id="638c8-177">När hello önskad principer som har konfigurerats, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="638c8-177">After hello desired policies are configured, click **Save**.</span></span>

![Spara en princip][api-management-policy-save]

## <span data-ttu-id="638c8-179"><a name="publish-product"></a> toopublish hello produkten</span><span class="sxs-lookup"><span data-stu-id="638c8-179"><a name="publish-product"> </a> toopublish hello product</span></span>
<span data-ttu-id="638c8-180">Nu när hello hello API: er läggs till och hello principerna är konfigurerade publiceras hello produkten så att den kan användas av utvecklare.</span><span class="sxs-lookup"><span data-stu-id="638c8-180">Now that hello hello APIs are added and hello policies are configured, hello product must be published so that it can be used by developers.</span></span> <span data-ttu-id="638c8-181">Klicka på **produkter** från hello **API Management** menyn på hello vänster och klicka sedan på **kostnadsfri utvärderingsversion** tooconfigure hello produkten.</span><span class="sxs-lookup"><span data-stu-id="638c8-181">Click **Products** from hello **API Management** menu on hello left, and then click **Free Trial** tooconfigure hello product.</span></span>

![Konfigurera produkten][api-management-configure-product]

<span data-ttu-id="638c8-183">Klicka på **publicera**, och klicka sedan på **Ja, publicera den** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="638c8-183">Click **Publish**, and then click **Yes, publish it** tooconfirm.</span></span>

![Publicera produkten][api-management-publish-product]

## <span data-ttu-id="638c8-185"><a name="subscribe-account"></a>toosubscribe en utvecklare konto toohello produkt</span><span class="sxs-lookup"><span data-stu-id="638c8-185"><a name="subscribe-account"> </a>toosubscribe a developer account toohello product</span></span>
<span data-ttu-id="638c8-186">Nu hello produkten publiceras, är tillgängliga toobe prenumererar tooand användas av utvecklare.</span><span class="sxs-lookup"><span data-stu-id="638c8-186">Now that hello product is published, it is available toobe subscribed tooand used by developers.</span></span>

> <span data-ttu-id="638c8-187">Administratörer av en API Management-instans är automatiskt prenumererade tooevery produkten.</span><span class="sxs-lookup"><span data-stu-id="638c8-187">Administrators of an API Management instance are automatically subscribed tooevery product.</span></span> <span data-ttu-id="638c8-188">I den här självstudiekursen steg ska vi prenumerera på något av hello-administratör developer konton toohello kostnadsfri utvärderingsversion produkt.</span><span class="sxs-lookup"><span data-stu-id="638c8-188">In this tutorial step, we will subscribe one of hello non-administrator developer accounts toohello Free Trial product.</span></span> <span data-ttu-id="638c8-189">Om ditt utvecklarkonto ingår i hello administratörsrollen kan följa du tillsammans med det här steget, även om du redan prenumererar.</span><span class="sxs-lookup"><span data-stu-id="638c8-189">If your developer account is part of hello Administrators role, then you can follow along with this step, even though you are already subscribed.</span></span>
> 
> 

<span data-ttu-id="638c8-190">Klicka på **användare** på hello **API Management** menyn på hello vänster och klicka sedan på hello namnet på ditt utvecklarkonto.</span><span class="sxs-lookup"><span data-stu-id="638c8-190">Click **Users** on hello **API Management** menu on hello left, and then click hello name of your developer account.</span></span> <span data-ttu-id="638c8-191">I det här exemplet använder vi hello **Clayton Gragg** utvecklarkonto.</span><span class="sxs-lookup"><span data-stu-id="638c8-191">In this example, we are using hello **Clayton Gragg** developer account.</span></span>

![Konfigurera en utvecklare][api-management-configure-developer]

<span data-ttu-id="638c8-193">Klicka på **Lägg till prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="638c8-193">Click **Add Subscription**.</span></span>

![Lägg till en prenumeration][api-management-add-subscription-menu]

<span data-ttu-id="638c8-195">Välj **Kostnadsfri utvärdering** och klicka sedan på **Prenumerera**.</span><span class="sxs-lookup"><span data-stu-id="638c8-195">Select **Free Trial**, and then click **Subscribe**.</span></span>

![Lägg till en prenumeration][api-management-add-subscription]

> [!NOTE]
> <span data-ttu-id="638c8-197">I den här självstudiekursen aktiveras inte flera samtidiga prenumerationer för hello kostnadsfri utvärderingsversion av produkten.</span><span class="sxs-lookup"><span data-stu-id="638c8-197">In this tutorial, multiple simultaneous subscriptions are not enabled for hello Free Trial product.</span></span> <span data-ttu-id="638c8-198">Om de vore kommer du att tillfrågas tooname hello prenumeration, som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="638c8-198">If they were, you would be prompted tooname hello subscription, as shown in hello following example.</span></span>
> 
> 

![Lägg till en prenumeration][api-management-add-subscription-multiple]

<span data-ttu-id="638c8-200">När du klickar på **prenumerera**, hello produkten visas i hello **prenumeration** listan för hello användaren.</span><span class="sxs-lookup"><span data-stu-id="638c8-200">After clicking **Subscribe**, hello product appears in hello **Subscription** list for hello user.</span></span>

![Prenumerationen har lagts till][api-management-subscription-added]

## <span data-ttu-id="638c8-202"><a name="test-rate-limit"></a>toocall en åtgärd och testa hello hastighetsbegränsning</span><span class="sxs-lookup"><span data-stu-id="638c8-202"><a name="test-rate-limit"> </a>toocall an operation and test hello rate limit</span></span>
<span data-ttu-id="638c8-203">Nu när hello kostnadsfri utvärderingsversion produkten har konfigurerats och publicerade, vi anropa vissa åtgärder och testa hello hastighet gränsen principen.</span><span class="sxs-lookup"><span data-stu-id="638c8-203">Now that hello Free Trial product is configured and published, we can call some operations and test hello rate limit policy.</span></span>
<span data-ttu-id="638c8-204">Växeln toohello developer-portalen genom att klicka på **utvecklarportalen** hello övre högra menyn.</span><span class="sxs-lookup"><span data-stu-id="638c8-204">Switch toohello developer portal by clicking **Developer portal** in hello upper-right menu.</span></span>

![Utvecklarportalen][api-management-developer-portal-menu]

<span data-ttu-id="638c8-206">Klicka på **API: er** i hello översta menyn och klicka sedan på **Echo API**.</span><span class="sxs-lookup"><span data-stu-id="638c8-206">Click **APIs** in hello top menu, and then click **Echo API**.</span></span>

![Utvecklarportalen][api-management-developer-portal-api-menu]

<span data-ttu-id="638c8-208">Klicka på **GET Resource** och klicka sedan på **Prova**.</span><span class="sxs-lookup"><span data-stu-id="638c8-208">Click **GET Resource**, and then click **Try it**.</span></span>

![Öppna konsolen][api-management-open-console]

<span data-ttu-id="638c8-210">Behåller hello standardvärdet parametervärden och välj sedan din prenumeration nyckel för hello kostnadsfri utvärderingsversion av produkten.</span><span class="sxs-lookup"><span data-stu-id="638c8-210">Keep hello default parameter values, and then select your subscription key for hello Free Trial product.</span></span>

![Prenumerationsnyckel][api-management-select-key]

> [!NOTE]
> <span data-ttu-id="638c8-212">Om du har flera prenumerationer kan vara säker på att tooselect hello nyckel för **kostnadsfri utvärderingsversion**, eller annan hello principer som konfigurerades i hello föregående steg inte gäller.</span><span class="sxs-lookup"><span data-stu-id="638c8-212">If you have multiple subscriptions, be sure tooselect hello key for **Free Trial**, or else hello policies that were configured in hello previous steps won't be in effect.</span></span>
> 
> 

<span data-ttu-id="638c8-213">Klicka på **skicka**, och sedan visa hello svar.</span><span class="sxs-lookup"><span data-stu-id="638c8-213">Click **Send**, and then view hello response.</span></span> <span data-ttu-id="638c8-214">Obs hello **svarsstatusen** av **200 OK**.</span><span class="sxs-lookup"><span data-stu-id="638c8-214">Note hello **Response status** of **200 OK**.</span></span>

![Åtgärdsresultat][api-management-http-get-results]

<span data-ttu-id="638c8-216">Klicka på **skicka** med en hastighet som är större än hello hastighet gränsen princip av 10 anrop per minut.</span><span class="sxs-lookup"><span data-stu-id="638c8-216">Click **Send** at a rate greater than hello rate limit policy of 10 calls per minute.</span></span> <span data-ttu-id="638c8-217">När hello hastighet gränsen princip överskrids svarsstatusen **429 för många begäranden** returneras.</span><span class="sxs-lookup"><span data-stu-id="638c8-217">After hello rate limit policy is exceeded, a response status of **429 Too Many Requests** is returned.</span></span>

![Åtgärdsresultat][api-management-http-get-429]

<span data-ttu-id="638c8-219">Hej **svar innehåll** anger hello återstående intervallet innan försök kommer att lyckas.</span><span class="sxs-lookup"><span data-stu-id="638c8-219">hello **Response content** indicates hello remaining interval before retries will be successful.</span></span>

<span data-ttu-id="638c8-220">När hello hastighet gränsen princip av 10 anrop per minut har aktiverats kan efterföljande anrop misslyckas tills 60 sekunder har förflutit från hello första produktens hello 10 antal samtal toohello innan hello hastighetsbegränsning har överskridits.</span><span class="sxs-lookup"><span data-stu-id="638c8-220">When hello rate limit policy of 10 calls per minute is in effect, subsequent calls will fail until 60 seconds have elapsed from hello first of hello 10 successful calls toohello product before hello rate limit was exceeded.</span></span> <span data-ttu-id="638c8-221">I det här exemplet är hello återstående intervall 54 sekunder.</span><span class="sxs-lookup"><span data-stu-id="638c8-221">In this example, hello remaining interval is 54 seconds.</span></span>

## <span data-ttu-id="638c8-222"><a name="next-steps"> </a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="638c8-222"><a name="next-steps"> </a>Next steps</span></span>
* <span data-ttu-id="638c8-223">Titta på en demonstration av inställningen hastighetsbegränsningar och kvoter i hello följande video.</span><span class="sxs-lookup"><span data-stu-id="638c8-223">Watch a demo of setting rate limits and quotas in hello following video.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How tooadd operations tooan API]: api-management-howto-add-operations.md
[How tooadd and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs tooa product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Manage your first API in Azure API Management]: api-management-get-started.md
[How toocreate and use groups in Azure API Management]: api-management-howto-create-groups.md
[View subscribers tooa product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Configure call rate limit and quota policies]: #policies
[Add an API toohello product]: #add-api
[Publish hello product]: #publish-product
[Subscribe a developer account toohello product]: #subscribe-account
[Call an operation and test hello rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
