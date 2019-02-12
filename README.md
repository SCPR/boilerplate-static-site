# boilerplate-static-site
Boilerplate for a static site repo that's deployable to S3

## in this repo
```
- README.MD (this)
  - public/ (everything under here will be uploaded to S3)
      - assets/
        - javascripts/
        - stylesheets/
      - index.html
```

## setup
1. create an S3 bucket with the same name as the route (ie: elections.laist.org). Bucket name and URL *must* match.
1. set the S3 bucket policy like so (replace `YOURNEWBUCKETNAME`):
    ```
    {
        "Version": "2012-10-17",
        "Statement": [
            {
                "Sid": "AddPerm",
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::YOURNEWBUCKETNAME/*"
            }
        ]
    }
    ```
1. set the property of that S3 bucket to serve static websites
1. rename the file in `.circleci/` from `config.yml.example` to `config.yml`
1. remove these lines from `config.yml` if not using Cloudfront:
    ```
     - run:
         name: Invalidate Cloudfront cache
         command: aws cloudfront create-invalidation --distribution-id $DIST_ID --paths "/*"
    ```
1. create or update the IAM policy for Circle CI to have control over this newly created bucket
1. `git push` this repo to master to get it to show up in Circle CI
1. log into circleci.com, click on this newly created Circle CI project from this repo
1. click the 'settings' cog at the top right for the project
1. scroll down to 'Permissions' and enter in the AWS keys
1. scroll up to 'Environment Variables' and add the following:
    1. BUCKET = [your newly-created bucket]
    1. DIST_ID = [cloudfront distribution id] (optional)
1. click back to the project page and select "rebuild" to rebuild with the new settings.
1. copy `sample_readme.md` into the real `README.md` for the project repo. Fill out relevant parts.

## likes/question/comments?

Create an issue in this repo.  Star this if you've found it helpful.