---
title: "aaaGet igång med en Azure logganalys-arbetsyta | Microsoft Docs"
description: "Du kan komma igång med en arbetsyta i Log Analytics på bara några minuter."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 508716de-72d3-4c06-9218-1ede631f23a6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/08/2017
ms.author: magoedte
ms.openlocfilehash: 442a9258a37ee79e8f0b45759ef24b5e3dae0130
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-a-log-analytics-workspace"></a>Komma igång med en arbetsyta för Log Analytics
Du kan snabbt komma igång med Azure Log Analytics som hjälper dig att utvärdera operativ information som samlas in från din IT-infrastruktur. Med den här artikeln kan du enkelt börja utforska, analysera och vidta åtgärder baserat på data som du samlar in *kostnadsfritt*.

Den här artikeln fungerar som en introduktion tooLog Analytics med hjälp av en kort självstudiekursen toowalk du via en minimal distribution i Azure så att du kan börja använda hello-tjänsten. hello logisk behållare där management-data i Azure lagras kallas för en arbetsyta. När du har granskat den informationen och egna utvärderats kan du ta bort hello utvärdering arbetsytan. Eftersom den här artikeln är en självstudie tar den inte upp vägledning för affärsbehov, planering och arkitektur.

>[!NOTE]
>Om du använder hello Microsoft Azure Government Cloud använda [Azure Government-övervakning + dokumentation](https://docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#log-analytics) i stället.

Här är en titt på hello process som används för tooget igång:

![processdiagram](./media/log-analytics-get-started/onboard-oms.png)

## <a name="1-create-an-azure-account-and-sign-in"></a>1 Skapa ett Azure-konto och logga in

Om du inte redan har ett Azure-konto, måste toocreate en toouse logganalys. Du kan skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/) som gäller i 30 dagar och som ger dig åtkomst till vilka Azure-tjänster du vill.

