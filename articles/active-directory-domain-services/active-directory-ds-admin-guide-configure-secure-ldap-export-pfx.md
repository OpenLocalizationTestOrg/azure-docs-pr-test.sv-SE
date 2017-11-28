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
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: 356b28f8392b0e203df9c81177ec842d52866c4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="fcfa8-103">Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="fcfa8-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="fcfa8-104">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="fcfa8-104">Before you begin</span></span>
<span data-ttu-id="fcfa8-105">Se till att du har slutfört [uppgift 1 – skaffa ett certifikat för säker LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).</span><span class="sxs-lookup"><span data-stu-id="fcfa8-105">Ensure you've completed [Task 1 - obtain a certificate for secure LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).</span></span>


## <a name="task-2---export-hello-secure-ldap-certificate-tooa-pfx-file"></a><span data-ttu-id="fcfa8-106">Uppgift 2 – exportera hello säker LDAP certifikat tooa. PFX-fil</span><span class="sxs-lookup"><span data-stu-id="fcfa8-106">Task 2 - export hello secure LDAP certificate tooa .PFX file</span></span>
<span data-ttu-id="fcfa8-107">Innan du börjar den här uppgiften, se till att du har fått hello säker LDAP-certifikat från en offentlig certifikatutfärdare har skapat ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-107">Before you start this task, ensure that you have obtained hello secure LDAP certificate from a public certification authority or have created a self-signed certificate.</span></span>

<span data-ttu-id="fcfa8-108">Utför följande steg hello kan tooexport hello LDAPS certifikat tooa. PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-108">Perform hello following steps, tooexport hello LDAPS certificate tooa .PFX file.</span></span>

