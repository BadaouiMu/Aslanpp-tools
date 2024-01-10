# Aslanpp-tools
## Installation using Docker Images
To install Aslanpp tools using Docker images, use the following command:

```bash
docker run -it badaouimu/aslanpp-tools
```

## Manual Installation of Aslanpp tools: 
### openjdk-7-jdk
It only works with OpenJDK 7, which is no longer supported by Oracle. To install it, you may use old images as an example: 
```bash
docker run -it enoniccloud/java6 
```
The repository is included by default, so proceed with:
```
apt update -y
apt install openjdk-7-jdk -y
export JAVA_HOME=/usr/lib/jvm/java-7-openjdk-amd64/
export PATH=$PATH:$JAVA_HOME/bin
```
### maven 3.3.3 :
I tested it, and the latest Maven version didn't work. It's better to use version 3.3.3: 
```
wget https://archive.apache.org/dist/maven/maven-3/3.3.3/binaries/apache-maven-3.3.3-bin.zip
unzip apache-maven-3.3.3-bin.zip
cd apache-maven-3.3.3
cd bin/
export PATH=/apache-maven-3.3.3/bin/:$PATH
```
### Additional things : 
`apt install ghc ocaml make git wget zip nano sudo software-properties-common`

### Installation using a Makefile : 
```
git clone https://github.com/BadaouiMu/Aslanpp-tools.git

cd Aslanpp-tools/file/
make setup
```
If you encounter installation bugs, potentially due to an outdated website, you can locate all the required packages under the "May Need" section.

## Usage : 
### using makefile :
Navigate to the file directory:
`cd /file/`
#### To convert from Aslanpp to Aslan (assuming /file/aslanpp/name_of_aslanpp.aslan++ exists): : 
`make aslan/name_of_aslanpp.aslan `
#### To use cl-atse (search for attack in Aslan): 
`make result/name_of_aslanpp.aslan.result`
#### For a better preview after the cl-atse command:
`make result/name_of_aslanpp.result`
#### exemple : by default there's /file/aslanpp/NSPK_Unsafe.aslan++ :
```
make aslan/NSPK_Unsafe.aslan
make result/NSPK_Unsafe.aslan.result
make result/NSPK_Unsafe.result
cat result/NSPK_Unsafe.result # to see result
```
### Without Makefile : 
```
alias aslanpp-connector="java -jar /file/git/ASLanPPConnector/binaries/aslanpp-connector.jar"
aslanpp-connector -h
alias cl-atse="/file/git/cl-atse/cl-atse_bytecode-OCaml"
cl-atse -h
```
