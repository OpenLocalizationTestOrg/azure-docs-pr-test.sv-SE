---
title: "aaaDeploy Azure Access panelen-tillägg för Internet Explorer med hjälp av ett grupprincipobjekt | Microsoft Docs"
description: "Hur toouse gruppera princip toodeploy hello Internet Explorer-tillägget för hello Mina appar portal."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 7c2d49c8-5be0-4e7e-abac-332f9dfda736
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 926f15950bbe81d2fbfe1b0b856470da40880d7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-hello-access-panel-extension-for-internet-explorer-using-group-policy"></a><span data-ttu-id="665db-103">Hur tooDeploy hello Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip</span><span class="sxs-lookup"><span data-stu-id="665db-103">How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy</span></span>
<span data-ttu-id="665db-104">Den här kursen visar hur toouse grupp princip tooremotely installera hello åtkomstpanelen tillägg för Internet Explorer på användarnas datorer.</span><span class="sxs-lookup"><span data-stu-id="665db-104">This tutorial shows how toouse group policy tooremotely install hello Access Panel extension for Internet Explorer on your users' machines.</span></span> <span data-ttu-id="665db-105">Det här tillägget krävs för Internet Explorer-användare som behöver toosign i appar som har konfigurerats med hjälp av [lösenordsbaserade enkel inloggning](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="665db-105">This extension is required for Internet Explorer users who need toosign into apps that are configured using [password-based single sign-on](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).</span></span>

