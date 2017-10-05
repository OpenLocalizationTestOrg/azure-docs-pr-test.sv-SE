---
title: "Aktivera Microsoft Windows Hello för företag i din organisation | Microsoft Docs"
description: Stegvisa anvisningar om att aktivera Microsoft Passport i din organisation.
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
ms.openlocfilehash: 58943e1e29755c983e55c675dd4fe7b75ac47b34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a><span data-ttu-id="bcdf7-104">Aktivera Microsoft Windows Hello för företag i din organisation</span><span class="sxs-lookup"><span data-stu-id="bcdf7-104">Enable Microsoft Windows Hello for Business in your organization</span></span>
<span data-ttu-id="bcdf7-105">Efter [ansluta domänanslutna Windows 10-enheter med Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), gör följande för att aktivera Microsoft Windows Hello för företag i din organisation:</span><span class="sxs-lookup"><span data-stu-id="bcdf7-105">After [connecting Windows 10 domain-joined devices with Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), do the following to enable Microsoft Windows Hello for Business in your organization:</span></span>

1. <span data-ttu-id="bcdf7-106">Distribuera System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="bcdf7-106">Deploy System Center Configuration Manager</span></span>  
2. <span data-ttu-id="bcdf7-107">Konfigurera principinställningar</span><span class="sxs-lookup"><span data-stu-id="bcdf7-107">Configure policy settings</span></span>
3. <span data-ttu-id="bcdf7-108">Konfigurera certifikatprofilen</span><span class="sxs-lookup"><span data-stu-id="bcdf7-108">Configure the certificate profile</span></span>  

## <a name="deploy-system-center-configuration-manager"></a><span data-ttu-id="bcdf7-109">Distribuera System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="bcdf7-109">Deploy System Center Configuration Manager</span></span>
<span data-ttu-id="bcdf7-110">För att distribuera certifikat baserat på Windows Hello för företag nycklar, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="bcdf7-110">To deploy user certificates based on Windows Hello for Business keys, you need the following:</span></span>

