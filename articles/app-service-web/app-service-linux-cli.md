---
title: "aaaManage webbprogrammet på Linux med hjälp av Azure CLI 2.0 | Microsoft Docs"
description: "Hantera webbprogrammet på Linux med hjälp av Azure CLI."
keywords: "Azure apptjänst, webbprogram, cli, linux, oss"
services: app-service
documentationCenter: 
author: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2017
ms.author: aelnably
ms.openlocfilehash: 5e8e0da8a362450c56d2e87e087f77598ec874ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-web-app-on-linux-using-azure-cli"></a>Hantera webbprogrammet på Linux med hjälp av Azure CLI

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]

Hello kommandona i den här artikeln du kan toocreate och hantera ett webbprogram i Linux med Azure CLI 2.0.
Du kan börja använda hello ny version av hello CLI på två sätt:

* [Installera Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli) på din dator.
* Med hjälp av [Azure-molnet Shell (förhandsgranskning)](../cloud-shell/overview.md)


## <a name="create-a-linux-app-service-plan"></a>Skapa en Linux-Programtjänstplan

toocreate en Linux App Service-Plan, kan du använda hello följande kommando:

```azurecli-interactive
az appservice plan create -n appname -g rgname --islinux -l "South Central US" --sku S1 --number-of-workers 1
``` 

## <a name="create-a-custom-docker-container-web-app"></a>Skapa en anpassad dockerbehållare Web App

toocreate en webbapp och konfigurerar den toorun anpassade dockerbehållare, du kan använda hello följande kommando:

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -i elnably/dockerimagetest
```
 
## <a name="activate-hello-docker-container-logging"></a>Aktivera loggning för hello Docker-behållare

tooactivate hello Docker behållare loggning kan du använda hello följande kommando:

```azurecli-interactive
az webapp log config -n sname -g rgname --web-server-logging filesystem
```
 
## <a name="change-hello-custom-docker-container-for-an-existing-web-app-on-linux-app"></a>Ändra hello anpassade dockerbehållare för en befintlig Webbapp på Linux-App

toochange en tidigare skapad app från hello aktuella Docker bild tooa ny avbildning, kan du använda hello följande kommando:

```azurecli-interactive
az webapp config container set -n sname -g rgname -c apurvajo/mariohtml5
``` 

## <a name="using-docker-images-from-a-private-registry"></a>Använda Docker-avbildningar från ett privat register

Du kan konfigurera din app toouse bilder från ett privat register. Du behöver tooprovide hello url för ditt registret, användarnamn och lösenord. Göra du kan detta med hjälp av hello följande kommando:

```azurecli-interactive
az webapp config container set -n sname1 -g rgname -c <container name> -r <server url> -u <username> -p <password>
``` 

## <a name="enable-continuous-deployments-for-custom-docker-images"></a>Aktivera kontinuerlig distribution för anpassade Docker-avbildningar

Med följande kommando som du kan aktivera hello hello CD-funktioner och få hello webhooksadressen. URL: en kan vara används tooconfigure du DockerHub eller Azure-behållare registret repor.

```azurecli-interactive
az webapp deployment container config -n sname -g rgname -e true
``` 

## <a name="create-a-web-app-on-linux-app-using-one-of-our-built-in-runtime-frameworks"></a>Skapa ett webbprogram på Linux-App med en av våra inbyggda runtime-ramverk

toocreate ett webbprogram för PHP-5.6 på Linux-App som du kan använda följande kommando hello.

```azurecli-interactive
az webapp create -n sname -g rgname -p pname -r "php|5.6"
``` 

## <a name="change-framework-version-for-an-existing-web-app-on-linux-app"></a>Ändra framework-version för en befintlig Webbapp på Linux-App

toochange en tidigare skapad app från hello aktuella framework version tooNode.js 6.11, kan du använda hello följande kommando:

```azurecli-interactive
az webapp config set -n sname -g rgname --linux-fx-version "node|6.11"
``` 

## <a name="set-up-git-deployments-for-your-web-app"></a>Konfigurera Git-distributioner för Webbappen

tooset Git-distributioner för din app kan du använda hello följande kommando:

```azurecli-interactive
az webapp deployment source config -n sname -g rgname --repo-url <gitrepo url> --branch <branch>
``` 


## <a name="next-steps"></a>Nästa steg
* [Vad är Azure Web App på Linux?](app-service-linux-intro.md)
* [Installera Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
* [Azure-molnet Shell (förhandsgranskning)](../cloud-shell/overview.md)
* [Skapa mellanlagringsmiljöer i Azure App Service](./web-sites-staged-publishing.md)
* [Kontinuerlig distribution med Azure-Webbapp på Linux](./app-service-linux-ci-cd.md)
