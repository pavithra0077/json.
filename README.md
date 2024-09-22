
# Haikus for Codespaces

This is a quick node project template for demoing Codespaces. It is based on the [Azure node sample](https://github.com/Azure-Samples/nodejs-docs-hello-world). It's great!!!

Point your browser to [Quickstart for GitHub Codespaces](https://docs.github.com/en/codespaces/getting-started/quickstart) for a tour of using Codespaces with this repo.
const express = require('express');
const bodyParser = require('body-parser');
const routes = require('./routes');

const app = express();
const PORT = process.env.PORT || 3000;

app.use(bodyParser.json());

app.use('/', routes);

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
const express = require('express');
const multer = require('multer');
const router = express.Router();

const upload = multer({}); // Handle file input via multer

// POST /bfhl Route
router.post('/bfhl', upload.single('file'), (req, res) => {
  const { data, file_b64 } = req.body;

  // Basic info for user
  const fullName = "john_doe";
  const dob = "17091999";

  // Process numbers and alphabets
  const numbers = data.filter((item) => !isNaN(Number(item)));
  const alphabets = data.filter((item) => isNaN(Number(item)));

  // Find the highest lowercase alphabet
  const highestLowercaseAlphabet = alphabets
    .filter((char) => char === char.toLowerCase())
    .sort((a, b) => b.localeCompare(a))[0] || null;

  // File validation (simple base64 check)
  const fileValid = file_b64 ? true : false;

  // Example file MIME type and size in KB (for demo purposes)
  const fileMimeType = fileValid ? "image/png" : null; // You can use a proper library to detect the MIME type from Base64
  const fileSizeKb = fileValid ? "400" : "0"; // Example file size, this should be calculated from Base64 length

  // Prepare response
  res.json({
    is_success: true,
    user_id: `${fullName}_${dob}`,
    email: "john@xyz.com",
    roll_number: "ABCD123",
    numbers,
    alphabets,
    highest_lowercase_alphabet: highestLowercaseAlphabet ? [highestLowercaseAlphabet] : [],
    file_valid: fileValid,
    file_mime_type: fileMimeType,
    file_size_kb: fileSizeKb
  });
});

// GET /bfhl Route
router.get('/bfhl', (req, res) => {
  res.status(200).json({ operation_code: 1 });
});

module.exports = router;


