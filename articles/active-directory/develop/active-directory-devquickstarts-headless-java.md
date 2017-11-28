---
title: "aaaAzure AD Java-kommandoraden komma igång | Microsoft Docs"
description: "Hur kommandot toobuild en Java rad app som signerar användare i tooaccess en API."
services: active-directory
documentationcenter: java
author: navyasric
manager: mbaldwin
editor: 
ms.assetid: 51e1a8f9-6ff0-4643-a350-0ba794e26fd1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 01/23/2017
ms.author: nacanuma
ms.custom: aaddev
ms.openlocfilehash: 9ba1d1e794928a39ca1f091bd0e6eba57ce3d6aa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-java-command-line-app-tooaccess-an-api-with-azure-ad"></a><span data-ttu-id="f80bc-103">Med Java Kommandoradsapp tooAccess en API med Azure AD</span><span class="sxs-lookup"><span data-stu-id="f80bc-103">Using Java Command Line App tooAccess An API with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="f80bc-104">Azure AD gör det lätt toooutsource ditt webbprogram Identitetshantering att tillhandahålla enkel inloggning och utloggning med bara några få rader med kod.</span><span class="sxs-lookup"><span data-stu-id="f80bc-104">Azure AD makes it simple and straightforward toooutsource your web app's identity management, providing single sign-in and sign-out with only a few lines of code.</span></span>  <span data-ttu-id="f80bc-105">I Java-webbappar, kan du göra detta med hjälp av Microsofts implementering av hello community-driven ADAL4J.</span><span class="sxs-lookup"><span data-stu-id="f80bc-105">In Java web apps, you can accomplish this using Microsoft's implementation of hello community-driven ADAL4J.</span></span>

  <span data-ttu-id="f80bc-106">Vi använder här ADAL4J till:</span><span class="sxs-lookup"><span data-stu-id="f80bc-106">Here we'll use ADAL4J to:</span></span>

* <span data-ttu-id="f80bc-107">Logga hello användare in hello app med Azure AD som hello identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="f80bc-107">Sign hello user into hello app using Azure AD as hello identity provider.</span></span>
* <span data-ttu-id="f80bc-108">Visa information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="f80bc-108">Display some information about hello user.</span></span>
* <span data-ttu-id="f80bc-109">Logga hello användare utanför hello appen.</span><span class="sxs-lookup"><span data-stu-id="f80bc-109">Sign hello user out of hello app.</span></span>

<span data-ttu-id="f80bc-110">I order toodo detta, måste du:</span><span class="sxs-lookup"><span data-stu-id="f80bc-110">In order toodo this, you'll need to:</span></span>

1. <span data-ttu-id="f80bc-111">Registrera ett program med Azure AD</span><span class="sxs-lookup"><span data-stu-id="f80bc-111">Register an application with Azure AD</span></span>
2. <span data-ttu-id="f80bc-112">Konfigurera biblioteket app toouse hello ADAL4J.</span><span class="sxs-lookup"><span data-stu-id="f80bc-112">Set up your app toouse hello ADAL4J library.</span></span>
3. <span data-ttu-id="f80bc-113">Använd hello ADAL4J biblioteket tooissue inloggning och utloggning begäranden tooAzure AD.</span><span class="sxs-lookup"><span data-stu-id="f80bc-113">Use hello ADAL4J library tooissue sign-in and sign-out requests tooAzure AD.</span></span>
4. <span data-ttu-id="f80bc-114">Skriva ut data om hello användare.</span><span class="sxs-lookup"><span data-stu-id="f80bc-114">Print out data about hello user.</span></span>

