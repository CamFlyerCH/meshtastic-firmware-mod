<div align="center" markdown="1">

<img src=".github/meshtastic_logo.png" alt="Meshtastic Logo" width="80"/>
<h1>Modified Meshtastic Firmware</h1>

</div>

</div>

<div align="center">
	<a href="https://meshtastic.org">Website</a>
	-
	<a href="https://meshtastic.org/docs/">Documentation</a>
</div>

## Overview

This repository contains a fork of the official device firmware for Meshtastic. 

These modifactions are built in in this firmware version:
- Messages with sent from a node configured with max hops > 3 are discarded. These packets are not forwarded to other nodes or to the phone. (Rebroadcast mode must be configured to LOCAL_ONLY to enable this feature, also it is only applyed on the default channel)
- Messages like Telemetry are always brodcasted with 0 hops left, so they are not rebroadcasted any further. This allows to use a max hops setting of 1-3 but still not flooding the mesh with telemetry packets probably only relevant to you.

All changes are only in this file: https://github.com/CamFlyerCH/meshtastic-firmware-mod/blob/master/src/mesh/Router.cpp   (Search for // JM mod start comments)

