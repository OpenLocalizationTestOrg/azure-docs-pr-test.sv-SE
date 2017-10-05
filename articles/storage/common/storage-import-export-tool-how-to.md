---
title: Med verktyget Azure Import/Export | Microsoft Docs
description: "Lär dig hur du använder verktyget Import/Export för att förbereda hårddiskar för ett importjobb, reparera ett importjobb eller reparera ett exportjobb."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: f77535bb-d577-438a-bdd3-e15a82e0c543
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 20a720833842f9579fd4fccaa39e964def48197e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="using-the-azure-importexport-tool"></a><span data-ttu-id="8e7dc-103">Med verktyget Azure Import/Export</span><span class="sxs-lookup"><span data-stu-id="8e7dc-103">Using the Azure Import/Export Tool</span></span> 

<span data-ttu-id="8e7dc-104">Verktyget Azure Import/Export (WAImportExport.exe) används för att skapa och hantera jobb för tjänsten Azure Import/Export så att du kan överföra stora mängder data till eller från Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="8e7dc-104">The Azure Import/Export Tool (WAImportExport.exe) is used to create and manage jobs for the Azure Import/Export service, enabling you to transfer large amounts of data into or out of Azure Blob Storage.</span></span>

<span data-ttu-id="8e7dc-105">Den här dokumentationen är för den senaste versionen av verktyget Azure Import/Export.</span><span class="sxs-lookup"><span data-stu-id="8e7dc-105">This documentation is for the most recent version of the Azure Import/Export Tool.</span></span> <span data-ttu-id="8e7dc-106">Information om hur du använder den klassiska distributionsmodellen finns [med hjälp av Azure Import/Export verktyget v1](storage-import-export-tool-how-to-v1.md).</span><span class="sxs-lookup"><span data-stu-id="8e7dc-106">For information about using the classic deployment model, see [Using the Azure Import/Export Tool v1](storage-import-export-tool-how-to-v1.md).</span></span>

<span data-ttu-id="8e7dc-107">I följande artiklar visar hur till:</span><span class="sxs-lookup"><span data-stu-id="8e7dc-107">The following articles show you how to:</span></span>  

- <span data-ttu-id="8e7dc-108">Installera och konfigurera verktyget Azure Import/Export.</span><span class="sxs-lookup"><span data-stu-id="8e7dc-108">Install and set up the Azure Import/Export Tool.</span></span>
- <span data-ttu-id="8e7dc-109">Förbered din hårddiskar för ett jobb när du importerar data från dina enheter till Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="8e7dc-109">Prepare your hard drives for a job where you import data from your drives to Azure Blob Storage.</span></span>
- <span data-ttu-id="8e7dc-110">Granska statusen för ett jobb med kopiera loggfiler.</span><span class="sxs-lookup"><span data-stu-id="8e7dc-110">Review the status of a job with Copy Log Files.</span></span> 
- <span data-ttu-id="8e7dc-111">Reparera importen.</span><span class="sxs-lookup"><span data-stu-id="8e7dc-111">Repair an import job.</span></span> 
- <span data-ttu-id="8e7dc-112">Reparera ett exportjobb.</span><span class="sxs-lookup"><span data-stu-id="8e7dc-112">Repair an export job.</span></span> 
- <span data-ttu-id="8e7dc-113">Felsöka Azure Import/Export-verktyget.</span><span class="sxs-lookup"><span data-stu-id="8e7dc-113">Troubleshoot the Azure Import/Export Tool.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="8e7dc-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8e7dc-114">Next steps</span></span>

* [<span data-ttu-id="8e7dc-115">Konfigurera verktyget WAImportExport</span><span class="sxs-lookup"><span data-stu-id="8e7dc-115">Setting up the WAImportExport tool</span></span>](storage-import-export-tool-setup.md)