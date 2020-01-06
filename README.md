[![Status](https://travis-ci.org/apoelstra/rust-jsonrpc.png?branch=master)](https://travis-ci.org/apoelstra/rust-jsonrpc)

# Rust JSONRPC Client

Rudimentary support for sending JSONRPC 2.0 requests and receiving responses.

To send a request which should retrieve the above structure, consider the following
example code

```rust
extern crate jsonrpc;
extern crate serde;
#[macro_use] extern crate serde_derive;

#[derive(Deserialize, Debug)]
struct MyStruct {
    elem1: bool,
    elem2: String,
    elem3: Vec<usize>
}

fn main() {
    // The two Nones are for user/pass for authentication
    let client = jsonrpc::client::Client::new(String::from("localhost"), None, None);
    let request = &client.build_request("getmystruct", &[]);
    match client.send_request(request).and_then(|res| res.into_result::<MyStruct>()) {
        Ok(resp) => println!("response recieved: {:?}", resp),
        Err(e) => println!("error: {:?}", e),
    }
}
```
