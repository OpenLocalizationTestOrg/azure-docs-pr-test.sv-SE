---
title: "Konfigurera en virtuell dator i SQL Server som en bärbar dator IPython server | Microsoft Docs"
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
ms.openlocfilehash: 8a151a6a15d4d000a774e3ec4e38bfa0e58ca33b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Konfigurera en virtuell dator i Azure SQL Server som en IPython Notebook-server för avancerade analyser
Det här avsnittet visar hur du etablera och konfigurera en virtuell dator i SQL Server som ska användas som en del av en molnbaserad datavetenskap miljö. Windows virtuell dator konfigureras med stödjande verktyg som IPython anteckningsboken, Azure Lagringsutforskaren AzCopy samt andra verktyg som är användbara för datavetenskap projekt. Azure Lagringsutforskaren och AzCopy, till exempel ange praktiskt sätt att överföra data till Azure blob storage från din lokala dator eller ladda ned den till den lokala datorn från blob storage.

Galleriet för virtuella Azure-datorn innehåller flera avbildningar som innehåller Microsoft SQL Server. Välj en SQL Server-VM-avbildning som passar dina databehov. Rekommenderade bilder är:

* SQL Server 2012 SP2 Enterprise för små till medelstora datamängder
* SQL Server 2012 SP2 Enterprise optimerade för DataWarehousing arbetsbelastningar för stor för att mycket stora datamängder
  
  > [!NOTE]
  > Bild av SQL Server 2012 SP2 Enterprise **innehåller inte en datadisk**. Du måste lägga till och/eller ansluta en eller flera virtuella hårddiskar för att lagra data. När du skapar en virtuell Azure-dator har en disk för operativsystemet som är mappad till enhet C och en tillfällig disk som är mappad till enhet D. Använd inte enhet D för att lagra data. Som namnet antyder ger tillfällig lagring. Det finns ingen redundans eller säkerhetskopiering eftersom den inte finns i Azure-lagring.
  > 
  > 

