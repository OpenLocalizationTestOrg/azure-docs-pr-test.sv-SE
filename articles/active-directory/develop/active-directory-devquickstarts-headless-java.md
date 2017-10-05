---
title: "Azure AD-Java-kommandoraden komma igång | Microsoft Docs"
description: "Hur du skapar en Java-kommandoradsapp som loggar användarna in för att få åtkomst till en API."
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
ms.openlocfilehash: 91e4a7b2ac454465d5cce4948a4d5f0b542d2b55
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-java-command-line-app-to-access-an-api-with-azure-ad"></a><span data-ttu-id="017a8-103">Använder appen för Java-kommandoraden för att komma åt ett API med Azure AD</span><span class="sxs-lookup"><span data-stu-id="017a8-103">Using Java Command Line App To Access An API with Azure AD</span></span>
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

<span data-ttu-id="017a8-104">Azure AD gör det lätt att flytta ut ditt webbprogram för Identitetshantering, att tillhandahålla enkel inloggning och utloggning med bara några få rader med kod.</span><span class="sxs-lookup"><span data-stu-id="017a8-104">Azure AD makes it simple and straightforward to outsource your web app's identity management, providing single sign-in and sign-out with only a few lines of code.</span></span>  <span data-ttu-id="017a8-105">I Java-webbappar, kan du göra detta med hjälp av Microsofts implementering av community-driven ADAL4J.</span><span class="sxs-lookup"><span data-stu-id="017a8-105">In Java web apps, you can accomplish this using Microsoft's implementation of the community-driven ADAL4J.</span></span>

  <span data-ttu-id="017a8-106">Vi använder här ADAL4J till:</span><span class="sxs-lookup"><span data-stu-id="017a8-106">Here we'll use ADAL4J to:</span></span>

* <span data-ttu-id="017a8-107">Logga in användaren till appen med Azure AD som identitetsleverantören.</span><span class="sxs-lookup"><span data-stu-id="017a8-107">Sign the user into the app using Azure AD as the identity provider.</span></span>
* <span data-ttu-id="017a8-108">Visa information om användaren.</span><span class="sxs-lookup"><span data-stu-id="017a8-108">Display some information about the user.</span></span>
* <span data-ttu-id="017a8-109">Logga ut från appen användaren.</span><span class="sxs-lookup"><span data-stu-id="017a8-109">Sign the user out of the app.</span></span>

<span data-ttu-id="017a8-110">För att kunna göra detta måste du:</span><span class="sxs-lookup"><span data-stu-id="017a8-110">In order to do this, you'll need to:</span></span>

1. <span data-ttu-id="017a8-111">Registrera ett program med Azure AD</span><span class="sxs-lookup"><span data-stu-id="017a8-111">Register an application with Azure AD</span></span>
2. <span data-ttu-id="017a8-112">Konfigurera din app att använda ADAL4J-biblioteket.</span><span class="sxs-lookup"><span data-stu-id="017a8-112">Set up your app to use the ADAL4J library.</span></span>
3. <span data-ttu-id="017a8-113">Använd ADAL4J-biblioteket för att utfärda inloggnings- och utloggningsförfrågningar till Azure AD.</span><span class="sxs-lookup"><span data-stu-id="017a8-113">Use the ADAL4J library to issue sign-in and sign-out requests to Azure AD.</span></span>
4. <span data-ttu-id="017a8-114">Skriva ut data om användaren.</span><span class="sxs-lookup"><span data-stu-id="017a8-114">Print out data about the user.</span></span>

