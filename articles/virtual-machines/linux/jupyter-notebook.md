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
# <a name="creating-an-azure-vm-installing-jupyter-and-running-a-jupyter-notebook-on-azure"></a><span data-ttu-id="be81f-103">Skapa en Azure VM, installera Jupyter och kör en Jupyter-anteckningsbok på Azure</span><span class="sxs-lookup"><span data-stu-id="be81f-103">Creating an Azure VM, installing Jupyter, and running a Jupyter Notebook on Azure</span></span>
<span data-ttu-id="be81f-104">Hej [Jupyter-projektet](http://jupyter.org), tidigare hello [IPython projekt](http://ipython.org), tillhandahåller en samling med verktyg för vetenskapliga datoranvändning med kraftfulla interaktiva tankar som kombinerar kodkörning med hello skapandet av en levande beräkningar dokument.</span><span class="sxs-lookup"><span data-stu-id="be81f-104">hello [Jupyter project](http://jupyter.org), formerly hello [IPython project](http://ipython.org), provides a collection of tools for scientific computing using powerful interactive shells that combine code execution with hello creation of a live computational document.</span></span> <span data-ttu-id="be81f-105">De här filerna kan innehålla godtycklig text, matematiska formler, inkommande kod, resultat, bilder, videor och andra typer av media som en modern webbläsare kan visa.</span><span class="sxs-lookup"><span data-stu-id="be81f-105">These notebook files can contain arbitrary text, mathematical formulas, input code, results, graphics, videos and any other kind of media that a modern web browser is capable of displaying.</span></span> <span data-ttu-id="be81f-106">Om du är helt nya tooPython och vill toolearn den i en roliga, interaktiv miljö eller gör vissa allvarliga parallell-tekniska datoranvändning, hello Jupyter-anteckningsbok är ett bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="be81f-106">Whether you're absolutely new tooPython and want toolearn it in a fun, interactive environment or do some serious parallel/technical computing, hello Jupyter Notebook is a great choice.</span></span>

<span data-ttu-id="be81f-107">![Skärmbild](./media/jupyter-notebook/ipy-notebook-spectral.png) med SciPy och Matplotlib paket tooanalyze hello strukturen i en inspelning av ljud.</span><span class="sxs-lookup"><span data-stu-id="be81f-107">![Screenshot](./media/jupyter-notebook/ipy-notebook-spectral.png) Using SciPy and Matplotlib packages tooanalyze hello structure of a sound recording.</span></span>

## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a><span data-ttu-id="be81f-108">Jupyter två sätt: Azure-datorer eller anpassad distribution</span><span class="sxs-lookup"><span data-stu-id="be81f-108">Jupyter Two Ways: Azure Notebooks or Custom Deployment</span></span>
<span data-ttu-id="be81f-109">Azure tillhandahåller en tjänst som du kan använda för[snabbt kommer igång med Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).</span><span class="sxs-lookup"><span data-stu-id="be81f-109">Azure provides a service that you can use too[quickly start using Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).</span></span>  <span data-ttu-id="be81f-110">Genom att använda hello Azure anteckningsboken Service kan du enkelt få åtkomst tooJupyter webben gränssnittet till skalbara resurser med alla hello ström av Python och dess många bibliotek.</span><span class="sxs-lookup"><span data-stu-id="be81f-110">By using hello Azure Notebook Service, you can easily gain access tooJupyter's web-accessible interface to scalable computational resources with all hello power of Python and its many libraries.</span></span>  <span data-ttu-id="be81f-111">Eftersom hello installation hanteras av hello-tjänsten kan användare komma åt dessa resurser utan hello behovet av administration och konfiguration av hello användare.</span><span class="sxs-lookup"><span data-stu-id="be81f-111">Since hello installation is handled by hello service, users can access these resources without hello need for administration and configuration by hello user.</span></span>

<span data-ttu-id="be81f-112">Om hello anteckningsboken tjänsten inte fungerar för ditt scenario Fortsätt tooread den här artikeln som ska visas hur toodeploy hello Jupyter Notebook i Microsoft Azure med hjälp av virtuella Linux-datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="be81f-112">If hello notebook service does not work for your scenario please continue tooread this article which will will show you how toodeploy hello Jupyter Notebook on Microsoft Azure, using Linux virtual machines (VMs).</span></span>

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a><span data-ttu-id="be81f-113">Skapa och konfigurera en virtuell dator på Azure</span><span class="sxs-lookup"><span data-stu-id="be81f-113">Create and configure a VM on Azure</span></span>
<span data-ttu-id="be81f-114">hello första steget är toocreate en virtuell dator (VM) som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="be81f-114">hello first step is toocreate a virtual machine (VM)  running on Azure.</span></span>
<span data-ttu-id="be81f-115">Den här virtuella datorn är en hela operativsystemet i hello molnet och används för att köra hello Jupyter-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="be81f-115">This VM is a complete operating system in hello cloud and will be used to run hello Jupyter Notebook.</span></span> <span data-ttu-id="be81f-116">Azure kan både Linux och Windows-datorer som körs och tar vi upp hello installationen av Jupyter på båda typer av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="be81f-116">Azure is capable of running both Linux and Windows virtual machines, and we will cover hello setup of Jupyter on both types of virtual machines.</span></span>

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a><span data-ttu-id="be81f-117">Skapa en Linux VM och öppnar en port för Jupyter</span><span class="sxs-lookup"><span data-stu-id="be81f-117">Create a Linux VM and open a port for Jupyter</span></span>
<span data-ttu-id="be81f-118">Följ instruktionerna för hello [här] [ portal-vm-linux] toocreate en virtuell dator för hello *Ubuntu* distribution.</span><span class="sxs-lookup"><span data-stu-id="be81f-118">Follow hello instructions given [here][portal-vm-linux] toocreate a virtual machine of hello *Ubuntu* distribution.</span></span> <span data-ttu-id="be81f-119">Den här kursen använder Ubuntu Server 14.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="be81f-119">This tutorial uses Ubuntu Server 14.04 LTS.</span></span> <span data-ttu-id="be81f-120">Hello användarnamn antar vi *azureuser*.</span><span class="sxs-lookup"><span data-stu-id="be81f-120">We'll assume hello user name *azureuser*.</span></span>

<span data-ttu-id="be81f-121">När hello virtuell dator distribuerar måste vi tooopen upp en säkerhetsregel på hello nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="be81f-121">After hello virtual machine deploys we need tooopen up a security rule on hello network security group.</span></span>  <span data-ttu-id="be81f-122">Hello Azure-portalen, gå för**Nätverkssäkerhetsgrupper** och öppna hello fliken för hello säkerhetsgrupp motsvarande tooyour VM.</span><span class="sxs-lookup"><span data-stu-id="be81f-122">From hello Azure portal, go too**Network Security Groups** and open hello tab for hello Security Group corresponding tooyour VM.</span></span> <span data-ttu-id="be81f-123">Du behöver tooadd en regel för inkommande säkerhet med hello följande inställningar: **TCP** för hello protokollet  **\***  för hello-källport (offentlig) och **9999** för hello-målport (privat).</span><span class="sxs-lookup"><span data-stu-id="be81f-123">You need tooadd an Inbound Security rule with hello following settings: **TCP** for hello protocol, **\*** for hello source (public) port and **9999** for hello destination (private) port.</span></span>

![Skärmbild](./media/jupyter-notebook/azure-add-endpoint.png)

<span data-ttu-id="be81f-125">I din Nätverkssäkerhetsgruppen klickar du på **nätverksgränssnitt** och Observera hello **offentliga IP-adressen** eftersom den nödvändiga tooconnect tooyour VM i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="be81f-125">While in your Network Security Group, click on **Network Interfaces** and note hello **Public IP Address** as it will be needed tooconnect tooyour VM in hello next step.</span></span>

## <a name="install-required-software-on-hello-vm"></a><span data-ttu-id="be81f-126">Installera nödvändig programvara på hello VM</span><span class="sxs-lookup"><span data-stu-id="be81f-126">Install required software on hello VM</span></span>
<span data-ttu-id="be81f-127">toorun Hej Jupyter-anteckningsbok på vår VM, vi måste först installera Jupyter och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="be81f-127">toorun hello Jupyter Notebook on our VM, we must first install Jupyter and its dependencies.</span></span> <span data-ttu-id="be81f-128">Anslut tooyour virtuell linux-dator med hjälp av ssh och hello användarnamn/lösenord par som du valde när du skapade hello vm.</span><span class="sxs-lookup"><span data-stu-id="be81f-128">Connect tooyour linux vm using ssh and hello username/password pair you chose when you created hello vm.</span></span> <span data-ttu-id="be81f-129">I den här självstudiekursen kommer vi använda PuTTY och ansluta från Windows.</span><span class="sxs-lookup"><span data-stu-id="be81f-129">In this tutorial we will use PuTTY and connect from Windows.</span></span>

### <a name="installing-jupyter-on-ubuntu"></a><span data-ttu-id="be81f-130">Installera Jupyter på Ubuntu</span><span class="sxs-lookup"><span data-stu-id="be81f-130">Installing Jupyter on Ubuntu</span></span>
<span data-ttu-id="be81f-131">Installera Anaconda, en populär datavetenskap python-distribution, med någon av hello länkarna från [produktserier Analytics](https://www.continuum.io/downloads).</span><span class="sxs-lookup"><span data-stu-id="be81f-131">Install Anaconda, a popular data science python distribution, using one of hello links provided from [Continuum Analytics](https://www.continuum.io/downloads).</span></span>  <span data-ttu-id="be81f-132">Från och med hello skrivning av det här dokumentet är hello länkarna nedan hello mest upp toodate versioner.</span><span class="sxs-lookup"><span data-stu-id="be81f-132">As of hello writing of this document, hello below links are hello most up toodate versions.</span></span>

#### <a name="anaconda-installs-for-linux"></a><span data-ttu-id="be81f-133">Anaconda installerar för Linux</span><span class="sxs-lookup"><span data-stu-id="be81f-133">Anaconda Installs for Linux</span></span>
<table>
  <th><span data-ttu-id="be81f-134">Python 3.4</span><span class="sxs-lookup"><span data-stu-id="be81f-134">Python 3.4</span></span></th>
  <th><span data-ttu-id="be81f-135">Python 2.7</span><span class="sxs-lookup"><span data-stu-id="be81f-135">Python 2.7</span></span></th>
  <tr>
    <td><span data-ttu-id="be81f-136">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64-bitars</href>
    </span><span class="sxs-lookup"><span data-stu-id="be81f-136">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64 bit</href>
    </span></span></td>
    <td><span data-ttu-id="be81f-137">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64-bitars</href>
    </span><span class="sxs-lookup"><span data-stu-id="be81f-137">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64 bit</href>
    </span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="be81f-138">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32-bitars</href>
    </span><span class="sxs-lookup"><span data-stu-id="be81f-138">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32 bit</href>
    </span></span></td>
    <td><span data-ttu-id="be81f-139">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32-bitars</href>
    </span><span class="sxs-lookup"><span data-stu-id="be81f-139">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32 bit</href>
    </span></span></td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a><span data-ttu-id="be81f-140">Installera Anaconda3 2.3.0 64-bitars på Ubuntu</span><span class="sxs-lookup"><span data-stu-id="be81f-140">Installing Anaconda3 2.3.0 64-bit on Ubuntu</span></span>
<span data-ttu-id="be81f-141">Detta är hur du kan installera Anaconda på Ubuntu kan t.ex.</span><span class="sxs-lookup"><span data-stu-id="be81f-141">As an example, this is how you can install Anaconda on Ubuntu</span></span>

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

### <a name="configuring-jupyter-and-using-ssl"></a><span data-ttu-id="be81f-143">Konfigurera Jupyter och använda SSL</span><span class="sxs-lookup"><span data-stu-id="be81f-143">Configuring Jupyter and using SSL</span></span>
<span data-ttu-id="be81f-144">När du har installerat behöver vi tootake ett ögonblick toosetup hello konfigurationsfiler för Jupyter.</span><span class="sxs-lookup"><span data-stu-id="be81f-144">After installing we need tootake a moment toosetup hello configuration files for Jupyter.</span></span> <span data-ttu-id="be81f-145">Om du upplever troubles med att konfigurera Jupyter kan det vara bra toolook på hello [Jupyter-dokumentation för att köra en bärbar dator Server](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).</span><span class="sxs-lookup"><span data-stu-id="be81f-145">If you experience troubles with configuring Jupyter it may be helpful toolook at hello [Jupyter Documentation for Running a Notebook Server](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).</span></span>

<span data-ttu-id="be81f-146">Nästa vi `cd` toohello profilen directory toocreate våra SSL-certifikat och redigera konfigurationsfilen för hello profiler.</span><span class="sxs-lookup"><span data-stu-id="be81f-146">Next we `cd` toohello profile directory toocreate our SSL certificate and edit hello profiles configuration file.</span></span>

<span data-ttu-id="be81f-147">Använd följande kommando hello på Linux.</span><span class="sxs-lookup"><span data-stu-id="be81f-147">On Linux use hello following command.</span></span>

    cd ~/.jupyter

<span data-ttu-id="be81f-148">Använd hello efter kommandot toocreate hello SSL-certifikat (Linux och Windows).</span><span class="sxs-lookup"><span data-stu-id="be81f-148">Use hello following command toocreate hello SSL certificate(Linux and Windows).</span></span>

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

<span data-ttu-id="be81f-149">Observera att eftersom vi skapar ett självsignerat SSL-certifikat när du ansluter toohello anteckningsboken webbläsaren ger dig en säkerhetsvarning.</span><span class="sxs-lookup"><span data-stu-id="be81f-149">Note that since we are creating a self-signed SSL certificate, when connecting toohello notebook your browser will give you a security warning.</span></span>  <span data-ttu-id="be81f-150">För långsiktig produktion ska toouse ett felaktigt signerade certifikat som är associerade med din organisation.</span><span class="sxs-lookup"><span data-stu-id="be81f-150">For long-term production use, you will want toouse a properly signed certificate associated with your organization.</span></span>  <span data-ttu-id="be81f-151">Eftersom certifikathantering ligger utanför den här demon hello, kommer vi behålla tooa självsignerat certifikat för tillfället.</span><span class="sxs-lookup"><span data-stu-id="be81f-151">Since certificate management is beyond hello scope of this demo, we will stick tooa self-signed certificate for now.</span></span>

<span data-ttu-id="be81f-152">Dessutom toousing ett certifikat, måste du även ange ett lösenord tooprotect anteckningsboken från obehörig användning.</span><span class="sxs-lookup"><span data-stu-id="be81f-152">In addition toousing a certificate, you must also provide a password tooprotect your notebook from unauthorized use.</span></span>  <span data-ttu-id="be81f-153">Av säkerhetsskäl Jupyter använder krypterade lösenord i dess konfigurationsfil, så du behöver tooencrypt lösenordet först.</span><span class="sxs-lookup"><span data-stu-id="be81f-153">For security reasons Jupyter uses encrypted passwords in its configuration file, so you'll need tooencrypt your password first.</span></span>  <span data-ttu-id="be81f-154">IPython ger en verktyget toodo. Kör hello följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="be81f-154">IPython provides a utility toodo so; at a command prompt run hello following command.</span></span>

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

<span data-ttu-id="be81f-155">Detta kommer att fråga efter en lösenordet och bekräftelsen och skriver sedan ut hello lösenord.</span><span class="sxs-lookup"><span data-stu-id="be81f-155">This will prompt you for a password and confirmation, and will then print hello password.</span></span> <span data-ttu-id="be81f-156">Observera att detta för hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="be81f-156">Note this for hello following step.</span></span>

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided hello rest for security)

<span data-ttu-id="be81f-157">Sedan redigerar vi hello profil konfigurationsfil, vilket är den `jupyter_notebook_config.py` fil i hello du befinner dig i.</span><span class="sxs-lookup"><span data-stu-id="be81f-157">Next, we will edit hello profile's configuration file, which is the `jupyter_notebook_config.py` file in hello directory you are in.</span></span>  <span data-ttu-id="be81f-158">Observera att den här filen inte finns--bara skapa om så är fallet hello.</span><span class="sxs-lookup"><span data-stu-id="be81f-158">Note that this file may not exist -- just create it if that is hello case.</span></span>  <span data-ttu-id="be81f-159">Den här filen har ett antal fält och som standard alla är kommenterade.  Du kan öppna den här filen med en textredigerare för dina önskemål och bör du kontrollera att den har minst hello följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="be81f-159">This file has a number of fields and by default all are commented out.  You can open this file with any text editor of your liking, and you should ensure that it has at least hello following content.</span></span> <span data-ttu-id="be81f-160">**Vara säker på att tooreplace hello c.NotebookApp.password i hello config med hello sha1 hello föregående steg**.</span><span class="sxs-lookup"><span data-stu-id="be81f-160">**Be sure tooreplace hello c.NotebookApp.password in hello config with hello sha1 from hello previous step**.</span></span>

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

### <a name="run-hello-jupyter-notebook"></a><span data-ttu-id="be81f-161">Kör hello Jupyter-anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="be81f-161">Run hello Jupyter Notebook</span></span>
<span data-ttu-id="be81f-162">Vi är nu redo toostart hello Jupyter-anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="be81f-162">At this point we are ready toostart hello Jupyter Notebook.</span></span> <span data-ttu-id="be81f-163">toodo detta, navigera toohello directory du vill använda toostore anteckningsböcker och börja hello Jupyter-anteckningsbok server med hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="be81f-163">toodo this, navigate toohello directory you want toostore notebooks in and start hello Jupyter Notebook server with hello following command.</span></span>

    /anaconda3/bin/jupyter-notebook

<span data-ttu-id="be81f-164">Du bör nu vara kan tooaccess Jupyter-anteckningsbok på hello adressen `https://[PUBLIC-IP-ADDRESS]:9999`.</span><span class="sxs-lookup"><span data-stu-id="be81f-164">You should now be able tooaccess your Jupyter Notebook at hello address `https://[PUBLIC-IP-ADDRESS]:9999`.</span></span>

<span data-ttu-id="be81f-165">När du först åtkomst till din bärbara dator frågar hello inloggningssidan för ditt lösenord.</span><span class="sxs-lookup"><span data-stu-id="be81f-165">When you first access your notebook, hello login page asks for your password.</span></span> <span data-ttu-id="be81f-166">Och när du loggar in ser hello ”Jupyter-anteckningsbok instrumentpanel”, vilket är roll center för alla åtgärder för bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="be81f-166">And once you log in, you will see hello "Jupyter Notebook Dashboard", which is the ontrol center for all notebook operations.</span></span>  <span data-ttu-id="be81f-167">Du kan skapa nya bärbara datorer och öppna befintliga från den här sidan.</span><span class="sxs-lookup"><span data-stu-id="be81f-167">From this page you can create new notebooks and open existing ones.</span></span>

![Skärmbild](./media/jupyter-notebook/jupyter-tree-view.png)

### <a name="using-hello-jupyter-notebook"></a><span data-ttu-id="be81f-169">Med hjälp av hello Jupyter-anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="be81f-169">Using hello Jupyter Notebook</span></span>
<span data-ttu-id="be81f-170">Om du klickar på hello **ny** knappen, visas hello följande öppningssidan.</span><span class="sxs-lookup"><span data-stu-id="be81f-170">If you click hello **New** button, you will see hello following opening page.</span></span>

![Skärmbild](./media/jupyter-notebook/jupyter-untitled-notebook.png)

<span data-ttu-id="be81f-172">hello område som markerats med en `In []:` är fråga hello inkommande område, och här kan du ange en giltig Python-kod och den kommer att köras när du klickar på `Shift-Enter` eller klicka på ikonen för hello ”spela upp” (hello pekar åt höger triangel i hello verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="be81f-172">hello area marked with an `In []:` prompt is hello input area, and here you can type any valid Python code and it will execute when you hit `Shift-Enter` or click on hello "Play" icon (hello right-pointing triangle in hello toolbar).</span></span>

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a><span data-ttu-id="be81f-173">En kraftfull paradigmet: live beräkningar dokument med omfattande media</span><span class="sxs-lookup"><span data-stu-id="be81f-173">A powerful paradigm: live computational documents with rich media</span></span>
<span data-ttu-id="be81f-174">hello anteckningsboken själva bör känna sig mycket fysiska tooanyone som har använt Python och ett ordbehandlingsprogram eftersom det på vissa sätt är en blandning av båda: du kan köra kodblock Python, men du kan också hålla anteckningar och annan text genom att ändra hello formatet för en cell från ”kod” för ”Ma rkdown ”använda hello nedrullningsbara menyn i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="be81f-174">hello notebook itself should feel very natural tooanyone who has used Python and a word processor, because it is in some ways a mix of both: you can execute blocks of Python code, but you can also keep notes and other text by changing hello style of a cell from "Code" too"Markdown" using hello drop-down menu in the toolbar.</span></span>

<span data-ttu-id="be81f-175">Jupyter är mycket mer än ett ordbehandlingsprogram som tillåter blandning av beräkningar och omfattande media (text, bilder, video och praktiken vad en modern webbläsare kan visa).</span><span class="sxs-lookup"><span data-stu-id="be81f-175">Jupyter is much more than a word processor as it allows the mixing of computation and rich media (text, graphics, video and virtually anything a modern web browser can display).</span></span> <span data-ttu-id="be81f-176">Du kan blanda, text, kod, videor och mer!</span><span class="sxs-lookup"><span data-stu-id="be81f-176">You can mix, text, code, videos and more!</span></span>

![Skärmbild](./media/jupyter-notebook/jupyter-editing-experience.png)

<span data-ttu-id="be81f-178">Och med hello kraften i Pythons många utmärkt bibliotek för vetenskapliga och tekniska databehandling i hello följande skärmbild, en enkel beräkning kan utföras med hello samma underlättar som en komplexa nätverk analys, allt i en miljö.</span><span class="sxs-lookup"><span data-stu-id="be81f-178">And with hello power of Python's many excellent libraries for scientific and technical computing, in hello following screenshot, a simple calculation can be performed with hello same ease as a complex network analysis, all in one environment.</span></span>

<span data-ttu-id="be81f-179">Den här paradigmet blanda hello kraften hos hello moderna web med levande beräkning erbjuder många möjligheter och är idealisk för hello molnet; hello anteckningsboken kan användas:</span><span class="sxs-lookup"><span data-stu-id="be81f-179">This paradigm of mixing hello power of hello modern web with live computation offers many possibilities, and is ideally suited for hello cloud; hello Notebook can be used:</span></span>

* <span data-ttu-id="be81f-180">Som en undersökande beräkningar anteckningsblocket toorecord fungera på ett problem.</span><span class="sxs-lookup"><span data-stu-id="be81f-180">As a computational scratchpad toorecord exploratory work on a problem.</span></span>
* <span data-ttu-id="be81f-181">tooshare resulterar med kollegor i ”live” beräkningar formulär eller i papperskopia format (HTML-, PDF).</span><span class="sxs-lookup"><span data-stu-id="be81f-181">tooshare results with colleagues, either in 'live' computational form or in hardcopy formats (HTML, PDF).</span></span>
* <span data-ttu-id="be81f-182">toodistribute och finns live lärare material som rör beräkning, så studenter kan omedelbart experimentera med hello verkliga kod, ändra den och kör det igen interaktivt.</span><span class="sxs-lookup"><span data-stu-id="be81f-182">toodistribute and present live teaching materials that involve computation, so students can immediately experiment with hello real code, modify it and re-execute it interactively.</span></span>
* <span data-ttu-id="be81f-183">tooprovide ”körbara papper” som presenterar hello resultaten av forskning på ett sätt som kan återges omedelbart, verifiera och utökas med andra.</span><span class="sxs-lookup"><span data-stu-id="be81f-183">tooprovide "executable papers" that present hello results of research in a way that can be immediately reproduced, validated and extended by others.</span></span>
* <span data-ttu-id="be81f-184">Som en plattform för samarbetsfunktioner datoranvändning: flera användare kan logga in toothe samma anteckningsboken server tooshare en levande beräkningar session.</span><span class="sxs-lookup"><span data-stu-id="be81f-184">As a platform for collaborative computing: multiple users can log in toothe same notebook server tooshare a live computational session.</span></span>

<span data-ttu-id="be81f-185">Om du går källkoden för toohello IPython [databasen][repository], hittar du en hel katalog med exempel på bärbara datorer som du kan hämta och sedan experimentera med på din egen Azure Jupyter-VM.</span><span class="sxs-lookup"><span data-stu-id="be81f-185">If you go toohello IPython source code [repository][repository], you will find an entire directory with notebook examples which you can download and then experiment with on your own Azure Jupyter VM.</span></span>  <span data-ttu-id="be81f-186">Bara hämta hello `.ipynb` filer från hello plats och överför dem till hello instrumentpanelen i anteckningsboken Azure VM (eller hämta dem direkt i hello VM).</span><span class="sxs-lookup"><span data-stu-id="be81f-186">Simply download hello `.ipynb` files from hello site and upload them onto hello dashboard of your notebook Azure VM (or download them directly into hello VM).</span></span>

## <a name="conclusion"></a><span data-ttu-id="be81f-187">Slutsats</span><span class="sxs-lookup"><span data-stu-id="be81f-187">Conclusion</span></span>
<span data-ttu-id="be81f-188">Hej Jupyter-anteckningsbok erbjuder ett kraftfullt gränssnitt för att komma åt interaktivt hello kraften hos hello Python-ekosystemet i Azure.</span><span class="sxs-lookup"><span data-stu-id="be81f-188">hello Jupyter Notebook provides a powerful interface for accessing interactively hello power of hello Python ecosystem on Azure.</span></span>  <span data-ttu-id="be81f-189">Den omfattar en mängd olika användning fall inklusive enkel undersökning och Python, dataanalys och visualisering, simulering och parallell datorbearbetning.</span><span class="sxs-lookup"><span data-stu-id="be81f-189">It covers a wide range of usage cases including simple exploration and learning Python, data analysis and visualization, simulation and parallel computing.</span></span> <span data-ttu-id="be81f-190">hello resulterande anteckningsboken dokument innehåller en fullständig hello beräkningar som utförs och kan delas med andra användare i Jupyter-post.</span><span class="sxs-lookup"><span data-stu-id="be81f-190">hello resulting Notebook documents contain a complete record of hello computations that are performed and can be shared with other Jupyter users.</span></span>  <span data-ttu-id="be81f-191">Hej Jupyter-anteckningsbok kan användas som ett lokalt program, men den är idealisk för molndistributioner på Azure</span><span class="sxs-lookup"><span data-stu-id="be81f-191">hello Jupyter Notebook can be used as a local application, but it is ideally suited for cloud deployments on Azure</span></span>

<span data-ttu-id="be81f-192">hello kärnfunktioner Jupyter finns också tillgängliga i Visual Studio via den [Python Tools för Visual Studio] [ Python Tools for Visual Studio] (PTVS).</span><span class="sxs-lookup"><span data-stu-id="be81f-192">hello core features of Jupyter are also available inside Visual Studio via the [Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS).</span></span> <span data-ttu-id="be81f-193">PTVS är ett kostnadsfritt och integrering på plugin-program från Microsoft som blir en avancerad Python-utvecklingsmiljö som innehåller en redigeraren med IntelliSense, felsökning, Visual Studio profilering och parallella för öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="be81f-193">PTVS is a free and open-source plug-in from Microsoft that turns Visual Studio into an advanced Python development environment that includes an advanced editor with IntelliSense, debugging, profiling and parallel computing integration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="be81f-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="be81f-194">Next steps</span></span>
<span data-ttu-id="be81f-195">Mer information finns i hello [Python Developer Center](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="be81f-195">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
