---
title: aaaBuild och distribuera en Java API-app i Azure App Service
description: "Lär dig hur toocreate en Java API-app-paketet och distribuerar den tooAzure Apptjänst."
services: app-service\api
documentationcenter: java
author: rmcmurray
manager: erikre
editor: tdykstra
ms.assetid: 8d21ba5f-fc57-4269-bc8f-2fcab936ec22
ms.service: app-service-api
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: get-started-article
ms.date: 04/25/2017
ms.author: rachelap;robmcm
ms.openlocfilehash: a4056fec870b1c4bed8ee14bb0e748b3ee89b9e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a>Skapa och distribuera en Java API-app i Azure Apptjänst
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

Den här kursen visar hur toocreate ett Java-program och distribuerar den tooAzure API Apps i Apptjänst med hjälp av [Git]. hello anvisningarna i den här kursen kan tillämpas på alla operativsystem som kan köra Java. hello koden i självstudierna byggs med [Maven]. [Jax-RS] är används toocreate hello RESTful-tjänst och genereras baserat på hello [Swagger] metadata specifikationen med hello [Swagger Editor].

## <a name="prerequisites"></a>Krav
1. [Java Developer's Kit 8] \(eller senare)
2. [Maven] installerat på utvecklingsdatorn
3. [Git] installerat på utvecklingsdatorn
4. En betald eller [kostnadsfri utvärderingsversion] prenumerationen för[Microsoft Azure]
5. Ett program för HTTP-test som [Postman]

## <a name="scaffold-hello-api-using-swaggerio"></a>Kodskelett hello API: et med Swagger.IO
Med Redigeraren för hello swagger.io online kan ange du Swagger JSON- eller YAML-kod som representerar hello strukturen för din API. När du har hello API-ytan utformats kan exportera du kod för olika plattformar och ramverk. I nästa avsnitt om hello blir hello Autogenererad kod ändrade tooinclude fingerade funktioner. 

Den här demonstrationen börjar med en Swagger JSON-text som du ska klistra in i redigeraren swagger.io för hello som kommer sedan att använda toogenerate kod genom att utnyttja JAX-RS tooaccess REST API-slutpunkt. Sedan redigerar du hello Autogenererad kod tooreturn fingerade data genom simulera en REST-API som byggts ovanpå en mekanism för datapersistence.  

1. Kopiera följande Swagger JSON-kod tooyour Urklipp hello:
   
        {
            "swagger": "2.0",
            "info": {
                "version": "v1",
                "title": "Contact List",
                "description": "A Contact list API based on Swagger and built using Java"
            },
            "host": "localhost",
            "schemes": [
                "http",
                "https"
            ],
            "basePath": "/api",
            "paths": {
                "/contacts": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_get",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                },
                "/contacts/{id}": {
                    "get": {
                        "tags": [
                            "Contact"
                        ],
                        "operationId": "contacts_getById",
                        "consumes": [],
                        "produces": [
                            "application/json",
                            "text/json"
                        ],
                        "parameters": [
                            {
                                "name": "id",
                                "in": "path",
                                "required": true,
                                "type": "integer",
                                "format": "int32"
                            }
                        ],
                        "responses": {
                            "200": {
                                "description": "OK",
                                "schema": {
                                    "type": "array",
                                    "items": {
                                        "$ref": "#/definitions/Contact"
                                    }
                                }
                            }
                        },
                        "deprecated": false
                    }
                }
            },
            "definitions": {
                "Contact": {
                    "type": "object",
                    "properties": {
                        "Id": {
                            "format": "int32",
                            "type": "integer"
                        },
                        "Name": {
                            "type": "string"
                        },
                        "EmailAddress": {
                            "type": "string"
                        }
                    }
                }
            }
        }
2. Navigera toohello [Online Swagger Editor]. När det, klickar du på hello **klistra in JSON-fil >** menyalternativet.
   
    ![Menyalternativet Klistra in JSON][paste-json]
3. Klistra in i hello kontakter listan API Swagger JSON du kopierade tidigare. 
   
    ![Klistra in JSON-kod i Swagger][pasted-swagger]
4. Visa hello dokumentationssidorna och API-översikten som visas i hello-redigeraren. 
   
    ![Visa Swagger-genererade dokument][view-swagger-generated-docs]
5. Välj hello **generera Server -> JAX-RS** menyn alternativet tooscaffold hello serverkod som du ska redigera senare tooadd fingerad implementering. 
   
    ![Menyalternativet Generera kod][generate-code-menu-item]
   
    När hello koden genereras, ska du angett toodownload en ZIP-filen. Den här filen innehåller hello koden som skapats av kodgeneratorn Swagger för hello och alla associerade byggnadsskript. Packa upp hello hela biblioteket tooa katalog på utvecklingsdatorn. 

## <a name="edit-hello-code-tooadd-api-implementation"></a>Redigera hello kod tooadd API-implementering
I det här avsnittet ska du ersätta hello genererade Swagger-kodens implementering för serversidan med din anpassade kod. hello nya koden returnerar en ArrayList kontakta entiteter toohello uppringande klient. 

1. Öppna hello *Contact.java* modellfil som finns i hello *src/gen/java/io/swagger/model* mappen med [Visual Studio Code] eller valfri textredigerare. 
   
    ![Öppna modellfilen för kontakter][open-contact-model-file]
2. Lägg till följande konstruktor inom hello hello **Kontakta** klass. 
   
        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }
