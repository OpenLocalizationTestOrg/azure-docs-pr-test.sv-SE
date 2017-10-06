---
title: "aaaTroubleshooting hello Azure Access panelen-tillägg för Internet Explorer | Microsoft Docs"
description: "Hur toouse gruppera princip toodeploy hello Internet Explorer-tillägget för hello Mina appar portal."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: f56b3230-26fd-42ec-9e3d-2c12daf15479
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 23cbb6117f34759330206de3a26f1397486acedb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-access-panel-extension-for-internet-explorer"></a><span data-ttu-id="63b65-103">Felsöka hello Access panelen-tillägg för Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="63b65-103">Troubleshooting hello Access Panel Extension for Internet Explorer</span></span>
<span data-ttu-id="63b65-104">Den här artikeln hjälper dig att felsöka hello följande problem:</span><span class="sxs-lookup"><span data-stu-id="63b65-104">This article helps you troubleshoot hello following problems:</span></span>

* <span data-ttu-id="63b65-105">Du är tooaccess dina appar via hello Mina appar portal när du använder Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="63b65-105">You're unable tooaccess your apps through hello My Apps portal while using Internet Explorer.</span></span>
* <span data-ttu-id="63b65-106">Du kan se hello ”installera programvara” visas trots att du redan har installerat hello programvara.</span><span class="sxs-lookup"><span data-stu-id="63b65-106">You see hello "Install Software" message even though you've already installed hello software.</span></span>

<span data-ttu-id="63b65-107">Om du är administratör kan se även: [hur tooDeploy hello Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip](active-directory-saas-ie-group-policy.md)</span><span class="sxs-lookup"><span data-stu-id="63b65-107">If you are an admin, see also: [How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy](active-directory-saas-ie-group-policy.md)</span></span>

## <a name="run-hello-diagnostic-tool"></a><span data-ttu-id="63b65-108">Kör hello diagnostikverktyg</span><span class="sxs-lookup"><span data-stu-id="63b65-108">Run hello Diagnostic Tool</span></span>
<span data-ttu-id="63b65-109">Du kan diagnostisera problem med hello tillägget för åtkomst-panelen genom att hämta och kör diagnostikverktyg för hello åtkomstpanelen:</span><span class="sxs-lookup"><span data-stu-id="63b65-109">You can diagnose installation problems with hello Access Panel Extension by downloading and running hello Access Panel diagnostic tool:</span></span>

