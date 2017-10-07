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
# <a name="django-hello-world-web-app-on-a-windows-server-vm"></a><span data-ttu-id="3a3ef-103">Django Hello World webbprogram på en Windows Server-VM</span><span class="sxs-lookup"><span data-stu-id="3a3ef-103">Django Hello World web app on a Windows Server VM</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="3a3ef-104">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och hello klassiska distributionsmodellen](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="3a3ef-104">Azure has two different deployment models for creating and working with resources: [Azure Resource Manager and hello classic deployment model](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="3a3ef-105">Den här artikeln beskriver hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-105">This article describes hello classic deployment model.</span></span> <span data-ttu-id="3a3ef-106">Vi rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-106">We recommend that most new deployments use hello Resource Manager model.</span></span>

<span data-ttu-id="3a3ef-107">De här självstudierna visar hur toohost en Django-baserade webbplats i Windows Server i Azure Virtual Machines.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-107">This tutorial shows you how toohost a Django-based website in Windows Server in Azure Virtual Machines.</span></span> <span data-ttu-id="3a3ef-108">I hello kursen förutsätter vi att några tidigare erfarenheter med Azure.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-108">In hello tutorial, we assume no prior experience with Azure.</span></span> <span data-ttu-id="3a3ef-109">När du är klar hello kursen har du ett Django-program in och körs i hello moln.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-109">When you finish hello tutorial, you can have a Django-based application up and running in hello cloud.</span></span>

<span data-ttu-id="3a3ef-110">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-110">Learn how to:</span></span>

* <span data-ttu-id="3a3ef-111">Konfigurera en virtuell dator i Azure-toohost Django.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-111">Set up an Azure virtual machine toohost Django.</span></span> <span data-ttu-id="3a3ef-112">Även om den här självstudiekursen beskrivs hur toodo för **Windows Server**, kan du göra samma hello för en Linux VM finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-112">Although this tutorial explains how toodo this for **Windows Server**, you can do hello same for a Linux VM hosted in Azure.</span></span>
* <span data-ttu-id="3a3ef-113">Skapa ett nytt Django-program i Windows.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-113">Create a new Django application in Windows.</span></span>

<span data-ttu-id="3a3ef-114">hello kursen visar hur toobuild ett grundläggande Hello World-webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-114">hello tutorial shows you how toobuild a basic Hello World web application.</span></span> <span data-ttu-id="3a3ef-115">hello program finns i en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-115">hello application is hosted in an Azure virtual machine.</span></span>

<span data-ttu-id="3a3ef-116">hello följande skärmbild visar hello slutförts program:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-116">hello following screenshot shows hello completed application:</span></span>

![Ett fönster i webbläsaren visar hello hello world-sidan i Azure][1]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-set-up-an-azure-virtual-machine-toohost-django"></a><span data-ttu-id="3a3ef-118">Skapa och konfigurera en virtuell dator i Azure-toohost Django</span><span class="sxs-lookup"><span data-stu-id="3a3ef-118">Create and set up an Azure virtual machine toohost Django</span></span>

