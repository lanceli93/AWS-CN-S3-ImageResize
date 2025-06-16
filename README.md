# AWS-CN-S3-ImageResize
AWS中国区S3 resize image dynamically方案实现

# 中国区轻量级实现resize image
本方案主要使用 S3 object lambda access point功能, 此功能已在 aws 北京和宁夏region上线.

[Globa方案](https://github.com/aws-solutions/dynamic-image-transformation-for-amazon-cloudfront)在中国区应用需要做功能上的裁剪, 且对于客户来说稍重, 因为多数客户只是想要一个类似于[阿里云OSS的resize image功能](https://www.alibabacloud.com/help/en/oss/user-guide/resize-images-4)

参考此博客的说明: 
https://aws.amazon.com/blogs/aws/introducing-amazon-s3-object-lambda-use-your-code-to-process-data-as-it-is-being-retrieved-from-s3/

> For example, if you ask to use an S3 Object Lambda Access Point for an image with name sunset_600x400.jpg, the Lambda function can look for an image named sunset.jpg and resize it to fit the maximum width and height as described in the file name. In this case, the Lambda function would need access permission to read the original image, because the object key is different from what was used in the presigned URL.

如何配置和创建 S3 lambda access point请参考官方文档, 本repo中包含两个lambda package. 注意本方案的lambda **必须有S3 object 读取权限** (get object).

Python 版本使用Pillow 库.
NodeJS 版本使用Sharp 库.
经测试暂未发现明显性能差距. 可能需要测试更多image size.

# 测试方法
上传对应lambda package 后可以使用如下aws cli命令进行测试(这里block了 public access)
```
aws s3api get-object --bucket arn:aws:s3-object-lambda:{region}:{accountid}:accesspoint/{object lambda access point name} --key image1_300x200.png image1.png
```
