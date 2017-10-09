---
title: aaaView SAML returnerades av hello Access Control Service (Java)
description: "Lär dig hur tooview SAML som returneras av hello åtkomstkontrolltjänsten i Java-program finns i Azure."
services: active-directory
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 6cd216f9-eb43-46b4-b30d-f194d0ae2d48
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.custom: aaddev
ms.openlocfilehash: b6733bc98b505cfa89a4ce456f368ee15da11427
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooview-saml-returned-by-hello-azure-access-control-service"></a><span data-ttu-id="a00b4-103">Hur tooview SAML returnerades av hello Azure Access Control Service</span><span class="sxs-lookup"><span data-stu-id="a00b4-103">How tooview SAML returned by hello Azure Access Control Service</span></span>
<span data-ttu-id="a00b4-104">Den här guiden visar hur tooview hello underliggande Security Assertion Markup Language (SAML) returnerade tooyour programmet hello Azure Access Control Service (ACS).</span><span class="sxs-lookup"><span data-stu-id="a00b4-104">This guide will show you how tooview hello underlying Security Assertion Markup Language (SAML) returned tooyour application by hello Azure Access Control Service (ACS).</span></span> <span data-ttu-id="a00b4-105">hello guiden bygger på hello [hur tooAuthenticate webbanvändare med Azure Access Control Service med Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) avsnittet genom att ange koden som visar hello SAML information.</span><span class="sxs-lookup"><span data-stu-id="a00b4-105">hello guide builds on hello [How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) topic, by providing code that displays hello SAML information.</span></span> <span data-ttu-id="a00b4-106">programmet hello slutförts ser liknande toohello följande.</span><span class="sxs-lookup"><span data-stu-id="a00b4-106">hello completed application will look similar toohello following.</span></span>

![Exempel SAML-utdata][saml_output]

