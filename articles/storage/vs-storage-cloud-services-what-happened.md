---
title: "aaaWhat hände toomy molntjänstprojekt? | Microsoft Docs"
description: "Beskriver vad som händer i ett cloud services-projekt när ansluter tooan Azure storage-konto med hjälp av Visual Studio anslutna tjänster"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: ca0ea68d-f417-4ce8-9413-40d76f69cdea
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-what-happened
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 65662dde45dd75bca1b57022283f76305f95e94a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-happened-toomy-cloud-services-project-visual-studio-azure-storage-connected-service"></a>Vad hände toomy molntjänster projektet (Visual Studio Azure Storage anslutna service)?
## <a name="references-added"></a>Referenser som lagts till
hello Azure Storage NuGet-paketet har lagts till tooyour Visual Studio-projekt.  
Det här paketet lägger till hello efter .NET referenser:

* **Microsoft.Data.Edm**
* **Microsoft.Data.OData**
* **Microsoft.Data.Services.Client**
* **Microsoft.WindowsAzure.Configuration**
* **Microsoft.WindowsAzure.Storage**
* **Newtonsoft.Json**
* **System.Data**
* **System.Spatial**

## <a name="connection-string-for-azure-storage-added"></a>Anslutningssträngen för Azure Storage som lagts till
Element har skapats med hello valt lagringskontots anslutningssträngen och nyckel. Ändringar har gjorts toohello följande filer:

* **ServiceDefinition.csdef**
* **ServiceConfiguration.Cloud.cscfg**
* **ServiceConfiguration.Local.cscfg**

