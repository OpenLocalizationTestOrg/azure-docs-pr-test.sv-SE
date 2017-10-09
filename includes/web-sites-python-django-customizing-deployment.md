Azure kommer att fastställa att ditt program använder Python **om bägge följande villkor stämmer**:

* Requirements.txt filen i rotmappen för hello
* någon .py-fil i rotmappen för hello eller en runtime.txt som specificerar python

När så är fallet hello används en Python-specifikt distributionsskript vilket utför hello standardsynkronisering av filer såväl som ytterligare Python-åtgärder som:

* Automatisk hantering av virtuell miljö
* Installation av paket som listas i requirements.txt med pip
* Skapa hello lämplig web.config baserat på hello valda Python-versionen.
* Samla in statiska filer för Django-program

Du kan kontrollera vissa aspekter av hello standard-distributionsstegen utan att behöva toocustomize hello skript.

Om du vill tooskip alla Python-specifika distributionssteg, kan du skapa den här tomma filen:

    \.skipPythonDeployment

Om du vill tooskip insamlingen av statiska filer för Django-programmet:

    \.skipDjango 

För mer kontroll över distributionen kan du åsidosätta hello standard-distributionsskriptet genom att skapa hello följande filer:

    \.deployment
    \deploy.cmd

Du kan använda hello [Azure-kommandoradsgränssnittet] [ Azure command-line interface] toocreate hello filer.  Använd det här kommandot från din projektmapp:

    azure site deploymentscript --python

Om de här filerna inte finns så skapar Azure ett tillfälligt distributionsskript och kör det.  Det är identiska toohello som du har skapat med hello kommandot ovan.

[Azure command-line interface]: http://azure.microsoft.com/downloads/
