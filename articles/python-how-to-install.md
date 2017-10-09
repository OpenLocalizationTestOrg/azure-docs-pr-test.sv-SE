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
# <a name="installing-python-and-hello-sdk"></a>Installera Python och hello SDK
Python är enkelt tooset in på Windows och finns förinstallerat på Linux, Mac och [Bash för Windows](https://msdn.microsoft.com/commandline/wsl/about). Den här guiden vägleder dig genom installationen och hämtar datorn redo för användning med Azure.

## <a name="whats-in-hello-python-azure-sdk"></a>Vad är i hello Azure SDK för Python?
hello Azure SDK för Python innehåller komponenter som gör att du toodevelop, distribuera och hantera Python program för Azure. Hello Azure SDK för Python innehåller främst hello följande:

* **Bibliotek för**. Dessa klassbibliotek tillhandahålla ett gränssnitt som hanterar Azure-resurser, till exempel lagringskonton, virtuella datorer.
* **Bibliotek körning**. Dessa klassbibliotek tillhandahåller ett gränssnitt för att komma åt Azure-funktioner, till exempel lagring och service bus.

## <a name="which-python-and-which-version-toouse"></a>Vilka Python och vilken version toouse
Det finns flera varianter av Python tolkar - exempel:

* CPython - hello standard och de vanligaste Python-tolkning
* PyPy - snabb, kompatibla alternativ implementering tooCPython
* IronPython - Python-tolkning som körs på .net/CLR
* Jython - Python-tolkning som körs på hello Java Virtual Machine

**CPython** v2.7 eller v3.3 + och PyPy 5.4.0 testats och stöds för hello Azure SDK för Python.

## <a name="where-tooget-python"></a>Där tooget Python?
Det finns flera sätt tooget CPython:

* Direkt från [www.python.org][www.python.org]
* Från en välkända distro som [www.continuum.io][www.continuum.io], [www.enthought.com] [ www.enthought.com] eller [www.activestate.com][www.activestate.com]
* Skapa från källan!

Om du inte har ett specifikt behov, rekommenderar vi hello två första alternativen.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>SDK-Installation på Windows, Linux och MacOS (endast klientbibliotek)
Om du redan har installerat Python kan du använda pip tooinstall ett paket av alla hello klientbibliotek i din befintliga Python 2.7 eller Python 3.3 + miljö. Hello paket laddas ned från hello [Python Package Index] [ Python Package Index] (PyPI).

Behöver du administratörsbehörighet:

* Linux- och MacOS, använda hello `sudo` kommando: `sudo pip install azure-mgmt-compute`.
* Windows: öppna PowerShell/Kommandotolken som administratör

Du kan installera individuellt varje bibliotek för varje Azure-tjänsten:

```console
   $ pip install azure-batch          # Install hello latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install hello latest Storage management library
```

Förhandsgranska paket kan installeras med hello `--pre` flaggan:

```console
   $ pip install --pre azure-mgmt-compute # installs only hello latest Compute Management library
```

Du kan också installera en uppsättning Azure-bibliotek på en rad med hello `azure` meta-paketet. Eftersom inte alla paket i metadata-paketet publiceras som stabil ännu, hello `azure` meta-paketet är fortfarande under förhandsgranskning.
Dock kan hello kärnpaket, från kod kvalitet/lagringskontonycklarna perspektiv betraktas som ”stabil” just nu

* officiellt heter som sådana synkroniserad med andra språk så snart som möjligt.
  Vi planerar inte på några ytterligare stora förändringar fram till dess.

Eftersom det är en förhandsversionen behöver toouse hello `--pre` flaggan:

```console
   $ pip install --pre azure
```

eller direkt

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>Hämta paket
Hej [Python Package Index] [ Python Package Index] (PyPI) har ett stort urval av Python-bibliotek.  Om du väljer tooinstall en Distro du redan har de flesta av hello intressanta bitar för olika scenarier från web development tooTechnical Computing.

## <a name="python-tools-for-visual-studio"></a>Python Tools för Visual Studio
[Python Tools för Visual Studio][Python Tools för Visual Studio] (PTVS) är en kostnadsfri/OSS plugin-program från Microsoft, som blir en komplett Python IDE VS:

![How-till-installation-python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

Med hjälp av PTVS är valfritt men rekommenderas eftersom den ger dig stöd Python och Web projekt/lösning, felsökning, profilering, interaktiva fönster, mallredigering och Intellisense.

PTVS gör det också enkelt toodeploy tooMicrosoft Azure, med stöd för distribution för[molntjänster](cloud-services/cloud-services-python-ptvs.md) och [webbplatser](app-service-web/web-sites-python-ptvs-django-mysql.md).

PTVS fungerar med din befintliga installation av Visual Studio 2013 eller 2015 2017.  Dokumentation, hämtningsbara filer och diskussioner finns [Python Tools för Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Azure scenarier för Linux och MacOS
För Linux eller MacOS huvudsakliga Azure scenarier som stöds:

1. Använda Azure-tjänster med hjälp av hello-klientbibliotek för Python
2. Kör appen i en virtuell Linux-dator
3. Utveckla och publicera tooAzure webbplatser med Git

hello första scenariot kan du tooauthor omfattande webbprogram som utnyttjar hello Azure PaaS-funktioner som [blob storage](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [kö lagring](storage/queues/storage-python-how-to-use-queue-storage.md), [tabell lagring](cosmos-db/table-storage-how-to-use-python.md) etc. via Pythonic omslutningar för hello Azure REST API: er. Dessa fungera på samma sätt i Windows, Mac och Linux.  Du kan också använda dessa klientbibliotek från lokala utvecklingsdatorn eller en Linux-VM som körs på Azure.

Hello VM scenariot kan du bara starta en Linux-VM önskat (Ubuntu, CentOS, Suse) och kör/hantera vad du tycker.  Exempelvis kan du köra [IPython] [ IPython] REPL dator på din Windows-Mac-Linux-datorn och plats som din webbläsare tooa Linux eller Windows multi-proc VM körs hello IPython motorn på Azure.

Mer information om hur tooset av en Linux-VM finns hello [skapa en virtuell dator kör Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) kursen.

Med Git-distribution kan du utveckla en Python-webbprogram och publicera den tooan Azure-webbplats från alla operativsystem.  När du trycker på din databas tooAzure, skapas automatiskt en virtuell miljö och pip installerar nödvändiga paketen.

Mer information om hur du utvecklar och publicera Azure Websites finns hello självstudier för [skapar webbplatser med Django](app-service-web/web-sites-python-create-deploy-django-app.md), [skapar webbplatser med Bottle](app-service-web/web-sites-python-create-deploy-bottle-app.md), och [skapar webbplatser med Flask](app-service-web/web-sites-python-create-deploy-flask-app.md). Mer allmän information om hur du använder någon WSGI-kompatibel framework finns [konfigurera Python med Azure Websites](app-service-web/web-sites-python-configure.md).

## <a name="additional-software-and-resources"></a>Ytterligare programvara och resurser:
* [Azure SDK för Python ReadTheDocs](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [Azure SDK för Python GitHub](https://github.com/Azure/azure-sdk-for-python)
* [Officiell Azure-exempel för Python](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Produktserier Analytics Python-Distribution][Continuum Analytics Python Distribution]
* [Enthought Python-Distribution][Enthought Python Distribution]
* [{ActiveState Python-Distribution][ActiveState Python Distribution]
* [SciPy - en uppsättning vetenskapliga Python-bibliotek][SciPy - A suite of Scientific Python libraries]
* [NumPy - ett siffror bibliotek för Python][NumPy - A numerics library for Python]
* [Django projekt - mogen web framework/CMS][Django Project - A mature web framework/CMS]
* [IPython - en avancerad REPL dator för Python][IPython - an advanced REPL/Notebook for Python]
* [Python Tools för Visual Studio på GitHub][Python Tools for Visual Studio on GitHub]
* [Python Developer Center](/develop/python/)

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
