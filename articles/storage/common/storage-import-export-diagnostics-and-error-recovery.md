---
title: "aaaDiagnostics och fel återställning för Azure Import/Export jobb | Microsoft Docs"
description: "Lär dig hur tooenable utförlig loggning för Microsoft Azure Import/Export service jobb."
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
ms.openlocfilehash: 48164279e7904c78fed802aa3cff66e589c3f12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a><span data-ttu-id="2cc39-103">Diagnostik- och återställning för Azure Import/Export-jobb</span><span class="sxs-lookup"><span data-stu-id="2cc39-103">Diagnostics and error recovery for Azure Import/Export jobs</span></span>
<span data-ttu-id="2cc39-104">För varje enhet som bearbetas skapar hello Azure Import/Export service en fellogg i hello associerade storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2cc39-104">For each drive processed, hello Azure Import/Export service creates an error log in hello associated storage account.</span></span> <span data-ttu-id="2cc39-105">Du kan också aktivera utförlig loggning genom att ange hello `LogLevel` egenskapen för`Verbose` när du anropar hello [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) eller [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2cc39-105">You can also enable verbose logging by setting hello `LogLevel` property too`Verbose` when calling hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) operations.</span></span>

 <span data-ttu-id="2cc39-106">Som standard skrivs tooa behållare med namnet `waimportexport`.</span><span class="sxs-lookup"><span data-stu-id="2cc39-106">By default, logs are written tooa container named `waimportexport`.</span></span> <span data-ttu-id="2cc39-107">Du kan ange ett annat namn genom att ange hello `DiagnosticsPath` egenskapen när du anropar hello `Put Job` eller `Update Job Properties` åtgärder.</span><span class="sxs-lookup"><span data-stu-id="2cc39-107">You can specify a different name by setting hello `DiagnosticsPath` property when calling hello `Put Job` or `Update Job Properties` operations.</span></span> <span data-ttu-id="2cc39-108">hello loggar lagras som blockblobar med följande namngivningskonvention hello: `waies/jobname_driveid_timestamp_logtype.xml`.</span><span class="sxs-lookup"><span data-stu-id="2cc39-108">hello logs are stored as block blobs with hello following naming convention: `waies/jobname_driveid_timestamp_logtype.xml`.</span></span>

 <span data-ttu-id="2cc39-109">Du kan hämta hello URI hello loggar för ett jobb genom att anropa hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) igen.</span><span class="sxs-lookup"><span data-stu-id="2cc39-109">You can retrieve hello URI of hello logs for a job by calling hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="2cc39-110">hello URI för hello utförlig logg returneras hello `VerboseLogUri` egenskap för varje enhet, medan hello URI för hello felloggen returneras hello `ErrorLogUri` egenskapen.</span><span class="sxs-lookup"><span data-stu-id="2cc39-110">hello URI for hello verbose log is returned in hello `VerboseLogUri` property for each drive, while hello URI for hello error log is returned in hello `ErrorLogUri` property.</span></span>

<span data-ttu-id="2cc39-111">Du kan använda hello loggning data tooidentify hello följande problem.</span><span class="sxs-lookup"><span data-stu-id="2cc39-111">You can use hello logging data tooidentify hello following issues.</span></span>

## <a name="drive-errors"></a><span data-ttu-id="2cc39-112">Fel för enheten</span><span class="sxs-lookup"><span data-stu-id="2cc39-112">Drive errors</span></span>

<span data-ttu-id="2cc39-113">hello följande objekt är klassificerade som enheten fel:</span><span class="sxs-lookup"><span data-stu-id="2cc39-113">hello following items are classified as drive errors:</span></span>

-   <span data-ttu-id="2cc39-114">Fel i åtkomst till eller läsa hello manifestfil</span><span class="sxs-lookup"><span data-stu-id="2cc39-114">Errors in accessing or reading hello manifest file</span></span>

