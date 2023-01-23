# awcli

A command-line utility for interacting and integrating with alphaweighted.com and downloadable packages, such
as the market data components.

`awcli` can be used for testing/debugging - e.g. to check that a market data component is correctly configured
and receiving price data properly.

It can also be used as part of a larger toolchain - e.g. to stream prices into another program to filter them and alert when conditions are met.

## Quickstart

Run the command, via Docker, with no parameters to show basic usage information.  Docker will automatically download the necessary images if required:

```shell
> docker run ghcr.io/alphaweighted/awcli
alphaweighted CLI interface
...
```

Basic usage information, including a list of  the available commands, is shown when run.   More information on
each of the commands is shown below.

## Commands

### instrument

TODO

### price

Get the current/last price for an instrument, and display it

```shell
> docker run ghcr.io/alphaweighted/awcli --data localhost:6011 price OANDA:GBP_USD --format json
{"ref":"OANDA:GBP_USD","at":1674480061632,"price":"1.236265","bid":"1.23618","ask":"1.23635","bid_size":"1000000","ask_size":"1000000"}
```

### streamprice

Connect and stream indefinitely live prices for the specified instrument, e.g.:

```shell
> docker run ghcr.io/alphaweighted/awcli --data localhost:6011 streamprice OANDA:GBP_USD --format json
{"ref":"OANDA:GBP_USD","at":1674480061632,"price":"1.236265","bid":"1.23618","ask":"1.23635","bid_size":"1000000","ask_size":"1000000"}
{"ref":"OANDA:GBP_USD","at":1674480061848,"price":"1.23627","bid":"1.23619","ask":"1.23635","bid_size":"1000000","ask_size":"1000000"}
{"ref":"OANDA:GBP_USD","at":1674480062149,"price":"1.236265","bid":"1.23618","ask":"1.23635","bid_size":"1000000","ask_size":"1000000"}
{"ref":"OANDA:GBP_USD","at":1674480062510,"price":"1.23627","bid":"1.23619","ask":"1.23635","bid_size":"1000000","ask_size":"1000000"}
{"ref":"OANDA:GBP_USD","at":1674480065209,"price":"1.236345","bid":"1.23625","ask":"1.23644","bid_size":"1000000","ask_size":"1000000"}
{"ref":"OANDA:GBP_USD","at":1674480065320,"price":"1.23635","bid":"1.23627","ask":"1.23643","bid_size":"1000000","ask_size":"1000000"}
...
```

Press Ctrl+C, or send a TERM/KILL signal to the process to terminate it.

### candles

TODO

### streamcandles

TODO

## Configuration

`awcli` can be configured through a mix of environment variables, command line arguments and a config
file.  Command line arguments have the highest precedence, then environment variables, then the
config file and finally any internal default.

### format

The output produced by `awcli` may be for human (i.e. you!) consumption, or to save to a file or pipe into another program.  The default output format is `human`.  The available options are:

- `human` - Formatted human-readable output
- `plain` - plain text/CSV suitable for piping into other programs
- `json` - JSON, compact and typically on one line
- `jsonpp` - the same as `json`, but pretty-printed (i.e. indented and split over multiple lines for readability)

### config

Specify a configuration file to load using `--config=/foo/bar/config.ini`.  Note that the config file
must be visible to `awcli` inside the docker container - i.e. you must also map a config file (or the directory it is in) into the docker container as a volume. 

Config files in INI, YAML, and TOML are supported, and may contain any/all of the same configuration
options as are described on this page.

### verbosity

By default, only errors are emitted - this is the best setting for normal usage where you intend to use the output.

For debugging, specify `--verbosity=info|debug|trace` (i.e. `info`, `debug` or `trace`).  `trace` will
produce the most verbose output.  This can be helpful in diagnosing why something isn't working the way you intended.

## Security

`awcli` is provided through Docker, partly for ease of packaging/distribution - but also so that it runs in a sandbox on your infrastructure; it has no access to any disks nor data on the machine you run it on.

Networking is, by default, unrestricted - but you can configure Docker to restrict network connectivity.   Normal operation of `awcli` only requires connectivity to any market data modules you run, and/or to https://alphaweighted.com

## License

By using this component you agree to [the license](LICENSE), including the notices below:

LICENSEE ACKNOWLEDGES AND UNDERSTANDS THAT THE SERVICE AND ANY SOFTWARE MAY CONTAIN ERRORS, OMISSIONS, AND PROBLEMS. LICENSEE HEREBY ACCEPTS THE SERVICE AND SOFTWARE, "AS IS" AND WITH ALL FAULTS, DEFECTS AND ERRORS AND LICENSEE UNDERSTANDS THAT IT ASSUMES ALL RISKS OF USE, QUALITY, AND PERFORMANCE. NEITHER ALPHAWEIGHTED NOR ANY OF ALPHAWEIGHTED'S LICENSORS MAKE ANY EXPRESS WARRANTIES, AND EACH OF THEM DISCLAIMS ALL IMPLIED WARRANTIES, INCLUDING IMPLIED WARRANTIES OF ACCURACY, MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT.

LICENSEE AGREES AND ACKNOWLEDGES THAT NEITHER ALPHAWEIGHTED NOR ANY OF ITS LICENSORS MAY BE HELD LIABLE FOR ANY CLAIM, LOSS, DAMAGES, EXPENSES OR COSTS OF AN INDIRECT NATURE, INCLUDING CONSEQUENTIAL OR SPECIAL DAMAGES, LOST PROFITS OR OTHERWISE AND IN NO EVENT SHALL THEY BE LIABLE FOR ANY DAMAGES IN EXCESS OF THE AMOUNT OF FEES PAID TO ALPHAWEIGHTED BY LICENSEE (IF ANY) UNDER THIS AGREEMENT DURING THE IMMEDIATELY PRECEDING SIX MONTHS. THIS LIMITATION APPLIES TO ALL CAUSES OF ACTION OR CLAIMS IN THE AGGREGATE, INCLUDING, WITHOUT LIMITATION, BREACH OF CONTRACT, BREACH OF WARRANTY, INDEMNITY, NEGLIGENCE, STRICT LIABILITY, MISREPRESENTATION AND OTHER TORTS. THE LIMITATIONS IN THIS SECTION APPLY TO YOU ONLY TO THE EXTENT THEY ARE LAWFUL IN YOUR JURISDICTION.
