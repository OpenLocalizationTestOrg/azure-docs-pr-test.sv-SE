---
title: "Azure Active Directory Domain Services: Anslut en RHEL VM till en hanterad domän | Microsoft Docs"
description: Anslut en virtuell Red Hat Enterprise Linux-dator till Azure AD Domain Services
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
ms.openlocfilehash: 69f1850bfed90392e9a4695e2443ffaa6bfc746d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a><span data-ttu-id="51ad4-103">Ansluta en Red Hat Enterprise Linux 7-virtuell dator till en hanterad domän</span><span class="sxs-lookup"><span data-stu-id="51ad4-103">Join a Red Hat Enterprise Linux 7 virtual machine to a managed domain</span></span>
<span data-ttu-id="51ad4-104">Den här artikeln visar hur du ansluter till en virtuell dator för Red Hat Enterprise Linux (RHEL) 7 till en hanterad Azure AD DS-domän.</span><span class="sxs-lookup"><span data-stu-id="51ad4-104">This article shows you how to join a Red Hat Enterprise Linux (RHEL) 7 virtual machine to an Azure AD Domain Services managed domain.</span></span>

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a><span data-ttu-id="51ad4-105">Etablera en virtuell dator med Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="51ad4-105">Provision a Red Hat Enterprise Linux virtual machine</span></span>
<span data-ttu-id="51ad4-106">Utför följande steg för att etablera en virtuell dator för RHEL 7 som använder Azure portal.</span><span class="sxs-lookup"><span data-stu-id="51ad4-106">Perform the following steps to provision a RHEL 7 virtual machine using the Azure portal.</span></span>

