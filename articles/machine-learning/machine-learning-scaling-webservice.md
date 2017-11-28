---
title: "aaaHow tooincrease samtidighet på en Azure Machine Learning-webbtjänst | Microsoft Docs"
description: "Lär dig hur tooincrease samtidighet på en Azure Machine Learning webbtjänsten genom att lägga till ytterligare slutpunkter."
services: machine-learning
documentationcenter: 
author: neerajkh
manager: srikants
editor: cgronlun
keywords: "Azure maskininlärning webbtjänster, operationalization, skalning, slutpunkt, samtidighet"
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.author: neerajkh
ms.openlocfilehash: e2ad16ec766820a64f36c31232f6a33a79196af4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-an-azure-machine-learning-web-service-by-adding-additional-endpoints"></a><span data-ttu-id="58950-104">Skala en Azure Machine Learning-webbtjänst genom att lägga till ytterligare slutpunkter</span><span class="sxs-lookup"><span data-stu-id="58950-104">Scaling an Azure Machine Learning web service by adding additional endpoints</span></span>
> [!NOTE]
> <span data-ttu-id="58950-105">Det här avsnittet beskrivs tekniker tillämpliga tooa **klassiska** Machine Learning-webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="58950-105">This topic describes techniques applicable tooa **Classic** Machine Learning Web service.</span></span> 
> 
> 

<span data-ttu-id="58950-106">Som standard varje publicerade webbtjänst är konfigurerade toosupport 20 samtidiga begäranden och kan vara så högt som 200 samtidiga förfrågningar.</span><span class="sxs-lookup"><span data-stu-id="58950-106">By default, each published Web service is configured toosupport 20 concurrent requests and can be as high as 200 concurrent requests.</span></span> <span data-ttu-id="58950-107">Medan hello klassiska Azure-portalen innehåller en sätt tooset det här värdet, Azure Machine Learning optimerar automatiskt hello inställningen tooprovide hello bästa prestanda för webbtjänsten och hello portal värdet ignoreras.</span><span class="sxs-lookup"><span data-stu-id="58950-107">While hello Azure classic portal provides a way tooset this value, Azure Machine Learning automatically optimizes hello setting tooprovide hello best performance for your web service and hello portal value is ignored.</span></span> 

<span data-ttu-id="58950-108">Om du planerar toocall hello-API med en högre belastning än maximalt antal samtidiga anrop värdet 200 stöder bör du skapa flera slutpunkter på hello samma webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="58950-108">If you plan toocall hello API with a higher load than a Max Concurrent Calls value of 200 will support, you should create multiple endpoints on hello same Web service.</span></span> <span data-ttu-id="58950-109">Du kan sedan slumpmässigt distribuera din belastningen över alla.</span><span class="sxs-lookup"><span data-stu-id="58950-109">You can then randomly distribute your load across all of them.</span></span>

<span data-ttu-id="58950-110">hello skalning av en webbtjänst är en vanlig aktivitet.</span><span class="sxs-lookup"><span data-stu-id="58950-110">hello scaling of a Web service is a common task.</span></span> <span data-ttu-id="58950-111">Vissa orsaker tooscale är toosupport mer än 200 samtidiga förfrågningar, öka tillgängligheten via flera slutpunkter, eller ange separata slutpunkter för hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="58950-111">Some reasons tooscale are toosupport more than 200 concurrent requests, increase availability through multiple endpoints, or provide separate endpoints for hello web service.</span></span> <span data-ttu-id="58950-112">Du kan öka hello skala genom att lägga till ytterligare slutpunkter för hello samma webbtjänst via [klassiska Azure-portalen](https://manage.windowsazure.com/) eller hello [Azure Machine Learning-webbtjänst](https://services.azureml.net/) portal.</span><span class="sxs-lookup"><span data-stu-id="58950-112">You can increase hello scale by adding additional endpoints for hello same Web service through [Azure classic portal](https://manage.windowsazure.com/) or hello [Azure Machine Learning Web Service](https://services.azureml.net/) portal.</span></span>

<span data-ttu-id="58950-113">Mer information om att lägga till nya slutpunkter finns [skapar slutpunkter](machine-learning-create-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="58950-113">For more information on adding new endpoints, see [Creating Endpoints](machine-learning-create-endpoint.md).</span></span>

<span data-ttu-id="58950-114">Tänk på att med hjälp av ett antal hög samtidighet kan vara skadliga om du inte anropar hello API med en relativt hög hastighet.</span><span class="sxs-lookup"><span data-stu-id="58950-114">Keep in mind that using a high concurrency count can be detrimental if you're not calling hello API with a correspondingly high rate.</span></span> <span data-ttu-id="58950-115">Du kan se sporadiska timeout och/eller toppar i hello latens om du placerar en relativt låg belastning på ett API som konfigurerats för hög belastning.</span><span class="sxs-lookup"><span data-stu-id="58950-115">You might see sporadic timeouts and/or spikes in hello latency if you put a relatively low load on an API configured for high load.</span></span>

<span data-ttu-id="58950-116">hello synkron API: er används vanligtvis i situationer där en låg latens önskas.</span><span class="sxs-lookup"><span data-stu-id="58950-116">hello synchronous APIs are typically used in situations where a low latency is desired.</span></span> <span data-ttu-id="58950-117">Svarstiden här innebär hello tid det tar för en begäran för hello API toocomplete och inte kontot för alla fördröjningar i nätverket.</span><span class="sxs-lookup"><span data-stu-id="58950-117">Latency here implies hello time it takes for hello API toocomplete one request, and doesn't account for any network delays.</span></span> <span data-ttu-id="58950-118">Anta att du har ett API med en 50 ms fördröjning.</span><span class="sxs-lookup"><span data-stu-id="58950-118">Let's say you have an API with a 50-ms latency.</span></span> <span data-ttu-id="58950-119">toofully använda hello tillgänglig kapacitet med begränsningen nivån hög och maximalt antal samtidiga anrop = 20, behöver du toocall detta API 20 * 1000 / 50 = 400 gånger per sekund.</span><span class="sxs-lookup"><span data-stu-id="58950-119">toofully consume hello available capacity with throttle level High and Max Concurrent Calls = 20, you need toocall this API 20 * 1000 / 50 = 400 times per second.</span></span> <span data-ttu-id="58950-120">Utöka detta ytterligare kan ett maximalt antal samtidiga anrop 200 du toocall hello API 4000 gånger per sekund, förutsatt att en 50 ms fördröjning.</span><span class="sxs-lookup"><span data-stu-id="58950-120">Extending this further, a Max Concurrent Calls of 200 allows you toocall hello API 4000 times per second, assuming a 50-ms latency.</span></span>

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
