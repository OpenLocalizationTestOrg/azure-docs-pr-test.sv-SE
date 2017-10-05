---
title: "Använda en Office 365-klient med en Azure-prenumeration | Microsoft Docs"
description: "Lär dig hur du lägger till en Office 365-katalog (klient) till en Azure-prenumeration."
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
ms.openlocfilehash: 7affb4a83cdb8ababef60e786a3c824e7477ed68
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="associate-an-office-365-tenant-to-an-azure-subscription"></a><span data-ttu-id="dca66-103">Koppla en Office 365-klient till en Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="dca66-103">Associate an Office 365 tenant to an Azure subscription</span></span>
<span data-ttu-id="dca66-104">Länka dina separat Azure och Office 365-prenumerationer så att du kan komma åt Office 365-klient från din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="dca66-104">Link your separate Azure and Office 365 subscriptions so that you can access the Office 365 tenant from your Azure subscription.</span></span> <span data-ttu-id="dca66-105">Logga in på Azure med Azure-tjänstens konto om du vill länka dina prenumerationer, lägga till en katalog och lägga till Office 365-organisationskonton i Azure Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="dca66-105">To link your subscriptions, sign in to Azure with the Azure service administrator account, add a directory, and add the Office 365 organizational accounts to the Azure Active Directory tenant.</span></span>

<span data-ttu-id="dca66-106">Om du vill använda Office 365-prenumeration för användare i din Azure Active Directory-instans eller du har en Office 365-konto men inte ett Azure-konto, se [registrera dig för Azure med Office 365-konto](billing-use-existing-office-365-account-azure-subscription.md).</span><span class="sxs-lookup"><span data-stu-id="dca66-106">If you want an Office 365 subscription for users in your Azure Active Directory instance or you have an Office 365 account but not an Azure account, see [Sign up for Azure with Office 365 account](billing-use-existing-office-365-account-azure-subscription.md).</span></span> 

