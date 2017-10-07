---
title: "aaaConfigure domänanslutna HDInsight-kluster - Azure | Microsoft Docs"
description: "Lär dig hur tooset upp och konfigurera domänanslutna HDInsight-kluster"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 0cbb49cc-0de1-4a1a-b658-99897caf827c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/02/2016
ms.author: saurinsh
ms.openlocfilehash: 8c4b3d269a7662d27a49b839e5cd05a3e24f7023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-domain-joined-hdinsight-clusters-preview"></a>Konfigurera domänanslutna HDInsight-kluster (förhandsgranskning)

Lär dig hur tooset upp ett Azure HDInsight-kluster med Azure Active Directory (Azure AD) och [Apache Ranger](http://hortonworks.com/apache/ranger/) tootake nytta av stark autentisering och omfattande rollbaserad åtkomstprinciper för åtkomstkontroll (RBAC).  Domänanslutna HDInsight kan bara konfigureras på Linux-baserade kluster. Mer information finns i [införa domänanslutna HDInsight-kluster](hdinsight-domain-joined-introduction.md).

> [!IMPORTANT]
> Oozie har inte aktiverats på domänanslutna HDInsight.

Den här artikeln är hello första kursen i en serie:

* Skapa ett HDInsight klustret anslutna tooAzure AD (via funktionen för hello Azure Directory Domain Services) med Apache Ranger aktiverad.
* Skapa och tillämpa principer för Hive via Apache Ranger och Tillåt att användare (till exempel datavetare) tooconnect tooHive med hjälp av ODBC-baserade verktyg, till exempel Excel, Tableau osv. Microsoft arbetar för att lägga till andra arbetsbelastningar, t.ex HBase, Spark, Storm och tooDomain ansluten HDInsight snart.

Ett exempel på hello slutliga topologi ser ut som följer:

![Domänanslutna HDInsight-topologi](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-topology.png)

Eftersom Azure AD för närvarande stöder endast klassiska virtuella nätverk (Vnet) och Linux-baserade HDInsight-kluster endast stöder Azure Resource Manager baserade Vnet, HDInsight Azure AD-integration kräver två Vnet och en peering mellan dem. Hello jämförelseinformation mellan hello två distributionsmodeller finns [Azure Resource Manager och klassisk distribution: Förstå distributionsmodeller och hello för dina resurser](../azure-resource-manager/resource-manager-deployment-model.md). hello två Vnet måste vara i hello samma region som hello Azure AD DS.

Azure-Tjänstenamn måste vara globalt unika. hello följande namn används i den här kursen. Contoso är ett fiktivt namn. Du måste ersätta *contoso* med ett annat namn när du går igenom kursen hello. 

**Namn:**

| Egenskap | Värde |
| --- | --- |
| Azure AD-VNet |contosoaadvnet |
| Azure AD-Vnet-resursgrupp |contosoaadrg |
| Azure AD-katalog |contosoaaddirectory |
| Azure AD-domännamn |Contoso (contoso.onmicrosoft.com) |
| HDInsight-VNet |contosohdivnet |
| HDInsight VNet resursgruppen. |contosohdirg |
| HDInsight-kluster |contosohdicluster |

Den här självstudiekursen innehåller hello steg för att konfigurera en domänansluten HDInsight-kluster. Varje avsnitt har länkar tooother artiklar med mer information.

## <a name="prerequisite"></a>Förutsättning:
* Bekanta dig med [Azure AD Domain Services](https://azure.microsoft.com/services/active-directory-ds/) dess [priser](https://azure.microsoft.com/pricing/details/active-directory-ds/) struktur.
* Se till att prenumerationen är godkända för den här public preview. Du kan göra det genom att skicka ett e-postmeddelande toohdipreview@microsoft.com med ditt prenumerations-ID.
* Ett SSL-certifikat som signerats av en utfärdare för signering för din domän. Det krävs hello certifikat genom att konfigurera säker LDAP. Självsignerade certifikat kan inte användas.

## <a name="procedures"></a>Procedurer
1. Skapa ett Azure klassiska virtuella nätverk för din Azure AD.  
2. Skapa och konfigurera Azure AD och Azure AD DS.
3. Skapa ett HDInsight-VNet i hanteringsläge för hello Azure-resurs.
4. Peer hello två Vnet.
5. Skapa ett HDInsight-kluster.

> [!NOTE]
> Den här kursen förutsätter att du inte har en Azure AD. Om du har ett kan du hoppa över hello-delen i steg 2.
> 
> 

Det finns ett PowerShell-skript som automatiserar steg 3 till 7.  Mer information finns i [konfigurera domänanslutna HDInsight-kluster använder du Azure PowerShell](hdinsight-domain-joined-configure-use-powershell.md).

## <a name="create-an-azure-virtual-network-classic"></a>Skapa ett Azure-nätverk (klassisk)
I det här avsnittet skapar du virtuella nätverk (klassiskt) med hjälp av hello Azure-portalen. I nästa avsnitt om hello aktivera hello Azure AD DS för din Azure AD i hello virtuella nätverk. Mer information om hello sätt och använda andra metoder för att skapa virtuella nätverk finns [skapa ett virtuellt nätverk (klassiskt) med hjälp av hello Azure-portalen](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).

**toocreate ett klassiskt virtuellt nätverk**

1. Logga in toohello [Azure-portalen](https://portal.azure.com). 
2. Klicka på **nya** > **nätverk** > **virtuellt nätverk**.
3. I **Välj en distributionsmodell**väljer **klassiska**, och klicka sedan på **skapa**.
4. Ange eller välj hello följande värden:
   
   * **Namn på**: contosoaadvnet
   * **Adressutrymmet**: 10.1.0.0/16
   * **Undernätnamnet**: Undernät1
   * **Adressintervall för gatewayundernät**: 10.1.0.0/24
   * **Prenumerationen**: (Välj en prenumeration som används för att skapa detta virtuella nätverk).
   * **ResourceGroup**: contosoaadrg
   * **Plats**: (Välj en region för din HDInsight-kluster).
     
     > [!IMPORTANT]
     > Du måste välja en plats som stöder Azure AD DS. Mer information finns i [Produkttillgänglighet per region](https://azure.microsoft.com/en-us/regions/services/). 
     > 
     > Både hello klassiska virtuella nätverk och hello resurs grupp VNet måste vara i hello samma region som hello Azure AD DS.
     > 
     > 
5. Klicka på **skapa** toocreate hello virtuella nätverk.

## <a name="create-and-configure-azure-ad-ds-for-your-azure-ad"></a>Skapa och konfigurera Azure AD DS för din Azure AD
I det här avsnittet kommer du att:

1. Skapa en Azure AD.
2. Skapa Azure AD-användare. Dessa användare är domänanvändare. Du kan använda hello första användaren för att konfigurera hello HDInsight-kluster med hello Azure AD.  hello är andra två användare valfria för den här självstudiekursen. De kommer att användas i [konfigurera Hive principer för domänanslutna HDInsight-kluster](hdinsight-domain-joined-run-hive.md) när du konfigurerar Apache Ranger principer.
3. Skapa hello AAD DC administratörsgruppen och Lägg till användargrupp för toohello på hello Azure AD. Du kan använda den här toocreate hello organisationsenheten för användaren.
4. Aktivera Azure AD Domain Services (Azure AD DS) för hello Azure AD.
5. Konfigurera LDAPS för hello Azure AD. hello Lightweight Directory Access Protocol (LDAP) är används tooread från och skriva tooAzure AD.

Om du vill toouse en befintlig Azure AD kan du hoppa över steg 1 och 2.

**toocreate en Azure AD**

1. Från hello [klassiska Azure-portalen](https://manage.windowsazure.com), klickar du på **ny** > **Apptjänster** > **Active Directory**  >  **Directory** > **skapa anpassade**. 
2. Ange eller välj hello följande värden:
   
   * **Namn på**: contosoaaddirectory
   * **Domännamn**: contoso.  Det här namnet måste vara globalt unika.
   * **Land eller region**: Välj land eller region.
3. Klicka på **Complete** (Slutför).

**Skapa en Azure AD-användare**

1. Från hello [klassiska Azure-portalen](https://manage.windowsazure.com), klickar du på **Active Directory** -> **contosoaaddirectory**. 
2. Klicka på **användare** hello översta menyn.
3. Klicka på **lägga till användare**.
4. Ange **användarnamn**, och klicka sedan på **nästa**. 
5. Konfigurera användarprofiler; I **rollen**väljer **Global administratör**; och klicka sedan på **nästa**.  hello rollen som Global administratör är nödvändiga toocreate organisationsenheter.
6. Klicka på **skapa** tooget ett tillfälligt lösenord.
7. Skapa en kopia av hello lösenord och klicka sedan på **Slutför**. Senare i den här kursen använder den här global administratör användaren toocreate hello HDInsight-kluster.

Följ hello samma procedur toocreate två fler användare med hello **användaren** roll, hiveuser1 och hiveuser2. hello följande användare kommer att användas i [konfigurera Hive principer för domänanslutna HDInsight-kluster](hdinsight-domain-joined-run-hive.md).

**toocreate hello AAD DC administratörer och Lägg till en Azure AD-användare**

1. Från hello [klassiska Azure-portalen](https://manage.windowsazure.com), klickar du på **Active Directory** > **contosoaaddirectory**. 
2. Klicka på **grupper** hello översta menyn.
3. Klicka på **lägga till en grupp** eller **Lägg till grupp**.
4. Ange eller välj hello följande värden:
   
   * **Namn på**: AAD DC-administratörer.  Ändra inte hello gruppnamn.
   * **Grupptyp**: säkerhet.
5. Klicka på **Complete** (Slutför).
6. Klicka på **AAD DC administratörer** tooopen hello grupp.
7. Klicka på **lägga till medlemmar**.
8. Välj hello första användare som du skapade i föregående steg i hello och klicka sedan på **Slutför**.
9. Upprepa hello samma steg toocreate en annan grupp som kallas **HiveUsers**, och Lägg till hello två Hive toohello användargruppen.

Mer information finns i [Azure AD Domain Services (förhandsvisning) – skapa hello AAD DC-administratörer grupp](../active-directory-domain-services/active-directory-ds-getting-started.md).

**tooenable Azure AD DS för din Azure AD**

1. Från hello [klassiska Azure-portalen](https://manage.windowsazure.com), klickar du på **Active Directory** > **contosoaaddirectory**. 
2. Klicka på **konfigurera** hello översta menyn.
3. Rulla nedåt för**Domain Services**, och ange hello följande värden:
   
   * **Aktivera domain services för den här katalogen**: Ja.
   * **DNS-domännamnet för domäntjänster**: Detta visar hello DNS för standardnamnet för hello Azure-katalogen. Till exempel contoso.onmicrosoft.com.
   * **Ansluta domain services toothis virtuellt nätverk**: Välj hello klassiska virtuella nätverk som du skapade tidigare, d.v.s. **contosoaadvnet**.
4. Klicka på **spara** från hello längst ned på sidan för hello. Du kommer att se **väntande...**  nästa för**aktivera domain services för den här katalogen**.  
5. Vänta tills **väntande...**  försvinner och **IP-adress** hämtar fylls i. Två IP-adresser ska hämta fylls i. Dessa är hello IP-adresser för hello domänkontrollanter som etablerats av Domain Services. Varje IP-adress kommer att vara synliga när hello motsvarande domänkontrollanten är allokerade och redo. Skriv ned hello två IP-adresser. Du behöver dem senare.

Mer information finns i [Azure AD Domain Services (förhandsvisning) – Aktivera Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-enableaadds.md).

**toosynchronize lösenord**

Om du använder en egen domän behöver du toosynchronize hello lösenord. Se [Aktivera synkronisering av lösenord tooAzure AD DS för en molnbaserad Azure AD-katalog](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md).

**tooconfigure LDAPS för hello Azure AD**

1. Hämta ett SSL-certifikat som signerats av en utfärdare för signering för din domän. Om du vill toouse ett självsignerat certifikat, nå ut toohdipreview@microsoft.com för ett undantag.
2. Från hello [klassiska Azure-portalen](https://manage.windowsazure.com), klickar du på **Active Directory** > **contosoaaddirectory**. 
3. Klicka på **konfigurera** hello översta menyn.
4. Rulla för**domäntjänster**.
5. Klicka på **konfigurera certifikat**.
6. Följ hello instruktion toospecify hello certifikatfilen och hello lösenord. Du kommer att se **väntande...**  nästa för**aktivera domain services för den här katalogen**.  
7. Vänta tills **väntande...**  försvinner och **säker LDAP-certifikat** har fyllts i.  Detta kan ta 10 minuter eller mer.

> [!NOTE]
> Om vissa bakgrundsaktiviteter körs på hello Azure AD DS, visas ett fel när du laddar upp certifikatet - <i>det finns en åtgärd som utförs för den här klienten. Försök igen senare</i>.  Om det här felet uppstår. Försök igen senare. hello andra domänkontrollantens IP kan ta upp too3 timmar toobe etableras.
> 
> 

Mer information finns i [konfigurera säker LDAP (LDAPS) för en Azure AD Domain Services-hanterad domän](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-resource-manager-vnet-for-hdinsight-cluster"></a>Skapa ett VNet för Resource Manager för HDInsight-kluster
I det här avsnittet skapar du en Azure Resource Manager-VNet som ska användas för hello HDInsight-kluster. Mer information om hur du skapar Azure VNET med andra metoder finns [skapa ett virtuellt nätverk](../virtual-network/virtual-networks-create-vnet-arm-pportal.md)

När du har skapat hello VNet, konfigurerar du hello Resource Manager VNet toouse hello samma DNS-servrar som för hello Azure AD-VNet. Om du har följt hello stegen i den här självstudiekursen toocreate hello klassiska VNet och hello Azure AD hello DNS-servrar är 10.1.0.4 och 10.1.0.5.

**toocreate en Resource Manager-VNet**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **ny**, **nätverk**, och sedan **för virtuella nätverk**. 
3. I **Välj en distributionsmodell**väljer **Resource Manager**, och klicka sedan på **skapa**.
4. Ange eller välj hello följande värden:
   
   * **Namn på**: contosohdivnet
   * **Adressutrymmet**: 10.2.0.0/16. Kontrollera att hello-adressintervall inte kan överlappa hello IP-adressintervall hello klassiska virtuella nätverk.
   * **Undernätnamnet**: Undernät1
   * **Adressintervall för gatewayundernät**: 10.2.0.0/24
   * **Prenumerationen**: (Välj din Azure-prenumeration.)
   * **Resursgruppen**: contosohdirg
   * **Plats**: (Välj hello samma plats som hello Azure AD VNet, d.v.s. contosoaadvnet.)
5. Klicka på **Skapa**.

**tooconfigure DNS för hello Resource Manager VNet**

1. Från hello [Azure-portalen](https://portal.azure.com), klickar du på **fler tjänster** -> **virtuella nätverken**. Se till att inte tooclick **virtuella nätverk (klassiskt)**.
2. Klicka på **contosohdivnet**.
3. Klicka på **DNS-servrar** från hello vänster sida av hello nytt blad.
4. Klicka på **anpassade**, och ange sedan hello följande värden:
   
   * 10.1.0.4
   * 10.1.0.5
     
     Dessa IP-adresser för DNS-servrar måste matcha toohello DNS-servrar i hello Azure AD-VNet (klassiska VNet).
5. Klicka på **Spara**.

## <a name="peer-hello-azure-ad-vnet-and-hello-hdinsight-vnet"></a>Peer hello Azure AD VNet och hello HDInsight VNet
**toopeer hello två virtuella nätverk**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **fler tjänster** hello vänstra menyn.
3. Klicka på **virtuella nätverk**. Klicka inte på **virtuella nätverk (klassiskt)**.
4. Klicka på **contosohdivnet**.  Detta är hello HDInsight VNet.
5. Klicka på **Peerkopplingar** på hello vänstra menyn i hello-bladet.
6. Klicka på **Lägg till** hello översta menyn. Hello öppnas **lägga till peering** bladet.
7. På hello **Lägg till peering** bladet, ange eller välj hello följande värden:
   
   * **Namn på**: ContosoAADHDIVNetPeering
   * **Virtuellt nätverk distributionsmodell**: klassiska
   * **Prenumerationen**: väljer prenumerationsnamn används för hello klassisk (Azure AD) vnet.
   * **Virtuellt nätverk**: contosoaadvnet.
   * **Tillåt åtkomst till virtuella nätverk**: (kontrollera)
   * **Tillåt vidarebefordrad trafik**: (kontrollera). Lämna hello andra två kryssrutor avmarkerade.
8. Klicka på **OK**.

## <a name="create-hdinsight-cluster"></a>Skapa HDInsight-kluster
I det här avsnittet skapar du ett Linux-baserade Hadoop-kluster i HDInsight med antingen hello Azure-portalen eller [Azure Resource Manager-mall](../azure-resource-manager/resource-group-template-deploy.md). För andra metoder för att skapa kluster och förstå hello inställningar, se [skapa HDInsight-kluster](hdinsight-hadoop-provision-linux-clusters.md). För mer information om hur du använder Resource Manager mallen toocreate Hadoop-kluster i HDInsight, se [skapa Hadoop-kluster i HDInsight med hjälp av Resource Manager-mallar](hdinsight-hadoop-create-windows-clusters-arm-templates.md)

**En domänansluten HDInsight-kluster med toocreate hello Azure-portalen**

1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. Klicka på **ny**, **Intelligence + analys**, och sedan **HDInsight**.
3. Från hello **nya HDInsight-kluster** bladet ange eller välj hello följande värden:
   
   * **Klusternamnet**: Ange ett nytt namn för klustret för hello domänanslutna HDInsight-kluster.
   * **Prenumerationen**: Välj en Azure-prenumeration används för att skapa det här klustret.
   * **Klusterkonfiguration**:
     
     * **Klustret typen**: Hadoop. Domänanslutna HDInsight är för närvarande endast stöds på Hadoop-kluster.
     * **Operativsystemet**: Linux.  Domänanslutna HDInsight stöds bara på Linux-baserade HDInsight-kluster.
     * **Version**: HDI 3,6. Domänanslutna HDInsight stöds bara på HDInsight-kluster av version 3,6.
     * **Klustret typen**: PREMIUM
       
       Klicka på **Välj** toosave hello ändringar.
   * **Autentiseringsuppgifter**: Konfigurera hello autentiseringsuppgifter för både hello klustret användar- och hello SSH-användare.
   * **Datakällan**: skapa ett nytt lagringskonto eller använda ett befintligt lagringskonto som hello standardkontot för lagring för hello HDInsight-kluster. hello plats måste hello samtidigt som hello två Vnet.  hello plats är också hello platsen för hello HDInsight-kluster.
   * **Priser**: Välj hello antalet arbetarnoder i klustret.
   * **Avancerade konfigurationer**: 
     
     * **Domänanslutning & Vnet /-undernätet**: 
       
       * **Domäninställningar**: 
         
         * **Domännamn**: contoso.onmicrosoft.com
         * **Namnet på användaren**: Ange ett användarnamn för domänen. Den här domänen måste ha följande behörigheter hello: ansluta till datorer toohello domän och placera dem i hello-organisationsenhet som du anger när klustret skapas; Skapa tjänstens huvudnamn i hello-organisationsenhet som du anger när klustret skapas; Skapa omvänd DNS-poster. Den här domänanvändare blir Hej administratör för den här datorn är domänansluten HDInsight-kluster.
         * **Domänlösenord**: Ange hello domänanvändarlösenord.
         * **Organisationsenheten**: Ange hello unika namnet för hello Organisationsenhet som du vill toouse med HDInsight-kluster. Till exempel: OU = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com. Om Organisationsenheten inte finns, försöker toocreate Organisationsenheten med HDInsight-kluster. Kontrollera att hello Organisationsenheten finns redan eller hello domänkonto som har behörigheter toocreate en ny. Om du använder hello-domänkonto som ingår i AADDC administratörer har behörighet för toocreate hello Organisationsenhet.
         * **LDAPS URL**: ldaps://contoso.onmicrosoft.com:636
         * **Åtkomst-användargrupp**: Ange hello säkerhetsgrupp vars användare du vill toosync toohello klustret. Till exempel HiveUsers.
           
           Klicka på **Välj** toosave hello ändringar.
           
           ![Domänanslutna HDInsight portal konfigurera domäninställningen](./media/hdinsight-domain-joined-configure/hdinsight-domain-joined-portal-domain-setting.png)
       * **Virtuellt nätverk**: contosohdivnet
       * **Undernät**: Undernät1
         
         Klicka på **Välj** toosave hello ändringar.        
         Klicka på **Välj** toosave hello ändringar.
   * **Resursgruppen**: Välj hello resursgruppens namn används för hello HDInsight VNet (contosohdirg).
4. Klicka på **Skapa**.  

Ett annat alternativ för att skapa domänanslutna HDInsight-kluster är toouse Azure Resource Manager-mallen. hello följande procedur visas hur:

**toocreate ett domänanslutna HDInsight-kluster med hjälp av en Resource Manager-mall**

1. Klicka på hello följande bild tooopen en Resource Manager-mall i hello Azure-portalen. hello Resource Manager-mall finns i en offentlig blob-behållare. 
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-domain-joined-hdinsight-cluster.json" target="_blank"><img src="./media/hdinsight-domain-joined-configure/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Från hello **parametrar** bladet ange hello följande värden:
   
   * **Prenumerationen**: (Välj din Azure-prenumeration).
   * **Resursgruppen**: Klicka på **använda befintliga**, och ange hello samma resursgrupp som du har använt.  Till exempel contosohdirg. 
   * **Plats**: Ange en resursgruppens plats.
   * **Klusternamn**: Ange ett namn för hello Hadoop-kluster som du vill skapa. Till exempel contosohdicluster.
   * **Klustret typen**: Välj en typ av kluster.  hello standardvärdet är **hadoop**.
   * **Plats**: Välj en plats för hello klustret.  hello standardkontot för lagring använder hello samma plats.
   * **Klustret Arbetsnoden antal**: Välj hello antalet arbetarnoder.
   * **Klustrets inloggningsnamn och lösenord**: hello standard inloggningsnamnet är **admin**.
   * **SSH-användarnamn och lösenord**: hello Standardanvändarnamnet är **sshuser**.  Du kan byta namn. 
   * **Virtuella nätverks-Id**: /subscriptions/&lt;SubscriptionID > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >
   * **Virtuella undernät**: /subscriptions/&lt;SubscriptionID > /resourceGroups/&lt;ResourceGroupName > /providers/Microsoft.Network/virtualNetworks/&lt;VNetName >/undernät/Undernät1
   * **Domännamn**: contoso.onmicrosoft.com
   * **Organisation enhet DN**: OU = HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com
   * **Användare klustergrupp DNs**: [\"HiveUsers\"]
   * **LDAPUrls**: [”ldaps://contoso.onmicrosoft.com:636”]
   * **DomainAdminUserName**: (Ange hello domain admin användarnamn)
   * **DomainAdminPassword**: (Ange hello domänanvändarlösenord för admin)
   * **Jag accepterar toohello villkoren ovan**: (kontrollera)
   * **PIN-kod toodashboard**: (kontrollera)
3. Klicka på **Köp**. En ny panel visas med rubriken **Skicka malldistribution**. Det tar cirka 20 minuter toocreate ett kluster. När hello klustret har skapats måste du klicka på hello klusterbladet i hello portal tooopen den.

När du har slutfört hello kursen kanske du vill toodelete hello klustret. Med HDInsight lagras dina data i Azure Storage så att du på ett säkert sätt kan ta bort ett kluster när det inte används. Du debiteras också för ett HDInsight-kluster, även när det inte används. Eftersom hello avgifterna för hello klustret är flera gånger större än hello avgifterna för lagring, är det ekonomiskt meningsfullt toodelete kluster när de inte används. Hello instruktioner för att ta bort ett kluster finns i [hantera Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen](hdinsight-administer-use-management-portal.md#delete-clusters).

## <a name="next-steps"></a>Nästa steg
* Om du vill konfigurera Hive-principer och köra Hive-frågor kan du läsa i [Konfigurera Hive-principer för domänanslutna HDInsight-kluster](hdinsight-domain-joined-run-hive.md).
* Använda SSH-tooconnect tooDomain ansluten HDInsight-kluster finns i [använda SSH med Linux-baserade Hadoop i HDInsight från Linux, Unix eller OS X](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

