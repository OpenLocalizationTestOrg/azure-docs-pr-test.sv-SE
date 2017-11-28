---
title: "aaaConfigure Azure funktionen App-inställningar | Microsoft Docs"
description: "Lär dig hur tooconfigure Azure fungerar app-inställningar."
services: 
documentationcenter: .net
author: rachelappel
manager: erikre
editor: 
ms.assetid: 81eb04f8-9a27-45bb-bf24-9ab6c30d205c
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: dotnet
ms.devlang: na
ms.topic: article
ms.date: 04/23/2017
ms.author: glenga
ms.openlocfilehash: 539e203ac449061ef3ceae5e93df3bdbb326e43b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-a-function-app-in-hello-azure-portal"></a><span data-ttu-id="c04d5-103">Hur toomanage en funktionsapp i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c04d5-103">How toomanage a function app in hello Azure portal</span></span> 

<span data-ttu-id="c04d5-104">I Azure Functions ger en funktionsapp hello körningskontexten för dina enskilda funktioner.</span><span class="sxs-lookup"><span data-stu-id="c04d5-104">In Azure Functions, a function app provides hello execution context for your individual functions.</span></span> <span data-ttu-id="c04d5-105">Funktionen app beteenden gäller tooall funktioner hos en viss funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="c04d5-105">Function app behaviors apply tooall functions hosted by a given function app.</span></span> <span data-ttu-id="c04d5-106">Det här avsnittet beskrivs hur tooconfigure och hantera dina appar för funktionen i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c04d5-106">This topic describes how tooconfigure and manage your function apps in hello Azure portal.</span></span>

