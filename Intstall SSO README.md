Install Dependencies
~~~~~~~~~~~~~~~~~~~~
sudo apt update
sudo apt install build-essential checkinstall zlib1g-dev -y

Download OpenSSL
~~~~~~~~~~~~~~~~
cd /usr/local/src/
wget https://www.openssl.org/source/openssl-3.0.8.tar.gz

Extract OpenSSL
~~~~~~~~~~~~~~~
tar -xf openssl-3.0.x.tar.gz
cd openssl-3.0.x

Install OpenSSL
~~~~~~~~~~~~~~~
openssl version -a (take note of existing install paths)
cd /usr/local/src/openssl-3.0.8

./config 
    --prefix=/usr/local/ssl \
    --openssldir=/usr/local/ssl \
    shared \
    zlib \
    no-weak-ssl-ciphers \
    no-ssl3 \
    no-shared \
    enable-ec_nistp_64_gcc_128 \
    -DOPENSSL_NO_HEARTBEATS \
    -fstack-protector-strong

make
make test
make install

OpenSSL is installed in the /usr/local/ssl directory.

Configure Link Libraries
cd /etc/ld.so.conf.d/
nano openssl-3.0.x.conf

Paste the openssl library path directory: /usr/local/ssl/lib64

Reload the dynamic link using the command: Sudo ldconfig -v

Configuire OpenSSL Library
~~~~~~~~~~~~~~~~~~~~~~~~~~
We will replace the default openssl binary '/usr/bin/openssl or /bin/openssl' with the new version '/usr/local/ssl/bin/openssl'.

Backup the binary files:
mv /usr/bin/c_rehash /usr/bin/c_rehash.bak
mv /usr/bin/openssl /usr/bin/openssl.bak

Edit the '/etc/environment' file using nano.
nano /etc/environment

Now add the new OpenSSL binary directory as below
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/usr/local/ssl/bin"

Reload the environment file and test the new updated binary PATH.

source /etc/environment
echo $PATH

Now check the OpenSSL binary file again.
which openssl

Testing
~~~~~~~
openssl version -a

