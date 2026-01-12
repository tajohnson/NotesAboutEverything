Bucket: mr-mailstore
	arn:aws:s3:::mr-mailstore
	Lifecycle rules that expire the files automatically after 15, 30, 45, 60, 90..monthly..365, 2 years..yearly..10 years


User: mailstore-ro
	password: See "Secure Info"
	AccessKey: See "Secure Info"
	SecretKey: See "Secure Info"
	
	
	
	Policy limits to read-only of mr-mailstore bucket, getting objects, rw on tags, though.
	
	
	
	First go to the AWS Console sign-in link:
		Console Sign-in link: https://195021147795.signin.aws.amazon.com/console
		
	Then you can access the bucket at 
		https://us-east-1.console.aws.amazon.com/s3/buckets/mr-mailstore?region=us-west-1&tab=objects