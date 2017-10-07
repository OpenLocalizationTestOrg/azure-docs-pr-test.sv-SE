---
title: "aaaContinuous distribution med Azure Web App på Linux | Microsoft Docs"
description: "Hur toosetup kontinuerlig distribution i Azure Web App på Linux."
keywords: "Azure apptjänst, linux, oss, acr"
services: app-service
documentationcenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: a47fb43a-bbbd-4751-bdc1-cd382eae49f8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: aelnably;wesmc
ms.openlocfilehash: f94d837e27605da58428f507ab2b0eb3af3297e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-with-azure-web-app-on-linux"></a>Kontinuerlig distribution med Azure Web App på Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

I kursen får du konfigurera kontinuerlig distribution för en avbildning för anpassade container från hanterade [Azure Container registret](https://azure.microsoft.com/en-us/services/container-registry/) databaser eller [Docker-hubb](https://hub.docker.com).

## <a name="step-1---sign-in-tooazure"></a>Steg 1 – Logga in tooAzure

Logga in toohello Azure-portalen på http://portal.azure.com

## <a name="step-2---enable-container-continuous-deployment-feature"></a>Steg 2 – aktivera funktionen för behållaren kontinuerlig distribution

Du kan aktivera funktionen-hello kontinuerlig distribution med [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) och köra följande kommando hello

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

I hello  **[Azure-portalen](https://portal.azure.com/)**, klicka på hello **Apptjänst** alternativet hello vänster hello-sidan.

Klicka på hello namnet på din app som du vill tooconfigure Docker-hubb kontinuerlig distribution för.

I hello **appinställningar**, Lägg till en app inställningen kallas `DOCKER_ENABLE_CI` med hello värdet `true`.

![infoga bilden app-inställning](./media/app-service-webapp-service-linux-ci-cd/step2.png)

## <a name="step-3---prepare-webhook-url"></a>Steg 3 – Förbereda Webhooksadressen

Du kan hämta hello Webhooksadressen med [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) och köra följande kommando hello

```azurecli-interactive
az webapp deployment container -n sname1 -g rgname -e true --show-cd-url
``` 

För hello Webhooksadressen, behöver du toohave hello följande slutpunkt: `https://<publishingusername>:<publishingpwd>@<sitename>.scm.azurewebsites.net/docker/hook`.

Du kan hämta din `publishingusername` och `publishingpwd` genom att hämta hello webbprogrammet publiceringsprofil med hello Azure-portalen.

![infoga bilden för att lägga till webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-3.png)

## <a name="step-4---add-a-web-hook"></a>Steg 4 – Lägg till en webbtjänst hook

### <a name="azure-container-registry"></a>Azure Container Registry

I din registret portalbladet klickar du på **Webhooks**, klicka på Skapa en ny webhook **Lägg till**. I hello **skapa webhook** bladet namnge din webhooken. För hello Webhook URI, behöver du tooprovide hello URL från **steg3**

Kontrollera att du har definierat hello scope som hello lagringsplatsen som innehåller bilden behållare.

![Infoga bild av webhooken](./media/app-service-webapp-service-linux-ci-cd/step3ACRWebhook-1.png)

När hello avbildningen uppdateras hämta hello webbprogrammet uppdateras automatiskt med hello ny avbildning.

### <a name="docker-hub"></a>Docker-hubb

På din hubb Docker klickar du på **Webhooks**, sedan **skapa en WEBHOOK**.

![infoga bilden för att lägga till webhook 1](./media/app-service-webapp-service-linux-ci-cd/step3-1.png)

För hello Webhooksadressen, behöver du tooprovide hello URL från **steg3**

![infoga bilden för att lägga till webhook 2](./media/app-service-webapp-service-linux-ci-cd/step3-2.png)

När hello avbildningen uppdateras hämta hello webbprogrammet uppdateras automatiskt med hello ny avbildning.

## <a name="next-steps"></a>Nästa steg
* [Vad är Azure Web App på Linux?](./app-service-linux-intro.md)
* [Azure-behållaren registret](https://azure.microsoft.com/en-us/services/container-registry/)
* [Med PM2 konfiguration för Node.js i Azure Webbapp på Linux](app-service-linux-using-nodejs-pm2.md)
* [Med .NET Core i Azure-Webbapp på Linux](app-service-linux-using-dotnetcore.md)
* [Med hjälp av Ruby i Azure Webbapp på Linux](app-service-linux-ruby-get-started.md)
* [Hur toouse anpassade Docker bild för Azure Web App på Linux](./app-service-linux-using-custom-docker-image.md)
* [Azure App Service Webbapp på Linux vanliga frågor och svar](./app-service-linux-faq.md) 
* [Hantera webbprogrammet på Linux med hjälp av Azure CLI 2.0](./app-service-linux-cli.md)



