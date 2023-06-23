# VCC

## REST SERVER:
GenericResource.java:
```
import javax.ws.rs.QueryParam;
import java.util.List;

@GET
    @Produces("text/html")
    public String getHtml(@QueryParam ("params") List<Integer> a) {
        //TODO return proper representation object
        return ("<h1>"+Integer.toString(a.get(0)+a.get(1))+"<h1>");
    }

http://localhost:8080/restservertest1/webresources/generic?params=50&params=50
```
## REST CLIENT:
NewJerseyClient.java:
```
import java.util.ArrayList;
import java.util.List;
import javax.ws.rs.client.WebTarget;

public String getHtml(int a,int b) throws javax.ws.rs.ClientErrorException {
        
        WebTarget resource = webTarget;
        
        List<Integer> list=new ArrayList<>();
        list.add(a);
        list.add(b);
        Integer[] params=list.toArray(new Integer[list.size()]);
        resource=resource.queryParam("params",(Object[])params);
        
        return resource.request(javax.ws.rs.core.MediaType.TEXT_HTML).get(String.class);
    }
```
newjsp.jsp:
```
<%@page contentType="text/html" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<%@page import="client.NewJerseyClient"%>
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <title>JSP Page</title>
    </head>
    <body>
        <h1>Hello World!</h1>
        <%
            client.NewJerseyClient n=new client.NewJerseyClient();
            out.println("Result ="+n.getHtml(250,250));
         %>
    </body>
</html>

http://localhost:8080/restclienttest1/newjsp.jsp
```
![image](https://github.com/Siddarthan999/VCC/assets/91734840/ac5bd042-0f43-45a0-8adb-52677f80b052)
![image](https://github.com/Siddarthan999/VCC/assets/91734840/406a56d0-639f-451b-9b76-fc99da285014)
