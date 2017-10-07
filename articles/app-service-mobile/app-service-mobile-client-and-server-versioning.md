---
title: aaaClient och server SDK versionshantering i Mobile Apps och Mobile Services | Microsoft Docs
description: "Lista över klient-SDK: er och kompatibilitet med server SDK-versioner för Mobile Services och Azure Mobile Apps"
services: app-service\mobile
documentationcenter: 
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 35b19672-c9d6-49b5-b405-a6dcd1107cd5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: 5874b7455ea407ca8c77fb1bd03d97d0767ebb47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a><span data-ttu-id="a64e4-103">Klienten och servern versionshantering i Mobile Apps och Mobile Services</span><span class="sxs-lookup"><span data-stu-id="a64e4-103">Client and server versioning in Mobile Apps and Mobile Services</span></span>
<span data-ttu-id="a64e4-104">hello senaste versionen av Azure Mobile Services är hello **Mobile Apps** funktion i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="a64e4-104">hello latest version of Azure Mobile Services is hello **Mobile Apps** feature of Azure App Service.</span></span>

<span data-ttu-id="a64e4-105">Hej Mobilappar klient och server SDK ursprungligen baseras på de i Mobile Services, men de är *inte* kompatibla med varandra.</span><span class="sxs-lookup"><span data-stu-id="a64e4-105">hello Mobile Apps client and server SDKs are originally based on those in Mobile Services, but they are *not* compatible with each other.</span></span>
<span data-ttu-id="a64e4-106">Det vill säga måste du använda en *Mobilappar* klient-SDK med en *Mobilappar* server-SDK och på samma sätt för *Mobile Services*.</span><span class="sxs-lookup"><span data-stu-id="a64e4-106">That is, you must use a *Mobile Apps* client SDK with a *Mobile Apps* server SDK and similarly for *Mobile Services*.</span></span> <span data-ttu-id="a64e4-107">Det här kontraktet tillämpas via en särskild huvudvärde används av hello klient och server SDK: er, `ZUMO-API-VERSION`.</span><span class="sxs-lookup"><span data-stu-id="a64e4-107">This contract is enforced through a special header value used by hello client and server SDKs, `ZUMO-API-VERSION`.</span></span>

<span data-ttu-id="a64e4-108">Obs: när det här dokumentet refererar tooa *Mobile Services* serverdel det inte nödvändigtvis behöver toobe finns i Mobile Services.</span><span class="sxs-lookup"><span data-stu-id="a64e4-108">Note: whenever this document refers tooa *Mobile Services* backend, it does not necessarily need toobe hosted on Mobile Services.</span></span> <span data-ttu-id="a64e4-109">Det är nu möjligt toomigrate en Mobiltjänst toorun på Apptjänst utan någon kodändringar men hello tjänsten skulle fortfarande använder *Mobile Services* SDK-versioner.</span><span class="sxs-lookup"><span data-stu-id="a64e4-109">It is now possible toomigrate a mobile service toorun on App Service without any code changes, but hello service would still be using *Mobile Services*  SDK versions.</span></span>

<span data-ttu-id="a64e4-110">toolearn mer om migrera tooApp tjänsten utan ändringar koden finns hello artikel [migrera en Mobiltjänst tooAzure Apptjänst].</span><span class="sxs-lookup"><span data-stu-id="a64e4-110">toolearn more about migrating tooApp Service without any code changes, see hello article [Migrate a Mobile Service tooAzure App Service].</span></span>

## <a name="header-specification"></a><span data-ttu-id="a64e4-111">Specifikation för sidhuvud</span><span class="sxs-lookup"><span data-stu-id="a64e4-111">Header specification</span></span>
<span data-ttu-id="a64e4-112">hello nyckeln `ZUMO-API-VERSION` kan anges i hello HTTP-sidhuvud eller frågesträng hello.</span><span class="sxs-lookup"><span data-stu-id="a64e4-112">hello key `ZUMO-API-VERSION` may be specified in either hello HTTP header or hello query string.</span></span> <span data-ttu-id="a64e4-113">hello-värdet är en versionssträng i form av hello **x.y.z**.</span><span class="sxs-lookup"><span data-stu-id="a64e4-113">hello value is a version string in hello form **x.y.z**.</span></span>

