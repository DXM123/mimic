
#Mimic, the Mycroft TTS Engine
==========

Mimic is a lightweight run-time speech synthesis engine, based on
Flite (Festival-Lite). The Flite project website can be found
here: http://www.festvox.org/flite/ - further information can be found
in the ACKNOWLEDGEMENTS file in the Mimic repo.

###Requirements:
==========

- A good C compiler, some of these files are quite large and some C
  compilers might choke on these, gcc is fine.  Sun CC 3.01 has been
  tested too.  Visual C++ 6.0 is known to fail on the large diphone
  database files.  We recommend you use GCC under Cygwin or mingw32
  instead.
- GNU Make
- An audio device isn't required as flite can write its output to
  a waveform file.

Supported platforms:

We have successfully compiled and run on

- Linux, with both ARM and Intel architectures under it
- Mac OS X
- Android

###Compilation
==========

In general

    TODO: update this to reflect compilation

###Usage:
==========

    TODO: Shorten and update this to reflect current process,
    update relevant filenames.

The ./bin/flite voices contains all supported voices and you may
choose between the voices with the -voice flag and list the supported
voices with the -lv flag.  Note the kal (diphone) voice is a different
technology from the others and is much less computationally expensive
but more robotic.  For each voice additional binaries that contain
only that voice are created in ./bin/flite_FULLVOICENAME,
e.g. ./bin/flite_cmu_us_awb.

If it compiles properly a binary will be put in bin/, note by
default -g is on so it will be bigger than is actually required

   ./bin/flite "Flite is a small fast run-time synthesis engine" flite.wav

Will produce an 8KHz riff headered waveform file (riff is Microsoft's
wave format often called .WAV).

   ./bin/flite doc/alice

Will play the text file doc/alice.  If the first argument contains
a space it is treated as text otherwise it is treated as a filename.
If a second argument is given a waveform file is written to it,
if no argument is given or "play" is given it will attempt to
write directly to the audio device (if supported).  if "none"
is given the audio is simply thrown away (used for benchmarking).
Explicit options are also available.

   ./bin/flite -v doc/alice none

Will synthesize the file without playing the audio and give a summary
of the speed.

   ./bin/flite doc/alice alice.wav

will synthesize the whole of alice into a single file (previous
versions would only give the last utterance in the file, but
that is fixed now).

An additional set of feature setting options are available, these are
*debug* options, Voices are represented as sets of feature values (see
lang/cmu_us_kal/cmu_us_kal.c) and you can override values on the
command line.  This can stop flite from working if malicious values
are set and therefor this facility is not intended to be made
available for standard users.  But these are useful for
debugging.  Some typical examples are

./bin/flite --sets join_type=simple_join doc/intro.txt
     Use simple concatenation of diphones without prosodic modification
./bin/flite -pw doc/alice
     Print sentences as they are said
./bin/flite --setf duration_stretch=1.5 doc/alice
     Make it speak slower
./bin/flite --setf int_f0_target_mean=145 doc/alice
     Make it speak higher

The talking clock is an example talking clode as discussed on
http://festvox.org/ldom it requires a single argument HH:MM
under Unix you can call it
    ./bin/flite_time `date +%H:%M`

./bin/flite -lv
    List the voices linked in directly in this build

./bin/flite -voice rms -f doc/alice
    Speak with the US male rms voice
./bin/flite -voice awb -f doc/alice
    Speak with the "Scottish" male awb voice
./bin/flite -voice slt -f doc/alice
    Speak with the US female slt voice

./bin/flite -voice http://www.festvox.org/flite/packed/flite-2.0/voices/cmu_us_ksp.flitevox -f doc/alice
    Speak with KSP voice, download on the fly from festvox.org
./bin/flite -voice voices/cmu_us_ahw.flitevox -f doc/alice
    Speak with AHW voice loaded from the local file.

Voice names are identified as loadable files if the name includes a
"/" (slash) otherwise they are treated as internal names.  So if you
want to load voices from the current directory you need to prefix them
with "./".

###Voices
==========

TODO: Explain where to find voices, and how to obtain new ones.

You can find existing Flite voices here:
  http://www.festvox.org/flite/packed/flite-2.0/voices/
