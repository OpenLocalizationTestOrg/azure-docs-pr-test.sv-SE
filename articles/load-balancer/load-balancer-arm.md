---
title: "aaaAzure Resource Manager-stöd för belastningsutjämnaren | Microsoft Docs"
description: "Med hjälp av powershell för belastningsutjämnaren med Azure Resource Manager. Med hjälp av mallar för belastningsutjämnare"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: tysonn
ms.assetid: d0394f11-ee5a-4407-9d86-79c936297265
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
ms.openlocfilehash: 3c02d9382de00fefe6af8f4f8a2b7b62b344f285
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-azure-resource-manager-support-with-azure-load-balancer"></a>Med Azure belastningsutjämnare Azure Resource Manager-stöd

Azure Resource Manager är hello önskade ramverk för tjänster i Azure. Azure belastningsutjämnare kan hanteras med Azure Resource Manager-baserade API: er och verktyg.

## <a name="concepts"></a>Koncept

Med Resource Manager innehåller Azure belastningsutjämnare hello följande underordnade resurser:

* Frontend IP-konfiguration – en belastningsutjämnare kan innehålla en eller flera klientdelen IP-adresser, kallas även för en virtuell IP-adresser (VIP). IP-adresserna fungera som ingång för hello-trafik.
* Backend-adresspool – det här är IP-adresser som är associerade med hello virtuella nätverk Interface Card (NIC) toowhich belastningen ska distribueras.
* Belastningsutjämning regler – en regel för egenskapen mappar en given klientdelen IP-adress och port kombination tooa uppsättning serverdel IP-adresser och port-kombination. En enskild belastningsutjämnare kan ha flera regler för belastningsutjämning. Varje regel är en kombination av en frontend-IP och port och backend-IP och port som associeras med virtuella datorer.
* Avsökningar – avsökningar aktiverar du tookeep spåra hello hälsotillståndet hos VM-instanser. Om en hälsoavsökningen misslyckas ska hello VM-instans tas bort från rotation automatiskt.
* Inkommande NAT-regler – NAT-regler som definierar hello inkommande trafik som passerar genom hello klientdelen IP och distribuerade toohello tillbaka sluta IP.

![](./media/load-balancer-arm/load-balancer-arm.png)

## <a name="quickstart-templates"></a>Snabbstartsmallar

Azure Resource Manager kan du tooprovision dina program med en deklarativ mall. I samma mall kan du distribuera flera tjänster tillsammans med deras beroenden. Du använder hello samma mall toorepeatedly distribuera programmet under varje steg i livscykeln för hello-programmet.

Mallar kan innehålla definitioner för virtuella datorer, virtuella nätverk, Tillgänglighetsuppsättningar, nätverksgränssnitt (NIC), Storage-konton, belastningsutjämnare, Nätverkssäkerhetsgrupper och offentliga IP-adresser. Du kan skapa allt du behöver för komplexa program med mallar. hello mallfilen kan kontrolleras till innehållshantering system för versionskontroll och samarbete.

[Mer information om mallar](../azure-resource-manager/resource-manager-template-walkthrough.md)

[Mer information om nätverksresurser](../virtual-network/resource-groups-networking.md)

Snabbstartsmallar med hjälp av Azure belastningsutjämnare finns i en [GitHub-lagringsplatsen](https://github.com/Azure/azure-quickstart-templates) värd för en uppsättning community genereras mallar.

Exempel på mallar:

* [2 virtuella datorer i en belastningsutjämnare och belastningsutjämningsregler](http://go.microsoft.com/fwlink/?LinkId=544799)
* [2 virtuella datorer i ett VNET med en intern belastningsutjämnare och belastningsutjämnare regler](http://go.microsoft.com/fwlink/?LinkId=544800)
* [2 virtuella datorer i en belastningsutjämnare och konfigurera NAT-regler på hello LB](http://go.microsoft.com/fwlink/?LinkId=544801)

## <a name="setting-up-azure-load-balancer-with-a-powershell-or-cli"></a>Konfigurera Azure belastningsutjämnare med PowerShell eller CLI

Kom igång med Azure Resource Manager cmdlets, kommandoradsverktyg och REST API: er

* [Azure-nätverk Cmdlets](https://msdn.microsoft.com/library/azure/mt163510.aspx) kan vara används toocreate belastningsutjämning.
* [Hur toocreate en belastningsutjämnare med Azure Resource Manager](load-balancer-get-started-ilb-arm-ps.md)
* [Med hjälp av hello Azure CLI med Azure Resource Manager](../xplat-cli-azure-resource-manager.md)
* [Belastningsutjämnaren REST API: er](https://msdn.microsoft.com/library/azure/mt163651.aspx)

## <a name="next-steps"></a>Nästa steg

Du kan också [komma igång med en Internetuppkopplad belastningsutjämnaren](load-balancer-get-started-internet-arm-ps.md) och konfigurera vilken typ av [distribution läge](load-balancer-distribution-mode.md) för en särskild belastningsutjämnare trafik nätverksproblem.

Lär dig hur toomanage [inaktiv TCP timeout-inställningar för en belastningsutjämnare](load-balancer-tcp-idle-timeout.md). Detta är viktigt när ditt program behöver tookeep anslutningar alive för servrar bakom en belastningsutjämnare.
