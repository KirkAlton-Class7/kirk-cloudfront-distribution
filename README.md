# S3 Static Website Hosting w/ CloudFront (Dec 6, 2025)

Deploy a static website using **Amazon S3** + **CloudFront** via AWS Console (ClickOps).

---

## 1. ClickOps (AWS Console)

### A. Gather Your Site Files
- Gather your static site assets (e.g., `index.html`, `error.html`)

---

### B. Create S3 Bucket

1. Choose your working region, then go to **S3** → Create bucket  
2. **Bucket name must be globally unique**  
4. Leave **Block Public Access settings** enabled. This keeps the bucket private.
5. Keep all other defaults as well.
---

### C. Upload Website Files

1. Open the new bucket  
2. Click **Upload** → Add files  
3. Upload your files `index.html`, `error.html`, etc.

> [!NOTE]  
> Do **not** enable static website hosting on the S3 bucket.

---

### D. Create CloudFront Distribution

1. Open the **CloudFront** → Create Distribution  
2. Origin domain: select your **S3 bucket** from the dropdown  
3. Protocol policy: **HTTPS only**  
4. Set **Default root object**: `index.html` (or your main entry file)  
5. Click **Create Distribution**

---

### E. Create Custom Error Pages (Example: 400 Bad Request)

1. Go to **CloudFront → Distributions → Select your distribution**  
2. Open the **Error pages** tab → **Create custom error response** 
3. Configure:
   - **HTTP error code:** `400`
   - **Select Yes for "Customize Error Response"**  
   - **Response page path:** `error.html` (must exist in S3)  
   - **Minimum TTL:** `300` seconds  (default)

---

### F. Test Access via CloudFront
You can test your distribution once CloudFront distribution status is **Deployed** To do this:
- Obtain the **CloudFront Distribution domain name** (e.g., `d3nmm0hhu5e0s1.cloudfront.net`)
- Past this into your web browser (if your browser doesn't recognize the domain, append it with `https://` )
- The default root object (e.g., `index.html`) should load  

---
> [!NOTE]  
> If you edit files in a distribution, it can take time for Cloud Front to update the changes. Use the notes below to force updates so CloudFront quickly loads your file changes.

## Cache Invalidation

CloudFront may cache old versions after updates. You can foce updates using the following methods:

### Option 1: Hard Refresh - Web Browswer
- Load your CloudFront distribution web page and then press:
- `Ctrl + Shift + R` (Windows/Linux)  
- `Cmd + Shift + R` (macOS)
- Once refereshed, your updated content should be visible.
- If this doesn't work, try option 2 in the console.

### Option 2: CloudFront Invalidation

1. CloudFront → Distributions → Select your distribution → **Invalidations**  
2. Click **Create Invalidation**  
3. Object Path: `/*` (points to all objects)
4. Reload your distribution web page and the changes should show.
5. If not, double check by opening the page in private/incognito mode.

---

## Study References

- CloudFront with S3 Origins  
  https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/DownloadDistS3AndCustomOrigins.html
- S3 Block Public Access  
  https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html
- CloudFront Default Root Object  
  https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/DefaultRootObject.html
- CloudFront Invalidation  
  https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Invalidation.html
- CloudFront Custom Error Pages  
  https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/custom-error-pages.html
