---
title: "aaaCreate en medelvärde stacken på en Linux-VM i Azure | Microsoft Docs"
description: "Lär dig hur toocreate en MongoDB, snabb, AngularJS och Node.js (medel) stacken på en Linux-VM i Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 82a8e34e60d2bb6e6670ee007faa1113ea78b716
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-mongodb-express-angularjs-and-nodejs-mean-stack-on-a-linux-vm-in-azure"></a><span data-ttu-id="c79c8-103">Skapa en grupp för MongoDB, snabb, AngularJS och Node.js (medel) på en Linux-VM i Azure</span><span class="sxs-lookup"><span data-stu-id="c79c8-103">Create a MongoDB, Express, AngularJS, and Node.js (MEAN) stack on a Linux VM in Azure</span></span>

<span data-ttu-id="c79c8-104">Den här kursen visar hur tooimplement en MongoDB, snabb, AngularJS och Node.js (medel) stacken på en Linux-VM i Azure.</span><span class="sxs-lookup"><span data-stu-id="c79c8-104">This tutorial shows you how tooimplement a MongoDB, Express, AngularJS, and Node.js (MEAN) stack on a Linux VM in Azure.</span></span> <span data-ttu-id="c79c8-105">hello medelvärde stack som du skapar kan lägga till, ta bort och listar böcker i en databas.</span><span class="sxs-lookup"><span data-stu-id="c79c8-105">hello MEAN stack that you create enables adding, deleting, and listing books in a database.</span></span> <span data-ttu-id="c79c8-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="c79c8-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c79c8-107">Skapa en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="c79c8-107">Create a Linux VM</span></span>
> * <span data-ttu-id="c79c8-108">Installera Node.js</span><span class="sxs-lookup"><span data-stu-id="c79c8-108">Install Node.js</span></span>
> * <span data-ttu-id="c79c8-109">Installera MongoDB och ställa in hello-server</span><span class="sxs-lookup"><span data-stu-id="c79c8-109">Install MongoDB and set up hello server</span></span>
> * <span data-ttu-id="c79c8-110">Installera Express och konfigurerat vägar toohello server</span><span class="sxs-lookup"><span data-stu-id="c79c8-110">Install Express and set up routes toohello server</span></span>
> * <span data-ttu-id="c79c8-111">Åtkomst hello vägar med AngularJS</span><span class="sxs-lookup"><span data-stu-id="c79c8-111">Access hello routes with AngularJS</span></span>
> * <span data-ttu-id="c79c8-112">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="c79c8-112">Run hello application</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c79c8-113">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="c79c8-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="c79c8-114">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="c79c8-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="c79c8-115">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c79c8-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>


## <a name="create-a-linux-vm"></a><span data-ttu-id="c79c8-116">Skapa en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="c79c8-116">Create a Linux VM</span></span>

