# Multer + AWS S3 + MEN Stack for Photo Upload

To let users upload photos using Multer, AWS S3, and the MEN (MongoDB, Express, Node.js) stack, follow these steps:

---

### **1. Set Up Your Project**

1. **Install Dependencies**:
   ```bash
   npm install express multer multer-s3 aws-sdk mongoose dotenv
   ```

2. **Create `.env` File**:
   Store your AWS credentials and bucket information:
   ```env
   AWS_ACCESS_KEY_ID=yourAccessKey
   AWS_SECRET_ACCESS_KEY=yourSecretKey
   AWS_BUCKET_NAME=yourBucketName
   AWS_REGION=yourRegion
   ```

---

### **2. Configure AWS S3**

1. **Initialize AWS SDK**:
   Create a `config/aws.js` file to configure the AWS SDK:
   ```javascript
   const AWS = require('aws-sdk');

   AWS.config.update({
     accessKeyId: process.env.AWS_ACCESS_KEY_ID,
     secretAccessKey: process.env.AWS_SECRET_ACCESS_KEY,
     region: process.env.AWS_REGION,
   });

   module.exports = new AWS.S3();
   ```

---

### **3. Configure Multer with S3**

1. **Set Up Multer**:
   Create a `middlewares/upload.js` file:
   ```javascript
   const multer = require('multer');
   const multerS3 = require('multer-s3');
   const s3 = require('../config/aws');

   const upload = multer({
     storage: multerS3({
       s3: s3,
       bucket: process.env.AWS_BUCKET_NAME,
       acl: 'public-read',
       metadata: (req, file, cb) => {
         cb(null, { fieldName: file.fieldname });
       },
       key: (req, file, cb) => {
         cb(null, `uploads/${Date.now().toString()}_${file.originalname}`);
       },
     }),
     limits: { fileSize: 5 * 1024 * 1024 }, // 5MB limit
   });

   module.exports = upload;
   ```

---

### **4. Create Express Routes**

1. **Set Up Upload Route**:
   ```javascript
   const express = require('express');
   const upload = require('./middlewares/upload');
   const router = express.Router();
   const Photo = require('./models/Photo'); // MongoDB model

   // Upload endpoint
   router.post('/upload', upload.single('photo'), async (req, res) => {
     try {
       const photo = new Photo({
         name: req.file.originalname,
         url: req.file.location,
       });

       await photo.save();
       res.status(200).json({ message: 'File uploaded successfully', photo });
     } catch (err) {
       res.status(500).json({ error: 'Failed to upload file' });
     }
   });

   module.exports = router;
   ```

---

### **5. Create MongoDB Model**

1. **Define Photo Model**:
   Create a `models/Photo.js` file:
   ```javascript
   const mongoose = require('mongoose');

   const photoSchema = new mongoose.Schema({
     name: { type: String, required: true },
     url: { type: String, required: true },
   });

   module.exports = mongoose.model('Photo', photoSchema);
   ```

---

### **6. Integrate in Your App**

1. **Set Up Express Server**:
   ```javascript
   const express = require('express');
   const mongoose = require('mongoose');
   const dotenv = require('dotenv');
   const uploadRoutes = require('./routes/upload');

   dotenv.config();

   const app = express();
   app.use(express.json());

   // MongoDB connection
   mongoose.connect(process.env.MONGO_URI, {
     useNewUrlParser: true,
     useUnifiedTopology: true,
   });

   // Routes
   app.use('/api', uploadRoutes);

   const PORT = process.env.PORT || 3000;
   app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
   ```

---

### **7. Frontend Integration**

1. **Upload Form**:
   Create a form in your frontend to send files via `multipart/form-data`:
   ```html
   <form action="/api/upload" method="POST" enctype="multipart/form-data">
     <input type="file" name="photo" />
     <button type="submit">Upload</button>
   </form>
   ```

2. **API Request**:
   Use `fetch` or Axios for more dynamic uploads:
   ```javascript
   const formData = new FormData();
   formData.append('photo', fileInput.files[0]);

   fetch('/api/upload', {
     method: 'POST',
     body: formData,
   })
     .then((res) => res.json())
     .then((data) => console.log(data));
   ```

---

### **8. Test and Debug**

- Ensure your AWS bucket allows public read access or appropriate IAM policies.
- Test file uploads via tools like Postman.
- Verify MongoDB stores the correct `url` and `name`.

This setup will enable users to upload photos to AWS S3 and store metadata in your MongoDB database.
