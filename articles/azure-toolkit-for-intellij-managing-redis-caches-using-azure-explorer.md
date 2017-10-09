---
title: "aaaManaging Redis-cache med hjälp av hello Azure Explorer för IntelliJ | Microsoft Docs"
description: "Lär dig hur toomanage din Azure redis cachelagrar med hello Azure Explorer för IntelliJ."
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
ms.openlocfilehash: 76ba37a2a35c26d0045e17003181108992eb957d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-redis-caches-using-hello-azure-explorer-for-intellij"></a>Hantera Redis-cache med hjälp av hello Azure Explorer för IntelliJ

hello Azure Explorer, som är en del av hello Azure Toolkit för IntelliJ, tillhandahåller Java-utvecklare med en enkel att använda lösning för hantering av redis-cache i Azure kontot från inuti hello IntelliJ IDE.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="create-a-redis-cache-by-using-intellij"></a>Skapa ett Redis-Cache med hjälp av IntelliJ

hello följande steg vägleder dig genom hello steg toocreate ett redis-cache med hjälp av hello Azure Explorer.

1. Logga in tooyour Azure-konto med hello steg i hello [logga i instruktioner för hello Azure Toolkit för IntelliJ] artikel.

1. I hello **Azure Explorer** verktyget fönster, expandera hello **Azure** noden, högerklickar du på **Redis-cache**, och klicka sedan på **skapa Redis-Cache**.

   ![Skapa Redis-Cache-menyn][CR01]

1. När hello **nytt Redis-Cache** i dialogrutan Ange hello följande alternativ:

   ![Skapa nytt Redis-Cache-dialogruta][CR02]

   a. **DNS-namnet**: Anger hello DNS-underdomänen för hello nya redis-cache som är inledd för ”. redis.cache.windows .net”, till exempel: *wingtiptoys.redis.cache.windows.net*.

   b. **Prenumerationen**: Anger hello Azure-prenumeration du vill använda toouse för hello nya redis-cache.

   c. **Resursgruppen**: Anger hello resursgruppen för redis-cache, behöver du toochoose något av följande alternativ för hello:
      * **Skapa ny**: Anger att du vill toocreate en ny resursgrupp.
      * **Använd befintlig**: Anger att du ska välja från en lista över resursgrupper som är associerade med ditt Azure-konto.

   d. **Plats**: Anger hello plats där redis-cache skapas, till exempel *västra USA*.

   e. **Prisnivån**: Anger vilken prisnivå redis-cache använder; den här inställningen avgör hello antalet klientanslutningar. (Mer information finns i [Redis-Cache priser].)

   f. **Icke-SSL-porten**: Anger om redis-cache kan icke-SSL-anslutningar; SSL-anslutningar tillåts som standard.

1. När du har angett alla redis cache-inställningar, klickar du på **OK**.

När redis-cache har skapats visas den i hello Azure Explorer.

   ![Redis-Cache i Azure Explorer][CR03]

> [!NOTE]
>
> Mer information om hur du konfigurerar din Azure redis-cache-inställningar, se [hur tooconfigure Azure Redis-Cache].
>

## <a name="display-hello-properties-for-your-redis-cache-in-intellij"></a>Visa hello egenskaper för din Redis-Cache i IntelliJ

1. I hello Azure Explorer, högerklicka på redis-cache och klickar på **visa egenskaper**.

   ![Azure Explorer kontexten menyn toodisplay egenskaper för ett redis-cache][SP01]

1. hello Azure Explorer visar hello egenskaper för redis-cache.

   ![Egenskaper för Redis-cache][SP02]

## <a name="delete-your-redis-cache-by-using-intellij"></a>Ta bort Redis-Cache med IntelliJ

1. I hello Azure Explorer, högerklicka på redis-cache och klickar på **ta bort**.

   ![Azure Explorer kontexten menyn toodelete ett redis-cache][DE01]

1. Klicka på **Ja** när du uppmanas till detta toodelete redis-cache.

   ![Ta bort redis-cache-fråga][DE02]

## <a name="next-steps"></a>Nästa steg

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

Mer information om Azure redis-cache, konfigurationsinställningar och priser finns i hello följande länkar:

* [Azure Redis Cache]
* [Redis-Cache-dokumentation]
* [Redis-Cache priser]
* [hur tooconfigure Azure Redis-Cache]

<!-- URL List -->

[Redis-Cache priser]: https://azure.microsoft.com/pricing/details/cache/
[Azure Redis Cache]: https://azure.microsoft.com/services/cache/
[Redis-Cache-dokumentation]: ./redis-cache/index.md
[hur tooconfigure Azure Redis-Cache]: ./redis-cache/cache-configure.md
[logga i instruktioner för hello Azure Toolkit för IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md

<!-- IMG List -->

[CR01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR01.png
[CR02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR02.png
[CR03]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/CR03.png

[SP01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP01.png
[SP02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/SP02.png

[DE01]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE01.png
[DE02]: ./media/azure-toolkit-for-intellij-managing-redis-caches-using-azure-explorer/DE02.png
