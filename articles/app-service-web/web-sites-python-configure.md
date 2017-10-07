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
# <a name="configuring-python-with-azure-app-service-web-apps"></a>Konfigurera Python med Azure App Service Web Apps
Den här självstudiekursen beskrivs alternativ för redigering och konfigurera en grundläggande Web Server Gateway Interface (WSGI) kompatibla Python-program på [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714).

Beskriver ytterligare funktioner för Git-distribution, till exempel virtuell miljö och installationen med hjälp av requirements.txt.

## <a name="bottle-django-or-flask"></a>Bottle, Django eller Flask?
hello Azure Marketplace innehåller mallar för hello Bottle, Django och Flask ramverk. Om du utvecklar din första webbapp i Azure App Service eller om du inte är bekant med Git, rekommenderar vi att du följer någon av dessa självstudier med stegvisa instruktioner för att skapa ett fungerande program från hello galleriet med Git-distribution från Windows- eller Mac:

* [Skapa webbappar med Bottle](web-sites-python-create-deploy-bottle-app.md)
* [Skapa webbappar med Django](web-sites-python-create-deploy-django-app.md)
* [Skapa webbappar med Flask](web-sites-python-create-deploy-flask-app.md)

## <a name="web-app-creation-on-azure-portal"></a>Skapa en webbapp i Azure Portal
Den här kursen förutsätter att en befintlig Azure-prenumeration och åtkomst toohello Azure-portalen.

Om du inte har en befintlig webbapp, kan du skapa en från hello [Azure Portal](https://portal.azure.com).  Klicka hello nya i hello övre vänstra hörnet och klicka sedan på **webb + mobilt** > **webbapp**.

## <a name="git-publishing"></a>Git-publicering
Konfigurera Git-publicering för nyskapade webbappen genom att följa anvisningarna hello på [lokal Git-distribution tooAzure Apptjänst](app-service-deploy-local-git.md). Den här kursen använder Git toocreate, hantera och publicera våra Python web app tooAzure Apptjänst.

När Git-publicering har ställts in, skapas en Git-lagringsplats och som är kopplad till ditt webbprogram. hello databasen URL visas och kan hädanefter används toopush data från hello lokal utveckling miljö toohello molnet. toopublish program via Git, kontrollera att en Git-klient installeras även och Använd hello instruktioner som toopush din web app innehåll tooAzure Apptjänst.

## <a name="application-overview"></a>Programöversikt
I nästa avsnitt hello skapas hello följande filer. De ska placeras i hello rot hello Git-lagringsplats.

    app.py
    requirements.txt
    runtime.txt
    web.config
    ptvs_virtualenv_proxy.py


## <a name="wsgi-handler"></a>WSGI hanterare
WSGI är en Python-standard som beskrivs av [program 3333](http://www.python.org/dev/peps/pep-3333/) definierar ett gränssnitt mellan hello webbserver och Python. Det ger en standardiserad gränssnitt för att skriva olika webbaserade program och ramverk som använder Python. Populära Python web ramverk använder dag WSGI. Azure App Service Web Apps får du stöd för ramverk; Dessutom kan avancerade användare även skapa egna så länge hello anpassad hanterare följer riktlinjer för hello WSGI-specifikationen.

Här är ett exempel på en `app.py` som definierar en anpassad hanterare:

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

Du kan köra programmet lokalt med `python app.py`, bläddrar sedan för`http://localhost:5555` i webbläsaren.

## <a name="virtual-environment"></a>Virtuell miljö
Även om hello exempelapp ovan inte kräver några externa paket, är det troligt att programmet kommer att kräva några.

toohelp hantera externa paketberoenden, Azure Git-distribution stöder hello skapandet av virtuella miljöer.

När Azure identifierar en requirements.txt i hello rot hello-databasen, skapas automatiskt en virtuell miljö som heter `env`. Det här inträffar bara på hello första distributionen eller under en distribution när hello valda Python-körning har ändrats.

Du vill förmodligen toocreate en virtuell miljö lokalt för utveckling, men inkludera inte den i Git-lagringsplatsen.

## <a name="package-management"></a>Pakethantering
Paket som anges i requirements.txt installeras automatiskt i hello virtuell miljö med hjälp av pip. Detta sker vid varje distribution, men pip hoppar över installationen om ett paket har redan installerats.

Exempel `requirements.txt`:

    azure==0.8.4


## <a name="python-version"></a>Python-versionen
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-runtime.md)]

Exempel `runtime.txt`:

    python-2.7


## <a name="webconfig"></a>Web.config
Du behöver toocreate en web.config-filen toospecify hur hello server ska hantera begäranden.

Observera att om du har en web.x.y.config-fil i databasen, där x.y matchar hello valda Python-körning och sedan Azure kommer automatiskt att kopiera hello filen Web.config.

hello web.config i exemplen förlitar sig på en virtuell miljö proxyskript som beskrivs i nästa avsnitt om hello.  De fungerar med hello WSGI hanteraren i hello exempel används `app.py` ovan.

Exempel `web.config` för Python 2.7:

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


Exempel `web.config` för Python 3.4:

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


Statiska filer ska hanteras av webbservern hello direkt, utan att gå via Python-kod för bättre prestanda.

Hello platsen för hello statiska filer på disk ska matcha hello plats i hello URL i hello exemplen ovan. Detta innebär att en begäran om `http://pythonapp.azurewebsites.net/static/site.css` fungerar hello-filen på disken på `\static\site.css`.

`WSGI_ALT_VIRTUALENV_HANDLER`är där du kan ange hello WSGI hanterare. I hello exemplen, ovan har `app.wsgi_app` eftersom hello hanterare är en funktion som heter `wsgi_app` i `app.py` i hello rotmapp.

`PYTHONPATH`kan anpassas, men om du installerar alla beroenden i hello virtuell miljö genom att ange dem i requirements.txt du bör inte toochange den.

## <a name="virtual-environment-proxy"></a>Virtuell miljö Proxy
hello följande skript används tooretrieve hello WSGI hanterare, aktivera hello virtuella miljö och logga fel. Det är utformad toobe generisk och används utan ändringar.

Innehållet i `ptvs_virtualenv_proxy.py`:

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


## <a name="customize-git-deployment"></a>Anpassa Git-distribution
[!INCLUDE [web-sites-python-customizing-runtime](../../includes/web-sites-python-customizing-deployment.md)]

## <a name="troubleshooting---package-installation"></a>Felsökning – installation av paket
[!INCLUDE [web-sites-python-troubleshooting-package-installation](../../includes/web-sites-python-troubleshooting-package-installation.md)]

## <a name="troubleshooting---virtual-environment"></a>Felsökning – virtuell miljö
[!INCLUDE [web-sites-python-troubleshooting-virtual-environment](../../includes/web-sites-python-troubleshooting-virtual-environment.md)]

## <a name="next-steps"></a>Nästa steg
Mer information finns i hello [Python Developer Center](/develop/python/).

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

