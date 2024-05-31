# Signing Your builds in Crave
To begin with signing there are two parts:
1. Generating Keys using backblaze_keygen
2. Signing the build using crave_sign
   
## 1. Generating Keys using backblaze_keygen

**Attention to Readers :**
**This script is meant to be run on Devspace CLI not on while building rom**
**Neither me [sppidy](https://github.com/sppidy) who made the script nor Crave team members are reposnsible if you run this script during build and leak your sensitive infos**

This script automates the process of generating, encrypting, and uploading Android certificates to Backblaze B2. It performs the following steps:
1. Prompts for passwords.
2. Generates and encrypts keys.
3. Authenticates with Backblaze B2.
4. Uploads the keys to Backblaze B2.
5. Cleans up temporary files and environment variables.

### Prerequisites

1. **Backblaze B2 CLI:** Ensure you have the Backblaze B2 CLI installed. If not, the script will attempt to install it using `pip`.
2. **Environment Variables:** Set the following environment variables before running the script:
   - `BUCKET_NAME`: Name of the Backblaze B2 bucket to store the certificates.
   - `BKEY_ID`: Backblaze B2 application key ID.
   - `BAPP_KEY`: Backblaze B2 application key.

### Setting Environment Variables

Export the required environment variables in your shell:

```sh
export BUCKET_NAME="your_bucket_name"
export BKEY_ID="your_bkey_id"
export BAPP_KEY="your_bapp_key"
```

### Running the Script

1. **Execute the Script:**

   Run the script using the following command:

   ```sh
   backblaze_keygen
   ```

2. **Password Prompts:**
   - **Password:** Enter the password to be used for key generation and encryption.
   - **Encryption Password:** Enter the password to encrypt the password file.

3. **Script Workflow:**
   - **Create Certificate Directory:** A temporary directory is created for storing certificates.
   - **Password for the certicates:** You will be promoted to enter the password which will be used for certificates.
   - **Encrypt Password:** The entered password is encrypted using OpenSSL and stored in the temporary directory.
   - **Generate Keys:** The script generates keys with the specified password.
   - **Install B2 CLI:** If the B2 CLI is not installed, the script installs it.
   - **Authenticate B2:** The script logs into Backblaze B2 using the provided credentials.
   - **Upload Keys:** The keys are uploaded to the specified B2 bucket.
   - **Clean Up:** The script removes the temporary directory containing the certificates and unsets environment variables.

### Completion

Upon successful execution, the script outputs a confirmation message and ensures that all temporary files and sensitive environment variables are cleared.

### Example Output

```sh
Enter the password:
Enter the Encryption Password:
B2 SDK Logging in...
Uploading keys to Backblaze B2...
Keys have been generated, password protected, and uploaded to Backblaze B2.
Clearing keys from devspace
Cleared Certificates from Devspace
Clearing ENV Variables
Cleared ENV Variables
```

This output indicates that the keys have been successfully generated, encrypted, uploaded, and that the temporary files and environment variables have been cleaned up.

## 2. Signing the build using crave_sign
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

Export the required environment variables in your shell:

```sh
export BUCKET_NAME="your_bucket_name"
export KEY_ENCRYPTION_PASSWORD="your_key_encryption_password"
export BKEY_ID="your_bkey_id"
export BAPP_KEY="your_bapp_key"
```

### Running the Script

1. **Execute the Script:**

   Run the script using the following command:

   ```sh
   crave_sign
   ```

2. **Script Workflow:**
   - **Authorize Backblaze B2:** The script logs into Backblaze B2.
   - **Retrieve Keys:** Synchronizes the keys from the specified B2 bucket to a temporary directory.
   - **Decrypt Key Password:** Decrypts the key password using the provided encryption password.
   - **Sign APK and APEX Files:** Signs the APK and APEX files using the retrieved and decrypted keys.
   - **Generate OTA Update Package:** Creates an OTA update package from the signed files.
   - **Clean Up:** Removes the temporary directory containing the certificates and unsets environment variables.

### Cleanup

After the script execution, it automatically cleans up by removing temporary files and unsetting environment variables to ensure no sensitive information is left in the environment.

### Completion

Upon successful execution, the script outputs a signed OTA update package and confirms the completion of the signing process.
