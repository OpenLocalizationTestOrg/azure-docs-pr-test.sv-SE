---
title: aaaCreate SharePoint-servergrupper i Azure | Microsoft Docs
description: "Snabbt skapa en ny servergrupp för SharePoint 2013 eller SharePoint 2016 i Azure med hjälp av hello Azure portal marketplace."
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
ms.openlocfilehash: d71c7177d9b99cf377bab767342508007285b452
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-sharepoint-server-farms-using-hello-azure-portal-marketplace"></a>Skapa SharePoint-servergrupper med hello Azure portal marketplace

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="sharepoint-2013-farms"></a>SharePoint 2013-servergrupper
Med marketplace-hello Microsoft Azure portal, kan du snabbt skapa förkonfigurerade SharePoint Server 2013-servergrupper. Detta kan spara mycket tid när du behöver en grundläggande eller hög tillgänglighet SharePoint-grupp för en miljö för utveckling och testning eller om du utvärderar SharePoint Server 2013 som en lösning för samarbete för din organisation.

> [!NOTE]
> Hej **SharePoint-servergrupp** objektet i hello Azure Marketplace för hello Azure-portalen har tagits bort. Den har ersatts med hello **SharePoint 2013 icke - hög tillgänglighet-grupp** och **SharePoint 2013-grupp för hög tillgänglighet** objekt.
>
>

hello grundläggande SharePoint-grupp består av tre virtuella datorer i den här konfigurationen.

![sharepointfarm](./media/sharepoint-farm/Non-HAFarm.png)

Du kan använda den här gruppkonfigurationen för en förenklad inställning för utveckling av appar i SharePoint eller första gången utvärderingen av SharePoint 2013.

toocreate hello grundläggande () SharePoint grupp med tre servrar:

1. Klicka på [här](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Klicka på **distribuera**.
3. På hello **SharePoint 2013 icke - hög tillgänglighet-grupp** rutan klickar du på **skapa**.
4. Ange inställningar på hello stegen för hello **skapa SharePoint 2013 icke - hög tillgänglighet-grupp** rutan och klicka sedan på **skapa**.

hello hög tillgänglighet SharePoint-grupp består av nio virtuella datorer i den här konfigurationen.

![sharepointfarm](./media/sharepoint-farm/HAFarm.png)

Du kan använda den här gruppen configuration tootest högre klienten belastning, hög tillgänglighet för hello externa SharePoint-webbplatsen och SQL Server AlwaysOn-Tillgänglighetsgrupper för en SharePoint-servergrupp. Du kan också använda den här konfigurationen för utveckling av appar i SharePoint i en miljö med hög tillgänglighet.

toocreate hello hög tillgänglighet (nio-server) SharePoint-grupp:

1. Klicka på [här](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Klicka på **distribuera**.
3. På hello **SharePoint 2013-grupp för hög tillgänglighet** rutan klickar du på **skapa**.
4. Ange inställningar på hello sju steg som krävs för hello **skapa SharePoint 2013 hög tillgänglighet-grupp** rutan och klicka sedan på **skapa**.

> [!NOTE]
> Du kan inte skapa hello **SharePoint 2013 icke - hög tillgänglighet-grupp** eller **SharePoint 2013-grupp för hög tillgänglighet** med en kostnadsfri utvärderingsversion av Azure.
>
>

hello Azure-portalen skapar båda dessa grupper i en endast molnbaserad virtuellt nätverk med en Internet-riktade webben. Det finns ingen plats-till-plats VPN eller ExpressRoute-anslutning tillbaka tooyour organisationens nätverk.

> [!NOTE]
> När du skapar hello grundläggande eller hello Azure-portalen med hög tillgänglighet SharePoint-servergrupper, du kan inte ange en befintlig resursgrupp. toowork runt den här begränsningen kan skapa dessa grupper med Azure PowerShell. Mer information finns i [skapa SharePoint 2013 utveckling och testning i grupper med Azure PowerShell](https://technet.microsoft.com/library/mt743093.aspx#powershell).
>
>

## <a name="sharepoint-2016-farms"></a>SharePoint 2016 servergrupper
Se [i den här artikeln](https://technet.microsoft.com/library/mt723354.aspx) hello instruktioner toobuild hello efter enskild server SharePoint Server 2016 servergruppen.

![sharepointfarm](./media/sharepoint-farm/SP2016Farm.png)

## <a name="managing-hello-sharepoint-farms"></a>Hantera hello SharePoint-servergrupper
Du kan administrera hello servrar av dessa anläggningar via anslutningar till fjärrskrivbord. Mer information finns i [inloggning toohello virtuella](quick-create-portal.md#connect-to-virtual-machine).

Du kan konfigurera min platser, SharePoint-program och andra funktioner hello Central Administration av SharePoint-webbplatsen. Mer information finns i [Konfigurera SharePoint](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Nästa steg
* Identifiera ytterligare [SharePoint konfigurationer](https://technet.microsoft.com/library/dn635309.aspx) i Azure infrastrukturtjänster.
