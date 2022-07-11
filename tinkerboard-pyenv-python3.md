# Installation notes

When installing Python 3.10 (other versions untested), the Tinkerboard does not provide the correct libraries to build with openssl. To resolve this, the following can be done:

```
mkdir -p $HOME/Repositories

# https://www.openssl.org/source/
wget https://www.openssl.org/source/openssl-1.1.1q.tar.gz

tar xvfz openssl-1.1.1q

cd openssl-1.1.1q

./config --prefix=$HOME/opt --openssldir=$HOME/opt/ssl shared zlib
make
make install
```

OpenSSL can be tested with `$HOME/opt/bin/openssl version` to validate shared objects were loaded correctly.

```
# TODO: LD_LIBRARY_PATH may need to be defined in your profile?
env PYTHON_CONFIGURE_OPTS="--enable-shared" PKG_CONFIG_PATH="$HOME/opt/lib/mkconfig" LD_LIBRARY_PATH="$HOME/opt/lib" pyenv install -v 3.10.5
```