### <a name="toocreate-a-free-account-and-sign-in"></a>toocreate ett kostnadsfritt konto och logga in
1. Följ hello riktningar i [skapa din kostnadsfria Azure-konto](https://azure.microsoft.com/free/).
2. Gå toohello [Azure-portalen](https://portal.azure.com) och logga in.

## <a name="2-create-a-workspace"></a>2 Skapa en arbetsyta

hello nästa steg är toocreate en arbetsyta.

1. Sök i hello Azure-portalen, hello listan över tjänster i hello Marketplace för *logganalys*, och välj sedan **logganalys**.  
    ![Azure Portal](./media/log-analytics-get-started/log-analytics-portal.png)
2. Klicka på **skapa**, välj sedan alternativ för hello följande objekt:
   * **OMS-arbetsytan** – Ange ett namn för arbetsytan.
   * **Prenumerationen** - om du har flera prenumerationer, Välj hello en du vill tooassociate med hello nya arbetsområdet.
   * **Resursgrupp**
   * **Plats**
   * **prisnivå**  
       ![snabbregistrering](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Klicka på **OK** toosee en lista över dina arbetsytor.
4. Välj en arbetsyta toosee information i hello Azure-portalen.       
    ![information om arbetsytan](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         

## <a name="3-upgrade-workspace-toonew-log-search"></a>3 uppgradera arbetsytan toonew loggen Sök
Microsoft har publicerat en ny Log Analytics-frågespråket och i ordning tootake nytta av det, behöver du tooconvert din arbetsyta.  Om hello region finns i ditt arbetsområde har uppgraderats, bör du se en lila banderoll överst hello arbetsytans uppmana tooconvert. hello uppgraderingen är helt frivilligt och påverkar inte din erfarenhet av logganalys och några lösningar som du lägger till.  

För ytterligare information toounderstand hello fördelar, överväganden och processen tooupgrade finns [uppgraderar Azure logganalys toonew loggen Sök](log-analytics-log-search-upgrade.md).  

## <a name="4-add-solutions-and-solution-offerings"></a>4 Lägg till lösningar och erbjudanden för lösningar

Lägg sedan till hanteringslösningar och erbjudanden för lösningar. Hanteringslösningar är en samling logik, visualisering och datahämtningsregler som tillhandahåller mått som pivoteras runt ett särskilt problematisk område. Ett erbjudande för lösningar är ett paket med hanteringslösningar.

Lägger till lösningar tooyour arbetsytan kan logganalys toocollect olika typer av data från datorer som är anslutna tooyour arbetsytan använder agenter. Vi täcker publicering av agenter senare.

### <a name="tooadd-solutions-and-solution-offerings"></a>tooadd lösningar och lösningen erbjudanden

1. I Azure-portalen klickar du på **ny** och klicka sedan på hello **Sök hello marketplace** skriver **aktivitet logganalys** och tryck sedan på RETUR.
2. Hej allt i bladet väljer **aktivitet logganalys** och klicka sedan på **skapa**.  
    ![Activity Log Analytics](./media/log-analytics-get-started/activity-log-analytics.png)  
3. I hello *management lösningsnamn* bladet Välj en arbetsyta som du vill tooassociate med hello hanteringslösning.
4. Klicka på **Skapa**.  
    ![lösningens arbetsyta](./media/log-analytics-get-started/solution-workspace.png)  
5. Upprepa steg 1 – 4 tooadd:
    - Hej **säkerhet och efterlevnad** tjänsterbjudande med hello bedömning av program mot skadlig kod och säkerhet och granska lösningar.
    - Hej **Automation- och kontrollservern** tjänsterbjudande med hello Automation Hybrid Worker ändringsspårning och utvärdering av systemet uppdateringar (kallas även för uppdateringshantering) lösningar. Du måste skapa ett Automation-konto när du lägger till hello lösningen-erbjudandet.  
        ![Automation-konto](./media/log-analytics-get-started/automation-account.png)  
6. Du kan visa hello hanteringslösningar som du har lagt till tooyour arbetsytan genom att navigera för**logganalys** > **prenumerationer** > ***Arbetsytenamn***  >  **Översikt**. Paneler för hello hanteringslösningar som du har lagt till visas.  
    >[!NOTE]
    >Eftersom vi inte har anslutit alla agenter toohello arbetsytan ännu ser du inte några data för hello lösningar som du har lagt till.  

    ![lösningspaneler utan data](./media/log-analytics-get-started/solutions-no-data.png)

## <a name="4-create-a-vm-and-onboard-an-agent"></a>4 Skapa en virtuell dator och publicera en agent

Härnäst skapar du en enkel virtuell dator i Azure. När du har skapat en virtuell dator, publicera hello OMS-agenten tooenable den. Aktiverar hello agenten börjar datainsamlingen från hello VM och skickar data tooLog Analytics.

### <a name="toocreate-a-virtual-machine"></a>toocreate en virtuell dator

- Följ hello riktningar i [skapa din första Windows virtuell dator i hello Azure-portalen](../virtual-machines/virtual-machines-windows-hero-tutorial.md) och starta hello nya virtuella datorn.

### <a name="connect-hello-virtual-machine-toolog-analytics"></a>Ansluta hello virtuella tooLog Analytics

- Följ hello riktningar i [ansluta Azure virtuella datorer tooLog Analytics](log-analytics-azure-vm-extension.md) tooconnect hello VM tooLog Analytics med hjälp av hello Azure-portalen.

## <a name="6-view-and-act-on-data"></a>6 Se och vidta åtgärder för data

Tidigare aktiverat hello aktivitet logganalys lösningen och hello säkerhet och efterlevnad och automatisering och kontroll Tjänsterbjudanden. Härnäst börjar vi titta på de data som samlas in av lösningar och resultat i loggsökningar.

toostart titt på data som visas från inom lösningar. Titta sedan på några loggsökningar som kan nås från loggsökningar. Loggen sökningar kan du toocombine och korrelera datorn data från flera källor i din miljö. Mer information finns i [logga sökningar i logganalys](log-analytics-log-searches.md) eller om du har konverterat din arbetsyta toohello nya frågespråket finns [förstå loggen söker i logganalys](log-analytics-log-search-new.md). 

### <a name="tooview-antimalware-data"></a>tooview data för program mot skadlig kod

1. I hello Azure portal, navigerar för**logganalys** > ***arbetsytan***.
2. Hello-bladet för din arbetsyta under **allmänna**, klickar du på **översikt**.  
    ![Översikt](./media/log-analytics-get-started/overview.png)
3. Klicka på hello **program mot skadlig kod Assessment** panelen. I det här exemplet kan du se att Windows Defender är installerat på en dator, men att dess signatur är inaktuell.  
    ![Programvara mot skadlig kod](./media/log-analytics-get-started/solution-antimalware.png)
4. I det här exemplet under **skyddsstatus**, klickar du på **signatur inaktuell** tooopen loggen Sök och visa information om hello-datorer som har löpt ut signaturer. I det här exemplet Observera hello datorn heter *getstarted*. Om det fanns mer än en dator med inaktuella signaturer, visas de alla i hello loggen Sök resultat.  
    ![Program mot skadlig kod är inaktuellt](./media/log-analytics-get-started/antimalware-search.png)

### <a name="tooview-security-and-audit-data"></a>tooview säkerhet och granska data

1. Hello-bladet för din arbetsyta under **allmänna**, klickar du på **översikt**.  
2. Klicka på hello **säkerhet och granska** panelen. I det här exemplet kan du se att det finns två viktiga problem: en dator saknar viktiga uppdateringar och en dator har otillräckligt skydd.  
    ![Säkerhet och granskning](./media/log-analytics-get-started/security-audit.png)
3. I det här exemplet under **anmärkningsvärda problem**, klickar du på **datorer som saknar kritiska uppdateringar** tooopen loggen Sök och visa information om datorer som saknar kritiska uppdateringar. I det här exemplet saknas en viktig uppdatering och 63 andra uppdateringar.  
    ![Loggsökning för säkerhet och granskning](./media/log-analytics-get-started/security-audit-log-search.png)

### <a name="tooview-and-act-on-system-update-data"></a>tooview och agera på systemuppdatering data

1. Hello-bladet för din arbetsyta under **allmänna**, klickar du på **översikt**.  
2. Klicka på hello **System Update Assessment** panelen. I det här exemplet kan du se att en Windows-dator med namnet *getstarted* saknar viktiga uppdateringar och en saknar definitionsuppdateringar.  
    ![Systemuppdateringar](./media/log-analytics-get-started/system-updates.png)
3. I det här exemplet under **saknas uppdateringar**, klickar du på **kritiska uppdateringar** tooopen loggen Sök och visa information om datorer som saknar kritiska uppdateringar. I det här exemplet saknas en uppdatering och det finns en nödvändig uppdatering.  
    ![Systemuppdateringar för loggsökning](./media/log-analytics-get-started/system-updates-log-search.png)
4. Gå toohello [Operations Management Suite](http://microsoft.com/oms) webbplatsen och logga in med ditt Azure-konto. När du är inloggad Observera hello lösningsinformationen liknande toowhat som du har finns i hello Azure-portalen.  
    ![OMS-portalen](./media/log-analytics-get-started/oms-portal.png)
5. Klicka på hello **uppdateringshantering** panelen.
6. Observera att hello Systeminformation uppdatering är liknande toohello att uppdatera information du har sett i hello Azure-portalen i instrumentpanelen för hello uppdatera hantering. Dock hello **hantera distributioner** panelen är ny. Klicka på hello **hantera distributioner** panelen.  
    ![ikonen Hantering av uppdateringar](./media/log-analytics-get-started/update-management.png)
7. På hello **uppdatera distributioner** klickar du på **Lägg till** toocreate en *uppdateringskörningen*.  
    ![Distributioner av uppdateringar](./media/log-analytics-get-started/update-management-update-deployments.png)
8.  På hello **nya uppdatera distributionen** sidan, Skriv ett namn för distribution av hello, Välj datorer tooupdate (i det här exemplet *getstarted*), väljer ett schema och klicka sedan på **spara**.  
    ![Ny distribution](./media/log-analytics-get-started/new-deployment.png)  
    När du har sparat hello distribution visas hello schemalagd uppdatering.  
    ![schemalagd uppdatering](./media/log-analytics-get-started/scheduled-update.png)  
    När hello uppdateringskörningen har slutförts hello status visas **avslutad**.
    ![färdig uppdatering](./media/log-analytics-get-started/completed-update.png)
9. När hello uppdateringskörningen har slutförts kan visa du om hello kör lyckades eller inte och du kan visa information om vilka uppdateringar tillämpas.

## <a name="after-evaluation"></a>Efter utvärdering

I den här självstudien installerade du en agent på en virtuell dator och kom igång snabbt. hello steg som du har följt har snabb och enkel. Men de flesta stora organisationer och företag har komplexa lokala IT-infrastrukturer. Så tar att samla in data från dessa komplexa miljöer ytterligare planering och arbete än hello kursen. Studera hello hello följa stegen nedan för länkar toohelpful artiklar.

Alternativt kan du ta bort hello arbetsytan som du skapade med den här kursen.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om hur du ansluter [Windows-agenter](log-analytics-windows-agents.md) tooLog Analytics.
* Lär dig mer om hur du ansluter [Operations Manager-agenter](log-analytics-om-agents.md) tooLog Analytics.
* [Lägg till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md) tooadd funktioner och samla data.
* Bekanta dig med [logga sökningar](log-analytics-log-searches.md) tooview detaljerad information som samlas in av lösningar.
