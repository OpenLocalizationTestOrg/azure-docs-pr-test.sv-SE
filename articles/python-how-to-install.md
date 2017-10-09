---
title: aaaInstall Python och hello SDK - Azure
description: "Lär dig hur toouse för tooinstall Python och hello SDK med Azure."
services: 
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: f36294be-daeb-4caf-9129-fce18130f552
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/06/2016
ms.author: lmazuel
ms.openlocfilehash: c1b394770f9abd3e654a23d79ae179a9af89e2fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="installing-python-and-hello-sdk"></a><span data-ttu-id="6e973-103">Installera Python och hello SDK</span><span class="sxs-lookup"><span data-stu-id="6e973-103">Installing Python and hello SDK</span></span>
<span data-ttu-id="6e973-104">Python är enkelt tooset in på Windows och finns förinstallerat på Linux, Mac och [Bash för Windows](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="6e973-104">Python is easy tooset up on Windows and comes pre-installed on Mac, Linux, and [Bash for Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span> <span data-ttu-id="6e973-105">Den här guiden vägleder dig genom installationen och hämtar datorn redo för användning med Azure.</span><span class="sxs-lookup"><span data-stu-id="6e973-105">This guide walks you through installation and getting your machine ready for use with Azure.</span></span>

## <a name="whats-in-hello-python-azure-sdk"></a><span data-ttu-id="6e973-106">Vad är i hello Azure SDK för Python?</span><span class="sxs-lookup"><span data-stu-id="6e973-106">What's in hello Python Azure SDK?</span></span>
<span data-ttu-id="6e973-107">hello Azure SDK för Python innehåller komponenter som gör att du toodevelop, distribuera och hantera Python program för Azure.</span><span class="sxs-lookup"><span data-stu-id="6e973-107">hello Azure SDK for Python includes components that allow you toodevelop, deploy, and manage Python applications for Azure.</span></span> <span data-ttu-id="6e973-108">Hello Azure SDK för Python innehåller främst hello följande:</span><span class="sxs-lookup"><span data-stu-id="6e973-108">Specifically, hello Azure SDK for Python includes hello following:</span></span>

* <span data-ttu-id="6e973-109">**Bibliotek för**.</span><span class="sxs-lookup"><span data-stu-id="6e973-109">**Management libraries**.</span></span> <span data-ttu-id="6e973-110">Dessa klassbibliotek tillhandahålla ett gränssnitt som hanterar Azure-resurser, till exempel lagringskonton, virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="6e973-110">These class libraries provide an interface managing Azure resources, such as storage accounts, virtual machines.</span></span>
* <span data-ttu-id="6e973-111">**Bibliotek körning**.</span><span class="sxs-lookup"><span data-stu-id="6e973-111">**Runtime libraries**.</span></span> <span data-ttu-id="6e973-112">Dessa klassbibliotek tillhandahåller ett gränssnitt för att komma åt Azure-funktioner, till exempel lagring och service bus.</span><span class="sxs-lookup"><span data-stu-id="6e973-112">These class libraries provide an interface for accessing Azure features, such as storage and service bus.</span></span>

## <a name="which-python-and-which-version-toouse"></a><span data-ttu-id="6e973-113">Vilka Python och vilken version toouse</span><span class="sxs-lookup"><span data-stu-id="6e973-113">Which Python and which version toouse</span></span>
<span data-ttu-id="6e973-114">Det finns flera varianter av Python tolkar - exempel:</span><span class="sxs-lookup"><span data-stu-id="6e973-114">There are several flavors of Python interpreters available - examples include:</span></span>

* <span data-ttu-id="6e973-115">CPython - hello standard och de vanligaste Python-tolkning</span><span class="sxs-lookup"><span data-stu-id="6e973-115">CPython - hello standard and most commonly used Python interpreter</span></span>
* <span data-ttu-id="6e973-116">PyPy - snabb, kompatibla alternativ implementering tooCPython</span><span class="sxs-lookup"><span data-stu-id="6e973-116">PyPy - fast, compliant alternative implementation tooCPython</span></span>
* <span data-ttu-id="6e973-117">IronPython - Python-tolkning som körs på .net/CLR</span><span class="sxs-lookup"><span data-stu-id="6e973-117">IronPython - Python interpreter that runs on .Net/CLR</span></span>
* <span data-ttu-id="6e973-118">Jython - Python-tolkning som körs på hello Java Virtual Machine</span><span class="sxs-lookup"><span data-stu-id="6e973-118">Jython - Python interpreter that runs on hello Java Virtual Machine</span></span>

<span data-ttu-id="6e973-119">**CPython** v2.7 eller v3.3 + och PyPy 5.4.0 testats och stöds för hello Azure SDK för Python.</span><span class="sxs-lookup"><span data-stu-id="6e973-119">**CPython** v2.7 or v3.3+ and PyPy 5.4.0 are tested and supported for hello Python Azure SDK.</span></span>

## <a name="where-tooget-python"></a><span data-ttu-id="6e973-120">Där tooget Python?</span><span class="sxs-lookup"><span data-stu-id="6e973-120">Where tooget Python?</span></span>
<span data-ttu-id="6e973-121">Det finns flera sätt tooget CPython:</span><span class="sxs-lookup"><span data-stu-id="6e973-121">There are several ways tooget CPython:</span></span>

* <span data-ttu-id="6e973-122">Direkt från [www.python.org][www.python.org]</span><span class="sxs-lookup"><span data-stu-id="6e973-122">Directly from [www.python.org][www.python.org]</span></span>
* <span data-ttu-id="6e973-123">Från en välkända distro som [www.continuum.io][www.continuum.io], [www.enthought.com] [ www.enthought.com] eller [www.activestate.com][www.activestate.com]</span><span class="sxs-lookup"><span data-stu-id="6e973-123">From a reputable distro such as [www.continuum.io][www.continuum.io], [www.enthought.com][www.enthought.com] or [www.activestate.com][www.activestate.com]</span></span>
* <span data-ttu-id="6e973-124">Skapa från källan!</span><span class="sxs-lookup"><span data-stu-id="6e973-124">Build from source!</span></span>

<span data-ttu-id="6e973-125">Om du inte har ett specifikt behov, rekommenderar vi hello två första alternativen.</span><span class="sxs-lookup"><span data-stu-id="6e973-125">Unless you have a specific need, we recommend hello first two options.</span></span>

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a><span data-ttu-id="6e973-126">SDK-Installation på Windows, Linux och MacOS (endast klientbibliotek)</span><span class="sxs-lookup"><span data-stu-id="6e973-126">SDK Installation on Windows, Linux, and MacOS (client libraries only)</span></span>
<span data-ttu-id="6e973-127">Om du redan har installerat Python kan du använda pip tooinstall ett paket av alla hello klientbibliotek i din befintliga Python 2.7 eller Python 3.3 + miljö.</span><span class="sxs-lookup"><span data-stu-id="6e973-127">If you already have Python installed, you can use pip tooinstall a bundle of all hello client libraries in your existing Python 2.7 or Python 3.3+ environment.</span></span> <span data-ttu-id="6e973-128">Hello paket laddas ned från hello [Python Package Index] [ Python Package Index] (PyPI).</span><span class="sxs-lookup"><span data-stu-id="6e973-128">This downloads hello packages from hello [Python Package Index][Python Package Index] (PyPI).</span></span>

<span data-ttu-id="6e973-129">Behöver du administratörsbehörighet:</span><span class="sxs-lookup"><span data-stu-id="6e973-129">You may need administrator rights:</span></span>

* <span data-ttu-id="6e973-130">Linux- och MacOS, använda hello `sudo` kommando: `sudo pip install azure-mgmt-compute`.</span><span class="sxs-lookup"><span data-stu-id="6e973-130">Linux and MacOS, use hello `sudo` command: `sudo pip install azure-mgmt-compute`.</span></span>
* <span data-ttu-id="6e973-131">Windows: öppna PowerShell/Kommandotolken som administratör</span><span class="sxs-lookup"><span data-stu-id="6e973-131">Windows: open your PowerShell/Command prompt as an administrator</span></span>

<span data-ttu-id="6e973-132">Du kan installera individuellt varje bibliotek för varje Azure-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="6e973-132">You can install individually each library for each Azure service:</span></span>

```console
   $ pip install azure-batch          # Install hello latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install hello latest Storage management library
```

<span data-ttu-id="6e973-133">Förhandsgranska paket kan installeras med hello `--pre` flaggan:</span><span class="sxs-lookup"><span data-stu-id="6e973-133">Preview packages can be installed using hello `--pre` flag:</span></span>

```console
   $ pip install --pre azure-mgmt-compute # installs only hello latest Compute Management library
```

<span data-ttu-id="6e973-134">Du kan också installera en uppsättning Azure-bibliotek på en rad med hello `azure` meta-paketet.</span><span class="sxs-lookup"><span data-stu-id="6e973-134">You can also install a set of Azure libraries in a single line using hello `azure` meta-package.</span></span> <span data-ttu-id="6e973-135">Eftersom inte alla paket i metadata-paketet publiceras som stabil ännu, hello `azure` meta-paketet är fortfarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="6e973-135">Since not all packages in this meta-package are published as stable yet, hello `azure` meta-package is still in preview.</span></span>
<span data-ttu-id="6e973-136">Dock kan hello kärnpaket, från kod kvalitet/lagringskontonycklarna perspektiv betraktas som ”stabil” just nu</span><span class="sxs-lookup"><span data-stu-id="6e973-136">However, hello core packages, from code quality/completeness perspectives can be considered "stable" at this time</span></span>

* <span data-ttu-id="6e973-137">officiellt heter som sådana synkroniserad med andra språk så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="6e973-137">it is officially labeled as such in sync with other languages as soon as possible.</span></span>
  <span data-ttu-id="6e973-138">Vi planerar inte på några ytterligare stora förändringar fram till dess.</span><span class="sxs-lookup"><span data-stu-id="6e973-138">We are not planning on any further major changes until then.</span></span>

<span data-ttu-id="6e973-139">Eftersom det är en förhandsversionen behöver toouse hello `--pre` flaggan:</span><span class="sxs-lookup"><span data-stu-id="6e973-139">Since it's a preview release, you need toouse hello `--pre` flag:</span></span>

```console
   $ pip install --pre azure
```

<span data-ttu-id="6e973-140">eller direkt</span><span class="sxs-lookup"><span data-stu-id="6e973-140">or directly</span></span>

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a><span data-ttu-id="6e973-141">Hämta paket</span><span class="sxs-lookup"><span data-stu-id="6e973-141">Getting More Packages</span></span>
<span data-ttu-id="6e973-142">Hej [Python Package Index] [ Python Package Index] (PyPI) har ett stort urval av Python-bibliotek.</span><span class="sxs-lookup"><span data-stu-id="6e973-142">hello [Python Package Index][Python Package Index] (PyPI) has a rich selection of Python libraries.</span></span>  <span data-ttu-id="6e973-143">Om du väljer tooinstall en Distro du redan har de flesta av hello intressanta bitar för olika scenarier från web development tooTechnical Computing.</span><span class="sxs-lookup"><span data-stu-id="6e973-143">If you chose tooinstall a Distro, you'll already have most of hello interesting bits for various scenarios from web development tooTechnical Computing.</span></span>

## <a name="python-tools-for-visual-studio"></a><span data-ttu-id="6e973-144">Python Tools för Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6e973-144">Python Tools for Visual Studio</span></span>
<span data-ttu-id="6e973-145">[Python Tools för Visual Studio][Python Tools för Visual Studio] (PTVS) är en kostnadsfri/OSS plugin-program från Microsoft, som blir en komplett Python IDE VS:</span><span class="sxs-lookup"><span data-stu-id="6e973-145">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) is a free/OSS plugin from Microsoft, which turns VS into a full-fledged Python IDE:</span></span>

![How-till-installation-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

<span data-ttu-id="6e973-147">Med hjälp av PTVS är valfritt men rekommenderas eftersom den ger dig stöd Python och Web projekt/lösning, felsökning, profilering, interaktiva fönster, mallredigering och Intellisense.</span><span class="sxs-lookup"><span data-stu-id="6e973-147">Using PTVS is optional, but is recommended as it gives you Python and Web Project/Solution support, debugging, profiling, interactive window, Template editing, and Intellisense.</span></span>

<span data-ttu-id="6e973-148">PTVS gör det också enkelt toodeploy tooMicrosoft Azure, med stöd för distribution för[molntjänster](cloud-services/cloud-services-python-ptvs.md) och [webbplatser](app-service-web/web-sites-python-ptvs-django-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="6e973-148">PTVS also makes it easy toodeploy tooMicrosoft Azure, with support for deployment too[Cloud Services](cloud-services/cloud-services-python-ptvs.md) and [Websites](app-service-web/web-sites-python-ptvs-django-mysql.md).</span></span>

<span data-ttu-id="6e973-149">PTVS fungerar med din befintliga installation av Visual Studio 2013 eller 2015 2017.</span><span class="sxs-lookup"><span data-stu-id="6e973-149">PTVS works with your existing Visual Studio 2013, 2015, or 2017 installation.</span></span>  <span data-ttu-id="6e973-150">Dokumentation, hämtningsbara filer och diskussioner finns [Python Tools för Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="6e973-150">For documentation, downloads and discussions, see [Python Tools for Visual Studio].</span></span>  

## <a name="python-azure-scenarios-for-linux-and-macos"></a><span data-ttu-id="6e973-151">Python Azure scenarier för Linux och MacOS</span><span class="sxs-lookup"><span data-stu-id="6e973-151">Python Azure Scenarios for Linux and MacOS</span></span>
<span data-ttu-id="6e973-152">För Linux eller MacOS huvudsakliga Azure scenarier som stöds:</span><span class="sxs-lookup"><span data-stu-id="6e973-152">For Linux or MacOS, main Azure scenarios that are supported:</span></span>

1. <span data-ttu-id="6e973-153">Använda Azure-tjänster med hjälp av hello-klientbibliotek för Python</span><span class="sxs-lookup"><span data-stu-id="6e973-153">Consuming Azure Services by using hello client libraries for Python</span></span>
2. <span data-ttu-id="6e973-154">Kör appen i en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="6e973-154">Running your app in a Linux VM</span></span>
3. <span data-ttu-id="6e973-155">Utveckla och publicera tooAzure webbplatser med Git</span><span class="sxs-lookup"><span data-stu-id="6e973-155">Developing and publishing tooAzure Websites using Git</span></span>

<span data-ttu-id="6e973-156">hello första scenariot kan du tooauthor omfattande webbprogram som utnyttjar hello Azure PaaS-funktioner som [blob storage](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [kö lagring](storage/queues/storage-python-how-to-use-queue-storage.md), [tabell lagring](cosmos-db/table-storage-how-to-use-python.md) etc. via Pythonic omslutningar för hello Azure REST API: er.</span><span class="sxs-lookup"><span data-stu-id="6e973-156">hello first scenario enables you tooauthor rich web apps that take advantage of hello Azure PaaS capabilities such as [blob storage](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [queue storage](storage/queues/storage-python-how-to-use-queue-storage.md), [table storage](cosmos-db/table-storage-how-to-use-python.md) etc. via Pythonic wrappers for hello Azure REST APIs.</span></span> <span data-ttu-id="6e973-157">Dessa fungera på samma sätt i Windows, Mac och Linux.</span><span class="sxs-lookup"><span data-stu-id="6e973-157">These work identically on Windows, Mac, and Linux.</span></span>  <span data-ttu-id="6e973-158">Du kan också använda dessa klientbibliotek från lokala utvecklingsdatorn eller en Linux-VM som körs på Azure.</span><span class="sxs-lookup"><span data-stu-id="6e973-158">You can also use these client libraries from your local development machine or a Linux VM running on Azure.</span></span>

<span data-ttu-id="6e973-159">Hello VM scenariot kan du bara starta en Linux-VM önskat (Ubuntu, CentOS, Suse) och kör/hantera vad du tycker.</span><span class="sxs-lookup"><span data-stu-id="6e973-159">For hello VM scenario, you simply start a Linux VM of your choice (Ubuntu, CentOS, Suse) and run/manage what you like.</span></span>  <span data-ttu-id="6e973-160">Exempelvis kan du köra [IPython] [ IPython] REPL dator på din Windows-Mac-Linux-datorn och plats som din webbläsare tooa Linux eller Windows multi-proc VM körs hello IPython motorn på Azure.</span><span class="sxs-lookup"><span data-stu-id="6e973-160">As an example, you can run [IPython][IPython] REPL/notebook on your Windows/Mac/Linux machine and point your browser tooa Linux or Windows multi-proc VM running hello IPython Engine on Azure.</span></span>

<span data-ttu-id="6e973-161">Mer information om hur tooset av en Linux-VM finns hello [skapa en virtuell dator kör Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kursen.</span><span class="sxs-lookup"><span data-stu-id="6e973-161">For information on how tooset up a Linux VM, see hello [Create a Virtual Machine Running Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tutorial.</span></span>

<span data-ttu-id="6e973-162">Med Git-distribution kan du utveckla en Python-webbprogram och publicera den tooan Azure-webbplats från alla operativsystem.</span><span class="sxs-lookup"><span data-stu-id="6e973-162">Using Git deployment, you can develop a Python web application and publish it tooan Azure Website from any operating system.</span></span>  <span data-ttu-id="6e973-163">När du trycker på din databas tooAzure, skapas automatiskt en virtuell miljö och pip installerar nödvändiga paketen.</span><span class="sxs-lookup"><span data-stu-id="6e973-163">When you push your repository tooAzure, it automatically creates a virtual environment and pip installs your required packages.</span></span>

<span data-ttu-id="6e973-164">Mer information om hur du utvecklar och publicera Azure Websites finns hello självstudier för [skapar webbplatser med Django](app-service-web/web-sites-python-create-deploy-django-app.md), [skapar webbplatser med Bottle](app-service-web/web-sites-python-create-deploy-bottle-app.md), och [skapar webbplatser med Flask](app-service-web/web-sites-python-create-deploy-flask-app.md).</span><span class="sxs-lookup"><span data-stu-id="6e973-164">For more information on developing and publishing Azure Websites, see hello tutorials for [Creating Websites with Django](app-service-web/web-sites-python-create-deploy-django-app.md), [Creating Websites with Bottle](app-service-web/web-sites-python-create-deploy-bottle-app.md), and [Creating Websites with Flask](app-service-web/web-sites-python-create-deploy-flask-app.md).</span></span> <span data-ttu-id="6e973-165">Mer allmän information om hur du använder någon WSGI-kompatibel framework finns [konfigurera Python med Azure Websites](app-service-web/web-sites-python-configure.md).</span><span class="sxs-lookup"><span data-stu-id="6e973-165">For more general information on using any WSGI-compliant framework, see [Configuring Python with Azure Websites](app-service-web/web-sites-python-configure.md).</span></span>

## <a name="additional-software-and-resources"></a><span data-ttu-id="6e973-166">Ytterligare programvara och resurser:</span><span class="sxs-lookup"><span data-stu-id="6e973-166">Additional Software and Resources:</span></span>
* [<span data-ttu-id="6e973-167">Azure SDK för Python ReadTheDocs</span><span class="sxs-lookup"><span data-stu-id="6e973-167">Azure SDK for Python ReadTheDocs</span></span>](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [<span data-ttu-id="6e973-168">Azure SDK för Python GitHub</span><span class="sxs-lookup"><span data-stu-id="6e973-168">Azure SDK for Python GitHub</span></span>](https://github.com/Azure/azure-sdk-for-python)
* [<span data-ttu-id="6e973-169">Officiell Azure-exempel för Python</span><span class="sxs-lookup"><span data-stu-id="6e973-169">Official Azure samples for Python</span></span>](https://azure.microsoft.com/documentation/samples/?platform=python)
* <span data-ttu-id="6e973-170">[Produktserier Analytics Python-Distribution][Continuum Analytics Python Distribution]</span><span class="sxs-lookup"><span data-stu-id="6e973-170">[Continuum Analytics Python Distribution][Continuum Analytics Python Distribution]</span></span>
* <span data-ttu-id="6e973-171">[Enthought Python-Distribution][Enthought Python Distribution]</span><span class="sxs-lookup"><span data-stu-id="6e973-171">[Enthought Python Distribution][Enthought Python Distribution]</span></span>
* <span data-ttu-id="6e973-172">[{ActiveState Python-Distribution][ActiveState Python Distribution]</span><span class="sxs-lookup"><span data-stu-id="6e973-172">[ActiveState Python Distribution][ActiveState Python Distribution]</span></span>
* <span data-ttu-id="6e973-173">[SciPy - en uppsättning vetenskapliga Python-bibliotek][SciPy - A suite of Scientific Python libraries]</span><span class="sxs-lookup"><span data-stu-id="6e973-173">[SciPy - A suite of Scientific Python libraries][SciPy - A suite of Scientific Python libraries]</span></span>
* <span data-ttu-id="6e973-174">[NumPy - ett siffror bibliotek för Python][NumPy - A numerics library for Python]</span><span class="sxs-lookup"><span data-stu-id="6e973-174">[NumPy - A numerics library for Python][NumPy - A numerics library for Python]</span></span>
* <span data-ttu-id="6e973-175">[Django projekt - mogen web framework/CMS][Django Project - A mature web framework/CMS]</span><span class="sxs-lookup"><span data-stu-id="6e973-175">[Django Project - A mature web framework/CMS][Django Project - A mature web framework/CMS]</span></span>
* <span data-ttu-id="6e973-176">[IPython - en avancerad REPL dator för Python][IPython - an advanced REPL/Notebook for Python]</span><span class="sxs-lookup"><span data-stu-id="6e973-176">[IPython - an advanced REPL/Notebook for Python][IPython - an advanced REPL/Notebook for Python]</span></span>
* <span data-ttu-id="6e973-177">[Python Tools för Visual Studio på GitHub][Python Tools for Visual Studio on GitHub]</span><span class="sxs-lookup"><span data-stu-id="6e973-177">[Python Tools for Visual Studio on GitHub][Python Tools for Visual Studio on GitHub]</span></span>
* [<span data-ttu-id="6e973-178">Python Developer Center</span><span class="sxs-lookup"><span data-stu-id="6e973-178">Python Developer Center</span></span>](/develop/python/)

[Continuum Analytics Python Distribution]: http://continuum.io
[Enthought Python Distribution]: http://www.enthought.com
[ActiveState Python Distribution]: http://www.activestate.com
[www.python.org]: http://www.python.org
[www.continuum.io]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.activestate.com]: http://www.activestate.com
[SciPy - A suite of Scientific Python libraries]: http://www.scipy.org
[NumPy - A numerics library for Python]: http://www.numpy.org
[Django Project - A mature web framework/CMS]: http://www.djangoproject.com
[IPython - an advanced REPL/Notebook for Python]: http://ipython.org
[IPython]: http://ipython.org
[IPython Notebook on Azure]: virtual-machines-linux-jupyter-notebook.md
[Cloud Services]: cloud-services-python-ptvs.md
[Websites]: web-sites-python-ptvs-django-mysql.md
[Python Tools för Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio on GitHub]: https://github.com/microsoft/ptvs
[Python Package Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via hello Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How toouse hello Azure Command-Line Interface]: crossplat-cmd-tools.md
[Create a Virtual Machine Running Linux]: virtual-machines-linux-quick-create-cli.md
[Creating Websites with Django]: web-sites-python-create-deploy-django-app.md
[Creating Websites with Bottle]: web-sites-python-create-deploy-bottle-app.md
[Creating Websites with Flask]: web-sites-python-create-deploy-flask-app.md
[Configuring Python with Azure Websites]: web-sites-python-configure.md
[table storage]: storage-python-how-to-use-table-storage.md
[queue storage]: storage-python-how-to-use-queue-storage.md
[blob storage]:storage/blobs/storage-python-how-to-use-blob-storage.md
