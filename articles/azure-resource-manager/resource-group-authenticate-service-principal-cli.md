---
title: "aaaCreate identitet för Azure-app med Azure CLI | Microsoft Docs"
description: "Beskriver hur toouse Azure CLI toocreate ett Azure Active Directory-program och tjänstens huvudnamn och ge det åtkomst till tooresources via rollbaserad åtkomst kontroll. Den visar hur tooauthenticate program med ett lösenord eller ett certifikat."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: c224a189-dd28-4801-b3e3-26991b0eb24d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: 0d693ec801d4f4d6c24ec420580776e73014b325
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-cli-toocreate-a-service-principal-tooaccess-resources"></a>Använda Azure CLI toocreate en service principal tooaccess resurser

När du har en app eller skript som behöver tooaccess resurser kan du ställa in en identitet för hello app och autentisera hello appen med sina egna autentiseringsuppgifter. Den här identiteten kallas ett huvudnamn för tjänsten. Den här metoden kan du:

* Tilldela behörigheter toohello app identitet som skiljer sig från din egen behörighet. Dessa behörigheter normalt begränsade tooexactly vilka hello program behöver toodo.
* Använda ett certifikat för autentisering när du kör ett oövervakat skript.

Den här artikeln beskrivs hur du toouse [Azure CLI 1.0](../cli-install-nodejs.md) tooset upp ett program toorun under sina egna autentiseringsuppgifter och identitet. Installera hello senaste versionen av [Azure CLI 1.0](../cli-install-nodejs.md) toomake att din miljö matchar hello exemplen i den här artikeln.

## <a name="required-permissions"></a>Behörigheter som krävs
toocomplete det här avsnittet, du måste ha tillräckliga behörigheter för både i Azure Active Directory och Azure-prenumerationen. Du måste specifikt vara kan toocreate en app i hello Azure Active Directory och tilldela hello service principal tooa roll. 

