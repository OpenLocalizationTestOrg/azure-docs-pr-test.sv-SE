---
title: aaaIntroduction tooAzure File storage | Microsoft Docs
description: "Introduktion tooAzure fillagring, som ger nätverksfil delar i hello Microsoft Cloud"
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/27/2017
ms.author: renash
ms.openlocfilehash: fe6826e79c364a6956831d2e273c4342a5fd47f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooazure-file-storage"></a>Introduktion tooAzure File storage

Azure File storage erbjuder nätverket filresurser i hello molnet med hello branschstandarden [Server Message Block (SMB) protokollet](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx) och [Common Internet File System (CIFS)](https://technet.microsoft.com/library/cc939973.aspx). Azure-filresurser kan monteras samtidigt av Azure Virtual Machines och lokala distributioner som kör Windows, macOS eller Linux. Ett allmänt lagringskonto ger du åtkomst till tooAzure File storage, Azure Blob storage och Azure Queue storage.

## <a name="videos"></a>Videoklipp
| Introduktion till Azure File Storage (27m) | Azure File Storage-självstudie (5 minuter)  |
|-|-|
| [![Skärmutsändning hello introduktion till Azure File storage video - klickar du på tooplay!](./media/storage-files-introduction/azure-files-introduction-video-snapshot1.png)](https://www.youtube.com/watch?v=zlrpomv5RLs) | [![Skärmutsändning för hello Azure File storage kursen - klickar du på tooplay!](./media/storage-files-introduction/azure-files-introduction-video-snapshot2.png)](https://channel9.msdn.com/Blogs/Azure/Azure-File-storage-with-Windows/) |

## <a name="why-azure-file-storage-is-useful"></a>Varför Azure File Storage är användbart

Azure File storage kan du tooreplace Windows Server, Linux eller NAS-baserade filservrar finns lokalt eller i hello moln med en OS-kostnadsfri molnet fil dela. Azure File storage har hello följande fördelar:

* **Delad åtkomst** stöd hello industry standard SMB-protokollet, vilket innebär att du sömlöst kan ersätta din lokala filresurser med Azure-filresurser utan att oroa programkompatibilitet för Azure-filresurser. En filresurs från flera datorer och program-instanser är som kan tooaccess en stor fördel med Azure File storage.

* **Fullständigt hanterade** Azure-filresurser kan skapas utan hello måste toomanage maskinvara eller ett operativsystem, vilket innebär att du inte har toodeal med korrigering hello server OS med kritiska säkerhetsuppdateringar eller ersätta en felande hårddiskar.

* **Skript och Tooling** PowerShell-cmdlets och Azure CLI kan vara används toocreate montera och hantera Azure-filresurser som en del av hello administration av Azure-program. Du kan skapa och hantera Azure-filresurser med hjälp av hello [Azure-portalen](https://portal.azure.com) och hello [Azure Lagringsutforskaren](https://storageexplorer.com). 

* **Återhämtning** Azure File storage har skapats från hello bakgrund in toobe alltid tillgängligt. Ersätta lokala filresurser med Azure File behöver storage du inte längre toowake in toodeal med lokala strömavbrott eller nätverksproblem. 

* **Bekant programmering** program som körs i Azure kan komma åt data på hello resursen via [filsystem I/O APIs](https://msdn.microsoft.com/library/system.io.file.aspx). Utvecklare kan därför utnyttja befintlig kod och kunskaper toomigrate befintliga program. Dessutom tooSystem-i/o-API: er, du kan använda någon av hello Azure storage client bibliotek, till exempel hello en för [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet), eller hello [Azure Storage REST API](/rest/api/storageservices/file-service-rest-api).

Azure-filresurser kan användas för att:

* **Ersätt lokala filservrar** Azure File storage kan vara används toocompletely Ersätt filresurser på traditionella lokala filservrar eller NAS-enheter. Populära operativsystem, till exempel Windows, macOS och Linux kan enkelt montera en filresurs på Azure, oavsett var de befinner sig i hello world.

* **"Lift and Shift"-hantera program**

    Azure File storage är det enkelt för tooshare data mellan olika delar av programmet hello delar ”lyfta och flytta” program moln för toohello som använder lokalt. tooimplement kan varje virtuell dator ansluter toohello filresurs och sedan den kan läsa och skriva filer precis som det skulle mot en lokal fil dela.

* **Förenkla molnutvecklingen**
    
    Azure File storage kan användas i ett antal olika sätt toosimplify nya molntjänster utvecklingsprojekt.
    
    * **Delade programinställningar**
    
        Ett vanligt mönster för distribuerade program är toohave configuration-filer på en central plats där de kan nås från många olika virtuella datorer. Sådana konfigurationsfiler kan nu lagras i en Azure-filresurs och läsas av alla instanser av programmet. De här inställningarna kan också hanteras via hello REST-gränssnitt, vilket ger globalt åtkomst toohello konfigurationsfiler.

    * **Diagnostikresurs**
    
        En filresurs i Azure kan också vara används toosave diagnostiska filer som loggar, mått och krascher Dumpar. Med filresurser som är tillgängliga via hello SMB- och REST-gränssnittet kan program toobuild eller utnyttja olika analysverktyg för att bearbeta och analysera hello diagnostikdata.

    * **Utveckling/testning/felsökning**

        När utvecklare och administratörer arbetar med virtuella datorer i molnet hello, måste de ofta en uppsättning verktyg. Att installera och distribuera dessa verktyg på varje virtuell dator där de behövs kan vara tidskrävande. Med Azure File storage kan kan en utvecklare eller administratören lagra sina favoritverktyg på en filresurs som kan vara enkelt anslutna toofrom någon virtuell dator.
        
## <a name="how-does-it-work"></a>Hur fungerar det?

Att hantera Azure-filresurser är mycket enklare än att hantera filresurser lokalt. hello följande diagram visar hello Azure File storage management konstruktioner:

![Filstruktur](./media/storage-files-introduction/files-concepts.png)

* **Storage-konto** komma åt tooAzure Storage görs genom ett lagringskonto. Se [Skalbarhets- och prestandamål](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) för information om kapacitet för lagringskonton.

* **Resurs** En File Storage-resurs är en SMB-filresurs i Azure. Alla kataloger och filer måste skapas i en överordnad resurs. Ett konto kan innehålla ett obegränsat antal resurser och en resurs kan lagra ett obegränsat antal filer, upp toohello 5 TB total kapacitet av hello filresurs.

* **Katalog** En valfri hierarki med kataloger.

* **Filen** en fil i hello resurs. En fil kan vara upp too1 TB i storlek.

* **Webbadressformatet** filer är adresserbara via följande URL-format hello:  

    ```
    https://<storage account>.file.core.windows.net/<share>/<directory/directories>/<file>
    ```

## <a name="next-steps"></a>Nästa steg

* [Skapa en Azure-filresurs](storage-how-to-create-file-share.md)
* [Anslut och montera på Windows](storage-how-to-use-files-windows.md)
* [Anslut och montera på Linux](storage-how-to-use-files-linux.md)
* [Anslut och montera på macOS](storage-how-to-use-files-mac.md)
* [Vanliga frågor och svar](../storage-files-faq.md)
* [Felsökning i Windows](storage-troubleshoot-windows-file-connection-problems.md)   
* [Felsökning i Linux](storage-troubleshoot-linux-file-connection-problems.md)   

<!-- Rena I would remove any articles from here that are more than a year old. - Robin-->
### <a name="conceptual-articles-and-videos"></a>Begreppsrelaterade artiklar och videoklipp
* [Azure Files Storage: ett friktionslöst SMB-filsystem i molnet för Windows och Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)

### <a name="tooling-support-for-azure-file-storage"></a>Verktygsstöd för Azure File Storage
* [Hur toouse AzCopy med Microsoft Azure Storage](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Använda hello Azure CLI med Azure Storage](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#create-and-manage-file-shares)

### <a name="blog-posts"></a>Blogginlägg
* [Azure File Storage finns nu allmänt tillgänglig](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inuti Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introduktion till Microsoft Azure File Service](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrera data tooAzure fil](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referens
* [Storage-klientbibliotek för .NET-referens](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [File Service REST API referens](http://msdn.microsoft.com/library/azure/dn167006.aspx)
