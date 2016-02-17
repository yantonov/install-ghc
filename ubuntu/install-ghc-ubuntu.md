### How to install latest GHC 7.10.3 from source  + stack 1.0.2 + cabal 1.22.7.0 + cabal-install 1.22.8.0 on ubuntu

for your convinience these instuction is available as:  
[gist](https://gist.github.com/yantonov/10083524)  
[git repo](https://github.com/yantonov/install-ghc)

# preferred way install stack than install ghc

### settings

    DOWNLOADS_DIR=$HOME/Downloads

    STACK_VERSION="1.0.2"  
    STACK_ARCHITECTURE="x86_64"  
    STACK_PLATFORM="osx"  
    STACK_DIST_FILENAME="stack-$STACK_VERSION-$STACK_PLATFORM-$STACK_ARCHITECTURE.tar.gz"  
    STACK_DIST_UNZIPPED_DIR="stack-$STACK_VERSION-$STACK_PLATFORM-$STACK_ARCHITECTURE"
    STACK_DIST_URL="https://www.stackage.org/stack/osx-x86_64"
    STACK_INSTALL_DIR="$HOME/Development/bin"
    STACK_TARGET_DIR="stack-$STACK_VERSION"

### stack (package manager and build tool, preferrered way to manage dependencies)

    cd $DOWNLOADS_DIR
    
    curl -L -o $STACK_DIST_FILENAME $STACK_DIST_URL  
    tar xvfz $STACK_DIST_FILENAME
    
    # move to home development dir  
    rm -rf $STACK_INSTALL_DIR/$STACK_TARGET_DIR  
    mv $STACK_DIST_UNZIPPED_DIR $STACK_INSTALL_DIR/$STACK_TARGET_DIR
    
    cd $STACK_INSTALL_DIR  
    
    # sym link
    rm -rvi stack  
    ln -s `pwd`/$STACK_TARGET_DIR stack  

    # add to PATH environment  
    STACK_HOME=$HOME/Development/bin/stack  
    PATH=$STACK_HOME:$PATH

    # clean up
    cd $DOWNLOADS_DIR
    rm -rf stack-$STACK_VERSION*

### install ghc using stack

    # install ghc
    stack setup
    # run repl
    stack ghci


# OLD way (manual installation of ghc)

### settings
    
    DOWNLOADS_DIR="$HOME/Downloads"

    GHC_VERSION="7.10.3"  
    ARCHITECTURE="x86_64"  
    # for 32 bit ARCHITECTURE="i386"      
    PLATFORM="deb8-linux"  
    GHC_DIST_FILENAME="ghc-$GHC_VERSION-$ARCHITECTURE-$PLATFORM.tar.bz2"
    
    CABAL_VERSION="1.22.7.0"
    CABAL_DIST_FILENAME="Cabal-$CABAL_VERSION.tar.gz"

    CABAL_INSTALL_VERSION="1.22.8.0"
    CABAL_INSTALL_DIST_FILENAME="cabal-install-$CABAL_INSTALL_VERSION.tar.gz"

### ghc

### ubuntu prerequisites

    # Multiprecision arithmetic library developers tools, zlib  
    sudo apt-get install libgmp-dev zlib1g-dev -y  
    sudo -K

### ghc installation

    # get distr  
    cd $DOWNLOADS_DIR
    GHC_DIST_URL="https://www.haskell.org/ghc/dist/$GHC_VERSION/$GHC_DIST_FILENAME"
    curl -L -O $GHC_DIST_URL  
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
    cd $DOWNLOADS_DIR  
    rm -rfv ghc-$GHC_VERSION*

### cabal (package manager, old way to manage dependencies)

    # remove old  
    rm -rfv $HOME/.cabal
    rm -rfv $HOME/.ghc

#### cabal library

    # clone dist  
    cd $DOWNLOADS_DIR  
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
    cd $DOWNLOADS_DIR
    rm -rfv Cabal-$CABAL_VERSION*

#### cabal-install

    # get distributive  
    cd $DOWNLOADS_DIR  
    curl -O "https://www.haskell.org/cabal/release/cabal-install-$CABAL_INSTALL_VERSION/$CABAL_INSTALL_DIST_FILENAME"  
    
    # extract archive  
    tar xzvf $CABAL_INSTALL_DIST_FILENAME  
    cd cabal-install-$CABAL_INSTALL_VERSION  
    
    # install  
    ./bootstrap.sh
    
    # remove temporary files  
    cd $DOWNLOADS_DIR  
    rm -rfv cabal-install-$CABAL_INSTALL_VERSION*  
    
    # add path to cabal to PATH environment
    export CABAL_HOME=$HOME/.cabal
    export PATH=$CABAL_HOME/bin:$PATH

