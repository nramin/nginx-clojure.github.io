Quick Start
=============

Download
--------------

1. Download the latest binaries release v0.2.5 from [here](https://sourceforge.net/projects/nginx-clojure/files/). 
1. Unzip the zip file downloaded then rename the file `nginx-${os-arc}` to `nginx`, eg. for linux is `nginx-linux-x64`


Configuration
--------------
1. Open conf/nginx.conf file
1. Setting JVM path and class path within `http {` block in  nginx.conf

	```nginx
	### jvm dynamic library path
	jvm_path '/usr/lib/jvm/java-7-oracle/jre/lib/amd64/server/libjvm.so';
	
	### my app jars e.g. clojure-1.5.1.jar , groovy-2.3.4.jar ,etc.
	jvm_var my_other_jars 'my_jar_dir/clojure-1.5.1.jar';
		
	### my app classpath, windows user should use ';' as the separator
	jvm_options "-Djava.class.path=jars/nginx-clojure-0.2.5.jar:#{my_other_jars}";
	```
1. Setting Http Service Handler

	For Clojure:
	```nginx

       location /clojure {
          handler_type 'clojure';
          handler_code ' 
						(fn[req]
						  {
						    :status 200,
						    :headers {"content-type" "text/plain"},
						    :body  "Hello Clojure & Nginx!"
						    })
          ';
       }
	```
	
	For Groovy:
	```nginx

       location /groovy {
          handler_type 'groovy';
          handler_code ' 
               import nginx.clojure.java.NginxJavaRingHandler;
               import java.util.Map;
               public class HelloGroovy implements NginxJavaRingHandler {
                  public Object[] invoke(Map<String, Object> request){
                     return [200, //http status 200
                             ["Content-Type":"text/html"], //headers map
                             "Hello, Groovy & Nginx!"]; //response body can be string, File or Array/Collection of them
                  }
               }
          ';
       }
	```

Start up
--------------


```nginx

$ cd nginx-clojure-0.2.5/nginx-1.6.0
$ ./nginx
``` 
If everything is ok, we can access our first http service by this url

```nginx
### For Clojure
http://localhost:8080/clojure

### For Groovy
http://localhost:8080/groovy
```

We can check the logs/error.log to see error information.

Reload
--------------

If we change some settings  we can reload the settings without stoping our services.

```nginx
$ ./nginx -s reload
```


Stop
--------------

```nginx
$ ./nginx -s stop
```

