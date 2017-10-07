---
title: "aaaSet in kontinuerlig integration för Azure mikrotjänster | Microsoft Docs"
description: "Få en översikt över hur tooset in kontinuerlig integrering och distribution för ett Service Fabric-program med hjälp av Visual Studio Team Services VSTS ()."
services: service-fabric
documentationcenter: na
author: mthalman-msft
manager: timlt
editor: 
ms.assetid: 3e8c2290-9e7a-456a-9b2c-db44d1b3988d
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 12/06/2016
ms.author: mthalman;mikhegn
ms.openlocfilehash: 041e081f4a42f379394f2d21f07db4f6de045b24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-service-fabric-continuous-integration-and-deployment-with-visual-studio-team-services"></a>Konfigurera Service Fabric kontinuerlig integrering och distribution med Visual Studio Team Services
Den här artikeln beskriver hello steg tooset in kontinuerlig integrering och distribution för ett Azure Service Fabric-program med hjälp av Visual Studio Team Services VSTS ().

Det här dokumentet visar hello aktuella proceduren och är förväntade toochange över tid.

## <a name="prerequisites"></a>Krav
tooget har startats så här:

1. Kontrollera att du har åtkomst tooa Team Services-konto eller [skapar du en](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services) själv.
2. Kontrollera att du har åtkomst tooa Team Services grupprojekt eller [skapar du en](https://www.visualstudio.com/docs/setup-admin/create-team-project) själv.
3. Kontrollera att du har ett Service Fabric-klustret toowhich du kan distribuera ditt program eller skapa en med hjälp av hello [Azure-portalen](service-fabric-cluster-creation-via-portal.md), en [Azure Resource Manager-mall](service-fabric-cluster-creation-via-arm.md), eller [Visual Studio ](service-fabric-cluster-creation-via-visual-studio.md).
4. Se till att du redan har skapat ett Service Fabric-program (.sfproj)-projekt. Du måste ha ett projekt som har skapats eller uppgraderas med Service Fabric SDK 2.1 eller högre (hello .sfproj filen ska innehålla ett egenskapsvärde för ProjectVersion 1.1 eller högre).

> [!NOTE]
> Anpassade build agenter inte längre behövs. Team Services finns agenter som nu levereras förinstallerade med Service Fabric klusterhantering för distribution av dina program direkt från dessa agenter.
> 
> 

## <a name="configure-and-share-your-source-files"></a>Konfigurera och dela dina källfiler
hello första som du vill använda toodo är förbereda en publiceringsprofil för användning av hello Distributionsprocess som körs i Team Services.  hello publiceringsprofil bör vara konfigurerade tootarget hello som du har förberett tidigare:

1. Välj en publiceringsprofil inom projektet som du vill toouse för kontinuerlig integration arbetsflödet. Följ hello [publicera instruktioner](service-fabric-publish-app-remote-cluster.md) om hur toopublish ett program tooa kluster. Behöver du inte verkligen toopublish tillämpningsprogrammet om. Du kan klicka på hello **spara** hyperlänk i hello publicera dialogrutan när du har konfigurerat saker på lämpligt sätt.
2. Om du vill att din ansökan toobe uppgraderas för varje distribution sker inom Team Services, vill du tooconfigure hello Publicera profil tooenable uppgraderingen. Se till att hello hello samma publicera dialogrutan används i steg 1, **uppgradera hello programmet** är markerad.  Lär dig mer om [konfigurera ytterligare inställningar](service-fabric-visualstudio-configure-upgrade.md). Klicka på hello **spara** hyperlink toosave hello inställningar toohello Publicera profil.
3. Kontrollera att du har sparat ändringarna toohello Publicera profil och avbryta hello publicera dialogrutan.
4. Nu är det dags för[dela källfilerna för programmet projektet](https://www.visualstudio.com/docs/setup-admin/team-services/connect-to-visual-studio-team-services#vs) med Team Services. När källfilerna är tillgängliga i Team Services, kan du nu flytta på toohello nästa steg att skapa versioner. 

## <a name="create-a-build-definition"></a>Skapa en build-definition
En definition av Team Services build beskriver ett arbetsflöde som består av en uppsättning build-åtgärder som utförs i tur och ordning. hello målet för hello build definition som du skapar är tooproduce ett Service Fabric-programpaket och andra artefakter som kan använda toodeploy hello program. Mer information om Team Services [skapa definitioner](https://www.visualstudio.com/docs/build/define/create).

### <a name="create-a-definition-from-hello-build-template"></a>Skapa en definition från hello build-mall
1. Öppna team projekt i Visual Studio Team Services.
2. Välj hello **skapa** fliken.
3. Välj hello grönt  **+**  logga toocreate en ny build-definition.
4. I hello dialogrutan som öppnas väljer **Azure Service Fabric-programmet** inom hello **skapa** mallkategori.
5. Välj **nästa**.
6. Välj hello databasen och gren kopplad till Service Fabric-programmet.
7. Välj hello agent kön som du vill toouse. Värdbaserade agenter stöds.
8. Välj **Skapa**.
9. Spara hello build definition och ange ett namn.
10. hello är följande punkt en beskrivning av hello build steg som genererats av hello mallen:

| Skapa steg | Beskrivning |
| --- | --- |
| NuGet-återställning |Återställer hello NuGet-paket för hello lösningen. |
| Skapa lösningen \*SLN |Skapar hello hela lösningen. |
| Skapa lösningen \*.sfproj |Genererar hello Service Fabric-programpaket som är används toodeploy hello program. hello programmet paketplatsen är angivna toobe i hello build artefakt katalog. |
| Uppdatera Appversioner för Service Fabric |Uppdaterar hello Versionsvärden i hello programmet paketet manifestfiler tooallow för stöd för uppgradering. Se hello [dokumentationssidan för aktiviteten](https://go.microsoft.com/fwlink/?LinkId=820529) för mer information. |
| Kopiera filer |Kopior hello Publicera profil och programmet parametrar filer toohello build's artefakter toobe används för distribution. |
| Publicera artefakt |Publicerar hello build-artefakter. Gör en definition av versionen tooconsume hello build-artefakter. |

### <a name="verify-hello-default-set-of-tasks"></a>Kontrollera hello standarduppsättning aktiviteter
1. Kontrollera hello **lösning** inmatningsfältet för hello **NuGet återställning** och **skapa lösning** skapa steg.  Som standard build steg köras på alla lösningsfiler som finns i hello associerade databasen.  Om du bara vill hello build definition toooperate på en av filerna lösningen måste tooexplicitly hello sökvägen toothat uppdateringsfil.
2. Kontrollera hello **lösning** inmatningsfältet för hello **paketera program** skapa steg.  Som standard förutsätter det här build-steget endast ett tjänstprogram för Fabric-projekt (.sfproj) finns i hello-databasen.  Om du har flera filer i databasen och vill tootarget endast en av dem för denna build-definition, måste tooexplicitly hello sökvägen toothat uppdateringsfil.  Om du vill toopackage flera program projekt i databasen, måste toocreate ytterligare **Visual Studio skapa** stegen i hello build definition varje mål Application-projekt.  Du sedan måste också tooupdate hello **MSBuild-argument** fält för var och en av de skapa steg så att hello paketplatsen är unika för dem.
3. Kontrollera hello versionshantering beteende som definierats i hello **App Uppdateringsversioner Service Fabric** skapa steg.  Som standard läggs det här steget Skapa hello build tooall version siffervärdena i hello programmet paketet manifest-filer. Se hello [dokumentationssidan för aktiviteten](https://go.microsoft.com/fwlink/?LinkId=820529) för mer information. Detta är användbart för att stödja uppgradering av ditt program eftersom varje uppgradera distributionen kräver olika Versionsvärden från tidigare hello-distribution. Om du inte avsikten är toouse Programuppgradering i arbetsflödet, kan du inaktivera det här build-steget. Det måste inaktiveras om din avsikt är tooproduce en version som kan vara används toooverwrite ett befintligt Service Fabric-program. hello distributionen misslyckas om hello version av hello-programmet som produceras av hello build inte matchar hello version av programmet hello i hello klustret.
4. Om din lösning innehåller ett .NET Core-projekt, måste du kontrollera att din build-definition innehåller ett build-steg som återställer hello beroenden:
   1. Välj **Lägg till build steg...** .
   2. Leta upp hello **kommandoradsverktyget** aktiviteten inom hello verktyget fliken och klicka på knappen Lägg till.
   3. Hello aktivitetens inmatningsfält, Använd hello följande värden:
   4. Verktyget: dotnet
   5. Argument: återställa
   6. Dra hello uppgift så att den är omedelbart efter hello **NuGet återställning** steg.
5. Spara alla ändringar har du gjort toohello build-definition.

### <a name="try-it"></a>Prova
Välj **kön skapa** toomanually starta en version. Bygger också utlösare på push eller incheckning. När du har kontrollerat att hello build körs korrekt kan flytta du nu en definition av versionen som distribuerar programmet tooa klustret på toodefining.

## <a name="create-a-release-definition"></a>Skapa en definition för versionen
En definition av Team Services versionen beskriver ett arbetsflöde som består av en uppsättning uppgifter som utförs i tur och ordning. hello målet för definition av hello versionen som du skapar är tootake ett programpaket och distribuerar den tooa klustret. När de används tillsammans hello skapa definition och versionen definition kan köra hello hela arbetsflödet från att starta med källan filer tooending med ett program som körs i klustret. Mer information om Team Services [viktiga definitioner](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition).

### <a name="create-a-definition-from-hello-release-template"></a>Skapa en definition av hello versionsmall
1. Öppna projektet i Visual Studio Team Services.
2. Välj hello **versionen** fliken.
3. Välj hello grönt  **+**  toocreate en ny version definition och markera **skapa versionen definition** hello-menyn.
4. I hello dialogrutan som öppnas väljer **Azure Service Fabric-distribution** inom hello **distribution** mallkategori.
5. Välj **nästa**.
6. Välj hello build definition av du vill använda toouse som hello källan för den här versionen definitionen.  hello versionen definition referenser hello artefakter av hello valt skapa definition.
7. Kontrollera hello **kontinuerlig distribution** kryssrutan om du inte vill toohave Team Services automatiskt skapa en ny version och distribuera hello Service Fabric program när ett build har slutförts.
8. Välj hello agent kön som du vill toouse. Värdbaserade agenter stöds.
9. Välj **Skapa**.
10. Redigera namnet på definitionen av hello genom att klicka på hello pennikonen hello överst på hello sidan.
11. Välj hello klustret toowhich programmet ska distribueras från hello **klustret anslutning** inmatningsfält hello-aktivitet. hello klustret anslutning ger hello nödvändig information som gör att hello distribution uppgiften tooconnect toohello klustret. Om du ännu inte har en kluster-anslutning för klustret, väljer hello **hantera** hyperlink nästa toohello fältet tooadd en. Utför följande hello på hello sida som öppnas:
    1. Välj **nya tjänstslutpunkten** och välj sedan **Azure Service Fabric** hello-menyn.
    2. Välj hello typ av autentisering som används av hello-kluster som är mål för den här slutpunkten.
    3. Ange ett namn för anslutningen i hello **anslutningsnamn** fältet.  Normalt använder du hello namnet på klustret.
    4. Definiera hello klienten anslutning slutpunkts-URL i hello **Klusterslutpunkten** fältet.  Exempel: tcp://contoso.westus.cloudapp.azure.com:19000.
    5. Azure Active Directory-autentiseringsuppgifter för att definiera hello autentiseringsuppgifter som du vill toouse tooconnect toohello klustret i hello **användarnamn** och **lösenord** fält.
    6. Definiera för certifikatbaserad autentisering hello Base64-kodning av hello klienten certifikatfilen i hello **klientcertifikat** fältet.  Se hello hjälp popup-fönster på fältet för information om hur tooget värde.  Om certifikatet är lösenordsskyddad, definiera hello lösenord i hello **lösenord** fältet.
    7. Bekräfta ändringarna genom att klicka på **OK**. När du navigerar bakåt tooyour släpper definition klickar du på hello-ikonen på hello **klustret anslutning** fältet toosee hello slutpunkt som du just lagt till.
12. Spara definition för hello-versionen.

> [!NOTE]
> Microsoft Accounts (till exempel @hotmail.com eller @outlook.com) stöds inte med Azure Active Directory-autentisering.
> 
> 

hello definition som skapas består av en aktivitet av typen **Service Fabric-programdistribution**. Se hello [dokumentationssidan för aktiviteten](https://go.microsoft.com/fwlink/?LinkId=820528) för mer information om den här uppgiften.

### <a name="verify-hello-template-defaults"></a>Kontrollera hello mallen standardvärden
1. Kontrollera hello **Publicera profil** inmatningsfältet för hello **distribuera Fabric tjänstprogrammet** aktivitet. Det här fältet refererar till en profil med namnet Cloud.xml i hello build artefakter som standard. Om du vill tooreference en annan profil, eller om hello build innehåller flera programpaket i dess artefakter, måste tooupdate hello sökväg på lämpligt sätt.
2. Kontrollera hello **programpaket** inmatningsfältet för hello **distribuera Fabric tjänstprogrammet** aktivitet. Som standard refererar till det här fältet hello programmet paketet standardsökvägen som används i mallen för hello build-definition.  Om du har ändrat hello standardsökvägen programmet paketet i hello skapa definition måste du tooupdate hello sökväg korrekt här samt.

### <a name="try-it"></a>Prova
Välj **skapa släpper** från hello **släpper** knappen menyn toomanually skapa en version. I hello dialogrutan som öppnas väljer du hello-version som du vill använda toobase hello versionen på och klicka sedan på **skapa**. Om du har aktiverat kontinuerlig distribution kommer versioner att skapas automatiskt när hello associerade build definition har slutfört en version.

## <a name="next-steps"></a>Nästa steg
toolearn mer om kontinuerlig integration med Service Fabric-program, läsa hello följande artiklar:

* [Team Services dokumentationen hemnätverk](https://www.visualstudio.com/docs/overview)
* [Skapa hantering i Team Services](https://www.visualstudio.com/docs/build/overview)
* [Versionshantering i Team Services](https://www.visualstudio.com/docs/release/overview)

