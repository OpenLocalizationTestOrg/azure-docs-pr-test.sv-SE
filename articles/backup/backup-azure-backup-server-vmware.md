---
title: "Säkerhetskopiera VMware-servrar med Azure Backup Server | Microsoft Docs"
description: "Använda Azure Backup Server för att säkerhetskopiera en VMware vCenter/ESXi-servrar till Azure eller disk. Den här artikeln innehåller steg för steg-instruktion om att säkerhetskopiera (eller skydda) = VMware-arbetsbelastningar."
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
ms.openlocfilehash: ad331dffb7c31d12290f4223967c568e4535fe3c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="back-up-a-vmware-server-to-azure"></a><span data-ttu-id="91969-104">Säkerhetskopiera en VMware-server till Azure</span><span class="sxs-lookup"><span data-stu-id="91969-104">Back up a VMware server to Azure</span></span>

<span data-ttu-id="91969-105">Den här artikeln förklarar hur du konfigurerar Azure Backup-Server för att skydda VMware-serverarbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="91969-105">This article explains how to configure Azure Backup Server to help protect VMware server workloads.</span></span> <span data-ttu-id="91969-106">Den här artikeln förutsätter att du redan har Azure Backup-Server installerad.</span><span class="sxs-lookup"><span data-stu-id="91969-106">This article assumes you already have Azure Backup Server installed.</span></span> <span data-ttu-id="91969-107">Om du inte har Azure Backup-Server installerad, se [förbereda säkerhetskopiering av arbetsbelastningar med Azure Backup Server](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="91969-107">If you don't have Azure Backup Server installed, see [Prepare to back up workloads using Azure Backup Server](backup-azure-microsoft-azure-backup.md).</span></span>

<span data-ttu-id="91969-108">Azure Backup-Server kan säkerhetskopiera eller att skydda VMware vCenter serverversion 6.5, 6.0 och 5.5.</span><span class="sxs-lookup"><span data-stu-id="91969-108">Azure Backup Server can back up, or help protect, VMware vCenter Server version 6.5, 6.0 and 5.5.</span></span>


## <a name="create-a-secure-connection-to-the-vcenter-server"></a><span data-ttu-id="91969-109">Skapa en säker anslutning till vCenter-servern</span><span class="sxs-lookup"><span data-stu-id="91969-109">Create a secure connection to the vCenter Server</span></span>

<span data-ttu-id="91969-110">Azure Backup-servern kommunicerar med varje vCenter-servern via en HTTPS-kanal som standard.</span><span class="sxs-lookup"><span data-stu-id="91969-110">By default, Azure Backup Server communicates with each vCenter Server via an HTTPS channel.</span></span> <span data-ttu-id="91969-111">Om du vill aktivera säker kommunikation, rekommenderar vi att du installerar certifikatet VMware certifikatutfärdare (CA) på Azure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="91969-111">To turn on the secure communication, we recommend that you install the VMware Certificate Authority (CA) certificate on Azure Backup Server.</span></span> <span data-ttu-id="91969-112">Om du inte kräver säker kommunikation och föredrar att inaktivera kravet på HTTPS, se [inaktivera säker kommunikationsprotokoll](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol).</span><span class="sxs-lookup"><span data-stu-id="91969-112">If you don't require secure communication, and would prefer to disable the HTTPS requirement, see [Disable secure communication protocol](backup-azure-backup-server-vmware.md#disable-secure-communication-protocol).</span></span> <span data-ttu-id="91969-113">Importera det betrodda certifikatet på Azure Backup Server för att skapa en säker anslutning mellan Azure Backup Server och vCenter-servern.</span><span class="sxs-lookup"><span data-stu-id="91969-113">To create a secure connection between Azure Backup Server and the vCenter Server, import the trusted certificate on Azure Backup Server.</span></span>

<span data-ttu-id="91969-114">Normalt använder du en webbläsare på datorn med Azure Backup Server för att ansluta till vCenter-servern via vSphere webbklienten.</span><span class="sxs-lookup"><span data-stu-id="91969-114">Typically, you use a browser on the Azure Backup Server machine to connect to the vCenter Server via the vSphere Web Client.</span></span> <span data-ttu-id="91969-115">Första gången du använder Azure Backup Server webbläsaren för att ansluta till vCenter-servern, anslutningen är inte säker.</span><span class="sxs-lookup"><span data-stu-id="91969-115">The first time you use the Azure Backup Server browser to connect to the vCenter Server, the connection isn't secure.</span></span> <span data-ttu-id="91969-116">Följande bild visar osäker anslutning.</span><span class="sxs-lookup"><span data-stu-id="91969-116">The following image shows the unsecured connection.</span></span>

![Exempel på osäker anslutning till VMware-server](./media/backup-azure-backup-server-vmware/unsecure-url.png)

<span data-ttu-id="91969-118">Ladda ned de betrodda rotcertifikatutfärdare för att åtgärda problemet och skapa en säker anslutning.</span><span class="sxs-lookup"><span data-stu-id="91969-118">To fix this issue, and create a secure connection, download the trusted root CA certificates.</span></span>

1. <span data-ttu-id="91969-119">Ange Webbadressen till vSphere webbklienten i webbläsaren på Azure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="91969-119">In the browser on Azure Backup Server, enter the URL to the vSphere Web Client.</span></span> <span data-ttu-id="91969-120">VSphere webbklienten inloggningssidan visas.</span><span class="sxs-lookup"><span data-stu-id="91969-120">The vSphere Web Client login page appears.</span></span>

    ![vSphere Webbklient](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

    <span data-ttu-id="91969-122">Längst ned i informationen för administratörer och utvecklare att leta upp den **Download betrodda rotcertifikatutfärdarcertifikat** länk.</span><span class="sxs-lookup"><span data-stu-id="91969-122">At the bottom of the information for administrators and developers, locate the **Download trusted root CA certificates** link.</span></span>

    ![Länk för att ladda ned de betrodda rotcertifikatutfärdare](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

  <span data-ttu-id="91969-124">Om du inte ser inloggningssidan vSphere Webbklient, kontrollera i webbläsarens proxyinställningar.</span><span class="sxs-lookup"><span data-stu-id="91969-124">If you don't see the vSphere Web Client login page, check your browser's proxy settings.</span></span>

2. <span data-ttu-id="91969-125">Klicka på **Download betrodda rotcertifikatutfärdarcertifikat**.</span><span class="sxs-lookup"><span data-stu-id="91969-125">Click **Download trusted root CA certificates**.</span></span>

    <span data-ttu-id="91969-126">VCenter-servern hämtar en fil på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="91969-126">The vCenter Server downloads a file to your local computer.</span></span> <span data-ttu-id="91969-127">Filnamnet heter **hämta**.</span><span class="sxs-lookup"><span data-stu-id="91969-127">The file's name is named **download**.</span></span> <span data-ttu-id="91969-128">Beroende på din webbläsare får du ett meddelande som frågar om du vill öppna eller spara filen.</span><span class="sxs-lookup"><span data-stu-id="91969-128">Depending on your browser, you receive a message that asks whether to open or save the file.</span></span>

    ![Hämta meddelande när certifikat hämtas](./media/backup-azure-backup-server-vmware/download-certs.png)

3. <span data-ttu-id="91969-130">Spara filen på en plats på Azure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="91969-130">Save the file to a location on Azure Backup Server.</span></span> <span data-ttu-id="91969-131">När du sparar filen, kan du lägga till filnamnstillägget ZIP.</span><span class="sxs-lookup"><span data-stu-id="91969-131">When you save the file, add the .zip file name extension.</span></span>

    <span data-ttu-id="91969-132">Filen är en .zip-fil som innehåller information om certifikat.</span><span class="sxs-lookup"><span data-stu-id="91969-132">The file is a .zip file that contains the information about the certificates.</span></span> <span data-ttu-id="91969-133">Du kan använda extrahering verktyg med filnamnstillägget .zip.</span><span class="sxs-lookup"><span data-stu-id="91969-133">With the .zip extension, you can use the extraction tools.</span></span>

4. <span data-ttu-id="91969-134">Högerklicka på **download.zip**, och välj sedan **extrahera alla** extrahera innehållet.</span><span class="sxs-lookup"><span data-stu-id="91969-134">Right-click **download.zip**, and then select **Extract All** to extract the contents.</span></span>

    <span data-ttu-id="91969-135">ZIP-filen extraherar innehållet till en mapp med namnet **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="91969-135">The .zip file extracts its contents to a folder named **certs**.</span></span> <span data-ttu-id="91969-136">Två typer av filer som visas i mappen certifikat.</span><span class="sxs-lookup"><span data-stu-id="91969-136">Two types of files appear in the certs folder.</span></span> <span data-ttu-id="91969-137">På rotcertifikatfilen har ett tillägg som börjar med en numrerad sekvens som.0 och.1.</span><span class="sxs-lookup"><span data-stu-id="91969-137">The root certificate file has an extension that begins with a numbered sequence like .0 and .1.</span></span>
    
    <span data-ttu-id="91969-138">CRL-filen har ett tillägg som börjar med en sekvens som .r0 eller .r1.</span><span class="sxs-lookup"><span data-stu-id="91969-138">The CRL file has an extension that begins with a sequence like .r0 or .r1.</span></span> <span data-ttu-id="91969-139">CRL-filen är associerad med ett certifikat.</span><span class="sxs-lookup"><span data-stu-id="91969-139">The CRL file is associated with a certificate.</span></span>

    ![<span data-ttu-id="91969-140">Hämta filen extraheras lokalt</span><span class="sxs-lookup"><span data-stu-id="91969-140">Download file extracted locally</span></span> ](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

5. <span data-ttu-id="91969-141">I den **certifikat** mappen, högerklicka på rotcertifikatfilen och klicka på **Byt namn på**.</span><span class="sxs-lookup"><span data-stu-id="91969-141">In the **certs** folder, right-click the root certificate file, and then click **Rename**.</span></span>

    ![<span data-ttu-id="91969-142">Byt namn på rotcertifikat</span><span class="sxs-lookup"><span data-stu-id="91969-142">Rename root certificate</span></span> ](./media/backup-azure-backup-server-vmware/rename-cert.png)

    <span data-ttu-id="91969-143">Ändra det rotcertifikat tillägget till .crt.</span><span class="sxs-lookup"><span data-stu-id="91969-143">Change the root certificate's extension to .crt.</span></span> <span data-ttu-id="91969-144">När du tillfrågas om du inte vill du ändra filnamnstillägget Klicka **Ja** eller **OK**.</span><span class="sxs-lookup"><span data-stu-id="91969-144">When you're asked if you're sure you want to change the extension, click **Yes** or **OK**.</span></span> <span data-ttu-id="91969-145">Annars kan ändra du filens avsedda funktion.</span><span class="sxs-lookup"><span data-stu-id="91969-145">Otherwise, you change the file's intended function.</span></span> <span data-ttu-id="91969-146">Ikon för filen ändras till en ikon som representerar ett rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="91969-146">The icon for the file changes to an icon that represents a root certificate.</span></span>

6. <span data-ttu-id="91969-147">Högerklicka på rotcertifikatet och popup-menyn, Välj **installera certifikat**.</span><span class="sxs-lookup"><span data-stu-id="91969-147">Right-click the root certificate and from the pop-up menu, select **Install Certificate**.</span></span>

    <span data-ttu-id="91969-148">Den **guiden Importera certifikat** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="91969-148">The **Certificate Import Wizard** dialog box appears.</span></span>

7. <span data-ttu-id="91969-149">I den **guiden Importera certifikat** dialogrutan **lokal dator** som mål för certifikat och klicka sedan på **nästa** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="91969-149">In the **Certificate Import Wizard** dialog box, select **Local Machine** as the destination for the certificate, and then click **Next** to continue.</span></span>

    ![<span data-ttu-id="91969-150">Certifikatet lagringsalternativ för mål</span><span class="sxs-lookup"><span data-stu-id="91969-150">Certificate storage destination options</span></span> ](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

    <span data-ttu-id="91969-151">Om du tillfrågas om du vill tillåta ändringar till datorn klickar du på **Ja** eller **OK**, för alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="91969-151">If you're asked if you want to allow changes to the computer, click **Yes** or **OK**, to all the changes.</span></span>

8. <span data-ttu-id="91969-152">På den **certifikatarkiv** väljer **placera alla certifikat i nedanstående arkiv**, och klicka sedan på **Bläddra** välja certifikatarkivet.</span><span class="sxs-lookup"><span data-stu-id="91969-152">On the **Certificate Store** page, select **Place all certificates in the following store**, and then click **Browse** to choose the certificate store.</span></span>

    ![Placera certifikat i en specifik lagring punkt](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

    <span data-ttu-id="91969-154">Den **Välj certifikatarkiv** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="91969-154">The **Select Certificate Store** dialog box appears.</span></span>

    ![Mappen certifikathierarkin för lagring](./media/backup-azure-backup-server-vmware/cert-store.png)

9. <span data-ttu-id="91969-156">Välj **betrodda rotcertifikatutfärdare** som målmapp för certifikat och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="91969-156">Select **Trusted Root Certification Authorities** as the destination folder for the certificates, and then click **OK**.</span></span>

    ![Målmappen för certifikat](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

    <span data-ttu-id="91969-158">Den **betrodda rotcertifikatutfärdare** mappen bekräftas som certifikatarkivet.</span><span class="sxs-lookup"><span data-stu-id="91969-158">The **Trusted Root Certification Authorities** folder is confirmed as the certificate store.</span></span> <span data-ttu-id="91969-159">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="91969-159">Click **Next**.</span></span>

    ![Certifikat-mappen](./media/backup-azure-backup-server-vmware/certificate-import-wizard2.png)

10. <span data-ttu-id="91969-161">På den **Slutför guiden Importera certifikat** sidan, kontrollera att certifikatet finns i mappen önskade och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="91969-161">On the **Completing the Certificate Import Wizard** page, verify that the certificate is in the desired folder, and then click **Finish**.</span></span>

    ![Kontrollera att certifikatet är i rätt mapp](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

    <span data-ttu-id="91969-163">En dialogruta visas har importera lyckade certifikat bekräftats.</span><span class="sxs-lookup"><span data-stu-id="91969-163">A dialog box appears, the successful certificate import is confirmed.</span></span>

11. <span data-ttu-id="91969-164">Logga in till vCenter-servern för att bekräfta att anslutningen är säker.</span><span class="sxs-lookup"><span data-stu-id="91969-164">Sign in to the vCenter Server to confirm that your connection is secure.</span></span>

  <span data-ttu-id="91969-165">Om det går inte att genomföra Importera certifikat och du kan inte upprätta en säker anslutning, VMware vSphere-dokumentationen finns på [Skaffa servercertifikat](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).</span><span class="sxs-lookup"><span data-stu-id="91969-165">If the certificate import is not successful, and you cannot establish a secure connection, consult the VMware vSphere documentation on [obtaining server certificates](http://pubs.vmware.com/vsphere-60/index.jsp#com.vmware.wssdk.dsg.doc/sdk_sg_server_certificate_Appendixes.6.4.html).</span></span>

  <span data-ttu-id="91969-166">Om du har säker gränser inom din organisation och inte vill aktivera HTTPS-protokollet, kan du använda följande procedur för att inaktivera säker kommunikation.</span><span class="sxs-lookup"><span data-stu-id="91969-166">If you have secure boundaries within your organization, and don't want to turn on the HTTPS protocol, use the following procedure to disable the secure communications.</span></span>

### <a name="disable-secure-communication-protocol"></a><span data-ttu-id="91969-167">Inaktivera säker kommunikationsprotokollet</span><span class="sxs-lookup"><span data-stu-id="91969-167">Disable secure communication protocol</span></span>

<span data-ttu-id="91969-168">Om din organisation inte kräver HTTPS-protokollet, kan du använda följande steg för att inaktivera HTTPS.</span><span class="sxs-lookup"><span data-stu-id="91969-168">If your organization doesn't require the HTTPS protocol, use the following steps to disable HTTPS.</span></span> <span data-ttu-id="91969-169">Skapa en registernyckel som ignorerar standardbeteendet för att inaktivera standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="91969-169">To disable the default behavior, create a registry key that ignores the default behavior.</span></span>

1. <span data-ttu-id="91969-170">Kopiera och klistra in följande text i en txt-fil.</span><span class="sxs-lookup"><span data-stu-id="91969-170">Copy and paste the following text into a .txt file.</span></span>

  ```
  Windows Registry Editor Version 5.00
  [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
  "IgnoreCertificateValidation"=dword:00000001
  ```

2. <span data-ttu-id="91969-171">Spara filen till Azure Backup Server-datorn.</span><span class="sxs-lookup"><span data-stu-id="91969-171">Save the file to your Azure Backup Server computer.</span></span> <span data-ttu-id="91969-172">För filnamnet, använder du DisableSecureAuthentication.reg.</span><span class="sxs-lookup"><span data-stu-id="91969-172">For the file name, use DisableSecureAuthentication.reg.</span></span>

3. <span data-ttu-id="91969-173">Dubbelklicka på filen för att aktivera registerposten.</span><span class="sxs-lookup"><span data-stu-id="91969-173">Double-click the file to activate the registry entry.</span></span>


## <a name="create-a-role-and-user-account-on-the-vcenter-server"></a><span data-ttu-id="91969-174">Skapa en roll och användare på vCenter Server</span><span class="sxs-lookup"><span data-stu-id="91969-174">Create a role and user account on the vCenter Server</span></span>

<span data-ttu-id="91969-175">En roll är en fördefinierad uppsättning behörigheter på vCenter Server.</span><span class="sxs-lookup"><span data-stu-id="91969-175">On the vCenter Server, a role is a predefined set of privileges.</span></span> <span data-ttu-id="91969-176">En vCenter Server-administratören skapar rollerna.</span><span class="sxs-lookup"><span data-stu-id="91969-176">A vCenter Server administrator creates the roles.</span></span> <span data-ttu-id="91969-177">Om du vill tilldela behörigheter par användarkonton med rollen som administratör.</span><span class="sxs-lookup"><span data-stu-id="91969-177">To assign permissions, the administrator pairs user accounts with a role.</span></span> <span data-ttu-id="91969-178">Skapa en roll med specifika privilegier för att upprätta nödvändiga autentiseringsuppgifter för att säkerhetskopiera vCenter Server-datorn, och sedan associera användarkonton med rollen.</span><span class="sxs-lookup"><span data-stu-id="91969-178">To establish the necessary user credentials to back up the vCenter Server computer, create a role with specific privileges, and then associate the user account with the role.</span></span>

<span data-ttu-id="91969-179">Azure Backup-servern använder ett användarnamn och lösenord för att autentisera till vCenter-servern.</span><span class="sxs-lookup"><span data-stu-id="91969-179">Azure Backup Server uses a username and password to authenticate with the vCenter Server.</span></span> <span data-ttu-id="91969-180">Azure Backup-servern använder autentiseringsuppgifterna som autentisering för alla säkerhetskopieringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="91969-180">Azure Backup Server uses these credentials as authentication for all backup operations.</span></span>

<span data-ttu-id="91969-181">Lägg till en vCenter-serverrollen och dess behörighet för en säkerhetskopiering administratör:</span><span class="sxs-lookup"><span data-stu-id="91969-181">To add a vCenter Server role and its privileges for a backup administrator:</span></span>

1. <span data-ttu-id="91969-182">Logga in till vCenter-servern och sedan i vCenter Server **Navigator** klickar du på **Administration**.</span><span class="sxs-lookup"><span data-stu-id="91969-182">Sign in to the vCenter Server, and then in the vCenter Server **Navigator** panel, click **Administration**.</span></span>

    ![Administrationsalternativ vCenter Server Navigator Kontrollpanelen](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

2. <span data-ttu-id="91969-184">I **Administration** Välj **roller**, och klicka sedan på den **roller** Kontrollpanelen klickar du på ikonen Lägg till roll (den symbolen +).</span><span class="sxs-lookup"><span data-stu-id="91969-184">In **Administration** select **Roles**, and then in the **Roles** panel click the add role icon (the + symbol).</span></span>

    ![Lägg till roll](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

    <span data-ttu-id="91969-186">Den **skapa roll** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="91969-186">The **Create Role** dialog box appears.</span></span>

    ![Skapa en roll](./media/backup-azure-backup-server-vmware/vmware-define-new-role-priv.png)

3. <span data-ttu-id="91969-188">I den **skapa roll** i dialogrutan den **rollnamn** ange *BackupAdminRole*.</span><span class="sxs-lookup"><span data-stu-id="91969-188">In the **Create Role** dialog box, in the **Role name** box, enter *BackupAdminRole*.</span></span> <span data-ttu-id="91969-189">Rollnamnet kan vara vad du vill, men det bör vara att känna igen för rollens ändamål.</span><span class="sxs-lookup"><span data-stu-id="91969-189">The role name can be whatever you like, but it should be recognizable for the role's purpose.</span></span>

4. <span data-ttu-id="91969-190">Välj behörigheter för rätt version av vCenter och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="91969-190">Select the privileges for the appropriate version of vCenter, and then click **OK**.</span></span> <span data-ttu-id="91969-191">I följande tabell visas behörigheter som krävs för vCenter 6.0 och vCenter 5.5.</span><span class="sxs-lookup"><span data-stu-id="91969-191">The following table identifies the required privileges for vCenter 6.0 and vCenter 5.5.</span></span>

  <span data-ttu-id="91969-192">När du väljer behörigheter, klickar du på ikonen bredvid överordnade etiketten Expandera överordnat och visa behörigheterna som underordnade.</span><span class="sxs-lookup"><span data-stu-id="91969-192">When you select the privileges, click the icon next to the parent label to expand the parent and view the child privileges.</span></span> <span data-ttu-id="91969-193">Om du vill välja VirtualMachine behörigheter som du behöver gå flera nivåer i hierarkin för överordnad-underordnad.</span><span class="sxs-lookup"><span data-stu-id="91969-193">To select the VirtualMachine privileges, you need to go several levels into the parent child hierarchy.</span></span> <span data-ttu-id="91969-194">Du behöver inte markera alla underordnade privilegier i privilegium som överordnad.</span><span class="sxs-lookup"><span data-stu-id="91969-194">You don't need to select all child privileges within a parent privilege.</span></span>

  ![Privilegiet hierarkin överordnad-underordnad](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

  <span data-ttu-id="91969-196">När du klickar på **OK**, den nya rollen visas i listan på panelen roller.</span><span class="sxs-lookup"><span data-stu-id="91969-196">After you click **OK**, the new role appears in the list on the Roles panel.</span></span>

|<span data-ttu-id="91969-197">Behörigheter för vCenter 6.0</span><span class="sxs-lookup"><span data-stu-id="91969-197">Privileges for vCenter 6.0</span></span>| <span data-ttu-id="91969-198">Behörigheter för vCenter 5.5</span><span class="sxs-lookup"><span data-stu-id="91969-198">Privileges for vCenter 5.5</span></span>|
|--------------------------|---------------------------|
|<span data-ttu-id="91969-199">Datastore.AllocateSpace</span><span class="sxs-lookup"><span data-stu-id="91969-199">Datastore.AllocateSpace</span></span>   | <span data-ttu-id="91969-200">Datastore.AllocateSpace</span><span class="sxs-lookup"><span data-stu-id="91969-200">Datastore.AllocateSpace</span></span>|
|<span data-ttu-id="91969-201">Global.ManageCustomFields</span><span class="sxs-lookup"><span data-stu-id="91969-201">Global.ManageCustomFields</span></span> | <span data-ttu-id="91969-202">Global.ManageCustomerFields</span><span class="sxs-lookup"><span data-stu-id="91969-202">Global.ManageCustomerFields</span></span>|
|<span data-ttu-id="91969-203">Global.SetCustomFields</span><span class="sxs-lookup"><span data-stu-id="91969-203">Global.SetCustomFields</span></span>    |   |
|<span data-ttu-id="91969-204">Host.Local.CreateVM</span><span class="sxs-lookup"><span data-stu-id="91969-204">Host.Local.CreateVM</span></span>       | <span data-ttu-id="91969-205">Network.Assign</span><span class="sxs-lookup"><span data-stu-id="91969-205">Network.Assign</span></span> |
|<span data-ttu-id="91969-206">Network.Assign</span><span class="sxs-lookup"><span data-stu-id="91969-206">Network.Assign</span></span>            |  |
|<span data-ttu-id="91969-207">Resource.AssignVMToPool</span><span class="sxs-lookup"><span data-stu-id="91969-207">Resource.AssignVMToPool</span></span>   |  |
|<span data-ttu-id="91969-208">VirtualMachine.Config.AddNewDisk</span><span class="sxs-lookup"><span data-stu-id="91969-208">VirtualMachine.Config.AddNewDisk</span></span>  | <span data-ttu-id="91969-209">VirtualMachine.Config.AddNewDisk</span><span class="sxs-lookup"><span data-stu-id="91969-209">VirtualMachine.Config.AddNewDisk</span></span>   |
|<span data-ttu-id="91969-210">VirtualMachine.Config.AdvanceConfig</span><span class="sxs-lookup"><span data-stu-id="91969-210">VirtualMachine.Config.AdvanceConfig</span></span>| <span data-ttu-id="91969-211">VirtualMachine.Config.AdvancedConfig</span><span class="sxs-lookup"><span data-stu-id="91969-211">VirtualMachine.Config.AdvancedConfig</span></span>|
|<span data-ttu-id="91969-212">VirtualMachine.Config.ChangeTracking</span><span class="sxs-lookup"><span data-stu-id="91969-212">VirtualMachine.Config.ChangeTracking</span></span>| <span data-ttu-id="91969-213">VirtualMachine.Config.ChangeTracking</span><span class="sxs-lookup"><span data-stu-id="91969-213">VirtualMachine.Config.ChangeTracking</span></span> |
|<span data-ttu-id="91969-214">VirtualMachine.Config.HostUSBDevice</span><span class="sxs-lookup"><span data-stu-id="91969-214">VirtualMachine.Config.HostUSBDevice</span></span>||
|<span data-ttu-id="91969-215">VirtualMachine.Config.QueryUnownedFiles</span><span class="sxs-lookup"><span data-stu-id="91969-215">VirtualMachine.Config.QueryUnownedFiles</span></span>|    |
|<span data-ttu-id="91969-216">VirtualMachine.Config.SwapPlacement</span><span class="sxs-lookup"><span data-stu-id="91969-216">VirtualMachine.Config.SwapPlacement</span></span>| <span data-ttu-id="91969-217">VirtualMachine.Config.SwapPlacement</span><span class="sxs-lookup"><span data-stu-id="91969-217">VirtualMachine.Config.SwapPlacement</span></span> |
|<span data-ttu-id="91969-218">VirtualMachine.Interact.PowerOff</span><span class="sxs-lookup"><span data-stu-id="91969-218">VirtualMachine.Interact.PowerOff</span></span>| <span data-ttu-id="91969-219">VirtualMachine.Interact.PowerOff</span><span class="sxs-lookup"><span data-stu-id="91969-219">VirtualMachine.Interact.PowerOff</span></span> |
|<span data-ttu-id="91969-220">VirtualMachine.Inventory.Create</span><span class="sxs-lookup"><span data-stu-id="91969-220">VirtualMachine.Inventory.Create</span></span>| <span data-ttu-id="91969-221">VirtualMachine.Inventory.Create</span><span class="sxs-lookup"><span data-stu-id="91969-221">VirtualMachine.Inventory.Create</span></span> |
|<span data-ttu-id="91969-222">VirtualMachine.Provisioning.DiskRandomAccess</span><span class="sxs-lookup"><span data-stu-id="91969-222">VirtualMachine.Provisioning.DiskRandomAccess</span></span>| |
|<span data-ttu-id="91969-223">VirtualMachine.Provisioning.DiskRandomRead</span><span class="sxs-lookup"><span data-stu-id="91969-223">VirtualMachine.Provisioning.DiskRandomRead</span></span>|<span data-ttu-id="91969-224">VirtualMachine.Provisioning.DiskRandomRead</span><span class="sxs-lookup"><span data-stu-id="91969-224">VirtualMachine.Provisioning.DiskRandomRead</span></span> |
|<span data-ttu-id="91969-225">VirtualMachine.State.CreateSnapshot</span><span class="sxs-lookup"><span data-stu-id="91969-225">VirtualMachine.State.CreateSnapshot</span></span>| <span data-ttu-id="91969-226">VirtualMachine.State.CreateSnapshot</span><span class="sxs-lookup"><span data-stu-id="91969-226">VirtualMachine.State.CreateSnapshot</span></span>|
|<span data-ttu-id="91969-227">VirtualMachine.State.RemoveSnapshot</span><span class="sxs-lookup"><span data-stu-id="91969-227">VirtualMachine.State.RemoveSnapshot</span></span>|<span data-ttu-id="91969-228">VirtualMachine.State.RemoveSnapshot</span><span class="sxs-lookup"><span data-stu-id="91969-228">VirtualMachine.State.RemoveSnapshot</span></span> |
</br>



## <a name="create-a-vcenter-server-user-account-and-permissions"></a><span data-ttu-id="91969-229">Skapa ett användarkonto för vCenter-servern och behörigheter</span><span class="sxs-lookup"><span data-stu-id="91969-229">Create a vCenter Server user account and permissions</span></span>

<span data-ttu-id="91969-230">Skapa ett användarkonto när rollen med privilegier som har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="91969-230">After the role with privileges is set up, create a user account.</span></span> <span data-ttu-id="91969-231">Användarkontot har ett namn och lösenord, som innehåller de autentiseringsuppgifter som används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="91969-231">The user account has a name and password, which provides the credentials that are used for authentication.</span></span>

1. <span data-ttu-id="91969-232">Skapa ett användarkonto i vCenter Server **Navigator** klickar du på **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="91969-232">To create a user account, in the vCenter Server **Navigator** panel, click **Users and Groups**.</span></span>

    ![Alternativet för användare och grupper](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    <span data-ttu-id="91969-234">Den **vCenter användare och grupper** panelen visas.</span><span class="sxs-lookup"><span data-stu-id="91969-234">The **vCenter Users and Groups** panel appears.</span></span>

    ![vCenter-panelen för användare och grupper](./media/backup-azure-backup-server-vmware/usersandgroups.png)

2. <span data-ttu-id="91969-236">I den **vCenter användare och grupper** Kontrollpanelen, Välj den **användare** fliken och klicka sedan på ikonen Lägg till användare (i symbolen +).</span><span class="sxs-lookup"><span data-stu-id="91969-236">In the **vCenter Users and Groups** panel, select the **Users** tab, and then click the add users icon (the + symbol).</span></span>

    <span data-ttu-id="91969-237">Den **ny användare** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="91969-237">The **New User** dialog box appears.</span></span>

3. <span data-ttu-id="91969-238">I den **ny användare** dialogrutan, Lägg till information om användaren och klicka sedan **OK**.</span><span class="sxs-lookup"><span data-stu-id="91969-238">In the **New User** dialog box, add the user's information and then click **OK**.</span></span> <span data-ttu-id="91969-239">Användarnamnet är BackupAdmin i den här proceduren.</span><span class="sxs-lookup"><span data-stu-id="91969-239">In this procedure, the username is BackupAdmin.</span></span>

    ![Dialogrutan Ny användare](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

    <span data-ttu-id="91969-241">Det nya användarkontot visas i listan.</span><span class="sxs-lookup"><span data-stu-id="91969-241">The new user account appears in the list.</span></span>

4. <span data-ttu-id="91969-242">Associera användarkonton med rollen, i den **Navigator** klickar du på **globala behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="91969-242">To associate the user account with the role, in the **Navigator** panel, click **Global Permissions**.</span></span> <span data-ttu-id="91969-243">I den **globala behörigheter** Kontrollpanelen, Välj den **hantera** fliken och klicka sedan på ikonen Lägg till (den symbolen +).</span><span class="sxs-lookup"><span data-stu-id="91969-243">In the **Global Permissions** panel, select the **Manage** tab, and then click the add icon (the + symbol).</span></span>

    ![Globala behörigheter panelen](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

    <span data-ttu-id="91969-245">Den **globala behörigheter rot - Lägg till behörigheten** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="91969-245">The **Global Permissions Root - Add Permission** dialog box appears.</span></span>

5. <span data-ttu-id="91969-246">I den **globala behörighet rot - Lägg till behörigheten** dialogrutan klickar du på **Lägg till** att välja den användare eller grupp.</span><span class="sxs-lookup"><span data-stu-id="91969-246">In the **Global Permission Root - Add Permission** dialog box, click **Add** to choose the user or group.</span></span>

    ![Välj användare eller grupp](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

    <span data-ttu-id="91969-248">Den **Välj användare eller grupper** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="91969-248">The **Select Users/Groups** dialog box appears.</span></span>

6. <span data-ttu-id="91969-249">I den **Välj användare eller grupper** dialogrutan Välj **BackupAdmin** och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="91969-249">In the **Select Users/Groups** dialog box, choose **BackupAdmin** and then click **Add**.</span></span>

    <span data-ttu-id="91969-250">I **användare**, *domän\användarnamn* formatet används för användarkontot.</span><span class="sxs-lookup"><span data-stu-id="91969-250">In **Users**, the *domain\username* format is used for the user account.</span></span> <span data-ttu-id="91969-251">Om du vill använda en annan domän, väljer du det från den **domän** lista.</span><span class="sxs-lookup"><span data-stu-id="91969-251">If you want to use a different domain, choose it from the **Domain** list.</span></span>

    ![Lägg till BackupAdmin användare](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

    <span data-ttu-id="91969-253">Klicka på **OK** att lägga till de valda användarna till den **lägga till behörigheten** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91969-253">Click **OK** to add the selected users to the **Add Permission** dialog box.</span></span>

7. <span data-ttu-id="91969-254">Nu när du har identifierat användaren, tilldelar du användaren rollen.</span><span class="sxs-lookup"><span data-stu-id="91969-254">Now that you've identified the user, assign the user to the role.</span></span> <span data-ttu-id="91969-255">I **tilldelas rollen**, från listrutan, Välj **BackupAdminRole**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="91969-255">In **Assigned Role**, from the drop-down list, select **BackupAdminRole**, and then click **OK**.</span></span>

    ![Tilldela användare till rollen](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

  <span data-ttu-id="91969-257">På den **hantera** fliken i den **globala behörigheter** panelen, det nya användarkontot och tillhörande rolltjänster som visas i listan.</span><span class="sxs-lookup"><span data-stu-id="91969-257">On the **Manage** tab in the **Global Permissions** panel, the new user account and the associated role appear in the list.</span></span>


## <a name="establish-vcenter-server-credentials-on-azure-backup-server"></a><span data-ttu-id="91969-258">Upprätta vCenter serverautentiseringsuppgifter för Azure Backup Server</span><span class="sxs-lookup"><span data-stu-id="91969-258">Establish vCenter Server credentials on Azure Backup Server</span></span>

<span data-ttu-id="91969-259">Innan du lägger till VMware server Azure Backup Server, installera [uppdatering 1 för Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).</span><span class="sxs-lookup"><span data-stu-id="91969-259">Before you add the VMware server to Azure Backup Server, install [Update 1 for Azure Backup Server](https://support.microsoft.com/help/3175529/update-1-for-microsoft-azure-backup-server).</span></span>

1. <span data-ttu-id="91969-260">Öppna Azure Backup Server Dubbelklicka på ikonen på Azure Backup Server-datorn.</span><span class="sxs-lookup"><span data-stu-id="91969-260">To open Azure Backup Server, double-click the icon on the Azure Backup Server desktop.</span></span>

    ![Azure Backup Server-ikon](./media/backup-azure-backup-server-vmware/mabs-icon.png)

    <span data-ttu-id="91969-262">Om du inte hittar ikonen på skrivbordet, öppna Azure Backup Server i listan över installerade appar.</span><span class="sxs-lookup"><span data-stu-id="91969-262">If you can't find the icon on the desktop, open Azure Backup Server from the list of installed apps.</span></span> <span data-ttu-id="91969-263">Appnamnet Azure Backup Server kallas för Microsoft Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="91969-263">The Azure Backup Server app name is called Microsoft Azure Backup.</span></span>

2. <span data-ttu-id="91969-264">Azure Backup Server-konsolen, klicka på **Management**, klickar du på **produktionsservrar**, och klicka på verktygsfliken **hantera VMware**.</span><span class="sxs-lookup"><span data-stu-id="91969-264">In the Azure Backup Server console, click **Management**, click **Production Servers**, and then on the tool ribbon, click **Manage VMware**.</span></span>

    ![Azure Backup Server-konsolen](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

    <span data-ttu-id="91969-266">Den **hantera autentiseringsuppgifter** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="91969-266">The **Manage Credentials** dialog box appears.</span></span>

    ![Dialogrutan för Azure Backup Server hantera autentiseringsuppgifter](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

3. <span data-ttu-id="91969-268">I den **hantera autentiseringsuppgifter** dialogrutan klickar du på **Lägg till** att öppna den **lägga till autentiseringsuppgifter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91969-268">In the **Manage Credentials** dialog box, click **Add** to open the **Add Credential** dialog box.</span></span>

4. <span data-ttu-id="91969-269">I den **lägga till autentiseringsuppgifter** dialogrutan Ange ett namn och en beskrivning för den nya autentiseringsuppgiften.</span><span class="sxs-lookup"><span data-stu-id="91969-269">In the **Add Credential** dialog box, enter a name and a description for the new credential.</span></span> <span data-ttu-id="91969-270">Ange användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="91969-270">Then specify the username and password.</span></span> <span data-ttu-id="91969-271">Namnet *Contoso Vcenter autentiseringsuppgifter* används för att identifiera autentiseringsuppgifterna i nästa procedur.</span><span class="sxs-lookup"><span data-stu-id="91969-271">The name, *Contoso Vcenter credential* is used to identify the credential in the next procedure.</span></span> <span data-ttu-id="91969-272">Använd samma användarnamn och lösenord som används för vCenter-servern.</span><span class="sxs-lookup"><span data-stu-id="91969-272">Use the same username and password that is used for the vCenter Server.</span></span> <span data-ttu-id="91969-273">Om vCenter Server och Azure Backup Server inte är i samma domän, i **användarnamn**, ange domänen.</span><span class="sxs-lookup"><span data-stu-id="91969-273">If the vCenter Server and Azure Backup Server are not in the same domain, in **User name**, specify the domain.</span></span>

    ![Azure Backup Server lägga till autentiseringsuppgifter i dialogrutan](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

    <span data-ttu-id="91969-275">Klicka på **Lägg till** att lägga till de nya autentiseringsuppgifter i Azure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="91969-275">Click **Add** to add the new credential to Azure Backup Server.</span></span> <span data-ttu-id="91969-276">De nya autentiseringsuppgifter som visas i listan i den **hantera autentiseringsuppgifter** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="91969-276">The new credential appears in the list in the **Manage Credentials** dialog box.</span></span>
    
    ![Dialogrutan för Azure Backup Server hantera autentiseringsuppgifter](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

5. <span data-ttu-id="91969-278">Stäng den **hantera autentiseringsuppgifter** dialogrutan klickar du på den **X** i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="91969-278">To close the **Manage Credentials** dialog box, click the **X** in the upper-right corner.</span></span>


## <a name="add-the-vcenter-server-to-azure-backup-server"></a><span data-ttu-id="91969-279">Lägg till vCenter-servern till Azure Backup-Server</span><span class="sxs-lookup"><span data-stu-id="91969-279">Add the vCenter Server to Azure Backup Server</span></span>

<span data-ttu-id="91969-280">Produktion Server tillägg guiden används för att lägga till vCenter-servern till Azure Backup-Server.</span><span class="sxs-lookup"><span data-stu-id="91969-280">Production Server Addition Wizard is used to add the vCenter Server to Azure Backup Server.</span></span>

<span data-ttu-id="91969-281">Slutför följande procedur för att öppna guiden för produktion Server tillägg:</span><span class="sxs-lookup"><span data-stu-id="91969-281">To open Production Server Addition Wizard, complete the following procedure:</span></span>

1. <span data-ttu-id="91969-282">Azure Backup Server-konsolen, klicka på **Management**, klickar du på **produktionsservrar**, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="91969-282">In the Azure Backup Server console, click **Management**, click **Production Servers**, and then click **Add**.</span></span>

    ![Öppna produktion tillägg guiden](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

    <span data-ttu-id="91969-284">Den **produktion tillägg guiden** dialogrutan visas.</span><span class="sxs-lookup"><span data-stu-id="91969-284">The **Production Server Addition Wizard** dialog box appears.</span></span>

    ![Guiden för produktion Server-tillägg](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

2. <span data-ttu-id="91969-286">På den **Välj produktionsservern typen** väljer **VMware-servrar**, och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91969-286">On the **Select Production Server type** page, select **VMware Servers**, and then click **Next**.</span></span>

3. <span data-ttu-id="91969-287">I **Server namn eller IP-adress**, ange det fullständigt kvalificerade domännamnet (FQDN) eller VMware-serverns IP-adress.</span><span class="sxs-lookup"><span data-stu-id="91969-287">In **Server Name/IP Address**, specify the fully qualified domain name (FQDN) or IP address of the VMware server.</span></span> <span data-ttu-id="91969-288">Om alla ESXi-servrar hanteras av samma vCenter, kan du använda vCenter-namn.</span><span class="sxs-lookup"><span data-stu-id="91969-288">If all the ESXi servers are managed by the same vCenter, you can use the vCenter name.</span></span>

    ![Ange VMware server FQDN eller IP-adress](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. <span data-ttu-id="91969-290">I **SSL-Port**, ange den port som används för att kommunicera med VMware-servern.</span><span class="sxs-lookup"><span data-stu-id="91969-290">In **SSL Port**, enter the port that is used to communicate with the VMware server.</span></span> <span data-ttu-id="91969-291">Använda port 443, som är standardporten, såvida du inte vet att en annan port krävs.</span><span class="sxs-lookup"><span data-stu-id="91969-291">Use port 443, which is the default port, unless you know that a different port is required.</span></span>

5. <span data-ttu-id="91969-292">I **ange autentiseringsuppgifter**, Välj de autentiseringsuppgifter som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="91969-292">In **Specify Credential**, select the credential that you created earlier.</span></span>

    ![Ange autentiseringsuppgifter](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. <span data-ttu-id="91969-294">Klicka på **Lägg till** lägga till VMware-server i listan över **lägga till VMware-servrar**, och klicka sedan på **nästa** att flytta till nästa sida i guiden.</span><span class="sxs-lookup"><span data-stu-id="91969-294">Click **Add** to add the VMware server to the list of **Added VMware Servers**, and then click **Next** to move to the next page in the wizard.</span></span>

    ![Lägg till VMWare-server och autentiseringsuppgifter](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. <span data-ttu-id="91969-296">I den **sammanfattning** klickar du på **Lägg till** att lägga till den angivna VMware-servern till Azure Backup-Server.</span><span class="sxs-lookup"><span data-stu-id="91969-296">In the **Summary** page, click **Add** to add the specified VMware server to Azure Backup Server.</span></span>

    ![Lägga till VMware-server i Azure Backup Server](./media/backup-azure-backup-server-vmware/tasks-screen.png)

  <span data-ttu-id="91969-298">VMware server backup är en agentlös säkerhetskopiering och den nya servern läggs omedelbart.</span><span class="sxs-lookup"><span data-stu-id="91969-298">The VMware server backup is an agentless backup, and the new server is added immediately.</span></span> <span data-ttu-id="91969-299">Den **Slutför** sidan visas resultaten.</span><span class="sxs-lookup"><span data-stu-id="91969-299">The **Finish** page shows you the results.</span></span>

  ![Sidan Slutför](./media/backup-azure-backup-server-vmware/summary-screen.png)

  <span data-ttu-id="91969-301">Upprepa föregående steg i det här avsnittet för att lägga till flera instanser av vCenter-servern till Azure Backup-Server.</span><span class="sxs-lookup"><span data-stu-id="91969-301">To add multiple instances of vCenter Server to Azure Backup Server, repeat the previous steps in this section.</span></span>

<span data-ttu-id="91969-302">När du lägger till vCenter-servern till Azure Backup Server, är nästa steg att skapa en skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="91969-302">After you add the vCenter Server to Azure Backup Server, the next step is to create a protection group.</span></span> <span data-ttu-id="91969-303">Skyddsgruppen anger detaljer för kortsiktig eller långsiktig kvarhållning och det är där du definierar och tillämpa principen för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="91969-303">The protection group specifies the various details for short or long-term retention, and it is where you define and apply the backup policy.</span></span> <span data-ttu-id="91969-304">Principen för säkerhetskopiering är schema för när säkerhetskopiering sker och vad som säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="91969-304">The backup policy is the schedule for when backups occur, and what is backed up.</span></span>


## <a name="configure-a-protection-group"></a><span data-ttu-id="91969-305">Konfigurera en skyddsgrupp</span><span class="sxs-lookup"><span data-stu-id="91969-305">Configure a protection group</span></span>

<span data-ttu-id="91969-306">Om du inte har använt System Center Data Protection Manager eller Azure Backup Server innan du läser [planera för säkerhetskopiering till disk](https://technet.microsoft.com/library/hh758026.aspx) att förbereda maskinvarumiljön.</span><span class="sxs-lookup"><span data-stu-id="91969-306">If you have not used System Center Data Protection Manager or Azure Backup Server before, see [Plan for disk backups](https://technet.microsoft.com/library/hh758026.aspx) to prepare your hardware environment.</span></span> <span data-ttu-id="91969-307">När du har kontrollerat att du har rätt lagring kan du använda guiden Skapa ny Skyddsgrupp för att lägga till virtuella VMware-datorer.</span><span class="sxs-lookup"><span data-stu-id="91969-307">After you check that you have proper storage, use the Create New Protection Group wizard to add VMware virtual machines.</span></span>

1. <span data-ttu-id="91969-308">Azure Backup Server-konsolen, klicka på **skydd**, och klicka på verktygsfliken **ny** att öppna guiden Skapa ny Skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="91969-308">In the Azure Backup Server console, click **Protection**, and in the tool ribbon, click **New** to open the Create New Protection Group wizard.</span></span>

    ![Öppna guiden Skapa ny Skyddsgrupp](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

    <span data-ttu-id="91969-310">Den **Skapa ny Skyddsgrupp** dialogruta visas.</span><span class="sxs-lookup"><span data-stu-id="91969-310">The **Create New Protection Group** wizard dialog box appears.</span></span>

    ![Dialogruta för guiden Skapa ny Skyddsgrupp](./media/backup-azure-backup-server-vmware/protection-wizard.png)

    <span data-ttu-id="91969-312">Klicka på **nästa** att gå vidare till den **Välj typ av skyddsgrupp** sidan.</span><span class="sxs-lookup"><span data-stu-id="91969-312">Click **Next** to advance to the **Select protection group type** page.</span></span>

2. <span data-ttu-id="91969-313">På den **typ av Välj Skyddsgrupp** väljer **servrar** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91969-313">On the **Select Protection group type** page, select **Servers** and then click **Next**.</span></span> <span data-ttu-id="91969-314">Den **Välj gruppmedlemmar** visas.</span><span class="sxs-lookup"><span data-stu-id="91969-314">The **Select group members** page appears.</span></span>

3. <span data-ttu-id="91969-315">På den **Välj gruppmedlemmar** sida, tillgängliga medlemmar och de markerade medlemmarna visas.</span><span class="sxs-lookup"><span data-stu-id="91969-315">On the **Select group members** page, the available members and the selected members appear.</span></span> <span data-ttu-id="91969-316">Välj de medlemmar som du vill skydda och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91969-316">Select the members that you want to protect, and then click **Next**.</span></span>

    ![Välj gruppmedlemmar](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

    <span data-ttu-id="91969-318">När du väljer en medlem, om du markerar en mapp som innehåller andra mappar eller virtuella datorer, väljs även dessa mappar och virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="91969-318">When you select a member, if you select a folder that contains other folders or VMs, those folders and VMs are also selected.</span></span> <span data-ttu-id="91969-319">Inkludering av mappar och virtuella datorer i den överordnade mappen kallas mappnivå skydd.</span><span class="sxs-lookup"><span data-stu-id="91969-319">The inclusion of the folders and VMs in the parent folder is called folder-level protection.</span></span> <span data-ttu-id="91969-320">Avmarkera kryssrutan om du vill ta bort en mapp eller en VM.</span><span class="sxs-lookup"><span data-stu-id="91969-320">To remove a folder or VM, clear the check box.</span></span>

    <span data-ttu-id="91969-321">Om en virtuell dator eller en mapp som innehåller en virtuell dator är redan skyddade till Azure kan välja du inte den virtuella datorn igen.</span><span class="sxs-lookup"><span data-stu-id="91969-321">If a VM, or a folder containing a VM, is already protected to Azure, you cannot select that VM again.</span></span> <span data-ttu-id="91969-322">Det vill säga när en virtuell dator skyddas i Azure, kan du inte skydda igen, som förhindrar att duplicerade återställningspunkter skapas för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="91969-322">That is, after a VM is protected to Azure, it cannot be protected again, which prevents duplicate recovery points from being created for one VM.</span></span> <span data-ttu-id="91969-323">Om du vill se vilka Azure Backup Server-instansen redan skyddar en medlem, peka på medlemmen ska se namnet på servern som skyddar.</span><span class="sxs-lookup"><span data-stu-id="91969-323">If you want to see which Azure Backup Server instance already protects a member, point to the member to see the name of the protecting server.</span></span>

4. <span data-ttu-id="91969-324">På den **Välj Dataskyddsmetod** anger du ett namn för skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="91969-324">On the **Select Data Protection Method** page, enter a name for the protection group.</span></span> <span data-ttu-id="91969-325">Kortsiktigt skydd (på disk) och online-skydd är markerade.</span><span class="sxs-lookup"><span data-stu-id="91969-325">Short-term protection (to disk) and online protection are selected.</span></span> <span data-ttu-id="91969-326">Om du vill använda onlineskydd (för Azure) använder du kortvarigt skydd till disk.</span><span class="sxs-lookup"><span data-stu-id="91969-326">If you want to use online protection (to Azure), you must use short-term protection to disk.</span></span> <span data-ttu-id="91969-327">Klicka på **nästa** fortsätta till intervallet för kortsiktigt skydd.</span><span class="sxs-lookup"><span data-stu-id="91969-327">Click **Next** to proceed to the short-term protection range.</span></span>

    ![Välj dataskyddsmetod](./media/backup-azure-backup-server-vmware/name-protection-group.png)

5. <span data-ttu-id="91969-329">På den **ange kortvariga mål** sidan för **Kvarhållningsintervall**, ange antalet dagar som du vill behålla återställningspunkter som är *lagrade till disk*.</span><span class="sxs-lookup"><span data-stu-id="91969-329">On the **Specify Short-Term Goals** page, for **Retention Range**, specify the number of days that you want to retain recovery points that are *stored to disk*.</span></span> <span data-ttu-id="91969-330">Om du vill ändra tid och dagar när återställningspunkter tas på **ändra**.</span><span class="sxs-lookup"><span data-stu-id="91969-330">If you want to change the time and days when recovery points are taken, click **Modify**.</span></span> <span data-ttu-id="91969-331">Kortsiktig återställningspunkter är fullständiga säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="91969-331">The short-term recovery points are full backups.</span></span> <span data-ttu-id="91969-332">De är inte inkrementella säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="91969-332">They are not incremental backups.</span></span> <span data-ttu-id="91969-333">När du är nöjd med kortsiktiga mål klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91969-333">When you are satisfied with the short-term goals, click **Next**.</span></span>

    ![Ange kortsiktiga mål](./media/backup-azure-backup-server-vmware/short-term-goals.png)

6. <span data-ttu-id="91969-335">På den **granska diskallokering** granskar och om det behövs ändrar du hur mycket diskutrymme för de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="91969-335">On the **Review Disk Allocation** page, review and if necessary, modify the disk space for the VMs.</span></span> <span data-ttu-id="91969-336">De rekommenderade diskallokering baseras på kvarhållningsintervallet som anges i den **ange kortvariga mål** sidan typ av arbetsbelastning och storleken på skyddade data (som identifieras i steg 3).</span><span class="sxs-lookup"><span data-stu-id="91969-336">The recommended disk allocations are based on the retention range that is specified in the **Specify Short-Term Goals** page, the type of workload, and the size of the protected data (identified in step 3).</span></span>  

  - <span data-ttu-id="91969-337">**Datastorlek:** storleken på data i skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="91969-337">**Data size:** Size of the data in the protection group.</span></span>
  - <span data-ttu-id="91969-338">**Diskutrymme:** den rekommenderade mängden ledigt diskutrymme för skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="91969-338">**Disk space:** The recommended amount of disk space for the protection group.</span></span> <span data-ttu-id="91969-339">Om du vill ändra den här inställningen allokerar du ett totalt utrymme som är något större än vad du uppskattar att varje datakälla växer.</span><span class="sxs-lookup"><span data-stu-id="91969-339">If you want to modify this setting, you should allocate total space that is slightly larger than the amount that you estimate each data source grows.</span></span>
  - <span data-ttu-id="91969-340">**Samordna data:** om du aktiverar samordning kan flera datakällor i skyddet kan mappa till en enda replik- och datapunktsvolym.</span><span class="sxs-lookup"><span data-stu-id="91969-340">**Colocate data:** If you turn on colocation, multiple data sources in the protection can map to a single replica and recovery point volume.</span></span> <span data-ttu-id="91969-341">Samordning stöds inte för alla arbetsbelastningar.</span><span class="sxs-lookup"><span data-stu-id="91969-341">Colocation isn't supported for all workloads.</span></span>
  - <span data-ttu-id="91969-342">**Utöka automatiskt:** om du aktiverar den här inställningen om data i gruppen skyddade blir större än den ursprungliga fördelningen, System Center Data Protection Manager försöker öka diskstorleken på med 25 procent.</span><span class="sxs-lookup"><span data-stu-id="91969-342">**Automatically grow:** If you turn on this setting, if data in the protected group outgrows the initial allocation, System Center Data Protection Manager tries to increase the disk size by 25 percent.</span></span>
  - <span data-ttu-id="91969-343">**Lagringspoolinformation:** visar status för lagringspoolen, inklusive total och återstående diskstorlek.</span><span class="sxs-lookup"><span data-stu-id="91969-343">**Storage pool details:** Shows the status of the storage pool, including total and remaining disk size.</span></span>

    ![Granska diskallokering](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

    <span data-ttu-id="91969-345">När du är nöjd med utrymmesallokeringen klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91969-345">When you are satisfied with the space allocation, click **Next**.</span></span>

7. <span data-ttu-id="91969-346">På den **Välj Replikskapandemetod** anger du hur du vill skapa den ursprungliga kopian eller replik av skyddade data på Azure Backup Server.</span><span class="sxs-lookup"><span data-stu-id="91969-346">On the **Choose Replica Creation Method** page, specify how you want to generate the initial copy, or replica, of the protected data on Azure Backup Server.</span></span>

    <span data-ttu-id="91969-347">Standardvärdet är **automatiskt via nätverket** och **nu**.</span><span class="sxs-lookup"><span data-stu-id="91969-347">The default is **Automatically over the network** and **Now**.</span></span> <span data-ttu-id="91969-348">Om du använder standardinställningarna, rekommenderar vi att du anger en tid.</span><span class="sxs-lookup"><span data-stu-id="91969-348">If you use the default, we recommend that you specify an off-peak time.</span></span> <span data-ttu-id="91969-349">Välj **senare** och ange en dag och tid.</span><span class="sxs-lookup"><span data-stu-id="91969-349">Choose **Later** and specify a day and time.</span></span>

    <span data-ttu-id="91969-350">Överväg att replikera data offline med hjälp av flyttbart medium för stora mängder data eller mindre än optimala nätverksförhållanden.</span><span class="sxs-lookup"><span data-stu-id="91969-350">For large amounts of data or less-than-optimal network conditions, consider replicating the data offline by using removable media.</span></span>

    <span data-ttu-id="91969-351">När du har gjort dina val klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91969-351">After you have made your choices, click **Next**.</span></span>

    ![Välj metod för skapande av replik](./media/backup-azure-backup-server-vmware/replica-creation.png)

8. <span data-ttu-id="91969-353">På den **alternativ för konsekvenskontroll** sidan, Välj hur och när att automatisera konsekvenskontroller.</span><span class="sxs-lookup"><span data-stu-id="91969-353">On the **Consistency Check Options** page, select how and when to automate the consistency checks.</span></span> <span data-ttu-id="91969-354">Du kan köra en konsekvenskontroll när replikdata blir inkonsekvent, eller på ett schema.</span><span class="sxs-lookup"><span data-stu-id="91969-354">You can run consistency checks when replica data becomes inconsistent, or on a set schedule.</span></span>

    <span data-ttu-id="91969-355">Om du inte vill konfigurera automatiska konsekvenskontroller, kan du köra en manuell kontroll.</span><span class="sxs-lookup"><span data-stu-id="91969-355">If you don't want to configure automatic consistency checks, you can run a manual check.</span></span> <span data-ttu-id="91969-356">I området för Azure Backup Server-konsolen, högerklickar på skyddsgruppen och välj sedan **utför konsekvenskontroll**.</span><span class="sxs-lookup"><span data-stu-id="91969-356">In the protection area of the Azure Backup Server console, right-click the protection group and then select **Perform Consistency Check**.</span></span>

    <span data-ttu-id="91969-357">Klicka på **nästa** att flytta till nästa sida.</span><span class="sxs-lookup"><span data-stu-id="91969-357">Click **Next** to move to the next page.</span></span>

9. <span data-ttu-id="91969-358">På den **ange Onlineskyddsdata** markerar du en eller flera datakällor som du vill skydda.</span><span class="sxs-lookup"><span data-stu-id="91969-358">On the **Specify Online Protection Data** page, select one or more data sources that you want to protect.</span></span> <span data-ttu-id="91969-359">Du kan välja medlemmarna individuellt eller klicka på **Markera alla** att välja alla medlemmar.</span><span class="sxs-lookup"><span data-stu-id="91969-359">You can select the members individually, or click **Select All** to choose all members.</span></span> <span data-ttu-id="91969-360">När du har valt medlemmar klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91969-360">After you choose the members, click **Next**.</span></span>

    ![Ange onlineskyddsdata](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

10. <span data-ttu-id="91969-362">På den **Ange schema för onlinesäkerhetskopiering** Ange schema för att skapa återställningspunkter från säkerhetskopian på disk.</span><span class="sxs-lookup"><span data-stu-id="91969-362">On the **Specify Online Backup Schedule** page, specify the schedule to generate recovery points from the disk backup.</span></span> <span data-ttu-id="91969-363">När återställningspunkten har skapats, överförs de till i Azure Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="91969-363">After the recovery point is generated, it is transferred to the Recovery Services vault in Azure.</span></span> <span data-ttu-id="91969-364">När du är nöjd med onlinesäkerhetskopieringsschemat klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91969-364">When you are satisfied with the online backup schedule, click **Next**.</span></span>

    ![Ange onlinesäkerhetskopieringsschema](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

11. <span data-ttu-id="91969-366">På den **ange bevarandeprincip** kan ange hur länge du vill behålla säkerhetskopierade data i Azure.</span><span class="sxs-lookup"><span data-stu-id="91969-366">On the **Specify Online Retention Policy** page, indicate how long you want to retain the backup data in Azure.</span></span> <span data-ttu-id="91969-367">När principen har definierats, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="91969-367">After the policy is defined, click **Next**.</span></span>

    ![Ange bevarandeprincip](./media/backup-azure-backup-server-vmware/retention-policy.png)

    <span data-ttu-id="91969-369">Det finns ingen tidsbegränsning för hur länge du kan behålla data i Azure.</span><span class="sxs-lookup"><span data-stu-id="91969-369">There is no time limit for how long you can keep data in Azure.</span></span> <span data-ttu-id="91969-370">När du lagrar återställningsdata punkt i Azure kan är endast gränsen att du inte kan ha fler än 9999 återställningspunkter per skyddad instans.</span><span class="sxs-lookup"><span data-stu-id="91969-370">When you store recovery point data in Azure, the only limit is that you cannot have more than 9999 recovery points per protected instance.</span></span> <span data-ttu-id="91969-371">I det här exemplet är skyddade instansen VMware-servern.</span><span class="sxs-lookup"><span data-stu-id="91969-371">In this example, the protected instance is the VMware server.</span></span>

12. <span data-ttu-id="91969-372">På den **sammanfattning** , granska informationen för skyddsgruppmedlemmar och inställningar, och klickar sedan på **Skapa grupp**.</span><span class="sxs-lookup"><span data-stu-id="91969-372">On the **Summary** page, review the details for your protection group members and settings, and then click **Create Group**.</span></span>

    ![Skyddsgruppsmedlem och inställningen sammanfattning](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="next-steps"></a><span data-ttu-id="91969-374">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91969-374">Next steps</span></span>
<span data-ttu-id="91969-375">Om du använder Azure Backup Server för att skydda VMware arbetsbelastningar, du kan vara intresserad av att använda Azure Backup Server för att skydda en [Microsoft Exchange server](./backup-azure-exchange-mabs.md), [Microsoft SharePoint-servergruppen](./backup-azure-backup-sharepoint-mabs.md), eller en [SQL Server-databas](./backup-azure-sql-mabs.md).</span><span class="sxs-lookup"><span data-stu-id="91969-375">If you use Azure Backup Server to protect VMware workloads, you may be interested in using Azure Backup Server to help protect a [Microsoft Exchange server](./backup-azure-exchange-mabs.md), a [Microsoft SharePoint farm](./backup-azure-backup-sharepoint-mabs.md), or a [SQL Server database](./backup-azure-sql-mabs.md).</span></span>

<span data-ttu-id="91969-376">Mer information om problem med att registrera agenten konfigurera skyddsgruppen eller för att säkerhetskopiera jobb finns [felsöka Azure Backup Server](./backup-azure-mabs-troubleshoot.md).</span><span class="sxs-lookup"><span data-stu-id="91969-376">For information on problems with registering the agent, configuring the protection group, or backing up jobs, see [Troubleshoot Azure Backup Server](./backup-azure-mabs-troubleshoot.md).</span></span>
