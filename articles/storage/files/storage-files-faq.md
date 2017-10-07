---
title: "aaaFrequently frågor och svar om Azure File storage | Microsoft Docs"
description: "Få svar toofrequently frågor och svar om Azure File storage."
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
ms.date: 07/19/2017
ms.author: renash
ms.openlocfilehash: d8a94fdc77e928dbc6996e1e635cd3527e0c67d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="frequently-asked-questions-about-azure-file-storage"></a>Vanliga frågor och svar om Azure File storage

## <a name="general"></a>Allmänt
* **FRÅGOR. Vad är Azure File storage?**  
   
    Azure File storage är ett distribuerat filsystem i Azure. Det tillhandahåller ett gränssnitt för SMB-protokollet som gör att användare toomount hello lagring som en intern resurs på Azure virtuella datorer eller den lokala datorn.

* **FRÅGOR. Varför är Azure File storage användbar?**  
   
    Azure File storage ger åtkomst till delade data över flera virtuella datorer och plattformar. Se för[varför Azure File storage är användbar](storage-files-introduction.md#why-azure-file-storage-is-useful).

* **FRÅGOR. När bör jag använda Azure File vs Azure Blob vs Azure-disken?**  
   
    Microsoft Azure tillhandahåller flera olika sätt toostore och komma åt data i hello molnet.  
   
    Azure File storage - tillhandahåller ett gränssnitt för SMB, klientbibliotek och ett REST-gränssnitt som möjliggör enkel åtkomst överallt toostored filer.  
   
    Azure BLOB-objekt - tillhandahåller klientbibliotek ett REST-gränssnitt som möjliggör Ostrukturerade data toobe lagras och komma åt i massiv skala i blockblobbar.  
   
    Azure Data diskar - tillhandahåller klientbibliotek ett REST-gränssnitt som gör att data toobe beständigt lagras och nås från en ansluten virtuell hårddisk.  
   
    Läs mer på [ska när toouse Azure BLOB-objekt, Azure-filer eller Azure Datadiskar](../common/storage-decide-blobs-files-disks.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)

* **FRÅGOR. Hur kommer jag igång med Azure File storage?**  
   
    Du kan börja med att skapa en Azure-filresursen. Du kan skapa Azure-filresurser med hjälp av Azure Portal, hello Azure Storage PowerShell-cmdlets, hello Azure Storage-klientbibliotek eller hello Azure Storage REST API. I kursen får du lära dig:

    * [Lär dig hur toocreate Azure File dela med hello Portal](storage-how-to-create-file-share.md#create-file-share-through-the-portal)
    * [Lär dig hur toocreate Azure File dela med hjälp av Powershell](storage-how-to-create-file-share.md#create-file-share-through-powershell)
    * [Lär dig hur toocreate Azure File dela med hjälp av CLI](storage-how-to-create-file-share.md#create-file-share-through-command-line-interface-cli)

* **FRÅGOR. Vilka replikeringar stöds av Azure File storage?**  
   
    Azure File storage stöder endast LRS eller GRS just nu. Vi planerar toosupport RA-GRS men det finns inga tidslinjen tooshare ännu.

## <a name="security-authentication-and-access-control"></a>Säkerhet, autentisering och åtkomstkontroll

* **FRÅGOR. Vad är olika sätt tooaccess filer i Azure File storage?**
    
    Du kan montera hello filresurs på den lokala datorn med hjälp av SMB 3.0-protokollet eller använda verktyg som [Lagringsutforskaren](http://storageexplorer.com/) tooaccess filer i filresursen. Du kan använda lagringsklientbiblioteken, REST API: er eller Powershell tooaccess dela dina filer i Azure-filen från ditt program.

* **FRÅGOR. Hur kan jag ge åtkomst till tooa specifika fil i en webbläsare?**
    
    Med SAS kan generera du token med särskilda behörigheter som gäller för ett angivet tidsintervall. Du kan till exempel generera en token med skrivskyddad åtkomst tooa viss fil för en viss tidsperiod. Alla som har denna url kan komma åt hello filen direkt från en webbläsare när den är giltig. SAS-nycklar går lätt att generera från användargränssnitt som Storage Explorer.

* **FRÅGOR. Är det möjligt toospecify skrivskyddad eller lässkyddad behörighet för mapparna hello resursen?**
    
    Du har inte den här kontrollnivån över behörigheterna om du monterar hello filresursen via SMB. Du kan dock åstadkomma detta genom att skapa en signatur för delad åtkomst (SAS) via hello REST API: et eller klientbiblioteken.  

* **FRÅGOR. Hur kan jag aktivera kryptering på serversidan för Azure File storage?**

    [Kryptering på serversidan](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) för Azure File storage är allmänt tillgänglig i alla regioner och offentliga och nationella moln. Du kan aktivera SSE för Azure File storage med [Azure-portalen](https://portal.azure.com/),[Microsoft Azure Storage Resource Provider API](/rest/api/storagerp/storageaccounts), [Azure Powershell](https://msdn.microsoft.com/library/azure/mt607151.aspx) eller [Azure CLI ](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).
    
    När du har aktiverat SSE på Azure File storage krypteras nya data skrivs toohello fillagring i detta lagringskonto automatiskt. Den här funktionen är tillgänglig för alla nya data skrivs tooexisting eller nya resurser i en befintlig eller ny storage-konto. Inga extra kostnader tillkommer för att aktivera den här funktionen. Läs mer på [hur tooenable SSE på Azure File storage](../common/storage-service-encryption.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

* **FRÅGOR. Stöds Active Directory-baserad autentisering av Azure File storage?**
   
    För närvarande stöds inte AD-baserad autentisering eller ACL:er, men vi har det i åtanke för framtida versioner. För tillfället är hello Azure lagringskontonycklar används tooprovide autentisering toohello filresurs. Vi erbjuder en lösning som använder signaturer för delad åtkomst (SAS) via hello REST API: et eller hello klientbibliotek. Med SAS kan generera du token med särskilda behörigheter som gäller för ett angivet tidsintervall. Du kan till exempel generera en token med skrivskyddad åtkomst tooa beroende fil med 10 minuter upphör att gälla. Alla som har denna token när den är giltig har skrivskyddad åtkomst till toothat-fil för de 10 minuterna.
   
    SAS stöds bara via hello REST API: et eller klientbiblioteken. När du monterar hello filresursen via hello SMB-protokollet, kan du inte använda en SAS toodelegate åtkomst tooits innehållet. 
    
* **FRÅGOR. Vad är hello datapolicyer för efterlevnad som stöds för Azure File storage?**

   Azure File Storage körs ovanpå hello samma lagringsarkitektur som andra storage services i Azure Storage och tillämpar hello policyer för efterlevnad för samma data. Mer information om kompatibilitet för Azure Storage-data som du kan ladda ned och hänvisa för[dataskydd i Microsoft Azure-dokumentet](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409).

## <a name="on-premises-access"></a>Lokal åtkomst

* **Q.Do jag har toouse Azure ExpressRoute tooconnect tooAzure File storage från en lokal virtuell dator?**
   
    Nej. Om du inte har ExpressRoute kan du fortfarande komma åt hello filresursen från lokala så länge som du har port 445 (TCP utgående öppen för Internetåtkomst). Du kan dock använda ExpressRoute med Azure File storage om du vill.

* **FRÅGOR. Hur kan jag montera en filresurs på Azure på min dator?** 
    
    Du kan montera hello filresursen via hello SMB-protokollet som port 445 (TCP utgående) är öppen och klienten stöder hello SMB 3.0-protokollet (till exempel du använder Windows 10 eller Windows Server 2012). Se tillsammans med en lokal Internet-leverantör toounblock hello port. Hello tillfälligt, du kan visa dina filer med [Lagringsutforskaren](../../vs-azure-tools-storage-explorer-files.md#view-a-file-shares-contents).


## <a name="billing-and-pricing"></a>Fakturering och priser
* **FRÅGOR. Hello nätverkstrafik mellan en Azure VM och en filresurs som extern bandbredd som debiteras toohello prenumeration?**
   
    Om filresursen hello och VM finns i hello samma Azure-regionen hello trafiken mellan dem är ledig. Om de finns i olika områden debiteras hello trafiken mellan dem som extern bandbredd.

## <a name="backup"></a>Säkerhetskopiering

* **FRÅGOR. Hur jag för att säkerhetskopiera min Azure-filresursen?**
    
    Du kan använda AzCopy RoboCopy, eller en 3 part säkerhetskopieringsverktyg som kan säkerhetskopiera en monterad filresurs. Vi räknar toohave hello möjlighet tootake ögonblicksbilder av filresurser i hello nära framtiden. Du kommer att kunna toouse den här funktionen toobackup din Azure-filresurs.

## <a name="performance"></a>Prestanda

* **FRÅGOR. Vad är hello gränser för Azure File storage?**
    Mer information om skalbarhets- och prestandamål för Azure File storage finns [Azure Storage skalbarhets- och prestandamål](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#scalability-targets-for-blobs-queues-tables-and-files).

* **FRÅGOR. Min prestanda var låga när försök toounzip filer till Azure File storage. Vad ska jag göra?**
    
    tootransfer stort antal filer till Azure File storage, rekommenderar vi att du använder AzCopy (Windows, förhandsgranskning för Linux/Unix) eller Azure Powershell som dessa verktyg har optimerats för nätverksöverföring.

* **FRÅGOR. Vad korrigeringsfiler har publicerat toofix låga prestanda problem med Azure File storage?**
    
    Windows hello-teamet släppte nyligen en korrigering toofix problemet med långsamma prestanda när hello kunden ansluter till Azure File storage från Windows 8.1 eller Windows Server 2012 R2. Mer information du checka ut hello associerade KB-artikeln [låga prestanda när du har åtkomst till Azure File storage från Windows 8.1 eller Server 2012 R2](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Funktioner och samverkan med andra tjänster
* **FRÅGOR. Är ett ”filresursvittne” för ett redundanskluster ett hello användningsfall för Azure File storage?**
   
    Detta stöds inte.

* **FRÅGOR. Finns det en åtgärd för namnbyten i REST API för hello?**
   
    Inte just nu.

* **FRÅGOR. Kan du använda kapslade resurser, med andra ord en resurs under en annan resurs?**
    
    Nej. hello filresursen är hello virtuella drivrutin som du kan montera, så kapslade resurser inte stöds.

* **FRÅGOR. Använda Azure File storage med IBM MQ**
    
    IBM har publicerat ett dokument tooguide IBM MQ-kunder när du konfigurerar Azure File storage med deras tjänst. Mer information finns på [hur toosetup IBM MQ Multi instansen köhanteraren med Microsoft Azures Filtjänst](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).


## <a name="troubleshooting"></a>Felsökning
* **FRÅGOR. Hur felsöker Azure File storage fel?**
    
    Du kan se för[Azure File storage felsökning artikel](storage-troubleshoot-windows-file-connection-problems.md) för slutpunkt till slutpunkt felsökningsinformation. 


## <a name="see-also"></a>Se även
Mer information om Azure File Storage finns på följande länkar.

### <a name="conceptual-articles-and-videos"></a>Begreppsrelaterade artiklar och videoklipp
* [Azure Files Storage: ett friktionslöst SMB-filsystem i molnet för Windows och Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Hur toouse Azure File storage med Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>Verktygsstöd för File Storage
* [Hur toouse AzCopy med Microsoft Azure Storage](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Använda hello Azure CLI med Azure Storage](../common/storage-azure-cli.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)
* [Felsökning av problem i Azure File Storage](storage-troubleshoot-linux-file-connection-problems.md)

### <a name="blog-posts"></a>Blogginlägg
* [Azure File Storage finns nu allmänt tillgänglig](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inuti Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introduktion till Microsoft Azure File Service](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrera data tooAzure File storage](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referens
* [Storage-klientbibliotek för .NET-referens](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [File Service REST API referens](http://msdn.microsoft.com/library/azure/dn167006.aspx)
