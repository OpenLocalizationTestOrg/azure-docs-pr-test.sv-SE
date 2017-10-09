---
title: "Felsökning av aaaMicrosoft Power BI Embedded Preview"
description: "Felsökning av Microsoft Power BI Embedded Preview"
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: c8aee652-ed8b-4c66-9c63-f798b7a655b4
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
ms.openlocfilehash: a0a25cd73977c0ea0bd6b7c82e215412245771bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a><span data-ttu-id="b2428-103">Felsökning av Microsoft Power BI Embedded Preview</span><span class="sxs-lookup"><span data-stu-id="b2428-103">Microsoft Power BI Embedded Preview troubleshooting</span></span>
<span data-ttu-id="b2428-104">Den här artikeln innehåller svar för hur tootroubleshoot **Power BI Embedded**.</span><span class="sxs-lookup"><span data-stu-id="b2428-104">This article provides answers for how  tootroubleshoot **Power BI Embedded**.</span></span>

<a name="connection-string"/>

## <a name="setting-sql-server-connection-strings"></a><span data-ttu-id="b2428-105">Ange SQL Server-anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="b2428-105">Setting SQL Server connection strings</span></span>
<span data-ttu-id="b2428-106">tooset en SQL Server-anslutning sträng måste toofollow ett specifikt format.</span><span class="sxs-lookup"><span data-stu-id="b2428-106">tooset a SQL Server connecting string, you need toofollow a specific format.</span></span> <span data-ttu-id="b2428-107">Nedan visas ett exempel anslutningssträngen för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b2428-107">Below is an example connection string for SQL Server.</span></span>

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

<span data-ttu-id="b2428-108">toolearn mer information om SQL Server-anslutningssträngar finns hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="b2428-108">toolearn more about SQL Server connection strings, see hello following articles:</span></span>

* [<span data-ttu-id="b2428-109">SQL Server-anslutningssträngar</span><span class="sxs-lookup"><span data-stu-id="b2428-109">SQL Server Connection Strings</span></span>](https://msdn.microsoft.com/library/jj653752.aspx)
* [<span data-ttu-id="b2428-110">SqlConnection.ConnectionString</span><span class="sxs-lookup"><span data-stu-id="b2428-110">SqlConnection.ConnectionString</span></span>](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>

## <a name="setting-credentials"></a><span data-ttu-id="b2428-111">Ställa in autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="b2428-111">Setting credentials</span></span>
<span data-ttu-id="b2428-112">Du kan behöva tooupdate autentiseringsuppgifter som matchar en lösning för produktion i hello fall där du har autentiseringsuppgifter för en utvecklings- eller mellanlagringsmiljön, till exempel användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="b2428-112">In hello case where you have credentials for a development or staging environment, such as user name and password, you might need tooupdate credentials that match a production solution.</span></span>

## <a name="see-also"></a><span data-ttu-id="b2428-113">Se även</span><span class="sxs-lookup"><span data-stu-id="b2428-113">See Also</span></span>
* [<span data-ttu-id="b2428-114">Komma igång med exemplet</span><span class="sxs-lookup"><span data-stu-id="b2428-114">Get started with sample</span></span>](power-bi-embedded-get-started-sample.md)
* [<span data-ttu-id="b2428-115">Vad är Power BI Embedded</span><span class="sxs-lookup"><span data-stu-id="b2428-115">What is Power BI Embedded</span></span>](power-bi-embedded-what-is-power-bi-embedded.md)

