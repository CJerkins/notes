REF
https://www.youtube.com/watch?v=5oddMaThZY4
https://github.com/ninjhacks/gsmevil2

prerequisites:
- DragonOS
- HackRF One SDR or RTL-SDR

# GSM Tracking

Open a terminal, use command to scan the GSM spectrum. Everycountry might be slightly different Bands. Look up what bands operate to narrow your search. This tut only is capible of scanning GSM only bands, NOT LTE.
Use the first to scan with the HackRF and the 2nd for RTL-SDR type radios. These scans may take a moment so be patient.

`kal -s GSM900`

`grgsm_scanner -b GSM900`

Once you have your freqs that operate your vicinity, take note what they are. Mine are at the moment are:

![Screen Shot 2020-10-18 at 5.23.49 AM.png](../../_resources/Screen Shot 2020-10-18 at 5.23.49 AM.png)

The lower the power level the stronger the signal. Once you have chosen your freq run the following to start capturing the packets.

Use for RTL-SDRs
`grgsm_livemon -f <your_frequency>M`
Use for HackRF
`grgsm_livemon -f <your_frequency>M -args hackrf -g 40`

Then start gsmevil2, it is located at;
`cd /usr/src/gsmevil2`
`sudo pyhton3 Gsmevil.py`
Once you get some red text with no errors, goto a brower and type int he address bar `localhost`. You will an option to to imsi sniff. Click the wait a moment. Make sure in the terminal on the livemon you have numbers running through. This means you are capturing packets. If not try another freq,

![Screen Shot 2020-10-18 at 6.03.52 AM.png](../../_resources/Screen Shot 2020-10-18 at 6.03.52 AM.png)

or you can run ismi-catcher

`cd /usr/src/IMSI-catcher`
`sudo python3 simple_IMSI_catcher.py --sniffer`


# GSM packet capturing

You can capture a few different ways but the easiest is to use wireshark. Check out the REF for others sources.

REF
https://www.youtube.com/watch?v=krJJKjYdwgc

For easy capture run wireshark as root.
First make sure the grgsm_livemon is running first then run wireshark.
`sudo wireshark`
Start the capture on the loopback interface.

![Screen Shot 2020-10-18 at 7.09.40 AM.png](../../_resources/Screen Shot 2020-10-18 at 7.09.40 AM.png)

