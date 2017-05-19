# Melf

Melf is a GELF broker specially designed to handle multi-line logs generated by Docker GELF log driver.

# How It Works

1. Listen at a UDP port, accept standard GELF messages.

2. Use `_instance_id` field (set by Docker GELF log driver), identify messages belonging to the same series.

3. Cache one message per series for a limited time (<1s) for later evaluation.

3. Evaluate rules to determine whether new message (`short_message` field) should be combined with previous one.

4. Send assembled GELF message to the real endpoint.

# Limits

* Only Gzip and chuncked Gzip are supported

# Usage

`melf -b 127.0.0.1:12201 -t 127.0.0.1:12202 -r W`

* `b` - bind UDP address, default to `0.0.0.0:12201`

* `t` - target UDP address, default to `127.0.0.1:12202`

* `r` - combination of activated rules, default to `W`

  * `W` - Regard whitespace leading messages as multi-line message

# License

Ryan Wade <ryan@magi.systems>, MIT License, see `LICENSE` for details.
