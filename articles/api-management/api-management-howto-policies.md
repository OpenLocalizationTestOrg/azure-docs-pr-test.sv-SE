---
title: aaaPolicies i Azure API Management | Microsoft Docs
description: "Lär dig hur toocreate, redigera och konfigurera principer i API-hantering."
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
ms.openlocfilehash: 9ab0f884a655004cb10c05085034df1795f512e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="policies-in-azure-api-management"></a><span data-ttu-id="68bf6-103">Principer i Azure API Management</span><span class="sxs-lookup"><span data-stu-id="68bf6-103">Policies in Azure API Management</span></span>
<span data-ttu-id="68bf6-104">I Azure API Management är-principer en kraftfull funktion hello system som tillåter hello publisher toochange hello funktionssätt hello API via konfiguration.</span><span class="sxs-lookup"><span data-stu-id="68bf6-104">In Azure API Management, policies are a powerful capability of hello system that allow hello publisher toochange hello behavior of hello API through configuration.</span></span> <span data-ttu-id="68bf6-105">Principer är en samling av instruktioner som utförs i tur och ordning på hello begäran eller svar på en API.</span><span class="sxs-lookup"><span data-stu-id="68bf6-105">Policies are a collection of Statements that are executed sequentially on hello request or response of an API.</span></span> <span data-ttu-id="68bf6-106">Populära instruktioner med Formatkonvertering från XML-tooJSON och anropa hastighet begränsa toorestrict hello mängden inkommande samtal från en utvecklare.</span><span class="sxs-lookup"><span data-stu-id="68bf6-106">Popular Statements include format conversion from XML tooJSON and call rate limiting toorestrict hello amount of incoming calls from a developer.</span></span> <span data-ttu-id="68bf6-107">Det finns många fler principer out of box hello.</span><span class="sxs-lookup"><span data-stu-id="68bf6-107">Many more policies are available out of hello box.</span></span>

<span data-ttu-id="68bf6-108">Se hello [principreferens] [ Policy Reference] för en fullständig lista över principrapporter och deras inställningar.</span><span class="sxs-lookup"><span data-stu-id="68bf6-108">See hello [Policy Reference][Policy Reference] for a full list of policy statements and their settings.</span></span>

<span data-ttu-id="68bf6-109">Principerna tillämpas i hello-gateway som placeras mellan hello API konsumenten och hello hanterade API.</span><span class="sxs-lookup"><span data-stu-id="68bf6-109">Policies are applied inside hello gateway which sits between hello API consumer and hello managed API.</span></span> <span data-ttu-id="68bf6-110">hello gateway tar emot alla förfrågningar och vanligtvis vidarebefordrar dem utan ändringar toohello underliggande API.</span><span class="sxs-lookup"><span data-stu-id="68bf6-110">hello gateway receives all requests and usually forwards them unaltered toohello underlying API.</span></span> <span data-ttu-id="68bf6-111">En princip kan dock använda ändringar tooboth hello inkommande begäran och utgående svar.</span><span class="sxs-lookup"><span data-stu-id="68bf6-111">However a policy can apply changes tooboth hello inbound request and outbound response.</span></span>

<span data-ttu-id="68bf6-112">Principen uttryck kan användas som attributvärden eller textvärden i någon av hello API Management-principer, såvida inte hello principen anger annars.</span><span class="sxs-lookup"><span data-stu-id="68bf6-112">Policy expressions can be used as attribute values or text values in any of hello API Management policies, unless hello policy specifies otherwise.</span></span> <span data-ttu-id="68bf6-113">Vissa principer, till exempel hello [Åtkomstkontrollflödet] [ Control flow] och [ange variabel] [ Set variable] principer är baserade på principuttrycken.</span><span class="sxs-lookup"><span data-stu-id="68bf6-113">Some policies such as hello [Control flow][Control flow] and [Set variable][Set variable] policies are based on policy expressions.</span></span> <span data-ttu-id="68bf6-114">Mer information finns i [avancerade principer] [ Advanced policies] och [principuttrycken][Policy expressions].</span><span class="sxs-lookup"><span data-stu-id="68bf6-114">For more information, see [Advanced policies][Advanced policies] and [Policy expressions][Policy expressions].</span></span>

## <span data-ttu-id="68bf6-115"><a name="scopes"></a>Hur tooconfigure principer</span><span class="sxs-lookup"><span data-stu-id="68bf6-115"><a name="scopes"> </a>How tooconfigure policies</span></span>
<span data-ttu-id="68bf6-116">Principer kan konfigureras globalt eller hello omfånget för en [produkten][Product], [API] [ API] eller [åtgärden] [Operation].</span><span class="sxs-lookup"><span data-stu-id="68bf6-116">Policies can be configured globally or at hello scope of a [Product][Product], [API][API] or [Operation][Operation].</span></span> <span data-ttu-id="68bf6-117">tooconfigure en princip, navigera toohello principer redigeraren i hello publisher-portalen.</span><span class="sxs-lookup"><span data-stu-id="68bf6-117">tooconfigure a policy, navigate toohello Policies editor in hello publisher portal.</span></span>

