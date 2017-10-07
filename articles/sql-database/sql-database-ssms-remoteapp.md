---
title: "aaaConnect tooSQL databasen med hjälp av SQL Server Management Studio i Azure RemoteApp | Microsoft Docs"
description: "Använd den här självstudiekursen toolearn hur toouse SQL Server Management Studio i Azure RemoteApp för säkerhet och prestanda när du ansluter tooSQL databas"
services: sql-database
documentationcenter: 
author: adhurwit
manager: jhubbard
ms.assetid: 1052c83c-e7f5-4736-922f-216194d8874b
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: adhurwit
ms.openlocfilehash: 73994f9a1eb3e48efa5d7c4f976b00cfcbc88d75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-sql-server-management-studio-in-azure-remoteapp-tooconnect-toosql-database"></a>Använda SQL Server Management Studio i Azure RemoteApp tooconnect tooSQL databas

> [!IMPORTANT]
> Azure RemoteApp upphör att gälla. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
>

## <a name="introduction"></a>Introduktion
De här självstudierna visar hur toouse SQL Server Management Studio (SSMS) i Azure RemoteApp tooconnect tooSQL databas. Vägleder dig genom processen hello för att konfigurera SQL Server Management Studio i Azure RemoteApp, förklarar hello fördelar och visar säkerhetsfunktioner som du kan använda i Azure Active Directory.

**Uppskattad tid toocomplete:** 45 minuter

## <a name="ssms-in-azure-remoteapp"></a>SSMS i Azure RemoteApp
Azure RemoteApp är en RDS-tjänst i Azure som levererar program. Du kan lära dig mer om den här: [vad är RemoteApp?](../remoteapp/remoteapp-whatis.md)

SSMS som körs i Azure RemoteApp ger hello samma upplevelse som körs lokalt SSMS.

![Skärmbild som visar SSMS som körs i Azure RemoteApp][1]

## <a name="benefits"></a>Fördelar
Det finns många fördelar toousing SSMS i Azure RemoteApp, inklusive:

* Port 1433 på Azure SQL server har inte toobe exponeras externt (utanför Azure).
* Inga måste tookeep att lägga till och ta bort IP-adresser i brandväggen för hello Azure SQL server.
* Alla Azure RemoteApp-anslutningar som sker över HTTPS med port 443 krypterade Remote Desktop protocol
* Det är flera användare och kan skalas.
* Finns det prestandafördelar från med SSMS i hello samma region som hello SQL-databas.
* Du kan granska användning av Azure RemoteApp med hello Premium edition av Azure Active Directory som har aktivitetsrapporter för användaren.
* Du kan aktivera multifaktorautentisering (MFA).
* Åtkomst SSMS var som helst när du använder någon av hello stöds Azure RemoteApp-klienter som innehåller iOS, Android, Mac, Windows Phone och Windows-dator.

## <a name="create-hello-azure-remoteapp-collection"></a>Skapa hello Azure RemoteApp-samling
Här följer hello steg toocreate Azure RemoteApp-samlingen med SSMS:

### <a name="1-create-a-new-windows-vm-from-image"></a>1. Skapa en ny Windows virtuell dator från bild
Använd hello ”Windows Server Remote Desktop Session Host Windows Server 2012 R2” bilden från hello galleriet toomake den nya virtuella datorn.

### <a name="2-install-ssms-from-sql-express"></a>2. Installera SSMS från SQL Express
Gå till hello ny virtuell dator och navigera toothis hämtningssidan: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/download/details.aspx?id=42299)

Det finns en alternativet tooonly nedladdning SSMS. Gå till installationskatalogen för hello efter hämtning, och kör installationsprogrammet tooinstall SSMS.

Du måste också tooinstall SQL Server 2014 Service Pack 1. Du kan hämta: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=46694)

SQL Server 2014 Service Pack 1 innehåller grundläggande funktioner för att arbeta med Azure SQL Database.

