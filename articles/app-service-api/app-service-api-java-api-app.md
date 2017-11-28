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
# <a name="build-and-deploy-a-java-api-app-in-azure-app-service"></a><span data-ttu-id="43a02-103">Skapa och distribuera en Java API-app i Azure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="43a02-103">Build and deploy a Java API app in Azure App Service</span></span>
[!INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

<span data-ttu-id="43a02-104">Den här kursen visar hur toocreate ett Java-program och distribuerar den tooAzure API Apps i Apptjänst med hjälp av [Git].</span><span class="sxs-lookup"><span data-stu-id="43a02-104">This tutorial shows how toocreate a Java application and deploy it tooAzure App Service API Apps using [Git].</span></span> <span data-ttu-id="43a02-105">hello anvisningarna i den här kursen kan tillämpas på alla operativsystem som kan köra Java.</span><span class="sxs-lookup"><span data-stu-id="43a02-105">hello instructions in this tutorial can be followed on any operating system that is capable of running Java.</span></span> <span data-ttu-id="43a02-106">hello koden i självstudierna byggs med [Maven].</span><span class="sxs-lookup"><span data-stu-id="43a02-106">hello code in this tutorial is built using [Maven].</span></span> <span data-ttu-id="43a02-107">[Jax-RS] är används toocreate hello RESTful-tjänst och genereras baserat på hello [Swagger] metadata specifikationen med hello [Swagger Editor].</span><span class="sxs-lookup"><span data-stu-id="43a02-107">[Jax-RS] is used toocreate hello RESTful Service, and is generated based on hello [Swagger] metadata specification using hello [Swagger Editor].</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43a02-108">Krav</span><span class="sxs-lookup"><span data-stu-id="43a02-108">Prerequisites</span></span>
1. <span data-ttu-id="43a02-109">[Java Developer's Kit 8] \(eller senare)</span><span class="sxs-lookup"><span data-stu-id="43a02-109">[Java Developer's Kit 8] \(or later)</span></span>
2. <span data-ttu-id="43a02-110">[Maven] installerat på utvecklingsdatorn</span><span class="sxs-lookup"><span data-stu-id="43a02-110">[Maven] installed on your development machine</span></span>
3. <span data-ttu-id="43a02-111">[Git] installerat på utvecklingsdatorn</span><span class="sxs-lookup"><span data-stu-id="43a02-111">[Git] installed on your development machine</span></span>
4. <span data-ttu-id="43a02-112">En betald eller [kostnadsfri utvärderingsversion] prenumerationen för[Microsoft Azure]</span><span class="sxs-lookup"><span data-stu-id="43a02-112">A paid or [free trial] subscription too[Microsoft Azure]</span></span>
5. <span data-ttu-id="43a02-113">Ett program för HTTP-test som [Postman]</span><span class="sxs-lookup"><span data-stu-id="43a02-113">An HTTP test application like [Postman]</span></span>

## <a name="scaffold-hello-api-using-swaggerio"></a><span data-ttu-id="43a02-114">Kodskelett hello API: et med Swagger.IO</span><span class="sxs-lookup"><span data-stu-id="43a02-114">Scaffold hello API using Swagger.IO</span></span>
<span data-ttu-id="43a02-115">Med Redigeraren för hello swagger.io online kan ange du Swagger JSON- eller YAML-kod som representerar hello strukturen för din API.</span><span class="sxs-lookup"><span data-stu-id="43a02-115">Using hello swagger.io online editor, you can enter Swagger JSON or YAML code representing hello structure of your API.</span></span> <span data-ttu-id="43a02-116">När du har hello API-ytan utformats kan exportera du kod för olika plattformar och ramverk.</span><span class="sxs-lookup"><span data-stu-id="43a02-116">Once you have hello API surface area designed, you can export code for a variety of platforms and frameworks.</span></span> <span data-ttu-id="43a02-117">I nästa avsnitt om hello blir hello Autogenererad kod ändrade tooinclude fingerade funktioner.</span><span class="sxs-lookup"><span data-stu-id="43a02-117">In hello next section, hello scaffolded code will be modified tooinclude mock functionality.</span></span> 

<span data-ttu-id="43a02-118">Den här demonstrationen börjar med en Swagger JSON-text som du ska klistra in i redigeraren swagger.io för hello som kommer sedan att använda toogenerate kod genom att utnyttja JAX-RS tooaccess REST API-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="43a02-118">This demonstration will begin with a Swagger JSON body that you will paste into hello swagger.io editor, which will then be used toogenerate code making use of JAX-RS tooaccess a REST API endpoint.</span></span> <span data-ttu-id="43a02-119">Sedan redigerar du hello Autogenererad kod tooreturn fingerade data genom simulera en REST-API som byggts ovanpå en mekanism för datapersistence.</span><span class="sxs-lookup"><span data-stu-id="43a02-119">Then, you'll edit hello scaffolded code tooreturn mock data, simulating a REST API built atop a data persistence mechanism.</span></span>  

1. <span data-ttu-id="43a02-120">Kopiera följande Swagger JSON-kod tooyour Urklipp hello:</span><span class="sxs-lookup"><span data-stu-id="43a02-120">Copy hello following Swagger JSON code tooyour clipboard:</span></span>
   
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
2. <span data-ttu-id="43a02-121">Navigera toohello [Online Swagger Editor].</span><span class="sxs-lookup"><span data-stu-id="43a02-121">Navigate toohello [Online Swagger Editor].</span></span> <span data-ttu-id="43a02-122">När det, klickar du på hello **klistra in JSON-fil >** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="43a02-122">Once there, click hello **File -> Paste JSON** menu item.</span></span>
   
    ![Menyalternativet Klistra in JSON][paste-json]
3. <span data-ttu-id="43a02-124">Klistra in i hello kontakter listan API Swagger JSON du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="43a02-124">Paste in hello Contacts List API Swagger JSON you copied earlier.</span></span> 
   
    ![Klistra in JSON-kod i Swagger][pasted-swagger]
4. <span data-ttu-id="43a02-126">Visa hello dokumentationssidorna och API-översikten som visas i hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="43a02-126">View hello documentation pages and API summary rendered in hello editor.</span></span> 
   
    ![Visa Swagger-genererade dokument][view-swagger-generated-docs]
5. <span data-ttu-id="43a02-128">Välj hello **generera Server -> JAX-RS** menyn alternativet tooscaffold hello serverkod som du ska redigera senare tooadd fingerad implementering.</span><span class="sxs-lookup"><span data-stu-id="43a02-128">Select hello **Generate Server -> JAX-RS** menu option tooscaffold hello server-side code you'll edit later tooadd mock implementation.</span></span> 
   
    ![Menyalternativet Generera kod][generate-code-menu-item]
   
    <span data-ttu-id="43a02-130">När hello koden genereras, ska du angett toodownload en ZIP-filen.</span><span class="sxs-lookup"><span data-stu-id="43a02-130">Once hello code is generated, you'll be provided a ZIP file toodownload.</span></span> <span data-ttu-id="43a02-131">Den här filen innehåller hello koden som skapats av kodgeneratorn Swagger för hello och alla associerade byggnadsskript.</span><span class="sxs-lookup"><span data-stu-id="43a02-131">This file contains hello code scaffolded by hello Swagger code generator and all associated build scripts.</span></span> <span data-ttu-id="43a02-132">Packa upp hello hela biblioteket tooa katalog på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="43a02-132">Unzip hello entire library tooa directory on your development workstation.</span></span> 

## <a name="edit-hello-code-tooadd-api-implementation"></a><span data-ttu-id="43a02-133">Redigera hello kod tooadd API-implementering</span><span class="sxs-lookup"><span data-stu-id="43a02-133">Edit hello Code tooadd API Implementation</span></span>
<span data-ttu-id="43a02-134">I det här avsnittet ska du ersätta hello genererade Swagger-kodens implementering för serversidan med din anpassade kod.</span><span class="sxs-lookup"><span data-stu-id="43a02-134">In this section, you'll replace hello Swagger-generated code's server-side implementation with your custom code.</span></span> <span data-ttu-id="43a02-135">hello nya koden returnerar en ArrayList kontakta entiteter toohello uppringande klient.</span><span class="sxs-lookup"><span data-stu-id="43a02-135">hello new code will return an ArrayList of Contact entities toohello calling client.</span></span> 

1. <span data-ttu-id="43a02-136">Öppna hello *Contact.java* modellfil som finns i hello *src/gen/java/io/swagger/model* mappen med [Visual Studio Code] eller valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="43a02-136">Open hello *Contact.java* model file, which is located in hello *src/gen/java/io/swagger/model* folder, using [Visual Studio Code] or your favorite text editor.</span></span> 
   
    ![Öppna modellfilen för kontakter][open-contact-model-file]
2. <span data-ttu-id="43a02-138">Lägg till följande konstruktor inom hello hello **Kontakta** klass.</span><span class="sxs-lookup"><span data-stu-id="43a02-138">Add hello following constructor within hello **Contact** class.</span></span> 
   
        public Contact(Integer id, String name, String email) 
        {
            this.id = id;
            this.name = name;
            this.emailAddress = email;
        }
3. <span data-ttu-id="43a02-139">Öppna hello *Contact.Java* tjänstimplementeringsfilen som finns i hello *src/main/java/io/swagger/api/impl* mappen med [Visual Studio Code]eller valfri textredigerare.</span><span class="sxs-lookup"><span data-stu-id="43a02-139">Open hello *ContactsApiServiceImpl.java* service implementation file, which is located in hello *src/main/java/io/swagger/api/impl* folder, using [Visual Studio Code] or your favorite text editor.</span></span>
   
    ![Öppna tjänstkodfilen för kontakter][open-contact-service-code-file]
4. <span data-ttu-id="43a02-141">Över hello kod i hello-filen med den här nya koden tooadd en fingerad implementering toohello service-kod.</span><span class="sxs-lookup"><span data-stu-id="43a02-141">Overwrite hello code in hello file with this new code tooadd a mock implementation toohello service code.</span></span> 
   
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
5. <span data-ttu-id="43a02-142">Öppna en kommandotolk och ändra katalogen toohello rotmappen för ditt program.</span><span class="sxs-lookup"><span data-stu-id="43a02-142">Open a command prompt and change directory toohello root folder of your application.</span></span>
6. <span data-ttu-id="43a02-143">Kör följande Maven-kommando toobuild hello kod hello och kör den med hjälp av hello Jetty-appservern lokalt.</span><span class="sxs-lookup"><span data-stu-id="43a02-143">Execute hello following Maven command toobuild hello code and run it using hello Jetty app server locally.</span></span> 
   
        mvn package jetty:run
7. <span data-ttu-id="43a02-144">Du bör se hello kommandofönstret att Jetty har startat koden på port 8080.</span><span class="sxs-lookup"><span data-stu-id="43a02-144">You should see hello command window reflect that Jetty has started your code on port 8080.</span></span> 
   
    ![Öppna tjänstkodfilen för kontakter][run-jetty-war]
8. <span data-ttu-id="43a02-146">Använd [Postman] toomake en begäran toohello ”hämta alla kontakter” API-metoden på http://localhost: 8080/api/contacts.</span><span class="sxs-lookup"><span data-stu-id="43a02-146">Use [Postman] toomake a request toohello "get all contacts" API method at http://localhost:8080/api/contacts.</span></span>
   
    ![Anropa hello API för kontakter][calling-contacts-api]
9. <span data-ttu-id="43a02-148">Använd [Postman] toomake en begäran toohello ”hämta specifika kontakter” API-metoden finns på http://localhost: 8080/api/contacts/2.</span><span class="sxs-lookup"><span data-stu-id="43a02-148">Use [Postman] toomake a request toohello "get specific contact" API method located at http://localhost:8080/api/contacts/2.</span></span>
   
    ![Anropa hello API för kontakter][calling-specific-contact-api]
10. <span data-ttu-id="43a02-150">Slutligen skapa hello Java WAR (webbarkiv)-filen genom att köra följande Maven-kommando i konsolen hello.</span><span class="sxs-lookup"><span data-stu-id="43a02-150">Finally, build hello Java WAR (Web ARchive) file by executing hello following Maven command in your console.</span></span> 
    
         mvn package war:war
11. <span data-ttu-id="43a02-151">När hello WAR-filen har skapats placeras den i hello **mål** mapp.</span><span class="sxs-lookup"><span data-stu-id="43a02-151">Once hello WAR file is built, it will be placed into hello **target** folder.</span></span> <span data-ttu-id="43a02-152">Navigera till hello **mål** mappen och Byt namn hello WAR-filen för**ROOT.war**.</span><span class="sxs-lookup"><span data-stu-id="43a02-152">Navigate into hello **target** folder and rename hello WAR file too**ROOT.war**.</span></span> <span data-ttu-id="43a02-153">(Kontrollera att hello skiftläget matchar detta format).</span><span class="sxs-lookup"><span data-stu-id="43a02-153">(Make sure hello capitalization matches this format).</span></span>
    
          rename swagger-jaxrs-server-1.0.0.war ROOT.war
12. <span data-ttu-id="43a02-154">Kör följande kommandon från hello rotmappen för ditt program toocreate hello slutligen en **distribuera** mappen toouse toodeploy hello WAR-filen tooAzure.</span><span class="sxs-lookup"><span data-stu-id="43a02-154">Finally, execute hello following commands from hello root folder of your application toocreate a **deploy** folder toouse toodeploy hello WAR file tooAzure.</span></span> 
    
          mkdir deploy
          mkdir deploy\webapps
          copy target\ROOT.war deploy\webapps
          cd deploy

## <a name="publish-hello-output-tooazure-app-service"></a><span data-ttu-id="43a02-155">Publicera hello utdata tooAzure Apptjänst</span><span class="sxs-lookup"><span data-stu-id="43a02-155">Publish hello output tooAzure App Service</span></span>
<span data-ttu-id="43a02-156">I det här avsnittet lär du dig hur toocreate en ny API-App med hello Azure-portalen, förbereder API-appen för Java-program och distribuera hello nyligen skapade WAR-filen tooAzure Apptjänst toorun din nya API-App.</span><span class="sxs-lookup"><span data-stu-id="43a02-156">In this section you'll learn how toocreate a new API App using hello Azure Portal, prepare that API App for hosting Java applications, and deploy hello newly-created WAR file tooAzure App Service toorun your new API App.</span></span> 

1. <span data-ttu-id="43a02-157">Skapa en ny API-app i hello [Azure-portalen], genom att klicka på hello **Ny -> webb + mobil > API-app** menyalternativet, ange information om dina appar och sedan klicka på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="43a02-157">Create a new API app in hello [Azure portal], by clicking hello **New -> Web + Mobile -> API app** menu item, entering your app details, and then clicking **Create**.</span></span>
   
    ![Skapa en ny API-app][create-api-app]
2. <span data-ttu-id="43a02-159">När din API-app har skapats, öppna appens **inställningar** bladet och klicka sedan på hello **programinställningar** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="43a02-159">Once your API app has been created, open your app's **Settings** blade, and then click hello **Application settings** menu item.</span></span> <span data-ttu-id="43a02-160">Välj hello senaste Java-versionerna från hello tillgängliga alternativ, och sedan väljer hello senaste Tomcat från hello **webbehållaren** -menyn och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="43a02-160">Select hello latest Java versions from hello available options, then select hello latest Tomcat from hello **Web container** menu, and then click **Save**.</span></span>
   
    ![Ställa in Java i hello bladet för API-App][set-up-java]
3. <span data-ttu-id="43a02-162">Klicka på hello **distributionsbehörigheterna** inställningsmenyn och ange ett användarnamn och lösenord som du vill toouse för att publicera filer tooyour API-App.</span><span class="sxs-lookup"><span data-stu-id="43a02-162">Click hello **Deployment credentials** settings menu item, and provide a username and password you wish toouse for publishing files tooyour API App.</span></span> 
   
    ![Ange autentiseringsuppgifter för distribution][deployment-credentials]
4. <span data-ttu-id="43a02-164">Klicka på hello **distributionskälla** menyalternativet för inställningar.</span><span class="sxs-lookup"><span data-stu-id="43a02-164">Click hello **Deployment source** settings menu item.</span></span> <span data-ttu-id="43a02-165">När det, klickar du på hello **Välj källa** knappen, Välj hello **lokal Git-lagringsplats** alternativ och klickar sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="43a02-165">Once there, click hello **Choose source** button, select hello **Local Git Repository** option, and then click **OK**.</span></span> <span data-ttu-id="43a02-166">Detta skapar en Git-lagringsplats som körs i Azure och som är kopplad till din API-app.</span><span class="sxs-lookup"><span data-stu-id="43a02-166">This will create a Git repository running in Azure, that has an association with your API App.</span></span> <span data-ttu-id="43a02-167">Varje gång du kopplar kod toohello *master* grenen av Git-lagringsplatsen koden kommer att publiceras till din live körs API-App-instans.</span><span class="sxs-lookup"><span data-stu-id="43a02-167">Each time you commit code toohello *master* branch of your Git repository, your code will be published into your live running API App instance.</span></span> 
   
    ![Skapa en ny lokal Git-lagringsplats][select-git-repo]
5. <span data-ttu-id="43a02-169">Kopiera hello nya Git-lagringsplatsen URL tooyour Urklipp.</span><span class="sxs-lookup"><span data-stu-id="43a02-169">Copy hello new Git repository's URL tooyour clipboard.</span></span> <span data-ttu-id="43a02-170">Spara den eftersom den behövs om en stund.</span><span class="sxs-lookup"><span data-stu-id="43a02-170">Save this as it will be important in a moment.</span></span> 
   
    ![Skapa en ny Git-lagringsplats för din app][copy-git-repo-url]
6. <span data-ttu-id="43a02-172">Git push hello WAR-filen toohello online-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="43a02-172">Git push hello WAR file toohello online repository.</span></span> <span data-ttu-id="43a02-173">toodo detta, navigera till hello **distribuera** mapp som du skapade tidigare så att du enkelt kan koppla hello koden upp toohello lagringsplats som körs i din App Service.</span><span class="sxs-lookup"><span data-stu-id="43a02-173">toodo this, navigate into hello **deploy** folder you created earlier so that you can easily commit hello code up toohello repository running in your App Service.</span></span> <span data-ttu-id="43a02-174">När du arbetar i konsolfönstret hello och navigerat till hello mapp där hello webbappens mapp finns, utfärda hello följande Git-kommandon toolaunch hello processen och utlösa en distribution.</span><span class="sxs-lookup"><span data-stu-id="43a02-174">Once you're in hello console window and navigated into hello folder where hello webapps folder is located, issue hello following Git commands toolaunch hello process and fire off a deployment.</span></span> 
   
         git init
         git add .
         git commit -m "initial commit"
         git remote add azure [YOUR GIT URL]
         git push azure master
   
    <span data-ttu-id="43a02-175">När du skickar hello **push** begäran, blir du ombedd hello-lösenord som du skapade tidigare för hello distribution av autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="43a02-175">Once you issue hello **push** request, you'll be asked for hello password you created for hello deployment credential earlier.</span></span> <span data-ttu-id="43a02-176">Du bör se din portal skärm som hello uppdateringen har distribuerats när du har angett dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="43a02-176">After you enter your credentials, you should see your portal display that hello update was deployed.</span></span>
7. <span data-ttu-id="43a02-177">Om du använder Postman igen för toohit hello nyligen distribuerade API-App körs i Azure App Service, ser du att hello beteendet är konsekvent och att nu den returnerar kontaktdata som förväntat med hjälp av enkel kod ändringar toohello Swagger.io Autogenererad Java-kod.</span><span class="sxs-lookup"><span data-stu-id="43a02-177">If you once again use Postman toohit hello newly-deployed API App running in Azure App Service, you'll see that hello behavior is consistent and that now it is returning contact data as expected, and using simple code changes toohello Swagger.io scaffolded Java code.</span></span> 
   
    ![Använda REST API för Java kontakter live i Azure][postman-calling-azure-contacts]

## <a name="next-steps"></a><span data-ttu-id="43a02-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="43a02-179">Next steps</span></span>
<span data-ttu-id="43a02-180">I den här artikeln har kan toostart med en Swagger JSON-fil och vissa autogenererade Java-kod som hämtats från hello-redigeraren Swagger.io.</span><span class="sxs-lookup"><span data-stu-id="43a02-180">In this article, you were able toostart with a Swagger JSON file and some scaffolded Java code obtained from hello Swagger.io editor.</span></span> <span data-ttu-id="43a02-181">Därifrån gjorde dina enkla ändringar och en Git-distributionsprocess att du skapade en fungerande API-app skriven i Java.</span><span class="sxs-lookup"><span data-stu-id="43a02-181">From there, your simple changes and a Git deploy process resulted in having a functional API app written in Java.</span></span> <span data-ttu-id="43a02-182">hello nästa kursen visar hur för[använder API apps från JavaScript-klienter med hjälp av CORS][App Service API CORS].</span><span class="sxs-lookup"><span data-stu-id="43a02-182">hello next tutorial shows how too[consume API apps from JavaScript clients, using CORS][App Service API CORS].</span></span> <span data-ttu-id="43a02-183">Senare självstudiekurser i hello serien visar hur tooimplement autentisering och auktorisering.</span><span class="sxs-lookup"><span data-stu-id="43a02-183">Later tutorials in hello series show how tooimplement authentication and authorization.</span></span>

<span data-ttu-id="43a02-184">toobuild på det här exemplet kan du läsa mer om hello [lagrings-SDK för Java] toopersist hello JSON-blobbar.</span><span class="sxs-lookup"><span data-stu-id="43a02-184">toobuild on this sample, you can learn more about hello [Storage SDK for Java] toopersist hello JSON blobs.</span></span> <span data-ttu-id="43a02-185">Du kan använda hello [dokumentet DB Java SDK] toosave din kontakt data tooAzure dokumentet DB.</span><span class="sxs-lookup"><span data-stu-id="43a02-185">Or, you could use hello [Document DB Java SDK] toosave your Contact data tooAzure Document DB.</span></span> 

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="43a02-186">Se även</span><span class="sxs-lookup"><span data-stu-id="43a02-186">See Also</span></span>
<span data-ttu-id="43a02-187">Mer information om hur du använder Azure med Java finns i [Azure för Java-utvecklare](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="43a02-187">For more information about using Azure with Java, visit [Azure for Java developers](/java/azure).</span></span>

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