## <a name="before-you-begin"></a><span data-ttu-id="dca66-107">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="dca66-107">Before you begin</span></span>
* <span data-ttu-id="dca66-108">Du måste ha autentiseringsuppgifter som tjänstadministratör för Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="dca66-108">You must have the credentials of the Azure subscription service administrator.</span></span> <span data-ttu-id="dca66-109">Medadministratör konton kan inte göra vissa av stegen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="dca66-109">Co-administrator accounts can't do some of the steps in this article.</span></span> <span data-ttu-id="dca66-110">Om du vill ändra tjänstadministratören [lägga till eller ändra Azure-administratörsroller](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).</span><span class="sxs-lookup"><span data-stu-id="dca66-110">To change your service administrator, see [How to add or change Azure administrator roles](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).</span></span>
* <span data-ttu-id="dca66-111">Du måste ha autentiseringsuppgifter för en global administratör för Office 365-klienten.</span><span class="sxs-lookup"><span data-stu-id="dca66-111">You must have the credentials of a global administrator of the Office 365 tenant.</span></span>
* <span data-ttu-id="dca66-112">Tjänstens e-postadress får inte vara i Office 365-klienten.</span><span class="sxs-lookup"><span data-stu-id="dca66-112">The email address of the service administrator must not be in the Office 365 tenant.</span></span>
* <span data-ttu-id="dca66-113">Tjänstadministratören e-postadress måste inte matcha en global administratör för Office 365-klienten.</span><span class="sxs-lookup"><span data-stu-id="dca66-113">The email address of the service administrator must not match that of any global administrator of the Office 365 tenant.</span></span>
* <span data-ttu-id="dca66-114">Om du använder en e-postadress som är både ett Microsoft-konto och ett organisationskonto kan du tillfälligt ändra tjänstadministratören för din Azure-prenumeration om du vill använda ett annat microsoftkonto.</span><span class="sxs-lookup"><span data-stu-id="dca66-114">If you use an email address that is both a Microsoft account and an organizational account, temporarily change the service administrator of your Azure subscription to use another Microsoft account.</span></span> <span data-ttu-id="dca66-115">Du kan skapa ett Microsoft-konto på den [inloggningssidan för Microsoft-konto](https://signup.live.com/).</span><span class="sxs-lookup"><span data-stu-id="dca66-115">You can create a Microsoft account at the [Microsoft account signup page](https://signup.live.com/).</span></span>

## <a name="link-office-365-tenant-to-azure-subscription"></a><span data-ttu-id="dca66-116">Länken Office 365-klient till Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="dca66-116">Link Office 365 tenant to Azure subscription</span></span>
<span data-ttu-id="dca66-117">Följ dessa steg om du vill associera Office 365-klient till Azure-prenumerationen:</span><span class="sxs-lookup"><span data-stu-id="dca66-117">To associate the Office 365 tenant to the Azure subscription, follow these steps:</span></span>

### <a name="step-1-add-office-365-tenant-to-your-azure-subscription"></a><span data-ttu-id="dca66-118">Steg 1: Lägg till Office 365-klient till din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="dca66-118">Step 1: Add Office 365 tenant to your Azure subscription</span></span>

1. <span data-ttu-id="dca66-119">Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com/) med administratörsautentiseringsuppgifter för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dca66-119">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) with the service administrator credentials.</span></span>

    ![Skärmbild av Azure-inloggning](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

2. <span data-ttu-id="dca66-121">I den vänstra rutan, Välj **ACTIVE DIRECTORY**.</span><span class="sxs-lookup"><span data-stu-id="dca66-121">In the left pane, select **ACTIVE DIRECTORY**.</span></span> <span data-ttu-id="dca66-122">Du bör se Office 365-klient.</span><span class="sxs-lookup"><span data-stu-id="dca66-122">You shouldn't see the Office 365 tenant.</span></span> <span data-ttu-id="dca66-123">Om du ser det gå vidare till [steg 2: Ändra katalogen som är associerade med Azure-prenumerationen](#Step2).</span><span class="sxs-lookup"><span data-stu-id="dca66-123">If you see it, skip to [Step 2: Change the directory associated with the Azure subscription](#Step2).</span></span>
   
   ![Skärmbild av Active Directory-post](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

3. <span data-ttu-id="dca66-125">Välj **ny** > **DIRECTORY** > **anpassade skapa**.</span><span class="sxs-lookup"><span data-stu-id="dca66-125">Select **NEW** > **DIRECTORY** > **CUSTOM CREATE**.</span></span>
   
    ![Skapa anpassade Skärmbild av Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
4. <span data-ttu-id="dca66-127">På den **Lägg till katalogen** sidan under **DIRECTORY**väljer **Använd befintlig katalog**.</span><span class="sxs-lookup"><span data-stu-id="dca66-127">On the **Add directory** page, under **DIRECTORY**, select **Use existing directory**.</span></span> <span data-ttu-id="dca66-128">Välj sedan **jag är redo att bli utloggad nu**, och välj **Slutför** ![slutföra ikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="dca66-128">Then select **I am ready to be signed out now**, and select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>
   
    ![Skärmbild av ”Använd befintlig katalog”](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
5. <span data-ttu-id="dca66-130">När du har loggat, logga in med autentiseringsuppgifterna för den globala administratören för din Office 365-klient.</span><span class="sxs-lookup"><span data-stu-id="dca66-130">After you are signed out, sign in with the global administrator’s credentials of your Office 365 tenant.</span></span>
   
    ![Skärmbild av Office 365 global administratör logga in](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
6. <span data-ttu-id="dca66-132">Välj **fortsätta**.</span><span class="sxs-lookup"><span data-stu-id="dca66-132">Select **Continue**.</span></span>
   
    ![Skärmbild av verifieringen](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
7. <span data-ttu-id="dca66-134">Välj **logga ut nu**.</span><span class="sxs-lookup"><span data-stu-id="dca66-134">Select **Sign out now**.</span></span>
   
    ![Skärmbild av utloggning](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
8. <span data-ttu-id="dca66-136">Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com/) med administratörsautentiseringsuppgifter för tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dca66-136">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) with the service administrator credentials.</span></span>
   
    ![Skärmbild av Azure-inloggning](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
9. <span data-ttu-id="dca66-138">Du bör se din Office 365-klient på instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="dca66-138">You should see your Office 365 tenant in the dashboard.</span></span>
   
    ![Skärmbild av instrumentpanelen](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <span data-ttu-id="dca66-140"><a name="Step2"></a>Steg 2: Ändra katalog som hör till Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="dca66-140"><a name="Step2"></a>Step 2: Change the directory associated with the Azure subscription</span></span>
   
1. <span data-ttu-id="dca66-141">Välj **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="dca66-141">Select **Settings**.</span></span>
   
    ![Skärmbild av Azure klassiska portal inställningsikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
2. <span data-ttu-id="dca66-143">Välj din Azure-prenumeration och välj sedan **redigera katalog**.</span><span class="sxs-lookup"><span data-stu-id="dca66-143">Select your Azure subscription, and then select **EDIT DIRECTORY**.</span></span>

    ![Skärmbild av Azure-prenumeration redigera katalog](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
3. <span data-ttu-id="dca66-145">Välj **nästa** ![nästa ikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).</span><span class="sxs-lookup"><span data-stu-id="dca66-145">Select **Next** ![Next icon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).</span></span>
   
    ![Skärmbild av ”ändra katalogen associerade”](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
4. <span data-ttu-id="dca66-147">Granska de berörda kontona.</span><span class="sxs-lookup"><span data-stu-id="dca66-147">Review the affected accounts.</span></span> <span data-ttu-id="dca66-148">Alla medadministratörer och [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md) användare med tilldelade åtkomst i befintliga resursgrupper tas bort.</span><span class="sxs-lookup"><span data-stu-id="dca66-148">All co-administrators and [Role-Based Access Control (RBAC)](../active-directory/role-based-access-control-configure.md) users with assigned access in the existing resource groups are removed.</span></span> <span data-ttu-id="dca66-149">Varningen visas nämner bara borttagning av medadministratörer.</span><span class="sxs-lookup"><span data-stu-id="dca66-149">The warning you receive only mentions the removal of co-administrators.</span></span>
      
    ![Skärmbild som visar kontona som medadministratör kan tas bort.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Skärmbild som visar ett exempel användarkonto som ska tas bort.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
5. <span data-ttu-id="dca66-152">Välj **Slutför** ![slutföra ikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="dca66-152">Select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>

### <a name="step-3-add-your-office-365-organizational-accounts-as-co-administrators-to-the-azure-active-directory-tenant"></a><span data-ttu-id="dca66-153">Steg 3: Lägg till din organisations Office 365-användarkonton som medadministratörer i Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="dca66-153">Step 3: Add your Office 365 organizational accounts as co-administrators to the Azure Active Directory tenant</span></span>
   
1. <span data-ttu-id="dca66-154">Välj den **ADMINISTRATÖRER** och välj sedan **lägga till**.</span><span class="sxs-lookup"><span data-stu-id="dca66-154">Select the **ADMINISTRATORS** tab, and then select **ADD**.</span></span>
   
    ![Skärmbild av Azure klassiska portal administratörer inställningsflik](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
2. <span data-ttu-id="dca66-156">Ange ett organisationskonto för din Office 365-klient, Välj den Azure-prenumerationen och välj sedan **Slutför** ![slutföra ikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span><span class="sxs-lookup"><span data-stu-id="dca66-156">Enter an organizational account of your Office 365 tenant, select the Azure subscription, and then select **Complete** ![complete-icon](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).</span></span>
   
    ![Skärmbild av Azure Lägg till medadministratör dialogruta](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
3. <span data-ttu-id="dca66-158">Gå tillbaka till den **ADMINISTRATÖRER** fliken.</span><span class="sxs-lookup"><span data-stu-id="dca66-158">Go back to the **ADMINISTRATORS** tab.</span></span> <span data-ttu-id="dca66-159">Du bör se organisationskonto visas som medadministratör.</span><span class="sxs-lookup"><span data-stu-id="dca66-159">You should see the organizational account displayed as co-administrator.</span></span>
   
    ![Skärmbild av fliken administratörer](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
4.  <span data-ttu-id="dca66-161">Testa åtkomst till Azure med administratörskontot för samtidigt.</span><span class="sxs-lookup"><span data-stu-id="dca66-161">Test access to Azure with the co-administrator account.</span></span>
   
    <span data-ttu-id="dca66-162">a.</span><span class="sxs-lookup"><span data-stu-id="dca66-162">a.</span></span> <span data-ttu-id="dca66-163">Logga ut från den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dca66-163">Sign out of the Azure classic portal.</span></span>
   
    <span data-ttu-id="dca66-164">b.</span><span class="sxs-lookup"><span data-stu-id="dca66-164">b.</span></span> <span data-ttu-id="dca66-165">Öppna [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="dca66-165">Open the [Azure portal](https://portal.azure.com/).</span></span>
   
    <span data-ttu-id="dca66-166">c.</span><span class="sxs-lookup"><span data-stu-id="dca66-166">c.</span></span> <span data-ttu-id="dca66-167">Ange autentiseringsuppgifterna för medadministratörerna och välj sedan **logga in**.</span><span class="sxs-lookup"><span data-stu-id="dca66-167">Enter the credentials of the co-administrator, and then select **Sign in**.</span></span>
   
    ![Skärmbild av Azure-inloggningssida](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="need-help-contact-support"></a><span data-ttu-id="dca66-169">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="dca66-169">Need help?</span></span> <span data-ttu-id="dca66-170">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="dca66-170">Contact support.</span></span>
<span data-ttu-id="dca66-171">Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="dca66-171">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) to get your issue resolved quickly.</span></span>


