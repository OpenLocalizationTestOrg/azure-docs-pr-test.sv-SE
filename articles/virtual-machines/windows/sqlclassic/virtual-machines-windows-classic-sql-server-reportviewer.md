---
title: "aaaUse ReportViewer på en webbplats | Microsoft Docs"
description: "Det här avsnittet beskrivs hur toobuild en Microsoft Azure-webbplats med hello Visual Studio ReportViewer-kontroll som visar en rapport lagras på en Microsoft Azure-dator."
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 78b76318-d9bf-48ef-9d9e-d1b7d8cf3042
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: asaxton
ms.openlocfilehash: 8e0729d6657f96c32a9ac7dffdff7745ff92b48d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-reportviewer-in-a-web-site-hosted-in-azure"></a>Använd ReportViewer på en webbplats i Azure
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Du kan skapa en Microsoft Azure-webbplats med hello Visual Studio ReportViewer-kontroll som visar en rapport som lagras på en Microsoft Azure-dator. hello ReportViewer-kontrollen är i ett webbprogram som du skapar med hello programmall för ASP.NET-webbprogram.

> [!IMPORTANT]
> hello ASP.NET MVC-Webbapp mallar stöder inte hello ReportViewer-kontrollen.

tooincorporate ReportViewer till Microsoft Azure-webbplatsen måste toocomplete hello följande uppgifter.

* **Lägg till** sammansättningar toohello distributionspaket
* **Konfigurera** autentisering och auktorisering
* **Publicera** hello ASP.NET Web application tooAzure

## <a name="prerequisites"></a>Krav
Granska hello ”allmänna rekommendationer och bästa praxis” i avsnittet [SQL Server Business Intelligence i Azure Virtual Machines](../classic/ps-sql-bi.md).

