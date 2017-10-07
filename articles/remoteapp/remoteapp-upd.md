---
title: "aaaHow stöder Azure RemoteApp Spara användardata och inställningar? | Microsoft Docs"
description: "Lär dig hur Azure RemoteApp sparar användardata med hello användarprofil-disk."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: e125af62-5c1a-4186-a238-52f537f7bb34
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: dccebc7861e8a0d87cb3ee174f399a2df7fe023c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-remoteapp-save-user-data-and-settings"></a>Hur Azure RemoteApp Spara användardata och inställningar?
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Azure RemoteApp sparar användarens identitet och anpassningar över enheter och sessioner. Den här informationen lagras i en per användare per samling disk, kallas även en användarprofil-disk (UPD). hello disken följer hello användaren och garanterar hello användaren har en konsekvent upplevelse oavsett var de loggar in.

Användarprofil-diskar är helt transparent toohello användare – användare sparar dokument tootheir dokument-mappen (på vad som visas toobe en lokal enhet) och ändra deras appinställningar som vanligt. AT hello samma tid, alla personliga inställningar kvar när du ansluter tooAzure RemoteApp från valfri enhet. Alla hello användaren ser sina data i hello är samma plats.

Varje UPD har 50GB beständig lagring och innehåller inställningar för både användare och program. 

Innehåller information om krav på data för användarprofiler.

> [!NOTE]
> Behöver toodisable hello UPD? Du kan göra den nu - utcheckningen Pavithras blogginlägget [inaktivera användarprofil-diskar (UPD: ar) i Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/), mer information.
> 
> 

## <a name="how-can-an-admin-get-toohello-data"></a>Hur kan en administratör få toohello data?
Om du behöver tooaccess hello data för en av dina användare (för katastrofåterställning eller om hello användaren lämnar företaget hello) kontakta Azure-supporten och ange hello prenumerationsinformation för hello samlingen och hello användaridentitet. hello Azure RemoteApp-teamet ger dig en URL-toohello VHD. Hämta den virtuella Hårddisken och hämta alla dokument och filer som du behöver. Observera att hello VHD är 50GB, så att det tar en bit toodownload den.

## <a name="is-hello-data-backed-up"></a>Hello data säkerhetskopieras?
Ja, vi sparar en säkerhetskopia av hello användardata per geografisk plats. hello data är skrivskyddad och kan nås i hello samma sätt som vanliga hello-data (kontakta Azure RemoteApp tooget den), om hello primära Datacenter är nere. hello data är kopierade realtid toohello säkerhetskopieringsplatsen och vi behålla inte kopior av olika versioner. På skadade data, vi kommer därför inte kan toorestore den tooa som tidigare kallades fungerande versionen men om hello primära Datacenter är nere, kommer du att kunna tooget användardata från hello annan plats.

## <a name="how-do-users-see-hello-upd-on-hello-server-side"></a>Hur kan användare se hello UPD på serversidan hello?
Varje användare kommer ha sina egna kataloger på hello-server som avbildar tootheir UPD: c:\Users\username.

## <a name="whats-hello-best-way-toouse-outlook-and-upd"></a>Vad är hello bästa sätt toouse Outlook och UPD?
Azure RemoteApp sparar hello Outlook tillstånd (postlådor PST) mellan sessioner. tooenable kan vi behöver hello PST toobe lagras i data för användarprofiler hello (c:\users\<användarnamn >). Detta är hello standardplatsen för hello data, så så länge du inte ändrar hello plats, hello data behålls mellan sessioner.

Vi rekommenderar också att du använder ”cacheläge” i Outlook och använda ”server/online” läge för sökning.

Checka ut [i den här artikeln](remoteapp-outlook.md) för mer information om hur du använder Outlook och Azure RemoteApp.

## <a name="what-about-redirection"></a>Nyheter om omdirigering?
Du kan konfigurera Azure RemoteApp toolet användare åtkomst till lokala enheter genom att ställa in [omdirigering](remoteapp-redirection.md). Lokala enheter kommer sedan att kunna tooaccess hello data på hello UPD.

