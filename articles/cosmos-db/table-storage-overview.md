---
title: "Översikt över Azure Table Storage | Microsoft Docs"
description: "Lagra strukturerade data i molnet med hjälp av Azure Table Storage, en NoSQL-databas."
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/23/2017
ms.author: mimig
ms.openlocfilehash: 9099e90c402185b371495379db943d64fb82cdb8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-table-storage-overview"></a><span data-ttu-id="4a8b4-103">Översikt över Azure Table Storage</span><span class="sxs-lookup"><span data-stu-id="4a8b4-103">Azure Table storage overview</span></span>

[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

<span data-ttu-id="4a8b4-104">Azure Table Storage är en tjänst som lagrar strukturerade NoSQL-data i molnet och som ger tillgång till ett nyckel-/attributlager med en schemalös design.</span><span class="sxs-lookup"><span data-stu-id="4a8b4-104">Azure Table storage is a service that stores structured NoSQL data in the cloud, providing a key/attribute store with a schemaless design.</span></span> <span data-ttu-id="4a8b4-105">Eftersom Table Storage är schemalös är det enkelt att anpassa dina data i takt med att programmets behov förändras.</span><span class="sxs-lookup"><span data-stu-id="4a8b4-105">Because Table storage is schemaless, it's easy to adapt your data as the needs of your application evolve.</span></span> <span data-ttu-id="4a8b4-106">Åtkomsten till data i Table Storage är snabb och kostnadseffektiv för många typer av program, och medför normalt lägre kostnad än traditionell SQL för liknande datavolymer.</span><span class="sxs-lookup"><span data-stu-id="4a8b4-106">Access to Table storage data is fast and cost-effective for many types of applications, and is typically lower in cost than traditional SQL for similar volumes of data.</span></span>

<span data-ttu-id="4a8b4-107">Du kan använda Table Storage för att lagra flexibla datauppsättningar som användardata för webbprogram, adressböcker, enhetsinformation eller andra typer av metadata som din tjänst kräver.</span><span class="sxs-lookup"><span data-stu-id="4a8b4-107">You can use Table storage to store flexible datasets like user data for web applications, address books, device information, or other types of metadata your service requires.</span></span> <span data-ttu-id="4a8b4-108">Du kan lagra valfritt antal enheter i en tabell, och ett lagringskonto kan innehålla valfritt antal tabeller, upp till lagringskontots kapacitetsgräns.</span><span class="sxs-lookup"><span data-stu-id="4a8b4-108">You can store any number of entities in a table, and a storage account may contain any number of tables, up to the capacity limit of the storage account.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

## <a name="next-steps"></a><span data-ttu-id="4a8b4-109">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4a8b4-109">Next steps</span></span>

* <span data-ttu-id="4a8b4-110">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en kostnadsfri, fristående app från Microsoft som gör det möjligt att arbeta visuellt med Azure Storage-data i Windows, macOS och Linux.</span><span class="sxs-lookup"><span data-stu-id="4a8b4-110">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

* [<span data-ttu-id="4a8b4-111">Komma igång med Azure Table Storage i .NET</span><span class="sxs-lookup"><span data-stu-id="4a8b4-111">Getting Started with Azure Table Storage in .NET</span></span>](table-storage-how-to-use-dotnet.md)

* <span data-ttu-id="4a8b4-112">Fullständig information om tillgängliga API:er finns i referensdokumentationen för tabelltjänsten:</span><span class="sxs-lookup"><span data-stu-id="4a8b4-112">View the Table service reference documentation for complete details about available APIs:</span></span>

    * [<span data-ttu-id="4a8b4-113">Storage-klientbibliotek för .NET-referens</span><span class="sxs-lookup"><span data-stu-id="4a8b4-113">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)

    * [<span data-ttu-id="4a8b4-114">REST API-referens</span><span class="sxs-lookup"><span data-stu-id="4a8b4-114">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
