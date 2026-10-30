# MKB Solar Cooling System — corrected Netlify website

## What was corrected
- Fixed the Firebase initialization condition. The uploaded source compared the API key with itself, which prevented Firebase initialization.
- Kept Firebase Email/Password login.
- Kept the live dashboard and the `/sensor`, `/status`, and `/control` database paths.
- Added protection against attaching duplicate database listeners after repeated login/logout cycles.

## Firebase configuration
The source currently contains the API key value from the supplied file. If Firebase still reports `auth/api-key-not-valid`, copy the `apiKey` directly using the copy control in Firebase Console → Project settings → General → Your web app, and replace only the `apiKey` value in `index.html`. Also check Google Cloud API key restrictions and that Identity Toolkit API is enabled.

## Firebase Authentication
Create the Email/Password user in Firebase Console → Authentication → Users. Do not put the account password in this file or share it.

In Firebase Authentication → Settings → Authorized domains, ensure `mkbcooling.netlify.app` is listed.

## Realtime Database security
Keep the database private. For a simple single-user prototype, rules can require authentication:
{
  "rules": {
    ".read": "auth != null",
    ".write": "auth != null"
  }
}
For a production system, narrow these rules to the required paths and authorized UID.

## ESP32 data contract
Website reads:
- `/sensor/temperature` number
- `/sensor/humidity` number
- `/sensor/updatedAt` Unix timestamp in milliseconds
- `/status/peltier` string `ON` or `OFF`

Website writes:
- `/control/mode` string `AUTO` or `MANUAL`
- `/control/targetTemperature` number
- `/control/manualPeltier` boolean

The ESP32 firmware must implement these same paths. Until that firmware is updated, the website cannot receive live sensor data or control the relay.

## Deploy
Commit `index.html` to the connected GitHub repository. Netlify should redeploy automatically. For a plain static site, use a blank build command and `.` as the publish directory.
