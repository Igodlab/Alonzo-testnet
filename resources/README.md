# Resources
This folder contains some useful resources, including some simple [pre-built Plutus scripts](plutus-scripts) and example [sources](plutus-sources), including for the `HelloWorld` and `AlwaysSucceeds` scripts.

The sources need to be built with GHC 8.10.4 (8.10.5 may also work), e.g.

``cabal build all -w ghc-8.10.4``

You may also need to install `libsodium` as described [here](https://github.com/input-output-hk/cardano-node/blob/master/doc/getting-started/install.md/), and update `cabal.project.local` to include:

```
package cardano-crypto-praos
  flags: -external-libsodium-vrf
```

Further examples and scripts can be found in the `cardano-node` repository:

[https://github.com/input-output-hk/cardano-node/tree/master/plutus-example](https://github.com/input-output-hk/cardano-node/tree/master/plutus-example)

[https://github.com/input-output-hk/cardano-node/tree/master/scripts/plutus](https://github.com/input-output-hk/cardano-node/tree/master/scripts/plutus)
