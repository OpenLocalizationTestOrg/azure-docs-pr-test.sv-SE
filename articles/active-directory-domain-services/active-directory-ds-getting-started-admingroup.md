---
title: "Azure Active Directory Domain Services: Komma igång | Microsoft Docs"
description: "Aktivera Azure Active Directory Domain Services med hello Azure-portalen (förhandsgranskning)"
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
ms.openlocfilehash: 8bde872a13bc9960d1e62c74017ff78a8953a0a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-azure-active-directory-domain-services-using-hello-azure-portal-preview"></a><span data-ttu-id="846b9-103">Aktivera Azure Active Directory Domain Services med hello Azure-portalen (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="846b9-103">Enable Azure Active Directory Domain Services using hello Azure portal (Preview)</span></span>


## <a name="task-3-configure-administrative-group"></a><span data-ttu-id="846b9-104">Uppgift 3: Konfigurera administrativ grupp</span><span class="sxs-lookup"><span data-stu-id="846b9-104">Task 3: configure administrative group</span></span>
<span data-ttu-id="846b9-105">I den här uppgiften skapar du en administrativ grupp i Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="846b9-105">In this configuration task, you create an administrative group in your Azure AD directory.</span></span> <span data-ttu-id="846b9-106">Den här särskilda administrativa gruppen kallas *AAD DC administratörer*.</span><span class="sxs-lookup"><span data-stu-id="846b9-106">This special administrative group is called *AAD DC Administrators*.</span></span> <span data-ttu-id="846b9-107">Medlemmar i den här gruppen har beviljats administratörsbehörighet på datorer som är ansluten till domänen toohello hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="846b9-107">Members of this group are granted administrative permissions on machines that are domain-joined toohello managed domain.</span></span> <span data-ttu-id="846b9-108">Den här gruppen har lagts till toohello administratörsgruppen på domänanslutna datorer.</span><span class="sxs-lookup"><span data-stu-id="846b9-108">On domain-joined machines, this group is added toohello administrators group.</span></span> <span data-ttu-id="846b9-109">Medlemmar i den här gruppen kan dessutom använda Fjärrskrivbord tooconnect via fjärranslutning toodomain anslutna datorer.</span><span class="sxs-lookup"><span data-stu-id="846b9-109">Additionally, members of this group can use Remote Desktop tooconnect remotely toodomain-joined machines.</span></span>

> [!NOTE]
> <span data-ttu-id="846b9-110">Du har inte behörighet som domänadministratör eller företagsadministratör på hello-hanterad domän som du skapat med hjälp av Azure Active Directory Domain Services.</span><span class="sxs-lookup"><span data-stu-id="846b9-110">You do not have Domain Administrator or Enterprise Administrator permissions on hello managed domain that you created by using Azure Active Directory Domain Services.</span></span> <span data-ttu-id="846b9-111">Dessa behörigheter är reserverade av hello-tjänsten på hanterade domäner och görs inte tillgängliga toousers inom hello-klient.</span><span class="sxs-lookup"><span data-stu-id="846b9-111">On managed domains, these permissions are reserved by hello service and are not made available toousers within hello tenant.</span></span> <span data-ttu-id="846b9-112">Du kan dock använda hello särskilda administrativa gruppen skapas i den här konfigurationen uppgiften tooperform vissa Privilegierade åtgärder.</span><span class="sxs-lookup"><span data-stu-id="846b9-112">However, you can use hello special administrative group created in this configuration task tooperform some privileged operations.</span></span> <span data-ttu-id="846b9-113">Dessa åtgärder är ansluta till datorer toohello domän, som tillhör toohello administratörsgrupp på domänanslutna datorer och konfigurera Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="846b9-113">These operations include joining computers toohello domain, belonging toohello administration group on domain-joined machines, and configuring Group Policy.</span></span>
>

<span data-ttu-id="846b9-114">hello guiden skapar automatiskt hello administrativ grupp i Azure AD-katalogen.</span><span class="sxs-lookup"><span data-stu-id="846b9-114">hello wizard automatically creates hello administrative group in your Azure AD directory.</span></span> <span data-ttu-id="846b9-115">Den här gruppen kallas AAD DC-administratörer.</span><span class="sxs-lookup"><span data-stu-id="846b9-115">This group is called 'AAD DC Administrators'.</span></span> <span data-ttu-id="846b9-116">Om du har en befintlig grupp med det här namnet i Azure AD-katalogen väljs hello den här gruppen.</span><span class="sxs-lookup"><span data-stu-id="846b9-116">If you have an existing group with this name in your Azure AD directory, hello wizard selects this group.</span></span> <span data-ttu-id="846b9-117">Du kan konfigurera gruppmedlemskap med hello **administratörsgruppen** sidan i guiden.</span><span class="sxs-lookup"><span data-stu-id="846b9-117">You can configure group membership using hello **Administrator group** wizard page.</span></span>

1. <span data-ttu-id="846b9-118">tooconfigure gruppmedlemskap, klickar du på **AAD DC administratörer**.</span><span class="sxs-lookup"><span data-stu-id="846b9-118">tooconfigure group membership, click **AAD DC Administrators**.</span></span>

    ![Konfigurera gruppmedlemskap](./media/getting-started/domain-services-blade-admingroup.png)

2. <span data-ttu-id="846b9-120">Klicka på hello **lägga till medlemmar** knappen tooadd användare från din Azure AD directory toohello administratörsgruppen.</span><span class="sxs-lookup"><span data-stu-id="846b9-120">Click hello **Add members** button tooadd users from your Azure AD directory toohello administrator group.</span></span>

3. <span data-ttu-id="846b9-121">När du är klar klickar du på **OK** toomove på toohello **sammanfattning** hello guiden.</span><span class="sxs-lookup"><span data-stu-id="846b9-121">When you are done, click **OK** toomove on toohello **Summary** page of hello wizard.</span></span>