## <a name="can-i-use-my-upd-as-a-network-share"></a>Kan jag använda min UPD som en nätverksresurs?
Nej. UPD: ar kan inte användas som en nätverksresurs. En UPD är bara tillgängliga toohello användare när hello användare är aktivt ansluten tooAzure RemoteApp.

## <a name="if-i-delete-a-user-from-a-collection-is-their-upd-deleted"></a>Om jag tar bort en användare från en samling sina UPD raderas?
Nej, när du tar bort en användare kan vi automatiskt bort inte hello UPD - istället Vi lagrar hello data tills du tar bort hello samling. 90 dagar efter att du tar bort hello samling vi att ta bort alla UPD: ar. 

Om du behöver toodelete en UPD från en samling kan kontakta Azure RemoteApp - vi kan ta bort UPD från vår sida.

## <a name="can-i-access-my-users-upds-either-current-or-deleted-users"></a>Kan jag åt mina användare UPD: ar (aktuella eller borttagna användare)?
Ja, om du kontaktar [Azure RemoteApp](mailto:remoteappforum@microsoft.com), vi kan skapa du med en URL-tooaccess hello data. Har du om 10 timmar toodownload alla data och filer från hello UPD innan hello åtkomst upphör att gälla.

## <a name="are-upds-available-offline"></a>Är UPD: ar tillgängliga offline?
Nu har vi inte ger offlineåtkomst tooUPDs utöver hello 10 timmar access-fönstret som beskrivs ovan. Det innebär att vi inte har en sätt tooprovide du med åtkomst till för tillräckligt lång toocomplete mer komplicerade uppgifter, som att köra antivirusprogram på hello UPD: ar eller dataåtkomst för en granskningslogg.

## <a name="do-registry-key-settings-persist"></a>Gör inställningar för registernycklar sparas?
Ja, något skrivs tooHKEY_Current_User är en del av hello UPD.

## <a name="can-i-disable-upds-for-a-collection"></a>Kan jag inaktivera UPD: ar för en samling?
Ja, du kan be Azure RemoteApp toodisable UPD: ar för en prenumeration, men du kan inte göra den själv. Det innebär att UPD: ar kommer att inaktiveras för alla samlingar i hello prenumeration.

Du kanske vill toodisable UPD: ar i någon av följande situationer hello: 

* Du måste slutföra åtkomst till och kontroll av användardata (för gransknings- och granska syfte, till exempel finansiella institutioner).
* Du har 3 parts användare profilen lösningar för lokal hantering och vill toocontinue som använder dem i Azure RemoteApp-domänansluten distributionen. Detta kräver hello profil agent toobe läses in i hello gold image. 
* Du behöver inte någon lokal datakvarhållning eller du har alla data i hello molnet eller filresurs och vill spara toocontrol data lokalt med hjälp av Azure RemoteApp.

