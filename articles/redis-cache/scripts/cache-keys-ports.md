---
title: "aaaAzure CLI skriptexempel - Get hello värdnamnet, portarna och nycklarna för Azure Redis-Cache | Microsoft Docs"
description: "Azure CLI skriptexempel - Get hello värdnamnet, portarna och nycklarna för en instans av Azure Redis-Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
tags: azure-service-management
ms.assetid: 761eb24e-2ba7-418d-8fc3-431153e69a90
ms.service: cache-redis
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 04/14/2017
ms.author: sdanie
ms.openlocfilehash: e6e794558087d6568438c439e2bf99fc46eeb8bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-hostname-ports-and-keys-for-azure-redis-cache"></a>Hämta hello värdnamnet, portarna och nycklarna för Azure Redis-Cache

I det här scenariot kan du lära dig hur tooretrieve hello värdnamnet, portarna och nycklarna användas tooconnect tooan Azure Redis-cacheinstansen.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli[main](../../../cli_scripts/redis-cache/cache-keys-ports/cache-keys-ports.sh "Azure Redis Cache")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon tooretrieve hello värdnamn, nycklar och portar för en Azure Redis-Cache-instans. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [AZ redis visa](https://docs.microsoft.com/cli/azure/redis#show) | Hämta information om Azure Redis-Cache-instans. |
| [AZ redis lista nycklar](https://docs.microsoft.com/cli/azure/redis#list-keys) | Hämta åtkomstnycklarna för en Azure Redis-Cache-instans. |


## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Azure Redis-Cache CLI skriptexempel finns i hello [dokumentation för Azure Redis-Cache](../cli-samples.md).