hello enklaste sättet toocheck om ditt konto har rätt behörigheter är hello-portalen. Se [Kontrollera behörighet som krävs i portalen](resource-group-create-service-principal-portal.md#required-permissions).

Nu fortsätter du tooa för antingen [lösenord](#create-service-principal-with-password) eller [certifikat](#create-service-principal-with-certificate) autentisering.

## <a name="create-service-principal-with-password"></a>Skapa tjänstens huvudnamn med lösenord
I det här avsnittet, utföra hello steg toocreate hello AD-program med ett lösenord och tilldela hello Reader rollen toohello tjänstens huvudnamn.

1. Logga in tooyour konto.
   
   ```azurecli
   azure login
   ```
2. toocreate en app-identitet innehåller hello namnet på hello app och ett lösenord, som visas i hello följande kommando:
     
   ```azurecli
   azure ad sp create -n exampleapp -p {your-password}
   ```
     
   hello nya tjänstens huvudnamn returneras. hello objekt-Id krävs när tillståndsbeviljande. hello guid som anges med hello Service Principal Names krävs när du loggar in. Detta guid är hello samma värde som hello app-id. I hello exempelprogrammen är det här värdet anges tooas hello `Client ID`. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating application exampleapp
     / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
     data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
     data:    Display Name:            exampleapp
     data:    Service Principal Names:
     data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
     data:                             https://www.contoso.org/example
     info:    ad sp create command OK
   ```

3. Bevilja hello service principal behörighet för din prenumeration. Lägg till hello service principal toohello rollen Läsare, vilket ger behörighet tooread alla resurser i hello prenumeration i det här exemplet. Andra roller, se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md). Ange hello objekt-Id som du använde när du skapar hello program för hello objekt-ID-parametern. Innan du kör det här kommandot måste du tillåta en stund innan hello nya service principal toopropagate i Azure Active Directory. När du kör kommandona manuellt, vanligtvis tillräckligt med tid har gått ut mellan aktiviteter. I ett skript, bör du lägga till ett steg toosleep mellan hello-kommandon (t.ex. `sleep 15`). Om du ser ett fel anger hello huvudnamn inte finns i hello directory kör hello kommandot igen.
   
   ```azurecli
   azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/
   ```
   
Klart! Din AD-program och tjänstens huvudnamn ställs in. hello nästa avsnitt visar hur toolog in med hello autentiseringsuppgifter via Azure CLI. Om du vill toouse hello autentiseringsuppgifter i koden programmet behöver inte toocontinue med det här avsnittet. Du kan hoppa toohello [programexempel](#sample-applications) exempel på Logga in med ditt program-id och lösenord. 

### <a name="provide-credentials-through-azure-cli"></a>Ange autentiseringsuppgifter via Azure CLI
Du måste nu toolog i som hello programmet tooperform funktioner.

1. Varje gång du loggar in som ett huvudnamn för tjänsten, behöver du tooprovide hello klient-id för hello katalog för din AD-app. En klient är en instans av Azure Active Directory. tooretrieve hello klient-id för prenumerationen för tillfället är autentiserat används:
   
   ```azurecli
   azure account show
   ```
   
   Som returnerar:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
     Om du behöver tooget hello klient-id för en annan prenumeration, använder du följande kommando hello:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. Om du behöver tooretrieve hello klient-id toouse för att logga in, Använd:
   
   ```azurecli
   azure ad sp show -c exampleapp --json
   ```
   
     hello värdet toouse för att logga in är hello guid som anges i hello tjänstens huvudnamn.
   
   ```azurecli
   [
     {
       "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "7132aca4-1bdb-4238-ad81-996ff91d8db4"
       ]
     }
   ]
   ```
3. Logga in som hello tjänstens huvudnamn.
   
   ```azurecli
   azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}
   ```
   
    Du ombeds hello lösenord. Ange hello-lösenord som du angav när du skapar hello AD-program.
   
   ```azurecli
   info:    Executing command login
   Password: ********
   ```

Du är nu autentiserad som hello tjänstens huvudnamn för hello tjänstens huvudnamn som du skapade.

Alternativt kan du anropa REST-åtgärder från hello kommandoraden toolog i. Du kan hämta hello åtkomsttoken för användning med andra åtgärder från hello autentiseringssvar. Ett exempel för att hämta hello åtkomst-token genom att anropa REST-åtgärder finns [genererar en åtkomst-Token](resource-manager-rest-api.md#generating-an-access-token).

## <a name="create-service-principal-with-certificate"></a>Skapa tjänstens huvudnamn med certifikat
I det här avsnittet kan du utföra hello steg för att:

* Skapa ett självsignerat certifikat
* Skapa hello AD-program med hello certifikat och hello tjänstens huvudnamn
* tilldela hello Reader rollen toohello tjänstens huvudnamn

toocomplete dessa steg, måste du ha [OpenSSL](http://www.openssl.org/) installerad.

1. Skapa ett självsignerat certifikat.
   
   ```
   openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'
   ```

2. hello före steget Skapa två filer - privkey.pem och cert.pem. Kombinera hello offentliga och privata nycklar i en enda fil.

   ```
   cat privkey.pem cert.pem > examplecert.pem
   ```

3. Öppna hello **examplecert.pem** filen och leta efter hello lång teckenföljd mellan **---BEGIN CERTIFICATE---** och **---END CERTIFICATE---**. Kopiera hello certifikatdata. Skicka informationen som en parameter när du skapar hello tjänstens huvudnamn.

4. Logga in tooyour konto.

   ```azurecli
   azure login
   ```
5. toocreate hello tjänstens huvudnamn, ange hello namn för hello app och hello certifikatdata som visas i hello följande kommando:
     
   ```azurecli
   azure ad sp create -n exampleapp --cert-value {certificate data}
   ```
     
   hello nya tjänstens huvudnamn returneras. hello objekt-Id krävs när tillståndsbeviljande. hello guid som anges med hello Service Principal Names krävs när du loggar in. Detta guid är hello samma värde som hello app-id. I hello exempelprogrammen är det här värdet anges tooas hello klient-ID. 
     
   ```azurecli
   info:    Executing command ad sp create
     
   Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
     data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
     data:    Display Name:     exampleapp
     data:    Service Principal Names:
     data:                      4fd39843-c338-417d-b549-a545f584a745
     data:                      https://www.contoso.org/example
     info:    ad sp create command OK
   ```
6. Bevilja hello service principal behörighet för din prenumeration. Lägg till hello service principal toohello rollen Läsare, vilket ger behörighet tooread alla resurser i hello prenumeration i det här exemplet. Andra roller, se [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md). Ange hello objekt-Id som du använde när du skapar hello program för hello objekt-ID-parametern. Innan du kör det här kommandot måste du tillåta en stund innan hello nya service principal toopropagate i Azure Active Directory. När du kör kommandona manuellt, vanligtvis tillräckligt med tid har gått ut mellan aktiviteter. I ett skript, bör du lägga till ett steg toosleep mellan hello-kommandon (t.ex. `sleep 15`). Om du ser ett fel anger hello huvudnamn inte finns i hello directory kör hello kommandot igen.
   
   ```azurecli
   azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/
   ```
  
### <a name="provide-certificate-through-automated-azure-cli-script"></a>Ange certifikat via automatisk Azure CLI-skript
Du måste nu toolog i som hello programmet tooperform funktioner.

1. Varje gång du loggar in som ett huvudnamn för tjänsten, behöver du tooprovide hello klient-id för hello katalog för din AD-app. En klient är en instans av Azure Active Directory. tooretrieve hello klient-id för prenumerationen för tillfället är autentiserat används:
   
   ```azurecli
   azure account show
   ```
   
   Som returnerar:
   
   ```azurecli
   info:    Executing command account show
   data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
   data:    ID                          : {guid}
   data:    State                       : Enabled
   data:    Tenant ID                   : {guid}
   data:    Is Default                  : true
   ...
   ```
   
   Om du behöver tooget hello klient-id för en annan prenumeration, använder du följande kommando hello:
   
   ```azurecli
   azure account show -s {subscription-id}
   ```
2. tooretrieve hello tumavtryck för certifikat och ta bort onödiga tecken kan använda:
   
   ```
   openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
   ```
   
   Som returnerar ett tumavtrycksvärde liknar:
   
   ```
   30996D9CE48A0B6E0CD49DBB9A48059BF9355851
   ```
3. Om du behöver tooretrieve hello klient-id toouse för att logga in, Använd:
   
   ```azurecli
   azure ad sp show -c exampleapp
   ```
   
   hello värdet toouse för att logga in är hello guid som anges i hello tjänstens huvudnamn.
     
   ```azurecli
   [
     {
       "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
       "objectType": "ServicePrincipal",
       "displayName": "exampleapp",
       "appId": "4fd39843-c338-417d-b549-a545f584a745",
       "servicePrincipalNames": [
         "https://www.contoso.org/example",
         "4fd39843-c338-417d-b549-a545f584a745"
       ]
     }
   ]
   ```
4. Logga in som hello tjänstens huvudnamn.
   
   ```azurecli
   azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}
   ```

Du är nu autentiserad som hello tjänstens huvudnamn för hello Azure Active Directory-program som du skapade.

## <a name="change-credentials"></a>Ändra autentiseringsuppgifter

toochange hello autentiseringsuppgifter för en AD-app, antingen på grund av ett säkerhetsintrång eller en autentiseringsuppgift förfallodatum, Använd `azure ad app set`.

toochange ett lösenord, Använd:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --password p@ssword
```

