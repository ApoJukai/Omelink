<div align="center">

# OmeLink

### Meet someone new. One conversation at a time.

A premium, privacy-minded random chat experience for instant text and video conversations.

[Features](#features) • [Getting Started](#getting-started) • [Configuration](#firebase-configuration) • [Deployment](#deployment) • [Privacy](#privacy-and-security)

</div>

---

## About OmeLink

OmeLink is a browser-based random chat application that connects two available people for a spontaneous text or video conversation. Visitors can optionally add interests to improve matching, use text-only mode, or enable camera and microphone access for video chat.

The project combines a premium product website and the complete chat application in a single deployable HTML file. Visitors begin on the marketing page and can open the app without navigating to a second URL.

## Features

### Product experience

- Premium responsive product page
- Animated product preview
- Feature, privacy, and FAQ sections
- Illustrated three-step "How it works" section
- Smooth transition from the product page into the app
- Direct app access through the `#app` URL fragment
- Browser Back navigation support
- Mobile, tablet, and desktop layouts
- Reduced-motion accessibility support

### Chat experience

- Random one-to-one matching
- Text-only and video chat modes
- Optional interest-based matching
- Removable interest chips
- Up to five interests per search
- Automatic fallback to random matching
- Real-time text messages
- Typing indicator
- Skip and reconnect flow
- Connection status feedback
- Camera and microphone permission handling

### Privacy-minded design

- No user account required
- Chat messages are not written to the Firebase matchmaking database
- Temporary Firebase-based waiting rooms
- Direct WebRTC media connections through PeerJS where network conditions allow
- Browser-managed encrypted transport for WebRTC connections
- Camera and microphone access requested only for video mode

> [!IMPORTANT]
> OmeLink uses Firebase Realtime Database for temporary matchmaking. Do not claim that the application uses "no database." A more accurate statement is that OmeLink does not store chat messages in its matchmaking database.

## Technology stack

- HTML5
- CSS3
- Vanilla JavaScript
- Firebase Realtime Database
- PeerJS
- WebRTC
- FormKit AutoAnimate

No package manager or build process is required for the current single-file version.

## Project structure

```text
omelink/
├── index.html       # Product page and embedded OmeLink application
├── README.md        # Project documentation
└── LICENSE          # Optional license file
```

The generated illustrations used by the product page are embedded in `index.html`, so separate image files are not required for deployment.

## Getting started

### Prerequisites

You need:

- A modern browser with WebRTC support
- A Firebase project with Realtime Database enabled
- A local web server for development
- HTTPS in production for camera and microphone access

### 1. Clone the repository

```bash
git clone https://github.com/YOUR_USERNAME/omelink.git
cd omelink
```

Replace `YOUR_USERNAME` with your GitHub username.

### 2. Configure Firebase

Open `index.html` and find the Firebase configuration:

```javascript
const firebaseConfig = {
  databaseURL: "https://YOUR_PROJECT_ID-default-rtdb.REGION.firebasedatabase.app/"
};
```

Replace the sample URL with the Realtime Database URL from your Firebase project.

### 3. Start a local server

Do not rely on opening the file directly with a `file://` URL. ES modules, browser permissions, and third-party scripts work more reliably through a local HTTP server.

Using Python:

```bash
python3 -m http.server 8080
```

Then open:

```text
http://localhost:8080
```

Other options include the VS Code Live Server extension, Node-based static servers, or your preferred local development server.

## Firebase configuration

### Create the database

1. Create or open a Firebase project.
2. Open **Build > Realtime Database**.
3. Create a database in the region closest to the expected audience.
4. Copy the database URL into `firebaseConfig`.
5. Configure database rules for the matchmaking paths used by OmeLink.

OmeLink currently uses paths with patterns similar to:

```text
waiting_room_text_random
waiting_room_video_random
waiting_room_text_int_music
waiting_room_video_int_gaming
```

### Database rules

Do not leave the Realtime Database in unrestricted test mode for production. Create rules that limit access to the temporary waiting-room paths required by the application and add abuse protection appropriate to the deployment.

For a production release, consider adding:

- Firebase App Check
- Authentication or anonymous session validation
- Per-session write limits
- Input validation
- Rate limiting through a trusted backend
- Automated cleanup of abandoned waiting-room entries
- Monitoring and abuse alerts

## How matching works

1. A visitor chooses text or video mode.
2. Optional interests are normalized and added as chips.
3. OmeLink checks interest-specific waiting rooms first.
4. If a compatible peer is available, PeerJS starts a connection.
5. If no interest match is found, OmeLink can fall back to the random waiting room.
6. Firebase is taken offline for that session after the peer connection opens.
7. Text messages are exchanged through the PeerJS data connection.
8. Video and audio are exchanged through WebRTC.

## Deployment

OmeLink can be hosted on any static hosting service that supports HTTPS.

### GitHub Pages

1. Push `index.html` and `README.md` to the repository.
2. Open the repository **Settings**.
3. Select **Pages**.
4. Choose **Deploy from a branch**.
5. Select the branch and root directory.
6. Save the configuration.

### Other hosting options

- Firebase Hosting
- Azure Static Web Apps
- Cloudflare Pages
- Netlify
- Vercel

For video chat, deploy over HTTPS. Most browsers restrict camera and microphone access on non-secure origins, except for local development on `localhost`.

## Direct links

The public product page uses the main URL:

```text
https://your-domain.example/
```

The app can be opened directly with:

```text
https://your-domain.example/#app
```

Both routes use the same `index.html` file.

## Privacy and security

OmeLink is privacy-minded, but production security depends on the hosting environment, Firebase rules, PeerJS configuration, moderation controls, and operational practices.

### Current behavior

- The application does not intentionally save chat-message content to Firebase.
- Firebase is used for temporary peer discovery and matchmaking.
- PeerJS coordinates data and media connections.
- WebRTC encrypts media and data in transit.
- Local camera and microphone tracks stop when a visitor leaves the video experience.

### Production recommendations

Before opening OmeLink to the public, add:

- Community guidelines
- Minimum-age requirements appropriate to the target region
- Reporting and blocking workflows
- Abuse and spam prevention
- Rate limits
- Moderation procedures
- A published privacy policy
- Terms of service
- A security contact
- A documented data-retention policy
- Content Security Policy headers
- Dependency version review and monitoring

> [!WARNING]
> Random chat applications can attract harassment, spam, and unsafe behavior. Do not launch a public service without effective safety controls, reporting processes, moderation, and legal review.

## Browser compatibility

OmeLink is intended for current versions of browsers that support:

- WebRTC
- `navigator.mediaDevices.getUserMedia()`
- JavaScript modules
- CSS backdrop filters
- Intersection Observer

Test the application on desktop and mobile versions of Chrome, Edge, Firefox, and Safari before production deployment.

## Accessibility

The interface includes or is designed to support:

- Keyboard-accessible controls
- Visible focus states
- Accessible labels for important controls
- Responsive touch targets
- Status text in addition to status colors
- Reduced-motion preferences
- Alternative text for generated illustrations

Further accessibility testing with keyboard-only navigation and screen readers is recommended before release.

## Known limitations

- Matchmaking currently depends on Firebase transaction behavior and temporary waiting-room entries.
- A public PeerJS service may not provide the reliability or operational control required for production.
- Some networks require a TURN server for dependable WebRTC connectivity.
- Interest strings should be further normalized or encoded before they are used as database path segments.
- The current project does not include a complete moderation backend.
- Reporting cannot preserve evidence unless a compliant evidence-handling system is designed and disclosed.
- Browser autoplay, camera, and microphone policies vary by platform.

## Recommended roadmap

- [ ] Add reporting and blocking
- [ ] Add Firebase App Check
- [ ] Add rate limiting and abuse prevention
- [ ] Add a production TURN server
- [ ] Add connection-quality feedback
- [ ] Add camera switching on mobile
- [ ] Add internationalization
- [ ] Add automated tests
- [ ] Add privacy policy and community-guideline pages
- [ ] Add analytics with privacy-preserving consent controls

## Development notes

The current version intentionally keeps the application in one HTML file for easy deployment. As the project grows, consider separating the code into modules:

```text
src/
├── app.js
├── matchmaking.js
├── peer.js
├── media.js
├── ui.js
└── styles.css
```

A modular structure will make testing, maintenance, security review, and collaboration easier.

## Contributing

Contributions are welcome.

1. Fork the repository.
2. Create a feature branch.
3. Make focused changes.
4. Test text and video modes on at least two browsers.
5. Submit a pull request with a clear description.

```bash
git checkout -b feature/your-feature
```

Please avoid committing real credentials, private service keys, personal information, or unrestricted production database rules.

## License

No license has been selected yet. Add a `LICENSE` file before inviting external reuse or contributions.

Common choices include:

- MIT for a permissive open-source license
- Apache License 2.0 for a permissive license with an explicit patent grant
- GNU GPLv3 for copyleft distribution

## Acknowledgements

OmeLink uses Firebase, PeerJS, WebRTC, and FormKit AutoAnimate to deliver matchmaking, peer connectivity, media communication, and interface animation.

---

<div align="center">

**OmeLink**

Real people. Real conversations.

</div>
