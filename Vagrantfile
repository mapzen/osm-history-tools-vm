# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  # processing the history files requires a fair amount of memory.
  # 10gb is configured below, but you might get away with less
  # than that. 5 or 6gb is probably the bare minimum.
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "10240"
  end

  config.vm.synced_folder "/ssd/tmp", "/vagrant/tmp"

  config.vm.provision "shell", privileged: false, inline: <<-SHELL
set -e

sudo apt-get update
sudo apt-get install -y cmake build-essential zlib1g-dev libexpat1-dev \
  libbz2-dev libboost-dev libboost-program-options-dev libgdal-dev \
  libproj-dev libxml2-dev libgeos-dev libgeos++-dev libsparsehash-dev \
  libprotobuf-dev protobuf-compiler git doxygen graphviz

if [ -e osmium ]; then rm -rf osmium; fi
git clone https://github.com/joto/osmium.git
cd osmium/
git checkout 7f23002a95
make doc
sudo make install
cd ..

if [ -e OSM-binary ]; then rm -rf OSM-binary; fi
git clone https://github.com/scrosby/OSM-binary.git
cd OSM-binary/
make -C src
sudo make -C src install
cd ..

if [ -e osm-history-splitter ]; then rm -rf osm-history-splitter; fi
git clone https://github.com/MaZderMind/osm-history-splitter.git
cd osm-history-splitter/
git checkout e496e375a3
make CXX="g++ -std=c++11"
sudo cp osm-history-splitter /usr/local/bin
cd ..

if [ -e libosmium ]; then rm -rf libosmium; fi
# note that this should switch back to the upstream source once
# https://github.com/osmcode/libosmium/pull/122 is merged.
git clone https://github.com/zerebubuth/libosmium.git
cd libosmium/
git checkout 65d31e9035c
cd ..

if [ -e osmium-tool ]; then rm -rf osmium-tool; fi
git clone https://github.com/osmcode/osmium-tool.git
cd osmium-tool/
git checkout v1.2.1
mkdir build
cd build/
cmake -D OSMIUM_INCLUDE_DIR:PATH=$HOME/libosmium/include ..
make
sudo make install
cd ../..

SHELL
end
