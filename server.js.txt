import express from "express";
import multer from "multer";
import fs from "fs";

const app = express();

// Ensure uploads folder exists (Render persistent disk mounts to /uploads)
if (!fs.existsSync("/uploads")) {
  fs.mkdirSync("/uploads", { recursive: true });
}

// Storage engine for Multer
const storage = multer.diskStorage({
  destination: "/uploads",
  filename: (req, file, cb) =>
    cb(null, Date.now() + "-" + file.originalname)
});

const upload = multer({ storage });

// Serve frontend
app.use(express.static("public"));

// Serve uploaded files
app.use("/uploads", express.static("/uploads"));

// Upload endpoint
app.post("/upload", upload.array("files", 50), (req, res) => {
  const fileURLs = req.files.map(f => `/uploads/${f.filename}`);
  res.json({ success: true, files: fileURLs });
});

// List all uploaded files
app.get("/list", (req, res) => {
  fs.readdir("/uploads", (err, files) => {
    if (err) return res.json([]);
    res.json(files);
  });
});

// Start server
app.listen(3000, () =>
  console.log("Server running at [localhost](http://localhost:3000)")
);
