        
        
        
           headers ← servletRequest中的所有请求头
           excludedHeaders ←  ["Content-Length","Connection", "Keep-Alive", "Proxy-Authenticate", 
                               "Proxy-Authorization","TE", "Trailers", "Transfer-Encoding", "Upgrade"]
           **for**  headers中的每个请求头header do
               if excludedHeaders contains header then
                   break
               else
                   header add to proxyRequest
               end if
           end for
           
           
