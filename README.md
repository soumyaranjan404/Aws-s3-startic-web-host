# AWS S3 Static Website Hosting

This project demonstrates how to host a static website using **Amazon S3**.

The website contains HTML, CSS and JavaScript files. The files are uploaded to an S3 bucket, public access is configured, and S3 Static Website Hosting is enabled.

---

## Architecture

```text
User / Browser
      |
      v
S3 Static Website Endpoint
      |
      v
S3 Bucket
      |
      +-- index.html
      +-- style.css
      +-- script.js
      +-- error.html
```

---

# Step 1: Create an S3 Bucket

1. Open the **AWS Management Console**.
2. Search for **S3**.
3. Click **Create bucket**.
4. Enter a globally unique bucket name.
5. Select the required AWS Region.
6. Configure **Object Ownership**.

There are two ways to make objects publicly accessible.

### Option 1: Using ACL

Select:

```text
ACLs enabled
```

This method allows public access to be configured through object ACLs.

### Option 2: Using Bucket Policy

Select:

```text
ACLs disabled
```

This is the recommended modern approach. Public access can be provided through a bucket policy.

For this project, ACL-based public access can be used for learning purposes.

---

# Step 2: Upload Website Files

Open the newly created S3 bucket.

Click:

```text
Upload
```

Upload the website files:

```text
index.html
style.css
script.js
error.html
```

The files should be directly inside the bucket.

Example:

```text
S3 Bucket
│
├── index.html
├── style.css
├── script.js
└── error.html
```

Make sure the file names match the references used inside `index.html`.

Example:

```html
<link rel="stylesheet" href="style.css">

<script src="script.js"></script>
```

---

# Step 3: Configure Public Access

For a public S3 static website, the website files must be readable by users.

## Method 1: Using ACL

If **ACLs enabled** was selected while creating the bucket:

1. Open the bucket.
2. Select `index.html`.
3. Go to **Permissions**.
4. Open the **Access control list (ACL)** section.
5. Give public read access.
6. Repeat the same process for the other website files.

Publicly readable files:

```text
index.html  → Public Read
style.css   → Public Read
script.js   → Public Read
error.html  → Public Read
```

The CSS and JavaScript files also need to be publicly readable because the browser requests them after loading `index.html`.

---

# Step 4: Disable Block Public Access

Go to:

```text
S3
→ Bucket
→ Permissions
→ Block public access
→ Edit
```

Turn off:

```text
Block all public access
```

Save the changes and confirm the warning.

This is required when intentionally making the website publicly accessible.

---

# Step 5: Configure Static Website Hosting

Open:

```text
S3
→ Your Bucket
→ Properties
```

Scroll down to:

```text
Static website hosting
```

Click:

```text
Edit
```

Select:

```text
Enable
```

Choose:

```text
Host a static website
```

---

# Step 6: Configure Index Document

In the **Index document** field, enter:

```text
index.html
```

The file name must exactly match the uploaded file.

S3 will use `index.html` as the default page when the website endpoint is opened.

---

# Step 7: Configure Error Document

If an error page has been created, enter:

```text
error.html
```

So the configuration will be:

```text
Static website hosting: Enable

Hosting type:
Host a static website

Index document:
index.html

Error document:
error.html
```

Click:

```text
Save changes
```

---

# Step 8: Access the Website

After enabling Static Website Hosting, go back to:

```text
Bucket
→ Properties
→ Static website hosting
```

AWS will show a:

```text
Bucket website endpoint
```

Open the endpoint in your browser.

The website will load:

```text
index.html
```

and the browser will also request:

```text
style.css
script.js
```

from the S3 bucket.

---

# Website Flow

```text
Browser
   |
   | Request website
   v
S3 Website Endpoint
   |
   v
index.html
   |
   +----> style.css
   |
   +----> script.js
   |
   +----> error.html (when required)
```

---

# Screenshot - Website Files / Configuration

![S3 Website Files](https://github.com/soumyaranjan404/Aws-s3-startic-web-host/blob/main/Screenshot%20%28138%29.png)

---

# Screenshot - Hosted Website

![S3 Static Website Hosting](https://github.com/soumyaranjan404/Aws-s3-startic-web-host/blob/main/Screenshot%20%28139%29.png)

---

# Two Ways to Make S3 Website Public

| Feature                      | Method 1: ACL                     | Method 2: Bucket Policy |
| ---------------------------- | --------------------------------- | ----------------------- |
| Object Ownership             | ACLs enabled                      | ACLs disabled           |
| Public access                | Object ACL                        | Bucket Policy           |
| Make individual files public | Yes                               | No                      |
| Block Public Access          | Off                               | Off                     |
| Static Website Hosting       | Enabled                           | Enabled                 |
| Recommended                  | Mainly for learning/legacy setups | Yes                     |

## Method 1

```text
ACLs enabled
      ↓
Upload files
      ↓
Make required files Public Read
      ↓
Disable Block Public Access
      ↓
Enable Static Website Hosting
```

## Method 2

```text
ACLs disabled
      ↓
Upload files
      ↓
Disable Block Public Access
      ↓
Add Bucket Policy
      ↓
Allow s3:GetObject
      ↓
Enable Static Website Hosting
```

Example bucket policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
    }
  ]
}
```

Replace:

```text
YOUR-BUCKET-NAME
```

with your actual bucket name.

---

# Important Notes

* S3 bucket names must be globally unique.
* `index.html` is the default index document.
* File names are case-sensitive.
* CSS and JavaScript files must also be accessible to the browser.
* Static Website Hosting provides an S3 website endpoint.
* The S3 website endpoint is different from the normal S3 object URL.
* For production websites, Amazon CloudFront can be used in front of S3 to provide HTTPS and additional security/performance features.

---

# Technologies Used

* Amazon S3
* HTML
* CSS
* JavaScript

---

# Result

The static website is successfully hosted using **Amazon S3 Static Website Hosting** and can be accessed publicly through the S3 website endpoint.