* <span data-ttu-id="bcdf7-111">**System Center Configuration Manager Current Branch** -måste du installera versionen 1606 eller bättre.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-111">**System Center Configuration Manager Current Branch** - You need to install version 1606 or better.</span></span> <span data-ttu-id="bcdf7-112">Mer information finns i [dokumentation för System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) och [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).</span><span class="sxs-lookup"><span data-stu-id="bcdf7-112">For more information, see the [Documentation for System Center Configuration Manager](https://technet.microsoft.com/library/mt346023.aspx) and [System Center Configuration Manager Team Blog](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).</span></span>
* <span data-ttu-id="bcdf7-113">**Infrastruktur för offentliga nycklar (PKI)** – för att aktivera Microsoft Windows Hello för företag med hjälp av certifikat, du måste ha en PKI på plats.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-113">**Public key infrastructure (PKI)** - To enable Microsoft Windows Hello for Business by using user certificates, you must have a PKI in place.</span></span> <span data-ttu-id="bcdf7-114">Om du inte har någon, eller om du inte vill använda för användarcertifikat, kan du distribuera en ny domänkontrollant med Windows Server 2016 version 10551 (eller senare) installerat.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-114">If you don’t have one, or you don’t want to use it for user certificates, you can deploy a new domain controller that has Windows Server 2016 build 10551 (or better) installed.</span></span> <span data-ttu-id="bcdf7-115">Följ stegen för att [installerar en replikeringsdomänkontrollant i en befintlig domän](https://technet.microsoft.com/library/jj574134.aspx) eller [installera en ny Active Directory-skog, om du skapar en ny miljö](https://technet.microsoft.com/library/jj574166).</span><span class="sxs-lookup"><span data-stu-id="bcdf7-115">Follow the steps to [install a replica domain controller in an existing domain](https://technet.microsoft.com/library/jj574134.aspx) or to [install a new Active Directory forest, if you're creating a new environment](https://technet.microsoft.com/library/jj574166).</span></span> <span data-ttu-id="bcdf7-116">(De ISO: er är tillgängliga för hämtning på [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)</span><span class="sxs-lookup"><span data-stu-id="bcdf7-116">(The ISOs are available for download on [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)</span></span>

## <a name="configure-policy-settings"></a><span data-ttu-id="bcdf7-117">Konfigurera principinställningar</span><span class="sxs-lookup"><span data-stu-id="bcdf7-117">Configure policy settings</span></span>
<span data-ttu-id="bcdf7-118">Om du vill konfigurera den Microsoft Windows Hello för företag principinställningar, har du två alternativ:</span><span class="sxs-lookup"><span data-stu-id="bcdf7-118">To configure the Microsoft Windows Hello for Business policy settings, you have two options:</span></span>

* <span data-ttu-id="bcdf7-119">Grupprincip i Active Directory</span><span class="sxs-lookup"><span data-stu-id="bcdf7-119">Group Policy in Active Directory</span></span> 
* <span data-ttu-id="bcdf7-120">System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="bcdf7-120">The System Center Configuration Manager</span></span> 

<span data-ttu-id="bcdf7-121">Med en Grupprincip i Active Directory är den rekommenderade metoden för att konfigurera Microsoft Windows Hello för företag principinställningar.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-121">Using Group Policy in Active Directory is the recommended method to configure Microsoft Windows Hello for Business policy settings.</span></span> 

<span data-ttu-id="bcdf7-122">Med System Center Configuration Manager är den bästa metoden när du också använda det för att distribuera certifikat.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-122">Using System Center Configuration Manager is the preferred method when you also use it to deploy certificates.</span></span> <span data-ttu-id="bcdf7-123">Det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="bcdf7-123">This scenario:</span></span>

* <span data-ttu-id="bcdf7-124">Garanterar kompatibilitet med nyare distributionsscenarier</span><span class="sxs-lookup"><span data-stu-id="bcdf7-124">Ensures compatibility with the newer deployment scenarios</span></span>
* <span data-ttu-id="bcdf7-125">Kräver på klientsidan Windows 10 Version 1607 eller bättre.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-125">Requires on the client side Windows 10 Version 1607 or better.</span></span>

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a><span data-ttu-id="bcdf7-126">Konfigurera Microsoft Windows Hello för företag via en Grupprincip i Active Directory</span><span class="sxs-lookup"><span data-stu-id="bcdf7-126">Configure Microsoft Windows Hello for Business via group policy in Active Directory</span></span>
<span data-ttu-id="bcdf7-127">**Steg**:</span><span class="sxs-lookup"><span data-stu-id="bcdf7-127">**Steps**:</span></span>

1. <span data-ttu-id="bcdf7-128">Öppna Serverhanteraren och gå till **verktyg** > **Grupprinciphantering**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-128">Open Server Manager, and navigate to **Tools** > **Group Policy Management**.</span></span>
2. <span data-ttu-id="bcdf7-129">Från Grupprinciphantering, navigera till domännod som motsvarar den domän där du vill aktivera Azure AD Join.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-129">From Group Policy Management, navigate to the domain node that corresponds to the domain in which you want to enable Azure AD Join.</span></span>
3. <span data-ttu-id="bcdf7-130">Högerklicka på **grupprincipobjekt**, och välj **ny**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-130">Right-click **Group Policy Objects**, and select **New**.</span></span> <span data-ttu-id="bcdf7-131">Namnge din grupprincipobjekt, till exempel aktivera Windows Hello för företag.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-131">Give your Group Policy Object a name, for example, Enable Windows Hello for Business.</span></span> <span data-ttu-id="bcdf7-132">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-132">Click **OK**.</span></span>
4. <span data-ttu-id="bcdf7-133">Högerklicka på din nya grupprincipobjektet och välj sedan **redigera**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-133">Right-click your new Group Policy Object, and then select **Edit**.</span></span>
5. <span data-ttu-id="bcdf7-134">Gå till **Datorkonfiguration** > **principer** > **Administrationsmallar** > **Windows Komponenter** > **Windows Hello för företag**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-134">Navigate to **Computer Configuration** > **Policies** > **Administrative Templates** > **Windows Components** > **Windows Hello for Business**.</span></span>
6. <span data-ttu-id="bcdf7-135">Högerklicka på **aktivera Windows Hello för företag**, och välj sedan **redigera**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-135">Right-click **Enable Windows Hello for Business**, and then select **Edit**.</span></span>
7. <span data-ttu-id="bcdf7-136">Välj den **aktiverad** alternativknapp och klicka sedan på **tillämpa**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-136">Select the **Enabled** option button, and then click **Apply**.</span></span> <span data-ttu-id="bcdf7-137">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-137">Click **OK**.</span></span>
8. <span data-ttu-id="bcdf7-138">Du kan nu länka grupprincipobjektet till en plats.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-138">You can now link the Group Policy Object to a location of your choice.</span></span> <span data-ttu-id="bcdf7-139">Om du vill aktivera den här principen för alla domänanslutna Windows 10-enheter i din organisation, länka grupprincipen till domänen.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-139">To enable this policy for all of the domain-joined Windows 10 devices in your organization, link the Group Policy to the domain.</span></span> <span data-ttu-id="bcdf7-140">Exempel:</span><span class="sxs-lookup"><span data-stu-id="bcdf7-140">For example:</span></span>
   * <span data-ttu-id="bcdf7-141">En viss organisationsenhet (OU) i Active Directory där Windows 10-domänanslutna datorer kommer att finnas</span><span class="sxs-lookup"><span data-stu-id="bcdf7-141">A specific organizational unit (OU) in Active Directory where Windows 10 domain-joined computers will be located</span></span>
   * <span data-ttu-id="bcdf7-142">En viss säkerhetsgrupp som innehåller Windows 10-domänanslutna datorer som ska registreras automatiskt med Azure AD</span><span class="sxs-lookup"><span data-stu-id="bcdf7-142">A specific security group that contains Windows 10 domain-joined computers that will be automatically registered with Azure AD</span></span>

### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a><span data-ttu-id="bcdf7-143">Konfigurera Windows Hello för företag med System Center Configuration Manager</span><span class="sxs-lookup"><span data-stu-id="bcdf7-143">Configure Windows Hello for Business using System Center Configuration Manager</span></span>
<span data-ttu-id="bcdf7-144">**Steg**:</span><span class="sxs-lookup"><span data-stu-id="bcdf7-144">**Steps**:</span></span>

1. <span data-ttu-id="bcdf7-145">Öppna den **System Center Configuration Manager**, och gå sedan till **tillgångar och efterlevnad > kompatibilitetsinställningar > åtkomst till företagsresurser > Windows Hello för företag-profiler**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-145">Open the **System Center Configuration Manager**, and then navigate to **Assets & Compliance > Compliance Settings > Company Resource Access > Windows Hello for Business Profiles**.</span></span>
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/01.png)
2. <span data-ttu-id="bcdf7-147">Klicka på i verktygsfältet högst upp **skapa Windows Hello för företag-profil**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-147">In the toolbar on the top, click **Create Windows Hello for business Profile**.</span></span>
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/02.png)
3. <span data-ttu-id="bcdf7-149">På den **allmänna** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bcdf7-149">On the **General** dialog, perform the following steps:</span></span>
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/03.png)
   
    <span data-ttu-id="bcdf7-151">a.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-151">a.</span></span> <span data-ttu-id="bcdf7-152">I den **namn** textruta, ange ett namn för din profil, till exempel **min WHfB profil**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-152">In the **Name** textbox, type a name for your profile, for example, **My WHfB Profile**.</span></span>
   
    <span data-ttu-id="bcdf7-153">b.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-153">b.</span></span> <span data-ttu-id="bcdf7-154">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-154">Click **Next**.</span></span>
