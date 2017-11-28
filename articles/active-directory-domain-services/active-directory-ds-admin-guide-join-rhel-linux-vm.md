---
title: "Azure Active Directory Domain Services: Anslut till en hanterad domän för RHEL VM-tooa | Microsoft Docs"
description: "Ansluta till en virtuell dator för Red Hat Enterprise Linux tooAzure AD DS"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 87291c47-1280-43f8-8fb2-da1bd61a4942
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 41ca2aaf2eefbf9c403d2b834d61a1aa0943d950
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-tooa-managed-domain"></a><span data-ttu-id="80a7e-103">Anslut till en hanterad domän för Red Hat Enterprise Linux 7 virtuella tooa</span><span class="sxs-lookup"><span data-stu-id="80a7e-103">Join a Red Hat Enterprise Linux 7 virtual machine tooa managed domain</span></span>
<span data-ttu-id="80a7e-104">Den här artikeln visar hur toojoin en Azure AD Domain Services med Red Hat Enterprise Linux (RHEL) 7 virtuella tooan hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="80a7e-104">This article shows you how toojoin a Red Hat Enterprise Linux (RHEL) 7 virtual machine tooan Azure AD Domain Services managed domain.</span></span>

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a><span data-ttu-id="80a7e-105">Etablera en virtuell dator med Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="80a7e-105">Provision a Red Hat Enterprise Linux virtual machine</span></span>
<span data-ttu-id="80a7e-106">Utföra hello följande steg tooprovision en RHEL 7 virtuell dator med hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="80a7e-106">Perform hello following steps tooprovision a RHEL 7 virtual machine using hello Azure portal.</span></span>

1. <span data-ttu-id="80a7e-107">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="80a7e-107">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

    ![Azure portalens instrumentpanel](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)
2. <span data-ttu-id="80a7e-109">Klicka på **ny** på hello vänster fönsterruta och typen **Red Hat** i sökfältet hello som visas i följande skärmbild hello.</span><span class="sxs-lookup"><span data-stu-id="80a7e-109">Click **New** on hello left pane and type **Red Hat** into hello search bar as shown in hello following screenshot.</span></span> <span data-ttu-id="80a7e-110">Poster för Red Hat Enterprise Linux visas i sökresultaten hello.</span><span class="sxs-lookup"><span data-stu-id="80a7e-110">Entries for Red Hat Enterprise Linux appear in hello search results.</span></span> <span data-ttu-id="80a7e-111">Klicka på **Red Hat Enterprise Linux 7.2**.</span><span class="sxs-lookup"><span data-stu-id="80a7e-111">Click **Red Hat Enterprise Linux 7.2**.</span></span>

    ![Välj RHEL i resultaten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)
3. <span data-ttu-id="80a7e-113">hello sökresultat i hello **allt** rutan bör innehålla hello Red Hat Enterprise Linux 7.2 avbildningen.</span><span class="sxs-lookup"><span data-stu-id="80a7e-113">hello search results in hello **Everything** pane should list hello Red Hat Enterprise Linux 7.2 image.</span></span> <span data-ttu-id="80a7e-114">Klicka på **Red Hat Enterprise Linux 7.2** tooview mer information om hello avbildning av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="80a7e-114">Click **Red Hat Enterprise Linux 7.2** tooview more information about hello virtual machine image.</span></span>

    ![Välj RHEL i resultaten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)
4. <span data-ttu-id="80a7e-116">I hello **Red Hat Enterprise Linux 7.2** rutan du bör se mer information om hello avbildning av virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="80a7e-116">In hello **Red Hat Enterprise Linux 7.2** pane, you should see more information about hello virtual machine image.</span></span> <span data-ttu-id="80a7e-117">I hello **Välj en distributionsmodell** listrutan, Välj **klassiska**.</span><span class="sxs-lookup"><span data-stu-id="80a7e-117">In hello **Select a deployment model** dropdown, select **Classic**.</span></span> <span data-ttu-id="80a7e-118">Klicka på hello **skapa** knappen.</span><span class="sxs-lookup"><span data-stu-id="80a7e-118">Then click hello **Create** button.</span></span>

    ![Visa information om bild](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)
