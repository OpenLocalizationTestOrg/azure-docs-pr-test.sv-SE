---
title: aaaOverview i Azure DNS | Microsoft Docs
description: "Översikt över DNS-värdtjänsten på Microsoft Azure. Värd för din domän i Microsoft Azure."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 68747a0d-b358-4b8e-b5e2-e2570745ec3f
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/19/2017
ms.author: gwallace
ms.openlocfilehash: a10f87c488356469e9c04aabde31129049563891
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-overview"></a>Översikt över Azure DNS

hello Domain Name System eller DNS är översätter (eller lösa) en webbplats eller tjänst name tooits IP-adress. Azure DNS är värdtjänsten för DNS-domäner som tillhandahåller namnmatchning med hjälp av Microsoft Azure-infrastrukturen. Värd för dina domäner i Azure, kan du hantera din DNS hello poster med samma autentiseringsuppgifter, API: er, verktyg och fakturering som andra Azure-tjänster.

![Översikt över DNS](./media/dns-overview/scenario.png)

## <a name="features"></a>Funktioner

* **Tillförlitlighet och prestanda** -DNS-domäner i Azure DNS finns på Azures globalt nätverk av DNS-namnservrar. Vi använder Anycast nätverk så att varje DNS-frågan har besvarats av hello närmaste tillgängliga DNS-server. Detta ger både snabb prestanda och hög tillgänglighet för din domän.

* **Sömlös integration** -hello Azure DNS-tjänsten kan använda toomanage DNS-posterna för din Azure-tjänster och kan vara används tooprovide DNS för din externa resurser. Azure DNS är integrerad i hello Azure-portalen och använder hello samma autentiseringsuppgifter, fakturering och support-kontrakt som andra Azure-tjänster.

* **Säkerhet** -hello Azure DNS-tjänsten är baserad på Azure Resource Manager. Därför fördelar från Resource Manager-funktioner, till exempel rollbaserad åtkomstkontroll, granskningsloggar och låsa resurs. Dina domäner och poster kan hanteras via hello Azure-portalen, Azure PowerShell-cmdlets och hello plattformsoberoende Azure CLI. Program som kräver automatisk DNS-hantering kan integreras med hello-tjänsten via hello REST-API och SDK: er.

Azure DNS stöder för närvarande inte köp av domännamn. Om du vill toopurchase domäner, måste toouse från tredje part domännamnsregistratorn. hello registrator debiterar vanligtvis en liten årlig avgift. hello domäner kan finnas i Azure DNS för för hantering av DNS-poster. Se [delegera en domän tooAzure DNS](dns-domain-delegation.md) mer information.

## <a name="pricing"></a>Prissättning

DNS-fakturering baseras på hello antal DNS-zoner i Azure och av hello antal DNS-frågor. Mer om prissättningen besök toolearn [priser för Azure DNS](https://azure.microsoft.com/pricing/details/dns/).

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

Vanliga frågor om Azure DNS finns hello [Azure DNS FAQ](dns-faq.md).

## <a name="next-steps"></a>Nästa steg

Lär dig mer om DNS-zoner och poster genom att besöka: [DNS-zoner och innehåller en översikt över](dns-zones-records.md).

Lär dig hur för[skapa en DNS-zon](./dns-getstarted-create-dnszone-portal.md) i Azure DNS.

Lär dig mer om hello andra nyckeln [nätverk](../networking/networking-overview.md) Azure.

