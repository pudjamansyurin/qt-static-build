# QT Static Build (mingw_32)
## HTTPS support (network)
### Install MSYS2
  - URL: https://www.msys2.org/wiki/MSYS2-installation/
  - Description: 
    - Used for correct Perl version
    - Used for compile environment 
### Compile OpenSSL
  - URL: https://www.openssl.org/source/old/1.1.1/
  - Compile: 
    ```console
    ./config --prefix=/d/openssl-1.1.1m/dist --release -static --static
    ```

## QT Framework
  - Install QT Maintenance Tool & QT Creator
  - Compile tutorial: https://decovar.dev/blog/2018/02/17/build-qt-statically/
  - Source qt-everywhere: https://download.qt.io/archive/qt/5.15/5.15.6/single/
  - Configure static build
    ```console
    ./configure.bat -v -recheck-all -prefix "/c/Qt/5.15.2-static" 
    -platform win32-g++ -static -debug-and-release -optimize-size 
    -opensource -confirm-license -opengl desktop 
    -make libs -nomake examples -nomake tests -nomake tools 
    -no-pch -no-dbus 
    -openssl-linked -I "/d/openssl-1.1.1m/dist/include" -L "/d/openssl-1.1.1m/dist/lib" OPENSSL_LIBS="-llibssl -llibcrypto -lcrypt32 -luser32 -lgdi32 -lwsock32 -lws2_32" 
    -skip qtandroidextras -skip qtmacextras -skip qtx11extras -skip qtconnectivity -skip qtdeclarative -skip qtdoc -skip qtgamepad -skip qtgraphicaleffects -skip qtlocation -skip qtlottie -skip qtmultimedia -skip qtnetworkauth -skip qtpurchasing -skip qtquick3d -skip qtquickcontrols -skip qtquickcontrols2 -skip qtquicktimeline -skip qtremoteobjects -skip qtscript -skip qtscxml -skip qtsensors -skip qtserialbus -skip qtspeech -skip qtsvg -skip qtvirtualkeyboard -skip qtwayland -skip qtwebchannel -skip qtwebengine -skip qtwebglplugin -skip qtwebsockets -skip qtwebview 
    ```
  - Compile
    ```console
    rm -rf config.cache
    configure.bat
    mingw32-make.exe -j8
    mingw32-make.exe -j8 install
    ```

## App Signing
  - Generate certificate
    ```powershell
    $cert = New-SelfSignedCertificate -DnsName almanshurin.com -CertStoreLocation cert:\LocalMachine\My -Type CodeSigning 

    $pwd = ConvertTo-SecureString -String "alman" -Force -AsPlainText

    Export-PfxCertificate -cert $cert -FilePath "certificates\cert.pfx" -Password $pwd
    ```
  - Sign application
    ```console
    signtool.exe sign /a /v /fd "SHA256" /f "certificates\cert.pfx" /p "alman" "release\qt-app.exe"
    ```