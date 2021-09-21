# async-datachannel-wasm

[![License](https://img.shields.io/badge/license-MIT%2FApache--2.0-blue.svg)](https://github.com/wngr/async-datachannel-wasm)
[![Cargo](https://img.shields.io/crates/v/async-datachannel-wasm.svg)](https://crates.io/crates/async-datachannel-wasm)
[![Documentation](https://docs.rs/async-datachannel-wasm/badge.svg)](https://docs.rs/async-datachannel-wasm)

Async wrapper API for browser provided WebRTC API.

------

## Quickstart

```rust
use async_datachannel_wasm::{Message, PeerConnection, PeerId, RtcConfig};
use tokio::{
    io::{AsyncReadExt, AsyncWriteExt},
    sync::mpsc,
};

let ice_servers = vec!["stun:stun.l.google.com:19302"];
let (tx_sig_outbound, mut rx_sig_outbound) = mpsc::channel(32);
let (tx_sig_inbound, rx_sig_inbound) = mpsc::channel(32);
let listener = PeerConnection::new(ice_servers, (tx_sig_outbound, rx_sig_inbound))?;

// TODO: Wire up `tx_sig_inbound` and `rx_sig_outbound` to a signalling
// mechanism.

let mut dc = listener.dial("Hangout").await?;
dc.write(b"Hello").await?;

let mut buf = vec![0; 32];
let n = dc.read(&mut buf).await?;
assert_eq!(b"World", &buf[..n]);
```

## License

Licensed under either of

 * Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or http://www.apache.org/licenses/LICENSE-2.0)
 * MIT license ([LICENSE-MIT](LICENSE-MIT) or http://opensource.org/licenses/MIT)

at your option.

#### Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.