<span data-ttu-id="a64e4-114">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a64e4-114">For example:</span></span>

<span data-ttu-id="a64e4-115">Hämta https://service.azurewebsites.net/tables/TodoItem</span><span class="sxs-lookup"><span data-stu-id="a64e4-115">GET https://service.azurewebsites.net/tables/TodoItem</span></span>

<span data-ttu-id="a64e4-116">HUVUDEN: ZUMO-API-VERSION: 2.0.0</span><span class="sxs-lookup"><span data-stu-id="a64e4-116">HEADERS: ZUMO-API-VERSION: 2.0.0</span></span>

<span data-ttu-id="a64e4-117">Bokför https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0</span><span class="sxs-lookup"><span data-stu-id="a64e4-117">POST https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0</span></span>

## <a name="opting-out-of-version-checking"></a><span data-ttu-id="a64e4-118">Väljer bort versionskontroll</span><span class="sxs-lookup"><span data-stu-id="a64e4-118">Opting out of version checking</span></span>
<span data-ttu-id="a64e4-119">Du kan välja bort versionskontroll genom att ange ett värde för **SANT** för hello appinställningen **MS_SkipVersionCheck**.</span><span class="sxs-lookup"><span data-stu-id="a64e4-119">You can opt out of version checking by setting a value of **true** for hello app setting **MS_SkipVersionCheck**.</span></span> <span data-ttu-id="a64e4-120">Ange det i filen web.config eller i hello delen programinställningar i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a64e4-120">Specify this either in your web.config or in hello Application Settings section of hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="a64e4-121">Det finns ett antal funktionsändringar mellan Mobile Services och Mobilappar, särskilt i områden som hello offlinesynkronisering, autentisering och push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a64e4-121">There are a number of behavior changes between Mobile Services and Mobile Apps, particularly in hello areas of offline sync, authentication, and push notifications.</span></span> <span data-ttu-id="a64e4-122">Du bör endast välja bort versionskontroll efter slutförd tester tooensure att dessa förändringar i beteendet inte bryter appens funktioner.</span><span class="sxs-lookup"><span data-stu-id="a64e4-122">You should only opt out of version checking after complete testing tooensure that these behavioral changes do not break your app's functionality.</span></span>
>
>

## <a name="summary-of-compatibility-for-all-versions"></a><span data-ttu-id="a64e4-123">Sammanfattning av kompatibilitet för alla versioner</span><span class="sxs-lookup"><span data-stu-id="a64e4-123">Summary of compatibility for all versions</span></span>
<span data-ttu-id="a64e4-124">hello diagrammet nedan visar hello kompatibilitet mellan alla typer av klienten och servern.</span><span class="sxs-lookup"><span data-stu-id="a64e4-124">hello chart below shows hello compatibility between all client and server types.</span></span> <span data-ttu-id="a64e4-125">En serverdel klassificeras som antingen Mobile **Services** eller Mobile **appar** baserat på server-hello SDK som används.</span><span class="sxs-lookup"><span data-stu-id="a64e4-125">A backend is classified as either Mobile **Services** or Mobile **Apps** based on hello server SDK that it uses.</span></span>

|  | <span data-ttu-id="a64e4-126">**Mobiltjänster** Node.js eller .NET</span><span class="sxs-lookup"><span data-stu-id="a64e4-126">**Mobile Services** Node.js or .NET</span></span> | <span data-ttu-id="a64e4-127">**Mobilappar** Node.js eller .NET</span><span class="sxs-lookup"><span data-stu-id="a64e4-127">**Mobile Apps** Node.js or .NET</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a64e4-128">[Klienter för Mobile Services]</span><span class="sxs-lookup"><span data-stu-id="a64e4-128">[Mobile Services clients]</span></span> |<span data-ttu-id="a64e4-129">Okej</span><span class="sxs-lookup"><span data-stu-id="a64e4-129">Ok</span></span> |<span data-ttu-id="a64e4-130">Fel\*</span><span class="sxs-lookup"><span data-stu-id="a64e4-130">Error\*</span></span> |
| <span data-ttu-id="a64e4-131">[Mobile Apps-klienter]</span><span class="sxs-lookup"><span data-stu-id="a64e4-131">[Mobile Apps clients]</span></span> |<span data-ttu-id="a64e4-132">Fel\*</span><span class="sxs-lookup"><span data-stu-id="a64e4-132">Error\*</span></span> |<span data-ttu-id="a64e4-133">Okej</span><span class="sxs-lookup"><span data-stu-id="a64e4-133">Ok</span></span> |

