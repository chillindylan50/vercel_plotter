# Actions Required from Developer

## Firebase Setup

To enable Google Authentication and Firestore data storage, you need to set up a Firebase project and configure the application with your credentials.

### Step 1: Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Add project" or select an existing project
3. Follow the setup wizard to create your project

### Step 2: Enable Google Authentication

1. In Firebase Console, go to **Authentication** > **Sign-in method**
2. Click on **Google** provider
3. Enable the toggle switch
4. Add your app domain to the authorized domains list
5. Save the configuration

### Step 3: Enable Firestore Database

1. In Firebase Console, go to **Firestore Database**
2. Click "Create database"
3. Choose **Start in production mode** (we'll add security rules next)
4. Select a Cloud Firestore location (choose one close to your users)
5. Click "Enable"

### Step 4: Set Up Firestore Security Rules

1. In Firestore Database, go to the **Rules** tab
2. Replace the default rules with the following:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can only access their own data
    match /users/{userId}/data/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Users can only access their own chat history
    match /users/{userId}/chats/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

3. Click "Publish"

### Step 5: Get Firebase Configuration

1. In Firebase Console, go to **Project Settings** (gear icon)
2. Scroll down to "Your apps" section
3. Click on the **Web** icon (`</>`) to add a web app
4. Register your app with a nickname (e.g., "Data Plotter")
5. Copy the Firebase configuration object that looks like this:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123..."
};
```

### Step 6: Update index.html with Your Firebase Config

1. Open `index.html`
2. Find the Firebase configuration section (search for `firebaseConfig`)
3. Replace the placeholder values with your actual Firebase configuration from Step 5

**IMPORTANT:** The configuration values should look like:
- `apiKey`: Starts with "AIzaSy..."
- `authDomain`: Ends with ".firebaseapp.com"
- `projectId`: Your Firebase project ID
- `storageBucket`: Ends with ".appspot.com"
- `messagingSenderId`: Numeric value
- `appId`: Starts with "1:" and contains your project number

📖 **See `FIREBASE_CONFIG_EXAMPLE.md` for a detailed example of what the configuration should look like.**

### Step 7: Deploy to Vercel

After updating the configuration:
1. Commit your changes to the repository
2. Push to GitHub
3. Vercel will automatically deploy the updated application
4. Make sure to add your Vercel domain to Firebase authorized domains (Step 2.4)

### Step 8: Test Your Implementation

After deploying, follow the testing guide to verify everything works correctly:

📖 **See `TESTING.md` for comprehensive testing instructions.**

Quick test checklist:
- [ ] Can sign in with Google
- [ ] User info appears after sign in
- [ ] Data persists after reload
- [ ] Different users see different data
- [ ] No console errors

### Optional: Enable OpenAI Chat Assistant

If you want to use the AI chat assistant:
1. Get an OpenAI API key from [OpenAI Platform](https://platform.openai.com/)
2. Users will enter their own API keys in the application interface
3. No server-side setup needed for this feature

## Security Notes

- The Firebase API key is safe to expose in client-side code - it only identifies your Firebase project
- Firestore security rules ensure data isolation between users
- Each user can only read/write their own data
- Never commit actual API keys or secrets to the repository
- The firebaseConfig object contains public configuration values, not secrets

### How Data Isolation Works

The application stores data in Firestore with the following structure:

```
/users/{userId}/data/tableData
```

Where `{userId}` is the authenticated user's unique Firebase UID. The security rules ensure that:

1. **Authentication Required**: Users must be signed in with Google to access any data
2. **User-Specific Access**: The rule `request.auth.uid == userId` ensures each user can only access documents in their own user collection
3. **Complete Isolation**: User A cannot read, write, or even see that User B's data exists

This means:
- If Alice signs in, she can only access `/users/alice-uid/data/*`
- If Bob signs in, he can only access `/users/bob-uid/data/*`
- Alice and Bob cannot see each other's data, even if they know the document path

### Data Structure in Firestore

For each authenticated user, the following document is created:

**Document Path**: `/users/{userId}/data/tableData`

**Document Contents**:
```javascript
{
  headers: ["Date", "Column 2", "Column 3", ...],
  data: [
    ["2024-01-01", "10", "20", ...],
    ["2024-01-02", "15", "35", ...],
    ...
  ],
  columnCount: 3,
  lastUpdated: <Firebase Timestamp>
}
```

This structure is automatically created and updated when:
- User makes changes to table headers
- User adds/edits/deletes rows or columns
- User imports CSV data
- Changes are debounced (saved 1 second after last edit)

## Troubleshooting

If authentication doesn't work:
- Check that your domain is in Firebase authorized domains
- Verify Firebase configuration is correct in index.html
- Check browser console for error messages
- Ensure Firestore security rules are published

If data doesn't save:
- Verify user is authenticated (check auth state)
- Check Firestore security rules are correct
- Look for errors in browser console
- Ensure Firestore is enabled in Firebase Console