4. <span data-ttu-id="846b9-122">På hello **sammanfattning** hello guiden granska hello konfigurationsinställningar för hello-hanterad domän.</span><span class="sxs-lookup"><span data-stu-id="846b9-122">On hello **Summary** page of hello wizard, review hello configuration settings for hello managed domain.</span></span> <span data-ttu-id="846b9-123">Du kan gå tillbaka tooany steg i hello guiden toomake ändringar om det behövs.</span><span class="sxs-lookup"><span data-stu-id="846b9-123">You can go back tooany step of hello wizard toomake changes, if necessary.</span></span> <span data-ttu-id="846b9-124">När du är klar klickar du på **OK** toocreate hello nya hanterade domän.</span><span class="sxs-lookup"><span data-stu-id="846b9-124">When you are done, click **OK** toocreate hello new managed domain.</span></span>

    ![Sammanfattning](./media/getting-started/domain-services-blade-summary.png)

5. <span data-ttu-id="846b9-126">Du ser ett meddelande som visar hello förloppet för distributionen av Azure AD Domain Services.</span><span class="sxs-lookup"><span data-stu-id="846b9-126">You see a notification that shows hello progress of your Azure AD Domain Services deployment.</span></span> <span data-ttu-id="846b9-127">Klicka på hello-meddelande toosee beskrivs förloppet för hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="846b9-127">Click hello notification toosee detailed progress for hello deployment.</span></span>

    ![Meddelande - distribution pågår](./media/getting-started/domain-services-blade-deployment-in-progress.png)


## <a name="provision-your-managed-domain"></a><span data-ttu-id="846b9-129">Etablera din hanterade domän</span><span class="sxs-lookup"><span data-stu-id="846b9-129">Provision your managed domain</span></span>
<span data-ttu-id="846b9-130">hello processen för etablering din hanterade domän kan ta upp tooan timme.</span><span class="sxs-lookup"><span data-stu-id="846b9-130">hello process of provisioning your managed domain can take up tooan hour.</span></span>

1. <span data-ttu-id="846b9-131">När din distribution pågår, kan du söka efter domain services i hello **söka resurser** sökrutan.</span><span class="sxs-lookup"><span data-stu-id="846b9-131">While your deployment is in progress, you can search for 'domain services' in hello **Search resources** search box.</span></span> <span data-ttu-id="846b9-132">Välj **Azure AD Domain Services** från hello sökresultatet.</span><span class="sxs-lookup"><span data-stu-id="846b9-132">Select **Azure AD Domain Services** from hello search result.</span></span> <span data-ttu-id="846b9-133">Hej **Azure AD Domain Services** bladet visar hello-hanterad domän som har etablerats.</span><span class="sxs-lookup"><span data-stu-id="846b9-133">hello **Azure AD Domain Services** blade lists hello managed domain that is being provisioned.</span></span>

    ![Hitta hanterad domän som har etablerats](./media/getting-started/domain-services-provisioning-state-find-resource.png)

2. <span data-ttu-id="846b9-135">Klicka på hello hanterade domän (till exempel ”contoso100.com”) toosee hello namn mer information om hello domän.</span><span class="sxs-lookup"><span data-stu-id="846b9-135">Click hello name of hello managed domain (for example, 'contoso100.com') toosee more details about hello domain.</span></span>

    ![DS - Etableringsstatus](./media/getting-started/domain-services-provisioning-state.png)

3. <span data-ttu-id="846b9-137">Hej **översikt** visar hello domänen håller på att etableras.</span><span class="sxs-lookup"><span data-stu-id="846b9-137">hello **Overview** tab shows that hello domain is currently being provisioned.</span></span> <span data-ttu-id="846b9-138">Du kan konfigurera hello-hanterad domän tills den är helt etablerad.</span><span class="sxs-lookup"><span data-stu-id="846b9-138">You cannot configure hello managed domain until it is fully provisioned.</span></span> <span data-ttu-id="846b9-139">Det kan ta upp tooan timme för din hanterade domän toobe helt etablerad.</span><span class="sxs-lookup"><span data-stu-id="846b9-139">It may take up tooan hour for your managed domain toobe fully provisioned.</span></span>

    ![<span data-ttu-id="846b9-140">DS - översiktsflik under hello Etableringsstatus</span><span class="sxs-lookup"><span data-stu-id="846b9-140">Domain Services - Overview tab during hello provisioning state</span></span> ](./media/getting-started/domain-services-provisioning-state-details.png)

4. <span data-ttu-id="846b9-141">När hello-hanterad domän är helt etablerad, hello **översikt** visar status för hello som **kör**.</span><span class="sxs-lookup"><span data-stu-id="846b9-141">When hello managed domain is fully provisioned, hello **Overview** tab shows hello domain status as **Running**.</span></span>

    ![Domain Services - översiktsflik vid full etablering](./media/getting-started/domain-services-provisioned.png)

5. <span data-ttu-id="846b9-143">På hello **egenskaper** kan du se två IP-adresser på vilken domän domänkontrollanter är tillgängliga för hello virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="846b9-143">On hello **Properties** tab, you see two IP addresses at which domain controllers are available for hello virtual network.</span></span>

    ![Domain Services – fliken Egenskaper när helt etablerad](./media/getting-started/domain-services-provisioned-properties.png)


## <a name="next-step"></a><span data-ttu-id="846b9-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="846b9-145">Next step</span></span>
[<span data-ttu-id="846b9-146">Uppgift 4: uppdatera hello DNS-inställningarna för hello Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="846b9-146">Task 4: update hello DNS settings for hello Azure virtual network</span></span>](active-directory-ds-getting-started-dns.md)
