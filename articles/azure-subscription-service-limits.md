---
title: "aaaAzure prenumeration gränser och kvoter | Microsoft Docs"
description: "Visar en lista över vanliga Azure-prenumeration och tjänsten gränser, kvoter och begränsningar. Detta omfattar information om hur tooincrease begränsar tillsammans med högsta värden."
services: 
documentationcenter: 
author: rothja
manager: jeffreyg
editor: 
tags: billing
ms.assetid: 60d848f9-ff26-496e-a5ec-ccf92ad7d125
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: byvinyal
ms.openlocfilehash: a754d56124520791254ab8f1729808f0750ff222
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-subscription-and-service-limits-quotas-and-constraints"></a>Azure-prenumeration och tjänstbegränsningar, kvoter och krav
Det här dokumentet innehåller några av hello de vanligaste Microsoft Azure gränser, som ibland kallas kvoter. Det här dokumentet omfattar inte för närvarande alla Azure-tjänster. Över tiden, hello listan ska expanderas och uppdatera toocover mer hello plattform.

Besök [översikt över priser för Azure](https://azure.microsoft.com/pricing/) toolearn mer om prissättningen för Azure. Det, kan du uppskatta dina kostnader med hello [Priskalkylatorn](https://azure.microsoft.com/pricing/calculator/) eller genom att besöka hello priser informationssidan för en tjänst (till exempel [virtuella Windows-datorer](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)). Hantera dina kostnader för tips toohelp, se [förhindrar oväntade kostnader med Azure fakturerings- och kostnaden management](billing/billing-getting-started.md).

> [!NOTE]
> Om du vill tooraise hello gränsen eller kvoten ovan hello **standard gränsen**, [öppna en supportbegäran för online customer utan kostnad](azure-supportability/resource-manager-core-quotas-request.md). hello gränser kan inte ökas ovan hello **maxgränsen** värdet som visas i följande tabeller hello. Om det finns inga **maxgränsen** kolumn, och sedan hello-resurs inte har justerbara gränser. 
> 
> Kostnadsfri utvärderingsversion prenumerationer är inte berättigad till gränsen eller kvoten ökar. Om du har en kostnadsfri utvärderingsversion kan du uppgradera tooa [betala per användning](https://azure.microsoft.com/offers/ms-azr-0003p/) prenumeration. Mer information finns i [uppgradera kostnadsfri utvärderingsversion av Azure tooPay-som-du-Go](billing/billing-upgrade-azure-subscription.md).
> 

## <a name="limits-and-hello-azure-resource-manager"></a>Gränser och hello Azure Resource Manager
Det är nu möjligt toocombine flera Azure-resurser i tooa enkel Azure-resursgrupp. När du använder resursgrupper hanteras gränser som en gång var globala på regional nivå med hello Azure Resource Manager. Läs mer om Azure-resursgrupper [översikt över Azure Resource Manager](azure-resource-manager/resource-group-overview.md).

I hello gränser nedan, en ny tabell har lagts till tooreflect eventuella skillnader i begränsningar när du använder hello Azure Resource Manager. Det finns till exempel en **prenumerationsbegränsningar** tabell och en **prenumerationsbegränsningar - Azure Resource Manager** tabell. När en gräns gäller tooboth scenarier, visas den bara i hello första tabellen. Om inget annat anges är gränserna globala för alla regioner.

> [!NOTE]
> Det är viktigt tooemphasize att kvoter för Azure-resursgrupper är per region få åtkomst till din prenumeration och är inte per prenumeration eftersom hello service management kvoter. Nu ska vi använda core kvoter som exempel. Om du behöver toorequest en kvot som ökar med stöd för kärnor, måste toodecide hur många kärnor du vill använda toouse i vilka regioner, och sedan en specifik begäran för Azure-resursgrupp core kvoter för hello belopp och regioner som du vill. Därför, om du behöver toouse 30 kärnor i Västeuropa toorun programmet där. Du bör begär 30 kärnor i västra Europa. Men du har inte en kärnkvot som ökar med en annan region – endast Västeuropa har hello 30 kärnor kvoten.
> <!-- -->
> Därför kan det vara användbart tooconsider bestämmer vad din Azure-resursgrupp kvoter behöver toobe för din arbetsbelastning i alla en region, och begäran som belopp i varje region som du planerar distributionen. Se [felsöka distributionsproblem](resource-manager-common-deployment-errors.md) för mer hjälp för att identifiera din aktuella kvoter för vissa regioner.
> 
> 

## <a name="service-specific-limits"></a>Tjänstspecifika gränser
* [Active Directory](#active-directory-limits)
* [API-hantering](#api-management-limits)
* [App Service](#app-service-limits)
* [Application Gateway](#application-gateway-limits)
* [Application Insights](#application-insights-limits)
* [Automation](#automation-limits)
* [Azure Cosmos DB](#azure-cosmos-db-limits)
* [Azure händelse rutnätet](#azure-event-grid-limits)
* [Azure Redis Cache](#azure-redis-cache-limits)
* [Azure RemoteApp](#azure-remoteapp-limits)
* [Säkerhetskopiering](#backup-limits)
* [Batch](#batch-limits)
* [BizTalk-tjänst](#biztalk-services-limits)
* [CDN](#cdn-limits)
* [Cloud Services](#cloud-services-limits)
* [Container Instances](#container-instances-limits)
* [Data Factory](#data-factory-limits)
* [Data Lake Analytics](#data-lake-analytics-limits)
* [Data Lake Store](#data-lake-store-limits)
* [DNS](#dns-limits)
* [Event Hubs](#event-hubs-limits)
* [IoT Hub](#iot-hub-limits)
* [Key Vault](#key-vault-limits)
* [Logga Analytics / Operational Insights](#log-analytics-limits)
* [Media Services](#media-services-limits)
* [Mobilt engagemang](#mobile-engagement-limits)
* [Mobile Services](#mobile-services-limits)
* [Övervaka](#monitor-limits)
* [Multi-Factor Authentication](#multi-factor-authentication)
* [Nätverk](#networking-limits)
* [Nätverksbevakaren](#network-watcher-limits)
* [Notification Hub Service](#notification-hub-service-limits)
* [Resursgrupp](#resource-group-limits)
* [Scheduler](#scheduler-limits)
* [Sök](#search-limits)
* [Service Bus](#service-bus-limits)
* [Site Recovery](#site-recovery-limits)
* [SQL Database](#sql-database-limits)
* [Storage](#storage-limits)
* [StorSimple-System](#storsimple-system-limits)
* [Stream Analytics](#stream-analytics-limits)
* [Prenumeration](#subscription-limits)
* [Traffic Manager](#traffic-manager-limits)
* [Virtual Machines](#virtual-machines-limits)
* [Skaluppsättningar för den virtuella datorn](#virtual-machine-scale-sets-limits)

### <a name="subscription-limits"></a>Prenumerationsbegränsningar
#### <a name="subscription-limits"></a>Prenumerationsbegränsningar
[!INCLUDE [azure-subscription-limits](../includes/azure-subscription-limits.md)]

#### <a name="subscription-limits---azure-resource-manager"></a>Prenumerationsbegränsningar - Azure Resource Manager
hello följande begränsningar gäller när du använder hello Azure Resource Manager och Azure-resursgrupper. Nedan visas inte gränser som inte har ändrats med hello Azure Resource Manager. Dessa begränsningar finns toohello föregående tabell.

Information om hur du hanterar gränser för Resource Manager-begäranden finns [begränsning Resource Manager begär](resource-manager-request-limits.md).

[!INCLUDE [azure-subscription-limits-azure-resource-manager](../includes/azure-subscription-limits-azure-resource-manager.md)]

### <a name="resource-group-limits"></a>Resursgruppen gränser
[!INCLUDE [azure-resource-groups-limits](../includes/azure-resource-groups-limits.md)]

### <a name="virtual-machines-limits"></a>Begränsningar för virtuella datorer
#### <a name="virtual-machine-limits"></a>Begränsningar för virtuell dator
[!INCLUDE [azure-virtual-machines-limits](../includes/azure-virtual-machines-limits.md)]

#### <a name="virtual-machines-limits---azure-resource-manager"></a>Virtuella datorer gränser - Azure Resource Manager
hello följande begränsningar gäller när du använder hello Azure Resource Manager och Azure-resursgrupper. Nedan visas inte gränser som inte har ändrats med hello Azure Resource Manager. Dessa begränsningar finns toohello föregående tabell.

[!INCLUDE [azure-virtual-machines-limits-azure-resource-manager](../includes/azure-virtual-machines-limits-azure-resource-manager.md)]

### <a name="virtual-machine-scale-sets-limits"></a>Skaluppsättningar för den virtuella datorn gränser
[!INCLUDE [virtual-machine-scale-sets-limits](../includes/azure-virtual-machine-scale-sets-limits.md)]

### <a name="container-instances-limits"></a>Behållaren instanser gränser
[!INCLUDE [container-instances-limits](../includes/container-instances-limits.md)]

### <a name="networking-limits"></a>Begränsningar för nätverk
[!INCLUDE [expressroute-limits](../includes/expressroute-limits.md)]

#### <a name="networking-limits"></a>Begränsningar för nätverk
[!INCLUDE [azure-virtual-network-limits](../includes/azure-virtual-network-limits.md)]

#### <a name="application-gateway-limits"></a>Begränsar Programgateway
[!INCLUDE [application-gateway-limits](../includes/application-gateway-limits.md)]

#### <a name="network-watcher-limits"></a>Nätverksbevakaren begränsar
[!INCLUDE [network-watcher-limits](../includes/network-watcher-limits.md)]

#### <a name="traffic-manager-limits"></a>Traffic Manager-gränser
[!INCLUDE [traffic-manager-limits](../includes/traffic-manager-limits.md)]

#### <a name="dns-limits"></a>DNS-begränsningar
[!INCLUDE [dns-limits](../includes/dns-limits.md)]

### <a name="storage-limits"></a>Lagringsgränser
Ytterligare information om lagringskontogränser finns [Azure Storage skalbarhets- och prestandamål](storage/common/storage-scalability-targets.md).
<!--like # storage accts --> 
#### <a name="storage-service-limits"></a>Lagringsgränser för tjänsten
[!INCLUDE [azure-storage-limits](../includes/azure-storage-limits.md)]

<!-- conceptual info about disk limits -- applies toounmanaged and managed -->
#### <a name="virtual-machine-disk-limits"></a>Virtuell disk gränser 
[!INCLUDE [azure-storage-limits-vm-disks](../includes/azure-storage-limits-vm-disks.md)]

Se [storlekar för virtuella datorer](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) för ytterligare information.

#### <a name="managed-virtual-machine-disks"></a>Hanterade virtuella diskar

[!INCLUDE [azure-storage-limits-vm-disks-managed](../includes/azure-storage-limits-vm-disks-managed.md)]

#### <a name="unmanaged-virtual-machine-disks"></a>Ohanterad virtuella diskar

[!INCLUDE [azure-storage-limits-vm-disks-standard](../includes/azure-storage-limits-vm-disks-standard.md)]

[!INCLUDE [azure-storage-limits-vm-disks-premium](../includes/azure-storage-limits-vm-disks-premium.md)]

#### <a name="storage-resource-provider-limits"></a>Begränsar Lagringsresursprovidern
[!INCLUDE [azure-storage-limits-azure-resource-manager](../includes/azure-storage-limits-azure-resource-manager.md)]

### <a name="cloud-services-limits"></a>Cloud Services gränser
[!INCLUDE [azure-cloud-services-limits](../includes/azure-cloud-services-limits.md)]

### <a name="app-service-limits"></a>Gränser för Apptjänst
hello följande gränser för Apptjänst innehåller gränser för Webbappar, Mobilappar, API Apps och Logikappar.

[!INCLUDE [azure-websites-limits](../includes/azure-websites-limits.md)]

### <a name="scheduler-limits"></a>Schemaläggaren gränser
[!INCLUDE [scheduler-limits-table](../includes/scheduler-limits-table.md)]

### <a name="batch-limits"></a>Batch-gränser
[!INCLUDE [azure-batch-limits](../includes/azure-batch-limits.md)]

### <a name="biztalk-services-limits"></a>BizTalk-tjänst gränser
hello visar följande tabell hello gränser för Azure Biztalk-tjänst.

[!INCLUDE [biztalk-services-service-limits](../includes/biztalk-services-service-limits.md)]

### <a name="azure-cosmos-db-limits"></a>Azure DB Cosmos-gränser
Azure Cosmos-DB är en global skala databas i vilken dataflöden och lagringsutrymmen kan skaländras toohandle oavsett krävs för ditt program. Om du har frågor om hello skala Azure Cosmos DB tillhandahåller skickar e-post tooaskcosmosdb@microsoft.com.

### <a name="mobile-engagement-limits"></a>Mobile Engagement gränser
[!INCLUDE [azure-mobile-engagement-limits](../includes/azure-mobile-engagement-limits.md)]

### <a name="search-limits"></a>Sök gränser
Prisnivåer fastställa hello kapacitet och gränser för din söktjänst. Nivåer omfattar:

* *Ledigt* tjänst med flera klienter, delas med andra Azure-prenumeranter avsedd för utvärdering och utveckling av små projekt.
* *Grundläggande* ger dedikerade datorresurser för produktionsarbetsbelastningar i mindre skala, toothree repliker för hög tillgänglighet frågan arbetsbelastningar.
* *Standard (S1, S2, S3, S3 hög densitet)* är för större produktionsarbetsbelastningar. Flera nivåer finns inom hello standardnivån så att du kan välja en resurskonfigurationen som bäst passar din arbetsbelastning profil.

**Gränser per prenumeration**

[!INCLUDE [azure-search-limits-per-subscription](../includes/azure-search-limits-per-subscription.md)]

**Gränser för search-tjänsten**

[!INCLUDE [azure-search-limits-per-service](../includes/azure-search-limits-per-service.md)]

toolearn mer om gränserna för en mer detaljerad nivå, till exempel storlek, frågor per sekund, nycklar, begäranden och svar, se [gränser i Azure Search](search/search-limits-quotas-capacity.md).

### <a name="media-services-limits"></a>Media Services-gränser
[!INCLUDE [azure-mediaservices-limits](../includes/azure-mediaservices-limits.md)]

### <a name="cdn-limits"></a>CDN-gränser
[!INCLUDE [cdn-limits](../includes/cdn-limits.md)]

### <a name="mobile-services-limits"></a>Begränsningar för Mobile Services
[!INCLUDE [mobile-services-limits](../includes/mobile-services-limits.md)]

### <a name="monitor-limits"></a>Övervakaren gränser
[!INCLUDE [monitoring-limits](../includes/monitoring-limits.md)]

### <a name="notification-hub-service-limits"></a>Tjänstbegränsningarna för Notification Hub
[!INCLUDE [notification-hub-limits](../includes/notification-hub-limits.md)]

### <a name="event-hubs-limits"></a>Event Hubs gränser
[!INCLUDE [azure-servicebus-limits](../includes/event-hubs-limits.md)]

### <a name="service-bus-limits"></a>Service Bus-gränser
[!INCLUDE [azure-servicebus-limits](../includes/service-bus-quotas-table.md)]

### <a name="iot-hub-limits"></a>IoT-hubb gränser
[!INCLUDE [azure-iothub-limits](../includes/iot-hub-limits.md)]

### <a name="data-factory-limits"></a>Data Factory begränsar
[!INCLUDE [azure-data-factory-limits](../includes/azure-data-factory-limits.md)]

### <a name="data-lake-analytics-limits"></a>Data Lake Analytics-gränser
[!INCLUDE [azure-data-lake-analytics-limits](../includes/azure-data-lake-analytics-limits.md)]

### <a name="data-lake-store-limits"></a>Data Lake Store-gränser
[!INCLUDE [azure-data-lake-store-limits](../includes/azure-data-lake-store-limits.md)]

### <a name="stream-analytics-limits"></a>Stream Analytics begränsar
[!INCLUDE [stream-analytics-limits-table](../includes/stream-analytics-limits-table.md)]

### <a name="active-directory-limits"></a>Active Directory-gränser
[!INCLUDE [AAD-service-limits](../includes/active-directory-service-limits-include.md)]

### <a name="azure-event-grid-limits"></a>Azure händelse rutnätet gränser
[!INCLUDE [event-grid-limits](../includes/event-grid-limits.md)]

### <a name="azure-remoteapp-limits"></a>Azure RemoteApp-gränser
[!INCLUDE [azure-remoteapp-limits](../includes/azure-remoteapp-limits.md)]

### <a name="storsimple-system-limits"></a>Begränsar StorSimple-System
[!INCLUDE [storsimple-limits-table](../includes/storsimple-limits-table.md)]

### <a name="log-analytics-limits"></a>Begränsar logganalys
[!INCLUDE [operational-insights-limits](../includes/operational-insights-limits.md)]

### <a name="backup-limits"></a>Säkerhetskopiering gränser
[!INCLUDE [azure-backup-limits](../includes/azure-backup-limits.md)]

### <a name="site-recovery-limits"></a>Gränser för Site Recovery
[!INCLUDE [site-recovery-limits](../includes/site-recovery-limits.md)]

### <a name="application-insights-limits"></a>Application Insights gränser
[!INCLUDE [application-insights-limits](../includes/application-insights-limits.md)]

### <a name="api-management-limits"></a>API Management-gränser
[!INCLUDE [api-management-service-limits](../includes/api-management-service-limits.md)]

### <a name="azure-redis-cache-limits"></a>Azure Redis-Cache-gränser
[!INCLUDE [redis-cache-service-limits](../includes/redis-cache-service-limits.md)]

### <a name="key-vault-limits"></a>Key Vault-gränser
[!INCLUDE [key-vault-limits](../includes/key-vault-limits.md)]

### <a name="multi-factor-authentication"></a>Multi-Factor Authentication
[!INCLUDE [azure-mfa-service-limits](../includes/azure-mfa-service-limits.md)]

### <a name="automation-limits"></a>Automation-gränser
[!INCLUDE [automation-limits](../includes/azure-automation-service-limits.md)]

### <a name="sql-database-limits"></a>Begränsar SQL-databas
SQL-databas gränser, se [gränserna för SQL-databasen](sql-database/sql-database-resource-limits.md).

## <a name="see-also"></a>Se även
[Förstå Azure gränser och ökar](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)

[Virtuell dator och Molntjänststorlekar för Azure](virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Storlekar för molntjänster](cloud-services/cloud-services-sizes-specs.md)

