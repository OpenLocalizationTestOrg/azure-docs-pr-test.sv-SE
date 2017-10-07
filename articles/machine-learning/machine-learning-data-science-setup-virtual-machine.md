---
title: "aaaSet av en virtuell dator som en bärbar dator IPython server | Microsoft Docs"
description: "Ställ in en Azure-dator för användning i en miljö med vetenskapliga data med IPython Server för avancerade analyser."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 818617c1-048e-49c2-b941-f9a983f93998
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: xibingao;bradsev
ms.openlocfilehash: 58386140ec7742ade1f7e183ec842a2b09b9dfca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-an-azure-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Konfigurera en virtuell dator i Azure som en IPython Notebook-server för avancerade analyser
Det här avsnittet visar hur tooprovision och konfigurera en virtuell Azure-dator för avancerade analyser som kan användas som en del av en datavetenskap miljö. hello Windows virtuell dator konfigureras med stödjande verktyg som IPython anteckningsboken, Azure Lagringsutforskaren, AzCopy samt andra verktyg som är användbara för avancerade analyser projekt. Azure Lagringsutforskaren och AzCopy, till exempel ange praktiskt sätt tooupload data tooAzure blob storage från din lokala dator eller toodownload den tooyour lokala datorn från blob storage.

## <a name="create-vm"></a>Steg 1: Skapa en generell Azure virtuell dator
Om du redan har en virtuell Azure-dator och bara vill tooset in en bärbar dator IPython server på den, kan du hoppa över detta steg och fortsätta för[steg 2: Lägg till en slutpunkt för IPython anteckningsböcker tooan befintlig virtuell dator](#add-endpoint).

Innan du börjar hello för att skapa en virtuell dator på Azure, måste toodetermine hello storleken på hello datorn som är nödvändiga tooprocess hello data för deras projekt. Mindre datorer har mindre minne och färre CPU-kärnor än större datorer, men de är också billigare. En lista över datortyper och priser finns hello <a href="http://azure.microsoft.com/pricing/details/virtual-machines/" target="_blank">prissättning för Virtual Machines </a> sidan

1. Logga in för<a href="https://manage.windowsazure.com" target="_blank">klassiska Azure-portalen</a>, och klicka på **ny** i hello nedre vänstra hörnet. Ett fönster visas. Välj **COMPUTE** -> **VIRTUELLA** -> **från GALLERIET**.
   
    ![Skapa arbetsyta][24]
2. Välj något av följande avbildningar hello:
   
   * Windows Server 2012 R2 Datacenter
   * Windows Server Essentials Experience (Windows Server 2012 R2)
     
     Klicka på hello pilen pekar åt höger på hello nedre högra toogo hello nästa konfigurationssidan.
     
     ![Skapa arbetsyta][25]
3. Ange ett namn för hello virtuell dator som du vill toocreate, Välj hello storleken på hello datorn (standard: A3) baserat på hello storleken på hello data hello datorn är pågående tooprocess och hur kraftfulla önskat hello datorn toobe (minne storlek och hello antal beräkning kärnor) , ange ett användarnamn och lösenord för hello-datorn. Klicka på hello pilen pekar rätt toogo toohello nästa konfigurationssidan.
   
    ![Skapa arbetsyta][26]
4. Välj hello **REGION/TILLHÖRIGHET grupp/VIRTUELLT nätverk** som innehåller hello **LAGRINGSKONTO** att du planerar toouse för den virtuella datorn och välj sedan det storage-kontot. Lägga till en slutpunkt längst ned hello i hello **SLUTPUNKTER** fältet genom att ange hello namnet på hello slutpunkt (”IPython” här). Du kan välja valfri sträng som hello **namn** hello slutpunkt och ett heltal mellan 0 och 65536 som är **tillgängliga** som hello **offentlig PORT**. Hej **privat PORT** har toobe **9999**. Du bör **undvika** med hjälp av offentliga portar som redan har tilldelats för internet-tjänster. <a href="http://www.chebucto.ns.ca/~rakerman/port-table.html" target="_blank">Portar för Internettjänster</a> innehåller en lista över portar som har tilldelats och bör undvikas.
   
    ![Skapa arbetsyta][27]
5. Klicka på hello markerat toostart hello virtuella etableringsprocessen.
   
    ![Skapa arbetsyta][28]

Det kan ta 15 25 minuter toocomplete hello virtuella etableringsprocessen. När hello virtuella datorn har skapats, hello statusen för den här datorn ska visa **kör**.

![Skapa arbetsyta][29]

## <a name="add-endpoint"></a>Steg 2: Lägg till en slutpunkt för IPython anteckningsböcker tooan befintlig virtuell dator
Om du har skapat hello virtuella datorn genom att följa hello anvisningar i steg 1 hello slutpunkt för IPython anteckningsboken redan har lagts till och det här steget kan hoppas över.

Om hello virtuella datorn finns redan och behöver du tooadd en slutpunkt för IPython bärbar dator som du ska installera i steg3 nedan, första inloggningen tooAzure klassiska portal, väljer hello virtuell dator och Lägg till hello slutpunkt för IPython anteckningsboken server. hello innehåller följande bild en skärmbild av hello portal efter hello slutpunkt för IPython anteckningsboken har lagts till tooa Windows-dator.

![Skapa arbetsyta][17]

## <a name="run-commands"></a>Steg 3: Installera IPython bärbara datorer och andra stödfiler verktyg
När hello virtuell dator har skapats kan du använda Remote Desktop Protocol (RDP) toolog på toohello Windows-dator. Instruktioner finns i [hur tooLog på tooa virtuell dator kör Windows Server](../virtual-machines/windows/classic/connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Öppna hello **kommandotolk** (**inte hello Powershell-kommandofönster**) som en **administratör** och hello kör följande kommando.

    set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"

När hello installationen är klar hello IPython anteckningsboken server startas automatiskt i hello *C:\\användare\\\<användarnamn\>\\dokument\\IPython Anteckningsböcker* directory.

När du uppmanas ange ett lösenord för hello IPython anteckningsboken och hello hello datorn administratör. Detta gör hello IPython anteckningsboken toorun som en tjänst på hello-datorn.

## <a name="access"></a>Steg 4: Åtkomst IPython bärbara datorer från en webbläsare
tooaccess hello IPython anteckningsboken server, öppna en webbplats webbläsaren och indata *https://&#60;virtual datorn DNS-namn >: &#60; offentliga portnumret >* i hello URL-textrutan. Här hello *&#60; offentliga portnumret >* ska vara hello-portnummer som du angav när hello IPython anteckningsboken slutpunkt har lagts till.

Hej *&#60; virtuella DNS-namn >* finns på hello klassiska Azure-portalen. När du loggar in toohello klassiska portalen klickar du på **VIRTUELLA datorer**väljer hello datorn du har skapat och välj sedan **INSTRUMENTPANELEN**, hello DNS-namnet visas på följande sätt:

![Skapa arbetsyta][19]

Du kommer märka en varning om att *det är problem med webbplatsens säkerhetscertifikat* (Internet Explorer) eller *anslutningen är inte privat* (Chrome), enligt följande hello siffror. Klicka på **Fortsätt toothis webbplats (rekommenderas inte)** (Internet Explorer) eller **Avancerat** och sedan  **fortsätta för &#60;* DNS-namnet*> (osäkra) ** toocontinue (Chrome). Ange sedan hello-lösenord som du specificerade tidigare tooaccess hello IPython bärbara datorer.

**Internet Explorer:**
![Skapa arbetsyta][20]

**Chrome:**
![Skapa arbetsyta][21]

När du loggar in toohello IPython bärbar dator, en katalog *DataScienceSamples* visar hello webbläsare. Den här katalogen innehåller exempel IPython anteckningsböcker som delas av toohelp användare genomföra datavetenskap uppgifter. Dessa exempel IPython anteckningsböcker har checkats ut från [ **GitHub-lagringsplatsen** ](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/DataScienceProcess/iPythonNotebooks) toohello virtuella datorer under hello IPython anteckningsboken server-installationen. Microsoft underhåller och uppdaterar databasen ofta. Användarna kan besöka hello GitHub-lagringsplatsen tooget hello senast uppdaterade exempel IPython bärbara datorer.
![Skapa arbetsyta][18]

## <a name="upload"></a>Steg 5: Överför en befintlig IPython anteckningsbok från en lokal dator toohello IPython anteckningsboken server
IPython bärbara datorer är enkelt för användare tooupload en befintlig IPython anteckningsbok på sina lokala datorer toohello IPython anteckningsboken server på hello virtuella datorer. När du loggar in toohello IPython bärbar dator i en webbläsare, klickar du på i hello **directory** som hello IPython bärbar dator som ska överföras till. Markera en bärbar dator IPython .ipynb filen tooupload från hello lokal dator i hello **Utforskaren**, och dra och släppa toohello IPython anteckningsboken directory på hello webbläsare. Klicka på hello **överför** knappen tooupload hello .ipynb filen toohello IPython anteckningsboken server. Andra användare kan sedan börja använda den i från sina webbläsare.

![Skapa arbetsyta][22]

![Skapa arbetsyta][23]

## <a name="shutdown"></a>Stäng av och frigör virtuell dator som
Virtuella Azure-datorer pris som **betala endast för att du använder**. tooensure som du inte debiteras när du inte använder den virtuella datorn, den har toobe i hello **Stoppad (Deallocated)** tillstånd inte under användning.

> [!NOTE]
> Om du stänger av hello virtuell dator från inuti hello VM (med Windows Energialternativ), hello VM stoppas, men förblir allokerade. tooensure toobe debiteras, inte Fortsätt alltid stoppa virtuella datorer från hello [klassiska Azure-portalen](http://manage.windowsazure.com/). Du kan också avbryta hello VM via Powershell genom att anropa **ShutdownRoleOperation** ”PostShutdownAction” lika för ”StoppedDeallocated”.
> 
> 

tooshut ned och frigör hello virtuell dator:

1. Logga in toohello [klassiska Azure-portalen](http://manage.windowsazure.com/) med ditt konto.  
2. Välj **VIRTUELLA datorer** från hello vänstra navigeringsfältet.
3. Hello av virtuella datorer på listan på hello namn för din virtuella datorn och sedan gå toohello **INSTRUMENTPANELEN** sidan.
4. Hello längst hello-sidan, klickar du på **avstängning**.

![Stäng VM][15]

hello virtuella datorn att frigöra men inte ta bort. Du kan starta om den virtuella datorn när som helst från hello klassiska Azure-portalen.

## <a name="your-azure-vm-is-ready-toouse-whats-next"></a>Azure VM är klar toouse: Vad kommer härnäst?
Den virtuella datorn är nu redo toouse i dina datavetenskap övningarna. hello virtuella datorn är också redo för användning som en bärbar dator IPython server för hello undersökning och bearbetning av data och andra uppgifter tillsammans med Azure Machine Learning och hello Team datavetenskap Process.

hello nästa steg i hello Team av vetenskapliga data mappas i hello [Learning sökväg](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) och kan innehålla steg som flytta data till HDInsight, process och exempel på den där inför learning från hello data med Azure-dator Learning.

[15]: ./media/machine-learning-data-science-setup-virtual-machine/vmshutdown.png
[17]: ./media/machine-learning-data-science-setup-virtual-machine/add-endpoints-after-creation.png
[18]: ./media/machine-learning-data-science-setup-virtual-machine/sample-ipnbs.png
[19]: ./media/machine-learning-data-science-setup-virtual-machine/dns-name-and-host-name.png
[20]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning-ie.png
[21]: ./media/machine-learning-data-science-setup-virtual-machine/browser-warning.png
[22]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-1.png
[23]: ./media/machine-learning-data-science-setup-virtual-machine/upload-ipnb-2.png
[24]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-1.png
[25]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-2.png
[26]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-3.png
[27]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-4.png
[28]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-5.png
[29]: ./media/machine-learning-data-science-setup-virtual-machine/create-virtual-machine-6.png
