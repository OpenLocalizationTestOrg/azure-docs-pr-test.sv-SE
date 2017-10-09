---
title: "aaaCreate bärbar dator med en Jupyter/IPython | Microsoft Docs"
description: "Lär dig hur du skapar toodeploy hello Jupyter/IPython bärbar dator på en virtuell Linux-dator med hello resource manager-distributionsmodellen i Azure."
services: virtual-machines-linux
documentationcenter: python
author: crwilcox
manager: wpickett
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 519f36dd-865e-4c1d-abe7-b87037796aa7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: python
ms.topic: article
ms.date: 11/10/2015
ms.author: crwilcox
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d7f2e45a8ba95163ebfb0f10babc91a2b3fd9390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-azure-vm-installing-jupyter-and-running-a-jupyter-notebook-on-azure"></a>Skapa en Azure VM, installera Jupyter och kör en Jupyter-anteckningsbok på Azure
Hej [Jupyter-projektet](http://jupyter.org), tidigare hello [IPython projekt](http://ipython.org), tillhandahåller en samling med verktyg för vetenskapliga datoranvändning med kraftfulla interaktiva tankar som kombinerar kodkörning med hello skapandet av en levande beräkningar dokument. De här filerna kan innehålla godtycklig text, matematiska formler, inkommande kod, resultat, bilder, videor och andra typer av media som en modern webbläsare kan visa. Om du är helt nya tooPython och vill toolearn den i en roliga, interaktiv miljö eller gör vissa allvarliga parallell-tekniska datoranvändning, hello Jupyter-anteckningsbok är ett bra alternativ.

![Skärmbild](./media/jupyter-notebook/ipy-notebook-spectral.png) med SciPy och Matplotlib paket tooanalyze hello strukturen i en inspelning av ljud.

## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter två sätt: Azure-datorer eller anpassad distribution
Azure tillhandahåller en tjänst som du kan använda för[snabbt kommer igång med Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Genom att använda hello Azure anteckningsboken Service kan du enkelt få åtkomst tooJupyter webben gränssnittet till skalbara resurser med alla hello ström av Python och dess många bibliotek.  Eftersom hello installation hanteras av hello-tjänsten kan användare komma åt dessa resurser utan hello behovet av administration och konfiguration av hello användare.

Om hello anteckningsboken tjänsten inte fungerar för ditt scenario Fortsätt tooread den här artikeln som ska visas hur toodeploy hello Jupyter Notebook i Microsoft Azure med hjälp av virtuella Linux-datorer (VM).

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Skapa och konfigurera en virtuell dator på Azure
hello första steget är toocreate en virtuell dator (VM) som körs på Azure.
Den här virtuella datorn är en hela operativsystemet i hello molnet och används för att köra hello Jupyter-anteckningsbok. Azure kan både Linux och Windows-datorer som körs och tar vi upp hello installationen av Jupyter på båda typer av virtuella datorer.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Skapa en Linux VM och öppnar en port för Jupyter
Följ instruktionerna för hello [här] [ portal-vm-linux] toocreate en virtuell dator för hello *Ubuntu* distribution. Den här kursen använder Ubuntu Server 14.04 LTS. Hello användarnamn antar vi *azureuser*.

När hello virtuell dator distribuerar måste vi tooopen upp en säkerhetsregel på hello nätverkssäkerhetsgruppen.  Hello Azure-portalen, gå för**Nätverkssäkerhetsgrupper** och öppna hello fliken för hello säkerhetsgrupp motsvarande tooyour VM. Du behöver tooadd en regel för inkommande säkerhet med hello följande inställningar: **TCP** för hello protokollet  **\***  för hello-källport (offentlig) och **9999** för hello-målport (privat).

![Skärmbild](./media/jupyter-notebook/azure-add-endpoint.png)

I din Nätverkssäkerhetsgruppen klickar du på **nätverksgränssnitt** och Observera hello **offentliga IP-adressen** eftersom den nödvändiga tooconnect tooyour VM i hello nästa steg.

## <a name="install-required-software-on-hello-vm"></a>Installera nödvändig programvara på hello VM
toorun Hej Jupyter-anteckningsbok på vår VM, vi måste först installera Jupyter och dess beroenden. Anslut tooyour virtuell linux-dator med hjälp av ssh och hello användarnamn/lösenord par som du valde när du skapade hello vm. I den här självstudiekursen kommer vi använda PuTTY och ansluta från Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Installera Jupyter på Ubuntu
Installera Anaconda, en populär datavetenskap python-distribution, med någon av hello länkarna från [produktserier Analytics](https://www.continuum.io/downloads).  Från och med hello skrivning av det här dokumentet är hello länkarna nedan hello mest upp toodate versioner.

#### <a name="anaconda-installs-for-linux"></a>Anaconda installerar för Linux
<table>
  <th>Python 3.4</th>
  <th>Python 2.7</th>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64-bitars</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64-bitars</href>
    </td>
  </tr>
  <tr>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32-bitars</href>
    </td>
    <td>
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32-bitars</href>
    </td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a>Installera Anaconda3 2.3.0 64-bitars på Ubuntu
Detta är hur du kan installera Anaconda på Ubuntu kan t.ex.

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter toohello latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Skärmbild](./media/jupyter-notebook/anaconda-install.png)

### <a name="configuring-jupyter-and-using-ssl"></a>Konfigurera Jupyter och använda SSL
När du har installerat behöver vi tootake ett ögonblick toosetup hello konfigurationsfiler för Jupyter. Om du upplever troubles med att konfigurera Jupyter kan det vara bra toolook på hello [Jupyter-dokumentation för att köra en bärbar dator Server](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).

Nästa vi `cd` toohello profilen directory toocreate våra SSL-certifikat och redigera konfigurationsfilen för hello profiler.

Använd följande kommando hello på Linux.

    cd ~/.jupyter

Använd hello efter kommandot toocreate hello SSL-certifikat (Linux och Windows).

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Observera att eftersom vi skapar ett självsignerat SSL-certifikat när du ansluter toohello anteckningsboken webbläsaren ger dig en säkerhetsvarning.  För långsiktig produktion ska toouse ett felaktigt signerade certifikat som är associerade med din organisation.  Eftersom certifikathantering ligger utanför den här demon hello, kommer vi behålla tooa självsignerat certifikat för tillfället.

Dessutom toousing ett certifikat, måste du även ange ett lösenord tooprotect anteckningsboken från obehörig användning.  Av säkerhetsskäl Jupyter använder krypterade lösenord i dess konfigurationsfil, så du behöver tooencrypt lösenordet först.  IPython ger en verktyget toodo. Kör hello följande kommando i Kommandotolken.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Detta kommer att fråga efter en lösenordet och bekräftelsen och skriver sedan ut hello lösenord. Observera att detta för hello följande steg.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided hello rest for security)

Sedan redigerar vi hello profil konfigurationsfil, vilket är den `jupyter_notebook_config.py` fil i hello du befinner dig i.  Observera att den här filen inte finns--bara skapa om så är fallet hello.  Den här filen har ett antal fält och som standard alla är kommenterade.  Du kan öppna den här filen med en textredigerare för dina önskemål och bör du kontrollera att den har minst hello följande innehåll. **Vara säker på att tooreplace hello c.NotebookApp.password i hello config med hello sha1 hello föregående steg**.

    c = get_config()

    # You must give hello path toohello certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-hello-jupyter-notebook"></a>Kör hello Jupyter-anteckningsbok
Vi är nu redo toostart hello Jupyter-anteckningsbok. toodo detta, navigera toohello directory du vill använda toostore anteckningsböcker och börja hello Jupyter-anteckningsbok server med hello följande kommando.

    /anaconda3/bin/jupyter-notebook

Du bör nu vara kan tooaccess Jupyter-anteckningsbok på hello adressen `https://[PUBLIC-IP-ADDRESS]:9999`.

När du först åtkomst till din bärbara dator frågar hello inloggningssidan för ditt lösenord. Och när du loggar in ser hello ”Jupyter-anteckningsbok instrumentpanel”, vilket är roll center för alla åtgärder för bärbar dator.  Du kan skapa nya bärbara datorer och öppna befintliga från den här sidan.

![Skärmbild](./media/jupyter-notebook/jupyter-tree-view.png)

### <a name="using-hello-jupyter-notebook"></a>Med hjälp av hello Jupyter-anteckningsbok
Om du klickar på hello **ny** knappen, visas hello följande öppningssidan.

![Skärmbild](./media/jupyter-notebook/jupyter-untitled-notebook.png)

hello område som markerats med en `In []:` är fråga hello inkommande område, och här kan du ange en giltig Python-kod och den kommer att köras när du klickar på `Shift-Enter` eller klicka på ikonen för hello ”spela upp” (hello pekar åt höger triangel i hello verktygsfältet).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>En kraftfull paradigmet: live beräkningar dokument med omfattande media
hello anteckningsboken själva bör känna sig mycket fysiska tooanyone som har använt Python och ett ordbehandlingsprogram eftersom det på vissa sätt är en blandning av båda: du kan köra kodblock Python, men du kan också hålla anteckningar och annan text genom att ändra hello formatet för en cell från ”kod” för ”Ma rkdown ”använda hello nedrullningsbara menyn i verktygsfältet.

Jupyter är mycket mer än ett ordbehandlingsprogram som tillåter blandning av beräkningar och omfattande media (text, bilder, video och praktiken vad en modern webbläsare kan visa). Du kan blanda, text, kod, videor och mer!

![Skärmbild](./media/jupyter-notebook/jupyter-editing-experience.png)

Och med hello kraften i Pythons många utmärkt bibliotek för vetenskapliga och tekniska databehandling i hello följande skärmbild, en enkel beräkning kan utföras med hello samma underlättar som en komplexa nätverk analys, allt i en miljö.

Den här paradigmet blanda hello kraften hos hello moderna web med levande beräkning erbjuder många möjligheter och är idealisk för hello molnet; hello anteckningsboken kan användas:

* Som en undersökande beräkningar anteckningsblocket toorecord fungera på ett problem.
* tooshare resulterar med kollegor i ”live” beräkningar formulär eller i papperskopia format (HTML-, PDF).
* toodistribute och finns live lärare material som rör beräkning, så studenter kan omedelbart experimentera med hello verkliga kod, ändra den och kör det igen interaktivt.
* tooprovide ”körbara papper” som presenterar hello resultaten av forskning på ett sätt som kan återges omedelbart, verifiera och utökas med andra.
* Som en plattform för samarbetsfunktioner datoranvändning: flera användare kan logga in toothe samma anteckningsboken server tooshare en levande beräkningar session.

Om du går källkoden för toohello IPython [databasen][repository], hittar du en hel katalog med exempel på bärbara datorer som du kan hämta och sedan experimentera med på din egen Azure Jupyter-VM.  Bara hämta hello `.ipynb` filer från hello plats och överför dem till hello instrumentpanelen i anteckningsboken Azure VM (eller hämta dem direkt i hello VM).

## <a name="conclusion"></a>Slutsats
Hej Jupyter-anteckningsbok erbjuder ett kraftfullt gränssnitt för att komma åt interaktivt hello kraften hos hello Python-ekosystemet i Azure.  Den omfattar en mängd olika användning fall inklusive enkel undersökning och Python, dataanalys och visualisering, simulering och parallell datorbearbetning. hello resulterande anteckningsboken dokument innehåller en fullständig hello beräkningar som utförs och kan delas med andra användare i Jupyter-post.  Hej Jupyter-anteckningsbok kan användas som ett lokalt program, men den är idealisk för molndistributioner på Azure

hello kärnfunktioner Jupyter finns också tillgängliga i Visual Studio via den [Python Tools för Visual Studio] [ Python Tools for Visual Studio] (PTVS). PTVS är ett kostnadsfritt och integrering på plugin-program från Microsoft som blir en avancerad Python-utvecklingsmiljö som innehåller en redigeraren med IntelliSense, felsökning, Visual Studio profilering och parallella för öppen källkod.

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello [Python Developer Center](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
