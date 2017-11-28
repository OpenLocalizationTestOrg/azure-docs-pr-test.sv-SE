---
title: "aaaConfigure säker LDAP (LDAPS) i Azure AD Domain Services | Microsoft Docs"
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
ms.openlocfilehash: 8781285cd02d690788056b985b017efd7e4d6b3f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="63dc2-103">Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="63dc2-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="63dc2-104">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="63dc2-104">Before you begin</span></span>
<span data-ttu-id="63dc2-105">Se till att du har slutfört [uppgift 2 - export hello säker LDAP certifikat tooa. PFX-filen](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span><span class="sxs-lookup"><span data-stu-id="63dc2-105">Ensure you've completed [Task 2 - export hello secure LDAP certificate tooa .PFX file](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span></span>

<span data-ttu-id="63dc2-106">Välj om toouse hello Förhandsgranska Azure-portaler eller hello Azure klassiska portal toocomplete den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="63dc2-106">Choose whether toouse hello preview Azure portal experience or hello Azure classic portal toocomplete this task.</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="63dc2-107">**Azure-portalen (förhandsgranskning)**: [aktivera säkert LDAP med hello Azure-portalen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="63dc2-107">**Azure portal (Preview)**: [Enable secure LDAP using hello Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
> * <span data-ttu-id="63dc2-108">**Klassiska Azure-portalen**: [aktivera säkert LDAP med hello klassiska Azure-portalen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="63dc2-108">**Azure classic portal**: [Enable secure LDAP using hello classic Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span></span>
>
>


## <a name="task-3---enable-secure-ldap-for-hello-managed-domain-using-hello-azure-portal-preview"></a><span data-ttu-id="63dc2-109">Uppgift 3 – aktivera säker LDAP för hello hanterade domänen med hello Azure-portalen (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="63dc2-109">Task 3 - enable secure LDAP for hello managed domain using hello Azure portal (Preview)</span></span>
<span data-ttu-id="63dc2-110">tooenable säkert LDAP, utför följande konfigurationssteg hello:</span><span class="sxs-lookup"><span data-stu-id="63dc2-110">tooenable secure LDAP, perform hello following configuration steps:</span></span>

1. <span data-ttu-id="63dc2-111">Navigera toohello  **[Azure-portalen](https://portal.azure.com)**.</span><span class="sxs-lookup"><span data-stu-id="63dc2-111">Navigate toohello **[Azure portal](https://portal.azure.com)**.</span></span>

2. <span data-ttu-id="63dc2-112">Sök efter domain services i hello **söka resurser** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="63dc2-112">Search for 'domain services' in hello **Search resources** search box.</span></span> <span data-ttu-id="63dc2-113">Välj **Azure AD Domain Services** från hello sökresultatet.</span><span class="sxs-lookup"><span data-stu-id="63dc2-113">Select **Azure AD Domain Services** from hello search result.</span></span> <span data-ttu-id="63dc2-114">Hej **Azure AD Domain Services** bladet visar din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="63dc2-114">hello **Azure AD Domain Services** blade lists your managed domain.</span></span>

    ![Hitta hanterad domän som har etablerats](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. <span data-ttu-id="63dc2-116">Klicka på hello hanterade domän (till exempel ”contoso100.com”) toosee hello namn mer information om hello domän.</span><span class="sxs-lookup"><span data-stu-id="63dc2-116">Click hello name of hello managed domain (for example, 'contoso100.com') toosee more details about hello domain.</span></span>

    ![DS - Etableringsstatus](./media/getting-started/domain-services-provisioning-state.png)

3. <span data-ttu-id="63dc2-118">Klicka på **säker LDAP** hello navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="63dc2-118">Click **Secure LDAP** on hello navigation pane.</span></span>

    ![DS - säker LDAP-bladet](./media/active-directory-domain-services-admin-guide/secure-ldap-blade.png)

4. <span data-ttu-id="63dc2-120">Säkert LDAP åtkomst tooyour hanterad domän är inaktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="63dc2-120">By default, secure LDAP access tooyour managed domain is disabled.</span></span> <span data-ttu-id="63dc2-121">Växla **säker LDAP** för**aktivera**.</span><span class="sxs-lookup"><span data-stu-id="63dc2-121">Toggle **Secure LDAP** too**Enable**.</span></span>

    ![Aktivera säker LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configure.png)
5. <span data-ttu-id="63dc2-123">Som standard hanteras säker LDAP åtkomst tooyour domänen via hello internet är inaktiverad.</span><span class="sxs-lookup"><span data-stu-id="63dc2-123">By default, secure LDAP access tooyour managed domain over hello internet is disabled.</span></span> <span data-ttu-id="63dc2-124">Växla **Tillåt säker LDAP åtkomst via hello internet** för**aktivera**om det behövs.</span><span class="sxs-lookup"><span data-stu-id="63dc2-124">Toggle **Allow secure LDAP access over hello internet** too**Enable**, if desired.</span></span> 

6. <span data-ttu-id="63dc2-125">Klicka på hello mappen ikonen följande **. PFX-filen med säker LDAP-certifikat**.</span><span class="sxs-lookup"><span data-stu-id="63dc2-125">Click hello folder icon following **.PFX file with secure LDAP certificate**.</span></span> <span data-ttu-id="63dc2-126">Ange hello sökvägen toohello PFX-filen med hello certifikat för säker LDAP åtkomst toohello hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="63dc2-126">Specify hello path toohello PFX file with hello certificate for secure LDAP access toohello managed domain.</span></span>

7. <span data-ttu-id="63dc2-127">Ange hello **lösenord toodecrypt. PFX-filen**.</span><span class="sxs-lookup"><span data-stu-id="63dc2-127">Specify hello **Password toodecrypt .PFX file**.</span></span> <span data-ttu-id="63dc2-128">Ange hello samma lösenord som du använde när du exporterar hello certifikatets toohello PFX-fil.</span><span class="sxs-lookup"><span data-stu-id="63dc2-128">Provide hello same password you used when exporting hello certificate toohello PFX file.</span></span>

8. <span data-ttu-id="63dc2-129">När du är klar klickar du på hello **spara** knappen.</span><span class="sxs-lookup"><span data-stu-id="63dc2-129">When you are done, click hello **Save** button.</span></span>

9. <span data-ttu-id="63dc2-130">Du ser ett meddelande som informerar säker LDAP konfigureras för hello-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="63dc2-130">You see a notification that informs you secure LDAP is being configured for hello managed domain.</span></span> <span data-ttu-id="63dc2-131">Du kan inte ändra andra inställningar för hello domän tills åtgärden är slutförd.</span><span class="sxs-lookup"><span data-stu-id="63dc2-131">Until this operation is complete, you cannot modify other settings for hello domain.</span></span>

    ![Konfigurera säker LDAP för hello-hanterad domän](./media/active-directory-domain-services-admin-guide/secure-ldap-blade-configuring.png)

> [!NOTE]
> <span data-ttu-id="63dc2-133">Det tar cirka 10 too15 minuter tooenable säkert LDAP för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="63dc2-133">It takes about 10 too15 minutes tooenable secure LDAP for your managed domain.</span></span> <span data-ttu-id="63dc2-134">Om hello säker LDAP-certifikat inte matchar hello krävs kriterier, säker LDAP har inte aktiverats för din katalog och du ser ett fel.</span><span class="sxs-lookup"><span data-stu-id="63dc2-134">If hello provided secure LDAP certificate does not match hello required criteria, secure LDAP is not enabled for your directory and you see a failure.</span></span> <span data-ttu-id="63dc2-135">Till exempel hello domännamn är felaktig, hello certifikatet redan har upphört att gälla eller upphör snart att gälla.</span><span class="sxs-lookup"><span data-stu-id="63dc2-135">For example, hello domain name is incorrect, hello certificate has already expired or expires soon.</span></span> <span data-ttu-id="63dc2-136">I det här fallet, försök igen med ett giltigt certifikat.</span><span class="sxs-lookup"><span data-stu-id="63dc2-136">In this case, retry with a valid certificate.</span></span>
>
>

<br>

## <a name="task-4---configure-dns-tooaccess-hello-managed-domain-from-hello-internet"></a><span data-ttu-id="63dc2-137">Uppgift 4 – konfigurera DNS-tooaccess hello hanterade domänen från hello internet</span><span class="sxs-lookup"><span data-stu-id="63dc2-137">Task 4 - configure DNS tooaccess hello managed domain from hello internet</span></span>
> [!NOTE]
> <span data-ttu-id="63dc2-138">**Frivillig uppgift** - om du inte planerar tooaccess hello hanterade domänen med LDAPS över Hej internet, hoppa över det här konfiguration.</span><span class="sxs-lookup"><span data-stu-id="63dc2-138">**Optional task** - If you do not plan tooaccess hello managed domain using LDAPS over hello internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="63dc2-139">Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen i hello [uppgift 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span><span class="sxs-lookup"><span data-stu-id="63dc2-139">Before you begin this task, ensure you have completed hello steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span></span>

<span data-ttu-id="63dc2-140">När du har aktiverat säker LDAP-åtkomst via hello internet för din hanterade domän måste tooupdate DNS så att klientdatorerna kan hitta den här hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="63dc2-140">Once you have enabled secure LDAP access over hello internet for your managed domain, you need tooupdate DNS so that client computers can find this managed domain.</span></span> <span data-ttu-id="63dc2-141">Hello slutet av uppgift 3, visas en extern IP-adress på hello **egenskaper** bladet i **extern IP-adress för LDAPS åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="63dc2-141">At hello end of task 3, an external IP address is displayed on hello **Properties** blade in **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

<span data-ttu-id="63dc2-142">Konfigurera externa DNS-providern så att hello DNS-namnet på hello hanterade domän (till exempel ldaps.contoso100.com) pekar toothis extern IP-adress.</span><span class="sxs-lookup"><span data-stu-id="63dc2-142">Configure your external DNS provider so that hello DNS name of hello managed domain (for example, 'ldaps.contoso100.com') points toothis external IP address.</span></span> <span data-ttu-id="63dc2-143">I vårt exempel behöver vi toocreate hello följande DNS-post:</span><span class="sxs-lookup"><span data-stu-id="63dc2-143">In our example, we need toocreate hello following DNS entry:</span></span>

    ldaps.contoso100.com  -> 52.165.38.113

<span data-ttu-id="63dc2-144">Det – nu är du redo tooconnect toohello hanterade domänen med hjälp av säker LDAP över hello internet.</span><span class="sxs-lookup"><span data-stu-id="63dc2-144">That's it - you are now ready tooconnect toohello managed domain using secure LDAP over hello internet.</span></span>

> [!WARNING]
> <span data-ttu-id="63dc2-145">Kom ihåg att klientdatorer måste lita hello utfärdaren av hello LDAPS certifikat toobe kan tooconnect har toohello hanterade domänen med LDAPS.</span><span class="sxs-lookup"><span data-stu-id="63dc2-145">Remember that client computers must trust hello issuer of hello LDAPS certificate toobe able tooconnect successfully toohello managed domain using LDAPS.</span></span> <span data-ttu-id="63dc2-146">Om du använder en betrodd offentlig certifikatutfärdare, behöver du inte toodo allt eftersom dessa certifikatutfärdare litar på klientdatorer.</span><span class="sxs-lookup"><span data-stu-id="63dc2-146">If you are using a publicly trusted certification authority, you do not need toodo anything since client computers trust these certificate issuers.</span></span> <span data-ttu-id="63dc2-147">Om du använder ett självsignerat certifikat, installera hello offentliga del av hello självsignerat certifikat till hello betrodda certifikatarkiv på hello-klientdator.</span><span class="sxs-lookup"><span data-stu-id="63dc2-147">If you are using a self-signed certificate, install hello public part of hello self-signed certificate into hello trusted certificate store on hello client computer.</span></span>
>
>


## <a name="task-5---lock-down-ldaps-access-tooyour-managed-domain-over-hello-internet"></a><span data-ttu-id="63dc2-148">Uppgift 5 - låsning LDAPS komma åt tooyour hanterade domänen via hello internet</span><span class="sxs-lookup"><span data-stu-id="63dc2-148">Task 5 - lock-down LDAPS access tooyour managed domain over hello internet</span></span>
> [!NOTE]
> <span data-ttu-id="63dc2-149">**Frivillig uppgift** - om du inte har aktiverat LDAPS åtkomst toohello hanterad domän över Hej internet, hoppa över det här konfiguration.</span><span class="sxs-lookup"><span data-stu-id="63dc2-149">**Optional task** - If you have not enabled LDAPS access toohello managed domain over hello internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="63dc2-150">Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen i hello [uppgift 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span><span class="sxs-lookup"><span data-stu-id="63dc2-150">Before you begin this task, ensure you have completed hello steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-azure-portal-preview).</span></span>

<span data-ttu-id="63dc2-151">Visa din hanterade domän för LDAPS åtkomst över hello representerar internet en säkerhetsrisk.</span><span class="sxs-lookup"><span data-stu-id="63dc2-151">Exposing your managed domain for LDAPS access over hello internet represents a security threat.</span></span> <span data-ttu-id="63dc2-152">hello hanterad domän kan nås från hello internet på hello-port som används för säker LDAP (det vill säga port 636).</span><span class="sxs-lookup"><span data-stu-id="63dc2-152">hello managed domain is reachable from hello internet at hello port used for secure LDAP (that is, port 636).</span></span> <span data-ttu-id="63dc2-153">Därför kan du välja toorestrict åtkomst toohello hanterade domänen toospecific kända IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="63dc2-153">Therefore, you can choose toorestrict access toohello managed domain toospecific known IP addresses.</span></span> <span data-ttu-id="63dc2-154">För förbättrad säkerhet skapar en nätverkssäkerhetsgrupp (NSG) och associera den med hello undernät där du har aktiverat Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="63dc2-154">For improved security, create a network security group (NSG) and associate it with hello subnet where you have enabled Azure AD Domain Services.</span></span>

<span data-ttu-id="63dc2-155">hello i tabellen nedan visas ett exempel på en NSG som du kan konfigurera toolock ned säker LDAP åtkomst via hello internet.</span><span class="sxs-lookup"><span data-stu-id="63dc2-155">hello following table illustrates a sample NSG you can configure, toolock down secure LDAP access over hello internet.</span></span> <span data-ttu-id="63dc2-156">hello NSG innehåller en uppsättning regler som tillåter inkommande LDAPS åtkomst via TCP-port 636 endast från en angiven mängd av IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="63dc2-156">hello NSG contains a set of rules that allow inbound LDAPS access over TCP port 636 only from a specified set of IP addresses.</span></span> <span data-ttu-id="63dc2-157">hello 'DenyAll' Standardregeln gäller tooall andra inkommande trafik från hello internet.</span><span class="sxs-lookup"><span data-stu-id="63dc2-157">hello default 'DenyAll' rule applies tooall other inbound traffic from hello internet.</span></span> <span data-ttu-id="63dc2-158">hello NSG regeln tooallow LDAPS åtkomst via hello internet från den angivna IP-adresser har högre prioritet än hello DenyAll NSG regeln.</span><span class="sxs-lookup"><span data-stu-id="63dc2-158">hello NSG rule tooallow LDAPS access over hello internet from specified IP addresses has a higher priority than hello DenyAll NSG rule.</span></span>

![Exempel NSG toosecure LDAPS åtkomst via hello internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

<span data-ttu-id="63dc2-160">**Mer information** - [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="63dc2-160">**More information** - [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="63dc2-161">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="63dc2-161">Related content</span></span>
* [<span data-ttu-id="63dc2-162">Azure AD Domain Services - komma igång-guide</span><span class="sxs-lookup"><span data-stu-id="63dc2-162">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="63dc2-163">Administrera en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="63dc2-163">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="63dc2-164">Administrera Grupprincip i en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="63dc2-164">Administer Group Policy on an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-group-policy.md)
* [<span data-ttu-id="63dc2-165">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="63dc2-165">Network security groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="63dc2-166">Skapa en säkerhetsgrupp för nätverk</span><span class="sxs-lookup"><span data-stu-id="63dc2-166">Create a Network Security Group</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