<span data-ttu-id="a64e4-134">\*Detta kan kontrolleras genom att ange **MS_SkipVersionCheck**.</span><span class="sxs-lookup"><span data-stu-id="a64e4-134">\*This can be controlled by specifying **MS_SkipVersionCheck**.</span></span>

<!-- IMPORTANT!  hello anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: hello fwlink toothis document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <span data-ttu-id="a64e4-135"><a name="1.0.0"></a>Mobile Services-klienten och servern</span><span class="sxs-lookup"><span data-stu-id="a64e4-135"><a name="1.0.0"></a>Mobile Services client and server</span></span>
<span data-ttu-id="a64e4-136">hello klienten SDK: er i hello nedan är kompatibla med **Mobile Services**.</span><span class="sxs-lookup"><span data-stu-id="a64e4-136">hello client SDKs in hello table below are compatible with **Mobile Services**.</span></span>

<span data-ttu-id="a64e4-137">Obs: hello Mobile Services klient-SDK: *inte* skicka ett värde för `ZUMO-API-VERSION`.</span><span class="sxs-lookup"><span data-stu-id="a64e4-137">Note: hello Mobile Services client SDKs *do not* send a header value for `ZUMO-API-VERSION`.</span></span> <span data-ttu-id="a64e4-138">Om tjänsten för hello får det här sidhuvudet eller frågesträngsvärdet ett fel returneras om du uttryckligen har valt ut enligt beskrivningen ovan.</span><span class="sxs-lookup"><span data-stu-id="a64e4-138">If hello service receives this header or query string value, an error will be returned, unless you have explicitly opted out as described above.</span></span>

