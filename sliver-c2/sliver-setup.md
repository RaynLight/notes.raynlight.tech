# Sliver Setup

Building your own sliver-server is essential for bypassing windows defender because the default settings for the donut shell code generator are to bypass AMSI. My solution is to compile the sliver server manually, but generating an executable with sliver then loading it with donut and disabling AMSI bypassing is also a good solution. Please follow the steps below to get a good sliver-server that doesn't compile shell code with an AMSI bypass.&#x20;

1. Clone the Sliver repository&#x20;

```bash
git clone https://github.com/BishopFox/sliver.git
```

2. Change the following file

[https://github.com/BishopFox/sliver/blob/master/server/generate/donut.go](https://github.com/BishopFox/sliver/blob/master/server/generate/donut.go)

```go
Bypass:     3,         // Change the 3 to a 1
config.Bypass = 3      // Change the 3 to a 1
```

3. Make the file

```bash
sudo apt install golang-go -y     # Installing dependencies
sudo apt install git mingw-w64 -y # Installing dependencies
cd ~/sliver
make
```

