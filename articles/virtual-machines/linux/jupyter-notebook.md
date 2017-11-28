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
# <a name="creating-an-azure-vm-installing-jupyter-and-running-a-jupyter-notebook-on-azure"></a><span data-ttu-id="0c263-103">Skapa en Azure VM, installera Jupyter och kör en Jupyter-anteckningsbok på Azure</span><span class="sxs-lookup"><span data-stu-id="0c263-103">Creating an Azure VM, installing Jupyter, and running a Jupyter Notebook on Azure</span></span>
<span data-ttu-id="0c263-104">Den [Jupyter projekt](http://jupyter.org), tidigare i [IPython projekt](http://ipython.org), tillhandahåller en samling med verktyg för vetenskapliga datoranvändning med kraftfulla interaktiva tankar som kombinerar kodkörning med skapandet av en live beräkningar dokument.</span><span class="sxs-lookup"><span data-stu-id="0c263-104">The [Jupyter project](http://jupyter.org), formerly the [IPython project](http://ipython.org), provides a collection of tools for scientific computing using powerful interactive shells that combine code execution with the creation of a live computational document.</span></span> <span data-ttu-id="0c263-105">De här filerna kan innehålla godtycklig text, matematiska formler, inkommande kod, resultat, bilder, videor och andra typer av media som en modern webbläsare kan visa.</span><span class="sxs-lookup"><span data-stu-id="0c263-105">These notebook files can contain arbitrary text, mathematical formulas, input code, results, graphics, videos and any other kind of media that a modern web browser is capable of displaying.</span></span> <span data-ttu-id="0c263-106">Om du inte har absolut använt Python och vill veta i en miljö med roliga, interaktiva eller göra vissa allvarliga parallell-tekniska computing är Jupyter-anteckningsbok ett bra alternativ.</span><span class="sxs-lookup"><span data-stu-id="0c263-106">Whether you're absolutely new to Python and want to learn it in a fun, interactive environment or do some serious parallel/technical computing, the Jupyter Notebook is a great choice.</span></span>

<span data-ttu-id="0c263-107">![Skärmbild](./media/jupyter-notebook/ipy-notebook-spectral.png) med SciPy och Matplotlib paket att analysera strukturen för en inspelning av ljud.</span><span class="sxs-lookup"><span data-stu-id="0c263-107">![Screenshot](./media/jupyter-notebook/ipy-notebook-spectral.png) Using SciPy and Matplotlib packages to analyze the structure of a sound recording.</span></span>

## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a><span data-ttu-id="0c263-108">Jupyter två sätt: Azure-datorer eller anpassad distribution</span><span class="sxs-lookup"><span data-stu-id="0c263-108">Jupyter Two Ways: Azure Notebooks or Custom Deployment</span></span>
<span data-ttu-id="0c263-109">Azure tillhandahåller en tjänst som du kan använda för att [snabbt kommer igång med Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).</span><span class="sxs-lookup"><span data-stu-id="0c263-109">Azure provides a service that you can use to [quickly start using Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).</span></span>  <span data-ttu-id="0c263-110">Genom att använda tjänsten Azure bärbar dator kan få du enkelt tillgång till Jupyters webben gränssnitt för skalbara resurser med alla kraften i Python och dess många bibliotek.</span><span class="sxs-lookup"><span data-stu-id="0c263-110">By using the Azure Notebook Service, you can easily gain access to Jupyter's web-accessible interface to scalable computational resources with all the power of Python and its many libraries.</span></span>  <span data-ttu-id="0c263-111">Eftersom installationen hanteras av tjänsten kan användare komma åt dessa resurser utan att behöva administration och konfiguration av användaren.</span><span class="sxs-lookup"><span data-stu-id="0c263-111">Since the installation is handled by the service, users can access these resources without the need for administration and configuration by the user.</span></span>

<span data-ttu-id="0c263-112">Om tjänsten anteckningsboken inte fungerar för ditt scenario fortsätter du till den här artikeln som kommer visar hur du distribuerar Jupyter Notebook i Microsoft Azure med hjälp av virtuella Linux-datorer (VM).</span><span class="sxs-lookup"><span data-stu-id="0c263-112">If the notebook service does not work for your scenario please continue to read this article which will will show you how to deploy the Jupyter Notebook on Microsoft Azure, using Linux virtual machines (VMs).</span></span>

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a><span data-ttu-id="0c263-113">Skapa och konfigurera en virtuell dator på Azure</span><span class="sxs-lookup"><span data-stu-id="0c263-113">Create and configure a VM on Azure</span></span>
<span data-ttu-id="0c263-114">Det första steget är att skapa en virtuell dator (VM) som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="0c263-114">The first step is to create a virtual machine (VM)  running on Azure.</span></span>
<span data-ttu-id="0c263-115">Den här virtuella datorn är en hela operativsystemet i molnet och används för att köra Jupyter-anteckningsboken.</span><span class="sxs-lookup"><span data-stu-id="0c263-115">This VM is a complete operating system in the cloud and will be used to run the Jupyter Notebook.</span></span> <span data-ttu-id="0c263-116">Azure kan både Linux och Windows-datorer som körs och tar vi upp installationen av Jupyter på båda typer av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0c263-116">Azure is capable of running both Linux and Windows virtual machines, and we will cover the setup of Jupyter on both types of virtual machines.</span></span>

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a><span data-ttu-id="0c263-117">Skapa en Linux VM och öppnar en port för Jupyter</span><span class="sxs-lookup"><span data-stu-id="0c263-117">Create a Linux VM and open a port for Jupyter</span></span>
<span data-ttu-id="0c263-118">Följ instruktionerna [här] [ portal-vm-linux] att skapa en virtuell dator av den *Ubuntu* distribution.</span><span class="sxs-lookup"><span data-stu-id="0c263-118">Follow the instructions given [here][portal-vm-linux] to create a virtual machine of the *Ubuntu* distribution.</span></span> <span data-ttu-id="0c263-119">Den här kursen använder Ubuntu Server 14.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="0c263-119">This tutorial uses Ubuntu Server 14.04 LTS.</span></span> <span data-ttu-id="0c263-120">Användarnamnet antar vi *azureuser*.</span><span class="sxs-lookup"><span data-stu-id="0c263-120">We'll assume the user name *azureuser*.</span></span>

<span data-ttu-id="0c263-121">När du distribuerar den virtuella datorn som vi behöver öppna en säkerhetsregel på nätverkssäkerhetsgruppen.</span><span class="sxs-lookup"><span data-stu-id="0c263-121">After the virtual machine deploys we need to open up a security rule on the network security group.</span></span>  <span data-ttu-id="0c263-122">Från Azure-portalen går du till **Nätverkssäkerhetsgrupper** och öppna fliken för säkerhetsgruppen som motsvarar den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0c263-122">From the Azure portal, go to **Network Security Groups** and open the tab for the Security Group corresponding to your VM.</span></span> <span data-ttu-id="0c263-123">Du måste lägga till en regel för inkommande säkerhet med följande inställningar: **TCP** för protokollet  **\***  för källport (offentlig) och **9999** för den Målport (privat).</span><span class="sxs-lookup"><span data-stu-id="0c263-123">You need to add an Inbound Security rule with the following settings: **TCP** for the protocol, **\*** for the source (public) port and **9999** for the destination (private) port.</span></span>

![Skärmbild](./media/jupyter-notebook/azure-add-endpoint.png)

<span data-ttu-id="0c263-125">I din Nätverkssäkerhetsgruppen klickar du på **nätverksgränssnitt** och notera den **offentliga IP-adressen** eftersom det krävs för att ansluta till den virtuella datorn i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="0c263-125">While in your Network Security Group, click on **Network Interfaces** and note the **Public IP Address** as it will be needed to connect to your VM in the next step.</span></span>

## <a name="install-required-software-on-the-vm"></a><span data-ttu-id="0c263-126">Installera nödvändiga program på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="0c263-126">Install required software on the VM</span></span>
<span data-ttu-id="0c263-127">Om du vill köra Jupyter-anteckningsbok på vår VM installera vi först Jupyter och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="0c263-127">To run the Jupyter Notebook on our VM, we must first install Jupyter and its dependencies.</span></span> <span data-ttu-id="0c263-128">Anslut till din linux vm med ssh och det användarnamn/lösenordet koppla du valde när du skapade den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="0c263-128">Connect to your linux vm using ssh and the username/password pair you chose when you created the vm.</span></span> <span data-ttu-id="0c263-129">I den här självstudiekursen kommer vi använda PuTTY och ansluta från Windows.</span><span class="sxs-lookup"><span data-stu-id="0c263-129">In this tutorial we will use PuTTY and connect from Windows.</span></span>

### <a name="installing-jupyter-on-ubuntu"></a><span data-ttu-id="0c263-130">Installera Jupyter på Ubuntu</span><span class="sxs-lookup"><span data-stu-id="0c263-130">Installing Jupyter on Ubuntu</span></span>
<span data-ttu-id="0c263-131">Installera Anaconda, en populär datavetenskap python-distribution, med hjälp av en av länkarna från [produktserier Analytics](https://www.continuum.io/downloads).</span><span class="sxs-lookup"><span data-stu-id="0c263-131">Install Anaconda, a popular data science python distribution, using one of the links provided from [Continuum Analytics](https://www.continuum.io/downloads).</span></span>  <span data-ttu-id="0c263-132">Från och med skrivning av det här dokumentet på länkarna nedan om du är mest datum versioner.</span><span class="sxs-lookup"><span data-stu-id="0c263-132">As of the writing of this document, the below links are the most up to date versions.</span></span>

#### <a name="anaconda-installs-for-linux"></a><span data-ttu-id="0c263-133">Anaconda installerar för Linux</span><span class="sxs-lookup"><span data-stu-id="0c263-133">Anaconda Installs for Linux</span></span>
<table>
  <th><span data-ttu-id="0c263-134">Python 3.4</span><span class="sxs-lookup"><span data-stu-id="0c263-134">Python 3.4</span></span></th>
  <th><span data-ttu-id="0c263-135">Python 2.7</span><span class="sxs-lookup"><span data-stu-id="0c263-135">Python 2.7</span></span></th>
  <tr>
    <td><span data-ttu-id="0c263-136">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64-bitars</href>
    </span><span class="sxs-lookup"><span data-stu-id="0c263-136">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64 bit</href>
    </span></span></td>
    <td><span data-ttu-id="0c263-137">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64-bitars</href>
    </span><span class="sxs-lookup"><span data-stu-id="0c263-137">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64 bit</href>
    </span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="0c263-138">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32-bitars</href>
    </span><span class="sxs-lookup"><span data-stu-id="0c263-138">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32 bit</href>
    </span></span></td>
    <td><span data-ttu-id="0c263-139">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32-bitars</href>
    </span><span class="sxs-lookup"><span data-stu-id="0c263-139">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32 bit</href>
    </span></span></td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a><span data-ttu-id="0c263-140">Installera Anaconda3 2.3.0 64-bitars på Ubuntu</span><span class="sxs-lookup"><span data-stu-id="0c263-140">Installing Anaconda3 2.3.0 64-bit on Ubuntu</span></span>
<span data-ttu-id="0c263-141">Detta är hur du kan installera Anaconda på Ubuntu kan t.ex.</span><span class="sxs-lookup"><span data-stu-id="0c263-141">As an example, this is how you can install Anaconda on Ubuntu</span></span>

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

### <a name="configuring-jupyter-and-using-ssl"></a><span data-ttu-id="0c263-143">Konfigurera Jupyter och använda SSL</span><span class="sxs-lookup"><span data-stu-id="0c263-143">Configuring Jupyter and using SSL</span></span>
<span data-ttu-id="0c263-144">Vi behöver ta en stund att konfigurera konfigurationsfilerna för Jupyter när du har installerat.</span><span class="sxs-lookup"><span data-stu-id="0c263-144">After installing we need to take a moment to setup the configuration files for Jupyter.</span></span> <span data-ttu-id="0c263-145">Om det uppstår troubles med att konfigurera Jupyter kan det vara bra att titta på den [Jupyter-dokumentation för att köra en bärbar dator Server](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).</span><span class="sxs-lookup"><span data-stu-id="0c263-145">If you experience troubles with configuring Jupyter it may be helpful to look at the [Jupyter Documentation for Running a Notebook Server](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).</span></span>

<span data-ttu-id="0c263-146">Nästa vi `cd` till profilkatalogen för att skapa våra SSL-certifikat och redigera konfigurationsfilen profiler.</span><span class="sxs-lookup"><span data-stu-id="0c263-146">Next we `cd` to the profile directory to create our SSL certificate and edit the profiles configuration file.</span></span>

<span data-ttu-id="0c263-147">Använd följande kommando på Linux.</span><span class="sxs-lookup"><span data-stu-id="0c263-147">On Linux use the following command.</span></span>

    cd ~/.jupyter

<span data-ttu-id="0c263-148">Använd följande kommando för att skapa SSL-certifikatet (Linux och Windows).</span><span class="sxs-lookup"><span data-stu-id="0c263-148">Use the following command to create the SSL certificate(Linux and Windows).</span></span>

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

<span data-ttu-id="0c263-149">Observera att eftersom vi skapar ett självsignerat SSL-certifikat när du ansluter till den bärbara datorn webbläsaren ger dig en säkerhetsvarning.</span><span class="sxs-lookup"><span data-stu-id="0c263-149">Note that since we are creating a self-signed SSL certificate, when connecting to the notebook your browser will give you a security warning.</span></span>  <span data-ttu-id="0c263-150">För långsiktig produktion ska du vill använda ett felaktigt signerade certifikat som är associerade med din organisation.</span><span class="sxs-lookup"><span data-stu-id="0c263-150">For long-term production use, you will want to use a properly signed certificate associated with your organization.</span></span>  <span data-ttu-id="0c263-151">Eftersom certifikathantering är utanför omfattningen för den här demon, kommer vi använda ett självsignerat certifikat för tillfället.</span><span class="sxs-lookup"><span data-stu-id="0c263-151">Since certificate management is beyond the scope of this demo, we will stick to a self-signed certificate for now.</span></span>

<span data-ttu-id="0c263-152">Förutom att använda ett certifikat kan ange du också ett lösenord för att skydda din bärbara dator mot obehörig användning.</span><span class="sxs-lookup"><span data-stu-id="0c263-152">In addition to using a certificate, you must also provide a password to protect your notebook from unauthorized use.</span></span>  <span data-ttu-id="0c263-153">Av säkerhetsskäl använder Jupyter krypterade lösenord i dess konfigurationsfil, så du behöver att kryptera lösenordet först.</span><span class="sxs-lookup"><span data-stu-id="0c263-153">For security reasons Jupyter uses encrypted passwords in its configuration file, so you'll need to encrypt your password first.</span></span>  <span data-ttu-id="0c263-154">IPython ger ett verktyg för att göra det. Kör följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="0c263-154">IPython provides a utility to do so; at a command prompt run the following command.</span></span>

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

<span data-ttu-id="0c263-155">Detta uppmanas du att ett lösenord och bekräfta och skriver sedan ut lösenordet.</span><span class="sxs-lookup"><span data-stu-id="0c263-155">This will prompt you for a password and confirmation, and will then print the password.</span></span> <span data-ttu-id="0c263-156">Observera att detta för följande steg.</span><span class="sxs-lookup"><span data-stu-id="0c263-156">Note this for the following step.</span></span>

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

<span data-ttu-id="0c263-157">Därefter kommer vi att redigera konfigurationsfilen för den profil som är den `jupyter_notebook_config.py` filen i katalogen i.</span><span class="sxs-lookup"><span data-stu-id="0c263-157">Next, we will edit the profile's configuration file, which is the `jupyter_notebook_config.py` file in the directory you are in.</span></span>  <span data-ttu-id="0c263-158">Observera att den här filen inte finns--bara skapa om så är fallet.</span><span class="sxs-lookup"><span data-stu-id="0c263-158">Note that this file may not exist -- just create it if that is the case.</span></span>  <span data-ttu-id="0c263-159">Den här filen har ett antal fält och som standard alla är kommenterade.</span><span class="sxs-lookup"><span data-stu-id="0c263-159">This file has a number of fields and by default all are commented out.</span></span>  <span data-ttu-id="0c263-160">Du kan öppna den här filen med en textredigerare för dina önskemål och bör du kontrollera att den har minst följande innehåll.</span><span class="sxs-lookup"><span data-stu-id="0c263-160">You can open this file with any text editor of your liking, and you should ensure that it has at least the following content.</span></span> <span data-ttu-id="0c263-161">**Se till att ersätta c.NotebookApp.password i konfigurationen med sha1 från föregående steg**.</span><span class="sxs-lookup"><span data-stu-id="0c263-161">**Be sure to replace the c.NotebookApp.password in the config with the sha1 from the previous step**.</span></span>

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

### <a name="run-the-jupyter-notebook"></a><span data-ttu-id="0c263-162">Kör Jupyter-anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="0c263-162">Run the Jupyter Notebook</span></span>
<span data-ttu-id="0c263-163">Vi är nu redo att börja Jupyter-anteckningsboken.</span><span class="sxs-lookup"><span data-stu-id="0c263-163">At this point we are ready to start the Jupyter Notebook.</span></span> <span data-ttu-id="0c263-164">Gör detta genom att gå till den katalog som du vill lagra notebooks i och starta om servern för Jupyter-anteckningsbok med följande kommando.</span><span class="sxs-lookup"><span data-stu-id="0c263-164">To do this, navigate to the directory you want to store notebooks in and start the Jupyter Notebook server with the following command.</span></span>

    /anaconda3/bin/jupyter-notebook

<span data-ttu-id="0c263-165">Du ska nu kunna åtkomst till Jupyter-anteckningsbok på adressen `https://[PUBLIC-IP-ADDRESS]:9999`.</span><span class="sxs-lookup"><span data-stu-id="0c263-165">You should now be able to access your Jupyter Notebook at the address `https://[PUBLIC-IP-ADDRESS]:9999`.</span></span>

<span data-ttu-id="0c263-166">När du först åtkomst till din bärbara dator frågar inloggningssidan för ditt lösenord.</span><span class="sxs-lookup"><span data-stu-id="0c263-166">When you first access your notebook, the login page asks for your password.</span></span> <span data-ttu-id="0c263-167">Och när du loggar in ser ”Jupyter-anteckningsbok instrumentpanel”, vilket är roll center för alla åtgärder för bärbar dator.</span><span class="sxs-lookup"><span data-stu-id="0c263-167">And once you log in, you will see the "Jupyter Notebook Dashboard", which is the ontrol center for all notebook operations.</span></span>  <span data-ttu-id="0c263-168">Du kan skapa nya bärbara datorer och öppna befintliga från den här sidan.</span><span class="sxs-lookup"><span data-stu-id="0c263-168">From this page you can create new notebooks and open existing ones.</span></span>

![Skärmbild](./media/jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a><span data-ttu-id="0c263-170">Med Jupyter-anteckningsbok</span><span class="sxs-lookup"><span data-stu-id="0c263-170">Using the Jupyter Notebook</span></span>
<span data-ttu-id="0c263-171">Om du klickar på den **ny** knappen, visas följande öppningssidan.</span><span class="sxs-lookup"><span data-stu-id="0c263-171">If you click the **New** button, you will see the following opening page.</span></span>

![Skärmbild](./media/jupyter-notebook/jupyter-untitled-notebook.png)

<span data-ttu-id="0c263-173">Området som markerats med en `In []:` är området inkommande fråga här kan du ange en giltig Python-kod och den kommer att köras när du klickar på `Shift-Enter` eller klicka på ikonen ”Play” (den pekar åt höger triangeln i verktygsfältet).</span><span class="sxs-lookup"><span data-stu-id="0c263-173">The area marked with an `In []:` prompt is the input area, and here you can type any valid Python code and it will execute when you hit `Shift-Enter` or click on the "Play" icon (the right-pointing triangle in the toolbar).</span></span>

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a><span data-ttu-id="0c263-174">En kraftfull paradigmet: live beräkningar dokument med omfattande media</span><span class="sxs-lookup"><span data-stu-id="0c263-174">A powerful paradigm: live computational documents with rich media</span></span>
<span data-ttu-id="0c263-175">Anteckningsboken själva bör dig mycket snabbt för alla som har använt Python och ett ordbehandlingsprogram eftersom det på vissa sätt är en blandning av båda: du kan köra kodblock Python, men du kan också hålla anteckningar och annan text genom att ändra formatet för en cell från ”kod” till ”Markdo ned ”med hjälp av den nedrullningsbara menyn i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="0c263-175">The notebook itself should feel very natural to anyone who has used Python and a word processor, because it is in some ways a mix of both: you can execute blocks of Python code, but you can also keep notes and other text by changing the style of a cell from "Code" to "Markdown" using the drop-down menu in the toolbar.</span></span>

<span data-ttu-id="0c263-176">Jupyter är mycket mer än ett ordbehandlingsprogram som tillåter blandning av beräkningar och omfattande media (text, bilder, video och praktiken vad en modern webbläsare kan visa).</span><span class="sxs-lookup"><span data-stu-id="0c263-176">Jupyter is much more than a word processor as it allows the mixing of computation and rich media (text, graphics, video and virtually anything a modern web browser can display).</span></span> <span data-ttu-id="0c263-177">Du kan blanda, text, kod, videor och mer!</span><span class="sxs-lookup"><span data-stu-id="0c263-177">You can mix, text, code, videos and more!</span></span>

![Skärmbild](./media/jupyter-notebook/jupyter-editing-experience.png)

<span data-ttu-id="0c263-179">Och med kraften i Pythons många utmärkt bibliotek för vetenskapliga och tekniska datorsammanhang i följande skärmbild en enkel beräkning kan utföras med det är lika enkelt som en analys för komplexa nätverk i en miljö.</span><span class="sxs-lookup"><span data-stu-id="0c263-179">And with the power of Python's many excellent libraries for scientific and technical computing, in the following screenshot, a simple calculation can be performed with the same ease as a complex network analysis, all in one environment.</span></span>

<span data-ttu-id="0c263-180">Den här paradigmet blanda kraften i moderna webben med levande beräkning erbjuder många möjligheter och är idealisk för molnet; Du kan använda den bärbara datorn:</span><span class="sxs-lookup"><span data-stu-id="0c263-180">This paradigm of mixing the power of the modern web with live computation offers many possibilities, and is ideally suited for the cloud; the Notebook can be used:</span></span>

* <span data-ttu-id="0c263-181">Som en beräkningar anteckningsblocket att registrera undersökande fungera på ett problem.</span><span class="sxs-lookup"><span data-stu-id="0c263-181">As a computational scratchpad to record exploratory work on a problem.</span></span>
* <span data-ttu-id="0c263-182">Om du vill dela resultatet med kollegor i ”live” beräkningar formulär eller i papperskopia format (HTML PDF).</span><span class="sxs-lookup"><span data-stu-id="0c263-182">To share results with colleagues, either in 'live' computational form or in hardcopy formats (HTML, PDF).</span></span>
* <span data-ttu-id="0c263-183">För att distribuera och presentera live lärare material som rör beräkning, så studenter kan omedelbart experimentera med den verkliga kod, ändra den och kör det igen interaktivt.</span><span class="sxs-lookup"><span data-stu-id="0c263-183">To distribute and present live teaching materials that involve computation, so students can immediately experiment with the real code, modify it and re-execute it interactively.</span></span>
* <span data-ttu-id="0c263-184">För att tillhandahålla ”körbara papper” som visar resultaten av forskning på ett sätt som kan vara omedelbart reproduceras, verifieras och utökas med andra.</span><span class="sxs-lookup"><span data-stu-id="0c263-184">To provide "executable papers" that present the results of research in a way that can be immediately reproduced, validated and extended by others.</span></span>
* <span data-ttu-id="0c263-185">Som en plattform för samarbetsfunktioner datoranvändning: flera användare kan logga in på samma server bärbara datorer att dela en beräkningar session.</span><span class="sxs-lookup"><span data-stu-id="0c263-185">As a platform for collaborative computing: multiple users can log in to the same notebook server to share a live computational session.</span></span>

<span data-ttu-id="0c263-186">Om du går till källkoden IPython [databasen][repository], hittar du en hel katalog med exempel på bärbara datorer som du kan hämta och sedan experimentera med på din egen Azure Jupyter-VM.</span><span class="sxs-lookup"><span data-stu-id="0c263-186">If you go to the IPython source code [repository][repository], you will find an entire directory with notebook examples which you can download and then experiment with on your own Azure Jupyter VM.</span></span>  <span data-ttu-id="0c263-187">Hämta bara de `.ipynb` filerna från webbplatsen och överför dem till instrumentpanelen för din Virtuella Azure-dator (eller hämta dem direkt i den virtuella datorn).</span><span class="sxs-lookup"><span data-stu-id="0c263-187">Simply download the `.ipynb` files from the site and upload them onto the dashboard of your notebook Azure VM (or download them directly into the VM).</span></span>

## <a name="conclusion"></a><span data-ttu-id="0c263-188">Slutsats</span><span class="sxs-lookup"><span data-stu-id="0c263-188">Conclusion</span></span>
<span data-ttu-id="0c263-189">Jupyter-anteckningsbok erbjuder ett kraftfullt gränssnitt för att komma åt interaktivt kraften i Python-ekosystemet i Azure.</span><span class="sxs-lookup"><span data-stu-id="0c263-189">The Jupyter Notebook provides a powerful interface for accessing interactively the power of the Python ecosystem on Azure.</span></span>  <span data-ttu-id="0c263-190">Den omfattar en mängd olika användning fall inklusive enkel undersökning och Python, dataanalys och visualisering, simulering och parallell datorbearbetning.</span><span class="sxs-lookup"><span data-stu-id="0c263-190">It covers a wide range of usage cases including simple exploration and learning Python, data analysis and visualization, simulation and parallel computing.</span></span> <span data-ttu-id="0c263-191">De resulterande anteckningsboken dokument innehåller en fullständig post för beräkningar som utförs och kan delas med andra Jupyter-användare.</span><span class="sxs-lookup"><span data-stu-id="0c263-191">The resulting Notebook documents contain a complete record of the computations that are performed and can be shared with other Jupyter users.</span></span>  <span data-ttu-id="0c263-192">Jupyter-anteckningsbok kan användas som ett lokalt program, men den är idealisk för molndistributioner på Azure</span><span class="sxs-lookup"><span data-stu-id="0c263-192">The Jupyter Notebook can be used as a local application, but it is ideally suited for cloud deployments on Azure</span></span>

<span data-ttu-id="0c263-193">Kärnfunktioner Jupyter finns också tillgängliga i Visual Studio via den [Python Tools för Visual Studio] [ Python Tools for Visual Studio] (PTVS).</span><span class="sxs-lookup"><span data-stu-id="0c263-193">The core features of Jupyter are also available inside Visual Studio via the [Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS).</span></span> <span data-ttu-id="0c263-194">PTVS är ett kostnadsfritt och integrering på plugin-program från Microsoft som blir en avancerad Python-utvecklingsmiljö som innehåller en redigeraren med IntelliSense, felsökning, Visual Studio profilering och parallella för öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="0c263-194">PTVS is a free and open-source plug-in from Microsoft that turns Visual Studio into an advanced Python development environment that includes an advanced editor with IntelliSense, debugging, profiling and parallel computing integration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c263-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0c263-195">Next steps</span></span>
<span data-ttu-id="0c263-196">Mer information finns i [Python Developer Center](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="0c263-196">For more information, see the [Python Developer Center](/develop/python/).</span></span>

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
