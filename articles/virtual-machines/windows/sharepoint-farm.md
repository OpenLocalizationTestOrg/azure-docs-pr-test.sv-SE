---
title: Skapa SharePoint-servergrupper i Azure | Microsoft Docs
description: "Snabbt skapa en ny servergrupp för SharePoint 2013 eller SharePoint 2016 i Azure med hjälp av Azure portal marketplace."
services: virtual-machines-windows
documentationcenter: 
author: JoeDavies-MSFT
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 89b124da-019d-4179-86dd-ad418d05a4f2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/30/2016
ms.author: josephd
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 00648ee35962b22fb7eeceaa6d9df7305c801abf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-sharepoint-server-farms-using-the-azure-portal-marketplace"></a>Skapa SharePoint-servergrupper med hjälp av Azure portal marketplace

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="sharepoint-2013-farms"></a>SharePoint 2013-servergrupper
Med Microsoft Azure portal marketplace, kan du snabbt skapa förkonfigurerade SharePoint Server 2013-servergrupper. Detta kan spara mycket tid när du behöver en grundläggande eller hög tillgänglighet SharePoint-grupp för en miljö för utveckling och testning eller om du utvärderar SharePoint Server 2013 som en lösning för samarbete för din organisation.

> [!NOTE]
> Den **SharePoint-servergrupp** objektet i Azure Marketplace för Azure-portalen har tagits bort. Den har ersatts med den **SharePoint 2013 icke - hög tillgänglighet-grupp** och **SharePoint 2013-grupp för hög tillgänglighet** objekt.
>
>

Den grundläggande SharePoint-gruppen består av tre virtuella datorer i den här konfigurationen.

![sharepointfarm](./media/sharepoint-farm/Non-HAFarm.png)

Du kan använda den här gruppkonfigurationen för en förenklad inställning för utveckling av appar i SharePoint eller första gången utvärderingen av SharePoint 2013.

För att skapa den grundläggande (tre servrar) SharePoint-gruppen:

1. Klicka på [här](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Klicka på **distribuera**.
3. På den **SharePoint 2013 icke - hög tillgänglighet-grupp** rutan klickar du på **skapa**.
4. Ange inställningar på stegen i den **skapa SharePoint 2013 icke - hög tillgänglighet-grupp** rutan och klicka sedan på **skapa**.

Hög tillgänglighet SharePoint-grupp består av nio virtuella datorer i den här konfigurationen.

![sharepointfarm](./media/sharepoint-farm/HAFarm.png)

Du kan använda den här konfigurationen för servergruppen för att testa högre belastning på klienten, hög tillgänglighet för externa SharePoint-webbplatsen och SQL Server AlwaysOn-Tillgänglighetsgrupper för en SharePoint-servergrupp. Du kan också använda den här konfigurationen för utveckling av appar i SharePoint i en miljö med hög tillgänglighet.

Skapa SharePoint-servergruppen hög tillgänglighet (nio-server):

1. Klicka på [här](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Klicka på **distribuera**.
3. På den **SharePoint 2013-grupp för hög tillgänglighet** rutan klickar du på **skapa**.
4. Ange inställningar på sju steg för den **skapa SharePoint 2013 hög tillgänglighet-grupp** rutan och klicka sedan på **skapa**.

> [!NOTE]
> Du kan inte skapa den **SharePoint 2013 icke - hög tillgänglighet-grupp** eller **SharePoint 2013-grupp för hög tillgänglighet** med en kostnadsfri utvärderingsversion av Azure.
>
>

Azure-portalen skapar båda dessa grupper i en endast molnbaserad virtuellt nätverk med en Internet-riktade webben. Det finns ingen plats-till-plats VPN eller ExpressRoute-anslutning till organisationens nätverk.

> [!NOTE]
> När du skapar grundläggande eller hög tillgänglighet SharePoint-servergrupper med Azure-portalen, du kan inte ange en befintlig resursgrupp. Skapa dessa grupper med Azure PowerShell för att kringgå den här begränsningen. Mer information finns i [skapa SharePoint 2013 utveckling och testning i grupper med Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).
>
>

## <a name="sharepoint-2016-farms"></a>SharePoint 2016 servergrupper
Se [i den här artikeln](https://technet.microsoft.com/library/mt723354.aspx) anvisningar att skapa följande enskild server SharePoint Server 2016-grupp.

![sharepointfarm](./media/sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>Hantera SharePoint-servergrupper
Du kan administrera servrar i dessa grupper via anslutningar till fjärrskrivbord. Mer information finns i [logga in på den virtuella datorn](quick-create-portal.md#connect-to-virtual-machine).

Du kan konfigurera min platser, SharePoint-program och andra funktioner i Central Administration av SharePoint-webbplatsen. Mer information finns i [Konfigurera SharePoint](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Nästa steg
* Identifiera ytterligare [SharePoint konfigurationer](https://technet.microsoft.com/library/dn635309.aspx) i Azure infrastrukturtjänster.
