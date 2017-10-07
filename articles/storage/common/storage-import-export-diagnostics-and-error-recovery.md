---
title: "aaaDiagnostics och fel återställning för Azure Import/Export jobb | Microsoft Docs"
description: "Lär dig hur tooenable utförlig loggning för Microsoft Azure Import/Export service jobb."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: 096cc795-9af6-4335-9fe8-fffa9f239a17
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.openlocfilehash: 48164279e7904c78fed802aa3cff66e589c3f12c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Diagnostik- och återställning för Azure Import/Export-jobb
För varje enhet som bearbetas skapar hello Azure Import/Export service en fellogg i hello associerade storage-konto. Du kan också aktivera utförlig loggning genom att ange hello `LogLevel` egenskapen för`Verbose` när du anropar hello [placera jobbet](/rest/api/storageimportexport/jobs#Jobs_CreateOrUpdate) eller [uppdatering jobbegenskaper](/rest/api/storageimportexport/jobs#Jobs_Update) åtgärder.

 Som standard skrivs tooa behållare med namnet `waimportexport`. Du kan ange ett annat namn genom att ange hello `DiagnosticsPath` egenskapen när du anropar hello `Put Job` eller `Update Job Properties` åtgärder. hello loggar lagras som blockblobar med följande namngivningskonvention hello: `waies/jobname_driveid_timestamp_logtype.xml`.

 Du kan hämta hello URI hello loggar för ett jobb genom att anropa hello [Get Job](/rest/api/storageimportexport/jobs#Jobs_Get) igen. hello URI för hello utförlig logg returneras hello `VerboseLogUri` egenskap för varje enhet, medan hello URI för hello felloggen returneras hello `ErrorLogUri` egenskapen.

Du kan använda hello loggning data tooidentify hello följande problem.

## <a name="drive-errors"></a>Fel för enheten

hello följande objekt är klassificerade som enheten fel:

-   Fel i åtkomst till eller läsa hello manifestfil

-   Felaktig BitLocker-nycklar

-   Enheten läsning och skrivning fel

## <a name="blob-errors"></a>BLOB-fel

hello följande objekt är klassificerade som blob-fel:

-   Felaktig eller motstridig blob eller namn

-   Filer som saknas

-   Det gick inte att hitta BLOB

-   Trunkerat filer (hello-filer på hello disken är mindre än vad som angetts i manifestet hello)

-   Skadad fil innehåll (för importjobb som identifieras med en felaktig matchning av MD5 kontrollsumma)

-   Skadat blob-metadata och egenskapen-filer som (identifieras med en felaktig matchning av MD5 kontrollsumma)

-   Felaktigt schema för hello blob-egenskaper och/eller metadatafiler

Det kan finnas fall där vissa delar av en import- eller jobbet inte kan slutföras medan hello övergripande jobbet fortfarande har slutförts. I det här fallet kan du antingen ladda upp eller hämta hello saknas datadelar hello över nätverk eller du kan skapa en ny jobbdata tootransfer hello. Se hello [referens för Azure Import/Export verktyg](storage-import-export-tool-how-to-v1.md) toolearn hur toorepair hello data över nätverket.

## <a name="next-steps"></a>Nästa steg

* [Med hjälp av hello Import/Export service REST API](storage-import-export-using-the-rest-api.md)
