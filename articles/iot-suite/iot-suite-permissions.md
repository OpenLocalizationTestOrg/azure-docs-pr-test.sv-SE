---
title: aaaAzure IoT Suite och Azure Active Directory | Microsoft Docs
description: "Beskriver hur Azure IoT Suite använder Azure Active Directory toomanage behörigheter."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 246228ba-954a-4d96-b6d6-e53e4590cb4f
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/09/2017
ms.author: dobett
ms.openlocfilehash: 4768630f2de4bb431731fbd4e8929232bc80b9f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="permissions-on-hello-azureiotsuitecom-site"></a><span data-ttu-id="398c8-103">Behörigheter för hello azureiotsuite.com plats</span><span class="sxs-lookup"><span data-stu-id="398c8-103">Permissions on hello azureiotsuite.com site</span></span>

## <a name="what-happens-when-you-sign-in"></a><span data-ttu-id="398c8-104">Vad händer när du loggar in</span><span class="sxs-lookup"><span data-stu-id="398c8-104">What happens when you sign in</span></span>

<span data-ttu-id="398c8-105">Hej första gången du loggar in på [azureiotsuite.com][lnk-azureiotsuite], hello platsen avgör hello behörighetsnivåer baserat på hello för närvarande valda Azure Active Directory (AAD)-klient och Azure prenumeration.</span><span class="sxs-lookup"><span data-stu-id="398c8-105">hello first time you sign in at [azureiotsuite.com][lnk-azureiotsuite], hello site determines hello permission levels you have based on hello currently selected Azure Active Directory (AAD) tenant and Azure subscription.</span></span>

1. <span data-ttu-id="398c8-106">Först toopopulate hello lista över klienter visas nästa tooyour användarnamn, hello plats hittar från Azure vilka AAD-klienter som du tillhör.</span><span class="sxs-lookup"><span data-stu-id="398c8-106">First, toopopulate hello list of tenants seen next tooyour username, hello site finds out from Azure which AAD tenants you belong to.</span></span> <span data-ttu-id="398c8-107">Hello plats kan för närvarande kan endast hämta användartoken för en klient i taget.</span><span class="sxs-lookup"><span data-stu-id="398c8-107">Currently, hello site can only obtain user tokens for one tenant at a time.</span></span> <span data-ttu-id="398c8-108">Därför när du växlar klienter med hjälp av hello listrutan i hello övre högra hörnet loggar hello plats du i toothat klient tooobtain hello token för den klienten.</span><span class="sxs-lookup"><span data-stu-id="398c8-108">Therefore, when you switch tenants using hello dropdown in hello top right corner, hello site logs you in toothat tenant tooobtain hello tokens for that tenant.</span></span>

2. <span data-ttu-id="398c8-109">Därefter hittar hello plats från Azure vilka prenumerationer som du har associerat med hello valt klient.</span><span class="sxs-lookup"><span data-stu-id="398c8-109">Next, hello site finds out from Azure which subscriptions you have associated with hello selected tenant.</span></span> <span data-ttu-id="398c8-110">Du kan se hello tillgängliga prenumerationer när du skapar en ny förkonfigurerade lösning.</span><span class="sxs-lookup"><span data-stu-id="398c8-110">You see hello available subscriptions when you create a new preconfigured solution.</span></span>

3. <span data-ttu-id="398c8-111">Slutligen hello plats hämtar alla hello resurser i hello prenumerationer och resursgrupper märks med förkonfigurerade lösningar och fyller hello paneler på hello startsidan.</span><span class="sxs-lookup"><span data-stu-id="398c8-111">Finally, hello site retrieves all hello resources in hello subscriptions and resource groups tagged as preconfigured solutions and populates hello tiles on hello home page.</span></span>

<span data-ttu-id="398c8-112">hello följande avsnitt beskrivs hello roller som styr toohello förkonfigurerade lösningar.</span><span class="sxs-lookup"><span data-stu-id="398c8-112">hello following sections describe hello roles that control access toohello preconfigured solutions.</span></span>

## <a name="aad-roles"></a><span data-ttu-id="398c8-113">AAD-roller</span><span class="sxs-lookup"><span data-stu-id="398c8-113">AAD roles</span></span>

