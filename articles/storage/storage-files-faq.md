---
title: "Vanliga frågor och svar om Azure File storage | Microsoft Docs"
description: "Få svar på vanliga frågor och svar om Azure File storage."
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
ms.openlocfilehash: cb2134502a585c4f9772e594f635c10f1841e46f
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="frequently-asked-questions-about-azure-file-storage"></a>Vanliga frågor och svar om Azure File storage

## <a name="general"></a>Allmänt
* **FRÅGOR. Vad är Azure File storage?**  
   
    Azure File storage är ett distribuerat filsystem i Azure. Det tillhandahåller ett gränssnitt för SMB-protokollet som tillåter användare att montera lagring som en intern resurs på Azure virtuella datorer eller den lokala datorn.

* **FRÅGOR. Varför är Azure File storage användbar?**  
   
    Azure File storage ger åtkomst till delade data över flera virtuella datorer och plattformar. Referera till [varför Azure File storage är användbar](storage-files-introduction.md#why-azure-file-storage-is-useful).

* **FRÅGOR. När bör jag använda Azure File vs Azure Blob vs Azure-disken?**  
   
    Microsoft Azure tillhandahåller flera olika sätt att lagra och komma åt data i molnet.  
   
    Azure File storage - tillhandahåller ett gränssnitt för SMB, klientbiblioteken, och ett REST-gränssnitt som möjliggör enkel åtkomst från var som helst till lagrade filer.  
   
    Azure BLOB-objekt - tillhandahåller klientbibliotek ett REST-gränssnitt som möjliggör Ostrukturerade data att lagras och används i massiv skala i blockblobbar.  
   
    Azure Data diskar - tillhandahåller klientbibliotek ett REST-gränssnitt som gör att data lagras beständigt och nås från en ansluten virtuell hårddisk.  
   
    Läs mer på [bestämmer när du ska använda Azure BLOB, Azure-filer eller Azure Datadiskar](storage-decide-blobs-files-disks.md)

* **FRÅGOR. Hur kommer jag igång med Azure File storage?**  
   
    Du kan börja med att skapa en Azure-filresursen. Du kan skapa Azure-filresurser med hjälp av Azure Portal, Azure Storage PowerShell-cmdlets, Azure Storage-klientbibliotek eller Azure Storage REST API. I kursen får du lära dig:

    * [Lär dig att skapa Azure-filresurs med hjälp av portalen](storage-file-how-to-create-file-share.md#create-file-share-through-the-portal)
    * [Lär dig att skapa Azure-filresurs med hjälp av Powershell](storage-file-how-to-create-file-share.md#create-file-share-through-powershell)
    * [Lär dig att skapa Azure-filresurs med hjälp av CLI](storage-file-how-to-create-file-share.md#create-file-share-through-command-line-interface-cli)

* **FRÅGOR. Vilka replikeringar stöds av Azure File storage?**  
   
    Azure File storage stöder endast LRS eller GRS just nu. Vi planerar att implementera stöd för RA-GRS men tidsplanen är inte fastställd än.

## <a name="security-authentication-and-access-control"></a>Säkerhet, autentisering och åtkomstkontroll

* **FRÅGOR. Vad är olika sätt att komma åt filer i Azure File storage?**
    
    Du kan montera filresursen på den lokala datorn med hjälp av SMB 3.0-protokollet eller använda verktyg som [Lagringsutforskaren](http://storageexplorer.com/) kan komma åt filer i filresursen. Du kan använda lagringsklientbiblioteken REST API: er eller Powershell för att komma åt dina filer i Azure-filresurs från ditt program.

* **FRÅGOR. Hur ger åtkomst till en viss fil i en webbläsare?**
    
    Med SAS kan generera du token med särskilda behörigheter som gäller för ett angivet tidsintervall. Du kan till exempel generera ett token med skrivskyddad åtkomst till en viss fil under en viss tidsperiod. Alla som har denna url kan komma åt filen direkt från en webbläsare när den är giltig. SAS-nycklar går lätt att generera från användargränssnitt som Storage Explorer.

* **FRÅGOR. Är det möjligt att ange skrivskyddad eller lässkyddad behörighet för mapparna på resursen?**
    
    Du har inte den här kontrollnivån över behörigheterna om du monterar filresursen via SMB. Du kan dock åstadkomma detta genom att skapa en signatur för delad åtkomst (SAS) via REST-API:et eller klientbiblioteken.  

* **FRÅGOR. Hur kan jag aktivera kryptering på serversidan för Azure File storage?**

    [Kryptering på serversidan](storage-service-encryption.md) för Azure File storage är allmänt tillgänglig i alla regioner och offentliga och nationella moln. Du kan aktivera SSE för Azure File storage med [Azure-portalen](https://portal.azure.com/),[Microsoft Azure Storage Resource Provider API](/rest/api/storagerp/storageaccounts), [Azure Powershell](https://msdn.microsoft.com/library/azure/mt607151.aspx) eller [Azure CLI ](storage-azure-cli.md).
    
    När du har aktiverat SSE på Azure File storage krypteras nya data skrivs till file storage i detta lagringskonto automatiskt. Den här funktionen är tillgänglig för alla nya data som skrivs till befintliga eller nya resurser i ett befintligt eller nytt lagringskonto. Inga extra kostnader tillkommer för att aktivera den här funktionen. Läs mer på [aktivera SSE på Azure File storage](storage-service-encryption.md).

* **FRÅGOR. Stöds Active Directory-baserad autentisering av Azure File storage?**
   
    För närvarande stöds inte AD-baserad autentisering eller ACL:er, men vi har det i åtanke för framtida versioner. Nu används i stället Azure Storage-kontonycklar för att tillhandahålla autentisering till filresursen. Vi erbjuder dock en lösning som använder signaturer för delad åtkomst (SAS) via REST-API:et eller klientbiblioteken. Med SAS kan generera du token med särskilda behörigheter som gäller för ett angivet tidsintervall. Du kan till exempel generera en token med skrivskyddad åtkomst till en viss fil med 10 minuter upphör att gälla. Alla som har denna token när den är giltig har skrivskyddad åtkomst till filen för de 10 minuterna.
   
    SAS stöds endast via REST-API:et eller klientbiblioteken. När du monterar filresursen via SMB-protokollet, kan du inte använda en SAS för att delegera åtkomst till innehållet. 
    
* **FRÅGOR. Vilka är de principer för efterlevnad som stöds för Azure File storage?**

   Azure File Storage körs ovanpå samma lagringsarkitekturen som andra storage-tjänster i Azure Storage och tillämpar principer för efterlevnad av samma data. Mer information om kompatibilitet för Azure Storage-data som du kan hämta och referera till [dataskydd i Microsoft Azure-dokumentet](http://go.microsoft.com/fwlink/?LinkID=398382&clcid=0x409).

## <a name="on-premises-access"></a>Lokal åtkomst

* **Q.Do som jag behöver använda Azure ExpressRoute för att ansluta till Azure File storage från en lokal virtuell dator?**
   
    Nej. Om du inte har ExpressRoute kan du fortfarande komma åt filresursen från lokala virtuella datorer under förutsättning att port 445 (TCP utgående) är öppen för Internetåtkomst. Du kan dock använda ExpressRoute med Azure File storage om du vill.

* **FRÅGOR. Hur kan jag montera en filresurs på Azure på min dator?** 
    
    Du kan montera filresursen via SMB-protokollet som port 445 (TCP utgående) är öppen och klienten stöder SMB 3.0-protokollet (till exempel du använder Windows 10 eller Windows Server 2012). Kontakta din lokala Internet-leverantör för att avblockera porten. Under tiden kan du visa dina filer med [Lagringsutforskaren](../vs-azure-tools-storage-explorer-files.md#view-a-file-shares-contents).


## <a name="billing-and-pricing"></a>Fakturering och priser
* **FRÅGOR. Räknas nätverkstrafik mellan en Azure VM och en filresurs som extern bandbredd som debiteras genom prenumerationen?**
   
    Om filresursen och den virtuella datorn finns i samma Azure-region, är trafiken mellan dem kostnadsfri. Om de finns i olika områden debiteras trafiken mellan dem som extern bandbredd.

## <a name="backup"></a>Säkerhetskopiering

* **FRÅGOR. Hur jag för att säkerhetskopiera min Azure-filresursen?**
    
    Du kan använda AzCopy RoboCopy, eller en 3 part säkerhetskopieringsverktyg som kan säkerhetskopiera en monterad filresurs. Vi räknar med att ha möjlighet att ta ögonblicksbilder av filresurser inom en snar framtid. Du kommer att kunna använda funktionen för att säkerhetskopiera dina Azure-filresurs.

## <a name="performance"></a>Prestanda

* **FRÅGOR. Vilka är skalgränser för Azure File storage?**
    Mer information om skalbarhets- och prestandamål för Azure File storage finns [Azure Storage skalbarhets- och prestandamål](storage-scalability-targets.md#scalability-targets-for-blobs-queues-tables-and-files).

* **FRÅGOR. Min prestanda var låga när försök att packa upp filer till Azure File storage. Vad ska jag göra?**
    
    Om du vill överföra stora mängder filer till Azure File storage rekommenderar vi att du använder AzCopy (Windows, förhandsgranskning för Linux/Unix) eller Azure Powershell som dessa verktyg har optimerats för nätverksöverföring.

* **FRÅGOR. Vad korrigeringsfiler har släppts för att åtgärda problemet med låga prestanda med Azure File storage?**
    
    Windows-teamet släppte nyligen en korrigering som löser problemet med långsamma prestanda när kunden ansluter till Azure File storage från Windows 8.1 eller Windows Server 2012 R2. Mer information finns i KB-artikeln, [låga prestanda när du har åtkomst till Azure File storage från Windows 8.1 eller Server 2012 R2](https://support.microsoft.com/kb/3114025).

## <a name="features-and-interoperability-with-other-services"></a>Funktioner och samverkan med andra tjänster
* **FRÅGOR. Är ett ”filresursvittne” för ett redundanskluster ett av användningsscenarierna för Azure File storage?**
   
    Detta stöds inte.

* **FRÅGOR. Finns det en åtgärd för namnbyten i REST API?**
   
    Inte just nu.

* **FRÅGOR. Kan du använda kapslade resurser, med andra ord en resurs under en annan resurs?**
    
    Nej. Filresursen är den virtuella drivrutin som du kan montera, så kapslade resurser stöds inte.

* **FRÅGOR. Använda Azure File storage med IBM MQ**
    
    IBM har publicerat ett dokument som hjälper IBM MQ-kunder när du konfigurerar Azure File storage med deras tjänst. Mer information finns i [Konfigurera IBM MQ MIQM (Multi Instance Queue Manager) med Microsoft Azures filtjänst](https://github.com/ibm-messaging/mq-azure/wiki/How-to-setup-IBM-MQ-Multi-instance-queue-manager-with-Microsoft-Azure-File-Service).


## <a name="troubleshooting"></a>Felsökning
* **FRÅGOR. Hur felsöker Azure File storage fel?**
    
    Du kan referera till [Azure File storage felsökning artikel](storage-troubleshoot-file-connection-problems.md) för slutpunkt till slutpunkt felsökningsinformation. 


## <a name="see-also"></a>Se även
Mer information om Azure File Storage finns på följande länkar.

### <a name="conceptual-articles-and-videos"></a>Begreppsrelaterade artiklar och videoklipp
* [Azure Files Storage: ett friktionslöst SMB-filsystem i molnet för Windows och Linux](https://azure.microsoft.com/documentation/videos/azurecon-2015-azure-files-storage-a-frictionless-cloud-smb-file-system-for-windows-and-linux/)
* [Använd Azure File Storage med Linux](storage-how-to-use-files-linux.md)

### <a name="tooling-support-for-file-storage"></a>Verktygsstöd för File Storage
* [Använd Azure PowerShell med Azure Storage](storage-powershell-guide-full.md)
* [Använd AzCopy med Microsoft Azure Storage](storage-use-azcopy.md)
* [Använd Azure CLI:et med Azure Storage](storage-azure-cli.md)
* [Felsökning av problem i Azure File Storage](storage-troubleshoot-file-connection-problems.md)

### <a name="blog-posts"></a>Blogginlägg
* [Azure File Storage finns nu allmänt tillgänglig](https://azure.microsoft.com/blog/azure-file-storage-now-generally-available/)
* [Inuti Azure File Storage](https://azure.microsoft.com/blog/inside-azure-file-storage/)
* [Introduktion till Microsoft Azure File Service](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)
* [Migrera data till Azure File storage](https://azure.microsoft.com/blog/migrating-data-to-microsoft-azure-files/)

### <a name="reference"></a>Referens
* [Storage-klientbibliotek för .NET-referens](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [File Service REST API referens](http://msdn.microsoft.com/library/azure/dn167006.aspx)