1. <span data-ttu-id="fcfa8-109">Tryck på hello **starta** knappen och skriv **R**. I hello **kör** dialogrutan, ange **mmc** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-109">Press hello **Start** button and type **R**. In hello **Run** dialog, type **mmc** and click **OK**.</span></span>

    ![Starta hello MMC-konsol](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. <span data-ttu-id="fcfa8-111">På hello **User Account Control** fråga klickar du på **Ja** toolaunch MMC (Microsoft Management Console) som administratör.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-111">On hello **User Account Control** prompt, click **YES** toolaunch MMC (Microsoft Management Console) as administrator.</span></span>
3. <span data-ttu-id="fcfa8-112">Från hello **filen** -menyn klickar du på **Lägg till/ta bort snapin-modulen...** .</span><span class="sxs-lookup"><span data-stu-id="fcfa8-112">From hello **File** menu, click **Add/Remove Snap-in...**.</span></span>

    ![Lägga till snapin-modulen tooMMC konsolen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. <span data-ttu-id="fcfa8-114">I hello **lägga till eller ta bort snapin-moduler** dialogrutan, Välj hello **certifikat** snapin-modulen och klicka på hello **Lägg till >** knappen.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-114">In hello **Add or Remove Snap-ins** dialog, select hello **Certificates** snap-in, and click hello **Add >** button.</span></span>

    ![Lägga till snapin-modulen tooMMC certifikatkonsolen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. <span data-ttu-id="fcfa8-116">I hello **snapin-modulen Certifikat** väljer **datorkonto** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-116">In hello **Certificates snap-in** wizard, select **Computer account** and click **Next**.</span></span>

    ![Lägga till snapin-modulen certifikat för datorkontot](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. <span data-ttu-id="fcfa8-118">På hello **Välj dator** väljer **lokal dator: (hello dator som den här konsolen körs)** och på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-118">On hello **Select Computer** page, select **Local computer: (hello computer this console is running on)** and click **Finish**.</span></span>

    ![Lägga till snapin-modulen Certifikat - Välj dator](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. <span data-ttu-id="fcfa8-120">I hello **lägga till eller ta bort snapin-moduler** dialogrutan klickar du på **OK** tooadd hello certifikat snapin-modulen tooMMC.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-120">In hello **Add or Remove Snap-ins** dialog, click **OK** tooadd hello certificates snap-in tooMMC.</span></span>

    ![Lägg till certifikat snapin-modulen tooMMC - klar](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. <span data-ttu-id="fcfa8-122">Klicka på tooexpand i hello MMC **Konsolrot**.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-122">In hello MMC window, click tooexpand **Console Root**.</span></span> <span data-ttu-id="fcfa8-123">Du bör se hello snapin-modulen certifikat har lästs in.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-123">You should see hello Certificates snap-in loaded.</span></span> <span data-ttu-id="fcfa8-124">Klicka på **certifikat (lokal dator)** tooexpand.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-124">Click **Certificates (Local Computer)** tooexpand.</span></span> <span data-ttu-id="fcfa8-125">Klicka på tooexpand hello **personliga** nod, följt av hello **certifikat** nod.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-125">Click tooexpand hello **Personal** node, followed by hello **Certificates** node.</span></span>

    ![Öppna personliga certifikat](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. <span data-ttu-id="fcfa8-127">Du bör se hello självsignerat certifikat som vi skapade.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-127">You should see hello self-signed certificate we created.</span></span> <span data-ttu-id="fcfa8-128">Du kan undersöka hello egenskaper för hello certifikat tooensure hello tumavtryck matchar rapporteras på hello PowerShell windows när du skapade hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-128">You can examine hello properties of hello certificate tooensure hello thumbprint matches that reported on hello PowerShell windows when you created hello certificate.</span></span>
10. <span data-ttu-id="fcfa8-129">Välj hello självsignerat certifikat och **Högerklicka**.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-129">Select hello self-signed certificate and **right click**.</span></span> <span data-ttu-id="fcfa8-130">Välj hello på snabbmenyn **alla aktiviteter** och välj **exportera...** .</span><span class="sxs-lookup"><span data-stu-id="fcfa8-130">From hello right-click menu, select **All Tasks** and select **Export...**.</span></span>

    ![Exportera certifikat](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. <span data-ttu-id="fcfa8-132">I hello **guiden Exportera certifikat**, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-132">In hello **Certificate Export Wizard**, click **Next**.</span></span>

    ![Exportera certifikat](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. <span data-ttu-id="fcfa8-134">På hello **Exportera privat nyckel** väljer **Ja, exportera privat nyckel för hello**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-134">On hello **Export Private Key** page, select **Yes, export hello private key**, and click **Next**.</span></span>

    ![Exportera certifikatets privata nyckel](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > <span data-ttu-id="fcfa8-136">Du måste exportera hello privata nyckeln tillsammans med hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-136">You MUST export hello private key along with hello certificate.</span></span> <span data-ttu-id="fcfa8-137">Om du anger en PFX som inte innehåller hello privata nyckeln för certifikatet för hello misslyckas aktivera säker LDAP för din hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-137">If you provide a PFX that does not contain hello private key for hello certificate, enabling secure LDAP for your managed domain fails.</span></span>
    >
    >
13. <span data-ttu-id="fcfa8-138">På hello **filformat för Export** väljer **Personal Information Exchange – PKCS #12 (. PFX)** hello filformat för hello exporteras certifikat.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-138">On hello **Export File Format** page, select **Personal Information Exchange - PKCS #12 (.PFX)** as hello file format for hello exported certificate.</span></span>

    ![Exportfilformat för certifikat](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > <span data-ttu-id="fcfa8-140">Endast hello. PFX-filformatet stöds.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-140">Only hello .PFX file format is supported.</span></span> <span data-ttu-id="fcfa8-141">Exportera inte hello certifikat toohello. CER-filformat.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-141">Do not export hello certificate toohello .CER file format.</span></span>
    >
    >
14. <span data-ttu-id="fcfa8-142">På hello **säkerhet** sidan, Välj hello **lösenord** alternativet och anger ett lösenord tooprotect hello. PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-142">On hello **Security** page, select hello **Password** option and type in a password tooprotect hello .PFX file.</span></span> <span data-ttu-id="fcfa8-143">Kom ihåg detta lösenord eftersom det krävs i hello nästa uppgift.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-143">Remember this password since it will be needed in hello next task.</span></span> <span data-ttu-id="fcfa8-144">Klicka på **nästa** tooproceed.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-144">Click **Next** tooproceed.</span></span>

    ![<span data-ttu-id="fcfa8-145">Lösenordet för certifikatexport</span><span class="sxs-lookup"><span data-stu-id="fcfa8-145">Password for certificate export</span></span> ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > <span data-ttu-id="fcfa8-146">Anteckna det här lösenordet.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-146">Make a note of this password.</span></span> <span data-ttu-id="fcfa8-147">Du behöver det för att aktivera säker LDAP för den här hanterade domänen i [uppgift 3 – aktivera säker LDAP för hello-hanterad domän](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="fcfa8-147">You need it while enabling secure LDAP for this managed domain in [Task 3 - enable secure LDAP for hello managed domain](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
    >
    >
15. <span data-ttu-id="fcfa8-148">På hello **filen tooExport** anger hello-filnamnet och sökvägen där du vill ha tooexport hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-148">On hello **File tooExport** page, specify hello file name and location where you'd like tooexport hello certificate.</span></span>

    ![Sökväg för certifikatexport](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. <span data-ttu-id="fcfa8-150">Efter sidan, klicka på hello **Slutför** tooexport hello certifikatets tooa PFX-fil.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-150">On hello following page, click **Finish** tooexport hello certificate tooa PFX file.</span></span> <span data-ttu-id="fcfa8-151">Du bör se bekräftelsedialogrutan när hello certifikatet har exporterats.</span><span class="sxs-lookup"><span data-stu-id="fcfa8-151">You should see confirmation dialog when hello certificate has been exported.</span></span>

    ![Exportera certifikat utfärdat](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a><span data-ttu-id="fcfa8-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fcfa8-153">Next step</span></span>
[<span data-ttu-id="fcfa8-154">Uppgift 3 – aktivera säker LDAP för hello-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="fcfa8-154">Task 3 - enable secure LDAP for hello managed domain</span></span>](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
