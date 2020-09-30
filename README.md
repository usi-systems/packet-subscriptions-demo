# Packet Subscriptions Demo

This demo uses Packet Subscriptions to filter ITCH market data. It uses [p4app](https://github.com/p4lang/p4app)
to setup a Fat Tree topology with `K=4`. It runs an ITCH feed publisher and three subscribers.
The subscribers have different filters, which are compiled with the Camus
compiler to generate P4Runtime forwarding rules which are installed on all the
switches in the topology.

## Dependencies

- Docker
- Ocaml 4.04.0

## Step-by-Step Instructions

This was tested on a freshly installed Ubuntu 18.04 VM.

1. [Install Docker](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-18-04):
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
sudo apt install docker-ce
sudo usermod -aG docker ${USER}
su - ${USER}
```
2. Install OCaml 4.04.0 and the required packages with Opam:
```
sudo apt install opam m4 libgmp-dev
opam init
eval `opam config env`
opam switch 4.04.0
opam install async core core_extended oasis menhir ocamlgraph jbuilder ocaml-migrate-parsetree bignum mparser ppx_deriving ipaddr stdint
eval `opam config env`
```

3. Build the Camus compiler:
```
git clone --recursive https://github.com/usi-systems/packet-subscriptions-demo
cd packet-subscriptions-demo/itch.p4app/camus-compiler
make
```

4. Run the demo:
```
cd ../../
./p4app/p4app run itch.p4app
```

If the demo ran correctly, each of the three subscribers should have printed
the ITCH add\_order it received:
```
{'price': 2, 'shares': 1, 'stock': 'GOOGL   '}
{'price': 2, 'shares': 1, 'stock': 'GOOGL   '}
{'price': 2, 'shares': 1, 'stock': 'GOOGL   '}
```
