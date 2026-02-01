# FortiGate baseline

Created and maintained by Kevin Guenay at \<https://guenay.at\>

Over the years, I have assembled my own baseline for FortiGates that I apply to basically every deployment. It contains sensible settings, some checks, best practices, and small things that not everyone knows about. I wanted it to be as generic as possible so that it can be used for every situation. The baseline grew with experience and time, and I am finally getting around to sharing it with the world.

## Points to consider

* I am not liable for any issues you experience when using any parts of my baseline.  
* The baseline will get updates, but they will come when I get to it and when I feel the changes make sense.  
* I am assuming a few things for my baseline:  
  * Internal and external traffic is handled on the same device. This is important for one point, which is highlighted.  
  * No VDOMs are used. This doesn’t mean that the baseline can’t be used for VDOM deployments, but lots of things need to be handled differently if VDOMs are used.  
  * An active internet connection is available. This isn’t necessary, but some things will not work without one.  
* Only IPv4 will be handled. No IPv6 settings are provided.  
* This is a CLI-first baseline. For some settings, both a GUI and a CLI way to configure exist. Due to the changing nature of the FortiGate GU, I will only provide the GUI way if necessary.  
* I am not infallible. If you believe the baseline can be improved, create an issue or a pull request, and I’ll look at it.  
  * The baseline should stay as generic as possible and shouldn’t interfere with existing configurations (performance impacts are highlighted, and decisions are up to the end user), so requests to change the baseline will have to keep this in mind. To give an example of what I wouldn’t accept: Deleting the SIP session helper and disabling the SIP ALG. While such a configuration is often required, it can create problems.

## Repository structure

This repository is structured as follows:

* The [**baseline.md**](./baseline.md) holds multiple sections.  
* Each section covers one topic, includes a description and the CLI commands to configure the relevant topic.  
* A [**fortigate\_baseline\_no\_input\_safe.conf**](./fortigate_baseline_no_input_safe.conf) file is available that includes all safe CLI commands that can be immediately applied to a device and should be non-disruptive. The “no\_input” refers to the fact that you don’t have to supply anything because, for some sections, you need to supply additional information, like interface names or services.  
  * **The only exception to the “safe” part is Private Data Encryption (PDE), because of how it [interacts with FortiManager](https://docs.fortinet.com/document/fortimanager/7.6.5/administration-guide/30332/managing-fortigates-with-private-data-encryption). This configuration includes PDE, because it is highly recommended. PDE will always be the very first item.**  
* A [**fortigate\_baseline\_no\_input\_safe\_create.conf**](./fortigate_baseline_no_input_safe_create.conf) is available that does everything the “no\_input” file does, but also creates objects (addresses, security profiles, threat feeds, etc.), but does not use them. All created objects have the prefix “**BASE\_”**.  
  * The activation of FortiGate Cloud Sandbox is also included in this, even though it requires input.  
  * **Note**: Security profiles will need to be further edited before being deployed.  
* A [**fortigate\_baseline.conf**](./fortigate_baseline.conf) file is available that includes **all** CLI commands and has placeholders (e.g. “\<\#\#\#PLACEHOLDER\_INTF\#\#\#\>”) for information you need to supply first.

