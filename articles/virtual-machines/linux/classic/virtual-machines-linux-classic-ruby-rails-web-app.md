---
title: "aaaHost en Ruby på spår webbplats på en Linux-VM | Microsoft Docs"
description: "Ställ in och vara värd för en Ruby på spår-baserade webbplats på Azure med hjälp av en virtuell Linux-dator."
services: virtual-machines-linux
documentationcenter: ruby
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: aad32685-3550-4bff-9c73-beb8d70b3291
ms.service: virtual-machines-linux
ms.workload: web
ms.tgt_pltfrm: vm-linux
ms.devlang: ruby
ms.topic: article
ms.date: 06/27/2017
ms.author: robmcm
ms.openlocfilehash: c545c24fc6c89497854bbe55a6d0d1d0b072c386
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a>Ruby on Rails-webbprogram på en virtuell Azure-dator
Den här kursen visar hur toohost en Ruby på spår webbplats på Azure med hjälp av en virtuell Linux-dator.  

Den här självstudiekursen har verifierats med hjälp av Ubuntu Server 14.04 LTS. Om du använder en annan Linux distributionsplats kan du behöva toomodify hello steg tooinstall spår.

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).  Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.
>
>

## <a name="create-an-azure-vm"></a>Skapa en virtuell dator i Azure
Börja med att skapa en virtuell dator i Azure med en Linux-avbildning.

toocreate Hej VM, du kan använda hello Azure-portalen eller hello Azure-kommandoradsgränssnittet (CLI).

### <a name="azure-portal"></a>Azure Portal
1. Logga in på hello [Azure-portalen](https://portal.azure.com)
2. Klicka på **ny**, Skriv ”Ubuntu Server 14.04” i sökrutan för hello. Klicka på hello-post som returneras av hello sökning. Hello distributionsmodell Välj **klassiska**, klicka på ”Skapa”.
3. Ange värden för hello obligatoriskt fält i hello grunderna i bladet: namnet (för hello VM), användarnamn, autentiseringstyp och hello motsvarande autentiseringsuppgifter, Azure-prenumeration, resursgrupp och plats.

   ![Skapa en ny Ubuntu-avbildning](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. När hello VM etableras, klicka på hello VM namn och klickar på **slutpunkter** i hello **inställningar** kategori. Hitta hello SSH slutpunkt, som visas under **fristående**.

   ![Standardslutpunkten](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a>Azure CLI
Gör så hello i [skapa en virtuell dator kör Linux][vm-instructions].

Efter hello VM har etablerats kan du hello SSH-slutpunkten genom att köra följande kommando hello:

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a>Installera Ruby spår
1. Använda SSH tooconnect toohello VM.
2. Använd följande kommandon tooinstall Ruby på hello VM hello från hello SSH-sessionen:

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > hello brightbox repository contains hello current Ruby distribution.

    hello-installationen kan ta några minuter. När den är klar, kommando Använd hello följande tooverify att Ruby är installerad:

        ruby -v

3. Använd hello följande kommando tooinstall spår:

        sudo gem install rails --no-rdoc --no-ri -V

    Använd hello--Nej rdoc och--Nej ri flaggor tooskip installerar hello dokumentation, vilket är snabbare.
    Det här kommandot kommer förmodligen ta lång tid tooexecute, så att lägga till hello -V visas information om hello installationens förlopp.

## <a name="create-and-run-an-app"></a>Skapa och köra en app
När du är inloggad fortfarande via SSH, kör du hello följande kommandon:

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

Hej [nya](http://guides.rubyonrails.org/command_line.html#rails-new) kommando skapar en ny app spår. Hej [server](http://guides.rubyonrails.org/command_line.html#rails-server) kommando startar hello WEBrick webbserver som medföljer spår. (För produktion, du kanske vill toouse en annan server, till exempel Unicorn eller passagerare.)

Du bör se utdata liknande toohello följande.

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C tooshutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a>Lägga till en slutpunkt
1. Gå toohello [Azure-portalen] [https://portal.azure.com] och välj den virtuella datorn.

2. Välj **SLUTPUNKTER** i hello **inställningar** längs hello vänsterkant hello sidan.

3. Klicka på **lägga till** hello överst på hello sidan.

4. I hello **lägga till slutpunkten** dialogrutan Ange hello följande information:

   * **Namnet**: HTTP
   * **Protokollet**: TCP
   * **Offentlig port**: 80
   * **Privat port**: 3000
   * **Flytande PI adress**: inaktiverad
   * **ACL - ordning**: 1001 eller ett annat värde som anger hello prioritet för den här regeln.
   * **ACL - namnet**: allowHTTP
   * **ACL - åtgärd**: Tillåt
   * **ACL - fjärrundernät**: 1.0.0.0/16

     Den här slutpunkten har en offentlig port 80 som dirigerar trafik toohello privat port 3000, där hello spår servern lyssnar. regeln för åtkomstkontrollista hello tillåter offentliga trafik på port 80.

     ![ny slutpunkt](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. Klicka på OK toosave hello slutpunkt.

6. Ett meddelande visas som anger **sparar den virtuella datorslutpunkten**. När det här meddelandet försvinner är hello slutpunkten aktiv. Du kan nu testa programmet genom att gå toohello DNS-namnet på den virtuella datorn. hello webbplats ska se ut ungefär toohello följande:

    ![spår standardsida][default-rails-cloud]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen har de flesta av hello steg manuellt. I en produktionsmiljö skulle du skriva din app på en utvecklingsdator och distribuerar den toohello Azure VM. De flesta produktionsmiljöer värd också hello spår program tillsammans med en annan server-processen, till exempel Apache eller NginX, som hanterar begäran routning toomultiple instanser av hello spår program och betjänar statiska resurser. Mer information finns i http://rubyonrails.org/deploy/.

toolearn mer om Ruby spår, besök hello [Ruby på spår guider][rails-guides].

toouse Azure-tjänster från tillämpningsprogrammet Ruby, se:

* [Lagra Ostrukturerade data med blobbar][blobs]
* [Lagra nyckel/värde-par med tabeller][tables]
* [Hantera hög bandbredd innehåll med hello innehåll Delivery Network][cdn-howto]

<!-- WA.com links -->
[blobs]:../../../storage/blobs/storage-ruby-how-to-use-blob-storage.md
[cdn-howto]:https://azure.microsoft.com/develop/ruby/app-services/
[tables]:../../../cosmos-db/table-storage-how-to-use-ruby.md
[vm-instructions]:createportal.md

<!-- External Links -->
[rails-guides]:http://guides.rubyonrails.org/
[sqlite3]:http://www.sqlite.org/

<!-- Images -->

[default-rails-cloud]:./media/virtual-machines-linux-classic-ruby-rails-web-app/basicrailscloud.png
[vmlist]:./media/virtual-machines-linux-classic-ruby-rails-web-app/vmlist.png
[endpoints]:./media/virtual-machines-linux-classic-ruby-rails-web-app/endpoints.png
[new-endpoint]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint.png
[new-endpoint1]:./media/virtual-machines-linux-classic-ruby-rails-web-app/newendpoint1.png
