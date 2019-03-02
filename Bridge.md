# Bridge

Rack is a standalone DAW-like application and not a VST/AU plugin because of the major limitations of these formats.
It is common to think of physical modular synthesizers as entire self-contained DAWs, so many people use Rack as a complete DAW to compose music and build patches without other software.

However, *VCV Bridge* allows audio, MIDI, DAW transport, and DAW clocks to be transferred between Rack and your DAW through the included VST/AU instrument/effect Bridge plugins.

The setup order between Rack and your DAW does not matter.

## Setting up Bridge in Rack

- Add an Audio or MIDI module to Rack from the [Core](Core.html) plugin, and select "Bridge" from the driver dropdown list.
- Open the device menu to select the Bridge port.

Up to 8 channels of audio entering the Bridge effect plugin are routed to the INPUT section of the Audio module in Rack and then back to the effect plugin.

The 16 automation parameters in the VST/AU Bridge plugin simply generate MIDI-CC messages 0-15, so you can use a [Core MIDI-CC](Core.html#midi-cc) interface to convert them to 0-10 V signals in Rack.

## Setting up Bridge in your DAW

- Make sure the VST or AU Bridge plugin is installed, and launch your DAW. See the [installation instructions](https://vcvrack.com/manual/Installing.html#installing-rack) for more information about installing the VST on your platform.
- Add the "VCV Bridge" instrument or "VCV Bridge fx" effect plugin to a track.
	- The instrument plugin is easier for sending MIDI to Rack, although it also supports audio input if supported by your DAW.
	- The effect plugin is easier for sending audio to Rack, although it also supports MIDI input if supported by your DAW.
- Open the plugin parameters to reveal the Bridge port setting and 16 automation parameters.

### Ableton Live

Add a "VCV-Bridge" plugin to a MIDI track and open the automation parameters by clicking the triangle icon next to the plugin's name.
*Bridge* will send MIDI and receive audio from Rack.

To send audio to Rack, select the Bridge's track under the "Audio To" menu on another track, and optionally select the channel pair (1/2, 3/4, 5/6, or 6/7).

To record audio from Rack, create a new audio track and select the Bridge's track and optionally the channel pair under the "Audio From" menu.
Make sure "Monitor" is set to "In" on the Bridge's track to enable audio output even when it is not record-enabled.

![Ableton Live VCV Bridge](images/BridgeLive.png)

### Cubase
In Cubase add a Track/Rack instrument in the VST Panel. If you make a rack instrument, select "Yes" when it asks if you want to create a MIDI channel for the rack instrument. In the track inspector, select the midi input of your choice (your controller keyboard for example. In the screenshot below, I am using "2-Impulse" for MIDI input). Next, select VCV-Bridge-64 for the track's VST Instrument output, and pick channel 1 to send MIDI data on. In the timeline, I added a single measure sending four notes, set the transport to loop, and pressed play. 

![Cubase 9x VCV Bridge](images/CubaseVCVRackBridge1.png)

 Switching to VCV Rack, create a MIDI module. Set the MIDI Driver to "Bridge." Set the Port to 1. Set the channel to 1. Send the CV and Gate to an OSC-type module's trigger/gate and V/Oct input, respectively. Set the OSC's output(s) to a Core Audio module. In the Core Audio module, set the Audio Driver to "Bridge." Set the port number to 1. You should now hear your MIDI loop playing your patch in VCV Rack through the defined output channel in Cubase.

![Cubase 9x Example Patch Settings in VCV Rack](images/CubaseVCVRackBridge2.png)

### FL Studio
In FL Studio Add a "VCV-Bridge" Channel. Inside Channel settings, navigate to Processing tab and check "Make bridged" and "Use fixed size buffers" (this is to be fixed sooner or later). For each output in VCV you will need separate Channel listening to separate port, as there's no option to handle many Wrapper inputs in FL Studio. 
VCV needs to be started *after* adding initial bridge in FL Studio, otherwise you won't hear any sound nor send any notes.
To send notes - in VCV - add MIDI-1 mapped to respective Port inside Channel in FL Studio.
To receive sound - in VCV - add SOUND, mapped to respective Port, Inputs 1-2 will send stereo sound to Wrapper in FL Studio.

Rendering notice: It was noticed, that it is impossible to render with ASIO (Presonus AudioBox ASIO) enabled, as it renders with tons of overloads and spikes. It is advised to try rendering with default audio driver with maximal possible latency.

### Propellerhead Reason
TODO

### REAPER
Right-click in the REAPER Track Control Panel (underneath the main toolbar, to the left of the Arrange area). Select "Insert virtual instrument on new track...". Locate "VCV Bridge" in your VST or VSTi folders. Select the 32- or 64-bit version of VCV Bridge per your own preferences and REAPER flavor. (If desired, you can also insert the VCV Bridge into an existing track by clicking the FX button for the track.)

When REAPER brings up the "Build Routing Confirmation" dialog, click "No" if you just want the bridge to make a typical set of stereo outputs available to REAPER, and "Yes" if you want to create eight discrete mono audio channels. REAPER will create eight additional audio tracks labeled "Output 1-8" if you select "Yes" at this dialog. Otherwise, it will only create a single track for MIDI and audio. 

(You can also manually create additional tracks / channels, and route the audio from the track containing the bridge insert into these additional tracks. This enables simultaneous live capture of VCV inbound MIDI and outbound audio on separate REAPER tracks.)

The bridge insert defaults to port 1 in REAPER. Make sure your MIDI and audio modules in Rack are all set to communicate with Bridge, and that the port settings in those Rack modules also match the intended port number from your REAPER bridge track or channel. 

You should now be able to play Rack via your MIDI controller from the armed bridge channel in REAPER. REAPER auto-maps and -arms the track for MIDI input, and also enables record monitoring, when the track is created with "Insert virtual instrument on new track." If you added VCV Bridge to your REAPER project another way, you may need to arm the track and turn record monitoring on. 

To record audio from Rack, right-click on the record arm button for the track that you want to capture Rack's output audio from the bridge, and select "Record: output" --> "Record: (whatever_audio_format_is_desired)". 

*Note:* If REAPER is already open with a VCV bridge instance created, and VCV is opened second with a patch that already contains MIDI and/or audio modules set to anything other than Bridge send/receive, VCV Rack may crash on startup or patch load, even if the VCV Bridge insert is currently bypassed in REAPER. You may need to close REAPER and specifically select Bridge mode in your Rack audio / MIDI setup first before attempting to bridge. 
