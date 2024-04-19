<p align="center">
  <a href="https://solana.com">
    <img alt="Solana" src="https://i.imgur.com/IKyzQ6T.png" width="250" />
  </a>
</p>

[![Build status](https://badge.buildkite.com/3a7c88c0f777e1a0fddacc190823565271ae4c251ef78d83a8.svg)](https://buildkite.com/jito/jito-solana)

# About
This repository contains Jito's fork of the Solana validator.

# TxIngest

This is a patched version of the Solana validator.  It adds functionality whereby the validator will connect out to a
remote listener and deliver events to that listener describing the state of QUIC connections and various aspects of
them, as well as other events that may be interesting when evaluating QUIC based tx ingestion.

A new command-line option is added: --txingest-host HOST:PORT

This option will cause the validator to connect out to that host and port, and deliver txingest events to it.  The
validator will attempt to connect to this HOST:PORT once per second, and will attempt a re-connect if the connection
is broken.  If the listener is not present, the cost to the validator is minimal; it will buffer a limited number of
messages and then stop buffering while waiting for a new connection.

The listener will receive events as defined in sdk/src/txingest.rs as bincode serialized structures.

# Building

We recommend checking out our [Gitbook](https://jito-foundation.gitbook.io/mev/jito-solana/building-the-software) for more detailed instructions on building and running Jito-Solana. 
