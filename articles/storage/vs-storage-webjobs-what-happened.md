---
title: "Vad hände med Webbjobb projektet (Visual Studio Azure Storage ansluten service)? | Microsoft Docs"
description: "Beskriver vad som hände i ett projekt för Azure Webjobs när anslutning till ett lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 3b28ddeadc87937941d60b16fae817e59a220b22
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-happened-to-my-webjob-project-visual-studio-azure-storage-connected-service"></a>Vad hände med Webbjobb projektet (Visual Studio Azure Storage ansluten service)?
## <a name="references-added"></a>Referenser som lagts till
Azure Storage NuGet-paketet har lagts till eller uppdateras i Visual Studio-projekt.  
Det här paketet lägger du till följande .NET referenser:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Anslutningssträngen för Azure Storage som lagts till
I filen App.config i ditt projekt i **AzureWebJobsStorage** och **AzureWebJobsDashboard** poster har uppdaterats med anslutningssträngen och nyckeln för det valda lagringskontot.

Mer information finns i [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?linkid=390226).

