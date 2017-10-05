---
title: "Skapa en Azure-webbapp som körs på Linux | Microsoft Docs"
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
ms.openlocfilehash: 49091d4a85bed23927850f9c0bbc5ea8b6e8c9e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-an-azure-web-app-running-on-linux"></a>Skapa en Azure-webbapp som körs på Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


## <a name="use-the-azure-portal-to-create-your-web-app"></a>Använda Azure portal för att skapa ditt webbprogram
Du kan börja skapa ditt webbprogram på Linux från den [Azure-portalen](https://portal.azure.com) som visas i följande bild:

![Skapa en webbapp på Azure portal][1]

Sedan den **skapa-bladet** öppnas som visas i följande bild:

![Skapa-bladet][2]

1. Namnge ditt webbprogram.
2. Välj en befintlig resursgrupp eller skapa en ny. (Se tillgängliga regioner i den [avsnittet](app-service-linux-intro.md).)
3. Välj en befintlig Azure App Service-plan eller skapa en ny. (Se App Service-plan kommentarer i den [avsnittet](app-service-linux-intro.md).)
4. Välj programstack som du tänker använda. Du kan välja mellan flera versioner av Node.js, PHP, .net kärn och Ruby.

När du har skapat appen, kan du ändra programstack från programinställningarna enligt följande bild:

![Programinställningar][3]

## <a name="deploy-your-web-app"></a>Distribuera ditt webbprogram
Om du väljer **distributionsalternativ** från management portal kan du välja att använda lokal Git eller GitHub-lagringsplats för att distribuera ditt program. Resten av instruktionerna är samma som för en icke-Linux-webbprogrammet. Du kan följa anvisningarna i [lokal Git-distribution](app-service-deploy-local-git.md) eller [kontinuerlig distribution](app-service-continuous-deployment.md) att distribuera din app.

Du kan också använda FTP för att överföra ditt program på webbplatsen. Du kan hämta FTP-slutpunkten för din webbapp från avsnittet diagnostics loggar som visas i följande bild:

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
