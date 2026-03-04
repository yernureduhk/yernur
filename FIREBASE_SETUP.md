# Firebase Setup for Essays Page

This guide will help you set up Firebase Storage to enable file uploads on the Essays page.

## Step 1: Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Create a project"
3. Enter a project name (e.g., "yernurs-yurt-essays")
4. Follow the setup wizard (you can disable Google Analytics if not needed)
5. Click "Create project"

## Step 2: Enable Authentication

1. In your Firebase project, go to **Build > Authentication** in the left sidebar
2. Click "Get started"
3. Select "Email/Password" provider
4. Toggle it **ON**
5. Click "Save"

## Step 3: Set Up Firebase Storage

1. Go to **Build > Storage** in the left sidebar
2. Click "Get started"
3. Select "Start in test mode" (for development)
4. Choose a storage location near you
5. Click "Done"

### Configure Storage Rules

In the Storage tab, click "Rules" and update the rules:

```
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /essays/{userId}/{allPaths=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

This ensures users can only access their own uploaded essays.

## Step 4: Get Your Firebase Configuration

1. Click the gear icon ⚙️ (next to "Project Overview") and select "Project settings"
2. Scroll down to "Your apps" section
3. Click the web icon `</>` to add a web app
4. Enter a nickname (e.g., "yernurs-yurt-web")
5. Click "Register app"
6. Copy the `firebaseConfig` object values

## Step 5: Update essays.html

Replace the placeholder values in `essays.html` with your actual Firebase config:

```javascript
const firebaseConfig = {
    apiKey: "YOUR_ACTUAL_API_KEY",
    authDomain: "your-project.firebaseapp.com",
    projectId: "your-project-id",
    storageBucket: "your-project.appspot.com",
    messagingSenderId: "your-sender-id",
    appId: "your-app-id"
};
```

## Step 6: Test Your Setup

1. Open `essays.html` in a browser
2. Create an account with your email and password
3. Upload a test PDF or DOC file
4. Verify the file appears in your Firebase Storage console

## Free Tier Limits (Firebase Spark Plan)

- **Storage**: 5 GB total
- **Downloads**: 1 GB/day
- **Upload operations**: 20,000/day
- **Authentication**: Unlimited email/password users

For a personal essays page, these limits are typically sufficient.

## Troubleshooting

### "Storage: User does not have permission to access"
- Make sure Storage rules are published
- Ensure you're signed in before uploading

### "Firebase: Error (auth/unauthorized-domain)"
- Go to Authentication > Settings > Authorized domains
- Add your domain (or `localhost` for local testing)

### Files not appearing after upload
- Check browser console for errors
- Verify localStorage is enabled
- Check Firebase Storage console to see if files were uploaded

## Security Notes

1. **Never commit your Firebase config to public repositories** if the project has sensitive data
2. The current setup requires authentication for uploads
3. Each user's files are isolated by their user ID
4. Consider adding file size limits in production

## Optional: Deploy to Firebase Hosting

If you want to host your website on Firebase:

1. Install Firebase CLI: `npm install -g firebase-tools`
2. Login: `firebase login`
3. Initialize: `firebase init hosting`
4. Deploy: `firebase deploy`

Your site will be available at `https://your-project.web.app`