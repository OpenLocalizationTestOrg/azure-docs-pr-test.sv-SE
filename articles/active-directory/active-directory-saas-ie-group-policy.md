---
title: "Distribuera Azure Access panelen-tillägg för Internet Explorer med hjälp av ett grupprincipobjekt | Microsoft Docs"
description: "Hur du använder grupprinciper för att distribuera Internet Explorer-tillägget för Mina appar portalen."
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
ms.openlocfilehash: b402ae326ab34ec71ad9de966e22be00045fee3e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a><span data-ttu-id="b7850-103">Hur du distribuerar Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip</span><span class="sxs-lookup"><span data-stu-id="b7850-103">How to Deploy the Access Panel Extension for Internet Explorer using Group Policy</span></span>
<span data-ttu-id="b7850-104">Den här kursen visar hur du använder grupprinciper för att fjärrinstallera åtkomstpanelen-tillägg för Internet Explorer på användarnas datorer.</span><span class="sxs-lookup"><span data-stu-id="b7850-104">This tutorial shows how to use group policy to remotely install the Access Panel extension for Internet Explorer on your users' machines.</span></span> <span data-ttu-id="b7850-105">Det här tillägget krävs för Internet Explorer-användare som behöver logga in på appar som är konfigurerade med [lösenordsbaserade enkel inloggning](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="b7850-105">This extension is required for Internet Explorer users who need to sign into apps that are configured using [password-based single sign-on](active-directory-appssoaccess-whatis.md#password-based-single-sign-on).</span></span>

