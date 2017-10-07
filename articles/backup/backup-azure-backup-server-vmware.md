---
title: aaaBack in VMware-servrar med Azure Backup Server | Microsoft Docs
description: "Använda Azure Backup Server tooback konfigurerar en VMware vCenter/ESXi servrar tooAzure eller disk. Den här artikeln innehåller steg för steg-instruktion om att säkerhetskopiera (eller skydda) = VMware-arbetsbelastningar."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
ms.assetid: 6b131caf-de85-4eba-b8e6-d8a04545cd9d
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/24/2017
ms.author: markgal;
ms.openlocfilehash: 3edb6880a526ed0b18605fee0fac27196a608e7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-vmware-server-tooazure"></a>Säkerhetskopiera en VMware server tooAzure

Den här artikeln förklarar hur tooconfigure Azure Backup Server toohelp skydda VMware-serverarbetsbelastningar. Den här artikeln förutsätter att du redan har Azure Backup-Server installerad. Om du inte har Azure Backup-Server installerad, se [förbereda tooback in arbetsbelastningar med Azure Backup Server](backup-azure-microsoft-azure-backup.md).

Azure Backup-Server kan säkerhetskopiera eller att skydda VMware vCenter serverversion 6.5, 6.0 och 5.5.


## <a name="create-a-secure-connection-toohello-vcenter-server"></a>Skapa en säker anslutning toohello vCenter-Server

Azure Backup-servern kommunicerar med varje vCenter-servern via en HTTPS-kanal som standard. tooturn på hello säker kommunikation, rekommenderar vi att du installerar hello VMware certifikatutfärdare (CA)-certifikat på Azure Backup Server. Om du inte kräver säker kommunikation och föredrar toodisable hello HTTPS krav, se [inaktivera säker kommunikationsprotokoll](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol). toocreate en säker anslutning mellan Azure Backup Server och hello vCenter Server importerar hello betrodda certifikat på Azure Backup Server.

Normalt använder du en webbläsare på hello Azure Backup Server datorn tooconnect toohello vCenter Server via hello vSphere webbklienten. hello första gången du använder hello Azure Backup Server browser tooconnect toohello vCenter Server hello anslutningen är inte säker. hello följande bild visar hello osäker anslutning.

![Exempel på osäker anslutning tooVMware server](./media/backup-azure-backup-server-vmware/unsecure-url.png)

toofix problemet, och skapa en säker anslutning, hämta hello betrodda rotcertifikatutfärdare.

