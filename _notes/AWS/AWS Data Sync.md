

- Allows you to **sync big amounts of data from your local data center with AWS**
- Uses the **agent installed on source machine**
- Can be used with **NFS** or **SMB systems**
- **Replication** can be done **hourly, daily** or **weekly**
- Can be used to sync with **EFS**
- Can be uploaded to **S3, EFS, FSx**

### Other features

- **MFA delete**
- Object lock
    - For fulfilling WORM \(Write once Read many regulations\) or adding additional layer of security
    - **Retention period**  period when file cannot be deleted
    - Modes
        - **Governance mode**  let you grant possibility of deleting file only for specific users
        - **Compliance lock**  nobody \(even root user\) cannot delete file and retention period cannot be shorten
        - **Legal hold**  object cannot be deleted until legal hold is active
- **Glacier Vault lock  just like s3 object lock \(but simpler\)**
- **Access logs available**
    - can be written to some bucket
    - You can then export it to another account by replication feature
- **Sharing S3 bucket**
    **S3 Select** and **Glacier Select**  allows you to **query** only a part of data **from an object stored on S3 \(eg. Csv file, json\)**
    - By **Bucket policy** \(only programmatically\)
    - By **Bucket ACL**
    - **Cross Account IAM Roles**
