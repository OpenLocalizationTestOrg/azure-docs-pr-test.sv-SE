#### <a name="tooinstall-mpio-on-hello-host"></a>tooinstall MPIO på hello värden
1. Öppna Serverhanteraren på Windows Server-värd. Serverhanteraren startar som standard när en medlem i administratörsgruppen för hello loggar in tooa dator som kör Windows Server 2012 R2 eller Windows Server 2012. Om hello Serverhanteraren inte redan är öppen klickar du på **Start > Serverhanteraren**.
   
    ![Serverhanteraren](./media/storsimple-install-mpio-windows-server/IC740997.png)
2. Klicka på **Serverhanteraren > instrumentpanelen > Lägg till roller och funktioner**. Detta startar hello **Lägg till roller och funktioner** guiden.
   
    ![Guiden Lägg till roller och funktioner 1](./media/storsimple-install-mpio-windows-server/IC740998.png)
3. I hello **Lägg till roller och funktioner** guiden hello följande:
   
   * På hello **innan du börjar** klickar du på **nästa**.
   * På hello **Välj installationstyp** godkänner hello standardinställningen **rollbaserad eller funktionsbaserad** installation. Klicka på **Nästa**.
     
       ![Guiden Lägg till roller och funktioner 2](./media/storsimple-install-mpio-windows-server/IC740999.png)
   * På hello **väljer målservern** väljer **välja en server från serverpoolen hello**. Värdservern bör identifieras automatiskt. Klicka på **Nästa**.
   * På hello **Välj serverroller** klickar du på **nästa**.
   * På hello **Välj funktioner** väljer **Multipath i/o**, och klicka på **nästa**.
     
       ![Guiden Lägg till roller och funktioner 5](./media/storsimple-install-mpio-windows-server/IC741000.png)
   * På hello **Bekräfta installationsinställningarna** bekräftar hello markering och välj sedan **omstart hello-målservern automatiskt om det behövs**, enligt nedan. Klicka på **Installera**.
     
       ![Guiden Lägg till roller och funktioner 8](./media/storsimple-install-mpio-windows-server/IC741001.png)
   * Du meddelas när hello installationen är klar. Klicka på **Stäng** tooclose hello guiden.
     
       ![Guiden Lägg till roller och funktioner 9](./media/storsimple-install-mpio-windows-server/IC741002.png)

