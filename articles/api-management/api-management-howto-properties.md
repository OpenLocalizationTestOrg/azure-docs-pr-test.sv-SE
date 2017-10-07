---
title: aaaHow toouse egenskaper i Azure API Management-principer
description: "Lär dig hur toouse egenskaper i Azure API Management-principer."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f39b00f-cf6e-4cef-9bf2-1f89202c0bc0
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 1ff096deeb97543b48dcf1f40be9dbfcbcd09542
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-properties-in-azure-api-management-policies"></a><span data-ttu-id="10a68-103">Hur toouse egenskaper i Azure API Management-principer</span><span class="sxs-lookup"><span data-stu-id="10a68-103">How toouse properties in Azure API Management policies</span></span>
<span data-ttu-id="10a68-104">API Management-principer är en kraftfull funktion hello system som tillåter hello publisher toochange hello funktionssätt hello API via konfiguration.</span><span class="sxs-lookup"><span data-stu-id="10a68-104">API Management policies are a powerful capability of hello system that allow hello publisher toochange hello behavior of hello API through configuration.</span></span> <span data-ttu-id="10a68-105">Principer är en samling av instruktioner som utförs i tur och ordning på hello begäran eller svar på en API.</span><span class="sxs-lookup"><span data-stu-id="10a68-105">Policies are a collection of statements that are executed sequentially on hello request or response of an API.</span></span> <span data-ttu-id="10a68-106">Principrapporter kan konstrueras med literal textvärden, principuttrycken och egenskaper.</span><span class="sxs-lookup"><span data-stu-id="10a68-106">Policy statements can be constructed using literal text values, policy expressions, and properties.</span></span> 

<span data-ttu-id="10a68-107">Varje instans för API Management-tjänsten har en egenskapssamling av nyckel/värde-par som är global toohello tjänstinstansen.</span><span class="sxs-lookup"><span data-stu-id="10a68-107">Each API Management service instance has a properties collection of key/value pairs that are global toohello service instance.</span></span> <span data-ttu-id="10a68-108">De här egenskaperna kan vara används toomanage konstanta strängvärden för alla API-konfiguration och principer.</span><span class="sxs-lookup"><span data-stu-id="10a68-108">These properties can be used toomanage constant string values across all API configuration and policies.</span></span> <span data-ttu-id="10a68-109">Varje egenskap har hello följande attribut.</span><span class="sxs-lookup"><span data-stu-id="10a68-109">Each property has hello following attributes.</span></span>