![Principer-menyn][policies-menu]

<span data-ttu-id="68bf6-119">Redigeraren för hello-principer består av tre huvudavsnitt: hello princip omfång (överst) hello principdefinitionen hello instruktioner lista över (höger) och där principer redigeras (vänster):</span><span class="sxs-lookup"><span data-stu-id="68bf6-119">hello policies editor consists of three main sections: hello policy scope (top), hello policy definition where policies are edited (left) and hello statements list (right):</span></span>

![Redigeraren för principer][policies-editor]

<span data-ttu-id="68bf6-121">toobegin konfigurerar en princip måste du först välja hello scope på vilka hello principen ska gälla.</span><span class="sxs-lookup"><span data-stu-id="68bf6-121">toobegin configuring a policy you must first select hello scope at which hello policy should apply.</span></span> <span data-ttu-id="68bf6-122">I hello skärmbilden nedan hello **Starter** produkt har valts.</span><span class="sxs-lookup"><span data-stu-id="68bf6-122">In hello screenshot below hello **Starter** product is selected.</span></span> <span data-ttu-id="68bf6-123">Observera att hello kvadratisk symbolen nästa toohello principnamn anger princip används redan på den här nivån.</span><span class="sxs-lookup"><span data-stu-id="68bf6-123">Note that hello square symbol next toohello policy name indicates that a policy is already applied at this level.</span></span>

![Omfång][policies-scope]

<span data-ttu-id="68bf6-125">Eftersom en princip har redan tillämpats, visas hello-konfiguration i hello definition vyn.</span><span class="sxs-lookup"><span data-stu-id="68bf6-125">Since a policy has already been applied, hello configuration is shown in hello definition view.</span></span>

![Konfigurera][policies-configure]

<span data-ttu-id="68bf6-127">hello principen visas skrivskyddad först.</span><span class="sxs-lookup"><span data-stu-id="68bf6-127">hello policy is displayed read-only at first.</span></span> <span data-ttu-id="68bf6-128">I ordning tooedit hello definition klickar du på hello **konfigurera principen för** åtgärd.</span><span class="sxs-lookup"><span data-stu-id="68bf6-128">In order tooedit hello definition click hello **Configure Policy** action.</span></span>

![Redigera][policies-edit]

<span data-ttu-id="68bf6-130">hello principdefinitionen är ett enkelt XML-dokument som beskriver en sekvens av inkommande och utgående rapporter.</span><span class="sxs-lookup"><span data-stu-id="68bf6-130">hello policy definition is a simple XML document that describes a sequence of inbound and outbound statements.</span></span> <span data-ttu-id="68bf6-131">hello XML kan redigeras direkt i hello definition fönster.</span><span class="sxs-lookup"><span data-stu-id="68bf6-131">hello XML can be edited directly in hello definition window.</span></span> <span data-ttu-id="68bf6-132">En lista över rapporter som är direkt toohello och instruktioner gäller toohello aktuella omfattningen är aktiverat markerat; som framgår av hello **gränsen anropa hastighet** instruktionen i hello skärmbilden ovan.</span><span class="sxs-lookup"><span data-stu-id="68bf6-132">A list of statements is provided toohello right and statements applicable toohello current scope are enabled and highlighted; as demonstrated by hello **Limit Call Rate** statement in hello screenshot above.</span></span>

<span data-ttu-id="68bf6-133">Klicka på en aktiverad instruktion läggs hello lämplig XML som hello platsen för hello markören i hello definition vy.</span><span class="sxs-lookup"><span data-stu-id="68bf6-133">Clicking an enabled statement will add hello appropriate XML at hello location of hello cursor in hello definition view.</span></span> 

> [!NOTE]
> <span data-ttu-id="68bf6-134">Om hello som du vill tooadd inte är aktiverad, kan du kontrollera att du är i hello rätt omfattning för grupprincipobjekt.</span><span class="sxs-lookup"><span data-stu-id="68bf6-134">If hello policy that you want tooadd is not enabled, ensure that you are in hello correct scope for that policy.</span></span> <span data-ttu-id="68bf6-135">Varje Principframställning är avsedd för användning i vissa scope och principen avsnitt.</span><span class="sxs-lookup"><span data-stu-id="68bf6-135">Each policy statement is designed for use in certain scopes and policy sections.</span></span> <span data-ttu-id="68bf6-136">tooreview hello princip avsnitt och scope för en princip, kontrollera hello **användning** avsnitt av principen i hello [principreferens][Policy Reference].</span><span class="sxs-lookup"><span data-stu-id="68bf6-136">tooreview hello policy sections and scopes for a policy, check hello **Usage** section for that policy in hello [Policy Reference][Policy Reference].</span></span>
> 
> 

