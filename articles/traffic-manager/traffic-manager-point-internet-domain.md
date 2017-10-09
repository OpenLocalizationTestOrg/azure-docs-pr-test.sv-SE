---
title: "aaaPoint ett företags Internet domän tooa Traffic Manager-domännamn | Microsoft Docs"
description: "Den här artikeln hjälper dig peka företagets namn tooa Traffic Manager-domän domännamn."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 29822946-2d45-4434-ba47-fc180a445cc3
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
ms.openlocfilehash: 84c428f60a1dc70452bf957d98a68c95e0b51715
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="point-a-company-internet-domain-tooan-azure-traffic-manager-domain"></a>Peka företagets domän tooan Azure Traffic Manager Internetdomän

När du skapar en Traffic Manager-profil, tilldelar Azure automatiskt ett DNS-namn för profilen. toouse ett namn från DNS-zon skapa en CNAME DNS-post som mappar toohello domännamnet för Traffic Manager-profilen. Du hittar hello Traffic Manager-domännamnet i hello **allmänna** avsnitt på sidan för konfiguration av hello i hello Traffic Manager-profilen.

Till exempel toopoint namnet www.contoso.com toohello contoso.trafficmanager.net för Traffic Manager-DNS-namn, du skulle skapa hello följande DNS-resursposten:

    www.contoso.com IN CNAME contoso.trafficmanager.net

Alla trafikförfrågningar för*www.contoso.com* hämta dirigeras för*contoso.trafficmanager.net*.

> [!IMPORTANT]
> Du kan inte peka en andranivådomän, t.ex *contoso.com*, toohello Traffic Manager-domän. DNS-protokollstandarder tillåter inte CNAME-poster för andra nivåns domännamn.

## <a name="next-steps"></a>Nästa steg

* [Routningsmetoder för Traffic Manager](traffic-manager-routing-methods.md)
* [Traffic Manager – Inaktivera, aktiver eller ta bort en profil](disable-enable-or-delete-a-profile.md)
* [Traffic Manager – Inaktivera eller aktivera en slutpunkt](disable-or-enable-an-endpoint.md)
