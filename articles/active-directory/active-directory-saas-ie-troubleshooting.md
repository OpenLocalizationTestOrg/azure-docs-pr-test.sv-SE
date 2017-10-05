---
title: "Felsöka Azure Access panelen-tillägg för Internet Explorer | Microsoft Docs"
description: "Hur du använder grupprinciper för att distribuera Internet Explorer-tillägget för Mina appar portalen."
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
ms.openlocfilehash: 938d0b4046afa8c80eabe542f4541d0554948f5d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a><span data-ttu-id="cc2ac-103">Felsökning av Access panelen-tillägg för Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="cc2ac-103">Troubleshooting the Access Panel Extension for Internet Explorer</span></span>
<span data-ttu-id="cc2ac-104">Den här artikeln hjälper dig att felsöka följande problem:</span><span class="sxs-lookup"><span data-stu-id="cc2ac-104">This article helps you troubleshoot the following problems:</span></span>

* <span data-ttu-id="cc2ac-105">Du kan inte komma åt dina appar via portalen Mina appar när du använder Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-105">You're unable to access your apps through the My Apps portal while using Internet Explorer.</span></span>
* <span data-ttu-id="cc2ac-106">Meddelandet ”installera programvara” även om du redan har installerat programvaran.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-106">You see the "Install Software" message even though you've already installed the software.</span></span>

<span data-ttu-id="cc2ac-107">Om du är administratör kan se även: [hur du distribuerar Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip](active-directory-saas-ie-group-policy.md)</span><span class="sxs-lookup"><span data-stu-id="cc2ac-107">If you are an admin, see also: [How to Deploy the Access Panel Extension for Internet Explorer using Group Policy](active-directory-saas-ie-group-policy.md)</span></span>

## <a name="run-the-diagnostic-tool"></a><span data-ttu-id="cc2ac-108">Kör verktyget diagnostik</span><span class="sxs-lookup"><span data-stu-id="cc2ac-108">Run the Diagnostic Tool</span></span>
<span data-ttu-id="cc2ac-109">Du kan diagnostisera problem med filnamnstillägget åtkomst panelen genom att hämta och kör diagnostikverktyg för åtkomstpanelen:</span><span class="sxs-lookup"><span data-stu-id="cc2ac-109">You can diagnose installation problems with the Access Panel Extension by downloading and running the Access Panel diagnostic tool:</span></span>

