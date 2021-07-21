# Generate the firmware update file and signature<a name="connect-iot-lorawan-script-fwupdate-sigkey"></a>

The steps in this procedure are optional and depend on the gateway you're using\. Gateway manufacturers provide their own firmware update in the form of an update file or a script and Basics Station runs this script in the background\. In this case, you'll most likely find the firmware update file in the release notes of the gateway you're using\. You can then use that update file or script instead and proceed to [Upload the firmware file to an S3 bucket and add an IAM role](connect-iot-lorawan-upload-firmware-s3bucket.md)\.

If you don't have this script, following shows the commands to run for generating the firmware update file\. The updates can also be signed to ensure that the code was not altered or corrupted and devices run code published only by trusted authors\.

**Topics**
+ [Generate the firmware update file](#connect-iot-lorawan-firmware-update-script)
+ [Generate signature for the firmware update](#connect-iot-lorawan-generate-signature-fwupdate)
+ [Review the next steps](#connect-iot-lorawan-fwupdate-sigkey-next-steps)

## Generate the firmware update file<a name="connect-iot-lorawan-firmware-update-script"></a>

The LoRa Basics Station software running on the gateway is capable of receiving firmware updates in the CUPS response\. If you don't have a script provided by the manufacturer, refer to the following firmware update script that is written for the Raspberry Pi based RAKWireless Gateway\. We have a base script and the new station binary, version file, and `station.conf` are attached to it\.

**Note**  
The script is specific to the RAKWireless Gateway, so you'll have to adapt it to your application depending on the gateway you're using\.

**Base script**  
Following shows a sample base script for the Raspberry Pi based RAKWireless Gateway\. You can save the following commands in a file `base.sh` and then run the script in the terminal on the Raspberry Pi's web browser\.

```
*#!/bin/bash*
execution_folder=/home/pi/Documents/basicstation/examples/aws_lorawan
station_path="$execution_folder/station"
version_path="$execution_folder/version.txt"
station_conf_path="$execution_folder/station_conf"

# Function to find the Basics Station binary at the end of this script 
# and store it in the station path
function prepare_station()
{
 match=$(grep --text --line-number '^STATION:$' $0 | cut -d ':' -f 1) 
 payload_start=$((match + 1)) 
 match_end=$(grep --text --line-number '^END_STATION:$' $0 | cut -d ':' -f 1) 
 payload_end=$((match_end - 1)) 
 lines=$(($payload_end-$payload_start+1)) 
 head -n $payload_end $0 | tail -n $lines  > $station_path
}

# Function to find the version.txt at the end of this script 
# and store it in the location for version.txt
function prepare_version()
{
  match=$(grep --text --line-number '^VERSION:$' $0 | cut -d ':' -f 1) 
  payload_start=$((match + 1))        
  match_end=$(grep --text --line-number '^END_VERSION:$' $0 | cut -d ':' -f 1) 
  payload_end=$((match_end - 1)) 
  lines=$(($payload_end-$payload_start+1)) 
  head -n $payload_end $0 | tail -n $lines  > $version_path
}

# Function to find the version.txt at the end of this script 
# and store it in the location for version.txt
function prepare_station_conf()
{
 match=$(grep --text --line-number '^CONF:$' $0 | cut -d ':' -f 1) 
 payload_start=$((match + 1)) 
 match_end=$(grep --text --line-number '^END_CONF:$' $0 | cut -d ':' -f 1) 
 payload_end=$((match_end - 1)) 
 lines=$(($payload_end-$payload_start+1)) 
 head -n $payload_end $0 | tail -n $lines  > $station_conf_path
}

# Stop the currently running Basics station so that it can be overwritten
# by the new one
killall station

# Store the different files
prepare_station
prepare_versionp
prepare_station_conf

# Provide execute permission for Basics station binary
chmod +x $station_path

# Remove update.bin so that it is not read again next time Basics station starts
rm -f /tmp/update.bin

# Exit so that rest of this script which has binaries attached does not get executed
exit 0
```

**Add payload script**  
To the base script, we append the Basics Station binary, the version\.txt that identifies the version to update to, and `station.conf` in a script called `addpayload.sh`\. Then, run this script\.

```
*#!/bin/bash
*
base.sh > fwstation

# Add station
echo "STATION:" >> fwstation
cat $1 >> fwstation
echo "" >> fwstation
echo "END_STATION:" >> fwstation

# Add version.txt
echo "VERSION:" >> fwstation
cat $2 >> fwstation
echo "" >> fwstation
echo "END_VERSION:" >> fwstation

# Add station.conf
echo "CONF:" >> fwstation
cat $3 >> fwstation
echo "END_CONF:" >> fwstation

# executable
chmod +x fwstation
```

After you've run these scripts, you can run the following command in the terminal to generate the firmware update file, `fwstation`\.

```
$ ./addpayload.sh station version.txt station.conf
```

## Generate signature for the firmware update<a name="connect-iot-lorawan-generate-signature-fwupdate"></a>

The LoRa Basics Station software provides signed firmware updates with ECDSA signatures\. To support signed updates, you'll need:
+ A signature that must be generated by an ECDSA private key and less than 128 bytes\. 
+ The private key that is used for the signature and must be stored in the gateway with file name of the format `sig-%d.key`\. We recommend using the file name `sig-0.key`\.
+ A 32\-bit CRC over the private key\.

The signature and CRC will be passed to the AWS IoT Core for LoRaWAN APIs\. To generate the previous files, you can use the following script `gen.sh` that is inspired by the [ basicstation](https://github.com/lorabasics/basicstation/blob/master/examples/cups/prep.sh) example in the GitHub repository\.

```
*#!/bin/bash

*function ecdsaKey() {
    # Key not password protected for simplicity    
    openssl ecparam -name prime256v1 -genkey | openssl ec -out $1
}

# Generate ECDSA key
ecdsaKey sig-0.prime256v1.pem

# Generate public key
openssl ec -in sig-0.prime256v1.pem -pubout -out sig-0.prime256v1.pub

# Generate signature private key
openssl ec -in sig-0.prime256v1.pub -inform PEM -outform DER -pubin | tail -c 64 > sig-0.key

# Generate signature
openssl dgst -sha512 -sign sig-0.prime256v1.pem $1 > sig-0.signature

# Convert signature to base64
openssl enc -base64 -in sig-0.signature -out sig-0.signature.base64

# Print the crc
crc_res=$(crc32 sig-0.key)printf "The crc for the private key=%d\n" $((16#$crc_res))

# Remove the generated files which won't be needed later
rm -rf sig-0.prime256v1.pem sig-0.signature sig-0.prime256v1.pub
```

The private key generated by the script should be saved into the gateway\. The key file is in binary format\.

```
./gen_sig.sh fwstation 
read EC key
writing EC key
read EC key
writing EC key
read EC key
writing EC key
The crc for the private key=3434210794

$ cat sig-0.signature.base64 
MEQCIDPY/p2ssgXIPNCOgZr+NzeTLpX+WfBo5tYWbh5pQWN3AiBROen+XlIdMScv
AsfVfU/ZScJCalkVNZh4esyS8mNIgA==

$ ls sig-0.key
sig-0.key

$ scp sig-0.key pi@192.168.1.11:/home/pi/Documents/basicstation/examples/iotwireless
```

## Review the next steps<a name="connect-iot-lorawan-fwupdate-sigkey-next-steps"></a>

Now that you have generated the firmware and signature, go to the next topic to upload the firmware file, `fwstation`, to an Amazon S3 bucket\. The bucket is a container that will store the firmware update file as an object\. You can add an IAM role that will give the CUPS server permission to read the firmware update file in the S3 bucket\. 