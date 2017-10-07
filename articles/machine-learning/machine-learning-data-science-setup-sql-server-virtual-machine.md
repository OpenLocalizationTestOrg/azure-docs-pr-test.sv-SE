---
title: "aaaSet av en virtuell dator i SQL Server som en bärbar dator IPython server | Microsoft Docs"
description: "Ställ in en datavetenskap virtuell dator med SQL Server och IPython Server."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 1fd6014a-d180-4558-b4eb-d9b5a331a99f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: xibingao;bradsev
ms.openlocfilehash: ee83d1d5de671d9817c1bc1abd6b4f9c256dde8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Konfigurera en virtuell dator i Azure SQL Server som en IPython Notebook-server för avancerade analyser
Det här avsnittet visar hur tooprovision och konfigurera en SQL Server virtuella toobe som används som en del av en molnbaserad datavetenskap miljö. hello Windows virtuell dator konfigureras med stödjande verktyg som IPython anteckningsboken, Azure Lagringsutforskaren AzCopy samt andra verktyg som är användbara för datavetenskap projekt. Azure Lagringsutforskaren och AzCopy, till exempel ange praktiskt sätt tooupload data tooAzure blob storage från din lokala dator eller toodownload den tooyour lokala datorn från blob storage.

hello-galleriet för virtuella Azure-datorn innehåller flera avbildningar som innehåller Microsoft SQL Server. Välj en SQL Server-VM-avbildning som passar dina databehov. Rekommenderade bilder är:

* SQL Server 2012 SP2 Enterprise för små toomedium datamängder
* SQL Server 2012 SP2 Enterprise optimerade för DataWarehousing arbetsbelastningar för stora toovery stora datamängder
  
  > [!NOTE]
  > Bild av SQL Server 2012 SP2 Enterprise **innehåller inte en datadisk**. Du kommer måste tooadd och/eller ansluta en eller flera virtuella hårddiskar toostore dina data. När du skapar en virtuell Azure-dator har en disk för hello mappas toohello C operativsystemsenheten och en tillfällig mappas toohello D diskenhet. Använd inte hello D enhet toostore data. Hello namnet antyder ger den tillfälligt lagringsutrymme. Det finns ingen redundans eller säkerhetskopiering eftersom den inte finns i Azure-lagring.
  > 
  > 

