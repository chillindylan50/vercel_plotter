# Testing Guide for Google Authentication and Firestore Integration

## Overview

This guide helps verify that Google Authentication and Firestore data persistence are working correctly.

## Prerequisites

Before testing, ensure you have:
1. Completed all steps in `actions_for_devs.md`
2. Updated `firebaseConfig` in `index.html` with your Firebase credentials
3. Set up Firestore security rules
4. Added your domain to Firebase authorized domains

## Test Scenarios

### Test 1: Firebase Configuration Check

**Expected Behavior**: 
- If Firebase is NOT configured (placeholder values remain), you should see a yellow warning banner: 
  > "⚠️ Firebase not configured. Please check actions_for_devs.md for setup instructions. Your data will not be saved."

**How to Verify**:
1. Open the application in your browser
2. Look for the yellow banner below the title
3. Check browser console for: "Firebase not configured - using local storage only"

### Test 2: Authentication Flow

**Expected Behavior**: 
- User can sign in with Google
- User info appears in top-right corner
- Warning banner disappears after sign-in

**Steps**:
1. Click "🔐 Sign in with Google" button in the top-right
2. Select your Google account in the popup
3. Verify the following:
   - Warning banner is hidden
   - Your name/email appears in top-right corner
   - Your profile picture appears (if available)
   - "Logout" button is visible

**What to Check in Console**:
```
Firebase initialized successfully
Data loaded successfully
```

### Test 3: Data Persistence - Save

**Expected Behavior**: 
- Data automatically saves to Firestore 1 second after changes
- No manual save button needed

**Steps**:
1. Ensure you are signed in
2. Edit a cell in the table (e.g., change a date or number)
3. Wait 2 seconds
4. Check browser console for: "Data saved successfully"
5. Open Firebase Console > Firestore Database
6. Navigate to: `users/{your-uid}/data/tableData`
7. Verify your data is stored there

**Firestore Document Structure**:
```javascript
{
  columnCount: 3,
  data: [
    ["2024-01-01", "10", "20"],
    ["2024-01-02", "15", "35"],
    ...
  ],
  headers: ["Date", "Column 2", "Column 3"],
  lastUpdated: Timestamp(...)
}
```

### Test 4: Data Persistence - Load

**Expected Behavior**: 
- User's saved data loads automatically on sign-in

**Steps**:
1. While signed in, make some changes to the table
2. Wait for auto-save (2 seconds)
3. Click "Logout"
4. Click "Sign in with Google" again
5. Verify your previous data is restored

**What to Check**:
- All your custom data is present
- Column headers are correct
- Row data matches what you entered

### Test 5: Data Isolation Between Users

**Expected Behavior**: 
- Different users see different data
- Users cannot access each other's data

**Steps**:
1. Sign in as User A
2. Enter some unique data (e.g., "UserA-Data" in a cell)
3. Wait for auto-save
4. Logout
5. Sign in as User B (different Google account)
6. Verify you see default/empty data, NOT User A's data
7. Enter different data as User B
8. Logout and sign back in as User A
9. Verify you see User A's original data

**Security Verification**:
- Open Browser DevTools > Network tab
- Look at Firestore requests
- Each request should include your authentication token
- Requests to other users' paths should fail with "permission-denied"

### Test 6: Auto-Save Features

**Expected Behavior**: 
- Changes trigger auto-save after 1 second debounce
- Multiple rapid changes only trigger one save

**Actions That Trigger Auto-Save**:
1. ✅ Editing cell values
2. ✅ Renaming column headers
3. ✅ Adding new rows
4. ✅ Adding new columns
5. ✅ Deleting columns
6. ✅ Importing CSV files

**Steps**:
1. Sign in
2. Make multiple rapid changes (type in cells quickly)
3. Check console - should see only ONE "Data saved successfully" message
4. This confirms debouncing is working

### Test 7: Offline Behavior

**Expected Behavior**: 
- App shows warning when Firebase is unavailable
- App continues to work locally without saving

**Steps**:
1. Sign in
2. Open Browser DevTools > Network tab
3. Enable "Offline" mode
4. Try to make changes
5. Check console for errors
6. Disable offline mode
7. Make changes again
8. Verify auto-save resumes

### Test 8: CSV Import with Auto-Save

**Expected Behavior**: 
- Imported CSV data automatically saves to Firestore

**Steps**:
1. Sign in
2. Prepare a CSV file:
   ```csv
   Date,Temperature,Humidity
   2024-01-01,72,45
   2024-01-02,75,50
   2024-01-03,70,48
   ```
3. Drag and drop the CSV file onto the drop zone
4. Verify data appears in table
5. Wait 2 seconds
6. Check console for "Data saved successfully"
7. Refresh the page
8. Verify imported data persists

## Common Issues and Solutions

### Issue: "Firebase not configured" warning persists

**Solution**:
- Check that `firebaseConfig` in `index.html` has been updated
- Verify all config values are correct (no "YOUR_" placeholders)
- Clear browser cache and reload

### Issue: "Login failed" error

**Possible Causes**:
1. Domain not in Firebase authorized domains
   - Go to Firebase Console > Authentication > Settings > Authorized domains
   - Add your domain (e.g., `localhost` for local testing, or your Vercel domain)

2. Google sign-in not enabled
   - Go to Firebase Console > Authentication > Sign-in method
   - Ensure Google provider is enabled

### Issue: Data not saving

**Debugging Steps**:
1. Check browser console for errors
2. Verify you are signed in (check top-right corner)
3. Check Firestore security rules are published
4. Try manually writing to Firestore from Firebase Console to test permissions

### Issue: "Permission denied" in console

**Solution**:
- Verify Firestore security rules match exactly:
  ```javascript
  match /users/{userId}/data/{document=**} {
    allow read, write: if request.auth != null && request.auth.uid == userId;
  }
  ```
- Make sure rules are published (not in draft mode)

## Developer Console Commands

You can use these commands in the browser console to debug:

```javascript
// Check current user
console.log('Current user:', currentUser);

// Check Firebase configuration
console.log('Firebase configured:', isFirebaseConfigured);

// Manually trigger save
saveUserData();

// Manually trigger load
loadUserData();

// Check table data
console.log('Table data:', getTableData());
```

## Success Criteria

All tests pass if:
- ✅ Google authentication works smoothly
- ✅ User data persists across sessions
- ✅ Different users see different data
- ✅ Auto-save works with 1-second debounce
- ✅ Warning banner shows/hides appropriately
- ✅ CSV import saves to Firestore
- ✅ No console errors during normal operation
- ✅ Firestore security rules prevent unauthorized access

## Next Steps

After successful testing:
1. Deploy to Vercel
2. Add Vercel domain to Firebase authorized domains
3. Test in production environment
4. Monitor Firebase usage in Firebase Console
