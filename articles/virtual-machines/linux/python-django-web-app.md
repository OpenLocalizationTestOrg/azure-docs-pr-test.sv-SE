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
# <a name="django-hello-world-web-app-on-a-linux-vm"></a>Django Hello World webbprogram på en Linux-VM
> [!div class="op_single_selector"]
> * [Windows](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
> * [Mac/Linux](../windows/classic/python-django-web-app.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> 
> 

<br>

De här självstudierna visar hur toohost en Django-baserade webbplats i Linux i Azure Virtual Machines. I hello kursen förutsätter vi att några tidigare erfarenheter med Azure. När du är klar hello kursen har du ett Django-program in och körs i hello moln.

Lär dig att:

* Konfigurera en virtuell dator i Azure-toohost Django. Även om den här självstudiekursen beskrivs hur toodo för **Linux**, kan du göra hello samma för en Windows Server-VM finns i Azure. 
* Skapa ett nytt Django-program i Linux.

hello kursen visar hur toobuild ett grundläggande Hello World-webbprogram. hello program finns i en virtuell Azure-dator.

hello följande skärmbild visar hello slutförts program:

![Ett fönster i webbläsaren visar hello Hello World-sidan i Azure](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-toohost-django"></a>Skapa och konfigurera en virtuell dator i Azure-toohost Django

1. toocreate en virtuell Azure-dator med hello Ubuntu Server 14.04 LTS distribution, se [skapa en virtuell Linux-dator i hello Azure-portalen](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Du kan också välja en lösenordsautentisering istället för att använda en offentlig SSH-nyckel.
2. tooedit hello network security group tooallow inkommande HTTP-trafik tooport 80, se [skapa nätverkssäkerhetsgrupper i hello Azure-portalen](../../virtual-network/virtual-networks-create-nsg-arm-pportal.md).
3. (Valfritt) Den nya virtuella datorn har inte ett fullständigt kvalificerat domännamn (FQDN) som standard.  toocreate en virtuell dator med ett FQDN finns [skapa ett fullständigt domännamn i hello Azure-portalen för en Windows VM](../windows/portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Det här steget krävs inte för den här kursen.

## <a id="setup"></a>Ställa in hello utvecklingsmiljö
> [!NOTE]
> Om du behöver tooinstall Python eller vill toouse hello klientbibliotek finns hello [Python installationsguiden](../../python-how-to-install.md).

hello Ubuntu Linux VM har Python 2.7 förinstallerat, men det finns inte i Apache eller Django. Slutför följande steg tooconnect tooyour VM hello och installera Apache och Django:

1. Öppna ett nytt fönster i Terminal.
2. tooconnect toohello Azure VM, ange hello följande kommando. Om du inte har skapat ett fullständigt domännamn, kan du ansluta med hjälp av hello offentlig IP-adress som visas i hello virtuella sammanfattning i hello Azure-portalen.
   
       $ ssh yourusername@yourVmUrl
3. tooinstall Django, ange hello följande kommandon:
   
       $ sudo apt-get install python-setuptools python-pip
       $ sudo pip install django
4. tooinstall Apache med mod wsgi ange hello följande kommando:
   
       $ sudo apt-get install apache2 libapache2-mod-wsgi

## <a name="create-a-new-django-app"></a>Skapa en ny Django-app
1. toouse SSH tooaccess din VM, öppna hello terminalfönster du använde i hello föregående avsnitt.
2. toocreate ett nytt projekt i Django ange hello följande kommandon:
   
       $ cd /var/www
       $ sudo django-admin.py startproject helloworld
   
   Hej `django-admin.py` skriptet skapar en grundläggande struktur för Django-baserade webbplatser:
   
   * `helloworld/manage.py`hjälper dig att värd för start och stopp där webbplatsen Django-baserade.
   * `helloworld/helloworld/settings.py`har Django-inställningar för ditt program.
   * `helloworld/helloworld/urls.py`har hello mappning kod mellan varje URL och vyn.
3. Skapa en ny fil med namnet views.py i hello /var/www/helloworld/helloworld directory. Den här filen har hello vy som återger hello ”hello world”-sida. Ange hello följande kommandon i Redigeraren för koden:
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. Ersätt hello innehållet i hello urls.py filen med hello följande kommandon:
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-apache"></a>Ställ in Apache
1. Skapa en konfigurationsfil för Apache virtuell värd i hello /etc/apache2/sites-available/helloworld.conf mappen. Ange hello innehållet toohello följande värden. Ersätt *yourVmName* med hello faktiska namnet på hello datorn du använder (till exempel *pyubuntu*).
   
       <VirtualHost *:80>
       ServerName yourVmName
       </VirtualHost>
       WSGIScriptAlias / /var/www/helloworld/helloworld/wsgi.py
       WSGIPythonPath /var/www/helloworld
2. tooactivate hello webbplats, Använd hello följande kommando:
   
       $ sudo a2ensite helloworld
3. toorestart Apache, Använd hello följande kommando:
   
       $ sudo service apache2 reload
4. Läsa in hello webbsidan i webbläsaren:
   
   ![Ett fönster i webbläsaren visar hello hello world-sidan i Azure](./media/python-django-web-app/mac-linux-django-helloworld-browser.png)

## <a name="shut-down-your-azure-virtual-machine"></a>Stäng av den virtuella Azure-datorn
När du är klar med den här kursen rekommenderar vi att du stänga av eller ta bort hello Azure VM som du skapade för hello kursen. Detta frigör resurser för andra kurser och du kan undvika medför Azure kostnader.

