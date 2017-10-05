---
title: "Vad hände med min 5 för ASP.NET-projekt (Visual Studio ansluten tjänster) | Microsoft Docs"
description: "Beskriver vad som händer när du ansluter till ett Azure storage-konto i Visual Studio ASP.NET 5-projekt med Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: e7caa9fa-c780-45eb-a546-299fc1c68455
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 4390993772eaf35516e48ad7adcdcec5f1df8d71
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-happened-to-my-aspnet-5-project-visual-studio-azure-storage-connected-services"></a>Vad hände med min 5 för ASP.NET-projekt (Visual Studio Azure Storage ansluten services)?
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

Dessutom NuGet-paketet **Microsoft.Framework.Configuration.Json** har lagts till.

## <a name="connection-string-for-azure-storage-added"></a>Anslutningssträngen för Azure Storage som lagts till
Ett element har skapats med det valda lagringskontot anslutningssträngen och nyckel i filen config.json för ditt projekt.

Mer information finns i [ASP.NET 5](http://www.asp.net/vnext).

