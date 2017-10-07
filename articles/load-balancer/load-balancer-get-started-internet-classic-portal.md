---
title: "belastningsutjämnaren aaaCreate mot en Internet - Azure klassiska portal | Microsoft Docs"
description: "Lär dig hur toocreate ett Internet belastningsutjämnare i den klassiska modellen med hello klassiska Azure-portalen"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: fa3e93c0-968a-472d-a17c-65665c050db2
ms.service: load-balancer
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 27b0d5af6e7b493fa94a9dfbfa260483ae95a2fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-creating-an-internet-facing-load-balancer-classic-in-hello-azure-classic-portal"></a>Komma igång med en Internetuppkopplad belastningsutjämnare (klassisk) i hello klassiska Azure-portalen

> [!div class="op_single_selector"]
> * [Klassisk Azure-portal](../load-balancer/load-balancer-get-started-internet-classic-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-internet-classic-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-internet-classic-cli.md)
> * [Azure Cloud Services](../load-balancer/load-balancer-get-started-internet-classic-cloud.md)

[!INCLUDE [load-balancer-get-started-internet-intro-include.md](../../includes/load-balancer-get-started-internet-intro-include.md)]

> [!IMPORTANT]
> Innan du börjar arbeta med Azure-resurser, är det viktigt toounderstand att Azure har två distributionsmodeller: Azure Resource Manager och klassisk. Se till att du förstår [distributionsmodeller och verktyg](../azure-classic-rm.md) innan du börjar arbeta med Azure-resurser. Du kan visa hello dokumentationen för olika verktyg genom att klicka på flikarna hello hello överst i den här artikeln. Den här artikeln beskriver hello klassiska distributionsmodellen. Du kan också [Lär dig hur toocreate Internet-riktade belastningsutjämnaren med Azure Resource Manager](load-balancer-get-started-internet-arm-ps.md).

[!INCLUDE [load-balancer-get-started-internet-scenario-include.md](../../includes/load-balancer-get-started-internet-scenario-include.md)]

## <a name="set-up-an-internet-facing-load-balancer-for-virtual-machines"></a>Konfigurera en Internetuppkopplad belastningsutjämnare för virtuella datorer

Du måste skapa en belastningsutjämnad uppsättning i ordning tooload saldo nätverkstrafiken från hello Internet över hello virtuella datorer på en tjänst i molnet. Den här proceduren förutsätter att du redan har skapat hello virtuella datorer och att de är alla inom hello samma molntjänst.

**tooconfigure en belastningsutjämnad uppsättning för virtuella datorer**

1. I hello klassiska Azure-portalen klickar du på **virtuella datorer**, och klicka sedan på hello namnet på en virtuell dator i hello belastningsutjämnade uppsättningen.
2. Klicka på **Slutpunkter** och sedan på **Lägg till**.
3. På hello **lägga till en virtuell dator i slutpunkt tooa** klickar du på hello högerpilen.
4. På hello **ange hello information om hello endpoint** sidan:

   * I **namn**, Skriv ett namn för hello slutpunkten eller välj hello namn i hello lista över fördefinierade slutpunkter för vanliga protokoll.
   * I **protokollet**, Välj hello protokoll som krävs av hello typ av slutpunkt, TCP eller UDP, vid behov.
   * I **offentlig Port och privat Port**, Skriv hello-portnummer som du vill hello virtuella toouse efter behov. Du kan använda hello privata porten och brandväggsregler på hello virtuella tooredirect trafik på sätt som passar ditt program. hello privat port kan hello samma som hello offentlig port. Till exempel för en slutpunkt för webbtrafik (HTTP) och tilldela port 80 tooboth hello offentliga och privata portar.

5. Välj **skapa en belastningsutjämnad uppsättning**, och klicka sedan på högerpilen för hello.
6. På hello **konfigurera hello belastningsutjämnad uppsättning** sidan, skriver du ett namn för hello belastningsutjämnade uppsättningen och tilldela hello värden för avsökningen funktionssätt hello Azure belastningsutjämnare. hello belastningsutjämnare använder avsökningar toodetermine om hello virtuella datorer i hello belastningsutjämnad uppsättning tillgängliga tooreceive inkommande trafik.
7. Klicka på hello markerat toocreate hello belastningsutjämnade slutpunkt. Du ser **Ja** i hello **belastningsutjämnade namnet** kolumn i hello **slutpunkter** för hello virtuell dator.
8. I hello-portalen klickar du på **virtuella datorer**, klicka på hello namnet på en ytterligare virtuell dator i hello belastningsutjämnade uppsättningen, **slutpunkter**, och klicka sedan på **Lägg till**.
9. På hello **lägga till en virtuell dator i slutpunkt tooa** klickar du på **lägga till slutpunkten tooan befintlig belastningsutjämnad uppsättning**Välj hello namnet på hello belastningsutjämnad uppsättning och klicka sedan på högerpilen för hello.
10. På hello **ange hello information om hello endpoint** sidan anger du ett namn för hello slutpunkten, och klicka sedan på hello är markerat.

Upprepa steg 8-10 för hello ytterligare virtuella datorer i hello belastningsutjämnade uppsättningen.

## <a name="next-steps"></a>Nästa steg

[Komma igång med att konfigurera en intern belastningsutjämnare](load-balancer-get-started-ilb-arm-ps.md)

[Konfigurera ett distributionsläge för belastningsutjämnare](load-balancer-distribution-mode.md)

[Konfigurera timeout-inställningar för inaktiv TCP för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md)
