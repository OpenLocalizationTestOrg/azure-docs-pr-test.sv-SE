En del paket kanske inte installeras med pip när det körs på Azure.  Det kan vara att hello-paketet inte är tillgängligt på hello Python Package Index.  Det kan vara att det krävs en kompilator (en kompilator är inte tillgänglig på hello datorn hello webbapp som körs i Azure App Service).

I det här avsnittet ska vi titta på olika sätt toodeal med det här problemet.

### <a name="request-wheels"></a>Begär hjul
Om hello installationen kräver en kompilator, bör du kontakta hello paketet ägare toorequest hjul görs tillgängliga för hello-paketet.

Med hello senaste tillgänglighet för [Microsoft Visual C++-kompilatorn för Python 2.7][Microsoft Visual C++ Compiler for Python 2.7], är det nu enklare toobuild paket som har inbyggd kod för Python 2.7.

### <a name="build-wheels-requires-windows"></a>Skapa hjul (kräver Windows)
Obs: När du använder det här alternativet, se till att toocompile hello paketet med en Python-miljö som matchar hello plattform/arkitektur/version som används på hello webbapp i Azure App Service (Windows/32-bit/2.7 eller 3.4).

Om hello paketet inte installeras eftersom den kräver en kompilator, kan du installera hello kompilatorn på den lokala datorn och skapa ett hjul för hello-paketet som du sedan inkluderar i databasen.

Mac/Linux-användare: Om du inte har åtkomst tooa Windows-dator, se [skapa en virtuell dator med Windows] [ Create a Virtual Machine Running Windows] för hur toocreate en virtuell dator på Azure.  Du kan använda den toobuild hello hjul, lägga till dem toohello databasen och om du vill ta bort hello VM. 

För Python 2.7, kan du installera [Microsoft Visual C++-kompilatorn för Python 2.7][Microsoft Visual C++ Compiler for Python 2.7].

För Python 3.4, kan du installera [Microsoft Visual C++ 2010 Express][Microsoft Visual C++ 2010 Express].

toobuild hjul, behöver du hjulpaketet hello:

    env\scripts\pip install wheel

Du kommer att använda `pip wheel` toocompile ett beroende:

    env\scripts\pip wheel azure==0.8.4

Detta skapar en .whl-fil i hello \wheelhouse mapp.  Lägg till hello \wheelhouse mapp och hjul filer tooyour databasen.

Redigera din requirements.txt tooadd hello `--find-links` alternativet hello överst. Det instruerar pip toolook efter en exakt matchning i hello lokala mappen innan kommer toohello python package index.

    --find-links wheelhouse
    azure==0.8.4

Om du vill att alla beroenden i hello \wheelhouse mappen och inte använder hello python package index alls tooinclude kan du tvinga pip tooignore hello paketindexet genom att lägga till `--no-index` toohello början av din requirements.txt.

    --no-index

### <a name="customize-installation"></a>Anpassa installationen
Du kan anpassa hello distribution skriptet tooinstall ett paket i hello virtuell miljö med hjälp av ett alternativt installationsprogram som easy\_installera.  Se deploy.cmd för ett exempel som har kommenterats bort.  Se till att paketen inte listats i requirements.txt tooprevent pip installerar dem..

Lägg till den här toohello distributionsskriptet:

    env\scripts\easy_install somepackage

Du kan också vara kan enkelt toouse\_installera tooinstall från ett exe-installationsprogram (en del är zip-kompatibla så easy\_install stöder dem).  Lägg till hello installer tooyour databas och anropa easy\_installera genom att ange hello sökvägen toohello körbara.

Lägg till den här toohello distributionsskriptet:

    env\scripts\easy_install "%DEPLOYMENT_SOURCE%\installers\somepackage.exe"

### <a name="include-hello-virtual-environment-in-hello-repository-requires-windows"></a>Inkludera hello virtuella miljön i hello-databasen (kräver Windows)
Obs: När du använder det här alternativet, se till att toouse en virtuell miljö som matchar hello plattform/arkitektur/version som används på hello webbapp i Azure App Service (Windows/32-bit/2.7 eller 3.4).

Om du inkluderar hello virtuella miljön i hello databas, kan du förhindra hello distributionsskriptet utför virtuell miljöhantering i Azure genom att skapa en tom fil:

    .skipPythonDeployment

Vi rekommenderar att du tar bort hello befintliga virtuella miljön i appen hello tooprevent överblivna filer från när hello virtuella miljön hanterades automatiskt.

[Create a Virtual Machine Running Windows]: http://azure.microsoft.com/documentation/articles/virtual-machines-windows-hero-tutorial/
[Microsoft Visual C++ Compiler for Python 2.7]: http://aka.ms/vcpython27
[Microsoft Visual C++ 2010 Express]: http://go.microsoft.com/?linkid=9709949
