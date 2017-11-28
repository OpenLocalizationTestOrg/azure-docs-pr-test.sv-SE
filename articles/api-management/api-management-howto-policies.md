---
title: Principer i Azure API Management | Microsoft Docs
description: "Lär dig mer om att skapa, redigera och konfigurera principer i API-hantering."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 537e5caf-708b-430e-a83f-72b70af28aa9
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2016
ms.author: apimpm
ms.openlocfilehash: 7c1f235343074ec11c635097f2b094a10f3fe781
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="policies-in-azure-api-management"></a><span data-ttu-id="bda01-103">Principer i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="bda01-103">Policies in Azure API Management</span></span>
<span data-ttu-id="bda01-104">I Azure API Management är-principer en kraftfull funktion för systemet som tillåter utgivaren för att ändra funktionssättet för API: et genom konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="bda01-104">In Azure API Management, policies are a powerful capability of the system that allow the publisher to change the behavior of the API through configuration.</span></span> <span data-ttu-id="bda01-105">Principer är en samling av instruktioner som utförs i tur och ordning på begäran eller svar på en API.</span><span class="sxs-lookup"><span data-stu-id="bda01-105">Policies are a collection of Statements that are executed sequentially on the request or response of an API.</span></span> <span data-ttu-id="bda01-106">Populära instruktioner med Formatkonvertering från XML till JSON och anropa hastighet begränsa om du vill begränsa mängden inkommande samtal från en utvecklare.</span><span class="sxs-lookup"><span data-stu-id="bda01-106">Popular Statements include format conversion from XML to JSON and call rate limiting to restrict the amount of incoming calls from a developer.</span></span> <span data-ttu-id="bda01-107">Många fler principer är tillgängliga direkt.</span><span class="sxs-lookup"><span data-stu-id="bda01-107">Many more policies are available out of the box.</span></span>

<span data-ttu-id="bda01-108">Finns det [principreferens] [ Policy Reference] för en fullständig lista över principrapporter och deras inställningar.</span><span class="sxs-lookup"><span data-stu-id="bda01-108">See the [Policy Reference][Policy Reference] for a full list of policy statements and their settings.</span></span>

<span data-ttu-id="bda01-109">Principerna tillämpas i gatewayen som placeras mellan API-klienten och hanterade API.</span><span class="sxs-lookup"><span data-stu-id="bda01-109">Policies are applied inside the gateway which sits between the API consumer and the managed API.</span></span> <span data-ttu-id="bda01-110">Gatewayen tar emot alla förfrågningar och vanligtvis vidarebefordrar dem utan ändringar underliggande API: et.</span><span class="sxs-lookup"><span data-stu-id="bda01-110">The gateway receives all requests and usually forwards them unaltered to the underlying API.</span></span> <span data-ttu-id="bda01-111">En princip kan dock använda ändringar till både inkommande begäran och utgående svar.</span><span class="sxs-lookup"><span data-stu-id="bda01-111">However a policy can apply changes to both the inbound request and outbound response.</span></span>

<span data-ttu-id="bda01-112">Principuttryck kan användas som attributvärden eller textvärden i API Management-principer, under förutsättning att principen tillåter det.</span><span class="sxs-lookup"><span data-stu-id="bda01-112">Policy expressions can be used as attribute values or text values in any of the API Management policies, unless the policy specifies otherwise.</span></span> <span data-ttu-id="bda01-113">Vissa principer som den [Åtkomstkontrollflödet] [ Control flow] och [ange variabel] [ Set variable] principer är baserade på principuttrycken.</span><span class="sxs-lookup"><span data-stu-id="bda01-113">Some policies such as the [Control flow][Control flow] and [Set variable][Set variable] policies are based on policy expressions.</span></span> <span data-ttu-id="bda01-114">Mer information finns i [avancerade principer] [ Advanced policies] och [principuttrycken][Policy expressions].</span><span class="sxs-lookup"><span data-stu-id="bda01-114">For more information, see [Advanced policies][Advanced policies] and [Policy expressions][Policy expressions].</span></span>

## <span data-ttu-id="bda01-115"><a name="scopes"></a>Hur du konfigurerar principer</span><span class="sxs-lookup"><span data-stu-id="bda01-115"><a name="scopes"> </a>How to configure policies</span></span>
<span data-ttu-id="bda01-116">Principer kan konfigureras globalt eller i omfånget för en [produkten][Product], [API] [ API] eller [åtgärden] [Operation].</span><span class="sxs-lookup"><span data-stu-id="bda01-116">Policies can be configured globally or at the scope of a [Product][Product], [API][API] or [Operation][Operation].</span></span> <span data-ttu-id="bda01-117">Om du vill konfigurera en princip, navigerar du till redigeraren principer i publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="bda01-117">To configure a policy, navigate to the Policies editor in the publisher portal.</span></span>

