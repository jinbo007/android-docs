#Android11适配

1.[App有几个存储位置](https://developer.android.com/training/data-storage?hl=zh-cn)



- 应用内部存储：getFilesDir() 和  getCacheDir

- 应用外部存储: getExternalFileDir()或 getExternalCacheDir

- 共享目录 Exter

2.App常用有几种存储需求
- 应用内下载文件或缓存图片  适用于存储到内部存储目录
- 版本升级的apk文件 
- 拍照 

3.[如何使用以兼容到Android11](https://developer.android.com/training/data-storage/use-cases?hl=zh-cn#capture-image-media)

- 应用内部存储 无需相关适配
- 应用外部存储 统一文件方式调用，无需进行适配
- 共享存储适配 用ContentUri方式调用

重点讲 andoid10 及以上拍照时生成uri方法

发起照相时的调用
```
    val captureIntent = Intent(MediaStore.ACTION_IMAGE_CAPTURE)
    imageUri = when {
                Build.VERSION.SDK_INT >= 29 -> createImageUri(mActivity)!!
                Build.VERSION.SDK_INT >= 24 ->
                    FileProvider.getUriForFile(mActivity, mActivity.packageName + ".fileprovider", file)
                else -> Uri.fromFile(file)
            }
    captureIntent.putExtra(MediaStore.EXTRA_OUTPUT, imageUri);
    captureIntent.addFlags(Intent.FLAG_GRANT_WRITE_URI_PERMISSION)
    startActivityForResult(captureIntent, CAMERA_REQUEST_CODE)   

```
其中，createImageUri 参考示例方法如下:
```
 /**
     * android10 获取图片机制
     */
    private fun createImageUri(context: Context): Uri? {
        val values = ContentValues()
        val fileName = "IMG_" + System.currentTimeMillis().toString() + ".jpg";
        // 需要指定文件信息时，非必须
        values.put(MediaStore.Images.Media.DISPLAY_NAME, fileName);
        values.put(MediaStore.Images.Media.MIME_TYPE, "image/png");
        values.put(MediaStore.Images.Media.TITLE, "Image.png");
        values.put(MediaStore.Images.Media.RELATIVE_PATH, "Pictures/Esign");
        return context.contentResolver.insert(MediaStore.Images.Media.EXTERNAL_CONTENT_URI, values);
    }
```

> 接收拍照图片
```
   fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == CAMERA_REQUEST_CODE) {
            if (resultCode == RESULT_OK) {
                if (Build.VERSION.SDK_INT >= 29) {
                    // Android 10 使用图片uri加载
                    imageView.setImageURI(mCameraUri);
                } else {
                    // 使用图片路径加载
                    imageView.setImageBitmap(BitmapFactory.decodeFile(mCameraImagePath));
                }
            }
        }
    }
```
