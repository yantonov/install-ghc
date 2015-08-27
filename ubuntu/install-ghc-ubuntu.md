### How to install latest GHC 7.10.1 + cabal 1.22.3.0 from source on ubuntu

for your convinience these instuction is available as:  
[gist](https://gist.github.com/yantonov/10083524)  
[git repo](https://github.com/yantonov/install-ghc)

### settings

    GHC_VERSION="7.10.1"  
    ARCHITECTURE="x86_64"  
    # for 32 bit ARCHITECTURE="i386"      
    PLATFORM="unknown-linux"  
    GHC_DIST_FILENAME="ghc-$GHC_VERSION-$ARCHITECTURE-$PLATFORM-deb7.tar.bz2"
    
    CABAL_VERSION="1.22.3.0"
    CABAL_DIST_FILENAME="Cabal-$CABAL_VERSION.tar.gz"

    CABAL_INSTALL_VERSION="1.22.3.0"
    CABAL_INSTALL_DIST_FILENAME="cabal-install-$CABAL_INSTALL_VERSION.tar.gz"

### ghc

ubuntu prerequisites

    # Multiprecision arithmetic library developers tools, zlib  
    sudo apt-get install libgmp-dev zlib1g-dev -y  
    sudo -K

ghc installation

    # get distr  
    cd $HOME/Downloads
    wget "https://www.haskell.org/ghc/dist/$GHC_VERSION/$GHC_DIST_FILENAME"  
    tar xvfj $GHC_DIST_FILENAME  
    cd ghc-$GHC_VERSION  

    # install to  
    mkdir $HOME/Development/bin/ghc-$GHC_VERSION  
    # or choose another path
    
    ./configure --prefix=$HOME/Development/bin/ghc-$GHC_VERSION  
    
    make install

    # symbol links  
    cd $HOME/Development/bin
    rm -f ghc
    ln -s `pwd`/ghc-$GHC_VERSION ghc  
    
    # add $HOME/Development/bin/ghc to $PATH  
    # add this line to ~/.profile  
    export GHC_HOME=$HOME/Development/bin/ghc  
    export PATH=$GHC_HOME/bin:${PATH}
    
    # to use updated path without log off
    source ~/.profile
    
    # remove temporary files  
    cd $HOME/Downloads  
    rm -rfv ghc-$GHC_VERSION*

### cabal (package manager for haskell)

    # remove old  
    rm -rfv $HOME/.cabal
    rm -rfv $HOME/.ghc

#### cabal library

    # clone dist  
    cd $HOME/Downloads  
    curl -O "https://www.haskell.org/cabal/release/cabal-$CABAL_VERSION/$CABAL_DIST_FILENAME"  
    
    # extract   
    tar xzvf $CABAL_DIST_FILENAME  
    cd Cabal-$CABAL_VERSION  
    
    # build
    ghc --make Setup.hs
    ./Setup configure --user
    ./Setup build
    ./Setup install
    
    # Remove temporary files
    cd $HOME/Downloads
    rm -rfv Cabal-$CABAL_VERSION*

#### cabal-install

    # get distributive  
    cd $HOME/Downloads  
    curl -O "https://www.haskell.org/cabal/release/cabal-install-$CABAL_INSTALL_VERSION/$CABAL_INSTALL_DIST_FILENAME"  
    
    # extract archive  
    tar xzvf $CABAL_INSTALL_DIST_FILENAME  
    cd cabal-install-$CABAL_INSTALL_VERSION  
    
    # install  
    ./bootstrap.sh
    
    # remove temporary files  
    cd $HOME/Downloads  
    rm -rfv cabal-install-$CABAL_INSTALL_VERSION*  
    
    # add path to cabal to PATH environment
    export CABAL_HOME=$HOME/.cabal
    export PATH=$CABAL_HOME/bin:$PATH
