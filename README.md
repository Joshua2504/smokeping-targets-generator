# smokeping-targets-generator

Reads INI formatted sets of hosts from stdin and produces a SmokePing compatible Targets file on stdout.

Smokeping has a weirdly verbose way of writing Target files which is painful to maintain manually. This tool aims to address the problem by using a much more succint input format and handling automatic generation of the various permutations on each host needed to specify its ID, location in the menu hierarchy, title etc.

## Usage

The INI formatted description should be of the form:

    [National:Foo Ltd]
    192.0.2.1=Host A
    hostb.example.com=Host B

    [National:Bar Ltd]
    203.0.113.1=Host One
    203.0.113.2=Host Two

Hosts may be specified at any point in the hierarchy, and hierarchies may be arbitrarily deep (e.g. `Level 1:Level 2:Level 3`).

This will produce a targets file like:

    *** Targets ***

    probe = FPing
    menu = Top
    title = Network Latency Graphs

    + 4E6174696F6E616C_National
    menu = National
    title = National

    ++ 466F6F204C7464_Foo_Ltd
    menu = Foo Ltd
    title = Foo Ltd

    +++ 3139322E302E322E31_192_0_2_1
    menu = Host A
    title = Host A (192.0.2.1)
    host = 192.0.2.1

    +++ 686F7374622E6578616D706C652E636F6D_hostb_example_com
    menu = Host B
    title = Host B (hostb.example.com)
    host = hostb.example.com

    ++ 426172204C7464_Bar_Ltd
    menu = Bar Ltd
    title = Bar Ltd

    +++ 3230332E302E3131332E31_203_0_113_1
    menu = Host One
    title = Host One (203.0.113.1)
    host = 203.0.113.1

    +++ 3230332E302E3131332E32_203_0_113_2
    menu = Host Two
    title = Host Two (203.0.113.2)
    host = 203.0.113.2