-   <span data-ttu-id="2cc39-115">Felaktig BitLocker-nycklar</span><span class="sxs-lookup"><span data-stu-id="2cc39-115">Incorrect BitLocker keys</span></span>

-   <span data-ttu-id="2cc39-116">Enheten läsning och skrivning fel</span><span class="sxs-lookup"><span data-stu-id="2cc39-116">Drive read/write errors</span></span>

## <a name="blob-errors"></a><span data-ttu-id="2cc39-117">BLOB-fel</span><span class="sxs-lookup"><span data-stu-id="2cc39-117">Blob errors</span></span>

<span data-ttu-id="2cc39-118">hello följande objekt är klassificerade som blob-fel:</span><span class="sxs-lookup"><span data-stu-id="2cc39-118">hello following items are classified as blob errors:</span></span>

-   <span data-ttu-id="2cc39-119">Felaktig eller motstridig blob eller namn</span><span class="sxs-lookup"><span data-stu-id="2cc39-119">Incorrect or conflicting blob or names</span></span>

-   <span data-ttu-id="2cc39-120">Filer som saknas</span><span class="sxs-lookup"><span data-stu-id="2cc39-120">Missing files</span></span>

-   <span data-ttu-id="2cc39-121">Det gick inte att hitta BLOB</span><span class="sxs-lookup"><span data-stu-id="2cc39-121">Blob not found</span></span>

-   <span data-ttu-id="2cc39-122">Trunkerat filer (hello-filer på hello disken är mindre än vad som angetts i manifestet hello)</span><span class="sxs-lookup"><span data-stu-id="2cc39-122">Truncated files (hello files on hello disk are smaller than specified in hello manifest)</span></span>

-   <span data-ttu-id="2cc39-123">Skadad fil innehåll (för importjobb som identifieras med en felaktig matchning av MD5 kontrollsumma)</span><span class="sxs-lookup"><span data-stu-id="2cc39-123">Corrupted file content (for import jobs, detected with an MD5 checksum mismatch)</span></span>

-   <span data-ttu-id="2cc39-124">Skadat blob-metadata och egenskapen-filer som (identifieras med en felaktig matchning av MD5 kontrollsumma)</span><span class="sxs-lookup"><span data-stu-id="2cc39-124">Corrupted blob metadata and property files (detected with an MD5 checksum mismatch)</span></span>

-   <span data-ttu-id="2cc39-125">Felaktigt schema för hello blob-egenskaper och/eller metadatafiler</span><span class="sxs-lookup"><span data-stu-id="2cc39-125">Incorrect schema for hello blob properties and/or metadata files</span></span>

<span data-ttu-id="2cc39-126">Det kan finnas fall där vissa delar av en import- eller jobbet inte kan slutföras medan hello övergripande jobbet fortfarande har slutförts.</span><span class="sxs-lookup"><span data-stu-id="2cc39-126">There may be cases where some parts of an import or export job do not complete successfully, while hello overall job still completes.</span></span> <span data-ttu-id="2cc39-127">I det här fallet kan du antingen ladda upp eller hämta hello saknas datadelar hello över nätverk eller du kan skapa en ny jobbdata tootransfer hello.</span><span class="sxs-lookup"><span data-stu-id="2cc39-127">In this case, you can either upload or download hello missing pieces of hello data over network, or you can create a new job tootransfer hello data.</span></span> <span data-ttu-id="2cc39-128">Se hello [referens för Azure Import/Export verktyg](storage-import-export-tool-how-to-v1.md) toolearn hur toorepair hello data över nätverket.</span><span class="sxs-lookup"><span data-stu-id="2cc39-128">See hello [Azure Import/Export Tool Reference](storage-import-export-tool-how-to-v1.md) toolearn how toorepair hello data over network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2cc39-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2cc39-129">Next steps</span></span>

* [<span data-ttu-id="2cc39-130">Med hjälp av hello Import/Export service REST API</span><span class="sxs-lookup"><span data-stu-id="2cc39-130">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
