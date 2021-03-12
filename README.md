# HTTP/3 Protocol Stack Diagrams
Diagrams visualizing the HTTP/3, HTTP/2 and HTTP/1 protocol stacks and their features.

The h3-protocol-stack.drawio file contains the source for all diagrams, to be used with https://www.diagrams.net/.

The main font used is [Myriad Pro Condensed](https://fontsgeek.com/fonts/Myriad-Pro-Condensed).
The font for the improved readability version is [Atkinson Hyperlegible](https://brailleinstitute.org/freefont)

The diagrams and source are provided under the standard MIT license. 
PNG exports in this repository can be freely used without including license/copyright, but attribution is appreciated (By name "Robin Marx" or by linking to [@programmingart](https://twitter.com/programmingart)).

## Basic HTTP/3 vs HTTP/2 comparison

This basic diagram includes only the most important features and changes between the versions.

Notes:
- HTTP semantics are things like HTTP methods (GET/POST/PUT/...), HTTP headers (Content-Type, Cookie, ...), Caching logic, etc. that are common across HTTP versions and are defined in [separate documents](https://httpwg.org/specs/). The difference between HTTP/1.1, HTTP/2 and HTTP/3 then is mainly on how these semantics are encoded and transported on the wire, sometimes also referred to as "syntax". 

<img src="https://raw.githubusercontent.com/rmarx/h3-protocol-stack/main/png/protocol-stack-h2-h3.png" width="75%" />

## Extended HTTP/3 vs HTTP/2 comparison

This extended diagram includes most core features and adds nuances.

Notes:
- HTTP/3 no longer includes priorities itself, but refers to a new extension called "[Extensible priorities](https://tools.ietf.org/html/draft-ietf-httpbis-priority-03)". This new approach described priorities in HTTP headers and can also be used with HTTP/2 (and so arguably belongs in HTTP semantics)
- Somewhat similarly, alt-svc can also be used with HTTP/2, but is (semi-)required for HTTP/3, so it makes more sense to put it in that "column"
- Session Resumption and 0-RTT is conceptually a TLS feature, but it requires support from the Transport layer to really achieve "0-RTT" (e.g., through QUIC's combined Transport and Cryptographic handshake, or through TCP's Fast Open extension). This is why this feature is indicated in a special way.
- Similarly, QUIC uses Transport Parameters as one of its main extension mechanisms. While these carry QUIC-related parameters, they are transported as a TLS extension, overlapping both protocols. 
- QUIC uses TLS internally for deriving encryption keys and certificate-based authentication (among other things), but does its own framing and encryption/decryption of packets, using the keys it gets from TLS. Put differently, TLS records are not used and after the QUIC handshake TLS as a wire protocol is also not used.
- QUIC's main extension points are its Transport Parameters, versioning and framing, which are all listed as separate features, as they are all new. The TCP equivalent is the TCP options field in the TCP packet header. I did not include that explicitly as it was difficult to place in relation to the 3 QUIC features/to make visually clear they are intended to allow equivalent things (protocol evolution/configuration). 

<img src="https://raw.githubusercontent.com/rmarx/h3-protocol-stack/main/png/protocol-stack-h2-h3-extended.png" width="75%" />

## Basic HTTP/3 vs HTTP/2 vs HTTP/1.1 comparison

This version extends the diagram to include HTTP/1.1 for completeness of the evolutionary comparison.

<img src="https://raw.githubusercontent.com/rmarx/h3-protocol-stack/main/png/protocol-stack-h1-h2-h3.png" width="75%" />

## Basic HTTP/3 vs HTTP/2 comparison - Improved legibility

This basic diagram is the same as the first, but with a different font for improved legibility at smaller sizes.

<img src="https://raw.githubusercontent.com/rmarx/h3-protocol-stack/main/png/protocol-stack-h2-h3-improved-readability.png" width="75%" />

## Basic HTTP/3 vs HTTP/2 comparison - Layering focus

This basic diagram is the same as the first, but _attempts_ to align the protocols according to the traditional OSI layering model.

Notes:
- Security is not really a separate OSI layer, but is typically grouped with Transport. I separate them out for clarity.
- You could say QUIC overlaps with Application with regards to stream multiplexing. However, you could also say HTTP/2 overlaps with Transport for that feature. I've chosen not to reflect those nuances for simplicity. 

<img src="https://raw.githubusercontent.com/rmarx/h3-protocol-stack/main/png/protocol-stack-h2-h3-layers.png" width="75%" />
