---
title: "Hur du använder Blitline för avbildningen bearbetning - Azure funktion guide"
description: "Lär dig hur du använder tjänsten Blitline för att behandla bilder i ett Azure-program."
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
ms.openlocfilehash: 1d90599e028b3407a513b04b878e3aefc39928a2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-blitline-with-azure-and-azure-storage"></a><span data-ttu-id="76808-103">Hur du använder Blitline med Azure och Azure Storage</span><span class="sxs-lookup"><span data-stu-id="76808-103">How to use Blitline with Azure and Azure Storage</span></span>
<span data-ttu-id="76808-104">Den här guiden förklarar hur du komma åt Blitline tjänster och skicka jobb till Blitline.</span><span class="sxs-lookup"><span data-stu-id="76808-104">This guide will explain how to access Blitline services and how to submit jobs to Blitline.</span></span>

## <a name="what-is-blitline"></a><span data-ttu-id="76808-105">Vad är Blitline?</span><span class="sxs-lookup"><span data-stu-id="76808-105">What is Blitline?</span></span>
<span data-ttu-id="76808-106">Blitline är en molnbaserad avbildning bearbetning av tjänst som tillhandahåller enterprise nivån avbildningen bearbetning på en bråkdel av det pris som den skulle kostnaden för att skapa den själv.</span><span class="sxs-lookup"><span data-stu-id="76808-106">Blitline is a cloud-based image processing service that provides enterprise level image processing at a fraction of the price that it would cost to build it yourself.</span></span>

<span data-ttu-id="76808-107">Faktum är att avbildningen bearbetningen har gjorts upprepade gånger, vanligtvis återskapas från grunden för varje webbplats.</span><span class="sxs-lookup"><span data-stu-id="76808-107">The fact is that image processing has been done over and over again, usually rebuilt from the ground up for each and every website.</span></span> <span data-ttu-id="76808-108">Vi förstår detta eftersom vi har byggt dem miljoner gånger för.</span><span class="sxs-lookup"><span data-stu-id="76808-108">We realize this because we’ve built them a million times too.</span></span> <span data-ttu-id="76808-109">En dag som vi valt kanske är det dags göra vi bara det för alla.</span><span class="sxs-lookup"><span data-stu-id="76808-109">One day we decided that perhaps it‘s time we just do it for everyone.</span></span> <span data-ttu-id="76808-110">Vi vet hur du gör den att göra det snabbt och effektivt och spara alla arbets under tiden.</span><span class="sxs-lookup"><span data-stu-id="76808-110">We know how to do it, to do it fast and efficiently, and save everyone work in the meantime.</span></span>

<span data-ttu-id="76808-111">Mer information finns i [http://www.blitline.com](http://www.blitline.com).</span><span class="sxs-lookup"><span data-stu-id="76808-111">For more information, see [http://www.blitline.com](http://www.blitline.com).</span></span>

## <a name="what-blitline-is-not"></a><span data-ttu-id="76808-112">Vad Blitline är inte...</span><span class="sxs-lookup"><span data-stu-id="76808-112">What Blitline is NOT...</span></span>
<span data-ttu-id="76808-113">För att förtydliga vad Blitline är användbart för, är det ofta enklare att identifiera vad Blitline utför inte innan du går vidare.</span><span class="sxs-lookup"><span data-stu-id="76808-113">To clarify what Blitline is useful for, it is often easier to identify what Blitline does NOT do before moving forward.</span></span>

* <span data-ttu-id="76808-114">Blitline har inte HTML-widgetar att ladda upp bilder.</span><span class="sxs-lookup"><span data-stu-id="76808-114">Blitline does NOT have HTML widgets to upload images.</span></span> <span data-ttu-id="76808-115">Du måste ha tillgängliga avbildningarna offentligt eller med begränsade behörigheter som är tillgängliga för Blitline att nå.</span><span class="sxs-lookup"><span data-stu-id="76808-115">You must have images available publicly or with restricted permissions available for Blitline to reach.</span></span>
* <span data-ttu-id="76808-116">Blitline utför inte live avbildningen bearbetning, till exempel Aviary.com</span><span class="sxs-lookup"><span data-stu-id="76808-116">Blitline does NOT do live image processing, like Aviary.com</span></span>
* <span data-ttu-id="76808-117">Blitline accepteras inte av bilder, kan inte vara utgivarinitierad bilderna till Blitline direkt.</span><span class="sxs-lookup"><span data-stu-id="76808-117">Blitline does NOT accept image uploads, you cannot push your images to Blitline directly.</span></span> <span data-ttu-id="76808-118">Du måste push-installera dem till Azure Storage eller andra platser har stöd för Blitline och berätta Blitline var hämta dem.</span><span class="sxs-lookup"><span data-stu-id="76808-118">You must push them to Azure Storage or other places Blitline supports and then tell Blitline where to go get them.</span></span>
* <span data-ttu-id="76808-119">Blitline är massivt parallell och utför inte någon synkron bearbetning.</span><span class="sxs-lookup"><span data-stu-id="76808-119">Blitline is massively parallel and does NOT do any synchronous processing.</span></span> <span data-ttu-id="76808-120">Vilket innebär att du måste ge oss en postback_url och vi kan meddela dig när vi är klar bearbetning.</span><span class="sxs-lookup"><span data-stu-id="76808-120">Meaning you must give us a postback_url and we can tell you when we are done processing.</span></span>

## <a name="create-a-blitline-account"></a><span data-ttu-id="76808-121">Skapa ett Blitline-konto</span><span class="sxs-lookup"><span data-stu-id="76808-121">Create a Blitline account</span></span>
[!INCLUDE [blitline-signup](../includes/blitline-signup.md)]

## <a name="how-to-create-a-blitline-job"></a><span data-ttu-id="76808-122">Så här skapar du ett Blitline-jobb</span><span class="sxs-lookup"><span data-stu-id="76808-122">How to create a Blitline job</span></span>
<span data-ttu-id="76808-123">Blitline använder JSON för att definiera de åtgärder som du vill ta med en bild.</span><span class="sxs-lookup"><span data-stu-id="76808-123">Blitline uses JSON to define the actions you want to take on an image.</span></span> <span data-ttu-id="76808-124">Den här JSON består av några enkla fält.</span><span class="sxs-lookup"><span data-stu-id="76808-124">This JSON is composed of a few simple fields.</span></span>

<span data-ttu-id="76808-125">Det enklaste exemplet är följande:</span><span class="sxs-lookup"><span data-stu-id="76808-125">The simplest example is as follows:</span></span>

        json : '{
       "application_id": "MY_APP_ID",
       "src" : "http://cdn.blitline.com/filters/boys.jpeg",
       "functions" : [ {
           "name": "resize_to_fit",
           "params" : { "width": 240, "height": 140 },
           "save" : { "image_identifier" : "external_sample_1" }
       } ]
    }'

