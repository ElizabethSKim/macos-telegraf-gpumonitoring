

# macos-telegraf-gpumonitoring

Sharing a tool to get GPU information on intel mac (AMD GPU) and on Apple silicon (more limited)

This tool is used with telegraf to populate an Influxdb bucket to display stats in grafana;

You like it? You use it? It saved you a couple of hours? 
Hey 👋👋👋 [You can now buy me a coffee](https://www.buymeacoffee.com/xrrxrr)! ☕️ 


## Installation

* Copy **gpuPerformanceStatistics** to */usr/local/bin*
* Update your **telegraf.conf** accordingly (see example bellow)
* Restart telegraf 
* Update and use provided grafana dashboard json as template
* Enjoy

GPU performance statistics are exported using Apple IOKitframework as a JSON. 
Telegraf is configured to invoke **gpuPerformanceStatistics** (command line app) and parse the JSON output to ingest all available fields.

Here's an example of **telegraf.conf** 

```
[[inputs.execd]]
  #On intel and apple silicon macs you can use the classname IOAccelerator 
  #Or be more specific with something like AMDRadeonX6000_AMDNavi23GraphicsAccelerator if you want to target a specific gpu...
  #(ioreg -l is your friend to find out)

  command = ["/usr/local/bin/gpuPerformanceStatistics", "IOAccelerator"]
  name_override = "macosgpu"
  signal = "none"
  restart_delay = "10s"
  data_format="json"

[[outputs.influxdb_v2]]
  urls = ["http://yourinfluxdbserver:8086"]
  ## Token for authentication.
  token = "yourtoken"
  ## Organization is the name of the organization you wish to write to; must exist.
  organization = "yourorganisation""

  ## Destination bucket to write into.
  ## Change it but update the dashboard accordingly (hint: find/replace into the grafana dashboard  json before importing it)
  bucket = "XRRMonitoring"
```

## main AMD GPU metrics (and more)

* Core Clock (*MHz*)
* Memory Clock (*MHz*)
* Total Power (*W*)
* Device Utilization (*%*)
* Temperature (*C*)
* Fan Speed (*%*)
* Fan Speed (*RPM*)
* GPU Activity (*%*)

## Apple Silicon metrics (that's all)

* Device Utilization
* Renderer Utilization 
* Tiler Utilization
* Alloc system memory
* In use system memory

![intel mac / AMD GPU Screenshot](https://github.com/XReyRobert/macos-telegraf-gpumonitoring/raw/main/screenshots/screenshot_intelAmd.png?raw=true)

![Apple silicon Screenshot](https://github.com/XReyRobert/macos-telegraf-gpumonitoring/raw/main/screenshots/screenshot_applesilicon.png?raw=true)

You like it? You use it? It saved you a couple of hours? 
Hey 👋👋👋 [You can now buy me a coffee](https://www.buymeacoffee.com/xrrxrr)! ☕️ 

