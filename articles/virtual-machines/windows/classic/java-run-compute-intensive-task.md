---
title: "aaaCompute-intensiva Java-program på en virtuell dator | Microsoft Docs"
description: "Lär dig hur toocreate en Azure-dator som kör ett beräkningsintensiva Java-program som kan övervakas av ett annat Java-program."
services: virtual-machines-windows
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: ae6f2737-94c7-4569-9913-d871450c2827
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 02a198802a8d78bd444cd5a9197a78cb94f48e3b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toorun-a-compute-intensive-task-in-java-on-a-virtual-machine"></a>Hur toorun en beräkningsintensiva uppgift i Java på en virtuell dator
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.

Med Azure, kan du använda en virtuell dator toohandle beräkningsintensiva aktiviteter. En virtuell dator kan hantera aktiviteter och leverera resultat tooclient datorer eller mobila program. När du har läst den här artikeln kommer du ha en förståelse av hur toocreate en virtuell dator som kör ett beräkningsintensiva Java-program som kan övervakas av ett annat Java-program.

Den här kursen förutsätter att du vet hur toocreate Java konsolprogram kan importera bibliotek tooyour Java-program och kan generera ett Java-Arkiv (JAR). Ingen kunskap om Microsoft Azure antas.

Du kommer att lära dig:

* Hur toocreate en virtuell dator med en Java Development Kit (JDK) redan installerats.
* Hur tooremotely logga in tooyour virtuella datorn.
* Hur toocreate en service bus namnområde.
* Hur toocreate ett Java-program som utför en krävande uppgift.
* Hur hello toocreate ett Java-program som övervakar förloppet för hello beräkningsintensiva aktiviteten.
* Hur toorun hello Java-program.
* Hur toostop hello Java-program.

Den här kursen använder hello Traveling säljare Problem för hello beräkningsintensiva aktiviteter. hello följande är ett exempel på hello Java programmet hello beräkningsintensiva aktivitet som körs.

![Traveling säljare problemet solver][solver_output]

hello följande är ett exempel på hello Java programmet hello beräkningsintensiva övervakningsaktiviteten.

![Klienten Traveling säljare Problem][client_output]

[!INCLUDE [create-account-and-vms-note](../../../../includes/create-account-and-vms-note.md)]

## <a name="toocreate-a-virtual-machine"></a>toocreate en virtuell dator
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Klicka på **ny**, klickar du på **Compute**, klickar du på **virtuella**, och klicka sedan på **från galleriet**.
3. I hello **virtuella avbildning väljer** dialogrutan **JDK 7 Windows Server 2012**.
   Observera att **JDK 6 Windows Server 2012** är tillgänglig om du har äldre program som inte ännu är klar toorun i JDK 7.
4. Klicka på **Nästa**.
5. I hello **konfiguration av virtuell dator** dialogrutan:
   1. Ange ett namn för hello virtuella datorn.
   2. Ange hello storlek toouse för hello virtuella datorn.
   3. Ange ett namn för Hej administratör i hello **användarnamn** fältet. Kom ihåg detta namn och hello lösenord skriver du sedan kan du använda dem när du loggar via fjärranslutning på toohello virtuella datorn.
   4. Ange ett lösenord i hello **nytt lösenord** fältet och ange den igen i hello **Bekräfta** fältet. Detta är hello lösenordet för administratörskontot.
   5. Klicka på **Nästa**.
6. I hello nästa **konfiguration av virtuell dator** dialogrutan:
   1. För **Molntjänsten**, Använd hello standard **skapa en ny molntjänst**.
   2. Hej värde för **DNS molntjänstnamnet** måste vara unikt inom cloudapp.net. Om det behövs, ändra värdet så att Azure anger att det är unikt.
   3. Ange en region, en tillhörighetsgrupp eller ett virtuellt nätverk. För den här kursen är att ange en region som **västra USA**.
   4. För **Lagringskonto**väljer **använda ett automatiskt genererat lagringskonto**.
   5. För **Tillgänglighetsuppsättning**väljer **(ingen)**.
   6. Klicka på **Nästa**.