![Principer-menyn][policies-menu]

<span data-ttu-id="bda01-119">Redigeraren principer består av tre huvudavsnitt: principområdet (överst), principdefinitionen instruktionerna lista över (höger) och där principer redigeras (vänster):</span><span class="sxs-lookup"><span data-stu-id="bda01-119">The policies editor consists of three main sections: the policy scope (top), the policy definition where policies are edited (left) and the statements list (right):</span></span>

![Redigeraren för principer][policies-editor]

<span data-ttu-id="bda01-121">Du måste välja vilka principen ska gälla för att börja konfigurera en princip.</span><span class="sxs-lookup"><span data-stu-id="bda01-121">To begin configuring a policy you must first select the scope at which the policy should apply.</span></span> <span data-ttu-id="bda01-122">På skärmbilden nedan i **Starter** produkt har valts.</span><span class="sxs-lookup"><span data-stu-id="bda01-122">In the screenshot below the **Starter** product is selected.</span></span> <span data-ttu-id="bda01-123">Observera att kvadratisk symbolen bredvid namnet på principen anger att en princip används redan på den här nivån.</span><span class="sxs-lookup"><span data-stu-id="bda01-123">Note that the square symbol next to the policy name indicates that a policy is already applied at this level.</span></span>

![Omfång][policies-scope]

<span data-ttu-id="bda01-125">Eftersom en princip har redan tillämpats, visas konfigurationen i vyn definition.</span><span class="sxs-lookup"><span data-stu-id="bda01-125">Since a policy has already been applied, the configuration is shown in the definition view.</span></span>

![Konfigurera][policies-configure]

<span data-ttu-id="bda01-127">Principen visas skrivskyddad först.</span><span class="sxs-lookup"><span data-stu-id="bda01-127">The policy is displayed read-only at first.</span></span> <span data-ttu-id="bda01-128">För att kunna redigera definition Klicka på **konfigurera principen för** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="bda01-128">In order to edit the definition click the **Configure Policy** action.</span></span>

![Redigera][policies-edit]

<span data-ttu-id="bda01-130">Principdefinitionen är ett enkelt XML-dokument som beskriver en sekvens av inkommande och utgående rapporter.</span><span class="sxs-lookup"><span data-stu-id="bda01-130">The policy definition is a simple XML document that describes a sequence of inbound and outbound statements.</span></span> <span data-ttu-id="bda01-131">XML-filen kan redigeras direkt i definitionsfönstret.</span><span class="sxs-lookup"><span data-stu-id="bda01-131">The XML can be edited directly in the definition window.</span></span> <span data-ttu-id="bda01-132">En lista över uttryck har angetts till höger och instruktioner gäller för den aktuella omfattningen är aktiverad och markerat; som framgår av de **gränsen anropa hastighet** instruktionen i skärmbilden ovan.</span><span class="sxs-lookup"><span data-stu-id="bda01-132">A list of statements is provided to the right and statements applicable to the current scope are enabled and highlighted; as demonstrated by the **Limit Call Rate** statement in the screenshot above.</span></span>

<span data-ttu-id="bda01-133">Klicka på en aktiverad instruktion lägger till rätt XML vid för markören i vyn definition.</span><span class="sxs-lookup"><span data-stu-id="bda01-133">Clicking an enabled statement will add the appropriate XML at the location of the cursor in the definition view.</span></span> 

> [!NOTE]
> <span data-ttu-id="bda01-134">Se till att du är i rätt omfattning för principen om den princip som du vill lägga till inte är aktiverad.</span><span class="sxs-lookup"><span data-stu-id="bda01-134">If the policy that you want to add is not enabled, ensure that you are in the correct scope for that policy.</span></span> <span data-ttu-id="bda01-135">Varje Principframställning är avsedd för användning i vissa scope och principen avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bda01-135">Each policy statement is designed for use in certain scopes and policy sections.</span></span> <span data-ttu-id="bda01-136">Om du vill granska princip avsnitt och scope för en princip, kontrollera den **användning** avsnittet för grupprincipobjekt i den [principreferens][Policy Reference].</span><span class="sxs-lookup"><span data-stu-id="bda01-136">To review the policy sections and scopes for a policy, check the **Usage** section for that policy in the [Policy Reference][Policy Reference].</span></span>
> 
> 

<span data-ttu-id="bda01-137">En fullständig lista över principrapporter och deras inställningar är tillgängliga i den [principreferens][Policy Reference].</span><span class="sxs-lookup"><span data-stu-id="bda01-137">A full list of policy statements and their settings are available in the [Policy Reference][Policy Reference].</span></span>

