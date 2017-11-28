---
title: "aaaHow toocheck jobbförloppet med hjälp av REST API | Microsoft Docs"
description: "Lär dig hur tootrack jobb pågår."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: a1a1f956-c035-448a-af9c-5ac15fcce9dd
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: juliako
ms.openlocfilehash: 32f12c81422566d980a7200b1662a3cc3ebc39a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-check-job-progress"></a><span data-ttu-id="b7f1f-103">Så här: följa jobbförloppet</span><span class="sxs-lookup"><span data-stu-id="b7f1f-103">How to: check job progress</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b7f1f-104">Portal</span><span class="sxs-lookup"><span data-stu-id="b7f1f-104">Portal</span></span>](media-services-portal-check-job-progress.md)
> * [<span data-ttu-id="b7f1f-105">.NET</span><span class="sxs-lookup"><span data-stu-id="b7f1f-105">.NET</span></span>](media-services-check-job-progress.md)
> * [<span data-ttu-id="b7f1f-106">REST</span><span class="sxs-lookup"><span data-stu-id="b7f1f-106">REST</span></span>](media-services-rest-check-job-progress.md)
> 
> 

<span data-ttu-id="b7f1f-107">När du kör jobb behöver du ofta ett sätt tootrack jobb pågår.</span><span class="sxs-lookup"><span data-stu-id="b7f1f-107">When you run jobs, you often require a way tootrack job progress.</span></span> <span data-ttu-id="b7f1f-108">Du kan ta reda hello jobbstatus med hello jobbets status-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="b7f1f-108">You can find out hello Job status by using hello Job's State property.</span></span> <span data-ttu-id="b7f1f-109">Läs mer på hello tillstånd egenskapen [jobbet Entitetsegenskaper](https://docs.microsoft.com/rest/api/media/operations/job#job_entity_properties).</span><span class="sxs-lookup"><span data-stu-id="b7f1f-109">For more information on hello State property, see [Job Entity Properties](https://docs.microsoft.com/rest/api/media/operations/job#job_entity_properties).</span></span>

## <a name="connect-toomedia-services"></a><span data-ttu-id="b7f1f-110">Ansluta tooMedia tjänster</span><span class="sxs-lookup"><span data-stu-id="b7f1f-110">Connect tooMedia Services</span></span>

<span data-ttu-id="b7f1f-111">Mer information om hur tooconnect toohello AMS API, se [åtkomst hello Azure Media Services API med Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span><span class="sxs-lookup"><span data-stu-id="b7f1f-111">For information on how tooconnect toohello AMS API, see [Access hello Azure Media Services API with Azure AD authentication](media-services-use-aad-auth-to-access-ams-api.md).</span></span> 

>[!NOTE]
><span data-ttu-id="b7f1f-112">När du har anslutit toohttps://media.windows.net, får du en 301 omdirigering att ange en annan Media Services-URI.</span><span class="sxs-lookup"><span data-stu-id="b7f1f-112">After successfully connecting toohttps://media.windows.net, you will receive a 301 redirect specifying another Media Services URI.</span></span> <span data-ttu-id="b7f1f-113">Du måste göra följande anrop toohello ny URI.</span><span class="sxs-lookup"><span data-stu-id="b7f1f-113">You must make subsequent calls toohello new URI.</span></span>

## <a name="check-job-progress"></a><span data-ttu-id="b7f1f-114">Kontrollera jobbförlopp</span><span class="sxs-lookup"><span data-stu-id="b7f1f-114">Check job progress</span></span>

<span data-ttu-id="b7f1f-115">Begäran:</span><span class="sxs-lookup"><span data-stu-id="b7f1f-115">Request:</span></span>

    GET https://media.windows.net/api/Jobs()?$filter=Id%20eq%20'nb%3Ajid%3AUUID%3Af3c43f94-327f-2347-90bb-3bf79f8559f1'&$top=1 HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Accept: application/json
    Accept-Charset: UTF-8
    Authorization: Bearer http%3a%2f%2fschemas.xmlsoap.org%2fws%2f2005%2f05%2fidentity%2fclaims%2fnameidentifier=juliakoams1&urn%3aSubscriptionId=zbbef702-2233-477b-9f16-bc4d3aa97387&http%3a%2f%2fschemas.microsoft.com%2faccesscontrolservice%2f2010%2f07%2fclaims%2fidentityprovider=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&Audience=urn%3aWindowsAzureMediaServices&ExpiresOn=1423640758&Issuer=https%3a%2f%2fwamsprodglobal001acs.accesscontrol.windows.net%2f&HMACSHA256=z5yFIG%2bk8Z2G2aXABqM60P9smHNKD7P4BfSxXanwKFc%3d
    x-ms-version: 2.11
    Host: media.windows.net



<span data-ttu-id="b7f1f-116">Svar:</span><span class="sxs-lookup"><span data-stu-id="b7f1f-116">Response:</span></span>

    HTTP/1.1 200 OK
    Cache-Control: no-cache
    Content-Length: 450
    Content-Type: application/json;odata=minimalmetadata;streaming=true;charset=utf-8
    Server: Microsoft-IIS/8.5
    x-ms-client-request-id: d9b83c57-e26c-4d10-a20b-2be634c4b6a8
    request-id: 91d2be35-20ed-4e1c-a147-e82cd000c193
    x-ms-request-id: 91d2be35-20ed-4e1c-a147-e82cd000c193
    X-Content-Type-Options: nosniff
    DataServiceVersion: 3.0;
    Strict-Transport-Security: max-age=31536000; includeSubDomains

    {"odata.metadata":"https://media.windows.net/api/$metadata#Jobs","value":[{"Id":"nb:jid:UUID:f3c43f94-327f-2347-90bb-3bf79f8559f1","Name":"Encoding BigBuckBunny into tooH264 Adaptive Bitrate MP4 Set 720p","Created":"2015-02-11T01:46:08.897","LastModified":"2015-02-11T01:46:08.897","EndTime":null,"Priority":0,"RunningDuration":0.0,"StartTime":"2015-02-11T01:46:16.58","State":2,"TemplateId":null,"JobNotificationSubscriptions":[]}]} 


## <a name="media-services-learning-paths"></a><span data-ttu-id="b7f1f-117">Sökvägar för Media Services-utbildning</span><span class="sxs-lookup"><span data-stu-id="b7f1f-117">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="b7f1f-118">Ge feedback</span><span class="sxs-lookup"><span data-stu-id="b7f1f-118">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a><span data-ttu-id="b7f1f-119">Se även</span><span class="sxs-lookup"><span data-stu-id="b7f1f-119">See also</span></span>

[<span data-ttu-id="b7f1f-120">Media Services operations REST API-översikt</span><span class="sxs-lookup"><span data-stu-id="b7f1f-120">Media Services operations REST API overview</span></span>](media-services-rest-how-to-use.md)
