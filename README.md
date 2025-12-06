# St. Petersburg Traffic Accidents Map

An interactive map visualization of traffic accidents in St. Petersburg, Russia, built with Leaflet.js.

## Features

- **Interactive Map**: Explore traffic accidents across St. Petersburg
- **Multiple Visualization Modes**: 
  - Heat map view at lower zoom levels
  - Individual point markers with clustering at higher zoom levels
- **Filtering**: Filter accidents by various criteria including:
  - Date range
  - Severity
  - Road conditions
  - Weather conditions
  - And more
- **Statistics**: View real-time statistics based on current filters
- **Hotspots**: Identify top 10 accident hotspots

## Hosting on GitHub Pages

This project is set up to be hosted on GitHub Pages. Follow these steps:

### 1. Create a GitHub Repository

1. Go to [GitHub](https://github.com) and create a new repository
2. Name it (e.g., `st-petersburg-accidents-map`)
3. Choose public or private (public is free for GitHub Pages)
4. **Do not** initialize with README, .gitignore, or license (we already have these)

### 2. Push Your Code to GitHub

In your terminal, navigate to this project directory and run:

```bash
# Initialize git repository (if not already done)
git init

# Add all files
git add .

# Commit the files
git commit -m "Initial commit: St. Petersburg accidents map"

# Add your GitHub repository as remote (replace YOUR_USERNAME and YOUR_REPO_NAME)
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git

# Push to GitHub
git branch -M main
git push -u origin main
```

### 3. Enable GitHub Pages

1. Go to your repository on GitHub
2. Click on **Settings** tab
3. Scroll down to **Pages** section (in the left sidebar)
4. Under **Source**, select:
   - **Branch**: `main`
   - **Folder**: `/ (root)`
5. Click **Save**
6. GitHub will provide you with a URL like: `https://YOUR_USERNAME.github.io/YOUR_REPO_NAME/`

### 4. Access Your Map

After a few minutes, your map will be live at the GitHub Pages URL!

## Files Included

- `index.html` - The main map visualization (automatically served as the homepage)
- `english_clipped.geojson` - GeoJSON data file containing accident locations and details

## Technologies Used

- [Leaflet.js](https://leafletjs.com/) - Interactive map library
- [Leaflet.markercluster](https://github.com/Leaflet/Leaflet.markercluster) - Marker clustering plugin
- [Leaflet.heat](https://github.com/Leaflet/Leaflet.heat) - Heat map plugin
- OpenStreetMap - Map tiles

## Local Development

To run this map locally:

1. Make sure you have a local web server (the map needs to be served, not opened directly as a file)
2. You can use Python's built-in server:
   ```bash
   python3 -m http.server 8000
   ```
3. Open your browser and go to `http://localhost:8000`

## License

This project is for educational purposes as part of a Data Visualization course.