Se [inaktivera användarprofil-diskar (UPD: ar) i Azure RemoteApp](https://blogs.technet.microsoft.com/enterprisemobility/2015/11/11/disable-user-profile-disks-upds-in-azure-remoteapp/) för mer information.

## <a name="can-i-restrict-users-from-saving-data-toohello-system-drive"></a>Kan jag hindra användare från att spara data toohello systemenhet?
Ja, men du behöver tooset som upp i hello mallen avbildning innan du skapar hello samling. Använd följande steg tooblock åtkomst toohello systemenhet hello:

1. Kör **gpedit.msc** på hello mallavbildningen.
2. Navigera för**Datorkonfiguration > Administrationsmallar > Windows-komponenter > Explorer**.
3. Välj hello följande alternativ:
   * **Dölj dessa enheter i den här datorn**
   * **Förhindra åtkomst toodrives från den här datorn**

## <a name="can-i-seed-upds-i-want-tooput-some-data-in-hello-upd-thats-available-hello-first-time-hello-user-signs-in"></a>Kan jag initiera UPD: ar? Jag vill tooput vissa data i hello UPD som är tillgängliga hello hello användare loggar in.
Ja, när du skapar hello mallavbildningen, du kan lägga till information toohello standardprofil. Informationen läggs sedan toohello UPD.

## <a name="can-i-change-hello-size-of-hello-upd-depending-on-how-much-data-i-want-toostore"></a>Kan jag ändra hello storleken på hello UPD beroende på hur mycket data som jag vill toostore?
Nej, alla UPD: ar har 50 GB lagringsutrymme. Om du vill toostore olika mängder data, försök hello följande:

1. Inaktivera UPD: ar för hello samling.
2. Skapa en filresurs för användare tooaccess.
3. Läs in hello filresurs med hjälp av ett startskript. Nedan visas information om startskript i Azure RemoteApp.
4. Dirigera användare toosave alla data toohello filresurs.

## <a name="how-do-i-run-a-startup-script-in-azure-remoteapp"></a>Hur kör ett startskript i Azure RemoteApp?
Om du vill toorun ett startskript starta genom att skapa en schemalagd aktivitet i hello mallavbildningen du kommer toouse för hello samling. (Gör *innan* du kör sysprep.) 

![Skapa en systemaktivitet](./media/remoteapp-upd/upd1.png)

![Skapa en systemaktivitet som körs när en användare loggar in](./media/remoteapp-upd/upd2.png)

På hello **allmänna** och vara säker på att toochange hello **användarkontot** under säkerhet för ”BUILTIN\ANVÄNDARE”.

![Ändra hello användargrupp konto tooa](./media/remoteapp-upd/upd4.png)

hello aktiviteten startar din startskript hello användarens autentiseringsuppgifter. Schemalägga hello uppgiften toorun var en gång en användare loggar in.

![Ange hello utlösare för hello aktivitet som ”vid inloggning”](./media/remoteapp-upd/upd3.png)

Du kan också använda [grupprincipbaserad startskript](https://technet.microsoft.com/library/cc779329%28v=ws.10%29.aspx). 

## <a name="what-about-placing-a-startup-script-in-hello-start-menu-would-that-work"></a>Vad händer om placera ett startskript i hello Start-menyn? Fungerar som?
Med andra ord kan jag skapa en .bat-fil som kör ett skript för config-fönstret och spara den toohello c:\ProgramData\Microsoft\Windows\Start meny\Program\Autostart mapp och har skriptet köras när en användare startar en RemoteApp-sessionen?

Nej, som inte stöds med Azure RemoteApp som använder RDSH som inte stöder heller startskript hello Start-menyn.

## <a name="can-i-use-mstscexe-hello-remote-desktop-program-tooconfigure-logon-scripts"></a>Kan jag använda mstsc.exe (hello fjärrskrivbord program) tooconfigure inloggningsskript?
Nix, stöds inte av Azure RemoteApp.

## <a name="can-i-store-data-on-hello-vm-locally"></a>Kan jag lagra data på hello VM lokalt?
Nej, lagrade data var som helst på hello VM än i hello UPD går förlorade. Det finns en hög chansen hello användare inte kommer att hämta hello samma VM hello nästa gång de loggar in på Azure RemoteApp. Vi underhåller inte användaren VM beständiga så hello användaren inte logga in på hello samma virtuella dator och hello data går förlorade. Dessutom när vi uppdaterar hello samling förloras hello befintliga virtuella datorer har ersatts med en ny uppsättning virtuella datorer – som innebär att data som lagrats på hello VM själva. hello rekommendation är toostore data i hello UPD, delad lagring som Azure-filer, en server i ett virtuellt nätverk eller på hello molnet med hjälp av ett moln lagringssystem som DropBox.

## <a name="how-do-i-mount-an-azure-file-share-on-a-vm-using-powershell"></a>Hur montera en Azure-filresurs på en virtuell dator med hjälp av PowerShell?
Du kan använda hello Net PSDrive cmdlet toomount hello-enheten på följande sätt:

    New-PSDrive -Name <drive-name> -PSProvider FileSystem -Root \\<storage-account-name>.file.core.windows.net\<share-name> -Credential :<storage-account-name>


Du kan också spara dina autentiseringsuppgifter genom att köra hello följande:

    cmdkey /add:<storage-account-name>.file.core.windows.net /user:<storage-account-name> /pass:<storage-account-key>


Ger möjlighet att hoppa över hello - parameter för autentiseringsuppgifter i hello ny PSDrive cmdlet.

