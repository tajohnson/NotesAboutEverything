


[cloudshell-user@ip-10-130-60-254 ~]$ aws iam create-access-key --user-name messagestoreservice

{
    "AccessKey": {
        "UserName": "messagestoreservice",
        "AccessKeyId": <See "Secure Info">,
        "Status": "Active",
        "SecretAccessKey": <See "Secure Info">,
        "CreateDate": "2024-12-09T22:15:33+00:00"
    }
}



[cloudshell-user@ip-10-130-60-254 ~]$ aws iam create-access-key --user-name messagestoreservicetest

{
    "AccessKey": {
        "UserName": "messagestoreservicetest",
        "AccessKeyId": <See "Secure Info">,
        "Status": "Active",
        "SecretAccessKey": <See "Secure Info">,
        "CreateDate": "2024-12-09T22:16:36+00:00"
    }
}
