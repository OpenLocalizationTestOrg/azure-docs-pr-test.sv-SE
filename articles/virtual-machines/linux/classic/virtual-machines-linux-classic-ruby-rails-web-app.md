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
# <a name="ruby-on-rails-web-application-on-an-azure-vm"></a><span data-ttu-id="5bf6b-103">Ruby on Rails-webbprogram på en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="5bf6b-103">Ruby on Rails Web application on an Azure VM</span></span>
<span data-ttu-id="5bf6b-104">Den här kursen visar hur toohost en Ruby på spår webbplats på Azure med hjälp av en virtuell Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-104">This tutorial shows how toohost a Ruby on Rails website on Azure using a Linux virtual machine.</span></span>  

<span data-ttu-id="5bf6b-105">Den här självstudiekursen har verifierats med hjälp av Ubuntu Server 14.04 LTS.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-105">This tutorial was validated using Ubuntu Server 14.04 LTS.</span></span> <span data-ttu-id="5bf6b-106">Om du använder en annan Linux distributionsplats kan du behöva toomodify hello steg tooinstall spår.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-106">If you use a different Linux distribution, you might need toomodify hello steps tooinstall Rails.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5bf6b-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="5bf6b-107">Azure has two different deployment models for creating and working with resources:  [Resource Manager and classic](../../../azure-resource-manager/resource-manager-deployment-model.md).</span></span>  <span data-ttu-id="5bf6b-108">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-108">This article covers using hello classic deployment model.</span></span> <span data-ttu-id="5bf6b-109">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>
>
>

## <a name="create-an-azure-vm"></a><span data-ttu-id="5bf6b-110">Skapa en virtuell dator i Azure</span><span class="sxs-lookup"><span data-stu-id="5bf6b-110">Create an Azure VM</span></span>
<span data-ttu-id="5bf6b-111">Börja med att skapa en virtuell dator i Azure med en Linux-avbildning.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-111">Start by creating an Azure VM with a Linux image.</span></span>

<span data-ttu-id="5bf6b-112">toocreate Hej VM, du kan använda hello Azure-portalen eller hello Azure-kommandoradsgränssnittet (CLI).</span><span class="sxs-lookup"><span data-stu-id="5bf6b-112">toocreate hello VM, you can use hello Azure portal or hello Azure Command-Line Interface (CLI).</span></span>

