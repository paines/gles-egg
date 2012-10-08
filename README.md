gles-egg
========

Chicken Scheme OpenGL ES egg for Android

Work in progress ....
This egg is based upon the project android-chicken-example.
Right now, it compiled and load fine under debian/i386.

Next step: use android-ndk to compile the module
Android NDK based cross-chicken: Refferng to $NDK/docs/STANDALONE-TOOLCHAIN.html

Use  $NDK/build/tools/make-standalone-toolchain.sh 
to install an Android NDK based toolchain to your home.

$NDK/build/tools/make-standalone-toolchain.sh --platform=android-14 --install-dir=~/my-android-toolchain

After that, build the cross-chicken like explained in 3 Step:
http://wiki.call-cc.org/man/4/Cross%20development#building-the-target-libraries


1st Step:
setup PTH to find the Android NDK compiler and linker
e.g:
export PATH=$PATH:$NDK/toolchains/arm-linux-androideabi-4.4.3/prebuilt/linux-x86/bin/


2nd Step:
Unpack chicken-4.8.0.tgz and do
make ARCH= \
    PREFIX=/usr \
    PLATFORM=linux \
    HOSTSYSTEM=arm-linux-androideabi \
    DESTDIR=$HOME/target \
    TARGET_FEATURES="-no-feature x86 -feature arm" \
    libs install-dev

I had to disable HAVE_EXITS_H in Makefile.linux + disable all references in posixunix.scm to pw-gecos.

3rd Step:
Unpack chicken-4.8.0.tgz AGAIN and do:
make PLATFORM=linux \
    PREFIX=$HOME/cross-chicken \
    TARGETSYSTEM=arm-linux-androideabi \
    PROGRAM_PREFIX=arm- \
    TARGET_PREFIX=$HOME/target/usr \
    TARGET_RUN_PREFIX=/usr \
    install

However, you need to fix some things manually, before building finally the gles-egg.
Go to ~/target/usr/include and link chicken to arm-chicken. 
ln -s chicken arm-chicken

After that copy libGLESv1_CM.so from $NDK/platforms/android-14/arch-arm/usr/lib/


After that, you can e.g. call ~/cross-chicken/bin/arm-chicken-install -target inside the gless-egg directory. 
