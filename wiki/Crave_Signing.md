# Signing Your builds in Crave

This script called crave_sign.sh automates the process of signing Android APK and APEX files using keys stored in a Backblaze B2 bucket and also sign your builds using the release keys from Backblaze B2 Bucket. Below are the steps to use this script effectively.

## Prerequisites

1. **Backblaze B2 CLI:** Ensure you have the Backblaze B2 CLI installed and configured on your machine.
2. **Certificates:** You should have generated keys and uploaded it in Backblaze Bucket beforehand.
3. **Environment Variables:** Set the following environment variables before running the script:
   - `BUCKET_NAME`: Name of the Backblaze B2 bucket containing the certificates.
   - `KEY_ENCRYPTION_PASSWORD`: Password used to decrypt the key password.
   - `BKEY_ID`: Backblaze B2 application key ID.
   - `BAPP_KEY`: Backblaze B2 application key.

## Setting Environment Variables

Export the required environment variables in your shell:

```sh
export BUCKET_NAME="your_bucket_name"
export KEY_ENCRYPTION_PASSWORD="your_key_encryption_password"
export BKEY_ID="your_bkey_id"
export BAPP_KEY="your_bapp_key"
```

## Running the Script

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

## Cleanup

After the script execution, it automatically cleans up by removing temporary files and unsetting environment variables to ensure no sensitive information is left in the environment.

## Completion

Upon successful execution, the script outputs a signed OTA update package and confirms the completion of the signing process.
