---
title: "Säkerhetskopiera Azure Import/Export enhet manifesten | Microsoft Docs"
description: "Lär dig mer om att din enhet manifest för tjänsten Microsoft Azure Import/Export säkerhetskopieras automatiskt."
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
ms.openlocfilehash: 33eb8e1eea8f8aa7b79ef3e54f2b1ed88dc794ae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="backing-up-drive-manifests-for-azure-importexport-jobs"></a><span data-ttu-id="7ca9c-103">Säkerhetskopiera enheten visar för Azure Import/Export-jobb</span><span class="sxs-lookup"><span data-stu-id="7ca9c-103">Backing up drive manifests for Azure Import/Export jobs</span></span>

<span data-ttu-id="7ca9c-104">Enheten manifest kan automatiskt säkerhetskopieras till blobbar genom att ange den `BackupDriveManifest` egenskapen `true` i den [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) eller [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) REST-API: et.</span><span class="sxs-lookup"><span data-stu-id="7ca9c-104">Drive manifests can be automatically backed up to blobs by setting the `BackupDriveManifest` property to `true` in the [Put Job](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) or [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) REST API operations.</span></span> <span data-ttu-id="7ca9c-105">Som standard säkerhetskopieras enhet manifesten inte.</span><span class="sxs-lookup"><span data-stu-id="7ca9c-105">By default, the drive manifests are not backed up.</span></span> <span data-ttu-id="7ca9c-106">Enheten manifestet säkerhetskopiorna lagras som blockblobar i en behållare i storage-konto som är associerat med jobbet.</span><span class="sxs-lookup"><span data-stu-id="7ca9c-106">The drive manifest backups are stored as block blobs in a container within the storage account associated with the job.</span></span> <span data-ttu-id="7ca9c-107">Behållarens namn är som standard `waimportexport`, men du kan ange ett nytt namn i den `DiagnosticsPath` egenskapen när du anropar den `Put Job` eller `Update Job Properties` åtgärder.</span><span class="sxs-lookup"><span data-stu-id="7ca9c-107">By default, the container name is `waimportexport`, but you can specify a different name in the `DiagnosticsPath` property when calling the `Put Job` or `Update Job Properties` operations.</span></span> <span data-ttu-id="7ca9c-108">Säkerhetskopiering manifestet blob namnges i följande format: `waies/jobname_driveid_timestamp_manifest.xml`.</span><span class="sxs-lookup"><span data-stu-id="7ca9c-108">The backup manifest blob are named in the following format: `waies/jobname_driveid_timestamp_manifest.xml`.</span></span>

 <span data-ttu-id="7ca9c-109">Du kan hämta URI för enhet manifesten för ett jobb genom att anropa den [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) igen.</span><span class="sxs-lookup"><span data-stu-id="7ca9c-109">You can retrieve the URI of the backup drive manifests for a job by calling the [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) operation.</span></span> <span data-ttu-id="7ca9c-110">Blob-URI returneras den `ManifestUri` egenskap för varje enhet.</span><span class="sxs-lookup"><span data-stu-id="7ca9c-110">The blob URI is returned in the `ManifestUri` property for each drive.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7ca9c-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7ca9c-111">Next steps</span></span>

* [<span data-ttu-id="7ca9c-112">Med hjälp av tjänsten Import/Export REST API</span><span class="sxs-lookup"><span data-stu-id="7ca9c-112">Using the Import/Export service REST API</span></span>](storage-import-export-using-the-rest-api.md)