| <span data-ttu-id="10a68-110">Attribut</span><span class="sxs-lookup"><span data-stu-id="10a68-110">Attribute</span></span> | <span data-ttu-id="10a68-111">Typ</span><span class="sxs-lookup"><span data-stu-id="10a68-111">Type</span></span> | <span data-ttu-id="10a68-112">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="10a68-112">Description</span></span> |
| --- | --- | --- |
| <span data-ttu-id="10a68-113">Namn</span><span class="sxs-lookup"><span data-stu-id="10a68-113">Name</span></span> |<span data-ttu-id="10a68-114">Sträng</span><span class="sxs-lookup"><span data-stu-id="10a68-114">string</span></span> |<span data-ttu-id="10a68-115">hello namnet på hello-egenskap.</span><span class="sxs-lookup"><span data-stu-id="10a68-115">hello name of hello property.</span></span> <span data-ttu-id="10a68-116">Den kan innehålla endast bokstäver, siffror, period, bindestreck och understreck.</span><span class="sxs-lookup"><span data-stu-id="10a68-116">It may contain only letters, digits, period, dash, and underscore characters.</span></span> |
| <span data-ttu-id="10a68-117">Värde</span><span class="sxs-lookup"><span data-stu-id="10a68-117">Value</span></span> |<span data-ttu-id="10a68-118">Sträng</span><span class="sxs-lookup"><span data-stu-id="10a68-118">string</span></span> |<span data-ttu-id="10a68-119">hello värdet för hello-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="10a68-119">hello value of hello property.</span></span> <span data-ttu-id="10a68-120">Kan inte vara tomt eller endast bestå av blanksteg.</span><span class="sxs-lookup"><span data-stu-id="10a68-120">It may not be empty or consist only of whitespace.</span></span> |
| <span data-ttu-id="10a68-121">Hemlighet</span><span class="sxs-lookup"><span data-stu-id="10a68-121">Secret</span></span> |<span data-ttu-id="10a68-122">Booleskt värde</span><span class="sxs-lookup"><span data-stu-id="10a68-122">boolean</span></span> |<span data-ttu-id="10a68-123">Anger om hello värde är en hemlighet och ska krypteras eller inte.</span><span class="sxs-lookup"><span data-stu-id="10a68-123">Determines whether hello value is a secret and should be encrypted or not.</span></span> |
| <span data-ttu-id="10a68-124">Taggar</span><span class="sxs-lookup"><span data-stu-id="10a68-124">Tags</span></span> |<span data-ttu-id="10a68-125">Strängmatris</span><span class="sxs-lookup"><span data-stu-id="10a68-125">array of string</span></span> |<span data-ttu-id="10a68-126">Valfritt taggar som när det ges kan vara används toofilter hello egenskapslistan.</span><span class="sxs-lookup"><span data-stu-id="10a68-126">Optional tags that when provided can be used toofilter hello property list.</span></span> |

<span data-ttu-id="10a68-127">Egenskaper som har konfigurerats i hello publisher portal på hello **egenskaper** fliken. I följande exempel hello, konfigureras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="10a68-127">Properties are configured in hello publisher portal on hello **Properties** tab. In hello following example, three properties are configured.</span></span>

![Egenskaper][api-management-properties]

