###  How to install latest GHC from source + latest stack + cabal 1.24.0.0 + cabal-install 1.24.0.0 on mac os

for your convinience these instuction is available as:  
[gist](https://gist.github.com/yantonov/23b15966eb46c45b73e0)  
[git repo](https://github.com/yantonov/install-ghc)  
    
# Prefererred way install stack then install ghc using stack    

### stack (package manager and build tool, preferrered way to manage dependencies)

    # settings
    DOWNLOADS_DIR=$HOME/Downloads

    STACK_ARCHITECTURE="x86_64"  
    STACK_PLATFORM="osx"  
    STACK_DIST_URL="https://www.stackage.org/stack/$STACK_PLATFORM-$STACK_ARCHITECTURE"
    STACK_INSTALL_DIR="$HOME/Development/bin"

    cd $DOWNLOADS_DIR
    
    curl -L -O $STACK_DIST_URL  
    STACK_DIST_FILENAME=`ls -1 | grep 'stack-.*\.tar\.gz'`
    STACK_VERSION=`echo $STACK_DIST_FILENAME | sed -E 's/stack-([.0-9]+)-.*/\1/'`
    STACK_TARGET_DIR="stack-$STACK_VERSION"

    tar xvf $STACK_DIST_FILENAME  
    STACK_DIST_UNZIPPED_DIR=`ls -d -1 stack-*/`
    
    # move to home development dir
    rm -rf $STACK_INSTALL_DIR/$STACK_TARGET_DIR  
    mkdir -p $STACK_INSTALL_DIR
    mv $STACK_DIST_UNZIPPED_DIR $STACK_INSTALL_DIR/$STACK_TARGET_DIR
    
    # sym link
    cd $STACK_INSTALL_DIR
    # delete old link
    rm -fv stack  
    ln -s `pwd`/$STACK_TARGET_DIR stack  

    # add to PATH environment  
    STACK_HOME=$STACK_INSTALL_DIR/stack  
    PATH=$STACK_HOME:$PATH

    # clean up
    cd $DOWNLOADS_DIR  
    rm -rfv stack-$STACK_VERSION*

### then install ghc

    # install ghc  
    stack setup  
    # run repl  
    stack ghci  

# OLD way (manual installation of ghc)

### settings

    DOWNLOADS_DIR=$HOME/Downloads

    ARCHITECTURE="x86_64"  
    PLATFORM="apple-darwin"  
    GHC_VERSION=`curl https://downloads.haskell.org/~ghc/ | grep -E '([.0-9]+)/' | sed -E 's/.*>([.0-9]+).*/\1/' | uniq | sort -r | head -n 1`

    GHC_DIST_FILENAME="ghc-$GHC_VERSION-$ARCHITECTURE-$PLATFORM.tar.xz"
    GHC_DIST_FILE="https://downloads.haskell.org/~ghc/$GHC_VERSION/$GHC_DIST_FILENAME"
    
    CABAL_VERSION="1.24.0.0"  
    CABAL_DIST_FILENAME="Cabal-$CABAL_VERSION.tar.gz"  

    CABAL_INSTALL_VERSION="1.24.0.0"  
    CABAL_INSTALL_DIST_FILENAME="cabal-install-$CABAL_INSTALL_VERSION.tar.gz"

### ghc

    # install xcode command line tools from here:  
    # [xcode command line tools site](https://developer.apple.com/downloads)

    # go to Downloads  
    cd $DOWNLOADS_DIR

    # get ghc sources  
    curl -O $GHC_DIST_FILE  

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
    cd $DOWNLOADS_DIR
    rm -rfv ghc-$GHC_VERSION*    

### cabal (package manager, old way to manage dependencies)

    # remove old  
    rm -rf $HOME/.cabal
    rm -rf $HOME/.ghc

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
    curl -L -O "https://www.haskell.org/cabal/release/cabal-install-$CABAL_INSTALL_VERSION/$CABAL_INSTALL_DIST_FILENAME"  
    
    # extract archive  
    tar xzvf $CABAL_INSTALL_DIST_FILENAME  
    cd cabal-install-$CABAL_INSTALL_VERSION  
    
    # install  
    ./bootstrap.sh
    
    # remove temporary files  
    cd $DOWNLOADS_DIR  
    rm -rfv cabal-install-$CABAL_INSTALL_VERSION*  
    
    # add path to cabal to PATH environment
    CABAL_HOME=$HOME/.cabal
    PATH=$CABAL_HOME/bin:$PATH  