## <a name="Provision"></a>Ansluta till den klassiska Azure-portalen och etablera en virtuell dator med SQL Server
1. Logga in på den [Azure klassiska portal](http://manage.windowsazure.com/) med ditt konto.
   Om du inte har något Azure-konto besöker du sidan för [kostnadsfria utvärderingsversioner av Azure](https://azure.microsoft.com/pricing/free-trial/).
2. På den klassiska Azure-portalen, längst ned till vänster på sidan, klickar du på **+ ny**, klickar du på **COMPUTE**, klickar du på **VIRTUELLA**, och klicka sedan på **från GALLERIET** .
3. På den **skapa en virtuell dator** sidan, Välj en avbildning av virtuell dator med SQL Server baserat på dina databehov klicka sedan på nästa-pilen längst ned till höger på sidan. Den senaste informationen på de SQL Server-avbildningarna som stöds i Azure finns [komma igång med SQL Server i Azure Virtual Machines](http://go.microsoft.com/fwlink/p/?LinkId=294720) -avsnittet i den [SQL Server i Azure Virtual Machines](http://go.microsoft.com/fwlink/p/?LinkId=294719) dokumentationsuppsättningen.
   
   ![Välj SQLServer-dator][1]
4. Första **konfiguration av virtuell dator** anger du följande information:
   
   * Ange en **VIRTUELLT datornamn**.
   * I den **nytt användarnamn** rutan, unikt namn för det lokala administratörskontot för VM.
   * I den **nytt lösenord** skriver ett starkt lösenord. Mer information finns i [Starka lösenord](http://msdn.microsoft.com/library/ms161962.aspx).
   * I den **BEKRÄFTA lösenord** rutan, ange lösenordet igen.
   * Välj lämpliga **storlek** från nedrullningsbara listan.
     
     > [!NOTE]
     > Storleken på den virtuella datorn har angetts under etablering: A2 är den minsta storlek som rekommenderas för produktionsarbetsbelastningar. Den minsta rekommenderade storleken för en virtuell dator är A3 när du använder SQL Server Enterprise Edition. Välj A3 eller senare när du använder SQL Server Enterprise Edition. Välj A4 när du använder SQL Server 2012 eller 2014 Enterprise optimerade för transaktionell arbetsbelastningar bilder.
     > Välj A7 när du använder SQL Server 2012 eller 2014 Enterprise optimerad för Data Warehousing arbetsbelastningar bilder. Den storlek som valts begränsar antalet datadiskar som du kan konfigurera. Uppdaterad information om tillgängliga virtuella datorstorlekar och antalet datadiskar som kan bifogas till en virtuell dator finns [storlekar för virtuella datorer för Azure](http://msdn.microsoft.com/library/azure/dn197896.aspx). Information om priser finns [virtuella Macines priser](https://azure.microsoft.com/pricing/details/virtual-machines/).
     > 
     > 
   
   Klicka på pilen längst ned till höger fortsätta nästa.
   
   ![VM-konfiguration][2]
5. På andra **konfiguration av virtuell dator** konfigurerar resurser för nätverk, lagring och tillgänglighet:
   
   * I den **Molntjänsten** väljer **skapa en ny molntjänst**.
   * I den **DNS Molntjänstnamnet** ange den första delen av ett DNS-namn du själv väljer, så att den är klar ett namn i formatet **TESTNAME.cloudapp.net**
   * I den **REGION/TILLHÖRIGHET grupp/VIRTUELLT nätverk** väljer du en region där den här virtuella bilden kommer att finnas.
   * I den **Lagringskonto**väljer du ett befintligt lagringskonto eller en skapas automatiskt.
   * I den **TILLGÄNGLIGHETSUPPSÄTTNING** väljer **(ingen)**.
   * Läs och Godkänn prisinformation.
6. I den **SLUTPUNKTER** klickar du på den tomma nedrullningsbara under **namn**, och välj **MSSQL** Skriv portnumret på instansen av databasmotorn (**1433** för standardinstansen).
7. SQL Server-VM kan också fungera som en IPython anteckningsboken-Server som kommer att konfigureras i ett senare steg.
   Lägg till en ny slutpunkt för att ange porten som ska användas för servern IPython bärbar dator. Ange ett namn i den **namn** kolumn, väljer du ett portnummer för dina val för den offentliga porten och 9999 för den privata porten.
   
   Klicka på pilen längst ned till höger fortsätta nästa.
   
   ![Välj MSSQL och IPython portar][3]
8. Acceptera standardvärdet **installera VM-agenten** alternativet som är markerat och klicka på kryssmarkeringen i nederkant högra hörnet av guiden för att slutföra etableringsprocessen VM.
   
   `![Sista alternativen för VM][4]
9. Vänta medan Azure förbereder den virtuella datorn. Förväntar dig virtuella datorns status ska genomföras:
   
   * Starta (allokering)
   * Stoppad
   * Starta (allokering)
   * Kör (allokering)
   * Körs

## <a name="RemoteDesktop"></a>Öppna den virtuella datorn med fjärrskrivbord och fullständig installation
1. När etableringen är slutförd, klicka på namnet på den virtuella datorn att gå till sidan INSTRUMENTPANELEN. Längst ned på sidan klickar du på **Anslut**.
2. Välj att öppna filen rpd med hjälp av fjärrskrivbord för Windows-program (`%windir%\system32\mstsc.exe`).
3. På den **Windows-säkerhet** dialogrutan Ange lösenordet för det lokala administratörskontot som du angav tidigare.
   (Du kan bli ombedd att kontrollera autentiseringsuppgifterna för den virtuella datorn.)
4. Första gången du loggar in på den här virtuella datorn kan flera processer behöva utföra, inklusive inställningar för din desktop, Windows-uppdateringar och slutförande av Windows Inledande konfigurationsåtgärder (sysprep). När Windows sysprep är klar slutför installationen av SQL Server konfigurationsåtgärder. Dessa uppgifter kan orsaka en fördröjning på några minuter medan den är klar. `SELECT @@SERVERNAME`kan inte returnera rätt namn förrän SQL Server-installationen är klar och SQL Server Management Studio kanske inte visable på startsidan.

När du är ansluten till den virtuella datorn med Windows fjärrskrivbord fungerar ungefär som andra datorer i den virtuella datorn. Ansluta till standardinstansen av SQL Server med SQL Server Management Studio (som körs på den virtuella datorn) på normalt sätt.

## <a name="InstallIPython"></a>Installera IPython bärbara datorer och andra stödfiler verktyg
Om du vill konfigurera din nya SQL Server-VM för att fungera som en bärbar dator IPython server och installera ytterligare stödjande verktyg sådana AzCopy, Azure Lagringsutforskaren, användbara datavetenskap Python-paket och andra tillhandahålls skript särskild anpassning. Installera:

1. Högerklicka på den **Windows starta** ikonen och klickar på **kommandotolk (Admin)**
2. Kopiera följande kommandon och klistra in i Kommandotolken.
  
        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"
3. När du uppmanas du ange ett lösenord önskat för IPython anteckningsboken-servern.
4. Skriptet anpassning automatiseras flera efter installationen procedurer, bland annat:
    * Installationen av IPython anteckningsboken server
    * Öppna TCP-portar i Windows-brandväggen för de slutpunkter som skapade tidigare:
    * För SQL Server för fjärranslutningar
    * För fjärranslutna IPython anteckningsboken serveranslutning
    * Hämtar exempel IPython anteckningsböcker och SQL-skript
    * Hämta och installera användbara datavetenskap Python-paket
    * Hämta och installera Azure-verktyg, till exempel AzCopy och Azure Lagringsutforskaren  
    <br>
5. Du kan komma åt och köra IPython bärbar dator från en lokal eller fjärransluten webbläsare med en URL i formatet `https://<virtual_machine_DNS_name>:<port>`, där porten är den IPython offentliga du valt vid etablering av virtuell dator.
6. IPython anteckningsboken servern körs som en bakgrundstjänst och kommer att startas om automatiskt när du startar om den virtuella datorn.

## <a name="Optional"></a>Ansluta en datadisk efter behov
Du måste lägga till en eller flera datadiskar för att lagra data om VM-avbildning inte innehåller datadiskar, d.v.s. diskar än C-enheten (OS-disk) och enhet D (tillfällig disk). VM-avbildning för SQL Server 2012 SP2 Enterprise optimerade för DataWarehousing arbetsbelastningar kommer förkonfigurerade med ytterligare diskar för SQL Server data- och loggfiler.

> [!NOTE]
> Använd inte enhet D för att lagra data. Som namnet antyder ger tillfällig lagring. Det finns ingen redundans eller säkerhetskopiering eftersom den inte finns i Azure-lagring.
> 
> 

Om du vill koppla ytterligare hårddiskar, Följ stegen som beskrivs i [hur du ansluter en datadisk till en virtuell Windows-dator](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), som får du hjälp:

1. Koppling av disk(ar) tom till den virtuella datorn som har etablerats i tidigare steg
2. Initieringen av de nya diskarna på den virtuella datorn

## <a name="SSMS"></a>Ansluta till SQL Server Management Studio och aktivera autentisering för blandat läge
SQL Server-databasmotorn kan inte använda Windows-autentisering utan domänmiljö. Om du vill ansluta till databasmotorn från en annan dator konfigurerar du SQL Server för blandat läge-autentisering. Med blandat läge-autentisering tillåts både SQL Server-autentisering och Windows-autentisering. SQL-autentiseringsläge krävs för att mata in data direkt från din SQL Server-VM-databaser i den [Azure Machine Learning Studio](https://studio.azureml.net) med hjälp av modulen importera Data.

1. När du är ansluten till den virtuella datorn med hjälp av fjärrskrivbord kan du använda Windows **Sök** fönstret och skriv **SQL Server Management Studio** (SMSS). Klicka för att starta SQL Server Management Studio (SSMS). Du kanske vill lägga till en genväg till SSMS på skrivbordet för framtida användning.
   
   ![Starta SSMS][5]
   
   Första gången du öppnar Management Studio måste programmet skapa en Management Studio-miljö för användarna. Det kan ta en stund.
2. När du öppnar, Management Studio presenterar den **Anslut till Server** dialogrutan. I den **servernamn** skriver du namnet på den virtuella datorn ska ansluta till databasmotorn med Object Explorer.
   (I stället för namnet på virtuella datorn kan du också använda **(lokal)** eller en enskild period som den **servernamn**. Välj **Windows-autentisering**, och lämna  ***din\_VM\_namn*\\din\_lokala\_administratör**  i den **användarnamn** rutan. Klicka på **Anslut**.
   
   ![Anslut till server][6]
   
   <br>
   
   > [!TIP]
   > Du kan ändra SQL Server-autentiseringsläge med hjälp av en nyckel registerändring för Windows eller SQL Server Management Studio. Om du vill ändra läget för autentisering med hjälp av ändringen av registernyckeln, starta en **ny fråga** och kör följande skript:
   > 
   > 
   
       USE master
       go
   
       EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
       go

    Att ändra autentiseringsläget som använder SQL Server management Studio:

1. I **SQL Server Management Studio Object Explorer**, högerklicka på namnet på instansen av SQL Server (virtuella datornamn) och klicka sedan på **egenskaper**.
   
   ![Serveregenskaper][7]
2. Markera **Läge för SQL Server- och Windows-autentisering** under **Serverautentisering** på sidan **Säkerhet**. Klicka sedan på **OK**.
   
   ![Välja autentiseringsläge][8]
3. I den **SQL Server Management Studio** dialogrutan klickar du på **OK** bekräfta kravet på att starta om SQL Server.
4. I **Object Explorer**, högerklicka på din server och klicka sedan på **starta om**. (Om SQL Server Agent körs måste den också startas om.)
   
   ![Starta om][9]
5. I den **SQL Server Management Studio** dialogrutan klickar du på **Ja** accepterar du vill starta om SQL Server.

## <a name="Logins"></a>Skapa SQL Server-autentisering-inloggningar
För att kunna ansluta till databasmotorn från en annan dator måste du skapa minst en SQL Server-autentiseringsinloggning.  

Du kan skapa nya SQL Server-inloggningar programmässigt eller med hjälp av SQL Server Management Studio. Skapa en ny sysadmin-användare med SQL-autentisering via programmering genom att starta en **ny fråga** och kör följande skript. Ersätt < nytt användarnamn\> och < nytt lösenord\> med ditt val av *användarnamn* och *lösenord*. 

    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Justera lösenordsprincipen efter behov (i exemplet kod stänger av principen kontrollerar och lösenord upphör att gälla). Mer information om SQL Server-inloggningar finns i [Skapa en inloggning](http://msdn.microsoft.com/library/aa337562.aspx).  

Skapa ny SQL Server-inloggningar med hjälp av SQL Server Management Studio:

1. I **SQL Server Management Studio Object Explorer**, expandera mappen för den serverinstans där du vill skapa en ny inloggning.
2. Högerklicka på den **säkerhet** mappen, peka på **ny**, och välj **inloggning...** .
   
   ![Ny inloggning][10]
3. På sidan **Allmänt** i dialogrutan **Inloggning – ny** anger du namnet på den nya användaren i rutan **Inloggningsnamn**.
4. Välj **SQL Server-autentisering**.
5. Ange ett lösenord för den nya användaren i rutan **Lösenord**. Ange lösenordet på nytt i rutan **Bekräfta lösenord**.
6. Om du vill tillämpa principen lösenordsalternativ för komplexitet och tvingande, Välj **genomdriva lösenordsprinciper** (rekommenderas). Detta är en standardalternativet när SQL Server-autentisering har valts.
7. Om du vill tillämpa principen lösenordsalternativ för förfallodatum, Välj **genomdriva Lösenordets giltighetstid** (rekommenderas). Tillämpa princip för lösenord måste väljas för att aktivera den här kryssrutan. Detta är en standardalternativet när SQL Server-autentisering har valts.
8. Om du vill tvinga användaren att skapa ett nytt lösenord efter inloggningen används första gången, Välj **användaren måste byta lösenord vid nästa inloggning** (rekommenderas om inloggningen är att någon annan kan använda. Om inloggningen för eget bruk, Välj inte det här alternativet.) Framtvinga förfallotid för lösenord måste väljas för att aktivera den här kryssrutan. Detta är en standardalternativet när SQL Server-autentisering har valts.
9. Välj en standarddatabas för inloggningen i listan **Standarddatabas**. **Master** är standard för det här alternativet. Om du inte har skapat någon användardatabas ännu använder du standardvärdet **Master**.
10. I den **standardspråk** listan, lämna **standard** som värde.
    
    ![Inloggningsegenskaper][11]
11. Om det här är den första inloggningen som du skapar kanske du vill utse inloggningen till SQL Server-administratör. I så fall markerar du **sysadmin** på sidan **Serverroller**.
    
    > [!IMPORTANT]
    > Medlemmar i den fasta serverrollen sysadmin har fullständig kontroll över databasmotorn. Av säkerhetsskäl bör du noggrant begränsa medlemskap i den här rollen.
    > 
    > 
    
    ![sysadmin][12]
12. Klicka på OK.

## <a name="DNS"></a>Bestämma DNS-namnet på den virtuella datorn
För att kunna ansluta till SQL Server-databasmotorn från en annan dator måste du känna till DNS-namnet (Domain Name System) för den virtuella datorn.

(Detta är det namn som används för att identifiera den virtuella datorn på Internet. Du kan använda IP-adressen, men IP-adress kan ändras när Azure flyttar resurser för redundans eller underhåll. DNS-namnet är stabilt eftersom det kan omdirigeras till en ny IP-adress.)

1. I den klassiska Azure-portalen (eller från föregående steg), Välj **VIRTUELLA datorer**.
2. På den **VIRTUELLA DATORINSTANSER** sidan den **DNS-namnet** kolumn, hitta och kopiera DNS-namn för den virtuella datorn som visas föregås av **http://**. (Användargränssnittet kanske inte visar hela namnet, men du kan högerklicka på den och välj Kopiera.)

## <a name="cde"></a>Anslut till databasmotorn från en annan dator
1. Öppna SQL Server Management Studio på en dator som är ansluten till Internet.
2. I den **Anslut till Server** eller **Anslut till databasmotor** i dialogrutan den **servernamn** ange DNS-namnet på den virtuella datorn (bestäms i föregående åtgärd) och en offentlig slutpunkt portnummer i formatet *DNSName, portnumber* som **tutorialtestVM.cloudapp.net,57500**.
3. I rutan **Autentisering**, markerar du **SQL Server-autentisering**.
4. I rutan **Inloggning** skriver du namnet på en inloggning som du har skapat i en tidigare uppgift.
5. I rutan **Lösenord** skriver du lösenordet för den inloggning som du skapade i en tidigare uppgift.
6. Klicka på **Anslut**.

## <a name="amlconnect"></a>Anslut till databasmotorn från Azure Machine Learning
I senare steg i Team av vetenskapliga data använder du den [Azure Machine Learning Studio](https://studio.azureml.net) att skapa och distribuera machine learning-modeller. För att mata in data från SQL Server-VM-databaser direkt i Azure Machine Learning utbildning eller bedömningen använder den **importera Data** modul i en ny [Azure Machine Learning Studio](https://studio.azureml.net) experiment. Det här avsnittet beskrivs mer detaljerat via Team datavetenskap Process guiden länkar. En introduktion finns [vad är Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

1. I den **egenskaper** rutan i den [importera Data modulen](https://msdn.microsoft.com/library/azure/dn905997.aspx)väljer **Azure SQL Database** från den **datakällan** listrutan.
2. I den **Databasservernamnet** text Ange`tcp:<DNS name of your virtual machine>,1433`
3. Ange namnet på SQL-användaren i den **Server användarkontonamnet** textruta.
4. Ange lösenordet för den sql-användaren i den **serverlösenord** textruta.
   
   ![Azure Machine Learning importera Data][13]

## <a name="shutdown"></a>Avstängning och frigör virtuell dator som
Virtuella Azure-datorer pris som **betala endast för att du använder**. För att säkerställa att du inte faktureras när du inte använder den virtuella datorn, den måste anges i den **Stoppad (Deallocated)** tillstånd.

> [!NOTE]
> Stänger av den virtuella datorn från inuti (med Windows Energialternativ), den virtuella datorn stoppas men förblir allokerade. Om du vill se till att du inte är faktureras alltid stoppa virtuella datorer från den [klassiska Azure-portalen](http://manage.windowsazure.com/). Du kan också stoppa den virtuella datorn via Powershell genom att anropa ShutdowRoleOperation med PostShutdownAction lika med StoppedDeallocated.
> 
> 

Avstängning och frigör den virtuella datorn:

1. Logga in på den [klassiska Azure-portalen](http://manage.windowsazure.com/) med ditt konto.  
2. Välj **VIRTUELLA datorer** från det vänstra navigeringsfältet.
3. I listan över virtuella datorer, klicka på namnet på den virtuella datorn och gå sedan till den **INSTRUMENTPANELEN** sidan.
4. Längst ned på sidan klickar du på **avstängning**.

![Stäng VM][15]

Den virtuella datorn att frigöra men inte ta bort. Du kan starta om den virtuella datorn när som helst från den klassiska Azure-portalen.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Azure SQL Server-VM är redo att användas: Vad kommer härnäst?
Den virtuella datorn är nu redo att användas i dina datavetenskap övningarna. Den virtuella datorn är också redo för användning som en bärbar dator IPython-server för undersökning och bearbetning av data och andra uppgifter tillsammans med Azure Machine Learning och Team Data vetenskap processen (TDSP).

Nästa steg i processen för datavetenskap mappas i den [Team datavetenskap Process](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) och kan innehålla steg som flytta data till HDInsight, process och exempel på den där inför learning från data med Azure Machine Learning .

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

