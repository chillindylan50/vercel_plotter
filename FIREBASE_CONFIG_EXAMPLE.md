# Firebase Configuration Example

This file shows what a properly configured `firebaseConfig` object looks like in `index.html`.

## ⚠️ IMPORTANT

**DO NOT** commit your actual Firebase configuration to a public repository!
This is just an EXAMPLE showing the structure.

## Example Configuration

In `index.html`, you'll find this section around line 672:

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

## What a Real Configuration Looks Like

Replace the placeholder values with your actual Firebase project values. Here's what each field looks like (with fake example values):

```javascript
const firebaseConfig = {
    apiKey: "AIzaSyDOCAbC123dEf456GhI789jKl012-MnO3P",
    authDomain: "my-data-plotter-123.firebaseapp.com",
    projectId: "my-data-plotter-123",
    storageBucket: "my-data-plotter-123.appspot.com",
    messagingSenderId: "123456789012",
    appId: "1:123456789012:web:abc123def456ghi789"
};
```

## How to Get Your Configuration

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select your project (or create a new one)
3. Click the gear icon (⚙️) > **Project settings**
4. Scroll to "Your apps" section
5. Click the Web app icon (`</>`) if you haven't registered an app
6. Copy the `firebaseConfig` object
7. Paste it into `index.html` replacing the placeholder config

## Field Descriptions

| Field | Description | Example Format |
|-------|-------------|----------------|
| `apiKey` | Your Firebase API key (safe to expose client-side) | `AIzaSy...` (37+ chars) |
| `authDomain` | Your Firebase auth domain | `your-project.firebaseapp.com` |
| `projectId` | Your Firebase project ID | `your-project-123` |
| `storageBucket` | Your Firebase storage bucket | `your-project.appspot.com` |
| `messagingSenderId` | Cloud messaging sender ID | `123456789012` (12 digits) |
| `appId` | Your Firebase app ID | `1:123456789012:web:abc123...` |

## Validation Checklist

Before deploying, verify:
- [ ] No field contains "YOUR_" prefix
- [ ] `apiKey` starts with "AIzaSy"
- [ ] `authDomain` ends with ".firebaseapp.com"
- [ ] `storageBucket` ends with ".appspot.com"
- [ ] `messagingSenderId` is a numeric string
- [ ] `appId` starts with "1:" followed by numbers

## Security Notes

### Why is it safe to expose the API key?

The Firebase API key is **not** a secret key. It's used to identify your Firebase project to the Firebase services. Security is enforced by:

1. **Firestore Security Rules** - Only allow authenticated users to access their own data
2. **Authentication** - Users must sign in with Google
3. **Domain Restrictions** - Only authorized domains can use Firebase Authentication

### What you SHOULD keep secret:

- ❌ Server-side private keys
- ❌ Service account credentials  
- ❌ Database admin passwords
- ❌ OpenAI API keys (though users provide their own)

### What's safe in client code:

- ✅ Firebase configuration (apiKey, projectId, etc.)
- ✅ Firebase SDKs
- ✅ Public domain names

## Testing Your Configuration

After updating the configuration:

1. Open the app in a browser
2. Check browser console for: `"Firebase initialized successfully"`
3. The warning banner should not say "Firebase not configured"
4. Try signing in - it should work without errors

If you see errors, double-check your configuration matches the format shown above.
