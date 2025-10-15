# vercel_plotter
Data plotter that we'll deploy to Vercel.

## Features

- **Google Authentication**: Secure sign-in with your Google account
- **Personal Data Storage**: Your data is automatically saved to Firestore and only accessible by you
- **Data Visualization**: Plot time-series data with dual Y-axis support
- **Statistical Analysis**: Calculate correlations between variables
- **CSV Import**: Drag and drop CSV files to import data
- **AI Chat Assistant**: Optional OpenAI-powered chat for data insights

## Setup

This application requires Firebase configuration to enable authentication and data storage. See `actions_for_devs.md` for detailed setup instructions.

### Quick Start

1. Create a Firebase project at [Firebase Console](https://console.firebase.google.com/)
2. Enable Google Authentication and Firestore Database
3. Update the `firebaseConfig` in `index.html` with your Firebase credentials
4. Deploy to Vercel or serve locally

## Data Privacy

- Each user's data is isolated in Firestore
- Firestore security rules ensure users can only access their own data
- Data is automatically saved as you make changes (with 1-second debounce)
- You must be signed in to save data

## Technologies

- Pure HTML/CSS/JavaScript (no build step required)
- Firebase Authentication (Google OAuth)
- Firestore for data persistence
- Native SVG for chart rendering

