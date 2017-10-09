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
# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Felsökning av Microsoft Power BI Embedded Preview
Den här artikeln innehåller svar för hur tootroubleshoot **Power BI Embedded**.

<a name="connection-string"/>

## <a name="setting-sql-server-connection-strings"></a>Ange SQL Server-anslutningssträngar
tooset en SQL Server-anslutning sträng måste toofollow ett specifikt format. Nedan visas ett exempel anslutningssträngen för SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

toolearn mer information om SQL Server-anslutningssträngar finns hello följande artiklar:

* [SQL Server-anslutningssträngar](https://msdn.microsoft.com/library/jj653752.aspx)
* [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>

## <a name="setting-credentials"></a>Ställa in autentiseringsuppgifter
Du kan behöva tooupdate autentiseringsuppgifter som matchar en lösning för produktion i hello fall där du har autentiseringsuppgifter för en utvecklings- eller mellanlagringsmiljön, till exempel användarnamn och lösenord.

## <a name="see-also"></a>Se även
* [Komma igång med exemplet](power-bi-embedded-get-started-sample.md)
* [Vad är Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md)

