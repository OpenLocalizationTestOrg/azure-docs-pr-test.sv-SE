---
title: "aaaAzure Analysis Services-databas säkerhetskopierar och återställer | Microsoft Docs"
description: "Beskriver hur toobackup och återställning av en Azure Analysis Services-databasen."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: cf0a782d237a95fdfa5ef628f998bd053aac0d9f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="backup-and-restore"></a>Säkerhetskopiering och återställning

Säkerhetskopiera tabellmodell databaser i Azure Analysis Services är mycket hello samma som för lokala Analysis Services. hello viktigaste skillnaden är där du vill lagra säkerhetskopiorna. Säkerhetskopieringsfilerna måste sparas tooa behållare i en [Azure storage-konto](../storage/common/storage-create-storage-account.md). Du kan använda ett lagringskonto och behållare som du redan har eller de kan skapas när du konfigurerar inställningarna för servern.

> [!NOTE]
> Skapa ett lagringskonto kan resultera i en ny fakturerbar tjänst. Det finns fler toolearn [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).
> 
> 

Säkerhetskopieringar sparas med tillägget abf. För InMemory-tabellmodeller lagras både modelldata och metadata. Endast modellmetadata lagras för DirectQuery-tabellmodeller. Säkerhetskopieringar kan komprimeras och krypteras, beroende på hello du väljer. 



## <a name="configure-storage-settings"></a>Konfigurera inställningar för lagring
Innan du säkerhetskopierar, måste tooconfigure lagringsinställningarna för servern.


### <a name="tooconfigure-storage-settings"></a>tooconfigure Lagringsinställningar
1.  I Azure-portalen > **inställningar**, klickar du på **säkerhetskopiering**.

    ![Säkerhetskopieringar i inställningarna](./media/analysis-services-backup/aas-backup-backups.png)

2.  Klicka på **aktiverad**, klicka på **Lagringsinställningar**.

    ![Aktivera](./media/analysis-services-backup/aas-backup-enable.png)

3. Välj ditt lagringskonto eller skapa en ny.

4. Välj en behållare eller skapa en ny.

    ![Välj behållare](./media/analysis-services-backup/aas-backup-container.png)

5. Spara inställningarna för säkerhetskopiering.

    ![Spara inställningar för säkerhetskopiering](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a>Säkerhetskopiering

### <a name="toobackup-by-using-ssms"></a>toobackup med hjälp av SSMS

1. Högerklicka på en databas i SSMS, > **säkerhetskopiera**.

2. I **Backup Database** > **säkerhetskopian**, klickar du på **Bläddra**.

3. I hello **spara fil som** dialogrutan verifiera hello mappsökvägen och skriv sedan ett namn för hello säkerhetskopian. 

4. I hello **Backup Database** dialogrutan, Välj alternativ.

    **Tillåt skriva över** – Välj det här alternativet toooverwrite säkerhetskopior av hello samma namn. Om inte det här alternativet väljs hello-filen som du sparar kan inte ha hello samma namn som en fil som redan finns i hello samma plats.

    **Tillämpa komprimering** – Välj det här alternativet toocompress hello säkerhetskopierade filen. Komprimerade säkerhetskopieringsfiler spara diskutrymme, men kräver något högre CPU-användning. 

    **Kryptera säkerhetskopian** – Välj det här alternativet tooencrypt hello säkerhetskopierade filen. Det här alternativet kräver en användardefinierat lösenord toosecure hello säkerhetskopia. hello lösenordet går inte att läsa av hello säkerhetskopieringsdata något annat sätt än en återställning. Om du väljer tooencrypt säkerhetskopieringar hello lösenordet lagras på en säker plats.

5. Klicka på **OK** toocreate och spara hello säkerhetskopian.


### <a name="powershell"></a>PowerShell
Använd [säkerhetskopiering ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) cmdlet.

## <a name="restore"></a>Återställ
När du återställer, måste din säkerhetskopia vara i hello storage-konto som du har konfigurerat för din server. Om du behöver toomove säkerhetskopian från en lokal plats tooyour storage-konto, använder [Microsoft Azure Lagringsutforskaren](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) eller hello [AzCopy](../storage/common/storage-use-azcopy.md) kommandoradsverktyg. 



> [!NOTE]
> Om du återställa från en lokal server, måste du ta bort alla hello domänanvändare från hello modellen roller och Lägg till dem tillbaka toohello roller som Azure Active Directory-användare.
> 
> 

### <a name="toorestore-by-using-ssms"></a>toorestore med hjälp av SSMS

1. Högerklicka på en databas i SSMS, > **återställa**.

2. I hello **Backup Database** dialogrutan i **säkerhetskopian**, klickar du på **Bläddra**.

3. I hello **Sök efter databasfiler** dialogrutan, Välj hello-fil som du vill toorestore.

4. I **återställningsdatabasen**väljer hello-databasen.

5. Ange alternativ. Säkerhetsalternativ måste matcha hello säkerhetskopieringsalternativ som du använde när du säkerhetskopierar.


### <a name="powershell"></a>PowerShell

Använd [återställning ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) cmdlet.


## <a name="related-information"></a>Relaterad information

[Azure storage-konton](../storage/common/storage-create-storage-account.md)  
[Hög tillgänglighet](analysis-services-bcdr.md)     
[Hantera Azure Analysis Services](analysis-services-manage.md)
