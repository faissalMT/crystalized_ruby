# Native Ruby extensions written in Crystal

Proof of concept (and more work coming!) for writing a Ruby extension in Crystal. This doesn't use FFI. There's some problems, and it's pretty rough for now, but it's going to keep improving. 

I'd like to get this to a point that it's incredibly simple and quick to write something and have it functional.

## Class conversion status

| Ruby class  | Ruby => Crystal | Crystal => Ruby |
| ----------- | :-------------: | :-------------: |
| String      | :white_check_mark: | :white_check_mark: |
| Symbol      | :x:                | :white_check_mark: |
| Integer     | :white_check_mark: | :white_check_mark: |
| Float       | :x:                | :x:                |
| Hash        | :x:                | :white_check_mark: |
| Array       | :white_check_mark: | :white_check_mark: |
| Regexp      | :white_check_mark: | :white_check_mark: |
| Nil         | :white_check_mark: | :white_check_mark: |
| True        | :white_check_mark: | :white_check_mark: |
| False       | :white_check_mark: | :white_check_mark: |

## Updates

See [updates.md](updates.md)

# Problems

* Negative integers don't convert correctly
* Floats aren't working
* I am having trouble with defining methods with various parameter counts. There's additional Crystal libs just for defining methods with zero or two parameters. This is obnoxious and the biggest annoyance I have right now, so I'd love to fix that soon.
* I can't get a proc as a C callback working. There's some broken code commented out. Would love assistance from someone more knowledgeable. Right now this is for converting a Ruby hash to a Crystal hash. 

# How to get this working

Minimum crystal version: 0.16.0

Make sure Crystal is installed (Homebrew on OSX is fine)

Test and benchmark scripts both require [fast_blank](https://github.com/SamSaffron/fast_blank) and [active_support](https://github.com/rails/rails/tree/master/activesupport), mainly for comparison. Test script also uses [descriptive_statistics](https://github.com/thirtysixthspan/descriptive_statistics), and the benchmark script uses [benchmark-ips](https://github.com/evanphx/benchmark-ips). None of these are required, except to run those two Ruby scripts. There's a Gemfile to install them if desired.

```
make
ruby test.rb
ruby benchmark.rb
```

# Benchmarking

There is a benchmark script that compares a few things. The methods replicating ActiveSupport methods are copy-pasted, not even re-implemented from scratch. This uses the improved String#blank? method from AS 5.0, but it's not a hard requirement.

Some highlights (see results.txt for more):

```
Comparison:
        cr fibonacci:    22743.9 i/s
        rb fibonacci:      923.2 i/s - 24.63x slower

Comparison:
     empty string rb:  7591363.0 i/s
empty string crystal:  6973264.7 i/s - same-ish: difference falls within error

Comparison:
     CR blank string:  2393668.6 i/s
     AS blank string:   923967.3 i/s - 2.59x slower

Comparison:
           cr squish:   691693.1 i/s
           AS squish:   202554.1 i/s - 3.41x slower

Comparison:
          cr ordinal:  5044785.3 i/s
          AS ordinal:  1775271.7 i/s - 2.84x slower

Comparison:
       fast_blank rb:  6599201.8 i/s
       blank crystal:  2199386.4 i/s - 3.00x slower
```


# Thanks and influences

There's three main projects that I've gained knowledge from to get this fully working:

- [manastech/crystal_ruby](https://github.com/manastech/crystal_ruby)
- [notozeki/crystal_example_ext](https://gist.github.com/notozeki/7159a9d9ab9707a22129)
- [gaar4ica/ruby_ext_in_crystal_math](https://github.com/gaar4ica/ruby_ext_in_crystal_math)

These have all been incredibly helpful, and this is very closely modeled after the last two sources. @gaar4ica's also includes a PDF from a talk she gave at Fosdem, which was highly informative.

# Future Ideas + Contributing

I'd like to get this more fully fleshed out, more functional, and get it usable. There is some question as to whether or not writing a native Ruby extension in Crystal is a useful idea, and I'd love to learn more about both why it _would_ and would _not_ be worthwhile, from people out there who are far more knowledgeable than I am.

If anyone is interested in this concept, please reach out to me either on this repo or on Twitter (@phoffer8). I'd love to collaborate with anyone interested, and just learn more in general.
