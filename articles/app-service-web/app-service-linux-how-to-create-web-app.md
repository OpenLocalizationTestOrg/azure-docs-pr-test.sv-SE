---
title: "aaaCreate en Azure web app som körs på Linux | Microsoft Docs"
description: "Web app skapa arbetsflöde för Azure Web App på Linux."
keywords: "Azure apptjänst, webbprogram, linux, oss"
services: app-service
documentationcenter: 
author: naziml
manager: erikre
editor: 
ms.assetid: 3a71d10a-a0fe-4d28-af95-03b2860057d5
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2017
ms.author: naziml;wesmc
ms.openlocfilehash: de1bd030345d5e2a8024012067b5bcaa2cca09dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-web-app-running-on-linux"></a>Skapa en Azure-webbapp som körs på Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


## <a name="use-hello-azure-portal-toocreate-your-web-app"></a>Använd hello Azure portal toocreate ditt webbprogram
Du kan börja skapa ditt webbprogram på Linux från hello [Azure-portalen](https://portal.azure.com) som visas i följande bild hello:

![Skapa en webbapp på hello Azure-portalen][1]

Därefter hello **skapa-bladet** öppnas som visas i följande bild hello:

![hello skapa-bladet][2]

1. Namnge ditt webbprogram.
2. Välj en befintlig resursgrupp eller skapa en ny. (Se tillgängliga regioner i hello [avsnittet](app-service-linux-intro.md).)
3. Välj en befintlig Azure App Service-plan eller skapa en ny. (Se App Service-plan kommentarer i hello [avsnittet](app-service-linux-intro.md).)
4. Välj programmet hello stack som du avser toouse. Du kan välja mellan flera versioner av Node.js, PHP, .net kärn och Ruby.

När du har skapat hello app, kan du ändra hello programstack från hello programinställningar som visas i följande bild hello:

![Programinställningar][3]

## <a name="deploy-your-web-app"></a>Distribuera ditt webbprogram
Om du väljer **distributionsalternativ** från hello management portal ger du hello alternativet toouse lokala Git eller GitHub-lagringsplatsen toodeploy ditt program. hello resten av hello instruktioner är liknande toothose för en icke-Linux-webbprogrammet. Du kan följa hello instruktionerna i [lokal Git-distribution](app-service-deploy-local-git.md) eller [kontinuerlig distribution](app-service-continuous-deployment.md) toodeploy din app.

Du kan också använda FTP tooupload webbplatsen tooyour program. Du kan hämta hello FTP-slutpunkten för ditt webbprogram från hello diagnostik loggar avsnittet som visas i följande bild hello:

![Diagnostik-loggar][4]

## <a name="next-steps"></a>Nästa steg
* [Vad är Azure Web App på Linux?](app-service-linux-intro.md)
* [Med PM2 konfiguration för Node.js i Azure Webbapp på Linux](app-service-linux-using-nodejs-pm2.md)
* [Med hjälp av Ruby i Azure App Service Webbapp på Linux](app-service-linux-ruby-get-started.md)
* [Azure App Service Webbapp på Linux vanliga frågor och svar](app-service-linux-faq.md)

<!--Image references-->
[1]: ./media/app-service-linux-how-to-create-a-web-app/top-level-create.png
[2]: ./media/app-service-linux-how-to-create-a-web-app/create-blade.png
[3]: ./media/app-service-linux-how-to-create-a-web-app/application-settings-change-stack.png
[4]: ./media/app-service-linux-how-to-create-a-web-app/diagnostic-logs-ftp.png