<span data-ttu-id="b7850-106">Vi rekommenderar att administratörer automatisera distributionen av det här tillägget.</span><span class="sxs-lookup"><span data-stu-id="b7850-106">It is recommended that admins automate the deployment of this extension.</span></span> <span data-ttu-id="b7850-107">I annat fall behöver användarna ladda ned och installera tillägget själva, som är lätt fel och måste ha administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="b7850-107">Otherwise, users have to download and install the extension themselves, which is prone to user error and requires administrator permissions.</span></span> <span data-ttu-id="b7850-108">Den här självstudiekursen beskriver en metod för att automatisera programvarudistributioner med hjälp av Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="b7850-108">This tutorial covers one method of automating software deployments by using group policy.</span></span> [<span data-ttu-id="b7850-109">Läs mer om grupprinciper.</span><span class="sxs-lookup"><span data-stu-id="b7850-109">Learn more about group policy.</span></span>](https://technet.microsoft.com/windowsserver/bb310732.aspx)

<span data-ttu-id="b7850-110">Tillägget åtkomstpanelen är också tillgängligt för [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) och [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), som inte kräver administratörsbehörighet för att installera.</span><span class="sxs-lookup"><span data-stu-id="b7850-110">The Access Panel extension is also available for [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) and [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), neither of which require administrator permissions to install.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b7850-111">Krav</span><span class="sxs-lookup"><span data-stu-id="b7850-111">Prerequisites</span></span>
* <span data-ttu-id="b7850-112">Du har ställt in [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), och du har anslutit användarnas datorer till domänen.</span><span class="sxs-lookup"><span data-stu-id="b7850-112">You have set up [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), and you have joined your users' machines to your domain.</span></span>
* <span data-ttu-id="b7850-113">Du måste ha behörigheten ”Redigera inställningar” så här redigerar du den grupprincipobjektet (GPO).</span><span class="sxs-lookup"><span data-stu-id="b7850-113">You must have the "Edit settings" permission to edit the Group Policy Object (GPO).</span></span> <span data-ttu-id="b7850-114">Som standard medlemmar i följande säkerhetsgrupper har denna behörighet: Domänadministratörer, Företagsadministratörer och skapare och ägare av Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="b7850-114">By default, members of the following security groups have this permission: Domain Administrators, Enterprise Administrators, and Group Policy Creator Owners.</span></span> [<span data-ttu-id="b7850-115">Läs mer.</span><span class="sxs-lookup"><span data-stu-id="b7850-115">Learn more.</span></span>](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-the-distribution-point"></a><span data-ttu-id="b7850-116">Steg 1: Skapa distributionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="b7850-116">Step 1: Create the Distribution Point</span></span>
<span data-ttu-id="b7850-117">Först måste du placera installer-paketet på en nätverksplats som kan användas av datorer som du vill fjärrinstallera tillägget på.</span><span class="sxs-lookup"><span data-stu-id="b7850-117">First, you must place the installer package on a network location that can be accessed by the machines that you wish to remotely install the extension on.</span></span> <span data-ttu-id="b7850-118">Följ dessa steg om du vill göra detta:</span><span class="sxs-lookup"><span data-stu-id="b7850-118">To do this, follow these steps:</span></span>

1. <span data-ttu-id="b7850-119">Logga in på servern som en administratör</span><span class="sxs-lookup"><span data-stu-id="b7850-119">Log on to the server as an administrator</span></span>
2. <span data-ttu-id="b7850-120">I den **Serverhanteraren** fönster, gå till **filer och lagringstjänster**.</span><span class="sxs-lookup"><span data-stu-id="b7850-120">In the **Server Manager** window, go to **Files and Storage Services**.</span></span>
   
    ![Öppna filer och lagringstjänster](./media/active-directory-saas-ie-group-policy/files-services.png)
3. <span data-ttu-id="b7850-122">Gå till den **resurser** fliken. Klicka på **uppgifter** > **ny resurs...**</span><span class="sxs-lookup"><span data-stu-id="b7850-122">Go to the **Shares** tab. Then click **Tasks** > **New Share...**</span></span>
   
    ![Öppna filer och lagringstjänster](./media/active-directory-saas-ie-group-policy/shares.png)
4. <span data-ttu-id="b7850-124">Slutför den **ny resurs Guide** och ange behörigheter så att den kan nås från användarnas datorer.</span><span class="sxs-lookup"><span data-stu-id="b7850-124">Complete the **New Share Wizard** and set permissions to ensure that it can be accessed from your users' machines.</span></span> [<span data-ttu-id="b7850-125">Mer information om resurser.</span><span class="sxs-lookup"><span data-stu-id="b7850-125">Learn more about shares.</span></span>](https://technet.microsoft.com/library/cc753175.aspx)
5. <span data-ttu-id="b7850-126">Hämta följande Microsoft Windows Installer-paketet (MSI-fil): [åtkomst panelen Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)</span><span class="sxs-lookup"><span data-stu-id="b7850-126">Download the following Microsoft Windows Installer package (.msi file): [Access Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)</span></span>
6. <span data-ttu-id="b7850-127">Kopiera installationspaketet till önskad plats för resursen.</span><span class="sxs-lookup"><span data-stu-id="b7850-127">Copy the installer package to a desired location on the share.</span></span>
   
    ![Kopiera MSI-filen till resursen.](./media/active-directory-saas-ie-group-policy/copy-package.png)
7. <span data-ttu-id="b7850-129">Kontrollera att dina klientdatorer har tillgång till installationspaketet från resursen.</span><span class="sxs-lookup"><span data-stu-id="b7850-129">Verify that your client machines are able to access the installer package from the share.</span></span> 

## <a name="step-2-create-the-group-policy-object"></a><span data-ttu-id="b7850-130">Steg 2: Skapa grupprincipobjektet</span><span class="sxs-lookup"><span data-stu-id="b7850-130">Step 2: Create the Group Policy Object</span></span>
1. <span data-ttu-id="b7850-131">Logga in på servern som är värd för Active Directory Domain Services (AD DS)-installationen.</span><span class="sxs-lookup"><span data-stu-id="b7850-131">Log on to the server that hosts your Active Directory Domain Services (AD DS) installation.</span></span>
2. <span data-ttu-id="b7850-132">I Serverhanteraren, gå till **verktyg** > **Grupprinciphantering**.</span><span class="sxs-lookup"><span data-stu-id="b7850-132">In the Server Manager, go to **Tools** > **Group Policy Management**.</span></span>
   
    ![Gå till Verktyg > Gruppera Policy Management](./media/active-directory-saas-ie-group-policy/tools-gpm.png)
3. <span data-ttu-id="b7850-134">I den vänstra rutan i **Grupprinciphantering** fönstret Visa organisationsenheten (OU) hierarkin och avgöra vilka definitionsområdet du vill tillämpa grupprincipen.</span><span class="sxs-lookup"><span data-stu-id="b7850-134">In the left pane of the **Group Policy Management** window, view your Organizational Unit (OU) hierarchy and determine at which scope you would like to apply the group policy.</span></span> <span data-ttu-id="b7850-135">Exempelvis kanske du vill välja en liten Organisationsenhet för att distribuera till ett fåtal användare för att testa eller du kan välja en översta Organisationsenhet för att distribuera till hela organisationen.</span><span class="sxs-lookup"><span data-stu-id="b7850-135">For instance, you may decide to pick a small OU to deploy to a few users for testing, or you may pick a top-level OU to deploy to your entire organization.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b7850-136">Om du vill skapa eller redigera dina organisationsenheter (OU), växla tillbaka till Server Manager och gå till **verktyg** > **Active Directory-användare och datorer**.</span><span class="sxs-lookup"><span data-stu-id="b7850-136">If you would like to create or edit your Organization Units (OUs), switch back to the Server Manager and go to **Tools** > **Active Directory Users and Computers**.</span></span>
   > 
   > 
4. <span data-ttu-id="b7850-137">När du har valt en Organisationsenhet, högerklicka och välj **skapa ett grupprincipobjekt i den här domänen och länka det här...**</span><span class="sxs-lookup"><span data-stu-id="b7850-137">Once you have selected an OU, right-click it and select **Create a GPO in this domain, and Link it here...**</span></span>
   
    ![Skapa ett nytt GPO](./media/active-directory-saas-ie-group-policy/create-gpo.png)
5. <span data-ttu-id="b7850-139">I den **nytt GPO** skriver ett namn på nytt grupprincipobjekt.</span><span class="sxs-lookup"><span data-stu-id="b7850-139">In the **New GPO** prompt, type in a name for the new Group Policy Object.</span></span>
   
    ![Namn på det nya Grupprincipobjektet](./media/active-directory-saas-ie-group-policy/name-gpo.png)
6. <span data-ttu-id="b7850-141">Högerklicka på grupprincipobjektet som du skapade och välj **redigera**.</span><span class="sxs-lookup"><span data-stu-id="b7850-141">Right-click the Group Policy Object that you created, and select **Edit**.</span></span>
   
    ![Redigera det nya Grupprincipobjektet](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

## <a name="step-3-assign-the-installation-package"></a><span data-ttu-id="b7850-143">Steg 3: Tilldela installationspaketet</span><span class="sxs-lookup"><span data-stu-id="b7850-143">Step 3: Assign the Installation Package</span></span>
1. <span data-ttu-id="b7850-144">Avgöra om du vill distribuera tillägget baserat på **Datorkonfiguration** eller **Användarkonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="b7850-144">Determine whether you would like to deploy the extension based on **Computer Configuration** or **User Configuration**.</span></span> <span data-ttu-id="b7850-145">När du använder [Datorkonfiguration](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), tillägget har installerats på datorn oavsett vilka användare loggar in på den.</span><span class="sxs-lookup"><span data-stu-id="b7850-145">When using [computer configuration](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), the extension is installed on the computer regardless of which users log on to it.</span></span> <span data-ttu-id="b7850-146">Med [Användarkonfiguration](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), användare ha filnamnstillägget installerats för dem oavsett vilka datorer de loggar in på.</span><span class="sxs-lookup"><span data-stu-id="b7850-146">With [user configuration](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), users have the extension installed for them regardless of which computers they log on to.</span></span>
2. <span data-ttu-id="b7850-147">I den vänstra rutan i **Redigeraren för Grupprinciphantering** fönster, gå till något av följande mappsökvägar, beroende på vilken typ av du har valt:</span><span class="sxs-lookup"><span data-stu-id="b7850-147">In the left pane of the **Group Policy Management Editor** window, go to either of the following folder paths, depending on which type of configuration you chose:</span></span>
   
   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`
3. <span data-ttu-id="b7850-148">Högerklicka på **Programvaruinstallation**och välj **ny** > **paket...**</span><span class="sxs-lookup"><span data-stu-id="b7850-148">Right-click **Software installation**, then select **New** > **Package...**</span></span>
   
    ![Skapa en ny installationspaket för programvara](./media/active-directory-saas-ie-group-policy/new-package.png)
4. <span data-ttu-id="b7850-150">Gå till den delade mappen som innehåller installationspaketet från [steg 1: skapa distributionsplatsen](#step-1-create-the-distribution-point), Välj MSI-filen och klicka på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="b7850-150">Go to the shared folder that contains the installer package from [Step 1: Create the Distribution Point](#step-1-create-the-distribution-point), select the .msi file, and click **Open**.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="b7850-151">Om resursen finns på den här samma server, kontrollerar du att du använder .msi via nätverket filsökväg i stället för den lokala filsökvägen.</span><span class="sxs-lookup"><span data-stu-id="b7850-151">If the share is located on this same server, verify that you are accessing the .msi through the network file path, rather than the local file path.</span></span>
   > 
   > 
   
    ![Välj installationspaketet från den delade mappen.](./media/active-directory-saas-ie-group-policy/select-package.png)
5. <span data-ttu-id="b7850-153">I den **distribuera programvara** fråga, Välj **tilldelad** för din distributionsmetod.</span><span class="sxs-lookup"><span data-stu-id="b7850-153">In the **Deploy Software** prompt, select **Assigned** for your deployment method.</span></span> <span data-ttu-id="b7850-154">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b7850-154">Then click **OK**.</span></span>
   
    ![Välj tilldelad och klicka sedan på OK.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

<span data-ttu-id="b7850-156">Tillägget har nu distribuerats till Organisationsenheten som du har valt.</span><span class="sxs-lookup"><span data-stu-id="b7850-156">The extension is now deployed to the OU that you selected.</span></span> [<span data-ttu-id="b7850-157">Läs mer om Programvaruinstallation för Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="b7850-157">Learn more about Group Policy Software Installation.</span></span>](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-the-extension-for-internet-explorer"></a><span data-ttu-id="b7850-158">Steg 4: Auto-aktivera tillägget för Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="b7850-158">Step 4: Auto-Enable the Extension for Internet Explorer</span></span>
<span data-ttu-id="b7850-159">Förutom att köra installationsprogrammet, måste alla tillägg för Internet Explorer uttryckligen aktiveras innan den kan användas.</span><span class="sxs-lookup"><span data-stu-id="b7850-159">In addition to running the installer, every extension for Internet Explorer must be explicitly enabled before it can be used.</span></span> <span data-ttu-id="b7850-160">Följ stegen nedan för att aktivera åtkomst panelen tillägget med hjälp av Grupprincip:</span><span class="sxs-lookup"><span data-stu-id="b7850-160">Follow the steps below to enable the Access Panel Extension using group policy:</span></span>

1. <span data-ttu-id="b7850-161">I den **Redigeraren för Grupprinciphantering** fönster, gå till något av följande sökvägar, beroende på vilken typ av du valde i [steg3: tilldela installationspaketet](#step-3-assign-the-installation-package):</span><span class="sxs-lookup"><span data-stu-id="b7850-161">In the **Group Policy Management Editor** window, go to either of the following paths, depending on which type of configuration you chose in [Step 3: Assign the Installation Package](#step-3-assign-the-installation-package):</span></span>
   
   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
2. <span data-ttu-id="b7850-162">Högerklicka på **lista över tillägg**, och välj **redigera**.</span><span class="sxs-lookup"><span data-stu-id="b7850-162">Right-click **Add-on List**, and select **Edit**.</span></span>
    <span data-ttu-id="b7850-163">![Redigera lista över tillägg.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)</span><span class="sxs-lookup"><span data-stu-id="b7850-163">![Edit Add-on List.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)</span></span>
3. <span data-ttu-id="b7850-164">I den **lista över tillägg** väljer **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="b7850-164">In the **Add-on List** window, select **Enabled**.</span></span> <span data-ttu-id="b7850-165">Sedan, under den **alternativ** klickar du på **visa...** .</span><span class="sxs-lookup"><span data-stu-id="b7850-165">Then, under the **Options** section, click **Show...**.</span></span>
   
    ![Klicka på Aktivera och klicka sedan på Visa...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)
4. <span data-ttu-id="b7850-167">I den **Visa innehåll** fönster, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="b7850-167">In the **Show Contents** window, perform the following steps:</span></span>
   
   1. <span data-ttu-id="b7850-168">För den första kolumnen (den **värdenamn** fältet), kopiera och klistra in följande klass-ID:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`</span><span class="sxs-lookup"><span data-stu-id="b7850-168">For the first column (the **Value Name** field), copy and paste the following Class ID: `{030E9A3F-7B18-4122-9A60-B87235E4F59E}`</span></span>
   2. <span data-ttu-id="b7850-169">För den andra kolumnen (den **värdet** fältet), anger följande värde:`1`</span><span class="sxs-lookup"><span data-stu-id="b7850-169">For the second column (the **Value** field), type in the following value: `1`</span></span>
   3. <span data-ttu-id="b7850-170">Klicka på **OK** att stänga den **Visa innehåll** fönster.</span><span class="sxs-lookup"><span data-stu-id="b7850-170">Click **OK** to close the **Show Contents** window.</span></span>
      
      ![Fyll i värdena som anges ovan.](./media/active-directory-saas-ie-group-policy/show-contents.png)
5. <span data-ttu-id="b7850-172">Klicka på **OK** att tillämpa ändringarna och stänga den **lista över tillägg** fönster.</span><span class="sxs-lookup"><span data-stu-id="b7850-172">Click **OK** to apply your changes and close the **Add-on List** window.</span></span>

<span data-ttu-id="b7850-173">Tillägget bör nu aktiveras för datorer i den valda Organisationsenheten.</span><span class="sxs-lookup"><span data-stu-id="b7850-173">The extension should now be enabled for the machines in the selected OU.</span></span> [<span data-ttu-id="b7850-174">Mer information om hur du använder grupprinciper för att aktivera eller inaktivera tillägg för Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="b7850-174">Learn more about using group policy to enable or disable Internet Explorer add-ons.</span></span>](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a><span data-ttu-id="b7850-175">Steg 5 (valfritt): inaktivera ”Kom ihåg lösenord” fråga</span><span class="sxs-lookup"><span data-stu-id="b7850-175">Step 5 (Optional): Disable "Remember Password" Prompt</span></span>
<span data-ttu-id="b7850-176">När användare logga in på webbplatser med åtkomst till Kontrollpanelen, kan Internet Explorer visa följande meddelande som ber ”vill du att lagra lösenordet”?</span><span class="sxs-lookup"><span data-stu-id="b7850-176">When users sign-in to websites using the Access Panel Extension, Internet Explorer may show the following prompt asking "Would you like to store your password?"</span></span>

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

<span data-ttu-id="b7850-177">Om du vill förhindra att användarna kan se den här uppmaningen följer du stegen nedan för att förhindra automatisk komplettering från att komma ihåg lösenord:</span><span class="sxs-lookup"><span data-stu-id="b7850-177">If you wish to prevent your users from seeing this prompt, then follow the steps below to prevent auto-complete from remembering passwords:</span></span>

1. <span data-ttu-id="b7850-178">I den **Redigeraren för Grupprinciphantering** fönster, gå till sökvägen som anges nedan.</span><span class="sxs-lookup"><span data-stu-id="b7850-178">In the **Group Policy Management Editor** window, go to the path listed below.</span></span> <span data-ttu-id="b7850-179">Den här inställningen är endast tillgängliga under **Användarkonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="b7850-179">This configuration setting is only available under **User Configuration**.</span></span>
   
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
2. <span data-ttu-id="b7850-180">Den inställning som heter **aktivera funktionen för automatisk komplettering för användarnamn och lösenord för formulär för**.</span><span class="sxs-lookup"><span data-stu-id="b7850-180">Find the setting named **Turn on the auto-complete feature for user names and passwords on forms**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b7850-181">Tidigare versioner av Active Directory kan visa en lista med den här inställningen med namnet **tillåter inte automatisk komplettering att spara lösenord**.</span><span class="sxs-lookup"><span data-stu-id="b7850-181">Previous versions of Active Directory may list this setting with the name **Do not allow auto-complete to save passwords**.</span></span> <span data-ttu-id="b7850-182">Konfigurationen för den här inställningen skiljer sig från inställningen som beskrivs i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="b7850-182">The configuration for that setting differs from the setting described in this tutorial.</span></span>
   > 
   > 
   
    ![Kom ihåg att leta efter detta under inställningar.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)
3. <span data-ttu-id="b7850-184">Högerklicka på inställningarna ovan och välj **redigera**.</span><span class="sxs-lookup"><span data-stu-id="b7850-184">Right click the above setting, and select **Edit**.</span></span>
4. <span data-ttu-id="b7850-185">I fönstret **aktivera funktionen för automatisk komplettering för användarnamn och lösenord för formulär för**väljer **inaktiverade**.</span><span class="sxs-lookup"><span data-stu-id="b7850-185">In the window titled **Turn on the auto-complete feature for user names and passwords on forms**, select **Disabled**.</span></span>
   
    ![Välj inaktivera](./media/active-directory-saas-ie-group-policy/disable-passwords.png)
5. <span data-ttu-id="b7850-187">Klicka på **OK** att tillämpa ändringarna och stänga fönstret.</span><span class="sxs-lookup"><span data-stu-id="b7850-187">Click **OK** to apply these changes and close the window.</span></span>

<span data-ttu-id="b7850-188">Användare kommer inte längre att kunna lagra sina autentiseringsuppgifter eller Använd automatisk komplettering för att komma åt tidigare lagrade autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b7850-188">Users will no longer be able to store their credentials or use auto-complete to access previously stored credentials.</span></span> <span data-ttu-id="b7850-189">Den här principen tillåter användare att fortsätta att använda automatisk komplettering för andra typer av fält, till exempel sökfält.</span><span class="sxs-lookup"><span data-stu-id="b7850-189">However, this policy does allow users to continue to use auto-complete for other types of form fields, such as search fields.</span></span>

> [!WARNING]
> <span data-ttu-id="b7850-190">Om den här principen är aktiverad när användare har valt att lagra vissa autentiseringsuppgifter, kommer den här principen *inte* avmarkerar du de autentiseringsuppgifter som redan har lagrats.</span><span class="sxs-lookup"><span data-stu-id="b7850-190">If this policy is enabled after users have chosen to store some credentials, this policy will *not* clear the credentials that have already been stored.</span></span>
> 
> 

## <a name="step-6-testing-the-deployment"></a><span data-ttu-id="b7850-191">Steg 6: Testa distributionen</span><span class="sxs-lookup"><span data-stu-id="b7850-191">Step 6: Testing the Deployment</span></span>
<span data-ttu-id="b7850-192">Följ stegen nedan för att kontrollera om tillägget distributionen har lyckats:</span><span class="sxs-lookup"><span data-stu-id="b7850-192">Follow the steps below to verify if the extension deployment was successful:</span></span>

1. <span data-ttu-id="b7850-193">Om du har distribuerat med **Datorkonfiguration**, logga in på en klientdator som hör till den Organisationsenhet som du valde i [steg 2: skapa grupprincipobjektet](#step-2-create-the-group-policy-object).</span><span class="sxs-lookup"><span data-stu-id="b7850-193">If you deployed using **Computer Configuration**, sign into a client machine that belongs to the OU that you selected in [Step 2: Create the Group Policy Object](#step-2-create-the-group-policy-object).</span></span> <span data-ttu-id="b7850-194">Om du har distribuerat med **Användarkonfiguration**, se till att logga in som en användare som tillhör denna Organisationsenhet.</span><span class="sxs-lookup"><span data-stu-id="b7850-194">If you deployed using **User Configuration**, make sure to sign in as a user who belongs to that OU.</span></span>
2. <span data-ttu-id="b7850-195">Det kan ta ett par logga moduler för grupprincipen ändras till fullständigt uppdatera till den här datorn.</span><span class="sxs-lookup"><span data-stu-id="b7850-195">It may take a couple sign ins for the group policy changes to fully update with this machine.</span></span> <span data-ttu-id="b7850-196">Om du vill tvinga uppdateringen, öppna en **kommandotolk** fönster och kör sedan följande kommando:`gpupdate /force`</span><span class="sxs-lookup"><span data-stu-id="b7850-196">To force the update, open a **Command Prompt** window and run the following command: `gpupdate /force`</span></span>
3. <span data-ttu-id="b7850-197">Du måste starta om datorn för att installationen ska kunna utföras.</span><span class="sxs-lookup"><span data-stu-id="b7850-197">You must restart the machine for the installation to take place.</span></span> <span data-ttu-id="b7850-198">Autostart kan ta betydligt längre tid än vanligt när tillägget installeras.</span><span class="sxs-lookup"><span data-stu-id="b7850-198">Bootup may take significantly more time than usual while the extension installs.</span></span>
4. <span data-ttu-id="b7850-199">Efter omstart, öppna **Internet Explorer**.</span><span class="sxs-lookup"><span data-stu-id="b7850-199">After restarting, open **Internet Explorer**.</span></span> <span data-ttu-id="b7850-200">Klicka på det övre högra hörnet i fönstret **verktyg** (kugghjulsikonen) och välj sedan **Hantera tillägg**.</span><span class="sxs-lookup"><span data-stu-id="b7850-200">On the upper-right corner of the window, click **Tools** (the gear icon), and then select **Manage add-ons**.</span></span>
   
    ![Gå till Verktyg > Hantera tillägg](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)
5. <span data-ttu-id="b7850-202">I den **Hantera tillägg** fönster, kontrollerar du att den **åtkomst panelen tillägget** har installerats och att dess **Status** har ställts in på **aktiverad**.</span><span class="sxs-lookup"><span data-stu-id="b7850-202">In the **Manage Add-ons** window, verify that the **Access Panel Extension** has been installed and that its **Status** has been set to **Enabled**.</span></span>
   
    ![Kontrollera att tillägget för åtkomst-panelen är installerat och aktiverat.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a><span data-ttu-id="b7850-204">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="b7850-204">Related Articles</span></span>
* [<span data-ttu-id="b7850-205">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b7850-205">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="b7850-206">Programåtkomst och enkel inloggning med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b7850-206">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="b7850-207">Felsökning av Access panelen-tillägg för Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="b7850-207">Troubleshooting the Access Panel Extension for Internet Explorer</span></span>](active-directory-saas-ie-troubleshooting.md)

