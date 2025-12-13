# S3 Static Website Hosting w/ CloudFront (Dec 6, 2025)

Deploy a static website using **Amazon S3** + **CloudFront** via AWS Console (ClickOps).

- No bucket policy required  
- Public access is blocked at the bucket level  
- CloudFront handles secure access and delivery  

---

## 1. ClickOps (AWS Console)

### A. Prepare Site Files
- Gather your static site assets (e.g., `index.html`, `error.html`)

---

### B. Create S3 Bucket

1. Go to the **S3 Console** → Create bucket  
2. Bucket name must be **globally unique**  
3. Choose your **region**  
4. In **Block Public Access settings**, leave all options **enabled** (keep bucket private)

---

### C. Upload Website Files

1. Open the new bucket  
2. Click **Upload** → Add files  
3. Select `index.html`, `error.html`, etc.

---

### D. Set Default Root Object (CloudFront)

1. Do **not** enable static website hosting on the S3 bucket  
2. Note your main entry file (e.g., `index.html`) for CloudFront configuration  

---

### E. Create CloudFront Distribution

1. Open the **CloudFront Console** → Create Distribution  
2. Origin domain: select your **S3 bucket** from the dropdown  
3. Protocol policy: **HTTPS only**  
4. Set **Default root object**: `index.html` (or your main entry file)  
5. Click **Create Distribution**

---

### F. Create Custom Error Pages (Example: 400 Bad Request)

1. Go to **CloudFront → Distributions → Select your distribution**  
2. Open the **Error pages** tab → **Create custom error response**  
3. Configure:
   - **HTTP error code:** `400`  
   - **Response page path:** `/error.html` (must exist in S3)  
   - **TTL:** e.g., `300` seconds  

---

### G. Test Access via CloudFront

- Use the **CloudFront URL** (e.g., `https://d123abc.cloudfront.net/`)  
- The default root object (e.g., `index.html`) should load  

If issues occur, verify:
- S3 objects are readable by CloudFront  
- Files exist and paths are correct  
- CloudFront distribution status is **Deployed**

---

## Cache Invalidation (Force Updates After File Changes)

CloudFront may cache old versions after updates.

### Option 1: Hard Refresh
- `Ctrl + Shift + R` (Windows/Linux)  
- `Cmd + Shift + R` (macOS)

### Option 2: CloudFront Invalidation

1. CloudFront → Distributions → Select yours → **Invalidations**  
2. Click **Create Invalidation**  
3. Path: `/*`

---

## Summary

| Component | Purpose |
|---------|--------|
| S3 Bucket | Hosts private static files |
| Public Access | Blocked at bucket level |
| CloudFront | Public access, caching, TLS, error handling |
| Default Root Object | Configured in CloudFront |
| Error Pages | Handled in CloudFront |
| Invalidation | Forces cache refresh |

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
