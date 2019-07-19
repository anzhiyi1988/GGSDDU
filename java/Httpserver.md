```java
package com.thunisoft.sffx.tansform.platform.http;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.URI;
import java.util.Collection;
import java.util.Iterator;
import java.util.List;
import java.util.Set;

import com.sun.net.httpserver.Headers;
import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;

public class MyHttpHandler implements HttpHandler {

    @Override
    public void handle(HttpExchange t) throws IOException {

        String portocol = t.getProtocol();
        System.out.println(portocol);
        String method = t.getRequestMethod();
        System.out.println(method);
        URI u = t.getRequestURI();
        String query = u.getQuery();
        System.out.println(query);
        Headers head = t.getRequestHeaders();
        Collection<List<String>> list = head.values();
        Iterator<List<String>> it = list.iterator();
        while (it.hasNext()) {
            List<String> l = it.next();
            for (String s : l) {
                System.out.println(s);
            }
        }
        System.out.println("------------------------");
        Set<String> set = head.keySet();
        for (String s : set) {
            System.out.print(s + ":");
            List<String> l = head.get(s);
            for (String str : l) {
                System.out.print(str + " | ");
            }
            System.out.println();
        }

        InputStream is = t.getRequestBody();
        InputStreamReader isr = new InputStreamReader(is);
        BufferedReader br = new BufferedReader(isr);
        String str = null;
        while ((str = br.readLine()) != null) {
            System.out.println(str);
        }

    }
}

```

