---
title: "aaaHow toouse Blitline för bearbetning av bilden – Azure funktion guide"
description: "Lär dig hur toouse hello Blitline tjänsten tooprocess bilderna i ett Azure-program."
services: 
documentationcenter: .net
author: blitline-dev
manager: jason@blitline.com
editor: jason@blitline.com
ms.assetid: 6c711248-0e52-4895-ba9e-8395628de924
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2014
ms.author: support@blitline.com
ms.openlocfilehash: 328fd177e25f45f29f8ad8e142d02b46017a858e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-blitline-with-azure-and-azure-storage"></a><span data-ttu-id="93a68-103">Hur toouse Blitline med Azure och Azure Storage</span><span class="sxs-lookup"><span data-stu-id="93a68-103">How toouse Blitline with Azure and Azure Storage</span></span>
<span data-ttu-id="93a68-104">Den här guiden förklarar hur tooaccess Blitline tjänster och hur toosubmit jobb tooBlitline.</span><span class="sxs-lookup"><span data-stu-id="93a68-104">This guide will explain how tooaccess Blitline services and how toosubmit jobs tooBlitline.</span></span>

## <a name="what-is-blitline"></a><span data-ttu-id="93a68-105">Vad är Blitline?</span><span class="sxs-lookup"><span data-stu-id="93a68-105">What is Blitline?</span></span>
<span data-ttu-id="93a68-106">Blitline är en molnbaserad avbildning bearbetning av tjänst som tillhandahåller enterprise nivån avbildningen bearbetning på en bråkdel av hello pris att det skulle kosta toobuild den själv.</span><span class="sxs-lookup"><span data-stu-id="93a68-106">Blitline is a cloud-based image processing service that provides enterprise level image processing at a fraction of hello price that it would cost toobuild it yourself.</span></span>

<span data-ttu-id="93a68-107">hello faktum är att avbildningen bearbetningen har gjorts upprepade gånger, vanligtvis återskapas från grunden hello varje webbplats.</span><span class="sxs-lookup"><span data-stu-id="93a68-107">hello fact is that image processing has been done over and over again, usually rebuilt from hello ground up for each and every website.</span></span> <span data-ttu-id="93a68-108">Vi förstår detta eftersom vi har byggt dem miljoner gånger för.</span><span class="sxs-lookup"><span data-stu-id="93a68-108">We realize this because we’ve built them a million times too.</span></span> <span data-ttu-id="93a68-109">En dag som vi valt kanske är det dags göra vi bara det för alla.</span><span class="sxs-lookup"><span data-stu-id="93a68-109">One day we decided that perhaps it‘s time we just do it for everyone.</span></span> <span data-ttu-id="93a68-110">Vi vet hur toodo den toodo den snabb och effektiv och spara alla fungerar i hello tiden.</span><span class="sxs-lookup"><span data-stu-id="93a68-110">We know how toodo it, toodo it fast and efficiently, and save everyone work in hello meantime.</span></span>

