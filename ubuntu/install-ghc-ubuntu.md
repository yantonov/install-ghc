### How to install latest GHC 7.10.2 from source  + stack 0.1.6.0 + cabal 1.22.4.0 + cabal-install 1.22.6.0 on ubuntu

for your convinience these instuction is available as:  
[gist](https://gist.github.com/yantonov/10083524)  
[git repo](https://github.com/yantonov/install-ghc)

### settings

    GHC_VERSION="7.10.2"  
    ARCHITECTURE="x86_64"  
    # for 32 bit ARCHITECTURE="i386"      
    PLATFORM="unknown-linux"  
    GHC_DIST_FILENAME="ghc-$GHC_VERSION-$ARCHITECTURE-$PLATFORM-deb7.tar.bz2"
    
    CABAL_VERSION="1.22.4.0"
    CABAL_DIST_FILENAME="Cabal-$CABAL_VERSION.tar.gz"

    CABAL_INSTALL_VERSION="1.22.6.0"
    CABAL_INSTALL_DIST_FILENAME="cabal-install-$CABAL_INSTALL_VERSION.tar.gz"

    STACK_VERSION="0.1.6.0"  
    STACK_ARCHITECTURE="x86_64"  
    STACK_PLATFORM="linux"  
    STACK_DIST_FILENAME="stack-$STACK_VERSION-$STACK_PLATFORM-$STACK_ARCHITECTURE.tar.gz"  

### ghc

### ubuntu prerequisites

    # Multiprecision arithmetic library developers tools, zlib  
    sudo apt-get install libgmp-dev zlib1g-dev -y  
    sudo -K

### ghc installation

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

### stack (package manager and build tool, preferrered way to manage dependencies)

    cd $HOME/Downloads
    STACK_DIST_URL="https://github.com/commercialhaskell/stack/releases/download/v$STACK_VERSION/$STACK_DIST_FILENAME"
    curl -L -O $STACK_DIST_URL  
    tar xvfz $STACK_DIST_FILENAME
    
    # move to home development dir  
    mkdir -p $HOME/Development/bin/stack-$STACK_VERSION/bin
    mv stack $HOME/Development/bin/stack-$STACK_VERSION/bin
    rm $STACK_DIST_FILENAME  
    cd $HOME/Development/bin  
    
    # sym link
    rm -ri stack  
    ln -s `pwd`/stack-$STACK_VERSION stack  

    # add to PATH environment  
    STACK_HOME=$HOME/Development/bin/stack  
    PATH=$STACK_HOME/bin:$PATH

    # clean up
    cd $HOME/Downloads
    rm -rf stack-$STACK_VERSION*

### cabal (package manager, old way to manage dependencies)

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

