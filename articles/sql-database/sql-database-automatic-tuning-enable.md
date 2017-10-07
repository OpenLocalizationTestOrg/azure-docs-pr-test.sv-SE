---
title: "aaaEnable automatisk inställning för Azure SQL Database | Microsoft Docs"
description: "Du kan aktivera automatisk justering på Azure SQL Database enkelt."
services: sql-database
documentationcenter: 
author: vvasic
manager: drasumic
editor: vvasic
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: NA
ms.date: 06/05/2016
ms.author: vvasic
ms.openlocfilehash: af9da161eabc0f8c4cb100c050288f234efb8093
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-automatic-tuning"></a>Aktivera automatisk inställning

Azure SQL Database är en automatiskt hanterade datatjänst som ständigt övervakar dina frågor och identifierar hello åtgärd som du kan utföra tooimprove prestandan för din arbetsbelastning. Du kan granska rekommendationer och manuellt koppla dem, eller låta Azure SQL Database automatiskt tillämpa korrigerande åtgärder – den här funktionen kallas **automatiskt prestandajustering läge**. Automatisk justering kan aktiveras på hello server eller hello i databasen.

## <a name="enable-automatic-tuning-on-server"></a>Aktivera automatisk justering på servern

tooenable automatisk justering på Azure SQL Database-server, navigera toohello server i Azure portal och välj sedan **automatisk justering** hello-menyn. Välj hello alternativ för automatisk justering tooenable och markera **Verkställ**:

![Server](./media/sql-database-automatic-tuning-enable/server.png)

Automatisk justering alternativ på servern är tillämpade tooall databaser på hello-servern. Som standard alla databaser ärver hello konfiguration från sina överordnade servern, men detta kan åsidosättas och anges separat för varje databas.

## <a name="configure-automatic-tuning-on-database"></a>Konfigurera automatisk inställning för databasen

hello Azure portal aktiverar du tooindividually ange hello automatisk justering konfiguration på varje databas.

> [!NOTE]
> hello allmänna rekommendationer är toomanage hello automatisk justering konfigurationen på servernivå så hello samma inställningar kan tillämpas på varje databas automatiskt. Konfigurera automatisk inställning på en individuell databas om hello databasen skiljer sig att hello andra på samma server.
>

automatisk inställning på en enskild databas tooenable navigera toohello databasen i hello Azure-portalen och sedan väljer **automatisk justering**. Du kan konfigurera en enskild databas tooinherit hello inställningar från hello databasen genom att markera kryssrutan hello eller du kan ange hello konfiguration för en databas individuellt.

![Databas](./media/sql-database-automatic-tuning-enable/database.png)

När du har valt rätt konfiguration, klickar du på **tillämpa**.

## <a name="next-steps"></a>Nästa steg
* Läs hello [automatisk justering artikel](sql-database-automatic-tuning.md) toolearn mer om automatisk justering och hur det kan hjälpa dig att förbättra prestandan.
* Se [rekommendationer](sql-database-advisor.md) en översikt över Azure SQL Database prestanda rekommendationer.
* Se [insikter i frågeprestanda](sql-database-query-performance.md) toolearn om hur du visar hello prestandapåverkan top-frågor.
