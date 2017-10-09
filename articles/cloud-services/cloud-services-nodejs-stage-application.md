---
title: aaaStage cloud service-distributionen (Node.js) | Microsoft Docs
description: "Lär dig hur toodeploy din Azure-program tooa mellanlagring miljö, distribuera tooa produktionsmiljön med virtuella IP-adresser (VIP) byte."
services: cloud-services
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: d65d26a6-b424-49cd-a88c-7ef46bb112a8
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2017
ms.author: tarcher
ms.openlocfilehash: 0656c84ac444a10f152f441265f85141347bf1fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="staging-an-application-in-azure"></a>Mellanlagring av ett program i Azure
En paketerad App kan vara distribuerade toohello mellanlagring miljön i Azure toobe testas innan du flyttar den toohello produktion miljö i vilken hello program är tillgängligt på hello Internet. Mellanlagringsmiljön är exakt samma sätt som hello produktionsmiljön, förutom att du kan bara åtkomst hello mellanlagrad program med en fördunklade URL som genereras av Azure. När du har kontrollerat att programmet fungerar korrekt, kan det vara distribuerade toohello produktionsmiljö genom att utföra en växling för virtuella IP-adresser (VIP).

> [!NOTE]
> hello gäller stegen i den här artikeln endast toonode program värdbaserad som en Azure-molntjänst.
> 
> 

## <a name="step-1-stage-an-application"></a>Steg 1: Förbered ett program
Den här uppgiften beskriver hur toostage ett program med hjälp av hello **Microsoft Azure PowerShell**.

1. När du publicerar en tjänst kan bara skicka hello **-fack** parameter till hello **Publish-AzureServiceProject** cmdlet.
   
   ```powershell
   Publish-AzureServiceProject -Slot staging
   ```
2. Logga in toohello [klassiska Azure-portalen] och välj **molntjänster**. Efter hello Molntjänsten skapas och hello **mellanlagring** kolumnen status har uppdaterats för**kör**, klicka på hello tjänstnamn.
   
   ![portalen visar en aktiv tjänst][cloud-service]
3. Välj hello **instrumentpanelen**, och välj sedan **mellanlagring**.
   
   ![cloud service-instrumentpanelen][cloud-service-dashboard]
4. Anteckna värdet för hello i hello **Webbadress** post toohello höger. hello DNS-namnet är ett dolda interna ID som genereras av Azure.
   
    ![webbadress][cloud-service-staging-url]

Nu kan du kontrollera att hello programmet fungerar korrekt i hello mellanlagring miljö med hjälp av hello mellanlagring Webbadress.

## <a name="step-2-upgrade-an-application-in-production-by-swapping-vips"></a>Steg 2: Uppgradera ett program i produktion av växling VIP: er
När du har kontrollerat hello uppgraderad version av ett program i mellanlagringsmiljön kan du snabbt göra den tillgänglig i produktionen genom växling hello virtuella IP-adresser (VIP) för hello mellanlagrings- och produktionsmiljöer.

> [!NOTE]
> Det här steget förutsätter att du redan har distribuerat en program-tooproduction och mellanlagrad hello uppgraderad version av programmet.
> 
> 

1. Logga in på hello [klassiska Azure-portalen], klickar du på **molntjänster** och välj sedan hello tjänstnamn.
2. Från hello **instrumentpanelen**väljer **mellanlagring**, och klicka sedan på **växla** på hello hello sidans nederkant. Hello VIP-växling dialogruta öppnas.
   
   ![VIP-växling dialogrutan][vip-swap-dialog]
3. Granska hello information och klicka sedan på **OK**. hello två distributioner börja uppdatera som hello mellanlagring av distribution växlar tooproduction och hello produktion distribution växlar toostaging.

Du har mellanlagrad en distribution och uppgraderas en Produktionsdistribution av växla VIP: er med hello distribution i Förproduktion.

## <a name="additional-resources"></a>Ytterligare resurser
* [Hur tooDeploy en uppgradera tjänsten tooProduction genom att byta VIP: er i Azure]

[klassiska Azure-portalen]: http://manage.windowsazure.com
[cloud-service]: ./media/cloud-services-nodejs-stage-application/staging-cloud-service-running.png
[cloud-service-dashboard]: ./media/cloud-services-nodejs-stage-application/cloud-service-dashboard-staging.png
[cloud-service-staging-url]: ./media/cloud-services-nodejs-stage-application/cloud-service-staging-url.png
[vip-swap-dialog]: ./media/cloud-services-nodejs-stage-application/vip-swap-dialog.png
[Hur tooDeploy en uppgradera tjänsten tooProduction genom att byta VIP: er i Azure]: cloud-services-how-to-manage.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