7. I sista hello **konfiguration av virtuell dator** dialogrutan:
   1. Acceptera hello standardvärden för slutpunkten.
   2. Klicka på **Complete** (Slutför).

## <a name="tooremotely-log-in-tooyour-virtual-machine"></a>tooremotely inloggningen tooyour virtuell dator
1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Klicka på **virtuella datorer**.
3. Klicka på hello virtuell dator som du vill toolog i hello namn.
4. Klicka på **Anslut**.
5. Svara toohello prompter som behövs tooconnect toohello virtuell dator. När du uppmanas Hej administratör användarnamn och lösenord för använda hello-värden som du angav när du skapade hello virtuella datorn.

Observera att hello Azure Service Bus-funktioner kräver hello Baltimore CyberTrust Root certificate toobe installeras som en del av din JRE **cacerts** lagras. Det här certifikatet ingår automatiskt i hello miljö JRE (Java Runtime) används av den här kursen. Om du inte har det här certifikatet i dina JRE **cacerts** finns i [att lägga till ett certifikat toohello certifikatarkiv för Java CA] [ add_ca_cert] information om att lägga till den (samt information om hur du visar hello certifikat i din cacerts store).

## <a name="how-toocreate-a-service-bus-namespace"></a>Hur toocreate en service bus namnområde
toobegin med hjälp av Service Bus-köer i Azure, måste du först skapa ett namnområde för tjänsten. Ett namnområde för tjänsten innehåller en omfattningsbehållare för adressering av Service Bus-resurser i ditt program.

toocreate ett namnområde för tjänsten:

1. Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com).
2. Hello nedre vänstra navigeringsfönstret för hello klassiska Azure-portalen, klicka på **Service Bus, Access Control och cachelagring**.
3. Klicka på hello i hello övre vänstra rutan hello klassiska Azure-portalen, **Service Bus** nod, och klicka sedan på hello **ny** knappen.  
   ![Skärmbild för Service Bus-nod][svc_bus_node]
4. I hello **skapa en ny Service Namespace** dialogrutan Ange en **Namespace**, och klicka på toomake till att det är unikt, den **Kontrollera tillgänglighet** knappen.  
   ![Skapa en ny Namespace skärmbild][create_namespace]
5. Kontrollerar hello namnområdesnamnet som är tillgänglig, väljer du land eller region där namnområdet ska finnas och klicka sedan på hello **skapa Namespace** knappen.  
   
   hello-namnområde som du skapat visas sedan i hello klassiska Azure-portalen och tar en stund tooactivate. Vänta tills hello status är **Active** innan du fortsätter med hello nästa steg.

## <a name="obtain-hello-default-management-credentials-for-hello-namespace"></a>Hämta hello standard autentiseringsuppgifter för hantering hello namnområdet
I ordning hanteringsåtgärder tooperform, till exempel skapa en kö på hello nya namnområdet, behöver du tooobtain hello hantering av autentiseringsuppgifter för namnområdet.

1. Hello vänstra navigeringsfönstret, klicka på hello **Service Bus** noder för att visa hello listan över tillgängliga namnområden.
   ![Tillgängliga namnområden skärmbild][avail_namespaces]
2. Välj hello-namnområde som du nyss skapat från hello listan som visas.
   ![Namespace-listan skärmbild][namespace_list]
3. hello högra **egenskaper** rutan visar hello egenskaper för det nya namnområdet.
   ![Egenskaper för fönstret skärmbild][properties_pane]
4. Hej **standard nyckeln** är dolt. Klicka på hello **visa** knappen toodisplay hello-identifiering.
   ![Skärmbild som standard nyckel][default_key]
5. Anteckna hello **standard utfärdaren** och hello **standard nyckeln** som du vill använda den här informationen nedan tooperform åtgärder med namnområdet.

