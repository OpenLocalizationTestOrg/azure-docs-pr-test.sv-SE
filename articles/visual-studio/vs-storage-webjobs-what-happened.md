---
title: "aaaWhat har hänt toomy Webbjobb projektet (Visual Studio Azure Storage ansluten service)? | Microsoft Docs"
description: "Beskriver vad som hände i ett projekt för Azure Webjobs när ansluter tooa lagringskonto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 36ae7ff7-c22c-47eb-b220-049d61618c74
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: ed0ce75f5b23eca3c41dacb48564d6e5b846f395
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-happened-toomy-webjob-project-visual-studio-azure-storage-connected-service"></a>Vad hände toomy Webbjobb projektet (Visual Studio Azure Storage ansluten service)?
## <a name="references-added"></a>Referenser som lagts till
hello Azure Storage NuGet-paketet har lagts till tooor uppdateras i Visual Studio-projekt.  
Det här paketet lägger till hello efter .NET referenser:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.ConfigurationManager**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Anslutningssträngen för Azure Storage som lagts till
I hello App.config-filen i ditt projekt hello **AzureWebJobsStorage** och **AzureWebJobsDashboard** poster har uppdaterats med hello valt lagringskontots anslutningssträngen och nyckel.

Mer information finns i [Azure WebJobs-dokumentation](http://go.microsoft.com/fwlink/?linkid=390226).

