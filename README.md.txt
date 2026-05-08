# StefanFiles – Mother’s Day Website ❤️

A full-featured Mother's Day website with:

- Permanent photo, video, and audio uploads  
- Animated slideshow (soft crossfade)  
- Responsive photo/video gallery  
- Floating heart animations  
- Letter to Mom section  
- Node.js backend with Express  
- Persistent storage using Render Disk  
- Fully deployable on Render  

---

## 📁 Project Structure

StefanFiles/ │ ├── server.js ├── package.json ├── render.yaml ├── .gitignore ├── README.md │ ├── uploads/ <-- Empty folder (Render will mount disk here) │ └── public/ ├── index.html ├── style.css └── script.js



---
## 🚀 Deploying on Render
### 1. Push this folder to GitHub
In your terminal:
git init git add . git commit -m "Initial Mother's Day site" git branch -M main git remote add origin 
github.com
 git push -u origin main



Replace `YOUR-USERNAME` with your GitHub username.
---
### 2. Deploy to Render
1. Go to [render.com](https://render.com)  
2. Create a **New Web Service**  
3. Connect your `stefanfiles` GitHub repo  
4. Render will read:
- `render.yaml`  
- `package.json`  
5. It will install dependencies and start your server.
---
### 3. Enable permanent uploads
Render will automatically create a **5GB disk** mounted to:
/uploads



Any photo, video, or song uploaded through your site will stay permanently.
---
## 🖼️ Features
### Slideshow
- Soft crossfade transition
- Automatically loops
- Updates when new images are uploaded
### Gallery
- Auto-grid layout
- Displays images and videos responsively
### Music Player
- Upload MP3, WAV, M4A
- Automatically loads on page refresh
---
## 💻 Development (Local)
Install dependencies:
npm install



Run server:
npm start



Open:

localhost



---
## ❤️ Made with love for Mother’s Day
This project was created to celebrate a beautiful mother with memories, love, and joy.