## <a name="how-toocreate-a-java-application-that-performs-a-compute-intensive-task"></a>Hur toocreate ett Java-program som utför en krävande uppgift
1. På utvecklingsdatorn (som inte har toobe hello virtuell dator som du skapade), hämta hello [Azure SDK för Java](https://azure.microsoft.com/develop/java/).
2. Skapa ett Java-konsolprogram med hello exempelkod hello slutet av det här avsnittet. I den här kursen använder vi **TSPSolver.java** som hello Java-filnamn. Ändra hello **din\_service\_bus\_namnområde**, **din\_service\_bus\_ägare**, och **din\_service\_bus\_nyckeln** platshållare toouse din service bus **namnområde**, **standard utfärdaren** och  **Standard nyckeln** värdena respektive.
3. Efter kodning krävs export hello programmet tooa körbara Java Arkiv (JAR) och paketet hello bibliotek i hello genereras JAR. I den här kursen använder vi **TSPSolver.jar** som hello genereras JAR namn.

<p/>

    // TSPSolver.java

    import com.microsoft.windowsazure.services.core.Configuration;
    import com.microsoft.windowsazure.services.core.ServiceException;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import java.io.*;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import java.util.ArrayList;
    import java.util.Date;
    import java.util.List;

    public class TSPSolver {

        //  Value specifying how often tooprovide an update toohello console.
        private static long loopCheck = 100000000;  

        private static long nTimes = 0, nLoops=0;

        private static double[][] distances;
        private static String[] cityNames;
        private static int[] bestOrder;
        private static double minDistance;
        private static ServiceBusContract service;

        private static void buildDistances(String fileLocation, int numCities) throws Exception{
            try{
                BufferedReader file = new BufferedReader(new InputStreamReader(new DataInputStream(new FileInputStream(new File(fileLocation)))));
                double[][] cityLocs = new double[numCities][2];
                for (int i = 0; i<numCities; i++){
                    String[] line = file.readLine().split(", ");
                    cityNames[i] = line[0];
                    cityLocs[i][0] = Double.parseDouble(line[1]);
                    cityLocs[i][1] = Double.parseDouble(line[2]);
                }
                for (int i = 0; i<numCities; i++){
                    for (int j = i; j<numCities; j++){
                        distances[i][j] = Math.hypot(Math.abs(cityLocs[i][0] - cityLocs[j][0]), Math.abs(cityLocs[i][1] - cityLocs[j][1]));
                        distances[j][i] = distances[i][j];
                    }
                }
            } catch (Exception e){
                throw e;
            }
        }

        private static void permutation(List<Integer> startCities, double distSoFar, List<Integer> restCities) throws Exception {

            try
            {
                nTimes++;
                if (nTimes == loopCheck)
                {
                    nLoops++;
                    nTimes = 0;
                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.print("Current time is " + dateFormat.format(date) + ". ");
                    System.out.println(  "Completed " + nLoops + " iterations of size of " + loopCheck + ".");
                }

                if ((restCities.size() == 1) && ((minDistance == -1) || (distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-1)] < minDistance))){
                    startCities.add(restCities.get(0));
                    newBestDistance(startCities, distSoFar + distances[restCities.get(0)][startCities.get(0)] + distances[restCities.get(0)][startCities.get(startCities.size()-2)]);
                    startCities.remove(startCities.size()-1);
                }
                else{
                    for (int i=0; i<restCities.size(); i++){
                        startCities.add(restCities.get(0));
                        restCities.remove(0);
                        permutation(startCities, distSoFar + distances[startCities.get(startCities.size()-1)][startCities.get(startCities.size()-2)],restCities);
                        restCities.add(startCities.get(startCities.size()-1));
                        startCities.remove(startCities.size()-1);
                    }
                }
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        private static void newBestDistance(List<Integer> cities, double distance) throws ServiceException, Exception {
            try
            {
                minDistance = distance;
                String cityList = "Shortest distance is "+minDistance+", with route: ";
                for (int i = 0; i<bestOrder.length; i++){
                    bestOrder[i] = cities.get(i);
                    cityList += cityNames[bestOrder[i]];
                    if (i != bestOrder.length -1)
                        cityList += ", ";
                }
                System.out.println(cityList);
                service.sendQueueMessage("TSPQueue", new BrokeredMessage(cityList));
            }
            catch (ServiceException se)
            {
                throw se;
            }
            catch (Exception e)
            {
                throw e;
            }
        }

        public static void main(String args[]){

            try {

                Configuration config = ServiceBusConfiguration.configureWithWrapAuthentication(
                        "your_service_bus_namespace", "your_service_bus_owner",
                        "your_service_bus_key",
                        ".servicebus.windows.net",
                        "-sb.accesscontrol.windows.net/WRAPv0.9");

                service = ServiceBusService.create(config);

                int numCities = 10;  // Use as hello default, if no value is specified at command line.
                if (args.length != 0)
                {
                    if (args[0].toLowerCase().compareTo("createqueue")==0)
                    {
                        // No processing toooccur other than creating hello queue.
                        QueueInfo queueInfo = new QueueInfo("TSPQueue");

                        service.createQueue(queueInfo);

                        System.out.println("Queue named TSPQueue was created.");

                        System.exit(0);
                    }

                    if (args[0].toLowerCase().compareTo("deletequeue")==0)
                    {
                        // No processing toooccur other than deleting hello queue.
                        service.deleteQueue("TSPQueue");

                        System.out.println("Queue named TSPQueue was deleted.");

                        System.exit(0);
                    }

                    // Neither creating or deleting a queue.
                    // Assume hello value passed in is hello number of cities toosolve.
                    numCities = Integer.valueOf(args[0]);  
                }

                System.out.println("Running for " + numCities + " cities.");

                List<Integer> startCities = new ArrayList<Integer>();
                List<Integer> restCities = new ArrayList<Integer>();
                startCities.add(0);
                for(int i = 1; i<numCities; i++)
                    restCities.add(i);
                distances = new double[numCities][numCities];
                cityNames = new String[numCities];
                buildDistances("c:\\TSP\\cities.txt", numCities);
                minDistance = -1;
                bestOrder = new int[numCities];
                permutation(startCities, 0, restCities);
                System.out.println("Final solution found!");
                service.sendQueueMessage("TSPQueue", new BrokeredMessage("Complete"));
            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }
        }

    }



## <a name="how-toocreate-a-java-application-that-monitors-hello-progress-of-hello-compute-intensive-task"></a>Hur toocreate ett Java-program som övervakar hello status för hello beräkningsintensiva aktivitet
1. Skapa en Java-konsoltillämpning med hello exempelkod hello slutet av det här avsnittet på utvecklingsdatorn. I den här kursen använder vi **TSPClient.java** som hello Java-filnamn. Som tidigare kan du ändra hello **din\_service\_bus\_namnområde**, **din\_service\_bus\_ägare**, och **din\_service\_bus\_nyckeln** platshållare toouse din service bus **namnområde**, **standard utfärdaren**och **standard nyckeln** värdena respektive.
2. Exportera hello programmet tooa körbara JAR och paketet hello krävs bibliotek i hello genereras JAR. I den här kursen använder vi **TSPClient.jar** som hello genereras JAR namn.

<p/>

    // TSPClient.java

    import java.util.Date;
    import java.text.DateFormat;
    import java.text.SimpleDateFormat;
    import com.microsoft.windowsazure.services.serviceBus.*;
    import com.microsoft.windowsazure.services.serviceBus.models.*;
    import com.microsoft.windowsazure.services.core.*;

    public class TSPClient
    {

        public static void main(String[] args)
        {
                try
                {

                    DateFormat dateFormat = new SimpleDateFormat("MM/dd/yyyy HH:mm:ss");
                    Date date = new Date();
                    System.out.println("Starting at " + dateFormat.format(date) + ".");

                    String namespace = "your_service_bus_namespace";
                    String issuer = "your_service_bus_owner";
                    String key = "your_service_bus_key";

                    Configuration config;
                    config = ServiceBusConfiguration.configureWithWrapAuthentication(
                            namespace, issuer, key,
                            ".servicebus.windows.net",
                            "-sb.accesscontrol.windows.net/WRAPv0.9");

                    ServiceBusContract service = ServiceBusService.create(config);

                    BrokeredMessage message;

                    int waitMinutes = 3;  // Use as hello default, if no value is specified at command line.
                    if (args.length != 0)
                    {
                        waitMinutes = Integer.valueOf(args[0]);  
                    }

                    String waitString;

                    waitString = (waitMinutes == 1) ? "minute." : waitMinutes + " minutes.";

                    // This queue must have previously been created.
                    service.getQueue("TSPQueue");

                    int numRead;

                    String s = null;

                    while (true)
                    {

                        ReceiveQueueMessageResult resultQM = service.receiveQueueMessage("TSPQueue");
                        message = resultQM.getValue();

                        if (null != message && null != message.getMessageId())
                        {

                            // Display hello queue message.
                            byte[] b = new byte[200];

                            System.out.print("From queue: ");

                            s = null;
                            numRead = message.getBody().read(b);
                            while (-1 != numRead)
                            {
                                s = new String(b);
                                s = s.trim();
                                System.out.print(s);
                                numRead = message.getBody().read(b);
                            }
                            System.out.println();
                            if (s.compareTo("Complete") == 0)
                            {
                                // No more processing toooccur.
                                date = new Date();
                                System.out.println("Finished at " + dateFormat.format(date) + ".");
                                break;
                            }
                        }
                        else
                        {
                            // hello queue is empty.
                            System.out.println("Queue is empty. Sleeping for another " + waitString);
                            Thread.sleep(60000 * waitMinutes);
                        }
                    }

            }
            catch (ServiceException se)
            {
                System.out.println(se.getMessage());
                se.printStackTrace();
                System.exit(-1);
            }
            catch (Exception e)
            {
                System.out.println(e.getMessage());
                e.printStackTrace();
                System.exit(-1);
            }

        }

    }

## <a name="how-toorun-hello-java-applications"></a>Hur toorun hello Java-program
Kör hello beräkningsintensiva programmet hello första toocreate hello kön och sedan toosolve reser Saleseman Problem som lägger till hello aktuella bästa vägen toohello service bus-kö. När hello beräkningsintensiva program är igång (eller senare), kör hello klienten toodisplay resultat från hello service bus-kö.

### <a name="toorun-hello-compute-intensive-application"></a>toorun hello beräkningsintensiva program
1. Logga in tooyour virtuella datorn.
2. Skapa en mapp där du kör programmet. Till exempel **c:\TSP**.
3. Kopiera **TSPSolver.jar** för**c:\TSP**,
4. Skapa en fil med namnet **c:\TSP\cities.txt** med hello efter innehållet.
   
        City_1, 1002.81, -1841.35
        City_2, -953.55, -229.6
        City_3, -1363.11, -1027.72
        City_4, -1884.47, -1616.16
        City_5, 1603.08, -1030.03
        City_6, -1555.58, 218.58
        City_7, 578.8, -12.87
        City_8, 1350.76, 77.79
        City_9, 293.36, -1820.01
        City_10, 1883.14, 1637.28
        City_11, -1271.41, -1670.5
        City_12, 1475.99, 225.35
        City_13, 1250.78, 379.98
        City_14, 1305.77, 569.75
        City_15, 230.77, 231.58
        City_16, -822.63, -544.68
        City_17, -817.54, -81.92
        City_18, 303.99, -1823.43
        City_19, 239.95, 1007.91
        City_20, -1302.92, 150.39
        City_21, -116.11, 1933.01
        City_22, 382.64, 835.09
        City_23, -580.28, 1040.04
        City_24, 205.55, -264.23
        City_25, -238.81, -576.48
        City_26, -1722.9, -909.65
        City_27, 445.22, 1427.28
        City_28, 513.17, 1828.72
        City_29, 1750.68, -1668.1
        City_30, 1705.09, -309.35
        City_31, -167.34, 1003.76
        City_32, -1162.85, -1674.33
        City_33, 1490.32, 821.04
        City_34, 1208.32, 1523.3
        City_35, 18.04, 1857.11
        City_36, 1852.46, 1647.75
        City_37, -167.44, -336.39
        City_38, 115.4, 0.2
        City_39, -66.96, 917.73
        City_40, 915.96, 474.1
        City_41, 140.03, 725.22
        City_42, -1582.68, 1608.88
        City_43, -567.51, 1253.83
        City_44, 1956.36, 830.92
        City_45, -233.38, 909.93
        City_46, -1750.45, 1940.76
        City_47, 405.81, 421.84
        City_48, 363.68, 768.21
        City_49, -120.3, -463.13
        City_50, 588.51, 679.33
5. Ändra kataloger tooc:\TSP vid en kommandotolk.
6. Kontrollera hello JRE bin-mappen är i hello PATH-miljövariabeln.
7. Du behöver toocreate hello service bus-kö innan du kör hello Telefontjänstprovider solver permutationer. Kör följande kommando toocreate hello service bus-kö hello.
   
        java -jar TSPSolver.jar createqueue
8. Nu när hello kön har skapats kan köra du hello Telefontjänstprovider solver permutationer. Till exempel köra följande kommando toorun hello solver för 8 städer hello.
   
        java -jar TSPSolver.jar 8
   
   Om du inte anger ett tal, körs efter 10 städer. Eftersom hello solver hittar aktuella kortaste vägar, läggs dem toohello kön.

> [!NOTE]
> hello större hello tal som du anger, hello längre hello solver körs. Till exempel köra för 14 orter kan ta flera minuter och köras för 15 orter kan ta flera timmar. Öka too16 eller flera orter resultera i dagar för körning (slutligen veckor, månader och år). Detta är på grund av toohello snabb ökning hello antalet permutationer utvärderas skiljde hello som hello antalet orter ökar.
> 
> 

### <a name="how-toorun-hello-monitoring-client-application"></a>Hur toorun hello övervakning klientprogrammet
1. Logga in tooyour datorn där du kör hello-klientprogrammet. Detta behövs inte toobe hello samma dator som kör hello **TSPSolver** program, även om det kan vara.
2. Skapa en mapp där du kör programmet. Till exempel **c:\TSP**.
3. Kopiera **TSPClient.jar** för**c:\TSP**,
4. Kontrollera hello JRE bin-mappen är i hello PATH-miljövariabeln.
5. Ändra kataloger tooc:\TSP vid en kommandotolk.
6. Kör följande kommando hello.
   
        java -jar TSPClient.jar
   
    Du kan också ange hello antal minuter toosleep between kontrollerar hello kö genom att passera i ett kommandoradsargument. hello strömsparläge standardperioden för att kontrollera hello kön är 3 minuter, som används om inget kommandoradsargument som skickas för**TSPClient**. Om du vill toouse ett annat värde för hello strömsparläge intervall, till exempel köra en minut hello följande kommando.
   
        java -jar TSPClient.jar 1
   
    hello-klienten körs förrän den ser ett kömeddelande på ”Slutför”. Observera att om du kör flera förekomster av hello solver utan att köra hello klienten kanske måste toorun hello klienten flera gånger toocompletely tom hello kön. Alternativt kan du ta bort hello kö och sedan skapa den igen. toodelete hello kö, kör följande hello **TSPSolver** (inte **TSPClient**) kommando.
   
        java -jar TSPSolver.jar deletequeue
   
    hello solver kommer att köras tills det är färdigt undersöka alla vägar.

## <a name="how-toostop-hello-java-applications"></a>Hur toostop hello Java-program
För både hello solver och klientprogram kan du trycka på **Ctrl + C** tooexit om du vill tooend tidigare toonormal slutförande.

[solver_output]:media/java-run-compute-intensive-task/WA_JavaTSPSolver.png
[client_output]:media/java-run-compute-intensive-task/WA_JavaTSPClient.png
[svc_bus_node]:media/java-run-compute-intensive-task/SvcBusQueues_02_SvcBusNode.jpg
[create_namespace]:media/java-run-compute-intensive-task/SvcBusQueues_03_CreateNewSvcNamespace.jpg
[avail_namespaces]:media/java-run-compute-intensive-task/SvcBusQueues_04_SvcBusNode_AvailNamespaces.jpg
[namespace_list]:media/java-run-compute-intensive-task/SvcBusQueues_05_NamespaceList.jpg
[properties_pane]:media/java-run-compute-intensive-task/SvcBusQueues_06_PropertiesPane.jpg
[default_key]:media/java-run-compute-intensive-task/SvcBusQueues_07_DefaultKey.jpg
[add_ca_cert]: ../../../java-add-certificate-ca-store.md
