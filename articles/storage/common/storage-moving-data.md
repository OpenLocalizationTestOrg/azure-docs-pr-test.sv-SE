---
title: "aaaMoving stora mängder data till/från molnlagring i Azure | Microsoft Docs"
description: "En översikt över hello olika metoder för att flytta data tooand från Azure Storage."
services: storage
documentationcenter: 
author: JarrettRenshaw
manager: msmets
editor: tysonn
ms.assetid: 5e3947a9-d99b-4108-9d57-3eb67c03e7ba
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: jarrettr
ms.openlocfilehash: 7c8ec9d99cbd48042b8146130c8827f9f25c024d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="moving-data-tooand-from-azure-storage"></a>Flytta data tooand från Azure Storage
Om du vill toomove lokala data tooAzure lagring (eller vice versa), det finns en mängd olika sätt toodo detta. hello-metod som passar dig bäst beror på ditt scenario. Den här artikeln ger en snabb överblick över olika scenarier och lämpliga erbjudanden för varje kriterium.

## <a name="building-applications"></a>Skapa program
Om du utvecklar ett program, är utveckling mot hello REST API eller en av våra många klientbibliotek ett bra sätt toomove data tooand från Azure Storage.

Azure Storage innehåller omfattande klientbibliotek för .NET, iOS, Java, Android, universella Windowsplattformen (UWP), Xamarin, C++, Node.JS, PHP, Ruby och Python. Hej klientbiblioteken har avancerade funktioner, t.ex logik för omprövning, loggning och parallell överföring. Du kan också utveckla direkt mot hello REST-API som kan anropas med valfritt språk som gör HTTP/HTTPS-förfrågningar.

Se [Kom igång med Azure Blob Storage](../blobs/storage-dotnet-how-to-use-blobs.md) toolearn mer.

Dessutom kan vi erbjuder även hello [Azure Storage Data Movement Library](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) som är ett bibliotek som är utformat för högpresterande kopiering av data tooand från Azure. Se tooour Data Movement Library [dokumentationen](https://github.com/Azure/azure-storage-net-data-movement) toolearn mer. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Visa/interagera snabbt med dina data
Om du vill tooview ett enkelt Azure Storage-data samtidigt som du har också hello möjlighet tooupload och hämta dina data, Överväg att använda en Azure Lagringsutforskaren.

Kolla vår lista över [Azure lagringsutforskare](../storage-explorers.md) toolearn mer.

## <a name="system-administration"></a>Systemadministration
Om du behöver eller inte bekvämare med kommandoradsverktyget (t.ex. systemadministratörer) följer här några alternativ för du tooconsider:

### <a name="azcopy"></a>AzCopy
AzCopy är en Windows-kommandoradsverktyg för högpresterande kopiering av data tooand från Azure Storage. Du kan också kopiera data inom ett lagringskonto eller mellan olika lagringskonton.

Se [överföra data med kommandoradsverktyget Azcopy hello](storage-use-azcopy.md) toolearn mer.

### <a name="azure-powershell"></a>Azure PowerShell
Azure PowerShell är en modul som tillhandahåller cmdletar för att hantera tjänster på Azure. Det är ett uppgiftsbaserat kommandoradsgränssnitt och skriptspråk som utformats specifikt för systemadministration.

Se [med hjälp av Azure PowerShell med Azure Storage](storage-powershell-guide-full.md) toolearn mer.

### <a name="azure-cli"></a>Azure CLI
Azure CLI tillhandahåller en uppsättning med öppen källkod, plattformsoberoende kommandon för att arbeta med Azure-tjänster. Azure CLI är tillgänglig på Windows, OSX och Linux.

Se [Using hello Azure CLI med Azure Storage](../storage-azure-cli.md) toolearn mer.

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Flytta stora mängder data med ett långsamt nätverk
En av hello största utmaningarna som är associerade med flytta stora mängder data är hello överföringstid. Om du vill tooget data till och från Azure Storage utan att bekymra dig om kostnaderna för nätverk eller skriva kod är en lämplig lösning med Azure Import/Export.

Se [Azure Import/Export](../storage-import-export-service.md) toolearn mer.

## <a name="backing-up-your-data"></a>Säkerhetskopiera dina data
Om du behöver bara toobackup dina data tooAzure lagring, är Azure Backup hello sätt toogo. Detta är en kraftfull lösning för att säkerhetskopiera data lokalt och virtuella Azure-datorer.

Se [Azure Backup](../../backup/backup-introduction-to-azure-backup.md) toolearn mer.

## <a name="accessing-your-data-on-premises-and-from-hello-cloud"></a>Åtkomst till data lokalt och från molnet hello
Om du behöver en lösning för att komma åt data lokalt och från molnet hello bör sedan du använda Azures moln hybridlagringslösning, StorSimple. Den här lösningen består av en fysisk enhet att Intelligent lagrar ofta använda data på SSD, används ibland data på hårddiskar och inaktiva/backup/arkiveringsdata på Azure Storage.

Se [StorSimple](../../storsimple/storsimple-overview.md) toolearn mer.

## <a name="recovering-your-data"></a>Återställning av data
När du har lokala arbetsbelastningar och program, måste en lösning som gör att ditt företag toocontinue som körs i en katastrof hello-händelse. Azure Site Recovery hanterar replikering, redundans och återställning av virtuella datorer och fysiska servrar. Replikerade data lagras i Azure Storage, så att du tooeliminate hello behovet av en sekundär plats datacenter.

Se [Azure Site Recovery](../../site-recovery/site-recovery-overview.md) toolearn mer.
### <a name="moving-data-faq"></a>Flytta Data vanliga frågor och svar:
## <a name="can-i-migrate-vhds-from-one-region-tooanother-without-copying"></a>Kan jag migrera virtuella hårddiskar från en region tooanother utan att kopiera?
hello är endast sätt toocopy virtuella hårddiskar mellan region toocopy hello data mellan lagringskonton på varje region. Du kan använda AZCopy för den här. Se överföra data med kommandoradsverktyget Azcopy toolearn i hello mer. För mycket stora mängder data kan du också Azure Import/Export. Se [Azure Import/Export](https://docs.microsoft.com/en-us/azure/storage/storage-import-export-service) toolearn mer.
