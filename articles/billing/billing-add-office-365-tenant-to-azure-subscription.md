---
title: aaaUse en Office 365-klient med en Azure-prenumeration | Microsoft Docs
description: "Lär dig hur tooadd en Office 365 directory (klient) tooan Azure-prenumeration."
services: 
documentationcenter: 
author: JiangChen79
manager: jlian
editor: 
tags: billing,top-support-issue
ms.assetid: cc9c57c6-7bfd-4dea-9027-c75ef3737589
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: cjiang
ms.openlocfilehash: e560370417bd074a7e37ceb7c60da45dbbbcf775
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="associate-an-office-365-tenant-tooan-azure-subscription"></a><span data-ttu-id="509d0-103">Koppla en Office 365-klient tooan Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="509d0-103">Associate an Office 365 tenant tooan Azure subscription</span></span>
<span data-ttu-id="509d0-104">Länka dina separat Azure och Office 365-prenumerationer så att du kan komma åt hello Office 365-klient från din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="509d0-104">Link your separate Azure and Office 365 subscriptions so that you can access hello Office 365 tenant from your Azure subscription.</span></span> <span data-ttu-id="509d0-105">toolink dina prenumerationer, logga in tooAzure med hello Azure service administratörskonto, lägga till en katalog och Lägg till hello Office 365 organisationskonton toohello Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="509d0-105">toolink your subscriptions, sign in tooAzure with hello Azure service administrator account, add a directory, and add hello Office 365 organizational accounts toohello Azure Active Directory tenant.</span></span>

