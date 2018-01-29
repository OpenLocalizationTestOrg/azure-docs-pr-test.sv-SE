---
title: "Felsöka Azure Backup-agenten | Microsoft Docs"
description: "Felsökning av installation och registrering av Azure Backup-agenten"
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shreeshd
editor: 
ms.assetid: 778c6ccf-3e57-4103-a022-367cc60c411a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/4/2017
ms.author: saurse;markgal;
ms.openlocfilehash: 52a32d61dd69110ace560fd1e52389140f322678
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/19/2017
---
# <a name="troubleshoot-azure-backup-agent-configuration-and-registration-issues"></a>Felsökning av problem för registrering och konfiguration av Azure Backup-agenten
## <a name="recommended-steps"></a>Rekommenderade åtgärder
Referera till de rekommenderade åtgärderna i följande tabeller för att lösa problem som kan uppstå under konfigurationen eller registrering av Azure Backup Agent.

## <a name="invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Ogiltiga valvautentiseringsuppgifter har angetts. Filen är antingen skadad eller har inte har de senaste autentiseringsuppgifterna som är associerade med återställningstjänsten.
| Felinformation | Möjliga orsaker | Rekommenderade åtgärder |
| ---     | ---     | ---    |
| **Fel** </br> *Ogiltiga valvautentiseringsuppgifter har angetts. Filen är antingen skadad eller har inte har de senaste autentiseringsuppgifterna som är associerade med återställningstjänsten. (ID: 34513)* | <ul><li> Autentiseringsuppgifter för valv är ogiltig (det vill säga de hämtades mer än 48 timmar innan registreringen).<li>Azure Backup-agenten kan inte hämta en temporär fil i mappen Windows Temp. <li>Valvautentiseringsuppgifter som finns på en nätverksplats. <li>TLS 1.0 är inaktiverad<li> En konfigurerad proxyserver blockerar anslutningen. <br> |  <ul><li>Hämta de nya autentiseringsuppgifterna för valvet.<li>Gå till **Internetalternativ** > **säkerhet** > **Internet**. Välj därefter **Anpassad nivå**, och Bläddra tills du ser att hämta filen. Välj sedan **aktivera**.<li>Du kan också behöva lägga till webbplatsen till [tillförlitliga platser](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Ändra inställningarna för att använda en proxyserver. Ange proxy-serverinformation. <li> Matcha datum och tid till din dator.<li>Gå till C:/Windows/Temp och kontrollera om det finns mer än 60 000 eller 65 000 filer med filnamnstillägget TMP. Om det finns kan du ta bort dessa filer.<li>Du kan testa detta genom att köra SDP-paketet på servern. Om du får ett felmeddelande om att filhämtningar inte tillåts, är det mycket troligt att det finns ett stort antal filer i C:/Windows/Temp-katalogen.<li>Se till att du har .NET framework 4.6.2 installerat. <li>Om du har inaktiverat TLS 1.0 på grund av PCI-överensstämmelse, [felsökning sidan](https://support.microsoft.com/help/4022913). <li>Om du har ett antivirusprogram installerat på servern, kan du undanta följande filer från virusgenomsökning: <ul><li>CBengine.exe<li>CSC.exe, som är relaterade till .NET Framework. Det finns en CSC.exe för varje .NET-version som är installerad på servern. Undanta CSC.exe-filer som är knutna till alla versioner av .NET framework på servern. <li>Scratch plats för mappen eller cache. <br>*Standardplatsen för den tillfälliga mappen eller sökvägen till cache är C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Microsoft Azure Recovery Service-agenten kunde inte ansluta till Microsoft Azure Backup

| Felinformation | Möjliga orsaker | Rekommenderade åtgärder |
| ---     | ---     | ---    |
| **Fel** </br><ol><li>*Microsoft Azure Recovery Service-agenten kunde inte ansluta till Microsoft Azure Backup. (ID: 100050) Kontrollera nätverksinställningarna och se till att du är ansluten till internet*<li>*(407) proxyautentisering krävs* |Proxy blockerar anslutningen. |  <ul><li>Gå till **Internetalternativ** > **säkerhet** > **Internet**. Välj sedan **Anpassad nivå** och Bläddra tills du ser att hämta filen. Välj **Aktivera**.<li>Du kan också behöva lägga till webbplatsen till [tillförlitliga platser](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Ändra inställningarna för att använda en proxyserver. Ange proxy-serverinformation. <li>Om du har ett antivirusprogram installerat på servern, Uteslut följande filer från ett virusskyddsprogram genomsökningen. <ul><li>CBEngine.exe (i stället för dpmra.exe).<li>CSC.exe (relaterade till .NET Framework). Det finns en CSC.exe för varje .NET-version som är installerad på servern. Undanta CSC.exe-filer som är knutna till alla versioner av .NET framework på servern. <li>Scratch plats för mappen eller cache. <br>*Standardplatsen för den tillfälliga mappen eller sökvägen till cache är C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Det gick inte att ange krypteringsnyckeln för säker säkerhetskopieringar

| Felinformation | Möjliga orsaker | Rekommenderade åtgärder |
| ---     | ---     | ---    |      
| **Fel** </br>*Kunde inte ange krypteringsnyckeln för säker säkerhetskopieringar. Den aktuella åtgärden misslyckades på grund av ett internt fel: Ogiltig inkommande tjänstfel'. Försök igen efter en stund. Kontakta Microsoft-supporten om problemet kvarstår*. |Servern är redan registrerad med ett annat valv.| Avregistrera servern från valvet och registrera igen.

## <a name="the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Aktiveringen slutfördes inte. Den aktuella åtgärden misslyckades på grund av ett internt tjänstfel [0x1FC07]

| Felinformation | Möjliga orsaker | Rekommenderade åtgärder |
| ---     | ---     | ---    |          
| **Fel** </br><ol><li>*Aktiveringen slutfördes inte. Den aktuella åtgärden misslyckades på grund av ett internt tjänstfel [0x1FC07]. Försök igen efter en stund. Kontakta Microsoft-supporten om problemet kvarstår* <li>*Fel 34506. Den krypteringslösenfras som sparats på den här datorn är inte korrekt konfigurerad*. | <li> Den tillfälliga mappen finns på en volym som har tillräckligt med utrymme. <li> Den tillfälliga mappen har felaktigt flyttats till en annan plats. <li> Filen OnlineBackup.KEK saknas. | <li>Flytta den tillfälliga mappen eller cacheplats till en volym med ledigt utrymme som motsvarar 5-10% av den totala storleken för säkerhetskopierade data. Om du vill flytta placeringen i cachen, följer du stegen i [frågor om Azure Backup-agenten](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Kontrollera att filen OnlineBackup.KEK finns. <br>*Standardplatsen för den tillfälliga mappen eller sökvägen till cache är C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.

## <a name="next-steps"></a>Nästa steg
* Visa mer detaljerad information om [hur du säkerhetskopierar Windows Server med Azure Backup-agenten](tutorial-backup-windows-server-to-azure.md).
* Om du behöver återställa en säkerhetskopia använder du den här artikeln för att [återställa filer till en Windows-dator](backup-azure-restore-windows-server.md).
