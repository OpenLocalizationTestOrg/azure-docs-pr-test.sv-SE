---
title: "Azure Active Directory Domain Services: Aktivera lösenordssynkronisering | Microsoft Docs"
description: "Komma igång med Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 5a32a0df-a3ca-4ebe-b980-91f58f8030fc
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: 8e073df9de2996f5ad159dda746881c014ea3d66
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-password-synchronization-tooazure-active-directory-domain-services"></a><span data-ttu-id="0a16a-103">Aktivera lösenord synkronisering tooAzure Active Directory Domain Services</span><span class="sxs-lookup"><span data-stu-id="0a16a-103">Enable password synchronization tooAzure Active Directory Domain Services</span></span>
<span data-ttu-id="0a16a-104">I föregående uppgifter aktiverade du Azure Active Directory Domain Services för din Azure Active Directory-klient (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0a16a-104">In preceding tasks, you enabled Azure Active Directory Domain Services for your Azure Active Directory (Azure AD) tenant.</span></span> <span data-ttu-id="0a16a-105">hello nästa uppgift är tooenable synkroniseringen av hashvärdena som krävs för NT LAN Manager (NTLM) och Kerberos-autentisering tooAzure AD DS.</span><span class="sxs-lookup"><span data-stu-id="0a16a-105">hello next task is tooenable synchronization of credential hashes required for NT LAN Manager (NTLM) and Kerberos authentication tooAzure AD Domain Services.</span></span> <span data-ttu-id="0a16a-106">När du har konfigurerat autentiseringsuppgifter synkronisering kan användarna logga in toohello hanterade domänen med sina företagsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0a16a-106">After you've set up credential synchronization, users can sign in toohello managed domain with their corporate credentials.</span></span>

<span data-ttu-id="0a16a-107">hello stegen skiljer sig för endast molnbaserad användaren konton jämfört med användarkonton som synkroniseras från din lokala katalog med Azure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="0a16a-107">hello steps involved are different for cloud-only user accounts vs user accounts that are synchronized from your on-premises directory using Azure AD Connect.</span></span>  <span data-ttu-id="0a16a-108">Om Azure AD-klienten har en kombination av molnet endast användare och användare från din lokala AD och du behöver tooperform båda dessa steg.</span><span class="sxs-lookup"><span data-stu-id="0a16a-108">If your Azure AD tenant has a combination of cloud only users and users from your on-premises AD, you need tooperform both steps.</span></span>

<br>

> [!div class="op_single_selector"]
> * <span data-ttu-id="0a16a-109">**Endast molnbaserad användarkonton**: [synkronisera lösenord för endast molnbaserad användarkonton tooyour hanterad domän](active-directory-ds-getting-started-password-sync.md)</span><span class="sxs-lookup"><span data-stu-id="0a16a-109">**Cloud-only user accounts**: [Synchronize passwords for cloud-only user accounts tooyour managed domain](active-directory-ds-getting-started-password-sync.md)</span></span>
> * <span data-ttu-id="0a16a-110">**Lokala användarkonton**: [synkronisera lösenord för användarkonton synkroniseras från din lokala AD tooyour hanterad domän](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span><span class="sxs-lookup"><span data-stu-id="0a16a-110">**On-premises user accounts**: [Synchronize passwords for user accounts synced from your on-premises AD tooyour managed domain](active-directory-ds-getting-started-password-sync-synced-tenant.md)</span></span>
>
>

<br>

## <a name="task-5-enable-password-synchronization-tooyour-managed-domain-for-cloud-only-user-accounts"></a><span data-ttu-id="0a16a-111">Uppgift 5: aktivera lösenord synkronisering tooyour hanterad domän för endast molnbaserad användarkonton</span><span class="sxs-lookup"><span data-stu-id="0a16a-111">Task 5: enable password synchronization tooyour managed domain for cloud-only user accounts</span></span>
<span data-ttu-id="0a16a-112">tooauthenticate användare på hello hanterade domän, Azure Active Directory Domain Services måste autentiseringsuppgifter hash-värden i ett format som är lämplig för NTLM och Kerberos-autentisering.</span><span class="sxs-lookup"><span data-stu-id="0a16a-112">tooauthenticate users on hello managed domain, Azure Active Directory Domain Services needs credential hashes in a format that's suitable for NTLM and Kerberos authentication.</span></span> <span data-ttu-id="0a16a-113">Azure AD inte generera eller lagra hashvärdena i hello-format som krävs för NTLM eller Kerberos-autentisering, tills du aktiverar Azure Active Directory Domain Services för din klient.</span><span class="sxs-lookup"><span data-stu-id="0a16a-113">Azure AD does not generate or store credential hashes in hello format that's required for NTLM or Kerberos authentication, until you enable Azure Active Directory Domain Services for your tenant.</span></span> <span data-ttu-id="0a16a-114">Av självklara säkerhetsskäl lagrar Azure AD inte heller lösenordsuppgifter i klartext.</span><span class="sxs-lookup"><span data-stu-id="0a16a-114">For obvious security reasons, Azure AD also does not store any password credentials in clear-text form.</span></span> <span data-ttu-id="0a16a-115">Azure AD har därför inte en sätt tooautomatically generera dessa NTLM eller Kerberos-hashvärdena baserat på användarnas befintliga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="0a16a-115">Therefore, Azure AD does not have a way tooautomatically generate these NTLM or Kerberos credential hashes based on users' existing credentials.</span></span>

> [!NOTE]
> <span data-ttu-id="0a16a-116">Om din organisation har endast molnbaserad användarkonton, ändra användare som behöver toouse Azure Active Directory Domain Services sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="0a16a-116">If your organization has cloud-only user accounts, users who need toouse Azure Active Directory Domain Services must change their passwords.</span></span> <span data-ttu-id="0a16a-117">En molnbaserad användarkonto är ett konto som har skapats i Azure AD-katalogen med hello Azure-portalen eller Azure AD PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="0a16a-117">A cloud-only user account is an account that was created in your Azure AD directory using either hello Azure portal or Azure AD PowerShell cmdlets.</span></span> <span data-ttu-id="0a16a-118">Dessa användarkonton är inte synkroniserade från en lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="0a16a-118">Such user accounts aren't synchronized from an on-premises directory.</span></span>
>
>

<span data-ttu-id="0a16a-119">Den här lösenordsändringsprocessen gör hello autentiseringsuppgifter hash-värden som krävs av Azure Active Directory Domain Services för Kerberos- och NTLM-autentisering toobe genereras i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0a16a-119">This password change process causes hello credential hashes that are required by Azure Active Directory Domain Services for Kerberos and NTLM authentication toobe generated in Azure AD.</span></span> <span data-ttu-id="0a16a-120">Du kan antingen ange hello lösenord för alla användare i hello-klient som behöver toouse Azure Active Directory Domain Services eller be dem toochange sina lösenord.</span><span class="sxs-lookup"><span data-stu-id="0a16a-120">You can either expire hello passwords for all users in hello tenant who need toouse Azure Active Directory Domain Services or instruct them toochange their passwords.</span></span>

### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-user-account"></a><span data-ttu-id="0a16a-121">Aktivera genereringen av hashvärden för NTLM- och Kerberos-autentisering för ett endast molnbaserat användarkonto</span><span class="sxs-lookup"><span data-stu-id="0a16a-121">Enable NTLM and Kerberos credential hash generation for a cloud-only user account</span></span>
<span data-ttu-id="0a16a-122">Här följer hello-instruktioner som du behöver tooprovide användare, så att de kan ändra sina lösenord:</span><span class="sxs-lookup"><span data-stu-id="0a16a-122">Here are hello instructions you need tooprovide users, so they can change their passwords:</span></span>

1. <span data-ttu-id="0a16a-123">Gå toohello [Azure AD-åtkomstpanelen](http://myapps.microsoft.com) för din organisation.</span><span class="sxs-lookup"><span data-stu-id="0a16a-123">Go toohello [Azure AD Access Panel](http://myapps.microsoft.com) page for your organization.</span></span>

    ![Starta hello Azure AD-åtkomstpanelen](./media/active-directory-domain-services-getting-started/access-panel.png)

2. <span data-ttu-id="0a16a-125">Klicka på ditt namn i hello längst upp till höger och välj **profil** hello-menyn.</span><span class="sxs-lookup"><span data-stu-id="0a16a-125">In hello top right corner, click on your name and select **Profile** from hello menu.</span></span>

    ![Välj profil](./media/active-directory-domain-services-getting-started/select-profile.png)

3. <span data-ttu-id="0a16a-127">På hello **profil** klickar du på **ändra lösenord**.</span><span class="sxs-lookup"><span data-stu-id="0a16a-127">On hello **Profile** page, click on **Change password**.</span></span>

    ![Klicka på "Ändra lösenord"](./media/active-directory-domain-services-getting-started/user-change-password.png)

   > [!NOTE]
   > <span data-ttu-id="0a16a-129">Om hello **ändra lösenord** alternativet visas inte i hello åtkomstpanelen fönster, kontrollera att din organisation har konfigurerat [lösenordshantering i Azure AD](../active-directory/active-directory-passwords-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="0a16a-129">If hello **Change password** option is not displayed in hello Access Panel window, ensure that your organization has configured [password management in Azure AD](../active-directory/active-directory-passwords-getting-started.md).</span></span>
   >
   >
4. <span data-ttu-id="0a16a-130">På hello **ändra lösenord** sidan, skriver du ditt befintliga (gamla) lösenord, Skriv ett nytt lösenord och bekräfta det.</span><span class="sxs-lookup"><span data-stu-id="0a16a-130">On hello **change password** page, type your existing (old) password, type a new password, and then confirm it.</span></span>

    ![Skapa ett virtuellt nätverk för Azure AD Domain Services.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

5. <span data-ttu-id="0a16a-132">Klicka på **Skicka**.</span><span class="sxs-lookup"><span data-stu-id="0a16a-132">Click **submit**.</span></span>

<span data-ttu-id="0a16a-133">Några minuter när du har ändrat ditt lösenord kan hello nytt lösenord användas i Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="0a16a-133">A few minutes after you have changed your password, hello new password is usable in Azure Active Directory Domain Services.</span></span> <span data-ttu-id="0a16a-134">Efter några minuter (vanligtvis ca 20 minuter som), kan du logga in toocomputers som är kopplade toohello hanterade domänen med hjälp av hello nyligen ändrade lösenord.</span><span class="sxs-lookup"><span data-stu-id="0a16a-134">After a few more minutes (typically, about 20 minutes), you can sign in toocomputers that are joined toohello managed domain by using hello newly changed password.</span></span>

## <a name="related-content"></a><span data-ttu-id="0a16a-135">Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="0a16a-135">Related Content</span></span>
* [<span data-ttu-id="0a16a-136">Hur tooupdate ditt eget lösenord</span><span class="sxs-lookup"><span data-stu-id="0a16a-136">How tooupdate your own password</span></span>](../active-directory/active-directory-passwords-update-your-own-password.md)
* [<span data-ttu-id="0a16a-137">Komma igång med lösenordshantering i Azure AD</span><span class="sxs-lookup"><span data-stu-id="0a16a-137">Getting started with Password Management in Azure AD</span></span>](../active-directory/active-directory-passwords-getting-started.md)
* [<span data-ttu-id="0a16a-138">Aktivera Lösenordssynkronisering tooAzure Active Directory Domain Services för en synkroniserad Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="0a16a-138">Enable password synchronization tooAzure Active Directory Domain Services for a synced Azure AD tenant</span></span>](active-directory-ds-getting-started-password-sync-synced-tenant.md)
* [<span data-ttu-id="0a16a-139">Administrera en Azure Active Directory Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="0a16a-139">Administer an Azure Active Directory Domain Services-managed domain</span></span>](active-directory-ds-admin-guide-administer-domain.md)
* [<span data-ttu-id="0a16a-140">Ansluta till en Windows virtuella tooan Azure Active Directory Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="0a16a-140">Join a Windows virtual machine tooan Azure Active Directory Domain Services-managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* [<span data-ttu-id="0a16a-141">Anslut till en Red Hat Enterprise Linux virtuella tooan Azure Active Directory Domain Services-hanterad domän</span><span class="sxs-lookup"><span data-stu-id="0a16a-141">Join a Red Hat Enterprise Linux virtual machine tooan Azure Active Directory Domain Services-managed domain</span></span>](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
