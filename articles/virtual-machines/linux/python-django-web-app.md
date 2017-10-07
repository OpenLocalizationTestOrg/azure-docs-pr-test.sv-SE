---
title: "aaaPython webbprogram med Django på en Azure Linux-dator | Microsoft Docs"
description: "Lär dig hur toohost en Django-baserade webbapp i Azure med hjälp av en Linux-VM."
services: virtual-machines-linux
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-resource-manager
ms.assetid: 00ad4c2c-4316-4f9a-913f-f7f49b158db7
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: python
ms.topic: article
ms.date: 05/31/2017
ms.author: huvalo
ms.openlocfilehash: 520c47e19e8ffb4bb866f70772d506ddf76e242c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="django-hello-world-web-app-on-a-linux-vm"></a><span data-ttu-id="3a1d8-103">Django Hello World webbprogram på en Linux-VM</span><span class="sxs-lookup"><span data-stu-id="3a1d8-103">Django Hello World web app on a Linux VM</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3a1d8-104">Windows</span><span class="sxs-lookup"><span data-stu-id="3a1d8-104">Windows</span></span>](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [<span data-ttu-id="3a1d8-105">Mac/Linux</span><span class="sxs-lookup"><span data-stu-id="3a1d8-105">Mac/Linux</span></span>](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<br>

<span data-ttu-id="3a1d8-106">De här självstudierna visar hur toohost en Django-baserade webbplats i Linux i Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-106">This tutorial shows you how toohost a Django-based website in Linux in Azure Virtual Machines.</span></span> <span data-ttu-id="3a1d8-107">I hello kursen förutsätter vi att några tidigare erfarenheter med Azure.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-107">In hello tutorial, we assume no prior experience with Azure.</span></span> <span data-ttu-id="3a1d8-108">När du är klar hello kursen har du ett Django-program in och körs i hello moln.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-108">When you finish hello tutorial, you can have a Django-based application up and running in hello cloud.</span></span>

<span data-ttu-id="3a1d8-109">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="3a1d8-109">Learn how to:</span></span>

* <span data-ttu-id="3a1d8-110">Konfigurera en virtuell dator i Azure-toohost Django.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-110">Set up an Azure virtual machine toohost Django.</span></span> <span data-ttu-id="3a1d8-111">Även om den här självstudiekursen beskrivs hur toodo för **Linux**, kan du göra hello samma för en Windows Server-VM finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-111">Although this tutorial explains how toodo this for **Linux**, you can do hello same for a Windows Server VM hosted in Azure.</span></span> 
* <span data-ttu-id="3a1d8-112">Skapa ett nytt Django-program i Linux.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-112">Create a new Django application in Linux.</span></span>

<span data-ttu-id="3a1d8-113">hello kursen visar hur toobuild ett grundläggande Hello World-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-113">hello tutorial shows you how toobuild a basic Hello World web application.</span></span> <span data-ttu-id="3a1d8-114">hello program finns i en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-114">hello application is hosted in an Azure virtual machine.</span></span>

<span data-ttu-id="3a1d8-115">hello följande skärmbild visar hello slutförts program:</span><span class="sxs-lookup"><span data-stu-id="3a1d8-115">hello following screenshot shows hello completed application:</span></span>

