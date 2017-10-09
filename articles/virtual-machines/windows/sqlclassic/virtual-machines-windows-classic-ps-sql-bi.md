---
title: aaaSQL Server Business Intelligence | Microsoft Docs
description: "Det här avsnittet använder resurser som har skapats med hello klassiska distributionsmodellen och beskriver hello Business Intelligence (BI)-funktioner som är tillgängliga för SQL Server som körs på Azure Virtual Machines (virtuella datorer)."
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: c681e7a7-eeda-48aa-bc35-6277f4828244
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/30/2017
ms.author: asaxton
ms.openlocfilehash: e3288f0835d6c4a19baeeea5f6b65fec16cd751f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>SQL Server Business Intelligence på Azure Virtuella datorer
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

hello virtuell dator i Microsoft Azure-galleriet innehåller bilder som innehåller SQL Server-installationer. hello SQLServer-versioner stöds i hello galleriavbildningar är hello samma installationsfiler som du kan installera tooon lokala datorer och virtuella datorer. Det här avsnittet sammanfattas hello SQL Server Business Intelligence (BI) funktioner installerade på hello avbildningar och konfigurationssteg som krävs när en virtuell dator har etablerats. Det här avsnittet beskriver också distributionstopologier som stöds för BI-funktionerna och bästa praxis.

## <a name="license-considerations"></a>Licens-överväganden
Det finns två sätt toolicense SQL Server i Microsoft Azure-datorer:

1. Licens mobility fördelar som ingår i Software Assurance. Mer information finns i [Licensmobilitet via Software Assurance på Azure](https://azure.microsoft.com/pricing/license-mobility/).
2. Betala per timme av Azure virtuella datorer med SQL Server installerad. Se hello ”SQL Server” i avsnittet [prissättning för Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Mer information om licensiering och aktuella kurser finns [virtuella datorer vanliga frågor om licensiering](https://azure.microsoft.com/pricing/licensing-faq/%20/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server-avbildningar finns i Azure galleriet för virtuella datorer
hello virtuell dator i Microsoft Azure-galleriet innehåller flera avbildningar som innehåller Microsoft SQL Server. hello-programvaran på hello virtuella datoravbildningar varierar beroende på hello av hello operativsystemet och hello versionen av SQL Server. hello lista över tillgängliga avbildningarna hello virtuell dator i Azure-galleriet ändras ofta.

<!--![SQL image in azure VM gallery](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)-->
![SQL-avbildningen i Virtuella Azure-galleriet](./media/virtual-machines-windows-classic-ps-sql-bi/vm-sql-images.png)

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) hello returnerar följande PowerShell-skript hello lista över Azure-avbildningar som innehåller ”SQL Server” i hello avbildning:

    # assumes you have already uploaded a management certificate tooyour Microsoft Azure Subscription. View hello thumbprint value from hello "Subscriptions" menu in Azure portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is hello one specified with hello -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Mer information om versioner och funktioner som stöds av SQL Server finns i hello följande:

* [SQL Server-versioner](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)
* [Funktioner som stöds av hello utgåvor av SQL Server 2016](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-hello-sql-server-virtual-machine-gallery-images"></a>BI-funktioner installerade på hello Galleriavbildningar för SQL Server virtuell dator
hello sammanfattas följande tabell hello Business Intelligence-funktioner installerade på galleriavbildningar hello vanliga Microsoft Azure-dator för SQL Server:

* SQL Server 2016 SP1 Enterprise
* SQL Server 2016 SP1 Standard
* SQL Server 2014 SP2 Enterprise
* SQL Server 2014 SP2 Standard
* SQL Server 2012 SP3 Enterprise
* SQL Server 2012 SP3 Standard

| Funktionen för SQL Server BI | Installerad på bild för hello-galleriet | Anteckningar |
| --- | --- | --- |
| **Reporting Services enhetligt läge** |Ja |Installerats men måste konfigurationen, inklusive hello report manager URL: en. Avsnittet hello [konfigurera Reporting Services](#configure-reporting-services). |
| **Reporting Services SharePoint-läge** |Nej |hello bild för virtuell dator i Microsoft Azure-galleriet inte innehåller SharePoint eller SharePoint installationsfilerna. <sup>1</sup> |
| **Analysis Services flerdimensionella och Data utvinningsmodellen (OLAP)** |Ja |Hello installerat och konfigurerat som standard Analysis Services-instans |
| **Analysis Services-tabell** |Nej |Stöds i SQL Server 2012, installeras 2014 2016 bilder, men den inte som standard. Installera en annan instans av Analysis Services. Hello i avsnittet installera andra SQL Server-tjänster och funktioner i det här avsnittet. |
| **Analysis Services Power Pivot för SharePoint** |Nej |hello bild för virtuell dator i Microsoft Azure-galleriet inte innehåller SharePoint eller SharePoint installationsfilerna. <sup>1</sup> |

<sup>1</sup> ytterligare information om Azure virtuella datorer och SharePoint finns [Microsoft Azure arkitekturer för SharePoint 2013](https://technet.microsoft.com/library/dn635309.aspx) och [SharePoint-distributionen på Microsoft Azure Virtual Machines](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Kör följande PowerShell-kommandot tooget hello en lista över installerade tjänster som innehåller ”SQL” i hello tjänstnamn.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Allmänna rekommendationer och bästa praxis
* hello minsta rekommenderade storleken för en virtuell dator är **A3** när du använder SQL Server Enterprise Edition. Hej **A4** storlek för virtuell dator rekommenderas för SQL Server BI distributioner av Analysis Services och Reporting Services.
  
    Mer information om hello aktuella VM-storlekar finns [storlekar för virtuella datorer för Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Bästa praxis för Diskhantering är toostore data och loggfilen säkerhetskopior på enheter än **C**: och **D**:. Till exempel skapa datadiskar **E**: och **F**:.
  
  * hello enhet princip för hello standardenheten för cachelagring av **C**: är inte optimalt för att arbeta med data.
  * Hej **D**: enheten är en temporär enhet som används främst för hello växlingsfilen. Hej **D**: enheten är inte beständiga och sparas inte i blob storage. Administrationsuppgifter som en storlek på virtuell dator ändra toohello återställa hello **D**: enheten. Det rekommenderas för**inte** använda hello **D**: enheten för databasfilerna, inklusive tempdb.
    
    Mer information om hur du skapar och bifogar diskar finns [hur tooAttach en datadisk tooa virtuella](../classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Stoppa eller avinstallera tjänster som du inte planerar toouse. För exempel om hello virtuella används endast för Reporting Services, stoppar eller avinstallerar Analysis Services och SQL Server Integration Services. hello är följande bild ett exempel på hello-tjänster som startas som standard.
  
    ![SQL Server-tjänster](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)
  
  > [!NOTE]
  > hello SQL Server-databasmotorn krävs i stöds hello BI scenarier. I en enskild server VM-topologi hello databasmotorn krävs toobe som körs på hello samma virtuella dator.
  
    Mer information finns i följande hello: [avinstallera Reporting Services](https://msdn.microsoft.com/library/hh479745.aspx) och [avinstallera en instans av Analysis Services](https://msdn.microsoft.com/library/ms143687.aspx).
* Kontrollera **Windows Update** för nya 'viktiga uppdateringar ”. hello Microsoft Azure-dator bilder uppdateras ofta; dock viktiga uppdateringar bli tillgängliga från **Windows Update** när hello VM-avbildning senast uppdaterades.

## <a name="example-deployment-topologies"></a>Exempel distributionstopologier
hello följande är exempeldistributioner som använder Microsoft Azure-datorer. hello topologier i dessa diagram är endast en del av hello möjliga topologier som du kan använda med SQL Server BI funktioner och virtuella datorer i Microsoft Azure.

### <a name="single-virtual-machine"></a>Enskild virtuell dator
Analysis Services, Reporting Services, SQL Server Database Engine och datakällor på en enskild virtuell dator.

![BI IAS-scenario med 1 virtuell dator](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Två virtuella datorer
* Analysis Services Reporting Services och hello databasmotor för SQL Server på en enskild virtuell dator. Den här distributionen innehåller hello report server-databaser.
* Datakällor på en andra virtuell dator. hello innehåller andra VM SQL Server Database Engine som en datakälla.

![BI iaas-scenario med 2 virtuella datorer](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Blandat Azure – data på Azure SQL-databas
* Analysis Services Reporting Services och hello databasmotor för SQL Server på en enskild virtuell dator. Den här distributionen innehåller hello report server-databaser.
* Datakällan är Azure SQL-databas.

![BI iaas scenarier vm och AzureSQL som datakälla](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hybrid – data lokalt
* I detta exempel på distribution köras Analysis Services och Reporting Services hello SQL Server Database Engine på en enskild virtuell dator. hello virtuella värdar hello report server-databaser. hello virtuella datorn är domänansluten tooan lokala domänen med hjälp av Azure virtuella nätverk eller några andra VPN-tunnel lösning.
* Datakällan är lokalt.

![BI iaas scenarier virtuella och lokala datakällor](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>Reporting Services-konfiguration för enhetligt läge
Bild av hello virtuella galleriet för SQL Server innehåller Reporting Services enhetligt läge installerat, men hello rapportservern inte har konfigurerats. hello stegen i det här avsnittet Konfigurera hello Reporting Services-rapportservern. Mer detaljerad information om hur du konfigurerar Reporting Services enhetligt läge finns [installera Reporting Services enhetligt läge Report Server (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

> [!NOTE]
> Liknande innehåll som använder Windows PowerShell-skript tooconfigure hello rapportservern finns [Använd PowerShell tooCreate en Azure VM med ett enhetligt läge rapportserver](../classic/ps-sql-report.md).

### <a name="connect-toohello-virtual-machine-and-start-hello-reporting-services-configuration-manager"></a>Anslut toohello virtuella datorn och starta hello Konfigurationshanteraren för Reporting Services
Det finns två vanliga arbetsflöden för att ansluta tooan Azure-dator:

* tooconnect i hello, klickar du på hello hello virtuella datorns namn och klicka sedan på **Anslut**. En anslutning till fjärrskrivbord öppnas och hello datornamn fylls i automatiskt.
  
    ![ansluta tooazure virtuell dator](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)
* Ansluta toohello virtuell dator med anslutning till fjärrskrivbord. I användargränssnittet för hello av hello fjärrskrivbord:
  
  1. Typen hello **molntjänstnamnet** som hello datornamn.
  2. Skriv kolon (:) och hello offentliga portnumret som har konfigurerats för hello TCP skrivbord fjärrslutpunkten.
     
      Myservice.cloudapp.NET:63133
     
      Mer information finns i [vad är en molnbaserad tjänst?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).


**Starta Reporting Services Configuration Manager**

I **Windows Server 2012/2016**:

1. Från hello **starta** skriver **Reporting Services** toosee en lista över appar.
2. Högerklicka på **Reporting Services Configuration Manager** och på **kör som administratör**.

I **Windows Server 2008 R2**:

1. Klicka på **starta**, och klicka sedan på **alla program**.
2. Klicka på **Microsoft SQL Server 2016**.
3. Klicka på **konfigurationsverktyg**.
4. Högerklicka på **Reporting Services Configuration Manager** och på **kör som administratör**.

Eller:

1. Klicka på **starta**.
2. I hello **Sök program och filer** dialogrutan typen **reporting services**. Om hello VM kör Windows Server 2012, Skriv **reporting services** på Windows Server 2012 starta hello-skärmen.
3. Högerklicka på **Reporting Services Configuration Manager** och på **kör som administratör**.
   
    ![Sök efter ssrs configuration manager](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Konfigurera Reporting Services
**Tjänstkonto och webbtjänst-URL:**

1. Kontrollera hello **servernamn** är hello lokala servernamnet och klickar på **Anslut**.
2. Obs hello tomt **Report Server-databasnamn**. hello databasen skapas när hello konfigurationen är klar.
3. Kontrollera hello **Report Server Status** är **igång**. Om du vill att tooverify hello-tjänsten i Windows Server Manager hello service är en hello **SQL Server Reporting Services** Windows-tjänst.
4. Klicka på **tjänstkonto** och ändra hello kontot efter behov. Om virtuella hello används i en icke-domänanslutna domänmiljö hello inbyggda **ReportServer** kontot är tillräckliga. Mer information om hello-tjänstkontot finns [tjänstkonto](https://msdn.microsoft.com/library/ms189964.aspx).
5. Klicka på **URL för webbtjänsten** hello vänster.
6. Klicka på **tillämpa** tooconfigure hello standardvärden.
7. Obs hello **Report Server-tjänsten webbadresser**. Observera att hello TCP-standardporten är 80 och är en del av hello URL. I ett senare steg kan du skapa en Microsoft Azure Virtual Machine-slutpunkt för hello port.
8. I hello **resultat** fönstret Kontrollera hello åtgärder har slutförts.

**Databas:**

1. Klicka på **databasen** hello vänster.
2. Klicka på **ändra databasen**.
3. Kontrollera **skapa en ny rapportserverdatabas** är markerad och klicka sedan på Nästa.
4. Kontrollera **servernamn** och på **Testanslutningen**.
5. Om hello resultatet är **testet av anslutningen lyckades**, klickar du på **OK** och klicka sedan på **nästa**.
6. Obs hello databasnamnet är **ReportServer** och hello **Report Server mode** är **interna** Klicka **nästa**.
7. Klicka på **nästa** på hello **autentiseringsuppgifter** sidan.
8. Klicka på **nästa** på hello **sammanfattning** sidan.
9. Klicka på **nästa** på hello **utvecklas och slutför** sidan.

**Web Portal-URL eller URL för Report Manager för 2012 och 2014:**

1. Klicka på **URL för Web Portal**, eller **URL för Report Manager** för 2014 och 2012 hello vänster.
2. Klicka på **Använd**.
3. I hello **resultat** fönstret Kontrollera hello åtgärder har slutförts.
4. Klicka på **avsluta**.

Information om server-behörighet för rapporten finns [bevilja behörigheter på en rapportserver för enhetligt läge](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-toohello-local-report-manager"></a>Bläddra toohello lokala Report Manager
tooverify hello konfiguration, bläddra tooreport manager på hello VM.

1. Starta Internet Explorer med administratörsbehörighet på hello VM.
2. Bläddra toohttp://localhost/reports på hello VM.

### <a name="tooconnect-tooremote-web-portal-or-report-manager-for-2014-and-2012"></a>tooConnect tooRemote webbportal eller Report Manager för 2014 och 2012
Om du vill tooconnect toohello web-portalen eller Report Manager för 2014 och 2012 på hello virtuell dator från en fjärrdator, skapa en ny virtuell dator TCP-slutpunkt. Som standard hello rapportservern lyssnar efter HTTP-begäranden på **port 80**. Om du konfigurerar hello report server URL: er toouse en annan port måste du ange det portnumret i hello följa anvisningar.

1. Skapa en slutpunkt för hello virtuella av TCP-Port 80. Mer information finns i hello [virtuella slutpunkter och brandväggsportar](#virtual-machine-endpoints-and-firewall-ports) i det här dokumentet.
2. Öppna port 80 i brandväggen hello virtuella datorn.
3. Bläddra toohello webbportal eller Rapporthanteraren, med hjälp av Azure virtuella **DNS-namnet** som hello servernamnet i hello-URL. Exempel:
   
    **Rapportservern**: http://uebi.cloudapp.net/reportserver **webbportalen**: http://uebi.cloudapp.net/reports
   
    [Konfigurera en brandvägg för Report Server Access](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="toocreate-and-publish-reports-toohello-azure-virtual-machine"></a>tooCreate och publicera rapporter toohello Azure-dator
hello följande tabell visas några av hello alternativ tillgängliga toopublish befintliga rapporter från en lokal dator toohello rapportservern finns på hello Microsoft Azure-dator:

* **Report Builder**: hello virtuell dator innehåller hello klicka – en gång version av Microsoft SQL Server Report Builder för SQL 2014 och 2012. toostart Report builder hello första gången på hello virtuell dator med SQL 2016:
  
  1. Starta din webbläsare med administratörsbehörighet.
  2. Bläddra toohello webbportal på hello virtuella datorn och välj hello **hämta** ikon i hello övre högra hörnet.
  3. Välj **Report Builder**.
     
     Mer information finns i [starta Report Builder](https://msdn.microsoft.com/library/ms159221.aspx).
* **SQL Server Data Tools**: VM: SQL Server Data Tools är installerat på hello virtuell dator och kan vara används toocreate **Report Server-projekt** och rapporter på hello virtuella datorn. SQL Server Data Tools kan publicera rapporter hello toohello rapportservern på hello virtuella datorn.
* **SQL Server Data Tools: Remote**: skapa en Reporting Services-projekt i SQL Server Data Tools som innehåller Reporting Services-rapporter på den lokala datorn. Konfigurera hello projektet tooconnect toohello webbtjänst-URL.
  
    ![SSDT projektegenskaperna för SSRS-projekt](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)
* Skapa en. VHD-hårddisk som innehåller rapporter och sedan ladda upp och ansluta hello-enhet.
  
  1. Skapa en. VHD-hårddisk på den lokala datorn som innehåller dina rapporter.
  2. Skapa och installera ett certifikat.
  3. Överför hello VHD-filen tooAzure cmdleten hello Add-AzureVHD [skapa och ladda upp en Windows Server VHD-tooAzure](../classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
  4. Koppla hello disk toohello virtuella datorn.

## <a name="install-other-sql-server-services-and-features"></a>Installera andra SQL Server-tjänster och funktioner
tooinstall ytterligare SQL Server-tjänster, till exempel Analysis Services i tabelläge måste köra installationsguiden för hello SQL server. hello installationsfilerna finns på hello virtuella datorns hårddisk.

1. Klicka på **starta** och klicka sedan på **alla program**.
2. Klicka på **Microsoft SQL Server 2016**, **Microsoft SQL Server 2014** eller **Microsoft SQL Server 2012** och klicka sedan på **konfigurationsverktyg** .
3. Klicka på **SQL Server Installationscenter**.

Eller kör C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe eller C:\SQLServer_11.0_full\setup.exe

> [!NOTE]
> hello första gången du kör installationen av SQL Server installeras filer kan hämtas och kräver en omstart av hello virtuella datorn och startar om installationen av SQL Server.
> 
> Om du behöver toorepeatedly anpassa hello bild vald från hello Microsoft Azure-dator kan du överväga att skapa en egen SQL Server-avbildning. Analysis Services SysPrep-funktioner har aktiverats med SQL Server 2012 SP1 CU2. Mer information finns i [överväganden för installation av SQL Server med hjälp av SysPrep](https://msdn.microsoft.com/library/ee210754.aspx) och [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).
> 
> 

### <a name="tooinstall-analysis-services-tabular-mode"></a>tooInstall tabelläge för Analysis Services
hello stegen i det här avsnittet **sammanfatta** hello installation av Analysis Services tabelläge. Mer information finns i hello följande:

* [Installera Analysis Services i tabelläge](https://msdn.microsoft.com/library/hh231722.aspx)
* [Tabell modellering (Adventure Works självstudier)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**tooInstall tabelläge för Analysis Services:**

1. I installationsguiden för hello SQL Server, klickar du på **Installation** i hello till vänster och klicka sedan på **fristående ny SQL server-installation eller Lägg till en befintlig installation av funktioner tooan**.
   
   * Om du ser hello **Bläddra efter mapp**Bläddra tooc:\SQLServer_13.0_full, c:\SQLServer_12.0_full eller c:\SQLServer_11.0_full och klicka sedan på **Ok**.
2. Klicka på **nästa** på hello uppdateringar produktsidan.
3. På hello **installationstyp** väljer **utför en ny installation av SQL Server** och på **nästa**.
4. På hello **konfigurera roll** klickar du på **Installation av SQL Server-funktioner**.
5. På hello **Funktionsurval** klickar du på **Analysis Services**.
6. På hello **instanskonfiguration** Skriv ett beskrivande namn, **Tabellmetadata** till **namngivna instansen** och **instans-Id** text rutor.
7. På hello **Analysis Services-konfiguration** väljer **tabelläge**. Lägg till hello aktuella toohello administratörsbehörighet användarlistan.
8. Slutför och Stäng hello installationsguiden för SQL Server.

## <a name="analysis-services-configuration"></a>Analysis Services-konfiguration
### <a name="remote-access-tooanalysis-services-server"></a>Remote Access tooAnalysis Services-Server
Analysis Services-server stöder endast windows-autentisering. tooaccess Analysis Services via en fjärranslutning från klientprogram, till exempel SQL Server Management Studio eller SQL Server Data Tools hello virtuella datorn måste toobe kopplade tooyour lokala domänen med hjälp av Azure virtuella nätverk. Mer information finns i [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md).

En **standardinstansen** av Analysis Services som lyssnar på TCP-port **2383**. Öppna hello porten i brandväggen för hello virtuella datorer. En klustrad namngivna instansen av Analysis Services också lyssnar på port **2383**.

För en **namngivna instansen** av Analysis Services hello SQL Server Browser-tjänsten krävs toomanage port åtkomst. hello standardkonfigurationen för SQL Server Browser är port **2382**.

Öppna port i brandväggen för hello virtuella datorer, **2382** och skapa en statisk Analysis Services namngivna instansen port.

1. tooverify portar som redan finns i använda på hello VM och vilken process som använder hello portar, kör följande kommando med administratörsbehörighet hello:
   
        netstat /ao
2. Använd SQL Server Management Studio toocreate en statisk Analysis Services med namnet instans porten genom att uppdatera ”Port” värdet i tabellform AS instansen allmänna egenskaper. Mer information finns i hello ”använda en fast port för ett standardvärde eller namngivna instansen” i [konfigurera hello Windows-brandväggen tooAllow Analysis Services-åtkomst](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).
3. Hello tabular instansen hello Analysis Services-tjänsten startas om.

Mer information finns i hello **virtuella slutpunkter och brandväggsportar** i det här dokumentet.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Virtuella slutpunkter och portar i brandväggen
Det här avsnittet sammanfattas Microsoft Azure virtuella slutpunkter toocreate och portar tooopen i hello virtuella brandväggar. hello följande tabell sammanfattas hello **TCP** portarna toocreate slutpunkter för och tooopen för hello-portar i brandväggen för hello virtuella datorer.

* Om du använder en enda virtuell dator och hello följande poster är uppfyllda, behöver du inte toocreate VM slutpunkter och du behöver inte tooopen hello portar i brandväggen hello på hello VM.
  
  * Du inte att fjärransluta toohello SQL Server-funktioner på hello VM. Upprätta en anslutning till fjärrskrivbord toohello VM och åtkomst till SQL Server-funktioner för hello lokalt på hello VM anses inte vara en anslutning till toohello SQL Server-funktioner.
  * Du ansluta inte hello VM tooan lokala domänen med hjälp av Azure virtuella nätverk eller en annan VPN-tunnlar lösning.
* Om hello virtuella datorn inte är domänansluten tooa domän, men du vill ansluta tooremotely toohello SQL Server-funktioner på den virtuella datorn:
  
  * Öppna hello portar i brandväggen hello på hello VM.
  * Skapa virtuella slutpunkter för hello anges portar (*).
* Hello slutpunkter är inte obligatoriska om hello virtuella datorn är domänansluten tooa domänen med en VPN-tunnel som virtuella Azure-nätverk. Men öppna hello portar i brandväggen hello på hello VM.
  
  | Port | Typ | Beskrivning |
  | --- | --- | --- |
  | **80** |TCP |Rapportserver fjärråtkomst (*). |
  | **1433** |TCP |SQL Server Management Studio (*). |
  | **1434** |UDP |SQL Server Browser. Detta krävs när hello VM i kopplade tooa domän. |
  | **2382** |TCP |SQL Server Browser. |
  | **2383** |TCP |Standardinstansen för SQL Server Analysis Services och klustrade namngivna instanser. |
  | **Användardefinierade** |TCP |Skapa en statisk Analysis Services namngiven instans port efter ett portnummer som du väljer och avblockera hello portnumret i hello-brandväggen. |

Mer information om hur du skapar slutpunkter finns hello följande:

* Skapa slutpunkter:[hur tooSet in slutpunkter tooa virtuella](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* SQLServer: Avsnittet hello ”hela konfigurationen steg tooconnect toohello virtuell dator med hjälp av SQL Server Management Studio” i [etablering av en SQL Server-dator på Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md).

hello följande diagram illustrerar hello portar tooopen i hello VM brandväggen tooallow fjärråtkomst toofeatures och komponenter på hello VM.

![portar tooopen för bi program i virtuella Azure-datorer](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Resurser
* Granska hello Supportpolicy för Microsoft server-programvara som används i hello Azure virtuella miljö. följande ämne hello sammanfattas stöd för funktioner som BitLocker, redundanskluster och Utjämning av nätverksbelastning. [Support för Microsoft server-programvara för Azure Virtual Machines](http://support.microsoft.com/kb/2721672).
* [SQLServer på Azure virtuella datorer – översikt](../sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Virtual Machines](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Etablering av en SQL Server-dator på Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Hur tooAttach en datadisk tooa virtuell dator](../classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Migrera en databas tooSQL Server på en Azure VM](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json)
* [Fastställa hello serverläge för en Analysis Services-instans](https://msdn.microsoft.com/library/gg471594.aspx)
* [Flerdimensionella modellering (Adventure Works självstudier)](https://technet.microsoft.com/library/ms170208.aspx)
* [Azure Documentation Center](https://azure.microsoft.com/documentation/)
* [Med Powerbi i en Hybridmiljö](https://msdn.microsoft.com/library/dn798994.aspx)

> [!NOTE]
> [Skicka feedback och kontaktuppgifter genom att ansluta till Microsoft SQL Server](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Community-innehåll
* [Hantering av Azure SQL-databas med PowerShell](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)