<span data-ttu-id="c04d5-107">toobegin, gå toohello [Azure-portalen](http://portal.azure.com) och logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c04d5-107">toobegin, go toohello [Azure portal](http://portal.azure.com) and sign in tooyour Azure account.</span></span> <span data-ttu-id="c04d5-108">Hello sökfältet hello överst i portalen hello hello typnamn för funktionen appen och markera den hello-listan.</span><span class="sxs-lookup"><span data-stu-id="c04d5-108">In hello search bar at hello top of hello portal, type hello name of your function app and select it from hello list.</span></span> <span data-ttu-id="c04d5-109">När du har valt appen funktionen, kan du se hello följande sida:</span><span class="sxs-lookup"><span data-stu-id="c04d5-109">After selecting your function app, you see hello following page:</span></span>

![Översikt över funktionen app i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <span data-ttu-id="c04d5-111"><a name="manage-app-service-settings"></a>Fliken för funktionen app-inställningar</span><span class="sxs-lookup"><span data-stu-id="c04d5-111"><a name="manage-app-service-settings"></a>Function app settings tab</span></span>

![Funktionen app översikt i hello Azure-portalen.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

<span data-ttu-id="c04d5-113">Hej **inställningar** är där du kan uppdatera hello funktioner körtidsversion som används av din funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="c04d5-113">hello **Settings** tab is where you can update hello Functions runtime version used by your function app.</span></span> <span data-ttu-id="c04d5-114">Det är också där du hanterar hello värd nycklar som används toorestrict HTTP-åtkomst tooall funktioner hos hello funktionsapp.</span><span class="sxs-lookup"><span data-stu-id="c04d5-114">It is also where you manage hello host keys used toorestrict HTTP access tooall functions hosted by hello function app.</span></span>

<span data-ttu-id="c04d5-115">Funktioner stöder både förbrukning värd och Apptjänst som är värd för planer.</span><span class="sxs-lookup"><span data-stu-id="c04d5-115">Functions supports both Consumption hosting and App Service hosting plans.</span></span> <span data-ttu-id="c04d5-116">Mer information finns i [Välj hello rätt service-plan för Azure Functions](functions-scale.md).</span><span class="sxs-lookup"><span data-stu-id="c04d5-116">For more information, see [Choose hello correct service plan for Azure Functions](functions-scale.md).</span></span> <span data-ttu-id="c04d5-117">Funktioner för bättre förutsägbarhet i hello förbrukning planen kan du begränsa användning av plattform genom att ange en daglig användning kvot i GB-sekunder.</span><span class="sxs-lookup"><span data-stu-id="c04d5-117">For better predictability in hello Consumption plan, Functions lets you limit platform usage by setting a daily usage quota, in gigabytes-seconds.</span></span> <span data-ttu-id="c04d5-118">När hello dagliga kvot har nåtts har hello funktionsapp stoppats.</span><span class="sxs-lookup"><span data-stu-id="c04d5-118">Once hello daily usage quota is reached, hello function app is stopped.</span></span> <span data-ttu-id="c04d5-119">En funktionsapp stoppades på grund av når hello utgifter kvoten kan återaktiveras från hello samma kontext som upprätta hello dagligen utgifter kvoten.</span><span class="sxs-lookup"><span data-stu-id="c04d5-119">A function app stopped as a result of reaching hello spending quota can be re-enabled from hello same context as establishing hello daily spending quota.</span></span> <span data-ttu-id="c04d5-120">Se hello [Azure Functions sida med priser](http://azure.microsoft.com/pricing/details/functions/) information om fakturering.</span><span class="sxs-lookup"><span data-stu-id="c04d5-120">See hello [Azure Functions pricing page](http://azure.microsoft.com/pricing/details/functions/) for details on billing.</span></span>   

## <a name="platform-features-tab"></a><span data-ttu-id="c04d5-121">Plattform funktioner fliken</span><span class="sxs-lookup"><span data-stu-id="c04d5-121">Platform features tab</span></span>

![Funktionen app plattform funktioner fliken.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

<span data-ttu-id="c04d5-123">Funktionen appar körs i och underhålls av hello Azure App Service-plattformen.</span><span class="sxs-lookup"><span data-stu-id="c04d5-123">Function apps run in, and are maintained, by hello Azure App Service platform.</span></span> <span data-ttu-id="c04d5-124">Därför har funktionen-appar åtkomst toomost av hello funktionerna i Azures core webbvärd plattform.</span><span class="sxs-lookup"><span data-stu-id="c04d5-124">As such, your function apps have access toomost of hello features of Azure's core web hosting platform.</span></span> <span data-ttu-id="c04d5-125">Hej **plattformsfunktioner** är där du access hello många funktioner i hello Apptjänst-plattformen som du kan använda i dina appar för funktionen.</span><span class="sxs-lookup"><span data-stu-id="c04d5-125">hello **Platform features** tab is where you access hello many features of hello App Service platform that you can use in your function apps.</span></span> 

> [!NOTE]
> <span data-ttu-id="c04d5-126">Inte alla Apptjänst funktioner är tillgängliga när en funktionsapp körs på värd plan för hello förbrukning.</span><span class="sxs-lookup"><span data-stu-id="c04d5-126">Not all App Service features are available when a function app runs on hello Consumption hosting plan.</span></span>

<span data-ttu-id="c04d5-127">hello resten av det här avsnittet fokuserar på följande App Service-funktioner i hello hello Azure-portalen som är användbara för funktioner:</span><span class="sxs-lookup"><span data-stu-id="c04d5-127">hello rest of this topic focuses on hello following App Service features in hello Azure portal that are useful for Functions:</span></span>

+ [<span data-ttu-id="c04d5-128">Redigeraren för Apptjänst</span><span class="sxs-lookup"><span data-stu-id="c04d5-128">App Service editor</span></span>](#editor)
+ [<span data-ttu-id="c04d5-129">Programinställningar</span><span class="sxs-lookup"><span data-stu-id="c04d5-129">Application settings</span></span>](#settings) 
+ [<span data-ttu-id="c04d5-130">Konsol</span><span class="sxs-lookup"><span data-stu-id="c04d5-130">Console</span></span>](#console)
+ [<span data-ttu-id="c04d5-131">Avancerade verktyg (Kudu)</span><span class="sxs-lookup"><span data-stu-id="c04d5-131">Advanced tools (Kudu)</span></span>](#kudu)
+ [<span data-ttu-id="c04d5-132">Distributionsalternativ</span><span class="sxs-lookup"><span data-stu-id="c04d5-132">Deployment options</span></span>](#deployment)
+ [<span data-ttu-id="c04d5-133">CORS</span><span class="sxs-lookup"><span data-stu-id="c04d5-133">CORS</span></span>](#cors)
+ [<span data-ttu-id="c04d5-134">Autentisering</span><span class="sxs-lookup"><span data-stu-id="c04d5-134">Authentication</span></span>](#auth)
+ [<span data-ttu-id="c04d5-135">API-definition</span><span class="sxs-lookup"><span data-stu-id="c04d5-135">API definition</span></span>](#swagger)

<span data-ttu-id="c04d5-136">Mer information om hur toowork med App Service-inställningar, se [konfigurera inställningarna för Azure App Service](../app-service-web/web-sites-configure.md).</span><span class="sxs-lookup"><span data-stu-id="c04d5-136">For more information about how toowork with App Service settings, see [Configure Azure App Service Settings](../app-service-web/web-sites-configure.md).</span></span>

### <span data-ttu-id="c04d5-137"><a name="editor"></a>App Service Editor</span><span class="sxs-lookup"><span data-stu-id="c04d5-137"><a name="editor"></a>App Service Editor</span></span>

| | |
|-|-|
| ![Funktionsapp Apptjänst-redigeraren.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | <span data-ttu-id="c04d5-139">hello Redigeraren för Apptjänst är en avancerad i portalen redigerare som du kan använda toomodify JSON-konfigurationsfiler och kodfiler likadan.</span><span class="sxs-lookup"><span data-stu-id="c04d5-139">hello App Service editor is an advanced in-portal editor that you can use toomodify JSON configuration files and code files alike.</span></span> <span data-ttu-id="c04d5-140">Det här alternativet startar en separat webbläsarflik med ett grundläggande redigeringsprogram.</span><span class="sxs-lookup"><span data-stu-id="c04d5-140">Choosing this option launches a separate browser tab with a basic editor.</span></span> <span data-ttu-id="c04d5-141">Detta gör att du toointegrate med hello Git-lagringsplatsen, kör och felsökning kod och ändra funktionen app-inställningar.</span><span class="sxs-lookup"><span data-stu-id="c04d5-141">This enables you toointegrate with hello Git repository, run and debug code, and modify function app settings.</span></span> <span data-ttu-id="c04d5-142">Den här redigeraren ger en förbättrad utvecklingsmiljön för dina funktioner jämfört med hello standardbladet funktionen app.</span><span class="sxs-lookup"><span data-stu-id="c04d5-142">This editor provides an enhanced development environment for your functions compared with hello default function app blade.</span></span>    |

![hello Redigeraren för Apptjänst](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <span data-ttu-id="c04d5-144"><a name="settings"></a>Programinställningar</span><span class="sxs-lookup"><span data-stu-id="c04d5-144"><a name="settings"></a>Application settings</span></span>

| | |
|-|-|
| ![Funktionen app programinställningar.](./media/functions-how-to-use-azure-function-app-settings/function-app-application-settings.png) | <span data-ttu-id="c04d5-146">Hej Apptjänst **programinställningar** bladet är där du kan konfigurera och hantera framework-versioner, fjärrfelsökning, app-inställningar och anslutningssträngar.</span><span class="sxs-lookup"><span data-stu-id="c04d5-146">hello App Service **Application settings** blade is where you configure and manage framework versions, remote debugging, app settings, and connection strings.</span></span> <span data-ttu-id="c04d5-147">Du kan ändra de här inställningarna när du integrerar dina funktionsapp med andra Azure och tjänster från tredje part.</span><span class="sxs-lookup"><span data-stu-id="c04d5-147">When you integrate your function app with other Azure and third-party services, you can modify those settings here.</span></span> |

![Konfigurera programinställningar](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

### <span data-ttu-id="c04d5-149"><a name="console"></a>Konsolen</span><span class="sxs-lookup"><span data-stu-id="c04d5-149"><a name="console"></a>Console</span></span>

| | |
|-|-|
| ![Funktionen app-konsol i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | <span data-ttu-id="c04d5-151">hello i portalen konsolen är ett perfekt developer verktyg när du föredrar toointeract med funktionen appen från hello-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="c04d5-151">hello in-portal console is an ideal developer tool when you prefer toointeract with your function app from hello command line.</span></span> <span data-ttu-id="c04d5-152">Vanliga kommandon omfattar directory skapas och navigering, samt och köra batch-filer och skript.</span><span class="sxs-lookup"><span data-stu-id="c04d5-152">Common commands include directory and file creation and navigation, as well as executing batch files and scripts.</span></span> |

![Funktionen app-konsolen](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <span data-ttu-id="c04d5-154"><a name="kudu"></a>Avancerade verktyg (Kudu)</span><span class="sxs-lookup"><span data-stu-id="c04d5-154"><a name="kudu"></a>Advanced tools (Kudu)</span></span>

| | |
|-|-|
| ![Funktionsapp Kudu i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | <span data-ttu-id="c04d5-156">hello ger avancerade verktyg för Apptjänst (även kallat Kudu) åtkomst tooadvanced appen funktionen administrativa funktioner.</span><span class="sxs-lookup"><span data-stu-id="c04d5-156">hello advanced tools for App Service (also known as Kudu) provide access tooadvanced administrative features of your function app.</span></span> <span data-ttu-id="c04d5-157">Från Kudu Hantera Systeminformation, app-inställningar, miljövariabler, webbplatstillägg, HTTP-huvuden och servervariabler.</span><span class="sxs-lookup"><span data-stu-id="c04d5-157">From Kudu, you manage system information, app settings, environment variables, site extensions, HTTP headers, and server variables.</span></span> <span data-ttu-id="c04d5-158">Du kan också starta **Kudu** genom att bläddra toohello SCM slutpunkt för appen funktion som`https://<myfunctionapp>.scm.azurewebsites.net/`</span><span class="sxs-lookup"><span data-stu-id="c04d5-158">You can also launch **Kudu** by browsing toohello SCM endpoint for your function app, like `https://<myfunctionapp>.scm.azurewebsites.net/`</span></span> |

![Konfigurera Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><span data-ttu-id="c04d5-160"><a name="deployment">Distributionsalternativ</span><span class="sxs-lookup"><span data-stu-id="c04d5-160"><a name="deployment">Deployment options</span></span>

| | |
|-|-|
| ![Funktionen app distributionsalternativ i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | <span data-ttu-id="c04d5-162">Funktioner kan du utveckla Funktionskoden på din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="c04d5-162">Functions lets you develop your function code on your local machine.</span></span> <span data-ttu-id="c04d5-163">Du kan sedan överföra dina lokala funktionen app-projekt tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c04d5-163">You can then upload your local function app project tooAzure.</span></span> <span data-ttu-id="c04d5-164">Dessutom tootraditional FTP överför funktioner kan du distribuera appen funktionen med hjälp av populära kontinuerlig integration lösningar som GitHub, VSTS, Dropbox, Bitbucket och andra.</span><span class="sxs-lookup"><span data-stu-id="c04d5-164">In addition tootraditional FTP upload, Functions lets you deploy your function app using popular continuous integration solutions, like GitHub, VSTS, Dropbox, Bitbucket, and others.</span></span> <span data-ttu-id="c04d5-165">Mer information finns i [kontinuerlig distribution för Azure Functions](functions-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="c04d5-165">For more information, see [Continuous deployment for Azure Functions](functions-continuous-deployment.md).</span></span> <span data-ttu-id="c04d5-166">tooupload manuellt med hjälp av FTP- eller lokal Git måste också [konfigurera dina autentiseringsuppgifter för distribution](functions-continuous-deployment.md#credentials).</span><span class="sxs-lookup"><span data-stu-id="c04d5-166">tooupload manually using FTP or local Git, you also must [configure your deployment credentials](functions-continuous-deployment.md#credentials).</span></span> |


### <span data-ttu-id="c04d5-167"><a name="cors"></a>CORS</span><span class="sxs-lookup"><span data-stu-id="c04d5-167"><a name="cors"></a>CORS</span></span>

| | |
|-|-|
| ![Funktionsapp CORS i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | <span data-ttu-id="c04d5-169">tooprevent körning av skadlig kod i dina tjänster och Apptjänst block anropar tooyour funktionen appar från externa källor.</span><span class="sxs-lookup"><span data-stu-id="c04d5-169">tooprevent malicious code execution in your services, App Service blocks calls tooyour function apps from external sources.</span></span> <span data-ttu-id="c04d5-170">Functions stöder resursdelning för korsande ursprung (CORS) toolet som du definierar en ”godkänd” för tillåtna ursprung som dina funktioner kan acceptera fjärrbegäranden.</span><span class="sxs-lookup"><span data-stu-id="c04d5-170">Functions supports cross-origin resource sharing (CORS) toolet you define a "whitelist" of allowed origins from which your functions can accept remote requests.</span></span>  |

![Konfigurera Funktionsapp CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <span data-ttu-id="c04d5-172"><a name="auth"></a>Autentisering</span><span class="sxs-lookup"><span data-stu-id="c04d5-172"><a name="auth"></a>Authentication</span></span>

| | |
|-|-|
| ![Funktionen app autentisering i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | <span data-ttu-id="c04d5-174">När funktioner använder en HTTP-utlösare, du kan kräva anrop toofirst autentiseras.</span><span class="sxs-lookup"><span data-stu-id="c04d5-174">When functions use an HTTP trigger, you can require calls toofirst be authenticated.</span></span> <span data-ttu-id="c04d5-175">Apptjänst har stöd för Azure Active Directory-autentisering och logga in med sociala leverantörer, till exempel Facebook, Microsoft och Twitter.</span><span class="sxs-lookup"><span data-stu-id="c04d5-175">App Service supports Azure Active Directory authentication and sign in with social providers, such as Facebook, Microsoft, and Twitter.</span></span> <span data-ttu-id="c04d5-176">Mer information om hur du konfigurerar specifika autentiseringsproviders finns [översikt över Azure App Service-autentisering](../app-service/app-service-authentication-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c04d5-176">For details on configuring specific authentication providers, see [Azure App Service authentication overview](../app-service/app-service-authentication-overview.md).</span></span> |

![Konfigurera autentisering för en funktionsapp](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <span data-ttu-id="c04d5-178"><a name="swagger"></a>API-definition</span><span class="sxs-lookup"><span data-stu-id="c04d5-178"><a name="swagger"></a>API definition</span></span>

| | |
|-|-|
| ![Funktionen app API swagger-definition i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | <span data-ttu-id="c04d5-180">Functions stöder Swagger tooallow klienter toomore enkelt använda din HTTP-utlösta funktioner.</span><span class="sxs-lookup"><span data-stu-id="c04d5-180">Functions supports Swagger tooallow clients toomore easily consume your HTTP-triggered functions.</span></span> <span data-ttu-id="c04d5-181">Mer information om hur du skapar API-definitioner med Swagger finns [Kom igång med API Apps, ASP.NET och Swagger i Azure](../app-service-api/app-service-api-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c04d5-181">For more information on creating API definitions with Swagger, visit [Get Started with API Apps, ASP.NET, and Swagger in Azure](../app-service-api/app-service-api-dotnet-get-started.md).</span></span> <span data-ttu-id="c04d5-182">Du kan också använda funktioner proxyservrar toodefine en enda API-yta för flera funktioner.</span><span class="sxs-lookup"><span data-stu-id="c04d5-182">You can also use Functions Proxies toodefine a single API surface for multiple functions.</span></span> <span data-ttu-id="c04d5-183">Mer information finns i [arbeta med Azure Functions proxyservrar](functions-proxies.md).</span><span class="sxs-lookup"><span data-stu-id="c04d5-183">For more information, see [Working with Azure Functions Proxies](functions-proxies.md).</span></span> |

![Konfigurera Funktionsapp-API](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a><span data-ttu-id="c04d5-185">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c04d5-185">Next steps</span></span>

+ [<span data-ttu-id="c04d5-186">Konfigurera inställningar för Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c04d5-186">Configure Azure App Service Settings</span></span>](../app-service-web/web-sites-configure.md)
+ [<span data-ttu-id="c04d5-187">Löpande distribution för Azure Functions</span><span class="sxs-lookup"><span data-stu-id="c04d5-187">Continuous deployment for Azure Functions</span></span>](functions-continuous-deployment.md)



