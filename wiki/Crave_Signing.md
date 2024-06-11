# Signing Your builds On Crave
This page is a work in progress, and so, the instructions and scripts may not work as intended. Please keep this in mind while you try it out. 

This guide also assumes you are familiar with crave and the procedure building and signing android, and debugging issues that arise. 

Let us know in the "Signed Builds" thread under ext-foss-aosp in [the discord server](https://discord.crave.io)

To begin with signing there are two parts:
1. Generating Keys using backblaze_keygen
2. Signing the build using crave_sign
   
## Generating Keys using backblaze_keygen

**Attention to Readers :**
**This script is meant to be run on Devspace CLI not while building the ROM**
**Neither [sppidy](https://github.com/sppidy), who made the script nor Crave team members are responsible if you run this script during build and leak your sensitive info**

This script automates the process of generating, encrypting, and uploading Android certificates to Backblaze B2. It performs the following steps:
1. Prompts for passwords.
2. Generates and encrypts keys.
3. Prompts for B2 Key ID, B2 Application Key and B2 Bucket Name.
4. Authenticates with Backblaze B2.
5. Uploads the keys to Backblaze B2.
6. Cleans up temporary files and environment variables.

### Prerequisites

1. **Backblaze B2 CLI:** Ensure you have the Backblaze B2 CLI installed. If not, the script will attempt to install it using `pip`.

### Running the Script

1. **Execute the Script:**

   Enter your Project folder. If you don't have one already, make one using [crave clone create](Crave_Devspace.md#setting-up-the-project).

   `cd Lineage21`

   Run the script using the following command:

   ```sh
   /opt/crave/backblaze_keygen.sh
   ```

   Source code to the script can be found [here](https://github.com/accupara/docker-images/blob/master/aosp/common/backblaze_keygen.sh)

1. **Password Prompts:**
   - **Password:** Enter the password to be used for key generation and encryption.
   - **Encryption Password:** Enter the password to encrypt the password file.

2. **Script Workflow:**
   - **Create Certificate Directory:** A temporary directory is created for storing certificates.
   - **Password for the certicates:** You will be promoted to enter the password which will be used for certificates.
   - **Encrypt Password:** The entered password is encrypted using OpenSSL and stored in the temporary directory.
   - **Generate Keys:** The script generates keys with the specified password.
   - **Install B2 CLI:** If the B2 CLI is not installed, the script installs it.
   - **Credentials For B2:** You will be prompted for B2 Credentials.
   - **Authenticate B2:** The script logs into Backblaze B2 using the provided credentials.
   - **Upload Keys:** The keys are uploaded to the specified B2 bucket.
   - **Clean Up:** The script removes the temporary directory containing the certificates and unsets environment variables.

### Completion

Upon successful execution, the script outputs a confirmation message and ensures that all temporary files and sensitive environment variables are cleared.

### Example Output

```sh
Enter the password: ********
Enter the Encryption Password: ********

Creating certificate directory: /tmp/android-certs.XYZ123

Encrypting password and storing it at: /tmp/android-certs.XYZ123/password.enc

Generating keys:
Generating key for bluetooth
Generating key for cyngn-app
Generating key for media
Generating key for networkstack
Generating key for platform
Generating key for releasekey
Generating key for sdk_sandbox
Generating key for shared
Generating key for testcert
Generating key for testkey
Generating key for verity

Generating APEX keys:
Generating key for com.android.adbd
Generating key for com.android.adservices
Generating key for com.android.adservices.api
Generating key for com.android.appsearch
Generating key for com.android.art
Generating key for com.android.bluetooth
Generating key for com.android.btservices
Generating key for com.android.cellbroadcast
Generating key for com.android.compos
Generating key for com.android.configinfrastructure
Generating key for com.android.connectivity.resources
Generating key for com.android.conscrypt
Generating key for com.android.devicelock
Generating key for com.android.extservices
Generating key for com.android.graphics.pdf
Generating key for com.android.hardware.biometrics.face.virtual
Generating key for com.android.hardware.biometrics.fingerprint.virtual
Generating key for com.android.hardware.boot
Generating key for com.android.hardware.cas
Generating key for com.android.hardware.wifi
Generating key for com.android.healthfitness
Generating key for com.android.hotspot2.osulogin
Generating key for com.android.i18n
Generating key for com.android.ipsec
Generating key for com.android.media
Generating key for com.android.media.swcodec
Generating key for com.android.mediaprovider
Generating key for com.android.nearby.halfsheet
Generating key for com.android.networkstack.tethering
Generating key for com.android.neuralnetworks
Generating key for com.android.ondevicepersonalization
Generating key for com.android.os.statsd
Generating key for com.android.permission
Generating key for com.android.resolv
Generating key for com.android.rkpd
Generating key for com.android.runtime
Generating key for com.android.safetycenter.resources
Generating key for com.android.scheduling
Generating key for com.android.sdkext
Generating key for com.android.support.apexer
Generating key for com.android.telephony
Generating key for com.android.telephonymodules
Generating key for com.android.tethering
Generating key for com.android.tzdata
Generating key for com.android.uwb
Generating key for com.android.uwb.resources
Generating key for com.android.virt
Generating key for com.android.vndk.current
Generating key for com.android.vndk.current.on_vendor
Generating key for com.android.wifi
Generating key for com.android.wifi.dialog
Generating key for com.android.wifi.resources
Generating key for com.google.pixel.camera.hal
Generating key for com.google.pixel.vibrator.hal
Generating key for com.qorvo.uwb

B2 CLI not found, installing...
Requirement already up-to-date: b2 in /usr/local/lib/python3.7/site-packages

Enter Bucket Name: my-bucket
Enter B2 Key Id: my-b2-key-id
Enter B2 App Key: my-b2-app-key

Authorizing B2...
B2 authorization successful

Uploading keys to Backblaze B2...
Upload complete

Keys have been generated, password protected, and uploaded to Backblaze B2.
Cleaning up...
Cleared Certificates from Devspace
Clearing ENV Variables
Cleared ENV Variables
```

This output indicates that the keys have been successfully generated, encrypted, uploaded, and that the temporary files and environment variables have been cleaned up.

## Signing the build using crave_sign
This script called crave_sign.sh automates the process of signing Android APK and APEX files using keys stored in a Backblaze B2 bucket and also sign your builds using the release keys from Backblaze B2 Bucket. Below are the steps to use this script effectively.

### Prerequisites

1. **Backblaze B2 CLI:** Ensure you have the Backblaze B2 CLI installed and configured on your machine.
2. **Certificates:** You should have generated keys and uploaded it in Backblaze Bucket beforehand.
3. **Environment Variables:** Set the following environment variables before running the script:
   - `BUCKET_NAME`: Name of the Backblaze B2 bucket containing the certificates.
   - `KEY_ENCRYPTION_PASSWORD`: Password used to decrypt the key password.
   - `BKEY_ID`: Backblaze B2 application key ID.
   - `BAPP_KEY`: Backblaze B2 application key.
**Note :** These variables can also directly be specified in yaml file.

### Setting Environment Variables

Create a crave.yaml inside .repo/manifests with the following contents:

```
LOS 21:
  ignoreClientHostname: true
  env:
    BUCKET_NAME: your_bucket_name
    KEY_ENCRYPTION_PASSWORD: your_key_encryption_password
    BKEY_ID: your_bkey_id
    BAPP_KEY: your_bapp_key
```

Replace "LOS 21" with your base project's name. Remember to use the correct name, get it from `crave clone list`. 

Also remember to replace the placeholder credentials with actual values.

It is also recommended to set ignoreClientHostname to preserve workflow persistence. Read more about it [here](Crave_Devspace#workspace-persistence).

If you're using sounddrill's crave_aosp_builder github actions workflow, you can set crave.yaml through secrets. Steps:

- Go to (repo) Settings -> Security -> Secrets and Variables -> Actions

- Set repository secret called CUSTOM_YAML

- Enter the contents of your crave.yaml

### Running the Script

1. **Execute the Script:**


   Run the script using the following command inside crave run:

   ```sh
   /opt/crave/crave_sign.sh
   ```

   Example:
   ```
   crave run  --no-patch -- "rm -rf .repo/local_manifests; \
   git clone https://github.com/sounddrill31/reponame --depth 1 -b branchname .repo/local_manifests; \
   /opt/crave/resync.sh; \
 
   source build/envsetup.sh; \
   
   # Sign Script instead of mka bacon
   breakfast oxygen userdebug; \
   mka target-files-package otatools; \
   /opt/crave/crave_sign.sh"
   ```

2. **Script Workflow:**
   - **Authorize Backblaze B2:** The script logs into Backblaze B2.
   - **Retrieve Keys:** Synchronizes the keys from the specified B2 bucket to a temporary directory.
   - **Decrypt Key Password:** Decrypts the key password using the provided encryption password.
   - **Sign APK and APEX Files:** Signs the APK and APEX files using the retrieved and decrypted keys.
   - **Generate OTA Update Package:** Creates an OTA update package from the signed files.
   - **Clean Up:** Removes the temporary directory containing the certificates and unsets environment variables.

   Source code to the script can be found [here](https://github.com/accupara/docker-images/blob/master/aosp/common/crave_sign.sh)

### Cleanup

After the script execution, it automatically cleans up by removing temporary files and unsetting environment variables to ensure no sensitive information is left in the environment.

### Completion

Upon successful execution, the script outputs a signed OTA update package and confirms the completion of the signing process.
