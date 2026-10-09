# Harbour — a support desk

A support desk a team leaving Zendesk or Help Scout would recognise: tickets that are conversations, an inbox with
queues and promises, a help centre and a customer portal, mail in and out — built on the kits (see `app.osy` for what
each one brings).

## Click through a filled desk yourself

A fresh desk is empty, and a desk with a month of work in it is what you want to feel. The sample desk — 500 tickets,
~30 customer companies, a team of eight, a month of history — lives under `tests/`, so it is never compiled into what
`osy compile` deploys. To use it, hold the one test that builds it:

```console
osy test --test 'tests/hold.test.osy::Floor::a_filled_desk_to_click_through' --hold
```

It seeds the desk (several minutes), then keeps that test's copy of the app serving on your machine and opens it in
your browser signed in as **Ines**, who runs the desk — no password, no second step. Click around. **Ctrl-C** discards
it; so does the local platform going idle.

To feel it from a distance — every request made to wait, as a real network would — start the local platform with a
delay. It reads the variable when it starts, so stop it first:

```console
osy stop
OSY_SIMULATED_LATENCY_MS=40 osy test --test 'tests/hold.test.osy::Floor::a_filled_desk_to_click_through' --hold
```

The line under `⏸ HELD` says which delay the serving platform is actually applying. All of it is described in
`osy docs testing-holding-a-test`.

## Run its tests

```console
osy test                       # the whole suite
osy test --pixels              # the same, in a real browser, with screenshots
```
