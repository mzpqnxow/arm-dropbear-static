# arm-dropbear-static-jr
Statically linked ARM dropbear SSH daemon with minimalist options, built with musl libc

## Contents

### output/

These are the statically linked executables, built for ARM. See the following in configuration/default_options.h:

```
#define DSS_PRIV_FILENAME "/tmp/dropbear/dropbear_dss_host_key"
#define RSA_PRIV_FILENAME "/tmp/dropbear/dropbear_rsa_host_key"
#define ECDSA_PRIV_FILENAME "/tmp/dropbear/dropbear_ecdsa_host_key"
#define DROPBEAR_PIDFILE "/tmp/dropbear.pid"
```

These are the most important for installing/running the daemon. You will need to generate these host keys and /tmp must be writable. Note that there is a problem here. You will need to deal with making the daemon stay persistent **and** start upon boot, since /tmp will almost certainly be wiped if it is `tmpfs`. Ditto with the host keys. Store them somewhere persistent and then have a script to copy them into place and start the daemon. I did not include instructions on how to generate the host keys but this information should be accessible on the Google.

### build/

This is the source used to build from. It is the latest as of 11/20/18. It was configured and built as follows:

```
cross_configure  \
  --enable-static \
  --disable-syslog \
  --enable-bundled-libtom \
  --disable-lastlog \
  --disable-utmp \
  --disable-utmpx \
  --disable-wtmp \
  --disable-wtmpx \
  --disable-zlib \
  --prefix=/tmp/dropbear/
```

The `cross_configure` command is an alias provided by the [embedded-toolkit activate script](https://github.com/mzpqnxow/embedded-toolkit/blob/master/build_scripts/activate-script-helpers/activate-musl-toolchain.env)

```
$ alias
alias cross_configure='./configure --host=arm-linux-musleabi --prefix=/toolchains/arm-linux-musleabi'
```


