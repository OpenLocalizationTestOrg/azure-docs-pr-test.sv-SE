---
title: "Azure Active Directory Domain Services: Skapa hello Azure AD DC-administratörsgruppen | Microsoft Docs"
description: Aktivera Azure Active Directory Domain Services med hello klassiska Azure-portalen
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
ms.openlocfilehash: d629ab9632ef6a45b549630999ff9a122d60c040
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-classic-portal"></a><span data-ttu-id="09a5e-103">Aktivera Azure Active Directory Domain Services med hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="09a5e-103">Enable Azure Active Directory Domain Services using hello Azure classic portal</span></span>
<span data-ttu-id="09a5e-104">Den här artikeln beskriver och går igenom hello konfigurationsåtgärder som krävs för att du tooenable Azure Active Directory Domain Services (Azure AD DS) för din Azure Active Directory (Azure AD)-klient.</span><span class="sxs-lookup"><span data-stu-id="09a5e-104">This article describes and walks through hello configuration tasks that are required for you tooenable Azure Active Directory Domain Services (Azure AD DS) for your Azure Active Directory (Azure AD) tenant.</span></span>

> [!NOTE]
> <span data-ttu-id="09a5e-105">[**Försök i stället hello (förhandsgranskning) Azure portal avanmäla**](active-directory-ds-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="09a5e-105">[**Try hello new (preview) Azure portal experience instead**](active-directory-ds-getting-started.md).</span></span> 
>

## <a name="task-1-create-hello-azure-ad-dc-administrators-group"></a><span data-ttu-id="09a5e-106">Uppgift 1: skapa administratörsgruppen för hello Azure AD-Domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="09a5e-106">Task 1: create hello Azure AD DC administrators group</span></span>
<span data-ttu-id="09a5e-107">hello första uppgiften är toocreate en administrativ grupp i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="09a5e-107">hello first task is toocreate an administrative group in your Azure AD tenant.</span></span> <span data-ttu-id="09a5e-108">Den här särskilda administrativa gruppen kallas *AAD DC administratörer*.</span><span class="sxs-lookup"><span data-stu-id="09a5e-108">This special administrative group is called *AAD DC Administrators*.</span></span> <span data-ttu-id="09a5e-109">Medlemmar i den här gruppen har beviljats administratörsbehörighet på datorer som är ansluten till domänen toohello Azure Active Directory Domain Services-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="09a5e-109">Members of this group are granted administrative permissions on machines that are domain-joined toohello Azure Active Directory Domain Services-managed domain.</span></span> <span data-ttu-id="09a5e-110">Den här gruppen har lagts till toohello administratörsgruppen på domänanslutna datorer.</span><span class="sxs-lookup"><span data-stu-id="09a5e-110">On domain-joined machines, this group is added toohello administrators group.</span></span> <span data-ttu-id="09a5e-111">Medlemmar i den här gruppen kan dessutom använda Fjärrskrivbord tooconnect via fjärranslutning toodomain anslutna datorer.</span><span class="sxs-lookup"><span data-stu-id="09a5e-111">Additionally, members of this group can use Remote Desktop tooconnect remotely toodomain-joined machines.</span></span>  

> [!NOTE]
> <span data-ttu-id="09a5e-112">Du har inte behörighet som domänadministratör eller företagsadministratör på hello-hanterad domän som du skapat med hjälp av Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="09a5e-112">You do not have Domain Administrator or Enterprise Administrator permissions on hello managed domain that you created by using Azure Active Directory Domain Services.</span></span> <span data-ttu-id="09a5e-113">Dessa behörigheter är reserverade av hello-tjänsten på hanterade domäner och görs inte tillgängliga toousers inom hello-klient.</span><span class="sxs-lookup"><span data-stu-id="09a5e-113">On managed domains, these permissions are reserved by hello service and are not made available toousers within hello tenant.</span></span> <span data-ttu-id="09a5e-114">Du kan dock använda hello särskilda administrativa gruppen skapas i den här konfigurationen uppgiften tooperform vissa Privilegierade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="09a5e-114">However, you can use hello special administrative group created in this configuration task tooperform some privileged operations.</span></span> <span data-ttu-id="09a5e-115">Dessa åtgärder är ansluta till datorer toohello domän, som tillhör toohello administratörsgrupp på domänanslutna datorer och konfigurera Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="09a5e-115">These operations include joining computers toohello domain, belonging toohello administration group on domain-joined machines, and configuring Group Policy.</span></span>
>

<span data-ttu-id="09a5e-116">Uppgiften konfiguration du skapar hello administrativ grupp och Lägg till en eller flera användare i katalogen toohello gruppen.</span><span class="sxs-lookup"><span data-stu-id="09a5e-116">In this configuration task, you create hello administrative group and add one or more users in your directory toohello group.</span></span> <span data-ttu-id="09a5e-117">toocreate hello administrativa gruppen för Azure Active Directory Domain Services, hello följande:</span><span class="sxs-lookup"><span data-stu-id="09a5e-117">toocreate hello administrative group for Azure Active Directory Domain Services, do hello following:</span></span>

1. <span data-ttu-id="09a5e-118">Gå toohello [klassiska Azure-portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="09a5e-118">Go toohello [Azure classic portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="09a5e-119">Välj hello hello vänster **Active Directory** knappen.</span><span class="sxs-lookup"><span data-stu-id="09a5e-119">In hello left pane, select hello **Active Directory** button.</span></span>
3. <span data-ttu-id="09a5e-120">Välj hello Azure AD-klienten (katalogen) som du vill tooenable Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="09a5e-120">Select hello Azure AD tenant (directory) for which you want tooenable Azure Active Directory Domain Services.</span></span> <span data-ttu-id="09a5e-121">Du kan skapa en enda domän för varje Azure AD-katalog.</span><span class="sxs-lookup"><span data-stu-id="09a5e-121">You can create only one domain for each Azure AD directory.</span></span>

    ![Välj Azure AD-katalog](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. <span data-ttu-id="09a5e-123">På hello **preview directory** klickar du på hello **grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="09a5e-123">On hello **preview directory** page, click hello **Groups** tab.</span></span>
5. <span data-ttu-id="09a5e-124">tooadd en grupp tooyour Azure AD-klient hello i åtgärdsfönstret längst ned hello hello-fönstret klickar du på **Lägg till grupp**.</span><span class="sxs-lookup"><span data-stu-id="09a5e-124">tooadd a group tooyour Azure AD tenant, on hello task pane at hello bottom of hello window, click **Add Group**.</span></span>

    ![hello-knappen Lägg till grupp](./media/active-directory-domain-services-getting-started/add-group-button.png)
6. <span data-ttu-id="09a5e-126">I hello **Lägg till grupp** dialogrutan skapar du en grupp med namnet **AAD DC administratörer**, och sedan ange **grupptyp** för**säkerhet**.</span><span class="sxs-lookup"><span data-stu-id="09a5e-126">In hello **Add Group** dialog box, create a group named **AAD DC Administrators**, and then set **Group Type** too**Security**.</span></span>

   > [!WARNING]
   > <span data-ttu-id="09a5e-127">tooenable åtkomst i Azure Active Directory Domain Services-hanterade domänen, skapa en grupp med det exakta namnet.</span><span class="sxs-lookup"><span data-stu-id="09a5e-127">tooenable access within your Azure Active Directory Domain Services-managed domain, create a group with this exact name.</span></span>
   >
   >

    ![dialogrutan Lägg till grupp för hello](./media/active-directory-domain-services-getting-started/create-admin-group.png)
7. <span data-ttu-id="09a5e-129">I hello **beskrivning** ange en beskrivning som gör det möjligt för andra toounderstand att den här gruppen ger administrativ behörighet i Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="09a5e-129">In hello **Description** box, enter a description that enables others toounderstand that this group grants administrative permissions within Azure Active Directory Domain Services.</span></span>
8. <span data-ttu-id="09a5e-130">När du har skapat hello grupp, klickar du på hello grupp namnet tooview dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="09a5e-130">After you've created hello group, click hello group name tooview its properties.</span></span>
9. <span data-ttu-id="09a5e-131">tooadd användare som medlemmar i gruppen hello, längst ned hello hello fönstret klickar du på hello **lägga till medlemmar** knappen.</span><span class="sxs-lookup"><span data-stu-id="09a5e-131">tooadd users as members of hello group, at hello bottom of hello window, click hello **Add Members** button.</span></span>

    ![Lägg till knappen för medlemmar av gruppen](./media/active-directory-domain-services-getting-started/add-group-members-button.png)
10. <span data-ttu-id="09a5e-133">I hello **lägga till medlemmar** dialogrutan Välj hello-användare som ska vara medlemmar i den här gruppen och klicka sedan på hello bock på hello lägre direkt.</span><span class="sxs-lookup"><span data-stu-id="09a5e-133">In hello **Add members** dialog box, select hello users who should be members of this group, and then click hello checkmark icon at hello lower right.</span></span>

    ![Lägg till användare tooadministrators grupp](./media/active-directory-domain-services-getting-started/add-group-members.png)


## <a name="next-step"></a><span data-ttu-id="09a5e-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="09a5e-135">Next step</span></span>
[<span data-ttu-id="09a5e-136">Uppgift 2: skapa eller välj ett virtuellt Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="09a5e-136">Task 2: create or select an Azure virtual network</span></span>](active-directory-ds-getting-started-vnet.md)
