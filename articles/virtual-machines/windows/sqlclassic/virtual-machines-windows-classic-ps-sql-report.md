---
title: "aaaUse PowerShell tooCreate en VM med ett enhetligt läge rapportserver | Microsoft Docs"
description: "Det här avsnittet beskriver och vägleder dig genom hello distributionen och konfigurationen av en rapportserver för SQL Server Reporting Services enhetligt läge i en virtuell dator i Azure. "
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 553af55b-d02e-4e32-904c-682bfa20fa0f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: asaxton
ms.openlocfilehash: e7791199c87dff106132f1535da12de40a8dbc9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-an-azure-vm-with-a-native-mode-report-server"></a>Använd PowerShell tooCreate en Azure VM med ett enhetligt läge Report Server
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Det här avsnittet beskriver och vägleder dig genom hello distributionen och konfigurationen av en rapportserver för SQL Server Reporting Services enhetligt läge i en virtuell dator i Azure. hello stegen i det här dokumentet används en kombination av manuella steg toocreate hello virtuell dator och en Windows PowerShell-skript tooconfigure Reporting Services på hello VM. hello konfigurationsskript innehåller öppnar en brandväggsport för HTTP eller HTTPs.

> [!NOTE]
> Om du inte behöver **HTTPS** på hello rapportserver **hoppa över steg 2**.
> 
> När du har skapat hello VM i steg 1, gå toohello avsnittet Använd skriptet tooconfigure hello report server- och HTTP. När du har kört skriptet hello är hello rapportservern klar toouse.