1. <span data-ttu-id="3a3ef-119">toocreate en virtuell Azure-dator med hello distribution av Windows Server 2012 R2 Datacenter, se [skapa en virtuell dator som kör Windows i hello Azure-portalen](tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="3a3ef-119">toocreate an Azure virtual machine with hello Windows Server 2012 R2 Datacenter distribution, see [Create a virtual machine running Windows in hello Azure portal](tutorial.md).</span></span>
2. <span data-ttu-id="3a3ef-120">Ange Azure toodirect port 80 trafik från hello web tooport 80 på hello virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-120">Set Azure toodirect port 80 traffic from hello web tooport 80 on hello virtual machine:</span></span>
   
   1. <span data-ttu-id="3a3ef-121">Gå toohello instrumentpanelen i hello Azure-portalen, och markera den nyligen skapade virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-121">In hello Azure portal, go toohello dashboard and select your newly created virtual machine.</span></span>
   2. <span data-ttu-id="3a3ef-122">Klicka på **Slutpunkter** och sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-122">Click **Endpoints**, and then click **Add**.</span></span>

     ![Lägga till en slutpunkt](./media/python-django-web-app/django-helloworld-add-endpoint-new-portal.png)

   3. <span data-ttu-id="3a3ef-124">På hello **lägga till slutpunkten** sidan för **namn**, ange **HTTP**.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-124">On hello **Add endpoint** page, for **Name**, enter **HTTP**.</span></span> <span data-ttu-id="3a3ef-125">Ställ in hello offentliga och privata TCP-portar för**80**.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-125">Set hello public and private TCP ports too**80**.</span></span>

     ![Ange namn och ange offentliga och privata portar](./media/python-django-web-app/django-helloworld-add-endpoint-set-ports-new-portal.png)

   4. <span data-ttu-id="3a3ef-127">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-127">Click **OK**.</span></span>
     
3. <span data-ttu-id="3a3ef-128">I hello instrumentpanelen, väljer du den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-128">In hello dashboard, select your VM.</span></span> <span data-ttu-id="3a3ef-129">toouse Remote Desktop Protocol (RDP) tooremotely inloggning toohello nyligen skapade virtuella Azure-datorn, klickar du på **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-129">toouse Remote Desktop Protocol (RDP) tooremotely sign in toohello newly created Azure virtual machine, click **Connect**.</span></span>  

> [!IMPORTANT] 
> <span data-ttu-id="3a3ef-130">hello följa anvisningarna förutsätter att du loggat in toohello virtuella datorn på rätt sätt.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-130">hello following instructions assume that you signed in toohello virtual machine correctly.</span></span> <span data-ttu-id="3a3ef-131">De förutsätter att du utfärdar kommandon i hello virtuella datorn och inte på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-131">They also assume that you are issuing commands in hello virtual machine and not on your local computer.</span></span>

## <span data-ttu-id="3a3ef-132"><a id="setup"></a>Installera Python och Django WFastCGI</span><span class="sxs-lookup"><span data-stu-id="3a3ef-132"><a id="setup"> </a>Install Python, Django, and WFastCGI</span></span>
> [!NOTE]
> <span data-ttu-id="3a3ef-133">toodownload med hjälp av Internet Explorer kan du ha tooconfigure Internet Explorer **Förbättrad säkerhetskonfiguration** inställningar.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-133">toodownload by using Internet Explorer, you might have tooconfigure Internet Explorer **Enhanced Security Configuration** settings.</span></span> <span data-ttu-id="3a3ef-134">toodo, genom att välja **starta** > **Administrationsverktyg** > **Serverhanteraren** > **lokal Server**.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-134">toodo this, click **Start** > **Administrative Tools** > **Server Manager** > **Local Server**.</span></span> <span data-ttu-id="3a3ef-135">Klicka på **Förbättrad säkerhetskonfiguration**, och välj sedan **av**.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-135">Click **IE Enhanced Security Configuration**, and then select **Off**.</span></span>

1. <span data-ttu-id="3a3ef-136">Installera hello senaste versionerna av Python 2.7 eller Python 3.4 från [python.org][python.org].</span><span class="sxs-lookup"><span data-stu-id="3a3ef-136">Install hello latest versions of Python 2.7 or Python 3.4 from [python.org][python.org].</span></span>
2. <span data-ttu-id="3a3ef-137">Installera hello wfastcgi och django paket med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-137">Install hello wfastcgi and django packages using pip.</span></span>
   
    <span data-ttu-id="3a3ef-138">För Python 2.7, använder du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-138">For Python 2.7, use hello following command:</span></span>
   
        c:\python27\scripts\pip install wfastcgi
        c:\python27\scripts\pip install django
   
    <span data-ttu-id="3a3ef-139">För Python 3.4, använder du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-139">For Python 3.4, use hello following command:</span></span>
   
        c:\python34\scripts\pip install wfastcgi
        c:\python34\scripts\pip install django

## <a name="install-iis-with-fastcgi"></a><span data-ttu-id="3a3ef-140">Installera IIS med FastCGI</span><span class="sxs-lookup"><span data-stu-id="3a3ef-140">Install IIS with FastCGI</span></span>
* <span data-ttu-id="3a3ef-141">Installera Internet Information Services (IIS) med stöd för FastCGI.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-141">Install Internet Information Services (IIS) with FastCGI support.</span></span> <span data-ttu-id="3a3ef-142">Det kan ta flera minuter tooexecute.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-142">This might take several minutes tooexecute.</span></span>
   
        start /wait %windir%\System32\PkgMgr.exe /iu:IIS-WebServerRole;IIS-WebServer;IIS-CommonHttpFeatures;IIS-StaticContent;IIS-DefaultDocument;IIS-DirectoryBrowsing;IIS-HttpErrors;IIS-HealthAndDiagnostics;IIS-HttpLogging;IIS-LoggingLibraries;IIS-RequestMonitor;IIS-Security;IIS-RequestFiltering;IIS-HttpCompressionStatic;IIS-WebServerManagementTools;IIS-ManagementConsole;WAS-WindowsActivationService;WAS-ProcessModel;WAS-NetFxEnvironment;WAS-ConfigurationAPI;IIS-CGI

## <a name="create-a-new-django-application"></a><span data-ttu-id="3a3ef-143">Skapa ett nytt Django-program</span><span class="sxs-lookup"><span data-stu-id="3a3ef-143">Create a new Django application</span></span>
1. <span data-ttu-id="3a3ef-144">Ange följande kommando hello i C:\inetpub\wwwroot toocreate ett nytt projekt i Django:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-144">In C:\inetpub\wwwroot, toocreate a new Django project, enter hello following command:</span></span>
   
   <span data-ttu-id="3a3ef-145">För Python 2.7, använder du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-145">For Python 2.7, use hello following command:</span></span>
   
       C:\Python27\Scripts\django-admin.exe startproject helloworld
   
   <span data-ttu-id="3a3ef-146">För Python 3.4, använder du följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-146">For Python 3.4, use hello following command:</span></span>
   
       C:\Python34\Scripts\django-admin.exe startproject helloworld
   
   ![hello resultatet av kommandot hello New-AzureService](./media/python-django-web-app/django-helloworld-cmd-new-azure-service.png)
2. <span data-ttu-id="3a3ef-148">Hej `django-admin` kommando skapar en grundläggande struktur för Django-baserade webbplatser:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-148">hello `django-admin` command generates a basic structure for Django-based websites:</span></span>
   
   * <span data-ttu-id="3a3ef-149">`helloworld\manage.py`hjälper dig att värd för start och stopp där webbplatsen Django-baserade.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-149">`helloworld\manage.py` helps you start hosting and stop hosting your Django-based website.</span></span>
   * <span data-ttu-id="3a3ef-150">`helloworld\helloworld\settings.py`har Django-inställningar för ditt program.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-150">`helloworld\helloworld\settings.py` has Django settings for your application.</span></span>
   * <span data-ttu-id="3a3ef-151">`helloworld\helloworld\urls.py`har hello mappning kod mellan varje URL och vyn.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-151">`helloworld\helloworld\urls.py` has hello mapping code between each URL and its view.</span></span>
3. <span data-ttu-id="3a3ef-152">Skapa en ny fil med namnet views.py i hello C:\inetpub\wwwroot\helloworld\helloworld directory.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-152">In hello C:\inetpub\wwwroot\helloworld\helloworld directory, create a new file named views.py.</span></span> <span data-ttu-id="3a3ef-153">Den här filen har hello vy som återger hello ”hello world”-sida.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-153">This file has hello view that renders hello "hello world" page.</span></span> <span data-ttu-id="3a3ef-154">Ange hello följande kommandon i Redigeraren för koden:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-154">In your code editor, enter hello following commands:</span></span>
   
       from django.http import HttpResponse
       def home(request):
           html = "<html><body>Hello World!</body></html>"
           return HttpResponse(html)
4. <span data-ttu-id="3a3ef-155">Ersätt hello innehållet i hello urls.py filen med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-155">Replace hello contents of hello urls.py file with hello following commands:</span></span>
   
       from django.conf.urls import patterns, url
       urlpatterns = patterns('',
           url(r'^$', 'helloworld.views.home', name='home'),
       )

## <a name="set-up-iis"></a><span data-ttu-id="3a3ef-156">Konfigurera IIS</span><span class="sxs-lookup"><span data-stu-id="3a3ef-156">Set up IIS</span></span>
1. <span data-ttu-id="3a3ef-157">Låsa upp hello hanteraravsnittet i hello globala applicationHost.config.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-157">In hello global applicationhost.config file, unlock hello handlers section.</span></span>  <span data-ttu-id="3a3ef-158">Detta gör att din web.config toouse hello Python Filhanteraren.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-158">This allows your web.config file toouse hello Python handler.</span></span> <span data-ttu-id="3a3ef-159">Lägg till det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-159">Add this command:</span></span>
   
        %windir%\system32\inetsrv\appcmd unlock config -section:system.webServer/handlers
2. <span data-ttu-id="3a3ef-160">Aktivera WFastCGI.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-160">Activate WFastCGI.</span></span> <span data-ttu-id="3a3ef-161">Detta lägger till en programmet toohello globala applicationhost.config-fil som refererar tooyour Python-tolken körbar fil och hello wfastcgi.py-skriptet.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-161">This adds an application toohello global applicationhost.config file, which refers tooyour Python interpreter executable and hello wfastcgi.py script.</span></span>
   
    <span data-ttu-id="3a3ef-162">I Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-162">In Python 2.7:</span></span>
   
        C:\python27\scripts\wfastcgi-enable
   
    <span data-ttu-id="3a3ef-163">I Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-163">In Python 3.4:</span></span>
   
        C:\python34\scripts\wfastcgi-enable
3. <span data-ttu-id="3a3ef-164">I C:\inetpub\wwwroot\helloworld, skapar du en web.config-fil.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-164">In C:\inetpub\wwwroot\helloworld, create a web.config file.</span></span> <span data-ttu-id="3a3ef-165">Hej värdet för hello `scriptProcessor` attributet ska matcha hello utdata från hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-165">hello value of hello `scriptProcessor` attribute should match hello output from hello preceding step.</span></span> <span data-ttu-id="3a3ef-166">Mer information om hur du hello wfastcgi finns [pypi wfastcgi][wfastcgi].</span><span class="sxs-lookup"><span data-stu-id="3a3ef-166">For more information about hello wfastcgi setting, see [pypi wfastcgi][wfastcgi].</span></span>
   
   <span data-ttu-id="3a3ef-167">I Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-167">In  Python 2.7:</span></span>
   
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
   
   <span data-ttu-id="3a3ef-168">I Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-168">In  Python 3.4:</span></span>
   
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
4. <span data-ttu-id="3a3ef-169">Uppdatera hello platsen för hello IIS standard webbplats toopoint toohello Django projektmappen:</span><span class="sxs-lookup"><span data-stu-id="3a3ef-169">Update hello location of hello IIS default website toopoint toohello Django project folder:</span></span>
   
        %windir%\system32\inetsrv\appcmd set vdir "Default Web Site/" -physicalPath:"C:\inetpub\wwwroot\helloworld"
5. <span data-ttu-id="3a3ef-170">Läsa in hello webbsidan i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-170">Load hello webpage in your browser.</span></span>

![Ett fönster i webbläsaren visar hello hello world-sidan på Azure][1]

## <a name="shut-down-your-azure-virtual-machine"></a><span data-ttu-id="3a3ef-172">Stäng av den virtuella Azure-datorn</span><span class="sxs-lookup"><span data-stu-id="3a3ef-172">Shut down your Azure virtual machine</span></span>
<span data-ttu-id="3a3ef-173">När du är klar med den här kursen rekommenderar vi att du stänga av eller ta bort hello Azure VM som du skapade för hello kursen.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-173">When you're done with this tutorial, we recommend that you shut down or remove hello Azure VM you created for hello tutorial.</span></span> <span data-ttu-id="3a3ef-174">Detta frigör resurser för andra kurser och du kan undvika medför Azure kostnader.</span><span class="sxs-lookup"><span data-stu-id="3a3ef-174">This frees up resources for other tutorials, and you can avoid incurring Azure usage charges.</span></span>

[1]: ./media/python-django-web-app/django-helloworld-browser-azure.png

[port80]: ./media/python-django-web-app/django-helloworld-port80.png

[Web Platform Installer]: http://www.microsoft.com/web/downloads/platform.aspx
[python.org]: https://www.python.org/downloads/
[wfastcgi]: https://pypi.python.org/pypi/wfastcgi
