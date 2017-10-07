---
title: aaaUse SSH tunnlar tooaccess Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse en SSH-tunnel toosecurely Bläddra webbresurser finns på Linux-baserat HDInsight-noder."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 879834a4-52d0-499c-a3ae-8d28863abf65
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 8a315c53751bbe6950a182674f4108c67c2f2f8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-ssh-tunneling-tooaccess-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a>Använda SSH-tunnlar tooaccess Ambari-webbgränssnittet, jobbhistorik, NameNode, Oozie och andra webb-användargränssnitt

Linux-baserade HDInsight-kluster ger åtkomst tooAmbari webbgränssnittet över hello Internet, men vissa funktioner i hello Användargränssnittet är inte. Till exempel hello webbgränssnittet för andra tjänster som är anslutna via Ambari. Du måste använda en SSH-tunnel toohello klustret head full funktionalitet för hello Ambari-webbgränssnittet.

## <a name="why-use-an-ssh-tunnel"></a>Varför ska jag använda en SSH-tunnel

Flera hello menyer i Ambari fungerar bara med en SSH-tunnel. Dessa menyer förlitar sig på webbplatser och tjänster som körs på andra nodtyper, till exempel arbetsnoderna. Ofta webbplatserna skyddas inte, så det inte är säker toodirectly exponera hello dem på internet.

hello följande Web användargränssnitt kräver en SSH-tunnel:

* Jobbhistorik
* NameNode
* Trådstackar
* Oozie webbgränssnittet
* HBase-huvud- och loggar UI

Om du använder skriptåtgärder toocustomize klustret kräver någon tjänst eller ett verktyg som du installerar som Exponerar en webbgränssnittet en SSH-tunnel. Om du installerar Hue med hjälp av en skriptåtgärd, måste du använda en SSH-tunnel tooaccess hello Hue-webbgränssnittet.

> [!IMPORTANT]
> Om du har direktåtkomst tooHDInsight via ett virtuellt nätverk kan behöver du inte toouse SSH-tunnlar. Ett exempel på direkt åtkomst till HDInsight via ett virtuellt nätverk, se hello [ansluta HDInsight tooyour lokalt nätverk](connect-on-premises-network.md) dokumentet.

## <a name="what-is-an-ssh-tunnel"></a>Vad är en SSH-tunnel

[Secure Shell (SSH) tunneling](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) dirigerar trafik som skickas tooa port på din lokala dator. hello trafik dirigeras via en SSH-anslutning tooyour HDInsight-klustrets huvudnod. hello-begäran har lösts som om den har sitt ursprung i hello huvudnod. hello svar dirigeras sedan tillbaka via hello tunnel tooyour arbetsstation.

## <a name="prerequisites"></a>Krav

* En SSH-klient. Mer information finns i [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) (Använda SSH med HDInsight).

* En webbläsare som kan vara konfigurerad toouse SOCKS5 proxy.

    > [!WARNING]
    > inbyggd i Windows hello SOCKS proxy stöd stöder inte SOCKS5 och fungerar inte med hello stegen i det här dokumentet. hello följande webbläsare förlitar sig på Windows-proxyinställningar och inte för närvarande fungerar med hello stegen i det här dokumentet:
    >
    > * Microsoft Edge
    > * Microsoft Internet Explorer
    >
    > Google Chrome använder också Windows hello för proxy-inställningar. Du kan dock installera tillägg som stöder SOCKS5. Vi rekommenderar [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).

## <a name="usessh"></a>Skapa en tunnel med hello SSH-kommando

Använd hello följande kommando toocreate en SSH-tunnel med hello `ssh` kommando. Ersätt **användarnamn** med en SSH-användare för din HDInsight-kluster och Ersätt **KLUSTERNAMN** med hello namnet på ditt HDInsight-kluster:

```bash
ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
```

Detta kommando skapar en anslutning som dirigerar trafik toolocal port 9876 toohello klustret via SSH. hello alternativ är:

* **D 9876** -hello lokal port som dirigerar trafik via hello-tunnel.
* **C** -Komprimera alla data eftersom webbtrafik är främst text.
* **2** -force SSH tootry-protokollversion 2 endast.
* **q** -tyst läge.
* **T** -inaktivera pseudokolumner tty-allokering, eftersom vi bara vidarebefordrar en port.
* **n**-Förhindra läsningen av STDIN, eftersom vi bara vidarebefordrar en port.
* **N** -inte köra fjärrkommandon, eftersom vi bara vidarebefordrar en port.
* **f** -körs i bakgrunden hello.

När hello-kommandot har slutförts, är trafik som skickas tooport 9876 på hello lokal dator routade toohello klustrets huvudnod.

## <a name="useputty"></a>Skapa en tunnel med PuTTY

[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) är en grafisk SSH-klient för Windows. Använd hello följande steg toocreate en SSH-tunnel med PuTTY:

1. Öppna PuTTY och ange anslutningsinformationen. Om du inte är bekant med PuTTY finns hello [PuTTY-dokumentation (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).

2. I hello **kategori** avsnitt toohello till vänster i dialogrutan hello, expandera **anslutning**, expandera **SSH**, och välj sedan **tunnlar**.

3. Ange följande information på hello hello **alternativ som styr SSH-port vidarebefordran** formulär:
   
   * **Källport** -hello port på hello-klient som du vill tooforward. Till exempel **9876**.

   * **Mål** -hello SSH-adressen för hello Linux-baserade HDInsight-kluster. Till exempel **mycluster-ssh.azurehdinsight.net**.

   * **Dynamisk** – Aktiverar dynamisk SOCKS-proxyroutning.
     
     ![Bild av tunneling alternativ](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. Klicka på **Lägg till** tooadd hello inställningar och klickar sedan på **öppna** tooopen en SSH-anslutning.

5. När du uppmanas att logga in toohello server.

## <a name="use-hello-tunnel-from-your-browser"></a>Använd hello tunnel från din webbläsare

> [!IMPORTANT]
> hello stegen i det här avsnittet Använd hello Mozilla FireFox webbläsaren, eftersom det ger hello samma proxyinställningar på alla plattformar. Andra moderna webbläsare, till exempel Google Chrome kan kräva ett tillägg, till exempel FoxyProxy toowork med hello-tunnel.

1. Konfigurera hello webbläsare toouse **localhost** och hello-port som du använde när du skapar hello tunnel som en **SOCKS v5** proxy. Här är hur hello Firefox inställningar se ut. Om du använder en annan port än 9876, ändra hello port toohello du används:
   
    ![Bild av Firefox inställningar](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]
   > Att välja **fjärr-DNS** löser Domain Name System (DNS)-begäranden med hjälp av hello HDInsight-kluster. Den här inställningen löser DNS med hello huvudnod hello-klustret.

2. Kontrollera att hello-tunnel fungerar genom att gå till en plats som [http://www.whatismyip.com/](http://www.whatismyip.com/). hello IP returnerade något som användas av hello Microsoft Azure-datacenter.

## <a name="verify-with-ambari-web-ui"></a>Kontrollera med Ambari-webbgränssnittet

När hello kluster har upprättats använder du följande steg tooverify att du kan komma åt tjänsten web användargränssnitt från hello Ambari Web hello:

1. Gå toohttp://headnodehost:8080 i webbläsaren. Hej `headnodehost` adress skickas över hello tunnel toohello klustret och lösa toohello headnode som Ambari körs på. När du uppmanas ange hello administratörsanvändarnamnet (admin) och lösenord för klustret. Du kan uppmanas en gång av hello Ambari-webbgränssnittet. I så fall, ange hello information.

   > [!NOTE]
   > När du använder hello http://headnodehost:8080 adress tooconnect toohello klustret måste ansluter du via hello-tunnel. Kommunikationen säkras med hello SSH-tunnel i stället för HTTPS. tooconnect över Hej internet med hjälp av HTTPS, använder https://CLUSTERNAME.azurehdinsight.net, där **KLUSTERNAMN** är hello hello klustrets namn.

2. Välj HDFS hello listan hello vänster på sidan hello hello Ambari-Webbgränssnittet.

    ![Bild med HDFS som valts](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. När hello HDFS tjänstinformation visas, väljer du **snabblänkar**. En lista över hello-klustrets huvudnoder visas. Välj en av hello huvudnoderna och välj sedan **NameNode UI**.

    ![Bild med hello snabblänkar menyn expanderas](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

   > [!NOTE]
   > När du väljer __snabblänkar__, kan du få en indikator för vänta. Det här tillståndet kan uppstå om du har en långsam Internetanslutning. Vänta en minut eller två för hello data toobe togs emot från hello-servern och försök sedan hello listan igen.
   >
   > Vissa poster i hello **snabblänkar** menyn kan vara kapa av hello höger sida av hello-skärmen. I så fall, expandera hello-menyn med hjälp av musen och använda hello HÖGERPIL viktiga tooscroll hello skärmen toohello rätt toosee hello resten av hello-menyn.

4. En sida liknande toohello följande bild visas:

    ![Bild av hello NameNode UI](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

   > [!NOTE]
   > Lägg märke till hello URL för den här sidan. Det bör likna för**http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/kluster**. URI: N använder hello interna fullständigt kvalificerade domännamnet (FQDN) för hello nod och är bara tillgänglig när du använder en SSH-tunnel.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur toocreate och använder en SSH-tunnel finns i följande dokument för andra sätt toouse Ambari hello:

* [Hantera HDInsight-kluster med Ambari](hdinsight-hadoop-manage-ambari.md)

Mer information om hur du använder SSH med HDInsight finns [använda SSH med HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).

