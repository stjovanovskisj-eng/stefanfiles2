/* ==========================================================
   FLOATING HEART ANIMATION
   ========================================================== */
function createHeart() {
  const heart = document.createElement("div");
  heart.className = "heart";
  heart.textContent = "💖";
  heart.style.left = Math.random() * window.innerWidth + "px";
  heart.style.animationDuration = 4 + Math.random() * 4 + "s";
  document.body.appendChild(heart);

  setTimeout(() => heart.remove(), 8000);
}

setInterval(createHeart, 1200);


/* ==========================================================
   LOAD FILE LIST FROM BACKEND
   ========================================================== */
async function listFiles() {
  const res = await fetch("/list");
  return await res.json();
}

function fileExt(f) {
  return f.split(".").pop().toLowerCase();
}


/* ==========================================================
   SLIDESHOW (Soft Crossfade)
   ========================================================== */
async function loadSlideshow(files) {
  const slideshow = document.getElementById("slideshow");
  slideshow.innerHTML = "";

  const images = files.filter(f =>
    ["jpg", "jpeg", "png", "gif", "webp"].includes(fileExt(f))
  );

  if (images.length === 0) {
    slideshow.innerHTML = "<p>No slideshow images yet. Upload some!</p>";
    return;
  }

  images.forEach((f, i) => {
    const img = document.createElement("img");
    img.src = "/uploads/" + f;
    if (i === 0) img.classList.add("active");
    slideshow.appendChild(img);
  });

  let index = 0;

  setInterval(() => {
    const slides = slideshow.querySelectorAll("img");
    slides[index].classList.remove("active");
    index = (index + 1) % slides.length;
    slides[index].classList.add("active");
  }, 3500);
}


/* ==========================================================
   GALLERY (Images & Videos)
   ========================================================== */
async function loadGallery(files) {
  const gallery = document.getElementById("gallery");
  gallery.innerHTML = "";

  files.forEach(file => {
    const ext = fileExt(file);
    const url = "/uploads/" + file;

    const div = document.createElement("div");

    if (["jpg", "jpeg", "png", "gif", "webp"].includes(ext)) {
      div.innerHTML = `<img src="${url}">`;
    } else if (["mp4", "mov", "webm"].includes(ext)) {
      div.innerHTML = `<video src="${url}" controls></video>`;
    }

    gallery.appendChild(div);
  });
}


/* ==========================================================
   SONG PLAYER
   ========================================================== */
async function loadSongs(files) {
  const songs = document.getElementById("songs");
  songs.innerHTML = "";

  const musicFiles = files.filter(f =>
    ["mp3", "wav", "m4a"].includes(fileExt(f))
  );

  musicFiles.forEach(file => {
    const url = "/uploads/" + file;
    songs.innerHTML += `<audio src="${url}" controls></audio>`;
  });
}


/* ==========================================================
   FILE UPLOADS (Universal)
   ========================================================== */
async function uploadFiles(inputId) {
  const input = document.getElementById(inputId);
  const fd = new FormData();

  for (let f of input.files) {
    fd.append("files", f);
  }

  await fetch("/upload", {
    method: "POST",
    body: fd
  });

  refreshContent();
}


/* ==========================================================
   REFRESH CONTENT (Slideshow, Gallery, Songs)
   ========================================================== */
async function refreshContent() {
  const files = await listFiles();
  loadSlideshow(files);
  loadGallery(files);
  loadSongs(files);
}


/* ==========================================================
   FORM EVENTS
   ========================================================== */
document.getElementById("uploadForm").addEventListener("submit", e => {
  e.preventDefault();
  uploadFiles("fileInput");
});

document.getElementById("songForm").addEventListener("submit", e => {
  e.preventDefault();
  uploadFiles("songInput");
});

document.getElementById("slideshowForm").addEventListener("submit", e => {
  e.preventDefault();
  uploadFiles("slideshowInput");
});


/* ==========================================================
   INIT
   ========================================================== */
refreshContent();