<span data-ttu-id="398c8-114">Hej AAD roller styr hello kan etablera förkonfigurerade lösningar och hantera användare i en förkonfigurerade lösning.</span><span class="sxs-lookup"><span data-stu-id="398c8-114">hello AAD roles control hello ability provision preconfigured solutions and manage users in a preconfigured solution.</span></span>

<span data-ttu-id="398c8-115">Du hittar mer information om administratörsroller i AAD i [Tilldela administratörsroller i Azure AD][lnk-aad-admin].</span><span class="sxs-lookup"><span data-stu-id="398c8-115">You can find more information about administrator roles in AAD in [Assigning administrator roles in Azure AD][lnk-aad-admin].</span></span> <span data-ttu-id="398c8-116">hello aktuella artikeln fokuserar på hello **Global administratör** och hello **användaren** directory roller som används av hello förkonfigurerade lösningar.</span><span class="sxs-lookup"><span data-stu-id="398c8-116">hello current article focuses on hello **Global Administrator** and hello **User** directory roles as used by hello preconfigured solutions.</span></span>

### <a name="global-administrator"></a><span data-ttu-id="398c8-117">Global administratör</span><span class="sxs-lookup"><span data-stu-id="398c8-117">Global administrator</span></span>

<span data-ttu-id="398c8-118">Det kan finnas flera globala administratörer per AAD-klient:</span><span class="sxs-lookup"><span data-stu-id="398c8-118">There can be many global administrators per AAD tenant:</span></span>

