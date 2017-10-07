---
title: "aaaEnable Microsoft Windows Hello för företag i din organisation | Microsoft Docs"
description: Distribution instruktioner tooenable Microsoft Passport i din organisation.
services: active-directory
documentationcenter: 
keywords: "Konfigurera Microsoft Passport, Microsoft Windows Hello för företag-distribution"
author: MarkusVi
manager: femila
tags: azure-classic-portal
ms.assetid: 7dbbe3c6-1cd7-429c-a9b2-115fcbc02416
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.openlocfilehash: 6041f5916f606752bc55844b1b2d0a423b913cd3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a><span data-ttu-id="bb3d7-104">Aktivera Microsoft Windows Hello för företag i din organisation</span><span class="sxs-lookup"><span data-stu-id="bb3d7-104">Enable Microsoft Windows Hello for Business in your organization</span></span>
<span data-ttu-id="bb3d7-105">Efter [ansluta domänanslutna Windows 10-enheter med Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), hello följande tooenable Microsoft Windows Hello för företag i din organisation:</span><span class="sxs-lookup"><span data-stu-id="bb3d7-105">After [connecting Windows 10 domain-joined devices with Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), do hello following tooenable Microsoft Windows Hello for Business in your organization:</span></span>

1. <span data-ttu-id="bb3d7-106">Distribuera System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="bb3d7-106">Deploy System Center Configuration Manager</span></span>  
2. <span data-ttu-id="bb3d7-107">Konfigurera principinställningar</span><span class="sxs-lookup"><span data-stu-id="bb3d7-107">Configure policy settings</span></span>
3. <span data-ttu-id="bb3d7-108">Konfigurera hello certifikatprofil</span><span class="sxs-lookup"><span data-stu-id="bb3d7-108">Configure hello certificate profile</span></span>  

## <a name="deploy-system-center-configuration-manager"></a><span data-ttu-id="bb3d7-109">Distribuera System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="bb3d7-109">Deploy System Center Configuration Manager</span></span>
<span data-ttu-id="bb3d7-110">toodeploy användarcertifikat baserat på Windows Hello för företag nycklar, behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="bb3d7-110">toodeploy user certificates based on Windows Hello for Business keys, you need hello following:</span></span>

