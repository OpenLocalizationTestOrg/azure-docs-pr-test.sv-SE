---
title: "aaaDjango webbprogram på en Windows Server Azure VM | Microsoft Docs"
description: "Lär dig hur toohost en Django-baserade webbplats i Azure med hjälp av en Windows Server 2012 R2 Datacenter VM med hello klassiska distributionsmodellen."
services: virtual-machines-windows
documentationcenter: python
author: huguesv
manager: wpickett
editor: 
tags: azure-service-management
ms.assetid: e36484d1-afbf-47f5-b755-5e65397dc1c3
ms.service: virtual-machines-windows
ms.workload: web
ms.tgt_pltfrm: vm-windows
ms.devlang: python
ms.topic: article
ms.date: 05/31/2017
ms.author: huvalo
ms.openlocfilehash: 55847e3c6d6769965be29077e8d4eeebad914637
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="django-hello-world-web-app-on-a-windows-server-vm"></a>Django Hello World webbprogram på en Windows Server-VM

> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och hello klassiska distributionsmodellen](../../../resource-manager-deployment-model.md). Den här artikeln beskriver hello klassiska distributionsmodellen. Vi rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

De här självstudierna visar hur toohost en Django-baserade webbplats i Windows Server i Azure Virtual Machines. I hello kursen förutsätter vi att några tidigare erfarenheter med Azure. När du är klar hello kursen har du ett Django-program in och körs i hello moln.

Lär dig att:

* Konfigurera en virtuell dator i Azure-toohost Django. Även om den här självstudiekursen beskrivs hur toodo för **Windows Server**, kan du göra samma hello för en Linux VM finns i Azure.
* Skapa ett nytt Django-program i Windows.

hello kursen visar hur toobuild ett grundläggande Hello World-webbprogram. hello program finns i en virtuell Azure-dator.

hello följande skärmbild visar hello slutförts program:

![Ett fönster i webbläsaren visar hello hello world-sidan i Azure][1]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-toohost-django"></a>Skapa och konfigurera en virtuell dator i Azure-toohost Django

1. toocreate en virtuell Azure-dator med hello distribution av Windows Server 2012 R2 Datacenter, se [skapa en virtuell dator som kör Windows i hello Azure-portalen](tutorial.md).
2. Ange Azure toodirect port 80 trafik från hello web tooport 80 på hello virtuell dator:
   
   1. Gå toohello instrumentpanelen i hello Azure-portalen, och markera den nyligen skapade virtuella datorn.
   2. Klicka på **Slutpunkter** och sedan på **Lägg till**.

     ![Lägga till en slutpunkt](./media/python-django-web-app/django-helloworld-add-endpoint-new-portal.png)

   3. På hello **lägga till slutpunkten** sidan för **namn**, ange **HTTP**. Ställ in hello offentliga och privata TCP-portar för**80**.

     ![Ange namn och ange offentliga och privata portar](./media/python-django-web-app/django-helloworld-add-endpoint-set-ports-new-portal.png)

   4. Klicka på **OK**.
     
3. I hello instrumentpanelen, väljer du den virtuella datorn. toouse Remote Desktop Protocol (RDP) tooremotely inloggning toohello nyligen skapade virtuella Azure-datorn, klickar du på **Anslut**.  

> [!IMPORTANT] 
> hello följa anvisningarna förutsätter att du loggat in toohello virtuella datorn på rätt sätt. De förutsätter att du utfärdar kommandon i hello virtuella datorn och inte på den lokala datorn.

## <a id="setup"></a>Installera Python och Django WFastCGI
> [!NOTE]
> toodownload med hjälp av Internet Explorer kan du ha tooconfigure Internet Explorer **Förbättrad säkerhetskonfiguration** inställningar. toodo, genom att välja **starta** > **Administrationsverktyg** > **Serverhanteraren** > **lokal Server**. Klicka på **Förbättrad säkerhetskonfiguration**, och välj sedan **av**.

