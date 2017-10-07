---
title: aaaAzure distribution av virtuella datorer med Chef | Microsoft Docs
description: "Lär dig hur toouse Chef toodo automatisk distribution av virtuella datorer och konfiguration på Microsoft Azure"
services: virtual-machines-windows
documentationcenter: 
author: diegoviso
manager: timlt
tags: azure-service-management,azure-resource-manager
editor: 
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: diviso
ms.openlocfilehash: c5ea98c673b2ee75dd4cedf27e50330af05230d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Automatisera distribution av virtuella Azure-datorer med Chef
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

Chef är ett bra verktyg för att leverera automation och önskade tillstånd konfigurationer.

Med vår senaste moln-api-versionen Chef ger sömlös integrering med Azure, så att du hello möjlighet tooprovision och distribuera konfigureringstillstånd via ett enda kommando.

I den här artikeln jag lära dig hur tooset upp din Chef miljö tooprovision Azure virtuella datorer och beskriver hur du skapar en princip eller ”CookBook” och sedan distribuera den här cookbook tooan virtuella Azure-datorn.

Vi börjar!

## <a name="chef-basics"></a>Chef grunderna
Innan du börjar rekommenderar jag du granska hello grundläggande begrepp för Chef. Det är bra material <a href="http://www.chef.io/chef" target="_blank">här</a> och jag rekommenderar att du har en snabb läsning innan du utför den här genomgången. Jag kommer dock Sammanfattningsvis hello grunderna innan vi börjar.

hello följande diagram visar hello övergripande Chef arkitektur.

![][2]

Chef har tre huvudkomponenter arkitektur: Chef servern, Chef klienten (nod) och Chef-arbetsstation.

hello Chef Server är vår hanteringsplatsen och det finns två alternativ för hello Chef Server: en värdbaserad lösning eller en lokal lösning. Vi kommer att använda en värdbaserad lösning.

hello Chef klienten är (nod) hello-agent som placeras på hello-servrar som du hanterar.

hello Chef arbetsstation är vår arbetsstation där vi skapar våra principer och köra vår management-kommandon. Vi kör hello **kniv** kommandot från hello Chef arbetsstation toomanage vår infrastruktur.

Det finns också hello begreppet ”Cookbooks” och ”recept”. Dessa är effektivt hello principer som vi definiera och tillämpa tooour servrar.

## <a name="preparing-hello-workstation"></a>Förbereda hello arbetsstation
Först gör prep hello arbetsstation. Jag använder en standard Windows-arbetsstation. Vi behöver toocreate en directory toostore våra konfigurationsfiler och cookbooks.

Först skapa en katalog med namnet C:\chef.

Skapa sedan den andra katalogen c:\chef\cookbooks.

Vi behöver nu toodownload vårt Azure inställningsfilen så Chef kan kommunicera med våra Azure-prenumeration.