1. [<span data-ttu-id="cc2ac-110">Klicka här om du vill hämta diagnostikverktyget.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-110">Click here to download the diagnostic tool.</span></span>](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. <span data-ttu-id="cc2ac-111">Öppna filen, och tryck på **extrahera alla** knappen.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-111">Open the file, and press **Extract all** button.</span></span>
   
    ![Klicka på Extrahera alla](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. <span data-ttu-id="cc2ac-113">Tryck på den **extrahera** för att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-113">Then press the **Extract** button to continue.</span></span>
   
    ![Klicka på Extrahera](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. <span data-ttu-id="cc2ac-115">Kör verktyget genom att högerklicka på filen med namnet **AccessPanelExtensionDiagnosticTool**och välj **öppna med > Microsoft Windows-baserad skriptmodul**.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-115">To run the tool, right-click the file named **AccessPanelExtensionDiagnosticTool**, then select **Open with > Microsoft Windows Based Script Host**.</span></span>
   
    ![Öppna med > Microsoft Windows-baserade skriptvärden](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. <span data-ttu-id="cc2ac-117">Sedan visas följande diagnostiska fönstret som beskriver vad kan vara fel med installationen.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-117">You will then see the following diagnostic window, which describes what might be wrong with your installation.</span></span>
   
    ![Ett exempel på fönstret diagnostik](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. <span data-ttu-id="cc2ac-119">Klicka på ”**Ja**” så att programmet åtgärda de problem som har hittats.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-119">Click "**YES**" to let the program fix the issues that have been found.</span></span>
7. <span data-ttu-id="cc2ac-120">Stäng alla fönster i Internet Explorer om du vill spara ändringarna och öppna Internet Explorer igen.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-120">To save these changes, close every Internet Explorer window, and then open Internet Explorer again.</span></span><br /><span data-ttu-id="cc2ac-121">Försök stegen nedan om du fortfarande inte kan komma åt dina appar.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-121">If you still can't access your apps, try the steps below.</span></span>

## <a name="check-that-the-access-panel-extension-is-enabled"></a><span data-ttu-id="cc2ac-122">Kontrollera att panelen-tillägget åtkomst är aktiverat</span><span class="sxs-lookup"><span data-stu-id="cc2ac-122">Check that the Access Panel Extension is enabled</span></span>
<span data-ttu-id="cc2ac-123">Kontrollera att tillägget för åtkomst-panelen är aktiverad i Internet Explorer:</span><span class="sxs-lookup"><span data-stu-id="cc2ac-123">To verify that the Access Panel Extension is enabled in Internet Explorer:</span></span>

1. <span data-ttu-id="cc2ac-124">I Internet Explorer klickar du på den **Kugghjulet ikonen** i det övre högra hörnet i fönstret.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-124">In Internet Explorer, click the **Gear icon** on the top right corner of the window.</span></span> <span data-ttu-id="cc2ac-125">Välj sedan **Internetalternativ**.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-125">Then select **Internet options**.</span></span><br /><span data-ttu-id="cc2ac-126">(I tidigare versioner av Internet Explorer kan du hitta detta under **Verktyg > Internetalternativ**.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-126">(In older versions of Internet Explorer you can find this under **Tools > Internet options**.</span></span>
   
    ![Gå till Verktyg > Internet-alternativ](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. <span data-ttu-id="cc2ac-128">Klicka på den **program** och klicka sedan på den **Hantera tillägg** knappen.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-128">Click the **Programs** tab, then click the **Manage add-ons** button.</span></span>
   
    ![Klicka på Hantera tillägg](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. <span data-ttu-id="cc2ac-130">I den här dialogrutan Välj **åtkomst panelen tillägget** och klicka sedan på den **aktivera** knappen.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-130">In this dialog, select **Access Panel Extension** and then click the **Enable** button.</span></span>
   
    ![Klicka på Aktivera](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. <span data-ttu-id="cc2ac-132">Stäng alla fönster i Internet Explorer och öppna Internet Explorer igen om du vill spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-132">To save these changes, close every Internet Explorer window and then open Internet Explorer again.</span></span>

## <a name="enable-extensions-for-inprivate-browsing"></a><span data-ttu-id="cc2ac-133">Aktivera tillägg för InPrivate-surfning</span><span class="sxs-lookup"><span data-stu-id="cc2ac-133">Enable Extensions for InPrivate Browsing</span></span>
<span data-ttu-id="cc2ac-134">Om du använder InPrivate-surfning-läge:</span><span class="sxs-lookup"><span data-stu-id="cc2ac-134">If you are using the InPrivate Browsing mode:</span></span>

1. <span data-ttu-id="cc2ac-135">I Internet Explorer klickar du på den **Kugghjulet ikonen** i det övre högra hörnet i fönstret.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-135">In Internet Explorer, click the **Gear icon** on the top right corner of the window.</span></span> <span data-ttu-id="cc2ac-136">Välj sedan **Internetalternativ**.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-136">Then select **Internet options**.</span></span><br /><span data-ttu-id="cc2ac-137">(I tidigare versioner av Internet Explorer kan du hitta detta under **Verktyg > Internetalternativ**.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-137">(In older versions of Internet Explorer you can find this under **Tools > Internet options**.</span></span>
   
    ![Ett exempel på fönstret diagnostik](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. <span data-ttu-id="cc2ac-139">Gå till den **sekretess** sedan fliken **avmarkera** kryssrutan **inaktivera verktygsfält och tillägg när InPrivate-surfning startar**</span><span class="sxs-lookup"><span data-stu-id="cc2ac-139">Go to the **Privacy** tab, then **uncheck** the checkbox labeled **Disable toolbars and extensions when InPrivate Browsing starts**</span></span></p>
   
    ![Avmarkera inaktivera verktygsfält och tillägg när InPrivate-surfning startar](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. <span data-ttu-id="cc2ac-141">Stäng alla fönster i Internet Explorer och öppna Internet Explorer igen om du vill spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-141">To save these changes, close every Internet Explorer window and then open Internet Explorer again.</span></span>

## <a name="uninstall-the-access-panel-extension"></a><span data-ttu-id="cc2ac-142">Avinstallera panelen-tillägget åtkomst</span><span class="sxs-lookup"><span data-stu-id="cc2ac-142">Uninstall the Access Panel Extension</span></span>
<span data-ttu-id="cc2ac-143">Så här avinstallerar åtkomstpanelen tillägget från datorn:</span><span class="sxs-lookup"><span data-stu-id="cc2ac-143">To uninstall the Access Panel extension from your computer:</span></span>

1. <span data-ttu-id="cc2ac-144">Tryck på på tangentbordet den **Windows-tangenten** att öppna Start-menyn.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-144">On your keyboard, press the **Windows key** to open the Start menu.</span></span> <span data-ttu-id="cc2ac-145">När menyn är öppen kan skriva du något om du vill göra en sökning.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-145">When the menu is open, you can type anything to do a search.</span></span> <span data-ttu-id="cc2ac-146">Skriv ”på Kontrollpanelen” och sedan öppna den **Kontrollpanelen** när den visas i sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-146">Type "Control Panel" and then open the **Control Panel** when it appears in the search results.</span></span>
   
    ![Sök efter Kontrollpanelen](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. <span data-ttu-id="cc2ac-148">I det övre högra hörnet av Kontrollpanelen ändrar den **visa med** att **stora ikoner**.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-148">In the top right corner of the Control Panel, change the **View by** option to **Large icons**.</span></span> <span data-ttu-id="cc2ac-149">Hitta och klicka sedan på **program och funktioner** knappen.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-149">Then find and click the **Programs and Features** button.</span></span>
   
    ![Ändra vyn för att visa stora ikoner](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. <span data-ttu-id="cc2ac-151">I listan, väljer du **åtkomst panelen tillägget**, och klicka på den **avinstallera** knappen.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-151">From the list, select **Access Panel Extension**, and the click the **Uninstall** button.</span></span>
   
    ![Klicka på Avinstallera](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. <span data-ttu-id="cc2ac-153">Du kan sedan försöker installera tillägget igen för att se om problemet är löst.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-153">You can then try to install the extension again to see if the problem has been resolved.</span></span>

<span data-ttu-id="cc2ac-154">Om du får problem med tillägget avinstallerar du kan också ta bort den med hjälp av den [Microsoft åtgärda det](https://go.microsoft.com/?linkid=9779673) verktyget.</span><span class="sxs-lookup"><span data-stu-id="cc2ac-154">If you encounter issues uninstalling the extension, you can also remove it using the [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) tool.</span></span>

## <a name="related-articles"></a><span data-ttu-id="cc2ac-155">Relaterade artiklar</span><span class="sxs-lookup"><span data-stu-id="cc2ac-155">Related Articles</span></span>
* [<span data-ttu-id="cc2ac-156">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cc2ac-156">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="cc2ac-157">Programåtkomst och enkel inloggning med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="cc2ac-157">Application access and single sign-on with Azure Active Directory</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="cc2ac-158">Hur du distribuerar Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip</span><span class="sxs-lookup"><span data-stu-id="cc2ac-158">How to Deploy the Access Panel Extension for Internet Explorer using Group Policy</span></span>](active-directory-saas-ie-group-policy.md)

