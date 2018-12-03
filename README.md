gr-dect2
========

Original Code Forked From: https://github.com/pavelyazev/gr-dect2

This project was developed to demonstrate the possibility of real-time DECT 
voice channel decoding by Gnuradio. It allows to listen to a voice when encryption
isn't applied. As an example DECT digital baby monitors don't perform 
encryption. 

**You take full responsibility when using this software. It is intended for demonstration purposes and use of it for eavesdropping on phone calls is illegal in most countries.**


Changes in my Fork
==================

I've added the US DECT channels (among others) to enable the use of the 1.9ghz band with the software.
I'm also working to hopefully implement the ability to select RFP or PP as your part rather than running all over the place or perhaps
scanning all parts automatically.
Finally I'd like to add a scan function so that you don't need to manually go through everything.

Shoutout to the original developer Pavelyazev for whom without this would be impossible to do on an SDR. We're getting close to something
that is comparable to the Com-on-air card for monitoring and I find it very exciting.

Hardware requirements
----------------------

DECT operates in the 1880–1900MHz band and occupies ten channels from 
1881.792MHz to 1897.344MHz. So in order to receive DECT digital stream an 
appropriate hardware is necessary. This project was developed and tested with 
USRP2 + WBX daughterboard and USRP B200. 

It also should work with other SDR radios that can cover 1880–1900 MHz band and
can provide sample rate at least two times more than DECT data rate (1152000bps). 
But some adaptation may be necessary. 

As a DECT link source the Motorola MBP12 baby monitor was used.

Because of the high DECT data rate a computer on with to run the project should 
be powerful enough. 

 
To build
---------

git clone git://github.com/pavelyazev/gr-dect2.git
cd gr-dect2/
mkdir build
cd build
cmake ../
make
sudo make install
sudo ldconfig

Then Gnuradio companion should be used to open and run the flow graph 
dect2.grc from gr-dect/grc

 
Usage
------

Each device that can emit DECT signal will be called a part. According to the 
DECT specification there are parts of two types – RFP (Radio Fixed Part) or 
base station and PP (Portable Part) or handset. RFP emits a signal that 
setups frame structure on the air. A RFP can be listened to independently.  
But in order to get a voice from a PP it is necessary to receive its pair RFP.

The project uses QT-based controls. There are RX gain slider, channels and 
receiver ID drop-down lists, status console.

The status console shows parts on the air. Information about a part consists
of a receiver ID, part’s ID in DECT system, part’s type and voice presence sign. 
The status is updated every time when a part is gained/lost or voice data is 
gained/lost. A pair of RFP and PP will have the same DECT ID.

The receiver ID is an internally assigned number inside receiver. The current 
implementation allows to listen to only one part. A necessary part is selected 
by ID from the drop-down list. The selected part will be marked by asterisk in 
console. If voice data is available a status line will have the “v” letter at 
the end and decoded voice will be routed to a sound card.

From time to time parts may change frequency channel. So to catch something 
a periodic manual scan over channels is necessary.


GNURADIO INSTALL 16.04LTS
-------------------------

Sudo apt-get update
sudo apt-get install cmake gnuradio gr-osmosdr swig git -y
volk-profile


