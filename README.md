# 🌱 Agro Task Log - Agricultural Task Management System

A Progressive Web App (PWA) for managing daily agricultural tasks, tracking farm operations, and documenting work with before/after photos.

![Version](https://img.shields.io/badge/version-1.0.0-green)
![PWA](https://img.shields.io/badge/PWA-enabled-blue)
![Offline](https://img.shields.io/badge/offline-ready-orange)

## 🚀 Live App

**Access the app here:** `https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/`

*(Replace with your actual GitHub Pages URL after setup)*

## ✨ Features

### 📝 Task Management
- Quick task entry with all essential details
- 7 task types: Fertilizer, Planting, Induce, Harvest, Weed, Sucker, Land Preparation
- Time tracking with auto-calculated duration
- Status tracking: Pending, In Progress, Completed, On Hold

### 📸 Photo Documentation
- **Before Work Photos** - Document initial state
- **After Work Photos** - Show completed work
- Multiple photos per task
- Click to view full-size images
- Visual comparison in activity log

### 📊 Statistics Dashboard
- Real-time task overview
- Activity details by task type
- Recent activity log with photo previews
- Operator performance tracking
- Completion rate metrics
- Monthly activity trends

### 📋 Task Records
- Complete task list with search & filter
- Update status and add photos after completion
- Color-coded status badges
- Detailed task information
- Quick visual reference

### 💾 Data Management
- Export to Excel with one click
- All data stored locally (privacy-first)
- Works completely offline after first load
- No server or database needed

### 📱 Mobile-First Design
- Install as app on phone (Android & iOS)
- Works offline
- Fast and responsive
- Touch-optimized interface

## 🏗️ Setup on GitHub Pages

### Step 1: Create Repository

1. Go to [GitHub](https://github.com) and log in
2. Click **"New repository"** (green button)
3. Name it: `agro-task-log` (or any name you prefer)
4. Set to **Public** (so staff can access)
5. Click **"Create repository"**

### Step 2: Upload Files

Download these files and upload to your repository:
- `index.html`
- `agro-task-log-pwa.html`
- `manifest.json`
- `service-worker.js`

**Two ways to upload:**

#### Option A: Drag & Drop (Easiest)
1. Go to your repository page
2. Click **"Add file"** → **"Upload files"**
3. Drag all 4 files into the upload area
4. Scroll down and click **"Commit changes"**

#### Option B: Git Command Line
```bash
git clone https://github.com/YOUR-USERNAME/agro-task-log.git
cd agro-task-log
# Copy all 4 files into this folder
git add .
git commit -m "Initial commit - Agro Task Log PWA"
git push origin main
```

### Step 3: Enable GitHub Pages

1. Go to repository **Settings**
2. Click **"Pages"** in left sidebar
3. Under **"Source"**, select **"Deploy from a branch"**
4. Select **"main"** branch and **"/ (root)"** folder
5. Click **"Save"**
6. Wait 2-3 minutes for deployment

### Step 4: Share with Staff

Your app will be available at:
```
https://YOUR-USERNAME.github.io/YOUR-REPO-NAME/
```

Share this link with your staff! 📱

## 📱 How Staff Can Use It

### On Computer:
1. Open the link in any browser
2. Use directly or bookmark it
3. All features work in browser

### On Phone (Android):
1. Open link in Chrome browser
2. Click **"📱 Install App"** button (floating button)
3. Or tap **⋮ menu** → **"Add to Home screen"**
4. App icon appears on home screen!

### On Phone (iPhone):
1. Open link in Safari browser
2. Tap **Share** button (box with arrow)
3. Select **"Add to Home Screen"**
4. App icon appears on home screen!

## 🔒 Privacy & Data

### Important Notes:

✅ **Each user's data is separate** - Stored only on their device
✅ **No central database** - Complete privacy
✅ **Works offline** - No internet needed after first load
✅ **Export feature** - Users can backup their data to Excel

### For Shared Data Across Team:

If you need all staff to share the same database, you'll need to set up a backend server. The current version is designed for individual use where each staff member tracks their own work.

**Want shared data?** Let me know and I can create a server version!

## 🛠️ Troubleshooting

**App not loading?**
- Check internet connection (first load only)
- Clear browser cache and reload
- Try different browser

**Install button not showing?**
- Only appears on HTTPS (GitHub Pages is HTTPS)
- Try Chrome on Android or Safari on iPhone
- Refresh the page

**Photos not saving?**
- Browser storage might be full
- Export data and clear old tasks
- Check browser permissions

## 📄 License

Free to use for agricultural operations. Modify as needed for your farm!

---

**Happy Farming! 🚜🌾**