<span data-ttu-id="c79c8-117">Skapa en resursgrupp med hello [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) kommando och skapa en Linux VM med hello [az vm skapa](https://docs.microsoft.com/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="c79c8-117">Create a resource group with hello [az group create](https://docs.microsoft.com/cli/azure/group#create) command and create a Linux VM with hello [az vm create](https://docs.microsoft.com/cli/azure/vm#create) command.</span></span> <span data-ttu-id="c79c8-118">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="c79c8-118">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span>

<span data-ttu-id="c79c8-119">hello följande exempel används hello Azure CLI toocreate en resursgrupp med namnet *myResourceGroupMEAN* i hello *eastus* plats.</span><span class="sxs-lookup"><span data-stu-id="c79c8-119">hello following example uses hello Azure CLI toocreate a resource group named *myResourceGroupMEAN* in hello *eastus* location.</span></span> <span data-ttu-id="c79c8-120">En virtuell dator skapas namngivna *myVM* med SSH-nycklar om de inte redan finns på standardplatsen nyckel.</span><span class="sxs-lookup"><span data-stu-id="c79c8-120">A VM is created named *myVM* with SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="c79c8-121">toouse en specifik uppsättning nycklar, Använd hello--ssh-nyckel / värde-alternativet.</span><span class="sxs-lookup"><span data-stu-id="c79c8-121">toouse a specific set of keys, use hello --ssh-key-value option.</span></span>

```azurecli-interactive
az group create --name myResourceGroupMEAN --location eastus
az vm create \
    --resource-group myResourceGroupMEAN \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --admin-password 'Azure12345678!' \
    --generate-ssh-keys
az vm open-port --port 3300 --resource-group myResourceGroupMEAN --name myVM
```

<span data-ttu-id="c79c8-122">När du har skapat hello VM visar hello Azure CLI information liknande toohello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="c79c8-122">When hello VM has been created, hello Azure CLI shows information similar toohello following example:</span></span> 

```azurecli-interactive
{
  "fqdns": "",
  "id": "/subscriptions/{subscription-id}/resourceGroups/myResourceGroupMEAN/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.72.77.9",
  "resourceGroup": "myResourceGroupMEAN"
}
```
<span data-ttu-id="c79c8-123">Anteckna hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="c79c8-123">Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="c79c8-124">Den här adressen är används tooaccess hello VM.</span><span class="sxs-lookup"><span data-stu-id="c79c8-124">This address is used tooaccess hello VM.</span></span>

<span data-ttu-id="c79c8-125">Använd hello följande kommando toocreate en SSH-session med hello VM.</span><span class="sxs-lookup"><span data-stu-id="c79c8-125">Use hello following command toocreate an SSH session with hello VM.</span></span> <span data-ttu-id="c79c8-126">Kontrollera att toouse hello korrekt offentlig IP-adress.</span><span class="sxs-lookup"><span data-stu-id="c79c8-126">Make sure toouse hello correct public IP address.</span></span> <span data-ttu-id="c79c8-127">I vårt exempel ovan vår IP var-adress 13.72.77.9.</span><span class="sxs-lookup"><span data-stu-id="c79c8-127">In our example above our IP address was 13.72.77.9.</span></span>

```bash
ssh azureuser@13.72.77.9
```

## <a name="install-nodejs"></a><span data-ttu-id="c79c8-128">Installera Node.js</span><span class="sxs-lookup"><span data-stu-id="c79c8-128">Install Node.js</span></span>

<span data-ttu-id="c79c8-129">[Node.js](https://nodejs.org/en/) är en JavaScript-körning som bygger på Chrome's V8 JavaScript-motorn.</span><span class="sxs-lookup"><span data-stu-id="c79c8-129">[Node.js](https://nodejs.org/en/) is a JavaScript runtime built on Chrome's V8 JavaScript engine.</span></span> <span data-ttu-id="c79c8-130">Node.js används i den här självstudiekursen tooset hello Express vägar och AngularJS-domänkontrollanter.</span><span class="sxs-lookup"><span data-stu-id="c79c8-130">Node.js is used in this tutorial tooset up hello Express routes and AngularJS controllers.</span></span>

<span data-ttu-id="c79c8-131">Installera Node.js på hello VM som använder hello bash-gränssnitt som du öppnade med SSH.</span><span class="sxs-lookup"><span data-stu-id="c79c8-131">On hello VM, using hello bash shell that you opened with SSH, install Node.js.</span></span>

```bash
sudo apt-get install -y nodejs
```

## <a name="install-mongodb-and-set-up-hello-server"></a><span data-ttu-id="c79c8-132">Installera MongoDB och ställa in hello-server</span><span class="sxs-lookup"><span data-stu-id="c79c8-132">Install MongoDB and set up hello server</span></span>
<span data-ttu-id="c79c8-133">[MongoDB](http://www.mongodb.com) lagrar data i flexibla, JSON-liknande dokument.</span><span class="sxs-lookup"><span data-stu-id="c79c8-133">[MongoDB](http://www.mongodb.com) stores data in flexible, JSON-like documents.</span></span> <span data-ttu-id="c79c8-134">Fält i en databas kan variera från dokumentet toodocument och datastruktur kan ändras med tiden.</span><span class="sxs-lookup"><span data-stu-id="c79c8-134">Fields in a database can vary from document toodocument and data structure can be changed over time.</span></span> <span data-ttu-id="c79c8-135">Vår exempelprogram vi lägger till book poster tooMongoDB som innehåller namn, isbn nummer, författare och antalet sidor.</span><span class="sxs-lookup"><span data-stu-id="c79c8-135">For our example application, we are adding book records tooMongoDB that contain book name, isbn number, author, and number of pages.</span></span> 

1. <span data-ttu-id="c79c8-136">Hello VM som använder hello bash-gränssnitt som du öppnade med SSH, ange hello MongoDB nyckel.</span><span class="sxs-lookup"><span data-stu-id="c79c8-136">On hello VM, using hello bash shell that you opened with SSH, set hello MongoDB key.</span></span>

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    ```

2. <span data-ttu-id="c79c8-137">Uppdatera hello Pakethanteraren med hello nyckel.</span><span class="sxs-lookup"><span data-stu-id="c79c8-137">Update hello package manager with hello key.</span></span>
  
    ```bash
    sudo apt-get update
    ```

3. <span data-ttu-id="c79c8-138">Installera MongoDB.</span><span class="sxs-lookup"><span data-stu-id="c79c8-138">Install MongoDB.</span></span>

    ```bash
    sudo apt-get install -y mongodb
    ```

4. <span data-ttu-id="c79c8-139">Starta hello-server.</span><span class="sxs-lookup"><span data-stu-id="c79c8-139">Start hello server.</span></span>

    ```bash
    sudo service mongodb start
    ```

5. <span data-ttu-id="c79c8-140">Vi behöver också tooinstall hello [body-parsern](https://www.npmjs.com/package/body-parser-json) paketet toohelp oss bearbeta hello JSON skickade begäranden toohello server.</span><span class="sxs-lookup"><span data-stu-id="c79c8-140">We also need tooinstall hello [body-parser](https://www.npmjs.com/package/body-parser-json) package toohelp us process hello JSON passed in requests toohello server.</span></span>

    <span data-ttu-id="c79c8-141">Installera hello npm Pakethanteraren.</span><span class="sxs-lookup"><span data-stu-id="c79c8-141">Install hello npm package manager.</span></span>

    ```bash
    sudo apt-get install npm
    ```

    <span data-ttu-id="c79c8-142">Installationspaket hello brödtext parsern.</span><span class="sxs-lookup"><span data-stu-id="c79c8-142">Install hello body parser package.</span></span>
    
    ```bash
    sudo npm install body-parser
    ```

6. <span data-ttu-id="c79c8-143">Skapa en mapp med namnet *böcker* och lägga till en fil tooit med namnet *server.js* som innehåller hello konfiguration för hello webbservern.</span><span class="sxs-lookup"><span data-stu-id="c79c8-143">Create a folder named *Books* and add a file tooit named *server.js* that contains hello configuration for hello web server.</span></span>

    ```node.js
    var express = require('express');
    var bodyParser = require('body-parser');
    var app = express();
    app.use(express.static(__dirname + '/public'));
    app.use(bodyParser.json());
    require('./apps/routes')(app);
    app.set('port', 3300);
    app.listen(app.get('port'), function() {
        console.log('Server up: http://localhost:' + app.get('port'));
    });
    ```

## <a name="install-express-and-set-up-routes-toohello-server"></a><span data-ttu-id="c79c8-144">Installera Express och konfigurerat vägar toohello server</span><span class="sxs-lookup"><span data-stu-id="c79c8-144">Install Express and set up routes toohello server</span></span>

<span data-ttu-id="c79c8-145">[Express](https://expressjs.com) är ett minimalt och flexibelt Node.js-program som innehåller funktioner för webb- och mobilprogram.</span><span class="sxs-lookup"><span data-stu-id="c79c8-145">[Express](https://expressjs.com) is a minimal and flexible Node.js web application framework that provides features for web and mobile applications.</span></span> <span data-ttu-id="c79c8-146">Express används i den här självstudiekursen toopass Boot information tooand från vår MongoDB-databas.</span><span class="sxs-lookup"><span data-stu-id="c79c8-146">Express is used in this tutorial toopass book information tooand from our MongoDB database.</span></span> <span data-ttu-id="c79c8-147">[Mongoose](http://mongoosejs.com) tillhandahåller en enkel, schema-baserad lösning toomodel dina programdata.</span><span class="sxs-lookup"><span data-stu-id="c79c8-147">[Mongoose](http://mongoosejs.com) provides a straight-forward, schema-based solution toomodel your application data.</span></span> <span data-ttu-id="c79c8-148">Mongoose används i den här självstudiekursen tooprovide book-schemat för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="c79c8-148">Mongoose is used in this tutorial tooprovide a book schema for hello database.</span></span>

1. <span data-ttu-id="c79c8-149">Installera Express och Mongoose.</span><span class="sxs-lookup"><span data-stu-id="c79c8-149">Install Express and Mongoose.</span></span>

    ```bash
    sudo npm install express mongoose
    ```

2. <span data-ttu-id="c79c8-150">I hello *böcker* mapp, skapa en mapp med namnet *appar* och lägga till en fil med namnet *routes.js* med hello express vägar som definierats.</span><span class="sxs-lookup"><span data-stu-id="c79c8-150">In hello *Books* folder, create a folder named *apps* and add a file named *routes.js* with hello express routes defined.</span></span>

    ```node.js
    var Book = require('./models/book');
    module.exports = function(app) {
      app.get('/book', function(req, res) {
        Book.find({}, function(err, result) {
          if ( err ) throw err;
          res.json(result);
        });
      }); 
      app.post('/book', function(req, res) {
        var book = new Book( {
          name:req.body.name,
          isbn:req.body.isbn,
          author:req.body.author,
          pages:req.body.pages
        });
        book.save(function(err, result) {
          if ( err ) throw err;
          res.json( {
            message:"Successfully added book",
            book:result
          });
        });
      });
      app.delete("/book/:isbn", function(req, res) {
        Book.findOneAndRemove(req.query, function(err, result) {
          if ( err ) throw err;
          res.json( {
            message: "Successfully deleted hello book",
            book: result
          });
        });
      });
      var path = require('path');
      app.get('*', function(req, res) {
        res.sendfile(path.join(__dirname + '/public', 'index.html'));
      });
    };
    ```

3. <span data-ttu-id="c79c8-151">I hello *appar* mapp, skapa en mapp med namnet *modeller* och lägga till en fil med namnet *book.js* med hello book modellkonfiguration definierad.</span><span class="sxs-lookup"><span data-stu-id="c79c8-151">In hello *apps* folder, create a folder named *models* and add a file named *book.js* with hello book model configuration defined.</span></span>  

    ```node.js
    var mongoose = require('mongoose');
    var dbHost = 'mongodb://localhost:27017/test';
    mongoose.connect(dbHost);
    mongoose.connection;
    mongoose.set('debug', true);
    var bookSchema = mongoose.Schema( {
      name: String,
      isbn: {type: String, index: true},
      author: String,
      pages: Number
    });
    var Book = mongoose.model('Book', bookSchema);
    module.exports = mongoose.model('Book', bookSchema); 
    ```

## <a name="access-hello-routes-with-angularjs"></a><span data-ttu-id="c79c8-152">Åtkomst hello vägar med AngularJS</span><span class="sxs-lookup"><span data-stu-id="c79c8-152">Access hello routes with AngularJS</span></span>

<span data-ttu-id="c79c8-153">[AngularJS](https://angularjs.org) innehåller ett webbramverk för att skapa dynamiska vyer i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="c79c8-153">[AngularJS](https://angularjs.org) provides a web framework for creating dynamic views in your web applications.</span></span> <span data-ttu-id="c79c8-154">I den här självstudiekursen vi använda AngularJS tooconnect vår webbsida med snabb och utföra åtgärder på vår book-databas.</span><span class="sxs-lookup"><span data-stu-id="c79c8-154">In this tutorial, we use AngularJS tooconnect our web page with Express and perform actions on our book database.</span></span>

1. <span data-ttu-id="c79c8-155">Ändra hello katalogen säkerhetskopiera för*böcker* (`cd ../..`), och sedan skapa en mapp med namnet *offentliga* och lägga till en fil med namnet *script.js* hello-styrenhet konfigurationen har definierats.</span><span class="sxs-lookup"><span data-stu-id="c79c8-155">Change hello directory back up too*Books* (`cd ../..`), and then create a folder named *public* and add a file named *script.js* with hello controller configuration defined.</span></span>

    ```node.js
    var app = angular.module('myApp', []);
    app.controller('myCtrl', function($scope, $http) {
      $http( {
        method: 'GET',
        url: '/book'
      }).then(function successCallback(response) {
        $scope.books = response.data;
      }, function errorCallback(response) {
        console.log('Error: ' + response);
      });
      $scope.del_book = function(book) {
        $http( {
          method: 'DELETE',
          url: '/book/:isbn',
          params: {'isbn': book.isbn}
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
      $scope.add_book = function() {
        var body = '{ "name": "' + $scope.Name + 
        '", "isbn": "' + $scope.Isbn +
        '", "author": "' + $scope.Author + 
        '", "pages": "' + $scope.Pages + '" }';
        $http({
          method: 'POST',
          url: '/book',
          data: body
        }).then(function successCallback(response) {
          console.log(response);
        }, function errorCallback(response) {
          console.log('Error: ' + response);
        });
      };
    });
    ```
    
2. <span data-ttu-id="c79c8-156">I hello *offentliga* mapp, skapa en fil med namnet *index.html* med hello webbsida som definierats.</span><span class="sxs-lookup"><span data-stu-id="c79c8-156">In hello *public* folder, create a file named *index.html* with hello web page defined.</span></span>

    ```html
    <!doctype html>
    <html ng-app="myApp" ng-controller="myCtrl">
      <head>
        <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.6.4/angular.min.js"></script>
        <script src="script.js"></script>
      </head>
      <body>
        <div>
          <table>
            <tr>
              <td>Name:</td> 
              <td><input type="text" ng-model="Name"></td>
            </tr>
            <tr>
              <td>Isbn:</td>
              <td><input type="text" ng-model="Isbn"></td>
            </tr>
            <tr>
              <td>Author:</td> 
              <td><input type="text" ng-model="Author"></td>
            </tr>
            <tr>
              <td>Pages:</td>
              <td><input type="number" ng-model="Pages"></td>
            </tr>
          </table>
          <button ng-click="add_book()">Add</button>
        </div>
        <hr>
        <div>
          <table>
            <tr>
              <th>Name</th>
              <th>Isbn</th>
              <th>Author</th>
              <th>Pages</th>
            </tr>
            <tr ng-repeat="book in books">
              <td><input type="button" value="Delete" data-ng-click="del_book(book)"></td>
              <td>{{book.name}}</td>
              <td>{{book.isbn}}</td>
              <td>{{book.author}}</td>
              <td>{{book.pages}}</td>
            </tr>
          </table>
        </div>
      </body>
    </html>
    ```

##  <a name="run-hello-application"></a><span data-ttu-id="c79c8-157">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="c79c8-157">Run hello application</span></span>

1. <span data-ttu-id="c79c8-158">Ändra hello katalogen säkerhetskopiera för*böcker* (`cd ..`) och starta hello server genom att köra det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="c79c8-158">Change hello directory back up too*Books* (`cd ..`) and start hello server by running this command:</span></span>

    ```bash
    nodejs server.js
    ```

2. <span data-ttu-id="c79c8-159">Öppna en webbläsare toohello webbadress som du antecknade för hello VM.</span><span class="sxs-lookup"><span data-stu-id="c79c8-159">Open a web browser toohello address that you recorded for hello VM.</span></span> <span data-ttu-id="c79c8-160">Till exempel *http://13.72.77.9:3300*.</span><span class="sxs-lookup"><span data-stu-id="c79c8-160">For example, *http://13.72.77.9:3300*.</span></span> <span data-ttu-id="c79c8-161">Du bör se något liknande hello följande sida:</span><span class="sxs-lookup"><span data-stu-id="c79c8-161">You should see something like hello following page:</span></span>

    ![En post](media/tutorial-mean/meanstack-init.png)

3. <span data-ttu-id="c79c8-163">Ange data i hello textrutor och klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c79c8-163">Enter data into hello textboxes and click **Add**.</span></span> <span data-ttu-id="c79c8-164">Exempel:</span><span class="sxs-lookup"><span data-stu-id="c79c8-164">For example:</span></span>

    ![Lägg till en post](media/tutorial-mean/meanstack-add.png)

4. <span data-ttu-id="c79c8-166">Du bör se något som liknar den här sidan när du har uppdaterat hello sidan:</span><span class="sxs-lookup"><span data-stu-id="c79c8-166">After refreshing hello page, you should see something like this page:</span></span>

    ![Book poster](media/tutorial-mean/meanstack-list.png)

5. <span data-ttu-id="c79c8-168">Du kan klicka på **ta bort** och tar bort en post i hello hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="c79c8-168">You could click **Delete** and remove hello book record from hello database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c79c8-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c79c8-169">Next steps</span></span>

<span data-ttu-id="c79c8-170">I kursen får du skapat ett webbprogram som övervakas av book poster med hjälp av en medelvärde stacken på en Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="c79c8-170">In this tutorial, you created a web application that keeps track of book records using a MEAN stack on a Linux VM.</span></span> <span data-ttu-id="c79c8-171">Du har lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="c79c8-171">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="c79c8-172">Skapa en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="c79c8-172">Create a Linux VM</span></span>
> * <span data-ttu-id="c79c8-173">Installera Node.js</span><span class="sxs-lookup"><span data-stu-id="c79c8-173">Install Node.js</span></span>
> * <span data-ttu-id="c79c8-174">Installera MongoDB och ställa in hello-server</span><span class="sxs-lookup"><span data-stu-id="c79c8-174">Install MongoDB and set up hello server</span></span>
> * <span data-ttu-id="c79c8-175">Installera Express och konfigurerat vägar toohello server</span><span class="sxs-lookup"><span data-stu-id="c79c8-175">Install Express and set up routes toohello server</span></span>
> * <span data-ttu-id="c79c8-176">Åtkomst hello vägar med AngularJS</span><span class="sxs-lookup"><span data-stu-id="c79c8-176">Access hello routes with AngularJS</span></span>
> * <span data-ttu-id="c79c8-177">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="c79c8-177">Run hello application</span></span>

<span data-ttu-id="c79c8-178">I förväg toohello nästa självstudiekurs toolearn hur toosecure webbservrar med SSL-certifikat.</span><span class="sxs-lookup"><span data-stu-id="c79c8-178">Advance toohello next tutorial toolearn how toosecure web servers with SSL certificates.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="c79c8-179">Säker webbserver med SSL</span><span class="sxs-lookup"><span data-stu-id="c79c8-179">Secure web server with SSL</span></span>](tutorial-secure-web-server.md)
