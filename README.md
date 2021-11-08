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