<span data-ttu-id="10a68-129">Egenskapsvärden kan innehålla teckensträngar och [principuttrycken](https://msdn.microsoft.com/library/azure/dn910913.aspx).</span><span class="sxs-lookup"><span data-stu-id="10a68-129">Property values can contain literal strings and [policy expressions](https://msdn.microsoft.com/library/azure/dn910913.aspx).</span></span> <span data-ttu-id="10a68-130">hello följande tabell visar hello föregående tre exempel egenskaper och deras attribut.</span><span class="sxs-lookup"><span data-stu-id="10a68-130">hello following table shows hello previous three sample properties and their attributes.</span></span> <span data-ttu-id="10a68-131">Hej värdet för `ExpressionProperty` är ett principuttryck som returnerar en sträng som innehåller hello aktuellt datum och tid.</span><span class="sxs-lookup"><span data-stu-id="10a68-131">hello value of `ExpressionProperty` is a policy expression that returns a string containing hello current date and time.</span></span> <span data-ttu-id="10a68-132">Hej egenskapen `ContosoHeaderValue` är markerad som en hemlighet, så dess värde inte visas.</span><span class="sxs-lookup"><span data-stu-id="10a68-132">hello property `ContosoHeaderValue` is marked as a secret, so its value is not displayed.</span></span>

| <span data-ttu-id="10a68-133">Namn</span><span class="sxs-lookup"><span data-stu-id="10a68-133">Name</span></span> | <span data-ttu-id="10a68-134">Värde</span><span class="sxs-lookup"><span data-stu-id="10a68-134">Value</span></span> | <span data-ttu-id="10a68-135">Hemlighet</span><span class="sxs-lookup"><span data-stu-id="10a68-135">Secret</span></span> | <span data-ttu-id="10a68-136">Taggar</span><span class="sxs-lookup"><span data-stu-id="10a68-136">Tags</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="10a68-137">ContosoHeader</span><span class="sxs-lookup"><span data-stu-id="10a68-137">ContosoHeader</span></span> |<span data-ttu-id="10a68-138">trackingId</span><span class="sxs-lookup"><span data-stu-id="10a68-138">TrackingId</span></span> |<span data-ttu-id="10a68-139">False</span><span class="sxs-lookup"><span data-stu-id="10a68-139">False</span></span> |<span data-ttu-id="10a68-140">Contoso</span><span class="sxs-lookup"><span data-stu-id="10a68-140">Contoso</span></span> |
| <span data-ttu-id="10a68-141">ContosoHeaderValue</span><span class="sxs-lookup"><span data-stu-id="10a68-141">ContosoHeaderValue</span></span> |<span data-ttu-id="10a68-142">••••••••••••••••••••••</span><span class="sxs-lookup"><span data-stu-id="10a68-142">••••••••••••••••••••••</span></span> |<span data-ttu-id="10a68-143">True</span><span class="sxs-lookup"><span data-stu-id="10a68-143">True</span></span> |<span data-ttu-id="10a68-144">Contoso</span><span class="sxs-lookup"><span data-stu-id="10a68-144">Contoso</span></span> |
| <span data-ttu-id="10a68-145">ExpressionProperty</span><span class="sxs-lookup"><span data-stu-id="10a68-145">ExpressionProperty</span></span> |<span data-ttu-id="10a68-146">@(DateTime.Now.ToString())</span><span class="sxs-lookup"><span data-stu-id="10a68-146">@(DateTime.Now.ToString())</span></span> |<span data-ttu-id="10a68-147">False</span><span class="sxs-lookup"><span data-stu-id="10a68-147">False</span></span> | |

## <a name="toouse-a-property"></a><span data-ttu-id="10a68-148">toouse en egenskap</span><span class="sxs-lookup"><span data-stu-id="10a68-148">toouse a property</span></span>
<span data-ttu-id="10a68-149">toouse en egenskap i en princip, plats hello egenskapsnamn i paret dubbla klammerparenteser som `{{ContosoHeader}}`som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="10a68-149">toouse a property in a policy, place hello property name inside a double pair of braces like `{{ContosoHeader}}`, as shown in hello following example.</span></span>

```xml
<set-header name="{{ContosoHeader}}" exists-action="override">
  <value>{{ContosoHeaderValue}}</value>
</set-header>
```

<span data-ttu-id="10a68-150">I det här exemplet `ContosoHeader` används som hello namnet på en rubrik i en `set-header` principen och `ContosoHeaderValue` används som hello värdet för det huvudet.</span><span class="sxs-lookup"><span data-stu-id="10a68-150">In this example, `ContosoHeader` is used as hello name of a header in a `set-header` policy, and `ContosoHeaderValue` is used as hello value of that header.</span></span> <span data-ttu-id="10a68-151">När den här principen utvärderas under en begäran eller svar toohello API Management gateway, `{{ContosoHeader}}` och `{{ContosoHeaderValue}}` ersättas med deras respektive egenskapsvärden.</span><span class="sxs-lookup"><span data-stu-id="10a68-151">When this policy is evaluated during a request or response toohello API Management gateway, `{{ContosoHeader}}` and `{{ContosoHeaderValue}}` are replaced with their respective property values.</span></span>

<span data-ttu-id="10a68-152">Egenskaper som kan användas som slutförts attribut eller elementvärden enligt hello föregående exempel, men de kan också läggs till eller kombineras med en del av ett uttryck med exakt text som visas i följande exempel hello:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`</span><span class="sxs-lookup"><span data-stu-id="10a68-152">Properties can be used as complete attribute or element values as shown in hello previous example, but they can also be inserted into or combined with part of a literal text expression as shown in hello following example: `<set-header name = "CustomHeader{{ContosoHeader}}" ...>`</span></span>

<span data-ttu-id="10a68-153">Egenskaper kan också innehålla princip uttryck.</span><span class="sxs-lookup"><span data-stu-id="10a68-153">Properties can also contain policy expressions.</span></span> <span data-ttu-id="10a68-154">I följande exempel hello, hello `ExpressionProperty` används.</span><span class="sxs-lookup"><span data-stu-id="10a68-154">In hello following example, hello `ExpressionProperty` is used.</span></span>

```xml
<set-header name="CustomHeader" exists-action="override">
    <value>{{ExpressionProperty}}</value>
</set-header>
```

<span data-ttu-id="10a68-155">När den här principen utvärderas `{{ExpressionProperty}}` ersätts med värdet: `@(DateTime.Now.ToString())`.</span><span class="sxs-lookup"><span data-stu-id="10a68-155">When this policy is evaluated, `{{ExpressionProperty}}` is replaced with its value: `@(DateTime.Now.ToString())`.</span></span> <span data-ttu-id="10a68-156">Eftersom hello-värdet är ett principuttryck, hello evalueras och hello princip fortsätter med körningen.</span><span class="sxs-lookup"><span data-stu-id="10a68-156">Since hello value is a policy expression, hello expression is evaluated and hello policy proceeds with its execution.</span></span>

<span data-ttu-id="10a68-157">Du kan testa detta ut i hello developer-portalen genom att anropa en åtgärd som har en princip med egenskaper i sitt omfång.</span><span class="sxs-lookup"><span data-stu-id="10a68-157">You can test this out in hello developer portal by calling an operation that has a policy with properties in scope.</span></span> <span data-ttu-id="10a68-158">I följande exempel hello, kallas en åtgärd med hello två föregående exempel `set-header` principer med egenskaper.</span><span class="sxs-lookup"><span data-stu-id="10a68-158">In hello following example, an operation is called with hello two previous example `set-header` policies with properties.</span></span> <span data-ttu-id="10a68-159">Observera att hello svaret innehåller två anpassade huvuden som har konfigurerats med principer med egenskaper.</span><span class="sxs-lookup"><span data-stu-id="10a68-159">Note that hello response contains two custom headers that were configured using policies with properties.</span></span>

![Utvecklarportalen][api-management-send-results]

<span data-ttu-id="10a68-161">Om du tittar på hello [API Inspector trace](api-management-howto-api-inspector.md) för samtal som innehåller hello två föregående exempel principer med egenskaper, kan du se hello två `set-header` principer med hello egenskapsvärden infogas samt hello principuttryck utvärdering för hello-egenskap som innehöll hello principuttryck.</span><span class="sxs-lookup"><span data-stu-id="10a68-161">If you look at hello [API Inspector trace](api-management-howto-api-inspector.md) for a call that includes hello two previous sample policies with properties, you can see hello two `set-header` policies with hello property values inserted as well as hello policy expression evaluation for hello property that contained hello policy expression.</span></span>

![API-Inspector spårning][api-management-api-inspector-trace]

<span data-ttu-id="10a68-163">Observera att egenskapsvärden inte kan innehålla andra egenskaper medan egenskapsvärden kan innehålla principuttrycken.</span><span class="sxs-lookup"><span data-stu-id="10a68-163">Note that while property values can contain policy expressions, property values can't contain other properties.</span></span> <span data-ttu-id="10a68-164">Om text som innehåller en referens som används för ett egenskapsvärde `Property value text {{MyProperty}}`, att egenskapsreferens inte ersättas och inkluderas som en del av hello egenskapsvärde.</span><span class="sxs-lookup"><span data-stu-id="10a68-164">If text containing a property reference is used for a property value, such as `Property value text {{MyProperty}}`, that property reference won't be replaced and will be included as part of hello property value.</span></span>

## <a name="toocreate-a-property"></a><span data-ttu-id="10a68-165">toocreate en egenskap</span><span class="sxs-lookup"><span data-stu-id="10a68-165">toocreate a property</span></span>
<span data-ttu-id="10a68-166">toocreate en egenskap, klickar du på **Lägg till egenskap** på hello **egenskaper** fliken.</span><span class="sxs-lookup"><span data-stu-id="10a68-166">toocreate a property, click **Add property** on hello **Properties** tab.</span></span>

![Lägg till egenskap][api-management-properties-add-property-menu]

<span data-ttu-id="10a68-168">**Namnet** och **värdet** är värden som krävs.</span><span class="sxs-lookup"><span data-stu-id="10a68-168">**Name** and **Value** are required values.</span></span> <span data-ttu-id="10a68-169">Kontrollera om det här egenskapsvärdet är en hemlighet hello **detta är en hemlighet** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="10a68-169">If this property value is a secret, check hello **This is a secret** checkbox.</span></span> <span data-ttu-id="10a68-170">Ange en eller flera valfria taggar toohelp med ordna dina egenskaper och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="10a68-170">Enter one or more optional tags toohelp with organizing your properties, and click **Save**.</span></span>

![Lägg till egenskap][api-management-properties-add-property]

<span data-ttu-id="10a68-172">När en ny egenskap sparas hello **söka egenskapen** textruta fylls med hello namnet på hello ny egenskap och hello nya egenskapen visas.</span><span class="sxs-lookup"><span data-stu-id="10a68-172">When a new property is saved, hello **Search property** textbox is populated with hello name of hello new property and hello new property is displayed.</span></span> <span data-ttu-id="10a68-173">toodisplay alla egenskaper, avmarkera hello **söka egenskapen** textrutan och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="10a68-173">toodisplay all properties, clear hello **Search property** textbox and press enter.</span></span>

![Egenskaper][api-management-properties-property-saved]

<span data-ttu-id="10a68-175">Information om hur du skapar en egenskap med hello REST-API finns [skapa en egenskap med hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).</span><span class="sxs-lookup"><span data-stu-id="10a68-175">For information on creating a property using hello REST API, see [Create a property using hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).</span></span>

## <a name="tooedit-a-property"></a><span data-ttu-id="10a68-176">tooedit en egenskap</span><span class="sxs-lookup"><span data-stu-id="10a68-176">tooedit a property</span></span>
<span data-ttu-id="10a68-177">tooedit en egenskap, klickar du på **redigera** bredvid hello egenskapen tooedit.</span><span class="sxs-lookup"><span data-stu-id="10a68-177">tooedit a property, click **Edit** beside hello property tooedit.</span></span>

![Ändra egenskapen][api-management-properties-edit]

<span data-ttu-id="10a68-179">Gör eventuella ändringar och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="10a68-179">Make any desired changes, and click **Save**.</span></span> <span data-ttu-id="10a68-180">Om du ändrar hello egenskapsnamn är de principer som refererar till den egenskapen uppdateras automatiskt toouse hello nytt namn.</span><span class="sxs-lookup"><span data-stu-id="10a68-180">If you change hello property name, any policies that reference that property are automatically updated toouse hello new name.</span></span>

![Ändra egenskapen][api-management-properties-edit-property]

<span data-ttu-id="10a68-182">Information om hur du redigerar en egenskap med hello REST-API finns [redigera en egenskap med hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).</span><span class="sxs-lookup"><span data-stu-id="10a68-182">For information on editing a property using hello REST API, see [Edit a property using hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).</span></span>

## <a name="toodelete-a-property"></a><span data-ttu-id="10a68-183">toodelete en egenskap</span><span class="sxs-lookup"><span data-stu-id="10a68-183">toodelete a property</span></span>
<span data-ttu-id="10a68-184">toodelete en egenskap, klickar du på **ta bort** bredvid hello egenskapen toodelete.</span><span class="sxs-lookup"><span data-stu-id="10a68-184">toodelete a property, click **Delete** beside hello property toodelete.</span></span>

![Ta bort egenskapen][api-management-properties-delete]

<span data-ttu-id="10a68-186">Klicka på **Ja, ta bort den** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="10a68-186">Click **Yes, delete it** tooconfirm.</span></span>

![Bekräfta borttagning][api-management-delete-confirm]

> [!IMPORTANT]
> <span data-ttu-id="10a68-188">Om egenskapen hello refereras av principerna som du inte toosuccessfully ta bort den tills du tar bort hello egenskap från alla principer som använder den.</span><span class="sxs-lookup"><span data-stu-id="10a68-188">If hello property is referenced by any policies, you will be unable toosuccessfully delete it until you remove hello property from all policies that use it.</span></span>
> 
> 

<span data-ttu-id="10a68-189">Mer information om du tar bort en egenskap med hello REST-API finns [ta bort en egenskap med hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).</span><span class="sxs-lookup"><span data-stu-id="10a68-189">For information on deleting a property using hello REST API, see [Delete a property using hello REST API](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).</span></span>

## <a name="toosearch-and-filter-properties"></a><span data-ttu-id="10a68-190">toosearch och filtrera egenskaper</span><span class="sxs-lookup"><span data-stu-id="10a68-190">toosearch and filter properties</span></span>
<span data-ttu-id="10a68-191">Hej **egenskaper** innehåller sökning och filtrering funktioner toohelp som du hanterar dina egenskaper.</span><span class="sxs-lookup"><span data-stu-id="10a68-191">hello **Properties** tab includes searching and filtering capabilities toohelp you manage your properties.</span></span> <span data-ttu-id="10a68-192">toofilter hello egenskapslistan av egenskapsnamn, ange ett sökvillkor i hello **söka egenskapen** textruta.</span><span class="sxs-lookup"><span data-stu-id="10a68-192">toofilter hello property list by property name, enter a search term in hello **Search property** textbox.</span></span> <span data-ttu-id="10a68-193">toodisplay alla egenskaper, avmarkera hello **söka egenskapen** textrutan och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="10a68-193">toodisplay all properties, clear hello **Search property** textbox and press enter.</span></span>

![Söka][api-management-properties-search]

<span data-ttu-id="10a68-195">toofilter hello egenskapslistan av värden, ange en eller flera taggar i hello **filtrera efter taggar** textruta.</span><span class="sxs-lookup"><span data-stu-id="10a68-195">toofilter hello property list by tag values, enter one or more tags into hello **Filter by tags** textbox.</span></span> <span data-ttu-id="10a68-196">toodisplay alla egenskaper, avmarkera hello **filtrera efter taggar** textrutan och tryck på RETUR.</span><span class="sxs-lookup"><span data-stu-id="10a68-196">toodisplay all properties, clear hello **Filter by tags** textbox and press enter.</span></span>

![Filter][api-management-properties-filter]

## <a name="next-steps"></a><span data-ttu-id="10a68-198">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="10a68-198">Next steps</span></span>
* <span data-ttu-id="10a68-199">Lär dig mer om hur du arbetar med principer</span><span class="sxs-lookup"><span data-stu-id="10a68-199">Learn more about working with policies</span></span>
  * [<span data-ttu-id="10a68-200">Principer för i API-hantering</span><span class="sxs-lookup"><span data-stu-id="10a68-200">Policies in API Management</span></span>](api-management-howto-policies.md)
  * [<span data-ttu-id="10a68-201">Principreferens</span><span class="sxs-lookup"><span data-stu-id="10a68-201">Policy reference</span></span>](https://msdn.microsoft.com/library/azure/dn894081.aspx)
  * [<span data-ttu-id="10a68-202">Principuttryck</span><span class="sxs-lookup"><span data-stu-id="10a68-202">Policy expressions</span></span>](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a><span data-ttu-id="10a68-203">Titta på en videoöversikt</span><span class="sxs-lookup"><span data-stu-id="10a68-203">Watch a video overview</span></span>
> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Use-Properties-in-Policies/player]
> 
> 

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

