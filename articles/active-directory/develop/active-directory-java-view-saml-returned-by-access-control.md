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
# <a name="how-tooview-saml-returned-by-hello-azure-access-control-service"></a>Hur tooview SAML returnerades av hello Azure Access Control Service
Den här guiden visar hur tooview hello underliggande Security Assertion Markup Language (SAML) returnerade tooyour programmet hello Azure Access Control Service (ACS). hello guiden bygger på hello [hur tooAuthenticate webbanvändare med Azure Access Control Service med Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) avsnittet genom att ange koden som visar hello SAML information. programmet hello slutförts ser liknande toohello följande.

![Exempel SAML-utdata][saml_output]

Mer information om ACS finns hello [nästa steg](#next_steps) avsnitt.

> [!NOTE]
> hello Azure Access Control tjänstfilter är en community technology preview. Som förhandsversionen stöds den formellt inte av Microsoft.
> 
> 

## <a name="prerequisites"></a>Krav
toocomplete hello uppgifterna i den här guiden är klar hello exempelprogram på [hur tooAuthenticate webbanvändare med Azure Access Control Service med Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md) och använda den som hello som startpunkt för den här självstudiekursen.

## <a name="add-hello-jspwriter-library-tooyour-build-path-and-deployment-assembly"></a>Lägg till hello JspWriter biblioteket tooyour build sökväg och distribution av sammansättningen
Lägga till hello bibliotek som innehåller hello **javax.servlet.jsp.JspWriter** klassen tooyour skapa sökvägen och distribution av sammansättningen. Om du använder Tomcat hello biblioteket är **jsp-api.jar**, som finns i hello Apache **lib** mapp.

1. Högerklicka i Eclipses Projektutforskaren **MyACSHelloWorld**, klickar du på **byggsökväg**, klickar du på **konfigurera byggsökväg**, klickar du på hello **bibliotek** fliken och klicka sedan på **lägga till externa JAR: er**.
2. I hello **JAR markeringen** dialogrutan navigera toohello nödvändiga JAR markerar du det och klickar sedan på **öppna**.
3. Med hello **egenskaper för MyACSHelloWorld** fortfarande öppna dialogrutan **distribution sammansättningen**.
4. I hello **Web distribution sammansättningen** dialogrutan klickar du på **Lägg till**.
5. I hello **nytt sammansättningen direktiv** dialogrutan klickar du på **Java Build Path poster** och klicka sedan på **nästa**.
6. Välj hello lämpligt bibliotek och klicka på **Slutför**.
7. Klicka på **OK** tooclose hello **egenskaper för MyACSHelloWorld** dialogrutan.

## <a name="modify-hello-jsp-file-toodisplay-saml"></a>Ändra hello JSP-fil toodisplay SAML
Ändra **index.jsp** toouse hello följande kod.

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

## <a name="run-hello-application"></a>Kör programmet hello
1. Kör ditt program i hello datorn emulator eller distribuera tooAzure, med hello steg som beskrivs i [hur tooAuthenticate webbanvändare med Azure Access Control Service med Eclipse](active-directory-java-authenticate-users-access-control-eclipse.md).
2. Starta en webbläsare och öppna ditt webbprogram. När du har loggat in tooyour program visas SAML-information, inklusive hello security assertion tillhandahålls av hello identitetsleverantör.

## <a name="next-steps"></a>Nästa steg
toofurther utforska Gransknings-och insamlingstjänstens funktioner och tooexperiment med mer avancerade scenarier, se [Access Control Service 2.0][Access Control Service 2.0].

[Prerequisites]: #pre
[Modify hello JSP file toodisplay SAML]: #modify_jsp
[Add hello JspWriter library tooyour build path and deployment assembly]: #add_library
[Run hello application]: #run_application
[Next steps]: #next_steps
[Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
[How tooAuthenticate Web Users with Azure Access Control Service Using Eclipse]: active-directory-java-authenticate-users-access-control-eclipse
[saml_output]: ./media/active-directory-java-view-saml-returned-by-access-control/SAML_Output.png
