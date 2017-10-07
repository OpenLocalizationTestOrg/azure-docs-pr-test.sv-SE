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
# <a name="using-java-command-line-app-tooaccess-an-api-with-azure-ad"></a>Med Java Kommandoradsapp tooAccess en API med Azure AD
[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure AD gör det lätt toooutsource ditt webbprogram Identitetshantering att tillhandahålla enkel inloggning och utloggning med bara några få rader med kod.  I Java-webbappar, kan du göra detta med hjälp av Microsofts implementering av hello community-driven ADAL4J.

  Vi använder här ADAL4J till:

* Logga hello användare in hello app med Azure AD som hello identitetsleverantör.
* Visa information om hello användare.
* Logga hello användare utanför hello appen.

I order toodo detta, måste du:

1. Registrera ett program med Azure AD
2. Konfigurera biblioteket app toouse hello ADAL4J.
3. Använd hello ADAL4J biblioteket tooissue inloggning och utloggning begäranden tooAzure AD.
4. Skriva ut data om hello användare.

tooget har startats [hämta hello app stommen](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/skeleton.zip) eller [hämta hello slutförts exempel](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect\\/archive/complete.zip).  Du måste också en Azure AD-klient i vilka tooregister ditt program.  Om du inte redan har en [Lär dig hur tooget en](active-directory-howto-tenant.md).

## <a name="1--register-an-application-with-azure-ad"></a>1.  Registrera ett program med Azure AD
tooenable tooauthenticate användare för din app behöver du tooregister ett nytt program i din klient.

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Hello översta, klicka på fältet för ditt konto och på hello **Directory** Välj plats tooregister programmet hello Active Directory-klient.
3. Klicka på **fler tjänster** i hello vänstra nav och välj **Azure Active Directory**.
4. Klicka på **App registreringar** och välj **Lägg till**.
5. Följ anvisningarna för hello och skapa en ny **webbprogram och/eller WebAPI**.
  * Hej **namn** av hello programmet beskriver dina program tooend-användare
  * Hej **inloggnings-URL** är hello bas-URL för din app.  hello stommen standardvärdet är `http://localhost:8080/adal4jsample/`.
6. När du har slutfört registreringen tilldelas AAD appen ett unikt program-ID.  Du behöver det här värdet i nästa avsnitt hello så kopiera den från hello programfliken.
7. Från hello **inställningar** -> **egenskaper** för programmet, uppdatera hello App-ID-URI. Hej **App-ID URI** är en unik identifierare för programmet.  hello konventionen är toouse `https://<tenant-domain>/<app-name>`, t.ex. `http://localhost:8080/adal4jsample/`.

En gång i hello portal för din app skapar en **nyckeln** från hello **inställningar** för programmet och kopiera ned.  Du behöver den snart.

## <a name="2-set-up-your-app-toouse-adal4j-library-and-prerequisites-using-maven"></a>2. Konfigurera din app toouse ADAL4J bibliotek och förutsättningar med Maven
Här kan konfigurerar vi ADAL4J toouse hello autentiseringsprotokollet OpenID Connect.  ADAL4J kommer att använda tooissue inloggning och utloggningsförfrågningar, hantera hello användarens session och få information om hello användare, bland annat.

* I hello rotkatalogen för ditt projekt, öppna/skapa `pom.xml` och leta upp hello `// TODO: provide dependencies for Maven` och Ersätt med hello följande:

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



## <a name="3-create-hello-java-publicclient-file"></a>3. Skapa hello Java PublicClient fil
Enligt ovan, kommer vi att använda hello Graph API tooget data om hello inloggad användare. För den här enkelt för oss toobe ska vi skapa både en fil toorepresent en **katalogobjekt** och en enskild fil toorepresent hello **användaren** så att hello OO mönstret för Java kan användas.

* Skapa en fil med namnet `DirectoryObject.java` som ska användas för toostore grundläggande information om alla DirectoryObject (du ledigt toouse detta senare för andra diagrammet frågor kan du göra). Du kan klipp ut och klistra in det här nedan:

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


## <a name="compile-and-run-hello-sample"></a>Kompilera och köra hello-exempel
Ändra tillbaka ut tooyour rotkatalog och kör hello efter kommandot toobuild hello exempel du fört in tillsammans med `maven`. Det här använder hello `pom.xml` fil som du skrev för beroenden.

`$ mvn package`

Du bör nu ha en `adal4jsample.war` filen i din `/targets` directory. Du kan distribuera som i Tomcat-behållaren och besök hello URL 

`http://localhost:8080/adal4jsample/`

> [!NOTE]
> Det är mycket enkelt toodeploy en WAR-fil med hello senaste Tomcat-servrar. Helt enkelt navigera för`http://localhost:8080/manager/` och följer instruktionerna för hello på Överför din '' adal4jsample.war-filen. Det kommer autodeploy du med hello korrekt slutpunkt.
> 
> 

## <a name="next-steps"></a>Nästa steg
Grattis! Du nu ha en fungerande Java-program som har hello möjlighet tooauthenticate användare på ett säkert sätt anropa webb-API: er med hjälp av OAuth 2.0 och hämta grundläggande information om hello användare.  Om du inte redan gjort nu är hello tid toopopulate din klient med vissa användare.

För referens anger hello slutförts exemplet (utan dina konfigurationsvärden) [har angetts som en .zip här](https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect/archive/complete.zip), eller kan du klona den från GitHub:

```git clone --branch complete https://github.com/Azure-Samples/active-directory-java-webapp-openidconnect.git```

