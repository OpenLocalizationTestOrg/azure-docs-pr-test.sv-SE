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
# <a name="create-a-mongodb-express-angularjs-and-nodejs-mean-stack-on-a-linux-vm-in-azure"></a>Skapa en grupp för MongoDB, snabb, AngularJS och Node.js (medel) på en Linux-VM i Azure

Den här kursen visar hur tooimplement en MongoDB, snabb, AngularJS och Node.js (medel) stacken på en Linux-VM i Azure. hello medelvärde stack som du skapar kan lägga till, ta bort och listar böcker i en databas. Lär dig att:

> [!div class="checklist"]
> * Skapa en virtuell Linux-dator
> * Installera Node.js
> * Installera MongoDB och ställa in hello-server
> * Installera Express och konfigurerat vägar toohello server
> * Åtkomst hello vägar med AngularJS
> * Kör programmet hello

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).


## <a name="create-a-linux-vm"></a>Skapa en virtuell Linux-dator

Skapa en resursgrupp med hello [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) kommando och skapa en Linux VM med hello [az vm skapa](https://docs.microsoft.com/cli/azure/vm#create) kommando. En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.

hello följande exempel används hello Azure CLI toocreate en resursgrupp med namnet *myResourceGroupMEAN* i hello *eastus* plats. En virtuell dator skapas namngivna *myVM* med SSH-nycklar om de inte redan finns på standardplatsen nyckel. toouse en specifik uppsättning nycklar, Använd hello--ssh-nyckel / värde-alternativet.

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

När du har skapat hello VM visar hello Azure CLI information liknande toohello följande exempel: 

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
Anteckna hello `publicIpAddress`. Den här adressen är används tooaccess hello VM.

Använd hello följande kommando toocreate en SSH-session med hello VM. Kontrollera att toouse hello korrekt offentlig IP-adress. I vårt exempel ovan vår IP var-adress 13.72.77.9.

```bash
ssh azureuser@13.72.77.9
```

## <a name="install-nodejs"></a>Installera Node.js

[Node.js](https://nodejs.org/en/) är en JavaScript-körning som bygger på Chrome's V8 JavaScript-motorn. Node.js används i den här självstudiekursen tooset hello Express vägar och AngularJS-domänkontrollanter.

Installera Node.js på hello VM som använder hello bash-gränssnitt som du öppnade med SSH.

```bash
sudo apt-get install -y nodejs
```

## <a name="install-mongodb-and-set-up-hello-server"></a>Installera MongoDB och ställa in hello-server
[MongoDB](http://www.mongodb.com) lagrar data i flexibla, JSON-liknande dokument. Fält i en databas kan variera från dokumentet toodocument och datastruktur kan ändras med tiden. Vår exempelprogram vi lägger till book poster tooMongoDB som innehåller namn, isbn nummer, författare och antalet sidor. 

1. Hello VM som använder hello bash-gränssnitt som du öppnade med SSH, ange hello MongoDB nyckel.

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    ```

2. Uppdatera hello Pakethanteraren med hello nyckel.
  
    ```bash
    sudo apt-get update
    ```

3. Installera MongoDB.

    ```bash
    sudo apt-get install -y mongodb
    ```

4. Starta hello-server.

    ```bash
    sudo service mongodb start
    ```

5. Vi behöver också tooinstall hello [body-parsern](https://www.npmjs.com/package/body-parser-json) paketet toohelp oss bearbeta hello JSON skickade begäranden toohello server.

    Installera hello npm Pakethanteraren.

    ```bash
    sudo apt-get install npm
    ```

    Installationspaket hello brödtext parsern.
    
    ```bash
    sudo npm install body-parser
    ```

6. Skapa en mapp med namnet *böcker* och lägga till en fil tooit med namnet *server.js* som innehåller hello konfiguration för hello webbservern.

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

## <a name="install-express-and-set-up-routes-toohello-server"></a>Installera Express och konfigurerat vägar toohello server

[Express](https://expressjs.com) är ett minimalt och flexibelt Node.js-program som innehåller funktioner för webb- och mobilprogram. Express används i den här självstudiekursen toopass Boot information tooand från vår MongoDB-databas. [Mongoose](http://mongoosejs.com) tillhandahåller en enkel, schema-baserad lösning toomodel dina programdata. Mongoose används i den här självstudiekursen tooprovide book-schemat för hello-databasen.

1. Installera Express och Mongoose.

    ```bash
    sudo npm install express mongoose
    ```

2. I hello *böcker* mapp, skapa en mapp med namnet *appar* och lägga till en fil med namnet *routes.js* med hello express vägar som definierats.

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

3. I hello *appar* mapp, skapa en mapp med namnet *modeller* och lägga till en fil med namnet *book.js* med hello book modellkonfiguration definierad.  

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

## <a name="access-hello-routes-with-angularjs"></a>Åtkomst hello vägar med AngularJS

[AngularJS](https://angularjs.org) innehåller ett webbramverk för att skapa dynamiska vyer i ditt webbprogram. I den här självstudiekursen vi använda AngularJS tooconnect vår webbsida med snabb och utföra åtgärder på vår book-databas.

1. Ändra hello katalogen säkerhetskopiera för*böcker* (`cd ../..`), och sedan skapa en mapp med namnet *offentliga* och lägga till en fil med namnet *script.js* hello-styrenhet konfigurationen har definierats.

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
    
2. I hello *offentliga* mapp, skapa en fil med namnet *index.html* med hello webbsida som definierats.

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

##  <a name="run-hello-application"></a>Kör programmet hello

1. Ändra hello katalogen säkerhetskopiera för*böcker* (`cd ..`) och starta hello server genom att köra det här kommandot:

    ```bash
    nodejs server.js
    ```

2. Öppna en webbläsare toohello webbadress som du antecknade för hello VM. Till exempel *http://13.72.77.9:3300*. Du bör se något liknande hello följande sida:

    ![En post](media/tutorial-mean/meanstack-init.png)

3. Ange data i hello textrutor och klicka på **Lägg till**. Exempel:

    ![Lägg till en post](media/tutorial-mean/meanstack-add.png)

4. Du bör se något som liknar den här sidan när du har uppdaterat hello sidan:

    ![Book poster](media/tutorial-mean/meanstack-list.png)

5. Du kan klicka på **ta bort** och tar bort en post i hello hello-databasen.

## <a name="next-steps"></a>Nästa steg

I kursen får du skapat ett webbprogram som övervakas av book poster med hjälp av en medelvärde stacken på en Linux-VM. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa en virtuell Linux-dator
> * Installera Node.js
> * Installera MongoDB och ställa in hello-server
> * Installera Express och konfigurerat vägar toohello server
> * Åtkomst hello vägar med AngularJS
> * Kör programmet hello

I förväg toohello nästa självstudiekurs toolearn hur toosecure webbservrar med SSL-certifikat.

> [!div class="nextstepaction"]
> [Säker webbserver med SSL](tutorial-secure-web-server.md)