5. <span data-ttu-id="80a7e-120">I hello **grunderna** sidan hello **Skapa virtuell dator** guiden ange hello **värdnamn** för hello nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="80a7e-120">In hello **Basics** page of hello **Create virtual machine** wizard, enter hello **Host Name** for hello new virtual machine.</span></span> <span data-ttu-id="80a7e-121">Ange ett användarnamn för lokal administratör också i hello **användarnamn** fältet och en **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="80a7e-121">Also specify a local administrator user name in hello **User name** field and a **Password**.</span></span> <span data-ttu-id="80a7e-122">Du kan också välja toouse SSH key tooauthenticate hello lokala administratörsanvändare.</span><span class="sxs-lookup"><span data-stu-id="80a7e-122">You may also choose toouse an SSH key tooauthenticate hello local administrator user.</span></span> <span data-ttu-id="80a7e-123">Också välja en **prisnivån** för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="80a7e-123">Also select a **Pricing Tier** for hello virtual machine.</span></span>

    ![Skapa VM - grunderna sida](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)
6. <span data-ttu-id="80a7e-125">I hello **storlek** sidan hello **Skapa virtuell dator** guiden, Välj hello storlek för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="80a7e-125">In hello **Size** page of hello **Create virtual machine** wizard, select hello size for hello virtual machine.</span></span>

    ![Skapa VM - väljer storlek](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-vm-size.png)

7. <span data-ttu-id="80a7e-127">I hello **inställningar** sidan hello **Skapa virtuell dator** guiden, Välj hello storage-konto för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="80a7e-127">In hello **Settings** page of hello **Create virtual machine** wizard, select hello storage account for hello virtual machine.</span></span> <span data-ttu-id="80a7e-128">Klicka på **virtuellt nätverk** tooselect hello virtuellt nätverk toowhich hello Linux VM ska distribueras.</span><span class="sxs-lookup"><span data-stu-id="80a7e-128">Click **Virtual Network** tooselect hello virtual network toowhich hello Linux VM should be deployed.</span></span> <span data-ttu-id="80a7e-129">I hello **virtuellt nätverk** bladet, Välj hello virtuellt nätverk där Azure AD Domain Services är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="80a7e-129">In hello **Virtual Network** blade, select hello virtual network in which Azure AD Domain Services is available.</span></span> <span data-ttu-id="80a7e-130">I det här exemplet väljer vi hello 'MyPreviewVNet' virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="80a7e-130">In this example, we pick hello 'MyPreviewVNet' virtual network.</span></span>

    ![Skapa VM - väljer virtuellt nätverk](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)
8. <span data-ttu-id="80a7e-132">På hello **sammanfattning** sidan hello **Skapa virtuell dator** guiden, granska och klickar på hello **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="80a7e-132">On hello **Summary** page of hello **Create virtual machine** wizard, review and click hello **OK** button.</span></span>

    ![Skapa VM - virtuella nätverk som har valts](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)
9. <span data-ttu-id="80a7e-134">Distribution av hello nya virtuella datorn baserat på hello RHEL 7.2 avbildningen ska starta.</span><span class="sxs-lookup"><span data-stu-id="80a7e-134">Deployment of hello new virtual machine based on hello RHEL 7.2 image should start.</span></span>

    ![Skapa VM - distributionen har startat](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)
10. <span data-ttu-id="80a7e-136">Efter några minuter ska hello virtuella datorn har distribuerats och redo för användning.</span><span class="sxs-lookup"><span data-stu-id="80a7e-136">After a few minutes, hello virtual machine should be deployed successfully and ready for use.</span></span>

    ![Skapa VM - distribueras](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)