<span data-ttu-id="76808-126">Här är JSON som tar en avbildning ”src” ”... boys.jpeg” och sedan ändra storlek på avbildningen till 240 x 140.</span><span class="sxs-lookup"><span data-stu-id="76808-126">Here we have JSON that will take a "src" image "...boys.jpeg" and then resize that image to 240x140.</span></span>

<span data-ttu-id="76808-127">Program-ID är något som du hittar i din **ANSLUTNINGSINFORMATION** eller **hantera** fliken på Azure.</span><span class="sxs-lookup"><span data-stu-id="76808-127">The Application ID is something you can find in your **CONNECTION INFO** or **MANAGE** tab on Azure.</span></span> <span data-ttu-id="76808-128">Det är din hemliga identifierare som gör det möjligt att köra på Blitline.</span><span class="sxs-lookup"><span data-stu-id="76808-128">It is your secret identifier that allows you to run jobs on Blitline.</span></span>

<span data-ttu-id="76808-129">Parametern ”spara” identifierar information om var du vill placera avbildningen när vi har bearbetat den.</span><span class="sxs-lookup"><span data-stu-id="76808-129">The "save" parameter identifies information about where you want to put the image once we have processed it.</span></span> <span data-ttu-id="76808-130">Vi har inte definierats någon i det här fallet är trivial.</span><span class="sxs-lookup"><span data-stu-id="76808-130">In this trivial case, we haven't defined one.</span></span> <span data-ttu-id="76808-131">Om ingen plats har definierats Blitline lagrar det lokalt (och tillfälligt) på en plats som unikt molnet.</span><span class="sxs-lookup"><span data-stu-id="76808-131">If no location is defined Blitline will store it locally (and temporarily) at a unique cloud location.</span></span> <span data-ttu-id="76808-132">Du kommer att kunna hämta den platsen från JSON som returneras av Blitline när du gör Blitline.</span><span class="sxs-lookup"><span data-stu-id="76808-132">You will be able to get that location from the JSON returned by Blitline when you make the Blitline.</span></span> <span data-ttu-id="76808-133">Identifieraren ”bild” krävs och returneras till dig när att identifiera den här viss spara bilden.</span><span class="sxs-lookup"><span data-stu-id="76808-133">The "image" identifier is required and is returned to you when to identify this particular saved image.</span></span>

<span data-ttu-id="76808-134">Du hittar mer information om den *funktioner* stöder vi här: <http://www.blitline.com/docs/functions></span><span class="sxs-lookup"><span data-stu-id="76808-134">You can find more information about the *functions* we support here: <http://www.blitline.com/docs/functions></span></span>

<span data-ttu-id="76808-135">Du kan också hitta dokumentation om jobbet alternativen: <http://www.blitline.com/docs/api></span><span class="sxs-lookup"><span data-stu-id="76808-135">You can also find documentation about the job options here: <http://www.blitline.com/docs/api></span></span>

<span data-ttu-id="76808-136">När du har din JSON är allt du behöver göra **POST** att`http://api.blitline.com/job`</span><span class="sxs-lookup"><span data-stu-id="76808-136">Once you have your JSON all you need to do is **POST** it to `http://api.blitline.com/job`</span></span>

