# OSM History Tools VM

This repository contains a [vagrant](https://www.vagrantup.com/) configuration file which will install some tools which are useful for dealing with OSM [history dumps](http://wiki.openstreetmap.org/wiki/Planet.osm/full). The software packages currently built are:

* [Osmium Tool](https://github.com/osmcode/osmium-tool), a "kitchen aid" with various different attachments for dealing with OSM data. (It slices! It dices!) One of the more important ones for dealing with history data is [`osmium time-filter`](https://github.com/osmcode/osmium-tool/blob/master/man/osmium-time-filter.md) for generating a planet file at a given point in time from a history dump.
* [OSM History Splitter](https://github.com/MaZderMind/osm-history-splitter), a tool for generating history files containing all the elements which ever intersected a bounding box. This is extremely useful for cutting the data down into manageable chunks.

## Requirements

You will need to download and install [vagrant](https://www.vagrantup.com/).


## Installation

From the directory where you checked out the source for this project, you should be able to run something like:

```
vagrant up
```

Which might take a while to run, as it might have to download the base image, install a whole bunch of development tools, checkout various sources and build them. After it has completed you should be able to run:

```
vagrant ssh
```

Which will give you a login shell on the built VM.

## Usage notes

Some binaries and libraries are installed on the VM in the `/usr/local` directory, so you sould either put this in one of your login configuration files, or run as appropriate before commands:

```
export LD_LIBRARY_PATH=/usr/local/lib
export PATH=$PATH:/usr/local/bin
```
