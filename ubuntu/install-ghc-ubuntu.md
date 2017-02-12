### How to install latest GHC from source + latest stack + cabal + cabal-install on ubuntu

for your convinience these instuction is available as:  
[gist](https://gist.github.com/yantonov/10083524)  
[git repo](https://github.com/yantonov/install-ghc)

# preferred way install stack than install ghc
### stack (package manager and build tool, preferrered way to manage dependencies)

    # settings
    DOWNLOADS_DIR=$HOME/Downloads

    STACK_ARCHITECTURE="x86_64"  
    STACK_PLATFORM="linux"  
    STACK_DIST_URL="https://www.stackage.org/stack/$STACK_PLATFORM-$STACK_ARCHITECTURE"
    STACK_INSTALL_DIR="$HOME/Development/bin"

    cd $DOWNLOADS_DIR
    
    curl -L -O $STACK_DIST_URL  
    
    # in case if error like this: 
    #curl: (77) error setting certificate verify locations: CAfile: 
    # /etc/pki/tls/certs/ca-bundle.crt CApath: 
    # ...
    # create ~/.curlrc file
    # and put this lines to it
    # capath=/etc/ssl/certs/
    # cacert=/etc/ssl/certs/ca-certificates.crt
    
    STACK_DIST_FILENAME=`ls -1 | grep 'stack-.*\.tar\.gz'`
    STACK_VERSION=`echo $STACK_DIST_FILENAME | sed -E 's/stack-([.0-9]+)-.*/\1/'`
    STACK_TARGET_DIR="stack-$STACK_VERSION"
    echo "stack $STACK_VERSION will be installed"  

    tar xvf $STACK_DIST_FILENAME  
    STACK_DIST_UNZIPPED_DIR=`ls -d -1 stack-*/`
    
    # move to home development dir
    rm -rf $STACK_INSTALL_DIR/$STACK_TARGET_DIR  
    mkdir -p $STACK_INSTALL_DIR
    mv $STACK_DIST_UNZIPPED_DIR $STACK_INSTALL_DIR/$STACK_TARGET_DIR
    
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

    ### ubuntu prerequisites

    # Multiprecision arithmetic library developers tools, zlib  
    sudo apt-get install libgmp-dev zlib1g-dev -y  
    sudo -K
    
### ghc  
    
    DOWNLOADS_DIR="$HOME/Downloads"
    
    GHC_VERSION=`curl https://downloads.haskell.org/~ghc/ | grep -E '([.0-9]+)/' | sed -E 's/.*>([.0-9]+).*/\1/' | uniq | sort -r | head -n 1`
    ARCHITECTURE="x86_64"  
    # for 32 bit ARCHITECTURE="i386"  
    PLATFORM="deb8-linux"  
    GHC_DIST_FILENAME="ghc-$GHC_VERSION-$ARCHITECTURE-$PLATFORM.tar.xz"
    
    ### ghc installation
    
    echo "GHC $GHC_VERSION will be installed"  

    # get distr  
    cd $DOWNLOADS_DIR
    GHC_DIST_URL="https://www.haskell.org/ghc/dist/$GHC_VERSION/$GHC_DIST_FILENAME"
    curl -L -O $GHC_DIST_URL  
    tar xvfJ $GHC_DIST_FILENAME  
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

    CABAL_VERSION=`curl https://www.haskell.org/cabal/release/cabal-latest/ | grep -E 'Cabal-([.0-9]+\.[0-9]).*' | sed -E 's/.*>Cabal-([.0-9]+\.[0-9]).*/\1/' | head -n 1`      
    CABAL_DIST_FILENAME="Cabal-$CABAL_VERSION.tar.gz"
    
    echo "Cabal $CABAL_VERSION will be installed..."  

    # clone dist  
    cd $DOWNLOADS_DIR  
    curl -O "https://www.haskell.org/cabal/release/cabal-$CABAL_VERSION/$CABAL_DIST_FILENAME"  
    
    # extract   
    tar xzvf $CABAL_DIST_FILENAME  
    cd Cabal-$CABAL_VERSION  
    
    # build
    ghc --make Setup.hs
    ./Setup configure --user --enable-library-profiling
    ./Setup build
    ./Setup install
    
    # Remove temporary files
    cd $DOWNLOADS_DIR
    rm -rfv Cabal-$CABAL_VERSION*

#### cabal-install

    CABAL_INSTALL_VERSION=`curl https://www.haskell.org/cabal/release/cabal-install-latest/ | grep -E 'cabal-install-([.0-9]+\.[0-9]).*' | sed -E 's/.*>cabal-install-([.0-9]+\.[0-9]).*/\1/' | head -n 1`      
    CABAL_INSTALL_DIST_FILENAME="cabal-install-$CABAL_INSTALL_VERSION.tar.gz"  
    
    echo "cabal install $CABAL_INSTALL_VERSION will be installed..."  

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