4. <span data-ttu-id="bcdf7-155">På den **plattformar som stöds av** dialogrutan Välj de plattformar som ska etableras med den här Windows Hello för företag-profilen och klickar sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-155">On the **Supported Platforms** dialog, select the platforms that will be provisioned with this Windows Hello for business profile, and then click **Next**.</span></span>
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/04.png)
5. <span data-ttu-id="bcdf7-157">På den **inställningar** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="bcdf7-157">On the **Settings** dialog, perform the following steps:</span></span>
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/05.png)
   
    <span data-ttu-id="bcdf7-159">a.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-159">a.</span></span> <span data-ttu-id="bcdf7-160">Som **konfigurera Windows Hello för företag**väljer **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-160">As **Configure Windows Hello for Business**, select **Enabled**.</span></span>
   
    <span data-ttu-id="bcdf7-161">b.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-161">b.</span></span> <span data-ttu-id="bcdf7-162">Som **använder en Trusted Platform Module (TPM)**väljer **krävs**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-162">As **Use a Trusted Platform Module (TPM)**, select **Required**.</span></span> 
   
    <span data-ttu-id="bcdf7-163">c.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-163">c.</span></span> <span data-ttu-id="bcdf7-164">Som **autentiseringsmetod**väljer **certifikatbaserad**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-164">As **Authentication method**, select **Certificate-based**.</span></span>
   
    <span data-ttu-id="bcdf7-165">d.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-165">d.</span></span> <span data-ttu-id="bcdf7-166">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-166">Click **Next**.</span></span>