1. [<span data-ttu-id="63b65-110">Klicka här toodownload hello diagnostikverktyg.</span><span class="sxs-lookup"><span data-stu-id="63b65-110">Click here toodownload hello diagnostic tool.</span></span>](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. <span data-ttu-id="63b65-111">Öppna hello-filen och klicka på **extrahera alla** knappen.</span><span class="sxs-lookup"><span data-stu-id="63b65-111">Open hello file, and press **Extract all** button.</span></span>
   
    ![Klicka på Extrahera alla](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. <span data-ttu-id="63b65-113">Tryck på hello **extrahera** knappen toocontinue.</span><span class="sxs-lookup"><span data-stu-id="63b65-113">Then press hello **Extract** button toocontinue.</span></span>
   
    ![Klicka på Extrahera](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. <span data-ttu-id="63b65-115">toorun hello verktyg, högerklicka på hello-fil med namnet **AccessPanelExtensionDiagnosticTool**och välj **öppna med > Microsoft Windows-baserad skriptmodul**.</span><span class="sxs-lookup"><span data-stu-id="63b65-115">toorun hello tool, right-click hello file named **AccessPanelExtensionDiagnosticTool**, then select **Open with > Microsoft Windows Based Script Host**.</span></span>
   
    ![Öppna med > Microsoft Windows-baserade skriptvärden](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. <span data-ttu-id="63b65-117">Sedan visas hello följande diagnostiska fönster som beskriver vad kan vara fel med installationen.</span><span class="sxs-lookup"><span data-stu-id="63b65-117">You will then see hello following diagnostic window, which describes what might be wrong with your installation.</span></span>
   
    ![Ett exempel på hello diagnostiska fönster](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. <span data-ttu-id="63b65-119">Klicka på ”**Ja**” toolet hello programmet korrigering hello problem som har hittats.</span><span class="sxs-lookup"><span data-stu-id="63b65-119">Click "**YES**" toolet hello program fix hello issues that have been found.</span></span>
7. <span data-ttu-id="63b65-120">toosave dessa ändringar och Stäng alla fönster i Internet Explorer och öppna Internet Explorer igen.</span><span class="sxs-lookup"><span data-stu-id="63b65-120">toosave these changes, close every Internet Explorer window, and then open Internet Explorer again.</span></span><br /><span data-ttu-id="63b65-121">Försök hello stegen nedan om du fortfarande inte kan komma åt dina appar.</span><span class="sxs-lookup"><span data-stu-id="63b65-121">If you still can't access your apps, try hello steps below.</span></span>

## <a name="check-that-hello-access-panel-extension-is-enabled"></a><span data-ttu-id="63b65-122">Kontrollera att hello åtkomst panelen tillägget är aktiverat</span><span class="sxs-lookup"><span data-stu-id="63b65-122">Check that hello Access Panel Extension is enabled</span></span>
<span data-ttu-id="63b65-123">tooverify som hello panelen tillägget för åtkomst är aktiverad i Internet Explorer:</span><span class="sxs-lookup"><span data-stu-id="63b65-123">tooverify that hello Access Panel Extension is enabled in Internet Explorer:</span></span>

1. <span data-ttu-id="63b65-124">I Internet Explorer klickar du på hello **Kugghjulet ikonen** i hello övre högra hörnet av hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="63b65-124">In Internet Explorer, click hello **Gear icon** on hello top right corner of hello window.</span></span> <span data-ttu-id="63b65-125">Välj sedan **Internetalternativ**.</span><span class="sxs-lookup"><span data-stu-id="63b65-125">Then select **Internet options**.</span></span><br /><span data-ttu-id="63b65-126">(I tidigare versioner av Internet Explorer kan du hitta detta under **Verktyg > Internetalternativ**.</span><span class="sxs-lookup"><span data-stu-id="63b65-126">(In older versions of Internet Explorer you can find this under **Tools > Internet options**.</span></span>
   
    ![Gå tooTools > Internet-alternativ](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. <span data-ttu-id="63b65-128">Klicka på hello **program** och klicka sedan på hello **Hantera tillägg** knappen.</span><span class="sxs-lookup"><span data-stu-id="63b65-128">Click hello **Programs** tab, then click hello **Manage add-ons** button.</span></span>
   
    ![Klicka på Hantera tillägg](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. <span data-ttu-id="63b65-130">I den här dialogrutan Välj **åtkomst panelen tillägget** och klicka sedan på hello **aktivera** knappen.</span><span class="sxs-lookup"><span data-stu-id="63b65-130">In this dialog, select **Access Panel Extension** and then click hello **Enable** button.</span></span>
   
    ![Klicka på Aktivera](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. <span data-ttu-id="63b65-132">toosave dessa ändringar och Stäng alla fönster i Internet Explorer och öppna Internet Explorer igen.</span><span class="sxs-lookup"><span data-stu-id="63b65-132">toosave these changes, close every Internet Explorer window and then open Internet Explorer again.</span></span>

## <a name="enable-extensions-for-inprivate-browsing"></a><span data-ttu-id="63b65-133">Aktivera tillägg för InPrivate-surfning</span><span class="sxs-lookup"><span data-stu-id="63b65-133">Enable Extensions for InPrivate Browsing</span></span>
<span data-ttu-id="63b65-134">Om du använder hello InPrivate-surfning läge:</span><span class="sxs-lookup"><span data-stu-id="63b65-134">If you are using hello InPrivate Browsing mode:</span></span>

1. <span data-ttu-id="63b65-135">I Internet Explorer klickar du på hello **Kugghjulet ikonen** i hello övre högra hörnet av hello-fönstret.</span><span class="sxs-lookup"><span data-stu-id="63b65-135">In Internet Explorer, click hello **Gear icon** on hello top right corner of hello window.</span></span> <span data-ttu-id="63b65-136">Välj sedan **Internetalternativ**.</span><span class="sxs-lookup"><span data-stu-id="63b65-136">Then select **Internet options**.</span></span><br /><span data-ttu-id="63b65-137">(I tidigare versioner av Internet Explorer kan du hitta detta under **Verktyg > Internetalternativ**.</span><span class="sxs-lookup"><span data-stu-id="63b65-137">(In older versions of Internet Explorer you can find this under **Tools > Internet options**.</span></span>
   
    ![Ett exempel på hello diagnostiska fönster](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. <span data-ttu-id="63b65-139">Gå toohello **sekretess** sedan fliken **avmarkera** hello kryssrutan **inaktivera verktygsfält och tillägg när InPrivate-surfning startar**</span><span class="sxs-lookup"><span data-stu-id="63b65-139">Go toohello **Privacy** tab, then **uncheck** hello checkbox labeled **Disable toolbars and extensions when InPrivate Browsing starts**</span></span></p>
   
    ![Avmarkera inaktivera verktygsfält och tillägg när InPrivate-surfning startar](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. <span data-ttu-id="63b65-141">toosave dessa ändringar och Stäng alla fönster i Internet Explorer och öppna Internet Explorer igen.</span><span class="sxs-lookup"><span data-stu-id="63b65-141">toosave these changes, close every Internet Explorer window and then open Internet Explorer again.</span></span>

## <a name="uninstall-hello-access-panel-extension"></a><span data-ttu-id="63b65-142">Avinstallera hello Access panelen-tillägg</span><span class="sxs-lookup"><span data-stu-id="63b65-142">Uninstall hello Access Panel Extension</span></span>
<span data-ttu-id="63b65-143">toouninstall hello åtkomstpanelen tillägg från datorn:</span><span class="sxs-lookup"><span data-stu-id="63b65-143">toouninstall hello Access Panel extension from your computer:</span></span>

1. <span data-ttu-id="63b65-144">Tryck på tangentbordet hello **Windows-tangenten** tooopen hello Start-menyn.</span><span class="sxs-lookup"><span data-stu-id="63b65-144">On your keyboard, press hello **Windows key** tooopen hello Start menu.</span></span> <span data-ttu-id="63b65-145">När hello-menyn är öppen kan du skriva vad toodo en sökning.</span><span class="sxs-lookup"><span data-stu-id="63b65-145">When hello menu is open, you can type anything toodo a search.</span></span> <span data-ttu-id="63b65-146">Skriv ”på Kontrollpanelen” och sedan öppna hello **Kontrollpanelen** när den visas i sökresultaten hello.</span><span class="sxs-lookup"><span data-stu-id="63b65-146">Type "Control Panel" and then open hello **Control Panel** when it appears in hello search results.</span></span>
   
    ![Sök efter Kontrollpanelen](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. <span data-ttu-id="63b65-148">Ändra hello i hello övre högra hörnet av hello Kontrollpanelen, **visa med** alternativet för**stora ikoner**.</span><span class="sxs-lookup"><span data-stu-id="63b65-148">In hello top right corner of hello Control Panel, change hello **View by** option too**Large icons**.</span></span> <span data-ttu-id="63b65-149">Sedan söka efter och klicka på hello **program och funktioner** knappen.</span><span class="sxs-lookup"><span data-stu-id="63b65-149">Then find and click hello **Programs and Features** button.</span></span>
   
    ![Ändra hello visa tooshow stora ikoner](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. <span data-ttu-id="63b65-151">Välj hello listan **åtkomst panelen tillägget**, och klicka på hello hello **avinstallera** knappen.</span><span class="sxs-lookup"><span data-stu-id="63b65-151">From hello list, select **Access Panel Extension**, and hello click hello **Uninstall** button.</span></span>
   
    ![Klicka på Avinstallera](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. <span data-ttu-id="63b65-153">Sedan kan du försöka tooinstall hello tillägget igen toosee om hello problemet har lösts.</span><span class="sxs-lookup"><span data-stu-id="63b65-153">You can then try tooinstall hello extension again toosee if hello problem has been resolved.</span></span>

<span data-ttu-id="63b65-154">Om du får problem med avinstallerar hello tillägg, du kan också ta bort den med hjälp av hello [Microsoft åtgärda det](https://go.microsoft.com/?linkid=9779673) verktyget.</span><span class="sxs-lookup"><span data-stu-id="63b65-154">If you encounter issues uninstalling hello extension, you can also remove it using hello [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) tool.</span></span>

## <a name="related-articles"></a><span data-ttu-id="63b65-155">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="63b65-155">Related Articles</span></span>
* [<span data-ttu-id="63b65-156">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="63b65-156">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="63b65-157">Programåtkomst och enkel inloggning med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="63b65-157">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="63b65-158">Hur tooDeploy hello Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip</span><span class="sxs-lookup"><span data-stu-id="63b65-158">How tooDeploy hello Access Panel Extension for Internet Explorer using Group Policy</span></span>](active-directory-saas-ie-group-policy.md)

