# Spring Boot提供的图片上传回显方案


做毕业设计遇到图片上传回显业务

记录一下，挺简单的
<!--more-->

废话不多说直接上代码

``` java
@ResponseBody
    @PostMapping("/upload")
    public String upImg(List<MultipartFile> files, HttpServletRequest request){
       
        //JDK 1.7提供的files
        if(files!=null&&files.size()>0){
            //循环获取file数组中得文件
            for(int i = 0;i<files.size();i++){
                MultipartFile file = files.get(i);
                try {
                    Files.copy(file.getInputStream(),Paths.get(uploadPath,file.getOriginalFilename()));
//这里的uploadPath是上传的地址,Files.copy即可保存文件
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return "success";
    }
```
回显也容易，springboot提供了一个ResourceLoader 接口，只要实现默认的DeafultResouceLoader即可
```java

private ResourceLoader resourceLoader=new DefaultResourceLoader();
@ResponseBody
@GetMapping(value = {"/getImgs/{filename}"},produces = MediaType.IMAGE_JPEG_VALUE)
public ResponseEntity getImgs(@PathVariable("filename") String fileName){
     return ResponseEntity.ok(resourceLoader.getResource("file:"+Paths.get(uploadPath+"\\project",fileName).toString()));
}
```

这样就可以通过localhost://getImgs/{图片名}，直接访问了，也可以放在<img><src>中。

遇到一个问题，如果图片是刚上传的，用此方式并不能回显，会出现空白，不知道为什么。所以改用如下方式

```java
@Override
public void addResourceHandlers(ResourceHandlerRegistry registry) {
   super.addResourceHandlers(registry);
   System.out.println(uploadPath);
   registry.addResourceHandler("/getImgs/project/**").addResourceLocations("file:"+uploadPath+"project/");
   registry.addResourceHandler("/getImgs/user/**").addResourceLocations("file:"+uploadPath+"user/    ");
}
```

增加spring的静态资源映射，这样就可以直接返回图片了。所以不用再像上面那样，单独写一个路由来返回图片了。