![Ett fönster i webbläsaren visar hello Hello World-sidan i Azure](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-toohost-django"></a><span data-ttu-id="3a1d8-117">Skapa och konfigurera en virtuell dator i Azure-toohost Django</span><span class="sxs-lookup"><span data-stu-id="3a1d8-117">Create and set up an Azure virtual machine toohost Django</span></span>

1. <span data-ttu-id="3a1d8-118">toocreate en virtuell Azure-dator med hello Ubuntu Server 14.04 LTS distribution, se [skapa en virtuell Linux-dator i hello Azure-portalen](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a1d8-118">toocreate an Azure virtual machine with hello Ubuntu Server 14.04 LTS distribution, see [Create a Linux virtual machine in hello Azure portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="3a1d8-119">Du kan också välja en lösenordsautentisering istället för att använda en offentlig SSH-nyckel.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-119">You also can choose password authentication instead of using an SSH public key.</span></span>
2. <span data-ttu-id="3a1d8-120">tooedit hello network security group tooallow inkommande HTTP-trafik tooport 80, se [skapa nätverkssäkerhetsgrupper i hello Azure-portalen](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="3a1d8-120">tooedit hello network security group tooallow incoming HTTP traffic tooport 80, see [Create network security groups in hello Azure portal](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).</span></span>
3. <span data-ttu-id="3a1d8-121">(Valfritt) Den nya virtuella datorn har inte ett fullständigt kvalificerat domännamn (FQDN) som standard.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-121">(Optional) By default, your new virtual machine doesn't have a fully qualified domain name (FQDN).</span></span>  <span data-ttu-id="3a1d8-122">toocreate en virtuell dator med ett FQDN finns [skapa ett fullständigt domännamn i hello Azure-portalen för en Windows VM](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3a1d8-122">toocreate a VM with an FQDN, see [Create an FQDN in hello Azure portal for a Windows VM](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="3a1d8-123">Det här steget krävs inte för den här kursen.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-123">This step is not required for completing this tutorial.</span></span>

## <span data-ttu-id="3a1d8-124"><a id="setup"></a>Ställa in hello utvecklingsmiljö</span><span class="sxs-lookup"><span data-stu-id="3a1d8-124"><a id="setup"> </a>Set up hello development environment</span></span>
> [!NOTE]
> <span data-ttu-id="3a1d8-125">Om du behöver tooinstall Python eller vill toouse hello klientbibliotek finns hello [Python installationsguiden](../../python-how-to-install.md).</span><span class="sxs-lookup"><span data-stu-id="3a1d8-125">If you need tooinstall Python or want toouse hello client libraries, see hello [Python installation guide](../../python-how-to-install.md).</span></span>

<span data-ttu-id="3a1d8-126">hello Ubuntu Linux VM har Python 2.7 förinstallerat, men det finns inte i Apache eller Django.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-126">hello Ubuntu Linux VM has Python 2.7 preinstalled, but it doesn't come with Apache or Django.</span></span> <span data-ttu-id="3a1d8-127">Slutför följande steg tooconnect tooyour VM hello och installera Apache och Django:</span><span class="sxs-lookup"><span data-stu-id="3a1d8-127">Complete hello following steps tooconnect tooyour VM and install Apache and Django:</span></span>

1. <span data-ttu-id="3a1d8-128">Öppna ett nytt fönster i Terminal.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-128">Open a new Terminal window.</span></span>
2. <span data-ttu-id="3a1d8-129">tooconnect toohello Azure VM, ange hello följande kommando.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-129">tooconnect toohello Azure VM, enter hello following command.</span></span> <span data-ttu-id="3a1d8-130">Om du inte har skapat ett fullständigt domännamn, kan du ansluta med hjälp av hello offentlig IP-adress som visas i hello virtuella sammanfattning i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-130">If you didn't create an FQDN, you can connect by using hello public IP address that's displayed in hello virtual machine summary in hello Azure portal.</span></span>
   
       $ ssh yourusername@yourVmUrl
3. <span data-ttu-id="3a1d8-131">tooinstall Django, ange hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3a1d8-131">tooinstall Django, enter hello following commands:</span></span>
   
       $ sudo apt-get install python-setuptools python-pip
       $ sudo pip install django
4. <span data-ttu-id="3a1d8-132">tooinstall Apache med mod wsgi ange hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3a1d8-132">tooinstall Apache with mod-wsgi, enter hello following command:</span></span>
   
       $ sudo apt-get install apache2 libapache2-mod-wsgi

## <a name="create-a-new-django-app"></a><span data-ttu-id="3a1d8-133">Skapa en ny Django-app</span><span class="sxs-lookup"><span data-stu-id="3a1d8-133">Create a new Django app</span></span>
1. <span data-ttu-id="3a1d8-134">toouse SSH tooaccess din VM, öppna hello terminalfönster du använde i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-134">toouse SSH tooaccess your VM, open hello Terminal window you used in hello preceding section.</span></span>
2. <span data-ttu-id="3a1d8-135">toocreate ett nytt projekt i Django ange hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3a1d8-135">toocreate a new Django project, enter hello following commands:</span></span>
   
       $ cd /var/www
       $ sudo django-admin.py startproject helloworld
   
   <span data-ttu-id="3a1d8-136">Hej `django-admin.py` skriptet skapar en grundläggande struktur för Django-baserade webbplatser:</span><span class="sxs-lookup"><span data-stu-id="3a1d8-136">hello `django-admin.py` script generates a basic structure for Django-based websites:</span></span>
   
   * <span data-ttu-id="3a1d8-137">`helloworld/manage.py`hjälper dig att värd för start och stopp där webbplatsen Django-baserade.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-137">`helloworld/manage.py` helps you start hosting and stop hosting your Django-based website.</span></span>
   * <span data-ttu-id="3a1d8-138">`helloworld/helloworld/settings.py`har Django-inställningar för ditt program.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-138">`helloworld/helloworld/settings.py` has Django settings for your application.</span></span>
   * <span data-ttu-id="3a1d8-139">`helloworld/helloworld/urls.py`har hello mappning kod mellan varje URL och vyn.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-139">`helloworld/helloworld/urls.py` has hello mapping code between each URL and its view.</span></span>
3. <span data-ttu-id="3a1d8-140">Skapa en ny fil med namnet views.py i hello /var/www/helloworld/helloworld directory.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-140">In hello /var/www/helloworld/helloworld directory, create a new file named views.py.</span></span> <span data-ttu-id="3a1d8-141">Den här filen har hello vy som återger hello ”hello world”-sida.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-141">This file has hello view that renders hello "hello world" page.</span></span> <span data-ttu-id="3a1d8-142">Ange hello följande kommandon i Redigeraren för koden:</span><span class="sxs-lookup"><span data-stu-id="3a1d8-142">In your code editor, enter hello following commands:</span></span>
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. <span data-ttu-id="3a1d8-143">Ersätt hello innehållet i hello urls.py filen med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3a1d8-143">Replace hello contents of hello urls.py file with hello following commands:</span></span>
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-apache"></a><span data-ttu-id="3a1d8-144">Ställ in Apache</span><span class="sxs-lookup"><span data-stu-id="3a1d8-144">Set up Apache</span></span>
1. <span data-ttu-id="3a1d8-145">Skapa en konfigurationsfil för Apache virtuell värd i hello /etc/apache2/sites-available/helloworld.conf mappen.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-145">In hello /etc/apache2/sites-available/helloworld.conf folder, create an Apache virtual host configuration file.</span></span> <span data-ttu-id="3a1d8-146">Ange hello innehållet toohello följande värden.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-146">Set hello contents toohello following values.</span></span> <span data-ttu-id="3a1d8-147">Ersätt *yourVmName* med hello faktiska namnet på hello datorn du använder (till exempel *pyubuntu*).</span><span class="sxs-lookup"><span data-stu-id="3a1d8-147">Replace *yourVmName* with hello actual name of hello machine you are using (for example, *pyubuntu*).</span></span>
   
       <VirtualHost *:80>
       ServerName yourVmName
       </VirtualHost>
       WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
       WSGIPythonPath /var/www/helloworld
2. <span data-ttu-id="3a1d8-148">tooactivate hello webbplats, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3a1d8-148">tooactivate hello site, use hello following command:</span></span>
   
       $ sudo a2ensite helloworld
3. <span data-ttu-id="3a1d8-149">toorestart Apache, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="3a1d8-149">toorestart Apache, use hello following command:</span></span>
   
       $ sudo service apache2 reload
4. <span data-ttu-id="3a1d8-150">Läsa in hello webbsidan i webbläsaren:</span><span class="sxs-lookup"><span data-stu-id="3a1d8-150">Load hello webpage in your browser:</span></span>
   
   ![Ett fönster i webbläsaren visar hello hello world-sidan i Azure](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

## <a name="shut-down-your-azure-virtual-machine"></a><span data-ttu-id="3a1d8-152">Stäng av den virtuella Azure-datorn</span><span class="sxs-lookup"><span data-stu-id="3a1d8-152">Shut down your Azure virtual machine</span></span>
<span data-ttu-id="3a1d8-153">När du är klar med den här kursen rekommenderar vi att du stänga av eller ta bort hello Azure VM som du skapade för hello kursen.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-153">When you're done with this tutorial, we recommend that you shut down or remove hello Azure VM you created for hello tutorial.</span></span> <span data-ttu-id="3a1d8-154">Detta frigör resurser för andra kurser och du kan undvika medför Azure kostnader.</span><span class="sxs-lookup"><span data-stu-id="3a1d8-154">This frees up resources for other tutorials, and you can avoid incurring Azure usage charges.</span></span>

