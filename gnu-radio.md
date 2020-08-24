# GNU Radio

## gr-gsm

### Install wireshark

```sh
sudo apt install wireshark
```

### Install gr-gsm

Based on [these instructions](https://osmocom.org/projects/gr-gsm/wiki/Installation).

Start by installing dependencies:

```sh
sudo apt-get update && \
sudo apt-get install -y \
    cmake \
    autoconf \
    libtool \
    pkg-config \
    build-essential \
    python-docutils \
    libcppunit-dev \
    swig \
    doxygen \
    liblog4cpp5-dev \
    python-scipy \
    python-gtk2 \
    gnuradio-dev \
    gr-osmosdr \
    libosmocore-dev
```

Clone and build `gr-gsm`:

```sh
git clone https://git.osmocom.org/gr-gsm
cd gr-gsm
mkdir build
cd build
cmake ..
mkdir $HOME/.grc_gnuradio/ $HOME/.gnuradio/
make -j $(nproc)
sudo make install
sudo ldconfig
```

### Sniff GSM packets

Start WireShark as follows:

```sh
wireshark -k -f udp -Y gsmtap -i lo
```

Run the `gr-gsm` decoder:

```sh
grgsm_livemon -f 945M
```
