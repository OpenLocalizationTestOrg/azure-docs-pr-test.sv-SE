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
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: 5d46f376d46b8bbf3f93de57a7d4e31abdbcdb2f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a><span data-ttu-id="cc5a3-103">Konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="cc5a3-103">Configure secure LDAP (LDAPS) for an Azure AD Domain Services managed domain</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="cc5a3-104">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="cc5a3-104">Before you begin</span></span>
<span data-ttu-id="cc5a3-105">Se till att du har slutfört [uppgift 1 – skaffa ett certifikat för säker LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).</span><span class="sxs-lookup"><span data-stu-id="cc5a3-105">Ensure you've completed [Task 1 - obtain a certificate for secure LDAP](active-directory-ds-admin-guide-configure-secure-ldap.md).</span></span>


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a><span data-ttu-id="cc5a3-106">Uppgift 2 – exportera säker LDAP-certifikatet till en. PFX-fil</span><span class="sxs-lookup"><span data-stu-id="cc5a3-106">Task 2 - export the secure LDAP certificate to a .PFX file</span></span>
<span data-ttu-id="cc5a3-107">Innan du börjar den här uppgiften, se till att du har fått säker LDAP-certifikatet från en offentlig certifikatutfärdare har skapat ett självsignerat certifikat.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-107">Before you start this task, ensure that you have obtained the secure LDAP certificate from a public certification authority or have created a self-signed certificate.</span></span>

<span data-ttu-id="cc5a3-108">Utför följande steg om du vill exportera certifikatet LDAPS till en. PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-108">Perform the following steps, to export the LDAPS certificate to a .PFX file.</span></span>

1. <span data-ttu-id="cc5a3-109">Tryck på den **starta** knappen och skriv **R**. I den **kör** dialogrutan, ange **mmc** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-109">Press the **Start** button and type **R**. In the **Run** dialog, type **mmc** and click **OK**.</span></span>

    ![Starta MMC-konsolen](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)
2. <span data-ttu-id="cc5a3-111">På den **User Account Control** fråga klickar du på **Ja** att starta MMC (Microsoft Management Console) som administratör.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-111">On the **User Account Control** prompt, click **YES** to launch MMC (Microsoft Management Console) as administrator.</span></span>
3. <span data-ttu-id="cc5a3-112">Från den **filen** -menyn klickar du på **Lägg till/ta bort snapin-modulen...** .</span><span class="sxs-lookup"><span data-stu-id="cc5a3-112">From the **File** menu, click **Add/Remove Snap-in...**.</span></span>

    ![Lägg till snapin-modulen MMC-konsolen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)
4. <span data-ttu-id="cc5a3-114">I den **lägga till eller ta bort snapin-moduler** markerar den **certifikat** snapin-modulen och klicka på den **Lägg till >** knappen.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-114">In the **Add or Remove Snap-ins** dialog, select the **Certificates** snap-in, and click the **Add >** button.</span></span>

    ![Lägga till snapin-modulen certifikat i MMC-konsolen](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)
5. <span data-ttu-id="cc5a3-116">I den **snapin-modulen Certifikat** väljer **datorkonto** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-116">In the **Certificates snap-in** wizard, select **Computer account** and click **Next**.</span></span>

    ![Lägga till snapin-modulen certifikat för datorkontot](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)
6. <span data-ttu-id="cc5a3-118">På den **Välj dator** väljer **lokal dator: (datorn som den här konsolen körs på)** och på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-118">On the **Select Computer** page, select **Local computer: (the computer this console is running on)** and click **Finish**.</span></span>

    ![Lägga till snapin-modulen Certifikat - Välj dator](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)
7. <span data-ttu-id="cc5a3-120">I den **lägga till eller ta bort snapin-moduler** dialogrutan klickar du på **OK** lägga till snapin-modulen certifikat i MMC.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-120">In the **Add or Remove Snap-ins** dialog, click **OK** to add the certificates snap-in to MMC.</span></span>

    ![Lägga till snapin-modulen certifikat i MMC - klar](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)
8. <span data-ttu-id="cc5a3-122">I MMC-fönstret klickar du på om du vill expandera **Konsolrot**.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-122">In the MMC window, click to expand **Console Root**.</span></span> <span data-ttu-id="cc5a3-123">Du bör se snapin-modulen Certifikat lästes in.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-123">You should see the Certificates snap-in loaded.</span></span> <span data-ttu-id="cc5a3-124">Klicka på **certifikat (lokal dator)** att expandera.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-124">Click **Certificates (Local Computer)** to expand.</span></span> <span data-ttu-id="cc5a3-125">Klicka om du vill expandera den **personliga** nod, följt av den **certifikat** nod.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-125">Click to expand the **Personal** node, followed by the **Certificates** node.</span></span>

    ![Öppna personliga certifikat](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)
