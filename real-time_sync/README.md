# Real-time Sync is a flow triggered by S3 bucket Put, Restore, and Remove events
A diagram and flow description is in the blog post: https://cloudinary.com/blog/ftp_api_for_cloudinary_part_1_real_time_synchronization


## Whitelist your S3 Cloudinary bucket to read directly from your private S3 bucket
https://cloudinary.com/documentation/upload_images#private_storage_url

## Mandatory environment variables:
- CLOUDINARY_URL => Cloudinary account details as appear in https://cloudinary.com/console under "reveal account details"
- cld_sync_root => Root of the sync folder on your Cloudinary account
- s3_sync_root => Root of the sync folder on S3 bucket

## Optional environment variables:
- notification_url => A webhook to receive the status of each completed operation. Documentation at https://cloudinary.com/documentation/upload_images#upload_notifications
- notification_topic => ARN of an SNS topic that can be used to alert on failures
- upload_only_mode => [true / false] Default: false.  
In this mode, deleted files from the FTP source would not be deleted from Cloudinary. This is to allow having a single source of truth at Cloudinary while saving storage from FTP storage.
  The FTP storage can be kept clean by a periodical script, or by hooking to the notification URL above.
- skip_reload_same_etag => [true / false] Default: false.  
This feature is about skipping upload of objects when there is the same eTag on both systems. Please avoid using it on the following conditions:
    1. When trying to override master files with modified upload presets
    2. When customer"s bucket uses KMS encryption
    3. When the object was loaded in multi-parts to the customer"s bucket - Maybe would be supported later
    4. When there was a short-cycle of overriding and then overriding with the previous version
   (due to S3 eventual consistency)
#define environment variable skip_reload_same_etag=true to enable

## License
Released under the MIT license.