<span data-ttu-id="017a8-115">Du kommer igång [ladda ned stommen app](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) eller [hämta det slutförda exemplet](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span><span class="sxs-lookup"><span data-stu-id="017a8-115">To get started, [download the app skeleton](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) or [download the completed sample](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).</span></span>  <span data-ttu-id="017a8-116">Du måste också en Azure AD-klient att registrera ditt program.</span><span class="sxs-lookup"><span data-stu-id="017a8-116">You'll also need an Azure AD tenant in which to register your application.</span></span>  <span data-ttu-id="017a8-117">Om du inte redan har en [Lär dig hur du skaffa en](active-directory-howto-tenant.md).</span><span class="sxs-lookup"><span data-stu-id="017a8-117">If you don't have one already, [learn how to get one](active-directory-howto-tenant.md).</span></span>

## <a name="1--register-an-application-with-azure-ad"></a><span data-ttu-id="017a8-118">1.  Registrera ett program med Azure AD</span><span class="sxs-lookup"><span data-stu-id="017a8-118">1.  Register an Application with Azure AD</span></span>
<span data-ttu-id="017a8-119">Om du vill aktivera din app för att autentisera användare, behöver du först registrera ett nytt program i din klient.</span><span class="sxs-lookup"><span data-stu-id="017a8-119">To enable your app to authenticate users, you'll first need to register a new application in your tenant.</span></span>

1. <span data-ttu-id="017a8-120">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="017a8-120">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="017a8-121">Klicka på den översta raden på ditt konto och under den **Directory** Välj Active Directory-klient som du vill registrera ditt program.</span><span class="sxs-lookup"><span data-stu-id="017a8-121">On the top bar, click on your account and under the **Directory** list, choose the Active Directory tenant where you wish to register your application.</span></span>
3. <span data-ttu-id="017a8-122">Klicka på **fler tjänster** i den vänstra nav och välj **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="017a8-122">Click on **More Services** in the left hand nav, and choose **Azure Active Directory**.</span></span>
4. <span data-ttu-id="017a8-123">Klicka på **App registreringar** och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="017a8-123">Click on **App registrations** and choose **Add**.</span></span>
5. <span data-ttu-id="017a8-124">Följ anvisningarna och skapa en ny **webbprogram och/eller WebAPI**.</span><span class="sxs-lookup"><span data-stu-id="017a8-124">Follow the prompts and create a new **Web Application and/or WebAPI**.</span></span>
  * <span data-ttu-id="017a8-125">Den **namn** av programmet beskriva programmet till slutanvändare</span><span class="sxs-lookup"><span data-stu-id="017a8-125">The **name** of the application will describe your application to end-users</span></span>
  * <span data-ttu-id="017a8-126">Den **inloggnings-URL** är den grundläggande Webbadressen för din app.</span><span class="sxs-lookup"><span data-stu-id="017a8-126">The **Sign-On URL** is the base URL of your app.</span></span>  <span data-ttu-id="017a8-127">Den stommen standardvärdet är `http://localhost:8080/adal4jsample/`.</span><span class="sxs-lookup"><span data-stu-id="017a8-127">The skeleton's default is `http://localhost:8080/adal4jsample/`.</span></span>
6. <span data-ttu-id="017a8-128">När du har slutfört registreringen tilldelas AAD appen ett unikt program-ID.</span><span class="sxs-lookup"><span data-stu-id="017a8-128">Once you've completed registration, AAD will assign your app a unique Application ID.</span></span>  <span data-ttu-id="017a8-129">Du behöver det här värdet i nästa avsnitt så kopiera den från programfliken.</span><span class="sxs-lookup"><span data-stu-id="017a8-129">You'll need this value in the next sections, so copy it from the application tab.</span></span>
7. <span data-ttu-id="017a8-130">Från den **inställningar** -> **egenskaper** för programmet, uppdatera App-ID-URI.</span><span class="sxs-lookup"><span data-stu-id="017a8-130">From the **Settings** -> **Properties** page for your application, update the App ID URI.</span></span> <span data-ttu-id="017a8-131">Den **App-ID URI** är en unik identifierare för programmet.</span><span class="sxs-lookup"><span data-stu-id="017a8-131">The **App ID URI** is a unique identifier for your application.</span></span>  <span data-ttu-id="017a8-132">Konventionen är att använda `https://<tenant-domain>/<app-name>`, t.ex. `http://localhost:8080/adal4jsample/`.</span><span class="sxs-lookup"><span data-stu-id="017a8-132">The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `http://localhost:8080/adal4jsample/`.</span></span>

<span data-ttu-id="017a8-133">Skapa en gång för din app i portalen en **nyckeln** från den **inställningar** för programmet och kopiera ned.</span><span class="sxs-lookup"><span data-stu-id="017a8-133">Once in the portal for your app create a **Key** from the **Settings** page for your application and copy it down.</span></span>  <span data-ttu-id="017a8-134">Du behöver den snart.</span><span class="sxs-lookup"><span data-stu-id="017a8-134">You will need it shortly.</span></span>

## <a name="2-set-up-your-app-to-use-adal4j-library-and-prerequisites-using-maven"></a><span data-ttu-id="017a8-135">2. Konfigurera din app att använda ADAL4J bibliotek och förutsättningar med Maven</span><span class="sxs-lookup"><span data-stu-id="017a8-135">2. Set up your app to use ADAL4J library and prerequisites using Maven</span></span>
<span data-ttu-id="017a8-136">Här kan konfigurerar vi ADAL4J för att använda autentiseringsprotokollet OpenID Connect.</span><span class="sxs-lookup"><span data-stu-id="017a8-136">Here, we'll configure ADAL4J to use the OpenID Connect authentication protocol.</span></span>  <span data-ttu-id="017a8-137">ADAL4J används för att utfärda inloggnings- och utloggningsförfrågningar, hantera användarens session och få information om användare, bland annat.</span><span class="sxs-lookup"><span data-stu-id="017a8-137">ADAL4J will be used to issue sign-in and sign-out requests, manage the user's session, and get information about the user, amongst other things.</span></span>

* <span data-ttu-id="017a8-138">I rotkatalogen för ditt projekt, öppna/skapa `pom.xml` och leta upp den `// TODO: provide dependencies for Maven` och Ersätt med följande:</span><span class="sxs-lookup"><span data-stu-id="017a8-138">In the root directory of your project, open/create `pom.xml` and locate the `// TODO: provide dependencies for Maven` and replace with the following:</span></span>

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



## <a name="3-create-the-java-publicclient-file"></a><span data-ttu-id="017a8-139">3. Skapa filen Java PublicClient</span><span class="sxs-lookup"><span data-stu-id="017a8-139">3. Create the Java PublicClient file</span></span>
<span data-ttu-id="017a8-140">Enligt ovan, kommer vi att använda Graph-API och hämta data om den inloggade användaren.</span><span class="sxs-lookup"><span data-stu-id="017a8-140">As indicated above, we will be using the Graph API to get data about the logged in user.</span></span> <span data-ttu-id="017a8-141">För att det ska vara enkelt för att vi ska vi skapa både en fil som representerar en **katalogobjekt** och en enskild fil som representerar den **användaren** så att OO mönstret för Java kan användas.</span><span class="sxs-lookup"><span data-stu-id="017a8-141">For this to be easy for us we should create both a file to represent a **Directory Object** and an individual file to represent the **User** so that the OO pattern of Java can be used.</span></span>

* <span data-ttu-id="017a8-142">Skapa en fil med namnet `DirectoryObject.java` som ska användas för att lagra grundläggande information om alla DirectoryObject (du kan passa på att använda detta senare för andra diagrammet frågor kan du göra).</span><span class="sxs-lookup"><span data-stu-id="017a8-142">Create a file called `DirectoryObject.java` which we will use to store basic data about any DirectoryObject (you can feel free to use this later for any other Graph Queries you may do).</span></span> <span data-ttu-id="017a8-143">Du kan klipp ut och klistra in det här nedan:</span><span class="sxs-lookup"><span data-stu-id="017a8-143">You can cut/paste this from below:</span></span>

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


## <a name="compile-and-run-the-sample"></a><span data-ttu-id="017a8-144">Kompilera och köra exemplet</span><span class="sxs-lookup"><span data-stu-id="017a8-144">Compile and run the sample</span></span>
<span data-ttu-id="017a8-145">Ändra tillbaka till din rotkatalog och kör följande kommando för att bygga exemplet du fört in tillsammans med `maven`.</span><span class="sxs-lookup"><span data-stu-id="017a8-145">Change back out to your root directory and run the following command to build the sample you just put together using `maven`.</span></span> <span data-ttu-id="017a8-146">Det här använder den `pom.xml` filen som du skrev för beroenden.</span><span class="sxs-lookup"><span data-stu-id="017a8-146">This will use the `pom.xml` file you wrote for dependencies.</span></span>

`$ mvn package`

<span data-ttu-id="017a8-147">Du bör nu ha en `adal4jsample.war` filen i din `/targets` directory.</span><span class="sxs-lookup"><span data-stu-id="017a8-147">You should now have a `adal4jsample.war` file in your `/targets` directory.</span></span> <span data-ttu-id="017a8-148">Du kan distribuera som i Tomcat-behållaren och besöker Webbadressen</span><span class="sxs-lookup"><span data-stu-id="017a8-148">You may deploy that in your Tomcat container and visit the URL</span></span> 

`http://localhost:8080/adal4jsample/`

> [!NOTE]
> <span data-ttu-id="017a8-149">Det är mycket enkelt att distribuera en WAR-fil med de senaste Tomcat-servrarna.</span><span class="sxs-lookup"><span data-stu-id="017a8-149">It is very easy to deploy a WAR with the latest Tomcat servers.</span></span> <span data-ttu-id="017a8-150">Helt enkelt navigera till `http://localhost:8080/manager/` och följ instruktionerna på Överför din '' adal4jsample.war-filen.</span><span class="sxs-lookup"><span data-stu-id="017a8-150">Simply navigate to `http://localhost:8080/manager/` and follow the instructions on uploading your ``adal4jsample.war\` file.</span></span> <span data-ttu-id="017a8-151">Det kommer autodeploy du med korrekt slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="017a8-151">It will autodeploy for you with the correct endpoint.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="017a8-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="017a8-152">Next Steps</span></span>
<span data-ttu-id="017a8-153">Grattis!</span><span class="sxs-lookup"><span data-stu-id="017a8-153">Congratulations!</span></span> <span data-ttu-id="017a8-154">Nu har du en fungerande Java-program som har möjlighet att autentisera användare på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och få grundläggande information om användaren.</span><span class="sxs-lookup"><span data-stu-id="017a8-154">You now have a working Java application that has the ability to authenticate users, securely call Web APIs using OAuth 2.0, and get basic information about the user.</span></span>  <span data-ttu-id="017a8-155">Om du inte redan gjort nu är det dags att fylla i din klient med vissa användare.</span><span class="sxs-lookup"><span data-stu-id="017a8-155">If you haven't already, now is the time to populate your tenant with some users.</span></span>

<span data-ttu-id="017a8-156">För referens anger det slutförda exemplet (utan dina konfigurationsvärden) [har angetts som en .zip här](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), eller kan du klona den från GitHub:</span><span class="sxs-lookup"><span data-stu-id="017a8-156">For reference, the completed sample (without your configuration values) [is provided as a .zip here](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), or you can clone it from GitHub:</span></span>

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