## <a name="Provision"></a>Ansluta toohello klassiska Azure-portalen och etablera en virtuell dator med SQL Server
1. Logga in toohello [Azure klassiska portal](http://manage.windowsazure.com/) med ditt konto.
   Om du inte har något Azure-konto besöker du sidan för [kostnadsfria utvärderingsversioner av Azure](https://azure.microsoft.com/pricing/free-trial/).
2. Hello Azure klassiska portal hello längst ned till vänster i hello webbsidan, klicka på **+ ny**, klickar du på **COMPUTE**, klickar du på **VIRTUELLA**, och klicka sedan på **från GALLERIET**.
3. På hello **skapa en virtuell dator** sidan, Välj en avbildning av virtuell dator med SQL Server baserat på dina databehov klicka sedan på hello nästa pilen längst ned till höger i hello-sidan. Hello uppdaterad information om hello stöds SQL Server-avbildningar i Azure, finns [komma igång med SQL Server i Azure Virtual Machines](http://go.microsoft.com/fwlink/p/?LinkId=294720) -avsnittet i hello [SQL Server i Azure Virtual Machines](http://go.microsoft.com/fwlink/p/?LinkId=294719) dokumentationsuppsättningen.
   
   ![Välj SQLServer-dator][1]
4. På hello första **konfiguration av virtuell dator** anger du följande information:
   
   * Ange en **VIRTUELLT datornamn**.
   * I hello **nytt användarnamn** rutan, unikt namn för hello VM lokalt administratörskonto.
   * I hello **nytt lösenord** skriver ett starkt lösenord. Mer information finns i [Starka lösenord](http://msdn.microsoft.com/library/ms161962.aspx).
   * I hello **BEKRÄFTA lösenord** rutan, Skriv hello lösenordet igen.
   * Välj lämplig hello **storlek** från hello listrutan.
     
     > [!NOTE]
     > hello storleken på hello virtuella datorn har angetts under etablering: A2 är hello minsta storlek som rekommenderas för produktionsarbetsbelastningar. Den minsta rekommenderade storleken för en virtuell dator är A3 när du använder SQL Server Enterprise Edition. Välj A3 eller senare när du använder SQL Server Enterprise Edition. Välj A4 när du använder SQL Server 2012 eller 2014 Enterprise optimerade för transaktionell arbetsbelastningar bilder.
     > Välj A7 när du använder SQL Server 2012 eller 2014 Enterprise optimerad för Data Warehousing arbetsbelastningar bilder. hello storlek som valts begränsar antalet datadiskar som du kan konfigurera. Uppdaterad information om tillgängliga virtuella datorstorlekar och hello antalet datadiskar som du kan koppla tooa virtuella datorn finns [storlekar för virtuella datorer för Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Information om priser finns [virtuella Macines priser](https://azure.microsoft.com/pricing/details/virtual-machines/).
     > 
     > 
   
   Klicka på pilen hello nästa hello nedre högra toocontinue.
   
   ![VM-konfiguration][2]
5. På hello andra **konfiguration av virtuell dator** konfigurerar resurser för nätverk, lagring och tillgänglighet:
   
   * I hello **Molntjänsten** väljer **skapa en ny molntjänst**.
   * I hello **DNS Molntjänstnamnet** ange hello första delen av ett DNS-namn du själv väljer, så att den är klar ett namn i formatet **TESTNAME.cloudapp.net**
   * I hello **REGION/TILLHÖRIGHET grupp/VIRTUELLT nätverk** väljer du en region där den här virtuella bilden kommer att finnas.
   * I hello **Lagringskonto**väljer du ett befintligt lagringskonto eller en skapas automatiskt.
   * I hello **TILLGÄNGLIGHETSUPPSÄTTNING** väljer **(ingen)**.
   * Läs och Godkänn hello prisinformation.
6. I hello **SLUTPUNKTER** klickar du på i hello tom listrutan under **namn**, och välj **MSSQL** Skriv hello portnumret på instansen av hello databasmotorn (**1433** för hello standardinstans).
7. SQL Server-VM kan också fungera som en IPython anteckningsboken-Server som kommer att konfigureras i ett senare steg.
   Lägg till en ny slutpunkt toospecify hello port toouse serverns IPython bärbar dator. Ange ett namn i hello **namn** kolumn, väljer du ett portnummer för dina val för hello offentliga porten och 9999 för hello privat port.
   
   Klicka på pilen hello nästa hello nedre högra toocontinue.
   
   ![Välj MSSQL och IPython portar][3]
8. Acceptera standardvärdet för hello **installera VM-agenten** alternativet markerat och klicka på hello hello markerat hello nedre högra hörnet av hello guiden toocomplete hello VM etableringsprocessen.
   
   `![Sista alternativen för VM][4]
9. Vänta medan Azure förbereder den virtuella datorn. Förväntar dig hello virtuella status tooproceed via:
   
   * Starta (allokering)
   * Stoppad
   * Starta (allokering)
   * Kör (allokering)
   * Körs

## <a name="RemoteDesktop"></a>Öppna hello virtuella datorn med fjärrskrivbord och fullständig installation
1. När etableringen är slutförd, klicka på hello namn för din virtuella toogo toohello INSTRUMENTPANELENS sida. Hello längst hello-sidan, klickar du på **Anslut**.
2. Välj tooopen hello rpd filen med hello Fjärrskrivbord för Windows-program (`%windir%\system32\mstsc.exe`).
3. Vid hello **Windows-säkerhet** dialogrutan Ange hello lösenord för det lokala administratörskontot som du angav tidigare.
   (Du kan uppmanas tooverify hello autentiseringsuppgifterna för hello virtuella datorn.)
4. hello behöva första gången du loggar in toothis virtuell dator, flera processer toocomplete, inklusive inställning av din desktop, Windows-uppdateringar och slutförande av åtgärder för inledande konfiguration av Windows hello (sysprep). När Windows sysprep är klar slutför installationen av SQL Server konfigurationsåtgärder. Dessa uppgifter kan orsaka en fördröjning på några minuter medan den är klar. `SELECT @@SERVERNAME`kan inte returnera hello rätt namn förrän SQL Server-installationen är klar och SQL Server Management Studio kanske inte visable på hello startsidan.

När du är ansluten toohello virtuella datorn med Fjärrskrivbord för Windows som hello virtuella datorn fungerar ungefär andra datorer. Ansluta toohello standardinstansen av SQL Server med SQL Server Management Studio (som körs på den virtuella datorn hello) i hello normalt sätt.

## <a name="InstallIPython"></a>Installera IPython bärbara datorer och andra stödfiler verktyg
tooconfigure din nya SQL Server-VM tooserve som en bärbar dator IPython server och installera ytterligare stöd för verktyg för sådana AzCopy, Azure Lagringsutforskaren, användbara datavetenskap Python-paket och andra, en särskild anpassning skript som tooyou. tooinstall:

1. Högerklicka på hello **Windows starta** ikonen och klickar på **kommandotolk (Admin)**
2. Kopiera hello följande kommandon och klistra in hello Kommandotolken.
  
        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"
3. När du uppmanas, anger du ett lösenord önskat för hello IPython anteckningsboken server.
4. hello anpassning skript automatiserar flera efter installationen procedurer, bland annat:
    * Installationen av IPython anteckningsboken server
    * Öppna TCP-portar i hello Windows-brandväggen för hello slutpunkter har skapats tidigare:
    * För SQL Server för fjärranslutningar
    * För fjärranslutna IPython anteckningsboken serveranslutning
    * Hämtar exempel IPython anteckningsböcker och SQL-skript
    * Hämta och installera användbara datavetenskap Python-paket
    * Hämta och installera Azure-verktyg, till exempel AzCopy och Azure Lagringsutforskaren  
    <br>
5. Du kan komma åt och köra IPython bärbar dator från en lokal eller fjärransluten webbläsare med en URL i formatet hello `https://<virtual_machine_DNS_name>:<port>`, där porten är hello IPython offentliga du valt vid etablering av hello virtuell dator.
6. IPython anteckningsboken servern körs som en bakgrundstjänst och kommer att startas om automatiskt när du startar om hello virtuell dator.

## <a name="Optional"></a>Ansluta en datadisk efter behov
Om VM-avbildning inte innehåller datadiskar, d.v.s. diskar än C-enheten (OS-disk) och enhet D (tillfällig disk), du måste tooadd en eller fler data diskar toostore dina data. hello VM-avbildning för SQL Server 2012 SP2 Enterprise optimerade för DataWarehousing arbetsbelastningar kommer förkonfigurerade med ytterligare diskar för SQL Server data- och loggfiler.

> [!NOTE]
> Använd inte hello D enhet toostore data. Hello namnet antyder ger den tillfälligt lagringsutrymme. Det finns ingen redundans eller säkerhetskopiering eftersom den inte finns i Azure-lagring.
> 
> 

tooattach ytterligare hårddiskar, följ hello stegen som beskrivs i [hur tooAttach en datadisk tooa Windows-dator](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), som får du hjälp:

1. Bifoga tom diskarna toohello virtuell dator etablerats i tidigare steg
2. Initieringen av hello nya diskarna i hello virtuell dator

## <a name="SSMS"></a>Ansluta tooSQL Server Management Studio och aktivera autentisering för blandat läge
hello SQL Server Database Engine kan inte använda Windows-autentisering utan domänmiljö. tooconnect toohello databasmotorn från en annan dator, konfigurerar SQL Server för verifiering i blandat läge. Med blandat läge-autentisering tillåts både SQL Server-autentisering och Windows-autentisering. SQL-autentiseringsläge krävs i ordning tooingest data direkt från din SQL Server-VM-databaser i den [Azure Machine Learning Studio](https://studio.azureml.net) hello importera Data modulen.

1. När du anslutna toohello virtuell dator med hjälp av fjärrskrivbord kan använda Windows hello **Sök** fönstret och skriv **SQL Server Management Studio** (SMSS). Klicka på toostart hello SQL Server Management Studio (SSMS). Du kanske vill tooadd tooSSMS en genväg på skrivbordet för framtida användning.
   
   ![Starta SSMS][5]
   
   hello första gången du öppnar Management Studio måste det skapa hello användare Management Studio miljö. Det kan ta en stund.
2. När du öppnar, Management Studio visar hello **ansluta tooServer** dialogrutan. I hello **servernamn** rutan, hello-typnamn för hello virtuella tooconnect toohello databasmotorn med hello Object Explorer.
   (I stället för namn på hello virtuell dator kan du också använda **(lokal)** eller en enskild period som hello **servernamn**. Välj **Windows-autentisering**, och lämna  ***din\_VM\_namn*\\din\_lokala\_administratör**  i hello **användarnamn** rutan. Klicka på **Anslut**.
   
   ![Ansluta tooServer][6]
   
   <br>
   
   > [!TIP]
   > Du kan ändra hello SQL Server-autentiseringsläge med hjälp av en Windows ändringen av registernyckeln eller hello SQL Server Management Studio. toochange autentiseringsläge med hello ändringen av registernyckeln, starta en **ny fråga** och köra hello följande skript:
   > 
   > 
   
       USE master
       go
   
       EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
       go

    toochange hello autentiseringsläge använder SQL Server management Studio:

1. I **SQL Server Management Studio Object Explorer**, högerklicka på namnet på hello instans av SQL Server (hello virtuella namn) och klicka sedan på **egenskaper**.
   
   ![Serveregenskaper][7]
2. På hello **säkerhet** sidan under **serverautentisering**väljer **SQL Server och Windows-autentisering läge**, och klicka sedan på **OK** .
   
   ![Välja autentiseringsläge][8]
3. I hello **SQL Server Management Studio** dialogrutan klickar du på **OK** bekräfta hello krav toorestart SQL Server.
4. I **Object Explorer**, högerklicka på din server och klicka sedan på **starta om**. (Om SQL Server Agent körs måste den också startas om.)
   
   ![Starta om][9]
5. I hello **SQL Server Management Studio** dialogrutan klickar du på **Ja** accepterar som du vill toorestart SQL Server.

## <a name="Logins"></a>Skapa SQL Server-autentisering-inloggningar
tooconnect toohello databasmotorn från en annan dator, måste du skapa minst en inloggning för SQL Server-autentisering.  

Du kan skapa nya SQL Server-inloggningar programmässigt eller med hjälp av hello SQL Server Management Studio. toocreate en ny sysadmin-användare med SQL-autentisering programmässigt, starta en **ny fråga** och kör följande skript hello. Ersätt < nytt användarnamn\> och < nytt lösenord\> med ditt val av *användarnamn* och *lösenord*. 

    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Justera hello lösenordsprincip efter behov (hello exempelkod inaktiverar principen kontrollerar och lösenord upphör att gälla). Mer information om SQL Server-inloggningar finns i [Skapa en inloggning](http://msdn.microsoft.com/library/aa337562.aspx).  

toocreate nya SQL Server-inloggningar med hello SQL Server Management Studio:

1. I **SQL Server Management Studio Object Explorer**, expandera hello mapp hello server-instans som du vill toocreate hello ny inloggning.
2. Högerklicka på hello **säkerhet** mappen, peka för**ny**, och välj **inloggning...** .
   
   ![Ny inloggning][10]
3. I hello **inloggning - ny** på hello dialogrutan **allmänna** anger hello namn för den nya användaren i hello i hello **inloggningsnamnet** rutan.
4. Välj **SQL Server-autentisering**.
5. I hello **lösenord** anger ett lösenord för hello ny användare. Ange lösenordet igen i hello **Bekräfta lösenord** rutan.
6. tooenforce lösenord principen för komplexitet och tillsyn, väljer **genomdriva lösenordsprinciper** (rekommenderas). Detta är en standardalternativet när SQL Server-autentisering har valts.
7. tooenforce lösenord princip för förfallodatum, väljer **genomdriva Lösenordets giltighetstid** (rekommenderas). Tillämpa princip för lösenord måste vara valda tooenable den här kryssrutan. Detta är en standardalternativet när SQL Server-autentisering har valts.
8. Välj tooforce hello användaren toocreate ett nytt lösenord när hello första gången inloggningen används **användaren måste byta lösenord vid nästa inloggning** (rekommenderas om inloggningen är för någon annan toouse. Om hello inloggningen är för eget bruk, Välj inte det här alternativet.) Framtvinga lösenordet upphör att gälla måste vara valda tooenable den här kryssrutan. Detta är en standardalternativet när SQL Server-autentisering har valts.
9. Från hello **standarddatabasen** väljer du en standarddatabas för hello inloggningen. **Master** är hello standardvärde för det här alternativet. Om du inte har skapat en användardatabas lämna det ställa in för**master**.
10. I hello **standardspråk** listan, lämna **standard** som hello-värde.
    
    ![Inloggningsegenskaper][11]
11. Om det här är första hello inloggning som du skapar kan du vill ange den här inloggningen som en SQL Server-administratör. I så fall markerar du **sysadmin** på sidan **Serverroller**.
    
    > [!IMPORTANT]
    > Medlemmar i hello fasta serverrollen sysadmin har fullständig kontroll över hello databasmotorn. Av säkerhetsskäl bör du noggrant begränsa medlemskap i den här rollen.
    > 
    > 
    
    ![sysadmin][12]
12. Klicka på OK.

## <a name="DNS"></a>Fastställa hello hello virtuella datorns DNS-namn
tooconnect toohello SQL Server Database Engine från en annan dator, måste du känna till hello Domain Name System (DNS) namn på hello virtuell dator.

(Detta är hello namn hello internet använder tooidentify hello virtuell dator. Du kan använda hello IP-adress, men hello IP-adress kan ändras när Azure flyttar resurser för redundans och underhåll. hello DNS-namn är stabil eftersom det kan vara omdirigeras tooa nya IP-adressen.)

1. I hello klassiska Azure-portalen (eller hello föregående steg), Välj **VIRTUELLA datorer**.
2. På hello **VIRTUELLA DATORINSTANSER** i hello sidan **DNS-namnet** kolumn, hitta och kopiera hello DNS-namn för hello virtuella datorn som visas föregås av **http://**. (hello användargränssnittet kanske inte visar hello hela namn, men du kan högerklicka på den och välj Kopiera.)

## <a name="cde"></a>Ansluta toohello databasmotorn från en annan dator
1. På en dator ansluten toohello internet, Öppna SQL Server Management Studio.
2. I hello **ansluta tooServer** eller **ansluta tooDatabase motorn** i dialogrutan hello **servernamn** ange hello DNS-namn för den virtuella datorn (bestäms i hello föregående aktivitet) och en offentlig slutpunkt portnummer i hello-format för *DNSName, portnumber* som **tutorialtestVM.cloudapp.net,57500**.
3. I hello **autentisering** väljer **SQL Server-autentisering**.
4. I hello **inloggning** rutan, hello-typnamn för en inloggning som du skapade i en tidigare uppgift.
5. I hello **lösenord** rutan, ange hello lösenord för hello-inloggning som du skapar i en tidigare uppgift.
6. Klicka på **Anslut**.

## <a name="amlconnect"></a>Ansluta toohello Database Engine från Azure Machine Learning
I senare steg i hello Team av vetenskapliga data använder du hello [Azure Machine Learning Studio](https://studio.azureml.net) toobuild och distribuera machine learning-modeller. tooingest data från SQL Server-VM-databaser direkt i Azure Machine Learning utbildning eller bedömningen, använda hello **importera Data** modul i en ny [Azure Machine Learning Studio](https://studio.azureml.net) experiment. Det här avsnittet beskrivs mer detaljerat via hello Team datavetenskap Process guiden länkar. En introduktion finns [vad är Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

1. I hello **egenskaper** rutan hello [importera Data modulen](https://msdn.microsoft.com/library/azure/dn905997.aspx)väljer **Azure SQL Database** från hello **datakällan** listrutan.
2. I hello **Databasservernamnet** text Ange`tcp:<DNS name of your virtual machine>,1433`
3. Anger användarnamnet för hello SQL i hello **Server användarkontonamnet** textruta.
4. Ange hello sql användarens lösenord i hello **serverlösenord** textruta.
   
   ![Azure Machine Learning importera Data][13]

## <a name="shutdown"></a>Avstängning och frigör virtuell dator som
Virtuella Azure-datorer pris som **betala endast för att du använder**. tooensure som du inte debiteras när du inte använder den virtuella datorn, den har toobe i hello **Stoppad (Deallocated)** tillstånd.

> [!NOTE]
> Stänger av hello virtuell dator från inuti (med Windows Energialternativ) hello VM stoppas men förblir allokerade. tooensure som du inte har faktureras, avbryter alltid virtuella datorer från hello [klassiska Azure-portalen](http://manage.windowsazure.com/). Du kan också avbryta hello VM via Powershell genom att anropa ShutdownRoleOperation med lika ”PostShutdownAction” för ”StoppedDeallocated”.
> 
> 

tooshutdown och frigör hello virtuell dator:

1. Logga in toohello [klassiska Azure-portalen](http://manage.windowsazure.com/) med ditt konto.  
2. Välj **VIRTUELLA datorer** från hello vänstra navigeringsfältet.
3. Hello av virtuella datorer på listan på hello namn för din virtuella datorn och sedan gå toohello **INSTRUMENTPANELEN** sidan.
4. Hello längst hello-sidan, klickar du på **avstängning**.

![Stäng VM][15]

hello virtuella datorn att frigöra men inte ta bort. Du kan starta om den virtuella datorn när som helst från hello klassiska Azure-portalen.

## <a name="your-azure-sql-server-vm-is-ready-toouse-whats-next"></a>Azure SQL Server-VM är klar toouse: Vad kommer härnäst?
Den virtuella datorn är nu redo toouse i dina datavetenskap övningarna. hello virtuella datorn är också redo för användning som en bärbar dator IPython server för hello undersökning och bearbetning av data och andra uppgifter tillsammans med Azure Machine Learning och hello Team Data vetenskap processen (TDSP).

hello nästa steg i processen för hello datavetenskap mappas i hello [Team datavetenskap Process](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) och kan innehålla steg som flytta data till HDInsight, process och exempel på den där inför learning från hello data med Azure-dator Learning.

[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png