1. <span data-ttu-id="51ad4-107">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="51ad4-107">Sign in to the [Azure portal](https://portal.azure.com).</span></span>

    ![Azure portalens instrumentpanel](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)
2. <span data-ttu-id="51ad4-109">Klicka på **ny** på den vänstra rutan och skriv **Red Hat** i sökfältet som visas i följande skärmbild.</span><span class="sxs-lookup"><span data-stu-id="51ad4-109">Click **New** on the left pane and type **Red Hat** into the search bar as shown in the following screenshot.</span></span> <span data-ttu-id="51ad4-110">Poster för Red Hat Enterprise Linux visas i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="51ad4-110">Entries for Red Hat Enterprise Linux appear in the search results.</span></span> <span data-ttu-id="51ad4-111">Klicka på **Red Hat Enterprise Linux 7.2**.</span><span class="sxs-lookup"><span data-stu-id="51ad4-111">Click **Red Hat Enterprise Linux 7.2**.</span></span>

    ![Välj RHEL i resultaten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)
3. <span data-ttu-id="51ad4-113">Sökresultat i den **allt** fönstret ska visa en lista med Red Hat Enterprise Linux 7.2 avbildningen.</span><span class="sxs-lookup"><span data-stu-id="51ad4-113">The search results in the **Everything** pane should list the Red Hat Enterprise Linux 7.2 image.</span></span> <span data-ttu-id="51ad4-114">Klicka på **Red Hat Enterprise Linux 7.2** att visa mer information om den virtuella datoravbildningen.</span><span class="sxs-lookup"><span data-stu-id="51ad4-114">Click **Red Hat Enterprise Linux 7.2** to view more information about the virtual machine image.</span></span>

    ![Välj RHEL i resultaten](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)
4. <span data-ttu-id="51ad4-116">I den **Red Hat Enterprise Linux 7.2** rutan du bör se mer information om den virtuella datoravbildningen.</span><span class="sxs-lookup"><span data-stu-id="51ad4-116">In the **Red Hat Enterprise Linux 7.2** pane, you should see more information about the virtual machine image.</span></span> <span data-ttu-id="51ad4-117">I den **Välj en distributionsmodell** listrutan, Välj **klassiska**.</span><span class="sxs-lookup"><span data-stu-id="51ad4-117">In the **Select a deployment model** dropdown, select **Classic**.</span></span> <span data-ttu-id="51ad4-118">Klicka på den **skapa** knappen.</span><span class="sxs-lookup"><span data-stu-id="51ad4-118">Then click the **Create** button.</span></span>

    ![Visa information om bild](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)
5. <span data-ttu-id="51ad4-120">I den **grunderna** sida av den **Skapa virtuell dator** guiden ange de **värdnamn** för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="51ad4-120">In the **Basics** page of the **Create virtual machine** wizard, enter the **Host Name** for the new virtual machine.</span></span> <span data-ttu-id="51ad4-121">Också ange ett användarnamn för lokal administratör i den **användarnamn** fältet och en **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="51ad4-121">Also specify a local administrator user name in the **User name** field and a **Password**.</span></span> <span data-ttu-id="51ad4-122">Du kan också välja att använda en SSH-nyckel för att autentisera användaren för lokal administratör.</span><span class="sxs-lookup"><span data-stu-id="51ad4-122">You may also choose to use an SSH key to authenticate the local administrator user.</span></span> <span data-ttu-id="51ad4-123">Också välja en **prisnivån** för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="51ad4-123">Also select a **Pricing Tier** for the virtual machine.</span></span>

    ![Skapa VM - grunderna sida](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)
6. <span data-ttu-id="51ad4-125">I den **storlek** sida av den **Skapa virtuell dator** guiden, Välj storlek för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="51ad4-125">In the **Size** page of the **Create virtual machine** wizard, select the size for the virtual machine.</span></span>

    ![Skapa VM - väljer storlek](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-vm-size.png)

7. <span data-ttu-id="51ad4-127">I den **inställningar** sida av den **Skapa virtuell dator** guiden, Välj lagring kontot för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="51ad4-127">In the **Settings** page of the **Create virtual machine** wizard, select the storage account for the virtual machine.</span></span> <span data-ttu-id="51ad4-128">Klicka på **virtuellt nätverk** att välja det virtuella nätverket som Linux VM ska distribueras.</span><span class="sxs-lookup"><span data-stu-id="51ad4-128">Click **Virtual Network** to select the virtual network to which the Linux VM should be deployed.</span></span> <span data-ttu-id="51ad4-129">I den **virtuellt nätverk** bladet väljer virtuella nätverk där Azure AD Domain Services är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="51ad4-129">In the **Virtual Network** blade, select the virtual network in which Azure AD Domain Services is available.</span></span> <span data-ttu-id="51ad4-130">I det här exemplet väljer vi det virtuella nätverket 'MyPreviewVNet'.</span><span class="sxs-lookup"><span data-stu-id="51ad4-130">In this example, we pick the 'MyPreviewVNet' virtual network.</span></span>

    ![Skapa VM - väljer virtuellt nätverk](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)
8. <span data-ttu-id="51ad4-132">På den **sammanfattning** sida av den **Skapa virtuell dator** guiden, granska och klickar på den **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="51ad4-132">On the **Summary** page of the **Create virtual machine** wizard, review and click the **OK** button.</span></span>

    ![Skapa VM - virtuella nätverk som har valts](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)
9. <span data-ttu-id="51ad4-134">Distribution av den nya virtuella datorn baserat på den RHEL 7.2 avbildningen ska starta.</span><span class="sxs-lookup"><span data-stu-id="51ad4-134">Deployment of the new virtual machine based on the RHEL 7.2 image should start.</span></span>

    ![Skapa VM - distributionen har startat](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)
10. <span data-ttu-id="51ad4-136">Efter några minuter, ska den virtuella datorn distribueras har och redo för användning.</span><span class="sxs-lookup"><span data-stu-id="51ad4-136">After a few minutes, the virtual machine should be deployed successfully and ready for use.</span></span>

    ![Skapa VM - distribueras](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)

## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a><span data-ttu-id="51ad4-138">Fjärransluta till den nyetablerade virtuella Linux-datorn</span><span class="sxs-lookup"><span data-stu-id="51ad4-138">Connect remotely to the newly provisioned Linux virtual machine</span></span>
<span data-ttu-id="51ad4-139">RHEL 7.2 virtuella datorn har etablerats i Azure.</span><span class="sxs-lookup"><span data-stu-id="51ad4-139">The RHEL 7.2 virtual machine has been provisioned in Azure.</span></span> <span data-ttu-id="51ad4-140">Nästa uppgift är att fjärransluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="51ad4-140">The next task is to connect remotely to the virtual machine.</span></span>

<span data-ttu-id="51ad4-141">**Ansluta till den virtuella datorn RHEL 7.2** följer du anvisningarna i artikeln [så att logga in på en virtuell dator som kör Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="51ad4-141">**Connect to the RHEL 7.2 virtual machine** Follow the instructions in the article [How to log on to a virtual machine running Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="51ad4-142">Resten av stegen förutsätter att du använder PuTTY SSH-klienten för att ansluta till den virtuella datorn RHEL.</span><span class="sxs-lookup"><span data-stu-id="51ad4-142">The rest of the steps assume you use the PuTTY SSH client to connect to the RHEL virtual machine.</span></span> <span data-ttu-id="51ad4-143">Mer information finns i [PuTTY-hämtningssida](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="51ad4-143">For more information, see the [PuTTY Download page](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

1. <span data-ttu-id="51ad4-144">Öppna PuTTY-programmet.</span><span class="sxs-lookup"><span data-stu-id="51ad4-144">Open the PuTTY program.</span></span>
2. <span data-ttu-id="51ad4-145">Ange den **värdnamn** för den nyligen skapade RHEL virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="51ad4-145">Enter the **Host Name** for the newly created RHEL virtual machine.</span></span> <span data-ttu-id="51ad4-146">I det här exemplet har vår virtuella värdnamn ”contoso-rhel.cloudapp .net'.</span><span class="sxs-lookup"><span data-stu-id="51ad4-146">In this example, our virtual machine has the host name 'contoso-rhel.cloudapp.net'.</span></span> <span data-ttu-id="51ad4-147">Om du inte är säker på namnet på den virtuella datorn finns på VM-instrumentpanelen på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="51ad4-147">If you are not sure of the host name of your VM, refer to the VM dashboard on the Azure portal.</span></span>

    ![PuTTY ansluta](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)
3. <span data-ttu-id="51ad4-149">Logga in på den virtuella datorn med lokal administratörsbehörighet som du angav när den virtuella datorn har skapats.</span><span class="sxs-lookup"><span data-stu-id="51ad4-149">Log on to the virtual machine using the local administrator credentials you specified when the virtual machine was created.</span></span> <span data-ttu-id="51ad4-150">I det här exemplet används vi det lokala administratörskontot ”mahesh”.</span><span class="sxs-lookup"><span data-stu-id="51ad4-150">In this example, we used the local administrator account "mahesh".</span></span>

    ![PuTTY-inloggning](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)

## <a name="install-required-packages-on-the-linux-virtual-machine"></a><span data-ttu-id="51ad4-152">Installera nödvändiga paket på den virtuella Linux-datorn</span><span class="sxs-lookup"><span data-stu-id="51ad4-152">Install required packages on the Linux virtual machine</span></span>
<span data-ttu-id="51ad4-153">När du har anslutit till den virtuella datorn, är nästa uppgift att installera paket som krävs för domänanslutning på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="51ad4-153">After connecting to the virtual machine, the next task is to install packages required for domain join on the virtual machine.</span></span> <span data-ttu-id="51ad4-154">Utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="51ad4-154">Perform the following steps:</span></span>

1. <span data-ttu-id="51ad4-155">**Installera realmd:** realmd paketet används för domänanslutning.</span><span class="sxs-lookup"><span data-stu-id="51ad4-155">**Install realmd:** The realmd package is used for domain join.</span></span> <span data-ttu-id="51ad4-156">Skriv följande kommando i PuTTY terminalen:</span><span class="sxs-lookup"><span data-stu-id="51ad4-156">In your PuTTY terminal, type the following command:</span></span>

    <span data-ttu-id="51ad4-157">sudo yum installera realmd</span><span class="sxs-lookup"><span data-stu-id="51ad4-157">sudo yum install realmd</span></span>

    ![Installera realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    <span data-ttu-id="51ad4-159">Efter några minuter ska realmd paketet installeras på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="51ad4-159">After a few minutes, the realmd package should get installed on the virtual machine.</span></span>

    ![realmd installerad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)
2. <span data-ttu-id="51ad4-161">**Installera sssd:** realmd paketet är beroende av sssd att utföra kopplingsåtgärder för domänen.</span><span class="sxs-lookup"><span data-stu-id="51ad4-161">**Install sssd:** The realmd package depends on sssd to perform domain join operations.</span></span> <span data-ttu-id="51ad4-162">Skriv följande kommando i PuTTY terminalen:</span><span class="sxs-lookup"><span data-stu-id="51ad4-162">In your PuTTY terminal, type the following command:</span></span>

    <span data-ttu-id="51ad4-163">sudo yum installera sssd</span><span class="sxs-lookup"><span data-stu-id="51ad4-163">sudo yum install sssd</span></span>

    ![Installera sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    <span data-ttu-id="51ad4-165">Efter några minuter ska sssd paketet installeras på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="51ad4-165">After a few minutes, the sssd package should get installed on the virtual machine.</span></span>

    ![realmd installerad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)
3. <span data-ttu-id="51ad4-167">**Installera kerberos:** i terminalen PuTTY skriver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="51ad4-167">**Install kerberos:** In your PuTTY terminal, type the following command:</span></span>

    <span data-ttu-id="51ad4-168">sudo yum installera krb5 arbetsstation krb5-bibliotek</span><span class="sxs-lookup"><span data-stu-id="51ad4-168">sudo yum install krb5-workstation krb5-libs</span></span>

    ![Installera kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    <span data-ttu-id="51ad4-170">Efter några minuter ska realmd paketet installeras på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="51ad4-170">After a few minutes, the realmd package should get installed on the virtual machine.</span></span>

    ![Kerberos installerad](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)

## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a><span data-ttu-id="51ad4-172">Anslut den virtuella Linux-datorn till den hanterade domänen</span><span class="sxs-lookup"><span data-stu-id="51ad4-172">Join the Linux virtual machine to the managed domain</span></span>
<span data-ttu-id="51ad4-173">De nödvändiga paketen är installerat på den virtuella Linux-datorn, är nästa uppgift att ansluta den virtuella datorn till den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="51ad4-173">Now that the required packages are installed on the Linux virtual machine, the next task is to join the virtual machine to the managed domain.</span></span>

1. <span data-ttu-id="51ad4-174">Identifiera den hanterade domänen AAD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="51ad4-174">Discover the AAD Domain Services managed domain.</span></span> <span data-ttu-id="51ad4-175">Skriv följande kommando i PuTTY terminalen:</span><span class="sxs-lookup"><span data-stu-id="51ad4-175">In your PuTTY terminal, type the following command:</span></span>

    <span data-ttu-id="51ad4-176">sudo sfär identifiera CONTOSO100.COM</span><span class="sxs-lookup"><span data-stu-id="51ad4-176">sudo realm discover CONTOSO100.COM</span></span>

    ![Identifiera sfär](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    <span data-ttu-id="51ad4-178">Om **sfär identifiera** gick inte att hitta din hanterade domän, se till att domänen kan nås från den virtuella datorn (försök ping).</span><span class="sxs-lookup"><span data-stu-id="51ad4-178">If **realm discover** is unable to find your managed domain, ensure that the domain is reachable from the virtual machine (try ping).</span></span> <span data-ttu-id="51ad4-179">Se också till att den virtuella datorn faktiskt har distribuerats till samma virtuella nätverk som den hanterade domänen är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="51ad4-179">Also ensure that the virtual machine has indeed been deployed to the same virtual network in which the managed domain is available.</span></span>
2. <span data-ttu-id="51ad4-180">Initiera kerberos.</span><span class="sxs-lookup"><span data-stu-id="51ad4-180">Initialize kerberos.</span></span> <span data-ttu-id="51ad4-181">Skriv följande kommando i terminalen PuTTY.</span><span class="sxs-lookup"><span data-stu-id="51ad4-181">In your PuTTY terminal, type the following command.</span></span> <span data-ttu-id="51ad4-182">Kontrollera att du anger en användare som tillhör gruppen AAD DC-administratörer.</span><span class="sxs-lookup"><span data-stu-id="51ad4-182">Ensure that you specify a user who belongs to the 'AAD DC Administrators' group.</span></span> <span data-ttu-id="51ad4-183">Endast dessa användare kan ansluta datorer till den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="51ad4-183">Only these users can join computers to the managed domain.</span></span>

    <span data-ttu-id="51ad4-184">kinitbob@CONTOSO100.COM</span><span class="sxs-lookup"><span data-stu-id="51ad4-184">kinit bob@CONTOSO100.COM</span></span>

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    <span data-ttu-id="51ad4-186">Se till att du anger domännamnet i versaler annan kinit misslyckas.</span><span class="sxs-lookup"><span data-stu-id="51ad4-186">Ensure that you specify the domain name in capital letters, else kinit fails.</span></span>
3. <span data-ttu-id="51ad4-187">Anslut datorn till domänen.</span><span class="sxs-lookup"><span data-stu-id="51ad4-187">Join the machine to the domain.</span></span> <span data-ttu-id="51ad4-188">Skriv följande kommando i terminalen PuTTY.</span><span class="sxs-lookup"><span data-stu-id="51ad4-188">In your PuTTY terminal, type the following command.</span></span> <span data-ttu-id="51ad4-189">Ange samma användare som du angav i föregående steg (kinit).</span><span class="sxs-lookup"><span data-stu-id="51ad4-189">Specify the same user you specified in the preceding step ('kinit').</span></span>

    <span data-ttu-id="51ad4-190">sudo sfär koppling--utförlig CONTOSO100.COM -U 'bob@CONTOSO100.COM'</span><span class="sxs-lookup"><span data-stu-id="51ad4-190">sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'</span></span>

    ![Anslut till domän](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

<span data-ttu-id="51ad4-192">Du bör få ett meddelande (”har registrerad dator i sfär”) när datorn har anslutit till den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="51ad4-192">You should get a message ("Successfully enrolled machine in realm") when the machine is successfully joined to the managed domain.</span></span>

## <a name="verify-domain-join"></a><span data-ttu-id="51ad4-193">Kontrollera domänanslutning</span><span class="sxs-lookup"><span data-stu-id="51ad4-193">Verify domain join</span></span>
<span data-ttu-id="51ad4-194">Du kan snabbt kontrollera om datorn har anslutit till den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="51ad4-194">You can quickly verify whether the machine has been successfully joined to the managed domain.</span></span> <span data-ttu-id="51ad4-195">Ansluta till den nya domänanslutna RHEL VM som använder SSH och ett domänanvändarkonto och kontrollera sedan för att se om användarkontot är borta på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="51ad4-195">Connect to the newly domain joined RHEL VM using SSH and a domain user account and then check to see if the user account is resolved correctly.</span></span>

1. <span data-ttu-id="51ad4-196">I terminalen PuTTY skriver du följande kommando för att ansluta till den nya domänanslutna RHEL virtuell dator med SSH.</span><span class="sxs-lookup"><span data-stu-id="51ad4-196">In your PuTTY terminal, type the following command to connect to the newly domain joined RHEL virtual machine using SSH.</span></span> <span data-ttu-id="51ad4-197">Använda ett domänkonto som tillhör den hanterade domänen (till exempel 'bob@CONTOSO100.COM' i det här fallet.)</span><span class="sxs-lookup"><span data-stu-id="51ad4-197">Use a domain account that belongs to the managed domain (for example, 'bob@CONTOSO100.COM' in this case.)</span></span>

    <span data-ttu-id="51ad4-198">SSH -l bob@CONTOSO100.COM contoso rhel.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="51ad4-198">ssh -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net</span></span>
2. <span data-ttu-id="51ad4-199">Skriv följande kommando för att se om arbetskatalogen har initierats korrekt i terminalen PuTTY.</span><span class="sxs-lookup"><span data-stu-id="51ad4-199">In your PuTTY terminal, type the following command to see if the home directory was initialized correctly.</span></span>

    <span data-ttu-id="51ad4-200">pwd</span><span class="sxs-lookup"><span data-stu-id="51ad4-200">pwd</span></span>
3. <span data-ttu-id="51ad4-201">Skriv följande kommando för att se om gruppmedlemskap som matchas korrekt i terminalen PuTTY.</span><span class="sxs-lookup"><span data-stu-id="51ad4-201">In your PuTTY terminal, type the following command to see if the group memberships are being resolved correctly.</span></span>

    <span data-ttu-id="51ad4-202">id</span><span class="sxs-lookup"><span data-stu-id="51ad4-202">id</span></span>

<span data-ttu-id="51ad4-203">Ett exempel på utdata från de här kommandona visas nedan:</span><span class="sxs-lookup"><span data-stu-id="51ad4-203">A sample output of these commands follows:</span></span>

![Kontrollera domänanslutning](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)

## <a name="troubleshooting-domain-join"></a><span data-ttu-id="51ad4-205">Felsöka domänanslutning</span><span class="sxs-lookup"><span data-stu-id="51ad4-205">Troubleshooting domain join</span></span>
<span data-ttu-id="51ad4-206">Referera till den [felsökning domänanslutning](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) artikel.</span><span class="sxs-lookup"><span data-stu-id="51ad4-206">Refer to the [Troubleshooting domain join](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) article.</span></span>

## <a name="related-content"></a><span data-ttu-id="51ad4-207">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="51ad4-207">Related Content</span></span>
* [<span data-ttu-id="51ad4-208">Azure AD Domain Services - komma igång-guide</span><span class="sxs-lookup"><span data-stu-id="51ad4-208">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="51ad4-209">Anslut en virtuell dator med Windows Server till en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="51ad4-209">Join a Windows Server virtual machine to an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* <span data-ttu-id="51ad4-210">[Logga in till en virtuell dator som kör Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="51ad4-210">[How to log on to a virtual machine running Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* [<span data-ttu-id="51ad4-211">Installera Kerberos</span><span class="sxs-lookup"><span data-stu-id="51ad4-211">Installing Kerberos</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [<span data-ttu-id="51ad4-212">Red Hat Enterprise Linux 7 - Guide för Windows-integrering</span><span class="sxs-lookup"><span data-stu-id="51ad4-212">Red Hat Enterprise Linux 7 - Windows Integration Guide</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