> [!NOTE]
> Kontroller för ReportViewer levereras med Visual Studio, Standard Edition eller senare. Om du använder hello Web Developer Express Edition, måste du installera hello [MICROSOFT REPORT VIEWER 2012 RUNTIME](https://www.microsoft.com/download/details.aspx?id=35747) toouse hello ReportViewer runtime funktioner.
> 
> ReportViewer som konfigurerats i lokalt läge stöds inte i Microsoft Azure.

## <a name="adding-assemblies-toohello-deployment-package"></a>Lägga till sammansättningar toohello distributionspaket
När du är värd för ASP.NET-program lokalt, hello ReportViewer sammansättningar installeras vanligtvis direkt i hello globala sammansättningscachen (GAC) hello IIS-servern under installationen av Visual Studio och kan nås direkt av programmet hello. Men när värden ASP.NET-programmet i hello moln, Microsoft Azure inte tillåter något toobe installerats i hello GAC, så måste du kontrollera finns hello ReportViewer sammansättningar lokalt i ditt program. Du kan göra detta genom att lägga till referenser toothem i ditt projekt och konfigurera dem toobe kopieras lokalt.

Hello ReportViewer-kontrollen använder hello följande sammansättningar i fjärrhantering läge:

* **Microsoft.ReportViewer.WebForms.dll**: innehåller hello ReportViewer-kod som du behöver toouse ReportViewer på sidan. En referens för den här sammansättningen läggs tooyour projekt när du släpper en ReportViewer-kontrollen på en ASP.NET-sida i projektet.
* **Microsoft.ReportViewer.Common.dll**: innehåller klasser som används av hello ReportViewer-kontrollens vid körning. Det läggs inte automatiskt tooyour projekt.

### <a name="tooadd-a-reference-toomicrosoftreportviewercommon"></a>tooadd en referens tooMicrosoft.ReportViewer.Common
* Högerklicka på ditt projekt **referenser** och välj **Lägg till referens**väljer hello sammansättningen hello .NET-fliken och klicka på **OK**.

### <a name="toomake-hello-assemblies-locally-accessible-by-your-aspnet-application"></a>toomake hello-sammansättningar lokalt nås av ASP.NET-program
1. I hello **referenser** mappen, klicka på hello Microsoft.ReportViewer.Common sammansättningen så att dess egenskaper visas i egenskapsrutan hello.
2. Ställ i fönstret Egenskaper hello **kopiera lokala** tooTrue.
3. Upprepa steg 1 och 2 för Microsoft.ReportViewer.WebForms.

### <a name="tooget-reportviewer-language-pack"></a>tooget ReportViewer Language Pack
1. Installera hello lämplig Microsoft Report Viewer 2012 Runtime redistributable package från [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=317386).
2. Välj hello språk från hello sidan och hello listrutan hämtar omdirigerade toohello motsvarande center hämtningssidan.
3. Klicka på **hämta** toostart hello hämtning av ReportViewerLP.exe.
4. När du har hämtat ReportViewerLP.exe klickar du på **kör** tooinstall omedelbart, eller klicka på **spara** toosave den tooyour dator. Om du klickar på **spara**, Kom ihåg hello namnet på hello mappen där du sparar hello-fil.
5. Hitta hello mappen där du sparade hello-filen. Högerklicka på ReportViewerLP.exe, klickar du på **kör som administratör**, och klicka sedan på **Ja**.
6. När du har kört ReportViewerLP.exe ser du hello c:\windows\assembly har hello resursfiler **Microsoft.ReportViewer.Webforms.Resources** och **Microsoft.ReportViewer.Common.Resources** .

### <a name="tooconfigure-for-localized-reportviewer-control"></a>tooconfigure för lokaliserade ReportViewer-kontrollen
1. Hämta och installera hello Microsoft Report Viewer 2012 Runtime omdistribuerbara paketet av följande hello ovan angivna instruktioner.
2. Skapa <language> mapp i hello projekt och kopiera hello associerade sammansättningen resursfiler det. hello resurs sammansättningen filer toobe kopieras är: **Microsoft.ReportViewer.Webforms.Resources.dll** och **Microsoft.ReportViewer.Common.Resources.dll**. Välj hello resursfiler sammansättningen och i hello egenskaper, ange **kopiera tooOutput Directory** för ”**Kopiera alltid**”.
3. Ange hello kultur & UICulture för hello webbprojekt. Mer information om hur tooset hello kultur och kultur för Användargränssnittet för en ASP.NET-webbsida, se [så här: Ange hello kultur och kultur för Användargränssnittet för ASP.NET-webbsida globalisering](http://go.microsoft.com/fwlink/?LinkId=237461).

## <a name="configuring-authentication-and-authorization"></a>Konfigurera autentisering och auktorisering
hello ReportViewer måste toouse rätt autentiseringsuppgifter tooauthenticate hello rapportservern och hello autentiseringsuppgifterna måste godkännas av hello tooaccess hello rapporter i report server du vill använda. Information om autentisering finns i faktabladet för hello [Reporting Services report viewer kontroll och Microsoft Azure virtuell dator baserat rapportservrar](https://msdn.microsoft.com/library/azure/dn753698.aspx).

## <a name="publish-hello-aspnet-web-application-tooazure"></a>Publicera hello ASP.NET Web application tooAzure
Anvisningar om hur du publicerar ett ASP.NET Web application tooAzure finns [så här: migrera och publicera en webbprogrammet tooAzure från Visual Studio](../../../vs-azure-tools-migrate-publish-web-app-to-cloud-service.md) och [Kom igång med Web Apps och ASP.NET](../../../app-service-web/app-service-web-get-started-dotnet.md).

> [!IMPORTANT]
> Om hello kommandot Lägg till Azure distributionsprojekt eller Lägg till Azure Molntjänstprojekt inte visas i hello snabbmenyn i Solution Explorer, kan du behöva toochange hello Målversionen av framework hello projektet too.NET Framework 4.
> 
> hello två kommandon är i stort sett hello samma funktioner. En eller hello andra kommandot kommer att visas i snabbmenyn hello beroende på vilken version av hello Microsoft Azure SDK om du har installerat.
> 
> 

## <a name="resources"></a>Resurser
[Microsoft-rapporter](http://go.microsoft.com/fwlink/?LinkId=205399)

[SQL Server Business Intelligence i virtuella Azure-datorer](../classic/ps-sql-bi.md)

[Använd PowerShell tooCreate en Azure VM med ett enhetligt läge Report Server](../classic/ps-sql-report.md)

