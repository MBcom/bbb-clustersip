This fork contains some additional information about configuring and can be used to setup an SIP Gateway for Scalelite BBB Clusters.
For use with Greenlight, we recommend our https://github.com/MBcom/enforce-authentication-greenlight fork, with integrated static SIP support, by setting `VOICE_BRIDGE_PHONE_NUMBER` environment variable.
  
We tested this setup on Ubuntu 20.04 Server.

# Setup

* see folder astregbackend and ast_config for information on how to setup the SIP-Gateway.
* see folder fsconfregger for information on how to setup a BBB-node.

# Problem

With BigBlueButton clusters (with Scalelite & Greenlight) one never knows on which Server the session will be processed.
This makes is quite difficult to route SIP-calls to the correct FreeSWITCH instance.

# Disclaimer

Please understand this project as a proof-of-concept. As there is no support for static voiceBridges within Greenlight the voiceBridge is not predictable at the moment. See security at the end of the readme as well.

# Approach

When a conference is created on a BBB-node, FreeSWITCH is instructed to send out a SIP REGISTER to a central SIP-Gateway for the cluster.
The registration contains the conference number (which is equivalent to the voiceBridge of the room).

The SIP-Gateway run very little piece of dialplan to let the caller enter the desired conference number.
If the conference is already registered the caller gets routed to the corresponding FreeSWITCH.
If the conference is not registered the caller gets waited until the conference registers.

The SIP-Gateway gets connected to a SIP-Provider or to a PBX to be reachable from the PSTN.
The assigned phone number should be used for all your BBB-nodes as dialNumber.

![alt text][setup]



# Security

At the moment the is no authentication for the registrations.
So make sure to setup the ACL in pjsip.conf properly, so it matches only your BBB-nodes and the Upstream.
Consider setting up a packet filter the SIP-Gateway as well.

[setup]: setup.png "Example setup"
