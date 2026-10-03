# Learning Lodge Cafe

A drinks-order app for the Learning Lodge, built to run on an iPad as a Home Screen web app. Everyone in the group taps their name and chooses **Tea**, **Coffee**, **Speciality** or **No thanks**. The organiser can see at a glance who has answered and who is still to order, then copy a report of the orders grouped by drink.

The whole app is a single file, `index.html`, with no server, build step or sign-in.

**Live app:** https://matt-ridley.github.io/LearningLodgeCafe/

**Current version:** 2.0.0

## Features

- **Name tiles:** names are shown in two columns, and each person has a tall tile with a large headshot with their headshot (or initials), showing whether they have ordered and what they chose.
- **One-tap ordering:** tapping a name opens a panel with four large drink buttons. One tap saves the answer, and an **Undo** prompt appears for 5 seconds in case of a wrong tap. Changing an order that has already been placed asks for confirmation first, showing the current and new drink side by side.
- **Speciality of the round:** the organiser enters the speciality on offer, and it appears in brackets under "Speciality" everywhere in the app.
- **Letter filter:** the full alphabet, A to Z, sits under the progress bar. Tap a letter to show only the names starting with it; letters with no matching names are greyed out. Titles like "Mrs" are ignored, so "Mrs Bella" is under B. Tap **All** (or the same letter again) to see everyone. The letter clears after each order, ready for the next person.
- **Ordered filter:** **Everyone**, **Not ordered** and **Ordered** buttons, each with a count, narrow the list by whether people have ordered yet. This works together with the letter filter.
- **Live progress:** a counter and progress bar show how many people have answered.
- **Report:** the Report tab groups everyone by drink, lists who is still to order, and has a **Copy report** button for pasting into a message or email.
- **Organiser tools:** add or remove people, add headshot photos, set the speciality, and start a round. Once a round starts, these tools lock behind a password.
- **Shared between iPads:** when the app is connected to the cafe's Firebase account, every iPad shares one list. An order, a new name, a photo, the speciality or a new round on one iPad appears on the others within about a second. An iPad that loses Wi-Fi keeps working and catches up when it reconnects. The footer shows **Live**, **Syncing** or **Offline, will sync**.
- **Backup and restore:** the organiser can save the names, speciality and photos to a file in the Files app, and restore them on the same or a different iPad.
- **Built for touch:** large tap targets, press feedback, landscape and portrait layouts, and light and dark mode.

## Using the app

### Organiser: setting up a round

1. Tap **Organiser** at the bottom of the screen.
2. Enter the **Speciality available this round**.
3. Add everyone's name in the **Add people** box, one per line (you can paste a whole list). The app ships with no names, so a new iPad starts empty. The list is saved on the iPad and kept for future rounds.
4. To add a headshot or remove someone, close the panel and tap their name. The organiser options appear at the bottom of their panel.
5. Tap **Start the round**. The organiser tools lock.

### Everyone: ordering

1. Tap the first letter of your name, then tap your name.
2. Tap **Tea**, **Coffee**, **Speciality** or **No thanks**.
3. To change your answer, tap your name again, choose the new drink, then tap **Yes, change to...** to confirm (or **Keep...** to leave it as it was).

### Organiser: during and after a round

- Open the **Report** tab to see orders grouped by drink, and tap **Copy report** to share them.
- To make changes during a round, tap **Organiser** and enter the organiser password.
- **Start a new round** clears everyone's answers but keeps the names, photos and speciality. Update the speciality first if it has changed.

### Organiser: backing up and restoring

Keep a backup even when the iPads are connected. The free Firebase plan has no automatic backups, and removing a name or photo on one iPad removes it from every iPad straight away, so a backup file is the way to get it back.

1. Tap **Organiser** (enter the password if the round has started).
2. Under **Backup**, tap **Save backup**, then choose **Save to Files** in the share sheet. The file is named `learning-lodge-cafe-backup-` followed by the date and time.
3. To restore, tap **Restore from backup** and pick the file. Check the summary, then tap **Replace with backup**.

Restoring replaces the current names, photos and speciality, and clears any answers in the current round. On a connected iPad this replaces the shared list, so it changes every iPad at once. Backups don't include answers.

Save a new backup after adding people or photos. If the iPads aren't connected to the shared list, also save one before moving to a new iPad, deleting the Home Screen app, or clearing Safari's data.

## Installing on an iPad

1. The app is published with GitHub Pages from the `main` branch of this repo.
2. On the iPad, open https://matt-ridley.github.io/LearningLodgeCafe/ in Safari.
3. Tap **Share**, then **Add to Home Screen**. The app is named "Lodge Cafe" and opens full screen.

Always open the app from the Home Screen icon. This keeps Safari from clearing the saved orders and photos after a period of not visiting the site.

## Sharing between iPads

The app uses a free Firebase project (Google) to share the list between iPads. GitHub Pages only serves the app's file, so the shared list lives in Firebase.

### One-time Firebase setup

