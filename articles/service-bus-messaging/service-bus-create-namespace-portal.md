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
# <a name="create-a-service-bus-namespace-using-hello-azure-portal"></a><span data-ttu-id="0ba26-103">Skapa ett namnområde för Service Bus använder hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0ba26-103">Create a Service Bus namespace using hello Azure portal</span></span>

<span data-ttu-id="0ba26-104">Ett namnområde är en omfångsbehållare för alla meddelandekomponenter.</span><span class="sxs-lookup"><span data-stu-id="0ba26-104">A namespace is a scoping container for all messaging components.</span></span> <span data-ttu-id="0ba26-105">Flera köer och ämnen kan finnas i ett enda namnområde och namnområden fungerar ofta som programbehållare.</span><span class="sxs-lookup"><span data-stu-id="0ba26-105">Multiple queues and topics can reside within a single namespace, and namespaces often serve as application containers.</span></span> <span data-ttu-id="0ba26-106">Det finns två olika sätt toocreate ett Service Bus-namnområde:</span><span class="sxs-lookup"><span data-stu-id="0ba26-106">There are two different ways toocreate a Service Bus namespace:</span></span>

1. <span data-ttu-id="0ba26-107">Azure Portal (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="0ba26-107">Azure portal (this article)</span></span>
2. <span data-ttu-id="0ba26-108">[Resource Manager-mallar][create-namespace-using-arm]</span><span class="sxs-lookup"><span data-stu-id="0ba26-108">[Resource Manager templates][create-namespace-using-arm]</span></span>

## <a name="create-a-namespace-in-hello-azure-portal"></a><span data-ttu-id="0ba26-109">Skapa ett namnområde i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="0ba26-109">Create a namespace in hello Azure portal</span></span>

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

<span data-ttu-id="0ba26-110">Grattis!</span><span class="sxs-lookup"><span data-stu-id="0ba26-110">Congratulations!</span></span> <span data-ttu-id="0ba26-111">Du har nu skapat ett namnområde för meddelandetjänsten i Service Bus.</span><span class="sxs-lookup"><span data-stu-id="0ba26-111">You have now created a Service Bus Messaging namespace.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ba26-112">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0ba26-112">Next steps</span></span>

<span data-ttu-id="0ba26-113">Kolla in våra [GitHub exempel][github-samples], som visar några av hello mer avancerade funktioner i Azure Service Bus-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="0ba26-113">Check out our [GitHub samples][github-samples], which show some of hello more advanced features of Azure Service Bus Messaging.</span></span>

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples
