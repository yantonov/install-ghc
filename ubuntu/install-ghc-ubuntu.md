### How to install latest GHC 7.10.1 + cabal 1.22.3.0 from source on ubuntu

for your convinience these instuction is available as:  
[gist](https://gist.github.com/yantonov/10083524)  
[git repo](https://github.com/yantonov/install-ghc)  

### ghc

ubuntu prerequisites

    # Multiprecision arithmetic library developers tools, zlib  
    sudo apt-get install libgmp-dev zlib1g-dev -y  
    sudo -K

ghc installation

    # get distr  
    cd $HOME/Downloads  
    # 64 bit
    wget https://www.haskell.org/ghc/dist/7.10.1/ghc-7.10.1-x86_64-unknown-linux-deb7.tar.bz2   
    tar xvfj ghc-7.10.1-x86_64-unknown-linux-deb7.tar.bz2  
    # 32 bit
    # wget https://www.haskell.org/ghc/dist/7.10.1/ghc-7.10.1-i386-unknown-linux-deb7.tar.bz2
    # tar xvfj ghc-7.10.1-i386-unknown-linux-deb7.tar.bz2  
    cd ghc-7.10.1  

    # install to  
    mkdir $HOME/Development/bin/ghc-7.10.1  
    # or choose another path
    
    ./configure --prefix=$HOME/Development/bin/ghc-7.10.1  
    
    make install

    # symbol links  
    cd $HOME/Development/bin
    rm -f ghc
    ln -s `pwd`/ghc-7.10.1 ghc  
    
    # add $HOME/Development/bin/ghc to $PATH  
    # add this line to ~/.profile  
    export GHC_HOME=$HOME/Development/bin/ghc  
    export PATH=$GHC_HOME/bin:${PATH}
    
    # to use updated path without log off
    source ~/.profile
    
    # remove temporary files  
    cd $HOME/Downloads  
    rm -rfv ghc-7.10.1*

### cabal (package manager for haskell)

    # remove old  
    rm -rfv $HOME/.cabal
    rm -rfv $HOME/.ghc

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
    export CABAL_HOME=$HOME/.cabal
    export PATH=$CABAL_HOME/bin:$PATH
