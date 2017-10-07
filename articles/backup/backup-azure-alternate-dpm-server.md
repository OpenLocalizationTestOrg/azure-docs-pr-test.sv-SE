---
title: "aaaRecover data från ett Azure Backup-Server | Microsoft Docs"
description: "Återställa hello data som du har skyddat tooa Recovery Services-valvet från alla registrerade toothat Azure Backup Server-valvet."
services: backup
documentationcenter: 
author: nkolli1
manager: shreeshd
editor: 
ms.assetid: a55f8c6b-3627-42e1-9d25-ed3e4ab17b1f
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: adigan;giridham;trinadhk;markgal
ms.openlocfilehash: 74847880e646c3c4f198afe318f1db30363d137a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="recover-data-from-azure-backup-server"></a>Återställa data från Azure Backup Server
Du kan använda Azure Backup Server toorecover hello data du säkerhetskopierade tooa Recovery Services-valvet. hello-processen för att göra så är integrerat i hanteringskonsolen för hello Azure Backup Server och är liknande toohello återställningsarbetsflöde för andra Azure Backup-komponenter.

> [!NOTE]
> Den här artikeln gäller för [System Center Data Protection Manager 2012 R2 med UR7 eller senare] (https://support.microsoft.com/en-us/kb/3065246), i kombination med hello [senaste Azure Backup-agenten](http://aka.ms/azurebackup_agent).
>
>

toorecover data från ett Azure Backup-Server:

1. Från hello **Recovery** av hello Azure Backup Server management-konsolen, klicka på **lägga till extern DPM** (på hello upp till vänster på skärmen hello).   
    ![Lägg till extern DPM](./media/backup-azure-alternate-dpm-server/add-external-dpm.png)
2. Hämta nya **valvet autentiseringsuppgifter** från hello valvet som är associerade med hello **Azure Backup Server** där hello data återställs, Välj hello Azure Backup Server hello listan över servrar för Azure-säkerhetskopiering registrerad hello Recovery Services-valvet och ange hello **krypteringslösenfrasen** som associeras med hello server vars data återställs.

    ![Extern DPM-autentiseringsuppgifter](./media/backup-azure-alternate-dpm-server/external-dpm-credentials.png)

   > [!NOTE]
   > Endast Azure Backup-servrar som är associerade med hello samma registrering valvet kan återställa varandras data.
   >
   >

    När hello externa Azure Backup-Server har lagts till, du kan bläddra hello data för hello extern server och hello lokala Azure Backup-Server från hello **Recovery** fliken.
3. Hello tillgängliga bläddringslistan produktionsservrar som skyddas av hello externa Azure Backup-Server och välj lämplig datakälla för hello.

    ![Bläddra externa DPM-servern](./media/backup-azure-alternate-dpm-server/browse-external-dpm.png)
4. Välj **hello månad och år** från hello **återställningspunkter** listrutan väljer hello krävs **Recovery datum** för när hello återställningspunkten har skapats och välj hello **Återställningstid**.

    En lista över filer och mappar som visas i hello längst ned i fönstret, som kan visas och återställas tooany plats.

    ![Återställningspunkter för externa DPM-Server](./media/backup-azure-alternate-dpm-server/external-dpm-recoverypoint.png)
5. Högerklicka på lämplig hello-objektet och klickar på **återställa**.

    ![Externa DPM-återställning](./media/backup-azure-alternate-dpm-server/recover.png)
6. Granska hello **återställa markeringen**. Kontrollera hello data och tid för hello säkerhetskopian som återställs, samt hello datakällan från vilken hello säkerhetskopian skapades. Om hello markeringen är felaktig, klickar du på **Avbryt** toonavigate tillbaka toorecovery lämplig återställningspunkt för fliken tooselect. Om hello markeringen är korrekt, klickar du på **nästa**.

    ![Externa DPM-återställning sammanfattning](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-summary.png)
7. Välj **återställa tooan alternativ plats**. **Bläddra** toohello rätt plats för hello återställning.

    ![Extern DPM alternativ återställningsplats](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-alternate-location.png)
8. Välj alternativet för hello relaterade för**Skapa kopia**, **hoppa över**, eller **Skriv över**.

   * **Skapa kopia** -skapar en kopia av hello-filen om det finns en namnkollision.
   * **Hoppa över** – om det finns en namnkollision kan inte återställa hello-fil som lämnar hello originalfilen.
   * **Skriv över** – om det finns en namnkollision skriver över hello befintliga kopian av hello-filen.

     Välj lämpligt alternativ för hello för**Återställ säkerhet**. Du kan använda hello säkerhetsinställningar för hello måldatorn där hello data återställs eller hello säkerhetsinställningar som var tillämpliga tooproduct vid hello tidpunkt hello återställningspunkten skapades.

     Identifiera om en **meddelande** skickas när hello återställning har slutförts.

     ![Extern DPM Recovery meddelanden](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-notifications.png)
9. Hej **sammanfattning** skärmen visar hello alternativ valt hittills. När du klickar på **”återställa”**, hello data är återställda toohello lämplig lokal plats.

    ![Extern DPM Recovery alternativ sammanfattning](./media/backup-azure-alternate-dpm-server/external-dpm-recovery-options-summary.png)

   > [!NOTE]
   > hello återställningsjobbet kan övervakas i hello **övervakning** fliken hello Azure Backup Server.
   >
   >

    ![Övervaka återställning](./media/backup-azure-alternate-dpm-server/monitoring-recovery.png)
10. Du kan klicka på **Rensa extern DPM** på hello **Recovery** fliken hello DPM server tooremove hello vy över hello externa DPM-servern.

    ![Rensa extern DPM](./media/backup-azure-alternate-dpm-server/clear-external-dpm.png)

## <a name="troubleshooting-error-messages"></a>Felsökning av felmeddelanden
| Nej. | Felmeddelande | Felsökningssteg |
|:---:|:--- |:--- |
| 1. |Den här servern är inte registrerad toohello valvet som är angivet av valvautentiseringen hello. |**Orsak:** det här felet uppstår när hello valvautentiseringsfilen som valts inte tillhör toohello Recovery Services-valvet som är associerade med Azure Backup-Server på vilken hello återställning görs. <br> **Lösning:** Download hello valvautentiseringsfilen från hello Recovery Services-valvet toowhich hello Azure Backup Server är registrerad. |
| 2. |Hello återställningsbara data är inte tillgänglig eller valda hello-servern är inte en DPM-server. |**Orsak:** det finns inga andra Azure Backup-servrar registrerade toohello Recovery Services-valvet, hello servrar ännu inte överförts hello metadata eller valda hello-servern är inte en Azure Backup Server (även kallat Windows Server eller Windows-klient). <br> **Lösning:** om det finns andra Azure Backup-servrar registrerade toohello Recovery Services-valvet, kontrollera att hello senaste Azure Backup-agenten är installerad. <br>Om det finns andra Azure Backup-servrar registrerade toohello Recovery Services-valv, vänta på en dag efter att installationen toostart hello återställningsprocessen. hello nattetid kommer att överföra hello metadata för alla skyddade hello säkerhetskopieringar toocloud. hello data ska vara tillgängliga för återställning. |
| 3. |Ingen annan DPM-server är registrerad toothis valvet. |**Orsak:** inga andra Azure Backup-servrar som är registrerade toohello valvet från vilka hello återställning görs.<br>**Lösning:** om det finns andra Azure Backup-servrar registrerade toohello Recovery Services-valvet, kontrollera att hello senaste Azure Backup-agenten är installerad.<br>Om det finns andra Azure Backup-servrar registrerade toohello Recovery Services-valv, vänta på en dag efter att installationen toostart hello återställningsprocessen. hello nattetid filöverföringar hello metadata för alla skyddade säkerhetskopior toocloud. hello data ska vara tillgängliga för återställning. |
| 4. |hello angivna krypteringslösenfrasen matchar inte lösenfrasen som associeras med hello följande server:**<server name>** |**Orsak:** hello krypteringslösenfras som används i hello processen för att kryptera hello data från hello Azure Backup-serverns data som återställs matchar inte den angivna krypteringslösenfrasen hello. hello-agenten är toodecrypt hello data. Därför misslyckas hello återställningen.<br>**Lösning:** ange hello exakt samma krypteringslösenfrasen som associeras med hello Azure Backup Server vars data återställs. |

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

### <a name="why-cant-i-add-an-external-dpm-server-after-installing-ur7-and-latest-azure-backup-agent"></a>Varför kan jag lägga till en extern DPM-server när du har installerat UR7 och senaste Azure Backup-agenten?

För hello DPM-servrar med datakällor som är skyddade toohello molnet (med hjälp av en samlad uppdatering tidigare än Update Rollup 7), måste du vänta minst en dag efter installationen hello UR7 och den senaste Azure Backup agent, toostart **lägga till extern DPM-servern** . hello en dag är tidsperiod nödvändiga tooupload hello metadata för hello DPM skydd grupper tooAzure. Skydd grupp metadata överförs hello första gången via ett jobb varje natt.

### <a name="what-is-hello-minimum-version-of-hello-microsoft-azure-recovery-services-agent-needed"></a>Vad är hello lägsta version av hello Microsoft Azure Recovery Services-agenten behövs?

hello lägsta version av hello Microsoft Azure Recovery Services-agenten eller Azure Backup-agenten, krävs tooenable den här funktionen är 2.0.8719.0.  tooview hello agent-version: Öppna Kontrollpanelen  **>**  alla objekt  **>**  program och funktioner  **>**  Microsoft Azure Recovery Services-agenten. Om hello version är mindre än 2.0.8719.0, hämta och installera hello [senaste Azure Backup-agenten](https://go.microsoft.com/fwLink/?LinkID=288905).

![Rensa extern DPM](./media/backup-azure-alternate-dpm-server/external-dpm-azurebackupagentversion.png)

## <a name="next-steps"></a>Nästa steg:
• [Azure Backup vanliga frågor och svar](backup-azure-backup-faq.md)
