# Nushell

[Nushell](https://www.nushell.sh) ("*A new type of shell*") uses `heim`
to power two bundled plugins: `ps` and `sys`.\
They are providing information about system processes and system components
correspondingly as a structured data:

[`heim::process`](https://docs.rs/heim/*/heim/process/index.html) routines:

```
❯ ps | where name == node
━━━━━━━┯━━━━━━┯━━━━━━━━━┯━━━━━━━━┯━━━━━━━━━┯━━━━━━━━━
 pid   │ name │ status  │ cpu    │ mem     │ virtual 
───────┼──────┼─────────┼────────┼─────────┼─────────
 15447 │ node │ Running │ 0.0000 │ 18.5 MB │  4.7 GB 
━━━━━━━┷━━━━━━┷━━━━━━━━━┷━━━━━━━━┷━━━━━━━━━┷━━━━━━━━━
```

[`heim::net::io_counters`](https://docs.rs/heim/*/heim/net/fn.io_counters.html):

```
> sys | get net | where sent > 0
───┬────────┬─────────┬──────────
 # │ name   │ sent    │ recv 
───┼────────┼─────────┼──────────
 0 │ tun0   │ 30.3 MB │ 653.4 MB 
 1 │ wlp3s0 │ 97.7 MB │   1.2 GB 
 2 │ lo     │ 97.1 MB │  97.1 MB 
───┴────────┴─────────┴──────────
```

[`heim::host::platform`](https://docs.rs/heim/*/heim/host/fn.platform.html),
[`heim::host::uptime`](https://docs.rs/heim/*/heim/host/fn.uptime.html), and
[`heim::host::users`](https://docs.rs/heim/*/heim/host/fn.users.html):

```
> sys | get host
───┬───────┬────────────────┬──────────────────────────────────────────┬──────────┬────────┬────────────────────────────┬────────────────
 # │ name  │ release        │ version                                  │ hostname │ arch   │ uptime                     │ users 
───┼───────┼────────────────┼──────────────────────────────────────────┼──────────┼────────┼────────────────────────────┼────────────────
 0 │ Linux │ 5.4.15-arch1-1 │ #1 SMP PREEMPT Sun, 26 Jan 2020 09:48:50 │ tardis   │ x86_64 │ [row days hours mins secs] │ [table 1 rows] 
   │       │                │ +0000                                    │          │        │                            │  
───┴───────┴────────────────┴──────────────────────────────────────────┴──────────┴────────┴────────────────────────────┴────────────────

```