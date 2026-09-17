# earwig-webex-oauth

The OAuth redirect (bounce) page for [Earwig](https://github.com/liptonj/earwig), a
local-first, bot-free meeting transcription app for macOS.

Live at **<https://liptonj.github.io/earwig-webex-oauth/>**.

## Why this exists

The Webex developer portal will not register a custom URL scheme as an integration's
redirect URI — it answers `us.5ls.earwig://webex-oauth` with *"A URI must begin with a
protocol identifier followed with `://`"* — so a real registration is always `http(s)`.
But `ASWebAuthenticationSession` cannot intercept an `http(s)` callback: its completion
handler fires only on a custom-scheme match, so handing it `https` opens the browser
sheet and then hangs forever with no error.

This page bridges the two. Webex redirects here, and the page forwards the untouched
`?code=&state=` query on to `us.5ls.earwig://webex-oauth`, which the app does intercept.

## What it does not do

It is deliberately self-contained: no scripts, styles, fonts or images are fetched from
anywhere, so the authorization code is never handed to a third party. `referrer` is set
to `no-referrer`, so the code cannot leak through a `Referer` header either. The code is
single-use and useless without the PKCE verifier and the client secret, both of which
never leave the user's Mac.

There is no server here — GitHub Pages serves one static file. Nothing is logged, stored
or forwarded anywhere except to the local app.

## Source of truth

`index.html` is a copy of `docs/webex-oauth-redirect.html` in the Earwig repository.
Change it there first.