<!--Download your publish settings from [here](https://manage.windowsazure.com/publishsettings/).-->
Hämtar dina publiceringsinställningar med hello PowerShell Azure [Get-AzurePublishSettingsFile](https://docs.microsoft.com/en-us/powershell/module/azure/get-azurepublishsettingsfile?view=azuresmps-4.0.0) kommando. 

Spara hello inställningsfilen för publicering i C:\chef.

## <a name="creating-a-managed-chef-account"></a>Skapa ett hanterat konto Chef
Registrera dig för en värdbaserad Chef konto [här](https://manage.chef.io/signup).

Under hello registreringsprocessen du kommer att tillfrågas toocreate en ny organisation.

![][3]

När din organisation har skapats kan du hämta hello startpaket.

![][4]

> [!NOTE]
> Om du får ett meddelande som varnar dig om att dina nycklar ska återställas är ok tooproceed som vi har inga befintliga infrastruktur konfigurerad ännu.
> 
> 

Den här starter kit zip-filen innehåller din organisation konfigurationsfiler och nycklar.

## <a name="configuring-hello-chef-workstation"></a>Konfigurera hello Chef arbetsstation
Extrahera hello innehållet i hello chef starter.zip tooC:\chef.

Kopiera alla filer under chef-starter\chef-lagringsplatsen\.chef tooyour c:\chef directory.

Din katalog bör nu se ut ungefär som följande exempel hello.

![][5]

Du bör nu ha fyra filer, inklusive hello Azure publishing filen i c:\chef hello rot.

hello PEM-filer innehåller din organisation och admin privata nycklar för kommunikation medan hello knife.rb filen innehåller kniv konfigurationen. Vi behöver tooedit hello knife.rb fil.

Öppna hello-filen i redigeringsprogrammet väljer och ändra hello ”cookbook_path” genom att ta bort hello /... / från hello sökvägen så visas det på Nästa.

    cookbook_path  ["#{current_dir}/cookbooks"]

Också lägga till hello följande rad reflekterande hello namnet på ditt Azure inställningsfilen för publicering.

    knife[:azure_publish_settings_file] = "yourfilename.publishsettings"

Filen knife.rb bör nu se ut ungefär toohello följande exempel.

![][6]

Dessa rader säkerställer att kniv refererar till hello cookbooks katalog under c:\chef\cookbooks och använder också vår Azure-publiceringsinställningsfil under åtgärder i Azure.

## <a name="installing-hello-chef-development-kit"></a>Installera hello Chef Development Kit
Nästa [ladda ned och installera](http://downloads.getchef.com/chef-dk/windows) hello ChefDK (Chef Development Kit) tooset upp arbetsstationen Chef.

![][7]

Installera i hello standardplatsen för c:\opscode. Den här installationen tar cirka 10 minuter.

Bekräfta din sökvägsvariabeln innehåller poster för C:\opscode\chefdk\bin; C:\opscode\chefdk\embedded\bin;c:\users\yourusername\.chefdk\gem\ruby\2.0.0\bin

Om de inte är det, se till att du lägger till dessa sökvägar!

*Obs hello ordning av hello sökväg är viktigt!* Om din opscode sökvägar inte kan i rätt ordning för hello har du problem.

Starta om arbetsstationen innan du fortsätter.

Därefter installerar vi hello kniv Azure-tillägget. Detta ger kniv hello ”Azure plugin-program”.

Kör följande kommando hello.

    chef gem install knife-azure ––pre

> [!NOTE]
> hello – pre argumentet säkerställer du tar emot hello senaste RC-versionen av hello kniv Azure plugin-program som ger åtkomst toohello senaste uppsättning API: er.
> 
> 

Det är troligt att installeras även ett antal beroenden på hello samtidigt.

![][8]

tooensure allt är konfigurerade korrekt, kör hello följande kommando.

    knife azure image list

Om allt är korrekt konfigurerad, visas en lista över tillgängliga Azure avbildningar rulla.

Grattis! hello arbetsstation ställs in!

## <a name="creating-a-cookbook"></a>Skapa en Cookbook
En Cookbook används av Chef toodefine en uppsättning kommandon som du vill tooexecute på en hanterad klient. Det är enkelt att skapa en Cookbook och vi använder hello **chef generera cookbook** kommandot toogenerate våra Cookbook mallen. Jag kommer att ringa upp webbservern Cookbook som jag vill att en princip som automatiskt distribuerar IIS.

Kör följande kommando hello under katalogen C:\Chef.

    chef generate cookbook webserver

Detta genererar en uppsättning filer under hello katalog C:\Chef\cookbooks\webserver. Vi behöver nu toodefine hello uppsättning kommandon som vi vill gärna vår Chef klienten tooexecute på vår hanterade virtuella datorn.

hello kommandon lagras i hello filen default.rb. I den här filen ska jag definierar en uppsättning kommandon som installerar IIS, startar IIS och kopierar en mall toohello wwwroot mapp.

Ändra hello C:\chef\cookbooks\webserver\recipes\default.rb filen och Lägg till följande rader hello.

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

Spara hello-filen när du är klar.

## <a name="creating-a-template"></a>Skapa en mall
Som vi nämnt tidigare behöver vi toogenerate en mallfil som ska användas som sidan med våra default.html.

Hello kör följande kommando toogenerate hello mallen.

    chef generate template webserver Default.htm

Gå nu toohello C:\chef\cookbooks\webserver\templates\default\Default.htm.erb fil. Redigera hello-filen genom att lägga till vissa enkel ”Hello World” HTML-kod och spara hello-filen.

## <a name="upload-hello-cookbook-toohello-chef-server"></a>Överför hello Cookbook toohello Chef Server
I det här steget är vi tar en kopia av hello Cookbook som vi har skapat på våra lokala dator och överföra den toohello Chef Hosted Server. När du har överfört hello Cookbook visas under hello **princip** fliken.

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Distribuera en virtuell dator med kniv Azure
Vi kommer nu distribuera en virtuell Azure-dator och tillämpa hello ”webbserver” Cookbook som installerar våra IIS web service och standard webbsida.

I ordning toodo detta använder hello **kniv azure-servern skapa** kommando.

Är exempel på hello kommandot visas.

    knife azure server create --azure-dns-name 'diegotest01' --azure-vm-name 'testserver01' --azure-vm-size 'Small' --azure-storage-account 'portalvhdsxxxx' --bootstrap-protocol 'cloud-api' --azure-source-image 'a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-Datacenter-201411.01-en.us-127GB.vhd' --azure-service-location 'Southeast Asia' --winrm-user azureuser --winrm-password 'myPassword123' --tcp-endpoints 80,3389 --r 'recipe[webserver]'

hello parametrar är självförklarande. Ersätta viss variabler och kör.

> [!NOTE]
> Via hello hello kommandoraden jag också automatisera min filterregler för slutpunkt-nätverk med hjälp av hello – tcp-slutpunkter parametern. Jag har öppnat portarna 80 och 3389 tooprovide åtkomst toomy webbsida och RDP-session.
> 
> 

När du kör kommandot hello gå toohello Azure portal och du kommer att se din dator börjar tooprovision.

![][13]

hello kommandotolken visas nästa.

![][10]

När hello distributionen är klar kan vara vi kan tooconnect toohello webbtjänsten via port 80 som vi har öppnat hello port när vi etableras hello virtuell dator med hello kniv Azure-kommando. Eftersom den här virtuella datorn är hello virtuell dator i min Molntjänsten, kommer den ansluts med hello molnet tjänst-url.

![][11]

Som du ser fick jag kreativa med HTML-kod.

Vi kan också ansluta via en RDP-session från hello Azure-portalen via port 3389 Glöm inte.

Hoppas det här har bra! Gå och starta din infrastruktur som koden resa med Azure idag!

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png


<!--Link references-->