toochange certifikatvärdet, Använd:

```azurecli
azure ad app set --applicationId 4fd39843-c338-417d-b549-a545f584a745 --cert-value {certificate data}
```

## <a name="debug"></a>Felsökning

Du kan stöta på hello följande fel när du skapar ett huvudnamn för tjänsten:

* **”Authentication_Unauthorized”** eller **”ingen prenumeration finns i kontexten för hello”.** – Du ser det här felet när ditt konto inte har hello [nödvändiga behörigheter](#required-permissions) på hello Azure Active Directory tooregister en app. Normalt ser du det här felet när endast administrativa användare i Azure Active Directory kan du registrera appar och ditt konto är inte en administratör. Be din administratör tooeither tilldela tooan administratörsroll eller tooenable användare tooregister appar.

* Ditt konto **”har inte tillstånd tooperform åtgärd 'Microsoft.Authorization/roleAssignments/write' över område '/subscriptions/ {guid}'”.**  -Det här felet visas när ditt konto inte har tillräckliga behörigheter tooassign en roll tooan identitet. Be din prenumeration administratören tooadd du tooUser åtkomst rollen Administratör.

## <a name="sample-applications"></a>Exempelprogram
För information om att logga in som hello program via olika plattformar, se:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Nästa steg
* Detaljerade anvisningar för att integrera ett program i Azure för att hantera resurser, se [Developer's guide tooauthorization med hello Azure Resource Manager API](resource-manager-api-authentication.md).
* Mer information om hur du använder certifikat och Azure CLI finns i tooget [certifikatbaserad autentisering med Azure-tjänstens huvudnamn från Linux-kommandorad](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
* En lista över tillgängliga åtgärder som kan beviljas eller nekas toousers finns [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).