### <a name="azure-portal"></a><span data-ttu-id="5bf6b-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5bf6b-113">Azure portal</span></span>
1. <span data-ttu-id="5bf6b-114">Logga in på hello [Azure-portalen](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="5bf6b-114">Sign into hello [Azure portal](https://portal.azure.com)</span></span>
2. <span data-ttu-id="5bf6b-115">Klicka på **ny**, Skriv ”Ubuntu Server 14.04” i sökrutan för hello.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-115">Click **New**, then type "Ubuntu Server 14.04" in hello search box.</span></span> <span data-ttu-id="5bf6b-116">Klicka på hello-post som returneras av hello sökning.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-116">Click hello entry returned by hello search.</span></span> <span data-ttu-id="5bf6b-117">Hello distributionsmodell Välj **klassiska**, klicka på ”Skapa”.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-117">For hello deployment model, select **Classic**, then click "Create".</span></span>
3. <span data-ttu-id="5bf6b-118">Ange värden för hello obligatoriskt fält i hello grunderna i bladet: namnet (för hello VM), användarnamn, autentiseringstyp och hello motsvarande autentiseringsuppgifter, Azure-prenumeration, resursgrupp och plats.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-118">In hello Basics blade, supply values for hello required fields: Name (for hello VM), User name, Authentication type and hello corresponding credentials, Azure subscription, Resource group, and Location.</span></span>

   ![Skapa en ny Ubuntu-avbildning](./media/virtual-machines-linux-classic-ruby-rails-web-app/createvm.png)

4. <span data-ttu-id="5bf6b-120">När hello VM etableras, klicka på hello VM namn och klickar på **slutpunkter** i hello **inställningar** kategori.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-120">After hello VM is provisioned, click on hello VM name, and click **Endpoints** in hello **Settings** category.</span></span> <span data-ttu-id="5bf6b-121">Hitta hello SSH slutpunkt, som visas under **fristående**.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-121">Find hello SSH endpoint, listed under **Standalone**.</span></span>

   ![Standardslutpunkten](./media/virtual-machines-linux-classic-ruby-rails-web-app/endpointsnewportal.png)

### <a name="azure-cli"></a><span data-ttu-id="5bf6b-123">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5bf6b-123">Azure CLI</span></span>
<span data-ttu-id="5bf6b-124">Gör så hello i [skapa en virtuell dator kör Linux][vm-instructions].</span><span class="sxs-lookup"><span data-stu-id="5bf6b-124">Follow hello steps in [Create a Virtual Machine Running Linux][vm-instructions].</span></span>

<span data-ttu-id="5bf6b-125">Efter hello VM har etablerats kan du hello SSH-slutpunkten genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="5bf6b-125">After hello VM is provisioned, you can get hello SSH endpoint by running hello following command:</span></span>

    azure vm endpoint list <vm-name>  

## <a name="install-ruby-on-rails"></a><span data-ttu-id="5bf6b-126">Installera Ruby spår</span><span class="sxs-lookup"><span data-stu-id="5bf6b-126">Install Ruby on Rails</span></span>
1. <span data-ttu-id="5bf6b-127">Använda SSH tooconnect toohello VM.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-127">Use SSH tooconnect toohello VM.</span></span>
2. <span data-ttu-id="5bf6b-128">Använd följande kommandon tooinstall Ruby på hello VM hello från hello SSH-sessionen:</span><span class="sxs-lookup"><span data-stu-id="5bf6b-128">From hello SSH session, use hello following commands tooinstall Ruby on hello VM:</span></span>

        sudo apt-get update -y
        sudo apt-get upgrade -y

        sudo apt-add-repository ppa:brightbox/ruby-ng
        sudo apt-get update
        sudo apt-get install ruby2.4

        > [!TIP]
        > hello brightbox repository contains hello current Ruby distribution.

    <span data-ttu-id="5bf6b-129">hello-installationen kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-129">hello installation may take a few minutes.</span></span> <span data-ttu-id="5bf6b-130">När den är klar, kommando Använd hello följande tooverify att Ruby är installerad:</span><span class="sxs-lookup"><span data-stu-id="5bf6b-130">When it completes, use hello following command tooverify that Ruby is installed:</span></span>

        ruby -v

3. <span data-ttu-id="5bf6b-131">Använd hello följande kommando tooinstall spår:</span><span class="sxs-lookup"><span data-stu-id="5bf6b-131">Use hello following command tooinstall Rails:</span></span>

        sudo gem install rails --no-rdoc --no-ri -V

    <span data-ttu-id="5bf6b-132">Använd hello--Nej rdoc och--Nej ri flaggor tooskip installerar hello dokumentation, vilket är snabbare.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-132">Use hello --no-rdoc and --no-ri flags tooskip installing hello documentation, which is faster.</span></span>
    <span data-ttu-id="5bf6b-133">Det här kommandot kommer förmodligen ta lång tid tooexecute, så att lägga till hello -V visas information om hello installationens förlopp.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-133">This command will likely take a long time tooexecute, so adding hello -V will display information about hello installation progress.</span></span>

## <a name="create-and-run-an-app"></a><span data-ttu-id="5bf6b-134">Skapa och köra en app</span><span class="sxs-lookup"><span data-stu-id="5bf6b-134">Create and run an app</span></span>
<span data-ttu-id="5bf6b-135">När du är inloggad fortfarande via SSH, kör du hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="5bf6b-135">While still logged in via SSH, run hello following commands:</span></span>

    rails new myapp
    cd myapp
    rails server -b 0.0.0.0 -p 3000

<span data-ttu-id="5bf6b-136">Hej [nya](http://guides.rubyonrails.org/command_line.html#rails-new) kommando skapar en ny app spår.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-136">hello [new](http://guides.rubyonrails.org/command_line.html#rails-new) command creates a new Rails app.</span></span> <span data-ttu-id="5bf6b-137">Hej [server](http://guides.rubyonrails.org/command_line.html#rails-server) kommando startar hello WEBrick webbserver som medföljer spår.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-137">hello [server](http://guides.rubyonrails.org/command_line.html#rails-server) command starts hello WEBrick web server that comes with Rails.</span></span> <span data-ttu-id="5bf6b-138">(För produktion, du kanske vill toouse en annan server, till exempel Unicorn eller passagerare.)</span><span class="sxs-lookup"><span data-stu-id="5bf6b-138">(For production use, you would probably want toouse a different server, such as Unicorn or Passenger.)</span></span>

<span data-ttu-id="5bf6b-139">Du bör se utdata liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-139">You should see output similar toohello following.</span></span>

    => Booting WEBrick
    => Rails 4.2.1 application starting in development on http://0.0.0.0:3000
    => Run `rails server -h` for more startup options
    => Ctrl-C tooshutdown server
    [2015-06-09 23:34:23] INFO  WEBrick 1.3.1
    [2015-06-09 23:34:23] INFO  ruby 1.9.3 (2013-11-22) [x86_64-linux]
    [2015-06-09 23:34:23] INFO  WEBrick::HTTPServer#start: pid=27766 port=3000

## <a name="add-an-endpoint"></a><span data-ttu-id="5bf6b-140">Lägga till en slutpunkt</span><span class="sxs-lookup"><span data-stu-id="5bf6b-140">Add an endpoint</span></span>
1. <span data-ttu-id="5bf6b-141">Gå toohello [Azure-portalen] [https://portal.azure.com] och välj den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-141">Go toohello [Azure portal][https://portal.azure.com] and select your VM.</span></span>

2. <span data-ttu-id="5bf6b-142">Välj **SLUTPUNKTER** i hello **inställningar** längs hello vänsterkant hello sidan.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-142">Select **ENDPOINTS** in hello **Settings** along hello left edge hello page.</span></span>

3. <span data-ttu-id="5bf6b-143">Klicka på **lägga till** hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-143">Click **ADD** at hello top of hello page.</span></span>

4. <span data-ttu-id="5bf6b-144">I hello **lägga till slutpunkten** dialogrutan Ange hello följande information:</span><span class="sxs-lookup"><span data-stu-id="5bf6b-144">In hello **Add endpoint** dialog page, enter hello following information:</span></span>

   * <span data-ttu-id="5bf6b-145">**Namnet**: HTTP</span><span class="sxs-lookup"><span data-stu-id="5bf6b-145">**Name**: HTTP</span></span>
   * <span data-ttu-id="5bf6b-146">**Protokollet**: TCP</span><span class="sxs-lookup"><span data-stu-id="5bf6b-146">**Protocol**: TCP</span></span>
   * <span data-ttu-id="5bf6b-147">**Offentlig port**: 80</span><span class="sxs-lookup"><span data-stu-id="5bf6b-147">**Public port**: 80</span></span>
   * <span data-ttu-id="5bf6b-148">**Privat port**: 3000</span><span class="sxs-lookup"><span data-stu-id="5bf6b-148">**Private port**: 3000</span></span>
   * <span data-ttu-id="5bf6b-149">**Flytande PI adress**: inaktiverad</span><span class="sxs-lookup"><span data-stu-id="5bf6b-149">**Floating PI address**: Disabled</span></span>
   * <span data-ttu-id="5bf6b-150">**ACL - ordning**: 1001 eller ett annat värde som anger hello prioritet för den här regeln.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-150">**Access control list - Order**: 1001, or another value that sets hello priority of this access rule.</span></span>
   * <span data-ttu-id="5bf6b-151">**ACL - namnet**: allowHTTP</span><span class="sxs-lookup"><span data-stu-id="5bf6b-151">**Access control list - Name**: allowHTTP</span></span>
   * <span data-ttu-id="5bf6b-152">**ACL - åtgärd**: Tillåt</span><span class="sxs-lookup"><span data-stu-id="5bf6b-152">**Access control list - Action**: permit</span></span>
   * <span data-ttu-id="5bf6b-153">**ACL - fjärrundernät**: 1.0.0.0/16</span><span class="sxs-lookup"><span data-stu-id="5bf6b-153">**Access control list - Remote subnet**: 1.0.0.0/16</span></span>

     <span data-ttu-id="5bf6b-154">Den här slutpunkten har en offentlig port 80 som dirigerar trafik toohello privat port 3000, där hello spår servern lyssnar.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-154">This endpoint  has a public port of 80 that will route traffic toohello private port of 3000, where hello Rails server is listening.</span></span> <span data-ttu-id="5bf6b-155">regeln för åtkomstkontrollista hello tillåter offentliga trafik på port 80.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-155">hello access control list rule allows public traffic on port 80.</span></span>

     ![ny slutpunkt](./media/virtual-machines-linux-classic-ruby-rails-web-app/createendpoint.png)

5. <span data-ttu-id="5bf6b-157">Klicka på OK toosave hello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-157">Click OK toosave hello endpoint.</span></span>

6. <span data-ttu-id="5bf6b-158">Ett meddelande visas som anger **sparar den virtuella datorslutpunkten**.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-158">A message should appear that states **Saving virtual machine endpoint**.</span></span> <span data-ttu-id="5bf6b-159">När det här meddelandet försvinner är hello slutpunkten aktiv.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-159">Once this message disappears, hello endpoint is active.</span></span> <span data-ttu-id="5bf6b-160">Du kan nu testa programmet genom att gå toohello DNS-namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-160">You may now test your application by navigating toohello DNS name of your virtual machine.</span></span> <span data-ttu-id="5bf6b-161">hello webbplats ska se ut ungefär toohello följande:</span><span class="sxs-lookup"><span data-stu-id="5bf6b-161">hello website should appear similar toohello following:</span></span>

    ![spår standardsida][default-rails-cloud]

## <a name="next-steps"></a><span data-ttu-id="5bf6b-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5bf6b-163">Next steps</span></span>
<span data-ttu-id="5bf6b-164">I den här självstudiekursen har de flesta av hello steg manuellt.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-164">In this tutorial, you did most of hello steps manually.</span></span> <span data-ttu-id="5bf6b-165">I en produktionsmiljö skulle du skriva din app på en utvecklingsdator och distribuerar den toohello Azure VM.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-165">In a production environment, you would write your app on a development machine and deploy it toohello Azure VM.</span></span> <span data-ttu-id="5bf6b-166">De flesta produktionsmiljöer värd också hello spår program tillsammans med en annan server-processen, till exempel Apache eller NginX, som hanterar begäran routning toomultiple instanser av hello spår program och betjänar statiska resurser.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-166">Also, most production environments host hello Rails application in conjunction with another server process such as Apache or NginX, which handles request routing toomultiple instances of hello Rails application and serving static resources.</span></span> <span data-ttu-id="5bf6b-167">Mer information finns i http://rubyonrails.org/deploy/.</span><span class="sxs-lookup"><span data-stu-id="5bf6b-167">For more information, see http://rubyonrails.org/deploy/.</span></span>

<span data-ttu-id="5bf6b-168">toolearn mer om Ruby spår, besök hello [Ruby på spår guider][rails-guides].</span><span class="sxs-lookup"><span data-stu-id="5bf6b-168">toolearn more about Ruby on Rails, visit hello [Ruby on Rails Guides][rails-guides].</span></span>

<span data-ttu-id="5bf6b-169">toouse Azure-tjänster från tillämpningsprogrammet Ruby, se:</span><span class="sxs-lookup"><span data-stu-id="5bf6b-169">toouse Azure services from your Ruby application, see:</span></span>

* <span data-ttu-id="5bf6b-170">[Lagra Ostrukturerade data med blobbar][blobs]</span><span class="sxs-lookup"><span data-stu-id="5bf6b-170">[Store unstructured data using blobs][blobs]</span></span>
* <span data-ttu-id="5bf6b-171">[Lagra nyckel/värde-par med tabeller][tables]</span><span class="sxs-lookup"><span data-stu-id="5bf6b-171">[Store key/value pairs using tables][tables]</span></span>
* <span data-ttu-id="5bf6b-172">[Hantera hög bandbredd innehåll med hello innehåll Delivery Network][cdn-howto]</span><span class="sxs-lookup"><span data-stu-id="5bf6b-172">[Serve high bandwidth content with hello Content Delivery Network][cdn-howto]</span></span>

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