<span data-ttu-id="f80bc-115">tooget har startats [hämta hello app stommen](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) eller [hämta hello slutförts exempel](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="f80bc-115">tooget started, [download hello app skeleton](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) or [download hello completed sample](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span></span>  <span data-ttu-id="f80bc-116">Du måste också en Azure AD-klient i vilka tooregister ditt program.</span><span class="sxs-lookup"><span data-stu-id="f80bc-116">You'll also need an Azure AD tenant in which tooregister your application.</span></span>  <span data-ttu-id="f80bc-117">Om du inte redan har en [Lär dig hur tooget en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="f80bc-117">If you don't have one already, [learn how tooget one](active-directory-howto-tenant.md).</span></span>

## <a name="1--register-an-application-with-azure-ad"></a><span data-ttu-id="f80bc-118">1.  Registrera ett program med Azure AD</span><span class="sxs-lookup"><span data-stu-id="f80bc-118">1.  Register an Application with Azure AD</span></span>
<span data-ttu-id="f80bc-119">tooenable tooauthenticate användare för din app behöver du tooregister ett nytt program i din klient.</span><span class="sxs-lookup"><span data-stu-id="f80bc-119">tooenable your app tooauthenticate users, you'll first need tooregister a new application in your tenant.</span></span>

1. <span data-ttu-id="f80bc-120">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f80bc-120">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f80bc-121">Hello översta, klicka på fältet för ditt konto och på hello **Directory** Välj plats tooregister programmet hello Active Directory-klient.</span><span class="sxs-lookup"><span data-stu-id="f80bc-121">On hello top bar, click on your account and under hello **Directory** list, choose hello Active Directory tenant where you wish tooregister your application.</span></span>
3. <span data-ttu-id="f80bc-122">Klicka på **fler tjänster** i hello vänstra nav och välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f80bc-122">Click on **More Services** in hello left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="f80bc-123">Klicka på **App registreringar** och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f80bc-123">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="f80bc-124">Följ anvisningarna för hello och skapa en ny **webbprogram och/eller WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="f80bc-124">Follow hello prompts and create a new **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="f80bc-125">Hej **namn** av hello programmet beskriver dina program tooend-användare</span><span class="sxs-lookup"><span data-stu-id="f80bc-125">hello **name** of hello application will describe your application tooend-users</span></span>
  * <span data-ttu-id="f80bc-126">Hej **inloggnings-URL** är hello bas-URL för din app.</span><span class="sxs-lookup"><span data-stu-id="f80bc-126">hello **Sign-On URL** is hello base URL of your app.</span></span>  <span data-ttu-id="f80bc-127">hello stommen standardvärdet är `http://localhost:8080/adal4jsample/`.</span><span class="sxs-lookup"><span data-stu-id="f80bc-127">hello skeleton's default is `http://localhost:8080/adal4jsample/`.</span></span>
6. <span data-ttu-id="f80bc-128">När du har slutfört registreringen tilldelas AAD appen ett unikt program-ID.</span><span class="sxs-lookup"><span data-stu-id="f80bc-128">Once you've completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="f80bc-129">Du behöver det här värdet i nästa avsnitt hello så kopiera den från hello programfliken.</span><span class="sxs-lookup"><span data-stu-id="f80bc-129">You'll need this value in hello next sections, so copy it from hello application tab.</span></span>
7. <span data-ttu-id="f80bc-130">Från hello **inställningar** -> **egenskaper** för programmet, uppdatera hello App-ID-URI.</span><span class="sxs-lookup"><span data-stu-id="f80bc-130">From hello **Settings** -> **Properties** page for your application, update hello App ID URI.</span></span> <span data-ttu-id="f80bc-131">Hej **App-ID URI** är en unik identifierare för programmet.</span><span class="sxs-lookup"><span data-stu-id="f80bc-131">hello **App ID URI** is a unique identifier for your application.</span></span>  <span data-ttu-id="f80bc-132">hello konventionen är toouse `https://<tenant-domain>/<app-name>`, t.ex. `http://localhost:8080/adal4jsample/`.</span><span class="sxs-lookup"><span data-stu-id="f80bc-132">hello convention is toouse `https://<tenant-domain>/<app-name>`, e.g. `http://localhost:8080/adal4jsample/`.</span></span>

<span data-ttu-id="f80bc-133">En gång i hello portal för din app skapar en **nyckeln** från hello **inställningar** för programmet och kopiera ned.</span><span class="sxs-lookup"><span data-stu-id="f80bc-133">Once in hello portal for your app create a **Key** from hello **Settings** page for your application and copy it down.</span></span>  <span data-ttu-id="f80bc-134">Du behöver den snart.</span><span class="sxs-lookup"><span data-stu-id="f80bc-134">You will need it shortly.</span></span>

## <a name="2-set-up-your-app-toouse-adal4j-library-and-prerequisites-using-maven"></a><span data-ttu-id="f80bc-135">2. Konfigurera din app toouse ADAL4J bibliotek och förutsättningar med Maven</span><span class="sxs-lookup"><span data-stu-id="f80bc-135">2. Set up your app toouse ADAL4J library and prerequisites using Maven</span></span>
<span data-ttu-id="f80bc-136">Här kan konfigurerar vi ADAL4J toouse hello autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="f80bc-136">Here, we'll configure ADAL4J toouse hello OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="f80bc-137">ADAL4J kommer att använda tooissue inloggning och utloggningsförfrågningar, hantera hello användarens session och få information om hello användare, bland annat.</span><span class="sxs-lookup"><span data-stu-id="f80bc-137">ADAL4J will be used tooissue sign-in and sign-out requests, manage hello user's session, and get information about hello user, amongst other things.</span></span>

* <span data-ttu-id="f80bc-138">I hello rotkatalogen för ditt projekt, öppna/skapa `pom.xml` och leta upp hello `// TODO: provide dependencies for Maven` och Ersätt med hello följande:</span><span class="sxs-lookup"><span data-stu-id="f80bc-138">In hello root directory of your project, open/create `pom.xml` and locate hello `// TODO: provide dependencies for Maven` and replace with hello following:</span></span>

```Java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>public-client-adal4j-sample</artifactId>
    <packaging>jar</packaging>
    <version>0.0.1-SNAPSHOT</version>
    <name>public-client-adal4j-sample</name>
    <url>http://maven.apache.org</url>
    <properties>
        <spring.version>3.0.5.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>adal4j</artifactId>
            <version>1.1.2</version>
        </dependency>
        <dependency>
            <groupId>com.nimbusds</groupId>
            <artifactId>oauth2-oidc-sdk</artifactId>
            <version>4.5</version>
        </dependency>
        <dependency>
            <groupId>org.json</groupId>
            <artifactId>json</artifactId>
            <version>20090211</version>
        </dependency>
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
        </dependency>
</dependencies>
    <build>
        <finalName>public-client-adal4j-sample</finalName>
        <plugins>
                <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <version>1.2.1</version>
            <configuration>
                <mainClass>PublicClient</mainClass>
            </configuration>
        </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>install</id>
                        <phase>install</phase>
                        <goals>
                            <goal>sources</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.5</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
        <artifactId>maven-assembly-plugin</artifactId>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
          </execution>
        </executions>
        <configuration>
          <descriptorRefs>
            <descriptorRef>jar-with-dependencies</descriptorRef>
          </descriptorRefs>
        </configuration>
      </plugin>
      <plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-assembly-plugin</artifactId>
  <configuration>
    <archive>
      <manifest>
        <mainClass>PublicClient</mainClass>
      </manifest>
    </archive>
  </configuration>
</plugin>
        </plugins>
    </build>

</project>


```



## <a name="3-create-hello-java-publicclient-file"></a><span data-ttu-id="f80bc-139">3. Skapa hello Java PublicClient fil</span><span class="sxs-lookup"><span data-stu-id="f80bc-139">3. Create hello Java PublicClient file</span></span>
<span data-ttu-id="f80bc-140">Enligt ovan, kommer vi att använda hello Graph API tooget data om hello inloggad användare.</span><span class="sxs-lookup"><span data-stu-id="f80bc-140">As indicated above, we will be using hello Graph API tooget data about hello logged in user.</span></span> <span data-ttu-id="f80bc-141">För den här enkelt för oss toobe ska vi skapa både en fil toorepresent en **katalogobjekt** och en enskild fil toorepresent hello **användaren** så att hello OO mönstret för Java kan användas.</span><span class="sxs-lookup"><span data-stu-id="f80bc-141">For this toobe easy for us we should create both a file toorepresent a **Directory Object** and an individual file toorepresent hello **User** so that hello OO pattern of Java can be used.</span></span>

* <span data-ttu-id="f80bc-142">Skapa en fil med namnet `DirectoryObject.java` som ska användas för toostore grundläggande information om alla DirectoryObject (du ledigt toouse detta senare för andra diagrammet frågor kan du göra).</span><span class="sxs-lookup"><span data-stu-id="f80bc-142">Create a file called `DirectoryObject.java` which we will use toostore basic data about any DirectoryObject (you can feel free toouse this later for any other Graph Queries you may do).</span></span> <span data-ttu-id="f80bc-143">Du kan klipp ut och klistra in det här nedan:</span><span class="sxs-lookup"><span data-stu-id="f80bc-143">You can cut/paste this from below:</span></span>

```Java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import javax.naming.ServiceUnavailableException;

import com.microsoft.aad.adal4j.AuthenticationContext;
import com.microsoft.aad.adal4j.AuthenticationResult;

public class PublicClient {

    private final static String AUTHORITY = "https://login.microsoftonline.com/common/";
    private final static String CLIENT_ID = "2a4da06c-ff07-410d-af8a-542a512f5092";

    public static void main(String args[]) throws Exception {

        try (BufferedReader br = new BufferedReader(new InputStreamReader(
                System.in))) {
            System.out.print("Enter username: ");
            String username = br.readLine();
            System.out.print("Enter password: ");
            String password = br.readLine();

            AuthenticationResult result = getAccessTokenFromUserCredentials(
                    username, password);
            System.out.println("Access Token - " + result.getAccessToken());
            System.out.println("Refresh Token - " + result.getRefreshToken());
            System.out.println("ID Token - " + result.getIdToken());
        }
    }

    private static AuthenticationResult getAccessTokenFromUserCredentials(
            String username, String password) throws Exception {
        AuthenticationContext context = null;
        AuthenticationResult result = null;
        ExecutorService service = null;
        try {
            service = Executors.newFixedThreadPool(1);
            context = new AuthenticationContext(AUTHORITY, false, service);
            Future<AuthenticationResult> future = context.acquireToken(
                    "https://graph.windows.net", CLIENT_ID, username, password,
                    null);
            result = future.get();
        } finally {
            service.shutdown();
        }

        if (result == null) {
            throw new ServiceUnavailableException(
                    "authentication result was null");
        }
        return result;
    }
}


```


## <a name="compile-and-run-hello-sample"></a><span data-ttu-id="f80bc-144">Kompilera och köra hello-exempel</span><span class="sxs-lookup"><span data-stu-id="f80bc-144">Compile and run hello sample</span></span>
<span data-ttu-id="f80bc-145">Ändra tillbaka ut tooyour rotkatalog och kör hello efter kommandot toobuild hello exempel du fört in tillsammans med `maven`.</span><span class="sxs-lookup"><span data-stu-id="f80bc-145">Change back out tooyour root directory and run hello following command toobuild hello sample you just put together using `maven`.</span></span> <span data-ttu-id="f80bc-146">Det här använder hello `pom.xml` fil som du skrev för beroenden.</span><span class="sxs-lookup"><span data-stu-id="f80bc-146">This will use hello `pom.xml` file you wrote for dependencies.</span></span>

`$ mvn package`

<span data-ttu-id="f80bc-147">Du bör nu ha en `adal4jsample.war` filen i din `/targets` directory.</span><span class="sxs-lookup"><span data-stu-id="f80bc-147">You should now have a `adal4jsample.war` file in your `/targets` directory.</span></span> <span data-ttu-id="f80bc-148">Du kan distribuera som i Tomcat-behållaren och besök hello URL</span><span class="sxs-lookup"><span data-stu-id="f80bc-148">You may deploy that in your Tomcat container and visit hello URL</span></span> 

`http://localhost:8080/adal4jsample/`

> [!NOTE]
> <span data-ttu-id="f80bc-149">Det är mycket enkelt toodeploy en WAR-fil med hello senaste Tomcat-servrar.</span><span class="sxs-lookup"><span data-stu-id="f80bc-149">It is very easy toodeploy a WAR with hello latest Tomcat servers.</span></span> <span data-ttu-id="f80bc-150">Helt enkelt navigera för`http://localhost:8080/manager/` och följer instruktionerna för hello på Överför din '' adal4jsample.war-filen.</span><span class="sxs-lookup"><span data-stu-id="f80bc-150">Simply navigate too`http://localhost:8080/manager/` and follow hello instructions on uploading your ``adal4jsample.war\` file.</span></span> <span data-ttu-id="f80bc-151">Det kommer autodeploy du med hello korrekt slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="f80bc-151">It will autodeploy for you with hello correct endpoint.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="f80bc-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f80bc-152">Next Steps</span></span>
<span data-ttu-id="f80bc-153">Grattis!</span><span class="sxs-lookup"><span data-stu-id="f80bc-153">Congratulations!</span></span> <span data-ttu-id="f80bc-154">Du nu ha en fungerande Java-program som har hello möjlighet tooauthenticate användare på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och hämta grundläggande information om hello användare.</span><span class="sxs-lookup"><span data-stu-id="f80bc-154">You now have a working Java application that has hello ability tooauthenticate users, securely call Web APIs using OAuth 2.0, and get basic information about hello user.</span></span>  <span data-ttu-id="f80bc-155">Om du inte redan gjort nu är hello tid toopopulate din klient med vissa användare.</span><span class="sxs-lookup"><span data-stu-id="f80bc-155">If you haven't already, now is hello time toopopulate your tenant with some users.</span></span>

<span data-ttu-id="f80bc-156">För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [har angetts som en .zip här](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), eller kan du klona den från GitHub:</span><span class="sxs-lookup"><span data-stu-id="f80bc-156">For reference, hello completed sample (without your configuration values) [is provided as a .zip here](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

