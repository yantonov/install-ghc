### how to install haskell GHC 7.8.3 + cabal 1.22 on mac os 

for your convinience these instuction is available as:  
[gist](https://gist.github.com/yantonov/23b15966eb46c45b73e0)
[git repo](https://github.com/yantonov/install-ghc)  


    # install xcode command line tools from here:  
    # [xcode command line tools site](https://developer.apple.com/downloads)

    # go to Downloads
    cd ~/Downloads

    # get ghc sources  
    curl -O https://downloads.haskell.org/~ghc/7.10.1/ghc-7.10.1-x86_64-apple-darwin.tar.bz2

    # extract files
    tar xvfj ghc-7.10.1-x86_64-apple-darwin.tar.bz2

    # goto extracted dir
    cd ghc-7.10.1

    # configure  
    mkdir -p $HOME/Development/bin/ghc-7.10.1  
    ./configure --prefix=$HOME/Development/bin/ghc-7.10.1

    # make and install  
    make install

    # symbol links  
    cd $HOME/Development/bin
    rm -fv ghc
    ln -s `pwd`/ghc-7.10.1/ ghc

    # add $HOME/Development/bin/ghc to $PATH
    GHC_HOME=$HOME/Development/bin/ghc
    PATH=$GHC_HOME/bin:${PATH}

    # remove temporary files  
    cd $HOME/Downloads  
    rm -rfv ghc-7.10.1*

### cabal (package manager for haskell)

    # remove old  
    rm -rf $HOME/.cabal
    rm -rf $HOME/.ghc

#### cabal library

    # clone dist  
    cd $HOME/Downloads  
    curl -O https://www.haskell.org/cabal/release/cabal-1.22.3.0/Cabal-1.22.3.0.tar.gz
    
    # extract   
    tar xzvf Cabal-1.22.3.0.tar.gz  
    cd Cabal-1.22.3.0  
    
    # build
    ghc --make Setup.hs
    ./Setup configure --user
    ./Setup build
    ./Setup install
    
    # Remove temporary files
    cd $HOME/Downloads
    rm -rfv Cabal-1.22.3.0*


#### cabal-install    

    # get distributive  
    cd $HOME/Downloads  
    curl -O https://www.haskell.org/cabal/release/cabal-install-1.22.3.0/cabal-install-1.22.3.0.tar.gz  
    
    # extract archive  
    tar xzvf cabal-install-1.22.3.0.tar.gz  
    cd cabal-install-1.22.3.0  
    
    # install  
    ./bootstrap.sh
    
    # remove temporary files  
    cd $HOME/Downloads  
    rm -rfv cabal-install-1.22.3.0*  
    
    # add path to cabal to PATH environment
    CABAL_HOME=$HOME/.cabal
    PATH=$CABAL_HOME/bin:$PATH
