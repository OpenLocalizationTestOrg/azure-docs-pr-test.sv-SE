* I **lägga till vCenter**, ange ett eget namn för hello vSphere-värd eller vCenter server och sedan ange hello IP-adress eller FQDN för hello-servern. Lämna hello port som 443 om VMware-servrar är konfigurerade toolisten för begäranden på en annan port. Välj hello-konto som är tooconnect toohello VMware vCenter eller vSphere ESXi-servern. Klicka på **OK**.

    ![VMware](./media/site-recovery-add-vcenter/vmware-server.png)

   > [!NOTE]
   > Om du vill lägga till hello VMware vCenter-server eller VMware vSphere-värd med ett konto som inte har administratörsbehörighet på hello vCenter eller värden, kontrollera att hello-tjänstkontot har dessa behörigheter aktiveras: Datacenter, datalagret, mapp, värd, nätverk , Resurs, virtuell dator och vSphere distribuerade växel. Hello VMware vCenter-servern måste dessutom hello lagring vyer privilegiet aktiverat.