<span data-ttu-id="665db-106">Vi rekommenderar att administratörer automatisera hello distribution av det här tillägget.</span><span class="sxs-lookup"><span data-stu-id="665db-106">It is recommended that admins automate hello deployment of this extension.</span></span> <span data-ttu-id="665db-107">Annars användare har toodownload och installera tillägg för hello själva, som är lätt toouser fel och måste ha administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="665db-107">Otherwise, users have toodownload and install hello extension themselves, which is prone toouser error and requires administrator permissions.</span></span> <span data-ttu-id="665db-108">Den här självstudiekursen beskriver en metod för att automatisera programvarudistributioner med hjälp av Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="665db-108">This tutorial covers one method of automating software deployments by using group policy.</span></span> [<span data-ttu-id="665db-109">Läs mer om grupprinciper.</span><span class="sxs-lookup"><span data-stu-id="665db-109">Learn more about group policy.</span></span>](https://technet.microsoft.com/windowsserver/bb310732.aspx)

<span data-ttu-id="665db-110">Hej åtkomstpanelen tillägg är också tillgängligt för [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) och [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), som inte kräver att administratören behörigheter tooinstall.</span><span class="sxs-lookup"><span data-stu-id="665db-110">hello Access Panel extension is also available for [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) and [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), neither of which require administrator permissions tooinstall.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="665db-111">Krav</span><span class="sxs-lookup"><span data-stu-id="665db-111">Prerequisites</span></span>
* <span data-ttu-id="665db-112">Du har ställt in [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), och du har anslutit användarnas datorer tooyour domän.</span><span class="sxs-lookup"><span data-stu-id="665db-112">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines tooyour domain.</span></span>
* <span data-ttu-id="665db-113">Du måste ha hello ”redigera inställningar för” behörighet tooedit hello grupprincipobjektet (GPO).</span><span class="sxs-lookup"><span data-stu-id="665db-113">You must have hello "Edit settings" permission tooedit hello Group Policy Object (GPO).</span></span> <span data-ttu-id="665db-114">Som standard medlemmar i hello följande säkerhetsgrupper har denna behörighet: Domänadministratörer, Företagsadministratörer och skapare och ägare av Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="665db-114">By default, members of hello following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> [<span data-ttu-id="665db-115">Läs mer.</span><span class="sxs-lookup"><span data-stu-id="665db-115">Learn more.</span></span>](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-hello-distribution-point"></a><span data-ttu-id="665db-116">Steg 1: Skapa hello distributionsplats</span><span class="sxs-lookup"><span data-stu-id="665db-116">Step 1: Create hello Distribution Point</span></span>
<span data-ttu-id="665db-117">Först måste du placera hello installer-paketet på en nätverksplats som kan nås av hello datorer som du vill tooremotely installera hello tillägg på.</span><span class="sxs-lookup"><span data-stu-id="665db-117">First, you must place hello installer package on a network location that can be accessed by hello machines that you wish tooremotely install hello extension on.</span></span> <span data-ttu-id="665db-118">toodo, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="665db-118">toodo this, follow these steps:</span></span>

1. <span data-ttu-id="665db-119">Logga in på toohello server som administratör</span><span class="sxs-lookup"><span data-stu-id="665db-119">Log on toohello server as an administrator</span></span>
2. <span data-ttu-id="665db-120">I hello **Serverhanteraren** fönstret gå för**filer och lagringstjänster**.</span><span class="sxs-lookup"><span data-stu-id="665db-120">In hello **Server Manager** window, go too**Files and Storage Services**.</span></span>
   
    ![Öppna filer och lagringstjänster](./media/active-directory-saas-ie-group-policy/files-services.png)
3. <span data-ttu-id="665db-122">Gå toohello **resurser** fliken. Klicka på **uppgifter** > **ny resurs...**</span><span class="sxs-lookup"><span data-stu-id="665db-122">Go toohello **Shares** tab. Then click **Tasks** > **New Share...**</span></span>
   
    ![Öppna filer och lagringstjänster](./media/active-directory-saas-ie-group-policy/shares.png)
4. <span data-ttu-id="665db-124">Fullständig hello **ny resurs Guide** och ange behörigheter tooensure kan nås från användarnas datorer.</span><span class="sxs-lookup"><span data-stu-id="665db-124">Complete hello **New Share Wizard** and set permissions tooensure that it can be accessed from your users' machines.</span></span> [<span data-ttu-id="665db-125">Mer information om resurser.</span><span class="sxs-lookup"><span data-stu-id="665db-125">Learn more about shares.</span></span>](https://technet.microsoft.com/library/cc753175.aspx)
5. <span data-ttu-id="665db-126">Hämta hello följande Microsoft Windows Installer-paketet (MSI-fil): [åtkomst panelen Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)</span><span class="sxs-lookup"><span data-stu-id="665db-126">Download hello following Microsoft Windows Installer package (.msi file): [Access Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)</span></span>
6. <span data-ttu-id="665db-127">Kopiera hello installer tooa önskad plats för hello-resursen.</span><span class="sxs-lookup"><span data-stu-id="665db-127">Copy hello installer package tooa desired location on hello share.</span></span>
   
    ![Kopiera hello .msi toohello filresurs.](./media/active-directory-saas-ie-group-policy/copy-package.png)
7. <span data-ttu-id="665db-129">Kontrollera att dina klientdatorer kan tooaccess hello installationspaketet från hello resurs.</span><span class="sxs-lookup"><span data-stu-id="665db-129">Verify that your client machines are able tooaccess hello installer package from hello share.</span></span> 

## <a name="step-2-create-hello-group-policy-object"></a><span data-ttu-id="665db-130">Steg 2: Skapa hello grupprincipobjekt</span><span class="sxs-lookup"><span data-stu-id="665db-130">Step 2: Create hello Group Policy Object</span></span>
1. <span data-ttu-id="665db-131">Logga in toohello-server som är värd för Active Directory Domain Services (AD DS)-installationen.</span><span class="sxs-lookup"><span data-stu-id="665db-131">Log on toohello server that hosts your Active Directory Domain Services (AD DS) installation.</span></span>
2. <span data-ttu-id="665db-132">Hello Serverhanteraren, gå för**verktyg** > **Grupprinciphantering**.</span><span class="sxs-lookup"><span data-stu-id="665db-132">In hello Server Manager, go too**Tools** > **Group Policy Management**.</span></span>
   
    ![Gå tooTools > Group Policy Management](./media/active-directory-saas-ie-group-policy/tools-gpm.png)
3. <span data-ttu-id="665db-134">Hello vänster i hello **Grupprinciphantering** fönstret Visa organisationsenheten (OU) hierarkin och avgöra vilka definitionsområdet som tooapply hello Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="665db-134">In hello left pane of hello **Group Policy Management** window, view your Organizational Unit (OU) hierarchy and determine at which scope you would like tooapply hello group policy.</span></span> <span data-ttu-id="665db-135">Exempelvis kan du välja toopick en liten OU toodeploy tooa några användare för att testa eller du kan välja en översta OU toodeploy tooyour hela organisationen.</span><span class="sxs-lookup"><span data-stu-id="665db-135">For instance, you may decide toopick a small OU toodeploy tooa few users for testing, or you may pick a top-level OU toodeploy tooyour entire organization.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="665db-136">Om du skulle t.ex. toocreate eller redigera dina organisationsenheter (OU), växlar tillbaka toohello Serverhanteraren och gå för**verktyg** > **Active Directory-användare och datorer**.</span><span class="sxs-lookup"><span data-stu-id="665db-136">If you would like toocreate or edit your Organization Units (OUs), switch back toohello Server Manager and go too**Tools** > **Active Directory Users and Computers**.</span></span>
   > 
   > 
4. <span data-ttu-id="665db-137">När du har valt en Organisationsenhet, högerklicka och välj **skapa ett grupprincipobjekt i den här domänen och länka det här...**</span><span class="sxs-lookup"><span data-stu-id="665db-137">Once you have selected an OU, right-click it and select **Create a GPO in this domain, and Link it here...**</span></span>
   
    ![Skapa ett nytt GPO](./media/active-directory-saas-ie-group-policy/create-gpo.png)
5. <span data-ttu-id="665db-139">I hello **nytt GPO** snabb, Skriv ett namn för hello nytt grupprincipobjekt.</span><span class="sxs-lookup"><span data-stu-id="665db-139">In hello **New GPO** prompt, type in a name for hello new Group Policy Object.</span></span>
   
    ![Namnet hello nytt GPO](./media/active-directory-saas-ie-group-policy/name-gpo.png)
6. <span data-ttu-id="665db-141">Högerklicka på hello grupprincipobjekt som du skapade och välj **redigera**.</span><span class="sxs-lookup"><span data-stu-id="665db-141">Right-click hello Group Policy Object that you created, and select **Edit**.</span></span>
   
    ![Redigera hello nytt GPO](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## <a name="step-3-assign-hello-installation-package"></a><span data-ttu-id="665db-143">Steg 3: Tilldela hello installationspaketet</span><span class="sxs-lookup"><span data-stu-id="665db-143">Step 3: Assign hello Installation Package</span></span>
1. <span data-ttu-id="665db-144">Avgöra om du vill att toodeploy hello tillägget baserat på **Datorkonfiguration** eller **Användarkonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="665db-144">Determine whether you would like toodeploy hello extension based on **Computer Configuration** or **User Configuration**.</span></span> <span data-ttu-id="665db-145">När du använder [Datorkonfiguration](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), hello tillägg har installerats på hello datorn oavsett vilka användare som loggar in tooit.</span><span class="sxs-lookup"><span data-stu-id="665db-145">When using [computer configuration](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), hello extension is installed on hello computer regardless of which users log on tooit.</span></span> <span data-ttu-id="665db-146">Med [Användarkonfiguration](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), användare ha hello-tillägg som installerats för dem oavsett vilka datorer de loggar in på.</span><span class="sxs-lookup"><span data-stu-id="665db-146">With [user configuration](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), users have hello extension installed for them regardless of which computers they log on to.</span></span>
2. <span data-ttu-id="665db-147">Hello vänster i hello **Redigeraren för Grupprinciphantering** fönstret gå tooeither av hello mappsökvägar, beroende på vilken typ av du har valt följande:</span><span class="sxs-lookup"><span data-stu-id="665db-147">In hello left pane of hello **Group Policy Management Editor** window, go tooeither of hello following folder paths, depending on which type of configuration you chose:</span></span>
   
   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`
3. <span data-ttu-id="665db-148">Högerklicka på **Programvaruinstallation**och välj **ny** > **paket...**</span><span class="sxs-lookup"><span data-stu-id="665db-148">Right-click **Software installation**, then select **New** > **Package...**</span></span>
   
    ![Skapa en ny installationspaket för programvara](./media/active-directory-saas-ie-group-policy/new-package.png)
4. <span data-ttu-id="665db-150">Gå toohello delad mapp som innehåller hello installationspaketet från [steg 1: skapa hello distributionsplats](#step-1-create-the-distribution-point), Välj hello MSI-filen och klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="665db-150">Go toohello shared folder that contains hello installer package from [Step 1: Create hello Distribution Point](#step-1-create-the-distribution-point), select hello .msi file, and click **Open**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="665db-151">Om hello resursen finns på den här samma server, kontrollerar du att du använder hello .msi via hello nätverkssökvägen fil i stället för hello lokal filsökväg.</span><span class="sxs-lookup"><span data-stu-id="665db-151">If hello share is located on this same server, verify that you are accessing hello .msi through hello network file path, rather than hello local file path.</span></span>
   > 
   > 
   
    ![Välj hello installationspaketet från hello delad mapp.](./media/active-directory-saas-ie-group-policy/select-package.png)
5. <span data-ttu-id="665db-153">I hello **distribuera programvara** fråga, Välj **tilldelad** för din distributionsmetod.</span><span class="sxs-lookup"><span data-stu-id="665db-153">In hello **Deploy Software** prompt, select **Assigned** for your deployment method.</span></span> <span data-ttu-id="665db-154">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="665db-154">Then click **OK**.</span></span>
   
    ![Välj tilldelad och klicka sedan på OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

<span data-ttu-id="665db-156">hello tillägget är nu distribuerade toohello Organisationsenhet som du har valt.</span><span class="sxs-lookup"><span data-stu-id="665db-156">hello extension is now deployed toohello OU that you selected.</span></span> [<span data-ttu-id="665db-157">Läs mer om Programvaruinstallation för Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="665db-157">Learn more about Group Policy Software Installation.</span></span>](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-hello-extension-for-internet-explorer"></a><span data-ttu-id="665db-158">Steg 4: Aktivera automatiskt hello tillägg för Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="665db-158">Step 4: Auto-Enable hello Extension for Internet Explorer</span></span>
<span data-ttu-id="665db-159">Dessutom toorunning hello installationsprogrammet för alla tillägg för Internet Explorer måste uttryckligen aktiveras innan den kan användas.</span><span class="sxs-lookup"><span data-stu-id="665db-159">In addition toorunning hello installer, every extension for Internet Explorer must be explicitly enabled before it can be used.</span></span> <span data-ttu-id="665db-160">Följ hello stegen nedan tooenable hello åtkomst panelen tillägget med hjälp av Grupprincip:</span><span class="sxs-lookup"><span data-stu-id="665db-160">Follow hello steps below tooenable hello Access Panel Extension using group policy:</span></span>

1. <span data-ttu-id="665db-161">I hello **Redigeraren för Grupprinciphantering** gå tooeither av hello följande sökvägar, beroende på vilken typ av du valde i fönstret [steg3: tilldela hello installationspaketet](#step-3-assign-the-installation-package):</span><span class="sxs-lookup"><span data-stu-id="665db-161">In hello **Group Policy Management Editor** window, go tooeither of hello following paths, depending on which type of configuration you chose in [Step 3: Assign hello Installation Package](#step-3-assign-the-installation-package):</span></span>
   
   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
2. <span data-ttu-id="665db-162">Högerklicka på **lista över tillägg**, och välj **redigera**.</span><span class="sxs-lookup"><span data-stu-id="665db-162">Right-click **Add-on List**, and select **Edit**.</span></span>
    <span data-ttu-id="665db-163">![Redigera lista över tillägg.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)</span><span class="sxs-lookup"><span data-stu-id="665db-163">![Edit Add-on List.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)</span></span>
3. <span data-ttu-id="665db-164">I hello **lista över tillägg** väljer **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="665db-164">In hello **Add-on List** window, select **Enabled**.</span></span> <span data-ttu-id="665db-165">Sedan, under hello **alternativ** klickar du på **visa...** .</span><span class="sxs-lookup"><span data-stu-id="665db-165">Then, under hello **Options** section, click **Show...**.</span></span>
   
    ![Klicka på Aktivera och klicka sedan på Visa...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)
4. <span data-ttu-id="665db-167">I hello **Visa innehåll** fönstret utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="665db-167">In hello **Show Contents** window, perform hello following steps:</span></span>
   
   1. <span data-ttu-id="665db-168">För hello första kolumnen (hello **värdenamn** fältet), kopiera och klistra in hello efter klass-ID:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`</span><span class="sxs-lookup"><span data-stu-id="665db-168">For hello first column (hello **Value Name** field), copy and paste hello following Class ID: `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`</span></span>
   2. <span data-ttu-id="665db-169">För andra kolumnen i hello (hello **värdet** fältet), ange hello följande värde:`1`</span><span class="sxs-lookup"><span data-stu-id="665db-169">For hello second column (hello **Value** field), type in hello following value: `1`</span></span>
   3. <span data-ttu-id="665db-170">Klicka på **OK** tooclose hello **Visa innehåll** fönster.</span><span class="sxs-lookup"><span data-stu-id="665db-170">Click **OK** tooclose hello **Show Contents** window.</span></span>
      
      ![Fyll i hello värden som anges ovan.](./media/active-directory-saas-ie-group-policy/show-contents.png)
5. <span data-ttu-id="665db-172">Klicka på **OK** tooapply dina ändringar och Stäng hello **lista över tillägg** fönster.</span><span class="sxs-lookup"><span data-stu-id="665db-172">Click **OK** tooapply your changes and close hello **Add-on List** window.</span></span>

<span data-ttu-id="665db-173">hello tillägget bör nu vara aktiverat för hello datorer i hello valda Organisationsenheten.</span><span class="sxs-lookup"><span data-stu-id="665db-173">hello extension should now be enabled for hello machines in hello selected OU.</span></span> [<span data-ttu-id="665db-174">Lär dig mer om hur du använder gruppen princip tooenable eller inaktivera tillägg för Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="665db-174">Learn more about using group policy tooenable or disable Internet Explorer add-ons.</span></span>](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a><span data-ttu-id="665db-175">Steg 5 (valfritt): inaktivera ”Kom ihåg lösenord” fråga</span><span class="sxs-lookup"><span data-stu-id="665db-175">Step 5 (Optional): Disable "Remember Password" Prompt</span></span>
<span data-ttu-id="665db-176">När användare loggar in toowebsites med hello tillägget för åtkomst-panelen, Internet Explorer kan fråga visa hello följande frågar ”vill du som toostore ditt lösenord”?</span><span class="sxs-lookup"><span data-stu-id="665db-176">When users sign-in toowebsites using hello Access Panel Extension, Internet Explorer may show hello following prompt asking "Would you like toostore your password?"</span></span>

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

<span data-ttu-id="665db-177">Om du vill tooprevent användarna ser den här uppmaningen sedan gör hello nedan tooprevent automatisk komplettering från komma ihåg lösenord:</span><span class="sxs-lookup"><span data-stu-id="665db-177">If you wish tooprevent your users from seeing this prompt, then follow hello steps below tooprevent auto-complete from remembering passwords:</span></span>

1. <span data-ttu-id="665db-178">I hello **Redigeraren för Grupprinciphantering** fönster, gå toohello sökväg som anges nedan.</span><span class="sxs-lookup"><span data-stu-id="665db-178">In hello **Group Policy Management Editor** window, go toohello path listed below.</span></span> <span data-ttu-id="665db-179">Den här inställningen är endast tillgängliga under **Användarkonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="665db-179">This configuration setting is only available under **User Configuration**.</span></span>
   
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
2. <span data-ttu-id="665db-180">Hitta hello inställning med namnet **aktivera hello automatisk komplettering funktionen för användarnamn och lösenord för formulär för**.</span><span class="sxs-lookup"><span data-stu-id="665db-180">Find hello setting named **Turn on hello auto-complete feature for user names and passwords on forms**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="665db-181">Tidigare versioner av Active Directory kan visa en lista med den här inställningen med hello namnet **tillåter inte automatisk komplettering toosave lösenord**.</span><span class="sxs-lookup"><span data-stu-id="665db-181">Previous versions of Active Directory may list this setting with hello name **Do not allow auto-complete toosave passwords**.</span></span> <span data-ttu-id="665db-182">hello-konfigurationen för den här inställningen skiljer sig från hello inställningen beskrivs i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="665db-182">hello configuration for that setting differs from hello setting described in this tutorial.</span></span>
   > 
   > 
   
    ![Kom ihåg toolook för detta under inställningar.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)
3. <span data-ttu-id="665db-184">Högerklicka på hello ovan inställningen och välj **redigera**.</span><span class="sxs-lookup"><span data-stu-id="665db-184">Right click hello above setting, and select **Edit**.</span></span>
4. <span data-ttu-id="665db-185">I fönstret hello **aktivera hello automatisk komplettering funktionen för användarnamn och lösenord för formulär för**väljer **inaktiverade**.</span><span class="sxs-lookup"><span data-stu-id="665db-185">In hello window titled **Turn on hello auto-complete feature for user names and passwords on forms**, select **Disabled**.</span></span>
   
    ![Välj inaktivera](./media/active-directory-saas-ie-group-policy/disable-passwords.png)
5. <span data-ttu-id="665db-187">Klicka på **OK** tooapply dessa ändringar och Stäng hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="665db-187">Click **OK** tooapply these changes and close hello window.</span></span>

<span data-ttu-id="665db-188">Användare kommer inte längre kan toostore sina autentiseringsuppgifter eller använda automatisk komplettering tooaccess tidigare lagrade autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="665db-188">Users will no longer be able toostore their credentials or use auto-complete tooaccess previously stored credentials.</span></span> <span data-ttu-id="665db-189">Den här principen tillåter dock att användare toocontinue toouse automatisk komplettering för andra typer av fält, till exempel sökfält.</span><span class="sxs-lookup"><span data-stu-id="665db-189">However, this policy does allow users toocontinue toouse auto-complete for other types of form fields, such as search fields.</span></span>

> [!WARNING]
> <span data-ttu-id="665db-190">Om den här principen är aktiverad när användare har valt toostore vissa autentiseringsuppgifter kan den här principen ska *inte* ta bort hello-autentiseringsuppgifter som redan har lagrats.</span><span class="sxs-lookup"><span data-stu-id="665db-190">If this policy is enabled after users have chosen toostore some credentials, this policy will *not* clear hello credentials that have already been stored.</span></span>
> 
> 

## <a name="step-6-testing-hello-deployment"></a><span data-ttu-id="665db-191">Steg 6: Testa hello distribution</span><span class="sxs-lookup"><span data-stu-id="665db-191">Step 6: Testing hello Deployment</span></span>
<span data-ttu-id="665db-192">Gör hello nedan tooverify om hello tillägget distributionen lyckades:</span><span class="sxs-lookup"><span data-stu-id="665db-192">Follow hello steps below tooverify if hello extension deployment was successful:</span></span>

1. <span data-ttu-id="665db-193">Om du har distribuerat med **Datorkonfiguration**, logga in på en klientdator som tillhör toohello Organisationsenhet som du valde i [steg 2: skapa hello grupprincipobjekt](#step-2-create-the-group-policy-object).</span><span class="sxs-lookup"><span data-stu-id="665db-193">If you deployed using **Computer Configuration**, sign into a client machine that belongs toohello OU that you selected in [Step 2: Create hello Group Policy Object](#step-2-create-the-group-policy-object).</span></span> <span data-ttu-id="665db-194">Om du har distribuerat med **Användarkonfiguration**, se till att toosign i som en användare som tillhör toothat Organisationsenhet.</span><span class="sxs-lookup"><span data-stu-id="665db-194">If you deployed using **User Configuration**, make sure toosign in as a user who belongs toothat OU.</span></span>
2. <span data-ttu-id="665db-195">Det kan ta ett par logga moduler för gruppolicy hello ändras toofully uppdateringen till den här datorn.</span><span class="sxs-lookup"><span data-stu-id="665db-195">It may take a couple sign ins for hello group policy changes toofully update with this machine.</span></span> <span data-ttu-id="665db-196">tooforce hello uppdatering, öppna en **kommandotolk** fönster och kör hello följande kommando:`gpupdate /force`</span><span class="sxs-lookup"><span data-stu-id="665db-196">tooforce hello update, open a **Command Prompt** window and run hello following command: `gpupdate /force`</span></span>
3. <span data-ttu-id="665db-197">Du måste starta om datorn hello hello installation tootake plats.</span><span class="sxs-lookup"><span data-stu-id="665db-197">You must restart hello machine for hello installation tootake place.</span></span> <span data-ttu-id="665db-198">Autostart kan ta betydligt längre tid än vanligt när hello tillägget installeras.</span><span class="sxs-lookup"><span data-stu-id="665db-198">Bootup may take significantly more time than usual while hello extension installs.</span></span>
4. <span data-ttu-id="665db-199">Efter omstart, öppna **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="665db-199">After restarting, open **Internet Explorer**.</span></span> <span data-ttu-id="665db-200">Klicka på hello övre högra hörnet av hello **verktyg** (hello växeln ikonen) och välj sedan **Hantera tillägg**.</span><span class="sxs-lookup"><span data-stu-id="665db-200">On hello upper-right corner of hello window, click **Tools** (hello gear icon), and then select **Manage add-ons**.</span></span>
   
    ![Gå tooTools > Hantera tillägg](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)
5. <span data-ttu-id="665db-202">I hello **Hantera tillägg** fönster, kontrollera att hello **åtkomst panelen tillägget** har installerats och att dess **Status** har ställts in för**aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="665db-202">In hello **Manage Add-ons** window, verify that hello **Access Panel Extension** has been installed and that its **Status** has been set too**Enabled**.</span></span>
   
    ![Kontrollera att hello åtkomst panelen tillägget är installerat och aktiverat.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a><span data-ttu-id="665db-204">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="665db-204">Related Articles</span></span>
* [<span data-ttu-id="665db-205">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="665db-205">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="665db-206">Programåtkomst och enkel inloggning med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="665db-206">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="665db-207">Felsöka hello Access panelen-tillägg för Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="665db-207">Troubleshooting hello Access Panel Extension for Internet Explorer</span></span>](active-directory-saas-ie-troubleshooting.md)

