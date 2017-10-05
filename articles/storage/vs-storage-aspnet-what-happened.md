---
title: "Vad hände med ASP.NET-projekt? | Microsoft Docs"
description: "Beskriver vad som händer när du lägger till Azure Storage till ett ASP.NET-projekt med Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: e1fe1b6d-4e3d-476d-8b2f-f7ade050515e
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: e2cdc2ff4df85f0224352bd32a3ec62480c3e6e5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-happened-to-my-aspnet-project-visual-studio-azure-storage-connected-service"></a>Vad hände med ASP.NET-projekt (Visual Studio Azure Storage ansluten service)?
## <a name="references-added"></a>Referenser som lagts till
Azure Storage NuGet-paketet har lagts till Visual Studio-projekt.  
Det här paketet lägger du till följande .NET referenser:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Anslutningssträngen för Azure Storage som lagts till
Ett element har skapats med det valda lagringskontot anslutningssträngen och nyckel i web.config-filen för ditt projekt.

Mer information finns i [ASP.NET](http://www.asp.net).

