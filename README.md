# Nuclei Firebase Discovery Template (Enhanced)
**A Comprehensive and Precise Firebase Service Scan for Nuclei**

This repository contains an advanced Nuclei template, `firebase-discovery.yaml`, meticulously designed to detect a wide array of Firebase services and configurations associated with a target project. This updated version incorporates stricter matching logic and provides more informative output.

## Template Description

The `firebase-discovery.yaml` template intelligently scans for common Firebase services including Realtime Database (with numerous common naming variants), Firestore, Cloud Storage, Hosting, Cloud Functions, and various other Firebase/Google Cloud Platform (GCP) API endpoints linked to a Firebase project. It employs a robust matching strategy, combining negative status checks with positive DSL-based conditions (status codes and body/header content) to ensure higher accuracy and reduce false positives. Extracted HTTP status codes are now also included in the findings.

## Features

* **Comprehensive Firebase Detection:** Identifies a broad spectrum of Firebase services and related API endpoints.
* **Precise Matching Logic:** Utilizes `matchers-condition: and` with negative status filters and specific DSL conditions to minimize false positives and confirm findings more reliably.
* **Status Code in Output:** Findings now include the HTTP status code of the response, aiding in quicker analysis (e.g., `[status=403]`).
* **Dynamic Project ID Handling:** Designed to take a direct Firebase Project ID as input for accurate targeting.
* **Extensive Path Coverage:** Includes numerous common variants for services like Firebase Realtime Database.

## Usage

1.  **Install Nuclei:** Ensure Nuclei is installed. Instructions are on the [Nuclei GitHub repository](https://github.com/projectdiscovery/nuclei).

2.  **Obtain the Template:**
    * If you've cloned this repository (e.g., `git clone https://github.com/SKHTW/Firebase_Nuclei.git` and `cd Firebase_Nuclei`) you'll have `firebase-discovery.yaml`.
    * Otherwise, ensure you have the latest version of `firebase-discovery.yaml`.

3.  **Run the Nuclei Template (Recommended Method):**
    The template is designed to take the **direct Firebase Project ID** as input.
    ```bash
    nuclei -t firebase-discovery.yaml -u <YOUR_FIREBASE_PROJECT_ID>
    ```
    * Replace `<YOUR_FIREBASE_PROJECT_ID>` with the actual Firebase Project ID (e.g., `google.com = google`, `facebook.com = facebook`).
    * The template's `variables` section uses `project: "{{Hostname}}"` which will correctly use this direct input.
    * Add `-v` for verbose output to see all attempted requests:
        ```bash
        nuclei -t firebase-discovery.yaml -u <YOUR_FIREBASE_PROJECT_ID> -v
        ```

4.  **Analyze Results:**
    Nuclei will output findings, which will now include the HTTP status code (e.g., `[status=403]`). Analyze these to understand the exposed Firebase footprint.

## Template Details (Illustrative Examples of Checks)

The template performs checks for a wide range of Firebase/GCP services. The `{{project}}` variable will be the Firebase Project ID you provide.

* **Firebase Realtime Database:**
    * Checks numerous permutations like `https://{{project}}.firebaseio.com/.json`, `https://{{project}}-default-rtdb.firebaseio.com/.json`, `https://{{project}}-dev.firebaseio.com/.json`, `https://dev-{{project}}.firebaseio.com/.json`, `https://{{project}}ios.firebaseio.com/.json`, and their corresponding `.settings/rules.json` paths.
* **Firebase Hosting:**
    * `https://{{project}}.web.app/`
    * `https://{{project}}.firebaseapp.com/`
* **Firebase Storage:**
    * `https://firebasestorage.googleapis.com/v0/b/{{project}}.appspot.com/o` (default bucket convention)
    * `https://firebasestorage.googleapis.com/v0/b/{{project}}/o` (project ID as bucket name)
* **Firebase Firestore:**
    * `https://firestore.googleapis.com/v1/projects/{{project}}/databases/(default)/documents/test` (attempts to list a test collection)
* **Firebase Cloud Functions:**
    * Checks regional endpoints like `https://us-central1-{{project}}.cloudfunctions.net/`, `https://europe-west1-{{project}}.cloudfunctions.net/`, etc.
* **Firebase App Check:**
    * `https://firebaseappcheck.googleapis.com/v1beta/projects/{{project}}/apps`
* **Firebase Recaptcha Enterprise:**
    * `https://recaptchaenterprise.googleapis.com/v1beta1/projects/{{project}}/keys`
* **Firebase Identity Toolkit:**
    * `https://identitytoolkit.googleapis.com/v2/projects/{{project}}/config`
* **Firebase Instance ID (Deprecated API):**
    * `https://iid.googleapis.com/iid/v1:batchImport` (Static path, checks for API presence)
* **Firebase Cloud Messaging (FCM):**
    * `https://fcm.googleapis.com/fcm/send` (Static path, POST request to test endpoint)
* **Firebase Dynamic Links:**
    * `https://{{project}}.page.link`
* **Firebase Installations:**
    * `https://firebaseinstallations.googleapis.com/v1/projects/{{project}}/installations` (POST request)

The template uses a combination of negative status code filtering and DSL expressions (checking for specific status codes like 200, 401, 403, 405 or content in the response body/headers) to identify these services.

## Important Notes

* **Purpose:** This template is for detection and reconnaissance. Further manual analysis is required to determine actual vulnerabilities or misconfigurations.
* **Accuracy:** While designed to be more precise, always verify findings. The nature of Firebase means some services might be intentionally public or protected as expected.
* **Authorization:** **Always obtain proper authorization before scanning any target.**
* **Ethical Use:** Use this tool responsibly and ethically.
* **Further Investigation:** For deep analysis, combine Nuclei results with manual testing, analysis of client-side JavaScript code (which often reveals Firebase project configurations), and other reconnaissance techniques.

## Contributing

Feel free to contribute to this repository by submitting pull requests or opening issues.

## License

This project is licensed under the MIT License LICENSE.
