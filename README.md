# Prebuilt Qt Webkit

This project builds the official, unmodified, Qt Webkit source code for C++17 using
GitHub actions runners.


## Installation

Using `curl` to download and extract to `/opt/qt5`:

    curl -L https://github.com/constructpm/qtwebkit-build/releases/download/v5.212.0-1/qtwebkit-d1c854e-cpp17-$PLATFORM-x64.tar.gz | sudo tar -xJC /opt

where `$PLATFORM` is `ubuntu-18.04` or `ubuntu-22.04`.
