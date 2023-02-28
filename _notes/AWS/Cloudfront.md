

- **CDN**
- **Distribution**  collection of edge location
- **Origin \-** resource serving the content \(eg. S3 bucket\)
- Has two modes
    - **Web distribution mode**
    - **RTMP**  for media streaming
- **S3 has its own signed urls and cookies**

### Signed urls & cookies

- Gives you possiblity to **restrict access only for specific users**
- **Signed url = 1 file**
- Can be used for **uploading files**
    **Cookie = multiple files**
- You have to define policy
    - **URL expiration**
    - **IP ranges**
    - **Trusted signers** \(which aws account can create urls\)
- **Flow**
    - **App is handling authorization**
    - **When user is authorized its requesting signed url by using AWS SDK and returning it to the client**
    - **User can request a resource directly from cloudfront by using signed url**
- Has limited lifetime