1. Ange hello URL toohello vSphere webbklienten i hello webbläsare på Azure Backup Server. Hej vSphere webbklienten inloggningssidan visas.

    ![vSphere Webbklient](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    Leta upp hello längst hello hello information för administratörer och utvecklare, **Download betrodda rotcertifikatutfärdare** länk.

    ![Länken toodownload hello betrodda rotcertifikatutfärdare](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  Om du inte ser hello vSphere webbklienten inloggningssidan, kontrollera i webbläsarens proxyinställningar.

2. Klicka på **Download betrodda rotcertifikatutfärdarcertifikat**.

    Hej vCenter-servern hämtar filen tooyour lokal dator. hello filnamnet heter **hämta**. Beroende på din webbläsare, visas ett meddelande som frågar om tooopen eller spara hello-filen.

    ![Hämta meddelande när certifikat hämtas](./media/backup-azure-backup-server-vmware/download-certs.png)

3. Spara hello tooa plats på Azure Backup Server. När du sparar hello-fil lägger du till hello .zip-filnamnstillägg.

    hello-filen är en .zip-fil som innehåller hello information om hello certifikat. Du kan använda hello extrahering verktyg med hello .zip-tillägget.

4. Högerklicka på **download.zip**, och välj sedan **extrahera alla** tooextract hello innehållet.

    hello ZIP-filen extraherar innehållet tooa mappen med namnet **certifikat**. Två typer av filer som visas i hello certifikat mappen. Hej rotcertifikatfilen har ett tillägg som börjar med en numrerad sekvens som.0 och.1.
    
    hello CRL-filen har ett tillägg som börjar med en sekvens som .r0 eller .r1. hello CRL-filen är associerad med ett certifikat.

    ![Hämta filen extraheras lokalt ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. I hello **certifikat** mappen, högerklicka på rotcertifikatfilen hello och klicka sedan på **Byt namn på**.

    ![Byt namn på rotcertifikat ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    Ändra hello root certificate tillägget too.crt. När du tillfrågas om du inte vill toochange hello tillägget Klicka **Ja** eller **OK**. Annars kan ändra du hello filen avsedda funktion. hello-ikon för hello ändringar tooan ikon som representerar ett rotcertifikat.

6. Högerklicka på hello rotcertifikat och hello popup-menyn, Välj **installera certifikat**.

    Hej **guiden Importera certifikat** dialogrutan visas.

7. I hello **guiden Importera certifikat** dialogrutan **lokal dator** som hello mål hello certifikat och klicka sedan på **nästa** toocontinue.

    ![Certifikatet lagringsalternativ för mål ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    Om du tillfrågas om du vill tooallow ändringar toohello datorn klickar du på **Ja** eller **OK**, tooall hello ändringar.

8. På hello **certifikatarkiv** väljer **placera alla certifikat i följande store hello**, och klicka sedan på **Bläddra** toochoose hello certifikatarkiv.

    ![Placera certifikat i en specifik lagring punkt](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    Hej **Välj certifikatarkiv** dialogrutan visas.

    ![Mappen certifikathierarkin för lagring](./media/backup-azure-backup-server-vmware/cert-store.png)

9. Välj **betrodda rotcertifikatutfärdare** som hello målmappen hello certifikat och klicka sedan på **OK**.

    ![Målmappen för certifikat](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    Hej **betrodda rotcertifikatutfärdare** mappen bekräftas som hello certifikatarkiv. Klicka på **Nästa**.

    ![Certifikat-mappen](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. På hello **hello Slutför guiden Importera certifikat** sidan Kontrollera hello certifikatet finns i hello önskade mappen och klicka sedan på **Slutför**.

    ![Kontrollera att certifikatet är i rätt mapp för hello](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    En dialogruta visas bekräftas hello lyckade certifikat import.

11. Logga in toohello vCenter Server tooconfirm att anslutningen är säker.

  Om det går inte att genomföra hello certifikat import och du kan inte upprätta en säker anslutning, dokumentationen hello VMware vSphere på [Skaffa servercertifikat](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).

  Om du har säker gränser inom din organisation och inte vill tooturn på hello HTTPS-protokollet, Använd hello följa proceduren toodisable hello säker kommunikation.

### <a name="disable-secure-communication-protocol"></a>Inaktivera säker kommunikationsprotokollet

Om din organisation inte kräver hello HTTPS-protokoll Använd hello följande steg toodisable HTTPS. toodisable Hej standardbeteendet, skapa en registernyckel som ignorerar hello standardbeteendet.

1. Kopiera och klistra in hello följande text i en txt-fil.

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. Spara hello filen tooyour Azure Backup Server-datorn. Använd DisableSecureAuthentication.reg för hello filnamn.

3. Dubbelklicka på hello filen tooactivate hello registerpost.


## <a name="create-a-role-and-user-account-on-hello-vcenter-server"></a>Skapa en roll och användare på hello vCenter Server

En roll är en fördefinierad uppsättning behörigheter på hello vCenter Server. En vCenter Server-administratören skapar hello roller. tooassign behörigheter Hej administratör par användarkonton med en roll. tooestablish hello nödvändiga användarens autentiseringsuppgifter tooback hello vCenter Server-dator, skapa en roll med specifika privilegier och sedan associerar hello användarkonto med hello-rollen.

Azure Backup-servern använder ett användarnamn och lösenord tooauthenticate med hello vCenter Server. Azure Backup-servern använder autentiseringsuppgifterna som autentisering för alla säkerhetskopieringsåtgärder.

tooadd vCenter-serverrollen och dess behörighet för en administratör för säkerhetskopiering:

1. Logga in i toohello vCenter Server och sedan i hello vCenter Server **Navigator** klickar du på **Administration**.

    ![Administrationsalternativ vCenter Server Navigator Kontrollpanelen](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. I **Administration** Välj **roller**, och klicka sedan på hello **roller** klickar du på panelen hello lägga till rollikon (symbolen + hello).

    ![Lägg till roll](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    Hej **skapa roll** dialogrutan visas.

    ![Skapa en roll](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. I hello **skapa roll** i dialogrutan hello **rollnamn** ange *BackupAdminRole*. hello rollnamn kan vara vad du vill, men den bör vara att känna igen för hello rollen ändamål.

4. Välj hello behörigheter för hello version vCenter och klicka sedan på **OK**. hello följande tabell identifierar hello krävs behörighet för vCenter 6.0 och vCenter 5.5.

  När du väljer hello privilegier, klickar du på hello ikonen nästa toohello överordnade etikett tooexpand hello överordnade och visa hello underordnade privilegier. tooselect hello VirtualMachine privilegier, behöver du toogo flera nivåer i hello överordnad-underordnad hierarki. Du behöver inte tooselect alla underordnade privilegier i privilegium som överordnad.

  ![Privilegiet hierarkin överordnad-underordnad](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  När du klickar på **OK**, hello nya rollen visas i hello listan på hello roller panelen.

|Behörigheter för vCenter 6.0| Behörigheter för vCenter 5.5|
|--------------------------|---------------------------|
|Datastore.AllocateSpace   | Datastore.AllocateSpace|
|Global.ManageCustomFields | Global.ManageCustomerFields|
|Global.SetCustomFields    |   |
|Host.Local.CreateVM       | Network.Assign |
|Network.Assign            |  |
|Resource.AssignVMToPool   |  |
|VirtualMachine.Config.AddNewDisk  | VirtualMachine.Config.AddNewDisk   |
|VirtualMachine.Config.AdvanceConfig| VirtualMachine.Config.AdvancedConfig|
|VirtualMachine.Config.ChangeTracking| VirtualMachine.Config.ChangeTracking |
|VirtualMachine.Config.HostUSBDevice||
|VirtualMachine.Config.QueryUnownedFiles|    |
|VirtualMachine.Config.SwapPlacement| VirtualMachine.Config.SwapPlacement |
|VirtualMachine.Interact.PowerOff| VirtualMachine.Interact.PowerOff |
|VirtualMachine.Inventory.Create| VirtualMachine.Inventory.Create |
|VirtualMachine.Provisioning.DiskRandomAccess| |
|VirtualMachine.Provisioning.DiskRandomRead|VirtualMachine.Provisioning.DiskRandomRead |
|VirtualMachine.State.CreateSnapshot| VirtualMachine.State.CreateSnapshot|
|VirtualMachine.State.RemoveSnapshot|VirtualMachine.State.RemoveSnapshot |
</br>



## <a name="create-a-vcenter-server-user-account-and-permissions"></a>Skapa ett användarkonto för vCenter-servern och behörigheter

Skapa ett användarkonto när hello roll med behörigheter har konfigurerats. hello användarkonto har ett namn och lösenord som ger hello-autentiseringsuppgifter som används för autentisering.

1. toocreate ett användarkonto i hello vCenter Server **Navigator** klickar du på **användare och grupper**.

    ![Alternativet för användare och grupper](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    Hej **vCenter användare och grupper** panelen visas.

    ![vCenter-panelen för användare och grupper](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. I hello **vCenter användare och grupper** Kontrollpanelen, Välj hello **användare** fliken och klicka sedan på hello lägga till användare ikon (symbolen + hello).

    Hej **ny användare** dialogrutan visas.

3. I hello **ny användare** dialogrutan, Lägg till hello användarinformation och klicka sedan **OK**. I den här proceduren är hello användarnamn BackupAdmin.

    ![Dialogrutan Ny användare](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    hello nya användarkontot visas i hello.

4. tooassociate hello-användarkonto med hello roll i hello **Navigator** klickar du på **globala behörigheter**. I hello **globala behörigheter** Kontrollpanelen, Välj hello **hantera** fliken och klicka sedan på hello lägga till ikonen (symbolen + hello).

    ![Globala behörigheter panelen](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    Hej **globala behörigheter rot - Lägg till behörigheten** dialogrutan visas.

5. I hello **globala behörighet rot - Lägg till behörigheten** dialogrutan klickar du på **Lägg till** toochoose hello användare eller grupp.

    ![Välj användare eller grupp](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    Hej **Välj användare eller grupper** dialogrutan visas.

6. I hello **Välj användare eller grupper** dialogrutan Välj **BackupAdmin** och klicka sedan på **Lägg till**.

    I **användare**, hello *domän\användarnamn* formatet används för hello användarkonto. Om du vill toouse en annan domän, väljer du den från hello **domän** lista.

    ![Lägg till BackupAdmin användare](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    Klicka på **OK** tooadd hello valda användare toohello **lägga till behörigheten** dialogrutan.

7. Nu när du har identifierat hello användaren tilldela hello toohello användarroll. I **tilldelas rollen**, Välj hello nedrullningsbara listan **BackupAdminRole**, och klicka sedan på **OK**.

    ![Tilldela användare toorole](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  På hello **hantera** fliken i hello **globala behörigheter** panelen, hello nytt användarkonto och hello associerade roll visas i hello-listan.


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a>Upprätta vCenter serverautentiseringsuppgifter för Azure Backup Server

Innan du lägger till hello VMware server tooAzure Backup Server, installera [uppdatering 1 för Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).

1. tooopen Azure Backup Server, klicka på ikonen hello hello Azure Backup Server skrivbordet.

    ![Azure Backup Server-ikon](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    Om du inte hittar hello-ikon på hello skrivbordet, öppna Azure Backup Server hello listan över installerade appar. Programnamn i hello Azure Backup Server kallas för Microsoft Azure Backup.

2. Hello Azure Backup Server-konsolen, klicka på **Management**, klickar du på **produktionsservrar**, och klicka på verktygsfliken hello **hantera VMware**.

    ![Azure Backup Server-konsolen](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    Hej **hantera autentiseringsuppgifter** dialogrutan visas.

    ![Dialogrutan för Azure Backup Server hantera autentiseringsuppgifter](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. I hello **hantera autentiseringsuppgifter** dialogrutan klickar du på **Lägg till** tooopen hello **lägga till autentiseringsuppgifter** dialogrutan.

4. I hello **lägga till autentiseringsuppgifter** dialogrutan Ange ett namn och en beskrivning för hello nya autentiseringsuppgifter. Ange hello användarnamn och lösenord. hello namnet *Contoso Vcenter autentiseringsuppgifter* används tooidentify hello autentiseringsuppgifter i hello nästa procedur. Använd hello samma användarnamn och lösenord som används för hello vCenter-servern. Om hello vCenter Server och Azure Backup Server inte är i samma domän, hello i **användarnamn**, ange hello domän.

    ![Azure Backup Server lägga till autentiseringsuppgifter i dialogrutan](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    Klicka på **Lägg till** tooadd hello nya autentiseringsuppgifter tooAzure Backup Server. hello ny autentiseringsuppgift visas i listan för hello hello **hantera autentiseringsuppgifter** dialogrutan.
    
    ![Dialogrutan för Azure Backup Server hantera autentiseringsuppgifter](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. tooclose hello **hantera autentiseringsuppgifter** dialogrutan klickar du på hello **X** i hello övre högra hörnet.


## <a name="add-hello-vcenter-server-tooazure-backup-server"></a>Lägg till hello vCenter Server tooAzure Backup Server

Produktion kan servern tillägg använda tooadd hello vCenter Server tooAzure Backup Server.

tooopen produktion Server tillägg guiden fullständig hello nedan:

1. Hello Azure Backup Server-konsolen, klicka på **Management**, klickar du på **produktionsservrar**, och klicka sedan på **Lägg till**.

    ![Öppna produktion tillägg guiden](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    Hej **produktion tillägg guiden** dialogrutan visas.

    ![Guiden för produktion Server-tillägg](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. På hello **Välj produktionsservern typen** väljer **VMware-servrar**, och klicka sedan på **nästa**.

3. I **Server namn eller IP-adress**, ange hello fullständigt kvalificerade domännamnet (FQDN) eller IP-adressen för hello VMware-server. Om alla hello ESXi servrar hanteras av hello samma vCenter kan du använda hello vCenter namn.

    ![Ange VMware server FQDN eller IP-adress](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. I **SSL-Port**, ange hello-port som används toocommunicate med hello VMware-server. Använda port 443, som är standardporten för hello, såvida du inte vet att en annan port krävs.

5. I **ange autentiseringsuppgifter**, Välj hello autentiseringsuppgifter som du skapade tidigare.

    ![Ange autentiseringsuppgifter](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. Klicka på **Lägg till** tooadd hello VMware toohello serverlistan i **lägga till VMware-servrar**, och klicka sedan på **nästa** toomove toohello nästa sida i guiden hello.

    ![Lägg till VMWare-server och autentiseringsuppgifter](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. I hello **sammanfattning** klickar du på **Lägg till** tooadd hello angetts VMware server tooAzure Backup Server.

    ![Lägg till VMware server tooAzure Backup Server](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  hello VMware server backup är en agentlös säkerhetskopiering och hello ny server läggs omedelbart. Hej **Slutför** sidan visar du hello resultat.

  ![Sidan Slutför](./media/backup-azure-backup-server-vmware/summary-screen.png)

  tooadd flera instanser av vCenter Server tooAzure Backup Server, upprepa hello föregående steg i det här avsnittet.

När du har lagt till hello vCenter Server tooAzure Backup Server är hello nästa steg toocreate en skyddsgrupp. Hej skyddsgrupp anger hello detaljer för kortsiktig eller långsiktig kvarhållning och det är där du definierar och tillämpa hello säkerhetskopieringsprincip. hello säkerhetskopieringsprincip är hello schema för när säkerhetskopiering sker och vad som säkerhetskopieras.


## <a name="configure-a-protection-group"></a>Konfigurera en skyddsgrupp

Om du inte har använt System Center Data Protection Manager eller Azure Backup Server innan du läser [planera för säkerhetskopiering till disk](https://technet.microsoft.com/library/hh758026.aspx) tooprepare maskinvarumiljön. När du har kontrollerat att du har rätt lagring kan du använda hello Skapa ny Skyddsgrupp guiden tooadd virtuella VMware-datorer.

1. Hello Azure Backup Server-konsolen, klicka på **skydd**, och klicka på verktygsfliken hello **ny** tooopen hello Skapa ny Skyddsgrupp guiden.

    ![Öppna hello Skapa ny Skyddsgrupp guiden](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    Hej **Skapa ny Skyddsgrupp** dialogruta visas.

    ![Dialogruta för guiden Skapa ny Skyddsgrupp](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    Klicka på **nästa** tooadvance toohello **Välj typ av skyddsgrupp** sidan.

2. På hello **typ av Välj Skyddsgrupp** väljer **servrar** och klicka sedan på **nästa**. Hej **Välj gruppmedlemmar** visas.

3. På hello **Välj gruppmedlemmar** sida, hello tillgängliga medlemmar och hello valda medlemmar visas. Välj hello medlemmar som du vill tooprotect och klicka sedan på **nästa**.

    ![Välj gruppmedlemmar](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    När du väljer en medlem, om du markerar en mapp som innehåller andra mappar eller virtuella datorer, väljs även dessa mappar och virtuella datorer. hello inkludering av hello mappar och virtuella datorer i hello överordnade mappen kallas mappnivå skydd. tooremove en mapp eller VM, rensa hello kryssrutan.

    Om en virtuell dator eller en mapp som innehåller en virtuell dator är redan skyddade tooAzure, kan du välja den virtuella datorn igen. Det vill säga när en virtuell dator är skyddade tooAzure, kan du inte skydda igen, som förhindrar att duplicerade återställningspunkter skapas för en virtuell dator. Om du vill toosee skyddar vilka Azure Backup Server-instansen redan en medlem, punkt toohello toosee hello medlemsnamn av hello skyddar server.

4. På hello **Välj Dataskyddsmetod** anger du ett namn för hello skyddsgruppen. Kortsiktigt skydd (toodisk) och online-skydd är markerade. Om du vill toouse online-skydd (tooAzure), måste du använda toodisk för kortsiktigt skydd. Klicka på **nästa** tooproceed toohello kortsiktigt skydd intervall.

    ![Välj dataskyddsmetod](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. På hello **ange kortvariga mål** sidan för **Kvarhållningsintervall**, ange hello antalet dagar som du vill tooretain återställningspunkter som är *lagras toodisk*. Om du vill att toochange hello tiden och dagarna när återställningspunkter tas **ändra**. hello kortsiktig återställningspunkter är fullständiga säkerhetskopieringar. De är inte inkrementella säkerhetskopieringar. När du är nöjd med hello kortsiktiga mål klickar du på **nästa**.

    ![Ange kortsiktiga mål](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. På hello **granska diskallokering** granskar och om det behövs ändrar hello diskutrymme för hello virtuella datorer. hello rekommenderad diskallokeringar baseras på hello Kvarhållningsintervall som anges i hello **ange kortvariga mål** sidan hello typ av arbetsbelastning och hello storleken på hello skyddade data (som identifieras i steg 3).  

  - **Datastorlek:** storleken på hello data i hello skyddsgruppen.
  - **Diskutrymme:** hello rekommenderade mängden diskutrymme för hello skyddsgruppen. Om du vill toomodify den här inställningen allokerar du ett totalt utrymme som är något större än hello belopp som du uppskattar att varje datakälla växer.
  - **Samordna data:** om du aktiverar samordning kan flera datakällor i hello skydd kan mappa tooa enda replik- och återställningspunktvolym. Samordning stöds inte för alla arbetsbelastningar.
  - **Utöka automatiskt:** om du aktiverar den här inställningen om data i gruppen för skyddade hello blir för stor hello första fördelning, System Center Data Protection Manager försöker tooincrease hello diskstorleken med 25 procent.
  - **Lagringspoolinformation:** visar hello status hello lagringspoolen, inklusive total och återstående diskstorlek.

    ![Granska diskallokering](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    När du är nöjd med hello utrymmestilldelning klickar du på **nästa**.

7. På hello **Välj Replikskapandemetod** anger du hur toogenerate hello inledande kopia eller replik av hello skyddade data på Azure Backup Server.

    hello standardvärdet är **automatiskt över hello nätverk** och **nu**. Om du använder hello standard, rekommenderar vi att du anger en tid. Välj **senare** och ange en dag och tid.

    Överväg att replikera hello data offline med hjälp av flyttbart medium för stora mängder data eller mindre än optimala nätverksförhållanden.

    När du har gjort dina val klickar du på **nästa**.

    ![Välj metod för skapande av replik](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. På hello **alternativ för konsekvenskontroll** anger hur och när tooautomate hello konsekvenskontroller. Du kan köra en konsekvenskontroll när replikdata blir inkonsekvent, eller på ett schema.

    Om du inte vill tooconfigure automatiska konsekvenskontroller, kan du köra en manuell kontroll. Under hello skydd i hello Azure Backup Server-konsolen, högerklicka på hello skyddsgrupp och välj sedan **utför konsekvenskontroll**.

    Klicka på **nästa** toomove toohello nästa sida.

9. På hello **ange Onlineskyddsdata** markerar du en eller flera datakällor som du vill tooprotect. Du kan välja hello medlemmar individuellt eller klicka på **Markera alla** toochoose alla medlemmar. När du väljer hello medlemmar, klickar du på **nästa**.

    ![Ange onlineskyddsdata](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. På hello **Ange schema för onlinesäkerhetskopiering** anger hello schema toogenerate återställningspunkterna från hello disksäkerhetskopiering. När hello återställningspunkt skapas, är det överförda toohello Recovery Services-valvet i Azure. När du är nöjd med hello schema för onlinesäkerhetskopiering klickar du på **nästa**.

    ![Ange onlinesäkerhetskopieringsschema](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. På hello **ange bevarandeprincip** kan ange hur länge du vill tooretain hello säkerhetskopierade data i Azure. När hello princip har definierats, klickar du på **nästa**.

    ![Ange bevarandeprincip](./media/backup-azure-backup-server-vmware/retention-policy.png)

    Det finns ingen tidsbegränsning för hur länge du kan behålla data i Azure. När du lagrar återställningsdata punkt i Azure hello bara gränsen är att du inte kan ha fler än 9999 återställningspunkter per skyddad instans. I det här exemplet är hello skyddade instansen hello VMware-server.

12. På hello **sammanfattning** , granska hello information för dina skyddsgruppmedlemmar och inställningar, och klickar sedan på **Skapa grupp**.

    ![Skyddsgruppsmedlem och inställningen sammanfattning](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a>Nästa steg
Om du använder Azure Backup Server tooprotect VMware arbetsbelastningar du kanske vill utnyttja Azure Backup Server toohelp skydda en [Microsoft Exchange server](./backup-azure-exchange-mabs.md), [Microsoft SharePoint-servergruppen](./backup-azure-backup-sharepoint-mabs.md), eller en [SQL Server-databas](./backup-azure-sql-mabs.md).

Mer information om problem med registrering hello agent konfigurera hello skyddsgrupp eller för att säkerhetskopiera jobb finns [felsöka Azure Backup Server](./backup-azure-mabs-troubleshoot.md).
