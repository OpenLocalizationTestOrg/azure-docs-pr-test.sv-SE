---
title: "Skapa en grupp på en Linux-VM i Azure | Microsoft Docs"
description: "Lär dig hur du skapar en stack MongoDB, snabb, AngularJS och Node.js (medel) på en Linux-VM i Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 1d74ead08dfb63276afb08bdcb7f4e3e3db5bfd3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-mongodb-express-angularjs-and-nodejs-mean-stack-on-a-linux-vm-in-azure"></a>Skapa en grupp för MongoDB, snabb, AngularJS och Node.js (medel) på en Linux-VM i Azure

Den här kursen visar hur du implementerar en stack MongoDB, snabb, AngularJS och Node.js (medel) på en Linux-VM i Azure. MEDELVÄRDE stacken som du skapar kan lägga till, ta bort och listar böcker i en databas. Lär dig att:

> [!div class="checklist"]
> * Skapa en virtuell Linux-dator
> * Installera Node.js
> * Installera MongoDB och konfigurera servern
> * Installera Express och ställa in vägar till servern
> * Åtkomst till vägar med AngularJS
> * Köra programmet

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt kursen krävs att du använder Azure CLI version 2.0.4 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).


## <a name="create-a-linux-vm"></a>Skapa en virtuell Linux-dator

Skapa en resursgrupp med det [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#az_group_create) kommando och skapa en Linux VM med den [az vm skapa](https://docs.microsoft.com/cli/azure/vm#az_vm_create) kommando. En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.

I följande exempel använder Azure CLI för att skapa en resursgrupp med namnet *myResourceGroupMEAN* i den *eastus* plats. En virtuell dator skapas namngivna *myVM* med SSH-nycklar om de inte redan finns på standardplatsen nyckel. Om du vill använda en specifik uppsättning nycklar--ssh-nyckel / värde-alternativet.

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

När den virtuella datorn har skapats, visar Azure CLI information liknar följande exempel: 

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
Anteckna `publicIpAddress`. Den här adressen används för att få åtkomst till den virtuella datorn.

Använd följande kommando för att skapa en SSH-session med den virtuella datorn. Se till att använda rätt offentlig IP-adress. I vårt exempel ovan vår IP var-adress 13.72.77.9.

```bash
ssh azureuser@13.72.77.9
```

## <a name="install-nodejs"></a>Installera Node.js

[Node.js](https://nodejs.org/en/) är en JavaScript-körning som bygger på Chrome's V8 JavaScript-motorn. Node.js används i den här självstudiekursen för att ställa in Express vägar och styrenheter AngularJS.

Installera Node.js på den virtuella datorn med hjälp av bash-gränssnitt som du öppnade med SSH.

```bash
sudo apt-get install -y nodejs
```

## <a name="install-mongodb-and-set-up-the-server"></a>Installera MongoDB och konfigurera servern
[MongoDB](http://www.mongodb.com) lagrar data i flexibla, JSON-liknande dokument. Fält i en databas kan variera från dokumentet och datastruktur kan ändras med tiden. För vår exempelprogram vi lägger till book poster till MongoDB som innehåller namn, isbn nummer, författare och antalet sidor. 

1. På den virtuella datorn ange med hjälp av bash-gränssnitt som du öppnade med SSH, den MongoDB-nyckeln.

    ```bash
    sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6
    echo "deb [ arch=amd64 ] http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.4 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.4.list
    ```

2. Uppdatera package manager med nyckel.
  
    ```bash
    sudo apt-get update
    ```

3. Installera MongoDB.

    ```bash
    sudo apt-get install -y mongodb
    ```

4. Starta servern.

    ```bash
    sudo service mongodb start
    ```

5. Vi behöver installera den [body-parsern](https://www.npmjs.com/package/body-parser-json) paket som hjälper oss att bearbeta JSON förfrågningar som skickas till servern.

    Installera npm package manager.

    ```bash
    sudo apt-get install npm
    ```

    Installera paketet brödtext parsern.
    
    ```bash
    sudo npm install body-parser
    ```

6. Skapa en mapp med namnet *böcker* och lägga till en fil i den namngivna *server.js* som innehåller konfiguration för webbservern.

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

## <a name="install-express-and-set-up-routes-to-the-server"></a>Installera Express och ställa in vägar till servern

[Express](https://expressjs.com) är ett minimalt och flexibelt Node.js-program som innehåller funktioner för webb- och mobilprogram. Express används i den här självstudiekursen för att skicka information till och från vår MongoDB-databas på adressboken. [Mongoose](http://mongoosejs.com) tillhandahåller en enkel, schema-baserad lösning för att modellera dina programdata. Mongoose används i den här självstudiekursen för att tillhandahålla en bok schemat för databasen.

1. Installera Express och Mongoose.

    ```bash
    sudo npm install express mongoose
    ```

2. I den *böcker* mapp, skapa en mapp med namnet *appar* och lägga till en fil med namnet *routes.js* med snabb vägar som definierats.

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
            message: "Successfully deleted the book",
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

3. I den *appar* mapp, skapa en mapp med namnet *modeller* och lägga till en fil med namnet *book.js* med book modellen konfigurationen.  

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

## <a name="access-the-routes-with-angularjs"></a>Åtkomst till vägar med AngularJS

[AngularJS](https://angularjs.org) innehåller ett webbramverk för att skapa dynamiska vyer i ditt webbprogram. I den här självstudiekursen använder vi AngularJS ansluta vår sida med snabb och utföra åtgärder på vår book-databas.

1. Ändra katalogen tillbaka till *böcker* (`cd ../..`), och sedan skapa en mapp med namnet *offentliga* och lägga till en fil med namnet *script.js* med konfigurationen av domänkontrollant definierats.

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
    
2. I den *offentliga* mapp, skapa en fil med namnet *index.html* på webbsidan som definierats.

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

##  <a name="run-the-application"></a>Köra programmet

1. Ändra katalogen tillbaka till *böcker* (`cd ..`) och starta servern genom att köra det här kommandot:

    ```bash
    nodejs server.js
    ```

2. Öppna en webbläsare till adressen som du antecknade för den virtuella datorn. Till exempel *http://13.72.77.9:3300*. Du bör se något liknande följande sida:

    ![En post](media/tutorial-mean/meanstack-init.png)

3. Ange data i textrutor och klickar på **Lägg till**. Exempel:

    ![Lägg till en post](media/tutorial-mean/meanstack-add.png)

4. Du bör se något som liknar den här sidan när du har uppdaterat sidan:

    ![Book poster](media/tutorial-mean/meanstack-list.png)

5. Du kan klicka på **ta bort** och ta bort boot record från databasen.

## <a name="next-steps"></a>Nästa steg

I kursen får du skapat ett webbprogram som övervakas av book poster med hjälp av en medelvärde stacken på en Linux-VM. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa en virtuell Linux-dator
> * Installera Node.js
> * Installera MongoDB och konfigurera servern
> * Installera Express och ställa in vägar till servern
> * Åtkomst till vägar med AngularJS
> * Köra programmet

Gå vidare till nästa kurs att lära dig säker webbserver med SSL-certifikat.

> [!div class="nextstepaction"]
> [Säker webbserver med SSL](tutorial-secure-web-server.md)