### <a name="3-run-validate-script-and-sysprep"></a>3. Kör Validera skript och Sysprep
På hello är hello Virtuella skrivbord ett PowerShell.skript som heter verifiera. Kör detta genom att dubbelklicka på. Den verifierar att hello VM är klar toobe som används för fjärråtkomst att vara värd för program. När verifieringen är klar uppmanas toorun sysprep - Välj toorun den.

När sysprep har slutförts stängs hello VM.

toolearn mer om hur du skapar en Azure RemoteApp-avbildning, se: [hur toocreate en mall för RemoteApp-avbildning i Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)

### <a name="4-capture-image"></a>4. Avbildning
När hello VM har avbrutits, hitta i aktuella hello-portalen och fånga den.

toolearn mer information om en avbildning finns [en avbildning av en virtuell dator för Windows Azure som skapats med hello klassiska distributionsmodellen](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="5-add-tooazure-remoteapp-template-images"></a>5. Lägg till tooAzure RemoteApp-mallavbildningar
Gå toohello Mallavbildningarna fliken hello Azure RemoteApp-avsnittet i aktuella hello-portalen, och klicka på Lägg till. Markera ”Importera en bild från biblioteket för virtuella datorer” i hello popup-ruta och välj sedan hello bilden som du nyss skapade.

### <a name="6-create-cloud-collection"></a>6. Skapa molnsamling
Skapa en ny Azure RemoteApp-Molnsamling i aktuella hello-portalen. Välj hello-Mallavbildningen som du nyss importerade med SSMS som är installerade på den.

![Skapa ny molnsamling][2]

### <a name="7-publish-ssms"></a>7. Publicera SSMS
Hej på hello publicering för den nya samlingen i molnet, välja publicera ett program från Start-menyn och välj sedan SSMS hello-listan.

![Publicera appen][5]

### <a name="8-add-users"></a>8. Lägga till användare
Du kan välja hello-användare som ska ha åtkomst toothis Azure RemoteApp-samling som bara innehåller SSMS hello användaråtkomst på fliken.

![Lägg till användare][6]

### <a name="9-install-hello-azure-remoteapp-client-application"></a>9. Installera klientprogrammet för hello Azure RemoteApp
Du kan hämta och installera en Azure RemoteApp-klient här: [hämta | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)

## <a name="configure-azure-sql-server"></a>Konfigurera Azure SQL-server
hello har konfiguration krävs är tooensure Azure Services aktiverats för hello brandväggen. Om du använder den här lösningen kan sedan behöver du inte tooadd alla IP-adresser tooopen hello brandväggen. hello trafik som tillåts toohello SQL Server är från andra Azure-tjänster.

![Tillåt Azure][4]

## <a name="multi-factor-authentication-mfa"></a>Multifaktorautentisering (MFA)
MFA kan aktiveras för det här programmet specifikt. Gå toohello program fliken i Azure Active Directory. Du hittar en post för Microsoft Azure RemoteApp. Om du klickar på programmet och sedan konfigurera visas hello sida där du kan aktivera MFA för det här programmet.

![Aktivera MFA][3]

## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Gransknings-och användaraktivitet med Azure Active Directory Premium
Om du inte har Azure AD Premium kan du ha tooturn hello den på i avsnittet licenser i din katalog. Med Premium aktiverat, kan du tilldela användare toohello Premium-nivå.

När du går tooa användare i Azure Active Directory går du sedan toohello aktivitet fliken toosee inloggningen information tooAzure RemoteApp.

## <a name="next-steps"></a>Nästa steg
När du har slutfört alla hello senare steg du ska kunna toorun hello Azure RemoteApp-klienten och logga in med en tilldelad användare. Visas med SSMS som en av dina program och du kan köra den som om den har installerats på datorn med åtkomst tooAzure SQLServer.

Mer information om hur toomake hello anslutning tooSQL databasen finns [ansluta tooSQL databasen med SQL Server Management Studio och utföra en exempelfråga i T-SQL](sql-database-connect-query-ssms.md).

Det är allt som för tillfället. Ha det så kul!

<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png