1. Go to https://console.firebase.google.com and create a project (Google Analytics isn't needed).
2. **Build > Firestore Database > Create database**. Choose production mode and a nearby location.
3. **Build > Authentication > Get started**, enable **Email/Password**, then on the **Users** tab add one user for the cafe (for example a cafe email address and a strong password). Note the user's **User UID**.
4. **Authentication > Settings > Authorized domains**: add `matt-ridley.github.io`.
5. **Firestore Database > Rules**: paste the contents of `firestore.rules`, replace `CAFE_ACCOUNT_UID` with the User UID from step 3, and click **Publish**.
6. **Project settings > General > Your apps**: add a **Web** app and copy its `firebaseConfig` values into `FIREBASE_CONFIG` near the top of the script in `index.html`. These values are public by design; the rules are what keep the data private.

The free Spark plan is far more than a cafe round needs.

### Connecting each iPad

1. Open the app from the Home Screen. A **Connect this iPad** panel appears.
2. Enter the cafe account's email and password, then tap **Connect**. The iPad stays connected from then on.
3. The first iPad to connect shares its existing names, photos and speciality. Other iPads then load the shared list in place of their own.

To disconnect an iPad, open **Organiser** and tap **Disconnect this iPad from the shared list**.

## Where data is stored

- **When connected**, names, orders, the speciality and photos are kept in the cafe's Firebase project and copied to each iPad for offline use. Only the cafe account can read them. Nothing is saved in this repo.
- **When `FIREBASE_CONFIG` is left empty**, everything is saved only on the iPad, in the browser's local storage, and each iPad has its own separate list.
- The Home Screen app keeps its own data, separate from Safari tabs. Set up names and photos from inside the Home Screen app.
- Closing the app, restarting the iPad, and deploying a new version of `index.html` all keep the saved data.
- Clearing Safari's website data, deleting the Home Screen app, or changing the site's address (for example renaming the repo) loses the saved data. Restore from a backup to get it back.
- Photos are cropped to a square and shrunk to about 20 KB each, so a full set of headshots takes well under 1 MB.

## Organiser password

The password is set by the `PASSWORD` constant near the top of the script in `index.html`. It prevents accidental changes during a round. It is not a security control: anyone who reads the page source can find it, so don't reuse a password from anywhere else.

## Versioning

The version number is the `VERSION` constant in `index.html`, and it is shown in the footer of the app. It follows `major.minor.patch`:

- **Major** (1.9.0 to 2.0.0) for a big change in how the app works, such as sharing the list between iPads.
- **Minor** (1.3.0 to 1.4.0) for each new feature.
- **Patch** (1.3.0 to 1.3.1) for fixes and smaller changes.

Update this README's **Current version** and the changelog in the same commit.

## Changelog

| Version | Date | Changes |
| --- | --- | --- |
| 2.0.0 | 2026-Oct-03 09:16:31 AM | Shared list across iPads using Firebase: orders, names, photos, the speciality and rounds sync live, with offline support. Each iPad connects once with the cafe account. Added a sync status to the footer. Backup and restore wording updated for the shared list: restoring on a connected iPad replaces the list on every iPad. |
| 1.8.0 | 2026-Oct-03 08:18:53 AM | Name tiles in two columns, twice as tall, with larger headshots. The letter filter always shows A to Z, greying out letters with no names. Added an Everyone / Not ordered / Ordered filter. |
| 1.7.0 | 2026-Oct-03 08:05:08 AM | Changing an order already placed this round now needs a second, explicit confirmation. Tapping the drink already chosen closes the panel without changes. |
| 1.6.0 | 2026-Oct-03 07:56:09 AM | Added a first-letter filter to the header to find names faster. Moved the Organiser button from the header to the footer. |
| 1.5.1 | 2026-Oct-02 11:34:25 PM | Moved to the LearningLodgeCafe repository with a fresh history. The app is now at https://matt-ridley.github.io/LearningLodgeCafe/. Re-add it to the Home Screen from the new address and restore from a backup. |
| 1.5.0 | 2026-Oct-02 11:30:23 PM | Organiser backup and restore of names, speciality and photos, using the iPad's Files app. Opening the Organiser panel no longer pops up the keyboard. |
| 1.4.0 | 2026-Oct-02 11:22:30 PM | Removed the built-in name list from the app so names are no longer published on the public site. The organiser now adds names on the iPad, where they are saved locally. Names already saved on an iPad are kept. |
| 1.3.1 | 2026-Oct-02 11:12:52 PM | Added a footer with the version number and credit. Rewrote this README. Removed the unused `tea-round.html`. |
| 1.3.0 | 2026-Oct-02 11:08:58 PM | Organiser sets the speciality for the round, shown in brackets under Speciality. Order panel now reads "Select order for" and "What would [name] like?". Fixed stray "nullnull" text in the locked order panel. |
| 1.2.0 | 2026-Oct-02 10:41:04 PM | Organiser-managed headshot photos, with initials when no photo is set. |
| 1.1.0 | 2026-Oct-02 10:28:32 PM | Renamed to Learning Lodge Cafe. Redesigned for iPad touch screens with name tiles, a drink panel, Order and Report tabs, Undo, and Home Screen support. |
| 1.0.0 | 2026-Oct-02 10:12:09 PM | First version (Tea Round): order list, No thanks option, report by drink, password-locked organiser tools. |

## Credits

Built by Matt Ridley.