* <span data-ttu-id="bb3d7-111">**System Center Configuration Manager Current Branch** -du behöver tooinstall version 1606 eller bättre.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-111">**System Center Configuration Manager Current Branch** - You need tooinstall version 1606 or better.</span></span> <span data-ttu-id="bb3d7-112">Mer information finns i hello [dokumentation för System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) och [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb3d7-112">For more information, see hello [Documentation for System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) and [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).</span></span>
* <span data-ttu-id="bb3d7-113">**Infrastruktur för offentliga nycklar (PKI)** -tooenable Microsoft Windows Hello för företag med hjälp av certifikat, måste du ha en PKI på plats.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-113">**Public key infrastructure (PKI)** - tooenable Microsoft Windows Hello for Business by using user certificates, you must have a PKI in place.</span></span> <span data-ttu-id="bb3d7-114">Om du inte har någon, eller om du inte vill toouse det för användarcertifikat, kan du distribuera en ny domänkontrollant med Windows Server 2016 skapa 10551 (eller senare) installerat.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-114">If you don’t have one, or you don’t want toouse it for user certificates, you can deploy a new domain controller that has Windows Server 2016 build 10551 (or better) installed.</span></span> <span data-ttu-id="bb3d7-115">Gör hello för[installerar en replikeringsdomänkontrollant i en befintlig domän](https://technet.microsoft.com/library/jj574134.aspx) eller för[installera en ny Active Directory-skog, om du skapar en ny miljö](https://technet.microsoft.com/library/jj574166).</span><span class="sxs-lookup"><span data-stu-id="bb3d7-115">Follow hello steps too[install a replica domain controller in an existing domain](https://technet.microsoft.com/library/jj574134.aspx) or too[install a new Active Directory forest, if you're creating a new environment](https://technet.microsoft.com/library/jj574166).</span></span> <span data-ttu-id="bb3d7-116">(hello ISO: er är tillgängliga för hämtning på [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)</span><span class="sxs-lookup"><span data-stu-id="bb3d7-116">(hello ISOs are available for download on [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)</span></span>

## <a name="configure-policy-settings"></a><span data-ttu-id="bb3d7-117">Konfigurera principinställningar</span><span class="sxs-lookup"><span data-stu-id="bb3d7-117">Configure policy settings</span></span>
<span data-ttu-id="bb3d7-118">tooconfigure hello Microsoft Windows Hello för företag principinställningar har du två alternativ:</span><span class="sxs-lookup"><span data-stu-id="bb3d7-118">tooconfigure hello Microsoft Windows Hello for Business policy settings, you have two options:</span></span>

* <span data-ttu-id="bb3d7-119">Grupprincip i Active Directory</span><span class="sxs-lookup"><span data-stu-id="bb3d7-119">Group Policy in Active Directory</span></span> 
* <span data-ttu-id="bb3d7-120">hello System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="bb3d7-120">hello System Center Configuration Manager</span></span> 

<span data-ttu-id="bb3d7-121">Med en Grupprincip i Active Directory är hello rekommenderas metoden tooconfigure Microsoft Windows Hello för företag principinställningar.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-121">Using Group Policy in Active Directory is hello recommended method tooconfigure Microsoft Windows Hello for Business policy settings.</span></span> 

<span data-ttu-id="bb3d7-122">Med System Center Configuration Manager är hello önskade metoden när du också använda det toodeploy certifikat.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-122">Using System Center Configuration Manager is hello preferred method when you also use it toodeploy certificates.</span></span> <span data-ttu-id="bb3d7-123">Det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="bb3d7-123">This scenario:</span></span>

* <span data-ttu-id="bb3d7-124">Garanterar kompatibilitet med hello senare distributionsscenarier</span><span class="sxs-lookup"><span data-stu-id="bb3d7-124">Ensures compatibility with hello newer deployment scenarios</span></span>
* <span data-ttu-id="bb3d7-125">Kräver på klientsidan hello Windows 10 Version 1607 eller bättre.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-125">Requires on hello client side Windows 10 Version 1607 or better.</span></span>

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a><span data-ttu-id="bb3d7-126">Konfigurera Microsoft Windows Hello för företag via en Grupprincip i Active Directory</span><span class="sxs-lookup"><span data-stu-id="bb3d7-126">Configure Microsoft Windows Hello for Business via group policy in Active Directory</span></span>
<span data-ttu-id="bb3d7-127">**Steg**:</span><span class="sxs-lookup"><span data-stu-id="bb3d7-127">**Steps**:</span></span>

1. <span data-ttu-id="bb3d7-128">Öppna Serverhanteraren och navigera för**verktyg** > **Grupprinciphantering**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-128">Open Server Manager, and navigate too**Tools** > **Group Policy Management**.</span></span>
2. <span data-ttu-id="bb3d7-129">Navigera toohello domännod som motsvarar toohello domän där du vill tooenable Azure AD Join från hantering av Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-129">From Group Policy Management, navigate toohello domain node that corresponds toohello domain in which you want tooenable Azure AD Join.</span></span>
3. <span data-ttu-id="bb3d7-130">Högerklicka på **grupprincipobjekt**, och välj **ny**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-130">Right-click **Group Policy Objects**, and select **New**.</span></span> <span data-ttu-id="bb3d7-131">Namnge din grupprincipobjekt, till exempel aktivera Windows Hello för företag.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-131">Give your Group Policy Object a name, for example, Enable Windows Hello for Business.</span></span> <span data-ttu-id="bb3d7-132">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-132">Click **OK**.</span></span>
4. <span data-ttu-id="bb3d7-133">Högerklicka på din nya grupprincipobjektet och välj sedan **redigera**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-133">Right-click your new Group Policy Object, and then select **Edit**.</span></span>
5. <span data-ttu-id="bb3d7-134">Navigera för**Datorkonfiguration** > **principer** > **Administrationsmallar** > **Windows Komponenter** > **Windows Hello för företag**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-134">Navigate too**Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Windows Hello for Business**.</span></span>
6. <span data-ttu-id="bb3d7-135">Högerklicka på **aktivera Windows Hello för företag**, och välj sedan **redigera**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-135">Right-click **Enable Windows Hello for Business**, and then select **Edit**.</span></span>
7. <span data-ttu-id="bb3d7-136">Välj hello **aktiverad** alternativknapp och klicka sedan på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-136">Select hello **Enabled** option button, and then click **Apply**.</span></span> <span data-ttu-id="bb3d7-137">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-137">Click **OK**.</span></span>
8. <span data-ttu-id="bb3d7-138">Du kan nu koppla hello grupprincipobjekt tooa önskad plats.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-138">You can now link hello Group Policy Object tooa location of your choice.</span></span> <span data-ttu-id="bb3d7-139">tooenable principen för alla hello domänanslutna Windows 10-enheter i din organisation, länk hello Grupprincip toohello domän.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-139">tooenable this policy for all of hello domain-joined Windows 10 devices in your organization, link hello Group Policy toohello domain.</span></span> <span data-ttu-id="bb3d7-140">Exempel:</span><span class="sxs-lookup"><span data-stu-id="bb3d7-140">For example:</span></span>
   * <span data-ttu-id="bb3d7-141">En viss organisationsenhet (OU) i Active Directory där Windows 10-domänanslutna datorer kommer att finnas</span><span class="sxs-lookup"><span data-stu-id="bb3d7-141">A specific organizational unit (OU) in Active Directory where Windows 10 domain-joined computers will be located</span></span>
   * <span data-ttu-id="bb3d7-142">En viss säkerhetsgrupp som innehåller Windows 10-domänanslutna datorer som ska registreras automatiskt med Azure AD</span><span class="sxs-lookup"><span data-stu-id="bb3d7-142">A specific security group that contains Windows 10 domain-joined computers that will be automatically registered with Azure AD</span></span>

### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a><span data-ttu-id="bb3d7-143">Konfigurera Windows Hello för företag med System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="bb3d7-143">Configure Windows Hello for Business using System Center Configuration Manager</span></span>
<span data-ttu-id="bb3d7-144">**Steg**:</span><span class="sxs-lookup"><span data-stu-id="bb3d7-144">**Steps**:</span></span>

1. <span data-ttu-id="bb3d7-145">Öppna hello **System Center Configuration Manager**, och sedan gå för**tillgångar och efterlevnad > kompatibilitetsinställningar > åtkomst till företagsresurser > Windows Hello för företag-profiler**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-145">Open hello **System Center Configuration Manager**, and then navigate too**Assets & Compliance > Compliance Settings > Company Resource Access > Windows Hello for Business Profiles**.</span></span>
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/01.png)
2. <span data-ttu-id="bb3d7-147">Klicka i hello verktygsfältet hello längst upp **skapa Windows Hello för företag-profil**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-147">In hello toolbar on hello top, click **Create Windows Hello for business Profile**.</span></span>
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/02.png)
3. <span data-ttu-id="bb3d7-149">På hello **allmänna** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bb3d7-149">On hello **General** dialog, perform hello following steps:</span></span>
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/03.png)
   
    <span data-ttu-id="bb3d7-151">a.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-151">a.</span></span> <span data-ttu-id="bb3d7-152">I hello **namn** textruta, ange ett namn för din profil, till exempel **min WHfB profil**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-152">In hello **Name** textbox, type a name for your profile, for example, **My WHfB Profile**.</span></span>
   
    <span data-ttu-id="bb3d7-153">b.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-153">b.</span></span> <span data-ttu-id="bb3d7-154">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-154">Click **Next**.</span></span>