<span data-ttu-id="93a68-111">Mer information finns i [http://www.blitline.com](http://www.blitline.com).</span><span class="sxs-lookup"><span data-stu-id="93a68-111">For more information, see [http://www.blitline.com](http://www.blitline.com).</span></span>

## <a name="what-blitline-is-not"></a><span data-ttu-id="93a68-112">Vad Blitline är inte...</span><span class="sxs-lookup"><span data-stu-id="93a68-112">What Blitline is NOT...</span></span>
<span data-ttu-id="93a68-113">tooclarify vad Blitline är användbart för, är det ofta enklare tooidentify vad Blitline inte utför innan du går vidare.</span><span class="sxs-lookup"><span data-stu-id="93a68-113">tooclarify what Blitline is useful for, it is often easier tooidentify what Blitline does NOT do before moving forward.</span></span>

* <span data-ttu-id="93a68-114">Blitline har inte HTML widgetar tooupload bilder.</span><span class="sxs-lookup"><span data-stu-id="93a68-114">Blitline does NOT have HTML widgets tooupload images.</span></span> <span data-ttu-id="93a68-115">Du måste ha tillgängliga avbildningarna offentligt eller med begränsade behörigheter som är tillgängliga för Blitline tooreach.</span><span class="sxs-lookup"><span data-stu-id="93a68-115">You must have images available publicly or with restricted permissions available for Blitline tooreach.</span></span>
* <span data-ttu-id="93a68-116">Blitline utför inte live avbildningen bearbetning, till exempel Aviary.com</span><span class="sxs-lookup"><span data-stu-id="93a68-116">Blitline does NOT do live image processing, like Aviary.com</span></span>
* <span data-ttu-id="93a68-117">Blitline accepteras inte av bilder, kan inte vara utgivarinitierad bilder-tooBlitline direkt.</span><span class="sxs-lookup"><span data-stu-id="93a68-117">Blitline does NOT accept image uploads, you cannot push your images tooBlitline directly.</span></span> <span data-ttu-id="93a68-118">Du måste push-installera dem tooAzure lagring eller andra platser som har stöd för Blitline och berätta var toogo få dem Blitline.</span><span class="sxs-lookup"><span data-stu-id="93a68-118">You must push them tooAzure Storage or other places Blitline supports and then tell Blitline where toogo get them.</span></span>
* <span data-ttu-id="93a68-119">Blitline är massivt parallell och utför inte någon synkron bearbetning.</span><span class="sxs-lookup"><span data-stu-id="93a68-119">Blitline is massively parallel and does NOT do any synchronous processing.</span></span> <span data-ttu-id="93a68-120">Vilket innebär att du måste ge oss en postback_url och vi kan meddela dig när vi är klar bearbetning.</span><span class="sxs-lookup"><span data-stu-id="93a68-120">Meaning you must give us a postback_url and we can tell you when we are done processing.</span></span>

## <a name="create-a-blitline-account"></a><span data-ttu-id="93a68-121">Skapa ett Blitline-konto</span><span class="sxs-lookup"><span data-stu-id="93a68-121">Create a Blitline account</span></span>
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-toocreate-a-blitline-job"></a><span data-ttu-id="93a68-122">Hur toocreate ett Blitline-jobb</span><span class="sxs-lookup"><span data-stu-id="93a68-122">How toocreate a Blitline job</span></span>
<span data-ttu-id="93a68-123">Blitline använder JSON toodefine hello åtgärder som du vill tootake på en bild.</span><span class="sxs-lookup"><span data-stu-id="93a68-123">Blitline uses JSON toodefine hello actions you want tootake on an image.</span></span> <span data-ttu-id="93a68-124">Den här JSON består av några enkla fält.</span><span class="sxs-lookup"><span data-stu-id="93a68-124">This JSON is composed of a few simple fields.</span></span>

<span data-ttu-id="93a68-125">hello enklaste exemplet är följande:</span><span class="sxs-lookup"><span data-stu-id="93a68-125">hello simplest example is as follows:</span></span>

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

<span data-ttu-id="93a68-126">Här är JSON som tar en avbildning ”src” ”... boys.jpeg” och sedan ändra storlek på det bild too240x140.</span><span class="sxs-lookup"><span data-stu-id="93a68-126">Here we have JSON that will take a "src" image "...boys.jpeg" and then resize that image too240x140.</span></span>

<span data-ttu-id="93a68-127">hello program-ID är något som du hittar i din **ANSLUTNINGSINFORMATION** eller **hantera** fliken på Azure.</span><span class="sxs-lookup"><span data-stu-id="93a68-127">hello Application ID is something you can find in your **CONNECTION INFO** or **MANAGE** tab on Azure.</span></span> <span data-ttu-id="93a68-128">Det är din hemliga identifierare som gör toorun jobb på Blitline.</span><span class="sxs-lookup"><span data-stu-id="93a68-128">It is your secret identifier that allows you toorun jobs on Blitline.</span></span>

<span data-ttu-id="93a68-129">Hej ”spara”-parametern identifierar information om där du vill tooput hello avbildningen när vi har bearbetat den.</span><span class="sxs-lookup"><span data-stu-id="93a68-129">hello "save" parameter identifies information about where you want tooput hello image once we have processed it.</span></span> <span data-ttu-id="93a68-130">Vi har inte definierats någon i det här fallet är trivial.</span><span class="sxs-lookup"><span data-stu-id="93a68-130">In this trivial case, we haven't defined one.</span></span> <span data-ttu-id="93a68-131">Om ingen plats har definierats Blitline lagrar det lokalt (och tillfälligt) på en plats som unikt molnet.</span><span class="sxs-lookup"><span data-stu-id="93a68-131">If no location is defined Blitline will store it locally (and temporarily) at a unique cloud location.</span></span> <span data-ttu-id="93a68-132">Du kommer att kunna tooget som platsen från hello JSON som returneras av Blitline när du gör hello Blitline.</span><span class="sxs-lookup"><span data-stu-id="93a68-132">You will be able tooget that location from hello JSON returned by Blitline when you make hello Blitline.</span></span> <span data-ttu-id="93a68-133">Hej ”bild” identifierare krävs och returneras tooyou när tooidentify sparade den här viss bild.</span><span class="sxs-lookup"><span data-stu-id="93a68-133">hello "image" identifier is required and is returned tooyou when tooidentify this particular saved image.</span></span>

<span data-ttu-id="93a68-134">Du hittar mer information om hello *funktioner* stöder vi här: <http://www.blitline.com/docs/functions></span><span class="sxs-lookup"><span data-stu-id="93a68-134">You can find more information about hello *functions* we support here: <http://www.blitline.com/docs/functions></span></span>

<span data-ttu-id="93a68-135">Du kan också hitta dokumentation om hello alternativ här: <http://www.blitline.com/docs/api></span><span class="sxs-lookup"><span data-stu-id="93a68-135">You can also find documentation about hello job options here: <http://www.blitline.com/docs/api></span></span>

<span data-ttu-id="93a68-136">När du har din JSON är allt du behöver toodo **POST** den för`http://api.blitline.com/job`</span><span class="sxs-lookup"><span data-stu-id="93a68-136">Once you have your JSON all you need toodo is **POST** it too`http://api.blitline.com/job`</span></span>

<span data-ttu-id="93a68-137">Får du JSON tillbaka som ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="93a68-137">You will get JSON back that looks something like this:</span></span>

    {
     "results":
         {"images":
            [{
              "image_identifier":"external_sample_1",
              "s3_url":"https://s3.amazonaws.com/dev.blitline/2011110722/YOUR_APP_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg"
            }],
          "job_id":"4eb8c9f72a50ee2a9900002f"
         }
    }


<span data-ttu-id="93a68-138">Du ser att Blitline har tagit emot din begäran, den har placerats i en kö för bearbetning och när den har slutförts hello avbildningen ska vara tillgängligt vid: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID /CK3f0xBF_2bV6wf7gEZE8w.jpg**</span><span class="sxs-lookup"><span data-stu-id="93a68-138">This tells you that Blitline has recieved your request, it has put it in a processing queue, and when it has completed hello image will be available at: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**</span></span>

## <a name="how-toosave-an-image-tooyour-azure-storage-account"></a><span data-ttu-id="93a68-139">Hur toosave en bild tooyour Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="93a68-139">How toosave an image tooyour Azure Storage account</span></span>
<span data-ttu-id="93a68-140">Om du har ett Azure Storage-konto kan ha du enkelt Blitline push hello bearbetas bilder till din Azure-behållaren.</span><span class="sxs-lookup"><span data-stu-id="93a68-140">If you have an Azure Storage account, you can easily have Blitline push hello processed images into your Azure container.</span></span> <span data-ttu-id="93a68-141">Genom att lägga till en ”azure_destination” definiera hello plats och behörigheter för Blitline toopush till.</span><span class="sxs-lookup"><span data-stu-id="93a68-141">By adding an "azure_destination" you define hello location and permissions for Blitline toopush to.</span></span>

<span data-ttu-id="93a68-142">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="93a68-142">Here is an example:</span></span>

    job : '{
      "application_id": "YOUR_APP_ID",
      "src" : "http://www.google.com/logos/2011/houdini11-hp.jpg",
         "functions" : [{
         "name": "blur",
         "save" : {
             "image_identifier" : "YOUR_IMAGE_IDENTIFIER",
             "azure_destination" : {
                 "account_name" : "YOUR_AZURE_CONTAINER_NAME",
                 "shared_access_signature" : "SAS_THAT_GIVES_BLITLINE_PERMISSION_TO_WRITE_THIS_OBJECT_TO_CONTAINER",
               }
           }
         }]
       }'