* <span data-ttu-id="398c8-119">När du skapar en AAD-klient är som standard hello global administratör för att klienten.</span><span class="sxs-lookup"><span data-stu-id="398c8-119">When you create an AAD tenant, you are by default hello global administrator of that tenant.</span></span>
* <span data-ttu-id="398c8-120">hello global administratör kan etablera en förkonfigurerade lösning och har tilldelats en **Admin** roll för hello program i deras AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="398c8-120">hello global administrator can provision a preconfigured solution and is assigned an **Admin** role for hello application inside their AAD tenant.</span></span>
* <span data-ttu-id="398c8-121">Om en annan användare i hello samma AAD-klient skapar ett program är hello standardroll beviljas toohello global administratör **ReadOnly**.</span><span class="sxs-lookup"><span data-stu-id="398c8-121">If another user in hello same AAD tenant creates an application, hello default role granted toohello global administrator is **ReadOnly**.</span></span>
* <span data-ttu-id="398c8-122">En global administratör kan tilldela användare tooroles för program som använder hello [Azure-portalen][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="398c8-122">A global administrator can assign users tooroles for applications using hello [Azure portal][lnk-portal].</span></span>

### <a name="domain-user"></a><span data-ttu-id="398c8-123">Domänanvändare</span><span class="sxs-lookup"><span data-stu-id="398c8-123">Domain user</span></span>

<span data-ttu-id="398c8-124">Det kan finnas många domänanvändare per AAD-klient:</span><span class="sxs-lookup"><span data-stu-id="398c8-124">There can be many domain users per AAD tenant:</span></span>

* <span data-ttu-id="398c8-125">En domänanvändare kan etablera en förkonfigurerade lösningen genom hello [azureiotsuite.com] [ lnk-azureiotsuite] plats.</span><span class="sxs-lookup"><span data-stu-id="398c8-125">A domain user can provision a preconfigured solution through hello [azureiotsuite.com][lnk-azureiotsuite] site.</span></span> <span data-ttu-id="398c8-126">Som standard beviljas hello domänanvändare hello **Admin** roll i hello etablerats program.</span><span class="sxs-lookup"><span data-stu-id="398c8-126">By default, hello domain user is granted hello **Admin** role in hello provisioned application.</span></span>
* <span data-ttu-id="398c8-127">En domänanvändare kan skapa ett program med hello build.cmd skript i hello [azure iot-fjärr-övervakning][lnk-rm-github-repo], [azure iot – förutsägande-Underhåll] [ lnk-pm-github-repo], eller [azure iot-ansluten-fabrik] [ lnk-cf-github-repo] databasen.</span><span class="sxs-lookup"><span data-stu-id="398c8-127">A domain user can create an application using hello build.cmd script in hello [azure-iot-remote-monitoring][lnk-rm-github-repo],  [azure-iot-predictive-maintenance][lnk-pm-github-repo], or [azure-iot-connected-factory][lnk-cf-github-repo] repository.</span></span> <span data-ttu-id="398c8-128">Dock hello standardroll beviljas toohello domänanvändare **ReadOnly**, eftersom en domänanvändare inte har behörigheten tooassign roller.</span><span class="sxs-lookup"><span data-stu-id="398c8-128">However, hello default role granted toohello domain user is **ReadOnly**, because a domain user does not have permission tooassign roles.</span></span>
* <span data-ttu-id="398c8-129">Om en annan användare i hello AAD-klient skapar ett program, hello domänanvändare tilldelas hello **ReadOnly** roll som standard för programmet.</span><span class="sxs-lookup"><span data-stu-id="398c8-129">If another user in hello AAD tenant creates an application, hello domain user is assigned hello **ReadOnly** role by default for that application.</span></span>
* <span data-ttu-id="398c8-130">En domänanvändare kan inte tilldela roller för program. därför en domänanvändare kan inte lägga till användare eller roller för användare för ett program även om de har etablerats den.</span><span class="sxs-lookup"><span data-stu-id="398c8-130">A domain user cannot assign roles for applications; therefore a domain user cannot add users or roles for users for an application even if they provisioned it.</span></span>

### <a name="guest-user"></a><span data-ttu-id="398c8-131">Gästanvändare</span><span class="sxs-lookup"><span data-stu-id="398c8-131">Guest User</span></span>

<span data-ttu-id="398c8-132">Det kan finnas många gästanvändare per AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="398c8-132">There can be many guest users per AAD tenant.</span></span> <span data-ttu-id="398c8-133">Gästanvändare har en begränsad uppsättning rättigheter i hello AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="398c8-133">Guest users have a limited set of rights in hello AAD tenant.</span></span> <span data-ttu-id="398c8-134">Därför kan inte gästanvändare etablera en förkonfigurerade lösning i hello AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="398c8-134">As a result, guest users cannot provision a preconfigured solution in hello AAD tenant.</span></span>

<span data-ttu-id="398c8-135">Mer information om användare och roller i AAD finns hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="398c8-135">For more information about users and roles in AAD, see hello following resources:</span></span>

* <span data-ttu-id="398c8-136">[Skapa användare i Azure AD][lnk-create-edit-users]</span><span class="sxs-lookup"><span data-stu-id="398c8-136">[Create users in Azure AD][lnk-create-edit-users]</span></span>
* <span data-ttu-id="398c8-137">[Tilldela användare tooapps][lnk-assign-app-roles]</span><span class="sxs-lookup"><span data-stu-id="398c8-137">[Assign users tooapps][lnk-assign-app-roles]</span></span>

## <a name="azure-subscription-administrator-roles"></a><span data-ttu-id="398c8-138">Administratörsroller i Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="398c8-138">Azure subscription administrator roles</span></span>

<span data-ttu-id="398c8-139">hello Azure-administratörsroller styra hello möjlighet toomap en Azure-prenumeration tooan AD-klient.</span><span class="sxs-lookup"><span data-stu-id="398c8-139">hello Azure admin roles control hello ability toomap an Azure subscription tooan AD tenant.</span></span>

<span data-ttu-id="398c8-140">Lär dig mer om hello Azure administratörsroller i hello artikeln [hur tooadd eller ändra Medadministratör för Azure och tjänstadministratör kontoadministratör][lnk-admin-roles].</span><span class="sxs-lookup"><span data-stu-id="398c8-140">Find out more about hello Azure admin roles in hello article [How tooadd or change Azure Co-Administrator, Service Administrator, and Account Administrator][lnk-admin-roles].</span></span>

## <a name="application-roles"></a><span data-ttu-id="398c8-141">Programroller</span><span class="sxs-lookup"><span data-stu-id="398c8-141">Application roles</span></span>

<span data-ttu-id="398c8-142">Hej programroller styra åtkomst toodevices i din förkonfigurerade lösning.</span><span class="sxs-lookup"><span data-stu-id="398c8-142">hello application roles control access toodevices in your preconfigured solution.</span></span>

<span data-ttu-id="398c8-143">Det finns två definierade roller och en implicit roll som definierats i ett etablerat program:</span><span class="sxs-lookup"><span data-stu-id="398c8-143">There are two defined roles and one implicit role defined in a provisioned application:</span></span>

* <span data-ttu-id="398c8-144">**Admin:** har fullständig kontroll tooadd, hantera, ta bort enheter och ändra inställningarna.</span><span class="sxs-lookup"><span data-stu-id="398c8-144">**Admin:** Has full control tooadd, manage, remove devices, and modify settings.</span></span>
* <span data-ttu-id="398c8-145">**Skrivskyddad:** kan visa enheter, regler, åtgärder, jobb och telemetri.</span><span class="sxs-lookup"><span data-stu-id="398c8-145">**ReadOnly:** Can view devices, rules, actions, jobs, and telemetry.</span></span>

<span data-ttu-id="398c8-146">Du kan hitta hello behörigheter tooeach roll i hello [RolePermissions.cs] [ lnk-resource-cs] källfilen.</span><span class="sxs-lookup"><span data-stu-id="398c8-146">You can find hello permissions assigned tooeach role in hello [RolePermissions.cs][lnk-resource-cs] source file.</span></span>

### <a name="changing-application-roles-for-a-user"></a><span data-ttu-id="398c8-147">Ändra programroller för en användare</span><span class="sxs-lookup"><span data-stu-id="398c8-147">Changing application roles for a user</span></span>

<span data-ttu-id="398c8-148">Du kan använda följande procedur toomake en användare i din Active Directory-administratör för din förkonfigurerade lösning hello.</span><span class="sxs-lookup"><span data-stu-id="398c8-148">You can use hello following procedure toomake a user in your Active Directory an administrator of your preconfigured solution.</span></span>

<span data-ttu-id="398c8-149">Du måste vara en AAD global administratör toochange roller för en användare:</span><span class="sxs-lookup"><span data-stu-id="398c8-149">You must be an AAD global administrator toochange roles for a user:</span></span>

1. <span data-ttu-id="398c8-150">Gå toohello [Azure-portalen][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="398c8-150">Go toohello [Azure portal][lnk-portal].</span></span>
2. <span data-ttu-id="398c8-151">Välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="398c8-151">Select **Azure Active Directory**.</span></span>
3. <span data-ttu-id="398c8-152">Kontrollera att du använder hello-katalog som du valde på azureiotsuite.com när du har etablerat din lösning.</span><span class="sxs-lookup"><span data-stu-id="398c8-152">Make sure you are using hello directory you chose on azureiotsuite.com when you provisioned your solution.</span></span> <span data-ttu-id="398c8-153">Om du har flera kataloger som hör till din prenumeration kan växla du mellan dem om du klickar på namnet på ditt konto hello längst upp till höger i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="398c8-153">If you have multiple directories associated with your subscription, you can switch between them if you click your account name at hello top-right of hello portal.</span></span>
4. <span data-ttu-id="398c8-154">Klicka på **företagsprogram**, sedan **alla program**.</span><span class="sxs-lookup"><span data-stu-id="398c8-154">Click **Enterprise applications**, then **All applications**.</span></span>
4. <span data-ttu-id="398c8-155">Visa **alla program** med **alla** status.</span><span class="sxs-lookup"><span data-stu-id="398c8-155">Show **All applications** with **Any** status.</span></span> <span data-ttu-id="398c8-156">Söka efter ett program med namnet på din förkonfigurerade lösning.</span><span class="sxs-lookup"><span data-stu-id="398c8-156">Then search for an application with name of your preconfigured solution.</span></span>
5. <span data-ttu-id="398c8-157">Klicka på hello namnet på hello-program som matchar din förkonfigurerade lösningsnamn.</span><span class="sxs-lookup"><span data-stu-id="398c8-157">Click hello name of hello application that matches your preconfigured solution name.</span></span>
6. <span data-ttu-id="398c8-158">Klicka på **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="398c8-158">Click **Users and groups**.</span></span>
7. <span data-ttu-id="398c8-159">Välj hello-användare som du vill tooswitch roller.</span><span class="sxs-lookup"><span data-stu-id="398c8-159">Select hello user you want tooswitch roles.</span></span>
8. <span data-ttu-id="398c8-160">Klicka på **tilldela** och välj hello roll (som **Admin**) som tooassign toohello användaren klicka på kryssmarkeringen hello.</span><span class="sxs-lookup"><span data-stu-id="398c8-160">Click **Assign** and select hello role (such as **Admin**) you'd like tooassign toohello user, click hello check mark.</span></span>

## <a name="faq"></a><span data-ttu-id="398c8-161">VANLIGA FRÅGOR OCH SVAR</span><span class="sxs-lookup"><span data-stu-id="398c8-161">FAQ</span></span>

### <a name="im-a-service-administrator-and-id-like-toochange-hello-directory-mapping-between-my-subscription-and-a-specific-aad-tenant-how-do-i-complete-this-task"></a><span data-ttu-id="398c8-162">Jag är en tjänstadministratör och jag vill toochange hello directory mappning mellan min prenumeration och en specifik AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="398c8-162">I'm a service administrator and I'd like toochange hello directory mapping between my subscription and a specific AAD tenant.</span></span> <span data-ttu-id="398c8-163">Hur jag för att slutföra den här uppgiften?</span><span class="sxs-lookup"><span data-stu-id="398c8-163">How do I complete this task?</span></span>

1. <span data-ttu-id="398c8-164">Gå toohello [klassiska Azure-portalen][lnk-classic-portal], klickar du på **inställningar** i hello lista över tjänster hello vänster.</span><span class="sxs-lookup"><span data-stu-id="398c8-164">Go toohello [Azure classic portal][lnk-classic-portal], click **Settings** in hello list of services on hello left-hand side.</span></span>
2. <span data-ttu-id="398c8-165">Välj hello-prenumeration som du vill att toochange hello katalogmappning till.</span><span class="sxs-lookup"><span data-stu-id="398c8-165">Select hello subscription you'd like toochange hello directory mapping to.</span></span>
3. <span data-ttu-id="398c8-166">Klicka på **redigera Directory**.</span><span class="sxs-lookup"><span data-stu-id="398c8-166">Click **Edit Directory**.</span></span>
4. <span data-ttu-id="398c8-167">Välj hello **Directory** som toouse i hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="398c8-167">Select hello **Directory** you would like toouse in hello dropdown.</span></span> <span data-ttu-id="398c8-168">Klicka på pilen för hello framåt.</span><span class="sxs-lookup"><span data-stu-id="398c8-168">Click hello forward arrow.</span></span>
5. <span data-ttu-id="398c8-169">Bekräfta hello katalogmappning och påverkas medadministratörer.</span><span class="sxs-lookup"><span data-stu-id="398c8-169">Confirm hello directory mapping and affected co-administrators.</span></span> <span data-ttu-id="398c8-170">Om du flyttar från en annan katalog tas alla hello medadministratörer från hello ursprungliga katalogen bort.</span><span class="sxs-lookup"><span data-stu-id="398c8-170">If you are moving from another directory, all hello co-administrators from hello original directory are removed.</span></span>

### <a name="im-a-domain-usermember-on-hello-aad-tenant-and-ive-created-a-preconfigured-solution-how-do-i-get-assigned-a-role-for-my-application"></a><span data-ttu-id="398c8-171">Jag är användaren/medlem i en domän på hello AAD-klient och du har skapat en förkonfigurerade lösning.</span><span class="sxs-lookup"><span data-stu-id="398c8-171">I'm a domain user/member on hello AAD tenant and I've created a preconfigured solution.</span></span> <span data-ttu-id="398c8-172">Hur jag tilldelas en roll för Mina program?</span><span class="sxs-lookup"><span data-stu-id="398c8-172">How do I get assigned a role for my application?</span></span>

<span data-ttu-id="398c8-173">Be en global administratör toomake du en global administratör på hello AAD-klient och sedan tilldela roller toousers själv.</span><span class="sxs-lookup"><span data-stu-id="398c8-173">Ask a global administrator toomake you a global administrator on hello AAD tenant and then assign roles toousers yourself.</span></span> <span data-ttu-id="398c8-174">Du kan också be en global administratör tooassign du en roll direkt.</span><span class="sxs-lookup"><span data-stu-id="398c8-174">Alternatively, ask a global administrator tooassign you a role directly.</span></span> <span data-ttu-id="398c8-175">Om du vill att toochange hello AAD-klient din förkonfigurerade lösning har distribuerats till, finns i hello nästa fråga.</span><span class="sxs-lookup"><span data-stu-id="398c8-175">If you'd like toochange hello AAD tenant your preconfigured solution has been deployed to, see hello next question.</span></span>

### <a name="how-do-i-switch-hello-aad-tenant-my-remote-monitoring-preconfigured-solution-and-application-are-assigned-to"></a><span data-ttu-id="398c8-176">Hur växlar min remote förkonfigurerade övervakningslösning och programmet har tilldelats hello AAD-klient?</span><span class="sxs-lookup"><span data-stu-id="398c8-176">How do I switch hello AAD tenant my remote monitoring preconfigured solution and application are assigned to?</span></span>

<span data-ttu-id="398c8-177">Du kan köra en molndistribution från <https://github.com/Azure/azure-iot-remote-monitoring> och distribuera till en nyligen skapade AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="398c8-177">You can run a cloud deployment from <https://github.com/Azure/azure-iot-remote-monitoring> and redeploy with a newly created AAD tenant.</span></span> <span data-ttu-id="398c8-178">Eftersom du är som standard en global administratör när du skapar en AAD-klient har behörigheter tooadd användare och tilldela roller toothose användare.</span><span class="sxs-lookup"><span data-stu-id="398c8-178">Because you are, by default, a global administrator when you create an AAD tenant, you have permissions tooadd users and assign roles toothose users.</span></span>

1. <span data-ttu-id="398c8-179">Skapa ett AAD-katalogen i hello [Azure-portalen][lnk-portal].</span><span class="sxs-lookup"><span data-stu-id="398c8-179">Create an AAD directory in hello [Azure portal][lnk-portal].</span></span>
2. <span data-ttu-id="398c8-180">Gå för<https://github.com/Azure/azure-iot-remote-monitoring>.</span><span class="sxs-lookup"><span data-stu-id="398c8-180">Go too<https://github.com/Azure/azure-iot-remote-monitoring>.</span></span>
3. <span data-ttu-id="398c8-181">Kör `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (till exempel `build.cmd cloud debug myRMSolution`)</span><span class="sxs-lookup"><span data-stu-id="398c8-181">Run `build.cmd cloud [debug | release] {name of previously deployed remote monitoring solution}` (For example, `build.cmd cloud debug myRMSolution`)</span></span>
4. <span data-ttu-id="398c8-182">När du uppmanas att ange hello **tenantid** toobe nyligen skapade klienten i stället för din tidigare klient.</span><span class="sxs-lookup"><span data-stu-id="398c8-182">When prompted, set hello **tenantid** toobe your newly created tenant instead of your previous tenant.</span></span>

### <a name="i-want-toochange-a-service-administrator-or-co-administrator-when-logged-in-with-an-organisational-account"></a><span data-ttu-id="398c8-183">Jag vill toochange en tjänstadministratör eller en Medadministratör när du har loggat in med ett organisatoriska konto</span><span class="sxs-lookup"><span data-stu-id="398c8-183">I want toochange a Service Administrator or Co-Administrator when logged in with an organisational account</span></span>

<span data-ttu-id="398c8-184">Se hello support-artikeln [ändra tjänstadministratören och Medadministratör när du har loggat in med ett konto som organisatoriska][lnk-service-admins].</span><span class="sxs-lookup"><span data-stu-id="398c8-184">See hello support article [Changing Service Administrator and Co-Administrator when logged in with an organisational account][lnk-service-admins].</span></span>

### <a name="why-am-i-seeing-this-error-your-account-does-not-have-hello-proper-permissions-toocreate-a-solution-please-check-with-your-account-administrator-or-try-with-a-different-account"></a><span data-ttu-id="398c8-185">Varför ser jag det här felet?</span><span class="sxs-lookup"><span data-stu-id="398c8-185">Why am I seeing this error?</span></span> <span data-ttu-id="398c8-186">”Ditt konto har inte hello åtkomstbehörighet toocreate en lösning.</span><span class="sxs-lookup"><span data-stu-id="398c8-186">"Your account does not have hello proper permissions toocreate a solution.</span></span> <span data-ttu-id="398c8-187">Kontrollera med din kontoadministratör eller försök med ett annat konto ”.</span><span class="sxs-lookup"><span data-stu-id="398c8-187">Please check with your account administrator or try with a different account."</span></span>

<span data-ttu-id="398c8-188">Titta på hello följande diagram anvisningar:</span><span class="sxs-lookup"><span data-stu-id="398c8-188">Look at hello following diagram for guidance:</span></span>

![][img-flowchart]

> [!NOTE]
> <span data-ttu-id="398c8-189">Om du fortsätter toosee hello fel efter verifiering är en global administratör på hello AAD-klient och en medadministratör för prenumerationen hello måste ha din kontoadministratör om du tar bort hello användare och tilldela behörigheter som krävs i den här ordningen.</span><span class="sxs-lookup"><span data-stu-id="398c8-189">If you continue toosee hello error after validating you are a global administrator on hello AAD tenant and a co-administrator on hello subscription, have your account administrator remove hello user and reassign necessary permissions in this order.</span></span> <span data-ttu-id="398c8-190">Först lägger till hello användare som en global administratör och sedan lägga till användare som medadministratör på hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="398c8-190">First, add hello user as a global administrator and then add user as a co-administrator on hello Azure subscription.</span></span> <span data-ttu-id="398c8-191">Om problem kvarstår kontaktar du [hjälp och Support][lnk-help-support].</span><span class="sxs-lookup"><span data-stu-id="398c8-191">If issues persist, contact [Help & Support][lnk-help-support].</span></span>

### <a name="why-am-i-seeing-this-error-when-i-have-an-azure-subscription-an-azure-subscription-is-required-toocreate-pre-configured-solutions-you-can-create-a-free-trial-account-in-just-a-couple-of-minutes"></a><span data-ttu-id="398c8-192">Varför ser jag det här felet när jag har en Azure-prenumeration?</span><span class="sxs-lookup"><span data-stu-id="398c8-192">Why am I seeing this error when I have an Azure subscription?</span></span> <span data-ttu-id="398c8-193">”En Azure-prenumeration är obligatoriska toocreate förkonfigurerade lösningar.</span><span class="sxs-lookup"><span data-stu-id="398c8-193">"An Azure subscription is required toocreate pre-configured solutions.</span></span> <span data-ttu-id="398c8-194">Du kan skapa ett kostnadsfritt utvärderingskonto på bara några minuter ”.</span><span class="sxs-lookup"><span data-stu-id="398c8-194">You can create a free trial account in just a couple of minutes."</span></span>

<span data-ttu-id="398c8-195">Om du är säker på att du har en Azure-prenumeration Validera hello klient mappning för din prenumeration och kontrollera hello rätt klient är markerad i listrutan hello.</span><span class="sxs-lookup"><span data-stu-id="398c8-195">If you're certain you have an Azure subscription, validate hello tenant mapping for your subscription and ensure hello correct tenant is selected in hello dropdown.</span></span> <span data-ttu-id="398c8-196">Om du har verifierats hello önskat klient är korrekt, följ hello föregående diagram och verifiera hello mappningen av din prenumeration och AAD-klient.</span><span class="sxs-lookup"><span data-stu-id="398c8-196">If you’ve validated hello desired tenant is correct, follow hello preceding diagram and validate hello mapping of your subscription and this AAD tenant.</span></span>

## <a name="next-steps"></a><span data-ttu-id="398c8-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="398c8-197">Next steps</span></span>
<span data-ttu-id="398c8-198">Lär dig mer om IoT Suite toocontinue se hur du kan [anpassa förkonfigurerade lösning][lnk-customize].</span><span class="sxs-lookup"><span data-stu-id="398c8-198">toocontinue learning about IoT Suite, see how you can [customize a preconfigured solution][lnk-customize].</span></span>

[img-flowchart]: media/iot-suite-permissions/flowchart.png

[lnk-azureiotsuite]: https://www.azureiotsuite.com/
[lnk-rm-github-repo]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-pm-github-repo]: https://github.com/Azure/azure-iot-predictive-maintenance
[lnk-cf-github-repo]: https://github.com/Azure/azure-iot-connected-factory
[lnk-aad-admin]: ../active-directory/active-directory-assign-admin-roles.md
[lnk-classic-portal]: https://manage.windowsazure.com/
[lnk-portal]: https://portal.azure.com/
[lnk-create-edit-users]: ../active-directory/active-directory-create-users.md
[lnk-assign-app-roles]: ../active-directory/active-directory-coreapps-assign-user-azure-portal.md
[lnk-service-admins]: https://azure.microsoft.com/support/changing-service-admin-and-co-admin/
[lnk-admin-roles]: ../billing/billing-add-change-azure-subscription-administrator.md
[lnk-resource-cs]: https://github.com/Azure/azure-iot-remote-monitoring/blob/master/DeviceAdministration/Web/Security/RolePermissions.cs
[lnk-help-support]: https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