3. Öppna hello *Contact.Java* tjänstimplementeringsfilen som finns i hello *src/main/java/io/swagger/api/impl* mappen med [Visual Studio Code]eller valfri textredigerare.
   
    ![Öppna tjänstkodfilen för kontakter][open-contact-service-code-file]
4. Över hello kod i hello-filen med den här nya koden tooadd en fingerad implementering toohello service-kod. 
   
        package io.swagger.api.impl;
   
        import io.swagger.api.*;
        
        import io.swagger.model.Contact;
        import java.util.*;
        import io.swagger.api.NotFoundException;
               
        import javax.ws.rs.core.Response;
        import javax.ws.rs.core.SecurityContext;
   
        @javax.annotation.Generated(value = "class io.swagger.codegen.languages.JaxRSServerCodegen", date = "2015-11-24T21:54:11.648Z")
        public class ContactsApiServiceImpl extends ContactsApiService {
   
            private ArrayList<Contact> loadContacts()
            {
                ArrayList<Contact> list = new ArrayList<Contact>();
                list.add(new Contact(1, "Barney Poland", "barney@contoso.com"));
                list.add(new Contact(2, "Lacy Barrera", "lacy@contoso.com"));
                list.add(new Contact(3, "Lora Riggs", "lora@contoso.com"));
                return list;
            }
   
            @Override
            public Response contactsGet(SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                return Response.ok().entity(list).build();
                }
   
            @Override
            public Response contactsGetById(Integer id, SecurityContext securityContext)
            throws NotFoundException {
                ArrayList<Contact> list = loadContacts();
                Contact ret = null;
   
                for(int i=0; i<list.size(); i++)
                {
                    if(list.get(i).getId() == id)
                        {
                            ret = list.get(i);
                        }
                }
                return Response.ok().entity(ret).build();
            }
        }
5. Öppna en kommandotolk och ändra katalogen toohello rotmappen för ditt program.
6. Kör följande Maven-kommando toobuild hello kod hello och kör den med hjälp av hello Jetty-appservern lokalt. 
   
        mvn package jetty:run
7. Du bör se hello kommandofönstret att Jetty har startat koden på port 8080. 
   
    ![Öppna tjänstkodfilen för kontakter][run-jetty-war]
8. Använd [Postman] toomake en begäran toohello ”hämta alla kontakter” API-metoden på http://localhost: 8080/api/contacts.
   
    ![Anropa hello API för kontakter][calling-contacts-api]
9. Använd [Postman] toomake en begäran toohello ”hämta specifika kontakter” API-metoden finns på http://localhost: 8080/api/contacts/2.
   
    ![Anropa hello API för kontakter][calling-specific-contact-api]
10. Slutligen skapa hello Java WAR (webbarkiv)-filen genom att köra följande Maven-kommando i konsolen hello. 
    
         mvn package war:war
11. När hello WAR-filen har skapats placeras den i hello **mål** mapp. Navigera till hello **mål** mappen och Byt namn hello WAR-filen för**ROOT.war**. (Kontrollera att hello skiftläget matchar detta format).
    
          rename swagger-jaxrs-server-1.0.0.war ROOT.war
12. Kör följande kommandon från hello rotmappen för ditt program toocreate hello slutligen en **distribuera** mappen toouse toodeploy hello WAR-filen tooAzure. 
    
          mkdir deploy
          mkdir deploy\webapps
          copy target\ROOT.war deploy\webapps
          cd deploy

## <a name="publish-hello-output-tooazure-app-service"></a>Publicera hello utdata tooAzure Apptjänst
I det här avsnittet lär du dig hur toocreate en ny API-App med hello Azure-portalen, förbereder API-appen för Java-program och distribuera hello nyligen skapade WAR-filen tooAzure Apptjänst toorun din nya API-App. 

1. Skapa en ny API-app i hello [Azure-portalen], genom att klicka på hello **Ny -> webb + mobil > API-app** menyalternativet, ange information om dina appar och sedan klicka på **skapa**.
   
    ![Skapa en ny API-app][create-api-app]
2. När din API-app har skapats, öppna appens **inställningar** bladet och klicka sedan på hello **programinställningar** menyalternativet. Välj hello senaste Java-versionerna från hello tillgängliga alternativ, och sedan väljer hello senaste Tomcat från hello **webbehållaren** -menyn och klicka sedan på **spara**.
   
    ![Ställa in Java i hello bladet för API-App][set-up-java]
3. Klicka på hello **distributionsbehörigheterna** inställningsmenyn och ange ett användarnamn och lösenord som du vill toouse för att publicera filer tooyour API-App. 
   
    ![Ange autentiseringsuppgifter för distribution][deployment-credentials]