<span data-ttu-id="76808-137">Får du JSON tillbaka som ser ut ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="76808-137">You will get JSON back that looks something like this:</span></span>

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


<span data-ttu-id="76808-138">Du ser att Blitline har tagit emot din begäran, den har placerats i en kö för bearbetning och när den har slutförts avbildningen ska vara tillgängligt vid: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**</span><span class="sxs-lookup"><span data-stu-id="76808-138">This tells you that Blitline has recieved your request, it has put it in a processing queue, and when it has completed the image will be available at: **https://s3.amazonaws.com/dev.blitline/2011110722/YOUR\_APP\_ID/CK3f0xBF_2bV6wf7gEZE8w.jpg**</span></span>

## <a name="how-to-save-an-image-to-your-azure-storage-account"></a><span data-ttu-id="76808-139">Hur du sparar en avbildning i Azure Storage-konto</span><span class="sxs-lookup"><span data-stu-id="76808-139">How to save an image to your Azure Storage account</span></span>
<span data-ttu-id="76808-140">Om du har ett Azure Storage-konto kan har du enkelt Blitline skicka bearbetade bilder till din Azure-behållaren.</span><span class="sxs-lookup"><span data-stu-id="76808-140">If you have an Azure Storage account, you can easily have Blitline push the processed images into your Azure container.</span></span> <span data-ttu-id="76808-141">Genom att lägga till en ”azure_destination” definiera plats och behörigheter för Blitline att skicka till.</span><span class="sxs-lookup"><span data-stu-id="76808-141">By adding an "azure_destination" you define the location and permissions for Blitline to push to.</span></span>

<span data-ttu-id="76808-142">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="76808-142">Here is an example:</span></span>

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


<span data-ttu-id="76808-143">Genom att fylla i CAPITALIZED värdena med dina egna, kan du skicka den här JSON till http://api.blitline.com/job och ”src” bilden kommer att bearbetas med ett filter för oskärpa och sedan pushas till Azure mål.</span><span class="sxs-lookup"><span data-stu-id="76808-143">By filling in the CAPITALIZED values with your own, you can submit this JSON to http://api.blitline.com/job and the "src" image will be processed with a blur filter and then pushed to you Azure destination.</span></span>

### <a name="please-note"></a><span data-ttu-id="76808-144">Obs!</span><span class="sxs-lookup"><span data-stu-id="76808-144">Please note:</span></span>
<span data-ttu-id="76808-145">SAS måste innehålla den hela SAS-url, inklusive filnamn för målfilen.</span><span class="sxs-lookup"><span data-stu-id="76808-145">The SAS must contain the entire SAS url, including the filename of the destination file.</span></span>

<span data-ttu-id="76808-146">Exempel:</span><span class="sxs-lookup"><span data-stu-id="76808-146">Example:</span></span>

    http://blitline.blob.core.windows.net/sample/image.jpg?sr=b&sv=2012-02-12&st=2013-04-12T03%3A18%3A30Z&se=2013-04-12T04%3A18%3A30Z&sp=w&sig=Bte2hkkbwTT2sqlkkKLop2asByrE0sIfeesOwj7jNA5o%3D


<span data-ttu-id="76808-147">Du kan också läsa den senaste utgåvan av Blitline's Azure Storage docs [här](http://www.blitline.com/docs/azure_storage).</span><span class="sxs-lookup"><span data-stu-id="76808-147">You can also read the latest edition of Blitline's Azure Storage docs [here](http://www.blitline.com/docs/azure_storage).</span></span>

## <a name="next-steps"></a><span data-ttu-id="76808-148">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="76808-148">Next Steps</span></span>
<span data-ttu-id="76808-149">Besök blitline.com att läsa om våra andra funktioner:</span><span class="sxs-lookup"><span data-stu-id="76808-149">Visit blitline.com to read about all our other features:</span></span>

* <span data-ttu-id="76808-150">Blitline API-slutpunkt Docs <http://www.blitline.com/docs/api></span><span class="sxs-lookup"><span data-stu-id="76808-150">Blitline API Endpoint Docs <http://www.blitline.com/docs/api></span></span>
* <span data-ttu-id="76808-151">Blitline API-funktioner <http://www.blitline.com/docs/functions></span><span class="sxs-lookup"><span data-stu-id="76808-151">Blitline API Functions <http://www.blitline.com/docs/functions></span></span>
* <span data-ttu-id="76808-152">Blitline API exempel <http://www.blitline.com/docs/examples></span><span class="sxs-lookup"><span data-stu-id="76808-152">Blitline API Examples <http://www.blitline.com/docs/examples></span></span>
* <span data-ttu-id="76808-153">Tredje Part Nuget biblioteket <http://nuget.org/packages/Blitline.Net></span><span class="sxs-lookup"><span data-stu-id="76808-153">Third Part Nuget Library <http://nuget.org/packages/Blitline.Net></span></span>

