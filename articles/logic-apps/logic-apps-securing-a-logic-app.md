---
title: "aaaSecure komma åt tooAzure Logic Apps | Microsoft Docs"
description: "Lägger till säkerhet för att skydda åtkomst tootriggers indata och utdata, åtgärdsparametrar och tjänster som används med arbetsflöden i Azure Logic Apps."
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 9fab1050-cfbc-4a8b-b1b3-5531bee92856
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/22/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: abda2179e4cc2d2295cd8332ec017c848a456264
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="secure-access-tooyour-logic-apps"></a><span data-ttu-id="2c18b-103">Säker åtkomst tooyour logikappar</span><span class="sxs-lookup"><span data-stu-id="2c18b-103">Secure access tooyour logic apps</span></span>

<span data-ttu-id="2c18b-104">Det finns många verktyg-tillgängliga toohelp du skydda din logikapp.</span><span class="sxs-lookup"><span data-stu-id="2c18b-104">There are many tools available toohelp you secure your logic app.</span></span>

* <span data-ttu-id="2c18b-105">Skydda åtkomst tootrigger en logikapp (http-begäran utlösare)</span><span class="sxs-lookup"><span data-stu-id="2c18b-105">Securing access tootrigger a logic app (HTTP Request Trigger)</span></span>
* <span data-ttu-id="2c18b-106">Skydda åtkomst toomanage, redigera eller läsa en logikapp</span><span class="sxs-lookup"><span data-stu-id="2c18b-106">Securing access toomanage, edit, or read a logic app</span></span>
* <span data-ttu-id="2c18b-107">Skydda åtkomst toocontents indata och utdata för en körning</span><span class="sxs-lookup"><span data-stu-id="2c18b-107">Securing access toocontents of inputs and outputs for a run</span></span>
* <span data-ttu-id="2c18b-108">Att säkra parametrar eller indata i åtgärder i ett arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="2c18b-108">Securing parameters or inputs within actions in a workflow</span></span>
* <span data-ttu-id="2c18b-109">Skydda åtkomst tooservices som tar emot förfrågningar från ett arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="2c18b-109">Securing access tooservices that receive requests from a workflow</span></span>

## <a name="secure-access-tootrigger"></a><span data-ttu-id="2c18b-110">Säker åtkomst tootrigger</span><span class="sxs-lookup"><span data-stu-id="2c18b-110">Secure access tootrigger</span></span>

<span data-ttu-id="2c18b-111">När du arbetar med en logikapp som utlöses på en HTTP-begäran ([begära](../connectors/connectors-native-reqres.md) eller [Webhook](../connectors/connectors-native-webhook.md)), kan du begränsa åtkomsten så att endast auktoriserade klienter kan utlösas hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="2c18b-111">When you work with a logic app that fires on an HTTP Request ([Request](../connectors/connectors-native-reqres.md) or [Webhook](../connectors/connectors-native-webhook.md)), you can restrict access so that only authorized clients can fire hello logic app.</span></span> <span data-ttu-id="2c18b-112">Alla förfrågningar till en logikapp krypteras och skyddas via SSL.</span><span class="sxs-lookup"><span data-stu-id="2c18b-112">All requests into a logic app are encrypted and secured via SSL.</span></span>

### <a name="shared-access-signature"></a><span data-ttu-id="2c18b-113">Signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="2c18b-113">Shared Access Signature</span></span>

