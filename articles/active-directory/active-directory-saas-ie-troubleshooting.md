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
# <a name="troubleshooting-hello-access-panel-extension-for-internet-explorer"></a>Felsöka hello Access panelen-tillägg för Internet Explorer
Den här artikeln hjälper dig att felsöka hello följande problem:

* Du är tooaccess dina appar via hello Mina appar portal när du använder Internet Explorer.
* Du kan se hello ”installera programvara” visas trots att du redan har installerat hello programvara.

Om du är administratör kan se även: [hur tooDeploy hello Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip](active-directory-saas-ie-group-policy.md)

## <a name="run-hello-diagnostic-tool"></a>Kör hello diagnostikverktyg
Du kan diagnostisera problem med hello tillägget för åtkomst-panelen genom att hämta och kör diagnostikverktyg för hello åtkomstpanelen:

1. [Klicka här toodownload hello diagnostikverktyg.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Öppna hello-filen och klicka på **extrahera alla** knappen.
   
    ![Klicka på Extrahera alla](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. Tryck på hello **extrahera** knappen toocontinue.
   
    ![Klicka på Extrahera](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. toorun hello verktyg, högerklicka på hello-fil med namnet **AccessPanelExtensionDiagnosticTool**och välj **öppna med > Microsoft Windows-baserad skriptmodul**.
   
    ![Öppna med > Microsoft Windows-baserade skriptvärden](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. Sedan visas hello följande diagnostiska fönster som beskriver vad kan vara fel med installationen.
   
    ![Ett exempel på hello diagnostiska fönster](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. Klicka på ”**Ja**” toolet hello programmet korrigering hello problem som har hittats.
7. toosave dessa ändringar och Stäng alla fönster i Internet Explorer och öppna Internet Explorer igen.<br />Försök hello stegen nedan om du fortfarande inte kan komma åt dina appar.

## <a name="check-that-hello-access-panel-extension-is-enabled"></a>Kontrollera att hello åtkomst panelen tillägget är aktiverat
tooverify som hello panelen tillägget för åtkomst är aktiverad i Internet Explorer:

1. I Internet Explorer klickar du på hello **Kugghjulet ikonen** i hello övre högra hörnet av hello-fönstret. Välj sedan **Internetalternativ**.<br />(I tidigare versioner av Internet Explorer kan du hitta detta under **Verktyg > Internetalternativ**.
   
    ![Gå tooTools > Internet-alternativ](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. Klicka på hello **program** och klicka sedan på hello **Hantera tillägg** knappen.
   
    ![Klicka på Hantera tillägg](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. I den här dialogrutan Välj **åtkomst panelen tillägget** och klicka sedan på hello **aktivera** knappen.
   
    ![Klicka på Aktivera](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. toosave dessa ändringar och Stäng alla fönster i Internet Explorer och öppna Internet Explorer igen.

## <a name="enable-extensions-for-inprivate-browsing"></a>Aktivera tillägg för InPrivate-surfning
Om du använder hello InPrivate-surfning läge:

1. I Internet Explorer klickar du på hello **Kugghjulet ikonen** i hello övre högra hörnet av hello-fönstret. Välj sedan **Internetalternativ**.<br />(I tidigare versioner av Internet Explorer kan du hitta detta under **Verktyg > Internetalternativ**.
   
    ![Ett exempel på hello diagnostiska fönster](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. Gå toohello **sekretess** sedan fliken **avmarkera** hello kryssrutan **inaktivera verktygsfält och tillägg när InPrivate-surfning startar**</p>
   
    ![Avmarkera inaktivera verktygsfält och tillägg när InPrivate-surfning startar](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. toosave dessa ändringar och Stäng alla fönster i Internet Explorer och öppna Internet Explorer igen.

## <a name="uninstall-hello-access-panel-extension"></a>Avinstallera hello Access panelen-tillägg
toouninstall hello åtkomstpanelen tillägg från datorn:

1. Tryck på tangentbordet hello **Windows-tangenten** tooopen hello Start-menyn. När hello-menyn är öppen kan du skriva vad toodo en sökning. Skriv ”på Kontrollpanelen” och sedan öppna hello **Kontrollpanelen** när den visas i sökresultaten hello.
   
    ![Sök efter Kontrollpanelen](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. Ändra hello i hello övre högra hörnet av hello Kontrollpanelen, **visa med** alternativet för**stora ikoner**. Sedan söka efter och klicka på hello **program och funktioner** knappen.
   
    ![Ändra hello visa tooshow stora ikoner](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. Välj hello listan **åtkomst panelen tillägget**, och klicka på hello hello **avinstallera** knappen.
   
    ![Klicka på Avinstallera](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. Sedan kan du försöka tooinstall hello tillägget igen toosee om hello problemet har lösts.

Om du får problem med avinstallerar hello tillägg, du kan också ta bort den med hjälp av hello [Microsoft åtgärda det](https://go.microsoft.com/?linkid=9779673) verktyget.

## <a name="related-articles"></a>Relaterade artiklar
* [Artikelindex för programhantering i Azure Active Directory](active-directory-apps-index.md)
* [Programåtkomst och enkel inloggning med Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [Hur tooDeploy hello Access panelen-tillägg för Internet Explorer med hjälp av Grupprincip](active-directory-saas-ie-group-policy.md)

