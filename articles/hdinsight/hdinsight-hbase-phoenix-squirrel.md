---
title: aaaUse Apache Phoenix och SQuirreL med Windows-baserade Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse Apache Phoenix i HDInsight, och hur tooinstall och konfigurera SQuirreL på din arbetsstation tooconnect tooan HBase-kluster i HDInsight."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1a756e98-75c1-44cd-a178-c5119683b7b7
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 147ac35fa882fd1bedbc5361ac804c36a4d56de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a>Använda Apache Phoenix och SQuirreL med Windows-baserade HBase-kluster i HDInsight
Lär dig hur toouse [Apache Phoenix](http://phoenix.apache.org/) i HDInsight, och hur tooinstall och konfigurera SQuirreL på din arbetsstation tooconnect tooan HBase-kluster i HDInsight. Läs mer om Phoenix [Phoenix i 15 minuter eller mindre](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Hello Phoenix grammatik, se [Phoenix grammatik](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Hello Phoenix versionsinformation i HDInsight, se [vad är nytt i hello Hadoop-klusterversioner som tillhandahålls av HDInsight?](hdinsight-component-versioning.md).
>

> [!IMPORTANT]
> hello stegen i det här dokumentet endast fungerar för Windows-baserade HDInsight-kluster. HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). Information om hur du använder Phoenix på Linux-baserat HDInsight finns i [använda Apache Phoenix med Linux-baserade HBase-kluster i HDInsight](hdinsight-hbase-phoenix-squirrel-linux.md).
>



