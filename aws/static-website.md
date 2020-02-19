The domain of S3 static website have two format:

* The legacy format:
  
      http://${bucket-name}.s3-website-${Region}.amazonaws.com

* The new format:

      http://${bucket-name}.s3-website.${Region}.amazonaws.com


The decision is base on the region. There is a mapping table on AWS's website:

<https://docs.aws.amazon.com/general/latest/gr/s3.html#s3_website_region_endpoints>
