---
title: Skriptexempel Azure CLI - bindning som ett anpassat SSL-certifikatet till en webbapp | Microsoft Docs
description: "Skriptexempel Azure CLI - binda ett SSL-certifikat som är anpassade till en webbapp"
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
ms.openlocfilehash: d4fab3fb2c297bf5f498b63bee46692febb9180b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="bind-a-custom-ssl-certificate-to-a-web-app"></a>Koppla en anpassad SSL-certifikatet till en webbapp

Det här exempelskriptet skapar en webbapp i App Service med dess relaterade resurser och sedan binda SSL-certifikatet för ett anpassat domännamn till den. För det här exemplet behöver du:

* Åtkomst till din domänregistrator DNS-konfigurationssidan.
* En giltig. PFX-filen och lösenordet för SSL-certifikatet som du vill ladda upp och binda.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 


## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/app-service/configure-ssl-certificate/configure-ssl-certificate.sh?highlight=3-5 "binda ett anpassat SSL-certifikat till en webbapp")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon. Varje kommando i tabellen länkar till kommandot viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ programtjänstplan](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Skapar en App Service-plan. |
| [Skapa AZ webapp](https://docs.microsoft.com/cli/azure/webapp#create) | Skapar ett Azure-webbapp. |
| [Lägg till AZ webapp config värdnamn](https://docs.microsoft.com/cli/azure/webapp/config/hostname#add) | Avbildar en anpassad domän till en webbapp. |
| [AZ webapp config ssl överför](https://docs.microsoft.com/cli/azure/webapp/config/ssl#upload) | Överför ett SSL-certifikat till ett webbprogram. |
| [AZ webapp config ssl-bindning](https://docs.microsoft.com/cli/azure/webapp/config/ssl#bind) | Binder en överförda SSL-certifikat till ett webbprogram. |

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare App Service CLI skriptexempel finns i den [dokumentation för Azure App Service](../app-service-cli-samples.md).
