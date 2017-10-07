---
title: "aaaGuide toocreating en lösningsmall för hello Marketplace | Microsoft Docs"
description: "Detaljerade anvisningar hur toocreate, certifiera och distribuera en Multi-VM avbildningen Lösningsmall för inköp på hello Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e14e05f2-2385-4ce0-b351-0747cb74ba19
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2016
ms.author: hascipio; v-divte
ms.openlocfilehash: b0e7067176337dd0d3f6f6ec04c963f80f706ab0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guide-toocreate-a-solution-template-for-azure-marketplace"></a>Guiden toocreate en för lösningsmall för Azure Marketplace
När du har slutfört steg 1, [skapande av konton och registrering][link-acct-creation], vi Interaktiv du på hello skapande av en Azure-kompatibel lösningsmall på [tekniska krav för att skapa en lösningsmall](marketplace-publishing-solution-template-creation-prerequisites.md). Nu går du igenom hello steg för att skapa en lösningsmall för för flera virtuella datorer på hello [Publiceringsportal] [ link-pubportal] för hello Azure Marketplace.

## <a name="create-your-solution-template-offer-in-hello-publishing-portal"></a>Skapa lösningen mallen erbjudandet i hello Publishing Portal
Gå för [https://publish.windowsazure.com](http://publish.windowsazure.com). När du loggar in för första gången toohello för hello [Publiceringsportal](https://publish.windowsazure.com/), Använd hello samma konto med företagets säljare profil har registrerats. Senare kan du lägga till anställda på företaget som medadministratör i hello Publishing Portal.

### <a name="1-select-solution-templates"></a>1. Välj ”lösningsmallar”
  ![Rita][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. Skapa en ny lösningsmall
  ![Rita][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. Börja med topologier
En lösningsmall är en ”överordnad” tooall dess topologier. Du kan definiera flera topologier i en erbjudande-/lösningsmall. När ett erbjudande pushas toostaging pushas den med alla sina topologier. Gör hello nedan toodefine erbjudandet:     

* Skapa en topologi: ”ID” är vanligtvis hello namnet på hello topologi för hello lösningsmall. hello-ID används i hello URL som visas nedan:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure-portalen: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}
* Lägg till en ny version.

### <a name="4-get-your-topology-versions-certified"></a>4. Hämta din topologi-versioner som är certifierade
Överför en zip-fil som innehåller alla nödvändiga filer tooprovision att viss version av hello-topologi. Den här zipfilen måste innehålla hello följande:

* *mainTemplate.json* och *createUiDefinition.json* fil på dess rotkatalog.
* Alla länkade mallar och alla nödvändiga skript.

  > [!TIP]
  > När utvecklarna arbetar på att skapa hello lösning mallen topologier och få dem certifierad, hello business kan marknadsföring och/eller juridiska avdelningar inom företaget arbeta med hello marknadsföring och juridiska innehåll.
  >
  >

## <a name="next-steps"></a>Nästa steg
Nu när du har skapat din lösning och överföra hello zip-filen följer du anvisningarna hello i hello [Marketplace marketing content guiden](marketplace-publishing-push-to-staging.md) innan du skickar hello erbjudande toostaging. toosee hello fullständig uppsättning marketplace publicera artiklar, besök [komma igång: hur toopublish ett erbjudande toohello Azure Marketplace](marketplace-publishing-getting-started.md).

Du kan också vara intresserad av dessa relaterade artiklar:

* VM-avbildningar: [om avbildningar av virtuella datorer i Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
* VM-tillägg: [VM-agenten och VM-tillägg översikt](https://msdn.microsoft.com/library/azure/dn832621.aspx) och [Azure VM-tillägg och funktioner](https://msdn.microsoft.com/library/azure/dn606311.aspx)
* Azure Resource Manager: [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md) och [enkel mall-exempel](https://github.com/rjmax/ArmExamples)
* Storage-konto begränsar: [hur tooMonitor för Storage-konto begränsning](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) och [Premium-lagring](../storage/common/storage-premium-storage.md#scalability-and-performance-targets)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