4. <span data-ttu-id="bb3d7-155">På hello **plattformar som stöds av** dialogrutan, Välj hello-plattformar som ska etableras med den här Windows Hello för företag-profilen och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-155">On hello **Supported Platforms** dialog, select hello platforms that will be provisioned with this Windows Hello for business profile, and then click **Next**.</span></span>
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/04.png)
5. <span data-ttu-id="bb3d7-157">På hello **inställningar** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bb3d7-157">On hello **Settings** dialog, perform hello following steps:</span></span>
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/05.png)
   
    <span data-ttu-id="bb3d7-159">a.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-159">a.</span></span> <span data-ttu-id="bb3d7-160">Som **konfigurera Windows Hello för företag**väljer **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-160">As **Configure Windows Hello for Business**, select **Enabled**.</span></span>
   
    <span data-ttu-id="bb3d7-161">b.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-161">b.</span></span> <span data-ttu-id="bb3d7-162">Som **använder en Trusted Platform Module (TPM)**väljer **krävs**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-162">As **Use a Trusted Platform Module (TPM)**, select **Required**.</span></span> 
   
    <span data-ttu-id="bb3d7-163">c.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-163">c.</span></span> <span data-ttu-id="bb3d7-164">Som **autentiseringsmetod**väljer **certifikatbaserad**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-164">As **Authentication method**, select **Certificate-based**.</span></span>
   
    <span data-ttu-id="bb3d7-165">d.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-165">d.</span></span> <span data-ttu-id="bb3d7-166">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-166">Click **Next**.</span></span>
