# Commands
## list
`aws s3 ls s3://{bucket}/{folder}/`
## copy
`aws s3 cp C:\{folder}\{file} s3://{bucket}/{folder}`
`aws s3 cps3://{bucket}/{folder} s3://ceg-postbox`
## remove / delete
`aws s3 rm s3://ceg-postbox/mariana.zip`  
## create expiring URL
`aws s3 presign s3://ceg-postbox/mariana.zip --region eu-west-2 --expires-in 172800`

