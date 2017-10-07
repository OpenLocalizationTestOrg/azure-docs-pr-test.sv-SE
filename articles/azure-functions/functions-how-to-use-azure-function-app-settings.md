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
# <a name="how-toomanage-a-function-app-in-hello-azure-portal"></a>Hur toomanage en funktionsapp i hello Azure-portalen 

I Azure Functions ger en funktionsapp hello körningskontexten för dina enskilda funktioner. Funktionen app beteenden gäller tooall funktioner hos en viss funktionsapp. Det här avsnittet beskrivs hur tooconfigure och hantera dina appar för funktionen i hello Azure-portalen.

toobegin, gå toohello [Azure-portalen](http://portal.azure.com) och logga in tooyour Azure-konto. Hello sökfältet hello överst i portalen hello hello typnamn för funktionen appen och markera den hello-listan. När du har valt appen funktionen, kan du se hello följande sida:

![Översikt över funktionen app i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-main.png)

## <a name="manage-app-service-settings"></a>Fliken för funktionen app-inställningar

![Funktionen app översikt i hello Azure-portalen.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-settings-tab.png)

Hej **inställningar** är där du kan uppdatera hello funktioner körtidsversion som används av din funktionsapp. Det är också där du hanterar hello värd nycklar som används toorestrict HTTP-åtkomst tooall funktioner hos hello funktionsapp.

Funktioner stöder både förbrukning värd och Apptjänst som är värd för planer. Mer information finns i [Välj hello rätt service-plan för Azure Functions](functions-scale.md). Funktioner för bättre förutsägbarhet i hello förbrukning planen kan du begränsa användning av plattform genom att ange en daglig användning kvot i GB-sekunder. När hello dagliga kvot har nåtts har hello funktionsapp stoppats. En funktionsapp stoppades på grund av når hello utgifter kvoten kan återaktiveras från hello samma kontext som upprätta hello dagligen utgifter kvoten. Se hello [Azure Functions sida med priser](http://azure.microsoft.com/pricing/details/functions/) information om fakturering.   

## <a name="platform-features-tab"></a>Plattform funktioner fliken

![Funktionen app plattform funktioner fliken.](./media/functions-how-to-use-azure-function-app-settings/azure-function-app-features-tab.png)

Funktionen appar körs i och underhålls av hello Azure App Service-plattformen. Därför har funktionen-appar åtkomst toomost av hello funktionerna i Azures core webbvärd plattform. Hej **plattformsfunktioner** är där du access hello många funktioner i hello Apptjänst-plattformen som du kan använda i dina appar för funktionen. 

> [!NOTE]
> Inte alla Apptjänst funktioner är tillgängliga när en funktionsapp körs på värd plan för hello förbrukning.

hello resten av det här avsnittet fokuserar på följande App Service-funktioner i hello hello Azure-portalen som är användbara för funktioner:

+ [Redigeraren för Apptjänst](#editor)
+ [Programinställningar](#settings) 
+ [Konsol](#console)
+ [Avancerade verktyg (Kudu)](#kudu)
+ [Distributionsalternativ](#deployment)
+ [CORS](#cors)
+ [Autentisering](#auth)
+ [API-definition](#swagger)

Mer information om hur toowork med App Service-inställningar, se [konfigurera inställningarna för Azure App Service](../app-service-web/web-sites-configure.md).

### <a name="editor"></a>App Service Editor

| | |
|-|-|
| ![Funktionsapp Apptjänst-redigeraren.](./media/functions-how-to-use-azure-function-app-settings/function-app-appsvc-editor.png)  | hello Redigeraren för Apptjänst är en avancerad i portalen redigerare som du kan använda toomodify JSON-konfigurationsfiler och kodfiler likadan. Det här alternativet startar en separat webbläsarflik med ett grundläggande redigeringsprogram. Detta gör att du toointegrate med hello Git-lagringsplatsen, kör och felsökning kod och ändra funktionen app-inställningar. Den här redigeraren ger en förbättrad utvecklingsmiljön för dina funktioner jämfört med hello standardbladet funktionen app.    |

![hello Redigeraren för Apptjänst](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-appservice-editor.png)

### <a name="settings"></a>Programinställningar

| | |
|-|-|
| ![Funktionen app programinställningar.](./media/functions-how-to-use-azure-function-app-settings/function-app-application-settings.png) | Hej Apptjänst **programinställningar** bladet är där du kan konfigurera och hantera framework-versioner, fjärrfelsökning, app-inställningar och anslutningssträngar. Du kan ändra de här inställningarna när du integrerar dina funktionsapp med andra Azure och tjänster från tredje part. |

![Konfigurera programinställningar](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-settings.png)

### <a name="console"></a>Konsolen

| | |
|-|-|
| ![Funktionen app-konsol i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/function-app-console.png) | hello i portalen konsolen är ett perfekt developer verktyg när du föredrar toointeract med funktionen appen från hello-kommandoraden. Vanliga kommandon omfattar directory skapas och navigering, samt och köra batch-filer och skript. |

![Funktionen app-konsolen](./media/functions-how-to-use-azure-function-app-settings/configure-function-console.png)

### <a name="kudu"></a>Avancerade verktyg (Kudu)

| | |
|-|-|
| ![Funktionsapp Kudu i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/function-app-advanced-tools.png) | hello ger avancerade verktyg för Apptjänst (även kallat Kudu) åtkomst tooadvanced appen funktionen administrativa funktioner. Från Kudu Hantera Systeminformation, app-inställningar, miljövariabler, webbplatstillägg, HTTP-huvuden och servervariabler. Du kan också starta **Kudu** genom att bläddra toohello SCM slutpunkt för appen funktion som`https://<myfunctionapp>.scm.azurewebsites.net/` |

![Konfigurera Kudu](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-kudu.png)


### <a name="a-namedeploymentdeployment-options"></a><a name="deployment">Distributionsalternativ

| | |
|-|-|
| ![Funktionen app distributionsalternativ i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/function-app-deployment-source.png) | Funktioner kan du utveckla Funktionskoden på din lokala dator. Du kan sedan överföra dina lokala funktionen app-projekt tooAzure. Dessutom tootraditional FTP överför funktioner kan du distribuera appen funktionen med hjälp av populära kontinuerlig integration lösningar som GitHub, VSTS, Dropbox, Bitbucket och andra. Mer information finns i [kontinuerlig distribution för Azure Functions](functions-continuous-deployment.md). tooupload manuellt med hjälp av FTP- eller lokal Git måste också [konfigurera dina autentiseringsuppgifter för distribution](functions-continuous-deployment.md#credentials). |


### <a name="cors"></a>CORS

| | |
|-|-|
| ![Funktionsapp CORS i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/function-app-cors.png) | tooprevent körning av skadlig kod i dina tjänster och Apptjänst block anropar tooyour funktionen appar från externa källor. Functions stöder resursdelning för korsande ursprung (CORS) toolet som du definierar en ”godkänd” för tillåtna ursprung som dina funktioner kan acceptera fjärrbegäranden.  |

![Konfigurera Funktionsapp CORS](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-cors.png)

### <a name="auth"></a>Autentisering

| | |
|-|-|
| ![Funktionen app autentisering i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/function-app-authentication.png) | När funktioner använder en HTTP-utlösare, du kan kräva anrop toofirst autentiseras. Apptjänst har stöd för Azure Active Directory-autentisering och logga in med sociala leverantörer, till exempel Facebook, Microsoft och Twitter. Mer information om hur du konfigurerar specifika autentiseringsproviders finns [översikt över Azure App Service-autentisering](../app-service/app-service-authentication-overview.md). |

![Konfigurera autentisering för en funktionsapp](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-authentication.png)


### <a name="swagger"></a>API-definition

| | |
|-|-|
| ![Funktionen app API swagger-definition i hello Azure-portalen](./media/functions-how-to-use-azure-function-app-settings/function-app-api-definition.png) | Functions stöder Swagger tooallow klienter toomore enkelt använda din HTTP-utlösta funktioner. Mer information om hur du skapar API-definitioner med Swagger finns [Kom igång med API Apps, ASP.NET och Swagger i Azure](../app-service-api/app-service-api-dotnet-get-started.md). Du kan också använda funktioner proxyservrar toodefine en enda API-yta för flera funktioner. Mer information finns i [arbeta med Azure Functions proxyservrar](functions-proxies.md). |

![Konfigurera Funktionsapp-API](./media/functions-how-to-use-azure-function-app-settings/configure-function-app-apidef.png)



## <a name="next-steps"></a>Nästa steg

+ [Konfigurera inställningar för Azure App Service](../app-service-web/web-sites-configure.md)
+ [Löpande distribution för Azure Functions](functions-continuous-deployment.md)