<span data-ttu-id="bda01-138">Till exempel för att lägga till en ny rapport för att begränsa inkommande begäranden till den angivna IP-adresser för markören innanför innehållet i den `inbound` XML-elementet och klicka på den **begränsa anroparen IP-adresser** instruktionen.</span><span class="sxs-lookup"><span data-stu-id="bda01-138">For example, to add a new statement to restrict incoming requests to specified IP addresses, place the cursor just inside the content of the `inbound` XML element and click the **Restrict caller IPs** statement.</span></span>

![Principer för begränsning av][policies-restrict]

<span data-ttu-id="bda01-140">Detta lägger till en XML-fragment till den `inbound` element som innehåller information om hur du konfigurerar instruktionen.</span><span class="sxs-lookup"><span data-stu-id="bda01-140">This will add an XML snippet to the `inbound` element that provides guidance on how to configure the statement.</span></span>

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

<span data-ttu-id="bda01-141">Begränsa inkommande begäranden och godkänner endast de från IP-adressen 1.2.3.4 ändra XML-filen på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="bda01-141">To limit inbound requests and accept only those from an IP address of 1.2.3.4 modify the XML as follows:</span></span>

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

![Spara][policies-save]

<span data-ttu-id="bda01-143">När du är klar konfigurerar instruktioner för principen, klickar du på **spara** och ändringarna sprids till API Management gateway omedelbart.</span><span class="sxs-lookup"><span data-stu-id="bda01-143">When complete configuring the statements for the policy, click **Save** and the changes will be propagated to the API Management gateway immediately.</span></span>

## <span data-ttu-id="bda01-144"><a name="sections"></a>Förstå principkonfiguration</span><span class="sxs-lookup"><span data-stu-id="bda01-144"><a name="sections"> </a>Understanding policy configuration</span></span>
<span data-ttu-id="bda01-145">En princip är en serie som utförs för en begäran och ett svar.</span><span class="sxs-lookup"><span data-stu-id="bda01-145">A policy is a series of statements that execute in order for a request and a response.</span></span> <span data-ttu-id="bda01-146">Konfigurationen är korrekt indelat i `inbound`, `backend`, `outbound`, och `on-error` avsnitten som visas i följande konfiguration.</span><span class="sxs-lookup"><span data-stu-id="bda01-146">The configuration is divided appropriately into `inbound`, `backend`, `outbound`, and `on-error` sections as shown in the following configuration.</span></span>

```xml
<policies>
  <inbound>
    <!-- statements to be applied to the request go here -->
  </inbound>
  <backend>
    <!-- statements to be applied before the request is forwarded to 
         the backend service go here -->
  </backend>
  <outbound>
    <!-- statements to be applied to the response go here -->
  </outbound>
  <on-error>
    <!-- statements to be applied if there is an error condition go here -->
  </on-error>
</policies> 
```