<span data-ttu-id="509d0-106">Om du vill använda Office 365-prenumeration för användare i din Azure Active Directory-instans eller du har en Office 365-konto men inte ett Azure-konto, se [registrera dig för Azure med Office 365-konto](billing-use-existing-office-365-account-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="509d0-106">If you want an Office 365 subscription for users in your Azure Active Directory instance or you have an Office 365 account but not an Azure account, see [Sign up for Azure with Office 365 account](billing-use-existing-office-365-account-azure-subscription.md).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="509d0-107">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="509d0-107">Before you begin</span></span>
* <span data-ttu-id="509d0-108">Du måste ha hello autentiseringsuppgifterna för tjänstadministratör för hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="509d0-108">You must have hello credentials of hello Azure subscription service administrator.</span></span> <span data-ttu-id="509d0-109">Medadministratör konton kan inte göra några av hello stegen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="509d0-109">Co-administrator accounts can't do some of hello steps in this article.</span></span> <span data-ttu-id="509d0-110">toochange tjänstadministratören, se [hur tooadd eller ändra Azure-administratörsroller](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).</span><span class="sxs-lookup"><span data-stu-id="509d0-110">toochange your service administrator, see [How tooadd or change Azure administrator roles](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).</span></span>
* <span data-ttu-id="509d0-111">Du måste ha hello autentiseringsuppgifterna för en global administratör för hello Office 365-klienten.</span><span class="sxs-lookup"><span data-stu-id="509d0-111">You must have hello credentials of a global administrator of hello Office 365 tenant.</span></span>
* <span data-ttu-id="509d0-112">hello hello tjänstadministratör e-postadress får inte vara i hello Office 365-klienten.</span><span class="sxs-lookup"><span data-stu-id="509d0-112">hello email address of hello service administrator must not be in hello Office 365 tenant.</span></span>
* <span data-ttu-id="509d0-113">hello hello tjänstadministratör e-postadress får inte matcha som en global administratör för hello Office 365-klienten.</span><span class="sxs-lookup"><span data-stu-id="509d0-113">hello email address of hello service administrator must not match that of any global administrator of hello Office 365 tenant.</span></span>
* <span data-ttu-id="509d0-114">Om du använder en e-postadress som är både ett Microsoft-konto och ett organisationskonto tillfälligt ändra hello tjänstadministratören för din Azure-prenumeration toouse ett annat microsoftkonto.</span><span class="sxs-lookup"><span data-stu-id="509d0-114">If you use an email address that is both a Microsoft account and an organizational account, temporarily change hello service administrator of your Azure subscription toouse another Microsoft account.</span></span> <span data-ttu-id="509d0-115">Du kan skapa ett Microsoft-konto på hello [inloggningssidan för Microsoft-konto](https://signup.live.com/).</span><span class="sxs-lookup"><span data-stu-id="509d0-115">You can create a Microsoft account at hello [Microsoft account signup page](https://signup.live.com/).</span></span>

## <a name="link-office-365-tenant-tooazure-subscription"></a><span data-ttu-id="509d0-116">Länka tooAzure prenumeration på Office 365-klient</span><span class="sxs-lookup"><span data-stu-id="509d0-116">Link Office 365 tenant tooAzure subscription</span></span>
<span data-ttu-id="509d0-117">tooassociate hello Office 365-klient toohello Azure-prenumeration, Följ dessa steg:</span><span class="sxs-lookup"><span data-stu-id="509d0-117">tooassociate hello Office 365 tenant toohello Azure subscription, follow these steps:</span></span>

### <a name="step-1-add-office-365-tenant-tooyour-azure-subscription"></a><span data-ttu-id="509d0-118">Steg 1: Lägg till Office 365-klient tooyour Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="509d0-118">Step 1: Add Office 365 tenant tooyour Azure subscription</span></span>

1. <span data-ttu-id="509d0-119">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) med administratörsautentiseringsuppgifter för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="509d0-119">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) with hello service administrator credentials.</span></span>

    ![Skärmbild av Azure-inloggning](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

2. <span data-ttu-id="509d0-121">I hello vänster och välj **ACTIVE DIRECTORY**.</span><span class="sxs-lookup"><span data-stu-id="509d0-121">In hello left pane, select **ACTIVE DIRECTORY**.</span></span> <span data-ttu-id="509d0-122">Du bör se hello Office 365-klient.</span><span class="sxs-lookup"><span data-stu-id="509d0-122">You shouldn't see hello Office 365 tenant.</span></span> <span data-ttu-id="509d0-123">Om du ser det vidare för[steg 2: ändra hello katalog som hör till hello Azure-prenumeration](#Step2).</span><span class="sxs-lookup"><span data-stu-id="509d0-123">If you see it, skip too[Step 2: Change hello directory associated with hello Azure subscription](#Step2).</span></span>
   
   ![Skärmbild av Active Directory-post](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

3. <span data-ttu-id="509d0-125">Välj **ny** > **DIRECTORY** > **anpassade skapa**.</span><span class="sxs-lookup"><span data-stu-id="509d0-125">Select **NEW** > **DIRECTORY** > **CUSTOM CREATE**.</span></span>
   
    ![Skapa anpassade Skärmbild av Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
4. <span data-ttu-id="509d0-127">På hello **Lägg till katalogen** sidan under **DIRECTORY**väljer **Använd befintlig katalog**.</span><span class="sxs-lookup"><span data-stu-id="509d0-127">On hello **Add directory** page, under **DIRECTORY**, select **Use existing directory**.</span></span> <span data-ttu-id="509d0-128">Välj sedan **jag är redo toobe utloggad nu**, och välj **Slutför** ![slutföra ikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="509d0-128">Then select **I am ready toobe signed out now**, and select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>
   
    ![Skärmbild av ”Använd befintlig katalog”](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
5. <span data-ttu-id="509d0-130">När du har loggat, logga in med autentiseringsuppgifterna för hello global administratör för din Office 365-klient.</span><span class="sxs-lookup"><span data-stu-id="509d0-130">After you are signed out, sign in with hello global administrator’s credentials of your Office 365 tenant.</span></span>
   
    ![Skärmbild av Office 365 global administratör logga in](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
6. <span data-ttu-id="509d0-132">Välj **fortsätta**.</span><span class="sxs-lookup"><span data-stu-id="509d0-132">Select **Continue**.</span></span>
   
    ![Skärmbild av verifieringen](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
7. <span data-ttu-id="509d0-134">Välj **logga ut nu**.</span><span class="sxs-lookup"><span data-stu-id="509d0-134">Select **Sign out now**.</span></span>
   
    ![Skärmbild av utloggning](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
8. <span data-ttu-id="509d0-136">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) med administratörsautentiseringsuppgifter för hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="509d0-136">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) with hello service administrator credentials.</span></span>
   
    ![Skärmbild av Azure-inloggning](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
9. <span data-ttu-id="509d0-138">Du bör se din Office 365-klient i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="509d0-138">You should see your Office 365 tenant in hello dashboard.</span></span>
   
    ![Skärmbild av instrumentpanelen](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <span data-ttu-id="509d0-140"><a name="Step2"></a>Steg 2: Ändra hello katalog som hör till hello Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="509d0-140"><a name="Step2"></a>Step 2: Change hello directory associated with hello Azure subscription</span></span>
   
1. <span data-ttu-id="509d0-141">Välj **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="509d0-141">Select **Settings**.</span></span>
   
    ![Skärmbild av Azure klassiska portal inställningsikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
2. <span data-ttu-id="509d0-143">Välj din Azure-prenumeration och välj sedan **redigera katalog**.</span><span class="sxs-lookup"><span data-stu-id="509d0-143">Select your Azure subscription, and then select **EDIT DIRECTORY**.</span></span>

    ![Skärmbild av Azure-prenumeration redigera katalog](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
3. <span data-ttu-id="509d0-145">Välj **nästa** ![nästa ikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).</span><span class="sxs-lookup"><span data-stu-id="509d0-145">Select **Next** ![Next icon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).</span></span>
   
    ![Skärmbild av ”ändra hello associerade directory”](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
4. <span data-ttu-id="509d0-147">Granska hello påverkas konton.</span><span class="sxs-lookup"><span data-stu-id="509d0-147">Review hello affected accounts.</span></span> <span data-ttu-id="509d0-148">Alla medadministratörer och [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md) användare med tilldelade åtkomst hello befintliga resursgrupper tas bort.</span><span class="sxs-lookup"><span data-stu-id="509d0-148">All co-administrators and [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) users with assigned access in hello existing resource groups are removed.</span></span> <span data-ttu-id="509d0-149">hello varning visas nämner bara hello borttagning av medadministratörer.</span><span class="sxs-lookup"><span data-stu-id="509d0-149">hello warning you receive only mentions hello removal of co-administrators.</span></span>
      
    ![Skärmbild som visar hello samtidigt administratörskonton toobe tas bort.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Skärmbild som visar ett exempel användarens konto toobe tas bort.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
5. <span data-ttu-id="509d0-152">Välj **Slutför** ![slutföra ikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="509d0-152">Select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>

### <a name="step-3-add-your-office-365-organizational-accounts-as-co-administrators-toohello-azure-active-directory-tenant"></a><span data-ttu-id="509d0-153">Steg 3: Lägg till din organisations Office 365-användarkonton som medadministratörer toohello Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="509d0-153">Step 3: Add your Office 365 organizational accounts as co-administrators toohello Azure Active Directory tenant</span></span>
   
1. <span data-ttu-id="509d0-154">Välj hello **ADMINISTRATÖRER** och välj sedan **lägga till**.</span><span class="sxs-lookup"><span data-stu-id="509d0-154">Select hello **ADMINISTRATORS** tab, and then select **ADD**.</span></span>
   
    ![Skärmbild av Azure klassiska portal administratörer inställningsflik](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
2. <span data-ttu-id="509d0-156">Ange ett organisationskonto för din Office 365-klient, Välj hello Azure-prenumeration och välj sedan **Slutför** ![slutföra ikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="509d0-156">Enter an organizational account of your Office 365 tenant, select hello Azure subscription, and then select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>
   
    ![Skärmbild av Azure Lägg till medadministratör dialogruta](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
3. <span data-ttu-id="509d0-158">Gå tillbaka toohello **ADMINISTRATÖRER** fliken. Du bör se hello organisationskonto visas som medadministratör.</span><span class="sxs-lookup"><span data-stu-id="509d0-158">Go back toohello **ADMINISTRATORS** tab. You should see hello organizational account displayed as co-administrator.</span></span>
   
    ![Skärmbild av fliken administratörer](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
4.  <span data-ttu-id="509d0-160">Testa åtkomst tooAzure med hello samtidigt administratörskonto.</span><span class="sxs-lookup"><span data-stu-id="509d0-160">Test access tooAzure with hello co-administrator account.</span></span>
   
    <span data-ttu-id="509d0-161">a.</span><span class="sxs-lookup"><span data-stu-id="509d0-161">a.</span></span> <span data-ttu-id="509d0-162">Logga ut från hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="509d0-162">Sign out of hello Azure classic portal.</span></span>
   
    <span data-ttu-id="509d0-163">b.</span><span class="sxs-lookup"><span data-stu-id="509d0-163">b.</span></span> <span data-ttu-id="509d0-164">Öppna hello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="509d0-164">Open hello [Azure portal](https://portal.azure.com/).</span></span>
   
    <span data-ttu-id="509d0-165">c.</span><span class="sxs-lookup"><span data-stu-id="509d0-165">c.</span></span> <span data-ttu-id="509d0-166">Ange hello autentiseringsuppgifter för hello medadministratör och välj sedan **logga in**.</span><span class="sxs-lookup"><span data-stu-id="509d0-166">Enter hello credentials of hello co-administrator, and then select **Sign in**.</span></span>
   
    ![Skärmbild av Azure-inloggningssida](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="need-help-contact-support"></a><span data-ttu-id="509d0-168">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="509d0-168">Need help?</span></span> <span data-ttu-id="509d0-169">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="509d0-169">Contact support.</span></span>
<span data-ttu-id="509d0-170">Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.</span><span class="sxs-lookup"><span data-stu-id="509d0-170">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>


