---
title: aaaMount Azure filresursen via SMB med macOS | Microsoft Docs
description: "Lär dig hur toomount en Azure-fil dela över SMB med macOS."
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
ms.openlocfilehash: 7b4924cb42247470521c1ae8b9d03ab1756996e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-share-over-smb-with-macos"></a>Montera en Azure-filresurs via SMB med macOS
[Azure File storage](storage-dotnet-how-to-use-files.md) Microsofts tjänst som gör att du toocreate och använda nätverksfilresurser i hello Azure använder hello branschstandard. Azure-filresurser kan monteras i macOS Sierra (10.12) och El Capitan (10.11). Den här artikeln innehåller två olika sätt toomount en Azure-filresurs på macOS med hjälp av hello Terminal och hello Finder Användargränssnittet.

> [!Note]  
> Innan du monterar en Azure-filresurs via SMB rekommenderar vi att du inaktiverar signering av SMB-paket. Inte gör det kan ge sämre prestanda vid åtkomst till filresursen för hello Azure från macOS. SMB-anslutning krypteras så att detta inte påverkar hello säkerheten för din anslutning. Från hello terminal, hello följande kommandon inaktiverar SMB-signering för paket, som beskrivs i den här [Apple support-artikeln om hur du inaktiverar SMB-signering av paket](https://support.apple.com/HT205926):  
>    ```
>    sudo -s
>    echo "[default]" >> /etc/nsmb.conf
>    echo "signing_required=no" >> /etc/nsmb.conf
>    exit
>    ```

## <a name="prerequisites-for-mounting-an-azure-file-share-on-macos"></a>Krav för att montera en Azure-filresurs på macOS
* **Lagringskontonamnet**: toomount ett Azure-filresurs, du behöver hello namnet på hello storage-konto.

* **Lagringskontonyckel**: toomount ett Azure-filresurs, du behöver hello primära (eller sekundära) lagringsnyckel. SAS-nycklar stöds inte för montering.

* **Kontrollera att port 445 är öppen**: SMB kommunicerar via TCP-port 445. Kontrollera att inte brandväggen blockerar TCP-port 445 toomake på din klientdator (hello Mac).

## <a name="mount-an-azure-file-share-via-finder"></a>Montera en Azure-filresurs via Finder
1. **Öppna Finder**: Finder är öppen på macOS som standard, men du kan kontrollera att det är hello markerat program genom att klicka på hello ”macOS står inför ikonen” hello dock:  
    ![hello macOS står inför ikon](media/storage-file-how-to-use-files-mac/mount-via-finder-1.png)

2. **Välj ”ansluta tooServer” hello ”gå” menyn**: med hello UNC-sökväg från hello [krav](#preq), konvertera hello början dubbla omvänt snedstreck (`\\`) för`smb://` och alla andra omvända snedstreck (`\`) tooforwards snedstreck (`/`). Länken bör se ut som följande hello: ![hello ”ansluta tooServer” dialogrutan](./media/storage-file-how-to-use-files-mac/mount-via-finder-2.png)

3. **Använd hello resursen namn och lagring kontonyckel när du tillfrågas om användarnamn och lösenord**: när du klickar på ”Anslut” hello ”ansluta tooServer” dialogrutan tillfrågas du om hello användarnamn och lösenord (detta är autopopulated med din macOS användarnamnet). Du har hello möjlighet att placera hello resursen namn/lagringskontonyckel i din macOS nyckelringar.

4. **Använd hello Azure-filresurs som önskade**: hello resursen monteras efter ersätter hello resursen namn och lagring kontonyckel i hello användarnamn och lösenord. Du kan använda som du vanligtvis använder en lokal mapp/filresurs, inklusive dra och släppa filer i hello filresurs:

    ![En ögonblicksbild av en monterad Azure-filresurs](./media/storage-file-how-to-use-files-mac/mount-via-finder-3.png)

## <a name="mount-an-azure-file-share-via-terminal"></a>Montera en Azure-filresurs via Terminal
1. Ersätt `<storage-account-name>` med hello namnet på ditt lagringskonto. Ange lagringskontonyckeln som lösenord när du uppmanas att ange lösenord. 

    ```
    mount_smbfs //<storage-account-name>@<storage-account-name>.file.core.windows.net/<share-name> <desired-mount-point>
    ```

2. **Använd hello Azure-filresurs som önskade**: hello Azure-filresurs monteras på hello monteringspunkt som anges av hello föregående kommando.  

    ![En ögonblicksbild av hello monterade Azure-filresurs](./media/storage-file-how-to-use-files-mac/mount-via-terminal-1.png)

## <a name="next-steps"></a>Nästa steg
Mer information om Azure File Storage finns på följande länkar.

* [Support-artikeln Apple - hur tooconnect med fildelning på din Mac.](https://support.apple.com/HT204445)
* [Vanliga frågor och svar](storage-files-faq.md)
* [Felsökning](storage-troubleshoot-file-connection-problems.md)