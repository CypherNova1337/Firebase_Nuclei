# Nuclei Firebase Discovery Template with httpx Validation
**The Most Comprehensive Firebase Scan For Nuclei**
  
This repository contains a Nuclei template `firebase-discovery.yaml` designed to detect various Firebase services and configurations in web applications. It also provides instructions on using `httpx` to validate the results.

## Template Description

The `firebase-discovery.yaml` template scans target URLs for common Firebase Realtime Database, Firestore, Storage, Hosting, Cloud Functions, and other related endpoints. It uses Nuclei's matching capabilities to identify potential Firebase services.

## Features

* **Comprehensive Firebase Detection:** Detects a wide range of Firebase services.
* **Nuclei Template:** Provides a Nuclei template for automated scanning.
* **httpx Validation:** Includes instructions on using `httpx` to verify status codes and content lengths.
* **Clear Output:** Designed to be easily understood and analyzed.

## Usage

1.  **Install Nuclei:** Make sure you have Nuclei installed on your system. You can find installation instructions on the [Nuclei GitHub repository](https://github.com/projectdiscovery/nuclei).

2.  **Install httpx:** Ensure you have `httpx` installed. If not, you can install it using `go`:

    ```bash
    go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
    ```

3.  **Clone the Repository:** Clone this repository to your local machine:

    ```bash
    git clone https://github.com/SKHTW/Firebase_Nuclei.git
    cd Firebase_Nuclei
    ```

4.  **Run the Nuclei Template:** Use the following command to run the Nuclei template:

    ```bash
    nuclei -u target_url -t firebase-discovery.yaml -var project=target_project
    ```

    * Replace `target_url` with the URL you want to scan (e.g., `https://thisisthedomain.com`).
    * Replace `target_project` with the project name or identifier you are using for the `{{project}}` variable (e.g., `thisisthedomain`). Sometimes the project name for a client and their firebase info can be something else, so try to figure out thier project name if it differs.

5.  **Validate with httpx:**

    * Create a file named `urls.txt` containing the URLs from the Nuclei template's `path` sections, replacing `{{project}}` with the appropriate value.
    * Example `urls.txt`:

        ```text
        https://example-default-rtdb.firebaseio.com/.json
        https://example.web.app/
        https://firebasestorage.googleapis.com/v0/b/example/
        # ... and so on
        ```

    * Run `httpx` to check status codes and content lengths:

        ```bash
        httpx -l urls.txt -status-code -content-length 2>&1 
        ```

    * Compare the `httpx` output with the Nuclei output. This will help you verify the accuracy of the template.

6.  **Analyze the Results:** Nuclei will output any detected Firebase endpoints or patterns. Analyze the results to identify potential vulnerabilities.

## Template Details

The template performs the following checks:

* **Firebase Realtime Database:**
    * `https://{{project}}-default-rtdb.firebaseio.com/.json`
    * `https://{{project}}-default-rtdb.firebaseio.com/.settings/rules.json`
    * `https://{{project}}.firebaseio.com/.json`
    * `https://{{project}}.firebaseio.com/.settings/rules.json`
    * `https://{{project}}.firebasedatabaseio.com/.lp?start=t`
    * `https://{{project}}.firebasedatabaseio.com/.ws?v=5`
* **Firebase Hosting:**
    * `https://{{project}}.web.app/`
    * `https://{{project}}.firebaseapp.com/`
* **Firebase Storage:**
    * `https://firebasestorage.googleapis.com/v0/b/{{project}}/`
    * `https://firebasestorage.googleapis.com/v0/b/{{project}}.appspot.com/`
* **Firebase Firestore:**
    * `https://firestore.googleapis.com/v1/projects/{{project}}/databases/(default)/documents/`
* **Firebase Cloud Functions:**
    * `https://{{project}}.cloudfunctions.net/`
    * `https://us-central1-{{project}}.cloudfunctions.net/`
    * `https://europe-west1-{{project}}.cloudfunctions.net/`
* **Firebase App Check:**
    * `https://appcheck.googleapis.com/v1/projects/{{project}}/`
* **Firebase Recaptcha:**
    * `https://recaptchaenterprise.googleapis.com/v1/projects/{{project}}/`
* **Firebase Identity Toolkit:**
    * `https://identitytoolkit.googleapis.com/v1/projects/{{project}}/`
* **Firebase Instance ID:**
    * `https://iid.googleapis.com/v1/projects/{{project}}/`
* **Firebase FCM:**
    * `https://fcm.googleapis.com/v1/projects/{{project}}/`
* **Firebase Dynamic Links:**
    * `https://dynamiclinks.googleapis.com/v1/shortLinks?key=`
* **Firebase Installations:**
    * `https://{{project}}.firebaseinstallations.googleapis.com/v1/projects/{{project}}/installations`

The template checks for specific keywords in the response body, regular expressions in the response body, and HTTP status codes to identify Firebase services.

## Important Notes

* This template is designed for detection purposes. Further analysis is required to identify actual vulnerabilities.
* Be aware that this template might produce false positives.
* Always obtain proper authorization before scanning any target.
* Use this tool responsibly and ethically.
* Combine this tool with JavaScript analysis and manual testing.

## Contributing

Feel free to contribute to this repository by submitting pull requests or opening issues.

## License

This project is licensed under the MIT License LICENSE.