9. <span data-ttu-id="cc5a3-127">Du bör se det självsignerade certifikatet som vi skapade.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-127">You should see the self-signed certificate we created.</span></span> <span data-ttu-id="cc5a3-128">Du kan kontrollera egenskaperna för certifikat för att kontrollera certifikatets tumavtryck matchar som rapporterats för windows PowerShell när du skapade certifikatet.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-128">You can examine the properties of the certificate to ensure the thumbprint matches that reported on the PowerShell windows when you created the certificate.</span></span>
10. <span data-ttu-id="cc5a3-129">Välj det självsignerade certifikatet och **Högerklicka**.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-129">Select the self-signed certificate and **right click**.</span></span> <span data-ttu-id="cc5a3-130">Snabbmenyn, Välj **alla aktiviteter** och välj **exportera...** .</span><span class="sxs-lookup"><span data-stu-id="cc5a3-130">From the right-click menu, select **All Tasks** and select **Export...**.</span></span>

    ![Exportera certifikat](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)
11. <span data-ttu-id="cc5a3-132">I den **guiden Exportera certifikat**, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-132">In the **Certificate Export Wizard**, click **Next**.</span></span>

    ![Exportera certifikat](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)
12. <span data-ttu-id="cc5a3-134">På den **Exportera privat nyckel** väljer **Ja, exportera den privata nyckeln**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-134">On the **Export Private Key** page, select **Yes, export the private key**, and click **Next**.</span></span>

    ![Exportera certifikatets privata nyckel](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [!WARNING]
    > <span data-ttu-id="cc5a3-136">Du måste exportera den privata nyckeln tillsammans med certifikatet.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-136">You MUST export the private key along with the certificate.</span></span> <span data-ttu-id="cc5a3-137">Aktivera säker LDAP för din hanterade domän misslyckas om du anger en PFX som inte innehåller den privata nyckeln för certifikatet.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-137">If you provide a PFX that does not contain the private key for the certificate, enabling secure LDAP for your managed domain fails.</span></span>
    >
    >
13. <span data-ttu-id="cc5a3-138">På den **filformat för Export** väljer **Personal Information Exchange – PKCS #12 (. PFX)** som filformat för det exporterade certifikatet.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-138">On the **Export File Format** page, select **Personal Information Exchange - PKCS #12 (.PFX)** as the file format for the exported certificate.</span></span>

    ![Exportfilformat för certifikat](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [!NOTE]
    > <span data-ttu-id="cc5a3-140">Endast den. PFX-filformatet stöds.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-140">Only the .PFX file format is supported.</span></span> <span data-ttu-id="cc5a3-141">Exportera inte certifikatet till den. CER-filformat.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-141">Do not export the certificate to the .CER file format.</span></span>
    >
    >
14. <span data-ttu-id="cc5a3-142">På den **säkerhet** väljer den **lösenord** alternativet och anger ett lösenord för att skydda den. PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-142">On the **Security** page, select the **Password** option and type in a password to protect the .PFX file.</span></span> <span data-ttu-id="cc5a3-143">Kom ihåg detta lösenord eftersom det krävs i nästa uppgift.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-143">Remember this password since it will be needed in the next task.</span></span> <span data-ttu-id="cc5a3-144">Klicka på **nästa** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-144">Click **Next** to proceed.</span></span>

    ![<span data-ttu-id="cc5a3-145">Lösenordet för certifikatexport</span><span class="sxs-lookup"><span data-stu-id="cc5a3-145">Password for certificate export</span></span> ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [!NOTE]
    > <span data-ttu-id="cc5a3-146">Anteckna det här lösenordet.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-146">Make a note of this password.</span></span> <span data-ttu-id="cc5a3-147">Du behöver det för att aktivera säker LDAP för den här hanterade domänen i [uppgift 3 – aktivera säker LDAP för den hanterade domänen](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span><span class="sxs-lookup"><span data-stu-id="cc5a3-147">You need it while enabling secure LDAP for this managed domain in [Task 3 - enable secure LDAP for the managed domain](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)</span></span>
    >
    >
15. <span data-ttu-id="cc5a3-148">På den **fil som ska exporteras** anger du filnamnet och platsen där du vill exportera certifikatet.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-148">On the **File to Export** page, specify the file name and location where you'd like to export the certificate.</span></span>

    ![Sökväg för certifikatexport](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)
16. <span data-ttu-id="cc5a3-150">På sidan följande **Slutför** att exportera certifikatet till en PFX-fil.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-150">On the following page, click **Finish** to export the certificate to a PFX file.</span></span> <span data-ttu-id="cc5a3-151">Du bör se bekräftelsedialogrutan när certifikatet har exporterats.</span><span class="sxs-lookup"><span data-stu-id="cc5a3-151">You should see confirmation dialog when the certificate has been exported.</span></span>

    ![Exportera certifikat utfärdat](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="next-step"></a><span data-ttu-id="cc5a3-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cc5a3-153">Next step</span></span>
[<span data-ttu-id="cc5a3-154">Uppgift 3 – aktivera säker LDAP för den hanterade domänen</span><span class="sxs-lookup"><span data-stu-id="cc5a3-154">Task 3 - enable secure LDAP for the managed domain</span></span>](active-directory-ds-admin-guide-configure-secure-ldap-enable-ldaps.md)
