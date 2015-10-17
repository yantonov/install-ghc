### How to install latest GHC 7.10.2 from source + stack 0.1.5.0 + cabal 1.22.4.0 + cabal-install 1.22.6.0 on mac os

for your convinience these instuction is available as:  
[gist](https://gist.github.com/yantonov/23b15966eb46c45b73e0)  
[git repo](https://github.com/yantonov/install-ghc)  

### settings

    GHC_VERSION="7.10.2"  
    ARCHITECTURE="x86_64"  
    PLATFORM="apple-darwin"  
    GHC_DIST_FILENAME="ghc-$GHC_VERSION-$ARCHITECTURE-$PLATFORM.tar.bz2"  
    
    CABAL_VERSION="1.22.4.0"  
    CABAL_DIST_FILENAME="Cabal-$CABAL_VERSION.tar.gz"  

    CABAL_INSTALL_VERSION="1.22.6.0"  
    CABAL_INSTALL_DIST_FILENAME="cabal-install-$CABAL_INSTALL_VERSION.tar.gz"

    STACK_VERSION="0.1.5.0"  
    STACK_ARCHITECTURE="x86_64"  
    STACK_PLATFORM="osx"  
    STACK_DIST_FILENAME="stack-$STACK_VERSION-$STACK_ARCHITECTURE-$STACK_PLATFORM.tar.gz"  

### ghc

    # install xcode command line tools from here:  
    # [xcode command line tools site](https://developer.apple.com/downloads)

    # go to Downloads
    cd ~/Downloads

    # get ghc sources  
    curl -O "https://downloads.haskell.org/~ghc/$GHC_VERSION/$GHC_DIST_FILENAME"

    # extract files
    tar xvfj $GHC_DIST_FILENAME

    # goto extracted dir
    cd ghc-$GHC_VERSION

    # configure  
    mkdir -p $HOME/Development/bin/ghc-$GHC_VERSION  
    ./configure --prefix=$HOME/Development/bin/ghc-$GHC_VERSION

    # make and install  
    make install
    # for xcode4:
    # see [explanation](http://haskell.1045720.n5.nabble.com/Installing-ghc-7-8-3-OS-X-bindist-fails-on-Xcode-4-CLI-only-machine-td5752678.html)
    # make install CC_CLANG_BACKEND=0

    # symbol links  
    cd $HOME/Development/bin
    rm -fv ghc
    ln -s `pwd`/ghc-$GHC_VERSION/ ghc

    # add $HOME/Development/bin/ghc to $PATH
    GHC_HOME=$HOME/Development/bin/ghc
    PATH=$GHC_HOME/bin:${PATH}

    # remove temporary files  
    cd $HOME/Downloads  
    rm -rfv ghc-$GHC_VERSION*

### stack (new package manager and build tool, preferrered way to manage dependencies)

    cd $HOME/Downloads  
    curl -L -O "https://github.com/commercialhaskell/stack/releases/download/v$STACK_VERSION/$STACK_DIST_FILENAME"  
    tar xvfz $STACK_DIST_FILENAME
    
    # move to home development dir  
    mkdir -p $HOME/Development/bin/stack-$STACK_VERSION/bin
    mv stack $HOME/Development/bin/stack-$STACK_VERSION/bin
    rm $STACK_DIST_FILENAME  
    cd $HOME/Development/bin  
    
    # sym link  
    ln -s `pwd`/stack-$STACK_VERSION stack  

    # add to PATH environment  
    STACK_HOME=$HOME/Development/bin/stack  
    PATH=$STACK_HOME/bin:$PATH  


### cabal (package manager for haskell)

    # remove old  
    rm -rf $HOME/.cabal
    rm -rf $HOME/.ghc

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
    CABAL_HOME=$HOME/.cabal
    PATH=$CABAL_HOME/bin:$PATH
    
