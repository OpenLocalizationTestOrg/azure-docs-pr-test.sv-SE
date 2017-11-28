---
title: "Konfigurera säker LDAP (LDAPS) i Azure AD Domain Services | Microsoft Docs"
description: "Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 9d563c45-9578-410d-96c8-355af64feae8
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 3aafe209aad7383cd0610d147b5fdba673023c93
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="facf8-103">Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="facf8-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="facf8-104">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="facf8-104">Before you begin</span></span>
<span data-ttu-id="facf8-105">Se till att du har slutfört [uppgift 2 – exportera säker LDAP-certifikatet till en. PFX-filen](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span><span class="sxs-lookup"><span data-stu-id="facf8-105">Ensure you've completed [Task 2 - export the secure LDAP certificate to a .PFX file](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span></span>

<span data-ttu-id="facf8-106">Välj om du vill använda förhandsversionen av Azure-portaler eller den klassiska Azure-portalen för att slutföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="facf8-106">Choose whether to use the preview Azure portal experience or the Azure classic portal to complete this task.</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="facf8-107">**Azure-portalen (förhandsgranskning)**: [aktivera säkert LDAP med Azure-portalen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="facf8-107">**Azure portal (Preview)**: [Enable secure LDAP using the Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
> * <span data-ttu-id="facf8-108">**Klassiska Azure-portalen**: [aktivera säkert LDAP med hjälp av den klassiska Azure-portalen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="facf8-108">**Azure classic portal**: [Enable secure LDAP using the classic Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span></span>
>
>


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal"></a><span data-ttu-id="facf8-109">Uppgift 3 – aktivera säker LDAP för den hanterade domänen med den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="facf8-109">Task 3 - enable secure LDAP for the managed domain using the classic Azure portal</span></span>
<span data-ttu-id="facf8-110">Utför följande konfigurationssteg för att aktivera säker LDAP:</span><span class="sxs-lookup"><span data-stu-id="facf8-110">To enable secure LDAP, perform the following configuration steps:</span></span>

1. <span data-ttu-id="facf8-111">Navigera till den  **[klassiska Azure-portalen](https://manage.windowsazure.com)**.</span><span class="sxs-lookup"><span data-stu-id="facf8-111">Navigate to the **[Azure classic portal](https://manage.windowsazure.com)**.</span></span>
2. <span data-ttu-id="facf8-112">Välj noden **Active Directory** i det vänstra fönstret.</span><span class="sxs-lookup"><span data-stu-id="facf8-112">Select the **Active Directory** node on the left pane.</span></span>
3. <span data-ttu-id="facf8-113">Välj Azure AD-katalogen (kallas även ”klient”), som du har aktiverat Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="facf8-113">Select the Azure AD directory (also referred to as 'tenant'), for which you have enabled Azure AD Domain Services.</span></span>

    ![Välja Azure AD-katalog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="facf8-115">Klicka på fliken **Konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="facf8-115">Click the **Configure** tab.</span></span>

    ![Fliken Konfigurera för katalogen](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. <span data-ttu-id="facf8-117">Rulla ned till avsnittet **domäntjänster**.</span><span class="sxs-lookup"><span data-stu-id="facf8-117">Scroll down to the section titled **domain services**.</span></span> <span data-ttu-id="facf8-118">Du bör se en alternativet **säker LDAP (LDAPS)** som visas i följande skärmbild:</span><span class="sxs-lookup"><span data-stu-id="facf8-118">You should see an option titled **Secure LDAP (LDAPS)** as shown in the following screenshot:</span></span>

    ![Konfigurationsavsnittet Domäntjänster](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. <span data-ttu-id="facf8-120">Klicka på den **konfigurera certifikat...**  du vill visa den **konfigurera certifikat för säker LDAP** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="facf8-120">Click the **Configure certificate ...** button to bring up the **Configure Certificate for Secure LDAP** dialog.</span></span>

    ![Konfigurera certifikat för säker LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. <span data-ttu-id="facf8-122">Klicka på mappen ikonen följande **PFX-filen med certifikat** att ange PFX-filen som innehåller det certifikat som du vill använda för säker LDAP-åtkomst till den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="facf8-122">Click the folder icon following **PFX FILE WITH CERTIFICATE** to specify the PFX file, which contains the certificate you wish to use for secure LDAP access to the managed domain.</span></span> <span data-ttu-id="facf8-123">Också ange lösenordet du angav när du exporterar certifikatet till PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="facf8-123">Also enter the password you specified when exporting the certificate to the PFX file.</span></span> <span data-ttu-id="facf8-124">Klicka på knappen klar längst ned.</span><span class="sxs-lookup"><span data-stu-id="facf8-124">Then, click the done button on the bottom.</span></span>

    ![Ange säker LDAP PFX-filen och lösenord](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. <span data-ttu-id="facf8-126">Den **domäntjänster** avsnitt i den **konfigurera** ska hämta nedtonad och är i den **väntande...**  tillstånd om en stund.</span><span class="sxs-lookup"><span data-stu-id="facf8-126">The **domain services** section of the **Configure** tab should get grayed out and is in the **Pending...** state for a few minutes.</span></span> <span data-ttu-id="facf8-127">Under denna tid LDAPS-certifikatet verifieras för Precision och säker LDAP har konfigurerats för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="facf8-127">During this period, the LDAPS certificate is verified for accuracy and secure LDAP is configured for your managed domain.</span></span>

    ![Säkert LDAP - väntetillstånd](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > <span data-ttu-id="facf8-129">Det tar cirka 10 – 15 minuter för att aktivera säker LDAP för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="facf8-129">It takes about 10 to 15 minutes to enable secure LDAP for your managed domain.</span></span> <span data-ttu-id="facf8-130">Om det tillhandahållna säkra LDAP-certifikatet inte matchar kriterierna som krävs, säker LDAP har inte aktiverats för din katalog och du ser ett fel.</span><span class="sxs-lookup"><span data-stu-id="facf8-130">If the provided secure LDAP certificate does not match the required criteria, secure LDAP is not enabled for your directory and you see a failure.</span></span> <span data-ttu-id="facf8-131">Till exempel domännamnet är felaktig, certifikatet redan har upphört att gälla eller upphör snart att gälla.</span><span class="sxs-lookup"><span data-stu-id="facf8-131">For example, the domain name is incorrect, the certificate has already expired or expires soon.</span></span>
   >
   >

9. <span data-ttu-id="facf8-132">När säker LDAP har aktiverats för din hanterade domän i **väntande...**  meddelandet ska försvinner.</span><span class="sxs-lookup"><span data-stu-id="facf8-132">When secure LDAP is successfully enabled for your managed domain, the **Pending...** message should disappear.</span></span> <span data-ttu-id="facf8-133">Du bör se tumavtrycket för certifikatet visas.</span><span class="sxs-lookup"><span data-stu-id="facf8-133">You should see the thumbprint of the certificate displayed.</span></span>

    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a><span data-ttu-id="facf8-135">Uppgift 4 – aktivera säker LDAP-åtkomst via internet</span><span class="sxs-lookup"><span data-stu-id="facf8-135">Task 4 - enable secure LDAP access over the internet</span></span>
<span data-ttu-id="facf8-136">**Frivillig uppgift** - om du inte planerar att få tillgång till den hanterade domänen med LDAPS via internet, hoppa över det här konfiguration.</span><span class="sxs-lookup"><span data-stu-id="facf8-136">**Optional task** - If you do not plan to access the managed domain using LDAPS over the internet, skip this configuration task.</span></span>

<span data-ttu-id="facf8-137">Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen som beskrivs i [uppgift 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="facf8-137">Before you begin this task, ensure you have completed the steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).</span></span>

1. <span data-ttu-id="facf8-138">Du bör se ett alternativ för att **aktivera säker LDAP åtkomst via INTERNET** i den **domäntjänster** avsnitt i den **konfigurera** sidan.</span><span class="sxs-lookup"><span data-stu-id="facf8-138">You should see an option to **ENABLE SECURE LDAP ACCESS OVER THE INTERNET** in the **domain services** section of the **Configure** page.</span></span> <span data-ttu-id="facf8-139">Det här alternativet är inställt på **nr** som standard eftersom internet-åtkomst till den hanterade domänen via säker LDAP är inaktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="facf8-139">This option is set to **NO** by default since internet access to the managed domain over secure LDAP is disabled by default.</span></span>

    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. <span data-ttu-id="facf8-141">Växla **aktivera säker LDAP åtkomst via INTERNET** till **Ja**.</span><span class="sxs-lookup"><span data-stu-id="facf8-141">Toggle **ENABLE SECURE LDAP ACCESS OVER THE INTERNET** to **YES**.</span></span> <span data-ttu-id="facf8-142">Klicka på den **spara** på nedre panelen.</span><span class="sxs-lookup"><span data-stu-id="facf8-142">Click the **SAVE** button on the bottom panel.</span></span>
    <span data-ttu-id="facf8-143">![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)</span><span class="sxs-lookup"><span data-stu-id="facf8-143">![Secure LDAP enabled](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)</span></span>
3. <span data-ttu-id="facf8-144">Den **domäntjänster** avsnitt i den **konfigurera** ska hämta nedtonad och är i den **väntande...**  tillstånd om en stund.</span><span class="sxs-lookup"><span data-stu-id="facf8-144">The **domain services** section of the **Configure** tab should get grayed out and is in the **Pending...** state for a few minutes.</span></span> <span data-ttu-id="facf8-145">Internet-åtkomst till din hanterade domän över säker LDAP aktiveras efter en stund.</span><span class="sxs-lookup"><span data-stu-id="facf8-145">After some time, internet access to your managed domain over secure LDAP is enabled.</span></span>

    ![Säkert LDAP - väntetillstånd](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > <span data-ttu-id="facf8-147">Det tar cirka 10 minuter för att aktivera Internetåtkomst via säker LDAP för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="facf8-147">It takes about 10 minutes to enable internet access over secure LDAP for your managed domain.</span></span>
   >
   >
4. <span data-ttu-id="facf8-148">När säker LDAP-åtkomst till din hanterade domän via internet har aktiverats på **väntande...**  meddelandet ska försvinner.</span><span class="sxs-lookup"><span data-stu-id="facf8-148">When secure LDAP access to your managed domain over the internet is successfully enabled, the **Pending...** message should disappear.</span></span> <span data-ttu-id="facf8-149">Du bör se den externa IP-adressen som kan användas för att få åtkomst till din katalog över LDAPS i fältet **extern IP-adress för LDAPS åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="facf8-149">You should see the external IP address that can be used to access your directory over LDAPS in the field **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a><span data-ttu-id="facf8-151">Uppgift 5 – konfigurera DNS för att komma åt den hanterade domänen från internet</span><span class="sxs-lookup"><span data-stu-id="facf8-151">Task 5 - configure DNS to access the managed domain from the internet</span></span>
<span data-ttu-id="facf8-152">**Frivillig uppgift** - om du inte planerar att få tillgång till den hanterade domänen med LDAPS via internet, hoppa över det här konfiguration.</span><span class="sxs-lookup"><span data-stu-id="facf8-152">**Optional task** - If you do not plan to access the managed domain using LDAPS over the internet, skip this configuration task.</span></span>

<span data-ttu-id="facf8-153">Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen som beskrivs i [uppgift 4](#task-4---enable-secure-ldap-access-over-the-internet).</span><span class="sxs-lookup"><span data-stu-id="facf8-153">Before you begin this task, ensure you have completed the steps outlined in [Task 4](#task-4---enable-secure-ldap-access-over-the-internet).</span></span>

<span data-ttu-id="facf8-154">När du har aktiverat säker LDAP-åtkomst via internet för din hanterade domän, måste du uppdatera DNS så att klientdatorerna kan hitta den här hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="facf8-154">Once you have enabled secure LDAP access over the internet for your managed domain, you need to update DNS so that client computers can find this managed domain.</span></span> <span data-ttu-id="facf8-155">I slutet av uppgift 4 visas en extern IP-adress på det **konfigurera** fliken i **extern IP-adress för LDAPS åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="facf8-155">At the end of task 4, an external IP address is displayed on the **Configure** tab in **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

<span data-ttu-id="facf8-156">Konfigurera externa DNS-providern så att DNS-namnet på den hanterade domänen (till exempel ldaps.contoso100.com) pekar på den här externa IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="facf8-156">Configure your external DNS provider so that the DNS name of the managed domain (for example, 'ldaps.contoso100.com') points to this external IP address.</span></span> <span data-ttu-id="facf8-157">I vårt exempel måste vi skapa följande DNS-post:</span><span class="sxs-lookup"><span data-stu-id="facf8-157">In our example, we need to create the following DNS entry:</span></span>

    ldaps.contoso100.com  -> 52.165.38.113

<span data-ttu-id="facf8-158">Det - du är nu redo att ansluta till den hanterade domänen med säker LDAP via internet.</span><span class="sxs-lookup"><span data-stu-id="facf8-158">That's it - you are now ready to connect to the managed domain using secure LDAP over the internet.</span></span>

> [!WARNING]
> <span data-ttu-id="facf8-159">Kom ihåg att klientdatorer måste lita LDAPS-certifikatets utfärdare för att kunna ansluta till den hanterade domänen med LDAPS.</span><span class="sxs-lookup"><span data-stu-id="facf8-159">Remember that client computers must trust the issuer of the LDAPS certificate to be able to connect successfully to the managed domain using LDAPS.</span></span> <span data-ttu-id="facf8-160">Om du använder en företagscertifikatutfärdare eller en betrodd offentlig certifikatutfärdare, behöver du inte göra något eftersom dessa certifikatutfärdare litar på klientdatorer.</span><span class="sxs-lookup"><span data-stu-id="facf8-160">If you are using an enterprise certification authority or a publicly trusted certification authority, you do not need to do anything since client computers trust these certificate issuers.</span></span> <span data-ttu-id="facf8-161">Om du använder ett självsignerat certifikat, måste du installera den offentliga delen av det självsignerade certifikatet i det betrodda certifikatarkivet på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="facf8-161">If you are using a self-signed certificate, you need to install the public part of the self-signed certificate into the trusted certificate store on the client computer.</span></span>
>
>


## <a name="lock-down-ldaps-access-to-your-managed-domain-over-the-internet"></a><span data-ttu-id="facf8-162">Låsning LDAPS åtkomst till din hanterade domän via internet</span><span class="sxs-lookup"><span data-stu-id="facf8-162">Lock-down LDAPS access to your managed domain over the internet</span></span>
> [!NOTE]
> <span data-ttu-id="facf8-163">**Frivillig uppgift** - om du inte har aktiverat LDAPS åtkomst till den hanterade domänen via internet, hoppa över det här konfiguration.</span><span class="sxs-lookup"><span data-stu-id="facf8-163">**Optional task** - If you have not enabled LDAPS access to the managed domain over the internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="facf8-164">Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen som beskrivs i [uppgift 4](#task-4---enable-secure-ldap-access-over-the-internet).</span><span class="sxs-lookup"><span data-stu-id="facf8-164">Before you begin this task, ensure you have completed the steps outlined in [Task 4](#task-4---enable-secure-ldap-access-over-the-internet).</span></span>

<span data-ttu-id="facf8-165">Exponera din hanterade domän för LDAPS åtkomst via internet representerar en säkerhetsrisk.</span><span class="sxs-lookup"><span data-stu-id="facf8-165">Exposing your managed domain for LDAPS access over the internet represents a security threat.</span></span> <span data-ttu-id="facf8-166">Den hanterade domänen kan nås från internet på porten som används för säker LDAP (det vill säga port 636).</span><span class="sxs-lookup"><span data-stu-id="facf8-166">The managed domain is reachable from the internet at the port used for secure LDAP (that is, port 636).</span></span> <span data-ttu-id="facf8-167">Därför kan du välja att begränsa åtkomsten till den hanterade domänen till specifika kända IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="facf8-167">Therefore, you can choose to restrict access to the managed domain to specific known IP addresses.</span></span> <span data-ttu-id="facf8-168">Skapa en nätverkssäkerhetsgrupp (NSG) för förbättrad säkerhet och associera den med undernätet där du har aktiverat Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="facf8-168">For improved security, create a network security group (NSG) and associate it with the subnet where you have enabled Azure AD Domain Services.</span></span>

<span data-ttu-id="facf8-169">I följande tabell visas ett exempel på en NSG som du kan konfigurera för att låsa säker LDAP-åtkomst via internet.</span><span class="sxs-lookup"><span data-stu-id="facf8-169">The following table illustrates a sample NSG you can configure, to lock down secure LDAP access over the internet.</span></span> <span data-ttu-id="facf8-170">NSG: N innehåller en uppsättning regler som tillåter inkommande LDAPS åtkomst via TCP-port 636 endast från en angiven mängd av IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="facf8-170">The NSG contains a set of rules that allow inbound LDAPS access over TCP port 636 only from a specified set of IP addresses.</span></span> <span data-ttu-id="facf8-171">'DenyAll' Standardregeln gäller för inkommande trafik från internet.</span><span class="sxs-lookup"><span data-stu-id="facf8-171">The default 'DenyAll' rule applies to all other inbound traffic from the internet.</span></span> <span data-ttu-id="facf8-172">NSG-regel som tillåter LDAPS åtkomst via internet från den angivna IP-adresser har högre prioritet än DenyAll NSG-regeln.</span><span class="sxs-lookup"><span data-stu-id="facf8-172">The NSG rule to allow LDAPS access over the internet from specified IP addresses has a higher priority than the DenyAll NSG rule.</span></span>

![Exempel NSG till säker LDAPS åtkomst via internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

<span data-ttu-id="facf8-174">**Mer information** - [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="facf8-174">**More information** - [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="facf8-175">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="facf8-175">Related content</span></span>
* [<span data-ttu-id="facf8-176">Azure AD Domain Services - komma igång-guide</span><span class="sxs-lookup"><span data-stu-id="facf8-176">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="facf8-177">Administrera en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="facf8-177">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="facf8-178">Administrera Grupprincip i en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="facf8-178">Administer Group Policy on an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-group-policy.md)
* [<span data-ttu-id="facf8-179">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="facf8-179">Network security groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="facf8-180">Skapa en säkerhetsgrupp för nätverk</span><span class="sxs-lookup"><span data-stu-id="facf8-180">Create a Network Security Group</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
