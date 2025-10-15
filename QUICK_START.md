# Quick Start Guide

## For Developers Setting Up Firebase

### Prerequisites
- Google account
- GitHub account
- Access to Firebase Console

### 5-Minute Setup

#### Step 1: Create Firebase Project (2 min)
1. Go to https://console.firebase.google.com/
2. Click "Add project"
3. Enter name: `data-plotter` (or your choice)
4. Disable Google Analytics (optional)
5. Click "Create project"

#### Step 2: Enable Authentication (1 min)
1. In Firebase Console, click **Authentication** in left menu
2. Click "Get started"
3. Click **Google** sign-in method
4. Toggle "Enable"
5. Select support email
6. Click "Save"

#### Step 3: Enable Firestore (1 min)
1. In Firebase Console, click **Firestore Database**
2. Click "Create database"
3. Select **"Start in production mode"**
4. Choose location closest to your users
5. Click "Enable"

#### Step 4: Add Security Rules (30 sec)
1. In Firestore, click **Rules** tab
2. Copy/paste this code:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/data/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```
3. Click "Publish"

#### Step 5: Get Config (30 sec)
1. Click gear icon ⚙️ > **Project settings**
2. Scroll to "Your apps"
3. Click Web icon (`</>`)
4. Register app: name it "Data Plotter"
5. Copy the `firebaseConfig` object

#### Step 6: Update Code (1 min)
1. Open `index.html` in your repository
2. Find line ~672 with `const firebaseConfig = {`
3. Replace the placeholder values with your config
4. Save and commit

#### Step 7: Deploy (30 sec)
1. Push to GitHub
2. Vercel auto-deploys (if connected)
3. Add your domain to Firebase authorized domains:
   - Firebase Console > Authentication > Settings > Authorized domains
   - Click "Add domain"
   - Enter your Vercel domain

### ✅ Done!
Visit your deployed app and click "Sign in with Google"

---

## For Users Using the App

### How to Get Started
1. Open the Data Plotter application
2. Click **"🔐 Sign in with Google"** in the top-right corner
3. Choose your Google account
4. Start entering data in the table

### Your Data is Automatically Saved
- Changes save automatically 1 second after you stop typing
- No save button needed
- Your data is private (only you can see it)
- Access your data from any device by signing in

### What You Can Do
✅ Enter data in the table
✅ Add/remove columns
✅ Add rows
✅ Import CSV files (drag & drop)
✅ Plot your data as charts
✅ Calculate correlations between variables

### Data Privacy
🔒 **Your data is private**
- Only you can see your data
- Other users cannot access your data
- Data is encrypted in transit and at rest
- Sign out anytime to stop saving

### Need Help?
- Yellow warning banner? You're not signed in - sign in to save data
- Can't sign in? Contact the app administrator
- Lost data? Make sure you're signed in with the correct Google account

---

## Troubleshooting

### "Firebase not configured" warning
**Problem**: The app administrator hasn't set up Firebase yet
**Solution**: Administrator must follow developer setup steps above

### "Login failed" error
**Problem**: Domain not authorized in Firebase
**Solution**: Administrator must add domain to Firebase authorized domains

### Data not saving
**Problem**: Not signed in or Firestore rules not configured
**Solutions**:
1. Make sure you're signed in (check top-right corner)
2. Check browser console for errors (F12)
3. Contact administrator to verify Firestore rules

### Data disappeared after refreshing
**Problem**: Signed in with different Google account
**Solution**: Sign in with the original Google account you used

### Can't import CSV
**Problem**: CSV file format not supported
**Solution**: 
- Make sure file has .csv extension
- First row should be headers
- Use commas as separators
- Save Excel files as CSV first

---

## FAQ

**Q: Is my data backed up?**
A: Yes, Firestore automatically handles backups and replication.

**Q: Can I access my data from multiple devices?**
A: Yes, sign in with the same Google account on any device.

**Q: Can I share my data with someone?**
A: Not currently - each user has private data only.

**Q: What happens if I sign out?**
A: Your data is saved. Sign back in to access it again.

**Q: Is there a storage limit?**
A: Firebase free tier includes 1GB storage (plenty for data tables).

**Q: Can I download my data?**
A: Not directly from the app, but you can view it in Firebase Console or export to CSV from the table.

**Q: What if I delete data by accident?**
A: Currently no undo feature. Be careful when deleting columns/rows.

**Q: Do I need to click a save button?**
A: No, data saves automatically as you make changes.

**Q: How do I know my data is saved?**
A: Look for "Data saved successfully" in browser console (F12), or refresh the page - your data should persist.

---

## Tips & Tricks

💡 **Fast Data Entry**
- Use Tab key to move between cells
- Copy/paste from Excel works great
- Import CSV for bulk data

💡 **Organization**
- First column is always Date (locked)
- Rename columns by clicking header names
- Delete unwanted columns with ✕ button

💡 **Visualization**
- Select X-axis (usually Date)
- Select Y1 and optionally Y2 axes
- Click "Plot Data" to see chart
- Hover over points for values

💡 **Statistics**
- Use "Calculate All Correlations" to see all relationships
- Pearson correlation coefficient (r) shown
- Strength indicators: Weak, Moderate, Strong

💡 **Data Management**
- Sign out when done for security
- Each Google account has separate data
- Use different accounts for different datasets
- Import same CSV on different accounts to compare

---

## Support

For issues or questions:
1. Check browser console (F12) for error messages
2. Review TESTING.md for detailed troubleshooting
3. Contact the app administrator
4. Check Firebase Console for service status

## Resources

- 📖 **Setup Instructions**: `actions_for_devs.md`
- 🧪 **Testing Guide**: `TESTING.md`
- 🔧 **Configuration Help**: `FIREBASE_CONFIG_EXAMPLE.md`
- 📊 **Technical Details**: `IMPLEMENTATION_SUMMARY.md`
- 🏠 **Project Overview**: `README.md`