## <a name="connect-remotely-toohello-newly-provisioned-linux-virtual-machine"></a><span data-ttu-id="80a7e-138">Fjärransluta toohello nyligen etablerats virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="80a7e-138">Connect remotely toohello newly provisioned Linux virtual machine</span></span>
<span data-ttu-id="80a7e-139">hello RHEL 7.2 virtuella datorn har etablerats i Azure.</span><span class="sxs-lookup"><span data-stu-id="80a7e-139">hello RHEL 7.2 virtual machine has been provisioned in Azure.</span></span> <span data-ttu-id="80a7e-140">hello nästa uppgift är tooconnect via fjärranslutning toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="80a7e-140">hello next task is tooconnect remotely toohello virtual machine.</span></span>

<span data-ttu-id="80a7e-141">**Ansluta toohello RHEL 7.2 virtuella** följer du anvisningarna hello i hello artikel [hur toolog på tooa virtuell dator som kör Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="80a7e-141">**Connect toohello RHEL 7.2 virtual machine** Follow hello instructions in hello article [How toolog on tooa virtual machine running Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="80a7e-142">hello resten av hello steg förutsätter att du använder hello PuTTY SSH tooconnect toohello RHEL virtuella klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="80a7e-142">hello rest of hello steps assume you use hello PuTTY SSH client tooconnect toohello RHEL virtual machine.</span></span> <span data-ttu-id="80a7e-143">Mer information finns i hello [PuTTY-hämtningssida](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="80a7e-143">For more information, see hello [PuTTY Download page](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

1. <span data-ttu-id="80a7e-144">Öppna hello PuTTY program.</span><span class="sxs-lookup"><span data-stu-id="80a7e-144">Open hello PuTTY program.</span></span>
2. <span data-ttu-id="80a7e-145">Ange hello **värdnamn** för hello nyskapad RHEL virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="80a7e-145">Enter hello **Host Name** for hello newly created RHEL virtual machine.</span></span> <span data-ttu-id="80a7e-146">I det här exemplet har vår virtuella värdnamn för hello ”contoso-rhel.cloudapp .net'.</span><span class="sxs-lookup"><span data-stu-id="80a7e-146">In this example, our virtual machine has hello host name 'contoso-rhel.cloudapp.net'.</span></span> <span data-ttu-id="80a7e-147">Om du inte är säker på hello värdnamnet för den virtuella datorn läser du toohello VM instrumentpanelen på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="80a7e-147">If you are not sure of hello host name of your VM, refer toohello VM dashboard on hello Azure portal.</span></span>

    ![PuTTY ansluta](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)
3. <span data-ttu-id="80a7e-149">Logga in toohello virtuell dator med hello lokal administratörsautentiseringsuppgifter du angav när hello virtuella datorn skapades.</span><span class="sxs-lookup"><span data-stu-id="80a7e-149">Log on toohello virtual machine using hello local administrator credentials you specified when hello virtual machine was created.</span></span> <span data-ttu-id="80a7e-150">I det här exemplet användes hello lokalt administratörskonto ”mahesh”.</span><span class="sxs-lookup"><span data-stu-id="80a7e-150">In this example, we used hello local administrator account "mahesh".</span></span>

    ![PuTTY-inloggning](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)

## <a name="install-required-packages-on-hello-linux-virtual-machine"></a><span data-ttu-id="80a7e-152">Installera nödvändiga paket på hello virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="80a7e-152">Install required packages on hello Linux virtual machine</span></span>
<span data-ttu-id="80a7e-153">Efter anslutning toohello virtuella är hello nästa uppgift tooinstall paket som krävs för domänanslutning på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="80a7e-153">After connecting toohello virtual machine, hello next task is tooinstall packages required for domain join on hello virtual machine.</span></span> <span data-ttu-id="80a7e-154">Utför följande steg hello:</span><span class="sxs-lookup"><span data-stu-id="80a7e-154">Perform hello following steps:</span></span>

1. <span data-ttu-id="80a7e-155">**Installera realmd:** hello realmd paketet används för domänanslutning.</span><span class="sxs-lookup"><span data-stu-id="80a7e-155">**Install realmd:** hello realmd package is used for domain join.</span></span> <span data-ttu-id="80a7e-156">Skriv följande kommando hello i PuTTY terminalen:</span><span class="sxs-lookup"><span data-stu-id="80a7e-156">In your PuTTY terminal, type hello following command:</span></span>

    <span data-ttu-id="80a7e-157">sudo yum installera realmd</span><span class="sxs-lookup"><span data-stu-id="80a7e-157">sudo yum install realmd</span></span>

    ![Installera realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    <span data-ttu-id="80a7e-159">Efter några minuter bör hello realmd paketet installeras på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="80a7e-159">After a few minutes, hello realmd package should get installed on hello virtual machine.</span></span>

    ![realmd installerad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)
2. <span data-ttu-id="80a7e-161">**Installera sssd:** hello realmd paket som beror på sssd tooperform domän join-operationer.</span><span class="sxs-lookup"><span data-stu-id="80a7e-161">**Install sssd:** hello realmd package depends on sssd tooperform domain join operations.</span></span> <span data-ttu-id="80a7e-162">Skriv följande kommando hello i PuTTY terminalen:</span><span class="sxs-lookup"><span data-stu-id="80a7e-162">In your PuTTY terminal, type hello following command:</span></span>

    <span data-ttu-id="80a7e-163">sudo yum installera sssd</span><span class="sxs-lookup"><span data-stu-id="80a7e-163">sudo yum install sssd</span></span>

    ![Installera sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    <span data-ttu-id="80a7e-165">Efter några minuter bör hello sssd paketet installeras på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="80a7e-165">After a few minutes, hello sssd package should get installed on hello virtual machine.</span></span>

    ![realmd installerad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)
3. <span data-ttu-id="80a7e-167">**Installera kerberos:** i terminalen PuTTY skriver du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="80a7e-167">**Install kerberos:** In your PuTTY terminal, type hello following command:</span></span>

    <span data-ttu-id="80a7e-168">sudo yum installera krb5 arbetsstation krb5-bibliotek</span><span class="sxs-lookup"><span data-stu-id="80a7e-168">sudo yum install krb5-workstation krb5-libs</span></span>

    ![Installera kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    <span data-ttu-id="80a7e-170">Efter några minuter bör hello realmd paketet installeras på hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="80a7e-170">After a few minutes, hello realmd package should get installed on hello virtual machine.</span></span>

    ![Kerberos installerad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)

## <a name="join-hello-linux-virtual-machine-toohello-managed-domain"></a><span data-ttu-id="80a7e-172">Anslut till hello Linux virtuella toohello hanterad domän</span><span class="sxs-lookup"><span data-stu-id="80a7e-172">Join hello Linux virtual machine toohello managed domain</span></span>
<span data-ttu-id="80a7e-173">Nu när hello krävs paket som är installerade på hello virtuell Linux-dator, är hello nästa uppgift toojoin hello virtuella toohello hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="80a7e-173">Now that hello required packages are installed on hello Linux virtual machine, hello next task is toojoin hello virtual machine toohello managed domain.</span></span>

1. <span data-ttu-id="80a7e-174">Identifiera hello AAD Domain Services-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="80a7e-174">Discover hello AAD Domain Services managed domain.</span></span> <span data-ttu-id="80a7e-175">Skriv följande kommando hello i PuTTY terminalen:</span><span class="sxs-lookup"><span data-stu-id="80a7e-175">In your PuTTY terminal, type hello following command:</span></span>

    <span data-ttu-id="80a7e-176">sudo sfär identifiera CONTOSO100.COM</span><span class="sxs-lookup"><span data-stu-id="80a7e-176">sudo realm discover CONTOSO100.COM</span></span>

    ![Identifiera sfär](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    <span data-ttu-id="80a7e-178">Om **sfär identifiera** är toofind din hanterade domän Kontrollera hello domänen kan nås från hello virtuell dator (försök ping).</span><span class="sxs-lookup"><span data-stu-id="80a7e-178">If **realm discover** is unable toofind your managed domain, ensure that hello domain is reachable from hello virtual machine (try ping).</span></span> <span data-ttu-id="80a7e-179">Kontrollera också att hello den virtuella datorn har faktiskt distribuerade toohello samma virtuella nätverk i vilka hello hanterade domänen är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="80a7e-179">Also ensure that hello virtual machine has indeed been deployed toohello same virtual network in which hello managed domain is available.</span></span>
2. <span data-ttu-id="80a7e-180">Initiera kerberos.</span><span class="sxs-lookup"><span data-stu-id="80a7e-180">Initialize kerberos.</span></span> <span data-ttu-id="80a7e-181">Skriv följande kommando hello i PuTTY terminalen.</span><span class="sxs-lookup"><span data-stu-id="80a7e-181">In your PuTTY terminal, type hello following command.</span></span> <span data-ttu-id="80a7e-182">Kontrollera att du anger en användare som tillhör toohello ' AAD DC-administratörsgruppen.</span><span class="sxs-lookup"><span data-stu-id="80a7e-182">Ensure that you specify a user who belongs toohello 'AAD DC Administrators' group.</span></span> <span data-ttu-id="80a7e-183">Endast dessa användare kan ansluta till datorer toohello hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="80a7e-183">Only these users can join computers toohello managed domain.</span></span>

    <span data-ttu-id="80a7e-184">kinitbob@CONTOSO100.COM</span><span class="sxs-lookup"><span data-stu-id="80a7e-184">kinit bob@CONTOSO100.COM</span></span>

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    <span data-ttu-id="80a7e-186">Se till att du anger hello domännamn i versaler annan kinit misslyckas.</span><span class="sxs-lookup"><span data-stu-id="80a7e-186">Ensure that you specify hello domain name in capital letters, else kinit fails.</span></span>
3. <span data-ttu-id="80a7e-187">Ansluta till hello datorn toohello domän.</span><span class="sxs-lookup"><span data-stu-id="80a7e-187">Join hello machine toohello domain.</span></span> <span data-ttu-id="80a7e-188">Skriv följande kommando hello i PuTTY terminalen.</span><span class="sxs-lookup"><span data-stu-id="80a7e-188">In your PuTTY terminal, type hello following command.</span></span> <span data-ttu-id="80a7e-189">Ange hello samma användare som du angav i föregående steg (kinit) hello.</span><span class="sxs-lookup"><span data-stu-id="80a7e-189">Specify hello same user you specified in hello preceding step ('kinit').</span></span>

    <span data-ttu-id="80a7e-190">sudo sfär koppling--utförlig CONTOSO100.COM -U 'bob@CONTOSO100.COM'</span><span class="sxs-lookup"><span data-stu-id="80a7e-190">sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'</span></span>

    ![Anslut till domän](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

<span data-ttu-id="80a7e-192">Du bör få ett meddelande (”har registrerad dator i sfär”) när hello datorn är har anslutits toohello hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="80a7e-192">You should get a message ("Successfully enrolled machine in realm") when hello machine is successfully joined toohello managed domain.</span></span>

## <a name="verify-domain-join"></a><span data-ttu-id="80a7e-193">Kontrollera domänanslutning</span><span class="sxs-lookup"><span data-stu-id="80a7e-193">Verify domain join</span></span>
<span data-ttu-id="80a7e-194">Du kan snabbt kontrollera om har hello datorn har anslutits toohello hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="80a7e-194">You can quickly verify whether hello machine has been successfully joined toohello managed domain.</span></span> <span data-ttu-id="80a7e-195">Ansluta toohello nyligen domänanslutna RHEL VM som använder SSH och ett domänanvändarkonto och sedan kontrollera toosee om hello-användarkontot är borta på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="80a7e-195">Connect toohello newly domain joined RHEL VM using SSH and a domain user account and then check toosee if hello user account is resolved correctly.</span></span>

1. <span data-ttu-id="80a7e-196">Din PuTTY terminal och typen hello efter kommandot tooconnect toohello nyligen domänanslutna RHEL virtuell dator med SSH.</span><span class="sxs-lookup"><span data-stu-id="80a7e-196">In your PuTTY terminal, type hello following command tooconnect toohello newly domain joined RHEL virtual machine using SSH.</span></span> <span data-ttu-id="80a7e-197">Använda ett domänkonto som tillhör toohello hanterad domän (till exempel 'bob@CONTOSO100.COM' i det här fallet.)</span><span class="sxs-lookup"><span data-stu-id="80a7e-197">Use a domain account that belongs toohello managed domain (for example, 'bob@CONTOSO100.COM' in this case.)</span></span>

    <span data-ttu-id="80a7e-198">SSH -l bob@CONTOSO100.COM contoso rhel.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="80a7e-198">ssh -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net</span></span>
2. <span data-ttu-id="80a7e-199">Ange hello efter kommandot toosee om hello arbetskatalog har initierats korrekt i terminalen PuTTY.</span><span class="sxs-lookup"><span data-stu-id="80a7e-199">In your PuTTY terminal, type hello following command toosee if hello home directory was initialized correctly.</span></span>

    <span data-ttu-id="80a7e-200">pwd</span><span class="sxs-lookup"><span data-stu-id="80a7e-200">pwd</span></span>
3. <span data-ttu-id="80a7e-201">Ange hello efter kommandot toosee om hello gruppmedlemskap som matchas korrekt i terminalen PuTTY.</span><span class="sxs-lookup"><span data-stu-id="80a7e-201">In your PuTTY terminal, type hello following command toosee if hello group memberships are being resolved correctly.</span></span>

    <span data-ttu-id="80a7e-202">id</span><span class="sxs-lookup"><span data-stu-id="80a7e-202">id</span></span>

<span data-ttu-id="80a7e-203">Ett exempel på utdata från de här kommandona visas nedan:</span><span class="sxs-lookup"><span data-stu-id="80a7e-203">A sample output of these commands follows:</span></span>

![Kontrollera domänanslutning](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)

## <a name="troubleshooting-domain-join"></a><span data-ttu-id="80a7e-205">Felsöka domänanslutning</span><span class="sxs-lookup"><span data-stu-id="80a7e-205">Troubleshooting domain join</span></span>
<span data-ttu-id="80a7e-206">Se toohello [felsökning domänanslutning](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) artikel.</span><span class="sxs-lookup"><span data-stu-id="80a7e-206">Refer toohello [Troubleshooting domain join](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) article.</span></span>

## <a name="related-content"></a><span data-ttu-id="80a7e-207">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="80a7e-207">Related Content</span></span>
* [<span data-ttu-id="80a7e-208">Azure AD Domain Services - komma igång-guide</span><span class="sxs-lookup"><span data-stu-id="80a7e-208">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="80a7e-209">Ansluta till en Windows Server virtuella tooan Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="80a7e-209">Join a Windows Server virtual machine tooan Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* <span data-ttu-id="80a7e-210">[Hur toolog på tooa virtuell dator som kör Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="80a7e-210">[How toolog on tooa virtual machine running Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* [<span data-ttu-id="80a7e-211">Installera Kerberos</span><span class="sxs-lookup"><span data-stu-id="80a7e-211">Installing Kerberos</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [<span data-ttu-id="80a7e-212">Red Hat Enterprise Linux 7 - Guide för Windows-integrering</span><span class="sxs-lookup"><span data-stu-id="80a7e-212">Red Hat Enterprise Linux 7 - Windows Integration Guide</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
