# Https Replay Attack

> A subset of Man-in-the-middle(MITM) attack, that copy request and send it to the server again pretending to be you

## Is it possible to perform replay attack on https connection?

- YES

## How it happened?

- the current version of TLS1.3 _**with 0-rtt enable is vulnerable**_
    - see [Cloudflare - Zero Round Trip Time Resumption ](https://blog.cloudflare.com/introducing-0-rtt/) under
      What’s the catch?
    - hacker(MITM) copy of the encrypted 0-RTT data (containing your first request) and send it to the server again to
      replay
    - can be resolved by disabling 0-rtt on server side
- [Trick the browser](https://vnhacker.blogspot.com/2015/12/bad-life-advice-never-give-up-replay.html)
    - you have no control of your clients' browser, cannot be resolved on server side
  > 1. The adversary sets itself up as a TCP layer relay for the targeted TLS connection to, say, google.com.
  > 2. When the adversary detects a request that it wants to replay (using traffic analysis), it copies all relevant TLS
       records, and instead of relaying the HTTP response from the server it just closes the socket to Chrome. It keeps
       the leg to google.com open.
  > 3. Over a fresh socket, Chrome would automatically retry the (presumably failed) request. The adversary would then
       forward it normally to google.com.
  > 4. Adversary replays the copied records to google.com, which will happily accept them.

## Conclusion

- You should always implement api that follow http semantics(eg: _**idempotent**_)
- Applications need to be replay safe to work with modern browsers, whether they support 0-RTT **_or not_**. Browsers
  replay data all the time due to normal network glitches

## Ref

- [Cloudflare - Zero Round Trip Time Resumption ](https://blog.cloudflare.com/introducing-0-rtt/)
- [trick the browser into to replaying requests](https://vnhacker.blogspot.com/2015/12/bad-life-advice-never-give-up-replay.html)