<span data-ttu-id="a00b4-108">Mer information om ACS finns hello [nästa steg](#next_steps) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a00b4-108">For more information on ACS, see hello [Next steps](#next_steps) section.</span></span>

> [!NOTE]
> <span data-ttu-id="a00b4-109">hello Azure Access Control tjänstfilter är en community technology preview.</span><span class="sxs-lookup"><span data-stu-id="a00b4-109">hello Azure Access Services Control Filter is a community technology preview.</span></span> <span data-ttu-id="a00b4-110">Som förhandsversionen stöds den formellt inte av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a00b4-110">As pre-release software, it is not formally supported by Microsoft.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="a00b4-111">Krav</span><span class="sxs-lookup"><span data-stu-id="a00b4-111">Prerequisites</span></span>
<span data-ttu-id="a00b4-112">toocomplete hello uppgifterna i den här guiden är klar hello exempelprogram på [hur tooAuthenticate webbanvändare med Azure Access Control Service med Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) och använda den som hello som startpunkt för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a00b4-112">toocomplete hello tasks in this guide, complete hello sample at [How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) and use it as hello starting point for this tutorial.</span></span>

## <a name="add-hello-jspwriter-library-tooyour-build-path-and-deployment-assembly"></a><span data-ttu-id="a00b4-113">Lägg till hello JspWriter biblioteket tooyour build sökväg och distribution av sammansättningen</span><span class="sxs-lookup"><span data-stu-id="a00b4-113">Add hello JspWriter library tooyour build path and deployment assembly</span></span>
<span data-ttu-id="a00b4-114">Lägga till hello bibliotek som innehåller hello **javax.servlet.jsp.JspWriter** klassen tooyour skapa sökvägen och distribution av sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="a00b4-114">Add hello library that contains hello **javax.servlet.jsp.JspWriter** class tooyour build path and deployment assembly.</span></span> <span data-ttu-id="a00b4-115">Om du använder Tomcat hello biblioteket är **jsp-api.jar**, som finns i hello Apache **lib** mapp.</span><span class="sxs-lookup"><span data-stu-id="a00b4-115">If you are using Tomcat, hello library is **jsp-api.jar**, which is located in hello Apache **lib** folder.</span></span>

1. <span data-ttu-id="a00b4-116">Högerklicka i Eclipses Projektutforskaren **MyACSHelloWorld**, klickar du på **byggsökväg**, klickar du på **konfigurera byggsökväg**, klickar du på hello **bibliotek** fliken och klicka sedan på **lägga till externa JAR: er**.</span><span class="sxs-lookup"><span data-stu-id="a00b4-116">In Eclipse's Project Explorer, right-click **MyACSHelloWorld**, click **Build Path**, click **Configure Build Path**, click hello **Libraries** tab, and then click **Add External JARs**.</span></span>
2. <span data-ttu-id="a00b4-117">I hello **JAR markeringen** dialogrutan navigera toohello nödvändiga JAR markerar du det och klickar sedan på **öppna**.</span><span class="sxs-lookup"><span data-stu-id="a00b4-117">In hello **JAR Selection** dialog, navigate toohello necessary JAR, select it, and then click **Open**.</span></span>
3. <span data-ttu-id="a00b4-118">Med hello **egenskaper för MyACSHelloWorld** fortfarande öppna dialogrutan **distribution sammansättningen**.</span><span class="sxs-lookup"><span data-stu-id="a00b4-118">With hello **Properties for MyACSHelloWorld** dialog still open, click **Deployment Assembly**.</span></span>
4. <span data-ttu-id="a00b4-119">I hello **Web distribution sammansättningen** dialogrutan klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="a00b4-119">In hello **Web Deployment Assembly** dialog, click **Add**.</span></span>
5. <span data-ttu-id="a00b4-120">I hello **nytt sammansättningen direktiv** dialogrutan klickar du på **Java Build Path poster** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="a00b4-120">In hello **New Assembly Directive** dialog, click **Java Build Path Entries** and then click **Next**.</span></span>
6. <span data-ttu-id="a00b4-121">Välj hello lämpligt bibliotek och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="a00b4-121">Select hello appropriate library and click **Finish**.</span></span>
7. <span data-ttu-id="a00b4-122">Klicka på **OK** tooclose hello **egenskaper för MyACSHelloWorld** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="a00b4-122">Click **OK** tooclose hello **Properties for MyACSHelloWorld** dialog.</span></span>

## <a name="modify-hello-jsp-file-toodisplay-saml"></a><span data-ttu-id="a00b4-123">Ändra hello JSP-fil toodisplay SAML</span><span class="sxs-lookup"><span data-stu-id="a00b4-123">Modify hello JSP file toodisplay SAML</span></span>
<span data-ttu-id="a00b4-124">Ändra **index.jsp** toouse hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="a00b4-124">Modify **index.jsp** toouse hello following code.</span></span>

    <%@ page language="java" contentType="text/html; charset=UTF-8"
        pageEncoding="UTF-8"%>
        <%@ page import="javax.xml.parsers.*"
                 import="javax.xml.transform.*"
                 import="org.w3c.dom.*"
                 import="java.io.*"
                 import="javax.xml.transform.stream.*"
                 import="javax.xml.transform.dom.*"
                 import="javax.xml.xpath.*"
                 import="javax.servlet.jsp.JspWriter" %>
    <!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
    <html>
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
    <title>Sample ACS Filter</title>
    </head>
    <body>
        <h3>SAML information from sample ACS program</h3>
        <%!
        void displaySAMLInfo(Node node, String parent, JspWriter out)
        {

            try
            {
                String nodeName;
                int nChild, i;

                nodeName = node.getNodeName();
                out.println("<br>");
                out.println("<u>Examining <b>" + parent + nodeName + "</b></u><br>");

                   // Attributes.
                   NamedNodeMap attribsMap = node.getAttributes();
                   if (null != attribsMap)
                   {
                         for (i=0; i < attribsMap.getLength(); i++)
                         {
                                Node attrib = attribsMap.item(i);
                                out.println("Attribute: <b>" + attrib.getNodeName() + "</b>: " + attrib.getNodeValue()  + "<br>");
                         }
                   }

                   // Child nodes.
                   NodeList list = node.getChildNodes();
                   if (null != list)
                    {
                          nChild = list.getLength();
                          if (nChild > 0)
                          {                    

                                 // If it is a text node, just print hello text.
                                 if (list.item(0).getNodeName() == "#text")
                                 {
                                     out.println("Text value: <b>" + list.item(0).getTextContent() + "</b><br>");
                                 }
                                 else
                                 {
                                     // Print out hello child node names.
                                     out.print("Contains " + nChild + " child node(s): ");   
                                        for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);

                                        out.print("<b>" + temp.getNodeName() + "</b>");
                                        if (i < nChild - 1)
                                        {
                                            // Separate hello names.
                                            out.print(", ");
                                        }
                                        else
                                        {
                                            // Finish hello sentence.
                                            out.print(".");
                                        }

                                     }
                                     out.println("<br>");

                                     // Process hello child nodes.
                                     for (i=0; i < nChild; i++)
                                     {
                                        Node temp = list.item(i);
                                        displaySAMLInfo(temp, parent + nodeName + "\\", out);
                                     }
                               }
                          }
                      }
                  }
                catch (Exception e)
                {
                    System.out.println("Exception encountered.");
                    e.printStackTrace();            
                }
            }
        %>

        <%
        try
        {
            String data  = (String) request.getAttribute("ACSSAML");

            DocumentBuilder docBuilder;
            Document doc = null;
            DocumentBuilderFactory docBuilderFactory = DocumentBuilderFactory.newInstance();
            docBuilderFactory.setIgnoringElementContentWhitespace(true);
            docBuilder = docBuilderFactory.newDocumentBuilder();
            byte[] xmlDATA = data.getBytes();

            ByteArrayInputStream in = new ByteArrayInputStream(xmlDATA);
            doc = docBuilder.parse(in);
            doc.getDocumentElement().normalize();

            // Iterate hello child nodes of hello doc.
            NodeList list = doc.getChildNodes();

            for (int i=0; i < list.getLength(); i++)
            {
                displaySAMLInfo(list.item(i), "", out);
            }
        }
        catch (Exception e)
        {
            out.println("Exception encountered.");
            e.printStackTrace();
        }

        %>
    </body>
    </html>

