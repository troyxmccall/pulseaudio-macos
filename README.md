# MacOS Pulse Audio Sync Setup (mainaly for docker audio, but other things too)

install
```bash
brew install pulseaudio
```

copy default configs to your user dir 
```bash
mdir ~/.pulse
cp -R $(brew --prefix)/opt/pulseaudio/etc/pulse/ ~/.pulse/
```

allow TCP connections from local networks (docker/vmware/localhost/other trusted local networks) 

```bash
echo -en  "\n### Allow docker to send us sound \nload-module module-native-protocol-tcp auth-ip-acl=127.0.0.0/8;10.0.0.0/8;172.16.0.0/12;192.168.0.0/16;fe80::/10" >> ~/.pulse/default.pa
```

add some HQ audio syncs

```bash
vim ~/.pulse/daemon.conf
```

```ini
# high quality audio syncs BB
default-sample-format = float32le
default-sample-rate = 48000
alternate-sample-rate = 44100
resample-method = soxr-vhq
remixing-produce-lfe = no
remixing-consume-lfe = no
high-priority = yes
nice-level = -11
realtime-scheduling = yes
daemonize = no
```

stop the service
```bash
brew services stop pulseaudio
```

check output of

```
$(brew --prefix)/opt/pulseaudio/bin/pulseaudio --exit-idle-time=300 --verbose
```

launch pulse-audio command (running `pacmd list-sources` or `pacmd list-sinks` will be truncated in bash with virtual devices, you need to fully go into the console

```
pacmd 
```

& set your syncs/sources