<span data-ttu-id="bda01-147">Om det finns ett fel under bearbetningen av en begäran, alla eventuella resterande steg i den `inbound`, `backend`, eller `outbound` avsnitt hoppas över och körning som leder till instruktionerna i den `on-error` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="bda01-147">If there is an error during the processing of a request, any remaining steps in the `inbound`, `backend`, or `outbound` sections are skipped and execution jumps to the statements in the `on-error` section.</span></span> <span data-ttu-id="bda01-148">Genom att placera principrapporter i den `on-error` avsnitt som du kan granska felet med hjälp av den `context.LastError` egenskapen inspektera och anpassa den fel svar med hjälp av den `set-body` principen, och konfigurera vad händer om ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="bda01-148">By placing policy statements in the `on-error` section you can review the error by using the `context.LastError` property, inspect and customize the error response using the `set-body` policy, and configure what happens if an error occurs.</span></span> <span data-ttu-id="bda01-149">Det finns felkoder för inbyggda steg och fel som kan uppstå under bearbetning av principrapporter.</span><span class="sxs-lookup"><span data-stu-id="bda01-149">There are error codes for built-in steps and for errors that may occur during the processing of policy statements.</span></span> <span data-ttu-id="bda01-150">Mer information finns i [felhantering i API Management-principer](https://msdn.microsoft.com/library/azure/mt629506.aspx).</span><span class="sxs-lookup"><span data-stu-id="bda01-150">For more information, see [Error handling in API Management policies](https://msdn.microsoft.com/library/azure/mt629506.aspx).</span></span>

<span data-ttu-id="bda01-151">Eftersom principer kan anges på olika nivåer (global, produkt, api och åtgärden) kan konfigurationen du ange vilken ordning som de principdefinitionen instruktioner köra med avseende på den överordnade principen.</span><span class="sxs-lookup"><span data-stu-id="bda01-151">Since policies can be specified at different levels (global, product, api and operation) the configuration provides a way for you to specify the order in which the policy definition's statements execute with respect to the parent policy.</span></span> 

<span data-ttu-id="bda01-152">Princip för scope utvärderas i följande ordning.</span><span class="sxs-lookup"><span data-stu-id="bda01-152">Policy scopes are evaluated in the following order.</span></span>

1. <span data-ttu-id="bda01-153">Globalt scope</span><span class="sxs-lookup"><span data-stu-id="bda01-153">Global scope</span></span>
2. <span data-ttu-id="bda01-154">Produkten omfång</span><span class="sxs-lookup"><span data-stu-id="bda01-154">Product scope</span></span>
3. <span data-ttu-id="bda01-155">API-scope</span><span class="sxs-lookup"><span data-stu-id="bda01-155">API scope</span></span>
4. <span data-ttu-id="bda01-156">Åtgärden omfång</span><span class="sxs-lookup"><span data-stu-id="bda01-156">Operation scope</span></span>

<span data-ttu-id="bda01-157">Instruktionerna i dem utvärderas enligt placeringen av den `base` element, om den finns.</span><span class="sxs-lookup"><span data-stu-id="bda01-157">The statements within them are evaluated according to the placement of the `base` element, if it is present.</span></span> <span data-ttu-id="bda01-158">Globala principen har ingen överordnad princip och använder den `<base>` element i den har ingen effekt.</span><span class="sxs-lookup"><span data-stu-id="bda01-158">Global policy has no parent policy and using the `<base>` element in it has no effect.</span></span>

<span data-ttu-id="bda01-159">Till exempel om du har en princip på global nivå och en princip som konfigurerats för ett API som tillämpas sedan varje gång det specifika API används båda principerna.</span><span class="sxs-lookup"><span data-stu-id="bda01-159">For example, if you have a policy at the global level and a policy configured for an API, then whenever that particular API is used both policies will be applied.</span></span> <span data-ttu-id="bda01-160">API Management gör deterministiska sorteringen av kombinerade hanteringsprinciper via baselementet.</span><span class="sxs-lookup"><span data-stu-id="bda01-160">API Management allows for deterministic ordering of combined policy statements via the base element.</span></span> 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

<span data-ttu-id="bda01-161">I ovanstående exempel principdefinitionen den `cross-domain` instruktionen skulle köras innan de högre principer som i sin tur följas av den `find-and-replace` princip.</span><span class="sxs-lookup"><span data-stu-id="bda01-161">In the example policy definition above, the `cross-domain` statement would execute before any higher policies which would in turn, be followed by the `find-and-replace` policy.</span></span> 

<span data-ttu-id="bda01-162">Klicka för att visa principer i det aktuella omfånget i Redigeraren för grupprinciper **beräkna om princip för valda omfång**.</span><span class="sxs-lookup"><span data-stu-id="bda01-162">To see the policies in the current scope in the policy editor, click **Recalculate effective policy for selected scope**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bda01-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bda01-163">Next steps</span></span>
<span data-ttu-id="bda01-164">Gå igenom följande video på principuttrycken.</span><span class="sxs-lookup"><span data-stu-id="bda01-164">Check out following video on policy expressions.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Policy-Expressions-in-Azure-API-Management/player]
> 
> 

[Policy Reference]: api-management-policy-reference.md
[Product]: api-management-howto-add-products.md
[API]: api-management-howto-add-products.md#add-apis 
[Operation]: api-management-howto-add-operations.md

[Advanced policies]: https://msdn.microsoft.com/library/azure/dn894085.aspx
[Control flow]: https://msdn.microsoft.com/library/azure/dn894085.aspx#choose
[Set variable]: https://msdn.microsoft.com/library/azure/dn894085.aspx#set_variable
[Policy expressions]: https://msdn.microsoft.com/library/azure/dn910913.aspx

[policies-menu]: ./media/api-management-howto-policies/api-management-policies-menu.png
[policies-editor]: ./media/api-management-howto-policies/api-management-policies-editor.png
[policies-scope]: ./media/api-management-howto-policies/api-management-policies-scope.png
[policies-configure]: ./media/api-management-howto-policies/api-management-policies-configure.png
[policies-edit]: ./media/api-management-howto-policies/api-management-policies-edit.png
[policies-restrict]: ./media/api-management-howto-policies/api-management-policies-restrict.png
[policies-save]: ./media/api-management-howto-policies/api-management-policies-save.png
