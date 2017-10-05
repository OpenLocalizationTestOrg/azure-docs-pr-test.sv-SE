---
title: "Azure Active Directory Domain Services: Komma igång | Microsoft Docs"
description: "Aktivera Azure Active Directory Domain Services med Azure-portalen (förhandsgranskning)"
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
ms.date: 07/15/2017
ms.author: maheshu
ms.openlocfilehash: f87bcf33d3b1eb21c7d84814e4c4086f664e293d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a><span data-ttu-id="897c0-103">Aktivera Azure Active Directory Domain Services med Azure-portalen (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="897c0-103">Enable Azure Active Directory Domain Services using the Azure portal (Preview)</span></span>


## <a name="task-3-configure-administrative-group"></a><span data-ttu-id="897c0-104">Uppgift 3: Konfigurera administrativ grupp</span><span class="sxs-lookup"><span data-stu-id="897c0-104">Task 3: configure administrative group</span></span>
<span data-ttu-id="897c0-105">I den här uppgiften skapar du en administrativ grupp i Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="897c0-105">In this configuration task, you create an administrative group in your Azure AD directory.</span></span> <span data-ttu-id="897c0-106">Den här särskilda administrativa gruppen kallas *AAD DC administratörer*.</span><span class="sxs-lookup"><span data-stu-id="897c0-106">This special administrative group is called *AAD DC Administrators*.</span></span> <span data-ttu-id="897c0-107">Medlemmar i den här gruppen har beviljats administratörsbehörighet på datorer som är ansluten till domänen i den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="897c0-107">Members of this group are granted administrative permissions on machines that are domain-joined to the managed domain.</span></span> <span data-ttu-id="897c0-108">Den här gruppen har lagts till administratörsgruppen på domänanslutna datorer.</span><span class="sxs-lookup"><span data-stu-id="897c0-108">On domain-joined machines, this group is added to the administrators group.</span></span> <span data-ttu-id="897c0-109">Medlemmar i den här gruppen kan dessutom använda Fjärrskrivbord för att fjärransluta till domänanslutna datorer.</span><span class="sxs-lookup"><span data-stu-id="897c0-109">Additionally, members of this group can use Remote Desktop to connect remotely to domain-joined machines.</span></span>

> [!NOTE]
> <span data-ttu-id="897c0-110">Du har inte behörighet som domänadministratör eller Företagsadministratörer på den hanterade domänen som du skapat med hjälp av Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="897c0-110">You do not have Domain Administrator or Enterprise Administrator permissions on the managed domain that you created by using Azure Active Directory Domain Services.</span></span> <span data-ttu-id="897c0-111">Dessa behörigheter är reserverade av tjänsten på hanterade domäner och görs inte tillgängliga för användare i klienten.</span><span class="sxs-lookup"><span data-stu-id="897c0-111">On managed domains, these permissions are reserved by the service and are not made available to users within the tenant.</span></span> <span data-ttu-id="897c0-112">Du kan dock använda särskilda administrativa gruppen som skapats i konfigurationsåtgärden utföra vissa Privilegierade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="897c0-112">However, you can use the special administrative group created in this configuration task to perform some privileged operations.</span></span> <span data-ttu-id="897c0-113">Dessa åtgärder omfattar ansluta datorer till domänen, som hör till gruppen administration på domänanslutna datorer och konfigurera Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="897c0-113">These operations include joining computers to the domain, belonging to the administration group on domain-joined machines, and configuring Group Policy.</span></span>
>

<span data-ttu-id="897c0-114">Den administrativa gruppen skapas automatiskt i Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="897c0-114">The wizard automatically creates the administrative group in your Azure AD directory.</span></span> <span data-ttu-id="897c0-115">Den här gruppen kallas AAD DC-administratörer.</span><span class="sxs-lookup"><span data-stu-id="897c0-115">This group is called 'AAD DC Administrators'.</span></span> <span data-ttu-id="897c0-116">Om du har en befintlig grupp med det här namnet i Azure AD-katalogen, väljs den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="897c0-116">If you have an existing group with this name in your Azure AD directory, the wizard selects this group.</span></span> <span data-ttu-id="897c0-117">Du kan konfigurera medlemskap med den **administratörsgruppen** sidan i guiden.</span><span class="sxs-lookup"><span data-stu-id="897c0-117">You can configure group membership using the **Administrator group** wizard page.</span></span>

1. <span data-ttu-id="897c0-118">För att konfigurera gruppmedlemskap, klickar du på **AAD DC administratörer**.</span><span class="sxs-lookup"><span data-stu-id="897c0-118">To configure group membership, click **AAD DC Administrators**.</span></span>

    ![Konfigurera gruppmedlemskap](./media/getting-started/domain-services-blade-admingroup.png)

2. <span data-ttu-id="897c0-120">Klicka på den **lägga till medlemmar** vill lägga till användare från din Azure AD-katalog i administratörsgruppen.</span><span class="sxs-lookup"><span data-stu-id="897c0-120">Click the **Add members** button to add users from your Azure AD directory to the administrator group.</span></span>

3. <span data-ttu-id="897c0-121">När du är klar klickar du på **OK** att gå vidare till den **sammanfattning** sidan i guiden.</span><span class="sxs-lookup"><span data-stu-id="897c0-121">When you are done, click **OK** to move on to the **Summary** page of the wizard.</span></span>

4. <span data-ttu-id="897c0-122">På den **sammanfattning** sidan i guiden, granska konfigurationsinställningarna för den hanterade domänen.</span><span class="sxs-lookup"><span data-stu-id="897c0-122">On the **Summary** page of the wizard, review the configuration settings for the managed domain.</span></span> <span data-ttu-id="897c0-123">Du kan gå tillbaka till något steg i guiden för att göra ändringar, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="897c0-123">You can go back to any step of the wizard to make changes, if necessary.</span></span> <span data-ttu-id="897c0-124">När du är klar klickar du på **OK** att skapa den nya Hantera domänen.</span><span class="sxs-lookup"><span data-stu-id="897c0-124">When you are done, click **OK** to create the new managed domain.</span></span>

    ![Sammanfattning](./media/getting-started/domain-services-blade-summary.png)

5. <span data-ttu-id="897c0-126">Du ser ett meddelande som visar förloppet för distributionen av Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="897c0-126">You see a notification that shows the progress of your Azure AD Domain Services deployment.</span></span> <span data-ttu-id="897c0-127">Klicka på meddelandet om du vill se detaljerad status för distributionen.</span><span class="sxs-lookup"><span data-stu-id="897c0-127">Click the notification to see detailed progress for the deployment.</span></span>

    ![Meddelande - distribution pågår](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="provision-your-managed-domain"></a><span data-ttu-id="897c0-129">Etablera din hanterade domän</span><span class="sxs-lookup"><span data-stu-id="897c0-129">Provision your managed domain</span></span>
<span data-ttu-id="897c0-130">Processen för etablering din hanterade domän kan ta upp till en timme.</span><span class="sxs-lookup"><span data-stu-id="897c0-130">The process of provisioning your managed domain can take up to an hour.</span></span>

1. <span data-ttu-id="897c0-131">När din distribution pågår kan du söka efter domain services i den **söka resurser** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="897c0-131">While your deployment is in progress, you can search for 'domain services' in the **Search resources** search box.</span></span> <span data-ttu-id="897c0-132">Välj **Azure AD Domain Services** från sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="897c0-132">Select **Azure AD Domain Services** from the search result.</span></span> <span data-ttu-id="897c0-133">Den **Azure AD Domain Services** bladet visar den hanterade domänen som har etablerats.</span><span class="sxs-lookup"><span data-stu-id="897c0-133">The **Azure AD Domain Services** blade lists the managed domain that is being provisioned.</span></span>

    ![Hitta hanterad domän som har etablerats](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. <span data-ttu-id="897c0-135">Klicka på namnet på den hanterade domänen (till exempel ”contoso100.com”) för att se mer information om domänen.</span><span class="sxs-lookup"><span data-stu-id="897c0-135">Click the name of the managed domain (for example, 'contoso100.com') to see more details about the domain.</span></span>

    ![DS - Etableringsstatus](./media/getting-started/domain-services-provisioning-state.png)

3. <span data-ttu-id="897c0-137">Den **översikt** visar att domänen håller på att etableras.</span><span class="sxs-lookup"><span data-stu-id="897c0-137">The **Overview** tab shows that the domain is currently being provisioned.</span></span> <span data-ttu-id="897c0-138">Du kan inte konfigurera den hanterade domänen förrän den är helt etablerad.</span><span class="sxs-lookup"><span data-stu-id="897c0-138">You cannot configure the managed domain until it is fully provisioned.</span></span> <span data-ttu-id="897c0-139">Det kan ta upp till en timme för din hanterade domän ska etableras fullständigt.</span><span class="sxs-lookup"><span data-stu-id="897c0-139">It may take up to an hour for your managed domain to be fully provisioned.</span></span>

    ![<span data-ttu-id="897c0-140">DS - översiktsflik under etableringsstatusen</span><span class="sxs-lookup"><span data-stu-id="897c0-140">Domain Services - Overview tab during the provisioning state</span></span> ](./media/getting-started/domain-services-provisioning-state-details.png)

4. <span data-ttu-id="897c0-141">När den hanterade domänen är helt etablerad, den **översikt** visar status för domänen som **kör**.</span><span class="sxs-lookup"><span data-stu-id="897c0-141">When the managed domain is fully provisioned, the **Overview** tab shows the domain status as **Running**.</span></span>

    ![Domain Services - översiktsflik vid full etablering](./media/getting-started/domain-services-provisioned.png)

5. <span data-ttu-id="897c0-143">På den **egenskaper** kan du se två IP-adresser på vilken domän domänkontrollanter är tillgängliga för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="897c0-143">On the **Properties** tab, you see two IP addresses at which domain controllers are available for the virtual network.</span></span>

    ![Domain Services – fliken Egenskaper när helt etablerad](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="next-step"></a><span data-ttu-id="897c0-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="897c0-145">Next step</span></span>
<span data-ttu-id="897c0-146">Uppgift 4: [uppdatera DNS-inställningarna för det virtuella Azure-nätverket](active-directory-ds-getting-started-dns.md)</span><span class="sxs-lookup"><span data-stu-id="897c0-146">[Task 4: update the DNS settings for the Azure virtual network](active-directory-ds-getting-started-dns.md)</span></span>
