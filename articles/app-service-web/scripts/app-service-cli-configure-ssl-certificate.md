---
title: "aaaAzure CLI skriptexempel - binda ett anpassat webbprogram för SSL-certifikat tooa | Microsoft Docs"
description: "Skriptexempel Azure CLI - Bind ett anpassat webbprogram för SSL-certifikat tooa"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: eb95d350-81ea-4145-a1e2-6eea3b7469b2
ms.service: app-service-web
ms.workload: web
ms.devlang: azurecli
ms.tgt_pltfrm: na
ms.topic: sample
ms.date: 06/19/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 2fec2db84a2007fa6b005776c84d4f8cba392b46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="bind-a-custom-ssl-certificate-tooa-web-app"></a>Bind ett anpassat webbprogram för SSL-certifikat tooa

Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och Binder hello SSL-certifikatet för en anpassad domän namnet tooit. För det här exemplet behöver du:

* Komma åt tooyour domän domänregistrators DNS-konfigurationssidan.
* En giltig. PFX-filen och lösenordet för hello SSL-certifikat som du vill tooupload och bind.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "Bind a custom SSL certificate tooa web app")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ programtjänstplan](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Skapar en App Service-plan. |
| [Skapa AZ webapp](https://docs.microsoft.com/cli/azure/webapp#create) | Skapar ett Azure-webbapp. |
| [Lägg till AZ webapp config värdnamn](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | Avbildar en anpassad domän tooa webbapp. |
| [AZ webapp config ssl överför](https://docs.microsoft.com/cli/azure/webapp/config/ssl#upload) | Överför en SSL-certifikat tooa webbapp. |
| [AZ webapp config ssl-bindning](https://docs.microsoft.com/cli/azure/webapp/config/ssl#bind) | Binder en överförda SSL-certifikat tooa webbapp. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare App Service CLI skriptexempel finns i hello [dokumentation för Azure App Service](../app-service-cli-samples.md).
