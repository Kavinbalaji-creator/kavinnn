# MKB Solar Cooling System — Netlify website

## Before deploying
1. Open `index.html`.
2. Find `PASTE_YOUR_FIREBASE_WEB_API_KEY`.
3. Replace it with the `apiKey` value from Firebase Console → Project settings → General → Your web app.
4. Save the file.

## Deploy with GitHub + Netlify
1. Create a GitHub repository (for example `mkb-solar-cooling-website`).
2. Upload `index.html` to the repository root and commit.
3. In Netlify choose **Add new site → Import an existing project**.
4. Connect GitHub and select the repository.
5. Build command: leave blank. Publish directory: `.` (root).
6. Deploy the site.

## Firebase data contract for the ESP32 firmware
The website listens for:
- `/sensor/temperature` — number, Celsius
- `/sensor/humidity` — number, percent
- `/sensor/updatedAt` — Unix timestamp in milliseconds
- `/status/peltier` — string `ON` or `OFF`

The website writes:
- `/control/mode` — `AUTO` or `MANUAL`
- `/control/targetTemperature` — number
- `/control/manualPeltier` — boolean

The ESP32 firmware must be updated to read `/control` and write `/sensor` and `/status` using the same paths. Website controls will not operate the relay until that firmware integration is completed.

## Security
Do not make the database publicly readable/writable. Configure Firebase Authentication and Realtime Database rules so only authenticated, authorized users can access the required paths. Do not put a Firebase service-account private key or user password in this website.
