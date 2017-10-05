---
title: "Hantera Redis-cache med hjälp av Azure-Explorer för IntelliJ | Microsoft Docs"
description: "Lär dig mer om att hantera Azure redis-cache med hjälp av Azure-Explorer för IntelliJ."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 06/14/2017
ms.author: robmcm
ms.openlocfilehash: 9ab8ae17ee2a92b5b16d2210366c00b5b8023fa8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="managing-redis-caches-using-the-azure-explorer-for-intellij"></a>Hantera Redis-cache med hjälp av Azure-Explorer för IntelliJ

Azure-Explorer, som är en del av Azure-verktygen för IntelliJ ger Java-utvecklare med ett enkelt att använda lösning för hantering av redis-cache i Azure kontot från inuti IntelliJ IDE.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a>Skapa ett Redis-Cache med hjälp av IntelliJ

Följande steg vägleder dig genom stegen för att skapa ett redis-cache med hjälp av Azure-Explorer.

1. Logga in på ditt Azure-konto med hjälp av stegen i den [logga i instruktioner för Azure-verktygen för IntelliJ] artikel.

1. I den **Azure Explorer** verktyget fönster, expandera den **Azure** noden, högerklickar du på **Redis-cache**, och klicka sedan på **skapa Redis-Cache**.

   ![Skapa Redis-Cache-menyn][CR01]

1. När den **nytt Redis-Cache** i dialogrutan anger du följande alternativ:

   ![Skapa nytt Redis-Cache-dialogruta][CR02]

   a. **DNS-namnet**: Anger DNS-underdomänen för den nya redis-cache, vilket till före ”. redis.cache.windows.net”, till exempel: *wingtiptoys.redis.cache.windows.net*.

   b. **Prenumerationen**: Anger den Azure-prenumeration du vill använda för nya redis-cache.

   c. **Resursgruppen**: Anger resursgruppen för redis-cache, måste du välja något av följande alternativ:
      * **Skapa ny**: Anger att du vill skapa en ny resursgrupp.
      * **Använd befintlig**: Anger att du ska välja från en lista över resursgrupper som är associerade med ditt Azure-konto.

   d. **Plats**: Anger platsen där redis-cache skapas, till exempel *västra USA*.

   e. **Prisnivån**: Anger vilken prisnivå redis-cache använder; den här inställningen avgör antalet klientanslutningar. (Mer information finns i [Redis-Cache priser].)

   f. **Icke-SSL-porten**: Anger om redis-cache kan icke-SSL-anslutningar; SSL-anslutningar tillåts som standard.

1. När du har angett alla redis cache-inställningar, klickar du på **OK**.

När redis-cache har skapats visas den i Azure-Explorer.

   ![Redis-Cache i Azure Explorer][CR03]

> [!NOTE]
>
> Mer information om hur du konfigurerar din Azure redis-cache-inställningar, se [hur du konfigurerar Azure Redis-Cache].
>

## <a name="display-the-properties-for-your-redis-cache-in-intellij"></a>Visa egenskaperna för din Redis-Cache i IntelliJ

1. Högerklicka på redis-cache i Azure-Explorer och klicka på **visa egenskaper**.

   ![Azure Explorer snabbmenyn att visa egenskaper för ett redis-cache][SP01]

1. Azure-Explorer visar egenskaper för redis-cache.

   ![Egenskaper för Redis-cache][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a>Ta bort Redis-Cache med IntelliJ

1. Högerklicka på redis-cache i Azure-Explorer och klicka på **ta bort**.

   ![Azure Explorer snabbmenyn att ta bort ett redis-cache][DE01]

1. Klicka på **Ja** när du uppmanas att ta bort redis-cache.

   ![Ta bort redis-cache-fråga][DE02]

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

Mer information om Azure redis-cache, konfigurationsinställningar och priser finns i följande länkar:

* [Azure Redis Cache]
* [Redis-Cache-dokumentation]
* [Redis-Cache priser]
* [hur du konfigurerar Azure Redis-Cache]

<!-- URL List -->

[Redis-Cache priser]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Redis-Cache-dokumentation]: ./redis-cache/index.md
[hur du konfigurerar Azure Redis-Cache]: ./redis-cache/cache-configure.md
[logga i instruktioner för Azure-verktygen för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
