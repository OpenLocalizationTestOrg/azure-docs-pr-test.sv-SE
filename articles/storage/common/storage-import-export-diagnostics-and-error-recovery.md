---
title: "Diagnostik- och återställning för Azure Import/Export jobb | Microsoft Docs"
description: "Lär dig hur du aktiverar utförlig loggning för Microsoft Azure Import/Export service jobb."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 096cc795-9af6-4335-9fe8-fffa9f239a17
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 0068aae9d6780aa41a070db0eb191d0d5a165d21
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a><span data-ttu-id="5bf3d-103">Diagnostik- och återställning för Azure Import/Export-jobb</span><span class="sxs-lookup"><span data-stu-id="5bf3d-103">Diagnostics and error recovery for Azure Import/Export jobs</span></span>
<span data-ttu-id="5bf3d-104">Tjänsten Azure Import/Export skapar en fellogg i det associerade lagringskontot för varje enhet som bearbetas.</span><span class="sxs-lookup"><span data-stu-id="5bf3d-104">For each drive processed, the Azure Import/Export service creates an error log in the associated storage account.</span></span> <span data-ttu-id="5bf3d-105">Du kan också aktivera utförlig loggning genom att ange den `LogLevel` egenskapen `Verbose` när du anropar den [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) eller [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) operations.</span><span class="sxs-lookup"><span data-stu-id="5bf3d-105">You can also enable verbose logging by setting the `LogLevel` property to `Verbose` when calling the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operations.</span></span>

 <span data-ttu-id="5bf3d-106">Som standard loggarna skrivs till en behållare med namnet `waimportexport`.</span><span class="sxs-lookup"><span data-stu-id="5bf3d-106">By default, logs are written to a container named `waimportexport`.</span></span> <span data-ttu-id="5bf3d-107">Du kan ange ett annat namn genom att ange den `DiagnosticsPath` egenskapen när du anropar den `Put Job` eller `Update Job Properties` åtgärder.</span><span class="sxs-lookup"><span data-stu-id="5bf3d-107">You can specify a different name by setting the `DiagnosticsPath` property when calling the `Put Job` or `Update Job Properties` operations.</span></span> <span data-ttu-id="5bf3d-108">Loggfilerna lagras som blockblobar med följande namngivningskonvention: `waies/jobname_driveid_timestamp_logtype.xml`.</span><span class="sxs-lookup"><span data-stu-id="5bf3d-108">The logs are stored as block blobs with the following naming convention: `waies/jobname_driveid_timestamp_logtype.xml`.</span></span>

 <span data-ttu-id="5bf3d-109">Du kan hämta URI av loggar för ett jobb genom att anropa den [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) igen.</span><span class="sxs-lookup"><span data-stu-id="5bf3d-109">You can retrieve the URI of the logs for a job by calling the [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="5bf3d-110">URI för den detaljerade loggen returneras den `VerboseLogUri` egenskap för varje enhet, medan URI för felloggen returneras den `ErrorLogUri` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="5bf3d-110">The URI for the verbose log is returned in the `VerboseLogUri` property for each drive, while the URI for the error log is returned in the `ErrorLogUri` property.</span></span>

<span data-ttu-id="5bf3d-111">Du kan använda loggningsdata för att identifiera följande problem.</span><span class="sxs-lookup"><span data-stu-id="5bf3d-111">You can use the logging data to identify the following issues.</span></span>

## <a name="drive-errors"></a><span data-ttu-id="5bf3d-112">Fel för enheten</span><span class="sxs-lookup"><span data-stu-id="5bf3d-112">Drive errors</span></span>

<span data-ttu-id="5bf3d-113">Följande objekt är klassificerade som enheten fel:</span><span class="sxs-lookup"><span data-stu-id="5bf3d-113">The following items are classified as drive errors:</span></span>

-   <span data-ttu-id="5bf3d-114">Fel i åtkomst till eller läsa manifestfilen</span><span class="sxs-lookup"><span data-stu-id="5bf3d-114">Errors in accessing or reading the manifest file</span></span>

-   <span data-ttu-id="5bf3d-115">Felaktig BitLocker-nycklar</span><span class="sxs-lookup"><span data-stu-id="5bf3d-115">Incorrect BitLocker keys</span></span>

-   <span data-ttu-id="5bf3d-116">Enheten läsning och skrivning fel</span><span class="sxs-lookup"><span data-stu-id="5bf3d-116">Drive read/write errors</span></span>

## <a name="blob-errors"></a><span data-ttu-id="5bf3d-117">BLOB-fel</span><span class="sxs-lookup"><span data-stu-id="5bf3d-117">Blob errors</span></span>

<span data-ttu-id="5bf3d-118">Följande objekt är klassificerade som blob-fel:</span><span class="sxs-lookup"><span data-stu-id="5bf3d-118">The following items are classified as blob errors:</span></span>

-   <span data-ttu-id="5bf3d-119">Felaktig eller motstridig blob eller namn</span><span class="sxs-lookup"><span data-stu-id="5bf3d-119">Incorrect or conflicting blob or names</span></span>

-   <span data-ttu-id="5bf3d-120">Filer som saknas</span><span class="sxs-lookup"><span data-stu-id="5bf3d-120">Missing files</span></span>

-   <span data-ttu-id="5bf3d-121">Det gick inte att hitta BLOB</span><span class="sxs-lookup"><span data-stu-id="5bf3d-121">Blob not found</span></span>

-   <span data-ttu-id="5bf3d-122">Trunkerat filer (filerna på disken är mindre än vad som angetts i manifestet)</span><span class="sxs-lookup"><span data-stu-id="5bf3d-122">Truncated files (the files on the disk are smaller than specified in the manifest)</span></span>

-   <span data-ttu-id="5bf3d-123">Skadad fil innehåll (för importjobb som identifieras med en felaktig matchning av MD5 kontrollsumma)</span><span class="sxs-lookup"><span data-stu-id="5bf3d-123">Corrupted file content (for import jobs, detected with an MD5 checksum mismatch)</span></span>

-   <span data-ttu-id="5bf3d-124">Skadat blob-metadata och egenskapen-filer som (identifieras med en felaktig matchning av MD5 kontrollsumma)</span><span class="sxs-lookup"><span data-stu-id="5bf3d-124">Corrupted blob metadata and property files (detected with an MD5 checksum mismatch)</span></span>

-   <span data-ttu-id="5bf3d-125">Felaktigt schema för blob-egenskaper och/eller metadatafiler</span><span class="sxs-lookup"><span data-stu-id="5bf3d-125">Incorrect schema for the blob properties and/or metadata files</span></span>

<span data-ttu-id="5bf3d-126">Det kan finnas fall där vissa delar av en import- eller jobbet inte kan slutföras medan övergripande jobbet slutförs ändå.</span><span class="sxs-lookup"><span data-stu-id="5bf3d-126">There may be cases where some parts of an import or export job do not complete successfully, while the overall job still completes.</span></span> <span data-ttu-id="5bf3d-127">I det här fallet kan du antingen ladda upp eller hämta saknade delar av data över nätverket och du kan skapa ett nytt jobb för att överföra data.</span><span class="sxs-lookup"><span data-stu-id="5bf3d-127">In this case, you can either upload or download the missing pieces of the data over network, or you can create a new job to transfer the data.</span></span> <span data-ttu-id="5bf3d-128">Finns det [referens för Azure Import/Export verktyg](storage-import-export-tool-how-to-v1.md) information om hur du reparera data över nätverket.</span><span class="sxs-lookup"><span data-stu-id="5bf3d-128">See the [Azure Import/Export Tool Reference](storage-import-export-tool-how-to-v1.md) to learn how to repair the data over network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5bf3d-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5bf3d-129">Next steps</span></span>

* [<span data-ttu-id="5bf3d-130">Med hjälp av tjänsten Import/Export REST API</span><span class="sxs-lookup"><span data-stu-id="5bf3d-130">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