6. <span data-ttu-id="bcdf7-167">På den **sammanfattning** dialogrutan klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-167">On the **Summary** dialog, click **Next**.</span></span>
7. <span data-ttu-id="bcdf7-168">På den **slutförande** dialogrutan klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-168">On the **Completion** dialog, click **Close**.</span></span>
8. <span data-ttu-id="bcdf7-169">Klicka på i verktygsfältet högst upp **distribuera**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-169">In the toolbar on the top, click **Deploy**.</span></span>
   
    ![Konfigurera Windows Hello för företag](./media/active-directory-azureadjoin-passport-deployment/06.png)

## <a name="configure-the-certificate-profile"></a><span data-ttu-id="bcdf7-171">Konfigurera certifikatprofilen</span><span class="sxs-lookup"><span data-stu-id="bcdf7-171">Configure the certificate profile</span></span>
<span data-ttu-id="bcdf7-172">Om du använder certifikatbaserad autentisering för lokal autentisering måste du konfigurera och distribuera en certifikatprofil.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-172">If you are using certificate-based authentication for on-premises authentication, you need to configure and deploy a certificate profile.</span></span> <span data-ttu-id="bcdf7-173">Den här uppgiften måste du ställa in en NDES-servern och Certifikatregistreringsplatsen platsroll i System Center Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-173">This task requires you to set up an NDES server and Certificate Registration Point site role in the System Center Configuration Manager.</span></span> <span data-ttu-id="bcdf7-174">Mer information finns i [krav för Certifikatprofiler i Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).</span><span class="sxs-lookup"><span data-stu-id="bcdf7-174">For more details, see the [Prerequisites for Certificate Profiles in Configuration Manager](https://technet.microsoft.com/library/dn261205.aspx).</span></span>

1. <span data-ttu-id="bcdf7-175">Öppna den **System Center Configuration Manager**, och gå sedan till **tillgångar och efterlevnad > kompatibilitetsinställningar > åtkomst till företagsresurser > Certifikatprofiler**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-175">Open the **System Center Configuration Manager**, and then navigate to **Assets & Compliance > Compliance Settings > Company Resource Access > Certificate Profiles**.</span></span>
2. <span data-ttu-id="bcdf7-176">Välj en mall som har smartkort-inloggning utökad nyckelanvändning (EKU).</span><span class="sxs-lookup"><span data-stu-id="bcdf7-176">Select a template that has Smart Card sign-in extended key usage (EKU).</span></span>

<span data-ttu-id="bcdf7-177">På den **SCEP-registrering** sidan av certifikatprofil, måste du välja **installera på Passport for Work, rapportera annars fel** som den **Nyckellagringsprovider**.</span><span class="sxs-lookup"><span data-stu-id="bcdf7-177">On the **SCEP Enrollment** page of the certificate profile, you need to choose **Install to Passport for Work otherwise fail** as the **Key Storage Provider**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bcdf7-178">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bcdf7-178">Next steps</span></span>
* [<span data-ttu-id="bcdf7-179">Windows 10 för företaget: Sätt att använda enheter för arbete</span><span class="sxs-lookup"><span data-stu-id="bcdf7-179">Windows 10 for the enterprise: Ways to use devices for work</span></span>](active-directory-azureadjoin-windows10-devices-overview.md)
* [<span data-ttu-id="bcdf7-180">Utöka molnkapaciteten till Windows 10-enheter via Azure Active Directory Join</span><span class="sxs-lookup"><span data-stu-id="bcdf7-180">Extending cloud capabilities to Windows 10 devices through Azure Active Directory Join</span></span>](active-directory-azureadjoin-user-upgrade.md)
* [<span data-ttu-id="bcdf7-181">Autentisera identiteter utan lösenord via Microsoft Passport</span><span class="sxs-lookup"><span data-stu-id="bcdf7-181">Authenticating identities without passwords through Microsoft Passport</span></span>](active-directory-azureadjoin-passport.md)
* [<span data-ttu-id="bcdf7-182">Läs mer om användningsscenarier för Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="bcdf7-182">Learn about usage scenarios for Azure AD Join</span></span>](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [<span data-ttu-id="bcdf7-183">Ansluta domänanslutna enheter till miljöer med Azure AD och Windows 10</span><span class="sxs-lookup"><span data-stu-id="bcdf7-183">Connect domain-joined devices to Azure AD for Windows 10 experiences</span></span>](active-directory-azureadjoin-devices-group-policy.md)
* [<span data-ttu-id="bcdf7-184">Konfigurera Azure AD Join</span><span class="sxs-lookup"><span data-stu-id="bcdf7-184">Set up Azure AD Join</span></span>](active-directory-azureadjoin-setup.md)

