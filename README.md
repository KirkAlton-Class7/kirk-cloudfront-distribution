# S3 Static Website Hosting with CloudFront (ClickOps)

## Goal

Deploy a static website using a private S3 buckent as the origin, with **CloudFront** as the CDN.

---

## Procedure

### Step 1: Gather Static Site Files

- Collect all static assets required for the site:
  - `index.html`
  - `error.html`
  - Any additional HTML, CSS, JS, or asset files

---

### Step 2: Create S3 Bucket

1. Choose your working region
2. Navigate to **S3 → Create bucket**
3. Enter a **globally unique bucket name**
4. Leave **Block Public Access** settings **enabled**
5. Keep all other settings as default
6. Create the bucket

> [!IMPORTANT]  
> The S3 bucket must remain private. CloudFront will handle public access.

---

### Step 3: Upload Website Files

1. Open the newly created bucket
2. Click **Upload → Add files**
3. Upload `index.html`, `error.html`, and any other site files

> [!NOTE]  
> Do **not** enable static website hosting on the S3 bucket when using CloudFront as the origin.

---

### Step 4: Create a CloudFront Distribution

1. Navigate to **CloudFront → Create distribution**
2. For **Origin domain**, select your S3 bucket from the dropdown
3. Set **Viewer protocol policy** to **HTTPS only**
4. Configure the **Default root object** as `index.html`
5. Create the distribution

---

### Step 5: Configure Custom Error Pages (Optional Example: 400 Error)

1. Go to **CloudFront → Distributions → Select your distribution**
2. Open the **Error pages** tab
3. Click **Create custom error response**
4. Configure the following:
   - **HTTP error code:** `400`
   - **Customize error response:** Yes
   - **Response page path:** `/error.html`
   - **Minimum TTL:** `300` seconds
5. Save changes

---

### Step 6: Test Access via CloudFront

Once the CloudFront distribution status is **Deployed**:

1. Copy the **CloudFront distribution domain name**  
   (example: `d3nmm0hhu5e0s1.cloudfront.net`)
2. Paste it into a browser
3. If needed, prefix with `https://`
4. Confirm the default root object loads correctly

> [!NOTE]  
> CloudFront changes may take time to update due to caching. View the notes below if you edit a file and have trouble viewing the updated version.

---

## Clearing Cache

### Option 1: Hard Refresh (Browser)
Clear the local cache on your browser with a hard refresh.
- Windows/Linux: `Ctrl + Shift + R`
- macOS: `Cmd + Shift + R`

If changes do not appear on the web page, try Option 2.

---

### Option 2: CloudFront Invalidation

1. Go to **CloudFront → Distributions → Select your distribution**
2. Open **Invalidations**
3. Click **Create invalidation**
4. Set **Object path** to `/*`
5. Create the invalidation
6. Reload the site (use private/incognito mode if needed)

---

## References

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
