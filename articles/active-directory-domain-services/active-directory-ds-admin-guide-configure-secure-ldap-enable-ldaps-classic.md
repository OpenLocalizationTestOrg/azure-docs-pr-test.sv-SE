---
title: "aaaConfigure säker LDAP (LDAPS) i Azure AD Domain Services | Microsoft Docs"
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
ms.openlocfilehash: a0d6e2faf474b1f0cbe157fb4ae2754b1d521ef9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="4d2bb-103">Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="4d2bb-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="4d2bb-104">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="4d2bb-104">Before you begin</span></span>
<span data-ttu-id="4d2bb-105">Se till att du har slutfört [uppgift 2 - export hello säker LDAP certifikat tooa. PFX-filen](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span><span class="sxs-lookup"><span data-stu-id="4d2bb-105">Ensure you've completed [Task 2 - export hello secure LDAP certificate tooa .PFX file](active-directory-ds-admin-guide-configure-secure-ldap-export-pfx.md).</span></span>

<span data-ttu-id="4d2bb-106">Välj om toouse hello Förhandsgranska Azure-portaler eller hello Azure klassiska portal toocomplete den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-106">Choose whether toouse hello preview Azure portal experience or hello Azure classic portal toocomplete this task.</span></span>
> [!div class="op_single_selector"]
> * <span data-ttu-id="4d2bb-107">**Azure-portalen (förhandsgranskning)**: [aktivera säkert LDAP med hello Azure-portalen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="4d2bb-107">**Azure portal (Preview)**: [Enable secure LDAP using hello Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
> * <span data-ttu-id="4d2bb-108">**Klassiska Azure-portalen**: [aktivera säkert LDAP med hello klassiska Azure-portalen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span><span class="sxs-lookup"><span data-stu-id="4d2bb-108">**Azure classic portal**: [Enable secure LDAP using hello classic Azure portal](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps-classic.md)</span></span>
>
>


## <a name="task-3---enable-secure-ldap-for-hello-managed-domain-using-hello-classic-azure-portal"></a><span data-ttu-id="4d2bb-109">Uppgift 3 – aktivera säker LDAP för hello hanterade domänen med hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="4d2bb-109">Task 3 - enable secure LDAP for hello managed domain using hello classic Azure portal</span></span>
<span data-ttu-id="4d2bb-110">tooenable säkert LDAP, utför följande konfigurationssteg hello:</span><span class="sxs-lookup"><span data-stu-id="4d2bb-110">tooenable secure LDAP, perform hello following configuration steps:</span></span>

1. <span data-ttu-id="4d2bb-111">Navigera toohello  **[klassiska Azure-portalen](https://manage.windowsazure.com)**.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-111">Navigate toohello **[Azure classic portal](https://manage.windowsazure.com)**.</span></span>
2. <span data-ttu-id="4d2bb-112">Välj hello **Active Directory** i hello vänstra fönstret.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-112">Select hello **Active Directory** node on hello left pane.</span></span>
3. <span data-ttu-id="4d2bb-113">Välj hello Azure AD-katalog (även hänvisade tooas ”klient”), som du har aktiverat Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-113">Select hello Azure AD directory (also referred tooas 'tenant'), for which you have enabled Azure AD Domain Services.</span></span>

    ![Välja Azure AD-katalog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="4d2bb-115">Klicka på hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-115">Click hello **Configure** tab.</span></span>

    ![Fliken Konfigurera för katalogen](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. <span data-ttu-id="4d2bb-117">Bläddra nedåt toohello avsnittet **domäntjänster**.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-117">Scroll down toohello section titled **domain services**.</span></span> <span data-ttu-id="4d2bb-118">Du bör se en alternativet **säker LDAP (LDAPS)** som visas i följande skärmbild hello:</span><span class="sxs-lookup"><span data-stu-id="4d2bb-118">You should see an option titled **Secure LDAP (LDAPS)** as shown in hello following screenshot:</span></span>

    ![Konfigurationsavsnittet Domäntjänster](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)
6. <span data-ttu-id="4d2bb-120">Klicka på hello **konfigurera certifikat...**  knappen toobring in hello **konfigurera certifikat för säker LDAP** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-120">Click hello **Configure certificate ...** button toobring up hello **Configure Certificate for Secure LDAP** dialog.</span></span>

    ![Konfigurera certifikat för säker LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)
7. <span data-ttu-id="4d2bb-122">Klicka på hello mappen ikonen följande **PFX-filen med certifikat** toospecify hello PFX-fil som innehåller hello-certifikat som du vill toouse för säker LDAP åtkomst toohello hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-122">Click hello folder icon following **PFX FILE WITH CERTIFICATE** toospecify hello PFX file, which contains hello certificate you wish toouse for secure LDAP access toohello managed domain.</span></span> <span data-ttu-id="4d2bb-123">Ange också hello lösenordet du angav när du exporterar hello certifikatets toohello PFX-fil.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-123">Also enter hello password you specified when exporting hello certificate toohello PFX file.</span></span> <span data-ttu-id="4d2bb-124">Klicka på hello klar hello ned-knappen.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-124">Then, click hello done button on hello bottom.</span></span>

    ![Ange säker LDAP PFX-filen och lösenord](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)
8. <span data-ttu-id="4d2bb-126">Hej **domäntjänster** avsnitt i hello **konfigurera** ska hämta nedtonad och är i hello **väntande...**  tillstånd om en stund.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-126">hello **domain services** section of hello **Configure** tab should get grayed out and is in hello **Pending...** state for a few minutes.</span></span> <span data-ttu-id="4d2bb-127">Under denna tid hello LDAPS certifikat har verifierats för Precision och säker LDAP har konfigurerats för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-127">During this period, hello LDAPS certificate is verified for accuracy and secure LDAP is configured for your managed domain.</span></span>

    ![Säkert LDAP - väntetillstånd](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

   > [!NOTE]
   > <span data-ttu-id="4d2bb-129">Det tar cirka 10 too15 minuter tooenable säkert LDAP för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-129">It takes about 10 too15 minutes tooenable secure LDAP for your managed domain.</span></span> <span data-ttu-id="4d2bb-130">Om hello säker LDAP-certifikat inte matchar hello krävs kriterier, säker LDAP har inte aktiverats för din katalog och du ser ett fel.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-130">If hello provided secure LDAP certificate does not match hello required criteria, secure LDAP is not enabled for your directory and you see a failure.</span></span> <span data-ttu-id="4d2bb-131">Till exempel hello domännamn är felaktig, hello certifikatet redan har upphört att gälla eller upphör snart att gälla.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-131">For example, hello domain name is incorrect, hello certificate has already expired or expires soon.</span></span>
   >
   >

9. <span data-ttu-id="4d2bb-132">När säker LDAP har aktiverats för din hanterade domän, hello **väntande...**  meddelandet ska försvinner.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-132">When secure LDAP is successfully enabled for your managed domain, hello **Pending...** message should disappear.</span></span> <span data-ttu-id="4d2bb-133">Du bör se hello tumavtrycket för hello certifikat visas.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-133">You should see hello thumbprint of hello certificate displayed.</span></span>

    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>

## <a name="task-4---enable-secure-ldap-access-over-hello-internet"></a><span data-ttu-id="4d2bb-135">Uppgift 4 – aktivera säker LDAP åtkomst över hello internet</span><span class="sxs-lookup"><span data-stu-id="4d2bb-135">Task 4 - enable secure LDAP access over hello internet</span></span>
<span data-ttu-id="4d2bb-136">**Frivillig uppgift** - om du inte planerar tooaccess hello hanterade domänen med LDAPS över Hej internet, hoppa över det här konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-136">**Optional task** - If you do not plan tooaccess hello managed domain using LDAPS over hello internet, skip this configuration task.</span></span>

<span data-ttu-id="4d2bb-137">Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen i hello [uppgift 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).</span><span class="sxs-lookup"><span data-stu-id="4d2bb-137">Before you begin this task, ensure you have completed hello steps outlined in [Task 3](#task-3---enable-secure-ldap-for-the-managed-domain-using-the-classic-azure-portal).</span></span>

1. <span data-ttu-id="4d2bb-138">Du bör se ett alternativ för**hello aktivera säker LDAP åtkomst via INTERNET** i hello **domäntjänster** avsnitt i hello **konfigurera** sidan.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-138">You should see an option too**ENABLE SECURE LDAP ACCESS OVER hello INTERNET** in hello **domain services** section of hello **Configure** page.</span></span> <span data-ttu-id="4d2bb-139">Det här alternativet anges för**nr** som standard eftersom internet access toohello hanterade domänen via säker LDAP är inaktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-139">This option is set too**NO** by default since internet access toohello managed domain over secure LDAP is disabled by default.</span></span>

    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)
2. <span data-ttu-id="4d2bb-141">Växla **hello aktivera säker LDAP åtkomst via INTERNET** för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-141">Toggle **ENABLE SECURE LDAP ACCESS OVER hello INTERNET** too**YES**.</span></span> <span data-ttu-id="4d2bb-142">Klicka på hello **spara** hello nedre panelen-knappen.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-142">Click hello **SAVE** button on hello bottom panel.</span></span>
    <span data-ttu-id="4d2bb-143">![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)</span><span class="sxs-lookup"><span data-stu-id="4d2bb-143">![Secure LDAP enabled](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)</span></span>
3. <span data-ttu-id="4d2bb-144">Hej **domäntjänster** avsnitt i hello **konfigurera** ska hämta nedtonad och är i hello **väntande...**  tillstånd om en stund.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-144">hello **domain services** section of hello **Configure** tab should get grayed out and is in hello **Pending...** state for a few minutes.</span></span> <span data-ttu-id="4d2bb-145">Efter en stund är åtkomst tooyour hanterade Internetdomän över säker LDAP aktiverat.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-145">After some time, internet access tooyour managed domain over secure LDAP is enabled.</span></span>

    ![Säkert LDAP - väntetillstånd](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

   > [!NOTE]
   > <span data-ttu-id="4d2bb-147">Det tar cirka 10 minuter tooenable Internetåtkomst via säker LDAP för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-147">It takes about 10 minutes tooenable internet access over secure LDAP for your managed domain.</span></span>
   >
   >
4. <span data-ttu-id="4d2bb-148">När säker LDAP åtkomst tooyour hanterade domänen via hello internet har aktiverats, hello **väntande...**  meddelandet ska försvinner.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-148">When secure LDAP access tooyour managed domain over hello internet is successfully enabled, hello **Pending...** message should disappear.</span></span> <span data-ttu-id="4d2bb-149">Du bör se hello externa IP-adress som kan vara används tooaccess din katalog över LDAPS hello fältet **extern IP-adress för LDAPS åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-149">You should see hello external IP address that can be used tooaccess your directory over LDAPS in hello field **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

    ![Säkert LDAP aktiverad](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-tooaccess-hello-managed-domain-from-hello-internet"></a><span data-ttu-id="4d2bb-151">Uppgift 5 – konfigurera DNS-tooaccess hello hanterade domänen från hello internet</span><span class="sxs-lookup"><span data-stu-id="4d2bb-151">Task 5 - configure DNS tooaccess hello managed domain from hello internet</span></span>
<span data-ttu-id="4d2bb-152">**Frivillig uppgift** - om du inte planerar tooaccess hello hanterade domänen med LDAPS över Hej internet, hoppa över det här konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-152">**Optional task** - If you do not plan tooaccess hello managed domain using LDAPS over hello internet, skip this configuration task.</span></span>

<span data-ttu-id="4d2bb-153">Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen i hello [uppgift 4](#task-4---enable-secure-ldap-access-over-the-internet).</span><span class="sxs-lookup"><span data-stu-id="4d2bb-153">Before you begin this task, ensure you have completed hello steps outlined in [Task 4](#task-4---enable-secure-ldap-access-over-the-internet).</span></span>

<span data-ttu-id="4d2bb-154">När du har aktiverat säker LDAP-åtkomst via hello internet för din hanterade domän måste tooupdate DNS så att klientdatorerna kan hitta den här hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-154">Once you have enabled secure LDAP access over hello internet for your managed domain, you need tooupdate DNS so that client computers can find this managed domain.</span></span> <span data-ttu-id="4d2bb-155">Hello slutet av uppgift 4, visas en extern IP-adress på hello **konfigurera** fliken i **extern IP-adress för LDAPS åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-155">At hello end of task 4, an external IP address is displayed on hello **Configure** tab in **EXTERNAL IP ADDRESS FOR LDAPS ACCESS**.</span></span>

<span data-ttu-id="4d2bb-156">Konfigurera externa DNS-providern så att hello DNS-namnet på hello hanterade domän (till exempel ldaps.contoso100.com) pekar toothis extern IP-adress.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-156">Configure your external DNS provider so that hello DNS name of hello managed domain (for example, 'ldaps.contoso100.com') points toothis external IP address.</span></span> <span data-ttu-id="4d2bb-157">I vårt exempel behöver vi toocreate hello följande DNS-post:</span><span class="sxs-lookup"><span data-stu-id="4d2bb-157">In our example, we need toocreate hello following DNS entry:</span></span>

    ldaps.contoso100.com  -> 52.165.38.113

<span data-ttu-id="4d2bb-158">Det – nu är du redo tooconnect toohello hanterade domänen med hjälp av säker LDAP över hello internet.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-158">That's it - you are now ready tooconnect toohello managed domain using secure LDAP over hello internet.</span></span>

> [!WARNING]
> <span data-ttu-id="4d2bb-159">Kom ihåg att klientdatorer måste lita hello utfärdaren av hello LDAPS certifikat toobe kan tooconnect har toohello hanterade domänen med LDAPS.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-159">Remember that client computers must trust hello issuer of hello LDAPS certificate toobe able tooconnect successfully toohello managed domain using LDAPS.</span></span> <span data-ttu-id="4d2bb-160">Om du använder en företagscertifikatutfärdare eller en betrodd offentlig certifikatutfärdare, behöver du inte toodo allt eftersom dessa certifikatutfärdare litar på klientdatorer.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-160">If you are using an enterprise certification authority or a publicly trusted certification authority, you do not need toodo anything since client computers trust these certificate issuers.</span></span> <span data-ttu-id="4d2bb-161">Om du använder ett självsignerat certifikat måste tooinstall hello offentliga tillhör hello självsignerat certifikat till hello betrodda certifikatarkiv på hello-klientdator.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-161">If you are using a self-signed certificate, you need tooinstall hello public part of hello self-signed certificate into hello trusted certificate store on hello client computer.</span></span>
>
>


## <a name="lock-down-ldaps-access-tooyour-managed-domain-over-hello-internet"></a><span data-ttu-id="4d2bb-162">Låsning LDAPS komma åt tooyour hanterade domänen via hello internet</span><span class="sxs-lookup"><span data-stu-id="4d2bb-162">Lock-down LDAPS access tooyour managed domain over hello internet</span></span>
> [!NOTE]
> <span data-ttu-id="4d2bb-163">**Frivillig uppgift** - om du inte har aktiverat LDAPS åtkomst toohello hanterad domän över Hej internet, hoppa över det här konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-163">**Optional task** - If you have not enabled LDAPS access toohello managed domain over hello internet, skip this configuration task.</span></span>
>
>

<span data-ttu-id="4d2bb-164">Innan du börjar den här uppgiften kan du kontrollera att du har slutfört stegen i hello [uppgift 4](#task-4---enable-secure-ldap-access-over-the-internet).</span><span class="sxs-lookup"><span data-stu-id="4d2bb-164">Before you begin this task, ensure you have completed hello steps outlined in [Task 4](#task-4---enable-secure-ldap-access-over-the-internet).</span></span>

<span data-ttu-id="4d2bb-165">Visa din hanterade domän för LDAPS åtkomst över hello representerar internet en säkerhetsrisk.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-165">Exposing your managed domain for LDAPS access over hello internet represents a security threat.</span></span> <span data-ttu-id="4d2bb-166">hello hanterad domän kan nås från hello internet på hello-port som används för säker LDAP (det vill säga port 636).</span><span class="sxs-lookup"><span data-stu-id="4d2bb-166">hello managed domain is reachable from hello internet at hello port used for secure LDAP (that is, port 636).</span></span> <span data-ttu-id="4d2bb-167">Därför kan du välja toorestrict åtkomst toohello hanterade domänen toospecific kända IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-167">Therefore, you can choose toorestrict access toohello managed domain toospecific known IP addresses.</span></span> <span data-ttu-id="4d2bb-168">För förbättrad säkerhet skapar en nätverkssäkerhetsgrupp (NSG) och associera den med hello undernät där du har aktiverat Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-168">For improved security, create a network security group (NSG) and associate it with hello subnet where you have enabled Azure AD Domain Services.</span></span>

<span data-ttu-id="4d2bb-169">hello i tabellen nedan visas ett exempel på en NSG som du kan konfigurera toolock ned säker LDAP åtkomst via hello internet.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-169">hello following table illustrates a sample NSG you can configure, toolock down secure LDAP access over hello internet.</span></span> <span data-ttu-id="4d2bb-170">hello NSG innehåller en uppsättning regler som tillåter inkommande LDAPS åtkomst via TCP-port 636 endast från en angiven mängd av IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-170">hello NSG contains a set of rules that allow inbound LDAPS access over TCP port 636 only from a specified set of IP addresses.</span></span> <span data-ttu-id="4d2bb-171">hello 'DenyAll' Standardregeln gäller tooall andra inkommande trafik från hello internet.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-171">hello default 'DenyAll' rule applies tooall other inbound traffic from hello internet.</span></span> <span data-ttu-id="4d2bb-172">hello NSG regeln tooallow LDAPS åtkomst via hello internet från den angivna IP-adresser har högre prioritet än hello DenyAll NSG regeln.</span><span class="sxs-lookup"><span data-stu-id="4d2bb-172">hello NSG rule tooallow LDAPS access over hello internet from specified IP addresses has a higher priority than hello DenyAll NSG rule.</span></span>

![Exempel NSG toosecure LDAPS åtkomst via hello internet](./media/active-directory-domain-services-admin-guide/secure-ldap-sample-nsg.png)

<span data-ttu-id="4d2bb-174">**Mer information** - [Nätverkssäkerhetsgrupper](../virtual-network/virtual-networks-nsg.md).</span><span class="sxs-lookup"><span data-stu-id="4d2bb-174">**More information** - [Network security groups](../virtual-network/virtual-networks-nsg.md).</span></span>

<br>

## <a name="related-content"></a><span data-ttu-id="4d2bb-175">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="4d2bb-175">Related content</span></span>
* [<span data-ttu-id="4d2bb-176">Azure AD Domain Services - komma igång-guide</span><span class="sxs-lookup"><span data-stu-id="4d2bb-176">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="4d2bb-177">Administrera en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="4d2bb-177">Administer an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="4d2bb-178">Administrera Grupprincip i en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="4d2bb-178">Administer Group Policy on an Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-administer-group-policy.md)
* [<span data-ttu-id="4d2bb-179">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="4d2bb-179">Network security groups</span></span>](../virtual-network/virtual-networks-nsg.md)
* [<span data-ttu-id="4d2bb-180">Skapa en säkerhetsgrupp för nätverk</span><span class="sxs-lookup"><span data-stu-id="4d2bb-180">Create a Network Security Group</span></span>](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)
