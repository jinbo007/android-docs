#Android11适配

1.[App有几个存储位置](https://developer.android.com/training/data-storage?hl=zh-cn)



- 应用内部存储：getFilesDir() 和  getCacheDir

- 应用外部存储: getExternalFileDir()或 getExternalCacheDir

- 共享目录 

2.App常用有几种存储需求
- 应用内下载文件或缓存图片  适用于存储到内部存储目录
- 版本升级的apk文件 
- 拍照 

3.[如何使用以兼容到Android11](https://developer.android.com/training/data-storage/use-cases?hl=zh-cn#capture-image-media)

- 应用内部存储 无需相关适配
- 应用外部存储 统一文件方式调用，无需进行适配
- 共享存储适配 用ContentUri方式调用


Ps: DatatStore