### <span data-ttu-id="a64e4-139"><a name="MobileServicesClients"></a>Mobila *Services* client SDK</span><span class="sxs-lookup"><span data-stu-id="a64e4-139"><a name="MobileServicesClients"></a> Mobile *Services* client SDKs</span></span>
| <span data-ttu-id="a64e4-140">Klientplattform</span><span class="sxs-lookup"><span data-stu-id="a64e4-140">Client platform</span></span> | <span data-ttu-id="a64e4-141">Version</span><span class="sxs-lookup"><span data-stu-id="a64e4-141">Version</span></span> | <span data-ttu-id="a64e4-142">Värdet för versionshuvudet</span><span class="sxs-lookup"><span data-stu-id="a64e4-142">Version header value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a64e4-143">Hanterad klient (Windows, Xamarin)</span><span class="sxs-lookup"><span data-stu-id="a64e4-143">Managed client (Windows, Xamarin)</span></span> |[<span data-ttu-id="a64e4-144">1.3.2</span><span class="sxs-lookup"><span data-stu-id="a64e4-144">1.3.2</span></span>](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) |<span data-ttu-id="a64e4-145">Saknas</span><span class="sxs-lookup"><span data-stu-id="a64e4-145">n/a</span></span> |
| <span data-ttu-id="a64e4-146">iOS</span><span class="sxs-lookup"><span data-stu-id="a64e4-146">iOS</span></span> |[<span data-ttu-id="a64e4-147">2.2.2</span><span class="sxs-lookup"><span data-stu-id="a64e4-147">2.2.2</span></span>](http://aka.ms/gc6fex) |<span data-ttu-id="a64e4-148">Saknas</span><span class="sxs-lookup"><span data-stu-id="a64e4-148">n/a</span></span> |
| <span data-ttu-id="a64e4-149">Android</span><span class="sxs-lookup"><span data-stu-id="a64e4-149">Android</span></span> |[<span data-ttu-id="a64e4-150">2.0.3</span><span class="sxs-lookup"><span data-stu-id="a64e4-150">2.0.3</span></span>](https://go.microsoft.com/fwLink/?LinkID=280126) |<span data-ttu-id="a64e4-151">Saknas</span><span class="sxs-lookup"><span data-stu-id="a64e4-151">n/a</span></span> |
| <span data-ttu-id="a64e4-152">HTML</span><span class="sxs-lookup"><span data-stu-id="a64e4-152">HTML</span></span> |[<span data-ttu-id="a64e4-153">1.2.7</span><span class="sxs-lookup"><span data-stu-id="a64e4-153">1.2.7</span></span>](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) |<span data-ttu-id="a64e4-154">Saknas</span><span class="sxs-lookup"><span data-stu-id="a64e4-154">n/a</span></span> |

### <a name="mobile-services-server-sdks"></a><span data-ttu-id="a64e4-155">Mobila *Services* server SDK</span><span class="sxs-lookup"><span data-stu-id="a64e4-155">Mobile *Services* server SDKs</span></span>
| <span data-ttu-id="a64e4-156">Server-plattformen</span><span class="sxs-lookup"><span data-stu-id="a64e4-156">Server platform</span></span> | <span data-ttu-id="a64e4-157">Version</span><span class="sxs-lookup"><span data-stu-id="a64e4-157">Version</span></span> | <span data-ttu-id="a64e4-158">Godkänd version-huvud</span><span class="sxs-lookup"><span data-stu-id="a64e4-158">Accepted version header</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a64e4-159">.NET</span><span class="sxs-lookup"><span data-stu-id="a64e4-159">.NET</span></span> |[<span data-ttu-id="a64e4-160">WindowsAzure.MobileServices.Backend.* Version 1.0.x</span><span class="sxs-lookup"><span data-stu-id="a64e4-160">WindowsAzure.MobileServices.Backend.* Version 1.0.x</span></span>](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) |<span data-ttu-id="a64e4-161">** Inga versionshuvud **</span><span class="sxs-lookup"><span data-stu-id="a64e4-161">**No version header **</span></span> |
| <span data-ttu-id="a64e4-162">Node.js</span><span class="sxs-lookup"><span data-stu-id="a64e4-162">Node.js</span></span> |<span data-ttu-id="a64e4-163">(kommer snart)</span><span class="sxs-lookup"><span data-stu-id="a64e4-163">(coming soon)</span></span> |<span data-ttu-id="a64e4-164">**Ingen version-huvud**</span><span class="sxs-lookup"><span data-stu-id="a64e4-164">**No version header**</span></span> |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a><span data-ttu-id="a64e4-165">Beteendet för Mobile Services serverdelar</span><span class="sxs-lookup"><span data-stu-id="a64e4-165">Behavior of Mobile Services backends</span></span>
| <span data-ttu-id="a64e4-166">ZUMO-API-VERSION</span><span class="sxs-lookup"><span data-stu-id="a64e4-166">ZUMO-API-VERSION</span></span> | <span data-ttu-id="a64e4-167">Värdet för MS_SkipVersionCheck</span><span class="sxs-lookup"><span data-stu-id="a64e4-167">Value of MS_SkipVersionCheck</span></span> | <span data-ttu-id="a64e4-168">Svar</span><span class="sxs-lookup"><span data-stu-id="a64e4-168">Response</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a64e4-169">Inte angetts</span><span class="sxs-lookup"><span data-stu-id="a64e4-169">Not specified</span></span> |<span data-ttu-id="a64e4-170">Alla</span><span class="sxs-lookup"><span data-stu-id="a64e4-170">Any</span></span> |<span data-ttu-id="a64e4-171">200 - OK</span><span class="sxs-lookup"><span data-stu-id="a64e4-171">200 - OK</span></span> |
| <span data-ttu-id="a64e4-172">Inget värde</span><span class="sxs-lookup"><span data-stu-id="a64e4-172">Any value</span></span> |<span data-ttu-id="a64e4-173">True</span><span class="sxs-lookup"><span data-stu-id="a64e4-173">True</span></span> |<span data-ttu-id="a64e4-174">200 - OK</span><span class="sxs-lookup"><span data-stu-id="a64e4-174">200 - OK</span></span> |
| <span data-ttu-id="a64e4-175">Inget värde</span><span class="sxs-lookup"><span data-stu-id="a64e4-175">Any value</span></span> |<span data-ttu-id="a64e4-176">FALSKT/inte angetts</span><span class="sxs-lookup"><span data-stu-id="a64e4-176">False/Not Specified</span></span> |<span data-ttu-id="a64e4-177">400 – Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a64e4-177">400 - Bad Request</span></span> |

## <span data-ttu-id="a64e4-178"><a name="2.0.0"></a>Azure Mobile Apps klient och server</span><span class="sxs-lookup"><span data-stu-id="a64e4-178"><a name="2.0.0"></a>Azure Mobile Apps client and server</span></span>
### <span data-ttu-id="a64e4-179"><a name="MobileAppsClients"></a>Mobila *appar* client SDK</span><span class="sxs-lookup"><span data-stu-id="a64e4-179"><a name="MobileAppsClients"></a> Mobile *Apps* client SDKs</span></span>
<span data-ttu-id="a64e4-180">Versionskontroll introducerades med hello följande versioner av hello klient-SDK för **Azure Mobile Apps**:</span><span class="sxs-lookup"><span data-stu-id="a64e4-180">Version checking was introduced starting with hello following versions of hello client SDK for **Azure Mobile Apps**:</span></span>

| <span data-ttu-id="a64e4-181">Klientplattform</span><span class="sxs-lookup"><span data-stu-id="a64e4-181">Client platform</span></span> | <span data-ttu-id="a64e4-182">Version</span><span class="sxs-lookup"><span data-stu-id="a64e4-182">Version</span></span> | <span data-ttu-id="a64e4-183">Värdet för versionshuvudet</span><span class="sxs-lookup"><span data-stu-id="a64e4-183">Version header value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a64e4-184">Hanterad klient (Windows, Xamarin)</span><span class="sxs-lookup"><span data-stu-id="a64e4-184">Managed client (Windows, Xamarin)</span></span> |[<span data-ttu-id="a64e4-185">2.0.0</span><span class="sxs-lookup"><span data-stu-id="a64e4-185">2.0.0</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) |<span data-ttu-id="a64e4-186">2.0.0</span><span class="sxs-lookup"><span data-stu-id="a64e4-186">2.0.0</span></span> |
| <span data-ttu-id="a64e4-187">iOS</span><span class="sxs-lookup"><span data-stu-id="a64e4-187">iOS</span></span> |[<span data-ttu-id="a64e4-188">3.0.0</span><span class="sxs-lookup"><span data-stu-id="a64e4-188">3.0.0</span></span>](http://go.microsoft.com/fwlink/?LinkID=529823) |<span data-ttu-id="a64e4-189">2.0.0</span><span class="sxs-lookup"><span data-stu-id="a64e4-189">2.0.0</span></span> |
| <span data-ttu-id="a64e4-190">Android</span><span class="sxs-lookup"><span data-stu-id="a64e4-190">Android</span></span> |[<span data-ttu-id="a64e4-191">3.0.0</span><span class="sxs-lookup"><span data-stu-id="a64e4-191">3.0.0</span></span>](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) |<span data-ttu-id="a64e4-192">3.0.0</span><span class="sxs-lookup"><span data-stu-id="a64e4-192">3.0.0</span></span> |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a><span data-ttu-id="a64e4-193">Mobila *appar* server SDK</span><span class="sxs-lookup"><span data-stu-id="a64e4-193">Mobile *Apps* server SDKs</span></span>
<span data-ttu-id="a64e4-194">Versionskontroll ingår i följande versioner av server-SDK:</span><span class="sxs-lookup"><span data-stu-id="a64e4-194">Version checking is included in following server SDK versions:</span></span>

| <span data-ttu-id="a64e4-195">Server-plattformen</span><span class="sxs-lookup"><span data-stu-id="a64e4-195">Server platform</span></span> | <span data-ttu-id="a64e4-196">SDK</span><span class="sxs-lookup"><span data-stu-id="a64e4-196">SDK</span></span> | <span data-ttu-id="a64e4-197">Godkänd version-huvud</span><span class="sxs-lookup"><span data-stu-id="a64e4-197">Accepted version header</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a64e4-198">.NET</span><span class="sxs-lookup"><span data-stu-id="a64e4-198">.NET</span></span> |[<span data-ttu-id="a64e4-199">Microsoft.Azure.Mobile.Server</span><span class="sxs-lookup"><span data-stu-id="a64e4-199">Microsoft.Azure.Mobile.Server</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) |<span data-ttu-id="a64e4-200">2.0.0</span><span class="sxs-lookup"><span data-stu-id="a64e4-200">2.0.0</span></span> |
| <span data-ttu-id="a64e4-201">Node.js</span><span class="sxs-lookup"><span data-stu-id="a64e4-201">Node.js</span></span> |[<span data-ttu-id="a64e4-202">Azure mobile apps)</span><span class="sxs-lookup"><span data-stu-id="a64e4-202">azure-mobile-apps)</span></span>](https://www.npmjs.com/package/azure-mobile-apps) |<span data-ttu-id="a64e4-203">2.0.0</span><span class="sxs-lookup"><span data-stu-id="a64e4-203">2.0.0</span></span> |

### <a name="behavior-of-mobile-apps-backends"></a><span data-ttu-id="a64e4-204">Beteendet för serverdelar för Mobilappar</span><span class="sxs-lookup"><span data-stu-id="a64e4-204">Behavior of Mobile Apps backends</span></span>
| <span data-ttu-id="a64e4-205">ZUMO-API-VERSION</span><span class="sxs-lookup"><span data-stu-id="a64e4-205">ZUMO-API-VERSION</span></span> | <span data-ttu-id="a64e4-206">Värdet för MS_SkipVersionCheck</span><span class="sxs-lookup"><span data-stu-id="a64e4-206">Value of MS_SkipVersionCheck</span></span> | <span data-ttu-id="a64e4-207">Svar</span><span class="sxs-lookup"><span data-stu-id="a64e4-207">Response</span></span> |
| --- | --- | --- |
| <span data-ttu-id="a64e4-208">x.y.z eller Null</span><span class="sxs-lookup"><span data-stu-id="a64e4-208">x.y.z or Null</span></span> |<span data-ttu-id="a64e4-209">True</span><span class="sxs-lookup"><span data-stu-id="a64e4-209">True</span></span> |<span data-ttu-id="a64e4-210">200 - OK</span><span class="sxs-lookup"><span data-stu-id="a64e4-210">200 - OK</span></span> |
| <span data-ttu-id="a64e4-211">Null</span><span class="sxs-lookup"><span data-stu-id="a64e4-211">Null</span></span> |<span data-ttu-id="a64e4-212">FALSKT/inte angetts</span><span class="sxs-lookup"><span data-stu-id="a64e4-212">False/Not Specified</span></span> |<span data-ttu-id="a64e4-213">400 – Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a64e4-213">400 - Bad Request</span></span> |
| <span data-ttu-id="a64e4-214">1.x.y</span><span class="sxs-lookup"><span data-stu-id="a64e4-214">1.x.y</span></span> |<span data-ttu-id="a64e4-215">FALSKT/inte angetts</span><span class="sxs-lookup"><span data-stu-id="a64e4-215">False/Not Specified</span></span> |<span data-ttu-id="a64e4-216">400 – Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a64e4-216">400 - Bad Request</span></span> |
| <span data-ttu-id="a64e4-217">2.0.0-2.x.y</span><span class="sxs-lookup"><span data-stu-id="a64e4-217">2.0.0-2.x.y</span></span> |<span data-ttu-id="a64e4-218">FALSKT/inte angetts</span><span class="sxs-lookup"><span data-stu-id="a64e4-218">False/Not Specified</span></span> |<span data-ttu-id="a64e4-219">200 - OK</span><span class="sxs-lookup"><span data-stu-id="a64e4-219">200 - OK</span></span> |
| <span data-ttu-id="a64e4-220">3.0.0-3.x.y</span><span class="sxs-lookup"><span data-stu-id="a64e4-220">3.0.0-3.x.y</span></span> |<span data-ttu-id="a64e4-221">FALSKT/inte angetts</span><span class="sxs-lookup"><span data-stu-id="a64e4-221">False/Not Specified</span></span> |<span data-ttu-id="a64e4-222">400 – Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="a64e4-222">400 - Bad Request</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a64e4-223">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a64e4-223">Next Steps</span></span>
* <span data-ttu-id="a64e4-224">[migrera en Mobiltjänst tooAzure Apptjänst]</span><span class="sxs-lookup"><span data-stu-id="a64e4-224">[Migrate a Mobile Service tooAzure App Service]</span></span>

[Klienter för Mobile Services]: #MobileServicesClients
[Mobile Apps-klienter]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[migrera en Mobiltjänst tooAzure Apptjänst]: app-service-mobile-migrating-from-mobile-services.md
