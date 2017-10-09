---
title: aaaTroubleshooting hello Azure Import/Export verktyget | Microsoft Docs
description: "Lär dig mer om hello vanliga problem som kan uppstå när du använder hello Azure Import/Export-verktyget och hur toohandle dem."
author: muralikk
manager: syadav
editor: tysonn
services: storage
documentationcenter: 
ms.assetid: b91ca5eb-c557-460a-9afc-0590b38471f9
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.openlocfilehash: 5445cefe2703edf4d9d285f761433b7b66d8cb6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-azure-importexport-tool"></a>Felsöka hello Azure Import/Export-verktyget
hello verktyget Azure Import/Export returnerar felmeddelanden om den körs i problem. Det här avsnittet beskrivs några vanliga problem som användarna kan köra till.  
  
## <a name="a-copy-session-fails-what-i-should-do"></a>En kopia session misslyckas vad jag ska göra?  
 När en kopia session misslyckas, finns det två alternativ:  
  
 Om hello fel är återförsökbart, till exempel om hello nätverksresurs var offline för en kort tidsperiod och nu är online igen kan återuppta du hello kopiera session. Om hello fel inte återförsökbart, till exempel om du har angett hello fel fil källkatalogen i hello kommandoradsparametrar måste tooabort hello kopiera session. Se [förbereder hårddiskar för ett importjobb](storage-import-export-tool-preparing-hard-drives-import-v1.md) för mer information om att återuppta och avbryter kopiera sessioner.  
  
## <a name="i-cant-resume-or-abort-a-copy-session"></a>Jag kan inte återuppta eller avsluta en session kopia.  
 Om hello kopiera sessionen är hello den första kopian sessionen för en enhet och sedan hello felmeddelandet ska ange: ”hello första kopian sessionen inte kan återupptas eller avbröts”. I det här fallet kan du ta bort hello gamla journal-fil och kör hello kommandot igen.  
  
 Om en kopia session inte är hello förstnämnda för en enhet, den alltid återupptas eller avbröts.  
  
## <a name="i-lost-hello-journal-file-can-i-still-create-hello-job"></a>Försvann hello journal-fil, kan jag fortfarande skapa hello jobbet?  
 hello journal-fil för en enhet innehåller hello fullständig information för att kopiera data toothis enheten och den nödvändiga tooadd flera filer toohello enhet och kommer att använda toocreate importen. Om hello journalfilen tappas bort, måste tooredo alla hello kopiera sessioner hello-enheten.  
  
## <a name="next-steps"></a>Nästa steg
 
* [Konfigurera hello azure import/export-verktyget](storage-import-export-tool-setup-v1.md)   
* [Förbereda hårddiskar för ett importjobb](storage-import-export-tool-preparing-hard-drives-import-v1.md)   
* [Granska jobbstatus med kopiera loggfiler](storage-import-export-tool-reviewing-job-status-v1.md)   
* [Reparera ett importjobb](storage-import-export-tool-repairing-an-import-job-v1.md)   
* [Reparera ett exportjobb](storage-import-export-tool-repairing-an-export-job-v1.md)