<span data-ttu-id="68bf6-137">En fullständig lista över principrapporter och deras inställningar är tillgängliga i hello [principreferens][Policy Reference].</span><span class="sxs-lookup"><span data-stu-id="68bf6-137">A full list of policy statements and their settings are available in hello [Policy Reference][Policy Reference].</span></span>

<span data-ttu-id="68bf6-138">Till exempel tooadd en ny instruktionen toorestrict inkommande begäranden toospecified IP-adresser, markören hello innanför hello innehållet i hello `inbound` XML-elementet och klicka på hello **begränsa anroparen IP-adresser** instruktionen.</span><span class="sxs-lookup"><span data-stu-id="68bf6-138">For example, tooadd a new statement toorestrict incoming requests toospecified IP addresses, place hello cursor just inside hello content of hello `inbound` XML element and click hello **Restrict caller IPs** statement.</span></span>

![Principer för begränsning av][policies-restrict]

<span data-ttu-id="68bf6-140">Detta lägger till en XML-fragment toohello `inbound` element som innehåller information om hur tooconfigure hello instruktionen.</span><span class="sxs-lookup"><span data-stu-id="68bf6-140">This will add an XML snippet toohello `inbound` element that provides guidance on how tooconfigure hello statement.</span></span>

```xml
<ip-filter action="allow | forbid">
    <address>address</address>
    <address-range from="address" to="address"/>
</ip-filter>
```

<span data-ttu-id="68bf6-141">toolimit inkommande begäranden och accepterar endast de från IP-adressen 1.2.3.4 ändra hello XML på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="68bf6-141">toolimit inbound requests and accept only those from an IP address of 1.2.3.4 modify hello XML as follows:</span></span>

```xml
<ip-filter action="allow">
    <address>1.2.3.4</address>
</ip-filter>
```

![Spara][policies-save]

<span data-ttu-id="68bf6-143">När du är färdig konfigurera hello instruktioner för hello principen klickar du på **spara** och hello ändringar kommer att spridda toohello API Management gateway omedelbart.</span><span class="sxs-lookup"><span data-stu-id="68bf6-143">When complete configuring hello statements for hello policy, click **Save** and hello changes will be propagated toohello API Management gateway immediately.</span></span>

## <span data-ttu-id="68bf6-144"><a name="sections"></a>Förstå principkonfiguration</span><span class="sxs-lookup"><span data-stu-id="68bf6-144"><a name="sections"> </a>Understanding policy configuration</span></span>
<span data-ttu-id="68bf6-145">En princip är en serie som utförs för en begäran och ett svar.</span><span class="sxs-lookup"><span data-stu-id="68bf6-145">A policy is a series of statements that execute in order for a request and a response.</span></span> <span data-ttu-id="68bf6-146">hello-konfigurationen är korrekt indelat i `inbound`, `backend`, `outbound`, och `on-error` avsnitten enligt hello efter konfiguration.</span><span class="sxs-lookup"><span data-stu-id="68bf6-146">hello configuration is divided appropriately into `inbound`, `backend`, `outbound`, and `on-error` sections as shown in hello following configuration.</span></span>

```xml
<policies>
  <inbound>
    <!-- statements toobe applied toohello request go here -->
  </inbound>
  <backend>
    <!-- statements toobe applied before hello request is forwarded too
         hello backend service go here -->
  </backend>
  <outbound>
    <!-- statements toobe applied toohello response go here -->
  </outbound>
  <on-error>
    <!-- statements toobe applied if there is an error condition go here -->
  </on-error>
</policies> 
```

