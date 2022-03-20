REF
https://www.rtl-sdr.com/tag/iridium/
https://github.com/muccc/gr-iridium
https://github.com/muccc/iridium-toolkit

prerequisites:
- DragonOS
- HackRF One SDR


Location to the sdr config files. Modify if need be. Choose the correct config file for your SDR.
`cd /usr/src/gr-iridium/examples/`

This will capture the complete Iridium band using a connected HackRF and demodulate detected bursts into frames. It uses decimation to keep up if there are many bursts at the same time.

The final grep "A:OK" filters the output for frames which have a valid unique word.
`iridium-extractor -D 4 examples/hackrf.conf | grep "A:OK" > ~/output.bits`

Either extract some Iridium frames from the air or a file using gr-iridium (recommended) or use the legacy code located in the extractror-python directory if you don't want to install GNURadio (not recommended).

It is assumed that the output of the extractor has been written to output.bits. Iridium frames can be decoded with

`cd /usr/src/iridium-toolkit/`
`python2 iridium-parser.py -p ~/output.bits > ~/output.parsed`

Use `stats-voc.py` to see streams of captured voice frames: `./stats-voc.py output.parsed`

