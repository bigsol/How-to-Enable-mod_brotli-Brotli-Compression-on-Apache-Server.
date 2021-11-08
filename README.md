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

[httpd.txt](https://github.com/bigsol/How-to-Enable-mod_brotli-Brotli-Compression-on-Apache-Server./files/7497056/httpd.txt)
![image](https://user-images.githubusercontent.com/51197053/140745881-1d3f9258-91cf-4526-a09d-f8c1a4697721.png)

<p>to enable brotli for all of your sites remove “#” before from AddOutputFilterByType</p>
<p>** BrotliCompressionQuality 6 for better compression you can select value 0-11 i’ll recommend value 6</p>

and 
<p>sudo systemctl restart httpd</p>

