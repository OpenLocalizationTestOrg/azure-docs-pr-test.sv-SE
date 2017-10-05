---
title: Med verktyget Azure Import/Export - v1 | Microsoft Docs
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
ms.date: 1/15/2017
ms.author: muralikk
ms.openlocfilehash: 4ce2273cc0dcc456c2edc8c5dd2fc22496f20380
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="using-the-azure-importexport-tool-classic-deployment-model"></a><span data-ttu-id="bee26-103">Med verktyget Azure Import/Export (klassiska distributionsmodellen)</span><span class="sxs-lookup"><span data-stu-id="bee26-103">Using the Azure Import/Export Tool (classic deployment model)</span></span>

<span data-ttu-id="bee26-104">Verktyget Azure Import/Export (WAImportExport.exe) används för att skapa och hantera jobb för tjänsten Azure Import/Export så att du kan överföra stora mängder data till eller från Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="bee26-104">The Azure Import/Export Tool (WAImportExport.exe) is used to create and manage jobs for the Azure Import/Export service, enabling you to transfer large amounts of data into or out of Azure Blob Storage.</span></span>

<span data-ttu-id="bee26-105">Den här dokumentationen avser den klassiska distributionsmodellen i verktyget Azure Import/Export.</span><span class="sxs-lookup"><span data-stu-id="bee26-105">This documentation is for the classic deployment model of the Azure Import/Export Tool.</span></span> <span data-ttu-id="bee26-106">Information om hur du använder den senaste versionen av verktyget finns i [med verktyget Azure Import/Export](../storage-import-export-tool-how-to.md).</span><span class="sxs-lookup"><span data-stu-id="bee26-106">For information about using the most recent version of the tool, see [Using the Azure Import/Export Tool](../storage-import-export-tool-how-to.md).</span></span>

<span data-ttu-id="bee26-107">I följande artiklar visar hur till:</span><span class="sxs-lookup"><span data-stu-id="bee26-107">The following articles show you how to:</span></span>

- <span data-ttu-id="bee26-108">Installera och konfigurera verktyget Import/Export.</span><span class="sxs-lookup"><span data-stu-id="bee26-108">Install and set up the Import/Export Tool.</span></span>
- <span data-ttu-id="bee26-109">Förbered din hårddiskar för ett jobb när du importerar data från dina enheter till Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="bee26-109">Prepare your hard drives for a job where you import data from your drives to Azure Blob Storage.</span></span>
- <span data-ttu-id="bee26-110">Granska statusen för ett jobb med kopiera loggfiler.</span><span class="sxs-lookup"><span data-stu-id="bee26-110">Review the status of a job with Copy Log Files.</span></span> 
- <span data-ttu-id="bee26-111">Reparera importen.</span><span class="sxs-lookup"><span data-stu-id="bee26-111">Repair an import job.</span></span> 
- <span data-ttu-id="bee26-112">Reparera ett exportjobb.</span><span class="sxs-lookup"><span data-stu-id="bee26-112">Repair an export job.</span></span> 
- <span data-ttu-id="bee26-113">Felsöka Azure Import/Export-verktyget om du hade problem under processen.</span><span class="sxs-lookup"><span data-stu-id="bee26-113">Troubleshoot the Azure Import/Export Tool, in case you had a problem during process.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="bee26-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bee26-114">Next steps</span></span>

* [<span data-ttu-id="bee26-115">Konfigurera verktyget WAImportExport</span><span class="sxs-lookup"><span data-stu-id="bee26-115">Setting up the WAImportExport tool</span></span>](../storage-import-export-tool-how-to.md)