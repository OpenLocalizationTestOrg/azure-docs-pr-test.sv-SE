
<br>

> [!NOTE]
> Om du fick ett användarnamn och lösenord som en administratör, det är troligt att du redan har ett arbets- eller skola ID (kallas även ibland ett *organisations-ID*). I så fall, kan du direkt börja toouse din Azure-konto tooaccess Azure-resurser som kräver ett. Om du upptäcker att du inte kan använda dessa resurser, kan du behöva tooreturn toothis artikel för hjälp. Mer information finns i [konton som du kan använda för inloggning](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts) och [hur Azure-prenumerationen är relaterade tooAzure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir).
> 
> 

hello stegen är enkel. Du behöver toolocate din signerade på identiteten i hello klassiska Azure-portalen identifiera standard Azure Active Directory-domänen och lägga till en ny användare tooit som administratör för Azure samtidigt.

## <a name="locate-your-default-directory-in-hello-azure-classic-portal"></a>Leta upp din standardkatalog i hello klassiska Azure-portalen
Starta genom att logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) med ditt personliga Microsoft-kontoidentitet. När du är inloggad, bläddra nedåt hello blå panelen på vänster sida hello och klicka **ACTIVE DIRECTORY**.

![Azure Active Directory](./media/virtual-machines-common-create-aad-work-id/azureactivedirectorywidget.png)

Låt oss börja med att hitta information om din identitet i Azure. Du bör se något liknande hello efter i hello huvudfönstret, visar att du har en standardkatalog.

![](./media/virtual-machines-common-create-aad-work-id/defaultaadlisting.png)

Låt oss ta reda på mer information om den. Klicka på hello standard directory raden, som ger dig till hello standardegenskaperna för katalogen.  

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectorypage.png)

tooview hello standarddomännamnet, klickar du på **DOMÄNER**.

![](./media/virtual-machines-common-create-aad-work-id/domainclicktoseeyourdefaultdomain.png)

Här bör du kunna toosee att Azure Active Directory när hello Azure-konto har skapats, skapas en personlig standarddomän som är ett hash-värde (ett tal som genereras från en textsträng) för din personligt ID-nummer som används som en underdomän till onmicrosoft.com. Det är hello domän toowhich nu ska du lägga till en ny användare.

## <a name="creating-a-new-user-in-hello-default-domain"></a>Skapa en ny användare i hello standarddomän
Klicka på **användare** och leta efter din enda personligt konto. Du bör se i hello **ursprung** kolumn som det är en **Microsoft-konto**. Vi vill toocreate en användare i din standard. onmicrosoft.com Azure Active Directory-domän.

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryuserslisting.png)

Vi toofollow [instruktionerna](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) i hello följande steg, men använder ett exempel.

Hello längst hello-sidan, klickar du på **+ Lägg till användare**. Skriv hello nytt användarnamn hello sida som visas, och att Hej **typ av användare** en **ny användare i din organisation**. I det här exemplet hello nytt användarnamn är `ahmet`. Välj hello standarddomän som du identifierade tidigare som hello domän för ahmet's e-postadress. Klicka på hello pilen Nästa när du är klar.

![](./media/virtual-machines-common-create-aad-work-id/addingauserwithdirectorydropdown.png)

Lägg till mer information för Ahmet, men se till att tooselect hello lämpliga **ROLLEN** värde. Det är enkelt toouse **Global administratör** toomake att allt fungerar, men om du kan använda en mindre roll, som är en bra idé. Det här exemplet används hello **användaren** roll. (Läs mer på [administratörsbehörigheter efter roll](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1).) Aktivera inte Multi-Factor authentication såvida du inte vill toouse flerfunktionsautentisering för varje logga in igen. Klicka på pilen för hello Nästa när du är klar.

![](./media/virtual-machines-common-create-aad-work-id/userprofileuseradmin.png)

Klicka på hello **skapa** knappen toogenerate och visar ett tillfälligt lösenord för Ahmet.

![](./media/virtual-machines-common-create-aad-work-id/gettemporarypasswordforuser.png)

Kopiera hello användarens e-postadress eller använda **skicka lösenord i e-post**. Du behöver hello information toolog på inom kort.

![](./media/virtual-machines-common-create-aad-work-id/receivedtemporarypassworddialog.png)

Nu bör du se hello ny användare **Ahmet hello Developer**, ursprung från Azure Active Directory. Du har skapat hello nytt arbete eller skola identitet med Azure Active Directory. Men den här identiteten ännu inte har behörigheter toouse Azure resurser.

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryusersaftercreate.png)

Om du använder **skicka lösenord i e-post**, hello efter typ av e-post skickas.

![](./media/virtual-machines-common-create-aad-work-id/emailreceivedfromnewusercreation.png)

## <a name="adding-azure-co-administrator-rights-for-subscriptions"></a>Lägga till Azure medadministratör rättigheter för prenumerationer
Nu behöver du tooadd hello ny användare som medadministratör för prenumerationen så hello ny användare kan logga in toohello hanteringsportalen. detta, i hello nedre vänstra panelen klickar du på toodo **inställningar**.

![](./media/virtual-machines-common-create-aad-work-id/thesettingswidget.png)

Hello huvudinställningarna Klicka på **ADMINISTRATÖRER** på hello top- och du bör se bara din personliga Microsoft-kontoidentitet. Hello längst hello-sidan, klickar du på **+ Lägg till** toospecify för en medadministratör. Här kan ange hello e-postadress hello ny användare som du har skapat, inklusive din standarddomän. I hello nästa skärmbild visas en grön bockmarkering nästa toohello användare för hello standardkatalogen. Kom ihåg tooselect alla hello prenumerationer som du vill att den här användaren toobe kan tooadminister.

![](./media/virtual-machines-common-create-aad-work-id/addingnewuserascoadmin.png)

Du bör nu se två användare, inklusive din nya medadministratör identitet när du är klar. Logga ut från hello-portalen.

![](./media/virtual-machines-common-create-aad-work-id/newuseraddedascoadministrator.png)

## <a name="logging-in-and-changing-hello-new-users-password"></a>Logga in och ändra hello nya lösenord
Logga in som hello nya användare som du skapade.

![](./media/virtual-machines-common-create-aad-work-id/signinginwithnewuser.png)

Du kommer direkt att ange toocreate ett nytt lösenord.

![](./media/virtual-machines-common-create-aad-work-id/mustupdateyourpassword.png)

Du bör tilldelas lyckade som ser ut som följande hello.

![](./media/virtual-machines-common-create-aad-work-id/successtourdialog.png)

## <a name="next-steps"></a>Nästa steg
Nu kan du använda din nya Azure Active Directory identitet toouse [Azure-resurs mallar](../articles/xplat-cli-azure-resource-manager.md).

    azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how tooset them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@aztrainpassxxxxxoutlook.onmicrosoft.com
    Password: *********
    /info:    Added subscription Azure Pass
    info:    Setting subscription Azure Pass as default
    +
    info:    login command OK
    ralph@local:~$ azure config mode arm
    info:    New mode is arm
    ralph@local:~$ azure group list
    info:    Executing command group list
    + Listing resource groups
    info:    No matched resource groups were found
    info:    group list command OK
    ralph@local:~$ azure group create newgroup westus
    info:    Executing command group create
    + Getting resource group newgroup
    + Creating resource group newgroup
    info:    Created resource group newgroup
    data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/newgroup
    data:    Name:                newgroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags:
    data:
    info:    group create command OK