4. Klicka på hello **distributionskälla** menyalternativet för inställningar. När det, klickar du på hello **Välj källa** knappen, Välj hello **lokal Git-lagringsplats** alternativ och klickar sedan på **OK**. Detta skapar en Git-lagringsplats som körs i Azure och som är kopplad till din API-app. Varje gång du kopplar kod toohello *master* grenen av Git-lagringsplatsen koden kommer att publiceras till din live körs API-App-instans. 
   
    ![Skapa en ny lokal Git-lagringsplats][select-git-repo]
5. Kopiera hello nya Git-lagringsplatsen URL tooyour Urklipp. Spara den eftersom den behövs om en stund. 
   
    ![Skapa en ny Git-lagringsplats för din app][copy-git-repo-url]
6. Git push hello WAR-filen toohello online-lagringsplats. toodo detta, navigera till hello **distribuera** mapp som du skapade tidigare så att du enkelt kan koppla hello koden upp toohello lagringsplats som körs i din App Service. När du arbetar i konsolfönstret hello och navigerat till hello mapp där hello webbappens mapp finns, utfärda hello följande Git-kommandon toolaunch hello processen och utlösa en distribution. 
   
         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master
   
    När du skickar hello **push** begäran, blir du ombedd hello-lösenord som du skapade tidigare för hello distribution av autentiseringsuppgifter. Du bör se din portal skärm som hello uppdateringen har distribuerats när du har angett dina autentiseringsuppgifter.
7. Om du använder Postman igen för toohit hello nyligen distribuerade API-App körs i Azure App Service, ser du att hello beteendet är konsekvent och att nu den returnerar kontaktdata som förväntat med hjälp av enkel kod ändringar toohello Swagger.io Autogenererad Java-kod. 
   
    ![Använda REST API för Java kontakter live i Azure][postman-calling-azure-contacts]

## <a name="next-steps"></a>Nästa steg
I den här artikeln har kan toostart med en Swagger JSON-fil och vissa autogenererade Java-kod som hämtats från hello-redigeraren Swagger.io. Därifrån gjorde dina enkla ändringar och en Git-distributionsprocess att du skapade en fungerande API-app skriven i Java. hello nästa kursen visar hur för[använder API apps från JavaScript-klienter med hjälp av CORS][App Service API CORS]. Senare självstudiekurser i hello serien visar hur tooimplement autentisering och auktorisering.

toobuild på det här exemplet kan du läsa mer om hello [lagrings-SDK för Java] toopersist hello JSON-blobbar. Du kan använda hello [dokumentet DB Java SDK] toosave din kontakt data tooAzure dokumentet DB. 

<a name="see-also"></a>

## <a name="see-also"></a>Se även
Mer information om hur du använder Azure med Java finns i [Azure för Java-utvecklare](/java/azure).

<!-- URL List -->

[App Service API CORS]: app-service-api-cors-consume-javascript.md
[Azure-portalen]: https://portal.azure.com/
[dokumentet DB Java SDK]: ../documentdb/documentdb-java-application.md
[kostnadsfri utvärderingsversion]: https://azure.microsoft.com/pricing/free-trial/
[Git]: http://www.git-scm.com/
[Azure Java Developer Center]: /develop/java/
[Java Developer's Kit 8]: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html
[Jax-RS]: https://jax-rs-spec.java.net/
[Maven]: https://maven.apache.org/
[Microsoft Azure]: https://azure.microsoft.com/
[Online Swagger Editor]: http://editor2.swagger.io/
[Postman]: https://www.getpostman.com/
[lagrings-SDK för Java]:../storage/blobs/storage-java-how-to-use-blob-storage.md
[Swagger]: http://swagger.io/
[Swagger Editor]: http://editor.swagger.io/
[Visual Studio Code]: https://code.visualstudio.com

<!-- IMG List -->

[paste-json]: ./media/app-service-api-java-api-app/paste-json.png
[pasted-swagger]: ./media/app-service-api-java-api-app/pasted-swagger.png
[view-swagger-generated-docs]: ./media/app-service-api-java-api-app/view-swagger-generated-docs.png
[generate-code-menu-item]: ./media/app-service-api-java-api-app/generate-code-menu-item.png
[open-contact-model-file]: ./media/app-service-api-java-api-app/open-contact-model-file.png
[open-contact-service-code-file]: ./media/app-service-api-java-api-app/open-contact-service-code-file.png
[run-jetty-war]: ./media/app-service-api-java-api-app/run-jetty-war.png
[calling-contacts-api]: ./media/app-service-api-java-api-app/calling-contacts-api.png
[calling-specific-contact-api]: ./media/app-service-api-java-api-app/calling-specific-contact-api.png
[create-api-app]: ./media/app-service-api-java-api-app/create-api-app.png
[set-up-java]: ./media/app-service-api-java-api-app/set-up-java.png
[deployment-credentials]: ./media/app-service-api-java-api-app/deployment-credentials.png
[select-git-repo]: ./media/app-service-api-java-api-app/select-git-repo.png
[copy-git-repo-url]: ./media/app-service-api-java-api-app/copy-git-repo-url.png
[postman-calling-azure-contacts]: ./media/app-service-api-java-api-app/postman-calling-azure-contacts.png