1. Installera hello senaste versionerna av Python 2.7 eller Python 3.4 från [python.org][python.org].
2. Installera hello wfastcgi och django paket med hjälp av pip.
   
    För Python 2.7, använder du följande kommando hello:
   
        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django
   
    För Python 3.4, använder du följande kommando hello:
   
        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="install-iis-with-fastcgi"></a>Installera IIS med FastCGI
* Installera Internet Information Services (IIS) med stöd för FastCGI. Det kan ta flera minuter tooexecute.
   
        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="create-a-new-django-application"></a>Skapa ett nytt Django-program
1. Ange följande kommando hello i C:\inetpub\wwwroot toocreate ett nytt projekt i Django:
   
   För Python 2.7, använder du följande kommando hello:
   
       C:\Python27\Scripts\django-admin.exe startproject helloworld
   
   För Python 3.4, använder du följande kommando hello:
   
       C:\Python34\Scripts\django-admin.exe startproject helloworld
   
   ![hello resultatet av kommandot hello New-AzureService](./media/python-django-web-app/django-helloworld-cmd-new-azure-service.png)
2. Hej `django-admin` kommando skapar en grundläggande struktur för Django-baserade webbplatser:
   
   * `helloworld\manage.py`hjälper dig att värd för start och stopp där webbplatsen Django-baserade.
   * `helloworld\helloworld\settings.py`har Django-inställningar för ditt program.
   * `helloworld\helloworld\urls.py`har hello mappning kod mellan varje URL och vyn.
3. Skapa en ny fil med namnet views.py i hello C:\inetpub\wwwroot\helloworld\helloworld directory. Den här filen har hello vy som återger hello ”hello world”-sida. Ange hello följande kommandon i Redigeraren för koden:
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. Ersätt hello innehållet i hello urls.py filen med hello följande kommandon:
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-iis"></a>Konfigurera IIS
1. Låsa upp hello hanteraravsnittet i hello globala applicationHost.config.  Detta gör att din web.config toouse hello Python Filhanteraren. Lägg till det här kommandot:
   
        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers
2. Aktivera WFastCGI. Detta lägger till en programmet toohello globala applicationhost.config-fil som refererar tooyour Python-tolken körbar fil och hello wfastcgi.py-skriptet.
   
    I Python 2.7:
   
        C:\python27\scripts\wfastcgi-enable
   
    I Python 3.4:
   
        C:\python34\scripts\wfastcgi-enable
3. I C:\inetpub\wwwroot\helloworld, skapar du en web.config-fil. Hej värdet för hello `scriptProcessor` attributet ska matcha hello utdata från hello föregående steg. Mer information om hur du hello wfastcgi finns [pypi wfastcgi][wfastcgi].
   
   I Python 2.7:
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python27\python.exe|C:\Python27\Lib\site-packages\wfastcgi.pyc" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
   
   I Python 3.4:
   
        <configuration>
          <appSettings>
            <add key="WSGI_HANDLER" value="django.core.handlers.wsgi.WSGIHandler()" />
            <add key="PYTHONPATH" value="C:\inetpub\wwwroot\helloworld" />
            <add key="DJANGO_SETTINGS_MODULE" value="helloworld.settings" />
          </appSettings>
          <system.webServer>
            <handlers>
                <add name="Python FastCGI" path="*" verb="*" modules="FastCgiModule" scriptProcessor="C:\Python34\python.exe|C:\Python34\Lib\site-packages\wfastcgi.py" resourceType="Unspecified" />
            </handlers>
          </system.webServer>
        </configuration>
4. Uppdatera hello platsen för hello IIS standard webbplats toopoint toohello Django projektmappen:
   
        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"
5. Läsa in hello webbsidan i webbläsaren.

![Ett fönster i webbläsaren visar hello hello world-sidan på Azure][1]

## <a name="shut-down-your-azure-virtual-machine"></a>Stäng av den virtuella Azure-datorn
När du är klar med den här kursen rekommenderar vi att du stänga av eller ta bort hello Azure VM som du skapade för hello kursen. Detta frigör resurser för andra kurser och du kan undvika medför Azure kostnader.

[1]: ./media/python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
