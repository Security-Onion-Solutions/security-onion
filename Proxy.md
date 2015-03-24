# What do I need to do if I'm behind a proxy? #

Put your proxy server settings in /etc/environment like this:<br>
<pre><code>export http_proxy=https://server:port<br>
export https_proxy=https://server:port<br>
export ftp_proxy=https://server:port<br>
export PERL_LWP_ENV_PROXY=https://server:port<br>
export no_proxy="localhost,127.0.0.1"<br>
</code></pre>
If you're going to run something using sudo, remember to use the "-i" option to force it to process the environment variables.  For example:<br>
<pre><code>sudo -i rule-update<br>
</code></pre>

For certain proxies (Bluecoat in particular), you may need to change from https to http in /etc/nsm/pulledpork/pulledpork.conf.  For more information, please see:<br>
<br>
<a href='https://code.google.com/p/pulledpork/issues/detail?id=154'>https://code.google.com/p/pulledpork/issues/detail?id=154</a>

<a href='https://groups.google.com/d/topic/security-onion/NQ-dLLPxR6A/discussion'>https://groups.google.com/d/topic/security-onion/NQ-dLLPxR6A/discussion</a>

<a href='https://groups.google.com/d/topic/security-onion-testing/piRYj-7Ar8M/discussion'>https://groups.google.com/d/topic/security-onion-testing/piRYj-7Ar8M/discussion</a>