## <a name="prerequisites-and-assumptions"></a>Krav och förutsättningar
* **Azure-prenumeration**: Kontrollera hello antal kärnor som är tillgängliga i Azure-prenumeration. Om du skapar hello rekommenderas VM-storlek för **A3**, behöver du **4** tillgängliga kärnor. Om du använder en VM-storlek för **A2**, behöver du **2** tillgängliga kärnor.
  
  * tooverify hello core gränsen för prenumerationen i hello klassiska Azure-portalen, klicka på inställningar i hello till vänster och sedan klicka på användning i hello huvudmenyn.
  * tooincrease hello kärnkvot, kontakta [Azure-supporten](https://azure.microsoft.com/support/options/). Virtuell Storleksinformation finns i [storlekar för virtuella datorer för Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* **Windows PowerShell-skript**: hello avsnittet förutsätter att du har grundläggande kunskaper om Windows PowerShell. Mer information om hur du använder Windows PowerShell finns i hello följande:
  
  * [Starta Windows PowerShell på Windows Server](https://technet.microsoft.com/library/hh847814.aspx)
  * [Komma igång med Windows PowerShell](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>Steg 1: Etablera en virtuell Azure-dator
1. Bläddra toohello klassiska Azure-portalen.
2. Klicka på **virtuella datorer** hello vänster.
   
    ![Microsoft azure virtuella datorer](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)
3. Klicka på **Ny**.
   
    ![Knappen Nytt](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)
4. Klicka på **från galleriet**.
   
    ![ny virtuell dator från galleriet](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)
5. Klicka på **SQL Server 2014 RTM Standard-Windows Server 2012 R2** och klicka sedan på hello pilen toocontinue.
   
    ![nästa](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
   
    Om du behöver hello Reporting Services datadrivna prenumerationer funktionen väljer **SQL Server 2014 RTM Enterprise – Windows Server 2012 R2**. Mer information om SQL Server-utgåvorna och funktioner som stöds finns i [funktioner som stöds av hello utgåvor av SQL Server 2012](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).
6. På hello **konfiguration av virtuell dator** , redigera hello följande fält:
   
   * Om det finns fler än en **VERSION LANSERINGSDATUMET**, Välj hello senaste versionen.
   * **Namn på virtuell dator**: hello datornamnet används också på konfigurationssidan för hello nästa som hello standard Cloud Service DNS-namn. hello DNS-namn måste vara unikt inom hello Azure-tjänsten. Överväg att konfigurera hello VM med ett datornamn som beskriver vilka hello VM används för. Till exempel ssrsnativecloud.
   * **Nivån**: Standard
   * **Storlek: A3** rekommenderas hello VM-storlek för SQL Server-arbetsbelastningar. Om en virtuell dator används endast som en rapportserver, räcker en VM-storlek för A2 om hello rapportservern påträffar en stor belastning. VM information om priser finns i [prissättning för Virtual Machines](https://azure.microsoft.com/pricing/details/virtual-machines/).
   * **Nytt användarnamn**: hello namn skapas som en administratör på hello VM.
   * **Nytt lösenord** och **Bekräfta**. Detta lösenord används för hello nya administratörskontot och det rekommenderas att du använder ett starkt lösenord.
   * Klicka på **Nästa**. ![Nästa](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
7. På nästa sida hello redigera hello följande fält:
   
   * **Molntjänsten**: Välj **skapa en ny molntjänst**.
   * **Cloud Service DNS-namnet**: är hello offentliga DNS-namnet på hello tjänst i molnet som är associerad med hello VM. hello standardnamnet är hello namnet du angav i hello VM-namn. Om i senare steg i hello avsnittet skapar du ett betrott certifikat för SSL och sedan hello DNS-namn används för hello värdet för hello ”**utfärdat till**” hello certifikat.
   * **Region/tillhörighet grupp/virtuellt nätverk**: Välj hello region närmaste tooyour slutanvändare.
   * **Lagringskontot**: Använd ett lagringskonto som skapas automatiskt.
   * **Tillgänglighetsuppsättningen**: Ingen.
   * **SLUTPUNKTER** Behåll hello **fjärrskrivbord** och **PowerShell** slutpunkter och Lägg sedan till en HTTP- eller HTTPS-slutpunkt, beroende på din miljö.
     
     * **HTTP**: hello standard offentliga och privata portar är **80**. Observera att om du använder ett privat port än 80, ändra **$HTTPport = 80** i hello http-skript.
     * **HTTPS**: hello standard offentliga och privata portar är **443**. Av säkerhetsskäl är toochange hello privat port och konfigurera din brandvägg och hello report server toouse hello privat port. Läs mer på slutpunkter [hur tooSet in kommunikation med en virtuell dator](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Observera att om du använder en annan port än 443, ändra hello parametern **$HTTPsport = 443** i hello HTTPS-skript.
   * Klicka på Nästa. ![nästa](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
8. På hello sista sidan i guiden hello, behåller du standardvärdet för hello **installera hello VM agent** valda. hello stegen i det här avsnittet gör du genom att använda hello VM agent men om du planerar tookeep på den här virtuella datorn hello VM agent och tillägg tillåter du tooenhance han CM.  Mer information om hello VM-agenten finns [VM-Agent och tillägg – del 1](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). En hello standard tillägg installerade ad kör hello ”BGINFO”-tillägget som visar hello Virtuella skrivbordet, Systeminformation, till exempel intern IP-adress och ledigt enhet utrymme.
9. Klicka på Slutför. ![Okej](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)
10. Hej **Status** av hello VM visas som **start (etablering)** under hello etablera processen och sedan visar som **kör** när hello VM är allokerade och redo toouse.

## <a name="step-2-create-a-server-certificate"></a>Steg 2: Skapa ett servercertifikat
> [!NOTE]
> Om du inte behöver HTTPS på hello rapportserver, kan du **hoppa över steg 2** och gå toohello avsnittet **använda skriptet tooconfigure hello report server- och HTTP-**. Använd hello HTTP skriptet tooquickly konfigurera hello rapportservern och hello report server kommer att vara klar toouse.

I ordning toouse HTTPS på hello VM behöver du ett betrott certifikat för SSL. Du kan använda något av följande två metoder hello beroende på ditt scenario:

* Ett giltigt SSL-certifikat utfärdat av en certifikatutfärdare (CA) och betrodd av Microsoft. hello Certifikatutfärdarens rotcertifikat är obligatoriska toobe distribueras via hello Microsoft Root Certificate Program. Mer information om det här programmet finns [Windows och Windows Phone 8 SSL Root Certificate Program (medlem CAs)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) och [introduktion toohello Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).
* Ett självsignerat certifikat. Självsignerade certifikat rekommenderas inte för produktionsmiljöer.

### <a name="toouse-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>toouse ett certifikat som skapas av en betrodd certifikatutfärdare (CA)
1. **Begära ett certifikat för hello webbplats från en certifikatutfärdare**. 
   
    Du kan använda antingen toogenerate en fil med certifikatbegäran (Certreq.txt) att du skickar tooa certifikatutfärdare eller toogenerate en begäran om en onlinecertifikatutfärdare hello guiden Servercertifikat. Till exempel Microsoft Certificate Services i Windows Server 2012. Beroende på hello säkerhetsnivå identifiering erbjuds av servercertifikatet, det är flera dagar tooseveral månader för hello certification authority tooapprove din begäran och skicka en certifikatfil. 
   
    Mer information om hur du begär finns ett servercertifikat hello följande: 
   
   * Använd [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
   * Security Tools tooAdminister Windows Server 2012.
     
     [Security Tools tooAdminister Windows Server 2012](https://technet.microsoft.com/library/jj730960.aspx)
     
     > [!NOTE]
     > hello **utfärdat till** för hello betrodda SSL-certifikat bör vara hello samma som hello **DNS MOLNTJÄNSTNAMNET** som du använde för hello ny virtuell dator.

2. **Installera hello servercertifikat på hello webbserver**. hello webbservern är i det här fallet hello VM att värdar hello rapportservern och hello webbplatsen har skapats i senare steg när du konfigurerar Reporting Services. Mer information om hur du installerar servercertifikatet hello på hello webbserver med hjälp av hello MMC snapin-modulen för certifikat finns [installera ett servercertifikat](https://technet.microsoft.com/library/cc740068).
   
    Om du vill toouse hello skript som ingår i det här avsnittet tooconfigure hello rapportservern, hello värdet för hello certifikat **tumavtrycket** krävs som en parameter för hello skript. Avsnittet hello nästa information om hur tooobtain hello hello certifikatets tumavtryck.
3. Tilldela hello server certifikat toohello-rapportservern. hello tilldelningen har slutförts i nästa avsnitt om hello när du konfigurerar hello rapportservern.

### <a name="toouse-hello-virtual-machines-self-signed-certificate"></a>toouse hello ett självsignerat certifikat för virtuella datorer
Ett självsignerat certifikat har skapats på hello VM när hello VM har etablerats. hello certifikatet har hello samma namn som hello VM DNS-namn. I ordning tooavoid certifikatfel förekommer det krävs att hello-certifikatet är betrott på hello Virtuella datorn och också av alla användare av hello plats.

1. tootrust hello rotcertifikatutfärdaren för hello certifikatet på hello lokala VM lägga till hello certifikat toohello **betrodda rotcertifikatutfärdare**. hello följer en sammanfattning av hello steg som krävs. Detaljerade anvisningar om hur tootrust hello Certifikatutfärdaren finns [installera ett servercertifikat](https://technet.microsoft.com/library/cc740068).
   
   1. Välj hello VM hello klassiska Azure-portalen, och klicka på Anslut. Beroende på din webbläsarkonfiguration du tillfrågas toosave en RDP-fil för att ansluta toohello VM.
      
       ![ansluta tooazure virtuell dator](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Använd hello VM användarnamn, användarnamn och lösenord som du konfigurerade när du har skapat hello VM. 
      
       Till exempel i följande bild hello, hello VM är namnet **ssrsnativecloud** och hello användarnamnet är **testuser**.
      
       ![inloggningsnamnet innehåller vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
   2. Kör mmc.exe. Mer information finns i [så här: Visa certifikat med hello MMC-snapin-modulen](https://msdn.microsoft.com/library/ms788967.aspx).
   3. I hello konsolprogram **filen** menyn Lägg till hello **certifikat** snapin-modulen, Välj **datorkonto** när en fråga och klicka sedan på **nästa**.
   4. Välj **lokal dator** toomanage och klicka sedan på **Slutför**.
   5. Klicka på **Ok** och expandera sedan hello **certifikat - personliga** noder och klicka sedan på **certifikat**. hello certifikat efter hello DNS-namnet på hello VM och slutar med **cloudapp.net**. Högerklicka på hello certifikatets namn och klicka på **kopiera**.
   6. Expandera hello **betrodda rotcertifikatutfärdare** nod och högerklicka sedan **certifikat** och klicka sedan på **klistra in**.
   7. toovalidate, dubbla klickar du på hello certifikatnamn under **betrodda rotcertifikatutfärdare** och verifiera att det inte finns några fel och du ser ditt certifikat. Om du vill toouse hello HTTPS skriptet ingår i det här avsnittet tooconfigure hello rapportservern, hello värdet för hello certifikat **tumavtrycket** krävs som en parameter för hello skript. **tooget hello tumavtrycksvärde**, Slutför följande hello. Det finns också ett PowerShell-exempel tooretrieve hello tumavtryck i avsnittet [använda skriptet tooconfigure hello report server- och HTTPS](#use-script-to-configure-the-report-server-and-HTTPS).
      
      1. Dubbelklicka på hello namnet på hello certifikat, till exempel ssrsnativecloud.cloudapp.net.
      2. Klicka på hello **information** fliken.
      3. Klicka på **tumavtrycket**. hello-värdet för hello tumavtrycket visas i Informationsfältet hello, till exempel a6 08 3c df f9 0b f7 e3 7c 25 ed a4 ed 7e ac 91 9c 2c fb 2f.
      4. Kopiera hello tumavtrycket och spara hello värdet för senare eller redigera hello skript nu.
      5. (*) Ta bort hello blanksteg mellan hello värdepar innan du kör hello skript. Exempel är nu hello tumavtrycket anges innan a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.
      6. Tilldela hello server certifikat toohello-rapportservern. hello tilldelningen har slutförts i nästa avsnitt om hello när du konfigurerar hello rapportservern.

Om du använder ett självsignerat SSL-certifikat matchar hello namnet på hello certifikatet redan hello värdnamn hello VM. Därför är redan registrerad globalt hello DNS för hello dator och kan nås från alla klienter.

## <a name="step-3-configure-hello-report-server"></a>Steg 3: Konfigurera hello Report Server
Det här avsnittet beskriver hur du konfigurerar hello VM som en rapportserver för Reporting Services-enhetligt läge. Du kan använda någon av följande metoder tooconfigure hello rapportservern hello:

* Använd hello skriptet tooconfigure hello report server
* Använd Configuration Manager tooConfigure hello Report Server.

Mer detaljerad stegen i avsnittet hello [Anslut toohello virtuella datorn och starta hello Reporting Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Autentisering Obs:** Windows-autentisering är hello rekommenderad autentiseringsmetod och hello standardautentisering Reporting Services. Endast användare som har konfigurerats på hello VM har åtkomst till Reporting Services och tilldelade tooReporting Services-roller.

### <a name="use-script-tooconfigure-hello-report-server-and-http"></a>Använd skriptet tooconfigure hello report server- och HTTP
toouse hello Windows PowerShell-skript tooconfigure hello rapportservern, fullständig hello följande steg. hello konfigurationen omfattar HTTP, HTTPS inte:

1. Välj hello VM hello klassiska Azure-portalen, och klicka på Anslut. Beroende på din webbläsarkonfiguration du tillfrågas toosave en RDP-fil för att ansluta toohello VM.
   
    ![ansluta tooazure virtuell dator](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Använd hello VM användarnamn, användarnamn och lösenord som du konfigurerade när du har skapat hello VM. 
   
    Till exempel i följande bild hello, hello VM är namnet **ssrsnativecloud** och hello användarnamnet är **testuser**.
   
    ![inloggningsnamnet innehåller vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. Öppna på hello VM, **Windows PowerShell ISE** med administratörsbehörighet. hello PowerShell ISE installeras som standard i Windows server 2012. Det rekommenderas att du använder hello ISE i stället för standard Windows PowerShell-fönstret så att du kan klistra in hello skript i hello ISE, ändra hello skriptet och kör sedan hello skript.
3. Klicka på hello i Windows PowerShell ISE **visa** -menyn och klicka sedan på **visa Skriptfönster**.
4. Kopiera hello följande skript och klistra in hello skript i hello Skriptfönster för Windows PowerShell ISE.
   
        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
   
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change hello value if you used a different port for hello private HTTP endpoint when hello VM was created.
   
        ## Set PowerShell execution policy toobe able toorun scripts
        Set-ExecutionPolicy RemoteSigned -Force
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change hello version portion of hello path too"v11" toouse hello script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Report Server Configuration Steps
   
        ## Setting hello web service URL ##
        write-host -foregroundcolor green "Setting hello web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
   
        ## Setting hello Database ##
        write-host -foregroundcolor green "Setting hello Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating hello database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script toocreate hello database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes toosqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## Setting hello Report Manager URL ##
   
        write-host -foregroundcolor green "Setting hello Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
   
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
5. Om du har skapat hello VM med en HTTP-port än 80 ändra hello parametern $HTTPport = 80.
6. hello skript är konfigurerad för Reporting Services. Om du vill toorun hello skript för Reporting Services, ändra hello version del av hello sökvägen toohello namnområdet för ”v11” på hello Get-WmiObject-instruktion.
7. Kör skriptet hello.

**Verifieringen**: tooverify hello grundläggande report server fungerar, se hello [Kontrollera hello configuration](#verify-the-configuration) senare i det här avsnittet.

### <a name="use-script-tooconfigure-hello-report-server-and-https"></a>Använd skriptet tooconfigure hello report server- och HTTPS
toouse Windows PowerShell tooconfigure hello rapportservern, fullständig hello följande steg. hello konfigurationen innefattar HTTPS, inte HTTP.

1. Välj hello VM hello klassiska Azure-portalen, och klicka på Anslut. Beroende på din webbläsarkonfiguration du tillfrågas toosave en RDP-fil för att ansluta toohello VM.
   
    ![ansluta tooazure virtuell dator](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Använd hello VM användarnamn, användarnamn och lösenord som du konfigurerade när du har skapat hello VM. 
   
    Till exempel i följande bild hello, hello VM är namnet **ssrsnativecloud** och hello användarnamnet är **testuser**.
   
    ![inloggningsnamnet innehåller vm](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. Öppna på hello VM, **Windows PowerShell ISE** med administratörsbehörighet. hello PowerShell ISE installeras som standard i Windows server 2012. Det rekommenderas att du använder hello ISE i stället för standard Windows PowerShell-fönstret så att du kan klistra in hello skript i hello ISE, ändra hello skriptet och kör sedan hello skript.
3. tooenable köra skript, kör hello följande Windows PowerShell-kommando:
   
        Set-ExecutionPolicy RemoteSigned
   
    Du kan sedan köra hello följande tooverify hello princip:
   
        Get-ExecutionPolicy
4. I **Windows PowerShell ISE**, klicka på hello **visa** -menyn och klicka sedan på **visa Skriptfönster**.
5. Kopiera hello följande skript och klistra in den i hello Skriptfönster för Windows PowerShell ISE.
   
        ## This script configures hello report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when hello HTTPS endpoint was created.
   
        # You can run hello following command tooget (.cloudapp.net certificates) so you can copy hello thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # hello certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # hello certificate hash should not contain spaces
   
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If hello certificate is not a wildcard certificate, comment out hello following line, and enable hello full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        write-host "hello script will use $DNSNameAndPort as hello DNS name and port" 
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change hello version portion of hello path too"v11" toouse hello script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Reporting Services Report Server Configuration Steps
   
        ## 1. Setting hello web service URL ##
        write-host -foregroundcolor green "Setting hello web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
   
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
   
        ## 2. Setting hello Database ##
        write-host -foregroundcolor green "Setting hello Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating hello database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script toocreate hello database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes toosqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## 3. Setting hello Report Manager URL ##
   
        write-host -foregroundcolor green "Setting hello Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
   
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
   
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
6. Ändra hello **$certificatehash** parameter i hello skript:
   
   * Det här är en **krävs** parameter. Om du inte sparade hello certifikatvärdet från hello föregående steg, med någon av följande metoder toocopy hello certifikatets hash-värde från hello certifikat-tumavtrycket hello.:
     
       Öppna Windows PowerShell ISE på hello VM, och kör följande kommando hello:
     
           dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
     
       hello utdata ser liknande toohello följande. Om hello skriptet returnerar en tom rad, hello VM inte har ett certifikat som konfigurerats i till exempel, finns i avsnittet hello [toouse hello virtuella datorer ett självsignerat certifikat](#to-use-the-virtual-machines-self-signed-certificate).
     
     ELLER
   * Hello VM kör mmc.exe och Lägg sedan till hello **certifikat** snapin-modulen.
   * Under hello **betrodda rotcertifikatutfärdare** nod Dubbelklicka på certifikatets namn. Om du använder hello självsignerat certifikat för hello VM hello certifikat efter hello DNS-namnet på hello VM och slutar med **cloudapp.net**.
   * Klicka på hello **information** fliken.
   * Klicka på **tumavtrycket**. hello-värdet för hello tumavtrycket visas i Informationsfältet hello, till exempel af 11 60 b6 4b 28 8 d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48
   * **Innan du kör skriptet hello**, ta bort hello blanksteg mellan hello värdepar. Till exempel af1160b64b288d890a8212ff6ba9c3664f319048
7. Ändra hello **$httpsport** parameter: 
   
   * Om du använder port 443 för HTTPS-slutpunkt för hello sedan behöver du inte tooupdate den här parametern i hello skript. Annars använda hello portvärde som du valde när du har konfigurerat privata hello HTTPS-slutpunkt på hello VM.
8. Ändra hello **$DNSName** parameter: 
   
   * hello skript har konfigurerats för ett certifikat med jokertecken $DNSName = ”+”. Om du gör inga vill tooconfigure för en bindning för certifikat med jokertecken kommentera ut $DNSName = ”+” och aktivera följande rad, hello fullständig $DNSNAme referens, ## $DNSName="$server.cloudapp.net hello”.
     
       Ändra hello $DNSName värde om du inte vill toouse hello virtuella datorns DNS-namn för Reporting Services. Om du använder hello parametern hello certifikatet måste också använda det här namnet och du registrera hello globalt på en DNS-server.
9. hello skript är konfigurerad för Reporting Services. Om du vill toorun hello skript för Reporting Services, ändra hello version del av hello sökvägen toohello namnområdet för ”v11” på hello Get-WmiObject-instruktion.
10. Kör skriptet hello.

**Verifieringen**: tooverify hello grundläggande report server fungerar, se hello [Kontrollera hello configuration](#verify-the-connection) senare i det här avsnittet. tooverify hello certifikat-bindning öppna en kommandotolk med administratörsbehörighet och kör sedan följande kommando hello:

    netsh http show sslcert

hello resultatet innehåller hello följande:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-tooconfigure-hello-report-server"></a>Använd Configuration Manager tooConfigure hello Report Server
Om du inte vill att toorun hello PowerShell-skriptet tooconfigure hello rapportservern åtgärderna hello i det här avsnittet toouse hello Reporting Services enhetligt läge configuration manager tooconfigure hello-rapportservern.

1. Välj hello VM hello klassiska Azure-portalen, och klicka på Anslut. Använd hello användarnamn och lösenord som du konfigurerade när du har skapat hello VM.
   
    ![ansluta tooazure virtuell dator](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)
2. Kör Windows update och installera uppdateringar toohello VM. Om det krävs en omstart av hello VM, starta om hello VM och återansluta toohello VM från hello klassiska Azure-portalen.
3. Hello Start-menyn på hello VM skriver **Reporting Services** och öppna **Reporting Services Configuration Manager**.
4. Lämna hello standardvärden för **servernamn** och **rapportserverinstansen**. Klicka på **Anslut**.
5. Hello vänster klickar du på **URL för webbtjänsten**.
6. Som standard konfigureras RS för HTTP-port 80 med IP-”alla tilldelade”. tooadd HTTPS:
   
   1. I **SSL-certifikat**: Välj hello-certifikat som du vill toouse, till exempel [VM name]. cloudapp.net. Om inga certifikat visas i avsnittet hello **steg 2: skapa ett servercertifikat** information om hur tooinstall och förtroende hello certifikatet på hello VM.
   2. Under **SSL-Port**: Välj 443. Om du har konfigurerat privata hello HTTPS-slutpunkt i hello VM med en annan privat port använda värdet här.
   3. Klicka på **tillämpa** och vänta tills hello åtgärden toocomplete.
7. Hello vänster klickar du på **databasen**.
   
   1. Klicka på **ändra Databas**e.
   2. Klicka på **skapa en ny rapportserverdatabas** och klicka sedan på **nästa**.
   3. Lämna hello standard **servernamn**: som hello VM namn och lämna hello standard **autentiseringstyp** som **aktuell användare** – **-integreradsäkerhet**. Klicka på **Nästa**.
   4. Lämna hello standard **databasnamnet** som **ReportServer** och på **nästa**.
   5. Lämna hello standard **autentiseringstyp** som **Tjänstereferenser** och på **nästa**.
   6. Klicka på **nästa** på hello **sammanfattning** sidan.
   7. När hello konfigurationen är klar klickar du på **Slutför**.
8. Hello vänster klickar du på **URL för Report Manager**. Lämna hello standard **virtuell katalog** som **rapporter** och på **tillämpa**.
9. Klicka på **avsluta** tooclose hello Reporting Services Configuration Manager.

## <a name="step-4-open-windows-firewall-port"></a>Steg 4: Öppna Windows-brandväggen porten
> [!NOTE]
> Om du använde en rapportserver för hello skript tooconfigure hello kan du hoppa över det här avsnittet. hello skript med en steg tooopen hello brandväggsport. hello standard var port 80 för HTTP och port 443 för HTTPS.
> 
> 

tooconnect via fjärranslutning tooReport Manager eller hello Report Server på hello virtuell dator, en TCP-slutpunkt krävs på hello VM. Det är obligatoriskt tooopen hello samma port i brandväggen hello VM. hello-slutpunkt skapas när hello VM har etablerats.

Det här avsnittet innehåller grundläggande information om hur tooopen hello brandväggsport. Mer information finns i [konfigurera en brandvägg för Report Server Access](https://technet.microsoft.com/library/bb934283.aspx)

> [!NOTE]
> Om du använde hello skriptet tooconfigure hello rapportservern kan du hoppa över det här avsnittet. hello skript med en steg tooopen hello brandväggsport.
> 
> 

Om du har konfigurerat en privat port för HTTPS än 443, ändra hello följande skript på lämpligt sätt. tooopen port **443** på hello Windows-brandväggen, utföra hello följande:

1. Öppna en Windows PowerShell-fönster med administratörsbehörighet.
2. Om du använder en annan port än 443 när du har konfigurerat hello HTTPS-slutpunkt på hello VM, uppdatera hello port i hello följande kommando och kör sedan hello-kommando:
   
        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443
3. När hello-kommandot har slutförts **Ok** visas i hello kommandotolk.

tooverify att hello porten är öppen, öppna ett Windows PowerShell-fönster och kör hello följande kommando:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-hello-configuration"></a>Verifiera konfigurationen av hello
tooverify hello grundläggande report server nu fungerar, öppna webbläsaren med administratörsbehörighet och sedan bläddra toohello följande report server ad report manager URL: er:

* På hello VM, bläddrar du toohello rapportserverns URL:
  
        http://localhost/reportserver
* Bläddra toohello report Manager URL: en på hello VM:
  
        http://localhost/Reports
* Från den lokala datorn, bläddra toohello **remote** report Manager på hello VM. Uppdatera hello DNS-namnet i hello följande exempel efter behov. När du uppmanas att ange ett lösenord, Använd hello administratörsautentiseringsuppgifter som du skapade när hello VM har etablerats. hello användarnamnet är i hello [domän]\[användarnamn] format, där hello domän är datornamnet för hello VM, till exempel ssrsnativecloud\testuser. Om du inte använder HTTP**S**, ta bort hello **s** i hello-URL. Hello nästa avsnittet finns information om hur du skapar ytterligare användare på den virtuella datorn.
  
        https://ssrsnativecloud.cloudapp.net/Reports
* Bläddra toohello remote rapportserverns URL från den lokala datorn. Uppdatera hello DNS-namnet i hello följande exempel efter behov. Ta bort hello s i hello URL Om du inte använder HTTPS.
  
        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Skapa användare och tilldela roller
När konfigurera och verifiera hello rapportservern, en vanliga administrativa uppgifter är toocreate en eller flera användare och tilldela användare tooReporting Services-roller. Mer information finns i hello följande:

* [Skapa ett lokalt användarkonto](https://technet.microsoft.com/library/cc770642.aspx)
* [Bevilja användaråtkomst tooa Report Server (Report Manager)](https://msdn.microsoft.com/library/ms156034.aspx))
* [Skapa och hantera rolltilldelningar](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="toocreate-and-publish-reports-toohello-azure-virtual-machine"></a>tooCreate och publicera rapporter toohello Azure-dator
hello följande tabell visas några av hello alternativ tillgängliga toopublish befintliga rapporter från en lokal dator toohello rapportservern finns på hello Microsoft Azure-dator:

* **RS.exe skript**: Använd RS.exe skriptet toocopy rapportobjekt från och befintliga report server tooyour Microsoft Azure-dator. Mer information finns i avsnittet hello ”enhetligt läge tooNative läge – Microsoft Azure-dator” i [exempel Reporting Services rs.exe skriptet tooMigrate innehåll mellan rapportservrar](https://msdn.microsoft.com/library/dn531017.aspx).
* **Report Builder**: hello virtuell dator innehåller hello klicka – en gång version av Microsoft SQL Server Report Builder. toostart Report builder hello första gången på hello virtuell dator:
  
  1. Starta din webbläsare med administratörsbehörighet.
  2. Bläddra tooreport manager på hello virtuella datorn och klicka på **Report Builder** hello menyfliken.
     
     Mer information finns i [installerar och avinstallerar stöder Report Builder](https://technet.microsoft.com/library/dd207038.aspx).
* **SQL Server Data Tools: VM**: Om du har skapat hello virtuell dator med SQL Server 2012 och SQL Server Data Tools är installerat på hello virtuell dator och kan vara används toocreate **Report Server-projekt** och rapporter om hello virtuella datorn. SQL Server Data Tools kan publicera rapporter hello toohello rapportservern på hello virtuella datorn.
  
    Om du har skapat hello virtuell dator med SQLServer 2014 kan du installera SQL Server Data Tools - BI för visual Studio. Mer information finns i hello följande:
  
  * [Microsoft SQL Server Data Tools - Business Intelligence för Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313)
  * [Microsoft SQL Server Data Tools - Business Intelligence för Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)
  * [SQL Server Data Tools och SQL Server Business Intelligence (SSDT BI)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)
* **SQL Server Data Tools: Remote**: skapa en Reporting Services-projekt i SQL Server Data Tools som innehåller Reporting Services-rapporter på den lokala datorn. Konfigurera hello projektet tooconnect toohello webbtjänst-URL.
  
    ![SSDT projektegenskaperna för SSRS-projekt](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)
* **Använda skript**: använda skriptet toocopy report serverinnehåll. Mer information finns i [exempel Reporting Services rs.exe skriptet tooMigrate innehåll mellan rapportservrar](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-hello-vm"></a>Minimera kostnaderna om du inte använder hello VM
> [!NOTE]
> toominimize avgifter för dina Azure virtuella datorer som, Stäng hello VM från hello klassiska Azure-portalen. Om du använder hello Windows Energialternativ inuti en VM tooshut ned hello VM fortfarande debiteras du hello samma belopp för hello VM. tooreduce debiterar måste tooshut ned hello VM i hello klassiska Azure-portalen. Om du inte längre behöver hello VM Kom ihåg toodelete hello VM och hello associerade VHD-filer tooavoid lagringskostnader för volymer. Mer information finns i avsnittet hello vanliga frågor och svar på [prisinformation för virtuella datorer](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="more-information"></a>Mer information
### <a name="resources"></a>Resurser
* För liknande innehåll relaterade tooa distribution på enskild server finns i SQL Server Business Intelligence och SharePoint 2013 [använda Windows PowerShell tooCreate en Azure VM med SQL Server BI och SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).
* Liknande innehåll relaterade tooa finns flera server-distributionen av SQL Server Business Intelligence och SharePoint 2013 [distribuera SQL Server Business Intelligence i Azure Virtual Machines](https://msdn.microsoft.com/library/dn321998.aspx).
* Allmän information finns i Närliggande toodeployments av SQL Server Business Intelligence i Azure Virtual Machines [SQL Server Business Intelligence i Azure Virtual Machines](virtual-machines-windows-classic-ps-sql-bi.md).
* Mer information om hello kostnad Azure datorkostnader finns hello virtuella datorer på fliken i [Azure prisnivå Kalkylatorn](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Community-innehåll
* Steg-för-steg-anvisningar om hur toocreate en Reporting Services enhetligt läge rapporterar server utan att använda skriptet, se [värd för SQL Reporting Service på Azure Virtual Machine](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-tooother-resources-for-sql-server-in-azure-vms"></a>Länkar tooother resurser för SQL Server i virtuella Azure-datorer
[SQLServer på Azure virtuella datorer – översikt](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

