#MinIO MinIO 8.2.0

## 文件上传
#MinIO文件上传

### 第一步 导入依赖

 ```pom
 
 <dependency>  
    <groupId>io.minio</groupId>  
    <artifactId>minio</artifactId>  
    <version>8.2.0</version>  
</dependency>

```


### 第二步 配置文件

```yaml

minio:
	endpointUrl: http:// ip地址 :9000
	accessKey: admin
	secreKey: admin123456
	bucketName: 桶名称
	
```

### 第三步 注入配置

```
	//  import org.springframework.beans.factory.annotation.Value;

    @Value("${minio.endpointUrl}")  
    public String endpointUrl;  
  
    @Value("${minio.accessKey}")  
    public String accessKey;  
  
    @Value("${minio.secreKey}")  
    public String secreKey;  
    @Value("${minio.bucketName}")  
    public String bucketName;  
	
```


### 第四步 编写上传方法

```java


    // 上传  
    //Path： /admin/product/fileUpload    //Method： POST    @PostMapping("/fileUpload")  
    public Result fileUpload(@RequestBody MultipartFile file){  
        String fileName= null;  
        try {  
            // 创建minio客户端  
            MinioClient minioClient =  
                    MinioClient.builder()  
                            .endpoint(endpointUrl)  
                            .credentials(accessKey, secreKey)  
                            .build();  
  
            // 配置指定的存储桶是否存在  
            boolean found =  
                    minioClient.bucketExists(BucketExistsArgs.builder().bucket(bucketName).build());  
            if (!found) {  
                //没有指定的桶就创建  
                minioClient.makeBucket(MakeBucketArgs.builder().bucket(bucketName).build());  
            } else {  
                System.out.println("Bucket 'asiatrip' already exists.");  
            }  
  
  
            //生成文件存储的名称  
            fileName = UUID.randomUUID().toString().replaceAll("-","")+file.getOriginalFilename();  
  
            //上传  
            minioClient.putObject(  
                    PutObjectArgs.builder().bucket(bucketName).object(fileName).stream(  
                            file.getInputStream(), file.getSize(), -1)  
                            .contentType(file.getContentType())  
                            .build());  
        } catch (Exception e) {  
            e.printStackTrace();  
        }  
  
        //构建访问 地址放回  
        //http://192.168.254.157:9000/gmall/oppo.png  
        //ip+端口    桶   文件名称  
        String fileUrl=endpointUrl+"/"+bucketName+"/"+fileName;  
        System.out.println(fileUrl);  
  
        return Result.ok(fileUrl);  
    }

```
  
  
  