<span data-ttu-id="2c18b-114">Varje begäran slutpunkt för en logikapp innehåller en [delade signatur åtkomst (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) som en del av hello-URL.</span><span class="sxs-lookup"><span data-stu-id="2c18b-114">Every request endpoint for a logic app includes a [Shared Access Signature (SAS)](../storage/common/storage-dotnet-shared-access-signature-part-1.md) as part of hello URL.</span></span> <span data-ttu-id="2c18b-115">Varje URL innehåller en `sp`, `sv`, och `sig` Frågeparametern.</span><span class="sxs-lookup"><span data-stu-id="2c18b-115">Each URL contains a `sp`, `sv`, and `sig` query parameter.</span></span> <span data-ttu-id="2c18b-116">Behörigheter som anges av `sp`, och motsvarar tooHTTP metoder tillåts, `sv` är hello version som används för toogenerate och `sig` är används tooauthenticate åtkomst tootrigger.</span><span class="sxs-lookup"><span data-stu-id="2c18b-116">Permissions are specified by `sp`, and correspond tooHTTP methods allowed, `sv` is hello version used toogenerate, and `sig` is used tooauthenticate access tootrigger.</span></span> <span data-ttu-id="2c18b-117">hello signatur skapas med en hemlig nyckel på alla hello URL-sökvägar och egenskaper hello SHA256-algoritmen.</span><span class="sxs-lookup"><span data-stu-id="2c18b-117">hello signature is generated using hello SHA256 algorithm with a secret key on all hello URL paths and properties.</span></span> <span data-ttu-id="2c18b-118">hello hemlig nyckel aldrig exponeras och publiceras, och hålls krypterade och lagras som en del av hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="2c18b-118">hello secret key is never exposed and published, and is kept encrypted and stored as part of hello logic app.</span></span> <span data-ttu-id="2c18b-119">Din logikapp tillåter endast utlösare som innehåller en giltig signatur som skapats med hello hemlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="2c18b-119">Your logic app only authorizes triggers that contain a valid signature created with hello secret key.</span></span>

#### <a name="regenerate-access-keys"></a><span data-ttu-id="2c18b-120">Återskapa åtkomstnycklar</span><span class="sxs-lookup"><span data-stu-id="2c18b-120">Regenerate access keys</span></span>

<span data-ttu-id="2c18b-121">Du kan återskapa en ny säker nyckel vid när som helst via hello REST API- eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2c18b-121">You can regenerate a new secure key at anytime through hello REST API or Azure portal.</span></span> <span data-ttu-id="2c18b-122">Alla aktuella URL: er som har skapats tidigare med hello gamla nyckeln är inaktuella och inte längre auktoriserade toofire hello logikapp.</span><span class="sxs-lookup"><span data-stu-id="2c18b-122">All current URLs that were generated previously using hello old key are invalidated and no longer authorized toofire hello logic app.</span></span>

1. <span data-ttu-id="2c18b-123">Öppna hello logikappen som du vill tooregenerate en nyckel i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="2c18b-123">In hello Azure portal, open hello logic app you want tooregenerate a key</span></span>
1. <span data-ttu-id="2c18b-124">Klicka på hello **åtkomstnycklar** menyalternativet **inställningar**</span><span class="sxs-lookup"><span data-stu-id="2c18b-124">Click hello **Access Keys** menu item under **Settings**</span></span>
1. <span data-ttu-id="2c18b-125">Välj hello viktiga tooregenerate och fullständig hello-processen</span><span class="sxs-lookup"><span data-stu-id="2c18b-125">Choose hello key tooregenerate and complete hello process</span></span>

<span data-ttu-id="2c18b-126">URL: er som du hämtar efter återskapandet är signerade med hello nya snabbtangent.</span><span class="sxs-lookup"><span data-stu-id="2c18b-126">URLs you retrieve after regeneration are signed with hello new access key.</span></span>

#### <a name="creating-callback-urls-with-an-expiration-date"></a><span data-ttu-id="2c18b-127">Skapa återanrop URL: er med ett förfallodatum</span><span class="sxs-lookup"><span data-stu-id="2c18b-127">Creating callback URLs with an expiration date</span></span>

<span data-ttu-id="2c18b-128">Om du delar hello URL med andra parter kan generera du URL: er med specifika nycklar och förfallodatum efter behov.</span><span class="sxs-lookup"><span data-stu-id="2c18b-128">If you are sharing hello URL with other parties, you can generate URLs with specific keys and expiration dates as needed.</span></span> <span data-ttu-id="2c18b-129">Du kan sedan sömlöst Återställ nycklar eller kontrollera åtkomst toofire en app är begränsad tooa vissa timespan.</span><span class="sxs-lookup"><span data-stu-id="2c18b-129">You can then seamlessly roll keys, or ensure access toofire an app is restricted tooa certain timespan.</span></span> <span data-ttu-id="2c18b-130">Du kan ange ett sista giltighetsdatum för en URL via hello [logikappar REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):</span><span class="sxs-lookup"><span data-stu-id="2c18b-130">You can specify an expiration date for a URL through hello [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers):</span></span>

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

<span data-ttu-id="2c18b-131">Ta hello-egenskapen i hello brödtext `NotAfter` som en JSON-datumsträng som returnerar en motringning-URL som är endast giltig förrän hello `NotAfter` datum och tid.</span><span class="sxs-lookup"><span data-stu-id="2c18b-131">In hello body, include hello property `NotAfter` as a JSON date string, which returns a callback URL that is only valid until hello `NotAfter` date and time.</span></span>

#### <a name="creating-urls-with-primary-or-secondary-secret-key"></a><span data-ttu-id="2c18b-132">Skapa URL: er med primär eller sekundär hemlig nyckel</span><span class="sxs-lookup"><span data-stu-id="2c18b-132">Creating URLs with primary or secondary secret key</span></span>

<span data-ttu-id="2c18b-133">När du generera eller listan återanrop URL: er för frågebaserad utlösare kan ange du också vilka viktiga toouse toosign hello-URL.</span><span class="sxs-lookup"><span data-stu-id="2c18b-133">When you generate or list callback URLs for request-based triggers, you can also specify which key toouse toosign hello URL.</span></span>  <span data-ttu-id="2c18b-134">Du kan generera en URL som signerats av en viss nyckel via hello [logikappar REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="2c18b-134">You can generate a URL signed by a specific key through hello [logic apps REST API](https://docs.microsoft.com/rest/api/logic/workflowtriggers) as follows:</span></span>

``` http
POST 
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Logic/workflows/{workflowName}/triggers/{triggerName}/listCallbackUrl?api-version=2016-06-01
```

<span data-ttu-id="2c18b-135">Ta hello-egenskapen i hello brödtext `KeyType` som antingen `Primary` eller `Secondary`.</span><span class="sxs-lookup"><span data-stu-id="2c18b-135">In hello body, include hello property `KeyType` as either `Primary` or `Secondary`.</span></span>  <span data-ttu-id="2c18b-136">Detta returnerar en URL som signerats av hello säker nyckel har angetts.</span><span class="sxs-lookup"><span data-stu-id="2c18b-136">This returns a URL signed by hello secure key specified.</span></span>

### <a name="restrict-incoming-ip-addresses"></a><span data-ttu-id="2c18b-137">Begränsa inkommande IP-adresser</span><span class="sxs-lookup"><span data-stu-id="2c18b-137">Restrict incoming IP addresses</span></span>

<span data-ttu-id="2c18b-138">Dessutom toohello signatur för delad åtkomst gärna toorestrict som anropar en logikapp endast från specifika klienter.</span><span class="sxs-lookup"><span data-stu-id="2c18b-138">In addition toohello Shared Access Signature, you may wish toorestrict calling a logic app only from specific clients.</span></span>  <span data-ttu-id="2c18b-139">Om du hanterar din slutpunkt via Azure API Management kan du till exempel begränsa hello logik app tooonly Godkänn begäran om hello när hello begäran kommer från hello API Management instans-IP-adress.</span><span class="sxs-lookup"><span data-stu-id="2c18b-139">For example, if you manage your endpoint through Azure API Management, you can restrict hello logic app tooonly accept hello request when hello request comes from hello API Management instance IP address.</span></span>

<span data-ttu-id="2c18b-140">Den här inställningen kan konfigureras i hello logik app-inställningar:</span><span class="sxs-lookup"><span data-stu-id="2c18b-140">This setting can be configured within hello logic app settings:</span></span>

1. <span data-ttu-id="2c18b-141">Öppna i hello Azure-portalen, hello logikappen som du vill tooadd IP-adressbegränsningar</span><span class="sxs-lookup"><span data-stu-id="2c18b-141">In hello Azure portal, open hello logic app you want tooadd IP address restrictions</span></span>
1. <span data-ttu-id="2c18b-142">Klicka på hello **konfiguration för behörighetskontroll** menyalternativet **inställningar**</span><span class="sxs-lookup"><span data-stu-id="2c18b-142">Click hello **Access control configuration** menu item under **Settings**</span></span>
1. <span data-ttu-id="2c18b-143">Ange hello lista över IP-adress adressintervall toobe accepteras av hello utlösare</span><span class="sxs-lookup"><span data-stu-id="2c18b-143">Specify hello list of IP address ranges toobe accepted by hello trigger</span></span>

<span data-ttu-id="2c18b-144">En giltig IP-adressintervall tar hello format `192.168.1.1/255`.</span><span class="sxs-lookup"><span data-stu-id="2c18b-144">A valid IP range takes hello format `192.168.1.1/255`.</span></span> <span data-ttu-id="2c18b-145">Om du vill hello logik app tooonly brand som en kapslad logikapp, väljer hello **andra logikappar** alternativet.</span><span class="sxs-lookup"><span data-stu-id="2c18b-145">If you want hello logic app tooonly fire as a nested logic app, select hello **Only other logic apps** option.</span></span> <span data-ttu-id="2c18b-146">Det här alternativet skriver en tom matris toohello resurs, vilket innebär att endast anrop från hello-tjänsten (överordnade logikappar) brand har.</span><span class="sxs-lookup"><span data-stu-id="2c18b-146">This option writes an empty array toohello resource, meaning only calls from hello service itself (parent logic apps) fire successfully.</span></span>

> [!NOTE]
> <span data-ttu-id="2c18b-147">Du kan fortfarande köra en logikapp med en utlösare för begäran via hello REST API / Management `/triggers/{triggerName}/run` oavsett IP.</span><span class="sxs-lookup"><span data-stu-id="2c18b-147">You can still run a logic app with a request trigger through hello REST API / Management `/triggers/{triggerName}/run` regardless of IP.</span></span> <span data-ttu-id="2c18b-148">Det här scenariot kräver autentisering mot hello Azure REST-API och alla händelser som visas i hello Azure granskningslogg.</span><span class="sxs-lookup"><span data-stu-id="2c18b-148">This scenario requires authentication against hello Azure REST API, and all events would appear in hello Azure Audit Log.</span></span> <span data-ttu-id="2c18b-149">Ange åtkomstkontroll principer därefter.</span><span class="sxs-lookup"><span data-stu-id="2c18b-149">Set access control policies accordingly.</span></span>

#### <a name="setting-ip-ranges-on-hello-resource-definition"></a><span data-ttu-id="2c18b-150">Ange IP-intervall på hello resursdefinitionen</span><span class="sxs-lookup"><span data-stu-id="2c18b-150">Setting IP ranges on hello resource definition</span></span>

<span data-ttu-id="2c18b-151">Om du använder en [Distributionsmall](logic-apps-create-deploy-template.md) tooautomate dina distributioner hello IP-adressintervall inställningar kan konfigureras på hello resource-mall.</span><span class="sxs-lookup"><span data-stu-id="2c18b-151">If you are using a [deployment template](logic-apps-create-deploy-template.md) tooautomate your deployments, hello IP range settings can be configured on hello resource template.</span></span>  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "triggers": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}

```

### <a name="adding-azure-active-directory-oauth-or-other-security"></a><span data-ttu-id="2c18b-152">Lägga till Azure Active Directory, OAuth eller annan säkerhet</span><span class="sxs-lookup"><span data-stu-id="2c18b-152">Adding Azure Active Directory, OAuth, or other security</span></span>

<span data-ttu-id="2c18b-153">tooadd mer auktorisering protokoll ovanpå en logikapp [Azure API Management](https://azure.microsoft.com/services/api-management/) erbjuder omfattande övervakning, säkerhet, principer och dokumentation för valfri slutpunkt med hello kapaciteten tooexpose en logikapp som en API.</span><span class="sxs-lookup"><span data-stu-id="2c18b-153">tooadd more authorization protocols on top of a logic app, [Azure API Management](https://azure.microsoft.com/services/api-management/) offers rich monitoring, security, policy, and documentation for any endpoint with hello capability tooexpose a logic app as an API.</span></span> <span data-ttu-id="2c18b-154">Azure API Management kan exponera en offentlig eller privat slutpunkt för hello logikapp, som kan använda Azure Active Directory, certifikat, OAuth eller andra säkerhetskrav.</span><span class="sxs-lookup"><span data-stu-id="2c18b-154">Azure API Management can expose a public or private endpoint for hello logic app, which could use Azure Active Directory, certificate, OAuth, or other security standards.</span></span> <span data-ttu-id="2c18b-155">När en begäran tas emot, vidarebefordrar Azure API Management hello begäran toohello logikapp (utför alla nödvändiga transformationer eller begränsningar relä).</span><span class="sxs-lookup"><span data-stu-id="2c18b-155">When a request is received, Azure API Management forwards hello request toohello logic app (performing any needed transformations or restrictions in-flight).</span></span> <span data-ttu-id="2c18b-156">Du kan använda hello inkommande IP-adressintervall med inställningarna på hello logik app tooonly kan hello logik app toobe utlöses från API-hantering.</span><span class="sxs-lookup"><span data-stu-id="2c18b-156">You can use hello incoming IP range settings on hello logic app tooonly allow hello logic app toobe triggered from API Management.</span></span>

## <a name="secure-access-toomanage-or-edit-logic-apps"></a><span data-ttu-id="2c18b-157">Säker åtkomst toomanage eller redigera logikappar</span><span class="sxs-lookup"><span data-stu-id="2c18b-157">Secure access toomanage or edit logic apps</span></span>

<span data-ttu-id="2c18b-158">Du kan begränsa åtkomst toomanagement åtgärder på en logikapp så att bara vissa användare eller grupper kan tooperform åtgärder på hello resurs.</span><span class="sxs-lookup"><span data-stu-id="2c18b-158">You can restrict access toomanagement operations on a logic app so that only specific users or groups are able tooperform operations on hello resource.</span></span> <span data-ttu-id="2c18b-159">Logikappar använda hello Azure [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md) funktion och kan anpassas med hello samma verktyg.</span><span class="sxs-lookup"><span data-stu-id="2c18b-159">Logic apps use hello Azure [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) feature, and can be customized with hello same tools.</span></span>  <span data-ttu-id="2c18b-160">Det finns några inbyggda roller som du kan tilldela medlemmar i din prenumeration tooas också:</span><span class="sxs-lookup"><span data-stu-id="2c18b-160">There are a few built-in roles you can assign members of your subscription tooas well:</span></span>

* <span data-ttu-id="2c18b-161">**Logik App deltagare** – ger åtkomst tooview, redigera och uppdatera en logikapp.</span><span class="sxs-lookup"><span data-stu-id="2c18b-161">**Logic App Contributor** - Provides access tooview, edit, and update a logic app.</span></span>  <span data-ttu-id="2c18b-162">Det går inte att ta bort hello resurs eller utföra administrativa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2c18b-162">Cannot remove hello resource or perform admin operations.</span></span>
* <span data-ttu-id="2c18b-163">**Logik App operatorn** – kan visa hello logikapp och kör historik och aktivera/inaktivera.</span><span class="sxs-lookup"><span data-stu-id="2c18b-163">**Logic App Operator** - Can view hello logic app and run history, and enable/disable.</span></span>  <span data-ttu-id="2c18b-164">Det går inte att redigera eller uppdatera hello definition.</span><span class="sxs-lookup"><span data-stu-id="2c18b-164">Cannot edit or update hello definition.</span></span>

<span data-ttu-id="2c18b-165">Du kan också använda [Azure Resource Lås](../azure-resource-manager/resource-group-lock-resources.md) tooprevent ändrar eller tar bort logikappar.</span><span class="sxs-lookup"><span data-stu-id="2c18b-165">You can also use [Azure Resource Lock](../azure-resource-manager/resource-group-lock-resources.md) tooprevent changing or deleting logic apps.</span></span> <span data-ttu-id="2c18b-166">Den här funktionen är värdefullt tooprevent produktionsresurser från ändringar eller borttagningar.</span><span class="sxs-lookup"><span data-stu-id="2c18b-166">This capability is valuable tooprevent production resources from changes or deletions.</span></span>

## <a name="secure-access-toocontents-of-hello-run-history"></a><span data-ttu-id="2c18b-167">Säker åtkomst toocontents av hello kör historik</span><span class="sxs-lookup"><span data-stu-id="2c18b-167">Secure access toocontents of hello run history</span></span>

<span data-ttu-id="2c18b-168">Du kan begränsa åtkomst toocontents av in- eller utdata från tidigare körs toospecific IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="2c18b-168">You can restrict access toocontents of inputs or outputs from previous runs toospecific IP address ranges.</span></span>  

<span data-ttu-id="2c18b-169">Alla data i ett arbetsflöde kör krypteras under överföring och i vila.</span><span class="sxs-lookup"><span data-stu-id="2c18b-169">All data within a workflow run is encrypted in transit and at rest.</span></span> <span data-ttu-id="2c18b-170">När en toorun samtalshistorik görs hello tjänsten autentiserar hello begäran och ger länkar toohello begäran och svar indata och utdata.</span><span class="sxs-lookup"><span data-stu-id="2c18b-170">When a call toorun history is made, hello service authenticates hello request and provides links toohello request and response inputs and outputs.</span></span> <span data-ttu-id="2c18b-171">Den här länken kan vara skyddad så att endast begäranden tooview innehåll från ett IP-adressintervall returnera hello innehållet.</span><span class="sxs-lookup"><span data-stu-id="2c18b-171">This link can be protected so only requests tooview content from a designated IP address range return hello contents.</span></span> <span data-ttu-id="2c18b-172">Du kan använda den här funktionen för ytterligare behörighet.</span><span class="sxs-lookup"><span data-stu-id="2c18b-172">You can use this capability for additional access control.</span></span> <span data-ttu-id="2c18b-173">Du kan även ange en IP-adress som `0.0.0.0` så ingen kan komma åt indata/utdata.</span><span class="sxs-lookup"><span data-stu-id="2c18b-173">You could even specify an IP address like `0.0.0.0` so no one could access inputs/outputs.</span></span> <span data-ttu-id="2c18b-174">Endast användare med administratörsbehörigheter kan ta bort den här begränsningen tillhandahåller hello möjligheten för 'just-in-time-åtkomst tooworkflow innehållet.</span><span class="sxs-lookup"><span data-stu-id="2c18b-174">Only someone with admin permissions could remove this restriction, providing hello possibility for 'just-in-time' access tooworkflow contents.</span></span>

<span data-ttu-id="2c18b-175">Den här inställningen kan konfigureras i hello resursinställningar för hello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="2c18b-175">This setting can be configured within hello resource settings of hello Azure portal:</span></span>

1. <span data-ttu-id="2c18b-176">Öppna i hello Azure-portalen, hello logikappen som du vill tooadd IP-adressbegränsningar</span><span class="sxs-lookup"><span data-stu-id="2c18b-176">In hello Azure portal, open hello logic app you want tooadd IP address restrictions</span></span>
1. <span data-ttu-id="2c18b-177">Klicka på hello **konfiguration för behörighetskontroll** menyalternativet **inställningar**</span><span class="sxs-lookup"><span data-stu-id="2c18b-177">Click hello **Access control configuration** menu item under **Settings**</span></span>
1. <span data-ttu-id="2c18b-178">Ange hello lista över IP-adressintervall för åtkomst toocontent</span><span class="sxs-lookup"><span data-stu-id="2c18b-178">Specify hello list of IP address ranges for access toocontent</span></span>

#### <a name="setting-ip-ranges-on-hello-resource-definition"></a><span data-ttu-id="2c18b-179">Ange IP-intervall på hello resursdefinitionen</span><span class="sxs-lookup"><span data-stu-id="2c18b-179">Setting IP ranges on hello resource definition</span></span>

<span data-ttu-id="2c18b-180">Om du använder en [Distributionsmall](logic-apps-create-deploy-template.md) tooautomate dina distributioner hello IP-adressintervall inställningar kan konfigureras på hello resource-mall.</span><span class="sxs-lookup"><span data-stu-id="2c18b-180">If you are using a [deployment template](logic-apps-create-deploy-template.md) tooautomate your deployments, hello IP range settings can be configured on hello resource template.</span></span>  

``` json
{
    "properties": {
        "definition": {
        },
        "parameters": {},
        "accessControl": {
            "contents": {
                "allowedCallerIpAddresses": [
                    {
                        "addressRange": "192.168.12.0/23"
                    },
                    {
                        "addressRange": "2001:0db8::/64"
                    }
                ]
            }
        }
    },
    "type": "Microsoft.Logic/workflows"
}
```

## <a name="secure-parameters-and-inputs-within-a-workflow"></a><span data-ttu-id="2c18b-181">Säker parametrar och indata i ett arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="2c18b-181">Secure parameters and inputs within a workflow</span></span>

<span data-ttu-id="2c18b-182">Du kanske vill tooparameterize vissa aspekter av en arbetsflödesdefinition för distribution i miljöer.</span><span class="sxs-lookup"><span data-stu-id="2c18b-182">You might want tooparameterize some aspects of a workflow definition for deployment across environments.</span></span> <span data-ttu-id="2c18b-183">Vissa parametrar kan också vara säkra parametrar som du inte vill tooappear när du redigerar ett arbetsflöde, till exempel en klient-ID och klienthemlighet för [Azure Active Directory-autentisering](../connectors/connectors-native-http.md#authentication) av en HTTP-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="2c18b-183">Also, some parameters might be secure parameters you don't want tooappear when editing a workflow, such as a client ID and client secret for [Azure Active Directory authentication](../connectors/connectors-native-http.md#authentication) of an HTTP action.</span></span>

### <a name="using-parameters-and-secure-parameters"></a><span data-ttu-id="2c18b-184">Med hjälp av parametrarna och säker</span><span class="sxs-lookup"><span data-stu-id="2c18b-184">Using parameters and secure parameters</span></span>

<span data-ttu-id="2c18b-185">tooaccess hello värdet för en resurs parameter vid körning, hello [språk i arbetsflödesdefinitionen](http://aka.ms/logicappsdocs) ger en `@parameters()` igen.</span><span class="sxs-lookup"><span data-stu-id="2c18b-185">tooaccess hello value of a resource parameter at runtime, hello [workflow definition language](http://aka.ms/logicappsdocs) provides a `@parameters()` operation.</span></span> <span data-ttu-id="2c18b-186">Du kan också [ange parametrar i hello Distributionsmall för resource](../azure-resource-manager/resource-group-authoring-templates.md#parameters).</span><span class="sxs-lookup"><span data-stu-id="2c18b-186">Also, you can [specify parameters in hello resource deployment template](../azure-resource-manager/resource-group-authoring-templates.md#parameters).</span></span> <span data-ttu-id="2c18b-187">Men om du anger hello parametertypen som `securestring`, hello parametern inte returneras med hello resten av hello resursdefinitionen och inte är tillgängliga genom att visa hello resurs efter distributionen.</span><span class="sxs-lookup"><span data-stu-id="2c18b-187">But if you specify hello parameter type as `securestring`, hello parameter won't be returned with hello rest of hello resource definition, and won't be accessible by viewing hello resource after deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="2c18b-188">Om parametern används i hello sidhuvud eller en begäran om kanske hello parametern visas genom att öppna hello kör historik och utgående HTTP-begäran.</span><span class="sxs-lookup"><span data-stu-id="2c18b-188">If your parameter is used in hello headers or body of a request, hello parameter might be visible by accessing hello run history and outgoing HTTP request.</span></span> <span data-ttu-id="2c18b-189">Se till att tooset principer för åtkomst till innehåll i enlighet med detta.</span><span class="sxs-lookup"><span data-stu-id="2c18b-189">Make sure tooset your content access policies accordingly.</span></span>
> <span data-ttu-id="2c18b-190">Auktorisering huvuden visas aldrig via in- eller utdata.</span><span class="sxs-lookup"><span data-stu-id="2c18b-190">Authorization headers are never visible through inputs or outputs.</span></span> <span data-ttu-id="2c18b-191">Så om hello hemlighet används det är hello hemlighet inte strängfält.</span><span class="sxs-lookup"><span data-stu-id="2c18b-191">So if hello secret is being used there, hello secret is not retrievable.</span></span>

#### <a name="resource-deployment-template-with-secrets"></a><span data-ttu-id="2c18b-192">Resurs-Distributionsmall med hemligheter</span><span class="sxs-lookup"><span data-stu-id="2c18b-192">Resource deployment template with secrets</span></span>

<span data-ttu-id="2c18b-193">hello följande exempel visas en distribution som refererar till en säker parameter av `secret` vid körning.</span><span class="sxs-lookup"><span data-stu-id="2c18b-193">hello following example shows a deployment that references a secure parameter of `secret` at runtime.</span></span> <span data-ttu-id="2c18b-194">Du kan ange hello värdet för hello i en separat parameterfilen `secret`, eller Använd [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) tooretrieve hemligheter på distribuera tid.</span><span class="sxs-lookup"><span data-stu-id="2c18b-194">In a separate parameters file, you could specify hello environment value for hello `secret`, or use [Azure Resource Manager KeyVault](../azure-resource-manager/resource-manager-keyvault-parameter.md) tooretrieve secrets at deploy time.</span></span>

``` json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "secretDeploymentParam": {
      "type": "securestring"
    }
  },
  "variables": {},
  "resources": [
    {
      "name": "secret-deploy",
      "type": "Microsoft.Logic/workflows",
      "location": "westus",
      "tags": {
        "displayName": "LogicApp"
      },
      "apiVersion": "2016-06-01",
      "properties": {
        "definition": {
          "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "actions": {
            "Call_External_API": {
              "type": "http",
              "inputs": {
                "headers": {
                  "Authorization": "@parameters('secret')"
                },
                "body": "This is hello request"
              },
              "runAfter": {}
            }
          },
          "parameters": {
            "secret": {
              "type": "SecureString"
            }
          },
          "triggers": {
            "manual": {
              "type": "Request",
              "kind": "Http",
              "inputs": {
                "schema": {}
              }
            }
          },
          "contentVersion": "1.0.0.0",
          "outputs": {}
        },
        "parameters": {
          "secret": {
            "value": "[parameters('secretDeploymentParam')]"
          }
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="secure-access-tooservices-receiving-requests-from-a-workflow"></a><span data-ttu-id="2c18b-195">Säker åtkomst tooservices tar emot förfrågningar från ett arbetsflöde</span><span class="sxs-lookup"><span data-stu-id="2c18b-195">Secure access tooservices receiving requests from a workflow</span></span>

<span data-ttu-id="2c18b-196">Det finns många sätt toohelp säker logikapp någon slutpunkt hello måste tooaccess.</span><span class="sxs-lookup"><span data-stu-id="2c18b-196">There are many ways toohelp secure any endpoint hello logic app needs tooaccess.</span></span>

### <a name="using-authentication-on-outbound-requests"></a><span data-ttu-id="2c18b-197">Med autentisering för utgående förfrågningar</span><span class="sxs-lookup"><span data-stu-id="2c18b-197">Using authentication on outbound requests</span></span>

<span data-ttu-id="2c18b-198">När du arbetar med en HTTP-, http- + Swagger (Öppna API) eller Webhook åtgärd kan du lägga till toohello autentiseringsbegäran skickas.</span><span class="sxs-lookup"><span data-stu-id="2c18b-198">When working with an HTTP, HTTP + Swagger (Open API), or Webhook action, you can add authentication toohello request being sent.</span></span> <span data-ttu-id="2c18b-199">Du kan inkludera grundläggande autentisering eller autentisering med datorcertifikat Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="2c18b-199">You could include basic authentication, certificate authentication, or Azure Active Directory authentication.</span></span> <span data-ttu-id="2c18b-200">Information om hur tooconfigure den här autentiseringen kan hittas [i den här artikeln](../connectors/connectors-native-http.md#authentication).</span><span class="sxs-lookup"><span data-stu-id="2c18b-200">Details on how tooconfigure this authentication can be found [in this article](../connectors/connectors-native-http.md#authentication).</span></span>

### <a name="restricting-access-toologic-app-ip-addresses"></a><span data-ttu-id="2c18b-201">Begränsa åtkomst toologic app IP-adresser</span><span class="sxs-lookup"><span data-stu-id="2c18b-201">Restricting access toologic app IP addresses</span></span>

<span data-ttu-id="2c18b-202">Alla anrop från logikappar komma från en specifik uppsättning IP-adresser per region.</span><span class="sxs-lookup"><span data-stu-id="2c18b-202">All calls from logic apps come from a specific set of IP addresses per region.</span></span> <span data-ttu-id="2c18b-203">Du kan lägga till ytterligare filtrering tooonly godkänna begäranden från de som anges av IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="2c18b-203">You can add additional filtering tooonly accept requests from those designated IP addresses.</span></span> <span data-ttu-id="2c18b-204">En lista över de IP-adresserna, se [logik app gränser och konfiguration](logic-apps-limits-and-config.md#configuration).</span><span class="sxs-lookup"><span data-stu-id="2c18b-204">For a list of those IP addresses, see [logic app limits and configuration](logic-apps-limits-and-config.md#configuration).</span></span>

### <a name="on-premises-connectivity"></a><span data-ttu-id="2c18b-205">Lokala anslutningsmöjligheter</span><span class="sxs-lookup"><span data-stu-id="2c18b-205">On-premises connectivity</span></span>

<span data-ttu-id="2c18b-206">Logikappar ange integrering med flera tjänster tooprovide säkert och tillförlitligt lokalt kommunikation.</span><span class="sxs-lookup"><span data-stu-id="2c18b-206">Logic apps provide integration with several services tooprovide secure and reliable on-premises communication.</span></span>

#### <a name="on-premises-data-gateway"></a><span data-ttu-id="2c18b-207">Lokal datagateway</span><span class="sxs-lookup"><span data-stu-id="2c18b-207">On-premises data gateway</span></span>

<span data-ttu-id="2c18b-208">Många hanterade anslutningar för logic apps ger säker anslutning tooon lokalt system, inklusive filsystem, SQL, SharePoint, DB2 och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="2c18b-208">Many managed connectors for logic apps provide secure connectivity tooon-premises systems, including File System, SQL, SharePoint, DB2, and more.</span></span> <span data-ttu-id="2c18b-209">hello gateway vidarebefordrar data från lokala datakällor på krypterade kanaler via hello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="2c18b-209">hello gateway relays data from on-premises sources on encrypted channels through hello Azure Service Bus.</span></span> <span data-ttu-id="2c18b-210">All trafik som kommer från som säkra utgående trafik från hello gateway agent.</span><span class="sxs-lookup"><span data-stu-id="2c18b-210">All traffic originates as secure outbound traffic from hello gateway agent.</span></span> <span data-ttu-id="2c18b-211">Lär dig mer om [hur hello datagateway fungerar](logic-apps-gateway-install.md#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="2c18b-211">Learn more about [how hello data gateway works](logic-apps-gateway-install.md#gateway-cloud-service).</span></span>

#### <a name="azure-api-management"></a><span data-ttu-id="2c18b-212">Azure API Management</span><span class="sxs-lookup"><span data-stu-id="2c18b-212">Azure API Management</span></span>

<span data-ttu-id="2c18b-213">[Azure API Management](https://azure.microsoft.com/services/api-management/) har lokala anslutningsmöjligheter, inklusive plats-till-plats VPN- och ExpressRoute-integrering för säker kommunikation för proxy och tooon lokala system.</span><span class="sxs-lookup"><span data-stu-id="2c18b-213">[Azure API Management](https://azure.microsoft.com/services/api-management/) has on-premises connectivity options, including site-to-site VPN and ExpressRoute integration for secured proxy and communication tooon-premises systems.</span></span> <span data-ttu-id="2c18b-214">Du kan snabbt markera en API exponeras från Azure API Management i ett arbetsflöde, vilket ger snabb åtkomst tooon lokala system i hello logik App Designer.</span><span class="sxs-lookup"><span data-stu-id="2c18b-214">In hello Logic App Designer, you can quickly select an API exposed from Azure API Management within a workflow, providing quick access tooon-premises systems.</span></span>

#### <a name="hybrid-connections-from-azure-app-service"></a><span data-ttu-id="2c18b-215">Hybridanslutningar från Azure App Service</span><span class="sxs-lookup"><span data-stu-id="2c18b-215">Hybrid connections from Azure App Service</span></span>

<span data-ttu-id="2c18b-216">Du kan använda hello lokalt hybrid anslutning funktionen för Azure API och Web apps toocommunicate lokalt.</span><span class="sxs-lookup"><span data-stu-id="2c18b-216">You can use hello on-premises hybrid connection feature for Azure API and Web apps toocommunicate on-premises.</span></span>  <span data-ttu-id="2c18b-217">Information om hybridanslutningar och hur du kan hitta tooconfigure [i den här artikeln](../app-service-web/web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="2c18b-217">Details on hybrid connections and how tooconfigure can be found [in this article](../app-service-web/web-sites-hybrid-connection-get-started.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c18b-218">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2c18b-218">Next steps</span></span>
[<span data-ttu-id="2c18b-219">Skapa en Distributionsmall för</span><span class="sxs-lookup"><span data-stu-id="2c18b-219">Create a deployment template</span></span>](logic-apps-create-deploy-template.md)  
[<span data-ttu-id="2c18b-220">Undantagshantering</span><span class="sxs-lookup"><span data-stu-id="2c18b-220">Exception handling</span></span>](logic-apps-exception-handling.md)  
[<span data-ttu-id="2c18b-221">Övervaka dina logikappar</span><span class="sxs-lookup"><span data-stu-id="2c18b-221">Monitor your logic apps</span></span>](logic-apps-monitor-your-logic-apps.md)  
[<span data-ttu-id="2c18b-222">Diagnostisera logik app fel och problem</span><span class="sxs-lookup"><span data-stu-id="2c18b-222">Diagnosing logic app failures and issues</span></span>](logic-apps-diagnosing-failures.md)  
