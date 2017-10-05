---
title: Skapa en Jupyter/IPython anteckningsbok | Microsoft Docs
description: "Lär dig hur du distribuerar Jupyter/IPython anteckningsbok på en Linux-dator som skapats med resource manager-distributionsmodellen i Azure."
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
ms.openlocfilehash: b5940190822cd5c5b78ea0e8f5c8695608d351d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="creating-an-azure-vm-installing-jupyter-and-running-a-jupyter-notebook-on-azure"></a>Skapa en Azure VM, installera Jupyter och kör en Jupyter-anteckningsbok på Azure
Den [Jupyter projekt](http://jupyter.org), tidigare i [IPython projekt](http://ipython.org), tillhandahåller en samling med verktyg för vetenskapliga datoranvändning med kraftfulla interaktiva tankar som kombinerar kodkörning med skapandet av en live beräkningar dokument. De här filerna kan innehålla godtycklig text, matematiska formler, inkommande kod, resultat, bilder, videor och andra typer av media som en modern webbläsare kan visa. Om du inte har absolut använt Python och vill veta i en miljö med roliga, interaktiva eller göra vissa allvarliga parallell-tekniska computing är Jupyter-anteckningsbok ett bra alternativ.

![Skärmbild](./media/jupyter-notebook/ipy-notebook-spectral.png) med SciPy och Matplotlib paket att analysera strukturen för en inspelning av ljud.

## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a>Jupyter två sätt: Azure-datorer eller anpassad distribution
Azure tillhandahåller en tjänst som du kan använda för att [snabbt kommer igång med Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).  Genom att använda tjänsten Azure bärbar dator kan få du enkelt tillgång till Jupyters webben gränssnitt för skalbara resurser med alla kraften i Python och dess många bibliotek.  Eftersom installationen hanteras av tjänsten kan användare komma åt dessa resurser utan att behöva administration och konfiguration av användaren.

Om tjänsten anteckningsboken inte fungerar för ditt scenario fortsätter du till den här artikeln som kommer visar hur du distribuerar Jupyter Notebook i Microsoft Azure med hjälp av virtuella Linux-datorer (VM).

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a>Skapa och konfigurera en virtuell dator på Azure
Det första steget är att skapa en virtuell dator (VM) som körs på Azure.
Den här virtuella datorn är en hela operativsystemet i molnet och används för att köra Jupyter-anteckningsboken. Azure kan både Linux och Windows-datorer som körs och tar vi upp installationen av Jupyter på båda typer av virtuella datorer.

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a>Skapa en Linux VM och öppnar en port för Jupyter
Följ instruktionerna [här] [ portal-vm-linux] att skapa en virtuell dator av den *Ubuntu* distribution. Den här kursen använder Ubuntu Server 14.04 LTS. Användarnamnet antar vi *azureuser*.

När du distribuerar den virtuella datorn som vi behöver öppna en säkerhetsregel på nätverkssäkerhetsgruppen.  Från Azure-portalen går du till **Nätverkssäkerhetsgrupper** och öppna fliken för säkerhetsgruppen som motsvarar den virtuella datorn. Du måste lägga till en regel för inkommande säkerhet med följande inställningar: **TCP** för protokollet  **\***  för källport (offentlig) och **9999** för den Målport (privat).

![Skärmbild](./media/jupyter-notebook/azure-add-endpoint.png)

I din Nätverkssäkerhetsgruppen klickar du på **nätverksgränssnitt** och notera den **offentliga IP-adressen** eftersom det krävs för att ansluta till den virtuella datorn i nästa steg.

## <a name="install-required-software-on-the-vm"></a>Installera nödvändiga program på den virtuella datorn
Om du vill köra Jupyter-anteckningsbok på vår VM installera vi först Jupyter och dess beroenden. Anslut till din linux vm med ssh och det användarnamn/lösenordet koppla du valde när du skapade den virtuella datorn. I den här självstudiekursen kommer vi använda PuTTY och ansluta från Windows.

### <a name="installing-jupyter-on-ubuntu"></a>Installera Jupyter på Ubuntu
Installera Anaconda, en populär datavetenskap python-distribution, med hjälp av en av länkarna från [produktserier Analytics](https://www.continuum.io/downloads).  Från och med skrivning av det här dokumentet på länkarna nedan om du är mest datum versioner.

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

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Skärmbild](./media/jupyter-notebook/anaconda-install.png)

### <a name="configuring-jupyter-and-using-ssl"></a>Konfigurera Jupyter och använda SSL
Vi behöver ta en stund att konfigurera konfigurationsfilerna för Jupyter när du har installerat. Om det uppstår troubles med att konfigurera Jupyter kan det vara bra att titta på den [Jupyter-dokumentation för att köra en bärbar dator Server](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).

Nästa vi `cd` till profilkatalogen för att skapa våra SSL-certifikat och redigera konfigurationsfilen profiler.

Använd följande kommando på Linux.

    cd ~/.jupyter

Använd följande kommando för att skapa SSL-certifikatet (Linux och Windows).

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

Observera att eftersom vi skapar ett självsignerat SSL-certifikat när du ansluter till den bärbara datorn webbläsaren ger dig en säkerhetsvarning.  För långsiktig produktion ska du vill använda ett felaktigt signerade certifikat som är associerade med din organisation.  Eftersom certifikathantering är utanför omfattningen för den här demon, kommer vi använda ett självsignerat certifikat för tillfället.

Förutom att använda ett certifikat kan ange du också ett lösenord för att skydda din bärbara dator mot obehörig användning.  Av säkerhetsskäl använder Jupyter krypterade lösenord i dess konfigurationsfil, så du behöver att kryptera lösenordet först.  IPython ger ett verktyg för att göra det. Kör följande kommando i Kommandotolken.

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

Detta uppmanas du att ett lösenord och bekräfta och skriver sedan ut lösenordet. Observera att detta för följande steg.

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

Därefter kommer vi att redigera konfigurationsfilen för den profil som är den `jupyter_notebook_config.py` filen i katalogen i.  Observera att den här filen inte finns--bara skapa om så är fallet.  Den här filen har ett antal fält och som standard alla är kommenterade.  Du kan öppna den här filen med en textredigerare för dina önskemål och bör du kontrollera att den har minst följande innehåll. **Se till att ersätta c.NotebookApp.password i konfigurationen med sha1 från föregående steg**.

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a>Kör Jupyter-anteckningsbok
Vi är nu redo att börja Jupyter-anteckningsboken. Gör detta genom att gå till den katalog som du vill lagra notebooks i och starta om servern för Jupyter-anteckningsbok med följande kommando.

    /anaconda3/bin/jupyter-notebook

Du ska nu kunna åtkomst till Jupyter-anteckningsbok på adressen `https://[PUBLIC-IP-ADDRESS]:9999`.

När du först åtkomst till din bärbara dator frågar inloggningssidan för ditt lösenord. Och när du loggar in ser ”Jupyter-anteckningsbok instrumentpanel”, vilket är roll center för alla åtgärder för bärbar dator.  Du kan skapa nya bärbara datorer och öppna befintliga från den här sidan.

![Skärmbild](./media/jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a>Med Jupyter-anteckningsbok
Om du klickar på den **ny** knappen, visas följande öppningssidan.

![Skärmbild](./media/jupyter-notebook/jupyter-untitled-notebook.png)

Området som markerats med en `In []:` är området inkommande fråga här kan du ange en giltig Python-kod och den kommer att köras när du klickar på `Shift-Enter` eller klicka på ikonen ”Play” (den pekar åt höger triangeln i verktygsfältet).

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a>En kraftfull paradigmet: live beräkningar dokument med omfattande media
Anteckningsboken själva bör dig mycket snabbt för alla som har använt Python och ett ordbehandlingsprogram eftersom det på vissa sätt är en blandning av båda: du kan köra kodblock Python, men du kan också hålla anteckningar och annan text genom att ändra formatet för en cell från ”kod” till ”Markdo ned ”med hjälp av den nedrullningsbara menyn i verktygsfältet.

Jupyter är mycket mer än ett ordbehandlingsprogram som tillåter blandning av beräkningar och omfattande media (text, bilder, video och praktiken vad en modern webbläsare kan visa). Du kan blanda, text, kod, videor och mer!

![Skärmbild](./media/jupyter-notebook/jupyter-editing-experience.png)

Och med kraften i Pythons många utmärkt bibliotek för vetenskapliga och tekniska datorsammanhang i följande skärmbild en enkel beräkning kan utföras med det är lika enkelt som en analys för komplexa nätverk i en miljö.

Den här paradigmet blanda kraften i moderna webben med levande beräkning erbjuder många möjligheter och är idealisk för molnet; Du kan använda den bärbara datorn:

* Som en beräkningar anteckningsblocket att registrera undersökande fungera på ett problem.
* Om du vill dela resultatet med kollegor i ”live” beräkningar formulär eller i papperskopia format (HTML PDF).
* För att distribuera och presentera live lärare material som rör beräkning, så studenter kan omedelbart experimentera med den verkliga kod, ändra den och kör det igen interaktivt.
* För att tillhandahålla ”körbara papper” som visar resultaten av forskning på ett sätt som kan vara omedelbart reproduceras, verifieras och utökas med andra.
* Som en plattform för samarbetsfunktioner datoranvändning: flera användare kan logga in på samma server bärbara datorer att dela en beräkningar session.

Om du går till källkoden IPython [databasen][repository], hittar du en hel katalog med exempel på bärbara datorer som du kan hämta och sedan experimentera med på din egen Azure Jupyter-VM.  Hämta bara de `.ipynb` filerna från webbplatsen och överför dem till instrumentpanelen för din Virtuella Azure-dator (eller hämta dem direkt i den virtuella datorn).

## <a name="conclusion"></a>Slutsats
Jupyter-anteckningsbok erbjuder ett kraftfullt gränssnitt för att komma åt interaktivt kraften i Python-ekosystemet i Azure.  Den omfattar en mängd olika användning fall inklusive enkel undersökning och Python, dataanalys och visualisering, simulering och parallell datorbearbetning. De resulterande anteckningsboken dokument innehåller en fullständig post för beräkningar som utförs och kan delas med andra Jupyter-användare.  Jupyter-anteckningsbok kan användas som ett lokalt program, men den är idealisk för molndistributioner på Azure

Kärnfunktioner Jupyter finns också tillgängliga i Visual Studio via den [Python Tools för Visual Studio] [ Python Tools for Visual Studio] (PTVS). PTVS är ett kostnadsfritt och integrering på plugin-program från Microsoft som blir en avancerad Python-utvecklingsmiljö som innehåller en redigeraren med IntelliSense, felsökning, Visual Studio profilering och parallella för öppen källkod.

## <a name="next-steps"></a>Nästa steg
Mer information finns i [Python Developer Center](/develop/python/).

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
