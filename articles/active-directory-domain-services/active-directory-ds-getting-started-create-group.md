---
title: "Azure Active Directory Domain Services: Skapa Azure AD-DC-administratörsgruppen | Microsoft Docs"
description: "Aktivera Azure Active Directory Domain Services med hjälp av den klassiska Azure-portalen"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: maheshu
ms.openlocfilehash: 5ed0125e05928cf0f6d9941e099b433ecb46e6e2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-classic-portal"></a><span data-ttu-id="b9393-103">Aktivera Azure Active Directory Domain Services med hjälp av den klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="b9393-103">Enable Azure Active Directory Domain Services using the Azure classic portal</span></span>
<span data-ttu-id="b9393-104">Den här artikeln beskriver och går igenom konfigurationsuppgifter som krävs om du vill aktivera Azure Active Directory Domain Services (Azure AD DS) för din Azure Active Directory (Azure AD)-klient.</span><span class="sxs-lookup"><span data-stu-id="b9393-104">This article describes and walks through the configuration tasks that are required for you to enable Azure Active Directory Domain Services (Azure AD DS) for your Azure Active Directory (Azure AD) tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="b9393-105">[**Prova den nya portalen (förhandsgranskning) Azure-upplevelsen i stället**](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="b9393-105">[**Try the new (preview) Azure portal experience instead**](active-directory-ds-getting-started.md).</span></span> 
>

## <a name="task-1-create-the-azure-ad-dc-administrators-group"></a><span data-ttu-id="b9393-106">Uppgift 1: skapa administratörsgruppen för Azure AD-Domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="b9393-106">Task 1: create the Azure AD DC administrators group</span></span>
<span data-ttu-id="b9393-107">Den första uppgiften är att skapa en administrativ grupp i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="b9393-107">The first task is to create an administrative group in your Azure AD tenant.</span></span> <span data-ttu-id="b9393-108">Den här särskilda administrativa gruppen kallas *AAD DC administratörer*.</span><span class="sxs-lookup"><span data-stu-id="b9393-108">This special administrative group is called *AAD DC Administrators*.</span></span> <span data-ttu-id="b9393-109">Medlemmar i den här gruppen har beviljats administratörsbehörighet på datorer som är ansluten till domänen till Azure Active Directory Domain Services-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="b9393-109">Members of this group are granted administrative permissions on machines that are domain-joined to the Azure Active Directory Domain Services-managed domain.</span></span> <span data-ttu-id="b9393-110">Den här gruppen har lagts till administratörsgruppen på domänanslutna datorer.</span><span class="sxs-lookup"><span data-stu-id="b9393-110">On domain-joined machines, this group is added to the administrators group.</span></span> <span data-ttu-id="b9393-111">Medlemmar i den här gruppen kan dessutom använda Fjärrskrivbord för att fjärransluta till domänanslutna datorer.</span><span class="sxs-lookup"><span data-stu-id="b9393-111">Additionally, members of this group can use Remote Desktop to connect remotely to domain-joined machines.</span></span>  

> [!NOTE]
> <span data-ttu-id="b9393-112">Du har inte behörighet som domänadministratör eller Företagsadministratörer på den hanterade domänen som du skapat med hjälp av Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="b9393-112">You do not have Domain Administrator or Enterprise Administrator permissions on the managed domain that you created by using Azure Active Directory Domain Services.</span></span> <span data-ttu-id="b9393-113">Dessa behörigheter är reserverade av tjänsten på hanterade domäner och görs inte tillgängliga för användare i klienten.</span><span class="sxs-lookup"><span data-stu-id="b9393-113">On managed domains, these permissions are reserved by the service and are not made available to users within the tenant.</span></span> <span data-ttu-id="b9393-114">Du kan dock använda särskilda administrativa gruppen som skapats i konfigurationsåtgärden utföra vissa Privilegierade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="b9393-114">However, you can use the special administrative group created in this configuration task to perform some privileged operations.</span></span> <span data-ttu-id="b9393-115">Dessa åtgärder omfattar ansluta datorer till domänen, som hör till gruppen administration på domänanslutna datorer och konfigurera Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="b9393-115">These operations include joining computers to the domain, belonging to the administration group on domain-joined machines, and configuring Group Policy.</span></span>
>

<span data-ttu-id="b9393-116">Uppgiften konfiguration du skapar den administrativa gruppen och lägger till en eller flera användare i din katalog i gruppen.</span><span class="sxs-lookup"><span data-stu-id="b9393-116">In this configuration task, you create the administrative group and add one or more users in your directory to the group.</span></span> <span data-ttu-id="b9393-117">Skapa den administrativa gruppen för Azure Active Directory Domain Services genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="b9393-117">To create the administrative group for Azure Active Directory Domain Services, do the following:</span></span>

1. <span data-ttu-id="b9393-118">Gå till den [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="b9393-118">Go to the [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="b9393-119">Välj knappen **Active Directory** i den vänstra rutan.</span><span class="sxs-lookup"><span data-stu-id="b9393-119">In the left pane, select the **Active Directory** button.</span></span>
3. <span data-ttu-id="b9393-120">Välj Azure AD-klienten (katalogen) som du vill aktivera Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="b9393-120">Select the Azure AD tenant (directory) for which you want to enable Azure Active Directory Domain Services.</span></span> <span data-ttu-id="b9393-121">Du kan skapa en enda domän för varje Azure AD-katalog.</span><span class="sxs-lookup"><span data-stu-id="b9393-121">You can create only one domain for each Azure AD directory.</span></span>

    ![Välj Azure AD-katalog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="b9393-123">På den **preview directory** klickar du på den **grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="b9393-123">On the **preview directory** page, click the **Groups** tab.</span></span>
5. <span data-ttu-id="b9393-124">Om du vill lägga till en grupp i din Azure AD-klient, i åtgärdsfönstret längst ned i fönstret klickar du på **Lägg till grupp**.</span><span class="sxs-lookup"><span data-stu-id="b9393-124">To add a group to your Azure AD tenant, on the task pane at the bottom of the window, click **Add Group**.</span></span>

    ![Knappen Lägg till grupp](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. <span data-ttu-id="b9393-126">I den **Lägg till grupp** dialogrutan skapar du en grupp med namnet **AAD DC administratörer**, och sedan ange **grupptyp** till **säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="b9393-126">In the **Add Group** dialog box, create a group named **AAD DC Administrators**, and then set **Group Type** to **Security**.</span></span>

   > [!WARNING]
   > <span data-ttu-id="b9393-127">Skapa en grupp för att möjliggöra åtkomst i Azure Active Directory Domain Services-hanterade domänen med det exakta namnet.</span><span class="sxs-lookup"><span data-stu-id="b9393-127">To enable access within your Azure Active Directory Domain Services-managed domain, create a group with this exact name.</span></span>
   >
   >

    ![Dialogrutan Lägg till grupp](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. <span data-ttu-id="b9393-129">I den **beskrivning** ange en beskrivning som gör det möjligt för andra att förstå att den här gruppen ger administrativ behörighet i Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="b9393-129">In the **Description** box, enter a description that enables others to understand that this group grants administrative permissions within Azure Active Directory Domain Services.</span></span>
8. <span data-ttu-id="b9393-130">När du har skapat gruppen klickar du på gruppnamnet att visa dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="b9393-130">After you've created the group, click the group name to view its properties.</span></span>
9. <span data-ttu-id="b9393-131">Om du vill lägga till användare som medlemmar i gruppen längst ned i fönstret klickar du på den **lägga till medlemmar** knappen.</span><span class="sxs-lookup"><span data-stu-id="b9393-131">To add users as members of the group, at the bottom of the window, click the **Add Members** button.</span></span>

    ![Lägg till knappen för medlemmar av gruppen](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. <span data-ttu-id="b9393-133">I den **lägga till medlemmar** dialogrutan väljer du de användare som ska vara medlemmar i den här gruppen och klicka sedan på bock längst ned till höger.</span><span class="sxs-lookup"><span data-stu-id="b9393-133">In the **Add members** dialog box, select the users who should be members of this group, and then click the checkmark icon at the lower right.</span></span>

    ![Lägga till användare i administratörsgruppen](./media/active-directory-domain-services-getting-started/add-group-members.png)


## <a name="next-step"></a><span data-ttu-id="b9393-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b9393-135">Next step</span></span>
[<span data-ttu-id="b9393-136">Uppgift 2: skapa eller välj ett virtuellt Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="b9393-136">Task 2: create or select an Azure virtual network</span></span>](active-directory-ds-getting-started-vnet.md)
