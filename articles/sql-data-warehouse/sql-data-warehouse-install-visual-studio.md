---
title: "aaaInstall Visual Studio och SSDT för SQL Data Warehouse | Microsoft Docs"
description: "Installera Visual Studio och SQL Server Development Tools (SSDT) för Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 0ed9b406-9b42-4fe6-b963-fe0a5b48aac1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 03/30/2017
ms.author: anvang;barbkess
ms.openlocfilehash: cf49c13d5cab598ed127f5702c04168b62ede0dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a>Installera Visual Studio och SSDT för SQL Data Warehouse
toodevelop program för SQL Data Warehouse, bör du använda hello senaste version av Visual Studio med hello den senaste versionen av SQL Server Data Tools (SSDT).  Visual Studio 2013, uppdatering 5 med SSDT, stöds också för bakåtkompatibilitet.  

Visual Studio med SSDT kan du toouse hello SQL Server Object Explorer toovisually Utforska tabeller, vyer, lagrade procedurer och många fler objekt i ditt SQL Data Warehouse samt att köra frågor.

> [!NOTE]
> SQL Data Warehouse stöder ännu inte Visual Studio Database Projects.  Den funktionen kommer läggas till i framtida versioner.
> 
> 

## <a name="step-1-install-visual-studio"></a>Steg 1: Installera Visual Studio
Följ dessa länkar toodownload och installera Visual Studio. Om du redan har Visual Studio 2013 eller senare, kan du hoppa över tooStep 2, installera SSDT.

1. [Hämta Visual Studio][].
2. Följ hello [installera Visual Studio] [ Installing Visual Studio] på MSDN och välj hello standardkonfigurationerna.

## <a name="step-2-install-ssdt"></a>Steg 2: Installera SSDT
tooinstall SSDT för Visual Studio Sök bara efter en SSDT uppdatering i Visual Studio genom att följa dessa steg.

1. I Visual Studio klickar du på **verktyg** / **tillägg och uppdateringar...** / **Uppdateringar**
2. Välj **Produktuppdateringar** och hitta sedan **Microsoft SQL Server-uppdatering för databas-tooling**

Om en uppdatering hittas, bör du ha hello senaste versionen installerad.  tooconfirm SSDT har installerats, klicka på **hjälp** / **om Microsoft Visual Studio** och leta efter SQL Server Data Tools i hello-listan.  hello senaste versionen av SSDT är 14.0.60525.0.  Hello alternativet tooinstall inte är tillgänglig från Visual Studio, alternativt kan du besöka hello [SSDT hämta] [ SSDT Download] sidan toodownload och installera SSDT manuellt.

## <a name="next-steps"></a>Nästa steg
Nu när du har hello senaste versionen av SSDT, är du redo för[ansluta] [ connect] tooyour SQL Data Warehouse.

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Hämta Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
