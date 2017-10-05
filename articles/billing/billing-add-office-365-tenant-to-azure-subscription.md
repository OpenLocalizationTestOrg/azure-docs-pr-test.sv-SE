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
# <a name="associate-an-office-365-tenant-to-an-azure-subscription"></a>Koppla en Office 365-klient till en Azure-prenumeration
Länka dina separat Azure och Office 365-prenumerationer så att du kan komma åt Office 365-klient från din Azure-prenumeration. Logga in på Azure med Azure-tjänstens konto om du vill länka dina prenumerationer, lägga till en katalog och lägga till Office 365-organisationskonton i Azure Active Directory-klient.

Om du vill använda Office 365-prenumeration för användare i din Azure Active Directory-instans eller du har en Office 365-konto men inte ett Azure-konto, se [registrera dig för Azure med Office 365-konto](billing-use-existing-office-365-account-azure-subscription.md). 

## <a name="before-you-begin"></a>Innan du börjar
* Du måste ha autentiseringsuppgifter som tjänstadministratör för Azure-prenumeration. Medadministratör konton kan inte göra vissa av stegen i den här artikeln. Om du vill ändra tjänstadministratören [lägga till eller ändra Azure-administratörsroller](billing-add-change-azure-subscription-administrator.md#change-service-administrator-for-a-subscription).
* Du måste ha autentiseringsuppgifter för en global administratör för Office 365-klienten.
* Tjänstens e-postadress får inte vara i Office 365-klienten.
* Tjänstadministratören e-postadress måste inte matcha en global administratör för Office 365-klienten.
* Om du använder en e-postadress som är både ett Microsoft-konto och ett organisationskonto kan du tillfälligt ändra tjänstadministratören för din Azure-prenumeration om du vill använda ett annat microsoftkonto. Du kan skapa ett Microsoft-konto på den [inloggningssidan för Microsoft-konto](https://signup.live.com/).

## <a name="link-office-365-tenant-to-azure-subscription"></a>Länken Office 365-klient till Azure-prenumeration
Följ dessa steg om du vill associera Office 365-klient till Azure-prenumerationen:

### <a name="step-1-add-office-365-tenant-to-your-azure-subscription"></a>Steg 1: Lägg till Office 365-klient till din Azure-prenumeration

1. Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com/) med administratörsautentiseringsuppgifter för tjänsten.

    ![Skärmbild av Azure-inloggning](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

2. I den vänstra rutan, Välj **ACTIVE DIRECTORY**. Du bör se Office 365-klient. Om du ser det gå vidare till [steg 2: Ändra katalogen som är associerade med Azure-prenumerationen](#Step2).
   
   ![Skärmbild av Active Directory-post](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

3. Välj **ny** > **DIRECTORY** > **anpassade skapa**.
   
    ![Skapa anpassade Skärmbild av Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)
   
4. På den **Lägg till katalogen** sidan under **DIRECTORY**väljer **Använd befintlig katalog**. Välj sedan **jag är redo att bli utloggad nu**, och välj **Slutför** ![slutföra ikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Skärmbild av ”Använd befintlig katalog”](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)
   
5. När du har loggat, logga in med autentiseringsuppgifterna för den globala administratören för din Office 365-klient.
   
    ![Skärmbild av Office 365 global administratör logga in](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)
   
6. Välj **fortsätta**.
   
    ![Skärmbild av verifieringen](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)
   
7. Välj **logga ut nu**.
   
    ![Skärmbild av utloggning](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)
   
8. Logga in på den [klassiska Azure-portalen](https://manage.windowsazure.com/) med administratörsautentiseringsuppgifter för tjänsten.
   
    ![Skärmbild av Azure-inloggning](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)
   
9. Du bör se din Office 365-klient på instrumentpanelen.
   
    ![Skärmbild av instrumentpanelen](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

### <a name="Step2"></a>Steg 2: Ändra katalog som hör till Azure-prenumeration
   
1. Välj **inställningar**.
   
    ![Skärmbild av Azure klassiska portal inställningsikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)
   
2. Välj din Azure-prenumeration och välj sedan **redigera katalog**.

    ![Skärmbild av Azure-prenumeration redigera katalog](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)
   
3. Välj **nästa** ![nästa ikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).
   
    ![Skärmbild av ”ändra katalogen associerade”](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)
   
4. Granska de berörda kontona. Alla medadministratörer och [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-configure.md) användare med tilldelade åtkomst i befintliga resursgrupper tas bort. Varningen visas nämner bara borttagning av medadministratörer.
      
    ![Skärmbild som visar kontona som medadministratör kan tas bort.](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)
   
    ![Skärmbild som visar ett exempel användarkonto som ska tas bort.](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)
   
5. Välj **Slutför** ![slutföra ikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

### <a name="step-3-add-your-office-365-organizational-accounts-as-co-administrators-to-the-azure-active-directory-tenant"></a>Steg 3: Lägg till din organisations Office 365-användarkonton som medadministratörer i Azure Active Directory-klient
   
1. Välj den **ADMINISTRATÖRER** och välj sedan **lägga till**.
   
    ![Skärmbild av Azure klassiska portal administratörer inställningsflik](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)
   
2. Ange ett organisationskonto för din Office 365-klient, Välj den Azure-prenumerationen och välj sedan **Slutför** ![slutföra ikonen](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).
   
    ![Skärmbild av Azure Lägg till medadministratör dialogruta](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)
   
3. Gå tillbaka till den **ADMINISTRATÖRER** fliken. Du bör se organisationskonto visas som medadministratör.
   
    ![Skärmbild av fliken administratörer](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)
4.  Testa åtkomst till Azure med administratörskontot för samtidigt.
   
    a. Logga ut från den klassiska Azure-portalen.
   
    b. Öppna [Azure-portalen](https://portal.azure.com/).
   
    c. Ange autentiseringsuppgifterna för medadministratörerna och välj sedan **logga in**.
   
    ![Skärmbild av Azure-inloggningssida](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="need-help-contact-support"></a>Behöver du hjälp? Kontakta supporten.
Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) få snabbt lösa problemet.