## <a name="run-hello-application"></a><span data-ttu-id="a00b4-125">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="a00b4-125">Run hello application</span></span>
1. <span data-ttu-id="a00b4-126">Kör ditt program i hello datorn emulator eller distribuera tooAzure, med hello steg som beskrivs i [hur tooAuthenticate webbanvändare med Azure Access Control Service med Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).</span><span class="sxs-lookup"><span data-stu-id="a00b4-126">Run your application in hello computer emulator or deploy tooAzure, using hello steps documented at [How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).</span></span>
2. <span data-ttu-id="a00b4-127">Starta en webbläsare och öppna ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="a00b4-127">Launch a browser and open your web application.</span></span> <span data-ttu-id="a00b4-128">När du har loggat in tooyour program visas SAML-information, inklusive hello security assertion tillhandahålls av hello identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="a00b4-128">After you log on tooyour application, you'll see SAML information, including hello security assertion provided by hello identity provider.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a00b4-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a00b4-129">Next steps</span></span>
<span data-ttu-id="a00b4-130">toofurther utforska Gransknings-och insamlingstjänstens funktioner och tooexperiment med mer avancerade scenarier, se [Access Control Service 2.0][Access Control Service 2.0].</span><span class="sxs-lookup"><span data-stu-id="a00b4-130">toofurther explore ACS's functionality and tooexperiment with more sophisticated scenarios, see [Access Control Service 2.0][Access Control Service 2.0].</span></span>

[Prerequisites]: #pre
[Modify hello JSP file toodisplay SAML]: #modify_jsp
[Add hello JspWriter library tooyour build path and deployment assembly]: #add_library
[Run hello application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse]: active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
