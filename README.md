# How-to-Enable-mod_brotli-Brotli-Compression-on-Apache-Server.
How to Enable mod_brotli Brotli Compression on Apache Server.
<text>
The mod_brotli module provides the BROTLI_COMPRESS output filter that allows output from your server to be compressed using the brotli compression format before being sent to the client over the network. This module uses the Brotli library found at https://github.com/google/brotli.</text>


<p>Follow the installation procedure steps to install mod_brotli :</p>

<p>Install Brotli on your server</p>

`yum install pcre-devel cmake -ycd usr/local/src`

`git clone https://github.com/google/brotli.git`

`cd brotli`

`git checkout v1.0`

`./configure-cmake`
`make && make install`

<p>Adding path for dependencies files:</p>

```
grep "/usr/local/lib/" /etc/ld.so.conf || echo "/usr/local/lib/" >> /etc/ld.so.conf
ldconfig
```

<p>Now add this line to /etc/httpd/conf/httpd.conf :</p>

```
LoadModule brotli_module modules/mod_brotli.so
<IfModule mod_brotli.c>
BrotliCompressionQuality 6

#To enable globally' 
#AddOutputFilterByType BROTLI_COMPRESS text/html text/plain text/xml text/css text/javascript application/x-javascript application/javascript application/json application/x-font-ttf application/vnd.ms-fontobject image/x-icon

BrotliFilterNote Input brotli_input_info
BrotliFilterNote Output brotli_output_info
BrotliFilterNote Ratio brotli_ratio_info
LogFormat '"%r" %{brotli_output_info}n/%{brotli_input_info}n (%{brotli_ratio_info}n%%)' brotli
CustomLog "logs/brotli_log" brotli

#Don't compress content which is already compressed
SetEnvIfNoCase Request_URI \
\.(gif|jpe?g|png|swf|woff|woff2) no-brotli dont-vary

#Make sure proxies don't deliver the wrong content
Header append Vary User-Agent env=!dont-vary
</IfModule>
```

<p>to enable brotli for all of your sites remove “#” before from AddOutputFilterByType</p>
<p>** BrotliCompressionQuality 6 for better compression you can select value 0-11 i’ll recommend value 6</p>

and

```
sudo systemctl restart httpd
```

<p>and <br><br>of course check that it works in your Chrome</p>

![image](https://user-images.githubusercontent.com/51197053/140746627-0b6e2053-bccc-4db8-859f-94be0942ca33.png)
