---
title: aaaBacking in Azure Import/Export enhet manifesten | Microsoft Docs
description: "Lär dig hur toohave enheten visar för hello Microsoft Azure Import/Export service säkerhetskopieras automatiskt."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 594abd80-b834-4077-a474-d8a0f4b7928a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: f48b97a2cce62714aace2b30a393305202c7ecd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a><span data-ttu-id="f49d5-103">Säkerhetskopiera enheten visar för Azure Import/Export-jobb</span><span class="sxs-lookup"><span data-stu-id="f49d5-103">Backing up drive manifests for Azure Import/Export jobs</span></span>

<span data-ttu-id="f49d5-104">Enheten manifest kan automatiskt säkerhetskopieras tooblobs genom att ange hello `BackupDriveManifest` egenskapen för`true` i hello [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) eller [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) REST-API: et.</span><span class="sxs-lookup"><span data-stu-id="f49d5-104">Drive manifests can be automatically backed up tooblobs by setting hello `BackupDriveManifest` property too`true` in hello [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) REST API operations.</span></span> <span data-ttu-id="f49d5-105">Som standard säkerhetskopieras hello enhet manifesten inte.</span><span class="sxs-lookup"><span data-stu-id="f49d5-105">By default, hello drive manifests are not backed up.</span></span> <span data-ttu-id="f49d5-106">hello enhet manifestet säkerhetskopior lagras som blockblobar i en behållare i hello storage-konto som är associerade med hello jobb.</span><span class="sxs-lookup"><span data-stu-id="f49d5-106">hello drive manifest backups are stored as block blobs in a container within hello storage account associated with hello job.</span></span> <span data-ttu-id="f49d5-107">Hello behållarens namn är som standard `waimportexport`, men du kan ange ett annat namn i hello `DiagnosticsPath` egenskapen när du anropar hello `Put Job` eller `Update Job Properties` åtgärder.</span><span class="sxs-lookup"><span data-stu-id="f49d5-107">By default, hello container name is `waimportexport`, but you can specify a different name in hello `DiagnosticsPath` property when calling hello `Put Job` or `Update Job Properties` operations.</span></span> <span data-ttu-id="f49d5-108">hello säkerhetskopiering manifestet blob namnges i hello följande format: `waies/jobname_driveid_timestamp_manifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="f49d5-108">hello backup manifest blob are named in hello following format: `waies/jobname_driveid_timestamp_manifest.xml`.</span></span>

 <span data-ttu-id="f49d5-109">Du kan hämta hello hello enhet-URI-manifestet för ett jobb genom att anropa hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) igen.</span><span class="sxs-lookup"><span data-stu-id="f49d5-109">You can retrieve hello URI of hello backup drive manifests for a job by calling hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="f49d5-110">hello blob-URI returneras hello `ManifestUri` egenskap för varje enhet.</span><span class="sxs-lookup"><span data-stu-id="f49d5-110">hello blob URI is returned in hello `ManifestUri` property for each drive.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f49d5-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f49d5-111">Next steps</span></span>

* [<span data-ttu-id="f49d5-112">Med hjälp av hello Import/Export service REST API</span><span class="sxs-lookup"><span data-stu-id="f49d5-112">Using hello Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
