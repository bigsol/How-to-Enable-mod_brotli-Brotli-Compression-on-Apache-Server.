# How-to-Enable-mod_brotli-Brotli-Compression-on-Apache-Server.
How to Enable mod_brotli Brotli Compression on Apache Server.
<text>
The mod_brotli module provides the BROTLI_COMPRESS output filter that allows output from your server to be compressed using the brotli compression format before being sent to the client over the network. This module uses the Brotli library found at https://github.com/google/brotli.</text>


<p>Follow the installation procedure steps to install mod_brotli :</p>

<p>Install Brotli on your server</p>

<li>yum install pcre-devel cmake -ycd usr/local/src</li>
<li>git clone https://github.com/google/brotli.git</li>
<li>cd brotli</li>
<li>git checkout v1.0</li>
<li>./configure-cmake</li>
<li>make && make install</li>
<p>Adding path for dependencies files:</p>

grep "/usr/local/lib/" /etc/ld.so.conf || echo "/usr/local/lib/" >> /etc/ld.so.conf<br>
ldconfig

<p>Now add this line to /etc/httpd/conf/httpd.conf :</p>

#+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

LoadModule brotli_module modules/mod_brotli.so
<IfModule mod_brotli.c><br>
BrotliCompressionQuality 6<br>
  <br>
#To enable globally'<br> 
#AddOutputFilterByType BROTLI_COMPRESS text/html text/plain text/xml text/css text/javascript application/x-javascript application/javascript application/json application/x-font-ttf application/vnd.ms-fontobject image/x-icon<br>
<br>
BrotliFilterNote Input brotli_input_info<br>
BrotliFilterNote Output brotli_output_info<br>
BrotliFilterNote Ratio brotli_ratio_info<br>
LogFormat '"%r" %{brotli_output_info}n/%{brotli_input_info}n (%{brotli_ratio_info}n%%)' brotli<br>
CustomLog "logs/brotli_log" brotli<br>
<br>
#Don't compress content which is already compressed<br>
SetEnvIfNoCase Request_URI \<br>
\.(gif|jpe?g|png|swf|woff|woff2) no-brotli dont-vary<br>
<br>
#Make sure proxies don't deliver the wrong content<br>
Header append Vary User-Agent env=!dont-vary<br>
</IfModule><br>
#+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++<br>
