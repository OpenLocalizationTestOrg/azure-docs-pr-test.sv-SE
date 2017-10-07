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
# <a name="back-up-a-vmware-server-tooazure"></a><span data-ttu-id="e424b-104">Säkerhetskopiera en VMware server tooAzure</span><span class="sxs-lookup"><span data-stu-id="e424b-104">Back up a VMware server tooAzure</span></span>

<span data-ttu-id="e424b-105">Den här artikeln förklarar hur tooconfigure Azure Backup Server toohelp skydda VMware-serverarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="e424b-105">This article explains how tooconfigure Azure Backup Server toohelp protect VMware server workloads.</span></span> <span data-ttu-id="e424b-106">Den här artikeln förutsätter att du redan har Azure Backup-Server installerad.</span><span class="sxs-lookup"><span data-stu-id="e424b-106">This article assumes you already have Azure Backup Server installed.</span></span> <span data-ttu-id="e424b-107">Om du inte har Azure Backup-Server installerad, se [förbereda tooback in arbetsbelastningar med Azure Backup Server](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="e424b-107">If you don't have Azure Backup Server installed, see [Prepare tooback up workloads using Azure Backup Server](backup-azure-microsoft-azure-backup.md).</span></span>

<span data-ttu-id="e424b-108">Azure Backup-Server kan säkerhetskopiera eller att skydda VMware vCenter serverversion 6.5, 6.0 och 5.5.</span><span class="sxs-lookup"><span data-stu-id="e424b-108">Azure Backup Server can back up, or help protect, VMware vCenter Server version 6.5, 6.0 and 5.5.</span></span>


## <a name="create-a-secure-connection-toohello-vcenter-server"></a><span data-ttu-id="e424b-109">Skapa en säker anslutning toohello vCenter-Server</span><span class="sxs-lookup"><span data-stu-id="e424b-109">Create a secure connection toohello vCenter Server</span></span>

<span data-ttu-id="e424b-110">Azure Backup-servern kommunicerar med varje vCenter-servern via en HTTPS-kanal som standard.</span><span class="sxs-lookup"><span data-stu-id="e424b-110">By default, Azure Backup Server communicates with each vCenter Server via an HTTPS channel.</span></span> <span data-ttu-id="e424b-111">tooturn på hello säker kommunikation, rekommenderar vi att du installerar hello VMware certifikatutfärdare (CA)-certifikat på Azure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="e424b-111">tooturn on hello secure communication, we recommend that you install hello VMware Certificate Authority (CA) certificate on Azure Backup Server.</span></span> <span data-ttu-id="e424b-112">Om du inte kräver säker kommunikation och föredrar toodisable hello HTTPS krav, se [inaktivera säker kommunikationsprotokoll](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol).</span><span class="sxs-lookup"><span data-stu-id="e424b-112">If you don't require secure communication, and would prefer toodisable hello HTTPS requirement, see [Disable secure communication protocol](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol).</span></span> <span data-ttu-id="e424b-113">toocreate en säker anslutning mellan Azure Backup Server och hello vCenter Server importerar hello betrodda certifikat på Azure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="e424b-113">toocreate a secure connection between Azure Backup Server and hello vCenter Server, import hello trusted certificate on Azure Backup Server.</span></span>

<span data-ttu-id="e424b-114">Normalt använder du en webbläsare på hello Azure Backup Server datorn tooconnect toohello vCenter Server via hello vSphere webbklienten.</span><span class="sxs-lookup"><span data-stu-id="e424b-114">Typically, you use a browser on hello Azure Backup Server machine tooconnect toohello vCenter Server via hello vSphere Web Client.</span></span> <span data-ttu-id="e424b-115">hello första gången du använder hello Azure Backup Server browser tooconnect toohello vCenter Server hello anslutningen är inte säker.</span><span class="sxs-lookup"><span data-stu-id="e424b-115">hello first time you use hello Azure Backup Server browser tooconnect toohello vCenter Server, hello connection isn't secure.</span></span> <span data-ttu-id="e424b-116">hello följande bild visar hello osäker anslutning.</span><span class="sxs-lookup"><span data-stu-id="e424b-116">hello following image shows hello unsecured connection.</span></span>

![Exempel på osäker anslutning tooVMware server](./media/backup-azure-backup-server-vmware/unsecure-url.png)

<span data-ttu-id="e424b-118">toofix problemet, och skapa en säker anslutning, hämta hello betrodda rotcertifikatutfärdare.</span><span class="sxs-lookup"><span data-stu-id="e424b-118">toofix this issue, and create a secure connection, download hello trusted root CA certificates.</span></span>

1. <span data-ttu-id="e424b-119">Ange hello URL toohello vSphere webbklienten i hello webbläsare på Azure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="e424b-119">In hello browser on Azure Backup Server, enter hello URL toohello vSphere Web Client.</span></span> <span data-ttu-id="e424b-120">Hej vSphere webbklienten inloggningssidan visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-120">hello vSphere Web Client login page appears.</span></span>

    ![vSphere Webbklient](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    <span data-ttu-id="e424b-122">Leta upp hello längst hello hello information för administratörer och utvecklare, **Download betrodda rotcertifikatutfärdare** länk.</span><span class="sxs-lookup"><span data-stu-id="e424b-122">At hello bottom of hello information for administrators and developers, locate hello **Download trusted root CA certificates** link.</span></span>

    ![Länken toodownload hello betrodda rotcertifikatutfärdare](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  <span data-ttu-id="e424b-124">Om du inte ser hello vSphere webbklienten inloggningssidan, kontrollera i webbläsarens proxyinställningar.</span><span class="sxs-lookup"><span data-stu-id="e424b-124">If you don't see hello vSphere Web Client login page, check your browser's proxy settings.</span></span>

2. <span data-ttu-id="e424b-125">Klicka på **Download betrodda rotcertifikatutfärdarcertifikat**.</span><span class="sxs-lookup"><span data-stu-id="e424b-125">Click **Download trusted root CA certificates**.</span></span>

    <span data-ttu-id="e424b-126">Hej vCenter-servern hämtar filen tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="e424b-126">hello vCenter Server downloads a file tooyour local computer.</span></span> <span data-ttu-id="e424b-127">hello filnamnet heter **hämta**.</span><span class="sxs-lookup"><span data-stu-id="e424b-127">hello file's name is named **download**.</span></span> <span data-ttu-id="e424b-128">Beroende på din webbläsare, visas ett meddelande som frågar om tooopen eller spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="e424b-128">Depending on your browser, you receive a message that asks whether tooopen or save hello file.</span></span>

    ![Hämta meddelande när certifikat hämtas](./media/backup-azure-backup-server-vmware/download-certs.png)

3. <span data-ttu-id="e424b-130">Spara hello tooa plats på Azure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="e424b-130">Save hello file tooa location on Azure Backup Server.</span></span> <span data-ttu-id="e424b-131">När du sparar hello-fil lägger du till hello .zip-filnamnstillägg.</span><span class="sxs-lookup"><span data-stu-id="e424b-131">When you save hello file, add hello .zip file name extension.</span></span>

    <span data-ttu-id="e424b-132">hello-filen är en .zip-fil som innehåller hello information om hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="e424b-132">hello file is a .zip file that contains hello information about hello certificates.</span></span> <span data-ttu-id="e424b-133">Du kan använda hello extrahering verktyg med hello .zip-tillägget.</span><span class="sxs-lookup"><span data-stu-id="e424b-133">With hello .zip extension, you can use hello extraction tools.</span></span>

4. <span data-ttu-id="e424b-134">Högerklicka på **download.zip**, och välj sedan **extrahera alla** tooextract hello innehållet.</span><span class="sxs-lookup"><span data-stu-id="e424b-134">Right-click **download.zip**, and then select **Extract All** tooextract hello contents.</span></span>

    <span data-ttu-id="e424b-135">hello ZIP-filen extraherar innehållet tooa mappen med namnet **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="e424b-135">hello .zip file extracts its contents tooa folder named **certs**.</span></span> <span data-ttu-id="e424b-136">Två typer av filer som visas i hello certifikat mappen.</span><span class="sxs-lookup"><span data-stu-id="e424b-136">Two types of files appear in hello certs folder.</span></span> <span data-ttu-id="e424b-137">Hej rotcertifikatfilen har ett tillägg som börjar med en numrerad sekvens som.0 och.1.</span><span class="sxs-lookup"><span data-stu-id="e424b-137">hello root certificate file has an extension that begins with a numbered sequence like .0 and .1.</span></span>
    
    <span data-ttu-id="e424b-138">hello CRL-filen har ett tillägg som börjar med en sekvens som .r0 eller .r1.</span><span class="sxs-lookup"><span data-stu-id="e424b-138">hello CRL file has an extension that begins with a sequence like .r0 or .r1.</span></span> <span data-ttu-id="e424b-139">hello CRL-filen är associerad med ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="e424b-139">hello CRL file is associated with a certificate.</span></span>

    ![<span data-ttu-id="e424b-140">Hämta filen extraheras lokalt</span><span class="sxs-lookup"><span data-stu-id="e424b-140">Download file extracted locally</span></span> ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. <span data-ttu-id="e424b-141">I hello **certifikat** mappen, högerklicka på rotcertifikatfilen hello och klicka sedan på **Byt namn på**.</span><span class="sxs-lookup"><span data-stu-id="e424b-141">In hello **certs** folder, right-click hello root certificate file, and then click **Rename**.</span></span>

    ![<span data-ttu-id="e424b-142">Byt namn på rotcertifikat</span><span class="sxs-lookup"><span data-stu-id="e424b-142">Rename root certificate</span></span> ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    <span data-ttu-id="e424b-143">Ändra hello root certificate tillägget too.crt.</span><span class="sxs-lookup"><span data-stu-id="e424b-143">Change hello root certificate's extension too.crt.</span></span> <span data-ttu-id="e424b-144">När du tillfrågas om du inte vill toochange hello tillägget Klicka **Ja** eller **OK**.</span><span class="sxs-lookup"><span data-stu-id="e424b-144">When you're asked if you're sure you want toochange hello extension, click **Yes** or **OK**.</span></span> <span data-ttu-id="e424b-145">Annars kan ändra du hello filen avsedda funktion.</span><span class="sxs-lookup"><span data-stu-id="e424b-145">Otherwise, you change hello file's intended function.</span></span> <span data-ttu-id="e424b-146">hello-ikon för hello ändringar tooan ikon som representerar ett rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="e424b-146">hello icon for hello file changes tooan icon that represents a root certificate.</span></span>

6. <span data-ttu-id="e424b-147">Högerklicka på hello rotcertifikat och hello popup-menyn, Välj **installera certifikat**.</span><span class="sxs-lookup"><span data-stu-id="e424b-147">Right-click hello root certificate and from hello pop-up menu, select **Install Certificate**.</span></span>

    <span data-ttu-id="e424b-148">Hej **guiden Importera certifikat** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-148">hello **Certificate Import Wizard** dialog box appears.</span></span>

7. <span data-ttu-id="e424b-149">I hello **guiden Importera certifikat** dialogrutan **lokal dator** som hello mål hello certifikat och klicka sedan på **nästa** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="e424b-149">In hello **Certificate Import Wizard** dialog box, select **Local Machine** as hello destination for hello certificate, and then click **Next** toocontinue.</span></span>

    ![<span data-ttu-id="e424b-150">Certifikatet lagringsalternativ för mål</span><span class="sxs-lookup"><span data-stu-id="e424b-150">Certificate storage destination options</span></span> ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    <span data-ttu-id="e424b-151">Om du tillfrågas om du vill tooallow ändringar toohello datorn klickar du på **Ja** eller **OK**, tooall hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="e424b-151">If you're asked if you want tooallow changes toohello computer, click **Yes** or **OK**, tooall hello changes.</span></span>

8. <span data-ttu-id="e424b-152">På hello **certifikatarkiv** väljer **placera alla certifikat i följande store hello**, och klicka sedan på **Bläddra** toochoose hello certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="e424b-152">On hello **Certificate Store** page, select **Place all certificates in hello following store**, and then click **Browse** toochoose hello certificate store.</span></span>

    ![Placera certifikat i en specifik lagring punkt](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    <span data-ttu-id="e424b-154">Hej **Välj certifikatarkiv** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-154">hello **Select Certificate Store** dialog box appears.</span></span>

    ![Mappen certifikathierarkin för lagring](./media/backup-azure-backup-server-vmware/cert-store.png)

9. <span data-ttu-id="e424b-156">Välj **betrodda rotcertifikatutfärdare** som hello målmappen hello certifikat och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e424b-156">Select **Trusted Root Certification Authorities** as hello destination folder for hello certificates, and then click **OK**.</span></span>

    ![Målmappen för certifikat](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    <span data-ttu-id="e424b-158">Hej **betrodda rotcertifikatutfärdare** mappen bekräftas som hello certifikatarkiv.</span><span class="sxs-lookup"><span data-stu-id="e424b-158">hello **Trusted Root Certification Authorities** folder is confirmed as hello certificate store.</span></span> <span data-ttu-id="e424b-159">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="e424b-159">Click **Next**.</span></span>

    ![Certifikat-mappen](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. <span data-ttu-id="e424b-161">På hello **hello Slutför guiden Importera certifikat** sidan Kontrollera hello certifikatet finns i hello önskade mappen och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="e424b-161">On hello **Completing hello Certificate Import Wizard** page, verify that hello certificate is in hello desired folder, and then click **Finish**.</span></span>

    ![Kontrollera att certifikatet är i rätt mapp för hello](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    <span data-ttu-id="e424b-163">En dialogruta visas bekräftas hello lyckade certifikat import.</span><span class="sxs-lookup"><span data-stu-id="e424b-163">A dialog box appears, hello successful certificate import is confirmed.</span></span>

11. <span data-ttu-id="e424b-164">Logga in toohello vCenter Server tooconfirm att anslutningen är säker.</span><span class="sxs-lookup"><span data-stu-id="e424b-164">Sign in toohello vCenter Server tooconfirm that your connection is secure.</span></span>

  <span data-ttu-id="e424b-165">Om det går inte att genomföra hello certifikat import och du kan inte upprätta en säker anslutning, dokumentationen hello VMware vSphere på [Skaffa servercertifikat](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).</span><span class="sxs-lookup"><span data-stu-id="e424b-165">If hello certificate import is not successful, and you cannot establish a secure connection, consult hello VMware vSphere documentation on [obtaining server certificates](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).</span></span>

  <span data-ttu-id="e424b-166">Om du har säker gränser inom din organisation och inte vill tooturn på hello HTTPS-protokollet, Använd hello följa proceduren toodisable hello säker kommunikation.</span><span class="sxs-lookup"><span data-stu-id="e424b-166">If you have secure boundaries within your organization, and don't want tooturn on hello HTTPS protocol, use hello following procedure toodisable hello secure communications.</span></span>

### <a name="disable-secure-communication-protocol"></a><span data-ttu-id="e424b-167">Inaktivera säker kommunikationsprotokollet</span><span class="sxs-lookup"><span data-stu-id="e424b-167">Disable secure communication protocol</span></span>

<span data-ttu-id="e424b-168">Om din organisation inte kräver hello HTTPS-protokoll Använd hello följande steg toodisable HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e424b-168">If your organization doesn't require hello HTTPS protocol, use hello following steps toodisable HTTPS.</span></span> <span data-ttu-id="e424b-169">toodisable Hej standardbeteendet, skapa en registernyckel som ignorerar hello standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="e424b-169">toodisable hello default behavior, create a registry key that ignores hello default behavior.</span></span>

1. <span data-ttu-id="e424b-170">Kopiera och klistra in hello följande text i en txt-fil.</span><span class="sxs-lookup"><span data-stu-id="e424b-170">Copy and paste hello following text into a .txt file.</span></span>

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. <span data-ttu-id="e424b-171">Spara hello filen tooyour Azure Backup Server-datorn.</span><span class="sxs-lookup"><span data-stu-id="e424b-171">Save hello file tooyour Azure Backup Server computer.</span></span> <span data-ttu-id="e424b-172">Använd DisableSecureAuthentication.reg för hello filnamn.</span><span class="sxs-lookup"><span data-stu-id="e424b-172">For hello file name, use DisableSecureAuthentication.reg.</span></span>

3. <span data-ttu-id="e424b-173">Dubbelklicka på hello filen tooactivate hello registerpost.</span><span class="sxs-lookup"><span data-stu-id="e424b-173">Double-click hello file tooactivate hello registry entry.</span></span>


## <a name="create-a-role-and-user-account-on-hello-vcenter-server"></a><span data-ttu-id="e424b-174">Skapa en roll och användare på hello vCenter Server</span><span class="sxs-lookup"><span data-stu-id="e424b-174">Create a role and user account on hello vCenter Server</span></span>

<span data-ttu-id="e424b-175">En roll är en fördefinierad uppsättning behörigheter på hello vCenter Server.</span><span class="sxs-lookup"><span data-stu-id="e424b-175">On hello vCenter Server, a role is a predefined set of privileges.</span></span> <span data-ttu-id="e424b-176">En vCenter Server-administratören skapar hello roller.</span><span class="sxs-lookup"><span data-stu-id="e424b-176">A vCenter Server administrator creates hello roles.</span></span> <span data-ttu-id="e424b-177">tooassign behörigheter Hej administratör par användarkonton med en roll.</span><span class="sxs-lookup"><span data-stu-id="e424b-177">tooassign permissions, hello administrator pairs user accounts with a role.</span></span> <span data-ttu-id="e424b-178">tooestablish hello nödvändiga användarens autentiseringsuppgifter tooback hello vCenter Server-dator, skapa en roll med specifika privilegier och sedan associerar hello användarkonto med hello-rollen.</span><span class="sxs-lookup"><span data-stu-id="e424b-178">tooestablish hello necessary user credentials tooback up hello vCenter Server computer, create a role with specific privileges, and then associate hello user account with hello role.</span></span>

<span data-ttu-id="e424b-179">Azure Backup-servern använder ett användarnamn och lösenord tooauthenticate med hello vCenter Server.</span><span class="sxs-lookup"><span data-stu-id="e424b-179">Azure Backup Server uses a username and password tooauthenticate with hello vCenter Server.</span></span> <span data-ttu-id="e424b-180">Azure Backup-servern använder autentiseringsuppgifterna som autentisering för alla säkerhetskopieringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="e424b-180">Azure Backup Server uses these credentials as authentication for all backup operations.</span></span>

<span data-ttu-id="e424b-181">tooadd vCenter-serverrollen och dess behörighet för en administratör för säkerhetskopiering:</span><span class="sxs-lookup"><span data-stu-id="e424b-181">tooadd a vCenter Server role and its privileges for a backup administrator:</span></span>

1. <span data-ttu-id="e424b-182">Logga in i toohello vCenter Server och sedan i hello vCenter Server **Navigator** klickar du på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="e424b-182">Sign in toohello vCenter Server, and then in hello vCenter Server **Navigator** panel, click **Administration**.</span></span>

    ![Administrationsalternativ vCenter Server Navigator Kontrollpanelen](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. <span data-ttu-id="e424b-184">I **Administration** Välj **roller**, och klicka sedan på hello **roller** klickar du på panelen hello lägga till rollikon (symbolen + hello).</span><span class="sxs-lookup"><span data-stu-id="e424b-184">In **Administration** select **Roles**, and then in hello **Roles** panel click hello add role icon (hello + symbol).</span></span>

    ![Lägg till roll](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    <span data-ttu-id="e424b-186">Hej **skapa roll** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-186">hello **Create Role** dialog box appears.</span></span>

    ![Skapa en roll](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. <span data-ttu-id="e424b-188">I hello **skapa roll** i dialogrutan hello **rollnamn** ange *BackupAdminRole*.</span><span class="sxs-lookup"><span data-stu-id="e424b-188">In hello **Create Role** dialog box, in hello **Role name** box, enter *BackupAdminRole*.</span></span> <span data-ttu-id="e424b-189">hello rollnamn kan vara vad du vill, men den bör vara att känna igen för hello rollen ändamål.</span><span class="sxs-lookup"><span data-stu-id="e424b-189">hello role name can be whatever you like, but it should be recognizable for hello role's purpose.</span></span>

4. <span data-ttu-id="e424b-190">Välj hello behörigheter för hello version vCenter och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e424b-190">Select hello privileges for hello appropriate version of vCenter, and then click **OK**.</span></span> <span data-ttu-id="e424b-191">hello följande tabell identifierar hello krävs behörighet för vCenter 6.0 och vCenter 5.5.</span><span class="sxs-lookup"><span data-stu-id="e424b-191">hello following table identifies hello required privileges for vCenter 6.0 and vCenter 5.5.</span></span>

  <span data-ttu-id="e424b-192">När du väljer hello privilegier, klickar du på hello ikonen nästa toohello överordnade etikett tooexpand hello överordnade och visa hello underordnade privilegier.</span><span class="sxs-lookup"><span data-stu-id="e424b-192">When you select hello privileges, click hello icon next toohello parent label tooexpand hello parent and view hello child privileges.</span></span> <span data-ttu-id="e424b-193">tooselect hello VirtualMachine privilegier, behöver du toogo flera nivåer i hello överordnad-underordnad hierarki.</span><span class="sxs-lookup"><span data-stu-id="e424b-193">tooselect hello VirtualMachine privileges, you need toogo several levels into hello parent child hierarchy.</span></span> <span data-ttu-id="e424b-194">Du behöver inte tooselect alla underordnade privilegier i privilegium som överordnad.</span><span class="sxs-lookup"><span data-stu-id="e424b-194">You don't need tooselect all child privileges within a parent privilege.</span></span>

  ![Privilegiet hierarkin överordnad-underordnad](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  <span data-ttu-id="e424b-196">När du klickar på **OK**, hello nya rollen visas i hello listan på hello roller panelen.</span><span class="sxs-lookup"><span data-stu-id="e424b-196">After you click **OK**, hello new role appears in hello list on hello Roles panel.</span></span>

|<span data-ttu-id="e424b-197">Behörigheter för vCenter 6.0</span><span class="sxs-lookup"><span data-stu-id="e424b-197">Privileges for vCenter 6.0</span></span>| <span data-ttu-id="e424b-198">Behörigheter för vCenter 5.5</span><span class="sxs-lookup"><span data-stu-id="e424b-198">Privileges for vCenter 5.5</span></span>|
|--------------------------|---------------------------|
|<span data-ttu-id="e424b-199">Datastore.AllocateSpace</span><span class="sxs-lookup"><span data-stu-id="e424b-199">Datastore.AllocateSpace</span></span>   | <span data-ttu-id="e424b-200">Datastore.AllocateSpace</span><span class="sxs-lookup"><span data-stu-id="e424b-200">Datastore.AllocateSpace</span></span>|
|<span data-ttu-id="e424b-201">Global.ManageCustomFields</span><span class="sxs-lookup"><span data-stu-id="e424b-201">Global.ManageCustomFields</span></span> | <span data-ttu-id="e424b-202">Global.ManageCustomerFields</span><span class="sxs-lookup"><span data-stu-id="e424b-202">Global.ManageCustomerFields</span></span>|
|<span data-ttu-id="e424b-203">Global.SetCustomFields</span><span class="sxs-lookup"><span data-stu-id="e424b-203">Global.SetCustomFields</span></span>    |   |
|<span data-ttu-id="e424b-204">Host.Local.CreateVM</span><span class="sxs-lookup"><span data-stu-id="e424b-204">Host.Local.CreateVM</span></span>       | <span data-ttu-id="e424b-205">Network.Assign</span><span class="sxs-lookup"><span data-stu-id="e424b-205">Network.Assign</span></span> |
|<span data-ttu-id="e424b-206">Network.Assign</span><span class="sxs-lookup"><span data-stu-id="e424b-206">Network.Assign</span></span>            |  |
|<span data-ttu-id="e424b-207">Resource.AssignVMToPool</span><span class="sxs-lookup"><span data-stu-id="e424b-207">Resource.AssignVMToPool</span></span>   |  |
|<span data-ttu-id="e424b-208">VirtualMachine.Config.AddNewDisk</span><span class="sxs-lookup"><span data-stu-id="e424b-208">VirtualMachine.Config.AddNewDisk</span></span>  | <span data-ttu-id="e424b-209">VirtualMachine.Config.AddNewDisk</span><span class="sxs-lookup"><span data-stu-id="e424b-209">VirtualMachine.Config.AddNewDisk</span></span>   |
|<span data-ttu-id="e424b-210">VirtualMachine.Config.AdvanceConfig</span><span class="sxs-lookup"><span data-stu-id="e424b-210">VirtualMachine.Config.AdvanceConfig</span></span>| <span data-ttu-id="e424b-211">VirtualMachine.Config.AdvancedConfig</span><span class="sxs-lookup"><span data-stu-id="e424b-211">VirtualMachine.Config.AdvancedConfig</span></span>|
|<span data-ttu-id="e424b-212">VirtualMachine.Config.ChangeTracking</span><span class="sxs-lookup"><span data-stu-id="e424b-212">VirtualMachine.Config.ChangeTracking</span></span>| <span data-ttu-id="e424b-213">VirtualMachine.Config.ChangeTracking</span><span class="sxs-lookup"><span data-stu-id="e424b-213">VirtualMachine.Config.ChangeTracking</span></span> |
|<span data-ttu-id="e424b-214">VirtualMachine.Config.HostUSBDevice</span><span class="sxs-lookup"><span data-stu-id="e424b-214">VirtualMachine.Config.HostUSBDevice</span></span>||
|<span data-ttu-id="e424b-215">VirtualMachine.Config.QueryUnownedFiles</span><span class="sxs-lookup"><span data-stu-id="e424b-215">VirtualMachine.Config.QueryUnownedFiles</span></span>|    |
|<span data-ttu-id="e424b-216">VirtualMachine.Config.SwapPlacement</span><span class="sxs-lookup"><span data-stu-id="e424b-216">VirtualMachine.Config.SwapPlacement</span></span>| <span data-ttu-id="e424b-217">VirtualMachine.Config.SwapPlacement</span><span class="sxs-lookup"><span data-stu-id="e424b-217">VirtualMachine.Config.SwapPlacement</span></span> |
|<span data-ttu-id="e424b-218">VirtualMachine.Interact.PowerOff</span><span class="sxs-lookup"><span data-stu-id="e424b-218">VirtualMachine.Interact.PowerOff</span></span>| <span data-ttu-id="e424b-219">VirtualMachine.Interact.PowerOff</span><span class="sxs-lookup"><span data-stu-id="e424b-219">VirtualMachine.Interact.PowerOff</span></span> |
|<span data-ttu-id="e424b-220">VirtualMachine.Inventory.Create</span><span class="sxs-lookup"><span data-stu-id="e424b-220">VirtualMachine.Inventory.Create</span></span>| <span data-ttu-id="e424b-221">VirtualMachine.Inventory.Create</span><span class="sxs-lookup"><span data-stu-id="e424b-221">VirtualMachine.Inventory.Create</span></span> |
|<span data-ttu-id="e424b-222">VirtualMachine.Provisioning.DiskRandomAccess</span><span class="sxs-lookup"><span data-stu-id="e424b-222">VirtualMachine.Provisioning.DiskRandomAccess</span></span>| |
|<span data-ttu-id="e424b-223">VirtualMachine.Provisioning.DiskRandomRead</span><span class="sxs-lookup"><span data-stu-id="e424b-223">VirtualMachine.Provisioning.DiskRandomRead</span></span>|<span data-ttu-id="e424b-224">VirtualMachine.Provisioning.DiskRandomRead</span><span class="sxs-lookup"><span data-stu-id="e424b-224">VirtualMachine.Provisioning.DiskRandomRead</span></span> |
|<span data-ttu-id="e424b-225">VirtualMachine.State.CreateSnapshot</span><span class="sxs-lookup"><span data-stu-id="e424b-225">VirtualMachine.State.CreateSnapshot</span></span>| <span data-ttu-id="e424b-226">VirtualMachine.State.CreateSnapshot</span><span class="sxs-lookup"><span data-stu-id="e424b-226">VirtualMachine.State.CreateSnapshot</span></span>|
|<span data-ttu-id="e424b-227">VirtualMachine.State.RemoveSnapshot</span><span class="sxs-lookup"><span data-stu-id="e424b-227">VirtualMachine.State.RemoveSnapshot</span></span>|<span data-ttu-id="e424b-228">VirtualMachine.State.RemoveSnapshot</span><span class="sxs-lookup"><span data-stu-id="e424b-228">VirtualMachine.State.RemoveSnapshot</span></span> |
</br>



## <a name="create-a-vcenter-server-user-account-and-permissions"></a><span data-ttu-id="e424b-229">Skapa ett användarkonto för vCenter-servern och behörigheter</span><span class="sxs-lookup"><span data-stu-id="e424b-229">Create a vCenter Server user account and permissions</span></span>

<span data-ttu-id="e424b-230">Skapa ett användarkonto när hello roll med behörigheter har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="e424b-230">After hello role with privileges is set up, create a user account.</span></span> <span data-ttu-id="e424b-231">hello användarkonto har ett namn och lösenord som ger hello-autentiseringsuppgifter som används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="e424b-231">hello user account has a name and password, which provides hello credentials that are used for authentication.</span></span>

1. <span data-ttu-id="e424b-232">toocreate ett användarkonto i hello vCenter Server **Navigator** klickar du på **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="e424b-232">toocreate a user account, in hello vCenter Server **Navigator** panel, click **Users and Groups**.</span></span>

    ![Alternativet för användare och grupper](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    <span data-ttu-id="e424b-234">Hej **vCenter användare och grupper** panelen visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-234">hello **vCenter Users and Groups** panel appears.</span></span>

    ![vCenter-panelen för användare och grupper](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. <span data-ttu-id="e424b-236">I hello **vCenter användare och grupper** Kontrollpanelen, Välj hello **användare** fliken och klicka sedan på hello lägga till användare ikon (symbolen + hello).</span><span class="sxs-lookup"><span data-stu-id="e424b-236">In hello **vCenter Users and Groups** panel, select hello **Users** tab, and then click hello add users icon (hello + symbol).</span></span>

    <span data-ttu-id="e424b-237">Hej **ny användare** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-237">hello **New User** dialog box appears.</span></span>

3. <span data-ttu-id="e424b-238">I hello **ny användare** dialogrutan, Lägg till hello användarinformation och klicka sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="e424b-238">In hello **New User** dialog box, add hello user's information and then click **OK**.</span></span> <span data-ttu-id="e424b-239">I den här proceduren är hello användarnamn BackupAdmin.</span><span class="sxs-lookup"><span data-stu-id="e424b-239">In this procedure, hello username is BackupAdmin.</span></span>

    ![Dialogrutan Ny användare](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    <span data-ttu-id="e424b-241">hello nya användarkontot visas i hello.</span><span class="sxs-lookup"><span data-stu-id="e424b-241">hello new user account appears in hello list.</span></span>

4. <span data-ttu-id="e424b-242">tooassociate hello-användarkonto med hello roll i hello **Navigator** klickar du på **globala behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="e424b-242">tooassociate hello user account with hello role, in hello **Navigator** panel, click **Global Permissions**.</span></span> <span data-ttu-id="e424b-243">I hello **globala behörigheter** Kontrollpanelen, Välj hello **hantera** fliken och klicka sedan på hello lägga till ikonen (symbolen + hello).</span><span class="sxs-lookup"><span data-stu-id="e424b-243">In hello **Global Permissions** panel, select hello **Manage** tab, and then click hello add icon (hello + symbol).</span></span>

    ![Globala behörigheter panelen](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    <span data-ttu-id="e424b-245">Hej **globala behörigheter rot - Lägg till behörigheten** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-245">hello **Global Permissions Root - Add Permission** dialog box appears.</span></span>

5. <span data-ttu-id="e424b-246">I hello **globala behörighet rot - Lägg till behörigheten** dialogrutan klickar du på **Lägg till** toochoose hello användare eller grupp.</span><span class="sxs-lookup"><span data-stu-id="e424b-246">In hello **Global Permission Root - Add Permission** dialog box, click **Add** toochoose hello user or group.</span></span>

    ![Välj användare eller grupp](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    <span data-ttu-id="e424b-248">Hej **Välj användare eller grupper** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-248">hello **Select Users/Groups** dialog box appears.</span></span>

6. <span data-ttu-id="e424b-249">I hello **Välj användare eller grupper** dialogrutan Välj **BackupAdmin** och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e424b-249">In hello **Select Users/Groups** dialog box, choose **BackupAdmin** and then click **Add**.</span></span>

    <span data-ttu-id="e424b-250">I **användare**, hello *domän\användarnamn* formatet används för hello användarkonto.</span><span class="sxs-lookup"><span data-stu-id="e424b-250">In **Users**, hello *domain\username* format is used for hello user account.</span></span> <span data-ttu-id="e424b-251">Om du vill toouse en annan domän, väljer du den från hello **domän** lista.</span><span class="sxs-lookup"><span data-stu-id="e424b-251">If you want toouse a different domain, choose it from hello **Domain** list.</span></span>

    ![Lägg till BackupAdmin användare](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    <span data-ttu-id="e424b-253">Klicka på **OK** tooadd hello valda användare toohello **lägga till behörigheten** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e424b-253">Click **OK** tooadd hello selected users toohello **Add Permission** dialog box.</span></span>

7. <span data-ttu-id="e424b-254">Nu när du har identifierat hello användaren tilldela hello toohello användarroll.</span><span class="sxs-lookup"><span data-stu-id="e424b-254">Now that you've identified hello user, assign hello user toohello role.</span></span> <span data-ttu-id="e424b-255">I **tilldelas rollen**, Välj hello nedrullningsbara listan **BackupAdminRole**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e424b-255">In **Assigned Role**, from hello drop-down list, select **BackupAdminRole**, and then click **OK**.</span></span>

    ![Tilldela användare toorole](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  <span data-ttu-id="e424b-257">På hello **hantera** fliken i hello **globala behörigheter** panelen, hello nytt användarkonto och hello associerade roll visas i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="e424b-257">On hello **Manage** tab in hello **Global Permissions** panel, hello new user account and hello associated role appear in hello list.</span></span>


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a><span data-ttu-id="e424b-258">Upprätta vCenter serverautentiseringsuppgifter för Azure Backup Server</span><span class="sxs-lookup"><span data-stu-id="e424b-258">Establish vCenter Server credentials on Azure Backup Server</span></span>

<span data-ttu-id="e424b-259">Innan du lägger till hello VMware server tooAzure Backup Server, installera [uppdatering 1 för Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).</span><span class="sxs-lookup"><span data-stu-id="e424b-259">Before you add hello VMware server tooAzure Backup Server, install [Update 1 for Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).</span></span>

1. <span data-ttu-id="e424b-260">tooopen Azure Backup Server, klicka på ikonen hello hello Azure Backup Server skrivbordet.</span><span class="sxs-lookup"><span data-stu-id="e424b-260">tooopen Azure Backup Server, double-click hello icon on hello Azure Backup Server desktop.</span></span>

    ![Azure Backup Server-ikon](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    <span data-ttu-id="e424b-262">Om du inte hittar hello-ikon på hello skrivbordet, öppna Azure Backup Server hello listan över installerade appar.</span><span class="sxs-lookup"><span data-stu-id="e424b-262">If you can't find hello icon on hello desktop, open Azure Backup Server from hello list of installed apps.</span></span> <span data-ttu-id="e424b-263">Programnamn i hello Azure Backup Server kallas för Microsoft Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="e424b-263">hello Azure Backup Server app name is called Microsoft Azure Backup.</span></span>

2. <span data-ttu-id="e424b-264">Hello Azure Backup Server-konsolen, klicka på **Management**, klickar du på **produktionsservrar**, och klicka på verktygsfliken hello **hantera VMware**.</span><span class="sxs-lookup"><span data-stu-id="e424b-264">In hello Azure Backup Server console, click **Management**, click **Production Servers**, and then on hello tool ribbon, click **Manage VMware**.</span></span>

    ![Azure Backup Server-konsolen](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    <span data-ttu-id="e424b-266">Hej **hantera autentiseringsuppgifter** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-266">hello **Manage Credentials** dialog box appears.</span></span>

    ![Dialogrutan för Azure Backup Server hantera autentiseringsuppgifter](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. <span data-ttu-id="e424b-268">I hello **hantera autentiseringsuppgifter** dialogrutan klickar du på **Lägg till** tooopen hello **lägga till autentiseringsuppgifter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e424b-268">In hello **Manage Credentials** dialog box, click **Add** tooopen hello **Add Credential** dialog box.</span></span>

4. <span data-ttu-id="e424b-269">I hello **lägga till autentiseringsuppgifter** dialogrutan Ange ett namn och en beskrivning för hello nya autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e424b-269">In hello **Add Credential** dialog box, enter a name and a description for hello new credential.</span></span> <span data-ttu-id="e424b-270">Ange hello användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="e424b-270">Then specify hello username and password.</span></span> <span data-ttu-id="e424b-271">hello namnet *Contoso Vcenter autentiseringsuppgifter* används tooidentify hello autentiseringsuppgifter i hello nästa procedur.</span><span class="sxs-lookup"><span data-stu-id="e424b-271">hello name, *Contoso Vcenter credential* is used tooidentify hello credential in hello next procedure.</span></span> <span data-ttu-id="e424b-272">Använd hello samma användarnamn och lösenord som används för hello vCenter-servern.</span><span class="sxs-lookup"><span data-stu-id="e424b-272">Use hello same username and password that is used for hello vCenter Server.</span></span> <span data-ttu-id="e424b-273">Om hello vCenter Server och Azure Backup Server inte är i samma domän, hello i **användarnamn**, ange hello domän.</span><span class="sxs-lookup"><span data-stu-id="e424b-273">If hello vCenter Server and Azure Backup Server are not in hello same domain, in **User name**, specify hello domain.</span></span>

    ![Azure Backup Server lägga till autentiseringsuppgifter i dialogrutan](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    <span data-ttu-id="e424b-275">Klicka på **Lägg till** tooadd hello nya autentiseringsuppgifter tooAzure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="e424b-275">Click **Add** tooadd hello new credential tooAzure Backup Server.</span></span> <span data-ttu-id="e424b-276">hello ny autentiseringsuppgift visas i listan för hello hello **hantera autentiseringsuppgifter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="e424b-276">hello new credential appears in hello list in hello **Manage Credentials** dialog box.</span></span>
    
    ![Dialogrutan för Azure Backup Server hantera autentiseringsuppgifter](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. <span data-ttu-id="e424b-278">tooclose hello **hantera autentiseringsuppgifter** dialogrutan klickar du på hello **X** i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="e424b-278">tooclose hello **Manage Credentials** dialog box, click hello **X** in hello upper-right corner.</span></span>


## <a name="add-hello-vcenter-server-tooazure-backup-server"></a><span data-ttu-id="e424b-279">Lägg till hello vCenter Server tooAzure Backup Server</span><span class="sxs-lookup"><span data-stu-id="e424b-279">Add hello vCenter Server tooAzure Backup Server</span></span>

<span data-ttu-id="e424b-280">Produktion kan servern tillägg använda tooadd hello vCenter Server tooAzure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="e424b-280">Production Server Addition Wizard is used tooadd hello vCenter Server tooAzure Backup Server.</span></span>

<span data-ttu-id="e424b-281">tooopen produktion Server tillägg guiden fullständig hello nedan:</span><span class="sxs-lookup"><span data-stu-id="e424b-281">tooopen Production Server Addition Wizard, complete hello following procedure:</span></span>

1. <span data-ttu-id="e424b-282">Hello Azure Backup Server-konsolen, klicka på **Management**, klickar du på **produktionsservrar**, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="e424b-282">In hello Azure Backup Server console, click **Management**, click **Production Servers**, and then click **Add**.</span></span>

    ![Öppna produktion tillägg guiden](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    <span data-ttu-id="e424b-284">Hej **produktion tillägg guiden** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-284">hello **Production Server Addition Wizard** dialog box appears.</span></span>

    ![Guiden för produktion Server-tillägg](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. <span data-ttu-id="e424b-286">På hello **Välj produktionsservern typen** väljer **VMware-servrar**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e424b-286">On hello **Select Production Server type** page, select **VMware Servers**, and then click **Next**.</span></span>

3. <span data-ttu-id="e424b-287">I **Server namn eller IP-adress**, ange hello fullständigt kvalificerade domännamnet (FQDN) eller IP-adressen för hello VMware-server.</span><span class="sxs-lookup"><span data-stu-id="e424b-287">In **Server Name/IP Address**, specify hello fully qualified domain name (FQDN) or IP address of hello VMware server.</span></span> <span data-ttu-id="e424b-288">Om alla hello ESXi servrar hanteras av hello samma vCenter kan du använda hello vCenter namn.</span><span class="sxs-lookup"><span data-stu-id="e424b-288">If all hello ESXi servers are managed by hello same vCenter, you can use hello vCenter name.</span></span>

    ![Ange VMware server FQDN eller IP-adress](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. <span data-ttu-id="e424b-290">I **SSL-Port**, ange hello-port som används toocommunicate med hello VMware-server.</span><span class="sxs-lookup"><span data-stu-id="e424b-290">In **SSL Port**, enter hello port that is used toocommunicate with hello VMware server.</span></span> <span data-ttu-id="e424b-291">Använda port 443, som är standardporten för hello, såvida du inte vet att en annan port krävs.</span><span class="sxs-lookup"><span data-stu-id="e424b-291">Use port 443, which is hello default port, unless you know that a different port is required.</span></span>

5. <span data-ttu-id="e424b-292">I **ange autentiseringsuppgifter**, Välj hello autentiseringsuppgifter som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="e424b-292">In **Specify Credential**, select hello credential that you created earlier.</span></span>

    ![Ange autentiseringsuppgifter](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. <span data-ttu-id="e424b-294">Klicka på **Lägg till** tooadd hello VMware toohello serverlistan i **lägga till VMware-servrar**, och klicka sedan på **nästa** toomove toohello nästa sida i guiden hello.</span><span class="sxs-lookup"><span data-stu-id="e424b-294">Click **Add** tooadd hello VMware server toohello list of **Added VMware Servers**, and then click **Next** toomove toohello next page in hello wizard.</span></span>

    ![Lägg till VMWare-server och autentiseringsuppgifter](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. <span data-ttu-id="e424b-296">I hello **sammanfattning** klickar du på **Lägg till** tooadd hello angetts VMware server tooAzure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="e424b-296">In hello **Summary** page, click **Add** tooadd hello specified VMware server tooAzure Backup Server.</span></span>

    ![Lägg till VMware server tooAzure Backup Server](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  <span data-ttu-id="e424b-298">hello VMware server backup är en agentlös säkerhetskopiering och hello ny server läggs omedelbart.</span><span class="sxs-lookup"><span data-stu-id="e424b-298">hello VMware server backup is an agentless backup, and hello new server is added immediately.</span></span> <span data-ttu-id="e424b-299">Hej **Slutför** sidan visar du hello resultat.</span><span class="sxs-lookup"><span data-stu-id="e424b-299">hello **Finish** page shows you hello results.</span></span>

  ![Sidan Slutför](./media/backup-azure-backup-server-vmware/summary-screen.png)

  <span data-ttu-id="e424b-301">tooadd flera instanser av vCenter Server tooAzure Backup Server, upprepa hello föregående steg i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="e424b-301">tooadd multiple instances of vCenter Server tooAzure Backup Server, repeat hello previous steps in this section.</span></span>

<span data-ttu-id="e424b-302">När du har lagt till hello vCenter Server tooAzure Backup Server är hello nästa steg toocreate en skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="e424b-302">After you add hello vCenter Server tooAzure Backup Server, hello next step is toocreate a protection group.</span></span> <span data-ttu-id="e424b-303">Hej skyddsgrupp anger hello detaljer för kortsiktig eller långsiktig kvarhållning och det är där du definierar och tillämpa hello säkerhetskopieringsprincip.</span><span class="sxs-lookup"><span data-stu-id="e424b-303">hello protection group specifies hello various details for short or long-term retention, and it is where you define and apply hello backup policy.</span></span> <span data-ttu-id="e424b-304">hello säkerhetskopieringsprincip är hello schema för när säkerhetskopiering sker och vad som säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="e424b-304">hello backup policy is hello schedule for when backups occur, and what is backed up.</span></span>


## <a name="configure-a-protection-group"></a><span data-ttu-id="e424b-305">Konfigurera en skyddsgrupp</span><span class="sxs-lookup"><span data-stu-id="e424b-305">Configure a protection group</span></span>

<span data-ttu-id="e424b-306">Om du inte har använt System Center Data Protection Manager eller Azure Backup Server innan du läser [planera för säkerhetskopiering till disk](https://technet.microsoft.com/library/hh758026.aspx) tooprepare maskinvarumiljön.</span><span class="sxs-lookup"><span data-stu-id="e424b-306">If you have not used System Center Data Protection Manager or Azure Backup Server before, see [Plan for disk backups](https://technet.microsoft.com/library/hh758026.aspx) tooprepare your hardware environment.</span></span> <span data-ttu-id="e424b-307">När du har kontrollerat att du har rätt lagring kan du använda hello Skapa ny Skyddsgrupp guiden tooadd virtuella VMware-datorer.</span><span class="sxs-lookup"><span data-stu-id="e424b-307">After you check that you have proper storage, use hello Create New Protection Group wizard tooadd VMware virtual machines.</span></span>

1. <span data-ttu-id="e424b-308">Hello Azure Backup Server-konsolen, klicka på **skydd**, och klicka på verktygsfliken hello **ny** tooopen hello Skapa ny Skyddsgrupp guiden.</span><span class="sxs-lookup"><span data-stu-id="e424b-308">In hello Azure Backup Server console, click **Protection**, and in hello tool ribbon, click **New** tooopen hello Create New Protection Group wizard.</span></span>

    ![Öppna hello Skapa ny Skyddsgrupp guiden](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    <span data-ttu-id="e424b-310">Hej **Skapa ny Skyddsgrupp** dialogruta visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-310">hello **Create New Protection Group** wizard dialog box appears.</span></span>

    ![Dialogruta för guiden Skapa ny Skyddsgrupp](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    <span data-ttu-id="e424b-312">Klicka på **nästa** tooadvance toohello **Välj typ av skyddsgrupp** sidan.</span><span class="sxs-lookup"><span data-stu-id="e424b-312">Click **Next** tooadvance toohello **Select protection group type** page.</span></span>

2. <span data-ttu-id="e424b-313">På hello **typ av Välj Skyddsgrupp** väljer **servrar** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e424b-313">On hello **Select Protection group type** page, select **Servers** and then click **Next**.</span></span> <span data-ttu-id="e424b-314">Hej **Välj gruppmedlemmar** visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-314">hello **Select group members** page appears.</span></span>

3. <span data-ttu-id="e424b-315">På hello **Välj gruppmedlemmar** sida, hello tillgängliga medlemmar och hello valda medlemmar visas.</span><span class="sxs-lookup"><span data-stu-id="e424b-315">On hello **Select group members** page, hello available members and hello selected members appear.</span></span> <span data-ttu-id="e424b-316">Välj hello medlemmar som du vill tooprotect och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e424b-316">Select hello members that you want tooprotect, and then click **Next**.</span></span>

    ![Välj gruppmedlemmar](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    <span data-ttu-id="e424b-318">När du väljer en medlem, om du markerar en mapp som innehåller andra mappar eller virtuella datorer, väljs även dessa mappar och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e424b-318">When you select a member, if you select a folder that contains other folders or VMs, those folders and VMs are also selected.</span></span> <span data-ttu-id="e424b-319">hello inkludering av hello mappar och virtuella datorer i hello överordnade mappen kallas mappnivå skydd.</span><span class="sxs-lookup"><span data-stu-id="e424b-319">hello inclusion of hello folders and VMs in hello parent folder is called folder-level protection.</span></span> <span data-ttu-id="e424b-320">tooremove en mapp eller VM, rensa hello kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="e424b-320">tooremove a folder or VM, clear hello check box.</span></span>

    <span data-ttu-id="e424b-321">Om en virtuell dator eller en mapp som innehåller en virtuell dator är redan skyddade tooAzure, kan du välja den virtuella datorn igen.</span><span class="sxs-lookup"><span data-stu-id="e424b-321">If a VM, or a folder containing a VM, is already protected tooAzure, you cannot select that VM again.</span></span> <span data-ttu-id="e424b-322">Det vill säga när en virtuell dator är skyddade tooAzure, kan du inte skydda igen, som förhindrar att duplicerade återställningspunkter skapas för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e424b-322">That is, after a VM is protected tooAzure, it cannot be protected again, which prevents duplicate recovery points from being created for one VM.</span></span> <span data-ttu-id="e424b-323">Om du vill toosee skyddar vilka Azure Backup Server-instansen redan en medlem, punkt toohello toosee hello medlemsnamn av hello skyddar server.</span><span class="sxs-lookup"><span data-stu-id="e424b-323">If you want toosee which Azure Backup Server instance already protects a member, point toohello member toosee hello name of hello protecting server.</span></span>

4. <span data-ttu-id="e424b-324">På hello **Välj Dataskyddsmetod** anger du ett namn för hello skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="e424b-324">On hello **Select Data Protection Method** page, enter a name for hello protection group.</span></span> <span data-ttu-id="e424b-325">Kortsiktigt skydd (toodisk) och online-skydd är markerade.</span><span class="sxs-lookup"><span data-stu-id="e424b-325">Short-term protection (toodisk) and online protection are selected.</span></span> <span data-ttu-id="e424b-326">Om du vill toouse online-skydd (tooAzure), måste du använda toodisk för kortsiktigt skydd.</span><span class="sxs-lookup"><span data-stu-id="e424b-326">If you want toouse online protection (tooAzure), you must use short-term protection toodisk.</span></span> <span data-ttu-id="e424b-327">Klicka på **nästa** tooproceed toohello kortsiktigt skydd intervall.</span><span class="sxs-lookup"><span data-stu-id="e424b-327">Click **Next** tooproceed toohello short-term protection range.</span></span>

    ![Välj dataskyddsmetod](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. <span data-ttu-id="e424b-329">På hello **ange kortvariga mål** sidan för **Kvarhållningsintervall**, ange hello antalet dagar som du vill tooretain återställningspunkter som är *lagras toodisk*.</span><span class="sxs-lookup"><span data-stu-id="e424b-329">On hello **Specify Short-Term Goals** page, for **Retention Range**, specify hello number of days that you want tooretain recovery points that are *stored toodisk*.</span></span> <span data-ttu-id="e424b-330">Om du vill att toochange hello tiden och dagarna när återställningspunkter tas **ändra**.</span><span class="sxs-lookup"><span data-stu-id="e424b-330">If you want toochange hello time and days when recovery points are taken, click **Modify**.</span></span> <span data-ttu-id="e424b-331">hello kortsiktig återställningspunkter är fullständiga säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="e424b-331">hello short-term recovery points are full backups.</span></span> <span data-ttu-id="e424b-332">De är inte inkrementella säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="e424b-332">They are not incremental backups.</span></span> <span data-ttu-id="e424b-333">När du är nöjd med hello kortsiktiga mål klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e424b-333">When you are satisfied with hello short-term goals, click **Next**.</span></span>

    ![Ange kortsiktiga mål](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. <span data-ttu-id="e424b-335">På hello **granska diskallokering** granskar och om det behövs ändrar hello diskutrymme för hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e424b-335">On hello **Review Disk Allocation** page, review and if necessary, modify hello disk space for hello VMs.</span></span> <span data-ttu-id="e424b-336">hello rekommenderad diskallokeringar baseras på hello Kvarhållningsintervall som anges i hello **ange kortvariga mål** sidan hello typ av arbetsbelastning och hello storleken på hello skyddade data (som identifieras i steg 3).</span><span class="sxs-lookup"><span data-stu-id="e424b-336">hello recommended disk allocations are based on hello retention range that is specified in hello **Specify Short-Term Goals** page, hello type of workload, and hello size of hello protected data (identified in step 3).</span></span>  

  - <span data-ttu-id="e424b-337">**Datastorlek:** storleken på hello data i hello skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="e424b-337">**Data size:** Size of hello data in hello protection group.</span></span>
  - <span data-ttu-id="e424b-338">**Diskutrymme:** hello rekommenderade mängden diskutrymme för hello skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="e424b-338">**Disk space:** hello recommended amount of disk space for hello protection group.</span></span> <span data-ttu-id="e424b-339">Om du vill toomodify den här inställningen allokerar du ett totalt utrymme som är något större än hello belopp som du uppskattar att varje datakälla växer.</span><span class="sxs-lookup"><span data-stu-id="e424b-339">If you want toomodify this setting, you should allocate total space that is slightly larger than hello amount that you estimate each data source grows.</span></span>
  - <span data-ttu-id="e424b-340">**Samordna data:** om du aktiverar samordning kan flera datakällor i hello skydd kan mappa tooa enda replik- och återställningspunktvolym.</span><span class="sxs-lookup"><span data-stu-id="e424b-340">**Colocate data:** If you turn on colocation, multiple data sources in hello protection can map tooa single replica and recovery point volume.</span></span> <span data-ttu-id="e424b-341">Samordning stöds inte för alla arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="e424b-341">Colocation isn't supported for all workloads.</span></span>
  - <span data-ttu-id="e424b-342">**Utöka automatiskt:** om du aktiverar den här inställningen om data i gruppen för skyddade hello blir för stor hello första fördelning, System Center Data Protection Manager försöker tooincrease hello diskstorleken med 25 procent.</span><span class="sxs-lookup"><span data-stu-id="e424b-342">**Automatically grow:** If you turn on this setting, if data in hello protected group outgrows hello initial allocation, System Center Data Protection Manager tries tooincrease hello disk size by 25 percent.</span></span>
  - <span data-ttu-id="e424b-343">**Lagringspoolinformation:** visar hello status hello lagringspoolen, inklusive total och återstående diskstorlek.</span><span class="sxs-lookup"><span data-stu-id="e424b-343">**Storage pool details:** Shows hello status of hello storage pool, including total and remaining disk size.</span></span>

    ![Granska diskallokering](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    <span data-ttu-id="e424b-345">När du är nöjd med hello utrymmestilldelning klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e424b-345">When you are satisfied with hello space allocation, click **Next**.</span></span>

7. <span data-ttu-id="e424b-346">På hello **Välj Replikskapandemetod** anger du hur toogenerate hello inledande kopia eller replik av hello skyddade data på Azure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="e424b-346">On hello **Choose Replica Creation Method** page, specify how you want toogenerate hello initial copy, or replica, of hello protected data on Azure Backup Server.</span></span>

    <span data-ttu-id="e424b-347">hello standardvärdet är **automatiskt över hello nätverk** och **nu**.</span><span class="sxs-lookup"><span data-stu-id="e424b-347">hello default is **Automatically over hello network** and **Now**.</span></span> <span data-ttu-id="e424b-348">Om du använder hello standard, rekommenderar vi att du anger en tid.</span><span class="sxs-lookup"><span data-stu-id="e424b-348">If you use hello default, we recommend that you specify an off-peak time.</span></span> <span data-ttu-id="e424b-349">Välj **senare** och ange en dag och tid.</span><span class="sxs-lookup"><span data-stu-id="e424b-349">Choose **Later** and specify a day and time.</span></span>

    <span data-ttu-id="e424b-350">Överväg att replikera hello data offline med hjälp av flyttbart medium för stora mängder data eller mindre än optimala nätverksförhållanden.</span><span class="sxs-lookup"><span data-stu-id="e424b-350">For large amounts of data or less-than-optimal network conditions, consider replicating hello data offline by using removable media.</span></span>

    <span data-ttu-id="e424b-351">När du har gjort dina val klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e424b-351">After you have made your choices, click **Next**.</span></span>

    ![Välj metod för skapande av replik](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. <span data-ttu-id="e424b-353">På hello **alternativ för konsekvenskontroll** anger hur och när tooautomate hello konsekvenskontroller.</span><span class="sxs-lookup"><span data-stu-id="e424b-353">On hello **Consistency Check Options** page, select how and when tooautomate hello consistency checks.</span></span> <span data-ttu-id="e424b-354">Du kan köra en konsekvenskontroll när replikdata blir inkonsekvent, eller på ett schema.</span><span class="sxs-lookup"><span data-stu-id="e424b-354">You can run consistency checks when replica data becomes inconsistent, or on a set schedule.</span></span>

    <span data-ttu-id="e424b-355">Om du inte vill tooconfigure automatiska konsekvenskontroller, kan du köra en manuell kontroll.</span><span class="sxs-lookup"><span data-stu-id="e424b-355">If you don't want tooconfigure automatic consistency checks, you can run a manual check.</span></span> <span data-ttu-id="e424b-356">Under hello skydd i hello Azure Backup Server-konsolen, högerklicka på hello skyddsgrupp och välj sedan **utför konsekvenskontroll**.</span><span class="sxs-lookup"><span data-stu-id="e424b-356">In hello protection area of hello Azure Backup Server console, right-click hello protection group and then select **Perform Consistency Check**.</span></span>

    <span data-ttu-id="e424b-357">Klicka på **nästa** toomove toohello nästa sida.</span><span class="sxs-lookup"><span data-stu-id="e424b-357">Click **Next** toomove toohello next page.</span></span>

9. <span data-ttu-id="e424b-358">På hello **ange Onlineskyddsdata** markerar du en eller flera datakällor som du vill tooprotect.</span><span class="sxs-lookup"><span data-stu-id="e424b-358">On hello **Specify Online Protection Data** page, select one or more data sources that you want tooprotect.</span></span> <span data-ttu-id="e424b-359">Du kan välja hello medlemmar individuellt eller klicka på **Markera alla** toochoose alla medlemmar.</span><span class="sxs-lookup"><span data-stu-id="e424b-359">You can select hello members individually, or click **Select All** toochoose all members.</span></span> <span data-ttu-id="e424b-360">När du väljer hello medlemmar, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e424b-360">After you choose hello members, click **Next**.</span></span>

    ![Ange onlineskyddsdata](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. <span data-ttu-id="e424b-362">På hello **Ange schema för onlinesäkerhetskopiering** anger hello schema toogenerate återställningspunkterna från hello disksäkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="e424b-362">On hello **Specify Online Backup Schedule** page, specify hello schedule toogenerate recovery points from hello disk backup.</span></span> <span data-ttu-id="e424b-363">När hello återställningspunkt skapas, är det överförda toohello Recovery Services-valvet i Azure.</span><span class="sxs-lookup"><span data-stu-id="e424b-363">After hello recovery point is generated, it is transferred toohello Recovery Services vault in Azure.</span></span> <span data-ttu-id="e424b-364">När du är nöjd med hello schema för onlinesäkerhetskopiering klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e424b-364">When you are satisfied with hello online backup schedule, click **Next**.</span></span>

    ![Ange onlinesäkerhetskopieringsschema](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. <span data-ttu-id="e424b-366">På hello **ange bevarandeprincip** kan ange hur länge du vill tooretain hello säkerhetskopierade data i Azure.</span><span class="sxs-lookup"><span data-stu-id="e424b-366">On hello **Specify Online Retention Policy** page, indicate how long you want tooretain hello backup data in Azure.</span></span> <span data-ttu-id="e424b-367">När hello princip har definierats, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="e424b-367">After hello policy is defined, click **Next**.</span></span>

    ![Ange bevarandeprincip](./media/backup-azure-backup-server-vmware/retention-policy.png)

    <span data-ttu-id="e424b-369">Det finns ingen tidsbegränsning för hur länge du kan behålla data i Azure.</span><span class="sxs-lookup"><span data-stu-id="e424b-369">There is no time limit for how long you can keep data in Azure.</span></span> <span data-ttu-id="e424b-370">När du lagrar återställningsdata punkt i Azure hello bara gränsen är att du inte kan ha fler än 9999 återställningspunkter per skyddad instans.</span><span class="sxs-lookup"><span data-stu-id="e424b-370">When you store recovery point data in Azure, hello only limit is that you cannot have more than 9999 recovery points per protected instance.</span></span> <span data-ttu-id="e424b-371">I det här exemplet är hello skyddade instansen hello VMware-server.</span><span class="sxs-lookup"><span data-stu-id="e424b-371">In this example, hello protected instance is hello VMware server.</span></span>

12. <span data-ttu-id="e424b-372">På hello **sammanfattning** , granska hello information för dina skyddsgruppmedlemmar och inställningar, och klickar sedan på **Skapa grupp**.</span><span class="sxs-lookup"><span data-stu-id="e424b-372">On hello **Summary** page, review hello details for your protection group members and settings, and then click **Create Group**.</span></span>

    ![Skyddsgruppsmedlem och inställningen sammanfattning](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a><span data-ttu-id="e424b-374">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e424b-374">Next steps</span></span>
<span data-ttu-id="e424b-375">Om du använder Azure Backup Server tooprotect VMware arbetsbelastningar du kanske vill utnyttja Azure Backup Server toohelp skydda en [Microsoft Exchange server](./backup-azure-exchange-mabs.md), [Microsoft SharePoint-servergruppen](./backup-azure-backup-sharepoint-mabs.md), eller en [SQL Server-databas](./backup-azure-sql-mabs.md).</span><span class="sxs-lookup"><span data-stu-id="e424b-375">If you use Azure Backup Server tooprotect VMware workloads, you may be interested in using Azure Backup Server toohelp protect a [Microsoft Exchange server](./backup-azure-exchange-mabs.md), a [Microsoft SharePoint farm](./backup-azure-backup-sharepoint-mabs.md), or a [SQL Server database](./backup-azure-sql-mabs.md).</span></span>

<span data-ttu-id="e424b-376">Mer information om problem med registrering hello agent konfigurera hello skyddsgrupp eller för att säkerhetskopiera jobb finns [felsöka Azure Backup Server](./backup-azure-mabs-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="e424b-376">For information on problems with registering hello agent, configuring hello protection group, or backing up jobs, see [Troubleshoot Azure Backup Server](./backup-azure-mabs-troubleshoot.md).</span></span>