## <a name="use-sqlline"></a>Använd SQLLine
[SQLLine](http://sqlline.sourceforge.net/) är en kommandorad verktyget tooexecute SQL.

### <a name="prerequisites"></a>Krav
Innan du kan använda SQLLine, måste du ha hello följande:

* **En HBase-kluster i HDInsight**. Mer information om etablera HBase-kluster, se [Kom igång med Apache HBase i HDInsight][hdinsight-hbase-get-started].
* **Ansluta toohello HBase-kluster via hello Fjärrskrivbordsprotokollet**. Instruktioner finns i [hantera Hadoop-kluster i HDInsight med hjälp av hello Azure klassiska Portal][hdinsight-manage-portal].

**toofind ut hello värdnamn**

1. Öppna **Hadoop kommandoraden** från hello skrivbordet.
2. Kör följande kommando tooget hello DNS-suffix hello:

        ipconfig

    Skriv ned **anslutningsspecifika DNS-suffixet**. Till exempel *myhbasecluster.f5.internal.cloudapp.net*. När du ansluter tooan HBase-kluster måste tooconnect tooone av hello Zookeepers med FQDN. Varje HDInsight-kluster har 3 Zookeepers. De är *zookeeper0*, *zookeeper1*, och *zookeeper2*. hello FQDN blir något som liknar *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.

**toouse SQLLine**

1. Öppna **Hadoop kommandoraden** från hello skrivbordet.
2. Kör följande kommandon tooopen SQLLine hello:

        cd %phoenix_home%\bin
        sqlline.py [hello FQDN of one of hello Zookeepers]

    ![HDInsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    hello-kommandon som används i hello exemplet:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

Mer information finns i [SQLLine manuell](http://sqlline.sourceforge.net/#manual) och [Phoenix grammatik](http://phoenix.apache.org/language/index.html).

## <a name="use-squirrel"></a>Använd SQuirreL
[SQuirreL SQL Client](http://squirrel-sql.sourceforge.net/) är ett grafiskt Java-program som gör att du tooview hello struktur för en JDBC-kompatibel databas, bläddra hello data i tabeller, utfärda SQL-kommandon osv. Det kan vara används tooconnect tooApache Phoenix på HDInsight.

Det här avsnittet beskrivs hur du tooinstall och konfigurera SQuirreL på din arbetsstation tooconnect tooan HBase-kluster i HDInsight via VPN.

### <a name="prerequisites"></a>Krav
Innan du följer hello procedurer, måste du ha hello följande:

* Ett HBase-kluster distribueras tooan Azure-nätverk med en virtuell dator i DNS.  Instruktioner finns i [skapa HBase-kluster i Azure Virtual Network][hdinsight-hbase-provision-vnet].

* Hämta hello HBase-kluster klustret anslutningsspecifika DNS-suffix. tooget den RDP till hello-kluster och kör sedan IPConfig.  hello DNS-suffix är ungefär:

        myhbase.b7.internal.cloudapp.net
* Hämta och installera [Microsoft Visual Studio Express för Windows-skrivbordet](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) på arbetsstationen. Du behöver makecert från hello paketet toocreate ditt certifikat.  
* Hämta och installera [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) på arbetsstationen.  SQuirreL SQL klientversion 3.0 och senare kräver JRE version 1.6 eller senare.  

### <a name="configure-a-point-to-site-vpn-connection-toohello-azure-virtual-network"></a>Konfigurera en punkt-till-plats VPN-anslutningen toohello virtuella Azure-nätverket
Ingår 3 steg hur du konfigurerar en punkt-till-plats VPN-anslutning:

1. [Konfigurera ett virtuellt nätverk och en dynamisk routning gateway](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [Skapa certifikat](#Create-your-certificates)
3. [Konfigurera VPN-klienten](#Configure-your-VPN-client)

Se [konfigurerar en punkt-till-plats VPN-anslutningen tooan Azure Virtual Network](../vpn-gateway/vpn-gateway-point-to-site-create.md) för mer information.

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a>Konfigurera ett virtuellt nätverk och en dynamisk routning gateway
Garantera att du har etablerat ett HBase-kluster i Azure-nätverk (se hello krav för det här avsnittet). hello nästa steg är tooconfigure en punkt-till-plats-anslutning.

**tooconfigure hello punkt-till-plats-anslutning**

1. Logga in toohello [klassiska Azure-portalen][azure-portal].
2. Klicka på vänster hello **nätverk**.
3. Klicka på hello virtuella nätverk som du har skapat (se [etablera HBase-kluster i Azure Virtual Network][hdinsight-hbase-provision-vnet]).
4. Klicka på **konfigurera** hello uppifrån.
5. I hello **punkt-till-plats-anslutning** väljer **konfigurera punkt-till-plats-anslutning**.
6. Konfigurera **första IP-** och **CIDR** toospecify hello IP-adressintervall från vilka dina VPN-klienter tar emot en IP-adress vid anslutning. hello-intervallet får inte överlappa med någon av hello adressintervall som finns på ditt lokala nätverk och hello du ska kunna ansluta till virtuella Azure-nätverket. Till exempel. Om du har valt 10.0.0.0/20 för hello virtuella nätverk kan du välja 10.1.0.0/24 för hello klientens adressutrymme. Se hello [punkt-till-plats-anslutning] [ vnet-point-to-site-connectivity] för mer information.
7. På hello virtuellt nätverk adress blanksteg under **Lägg till gateway-undernätet**.
8. Klicka på **spara** på hello längst ned på sidan för hello.
9. Klicka på **Ja** tooconfirm hello ändringen. Vänta tills hello system är klar med dina hello ändra innan du fortsätter toohello nästa procedur.

**toocreate en dynamisk routning gateway**

1. Hello klassiska Azure-portalen, klicka på **INSTRUMENTPANELEN** från hello hello överst.
2. Klicka på **skapa GATEWAY** från hello längst ned på sidan för hello.
3. Klicka på **Ja** tooconfirm. Vänta tills hello gateway har skapats.
4. Klicka på **INSTRUMENTPANELEN** hello uppifrån.  Ett visual diagram över hello virtuella nätverk visas:

    ![Virtuella Azure-nätverket punkt-till-plats-diagram][img-vnet-diagram]

    hello diagram visar 0-klientanslutningar. När du har gjort ett virtuellt nätverk anslutning toohello blir hello nummer uppdaterade tooone.

#### <a name="create-your-certificates"></a>Skapa certifikat
Enkelriktade toocreate ett X.509-certifikat är att använda hello certifikat verktyg för att skapa (makecert.exe) som medföljer [Microsoft Visual Studio Express för Windows-skrivbordet](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).

**toocreate ett självsignerat rotcertifikat**

1. Öppna Kommandotolken från din arbetsstation.
2. Navigera toohello Visual Studio tools mapp.
3. hello följande kommando i hello exemplet nedan skapar och installera ett rotcertifikat i hello personliga certifikatarkivet på din arbetsstation och även skapa en motsvarande .cer-fil som du kommer senare överför toohello klassiska Azure-portalen.

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    Ändra toohello katalog hello .cer-filen toobe finns i, där HBaseVnetVPNRootCertificate är hello namn som du vill toouse för hello certifikatet.

    Stäng inte hello kommandotolk.  Du behöver det i hello nästa procedur.

   > [!NOTE]
   > Eftersom du har skapat ett rotcertifikat som klientcertifikat genereras, kanske du vill tooexport detta certifikat tillsammans med dess privata nyckel och spara den tooa säker plats där den kan återställas.
   >
   >

**toocreate ett klientcertifikat**

* Från hello samma kommandotolk (den har toobe på hello samma dator där du skapade hello rotcertifikat. hello klientcertifikatet måste skapas från hello rotcertifikat), kör hello följande kommando:

          makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    HBaseVnetVPNRootCertificate är hello rotnamn för certifikatet.  Den har toomatch hello rotnamn certifikat.  

    Både hello rotcertifikatet och klientcertifikatet hello lagras i ditt personliga certifikatarkiv på datorn. Använd certmgr.msc tooverify.

    ![Punkt-till-plats VPN-certifikat för virtuella Azure-nätverket][img-certificate]

    Ett certifikat måste installeras på varje dator som du vill tooconnect toohello virtuellt nätverk. Vi rekommenderar att du skapar unika certifikat för varje dator som du vill tooconnect toohello virtuellt nätverk. tooexport hello klientcertifikat, Använd certmgr.msc.

**tooupload hello root certificate toohello klassiska Azure-portalen**

1. Hello klassiska Azure-portalen, klicka på **nätverk** hello vänster.
2. Klicka på hello virtuella nätverk där din HBase-kluster har distribuerats till.
3. Klicka på **certifikat** hello uppifrån.
4. Klicka på **överför** från hello nederkanten och ange hello rotcertifikatfilen som du har skapat i hello proceduren före senaste. Vänta tills hello certifikatet har importerats.
5. Klicka på **INSTRUMENTPANELEN** hello längst upp.  hello virtuella diagram visar hello status.

#### <a name="configure-your-vpn-client"></a>Konfigurera VPN-klienten
**toodownload och installera hello VPN-klientpaketet**

1. Klicka på antingen från hello INSTRUMENTPANELEN i hello virtuellt nätverk under hello snabböversikten **Download hello 64-bitars VPN-klientpaketet** eller **Download hello 32-bitars VPN-klientpaketet** baserat på din arbetsstation OS-version.
2. Klicka på **kör** tooinstall hello paketet.
3. Hello säkerhetsmeddelande klickar du på **mer info**, och klicka sedan på **kör ändå**.
4. Klicka på **Ja** två gånger.

**tooconnect tooVPN**

1. Klicka på hello nätverk-ikonen i Aktivitetsfältet hello hello skrivbordet på din arbetsstation. En VPN-anslutning visas med namn på din virtuella nätverk.
2. Klicka på hello VPN-anslutningens namn.
3. Klicka på **Anslut**.

**tootest hello namnmatchning för VPN-anslutning och domän**

* Öppna en kommandotolk från hello arbetsstation och pinga en hello följande namn tilldelat hello HBase klustret DNS-suffix är myhbase.b7.internal.cloudapp.net:

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

### <a name="install-and-configure-squirrel-on-your-workstation"></a>Installera och konfigurera SQuirreL på din arbetsstation
**tooinstall SQuirreL**

1. Hämta hello SQuirreL SQL klienten jar-filen från [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).
2. Öppna/köra hello jar-filen. Det krävs hello [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).
3. Klicka på **nästa** två gånger.
4. Ange en sökväg där du har hello skrivbehörighet och klicka sedan på **nästa**.

  > [!NOTE]
  > hello standardinstallationsmappen finns i hello C:\Program Files\squirrel-sql-3,6 mapp.  Hello installer måste beviljas hello administratörsbehörighet för i ordning toowrite toothis sökväg. Du kan öppna en kommandotolk som administratör, gå Toojava's bin-mappen och kör sedan:
  >
  >     Java.exe-jar [hello SQuirreL jar-filen hello sökväg]
5. Klicka på **OK** tooconfirm skapar hello målkatalogen.
6. hello standardinställningen är tooinstall hello Base och Standard paket.  Klicka på **Nästa**.
7. Klicka på **nästa** två gånger, och klicka sedan på **klar**.

**tooinstall hello Phoenix drivrutin**

hello phoenix drivrutinen jar-filen finns på hello HBase-kluster. hello sökvägen är liknande toohello följande baserat på hello versioner:

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
Du behöver toocopy den tooyour arbetsstation under hello [SQuirreL installationsmappen] / lib sökväg.  hello enklast tooRDP i hello klustret och Använd fil kopiera och klistra in (CTRL + C och CTRL + V) toocopy den tooyour arbetsstation.

**tooadd en Phoenix drivrutinen tooSQuirreL**

1. Öppna SQuirreL SQL-klienten från din arbetsstation.
2. Klicka på hello **drivrutinen** fliken hello vänster.
3. Från hello **drivrutiner** -menyn klickar du på **ny drivrutin**.
4. Ange hello följande information:

   * **Namnet**: Phoenix
   * **Exempel-URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net
   * **Klassnamn**: org.apache.phoenix.jdbc.PhoenixDriver

     > [!WARNING]
     > Användaren alla med gemener hello exempel-URL. Du kan använda de fullständiga zookeeper kvorum om en av dem är nere.  hello värdnamn är zookeeper0, zookeeper1 och zookeeper2.
     >
     >

     ![HDInsight HBase Phoenix SQuirreL drivrutin][img-squirrel-driver]
5. Klicka på **OK**.

**toocreate ett alias toohello HBase-kluster**

1. Klicka på hello SQuirreL, **alias** fliken hello vänster.
2. Från hello **alias** -menyn klickar du på **nytt Alias**.
3. Ange hello följande information:

   * **Namnet**: hello namnet på hello HBase-kluster eller ett namn som du föredrar.
   * **Drivrutinen**: Phoenix.  Detta måste matcha hello drivrutinsnamn som du skapade i föregående procedur för hello.
   * **URL: en**: hello URL kopieras från konfigurationen av drivrutinen. Se till att toouser alla gemen.
   * **Användarnamnet**: Det kan vara text.  Eftersom du använder VPN-anslutningar här används inte hello användarnamn alls.
   * **Lösenordet**: Det kan vara text.

     ![HDInsight HBase Phoenix SQuirreL drivrutin][img-squirrel-alias]
4. Klicka på **Test**.
5. Klicka på **Anslut**. När den gör hello anslutning SQuirreL ser ut som:

    ![HBase Phoenix SQuirreL][img-squirrel]

**toorun ett test**

1. Klicka på hello **SQL** fliken höger nästa toohello **objekt** fliken.
2. Kopiera och klistra in följande kod hello:

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. Klicka hello kör.

    ![HBase Phoenix SQuirreL][img-squirrel-sql]
4. Växla tillbaka toohello **objekt** fliken.
5. Expandera hello aliasnamn och expandera sedan **tabellen**.  Du bör se hello ny tabell visas under.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig hur toouse Apache Phoenix i HDInsight.  Det finns fler toolearn

* [HDInsight HBase-översikt][hdinsight-hbase-overview]: HBase är en NoSQL-databas av Apachetyp med öppen källkod som bygger på Hadoop och som ger direktåtkomst och stark konsekvens för stora mängder ostrukturerade och semistrukturerade data.
* [Etablera HBase-kluster i Azure Virtual Network][hdinsight-hbase-provision-vnet]: med virtuell nätverksintegration HBase-kluster kan vara distribuerade toohello samma virtuella nätverk som dina program så att program kan kommunicera med HBase direkt.
* [Konfigurera HBase-replikering i HDInsight](hdinsight-hbase-replication.md): Lär dig hur tooconfigure HBase-replikering mellan två Azure-datacenter.
* [Analysera Twitter-åsikter med HBase i HDInsight][hbase-twitter-sentiment]: Lär dig hur toodo realtid [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) av stordata med HBase i ett Hadoop-kluster i HDInsight.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
