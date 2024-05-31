# Signing Your builds in Crave
To begin with signing there are two parts:
1. Generating Keys using backblaze_keygen
2. Signing the build using crave_sign
   
## 1. Generating Keys using backblaze_keygen
This script is meant to be run on devspace cli running
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
   - **Encrypt Password:** The entered password is encrypted using OpenSSL and stored in the temporary directory.
   - **Generate Keys:** The script generates keys with the specified password.
   - **Install B2 CLI:** If the B2 CLI is not installed, the script installs it.
   - **Authenticate B2:** The script logs into Backblaze B2 using the provided credentials.
   - **Upload Keys:** The keys are uploaded to the specified B2 bucket.
   - **Clean Up:** The script removes the temporary directory containing the certificates and unsets environment variables.

### Detailed Steps

### 1. Encrypting the Password with OpenSSL

The script uses OpenSSL to encrypt the password used for key generation. The encryption is done with AES-256-CBC:

```sh
echo "$PASSWORD" | openssl enc -aes-256-cbc -iter 256 -salt -out "$CERT_DIR/password.enc" -pass pass:"$PASS_ENCRYPT"
```

- **`openssl enc -aes-256-cbc -iter 256 -salt`**: Specifies the encryption algorithm (AES-256-CBC), iteration count (256), and salt.
- **`-out "$CERT_DIR/password.enc"`**: Output encrypted password to a file.
- **`-pass pass:"$PASS_ENCRYPT"`**: Use the provided encryption password to encrypt the password.

### 2. Generating Keys

Keys for various certificates and APEX packages are generated using a specified subject and the encrypted password:

```sh
for cert in bluetooth cyngn-app media networkstack platform releasekey sdk_sandbox shared testcert testkey verity; do
    ./development/tools/make_key "$CERT_DIR/$cert" "$SUBJECT" -password pass:"$PASSWORD"
done

for apex in com.android.adbd com.android.adservices com.android.adservices.api ...; do
    apex_subject="/C=US/ST=California/L=Mountain View/O=Android/OU=Android/CN=$apex/emailAddress=android@android.com"
    ./development/tools/make_key "$CERT_DIR/$apex" "$apex_subject" -password pass:"$PASSWORD"
    openssl pkcs8 -in "$CERT_DIR/$apex.pk8" -inform DER -out "$CERT_DIR/$apex.pem" -passin pass:"$PASSWORD" -passout pass:"$PASSWORD"
done
```

### 3. Authenticating and Uploading to Backblaze B2

The script authenticates with Backblaze B2 and uploads the generated and encrypted keys:

```sh
b2 account authorize "$KEY_ID" "$APPLICATION_KEY"
b2 sync --replace-newer "$CERT_DIR" "b2://$BUCKET_NAME/android-certs"
```

### 4. Cleanup

After uploading, the script cleans up the temporary files and environment variables to ensure no sensitive information is left behind:

```sh
rm -rf "$CERT_DIR"
unset BUCKET_NAME
unset BKEY_ID
unset BAPP_KEY
unset PASS_ENCRYPT
unset PASSWORD
```

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
