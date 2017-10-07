---
title: aaaConfiguring Python med Azure App Service Web Apps
description: "Den här självstudiekursen beskrivs alternativ för redigering och konfigurerar en enkel webbserver Gateway Interface (WSGI) kompatibla Python-program i Azure App Service Web Apps."
services: app-service
documentationcenter: python
tags: python
author: huguesv
manager: erikre
editor: 
ms.assetid: fd00dc91-9935-4331-b955-4bd71e66d518
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 02/26/2016
ms.author: huvalo
ms.openlocfilehash: 00d49fb01491e9adb4b6fededfb95669a8dbd485
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-python-with-azure-app-service-web-apps"></a><span data-ttu-id="31b20-103">Konfigurera Python med Azure App Service Web Apps</span><span class="sxs-lookup"><span data-stu-id="31b20-103">Configuring Python with Azure App Service Web Apps</span></span>
<span data-ttu-id="31b20-104">Den här självstudiekursen beskrivs alternativ för redigering och konfigurera en grundläggande Web Server Gateway Interface (WSGI) kompatibla Python-program på [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span><span class="sxs-lookup"><span data-stu-id="31b20-104">This tutorial describes options for authoring and configuring a basic Web Server Gateway Interface (WSGI) compliant Python application on [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).</span></span>

<span data-ttu-id="31b20-105">Beskriver ytterligare funktioner för Git-distribution, till exempel virtuell miljö och installationen med hjälp av requirements.txt.</span><span class="sxs-lookup"><span data-stu-id="31b20-105">It describes additional features of Git deployment, such as virtual environment and package installation using requirements.txt.</span></span>

## <a name="bottle-django-or-flask"></a><span data-ttu-id="31b20-106">Bottle, Django eller Flask?</span><span class="sxs-lookup"><span data-stu-id="31b20-106">Bottle, Django or Flask?</span></span>
<span data-ttu-id="31b20-107">hello Azure Marketplace innehåller mallar för hello Bottle, Django och Flask ramverk.</span><span class="sxs-lookup"><span data-stu-id="31b20-107">hello Azure Marketplace contains templates for hello Bottle, Django and Flask frameworks.</span></span> <span data-ttu-id="31b20-108">Om du utvecklar din första webbapp i Azure App Service eller om du inte är bekant med Git, rekommenderar vi att du följer någon av dessa självstudier med stegvisa instruktioner för att skapa ett fungerande program från hello galleriet med Git-distribution från Windows- eller Mac:</span><span class="sxs-lookup"><span data-stu-id="31b20-108">If you are developing your first web app in Azure App Service, or you are not familiar with Git, we recommend that you follow one of these tutorials, which include step-by-step instructions for building a working application from hello gallery using Git deployment from Windows or Mac:</span></span>

* [<span data-ttu-id="31b20-109">Skapa webbappar med Bottle</span><span class="sxs-lookup"><span data-stu-id="31b20-109">Creating web apps with Bottle</span></span>](web-sites-python-create-deploy-bottle-app.md)
* [<span data-ttu-id="31b20-110">Skapa webbappar med Django</span><span class="sxs-lookup"><span data-stu-id="31b20-110">Creating web apps with Django</span></span>](web-sites-python-create-deploy-django-app.md)
* [<span data-ttu-id="31b20-111">Skapa webbappar med Flask</span><span class="sxs-lookup"><span data-stu-id="31b20-111">Creating web apps with Flask</span></span>](web-sites-python-create-deploy-flask-app.md)

## <a name="web-app-creation-on-azure-portal"></a><span data-ttu-id="31b20-112">Skapa en webbapp i Azure Portal</span><span class="sxs-lookup"><span data-stu-id="31b20-112">Web app creation on Azure Portal</span></span>
<span data-ttu-id="31b20-113">Den här kursen förutsätter att en befintlig Azure-prenumeration och åtkomst toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="31b20-113">This tutorial assumes an existing Azure subscription and access toohello Azure Portal.</span></span>

<span data-ttu-id="31b20-114">Om du inte har en befintlig webbapp, kan du skapa en från hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="31b20-114">If you do not have an existing web app, you can create one from hello [Azure Portal](https://portal.azure.com).</span></span>  <span data-ttu-id="31b20-115">Klicka hello nya i hello övre vänstra hörnet och klicka sedan på **webb + mobilt** > **webbapp**.</span><span class="sxs-lookup"><span data-stu-id="31b20-115">Click hello NEW button in hello top left corner, then click **Web + Mobile** > **Web app**.</span></span>

## <a name="git-publishing"></a><span data-ttu-id="31b20-116">Git-publicering</span><span class="sxs-lookup"><span data-stu-id="31b20-116">Git Publishing</span></span>
<span data-ttu-id="31b20-117">Konfigurera Git-publicering för nyskapade webbappen genom att följa anvisningarna hello på [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md).</span><span class="sxs-lookup"><span data-stu-id="31b20-117">Configure Git publishing for your newly created web app by following hello instructions at [Local Git Deployment tooAzure App Service](app-service-deploy-local-git.md).</span></span> <span data-ttu-id="31b20-118">Den här kursen använder Git toocreate, hantera och publicera våra Python web app tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="31b20-118">This tutorial uses Git toocreate, manage, and publish our Python web app tooAzure App Service.</span></span>

<span data-ttu-id="31b20-119">När Git-publicering har ställts in, skapas en Git-lagringsplats och som är kopplad till ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="31b20-119">Once Git publishing is set up, a Git repository will be created and associated with your web app.</span></span> <span data-ttu-id="31b20-120">hello databasen URL visas och kan hädanefter används toopush data från hello lokal utveckling miljö toohello molnet.</span><span class="sxs-lookup"><span data-stu-id="31b20-120">hello repository's URL will be displayed and can henceforth be used toopush data from hello local development environment toohello cloud.</span></span> <span data-ttu-id="31b20-121">toopublish program via Git, kontrollera att en Git-klient installeras även och Använd hello instruktioner som toopush din web app innehåll tooAzure Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="31b20-121">toopublish applications via Git, make sure a Git client is also installed and use hello instructions provided toopush your web app content tooAzure App Service.</span></span>

## <a name="application-overview"></a><span data-ttu-id="31b20-122">Programöversikt</span><span class="sxs-lookup"><span data-stu-id="31b20-122">Application Overview</span></span>
<span data-ttu-id="31b20-123">I nästa avsnitt hello skapas hello följande filer.</span><span class="sxs-lookup"><span data-stu-id="31b20-123">In hello next sections, hello following files are created.</span></span> <span data-ttu-id="31b20-124">De ska placeras i hello rot hello Git-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="31b20-124">They should be placed in hello root of hello Git repository.</span></span>

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a><span data-ttu-id="31b20-125">WSGI hanterare</span><span class="sxs-lookup"><span data-stu-id="31b20-125">WSGI Handler</span></span>
<span data-ttu-id="31b20-126">WSGI är en Python-standard som beskrivs av [program 3333](http://www.python.org/dev/peps/pep-3333/) definierar ett gränssnitt mellan hello webbserver och Python.</span><span class="sxs-lookup"><span data-stu-id="31b20-126">WSGI is a Python standard described by [PEP 3333](http://www.python.org/dev/peps/pep-3333/) defining an interface between hello web server and Python.</span></span> <span data-ttu-id="31b20-127">Det ger en standardiserad gränssnitt för att skriva olika webbaserade program och ramverk som använder Python.</span><span class="sxs-lookup"><span data-stu-id="31b20-127">It provides a standardized interface for writing various web applications and frameworks using Python.</span></span> <span data-ttu-id="31b20-128">Populära Python web ramverk använder dag WSGI.</span><span class="sxs-lookup"><span data-stu-id="31b20-128">Popular Python web frameworks today use WSGI.</span></span> <span data-ttu-id="31b20-129">Azure App Service Web Apps får du stöd för ramverk; Dessutom kan avancerade användare även skapa egna så länge hello anpassad hanterare följer riktlinjer för hello WSGI-specifikationen.</span><span class="sxs-lookup"><span data-stu-id="31b20-129">Azure App Service Web Apps gives you support for any such frameworks; in addition, advanced users can even author their own as long as hello custom handler follows hello WSGI specification guidelines.</span></span>

<span data-ttu-id="31b20-130">Här är ett exempel på en `app.py` som definierar en anpassad hanterare:</span><span class="sxs-lookup"><span data-stu-id="31b20-130">Here's an example of an `app.py` that defines a custom handler:</span></span>

    def wsgi_app(environ, start_response):
        status = '200 OK'
        response_headers = [('Content-type', 'text/plain')]
        start_response(status, response_headers)
        response_body = 'Hello World'
        yield response_body.encode()

    if __name__ == '__main__':
        from wsgiref.simple_server import make_server

        httpd = make_server('localhost', 5555, wsgi_app)
        httpd.serve_forever()

<span data-ttu-id="31b20-131">Du kan köra programmet lokalt med `python app.py`, bläddrar sedan för`http://localhost:5555` i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="31b20-131">You can run this application locally with `python app.py`, then browse too`http://localhost:5555` in your web browser.</span></span>

## <a name="virtual-environment"></a><span data-ttu-id="31b20-132">Virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="31b20-132">Virtual Environment</span></span>
<span data-ttu-id="31b20-133">Även om hello exempelapp ovan inte kräver några externa paket, är det troligt att programmet kommer att kräva några.</span><span class="sxs-lookup"><span data-stu-id="31b20-133">Although hello example app above doesn't require any external packages, it is likely that your application will require some.</span></span>

<span data-ttu-id="31b20-134">toohelp hantera externa paketberoenden, Azure Git-distribution stöder hello skapandet av virtuella miljöer.</span><span class="sxs-lookup"><span data-stu-id="31b20-134">toohelp manage external package dependencies, Azure Git deployment supports hello creation of virtual environments.</span></span>

<span data-ttu-id="31b20-135">När Azure identifierar en requirements.txt i hello rot hello-databasen, skapas automatiskt en virtuell miljö som heter `env`.</span><span class="sxs-lookup"><span data-stu-id="31b20-135">When Azure detects a requirements.txt in hello root of hello repository, it automatically creates a virtual environment named `env`.</span></span> <span data-ttu-id="31b20-136">Det här inträffar bara på hello första distributionen eller under en distribution när hello valda Python-körning har ändrats.</span><span class="sxs-lookup"><span data-stu-id="31b20-136">This only occurs on hello first deployment, or during any deployment after hello selected Python runtime has changed.</span></span>

<span data-ttu-id="31b20-137">Du vill förmodligen toocreate en virtuell miljö lokalt för utveckling, men inkludera inte den i Git-lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="31b20-137">You will probably want toocreate a virtual environment locally for development, but don't include it in your Git repository.</span></span>

## <a name="package-management"></a><span data-ttu-id="31b20-138">Pakethantering</span><span class="sxs-lookup"><span data-stu-id="31b20-138">Package Management</span></span>
<span data-ttu-id="31b20-139">Paket som anges i requirements.txt installeras automatiskt i hello virtuell miljö med hjälp av pip.</span><span class="sxs-lookup"><span data-stu-id="31b20-139">Packages listed in requirements.txt will be installed automatically in hello virtual environment using pip.</span></span> <span data-ttu-id="31b20-140">Detta sker vid varje distribution, men pip hoppar över installationen om ett paket har redan installerats.</span><span class="sxs-lookup"><span data-stu-id="31b20-140">This happens on every deployment, but pip will skip installation if a package is already installed.</span></span>

<span data-ttu-id="31b20-141">Exempel `requirements.txt`:</span><span class="sxs-lookup"><span data-stu-id="31b20-141">Example `requirements.txt`:</span></span>

    azure==0.8.4


## <a name="python-version"></a><span data-ttu-id="31b20-142">Python-versionen</span><span class="sxs-lookup"><span data-stu-id="31b20-142">Python Version</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

<span data-ttu-id="31b20-143">Exempel `runtime.txt`:</span><span class="sxs-lookup"><span data-stu-id="31b20-143">Example `runtime.txt`:</span></span>

    python-2.7


## <a name="webconfig"></a><span data-ttu-id="31b20-144">Web.config</span><span class="sxs-lookup"><span data-stu-id="31b20-144">Web.config</span></span>
<span data-ttu-id="31b20-145">Du behöver toocreate en web.config-filen toospecify hur hello server ska hantera begäranden.</span><span class="sxs-lookup"><span data-stu-id="31b20-145">You'll need toocreate a web.config file toospecify how hello server should handle requests.</span></span>

<span data-ttu-id="31b20-146">Observera att om du har en web.x.y.config-fil i databasen, där x.y matchar hello valda Python-körning och sedan Azure kommer automatiskt att kopiera hello filen Web.config.</span><span class="sxs-lookup"><span data-stu-id="31b20-146">Note that if you have a web.x.y.config file in your repository, where x.y matches hello selected Python runtime, then Azure will automatically copy hello appropriate file as web.config.</span></span>

<span data-ttu-id="31b20-147">hello web.config i exemplen förlitar sig på en virtuell miljö proxyskript som beskrivs i nästa avsnitt om hello.</span><span class="sxs-lookup"><span data-stu-id="31b20-147">hello following web.config examples rely on a virtual environment proxy script, which is described in hello next section.</span></span>  <span data-ttu-id="31b20-148">De fungerar med hello WSGI hanteraren i hello exempel används `app.py` ovan.</span><span class="sxs-lookup"><span data-stu-id="31b20-148">They work with hello WSGI handler used in hello example `app.py` above.</span></span>

<span data-ttu-id="31b20-149">Exempel `web.config` för Python 2.7:</span><span class="sxs-lookup"><span data-stu-id="31b20-149">Example `web.config` for Python 2.7:</span></span>

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\activate_this.py" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_virtualenv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python27\python.exe|D:\Python27\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite"
                      url="handler.fcgi/{R:1}"
                      appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


<span data-ttu-id="31b20-150">Exempel `web.config` för Python 3.4:</span><span class="sxs-lookup"><span data-stu-id="31b20-150">Example `web.config` for Python 3.4:</span></span>

    <?xml version="1.0"?>
    <configuration>
      <appSettings>
        <add key="WSGI_ALT_VIRTUALENV_HANDLER" value="app.wsgi_app" />
        <add key="WSGI_ALT_VIRTUALENV_ACTIVATE_THIS"
             value="D:\home\site\wwwroot\env\Scripts\python.exe" />
        <add key="WSGI_HANDLER"
             value="ptvs_virtualenv_proxy.get_venv_handler()" />
        <add key="PYTHONPATH" value="D:\home\site\wwwroot" />
      </appSettings>
      <system.web>
        <compilation debug="true" targetFramework="4.0" />
      </system.web>
      <system.webServer>
        <modules runAllManagedModulesForAllRequests="true" />
        <handlers>
          <remove name="Python27_via_FastCGI" />
          <remove name="Python34_via_FastCGI" />
          <add name="Python FastCGI"
               path="handler.fcgi"
               verb="*"
               modules="FastCgiModule"
               scriptProcessor="D:\Python34\python.exe|D:\Python34\Scripts\wfastcgi.py"
               resourceType="Unspecified"
               requireAccess="Script" />
        </handlers>
        <rewrite>
          <rules>
            <rule name="Static Files" stopProcessing="true">
              <conditions>
                <add input="true" pattern="false" />
              </conditions>
            </rule>
            <rule name="Configure Python" stopProcessing="true">
              <match url="(.*)" ignoreCase="false" />
              <conditions>
                <add input="{REQUEST_URI}" pattern="^/static/.*" ignoreCase="true" negate="true" />
              </conditions>
              <action type="Rewrite" url="handler.fcgi/{R:1}" appendQueryString="true" />
            </rule>
          </rules>
        </rewrite>
      </system.webServer>
    </configuration>


<span data-ttu-id="31b20-151">Statiska filer ska hanteras av webbservern hello direkt, utan att gå via Python-kod för bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="31b20-151">Static files will be handled by hello web server directly, without going through Python code, for improved performance.</span></span>

<span data-ttu-id="31b20-152">Hello platsen för hello statiska filer på disk ska matcha hello plats i hello URL i hello exemplen ovan.</span><span class="sxs-lookup"><span data-stu-id="31b20-152">In hello above examples, hello location of hello static files on disk should match hello location in hello URL.</span></span> <span data-ttu-id="31b20-153">Detta innebär att en begäran om `http://pythonapp.azurewebsites.net/static/site.css` fungerar hello-filen på disken på `\static\site.css`.</span><span class="sxs-lookup"><span data-stu-id="31b20-153">This means that a request for `http://pythonapp.azurewebsites.net/static/site.css` will serve hello file on disk at `\static\site.css`.</span></span>

<span data-ttu-id="31b20-154">`WSGI_ALT_VIRTUALENV_HANDLER`är där du kan ange hello WSGI hanterare.</span><span class="sxs-lookup"><span data-stu-id="31b20-154">`WSGI_ALT_VIRTUALENV_HANDLER` is where you specify hello WSGI handler.</span></span> <span data-ttu-id="31b20-155">I hello exemplen, ovan har `app.wsgi_app` eftersom hello hanterare är en funktion som heter `wsgi_app` i `app.py` i hello rotmapp.</span><span class="sxs-lookup"><span data-stu-id="31b20-155">In hello above examples, it's `app.wsgi_app` because hello handler is a function named `wsgi_app` in `app.py` in hello root folder.</span></span>

<span data-ttu-id="31b20-156">`PYTHONPATH`kan anpassas, men om du installerar alla beroenden i hello virtuell miljö genom att ange dem i requirements.txt du bör inte toochange den.</span><span class="sxs-lookup"><span data-stu-id="31b20-156">`PYTHONPATH` can be customized, but if you install all your dependencies in hello virtual environment by specifying them in requirements.txt, you shouldn't need toochange it.</span></span>

## <a name="virtual-environment-proxy"></a><span data-ttu-id="31b20-157">Virtuell miljö Proxy</span><span class="sxs-lookup"><span data-stu-id="31b20-157">Virtual Environment Proxy</span></span>
<span data-ttu-id="31b20-158">hello följande skript används tooretrieve hello WSGI hanterare, aktivera hello virtuella miljö och logga fel.</span><span class="sxs-lookup"><span data-stu-id="31b20-158">hello following script is used tooretrieve hello WSGI handler, activate hello virtual environment and log errors.</span></span> <span data-ttu-id="31b20-159">Det är utformad toobe generisk och används utan ändringar.</span><span class="sxs-lookup"><span data-stu-id="31b20-159">It is designed toobe generic and used without modifications.</span></span>

<span data-ttu-id="31b20-160">Innehållet i `ptvs_virtualenv_proxy.py`:</span><span class="sxs-lookup"><span data-stu-id="31b20-160">Contents of `ptvs_virtualenv_proxy.py`:</span></span>

     # ############################################################################
     #
     # Copyright (c) Microsoft Corporation. 
     #
     # This source code is subject tooterms and conditions of hello Apache License, Version 2.0. A 
     # copy of hello license can be found in hello License.html file at hello root of this distribution. If 
     # you cannot locate hello Apache License, Version 2.0, please send an email too
     # vspython@microsoft.com. By using this source code in any fashion, you are agreeing toobe bound 
     # by hello terms of hello Apache License, Version 2.0.
     #
     # You must not remove this notice, or any other, from this software.
     #
     # ###########################################################################

    import datetime
    import os
    import sys
    import traceback

    if sys.version_info[0] == 3:
        def to_str(value):
            return value.decode(sys.getfilesystemencoding())

        def execfile(path, global_dict):
            """Execute a file"""
            with open(path, 'r') as f:
                code = f.read()
            code = code.replace('\r\n', '\n') + '\n'
            exec(code, global_dict)
    else:
        def to_str(value):
            return value.encode(sys.getfilesystemencoding())

    def log(txt):
        """Logs fatal errors tooa log file if WSGI_LOG env var is defined"""
        log_file = os.environ.get('WSGI_LOG')
        if log_file:
            f = open(log_file, 'a+')
            try:
                f.write('%s: %s' % (datetime.datetime.now(), txt))
            finally:
                f.close()

    ptvsd_secret = os.getenv('WSGI_PTVSD_SECRET')
    if ptvsd_secret:
        log('Enabling ptvsd ...\n')
        try:
            import ptvsd
            try:
                ptvsd.enable_attach(ptvsd_secret)
                log('ptvsd enabled.\n')
            except: 
                log('ptvsd.enable_attach failed\n')
        except ImportError:
            log('error importing ptvsd.\n')

    def get_wsgi_handler(handler_name):
        if not handler_name:
            raise Exception('WSGI_ALT_VIRTUALENV_HANDLER env var must be set')

        if not isinstance(handler_name, str):
            handler_name = to_str(handler_name)

        module_name, _, callable_name = handler_name.rpartition('.')
        should_call = callable_name.endswith('()')
        callable_name = callable_name[:-2] if should_call else callable_name
        name_list = [(callable_name, should_call)]
        handler = None
        last_tb = ''

        while module_name:
            try:
                handler = __import__(module_name, fromlist=[name_list[0][0]])
                last_tb = ''
                for name, should_call in name_list:
                    handler = getattr(handler, name)
                    if should_call:
                        handler = handler()
                break
            except ImportError:
                module_name, _, callable_name = module_name.rpartition('.')
                should_call = callable_name.endswith('()')
                callable_name = callable_name[:-2] if should_call else callable_name
                name_list.insert(0, (callable_name, should_call))
                handler = None
                last_tb = ': ' + traceback.format_exc()

        if handler is None:
            raise ValueError('"%s" could not be imported%s' % (handler_name, last_tb))

        return handler

    activate_this = os.getenv('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS')
    if not activate_this:
        raise Exception('WSGI_ALT_VIRTUALENV_ACTIVATE_THIS is not set')

    def get_virtualenv_handler():
        log('Activating virtualenv with %s\n' % activate_this)
        execfile(activate_this, dict(__file__=activate_this))

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler

    def get_venv_handler():
        log('Activating venv with executable at %s\n' % activate_this)
        import site
        sys.executable = activate_this
        old_sys_path, sys.path = sys.path, []

        site.main()

        sys.path.insert(0, '')
        for item in old_sys_path:
            if item not in sys.path:
                sys.path.append(item)

        log('Getting handler %s\n' % os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        handler = get_wsgi_handler(os.getenv('WSGI_ALT_VIRTUALENV_HANDLER'))
        log('Got handler: %r\n' % handler)
        return handler


## <a name="customize-git-deployment"></a><span data-ttu-id="31b20-161">Anpassa Git-distribution</span><span class="sxs-lookup"><span data-stu-id="31b20-161">Customize Git deployment</span></span>
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]

## <a name="troubleshooting---package-installation"></a><span data-ttu-id="31b20-162">Felsökning – installation av paket</span><span class="sxs-lookup"><span data-stu-id="31b20-162">Troubleshooting - Package Installation</span></span>
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a><span data-ttu-id="31b20-163">Felsökning – virtuell miljö</span><span class="sxs-lookup"><span data-stu-id="31b20-163">Troubleshooting - Virtual Environment</span></span>
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a><span data-ttu-id="31b20-164">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="31b20-164">Next steps</span></span>
<span data-ttu-id="31b20-165">Mer information finns i hello [Python Developer Center](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="31b20-165">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

> [!NOTE]
> <span data-ttu-id="31b20-166">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="31b20-166">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="31b20-167">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="31b20-167">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="31b20-168">Nyheter</span><span class="sxs-lookup"><span data-stu-id="31b20-168">What's changed</span></span>
* <span data-ttu-id="31b20-169">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="31b20-169">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

