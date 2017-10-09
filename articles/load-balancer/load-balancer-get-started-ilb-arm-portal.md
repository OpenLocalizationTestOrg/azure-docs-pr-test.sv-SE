---
title: "aaaCreate en intern belastningsutjämnare - Azure-portalen | Microsoft Docs"
description: "Lär dig hur toocreate en intern belastningsutjämnare i Resource Manager med hello Azure-portalen"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ac14fb9-8d14-4892-bfe6-8bc74c48ae2c
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 80124217a84857b542eb41cb814ec97234176dd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-in-hello-azure-portal"></a>Skapa en intern belastningsutjämnare i hello Azure-portalen

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Mall](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln täcker hello Resource Manager-distributionsmodellen, som Microsoft rekommenderar för de flesta nya distributioner i stället för hello [klassiska distributionsmodellen](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Kom igång med att skapa en intern belastningsutjämnare med hjälp av Azure Portal

Använd hello följande steg toocreate en intern belastningsutjämnare från hello Azure-portalen.

1. Öppna en webbläsare, navigera toohello [Azure-portalen](http://portal.azure.com), och logga in med ditt Azure-konto.
2. Hello övre vänstra sida av hello-skärmen klickar du på **ny** > **nätverk** > **belastningsutjämnaren**.
3. I hello **skapa belastningsutjämnaren** bladet anger du en **namn** för din belastningsutjämnare.
4. Under **Schema** klickar du på **Intern**.
5. Klicka på **för virtuella nätverk**, och sedan väljer hello virtuellt nätverk där du vill att toocreate hello belastningsutjämnaren.

   > [!NOTE]
   > Om du inte ser hello virtuella nätverk som du vill toouse Kontrollera hello **plats** du använder för hello belastningsutjämnaren och ändras den.

6. Klicka på **undernät**, och välj sedan hello undernät där du vill att toocreate hello belastningsutjämnaren.
7. Under **IP-adresstilldelning**, klicka på antingen **dynamiska** eller **statiska**, beroende på om du vill hello IP-adress för hello load balancer toobe fast (statisk) eller inte.

   > [!NOTE]
   > Om du väljer toouse statisk IP-adress har du tooprovide en adress för hello belastningsutjämnaren.

8. Under **resursgruppen** ange hello namnet på en ny resursgrupp för hello belastningsutjämnare, eller klicka på **Välj befintlig** och välj en befintlig resursgrupp.
9. Klicka på **Skapa**.

## <a name="configure-load-balancing-rules"></a>Konfigurera belastningsutjämningsregler

När hello ladda belastningsutjämnaren skapande, navigera toohello load balancer resurs tooconfigure den.
Du behöver tooconfigure först en backend-adresspool och en avsökning innan du konfigurerar en belastningsutjämningsregel.

### <a name="step-1-configure-a-back-end-pool"></a>Steg 1: Konfigurera en serverdelspool

1. I hello Azure-portalen klickar du på **Bläddra** > **belastningsutjämnare**, och klicka sedan på hello belastningsutjämnare som du skapade ovan.
2. I hello **inställningar** bladet, klickar du på **serverdelspooler**.
3. I hello **serverdelsadresspooler** bladet, klickar du på **Lägg till**.
4. I hello **lägga till serverdelspoolen** bladet anger du en **namn** hello serverdelspool och klicka sedan på **OK**.

### <a name="step-2-configure-a-probe"></a>Steg 2: Konfigurera en avsökning

1. I hello Azure-portalen klickar du på **Bläddra** > **belastningsutjämnare**, och klicka sedan på hello belastningsutjämnare som du skapade ovan.
2. I hello **inställningar** bladet, klickar du på **avsökningar**.
3. I hello **avsökningar** bladet, klickar du på **Lägg till**.
4. I hello **Lägg till avsökning** bladet anger du en **namn** för hello avsökningen.
5. Under **Protokoll** väljer du **HTTP** (för webbplatser) eller **TCP** (för andra TCP-baserade program).
6. Under **Port**, ange hello port toouse vid åtkomst till hello avsökning.
7. Under **sökväg** (för HTTP-avsökningar endast), ange hello sökvägen toouse som en avsökning.
8. Under **intervall** ange hur ofta tooprobe hello program.
9. Under **tröskelvärde för ohälsosamt värde**, ange hur många försök får misslyckas innan hello backend virtuella datorn markeras som ohälsosam.
10. Klicka på **OK** toocreate avsökning.

### <a name="step-3-configure-load-balancing-rules"></a>Steg 3: Konfigurera belastningsutjämningsregler

1. I hello Azure-portalen klickar du på **Bläddra** > **belastningsutjämnare**, och klicka sedan på hello belastningsutjämnare som du skapade ovan.
2. I hello **inställningar** bladet, klickar du på **belastningsutjämningsregler**.
3. I hello **belastningsutjämningsregler** bladet, klickar du på **Lägg till**.
4. I hello **Lägg till regel för belastningsutjämning** bladet anger du en **namn** för hello regeln.
5. Under **Protokoll** väljer du **HTTP** (för webbplatser) eller **TCP** (för andra TCP-baserade program).
6. Under **Port**, ange hello port klienterna ansluter tooin hello belastningsutjämnaren.
7. Under **serverdelsport**, ange hello port toobe används i hello serverdelspool (vanligtvis hello load balancer porten och serverdelsporten för hello är hello samma).
8. Under **serverdelspool**väljer hello backend programpool du skapade ovan.
9. Under **sessionspersistence**, Välj hur du vill sessioner toopersist.
10. Under **timeout för inaktivitet (minuter)**, ange hello timeout för inaktivitet.
11. Under **Flytande IP (direkt serverreturnering)** klickar du på **Inaktiverad** eller **Aktiverad**.
12. Klicka på **OK**.

## <a name="next-steps"></a>Nästa steg

[Konfigurera ett distributionsläge för belastningsutjämnare](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)

