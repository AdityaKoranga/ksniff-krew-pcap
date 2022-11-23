# ksniff-krew-pcap
## Making pcap file of a sniffed pod in k8s

## Install Krew so that you can install knsiff plugin

Run the following command

```bash
(
  set -x; cd "$(mktemp -d)" &&
  OS="$(uname | tr '[:upper:]' '[:lower:]')" &&
  ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" &&
  KREW="krew-${OS}_${ARCH}" &&
  curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" &&
  tar zxvf "${KREW}.tar.gz" &&
  ./"${KREW}" install krew
)
```

Now add the following line at the end of ~/.bashrc file(use vim or any other text editor
```bash
export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"
```

Now restart your shell
```bash
source ~/.bashrc
```
To check whether krew is installed or not:
```bash
kubectl krew
```

## Install ksniff

```bash
kubectl krew install sniff
```
It will take few minutes.

## How to run sniff command and make pcap file
1. Firstly run a k8s cluster
2. Check the pods 
3. Run the following command
```bash
kubectl sniff <pod-name> -n <namespace> -o <output_file_name>.pcap
```
Example in my case it is:
```bash
kubectl sniff amf-78d9c79889-jnzx5 -n sdcore-5g -o sdcore_amf.pcap
```

## Now to get the pcap file in your local machine

Open a new terminal in your local system

Run the command
```bash
scp <vm-name>@<ip_of_the_vm>:<path_to_the_file_in_the_vm> <path_to_save_the_file_in_local_machine>
```
For example in my case

```bash
scp ubuntu@192.168.5.185:~/sdcore_amf.pcap ~/
```
