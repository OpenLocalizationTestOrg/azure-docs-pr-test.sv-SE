---
title: aaaHow toocreate en Service Bus-namnrymd i hello Azure-portalen | Microsoft Docs
description: "Skapa ett namnområde för Service Bus använder hello Azure-portalen."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: d8907e7e4a804056f6d66d5a177d9ace967ed2ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-bus-namespace-using-hello-azure-portal"></a>Skapa ett namnområde för Service Bus använder hello Azure-portalen

Ett namnområde är en omfångsbehållare för alla meddelandekomponenter. Flera köer och ämnen kan finnas i ett enda namnområde och namnområden fungerar ofta som programbehållare. Det finns två olika sätt toocreate ett Service Bus-namnområde:

1. Azure Portal (den här artikeln)
2. [Resource Manager-mallar][create-namespace-using-arm]

## <a name="create-a-namespace-in-hello-azure-portal"></a>Skapa ett namnområde i hello Azure-portalen

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Grattis! Du har nu skapat ett namnområde för meddelandetjänsten i Service Bus.

## <a name="next-steps"></a>Nästa steg

Kolla in våra [GitHub exempel][github-samples], som visar några av hello mer avancerade funktioner i Azure Service Bus-meddelanden.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples
