---
title: "aaaSet in Apache Tomcat på en Linux-dator | Microsoft Docs"
description: "Lär dig hur tooset in Apache Tomcat7 med hjälp av Azure virtuella datorer som kör Linux."
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 45ecc89c-1cb0-4e80-8944-bd0d0bbedfdc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: ningk
ms.openlocfilehash: b837a73e91fcb25d5459d993a0e93ceef1a1fc8b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a>Ställa in Tomcat7 på en Linux-dator med Azure
Apache Tomcat (eller bara Tomcat kan också kallades Djakarta Tomcat) är en öppen källkod webbservern och servlet-behållare som utvecklats av hello Apache Software Foundation (ASF). Tomcat implementerar hello Java Servlet och hello JavaServer sidor (JSP) specifikationer från Sun Microsystems. Tomcat ger en ren Java HTTP web server-miljö i vilken toorun Java-kod. I hello enklaste konfigurationen körs Tomcat i en enda process. Den här processen körs en Java virtual machine (JVM). Varje HTTP-begäran från en webbläsare tooTomcat behandlas som en separat tråd hello Tomcat pågår.  

> [!IMPORTANT]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Azure Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln beskriver hur toouse hello klassiska distributionsmodellen. Vi rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. toouse toodeploy en Resource Manager-mallen en Ubuntu VM med öppna JDK och Tomcat, se [i den här artikeln](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).

I den här artikeln får du installerar Tomcat7 på en Linux-avbildning och distribuera den i Azure.  

Du kommer att lära dig:  

* Hur toocreate en virtuell dator i Azure.
* Hur hello tooprepare virtuell dator för Tomcat7.
* Hur tooinstall Tomcat7.

Det förutsätts att du redan har en Azure-prenumeration.  Om inte, du kan registrera dig för en kostnadsfri utvärderingsversion på [hello Azure-webbplatsen](https://azure.microsoft.com/). Om du har en MSDN-prenumeration, se [Microsoft särskilda priser för Azure: MSDN, MPN och BizSpark fördelar](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). toolearn mer om Azure, se [vad är Azure?](https://azure.microsoft.com/overview/what-is-azure/).

Den här artikeln förutsätter att du har grundläggande kunskaper om Tomcat- och Linux.  

## <a name="phase-1-create-an-image"></a>Fas 1: Skapa en avbildning
I det här steget ska du skapa en virtuell dator med hjälp av en Linux-avbildning i Azure.  

### <a name="step-1-generate-an-ssh-authentication-key"></a>Steg 1: Skapa en SSH-nyckel för autentisering
SSH är ett viktigt verktyg för administratörer. Dock rekommenderas konfigurera åtkomstsäkerhet baserat på ett fastställt mänskliga lösenord inte. Angripare kan dela upp i ditt system baserat på ett användarnamn och ett svagt lösenord.

hello bra är att det finns ett sätt tooleave fjärråtkomst öppna och oroa dig inte om lösenord. Den här metoden består av autentisering med asymmetrisk kryptering. hello är användares privata nyckel hello som ger hello-autentisering. Du kan även låsa hello användarkonto toonot Tillåt lösenordsautentisering.

En annan fördel med den här metoden är att du inte behöver olika lösenord toosign i toodifferent servrar. Du kan autentisera med hjälp av hello personliga privata nyckel på alla servrar som förhindrar att tooremember flera lösenord.



Följ dessa steg toogenerate hello SSH-autentisering-nyckel.

1. Hämta och installera PuTTYgen från hello följande plats: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Kör Puttygen.exe.
3. Klicka på **generera** toogenerate hello nycklar. Hello processen att du kan öka slumpmässighet genom glidande hello muspekaren över hello tomt område i hello-fönstret.  
   ![PuTTY nyckel Generator skärmbild som visar hello genererar nya nycklar knappen][1]
4. När hello generera process, visas Puttygen.exe den offentliga nyckeln.  
   ![PuTTY nyckel Generator skärmbild som visar hello ny offentlig nyckel och hello Spara privat nyckel knapp][2]
5. Markera och kopiera hello offentliga nyckeln och spara det i en fil med namnet publicKey.pem. Klicka inte på **spara offentlig nyckel**eftersom hello spara offentlig nyckel filformatet skiljer sig från hello offentlig nyckel som vi vill.
6. Klicka på **Spara privat nyckel**, och spara den i en fil med namnet privateKey.ppk.

### <a name="step-2-create-hello-image-in-hello-azure-portal"></a>Steg 2: Skapa hello bild i hello Azure-portalen
1. I hello [portal](https://portal.azure.com/), klickar du på **ny** i hello uppgift liggande toocreate en bild. Välj hello Linux bild är baserad på dina behov. hello används följande exempel hello Ubuntu 14.04 bild.
![Skärmbild av hello portal som visar hello ny knapp][3]

2. För **värdnamn**, ange hello namn för hello-URL att du och Internet-klienter kommer att använda tooaccess den här virtuella datorn. Definiera hello sista delen av hello DNS-namn, till exempel tomcatdemo. Azure skapar sedan hello URL som tomcatdemo.cloudapp.net.  

3. För **SSH autentiseringsnyckel**, kopiera hello nyckelvärde från hello publicKey.pem fil som innehåller hello offentliga nyckel som genererats av PuTTYgen.  
![Autentiseringsnyckeln för SSH rutan i hello-portalen][4]

4. Konfigurera övriga inställningar efter behov och klicka sedan på **skapa**.  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Fas 2: Förbereda den virtuella datorn för Tomcat7
I det här steget kommer du konfigurerar en slutpunkt för Tomcat trafik och ansluter sedan tooyour ny virtuell dator.

### <a name="step-1-open-hello-http-port-tooallow-web-access"></a>Steg 1: Öppna hello HTTP port tooallow webbåtkomst
Slutpunkter i Azure består av ett TCP- eller UDP-protokoll, tillsammans med en offentlig och privat port. hello privata porten är hello hello tjänsten lyssnar tooon hello virtuell dator. hello offentlig port är hello-port som hello Azure-molntjänst lyssnar tooexternally för inkommande, Internet-baserade trafik.  

TCP-port 8080 är hello standardportnumret att Tomcat använder toolisten. Om den här porten är öppen med en Azure slutpunkt du och andra Internet-klienter kan komma åt Tomcat sidor.  

1. I hello-portalen klickar du på **Bläddra** > **virtuella datorer**, och klicka sedan på hello virtuell dator som du skapade.  
   ![Skärmbild av hello virtuella datorer directory][5]
2. tooadd en slutpunkt tooyour virtuell dator, klicka på hello **slutpunkter** rutan.
   ![Skärmbild som visar hello slutpunkter rutan][6]
3. Klicka på **Lägg till**.  

   1. För hello slutpunkt, ange ett namn för hello slutpunkt i **Endpoint**, och ange sedan 80 i **offentlig Port**.  

      Om du anger det too80, behöver du inte tooinclude hello portnumret i hello URL-adress används tooaccess Tomcat. Till exempel http://tomcatdemo.cloudapp.net.    

      Om du anger det tooanother värde, exempelvis 81, måste tooadd hello port number toohello URL tooaccess Tomcat. Till exempel http://tomcatdemo.cloudapp.net:81 /.
   2. Ange 8080 i **privat Port**. Som standard lyssnar Tomcat på TCP-port 8080. Om du har ändrat hello standard lyssna port Tomcat bör du uppdatera **privat Port** toobe hello samma som hello lyssningsporten för Tomcat.  
      ![Skärmbild av Användargränssnittet som visar Lägg till kommandot offentlig Port och privat Port][7]
4. Klicka på **OK** tooadd hello endpoint tooyour virtuella datorn.

### <a name="step-2-connect-toohello-image-you-created"></a>Steg 2: Anslut toohello bilden som du skapat
Du kan välja en SSH-verktyget tooconnect tooyour virtuell dator. I det här exemplet använder vi PuTTY.  

1. Hämta hello DNS-namnet på den virtuella datorn från hello-portalen.
    1. Klicka på **Bläddra** > **virtuella datorer**.
    2. Välj hello namnet på den virtuella datorn och klicka sedan på **egenskaper**.
    3. I hello **egenskaper** panelen, titta i hello **domännamn** rutan tooget hello DNS-namn.  

2. Hämta hello port för SSH-anslutningar från hello **SSH** rutan.  
![Skärmbild som visar hello SSH anslutningsnummer port][8]

3. Hämta [PuTTY](http://www.putty.org/).  

4. Klicka på hello körbar fil Putty.exe efter hämtningen. PuTTY-konfiguration, konfigurera hello grundalternativ med hello värdnamn och portnummer som erhålls från hello egenskaperna för den virtuella datorn.   
![Skärmbild som visar hello PuTTY configuration värden namn på och portnummer alternativ][9]

5. Hello vänster klickar du på **anslutning** > **SSH** > **Auth**, och klicka sedan på **Bläddra** toospecify hello sökväg till hello privateKey.ppk. Hej privateKey.ppk filen innehåller hello privata nyckeln som genereras av PuTTYgen tidigare i hello ”fas 1: skapa en avbildning” i den här artikeln.  
![Skärmbild som visar hello anslutning directory-hierarkin och knappen Bläddra.][10]

6. Klicka på **öppna**. Du kan bli aviserad genom en meddelanderuta. Om du har konfigurerat hello DNS-namn och portnummer korrekt, klickar du på **Ja**.
![Skärmbild som visar hello-meddelande][11]

7. Du är tooenter ange ditt användarnamn.  
![Skärmbild som visar var tooenter användarnamn][12]

8. Ange hello användarnamn som du använt toocreate hello virtuell dator i hello ”fas 1: skapa en avbildning” tidigare i den här artikeln. Du ser något som liknar hello följande:  
![Skärmbild som visar hello autentisering bekräftelse][13]

## <a name="phase-3-install-software"></a>Fas 3: Installera programvara
I det här steget installera hello Java runtime environment, Tomcat7 och andra Tomcat7-komponenter.  

### <a name="java-runtime-environment"></a>Java runtime environment
Tomcat är skriven i Java. Det finns två typer av Java Development Kit (JDKs), OpenJDK och Oracle JDK. Du kan välja hello du.  

> [!NOTE]
> Både JDKs har nästan hello samma Platskod för hello klasser i hello Java API, men hello koden för hello virtuell dator är olika. OpenJDK tenderar toouse öppna bibliotek, medan Oracle JDK tenderar toouse stängd viktiga. Oracle JDK har flera klasser och vissa fast buggar och Oracle JDK är mer stabilt än OpenJDK.

#### <a name="install-openjdk"></a>Installera OpenJDK  

Använd följande kommando toodownload OpenJDK hello.   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  


* toocreate en directory toocontain hello JDK-filer:  

        sudo mkdir /usr/lib/jvm  
* tooextract hello JDK filer i katalogen/usr/lib/jvm/hello:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/

#### <a name="install-oracle-jdk"></a>Installera Oracle JDK


Använd hello efter kommandot toodownload Oracle JDK från hello Oracle webbplats.  

     wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  
* toocreate en directory toocontain hello JDK-filer:  

        sudo mkdir /usr/lib/jvm  
* tooextract hello JDK filer i katalogen/usr/lib/jvm/hello:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  
* tooset Oracle JDK som hello standard Java virtual machine:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  

        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

#### <a name="confirm-that-java-installation-is-successful"></a>Bekräfta att Java-installationen har lyckats
Du kan använda ett kommando som hello efter tootest om hello Java runtime environment har installerats på rätt sätt:  

    java -version  

Om du har installerat OpenJDK, bör du se ett meddelande som hello nedan: ![lyckade OpenJDK installationen visas][14]

Om du har installerat Oracle JDK, bör du se ett meddelande som hello nedan: ![lyckade Oracle JDK installationen visas][15]

### <a name="install-tomcat7"></a>Installera Tomcat7
Använd följande kommando tooinstall Tomcat7 hello.  

    sudo apt-get install tomcat7  

Om du inte använder Tomcat7, använder du hello lämplig variation av det här kommandot.  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a>Bekräfta att Tomcat7 installationen har lyckats
toocheck om Tomcat7 har installerats, bläddra tooyour Tomcat server DNS-namn. Hello exempel-URL är http://tomcatexample.cloudapp.net/ i den här artikeln. Om du ser ett meddelande som hello följande är Tomcat7 korrekt installerad.
![Lyckad Tomcat7 installationen visas][16]

### <a name="install-other-tomcat7-components"></a>Installera komponenter för andra Tomcat7
Det finns andra valfria Tomcat-komponenter som du kan installera.  

Använd hello **sudo lgh cache-sökning tomcat7** kommandot toosee alla tillgängliga hello-komponenter. Använd följande kommandon tooinstall hello vissa användbara komponenter.  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools toocreate user instances  

## <a name="phase-4-configure-tomcat7"></a>Steg 4: Konfigurera Tomcat7
I det här steget kan du administrera Tomcat.

### <a name="start-and-stop-tomcat7"></a>Starta och stoppa Tomcat7
Hej Tomcat7 servern startar automatiskt när du installerar den. Du kan också starta den med hello följande kommando:   

    sudo /etc/init.d/tomcat7 start

toostop Tomcat7:

    sudo /etc/init.d/tomcat7 stop

tooview hello status för Tomcat7:

    sudo /etc/init.d/tomcat7 status

toorestart Tomcat-tjänster: 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a>Tomcat7 administration
Du kan redigera hello Tomcat användaren configuration file tooset in dina administratörsautentiseringsuppgifter. Använd hello följande kommando:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Här är ett exempel:  
![Skärmbild som visar hello sudo vi kommandoutdata][17]  

> [!NOTE]
> Skapa ett starkt lösenord för hello administratörsanvändarnamnet.  

När du redigerar filen, bör du starta om Tomcat7 tjänster med hello efter kommandot tooensure att hello ändringarna gälla:  

    sudo /etc/init.d/tomcat7 restart  

Öppna din webbläsare och ange **http://<your tomcat server DNS name>/manager/html** som hello URL. I den här artikeln hello exempelvis är hello URL http://tomcatexample.cloudapp.net/manager/html.  

Efter anslutning, bör du se något liknande toohello följande:  
![Skärmbild av hello Tomcat Web Application Manager][18]

## <a name="common-issues"></a>Vanliga problem
### <a name="cant-access-hello-virtual-machine-with-tomcat-and-moodle-from-hello-internet"></a>Det går inte att komma åt hello virtuell dator med Tomcat och Moodle från hello Internet
#### <a name="symptom"></a>Symtom  
  Tomcat körs men du kan inte se hello Tomcat standardsida med din webbläsare.
#### <a name="possible-root-cause"></a>Möjliga underliggande orsaker   

  * Hej Tomcat lyssna port är inte hello samma som hello privat port för den virtuella datorns slutpunkten för Tomcat-trafik.  

     Kontrollera din offentliga porten och privat portinställningarna endpoint och kontrollera hello privat port är hello samma som hello Tomcat lyssningsport. Se ”fas 1: skapa en avbildning” i den här artikeln för instruktioner om hur du konfigurerar slutpunkter för den virtuella datorn.  

     toodetermine hello Tomcat lyssningsport, öppna /etc/httpd/conf/httpd.conf (Red Hat-version) eller /etc/tomcat7/server.xml (Debian-version). Som standard är hello Tomcat lyssna port 8080. Här är ett exempel:  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     Om du använder en virtuell dator som Debian och Ubuntu och du vill att toochange hello standard port för Tomcat lyssna (till exempel 8081) kan öppna du också hello port för hello-operativsystem. Öppna först hello-profil:  

        sudo vi /etc/default/tomcat7  

     Sedan ta bort kommentarerna hello sista raden och ändra ”Nej” för ”Ja”.  

        AUTHBIND=yes
  2. hello-brandväggen har inaktiverat hello lyssna port för Tomcat.

     Du kan bara se hello Tomcat standardsida från hello lokala värden. hello problemet sannolikt att hello-port som lyssnar tooby Tomcat, blockeras av hello brandväggen. Du kan använda hello w3m verktyget toobrowse hello webbsidan. hello följande kommandon installera w3m och bläddra toohello Tomcat standardsida:  


        sudo yum installera w3m w3m-img


        w3m http://localhost: 8080  
#### <a name="solution"></a>Lösning

  * Om hello lyssningsporten för Tomcat inte är hello samma som hello privat port för hello slutpunkten för trafik toohello virtuell dator, behöver du ändra hello privat port toobe hello samma som hello lyssningsporten för Tomcat.   
  2. Lägg till följande rader för/etc/sysconfig/iptables hello om hello problemet orsakas av brandvägg/iptables. hello andra raden krävs endast för https-trafik:  

      -A -p tcp -m tcp--datorer 80 -j ACCEPTERA indata

      -A -p tcp -m tcp--datorer 443 -j ACCEPTERA indata  

     > [!IMPORTANT]
     > Kontrollera att hello tidigare rader placeras ovanför de rader som skulle globalt att begränsa åtkomsten till exempel hello följande: - A indata -j AVVISA--avvisa-med icmp värden förbjuden



tooreload hello iptables kör hello följande kommando:

    service iptables restart

Det här har testats på CentOS 6.3.

### <a name="permission-denied-when-you-upload-project-files-toovarlibtomcat7webapps"></a>Åtkomst nekad när du överför projektet filer för/var/lib/tomcat7/webbappar /
#### <a name="symptom"></a>Symtom
  När du använder en SFTP (till exempel FileZilla) tooconnect tooyour virtuella klientdatorn och gå för/var/lib/tomcat7/webbappar/toopublish webbplatsen du får ett fel meddelande liknande toohello följande:  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a>Möjliga underliggande orsaker
  Du har inga behörigheter tooaccess hello /var/lib/tomcat7/webapps mapp.  
#### <a name="solution"></a>Lösning  
  Du behöver tooget behörighet från hello rotkontot. Du kan ändra hello ägare till den mappen i roten toohello användarnamn som du använde när du har etablerat hello-datorn. Här är ett exempel med hello azureuser kontonamn:  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  Använd hello -R alternativet tooapply hello behörigheter för alla filer i en katalog för.  

  Det här kommandot fungerar även för kataloger. hello -R alternativet ändringar hello behörigheter för alla filer och kataloger i hello directory. Här är ett exempel:  

     sudo chown -R username:group directory  

  Det här kommandot ändrar ägarskap (både användare och grupper) för alla filer och mappar som finns inuti hello directory.  

  hello ändrar följande kommando bara hello behörighet för hello mappkatalog. hello filer och mappar i hello directory ändras inte.  

     sudo chown username:group directory

[1]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