6. <span data-ttu-id="bb3d7-167">På hello **sammanfattning** dialogrutan klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-167">On hello **Summary** dialog, click **Next**.</span></span>
7. <span data-ttu-id="bb3d7-168">På hello **slutförande** dialogrutan klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-168">On hello **Completion** dialog, click **Close**.</span></span>
8. <span data-ttu-id="bb3d7-169">Klicka i hello verktygsfältet hello längst upp **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-169">In hello toolbar on hello top, click **Deploy**.</span></span>
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/06.png)

## <a name="configure-hello-certificate-profile"></a><span data-ttu-id="bb3d7-171">Konfigurera hello certifikatprofil</span><span class="sxs-lookup"><span data-stu-id="bb3d7-171">Configure hello certificate profile</span></span>
<span data-ttu-id="bb3d7-172">Om du använder certifikatbaserad autentisering för lokal autentisering måste tooconfigure och distribuera en certifikatprofil.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-172">If you are using certificate-based authentication for on-premises authentication, you need tooconfigure and deploy a certificate profile.</span></span> <span data-ttu-id="bb3d7-173">Den här aktiviteten kräver tooset skapat en NDES-servern och Certifikatregistreringsplatsen platsroll i hello System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-173">This task requires you tooset up an NDES server and Certificate Registration Point site role in hello System Center Configuration Manager.</span></span> <span data-ttu-id="bb3d7-174">Mer information finns i hello [krav för Certifikatprofiler i Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).</span><span class="sxs-lookup"><span data-stu-id="bb3d7-174">For more details, see hello [Prerequisites for Certificate Profiles in Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).</span></span>

1. <span data-ttu-id="bb3d7-175">Öppna hello **System Center Configuration Manager**, och sedan gå för**tillgångar och efterlevnad > kompatibilitetsinställningar > åtkomst till företagsresurser > Certifikatprofiler**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-175">Open hello **System Center Configuration Manager**, and then navigate too**Assets & Compliance > Compliance Settings > Company Resource Access > Certificate Profiles**.</span></span>
2. <span data-ttu-id="bb3d7-176">Välj en mall som har smartkort-inloggning utökad nyckelanvändning (EKU).</span><span class="sxs-lookup"><span data-stu-id="bb3d7-176">Select a template that has Smart Card sign-in extended key usage (EKU).</span></span>

<span data-ttu-id="bb3d7-177">På hello **SCEP-registrering** sidan hello certifikat profil, behöver du toochoose **installera tooPassport for Work, rapportera annars fel** som hello **Nyckellagringsprovider**.</span><span class="sxs-lookup"><span data-stu-id="bb3d7-177">On hello **SCEP Enrollment** page of hello certificate profile, you need toochoose **Install tooPassport for Work otherwise fail** as hello **Key Storage Provider**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb3d7-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bb3d7-178">Next steps</span></span>
* [<span data-ttu-id="bb3d7-179">Windows 10 för hello företaget: sätt toouse enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="bb3d7-179">Windows 10 for hello enterprise: Ways toouse devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="bb3d7-180">Utöka molnet funktioner tooWindows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="bb3d7-180">Extending cloud capabilities tooWindows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="bb3d7-181">Autentisera identiteter utan lösenord via Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="bb3d7-181">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="bb3d7-182">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="bb3d7-182">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="bb3d7-183">Ansluta domänanslutna enheter tooAzure AD för Windows 10-upplevelser</span><span class="sxs-lookup"><span data-stu-id="bb3d7-183">Connect domain-joined devices tooAzure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="bb3d7-184">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="bb3d7-184">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

