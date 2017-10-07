---
title: aaaConsume Azure hanterade program i marketplace | Microsoft Docs
description: "Describeshow toocreate ett Azure hanterade program som är tillgängliga via hello Marketplace."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/11/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: 9ae6e11a3f63eb58a9f3199364b5606a7afe5618
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="consume-azure-managed-applications-in-hello-marketplace"></a>Använda Azure hanterade program i hello Marketplace

Enligt beskrivningen i hello [hanteras Programöversikt](managed-application-overview.md) artikel, det finns två scenarier i hello slutet tooend upplevelsen. En är hello utgivare eller leverantören som vill toocreate ett hanterat program för användning av kunder. hello är andra hello slutet kund eller hello konsumenter av hello hanterade program. Den här artikeln beskriver hello andra scenariot. Beskriver hur du distribuerar ett hanterat program från hello Microsoft Azure Marketplace.

## <a name="create-from-hello-marketplace"></a>Skapa från hello Marketplace

Distribuera ett hanterat program från hello Marketplace är liknande toodeploying någon typ av resurser från hello Marketplace. 

I hello portal, väljer **+ ny** och Sök efter hello typ av lösningen toodeploy. Hello tillgängliga alternativ, Välj hello som du behöver.

![söklösningar](./media/managed-application-consume-marketplace/search-apps.png)

Granska hello sammanfattning av programmet hello och välj **skapa**.

![Skapa hanterade program](./media/managed-application-consume-marketplace/create-marketplace-managed-app.png)

Precis som alla andra lösningar visas för fält tooprovide värden för. De här fälten varierar beroende på hello typ av hanterade program som du skapar. 

## <a name="view-support-information"></a>Visa information om support

Visa hello supportinformation för hello programmet när det hanterade programmet har distribuerats. I bladet hanterade programmet hello visas hello supportinformation.

![Support](./media/managed-application-consume-marketplace/support.png)

## <a name="view-publisher-permissions"></a>Visa publisher behörigheter

hello-leverantör som hanterar ditt program har beviljats åtkomst tooyour resurser. toosee dessa behörigheter och välj **tillstånd**.

![Tillstånd](./media/managed-application-consume-marketplace/authorizations.png)

## <a name="next-steps"></a>Nästa steg

* Information om hur du publicerar ett hanterat program i hello Marketplace finns [Azure hanterade program i Marketplace](managed-application-author-marketplace.md).
* toopublish hanterade program som är endast tillgänglig toousers i din organisation, se [skapa och publicera katalog hanteras tjänstprogrammet](managed-application-publishing.md).
* tooconsume hanterade program som är endast tillgänglig toousers i din organisation, se [använda en katalog hanteras tjänstprogrammet](managed-application-consumption.md).