<span data-ttu-id="93a68-143">Genom att fylla i hello CAPITALIZED värden med dina egna, kan du skicka den här JSON-toohttp://api.blitline.com/job och hello ”src” bild bearbetas med ett filter för oskärpa och pushas tooyou Azure mål.</span><span class="sxs-lookup"><span data-stu-id="93a68-143">By filling in hello CAPITALIZED values with your own, you can submit this JSON toohttp://api.blitline.com/job and hello "src" image will be processed with a blur filter and then pushed tooyou Azure destination.</span></span>

### <a name="please-note"></a><span data-ttu-id="93a68-144">Obs!</span><span class="sxs-lookup"><span data-stu-id="93a68-144">Please note:</span></span>
<span data-ttu-id="93a68-145">hello SAS måste innehålla hello hela SAS-url, inklusive hello filnamnet hello målfilen.</span><span class="sxs-lookup"><span data-stu-id="93a68-145">hello SAS must contain hello entire SAS url, including hello filename of hello destination file.</span></span>

<span data-ttu-id="93a68-146">Exempel:</span><span class="sxs-lookup"><span data-stu-id="93a68-146">Example:</span></span>

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


<span data-ttu-id="93a68-147">Du kan också läsa hello senaste utgåvan av Blitline's Azure Storage docs [här](http://www.blitline.com/docs/azure_storage).</span><span class="sxs-lookup"><span data-stu-id="93a68-147">You can also read hello latest edition of Blitline's Azure Storage docs [here](http://www.blitline.com/docs/azure_storage).</span></span>

## <a name="next-steps"></a><span data-ttu-id="93a68-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="93a68-148">Next Steps</span></span>
<span data-ttu-id="93a68-149">Besök blitline.com tooread om våra andra funktioner:</span><span class="sxs-lookup"><span data-stu-id="93a68-149">Visit blitline.com tooread about all our other features:</span></span>

* <span data-ttu-id="93a68-150">Blitline API-slutpunkt Docs <http://www.blitline.com/docs/api></span><span class="sxs-lookup"><span data-stu-id="93a68-150">Blitline API Endpoint Docs <http://www.blitline.com/docs/api></span></span>
* <span data-ttu-id="93a68-151">Blitline API-funktioner <http://www.blitline.com/docs/functions></span><span class="sxs-lookup"><span data-stu-id="93a68-151">Blitline API Functions <http://www.blitline.com/docs/functions></span></span>
* <span data-ttu-id="93a68-152">Blitline API exempel <http://www.blitline.com/docs/examples></span><span class="sxs-lookup"><span data-stu-id="93a68-152">Blitline API Examples <http://www.blitline.com/docs/examples></span></span>
* <span data-ttu-id="93a68-153">Tredje Part Nuget biblioteket <http://nuget.org/packages/Blitline.Net></span><span class="sxs-lookup"><span data-stu-id="93a68-153">Third Part Nuget Library <http://nuget.org/packages/Blitline.Net></span></span>

