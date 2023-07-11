# Gobuster

## INSTALACIÓN

La instalación de gobuster puede realizarse de varias formas.

Si disponemos de una distribución basada en debian podemos hacerlo con apt.

{% code lineNumbers="true" %}
```
sudo apt install gobuster
```
{% endcode %}

En caso contrario se puede descargar desde su página de [github](https://github.com/OJ/gobuster) e instalar y compilar con go.

{% code lineNumbers="true" %}
```
git clone https://github.com/OJ/gobuster.git
cd gobuster
go get && go build
go install
```
{% endcode %}

O hacer todo directamente desde go.

{% code lineNumbers="true" %}
```
go install github.com/OJ/gobuster/v3@latest
```
{% endcode %}

## USO DE GOBUSTER

Como ya indicábamos se pueden hacer 4 tipos de enumeraciones, de URIs, DNS, vhosts y buckets S3 así que vamos a ver cada una de ellas.

### **ENUMERACIÓN DE URIs**

Para la enumeración de **directorios y ficheros** utilizaremos la palabra clave dir, que como todo comando tiene su propia ayuda.

{% code lineNumbers="true" %}
```
gobuster dir --help
Uses directory/file enumeration mode

Usage:
  gobuster dir [flags]

Flags:
  -f, --add-slash                       Append / to each request
  -c, --cookies string                  Cookies to use for the requests
  -d, --discover-backup                 Upon finding a file search for backup files
      --exclude-length ints             exclude the following content length (completely ignores the status). Supply multiple times to exclude multiple sizes.
  -e, --expanded                        Expanded mode, print full URLs
  -x, --extensions string               File extension(s) to search for
  -r, --follow-redirect                 Follow redirects
  -H, --headers stringArray             Specify HTTP headers, -H 'Header1: val1' -H 'Header2: val2'
  -h, --help                            help for dir
      --hide-length                     Hide the length of the body in the output
  -m, --method string                   Use the following HTTP method (default "GET")
  -n, --no-status                       Don't print status codes
  -k, --no-tls-validation               Skip TLS certificate verification
  -P, --password string                 Password for Basic Auth
      --proxy string                    Proxy to use for requests [http(s)://host:port]
      --random-agent                    Use a random User-Agent string
  -s, --status-codes string             Positive status codes (will be overwritten with status-codes-blacklist if set)
  -b, --status-codes-blacklist string   Negative status codes (will override status-codes if set) (default "404")
      --timeout duration                HTTP Timeout (default 10s)
  -u, --url string                      The target URL
  -a, --useragent string                Set the User-Agent string (default "gobuster/3.1.0")
  -U, --username string                 Username for Basic Auth
      --wildcard                        Force continued operation when wildcard found

Global Flags:
      --delay duration    Time each thread waits between requests (e.g. 1500ms)
      --no-error          Don't display errors
  -z, --no-progress       Don't display progress
  -o, --output string     Output file to write results to (defaults to stdout)
  -p, --pattern string    File containing replacement patterns
  -q, --quiet             Don't print the banner and other noise
  -t, --threads int       Number of concurrent threads (default 10)
  -v, --verbose           Verbose output (errors)
  -w, --wordlist string   Path to the wordlist
```
{% endcode %}

Por ejemplo podemos escanear directorios

{% code lineNumbers="true" %}
```
gobuster dir -u http://www.example.com/ -w /path/to/dictionary
```
{% endcode %}

Al anterior le podemos indicar el número de hilos que queremos utilizar para que sea más rápido el escaneo

<pre data-line-numbers><code><strong>gobuster dir -u http://www.example.com/ -w /path/to/dictionary -t 100
</strong></code></pre>

O los códigos de respuesta que esperamos obtener

<pre data-line-numbers><code><strong>gobuster dir -u http://www.example.com/ -w /path/to/dictionary -t 100 -s 200,301,302,403
</strong></code></pre>

Si quisiéramos buscar ficheros, tendríamos que añadir la opción -x con las extensiones que se desea buscar, por ejemplo:

{% code lineNumbers="true" %}
```
gobuster dir -u http://www.example.com/ -w /path/to/dictionary -x php,txt
```
{% endcode %}

### **ENUMERACIÓN DE DNS**

Para la enumeración de **subdominios dns** utilizaremos la palabra clave dir, que como todo comando tiene su propia ayuda

{% code lineNumbers="true" %}
```
gobuster dns --help
Uses DNS subdomain enumeration mode

Usage:
  gobuster dns [flags]

Flags:
  -d, --domain string      The target domain
  -h, --help               help for dns
  -r, --resolver string    Use custom DNS server (format server.com or server.com:port)
  -c, --show-cname         Show CNAME records (cannot be used with '-i' option)
  -i, --show-ips           Show IP addresses
      --timeout duration   DNS resolver timeout (default 1s)
      --wildcard           Force continued operation when wildcard found

Global Flags:
      --delay duration    Time each thread waits between requests (e.g. 1500ms)
      --no-error          Don't display errors
  -z, --no-progress       Don't display progress
  -o, --output string     Output file to write results to (defaults to stdout)
  -p, --pattern string    File containing replacement patterns
  -q, --quiet             Don't print the banner and other noise
  -t, --threads int       Number of concurrent threads (default 10)
  -v, --verbose           Verbose output (errors)
  -w, --wordlist string   Path to the wordlist
```
{% endcode %}

Veamos un ejemplo de enumeración de subdominios

{% code lineNumbers="true" %}
```
gobuster dns -d example.com -w /path/to/dictionary -t 100
```
{% endcode %}

Es posible añadir la opción -i para que muestra la dirección ip del subdominio

{% code lineNumbers="true" %}
```
gobuster dns -d example.com -w /path/to/dictionary -t 100 -i
```
{% endcode %}

### **ENUMERACIÓN DE VIRTUAL HOSTS**

Para la enumeración de **virtual hosts** utilizaremos la palabra clave vhost, que como todo comando tiene su propia ayuda

{% code lineNumbers="true" %}
```
gobuster vhost --help
Uses VHOST enumeration mode

Usage:
  gobuster vhost [flags]

Flags:
  -c, --cookies string        Cookies to use for the requests
  -r, --follow-redirect       Follow redirects
  -H, --headers stringArray   Specify HTTP headers, -H 'Header1: val1' -H 'Header2: val2'
  -h, --help                  help for vhost
  -m, --method string         Use the following HTTP method (default "GET")
  -k, --no-tls-validation     Skip TLS certificate verification
  -P, --password string       Password for Basic Auth
      --proxy string          Proxy to use for requests [http(s)://host:port]
      --random-agent          Use a random User-Agent string
      --timeout duration      HTTP Timeout (default 10s)
  -u, --url string            The target URL
  -a, --useragent string      Set the User-Agent string (default "gobuster/3.1.0")
  -U, --username string       Username for Basic Auth

Global Flags:
      --delay duration    Time each thread waits between requests (e.g. 1500ms)
      --no-error          Don't display errors
  -z, --no-progress       Don't display progress
  -o, --output string     Output file to write results to (defaults to stdout)
  -p, --pattern string    File containing replacement patterns
  -q, --quiet             Don't print the banner and other noise
  -t, --threads int       Number of concurrent threads (default 10)
  -v, --verbose           Verbose output (errors)
  -w, --wordlist string   Path to the wordlist
```
{% endcode %}

Para la enumeración de virtualhost podemos ver un simple ejemplo

{% code lineNumbers="true" %}
```
gobuster vhost -u http://www.example.com/ -w /path/to/dictionary
```
{% endcode %}

Al cual, entre los ya vistos, podemos añadir autenticación si es necesaria

{% code lineNumbers="true" %}
```
gobuster vhost -u http://www.example.com/ -w /path/to/dictionary -U username -P password
```
{% endcode %}

O el método GET o POST a utilizar aunque por defecto ya utiliza GET

{% code lineNumbers="true" %}
```
gobuster vhost -u http://www.example.com/ -w /path/to/dictionary -m POST
```
{% endcode %}

### **ENUMERACIÓN DE BUCKET S3**

Para la enumeración de **buckets S3** utilizaremos la palabra clave s3, que como todo comando tiene su propia ayuda

{% code lineNumbers="true" %}
```
gobuster s3 --help
Uses aws bucket enumeration mode

Usage:
  gobuster s3 [flags]

Flags:
  -h, --help               help for s3
  -m, --maxfiles int       max files to list when listing buckets (only shown in verbose mode) (default 5)
      --proxy string       Proxy to use for requests [http(s)://host:port]
      --random-agent       Use a random User-Agent string
      --timeout duration   HTTP Timeout (default 10s)
  -a, --useragent string   Set the User-Agent string (default "gobuster/3.1.0")

Global Flags:
      --delay duration    Time each thread waits between requests (e.g. 1500ms)
      --no-error          Don't display errors
  -z, --no-progress       Don't display progress
  -o, --output string     Output file to write results to (defaults to stdout)
  -p, --pattern string    File containing replacement patterns
  -q, --quiet             Don't print the banner and other noise
  -t, --threads int       Number of concurrent threads (default 10)
  -v, --verbose           Verbose output (errors)
  -w, --wordlist string   Path to the wordlist
```
{% endcode %}

Que podemos ver una forma simple de hacerlo con

{% code lineNumbers="true" %}
```
gobuster s3 -w /path/to/bucket-dictionary.txt
```
{% endcode %}