<span data-ttu-id="68bf6-147">Om det finns ett fel under hello bearbetningen av en begäran, alla eventuella resterande steg i hello `inbound`, `backend`, eller `outbound` avsnitt hoppas över och körningen hoppar toohello instruktioner i hello `on-error` avsnitt.</span><span class="sxs-lookup"><span data-stu-id="68bf6-147">If there is an error during hello processing of a request, any remaining steps in hello `inbound`, `backend`, or `outbound` sections are skipped and execution jumps toohello statements in hello `on-error` section.</span></span> <span data-ttu-id="68bf6-148">Placera principrapporter i hello `on-error` avsnitt som du kan granska hello fel med hjälp av hello `context.LastError` egenskapen inspektera och anpassa hello felsvar med hello `set-body` princip, och konfigurera vad händer om ett fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="68bf6-148">By placing policy statements in hello `on-error` section you can review hello error by using hello `context.LastError` property, inspect and customize hello error response using hello `set-body` policy, and configure what happens if an error occurs.</span></span> <span data-ttu-id="68bf6-149">Det finns felkoder för inbyggda steg och fel som kan uppstå under hello behandling av princip för instruktioner.</span><span class="sxs-lookup"><span data-stu-id="68bf6-149">There are error codes for built-in steps and for errors that may occur during hello processing of policy statements.</span></span> <span data-ttu-id="68bf6-150">Mer information finns i [felhantering i API Management-principer](https://msdn.microsoft.com/library/azure/mt629506.aspx).</span><span class="sxs-lookup"><span data-stu-id="68bf6-150">For more information, see [Error handling in API Management policies](https://msdn.microsoft.com/library/azure/mt629506.aspx).</span></span>

<span data-ttu-id="68bf6-151">Eftersom principer kan anges på olika nivåer (global, produkt, api och åtgärden) kan hello konfiguration du toospecify hello ordning där uttryck hello principdefinition köra med avseende toohello överordnade principen.</span><span class="sxs-lookup"><span data-stu-id="68bf6-151">Since policies can be specified at different levels (global, product, api and operation) hello configuration provides a way for you toospecify hello order in which hello policy definition's statements execute with respect toohello parent policy.</span></span> 

<span data-ttu-id="68bf6-152">Princip för scope utvärderas i ordning hello.</span><span class="sxs-lookup"><span data-stu-id="68bf6-152">Policy scopes are evaluated in hello following order.</span></span>

1. <span data-ttu-id="68bf6-153">Globalt scope</span><span class="sxs-lookup"><span data-stu-id="68bf6-153">Global scope</span></span>
2. <span data-ttu-id="68bf6-154">Produkten omfång</span><span class="sxs-lookup"><span data-stu-id="68bf6-154">Product scope</span></span>
3. <span data-ttu-id="68bf6-155">API-scope</span><span class="sxs-lookup"><span data-stu-id="68bf6-155">API scope</span></span>
4. <span data-ttu-id="68bf6-156">Åtgärden omfång</span><span class="sxs-lookup"><span data-stu-id="68bf6-156">Operation scope</span></span>

<span data-ttu-id="68bf6-157">hello instruktioner i dem utvärderas enligt toohello placering av hello `base` element, om den finns.</span><span class="sxs-lookup"><span data-stu-id="68bf6-157">hello statements within them are evaluated according toohello placement of hello `base` element, if it is present.</span></span> <span data-ttu-id="68bf6-158">Globala principen har ingen överordnad och använder hello `<base>` element i den har ingen effekt.</span><span class="sxs-lookup"><span data-stu-id="68bf6-158">Global policy has no parent policy and using hello `<base>` element in it has no effect.</span></span>

<span data-ttu-id="68bf6-159">Till exempel om du har en princip på global nivå i hello och en princip som konfigurerats för ett API som tillämpas sedan varje gång det specifika API används båda principerna.</span><span class="sxs-lookup"><span data-stu-id="68bf6-159">For example, if you have a policy at hello global level and a policy configured for an API, then whenever that particular API is used both policies will be applied.</span></span> <span data-ttu-id="68bf6-160">API Management gör deterministiska sorteringen av kombinerade hanteringsprinciper via hello baselementet.</span><span class="sxs-lookup"><span data-stu-id="68bf6-160">API Management allows for deterministic ordering of combined policy statements via hello base element.</span></span> 

```xml
<policies>
    <inbound>
        <cross-domain />
        <base />
        <find-and-replace from="xyz" to="abc" />
    </inbound>
</policies>
```

<span data-ttu-id="68bf6-161">I hello exempel principdefinitionen ovan, hello `cross-domain` instruktionen skulle köras innan de högre principer som i sin tur följas av hello `find-and-replace` princip.</span><span class="sxs-lookup"><span data-stu-id="68bf6-161">In hello example policy definition above, hello `cross-domain` statement would execute before any higher policies which would in turn, be followed by hello `find-and-replace` policy.</span></span> 

<span data-ttu-id="68bf6-162">toosee hello principer i hello aktuella omfattning i hello Redigeraren för grupprincipobjekt, klickar du på **beräkna om princip för valda omfång**.</span><span class="sxs-lookup"><span data-stu-id="68bf6-162">toosee hello policies in hello current scope in hello policy editor, click **Recalculate effective policy for selected scope**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="68bf6-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="68bf6-163">Next steps</span></span>
<span data-ttu-id="68bf6-164">Gå igenom följande video på principuttrycken.</span><span class="sxs-lookup"><span data-stu-id="68bf6-164">Check out following video on policy expressions.</span></span>

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
