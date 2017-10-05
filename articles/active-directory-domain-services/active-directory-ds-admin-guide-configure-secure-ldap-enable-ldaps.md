---
title: "Konfigurera säker LDAP (LDAPS) i Azure AD Domain Services | Microsoft Docs"
description: "Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c6da94b6-4328-4230-801a-4b646055d4d7
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: maheshu
ms.openlocfilehash: 3b19f078b0d6dc3e02d951014056406fd1b099a8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="ac532-103">Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="ac532-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="ac532-104">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="ac532-104">Before you begin</span></span>
<span data-ttu-id="ac532-105">Se till att du har slutfört [uppgift 2 – exportera säker LDAP-certifikatet till en. PFX-filen](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span><span class="sxs-lookup"><span data-stu-id="ac532-105">Ensure you've completed [Task 2 - export the secure LDAP certificate to a .PFX file](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span></span>

<span data-ttu-id="ac532-106">Välj om du vill använda förhandsversionen av Azure-portaler eller den klassiska Azure-portalen för att slutföra åtgärden.</span><span class="sxs-lookup"><span data-stu-id="ac532-106">Choose whether to use the preview Azure portal experience or the Azure classic portal to complete this task.</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="ac532-107">**Azure-portalen (förhandsgranskning)**: [aktivera säkert LDAP med Azure-portalen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="ac532-107">**Azure portal (Preview)**: [Enable secure LDAP using the Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
> * <span data-ttu-id="ac532-108">**Klassiska Azure-portalen**: [aktivera säkert LDAP med hjälp av den klassiska Azure-portalen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="ac532-108">**Azure classic portal**: [Enable secure LDAP using the classic Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span></span>
>
>


## <a name="task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview"></a><span data-ttu-id="ac532-109">Uppgift 3 – aktivera säker LDAP för den hanterade domänen med Azure-portalen (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="ac532-109">Task 3 - enable secure LDAP for the managed domain using the Azure portal (Preview)</span></span>
<span data-ttu-id="ac532-110">Utför följande konfigurationssteg för att aktivera säker LDAP:</span><span class="sxs-lookup"><span data-stu-id="ac532-110">To enable secure LDAP, perform the following configuration steps:</span></span>

1. <span data-ttu-id="ac532-111">Navigera till den  **[Azure-portalen](https://portal.azure.com)**.</span><span class="sxs-lookup"><span data-stu-id="ac532-111">Navigate to the **[Azure portal](https://portal.azure.com)**.</span></span>

2. <span data-ttu-id="ac532-112">Söka efter domain services i den **söka resurser** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="ac532-112">Search for 'domain services' in the **Search resources** search box.</span></span> <span data-ttu-id="ac532-113">Välj **Azure AD Domain Services** från sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="ac532-113">Select **Azure AD Domain Services** from the search result.</span></span> <span data-ttu-id="ac532-114">Den **Azure AD Domain Services** bladet visar din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="ac532-114">The **Azure AD Domain Services** blade lists your managed domain.</span></span>

    ![Hitta hanterad domän som har etablerats](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. <span data-ttu-id="ac532-116">Klicka på namnet på den hanterade domänen (till exempel ”contoso100.com”) för att se mer information om domänen.</span><span class="sxs-lookup"><span data-stu-id="ac532-116">Click the name of the managed domain (for example, 'contoso100.com') to see more details about the domain.</span></span>

    ![DS - Etableringsstatus](./media/getting-started/domain-services-provisioning-state.png)

3. <span data-ttu-id="ac532-118">Klicka på **säker LDAP** i navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="ac532-118">Click **Secure LDAP** on the navigation pane.</span></span>

    ![DS - säker LDAP-bladet](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. <span data-ttu-id="ac532-120">Säker LDAP-åtkomst till din hanterade domän är inaktiverat som standard.</span><span class="sxs-lookup"><span data-stu-id="ac532-120">By default, secure LDAP access to your managed domain is disabled.</span></span> <span data-ttu-id="ac532-121">Växla **säkert LDAP** till **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="ac532-121">Toggle **Secure LDAP** to **Enable**.</span></span>

    ![Aktivera säker LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. <span data-ttu-id="ac532-123">Som standard inaktiveras säker LDAP-åtkomst till din hanterade domän via internet.</span><span class="sxs-lookup"><span data-stu-id="ac532-123">By default, secure LDAP access to your managed domain over the internet is disabled.</span></span> <span data-ttu-id="ac532-124">Växla **ger LDAP åtkomst via internet** till **aktivera**om det behövs.</span><span class="sxs-lookup"><span data-stu-id="ac532-124">Toggle **Allow secure LDAP access over the internet** to **Enable**, if desired.</span></span> 

6. <span data-ttu-id="ac532-125">Klicka på mappen ikonen följande **. PFX-filen med säker LDAP-certifikat**.</span><span class="sxs-lookup"><span data-stu-id="ac532-125">Click the folder icon following **.PFX file with secure LDAP certificate**.</span></span> <span data-ttu-id="ac532-126">Ange sökvägen till PFX-filen med certifikatet för säker LDAP-åtkomst till den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="ac532-126">Specify the path to the PFX file with the certificate for secure LDAP access to the managed domain.</span></span>

7. <span data-ttu-id="ac532-127">Ange den **lösenord för att dekryptera. PFX-filen**.</span><span class="sxs-lookup"><span data-stu-id="ac532-127">Specify the **Password to decrypt .PFX file**.</span></span> <span data-ttu-id="ac532-128">Ange samma lösenord som du använde när du exporterar certifikatet till PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="ac532-128">Provide the same password you used when exporting the certificate to the PFX file.</span></span>

8. <span data-ttu-id="ac532-129">När du är klar klickar du på den **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="ac532-129">When you are done, click the **Save** button.</span></span>

9. <span data-ttu-id="ac532-130">Du ser ett meddelande som informerar du säkert LDAP konfigureras för den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="ac532-130">You see a notification that informs you secure LDAP is being configured for the managed domain.</span></span> <span data-ttu-id="ac532-131">Andra inställningar för domänen kan inte ändras förrän åtgärden har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ac532-131">Until this operation is complete, you cannot modify other settings for the domain.</span></span>

    ![Konfigurera säker LDAP för den hanterade domänen](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> <span data-ttu-id="ac532-133">Det tar cirka 10 – 15 minuter för att aktivera säker LDAP för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="ac532-133">It takes about 10 to 15 minutes to enable secure LDAP for your managed domain.</span></span> <span data-ttu-id="ac532-134">Om det tillhandahållna säkra LDAP-certifikatet inte matchar kriterierna som krävs, säker LDAP har inte aktiverats för din katalog och du ser ett fel.</span><span class="sxs-lookup"><span data-stu-id="ac532-134">If the provided secure LDAP certificate does not match the required criteria, secure LDAP is not enabled for your directory and you see a failure.</span></span> <span data-ttu-id="ac532-135">Till exempel domännamnet är felaktig, certifikatet redan har upphört att gälla eller upphör snart att gälla.</span><span class="sxs-lookup"><span data-stu-id="ac532-135">For example, the domain name is incorrect, the certificate has already expired or expires soon.</span></span> <span data-ttu-id="ac532-136">I det här fallet, försök igen med ett giltigt certifikat.</span><span class="sxs-lookup"><span data-stu-id="ac532-136">In this case, retry with a valid certificate.</span></span>
>
>

<br>

## <a name="task-4---configure-dns-to-access-the-managed-domain-from-the-internet"></a><span data-ttu-id="ac532-137">Uppgift 4 – konfigurera DNS för att komma åt den hanterade domänen från internet</span><span class="sxs-lookup"><span data-stu-id="ac532-137">Task 4 - configure DNS to access the managed domain from the internet</span></span>
> [!NOTE]
> <span data-ttu-id="ac532-138">**Frivillig uppgift** - om du inte planerar att få tillgång till den hanterade domänen med LDAPS via internet, hoppa över det här konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ac532-138">**Optional task** - If you do not plan to access the managed domain using LDAPS over the internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="ac532-139">Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen som beskrivs i [uppgift 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span><span class="sxs-lookup"><span data-stu-id="ac532-139">Before you begin this task, ensure you have completed the steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span></span>

<span data-ttu-id="ac532-140">När du har aktiverat säker LDAP-åtkomst via internet för din hanterade domän, måste du uppdatera DNS så att klientdatorerna kan hitta den här hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="ac532-140">Once you have enabled secure LDAP access over the internet for your managed domain, you need to update DNS so that client computers can find this managed domain.</span></span> <span data-ttu-id="ac532-141">I slutet av uppgift 3 visas en extern IP-adress på det **egenskaper** bladet i **extern IP-adress för LDAPS åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="ac532-141">At the end of task 3, an external IP address is displayed on the **Properties** blade in **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

<span data-ttu-id="ac532-142">Konfigurera externa DNS-providern så att DNS-namnet på den hanterade domänen (till exempel ldaps.contoso100.com) pekar på den här externa IP-adressen.</span><span class="sxs-lookup"><span data-stu-id="ac532-142">Configure your external DNS provider so that the DNS name of the managed domain (for example, 'ldaps.contoso100.com') points to this external IP address.</span></span> <span data-ttu-id="ac532-143">I vårt exempel måste vi skapa följande DNS-post:</span><span class="sxs-lookup"><span data-stu-id="ac532-143">In our example, we need to create the following DNS entry:</span></span>

    ldaps.contoso100.com  -> 52.165.38.113

<span data-ttu-id="ac532-144">Det - du är nu redo att ansluta till den hanterade domänen med säker LDAP via internet.</span><span class="sxs-lookup"><span data-stu-id="ac532-144">That's it - you are now ready to connect to the managed domain using secure LDAP over the internet.</span></span>

> [!WARNING]
> <span data-ttu-id="ac532-145">Kom ihåg att klientdatorer måste lita LDAPS-certifikatets utfärdare för att kunna ansluta till den hanterade domänen med LDAPS.</span><span class="sxs-lookup"><span data-stu-id="ac532-145">Remember that client computers must trust the issuer of the LDAPS certificate to be able to connect successfully to the managed domain using LDAPS.</span></span> <span data-ttu-id="ac532-146">Om du använder en betrodd offentlig certifikatutfärdare, behöver du inte göra något eftersom dessa certifikatutfärdare litar på klientdatorer.</span><span class="sxs-lookup"><span data-stu-id="ac532-146">If you are using a publicly trusted certification authority, you do not need to do anything since client computers trust these certificate issuers.</span></span> <span data-ttu-id="ac532-147">Om du använder ett självsignerat certifikat, installera den offentliga delen av det självsignerade certifikatet i det betrodda certifikatarkivet på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="ac532-147">If you are using a self-signed certificate, install the public part of the self-signed certificate into the trusted certificate store on the client computer.</span></span>
>
>


## <a name="task-5---lock-down-ldaps-access-to-your-managed-domain-over-the-internet"></a><span data-ttu-id="ac532-148">Uppgift 5 - låsning LDAPS åtkomst till din hanterade domän via internet</span><span class="sxs-lookup"><span data-stu-id="ac532-148">Task 5 - lock-down LDAPS access to your managed domain over the internet</span></span>
> [!NOTE]
> <span data-ttu-id="ac532-149">**Frivillig uppgift** - om du inte har aktiverat LDAPS åtkomst till den hanterade domänen via internet, hoppa över det här konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ac532-149">**Optional task** - If you have not enabled LDAPS access to the managed domain over the internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="ac532-150">Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen som beskrivs i [uppgift 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span><span class="sxs-lookup"><span data-stu-id="ac532-150">Before you begin this task, ensure you have completed the steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span></span>

<span data-ttu-id="ac532-151">Exponera din hanterade domän för LDAPS åtkomst via internet representerar en säkerhetsrisk.</span><span class="sxs-lookup"><span data-stu-id="ac532-151">Exposing your managed domain for LDAPS access over the internet represents a security threat.</span></span> <span data-ttu-id="ac532-152">Den hanterade domänen kan nås från internet på porten som används för säker LDAP (det vill säga port 636).</span><span class="sxs-lookup"><span data-stu-id="ac532-152">The managed domain is reachable from the internet at the port used for secure LDAP (that is, port 636).</span></span> <span data-ttu-id="ac532-153">Därför kan du välja att begränsa åtkomsten till den hanterade domänen till specifika kända IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="ac532-153">Therefore, you can choose to restrict access to the managed domain to specific known IP addresses.</span></span> <span data-ttu-id="ac532-154">Skapa en nätverkssäkerhetsgrupp (NSG) för förbättrad säkerhet och associera den med undernätet där du har aktiverat Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="ac532-154">For improved security, create a network security group (NSG) and associate it with the subnet where you have enabled Azure AD Domain Services.</span></span>

<span data-ttu-id="ac532-155">I följande tabell visas ett exempel på en NSG som du kan konfigurera för att låsa säker LDAP-åtkomst via internet.</span><span class="sxs-lookup"><span data-stu-id="ac532-155">The following table illustrates a sample NSG you can configure, to lock down secure LDAP access over the internet.</span></span> <span data-ttu-id="ac532-156">NSG: N innehåller en uppsättning regler som tillåter inkommande LDAPS åtkomst via TCP-port 636 endast från en angiven mängd av IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="ac532-156">The NSG contains a set of rules that allow inbound LDAPS access over TCP port 636 only from a specified set of IP addresses.</span></span> <span data-ttu-id="ac532-157">'DenyAll' Standardregeln gäller för inkommande trafik från internet.</span><span class="sxs-lookup"><span data-stu-id="ac532-157">The default 'DenyAll' rule applies to all other inbound traffic from the internet.</span></span> <span data-ttu-id="ac532-158">NSG-regel som tillåter LDAPS åtkomst via internet från den angivna IP-adresser har högre prioritet än DenyAll NSG-regeln.</span><span class="sxs-lookup"><span data-stu-id="ac532-158">The NSG rule to allow LDAPS access over the internet from specified IP addresses has a higher priority than the DenyAll NSG rule.</span></span>

![Exempel NSG till säker LDAPS åtkomst via internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

<span data-ttu-id="ac532-160">**Mer information** - [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="ac532-160">**More information** - [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="ac532-161">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="ac532-161">Related content</span></span>
* [<span data-ttu-id="ac532-162">Azure AD Domain Services - komma igång-guide</span><span class="sxs-lookup"><span data-stu-id="ac532-162">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="ac532-163">Administrera en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="ac532-163">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="ac532-164">Administrera Grupprincip i en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="ac532-164">Administer Group Policy on an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-group-policy.md)
* [<span data-ttu-id="ac532-165">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="ac532-165">Network security groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="ac532-166">Skapa en säkerhetsgrupp för nätverk</span><span class="sxs-lookup"><span data-stu-id="ac532-166">Create a Network Security Group</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
